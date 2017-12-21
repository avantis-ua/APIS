# Товары и цены `price`
Ресурс `price` отвечает за вывод товаров, цен, наличия, свойств товаров интернет-магазина. В отличии от ресурса `product` ресурс `price` в нашей инфраструктуре имеет более высокое значение, в связи с тем что каталог товаров строиться не на виртуальной витрине, а на реальном наличии товаров. Именно ресурс `price` формирует каталог товаров, а не `product`.

### Торговая площадка должна потдерживать только `GET` запросы
- `GET /price` - Список всех товаров
- `GET /price?{param}` - Список всех товаров с фильтром по параметрам
- `GET /price/{price_id}` - Данные конкретного товара по `price_id`

### Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется четыри символа: `'` `.` `,` `:`

Сами ресурсы перечисляем через точку `'product'.'type'.'brand'.'serie'.'image'.'seo'.'property_product'` экранируя одинарными кавычками
```
"relations" => "'product'.'type'.'brand'.'serie'.'image'.'seo'.'property_product'"
```

Если необходимо запросить конкретные данные указываем их через двоеточие, если нужно несколько через запятую `'product'.'type:title'.'brand:title'.'serie:title'.'image:image_path,sort'.'seo:seo_title'.'property_product'`

В нашем запросе к ресурсу `price` мы хотим дополнительно получить: 
- из ресурса `currency` - поля: `name` `iso_code`
- из ресурса `product` - все данные
- из ресурса `type` - поля: `title`
- из ресурса `brand` - поля: `title`
- из ресурса `serie` - поля: `title`
- из ресурса `image` - поля: `image_path` `sort`
- из ресурса `seo` - поля: `seo_title`
- из ресурса `property_product` - все данные

```
"relations" => 
    "'currency:name,iso_code'.
    'product'.
    'type:title'.
    'brand:title'.
    'serie:title'.
    'image:image_path,sort'.
    'seo:seo_title'.
    'property_product'"
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
    'sort' => "score",
    'state' => 1,
    'relations' => 
        "'currency:name,iso_code'.
        'product'.
        'type:title'.
        'brand:title'.
        'serie:title'.
        'image:image_path,sort'.
        'seo:seo_title'.
        'property_product'.
        'delivery:terms'"
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/price?'.$data_query;

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
        "total": "30"
    },
    "request": {
        "query": "GET",
        "resource": "price",
        "limit": "30",
        "offset": "0",
        "order": "ASC",
        "sort": "created",
        "state": "1",
        "relations": "'currency:name,iso_code'.'product'.'type:title'.'brand:title'.'serie:title'.'image:image_path,sort'.'seo:seo_title'.'property_product'.'delivery:terms'"
    },
    "body": {
        "items": [{
                "item": {
                    "site_id": 1,
                    "product_id": 85321,
                    "category_id": 1,
                    "price": "2813.00",
                    "oldprice": "2900.00",
                    "currency_id": 1,
                    "pay_online": 1,
                    "available": 5,
                    "guarantee": 12,
                    "terms_of_delivery": "",
                    "locking": 0,
                    "supplier_item_id": "",
                    "alias": "",
                    "state": 1,
                    "created": "",
                    "modified": "",
                    "score": 100.01,
                    "currency": {
                        "name": "грн.",
                        "iso_code": "UAH"
                    },
                    "product": {
                        "id": 750,
                        "product_id": 85321,
                        "parent_id": 0,
                        "type_id": 155,
                        "brand_id": 1020,
                        "serie_id": 2095,
                        "articul": "",
                        "name": "",
                        "code": "",
                        "intro": "",
                        "template": "",
                        "mod_id": 0,
                        "complect_id": 0,
                        "priority": 10,
                        "authorize": 0,
                        "alias": "",
                        "state": 1,
                        "publish_beg": "",
                        "publish_end": "",
                        "created": "",
                        "score": ""
                    },
                    "type": {
                        "title": ""
                    },
                    "brand": {
                        "title": ""
                    },
                    "serie": {
                        "title": ""
                    },
                    "images": [{
                            "image": {
                                "image_path": "",
                                "sort": 1
                            },
                            "image": {
                                "image_path": "",
                                "sort": 1
                            }
                        }
                    ],
                    "seo": {
                        "seo_title": ""
                    },
                    "delivery": {
                        "terms": ""
                    },
                    "properties": [{
                            "property_product": {
                                "product_id": 85321,
                                "field": "Диагональ",
                                "value": "7",
                                "individual": 1,
                                "property_id": 105,
                                "property_value_id": 20735,
                                "sort": 1,
                                "score": ""
                            },
                            "property_product": {
                                "product_id": 85321,
                                "field": "Экран",
                                "value": "LED",
                                "individual": 1,
                                "property_id": 105,
                                "property_value_id": 20735,
                                "sort": 1,
                                "score": ""
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
