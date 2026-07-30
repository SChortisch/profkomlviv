# DDT / Newman — Department API

Датасети та Newman-колекція для data-driven тестування `/api/departments`.
Колекція: `../DDT JSONS/Newman DDT Department.postman_collection.json` — шість
самодостатніх папок, кожна прогоняється окремою командою newman зі своїм CSV.

Довідник ендпоінтів і параметрів: `../../data/api_reference.md` (розділ Department).

## Передумови

1. Бекенд піднятий на `http://localhost:8080`.
2. Запускати newman з теки `Sprint4` (тієї, що містить і `DDT/`, і `data/`) —
   інакше шляхи до логотипів (`data/cats/cat.1.jpg` тощо) не знайдуться.
3. Логін за замовчуванням — `Duma@profkom.com` / `@Duma2004`. Якщо потрібен
   інший адмін або пароль змінився, передайте:
   ```
   --env-var ddt_admin_user=<логін>
   --env-var ddt_admin_pass=<пароль>
   ```
4. Для `Edit`, `Delete`, `SQL_XSS Edit` потрібен id існуючого відділу — створіть
   його заздалегідь (наприклад, через `Create department DDT`) і передайте:
   ```
   --env-var dep_target_id=<ID>
   ```
   Позитивний рядок у `Delete` цей відділ видаляє, тож для повторного прогону
   `Delete` виставляйте свіжий `dep_target_id`.

## Команди

Функціональні DDT (перевіряють точний HTTP-код зі стовпця `Expected result`):

```
newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "Create department DDT" -d "DDT/DDT Department/ddt_create_department.csv" --env-var ddt_admin_pass=<pass>

newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "Edit department DDT" -d "DDT/DDT Department/ddt_edit_department.csv" --env-var ddt_admin_pass=<pass> --env-var dep_target_id=<ID>

newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "Delete department DDT" -d "DDT/DDT Department/ddt_delete_department.csv" --env-var ddt_admin_pass=<pass> --env-var dep_target_id=<ID>
```

Security DDT (SQL-ін'єкції, XSS, порожній рядок, довжина; правило: будь-яка
відповідь **2xx = провал тесту**):

```
newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Create department" -d "DDT/DDT Department/SQL_XSS_create_department.csv" --env-var ddt_admin_pass=<pass>

newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Edit department" -d "DDT/DDT Department/SQL_XSS_edit_department.csv" --env-var ddt_admin_pass=<pass> --env-var dep_target_id=<ID>

newman run "DDT/DDT JSONS/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Delete department" -d "DDT/DDT Department/SQL_XSS_delete_department.csv" --env-var ddt_admin_pass=<pass>
```

## Прибирання

- Створені відділи видаляйте через `DELETE /api/departments/{id}`.
- Рядок `HeadId=VALID` (у create/edit) створює тестового Team-члена «DDT Head».
  Через API його не видалити (баг: `GET/DELETE /api/Team/{id}` повертають 500).
  Приберіть напряму в БД (MySQL `profkomdb`):
  ```sql
  DELETE FROM Team WHERE Name = 'DDT Head';
  ```
