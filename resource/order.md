# Заказы `order`
Ресурс `order` отвечает за работу с заказами, а это: создание заказа, вывод данных по заказу, добавление или изменение данных итд. Этот ресурс должен позволять сделать все тоже, что позволяет сделать стандартный интернет-магазин работающий независимо. Вы можете сделать даже больше, внедрив единую корзину и одну учетною запись на все сайты работающие через вашу API.

### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT` `PATCH`
- `POST /order` - Создание заказа 
- `POST /order/{order_id}` - Ошибка
- `GET /order` - Список всех заказов
- `GET /order?{param}` - Список всех заказов с фильтром по параметрам
- `GET /order/{order_id}` - Данные конкретного заказа по `order_id`
- `PUT /order` - Обновить данные заказов
- `PUT /order/{order_id}` - Обновить данные конкретного заказа по `order_id`
- `PATCH /order/` Обновить данные заказов
- `PATCH /order//{order_id}` Обновить данные конкретной заказа по `order_id`

### Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется четыри символа: `'` `.` `,` `:`

Сами ресурсы перечисляем через точку `'product'.'user'.'user_data'.'address'` экранируя одинарными кавычками
```
"relations" => "'product'.'user'.'user_data'.'address'"
```

Если необходимо запросить конкретные данные указываем их через двоеточие, если нужно несколько через запятую `'product'.'user:email,phone'.'user_data:iname,fname,oname'.'address'`

В нашем запросе к ресурсу `user` мы хотим дополнительно получить:
- из таблицы `product` - все данные
- из таблицы `user` - поля: `email` `phone`
- из таблицы `user_data` - поля: `iname` `fname` `oname`
- из таблицы `address` - все данные
```
"relations" => "'product'.'user:email,phone'.'user_data:iname,fname,oname'.'address'"
```
### Пример `GET` запроса HTTP клиентом Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get('public_key');

