# Установка `install`
Ресурс `install` отвечает за установку системы.
 
### Торговая площадка должна потдерживать запросы: `POST` `PUT` `PATCH`
- `POST /install` - Добавить товар в корзину
- `PUT /install` - Обновить данные товаров
- `PUT /cart/{cart_id}` - Обновить данные конкретного товара по `cart_id`
- `PATCH /cart/` Обновить данные товаров
- `PATCH /cart/{cart_id}` Обновить данные конкретного товара по `cart_id`
 
Какие поля должен поддерживать ресурс `install` вы можете посмотреть в структуре базы данных [db/install](https://github.com/pllano/db.json/blob/master/db/install.md)
 
### Пример `POST` запроса HTTP клиентом Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get('public_key');

// Если order_id = null товар в корзине
// Если order_id >= 1 товар в заказе
// Наш запрос
$data = [
    'public_key' => $public_key,
    'site_id' => 1,
    'product_id' => 100,
    'order_id' => null,
    'status_id' => 1,
    'num' => 2,
    'price' => 1500.50,
    'currency_id' => 1,
    'state' => 1
];

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/cart';
// Подключаем Guzzle
$client = new Guzzle();
// Отправляем запрос
$response = $client->request('POST', $uri, ['form_params' => $data]);
// Получаем тело ответа
$output = $response->getBody();
// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records['headers']['code'])) {
if ($records['headers']['code'] == '201') {
    // Получаем id созданной записи
    print_r($records['response']['id']);
}
}
```
``` php
// Вывести на экран json
print_r($records);
```
### Структура ответа торговой площадки на наш `POST` запрос в json формате
```json
{
    "header": {
        "status": "201 Created",
        "code": "201",
        "message": "OK",
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/201.md"
    },
    "response": {
        "auth": "QueryKeyAuth",
        "id": "10002"
    },
    "request": {
        "query": "POST",
        "resource": "cart"
    },
    "body": {
        "items": []
    }
}
```
### Проверка запроса и обработка ошибок
Ваша API должна проверять запрашиваемые поля, и если данные отдавать не разрешено, не обрабатывать это поле. Если вы проигнорировали некоторые поля запроса, в конце обработки запроса, рекомендуется отправить `POST` на ресурс [`/feedback`](https://github.com/pllano/APIS-2018/blob/master/resource/feedback.md) в котором сообщить подробности об ошибке. Также необходимо в `request` вернуть те поля которые ваша API обработала успешно, и не выводить те, которые проигнорировала, чтобы запрашиваемая сторона могла обработать результат запроса и скорректировать его в будущем.

