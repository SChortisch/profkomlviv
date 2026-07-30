# DDT / Newman — News API

Датасети та Newman-колекція для data-driven тестування `/api/news`.

**Команди запуску — у спільному [`DDT/README.md`](../README.md#ddt-news--apinews).**
Там же передумови, авторизація (спільний env-файл) і змінні під конкретний прогін.

## Що в теці

- `Newman DDT.postman_collection.json` — колекція, шість самодостатніх папок
  (create / update / delete та їхні SQL_XSS-двійники), без chaining.
- `ddt_*.csv` — функціональні датасети (стовпець `Expected result` — очікуваний
  HTTP-код, `scenario` — опис рядка).
- `SQL_XSS_*.csv` — security-датасети (`category`, `target_field`, `payload`;
  правило перевірки: будь-яка відповідь `2xx` = провал).
