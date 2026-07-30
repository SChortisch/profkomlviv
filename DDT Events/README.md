```
npx newman run "Newman DDT.postman_collection.json" --folder "Create event DDT" -d "ddt_create_event.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create event" -d "SQL_XSS_create_event.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update event DDT" -d "ddt_update_event.csv" --env-var event_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update event" -d "SQL_XSS_update_event.csv" --env-var event_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete event DDT" -d "ddt_delete_event.csv" --env-var event_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete event" -d "SQL_XSS_delete_event.csv"
```
