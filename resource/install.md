# Установка `install`
Ресурс `install` отвечает за установку и активировацию магазина

Этот ресурс при `POST` запросах, должен работать без `public_key`, так как на этот момент у сайта нет `public_key` и он должен быть создан в результате этого запроса.
 
### Торговая площадка должна потдерживать запросы: `POST` `PUT`
- `POST /install` - Создать и активировать магазин
- `PUT /install` - Обновить данные магазина, при наличии `public_key`
 
Какие поля должен поддерживать ресурс `install` вы можете посмотреть в структуре базы данных [db/install](https://github.com/pllano/db.json/blob/master/db/install.md)
 
### Пример `POST` запроса HTTP клиентом Guzzle
``` php
use GuzzleHttp\Client as Guzzle;
 
// Наш запрос
// password - Пароль администратора магазина
$data = [
    'password' => password_hash($password, PASSWORD_DEFAULT),
    'phone' => "Телефон администратора магазина",
    'email' => "Email администратора магазина",
    'language' => "язык системы",
    'iname' => "Имя администратора магазина",
    'fname' => "Фамилия администратора магазина",
    'host' => "Адрес сайта"
];

// Формируем URL запроса
$uri = 'https://example.com/api/v1/json/install';
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
    // Получаем site_id
    print_r($records['response']['id']);
    // Получаем public_key
    print_r($records['response']['public_key']);
}
}
```
``` php
// Вывести на экран json
print_r($output);
```
### Структура ответа торговой площадки на наш `POST` запрос в json формате
```json
{
    "header": {
        "status": "201 Created",
        "code": 201,
        "message": "OK",
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/201.md"
    },
    "response": {
        "id": 10,
        "public_key": "public_key для подключения через API"
    },
    "request": {
        "query": "POST",
        "resource": "install"
    },
    "body": {
        "items": []
    }
}
```
### Проверка запроса и обработка ошибок
Ваша API должна проверять запрашиваемые поля, и если данные отдавать не разрешено, не обрабатывать это поле. Если вы проигнорировали некоторые поля запроса, в конце обработки запроса, рекомендуется отправить `POST` на ресурс [`/feedback`](https://github.com/pllano/APIS-2018/blob/master/resource/feedback.md) в котором сообщить подробности об ошибке. Также необходимо в `request` вернуть те поля которые ваша API обработала успешно, и не выводить те, которые проигнорировала, чтобы запрашиваемая сторона могла обработать результат запроса и скорректировать его в будущем.

