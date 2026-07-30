```
npx newman run "Newman DDT.postman_collection.json" --folder "Create faculty DDT" -d "ddt_create_faculty.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create faculty" -d "SQL_XSS_create_faculty.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update faculty DDT" -d "ddt_update_faculty.csv" --env-var faculty_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update faculty" -d "SQL_XSS_update_faculty.csv" --env-var faculty_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete faculty DDT" -d "ddt_delete_faculty.csv" --env-var faculty_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete faculty" -d "SQL_XSS_delete_faculty.csv"
```
