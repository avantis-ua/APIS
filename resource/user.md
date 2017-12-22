# Пользователь `user`
Ресурс `user` отвечает за работу с авторизационными данными пользователя, а это: регистрация пользователя, получение данных пользователя, изменение пароля, email или номера телефона. Этот ресурс должен позволять сделать все тоже, что позволяет сделать стандартный интернет-магазин работающий независимо. Вы можете сделать даже больше, внедрив вход под одной учетной записью на все сайты работающие через вашу API.

### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT`
- `POST /user` - Создание пользователя 
- `POST /user/{user_id}` - Ошибка
- `GET /user` - Список всех пользователей
- `GET /user?{param}` - Список всех пользователей с фильтром по параметрам
- `GET /user/{user_id}` - Данные конкретного пользователя по `user_id`
- `PUT /user` - Обновить данные пользователей
- `PUT /user/{user_id}` - Обновить данные конкретного пользователя по `user_id`

Какие поля должен поддерживать ресурс `user` вы можете посмотреть в структуре базы данных [db/user](https://github.com/pllano/db.json/blob/master/db/user.md)


### Параметр [`relations`](https://github.com/pllano/APIS-2018/blob/master/structure/relations.md)
[`relations`](https://github.com/pllano/APIS-2018/blob/master/structure/relations.md) - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.
 
Для передачи дополнительных параметров в `json` формате с последующим кодированием данных в формат MIME base64 функцией base64_encode
 
Параметры: Название связаного ресурса равно `"all"` или строка с параметрами

В нашем запросе к ресурсу `user` мы хотим дополнительно получить:
- из таблицы `address` - все данные

```
"relations": {
    "address": "all"
}
```
### Получить данные пользователя по `user_id`

``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get("public_key");

// Получить id пользователя
$user_id = 5;

// Строка запроса
$data = [
    "public_key" => $public_key,
    "relations": {
        "address": "all"
    }
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/user/".$user_id."?'.$data_query;

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем запрос
$response = $client->request("GET", $uri);

// Получаем тело ответа
$output = $response->getBody();

// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records["headers"]["code"])) {
    if ($records["headers"]["code"] == 200) {
	// Присвоим значения
	$user = $records["body"]["items"]["item"]["user"];
	$address = $records["body"]["items"]["item"]["address"];
 
        echo $user["fname"].' '.$user["iname"].' '.$user["oname"];
	// выведет: Иванов Юрий Петрович
	echo $address["city"].', '.$address["street"].' '.$address["number"];
	// выведет: Киев, Бажана 12а/17
    }
}
```
``` php
// Вывести на экран json
print_r($records);
```
### Структура ответа торговой площадки на наш `GET` запрос в json формате
```json
{
    "header": {
        "status": "200 OK",
        "code": "200",
        "message": "OK",
        "message_id": "https:\/\/github.com\/pllano\/api-json-db\/blob\/master\/doc\/http-codes\/200.md"
    },
    "response": {
        "auth": "CryptoAuth",
        "total": "1"
    },
    "request": {
        "query": "GET",
        "resource": "user",
        "user_id": "5",
        "relations": {
            "address": "all"
        }
    },
    "body": {
        "items": [{
                "item": {
                    "user_id": 5,
                    "role_id": 1,
                    "password": 0,
                    "password": "password hash",
                    "email": "user@example.com",
                    "phone": "380670000001",
                    "language": "ru",
                    "ticketed": 1,
                    "admin_access": 0,
                    "iname": "Юрий",
                    "fname": "Иванов",
                    "oname": "Петрович",
                    "cookie": "cookie hash",
                    "created": "2017-12-10 12:35",
                    "alias": "alias hash",
                    "state": 1,
                    "score": "",
                    "address": {
                        "country": "Украина",
                        "region": "Киевская область",
                        "postal_code": 0,
                        "city": "Киев",
                        "district": "Позняки",
                        "street": "Бажана",
                        "number": "12а/17",
                        "parade": "0",
                        "floor": "0",
                        "apartment": "75",
                        "additional": "Код на парадном 107"
                    }
                }
            }
        ]
    }
}
```
### Проверка запроса и обработка ошибок
Ваша API должна проверять запрашиваемые поля, и если данные отдавать не разрешено, не обрабатывать это поле. Если вы проигнорировали некоторые поля запроса, в конце обработки запроса, рекомендуется отправить `POST` на ресурс [`/feedback`](https://github.com/pllano/APIS-2018/blob/master/resource/feedback.md) в котором сообщить подробности об ошибке. Также необходимо в `request` вернуть те поля которые ваша API обработала успешно, и не выводить те, которые проигнорировала, чтобы запрашиваемая сторона могла обработать результат запроса и скорректировать его в будущем.
