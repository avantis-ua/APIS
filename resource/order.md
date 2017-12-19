# Заказы `order`
Пример запроса продавца и ответа торговой площадки
### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT`
- `POST /order` Создание заказа 
- `POST /order/{id}` Ошибка
- `GET /order` Список всех заказов
- `GET /order?{param}` Список всех заказов с фильтром по параметрам
- `GET /order/{id}` Данные конкретного заказа по `id`
- `PUT /order` Обновить данные заказов
- `PUT /order/{id}` Обновить данные конкретного заказа по `id`

### Пример `GET` запроса с HTTP клиентом Guzzle

В параметре `relations` - мы указываем какие связанные данные получить,в нашем случае: `product` `user` `address`
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
    'relations' => "product,user,address"
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
    "auth": "QueryKeyAuth",
    "total": "10"
  },
  "request": {
    "query": "GET",
    "resource": "orders",
    "limit": "10",
    "offset": "0",
    "order": "DESC",
    "sort": "created",
    "state": "1",
    "date_from": "2017-12-07",
    "date_to": "2017-12-14",
    "relations": "product, user, address"
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
          "fname": "Иванов",
          "iname": "Юрий",
          "oname": "Петрович",
          "phone": "380670000001",
          "email": "user@example.com"
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
        "products": [
           {
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
    }]
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
    'relations' => "product,user,address"
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
	$address = $records["body"]["items"]["item"]["address"];
	$products = $records["body"]["items"]["item"]["products"];
	
        echo $order["order_id"];
	// выведет: 10
        echo $user["fname"].' '.$user["iname'];
	// выведет: Иванов Юрий
	echo $address["city"].', '.$address["street'].' '.$address["number'];
	// выведет: Киев, Бажана 12а/17
    }
}

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
    "auth": "QueryKeyAuth",
    "total": "1"
  },
  "request": {
    "query": "GET",
    "resource": "order",
    "order_id": "10",
    "relations": "product, user, address"
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
          "fname": "Иванов",
          "iname": "Юрий",
          "oname": "Петрович",
          "phone": "380670000001",
          "email": "user@example.com"
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
        "products": [
           {
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
    }]
  }
}
```
