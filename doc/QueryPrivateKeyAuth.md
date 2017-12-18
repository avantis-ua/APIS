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
$crypto_key = $crypto->saveToAsciiSafeString();

echo $crypto_key;
```
ПРИМЕЧАНИЕ: И вы и пользователи вашего API должны использовать одну и туже библиотеку для шифрования !

### Пример аутентификации через GET запрос клиентом Guzzle
``` php
// $arr Массив с данными
$arr = array();
// Вы можете безопасно передавать любые данные
$arr["user"] = "test";
$arr["pasword"] = "test";
$arr["order_id"] = "12345";
$arr["status"] = "success";

// Преобразовать массив в json формат
$data = json_encode($arr);

// Взять crypto_key из конфигурации
$crypto_key = '';

// Шифруем данные
$public_key = Crypto::encrypt($data, Key::loadFromAsciiSafeString($crypto_key));

// Формируем URL
$uri = 'https://example.com/api/v1/json/auth?public_key='.$public_key;

// Подключаем Guzzle
$client = new \GuzzleHttp\Client();

// Отправляем данные
$response = $client->request('GET', $uri);
```
