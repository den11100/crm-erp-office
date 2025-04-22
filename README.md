## Аутентификация
Все методы требуют передачи заголовка:
`X-Api-Key: {your_api_key}`

При отсутствии или некорректном ключе возвращается ошибка **400 Bad Request**:
```json
{
  "name": "Bad Request",
  "message": "Bad API key",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

## Получить информацию об офисе по `erp_medoffice_id`
**GET** `/api/erp/office/{erp_medoffice_id}`

Пример запроса:
```http
GET /api/erp/office/1
Host: api.example.com
X-Api-Key: your_api_key
```

Пример успешного ответа 200:
```json
{
  "status": 200,
  "data": {
    "id": "1",
    "erp_medoffice_id": "1",
    "name": "МО Москва",
    "city": "Москва",
    "address": "Москва, ул. НазваниеУлицы, дом 20",
    "phones": "доб. 4017,4023",
    "is_deleted": "0"
  }
}
```

Ошибка 404:
```json
{
  "name": "Not Found",
  "message": "Офис не найден",
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```

## Создать новый офис
**POST** `/api/erp/office`

Тело запроса (JSON):

| Поле              | Обязат. | Тип    | Описание                              |
|-------------------|---------|--------|--------------------------------------|
| erp_medoffice_id  | Да      | int    | ID офиса из ERP                      |
| name              | Да      | string | Название офиса                       |
| city              | Да      | string | Город                                |
| address           | Да      | string | Адрес                                |
| phones            | Да      | string | Телефоны, разделённые запятыми       |

Пример запроса:
```http
POST /api/erp/office
Content-Type: application/json
X-Api-Key: your_api_key
```
```json
{
  "erp_medoffice_id": 1,
  "name": "МО Москва",
  "city": "Москва",
  "address": "Москва, ул. НазваниеУлицы, дом 20",
  "phones": "доб. 4017,4023"
}
```

Пример ответа 200:
```json
{
  "status": 200,
  "data": {
    "id": "1",
    "erp_medoffice_id": "1",
    "name": "МО Москва",
    "city": "Москва",
    "address": "Москва, ул. НазваниеУлицы, дом 20",
    "phones": "доб. 4017,4023",
    "is_deleted": "0"
  }
}
```

Ошибка 400:
```json
{
  "name": "Bad Request",
  "message": "Не переданы обязательные параметры или пустые значения: name, city",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Ошибка 422:
```json
{
  "name": "Error",
  "message": "Ошибка валидации: name cannot be blank",
  "status": 422,
  "type": "yii\\web\\HttpException"
}
```

## Обновить офис
**PUT** `/api/erp/office/{erp_medoffice_id}`

Пример запроса:
```http
PUT /api/erp/office/1
Content-Type: application/json
X-Api-Key: your_api_key
```
```json
{
  "name": "МО Москва",
  "city": "Москва",
  "address": "Москва, ул. НазваниеУлицы, дом 20",
  "phones": "доб. 4017,4023"
}
```

Пример ответа:
```json
{
  "status": 200,
  "data": {
    "id": "1",
    "erp_medoffice_id": "1",
    "name": "МО Москва",
    "city": "Москва",
    "address": "Москва, ул. НазваниеУлицы, дом 20",
    "phones": "доб. 4017,4023",
    "is_deleted": "0"
  }
}
```

Ошибка 404:
```json
{
  "name": "Not Found",
  "message": "Офис не найден",
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```

Ошибка 422:
```json
{
  "name": "Error",
  "message": "Ошибка валидации: name is too long",
  "status": 422,
  "type": "yii\\web\\HttpException"
}
```

## Изменить активность офиса
**PUT** `/api/erp/office/{erp_medoffice_id}/change-active`

Пример запроса:
```http
PUT /api/erp/office/1/change-active
Content-Type: application/json
X-Api-Key: your_api_key
```
```json
{
  "is_deleted": 1
}
```

Пример ответа:
```json
{
  "status": 200,
  "data": {
    "id": "1",
    "erp_medoffice_id": "1",
    "name": "МО Москва",
    "city": "Москва",
    "address": "Москва, ул. НазваниеУлицы, дом 20",
    "phones": "доб. 4017,4023",
    "is_deleted": "1"
  }
}
```

Ошибка 400:
```json
{
  "name": "Bad Request",
  "message": "Не передан обязательный параметр is_deleted",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```

Ошибка 404:
```json
{
  "name": "Not Found",
  "message": "Офис не найден",
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```

## Структура объекта офиса в ответах (Возвращаются всегда строки, приводить тип параметра на своей стороне самостоятельно)

| Поле              | Тип    | Описание                              |
|-------------------|--------|---------------------------------------|
| id                | int    | Внутренний ID                         |
| erp_medoffice_id  | int    | Внешний ID ERP                        |
| name              | string | Название                              |
| city              | string | Город                                 |
| address           | string | Адрес                                 |
| phones            | string | Телефоны, строка с номерами через `,` |
| is_deleted        | int    | 0 — активен, 1 — удалён               |


