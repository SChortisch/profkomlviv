# DDT / Newman — команди запуску

Запускати **з теки `Sprint4`** (містить `DDT/` і `data/`) — інакше не зрезолвляться
шляхи до файлів із CSV.

Логін адміна — у `DDT/ddt.postman_environment.json`, підключений через `-e` у кожній
команді. Якщо логін віддає `401` (паролі мутують прогони `Edit password DDT`),
оновіть там `ddt_admin_pass` — це полагодить усі набори одразу.

`<ID>` — id наперед створеного об'єкта: chaining у колекціях немає, тож ціль для
update/delete передається ззовні.

## DDT Admin — `/api/Admin`

Функціональні:

```
newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "Login DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/ddt_login.csv"

newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "Create admin DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/ddt_create_admin.csv"

newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "Edit password DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/ddt_edit_password.csv" --env-var edit_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "SQL_XSS Login" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/SQL_XSS_login.csv"

newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "SQL_XSS Create admin" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/SQL_XSS_create_admin.csv"

newman run "DDT/DDT Admin/Newman DDT.postman_collection.json" --folder "SQL_XSS Edit password" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Admin/SQL_XSS_edit_password.csv" --env-var edit_target_id=<ID>
```

## DDT Department — `/api/departments`

Функціональні:

```
newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "Create department DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/ddt_create_department.csv"

newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "Edit department DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/ddt_edit_department.csv" --env-var dep_target_id=<ID>

newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "Delete department DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/ddt_delete_department.csv" --env-var dep_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Create department" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/SQL_XSS_create_department.csv"

newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Edit department" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/SQL_XSS_edit_department.csv" --env-var dep_target_id=<ID>

newman run "DDT/DDT Department/Newman DDT Department.postman_collection.json" --folder "SQL_XSS Delete department" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Department/SQL_XSS_delete_department.csv"
```

## DDT Documents — `/api/documents`

Функціональні:

```
newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "Create document DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/ddt_create_document.csv"

newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "Update document DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/ddt_update_document.csv" --env-var doc_target_id=<ID>

newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "Delete document DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/ddt_delete_document.csv" --env-var doc_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "SQL_XSS Create document" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/SQL_XSS_create_document.csv"

newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "SQL_XSS Update document" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/SQL_XSS_update_document.csv" --env-var doc_target_id=<ID>

newman run "DDT/DDT Documents/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete document" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Documents/SQL_XSS_delete_document.csv"
```

## DDT Events — `/api/events`

Функціональні:

```
newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "Create event DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/ddt_create_event.csv"

newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "Update event DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/ddt_update_event.csv" --env-var event_target_id=<ID>

newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "Delete event DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/ddt_delete_event.csv" --env-var event_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "SQL_XSS Create event" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/SQL_XSS_create_event.csv"

newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "SQL_XSS Update event" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/SQL_XSS_update_event.csv" --env-var event_target_id=<ID>

newman run "DDT/DDT Events/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete event" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Events/SQL_XSS_delete_event.csv"
```

## DDT Faculty — `/api/faculties`

Функціональні:

```
newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "Create faculty DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/ddt_create_faculty.csv"

newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "Update faculty DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/ddt_update_faculty.csv" --env-var faculty_target_id=<ID>

newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "Delete faculty DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/ddt_delete_faculty.csv" --env-var faculty_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "SQL_XSS Create faculty" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/SQL_XSS_create_faculty.csv"

newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "SQL_XSS Update faculty" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/SQL_XSS_update_faculty.csv" --env-var faculty_target_id=<ID>

newman run "DDT/DDT Faculty/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete faculty" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Faculty/SQL_XSS_delete_faculty.csv"
```

## DDT News — `/api/news`

Функціональні:

```
newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "Create news DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/ddt_create_news.csv"

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "Update news DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/ddt_update_news.csv" --env-var news_target_id=<ID>

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "Delete news DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/ddt_delete_news.csv" --env-var news_target_id=<ID>

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "Delete news image DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/ddt_delete_news_image.csv" --env-var news_image_target_id=<IMAGE_ID>
```

Security:

```
newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "SQL_XSS Create news" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/SQL_XSS_create_news.csv"

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "SQL_XSS Update news" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/SQL_XSS_update_news.csv" --env-var news_target_id=<ID>

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete news" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/SQL_XSS_delete_news.csv"

newman run "DDT/DDT News/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete news image" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT News/SQL_XSS_delete_news_image.csv"
```

## DDT Team — `/api/Team`

Функціональні:

```
newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "Create team DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/ddt_create_team.csv"

newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "Update team DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/ddt_update_team.csv" --env-var team_target_id=<ID>

newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "Delete team DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/ddt_delete_team.csv" --env-var team_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "SQL_XSS Create team" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/SQL_XSS_create_team.csv"

newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "SQL_XSS Update team" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/SQL_XSS_update_team.csv" --env-var team_target_id=<ID>

newman run "DDT/DDT Team/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete team" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Team/SQL_XSS_delete_team.csv"
```

## DDT Unit — `/api/units`

Функціональні:

```
newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "Create unit DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/ddt_create_unit.csv"

newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "Update unit DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/ddt_update_unit.csv" --env-var unit_target_id=<ID>

newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "Delete unit DDT" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/ddt_delete_unit.csv" --env-var unit_target_id=<ID>
```

Security:

```
newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "SQL_XSS Create unit" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/SQL_XSS_create_unit.csv"

newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "SQL_XSS Update unit" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/SQL_XSS_update_unit.csv" --env-var unit_target_id=<ID>

newman run "DDT/DDT Unit/Newman DDT.postman_collection.json" --folder "SQL_XSS Delete unit" -e "DDT/ddt.postman_environment.json" -d "DDT/DDT Unit/SQL_XSS_delete_unit.csv"
```
