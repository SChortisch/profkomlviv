# DDT / Newman — Profkom Admin API

Дві окремі колекції (у корені `Sprint4/`):

- **`Postman GUI.postman_collection.json`** — ручний прогін у Postman UI. Один суцільний
  chaining-ланцюжок: `login -> create admin -> list admins -> edit admin ->
  new password check login -> delete admin -> list admins`. Дані для create/edit
  генеруються рандомно під час прогону. DDT/CSV тут не використовується.

- **`Newman DDT.postman_collection.json`** — виключно для Newman. Шість самодостатніх
  папок, кожна без chaining, кожна прогоняється окремою командою зі своїм CSV.

Перед першим запуском переконайтесь, що бекенд піднятий на `http://localhost:8080`
і в базі є seed-адміни з `DbInitializer.cs`. Адмін `admin@gmail.com` (id=1)
використовується для прихованого службового логіну в DDT-запитах, що потребують
авторизації.

> **Пароль id=1 розійшовся з кодом.** У поточній БД `admin@gmail.com` має пароль
> `xb8InQ$w%8#^`, а не `@Admin123` з `DbInitializer.cs` — його змінили пізніше, і
> `DbInitializer.Seed` не перезапише його (він додає адміна лише якщо username
> відсутній). Тому це значення захардкоджене в prerequest-скриптах колекції та в
> `ddt_login.csv`. Якщо піднімати проєкт на **чистій** БД, seed створить id=1 з
> `@Admin123`, і обидва місця треба буде повернути на `@Admin123` — інакше
> `Login DDT` дасть 2 провали, а всі create/edit сегменти не отримають токен.
>
> Тримати справжній пароль у репозиторії — свідомий компроміс. Альтернатива:
> винести його в змінну і передавати через `--env-var admin_pw=...`, не комітячи.

## Передумова для edit-сегментів

`Edit password DDT` та `SQL_XSS Edit password` редагують пароль конкретного,
наперед відомого адміна (без chaining немає кроку "спочатку створити"). Перед
запуском цих двох команд виставте колекційну змінну `edit_target_id` на id
одноразового тестового адміна (наприклад, створеного вручну через
"Postman GUI" або через `Create admin DDT`):

```
npx newman run "Newman DDT.postman_collection.json" ... --env-var edit_target_id=<ID>
```

## Команди

Функціональні DDT-сценарії (перевіряють `Expected result` — точний HTTP-код):

```
npx newman run "Newman DDT.postman_collection.json" --folder "Login DDT" -d "DDT/ddt_login.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Create admin DDT" -d "DDT/ddt_create_admin.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Edit password DDT" -d "DDT/ddt_edit_password.csv" --env-var edit_target_id=<ID>
```

Security-сценарії (SQL-ін'єкції, XSS, порожній рядок, довжина >80 символів;
правило: будь-яка відповідь 2xx = провал тесту):

```
npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Login" -d "DDT/SQL_XSS_login.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create admin" -d "DDT/SQL_XSS_create_admin.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Edit password" -d "DDT/SQL_XSS_edit_password.csv" --env-var edit_target_id=<ID>
```

`ddt_create_admin.csv` розрахований на одноразовий прогін (username-значення
статичні, не перегенеровуються) — повторний прогін без очищення БД поверне
`400` замість `200` на позитивних рядках через колізію унікальності username.
Те саме стосується `SQL_XSS_create_admin.csv`: рядки з `target_field=username`
створюють реальних адмінів, тож на другому прогоні вони дають `400` (дублікат)
і «зеленіють» через побічний ефект, а не через валідацію.

## Формат CSV

Усі шість файлів — UTF-8 **з BOM**, роздільник `,`, переноси CRLF, лапки за
RFC 4180. BOM потрібен, щоб Excel відкривав кирилицю в колонці `scenario` без
«зламаних символів»; Newman (csv-parse) BOM коректно ігнорує — перевірено
прогоном. Не пересохранюйте файли в ANSI/Windows-1251: кирилиця зламається.

Комірки беруться в лапки лише там, де це змістовно (значення з комою або зі
значущими крайніми пробілами, напр. `"Duma@profkom.com "` — тест на PAD SPACE).

Обсяг: `ddt_login` 39 рядків, `ddt_create_admin` 25, `ddt_edit_password` 19,
`SQL_XSS_login` 53, `SQL_XSS_create_admin` 22, `SQL_XSS_edit_password` 18.

Payload'и в security-CSV свідомо не містять `"` та `\`: Postman підставляє
змінну в raw-JSON **без екранування**, тож лапка зламала б сам JSON і дала б
`400` від парсера — тест «проходив» би з неправильної причини.

## Відомі знахідки (не помилки тестів — реальна поведінка бекенду)

Усі коди нижче перевірені прогоном проти живого бекенду на `localhost:8080`
(MariaDB 10.4.32). Очікуваний результат на чистій БД із коректним seed'ом:

