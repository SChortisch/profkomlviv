# Profkom API — довідник ендпоінтів (Swagger v1)

> Згенеровано зі `swagger.json` (`openapi: 3.0.1`, `title: Profkom API`, `version: v1`).
> Авторизація: `Bearer` JWT (`Authorization: Bearer <token>`) — застосовується глобально до всього API.

---

## Admin

### `POST /api/Admin/login`
Body (`application/json`): `AuthRequest`
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| username | string | так | minLength: 1, maxLength: 100 |
| password | string | так | minLength: 1, maxLength: 100 |

Response: `200 Success`

### `GET /api/Admin/list`
Без параметрів.
Response: `200 Success`

### `POST /api/Admin/create`
Body (`application/json`): `AdminCreateRequest`
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| username | string, nullable | ні | — |
| password | string, nullable | ні | — |
| role | string, nullable | ні | — |

Response: `200 Success`

### `PUT /api/Admin/edit/{id}`
Path params: `id` (integer, int32, required)
Body (`application/json`): `AdminEditRequest`
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| password | string, nullable | ні | — |
| role | string, nullable | ні | — |

Response: `200 Success`

### `DELETE /api/Admin/delete/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

**Практично, зміна username/password (для синхронізації з DDT-даними):**
- Пароль хешується BCrypt (`Admin.PasswordHash`) — міняти лише через `PUT /api/Admin/edit/{id}` (поле `password` у тілі), напряму в БД через SQL не можна (`UPDATE ... SET PasswordHash='текст'` зламає логін).
- Валідація пароля (і при `create`, і при `edit`): `^(?=.*[A-Z])(?=.*\d)(?=.*[\W_]).{8,}$` — мін. 8 символів, 1 велика літера, 1 цифра, 1 спецсимвол.
- `AdminEditRequest` не має поля `username` (див. схему нижче) — змінити логін можна лише напряму в БД: `UPDATE Admins SET Username='...' WHERE Id=...` (це безпечно, бо `Username`, на відміну від пароля, не хешується).
- Seed-акаунти (`backend/Backend/Data/DbInitializer.cs`) додаються, лише якщо такого `Username` ще нема в БД — правка seed-файлу **не оновить** вже існуючі записи, вплине тільки на нову/порожню базу.

---

## Department

### `GET /api/departments`
Response: `200 Success` — масив `Department[]`

### `POST /api/departments`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| HeadId | integer, int32 | ні | — |
| Name | string | ні (у формі, хоча в моделі `Department.name` required, minLength:1) | — |
| Description | string | ні | — |
| LogoUrl | string | ні | — |
| IsActive | boolean | ні | — |
| Logo | file (binary) | ні | — |

Response: `200 Success` — `Department`

### `GET /api/departments/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success` — `Department`

### `PUT /api/departments/{id}`
Path params: `id` (integer, int32, required)
Body (`multipart/form-data`): те саме, що `POST /api/departments`
Response: `200 Success`

### `DELETE /api/departments/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## Documents

### `GET /api/documents`
Response: `200 Success` — масив `Document[]`

### `POST /api/documents`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| Title | string | так | — (без явних minLength/maxLength у схемі) |
| Description | string | ні | — |
| File | file (binary) | ні | — |

Response: `200 Success` — `Document`

### `GET /api/documents/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success` — `Document`

### `PUT /api/documents/{id}`
Path params: `id` (integer, int32, required)
Body: те саме, що `POST /api/documents` (Title required)
Response: `200 Success`

### `DELETE /api/documents/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## Events

### `GET /api/Events`
Response: `200 Success`

### `POST /api/Events`
Body (`application/json`): `Event`
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| title | string | так | minLength: 1 |
| description | string, nullable | ні | — |
| startsAt | string (date-time) | ні | — |
| endsAt | string (date-time) | ні | — |
| location | string, nullable | ні | — |
| imageUrl | string, nullable | ні | — |

Response: `200 Success`

### `GET /api/Events/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

### `PUT /api/Events/{id}`
Path params: `id` (integer, int32, required)
Body (`application/json`): `Event` (як у POST)
Response: `200 Success`

### `DELETE /api/Events/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## Faculty

### `GET /api/faculties`
Response: `200 Success` — масив `Faculty[]`

### `POST /api/faculties`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| Name | string | ні | — |
| HeadId | integer, int32 | ні | — |
| Address | string | ні | — |
| Room | string | ні | — |
| Instagram_Link | string | ні | — |
| Telegram_Link | string | ні | — |
| ImageUrl | string | ні | — |
| Schedule | string | ні | — |
| Summary | string | ні | — |
| IsActive | boolean | ні | — |
| IsCollege | boolean | ні | — |
| Image | file (binary) | ні | — |

Response: `200 Success` — `Faculty`

### `GET /api/faculties/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success` — `Faculty`

### `PUT /api/faculties/{id}`
Path params: `id` (integer, int32, required)
Body: те саме, що `POST /api/faculties`
Response: `200 Success`

### `DELETE /api/faculties/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## News

### `GET /api/News`
Response: `200 Success`

### `POST /api/News`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| Title | string | так | — (без явних minLength/maxLength у схемі) |
| Content | string | ні | — |
| Images | array of file (binary) | ні | — |
| IsImportant | boolean | ні | — |

Response: `200 Success`

### `GET /api/News/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

### `PUT /api/News/{id}`
Path params: `id` (integer, int32, required)
Body: те саме, що `POST /api/News` (Title required)
Response: `200 Success`

### `DELETE /api/News/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

### `DELETE /api/News/image/{imageId}`
Path params: `imageId` (integer, int32, required)
Response: `200 Success`

---

## Team

