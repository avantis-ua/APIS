# 
# Заказы `user`
Пример запроса продавца и ответа торговой площадки
### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT`
- `POST /user` - Создание пользователя 
- `POST /user/{user_id}` - Ошибка
- `GET /user` - Список всех пользователей
- `GET /user?{param}` - Список всех пользователей с фильтром по параметрам
- `GET /user/{user_id}` - Данные конкретного пользователя по `user_id`
- `PUT /user` - Обновить данные пользователей
- `PUT /user/{user_id}` - Обновить данные конкретного пользователя по `user_id`

### Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется четыри символа: `'` `.` `,` `:`

Сами ресурсы перечисляем через точку `'user_data'.'address'` экранируя одинарными кавычками
```
"relations" => "'user_data'.'address'"
```

Если необходимо запросить конкретные данные указываем их через двоеточие `'user_data:iname,fname,oname'` если нужно несколько через запятую `''user_data:iname,fname,oname'.'address''`

В нашем запросе к ресурсу `user` мы хотим дополнительно получить: 
- из таблицы `user_data` - поля: `iname` `fname` `oname`
- из таблицы `address` - все данные

```
"relations" => "'user_data:iname,fname,oname'.'address'"
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
    "relations" => "'user_data:iname,fname,oname'.'address'"
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
	$user_data = $records["body"]["items"]["item"]["user_data"];
	$address = $records["body"]["items"]["item"]["address"];
 
        echo $user_data["fname"].' '.$user_data["iname"].' '.$user_data["oname"];
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
        "relations": "'user_data:iname,fname,oname'.'address'"
    },
    "body": {
        "items": [{
                "item": {
                    "user_id": 5,
                    "role_id": 1,
                    "password": "password hash",
                    "phone": "380670000001",
                    "email": "user@example.com",
                    "language": "ru",
                    "cookie": "cookie hash",
                    "created": "2017-12-10 12:35",
                    "alias": "alias hash",
                    "state": 1,
                    "user_data": {
                        "iname": "Юрий",
                        "fname": "Иванов",
                        "oname": "Петрович"
                    },
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
