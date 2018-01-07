# Параметр `relations`
Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов. С одной стороны этот запрос усложняет обработку, но с другой стороны делает сами запросы более простыми.

Примечание: через `relations` нельзя получить значение `password` и `cookie`

## Как это работает ?
Для примера возьмем два ресурса: `user` и `order`

Согласно спецификации в ресурсе `user` будет поле `user_id`, а в ресурсе `order` будет поле `order_id`

Для того чтобы связать две записи этих ресурсов, в одной из них должен хранится `id` другой записи.

Выходит в ресурсе `user` должно быть поле `order_id` или в ресурсе `order` должно быть поле `user_id`.

Мы проверяем оба варианта и если находим выполняем запрос.

В данном примере логично что мы найдем в таблице `order` поле `user_id`


### Два формата запроса
- Передача дополнительных параметров в `json` формате с последующим кодированием данных в формат MIME base64 функцией base64_encode. Строка с параметрами или если необходимо получить все поля  `"all"`
- Передача дополнительных параметров в строке с разделителями для ресурсов `,` и `:` для необходимых полей 

Например в нашем запросе к ресурсу `order` мы хотим дополнительно получить следующие данные: 
- `cart` - товары в заказе
- `user` - данные покупателя
- `address` - адрес покупателя

#### `GET` - Получение данных через [`RouterDb`](https://github.com/pllano/router-db)
```php
use RouterDb\Db;
 
// Подключаемся к базе
$db = new Db("mysql", $config);
// Передача дополнительных параметров в строке с разделителями
$relations = "cart,user:phone:email:fname:iname,address";
 
// Массив с данными запроса
$getArr = [
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1
    "relations" => $relations
];
 
$response = $db->get("order", $getArr);
 
// Обрабатываем ответ
if (isset($records["headers"]["code"])) {
    if ($records["headers"]["code"] == 200) {
        $count = count($records["body"]["items"]);
        if ($count >= 1) {
            foreach($records["body"]["items"] as $value)
            {
                // Если $value object переводим в array
		$item = is_array($value["item"]) ? $item["item"] : (array)$value["item"];
                // Получаем данные
                print_r($item["name"]);
            }
        }
    }
}
```
### `GET` запрос к `API` ресурс `order` через HTTP клиент Guzzle
Пример URL [`https://xti.com.ua/json-db/order?relation=address,cart,user:user_id:iname:oname:phone:email`](https://xti.com.ua/json-db/order?relation=address,cart,user:user_id:iname:oname:phone:email)
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
$resp = $client->request("GET", $uri);
// Получаем тело ответа
$get_body = $resp->getBody();
// json в массив
$response = json_decode($get_body, true);

// Обрабатываем ответ
if (isset($response["headers"]["code"])) {
    if ($response["headers"]["code"] == 200) {
        $count = count($response["body"]["items"]);
        if ($count >= 1) {
            foreach($response["body"]["items"] as $item)
            {
                // Если $value object переводим в array
                $item = is_array($value["item"]) ? $item["item"] : (array)$value["item"];
                // Получаем данные
                print_r($item["name"]);
            }
        }
    }
}

```
### Вывести результат
``` php
print_r($response);
```
```php
$records = array (
  'headers' => array (
    'status' => '200 OK',
    'code' => '200',
    'message' => 'OK',
    'message_id' => 'https://github.com/pllano/APIS-2018/tree/master/http-codes/200.md',
  ),
  'response' => array (
    'auth' => 'QueryKeyAuth',
    'total' => '10',
  ),
  'request' => array (
    'query' => 'GET',
    'resource' => 'order',
    'limit' => '10',
    'offset' => '0',
    'order' => 'DESC',
    'sort' => 'created',
    'state' => '1',
    'relations' => array (
      'product' => 'all',
      'user' => array (
        0 => 'phone',
        1 => 'email',
        2 => 'fname',
        3 => 'iname',
        4 => 'oname',
      ),
      'address' => 'all',
    ),
  ),
  'body' => array (
    'items' => array (
      0 => array (
        'item' => array (
          'id' => '1234567890',
          'created' => '2017-12-11 10:30',
          'status' => 1,
          'delivery' => 'Novaposhta',
          'delivery_code' => '1234567890121',
          'total_amount' => '10501.00',
          'currency_id' => 'UAH',
          'comment' => 'Могу принять после 17:00',
          'user' => array (
            0 => array (
              'phone' => '380670000001',
              'email' => 'user@example.com',
              'fname' => 'Иванов',
              'iname' => 'Юрий',
              'oname' => 'Петрович',
            ),
          ),
          'cart' => array (
            0 => array (
              'product_id' => '1000120',
              'name' => 'Ноутбук Asus X751NV (X751NV-TY001) Black',
              'type' => 'Ноутбук',
              'brand' => 'Asus',
              'serie' => 'X751NV',
              'articul' => '(X751NV-TY001) Black',
              'price' => '5750.50',
              'oldprice' => '5500.00',
              'num' => 1,
              'available' => 5,
              'total_price' => '5500.00',
              'currency_id' => 'UAH',
              'guarantee' => 36,
              'pay_online' => 1,
            ),
            1 => array (
              'product_id' => '1000120',
              'name' => 'Ноутбук Asus X751NV (X751NV-TY001) Black',
              'type' => 'Ноутбук',
              'brand' => 'Asus',
              'serie' => 'X751NV',
              'articul' => '(X751NV-TY001) Black',
              'price' => '5750.50',
              'oldprice' => '5500.00',
              'num' => 1,
              'available' => 5,
              'total_price' => '5500.00',
              'currency_id' => 'UAH',
              'guarantee' => 36,
              'pay_online' => 1,
            ),
          ),
          'address' => array (
            0 => array (
              'country' => 'Украина',
              'region' => 'Киевская область',
              'postal_code' => 0,
              'city' => 'Киев',
              'district' => 'Позняки',
              'street' => 'Бажана',
              'number' => '12а/17',
              'parade' => '0',
              'floor' => '0',
              'apartment' => '75',
              'additional' => 'Код на парадном 107',
            ),
          ),
        ),
      ),
    ),
  ),
);
```