### `GET /api/Team`
Response: `200 Success` — масив `Team[]`

### `POST /api/Team`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| Name | string | ні (у моделі `Team.name` required, minLength:1) | — |
| Position | string | ні (у моделі required, minLength:1) | — |
| Type | `MemberType` (enum: 0, 1, 2) | ні (у моделі required) | — |
| Email | string | ні | — |
| OrderInd | integer, int32 | ні (у моделі required) | — |
| IsTemporary | boolean | ні (у моделі required) | — |
| ImageUrl | string | ні | — |
| Image | file (binary) | ні | — |
| IsChoosed | boolean | ні | — |

Response: `200 Success` — `Team`

### `GET /api/Team/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success` — `Team`

### `PUT /api/Team/{id}`
Path params: `id` (integer, int32, required)
Body: те саме, що `POST /api/Team`
Response: `200 Success`

### `DELETE /api/Team/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## Unit

### `GET /api/Unit`
Response: `200 Success` — масив `Unit[]`

### `POST /api/Unit`
Body (`multipart/form-data`):
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| Name | string | ні | — |
| Content | string | ні | — |
| OrderInd | integer, int32 | ні | — |
| IsActive | boolean | ні | — |
| ImageUrl | string | ні | — |
| Image | file (binary) | ні | — |

Response: `200 Success` — `Unit`

### `GET /api/Unit/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success` — `Unit`

### `PUT /api/Unit/{id}`
Path params: `id` (integer, int32, required)
Body: те саме, що `POST /api/Unit`
Response: `200 Success`

### `DELETE /api/Unit/{id}`
Path params: `id` (integer, int32, required)
Response: `200 Success`

---

## Моделі даних (schemas)

### AuthRequest
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| username | string | так | minLength:1, maxLength:100 |
| password | string | так | minLength:1, maxLength:100 |

### AdminCreateRequest
| Поле | Тип | Обов'язкове |
|---|---|---|
| username | string, nullable | ні |
| password | string, nullable | ні |
| role | string, nullable | ні |

### AdminEditRequest
| Поле | Тип | Обов'язкове |
|---|---|---|
| password | string, nullable | ні |
| role | string, nullable | ні |

### Department
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| id | integer, int32 | — | — |
| name | string | так | minLength:1 |
| description | string, nullable | ні | — |
| logoUrl | string, nullable | ні | — |
| headId | integer, int32, nullable | ні | — |
| head | `Team` | ні | — |
| isActive | boolean | — | — |
| createdAt | date-time | — | — |
| updatedAt | date-time, nullable | ні | — |

### Document
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| id | integer, int32 | — | — |
| title | string | так | minLength:1 |
| description | string, nullable | ні | — |
| filePath | string | так | minLength:1 |
| fileSize | integer, int64 | — | — |
| createdAt | date-time | — | — |
| updatedAt | date-time, nullable | ні | — |

### Event
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| id | integer, int32 | — | — |
| title | string | так | minLength:1 |
| description | string, nullable | ні | — |
| startsAt | date-time | — | — |
| endsAt | date-time | — | — |
| location | string, nullable | ні | — |
| imageUrl | string, nullable | ні | — |

### Faculty
| Поле | Тип | Обов'язкове |
|---|---|---|
| id | integer, int32 | — |
| name | string, nullable | ні |
| headId | integer, int32, nullable | ні |
| head | `Team` | ні |
| address | string, nullable | ні |
| room | string, nullable | ні |
| schedule | string, nullable | ні |
| summary | string, nullable | ні |
| instagram_Link | string, nullable | ні |
| telegram_Link | string, nullable | ні |
| isActive | boolean | — |
| isCollege | boolean | — |
| imageUrl | string, nullable | ні |
| createdAt | date-time | — |
| updatedAt | date-time, nullable | ні |

### MemberType (enum)
`0`, `1`, `2` (integer, int32)

### Team
| Поле | Тип | Обов'язкове | Обмеження |
|---|---|---|---|
| id | integer, int32 | — | — |
| name | string | так | minLength:1 |
| position | string | так | minLength:1 |
| type | `MemberType` | так | — |
| imageUrl | string, nullable | ні | — |
| email | string, nullable | ні | — |
| orderInd | integer, int32 | так | — |
| isTemporary | boolean | так | — |
| createdAt | date-time | — | — |
| updatedAt | date-time | — | — |
| isChoosed | boolean | — | — |

### Unit
| Поле | Тип | Обов'язкове |
|---|---|---|
| id | integer, int32 | — |
| name | string, nullable | ні |
| content | string, nullable | ні |
| orderInd | integer, int32 | — |
| is_active | boolean | — |
| imageUrl | string, nullable | ні |
| createdAt | date-time | — |
| updatedAt | date-time, nullable | ні |

---

## Примітки для тестування

- Поля `Title`/`Content` у `News` та `Title`/`Description` у `Documents` **не мають** явних `minLength`/`maxLength` у Swagger-схемі (на відміну від `AuthRequest.username/password`, обмежених 100 символами) — це не гарантує відсутність валідації в коді контролера, лише те, що вона не задекларована через атрибути моделі й не потрапила в OpenAPI.
- Ендпоінти зі списками сутностей (`Department`, `Faculty`, `Team`, `Unit`, `Documents`) використовують `multipart/form-data` для create/edit (через завантаження файлу зображення/документа), тоді як `Admin`, `Events` використовують `application/json`.
- Усі `id` в path — `integer/int32`, обов'язкові.
- Авторизація `Bearer` застосована глобально до всього API (`security` на рівні документа) — для запитів у Postman потрібен валідний токен, отриманий через `POST /api/Admin/login`.
