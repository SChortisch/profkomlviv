```
npx newman run "Newman DDT.postman_collection.json" --folder "Create unit DDT" -d "ddt_create_unit.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create unit" -d "SQL_XSS_create_unit.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update unit DDT" -d "ddt_update_unit.csv" --env-var unit_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update unit" -d "SQL_XSS_update_unit.csv" --env-var unit_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete unit DDT" -d "ddt_delete_unit.csv" --env-var unit_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete unit" -d "SQL_XSS_delete_unit.csv"
```
