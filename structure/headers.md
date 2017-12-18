# Заголовки `headers`
```json
{
    "headers": {
        "status": "200 OK",
        "code": 200,
        "message": "RESTfull API json DB works!",
        "message_id": "https:\/\/github.com\/pllano\/api-json-db\/blob\/master\/doc\/http-codes\/200.md"
    }
}
```
## status
Всегда указывайте реальный статус в отличии от заголовков http, где желательно всегда указывать `200 OK`
## code
Поле для обработки принимающей стороной. Формат целое число соответствующее одному из состояний указанных в разделе [`Коды состояния HTTP`](https://github.com/pllano/api-json-db/tree/master/doc/http-codes)
## message
Сообщение не предназначено для обработки. Оно предназначено для информирования о реальном состоянии. Вы вольны выводить в нем любую информацию на свое усмотрение.
## message_id
Вы можете использовать ссылки на раздел: [`Коды состояния HTTP`](https://github.com/pllano/api-json-db/tree/master/doc/http-codes)

`URL`: `https://github.com/pllano/api-json-db/blob/master/doc/http-codes/{code}.md`
