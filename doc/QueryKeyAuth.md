# QueryKeyAuth
«QueryKeyAuth» — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и [`HttpTokenAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md), т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`

### Пример использования с HTTP клиентом Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

$public_key = $config->get('public_key'); // Взять public_key из конфигурации

$data = [
    'public_key' => $public_key,
    'action' => "pay",
    'order_id' => "12345",
    'amount' => "10.01",
    'currency' => "UAH",
    'server_url' => "URL ответа"
];

$data_query = http_build_query($data) . "\n";

$uri = 'https://example.com/api/v1/json/pay?'.$data_query;

$client = new Guzzle();
$response = $client->request('GET', $uri);
 
```
