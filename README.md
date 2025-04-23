# 📘 ERP API: Управление офисами

> Управление офисами реализовано через модуль `erp.collection_point`.

## 🔐 Аутентификация

Все методы требуют заголовок:

X-Api-Key: {your_api_key}

При отсутствии или неправильном API-ключе:
```json
{
  "status": "fail",
  "error": "Bad API key",
  "code": 2
}
```

## 📥 Получить офис по `erp_medoffice_id`

Метод: GET /api/erp/office/{erp_medoffice_id}

Пример запроса:

GET /api/erp/office/1  
X-Api-Key: your_api_key

Ответ 200:
```json
{
  "status": "success",
  "data": {
    "id": 1,
    "erp_medoffice_id": 1,
    "name": "МО Москва",
    "city": "Москва",
    "address": "Москва, ул. НазваниеУлицы, дом 20",
    "phones": "доб. 4017,4023",
    "is_deleted": 0
  }
}
```
Ошибка 404:
```json
{
  "status": "fail",
  "error": "Офис не найден",
  "code": 8
}
```
## ➕ Создать офис

Метод: POST /api/erp/office  
Content-Type: application/json

Тело запроса:

erp_medoffice_id (int, обяз.) — Идентификатор офиса в ERP  
name (string, обяз.) — Название  
city (string, обяз.) — Город  
address (string, обяз.) — Адрес  
phones (string, обяз.) — Телефоны через запятую

Пример:
```json
{
  "erp_medoffice_id": 1,
  "name": "МО Москва",
  "city": "Москва",
  "address": "Москва, ул. НазваниеУлицы, дом 20",
  "phones": "доб. 4017,4023"
}
```
Ответ 200: (как GET)

Ошибка — отсутствуют параметры:
```json
{
  "status": "fail",
  "error": "Не переданы обязательные параметры: name, city",
  "code": 10
}
```
Ошибка — офис уже существует:
```json
{
  "status": "fail",
  "error": "Офис с таким erp_medoffice_id уже существует",
  "code": 11
}
```
Ошибка валидации:
```json
{
  "status": "fail",
  "error": "Ошибка валидации: name cannot be blank",
  "code": 12
}
```
## 🔄 Обновить офис

Метод: PUT /api/erp/office/{erp_medoffice_id}  
Content-Type: application/json

Пример запроса:
```json
{
  "name": "МО Москва",
  "city": "Москва",
  "address": "Москва, ул. НазваниеУлицы, дом 20",
  "phones": "доб. 4017,4023"
}
```
Ответ 200: (как GET)

Ошибка 404:
```json
{
  "status": "fail",
  "error": "Офис не найден",
  "code": 8
}
```
Ошибка валидации:
```json
{
  "status": "fail",
  "error": "Ошибка валидации: name is too long",
  "code": 12
}
```
## 🚫 Изменить активность офиса

Метод: PUT /api/erp/office/{erp_medoffice_id}/change-active  
Content-Type: application/json

Пример:
```json
{
  "is_deleted": 1
}
```
Ответ 200: (как GET, только `is_deleted` изменён)

Ошибка — не передан is_deleted:
```json
{
  "status": "fail",
  "error": "Не передан обязательный параметр is_deleted",
  "code": 10
}
```
Ошибка 404:
```json
{
  "status": "fail",
  "error": "Офис не найден",
  "code": 8
}
```
## 🧱 Структура объекта офиса

id (int) — Внутренний ID  
erp_medoffice_id (int) — Внешний ID ERP  
name (string) — Название  
city (string) — Город  
address (string) — Адрес  
phones (string) — Телефоны (через запятую)  
is_deleted (int) — 0 — активен, 1 — удалён

## 🧾 Коды ошибок API

Описание всех возможных кодов ошибок, возвращаемых в поле `code` при статусе `"fail"`:

| Код | Константа              | Описание                                                          |
|-----|-------------------------|-------------------------------------------------------------------|
| 2   | `CODE_BAD_API`          | Неверный API-ключ. Клиент не прошёл проверку авторизации.        |
| 8   | `CODE_NOT_FOUND`        | Запрашиваемый объект (например, офис) не найден.                 |
| 9   | `CODE_INVALID_FORMAT`   | Ошибка формата данных — например, тело запроса невалидный JSON. |
| 10  | `CODE_REQUIRED_PARAM`   | Не переданы обязательные параметры запроса.                      |
| 11  | `CODE_ALREADY_EXIST`    | Объект с таким значением уже существует (например, ID офиса).    |
| 12  | `CODE_VALIDATION`       | Ошибка валидации модели — некорректные или пустые значения.     |

