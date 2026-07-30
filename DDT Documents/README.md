```
npx newman run "Newman DDT.postman_collection.json" --folder "Create document DDT" -d "ddt_create_document.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create document" -d "SQL_XSS_create_document.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update document DDT" -d "ddt_update_document.csv" --env-var doc_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update document" -d "SQL_XSS_update_document.csv" --env-var doc_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete document DDT" -d "ddt_delete_document.csv" --env-var doc_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete document" -d "SQL_XSS_delete_document.csv"
```