| Сегмент | Ітерацій | Провалів | Чому |
| --- | --- | --- | --- |
| `Login DDT` | 39 | 0 | — |
| `Create admin DDT` | 25 | 0 | — |
| `Edit password DDT` | 19 | 0 | — |
| `SQL_XSS Login` | 53 | 0 | усі payload'и відхиляються (`401`/`400`) |
| `SQL_XSS Create admin` | 22 | **14** | 12 × `username` не валідується + 2 × пароль проходить regex |
| `SQL_XSS Edit password` | 18 | **4** | порожній пароль ігнорується + 3 × пароль проходить regex |

Червоні рядки в двох security-сегментах — **задокументовані прогалини бекенду**,
а не хиби тестів. Кожен такий рядок пояснено нижче.

- **Порожні/пробільні поля логіну дають `400`, а не `401`.** `AuthRequest` має
  `[Required]` + `[MaxLength(100)]`, а `[ApiController]` вмикає автоматичну
  валідацію ModelState. `RequiredAttribute` відхиляє і порожній рядок, і рядок
  лише з пробілів (перевіряє `Trim().Length`), тож до `AdminController.Login`
  запит навіть не доходить. Так само `400` дає username/пароль довший за 100
  символів. Раніше рядок «лише пробіли» очікував `401` — це було невірно.
- **Регістр username у логіні.** MariaDB за замовчуванням має регістронезалежну
  collation, тож `ADMIN@GMAIL.COM` логіниться так само, як `admin@gmail.com`.
- **Хвостові пробіли в username ігноруються.** Collation `utf8mb4_general_ci` —
  PAD SPACE, тому `"Duma@profkom.com "` порівнюється як рівний і дає `200`.
  Провідний пробіл **не** ігнорується → `401`. Дублікат із хвостовим пробілом у
  `Create` теж ловиться як існуючий → `400`.
- **`AdminController.Create` не валідує формат `username`.** Перевіряються лише
  надійність пароля та унікальність. Тому рядки в `SQL_XSS_create_admin.csv` з
  `target_field=username` (SQL-ін'єкція, XSS, path traversal, порожній рядок,
  довжина) **очікувано червоні**: бекенд повертає `200` і створює адміна.
- **Username довший за 100 символів мовчки обрізається.** Колонка `varchar(100)`
  (`[MaxLength(100)]` на `Admin.Username`), MariaDB у non-strict режимі обрізає
  без помилки: запит на 107 символів повертає `200`, у відповіді відлунюється
  всі 107, а в БД лежить 100. Такий адмін недосяжний для логіну — повний
  username відхилить `MaxLength` у `AuthRequest` (`400`). Рядок у
  `ddt_create_admin.csv` фіксує саме `200`.
- **Порожній пароль у `Edit` мовчки ігнорується.** `Edit` міняє пароль лише під
  `if (!string.IsNullOrEmpty(req.Password))`, тож `{"password": ""}` повертає
  `200`, не змінивши нічого. Через це рядок `Empty,password` у
  `SQL_XSS_edit_password.csv` **очікувано червоний** (2xx = провал за правилом
  security-набору), хоча реальної вразливості тут немає.
- **Password-поле легітимно приймає спецсимволи.** Payload `' OR '1'='1' --`
  задовольняє regex `IsValidPassword` (є `[A-Z]`, цифра, спецсимвол, ≥8) і
  приймається як звичайний новий пароль — це не SQL-ін'єкція (EF Core
  параметризує запити), а наслідок того, що паролям дозволені спецсимволи. Те
  саме з `<SCRIPT>Alert1!</SCRIPT>`: пароль зберігається як BCrypt-хеш і ніколи
  не рендериться, тож XSS не виникає. Такі рядки — очікувано червоні.
- **Кирилична велика літера не рахується як `[A-Z]`.** Regex перевіряє лише
  ASCII, тож `Пароль1!` відхиляється (`400`), а `Passw0rd!😀` приймається
  (`200`) — emoji потрапляє під `[\W_]`.
- **BCrypt мовчки обрізає пароль до 72 байтів.** Паролі на 72 і 73 байти обидва
  дають `200`; BCrypt.Net-Next 4.0.3 не кидає помилку на перевищенні.
- **`role` не валідується.** `Create` приймає будь-який рядок (напр.
  `superuser`). У DDT-запитах `role` захардкоджений як `admin`, тож у CSV це не
  покрито — окрема знахідка для беклогу.

## Пароль як тестові дані

У `ddt_login.csv` пароль `admin@gmail.com` вживається у трьох різних ролях —
міняти їх наосліп не можна:

- **правильний пароль** (`xb8InQ$w%8#^`) — рядки, де очікується `200`, а також
  рядки «правильний пароль, але інший username» (homoglyph `аdmin@gmail.com`,
  `admin@gmail.com.ua`, `admin`). Там пароль **мусить** бути справжнім, інакше
  `401` прийде з неправильної причини і тест нічого не доведе;
- **завідомо невірний пароль** (`WrongPass123!`) — рядки з SQL-ін'єкцією чи XSS
  у `username`. Сенс саме в тому, що пароль невірний: `2xx` там означав би
  реальний обхід автентифікації;
- **паролі інших seed-адмінів** (`@Duma2004` тощо) — вони лежать відкрито в
  `DbInitializer.cs`, тож секретами не є.
