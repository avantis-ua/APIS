# QueryPrivateKeyAuth
«QueryPrivateKeyAuth» — работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается в строке запроса как `GET` параметр, только в данном случае он в зашифрованном виде с помощью `private_key`. По умолчанию параметр называется `public_key` и его можно расшифровать только зная `private_key`

## Основные преимущества данного метода:
- Простота реализации с обоих сторон.
- Отсекание запросов которые невозможно раскодировать что уменьшает количество запросов к базе данных.

## Шифрование и public_key
Для автоматической генерации ключей и шифрования мы рекомендуем использовать библиотеку [defuse/php-encryption](https://github.com/defuse/php-encryption) которой доверяют свыше 1,5 млн. разработчиков.
### Пример генерации ключей
``` php
// Генерируем public_key
$crypto = Defuse\Crypto\Key::createNewRandomKey();
$public_key = $crypto->saveToAsciiSafeString();

echo $public_key;
```
### Пример аутентификации клиентом Guzzle
``` php	
$public_key = ''; // Взять public_key из конфигурации
$uri = 'https://example.com/api/v1/json/auth?public_key='.$public_key;

$client = new \GuzzleHttp\Client();
$response = $client->request('GET', $uri);
```
