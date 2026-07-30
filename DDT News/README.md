```
npx newman run "Newman DDT.postman_collection.json" --folder "Create news DDT" -d "ddt_create_news.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Create news" -d "SQL_XSS_create_news.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Update news DDT" -d "ddt_update_news.csv" --env-var news_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Update news" -d "SQL_XSS_update_news.csv" --env-var news_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "Delete news DDT" -d "ddt_delete_news.csv" --env-var news_target_id=<ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete news" -d "SQL_XSS_delete_news.csv"

npx newman run "Newman DDT.postman_collection.json" --folder "Delete news image DDT" -d "ddt_delete_news_image.csv" --env-var news_image_target_id=<IMAGE_ID>

npx newman run "Newman DDT.postman_collection.json" --folder "SQL_XSS Delete news image" -d "SQL_XSS_delete_news_image.csv"
```
