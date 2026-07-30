# DDT / Newman — Profkom Admin API

Дві окремі колекції (у корені `Sprint4/`):

- **`Postman GUI.postman_collection.json`** — ручний прогін у Postman UI. Один суцільний
  chaining-ланцюжок: `login -> create admin -> list admins -> edit admin ->
  new password check login -> delete admin -> list admins`. Дані для create/edit
  генеруються рандомно під час прогону. DDT/CSV тут не використовується.

- **`Newman DDT.postman_collection.json`** — виключно для Newman. Шість самодостатніх
  папок, кожна без chaining, кожна прогоняється окремою командою зі своїм CSV.

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
npx newman run "Newman DDT.postman_collection.json" --folder "Login DDT" -d "ddt_login.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Create admin DDT" -d "ddt_create_admin.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Edit password DDT" -d "ddt_edit_password.csv" --env-var edit_target_id=<ID>
```

Security-сценарії (SQL-ін'єкції, XSS, порожній рядок, довжина >80 символів;
правило: будь-яка відповідь 2xx = провал тесту):

```
npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Login" -d "SQL_XSS_login.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create admin" -d "SQL_XSS_create_admin.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Edit password" -d "SQL_XSS_edit_password.csv" --env-var edit_target_id=<ID>
```

