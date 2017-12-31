# Корзина `cart`
Ресурс `cart` отвечает за работу с корзиной, а это: добавление товаров в корзину, вывод, добавление или изменение данных итд. Этот ресурс должен позволять сделать все тоже, что позволяет сделать стандартный интернет-магазин работающий независимо. Вы можете сделать даже больше, внедрив единую корзину и одну учетною запись на все сайты работающие через вашу API.

### Торговая площадка должна потдерживать запросы: `POST` `GET` `PUT` `PATCH` `DELETE`
- `POST /cart` - Добавить товар в корзину
- `POST /cart/{cart_id}` - Ошибка
- `GET /cart` - Список всех товаров
- `GET /cart?{param}` - Список всех товаров с фильтром по параметрам
- `GET /cart/{cart_id}` - Данные конкретного товара по `cart_id`
- `PUT /cart` - Обновить данные товаров
- `PUT /cart/{cart_id}` - Обновить данные конкретного товара по `cart_id`
- `PATCH /cart/` Обновить данные товаров
- `PATCH /cart/{cart_id}` Обновить данные конкретного товара по `cart_id`
- `DELETE /cart/` Удалить все товары
- `DELETE /cart/{cart_id}` Удалить конкретный товар по `cart_id`

Какие поля должен поддерживать ресурс `cart` вы можете посмотреть в структуре базы данных [db/cart](https://github.com/pllano/db.json/blob/master/db/cart.md)

### Параметр `relations`
Для ресурса `cart` параметр `relations` не поддерживается

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
### Структура ответа торговой площадки на наш `GET` запрос в json формате
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