// Наш запрос
$data = [
    'public_key' => $public_key,
    'limit' => 10,
    'offset' => 0,
    'order' => "DESC",
    'sort' => "created",
    'state' => 1,
    'date_from' => "2017-12-07",
    'date_to' => "2017-12-14",
    'relations' => "'product'.'user:email,phone'.'user_data:iname,fname,oname'.'address'"
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/order?'.$data_query;

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем запрос
$response = $client->request('GET', $uri);

// Получаем тело ответа
$output = $response->getBody();

// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records['headers']['code'])) {
if ($records['headers']['code'] == '200') {
	$count = count($records['body']['items']);
	if ($count >= 1) {
		foreach($records['body']['items'] as $item)
		{
			print_r($item['item']);
		}
	}
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
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/200.md"
    },
    "response": {
        "auth": "QueryKeyAuth",
        "total": "10"
    },
    "request": {
        "query": "GET",
        "resource": "order",
        "limit": "10",
        "offset": "0",
        "order": "DESC",
        "sort": "created",
        "state": "1",
        "date_from": "2017-12-07",
        "date_to": "2017-12-14",
        "relations": "'product'.'user:email,phone'.'user_data:iname,fname,oname'.'address'"
    },
    "body": {
        "items": [{
                "item": {
                    "id": "1234567890",
                    "created": "2017-12-11 10:30",
                    "status": 1,
                    "delivery": "Novaposhta",
                    "delivery_code": "1234567890121",
                    "total_amount": "10501.00",
                    "currency_id": "UAH",
                    "comment": "Могу принять после 17:00",
                    "user": {
                        "phone": "380670000001",
                        "email": "user@example.com"
                    },
                    "user_data": {
                        "fname": "Иванов",
                        "iname": "Юрий",
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
                    },
                    "products": [{
                            "product": {
                                "product_id": "2001500",
                                "status": 1,
                                "name": "Type BrendName SerieName Articul",
                                "price": "2500.50",
                                "oldprice": "2500.50",
                                "num": 2,
                                "available": 20,
                                "total_price": "5001.00",
                                "currency_id": "UAH",
                                "guarantee": 24,
                                "pay_online": 1
                            }
                        }, {
                            "product": {
                                "product_id": "1000120",
                                "status": 1,
                                "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
                                "type": "Ноутбук",
                                "brand": "Asus",
                                "type": "X751NV",
                                "articul": "(X751NV-TY001) Black",
                                "price": "5750.50",
                                "oldprice": "5500.00",
                                "num": 1,
                                "available": 5,
                                "total_price": "5500.00",
                                "currency_id": "UAH",
                                "guarantee": 36,
                                "pay_online": 1
                            }
                        }
                    ]
                }
            }
        ]
    }
}
```
### Получить данные заказа по `order_id`

``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get('public_key');

// Получить id заказа в своей базе
$order_id = 10;

// Строка запроса
$data = [
    'public_key' => $public_key,
    'relations' => "'product'.'user'.'user_data'.'address'"
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/order/'.$order_id.'?'.$data_query;

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем запрос
$response = $client->request('GET', $uri);

// Получаем тело ответа
$output = $response->getBody();

// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records["headers"]["code"])) {
    if ($records["headers"]["code"] == 200) {
	// Присвоим значения
        $order = $records["body"]["items"]["item"];
	$user = $records["body"]["items"]["item"]["user"];
	$user_data = $records["body"]["items"]["item"]["user_data"];
	$address = $records["body"]["items"]["item"]["address"];
	$products = $records["body"]["items"]["item"]["products"];
	
        echo $order["order_id"];
	// выведет: 10
        echo $user["email"];
	// выведет: user@example.com
        echo $user_data["fname"].' '.$user_data["iname"];
	// выведет: Иванов Юрий
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
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/200.md"
    },
    "response": {
        "auth": "QueryKeyAuth",
        "total": "1"
    },
    "request": {
        "query": "GET",
        "resource": "order",
        "order_id": "10",
        "relations": "'product'.'user:email,phone'.'user_data:iname,fname,oname'.'address'"
    },
    "body": {
        "items": [{
                "item": {
                    "order_id": "10",
                    "created": "2017-12-11 10:30",
                    "status": 1,
                    "delivery": "Novaposhta",
                    "delivery_code": "1234567890121",
                    "total_amount": "10501.00",
                    "currency_id": "UAH",
                    "comment": "Могу принять после 17:00",
                    "user": {
                        "phone": "380670000001",
                        "email": "user@example.com"
                    },
                    "user_data": {
                        "fname": "Иванов",
                        "iname": "Юрий",
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
                    },
                    "products": [{
                            "product": {
                                "product_id": "2001500",
                                "status": 1,
                                "name": "Type BrendName SerieName Articul",
                                "price": "2500.50",
                                "oldprice": "2500.50",
                                "num": 2,
                                "available": 20,
                                "total_price": "5001.00",
                                "currency_id": "UAH",
                                "guarantee": 24,
                                "pay_online": 1
                            }
                        }, {
                            "product": {
                                "product_id": "1000120",
                                "status": 1,
                                "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
                                "type": "Ноутбук",
                                "brand": "Asus",
                                "type": "X751NV",
                                "articul": "(X751NV-TY001) Black",
                                "price": "5750.50",
                                "oldprice": "5500.00",
                                "num": 1,
                                "available": 5,
                                "total_price": "5500.00",
                                "currency_id": "UAH",
                                "guarantee": 36,
                                "pay_online": 1
                            }
                        }
                    ]
                }
            }
        ]
    }
}

```
### Проверка запроса и обработка ошибок
Ваша API должна проверять запрашиваемые поля, и если данные отдавать не разрешено, не обрабатывать это поле. Если вы проигнорировали некоторые поля запроса, в конце обработки запроса, рекомендуется отправить `POST` на ресурс [`/feedback`](https://github.com/pllano/APIS-2018/blob/master/resource/feedback.md) в котором сообщить подробности об ошибке. Также необходимо в `request` вернуть те поля которые ваша API обработала успешно, и не выводить те, которые проигнорировала, чтобы запрашиваемая сторона могла обработать результат запроса и скорректировать его в будущем.
