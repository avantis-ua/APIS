# Параметр `relations`
Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов. С одной стороны этот запрос усложняет обработку, но с другой стороны делает сами запросы более простыми.

Примечание: через `relations` нельзя получить значение `password` и `cookie`

### Два формата запроса
- Передача дополнительных параметров в `json` формате с последующим кодированием данных в формат MIME base64 функцией base64_encode. Строка с параметрами или если необходимо получить все поля  `"all"`
- Передача дополнительных параметров в строке с разделителями для ресурсов `,` и `:` для необходимых полей 

Например в нашем запросе к ресурсу `order` мы хотим дополнительно получить следующие данные: 
- `cart` - товары в заказе
- `user` - данные покупателя
- `address` - адрес покупателя

Пример URL [`https://xti.com.ua/json-db/order?relation=address,cart,user:user_id:iname:oname:phone:email`](https://xti.com.ua/json-db/order?relation=address,cart,user:user_id:iname:oname:phone:email)

### Пример `GET` запроса к ресурсу `order` через HTTP клиент Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get("public_key");

// Если нужно только необходимые поля
$relations = base64_encode('{
    "cart": "all",
    "user": ["phone","email","fname","iname"],
    "address": "all"
}');

// Или более простой вариант: ресурсы через запятую, а необходимые поля через двоеточие
$relations = "cart,user:phone:email:fname:iname,address";

// Наш запрос
$data = [
    "public_key" => $public_key,
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1
    "relations" => $relations
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";
// Формируем URL запроса
$uri = "https://example.com/api/v1/json/order?".$data_query;
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
	$count = count($records["body"]["items"]);
	if ($count >= 1) {
		foreach($records["body"]["items"] as $item)
		{
			print_r($item["item"]);
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
    "headers": {
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
        "relations": {
            "product": "all",
            "user": ["phone","email","fname","iname","oname"],
            "address": "all"
        }
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
                    "email": "user@example.com",
                    "fname": "Иванов",
                    "iname": "Юрий",
                    "oname": "Петрович"
                },
                "cart": [
                    {
                        "product_id": "1000120",
                        "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
                        "type": "Ноутбук",
                        "brand": "Asus",
                        "serie": "X751NV",
                        "articul": "(X751NV-TY001) Black",
                        "price": "5750.50",
                        "oldprice": "5500.00",
                        "num": 1,
                        "available": 5,
                        "total_price": "5500.00",
                        "currency_id": "UAH",
                        "guarantee": 36,
                        "pay_online": 1
                    }, {
                        "product_id": "1000120",
                        "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
                        "type": "Ноутбук",
                        "brand": "Asus",
                        "serie": "X751NV",
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
                ],
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
        }]
    }
}
```
