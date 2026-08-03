# BR-172: GET /api/Team повертає 500 — нульова дата в `Team.UpdatedAt` ламає читання всієї таблиці

> Знайдено не через DDT-ітерацію, а під час прогону мануальної колекції
> `Postman Profkom GUI`, тому винесено в окремий файл (у `BUGS.md` діє правило
> «один багрепорт = рівно одна ітерація DDT»).

| | |
|---|---|
| **Severity** | Critical |
| **Priority** | High |
| **Компонент** | Backend / `TeamController`, модель `Team` |
| **Версія** | `Sprint4/profkomlviv/backend` |
| **Дата виявлення** | 2026-08-03 |
| **Статус** | Відтворюється стабільно |

---

## Опис

`GET /api/Team` повертає `500 Internal Server Error` з `System.InvalidCastException`
замість списку членів команди. Ендпоінт непрацездатний повністю: помилка виникає
під час матеріалізації результату, тому падає **весь запит**, а не окремий елемент —
достатньо одного зіпсованого рядка в таблиці, щоб список не читався взагалі.

Помилка проявляється **лише на читанні**. Запис (`POST /api/Team`) відпрацьовує
успішно й повертає `201 Created`, тому дефект накопичується непомітно: кожен
створений через API член команди робить таблицю нечитабельною.

## Кроки відтворення

1. Виконати `POST /api/Team` з валідним тілом (будь-який member) → отримати `201 Created`.
2. Виконати `GET /api/Team`.
3. Спостерігати `500 Internal Server Error`.

## Очікуваний результат

`200 OK` і масив `Team[]`.

## Фактичний результат

`500 Internal Server Error`, 4.5 KB, тіло — стектрейс:

```
System.InvalidCastException: Unable to convert MySQL date/time to System.DateTime,
set AllowZeroDateTime=True or ConvertZeroDateTime=True in the connection string.
   at MySqlConnector.ColumnReaders.TextDateTimeColumnReader.ParseDateTime(...)
   at MySqlConnector.Core.Row.GetValue(Int32 ordinal)
   ...
   at ProfkomBackend.Controllers.TeamController.GetAll()
      in E:\Study\Profkom site\Sprint4\profkomlviv\backend\Backend\Controllers\TeamController.cs:line 30
```

## Причина (root cause)

У моделі `Team` поле `UpdatedAt` оголошене **не-nullable**, на відміну від усіх
інших моделей проєкту:

```csharp
// Models/Team.cs
public DateTime  UpdatedAt { get; set; }   // Team      — без "?"
// для порівняння:
public DateTime? UpdatedAt { get; set; }   // Department, Unit, Faculty, Document
```

При цьому `TeamController.Create` присвоює `CreatedAt`, але `UpdatedAt` **не присвоює
взагалі**. Далі ланцюжок такий:

1. C# підставляє `default(DateTime)` → `0001-01-01 00:00:00`.
2. Діапазон MySQL `DATETIME` — від `1000-01-01`; значення поза діапазоном.
3. MySQL у нестрогому режимі записує «нульову дату» `0000-00-00 00:00:00`.
4. `MySqlConnector` при читанні відмовляється конвертувати її в `System.DateTime`
   і кидає `InvalidCastException`.

`TeamController.Update` натомість `UpdatedAt` виставляє коректно — тому баг
проявляється лише на записах, створених через `POST` і жодного разу не оновлених.

## Вражені ендпоінти

| Ендпоінт | Наслідок |
|---|---|
| `GET /api/Team` | 500 — підтверджено |
| `GET /api/Team/{id}` | 500 для конкретного зіпсованого запису |
| `GET /api/departments` | ризик 500: робить `.Include(d => d.Head)` і читає ту саму колонку |
| `GET /api/faculties` | ризик 500: робить `.Include(f => f.Head)` |

Через каскад ламаються також CRUD-сценарії Department і Faculty у колекції —
вони створюють голову відділу через `POST /api/Team`, а потім читають сутність
разом із вкладеним `head`.

## Виправлення

1. **Головне — не створювати нових зіпсованих рядків.** У `TeamController.Create`
   задати `UpdatedAt` явно:

   ```csharp
   CreatedAt = DateTime.UtcNow,
   UpdatedAt = DateTime.UtcNow
   ```

2. **Прочитати вже наявні зіпсовані рядки.** Додати до рядка підключення
   (`appsettings.json` → `ConnectionStrings:DefaultConnection`):

   ```
   ConvertZeroDateTime=True;AllowZeroDateTime=True;
   ```

   Тоді `0000-00-00` читається як `DateTime.MinValue` замість викидання винятку.

3. **Рекомендовано на майбутнє (не входить у цей фікс):** привести `Team.UpdatedAt`
   до `DateTime?`, як в решті моделей, і зробити міграцію EF — це усуває клас
   проблеми, а не конкретний її прояв. Також варто почистити наявні записи:

   ```sql
   UPDATE Team SET UpdatedAt = CreatedAt WHERE UpdatedAt = 0;
   ```

## Примітка щодо тестів

Дефект був у коді до написання нових перевірок — колекція його не спричинила,
а виявила. Нові тести створюють помітно більше записів у `Team` (голови відділів
для Department і Faculty плюс власне Team-сценарії), тому проблема, яка раніше
могла лишатись непоміченою, тепер відтворюється щопрогону.
