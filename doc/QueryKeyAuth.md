# QueryKeyAuth
«QueryKeyAuth» — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и [`HttpTokenAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md), т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`

### Пример использования с HTTP клиентом Guzzle
``` php	
$public_key = $config['settings']['api']['public_key']; // Взять public_key из конфигурации
$uri = 'https://example.com/api/v1/json/auth?public_key='.$public_key;

$client = new \GuzzleHttp\Client();
$response = $client->request('GET', $uri);
```
