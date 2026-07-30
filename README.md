# DDT / Newman — Profkom Admin API

Дві окремі колекції (у корені `Sprint4/`):


Перед першим запуском переконайтесь, що бекенд піднятий на `http://localhost:8080`
і в базі є seed-адміни з `DbInitializer.cs` (зокрема `admin@gmail.com` / `@Admin123` —
ці креди використовуються для прихованого службового логіну в DDT-запитах, що
потребують авторизації).

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
`400` замість `200` на позитивному рядку через колізію унікальності username.

## Відомі знахідки (не помилки тестів — реальна поведінка бекенду)

- **Регістр username у логіні.** MySQL/MariaDB за замовчуванням використовує
  регістронезалежну collation, тож `ADMIN@GMAIL.COM` логіниться так само, як
  `admin@gmail.com` — рядок у `ddt_login.csv` очікує `200`, а не `401`.
- **`AdminController.Create` не валідує формат `username`.** Перевіряються лише
  унікальність та надійність пароля. Тому кілька рядків у
  `SQL_XSS_create_admin.csv`, що цілять у поле `username` (SQL-ін'єкція, XSS,
  порожній рядок, довжина >80), швидше за все **впадуть** (поверне `200`
  замість очікуваного відхилення) проти поточного бекенду — це задокументована
  прогалина у вхідній валідації, а не хиба тесту.
- **Password-поле легітимно приймає спецсимволи.** У `SQL_XSS_edit_password.csv`
  рядок з payload `' OR '1'='1' --` формально задовольняє regex надійності
  пароля (`AdminController.IsValidPassword`) і може бути прийнятий як звичайний
  новий пароль — це не SQL-ін'єкція (EF Core параметризує запити), а очікуваний
  побічний ефект того, що паролям дозволені спецсимволи.
