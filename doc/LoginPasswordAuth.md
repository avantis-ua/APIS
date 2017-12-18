# LoginPasswordAuth

## Аутентификация методом LoginPasswordAuth
«LoginPasswordAuth» — реализация базовой Http-аутентификации [basic HTTP authentication](http://www.ietf.org/rfc/rfc2069.txt) в `Authorization` заголовке. Пара логин пароль передаются в виде строки login:password

### Пример использования с HTTP клиентом Guzzle
``` php	
$username = $config['settings']['api']['username']; // Взять username из конфигурации
$password = $config['settings']['api']['password']; // Взять password из конфигурации
$uri = 'https://example.com/api/v1/json/auth';

$client = new \GuzzleHttp\Client();
$client->request('GET', $uri, ['auth' => [$username, $password]]);
```
#### Документация
- [Guzzle](http://docs.guzzlephp.org/en/stable/request-options.html#auth)
