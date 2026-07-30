```
npx newman run "Newman DDT.postman_collection.json" --folder "Create team DDT" -d "ddt_create_team.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create team" -d "SQL_XSS_create_team.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update team DDT" -d "ddt_update_team.csv" --env-var team_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update team" -d "SQL_XSS_update_team.csv" --env-var team_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete team DDT" -d "ddt_delete_team.csv" --env-var team_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete team" -d "SQL_XSS_delete_team.csv"
```
