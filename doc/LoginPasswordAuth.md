# LoginPasswordAuth

## Аутентификация методом LoginPasswordAuth
«LoginPasswordAuth» — реализация базовой Http-аутентификации [basic HTTP authentication](http://www.ietf.org/rfc/rfc2069.txt) в `Authorization` заголовке. Пара логин пароль передаются в виде строки login:password

### Пример использования с HTTP клиентом Guzzle
``` php	
use GuzzleHttp\Client as Guzzle;

$username = $config['settings']['api']['username']; // Взять username из конфигурации
$password = $config['settings']['api']['password']; // Взять password из конфигурации

// передать данные
$data_post = [
    'action' => "pay",
    'order_id' => "12345",
    'amount' => "10.01",
    'currency' => "UAH",
    'server_url' => "URL ответа"
];

// URL вашей API
$api_uri = "https://example.com/api/v1/json/pay";

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем данные
$response = $client->request('POST', $api_uri, 
    [
        'auth' => [$username, $password],
        'data' => base64_encode($data_post)
    ]
);
 
```
#### Документация
- [Guzzle](http://docs.guzzlephp.org/en/stable/request-options.html#auth)
