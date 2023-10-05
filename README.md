# EdyaAPI Midjourney

Для использования API необходимо иметь токен доступа. Токен можно получить, в личном кабинете. Все обращения к API должны иметь в заголовке токен  ```api-key: <your_token>```

Base URL: https://api.userapi.ai

Bot: https://t.me/EdyaAIrobot

# Навигация

- [Imagine](https://github.com/blsdax/API-Midjourney#imagine)
- [Describe](https://github.com/blsdax/API-Midjourney#describe)
- [Upscale](https://github.com/blsdax/API-Midjourney#upscale)
- [Variation](https://github.com/blsdax/API-Midjourney#variation)
- [Blend](https://github.com/blsdax/API-Midjourney#blend)
- [Status](https://github.com/blsdax/API-Midjourney#status)

### Imagine

```
URL: /midjourney/v2/imagine

Метод: POST
```

Параметры запроса:
```
- prompt (required) - текстовый запрос.
- webhook_url (optional) - URL для оповещения
- webhook_type (optional) - Тип вебхука
- account_hash: (optional) - Hash ID аккаунта в системе
- is_disable_prefilter (optiona) - Пре-фильтер запрещенного контента
```
Пример запроса:
```
POST /midjourney/v2/imagine
Content-Type: application/json
api-key: <your_token>

{
  "prompt": "grass",
  "webhook_url": "https://example.com/imagine/webhook-url",
  "webhook_type": "progress", // or result
  "account_hash": "your_account_hash_id",
  "is_disable_prefilter": False
}
```

Пример ответа:
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "hash": "6794f6ef-866a-4bc3-b0bc-7b28ec010d1b"
}
```
###### В ответе возвращается hash задачи, это идентификатор по которому можно получить статус задачи

Пример тела запроса который будет отправлен по webhook_url:
```
{
  "account_hash": "grass",
  "hash": "6794f6ef-866a-4bc3-b0bc-7b28ec010d1b",
  "webhook_url":  "https://example.com/imagine/webhook-url",
  "webhook_type": "progress",
  "prompt": "grass",
  "type": "imagine",
  "progress": 100, // всегда 100, если status: done
  "status": "done",
  "result": {
      "url": "https://cdn.discordapp.com/...a88f0f357480.png",
      "proxy_url": "https://media.discordapp.net/...a88f0f357480.png",
      "filename": "...a88f0f357480.png",
      "content_type": "image/png",
      "width": 2048,
      "height": 2048,
      "size": 6950334
  },
  "status_reason": null,
  "prefilter_result": [],
  "created_at": "2006-01-02T15:04:05+00:00"
}
```

###Возможные значения элементов в **prefilter_result**:

- TOXICITY
- SEVERE_TOXICITY
- IDENTITY_ATTACK
- INSULT
- PROFANITY
- THREAT
- SEXUALLY_EXPLICIT
- FLIRTATION

###Возможные статусы (status) задачи: 

| status       | описание              
| ------------- |:------------------:| 
| sent     | задача  успешно отослана боту для обработки    | 
| waiting     | MJ принял задачу,  она стоит в очереди у MJ на обработку | 
| progress  | обновление прогресса по задаче, поле progress будет содержать прогресс в диапазоне от 0 до 100. Этот статус может быть для  type задачи imagine и upscale         | 
| done  | задача выполнена, в зависимости от типа задачи         | 
| error  | задача не выполнена. В поле status_reason при этом указывается причина         | 
---

## Describe

```
URL: /midjourney/v2/describe

Метод: POST
```
Параметры запроса:
```
- url (required) - URL к изображению.
- webhook_url (optional) - URL для оповещения
- webhook_type (optional) - Тип вебхука
- account_hash: (optional) - Hash ID аккаунта в системе
```
Пример запроса:
```
POST /midjourney/v2/describe
Content-Type: application/json
api-key: <your_token>

{
  "url": "https://example.com/image.jpg",
  "webhook_url": "https://example.com/describe/webhook-url",
  "webhook_type": "result",
  "account_hash": "a7d44910-88a6-4673-acc8-d60ffc3479a6"
}
```

Пример ответа:
```
{
  "account_hash": "a7d44910-88a6-4673-acc8-d60ffc3479a6",
  "hash": "2d7116bf-3006-41e7-9fcc-f1628fc4693a",
  "webhook_url": "https://example.com/describe/webhook-url",
  "webhook_type": "result",
  "prompt": "https://example.com/50a9.jpg",
  "type": "describe",
  "status": "done",
  "result": [
      "nice girl in the hat",
      "girl in the nice hat",
      "cute girl in the hat",
      "girl in the little hat"
  ],
  "status_reason": null,
  "created_at": "2006-01-02T15:04:05+00:00"
}
```
---
## Upscale
```
URL: /midjourney/v2/upscale

Метод: POST
```

Параметры запроса:
```
hash (required) - Hash задачи
choice (required) - Значение кнопки
webhook_url (optional) - URL для оповещения
webhook_type (optional) - Тип вебхука

```
Пример запроса:
```
POST /api/image HTTP/1.1
Content-Type: application/json
api-key: <your_token>

{
  "hash": "6794f6ef-866a-4bc3-b0bc-7b28ec010d1b",
  "choice": 3, // must be in [1..4],
  "webhook_url":  "https://example.com/upscale/webhook-url",
  "webhook_type": "result"
}
```

Пример ответа:
```
{
  "hash": "2c8abb2d-5f4d-4c55-8830-102a4fe754ed"
}
```

Пример тела запроса который будет отправлен по webhook_url:
```
{
  "account_hash": "a7d44910-88a6-4673-acc8-d60ffc3479a6",
  "hash": "2c8abb2d-5f4d-4c55-8830-102a4fe754ed",
  "webhook_url": null,
  "webhook_type": null,
  "choice": 3,
  "type": "upscale",
  "status": "done",
  "result": {
      "url": "https://cdn.discordapp.com/...039b7de3d18f.png",
      "proxy_url": "https://media.discordapp.net/...039b7de3d18f.png",
      "filename": "...039b7de3d18f.png",
      "content_type": "image/png",
      "width": 1024,
      "height": 1024,
      "size": 1512812
  },
  "status_reason": null,
  "created_at": "2006-01-02T15:04:05+00:00"
}
```
---
## Variation
```
URL: /midjourney/v2/variation

Метод: POST
```

Параметры запроса:
```
hash (required) - Hash задачи
choice (required) - Значение кнопки
webhook_url (optional) - URL для оповещения
webhook_type (optional) - Тип вебхука

```
Пример запроса:
```
POST /api/image HTTP/1.1
Content-Type: application/json
api-key: <your_token>

{
  "hash": "6794f6ef-866a-4bc3-b0bc-7b28ec010d1b",
  "choice": 3, // must be in [1..4],
  "webhook_url":  "https://example.com/upscale/webhook-url",
  "webhook_type": "result"
}
```

Пример ответа:
```
{
  "hash": "2c8abb2d-5f4d-4c55-8830-102a4fe754ed"
}
```

Пример тела запроса который будет отправлен по webhook_url:
```
{
  "account_hash": "a7d44910-88a6-4673-acc8-d60ffc3479a6",
  "hash": "c688a245-3964-4286-9572-d6c7e22f589a",
  "webhook_url": null,
  "webhook_type": null,
  "choice": 3,
  "type": "variation",
  "progress": 100,
  "status": "done",
  "result": {
      "url": "https://cdn.discordapp.com/...515a41ad678f.png",
      "proxy_url": "https://media.discordapp.net/...515a41ad678f.png",
      "filename": "...-515a41ad678f.png",
      "content_type": "image/png",
      "width": 2048,
      "height": 2048,
      "size": 7003237
  },
  "status_reason": null,
  "created_at": "2006-01-02T15:04:05+00:00"
}
```
## Blend
```
URL: /midjourney/v2/variation

Метод: POST
```

Параметры запроса:
```
urls (required) - Массив из URL
webhook_url (optional) - URL для оповещения
webhook_type (optional) - Тип вебхука
account_hash (optional) - Hash аккаунта в системе
```
Пример запроса:
```
POST /api/image HTTP/1.1
Content-Type: application/json
api-key: <your_token>

{
  "urls": [
      {
          "url": "https://example.com/image1.jpg",
          "ext": "jpg" // or png
      },
      {
          "url": "https://example.com/image2.png",
          "ext": "png"
      }
  ],
  "webhook_url":  "https://example.com/blend/webhook-url",
  "webhook_type": "result",
  "account_hash": "a7d44910-88a6-4673-acc8-d60ffc3479a6"
}
```

Пример ответа:
```
{
  "hash": "8fc469bf-3f5f-341e-c9fc-2d7116f1623a"
}
```

Пример тела запроса который будет отправлен по webhook_url:
```
{
  "account_hash": "3b2c7341-ec22-0301-810d-9cf0543e20ba",
  "hash": "fd88479e-455f-6a6f-6284-b0c49117d76f",
  "webhook_url": "https://example.com/blend/webhook-url",
  "webhook_type": "progress",
  "urls": [
      {
          "url": "https://example.com/image1.jpg",
          "ext": "jpg"
      },
      {
          "url": "https://example.com/image1.jpg",
          "ext": "jpg"
      }
  ],
  "type": "blend",
  "status": "done",
  "result": {
      "url": "https://cdn.discordapp.com/...6a65992f3587.png",
      "proxy_url": "https://media.discordapp.net/a56394/...6a65992f3587.png",
      "filename": "...-6a65992f3587.png",
      "content_type": "image/png",
      "width": 2048,
      "height": 2048,
      "size": 2145293
  },
  "status_reason": null,
  "created_at": "2006-01-02T15:04:05+00:00"
}
```
## Status
```
URL: /midjourney/v2/status?hash={hash}

Метод: GET
```
Данный  метод по hash возвращает статус задачи. Структура ответа такая же, как и приходящая в вебхуке для каждого type задачи
