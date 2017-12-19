# marketplace-order

### Необходимо потдерживать запросы: `POST` `GET` `PUT`
- `POST /marketplace-order` Создание заказа 
- `POST /marketplace-order/{id}` Ошибка
- `GET /marketplace-order` Список всех заказов
- `GET /marketplace-order?{param}` Список всех заказов с фильтром по параметрам
- `GET /marketplace-order/{id}` Данные конкретного заказа по `id`
- `PUT /marketplace-order` Обновить данные заказов
- `PUT /marketplace-order/{id}` Обновить данные конкретного заказа по `id`

### Пример `GET` запроса с HTTP клиентом Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

$public_key = $config->get('public_key'); // Взять public_key из конфигурации

$data = [
    'public_key' => $public_key,
    'limit' => 10,
    'offset' => 0,
    'order' => "DESC",
    'sort' => "created",
    'state' => 1,
    'date_from' => "2017-12-07",
    'date_to' => "2017-12-14",
    'search' => "Ноутбук"
];

$data_query = http_build_query($data) . "\n";

$uri = 'https://example.com/api/v1/json/marketplace-order?'.$data_query;

$client = new Guzzle();
$response = $client->request('GET', $uri);

$output = $response->getBody();

// Чистим все что не нужно, иначе json_decode не сможет конвертировать json в массив
for ($i = 0; $i <= 31; ++$i) {$output = str_replace(chr($i), "", $output);}
$output = str_replace(chr(127), "", $output);
if (0 === strpos(bin2hex($output), 'efbbbf')) {$output = substr($output, 3);}

$records = json_decode($output, true);

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
## Структура данных ответа торговой площадки на `GET` запрос
``` php
print_r($records);
```
Выведет json код ответа
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
    "resource": "supplier-order",
    "limit": "10",
    "offset": "0",
    "order": "DESC",
    "sort": "created",
    "state": "1",
    "date_from": "2017-12-07",
    "date_to": "2017-12-14",
    "search": "Ноутбук"
  },
  "body": {
    "items": [{
      "item": {
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
    }],
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
        }
  }
}
```
