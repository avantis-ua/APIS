# QueryCryptoAuth
«QueryCryptoAuth» — работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается в строке запроса как `GET` параметр, только в данном случае он в зашифрованном виде с помощью `private_key`. По умолчанию параметр называется `public_key` и его можно расшифровать только зная `private_key`

## Основные преимущества данного метода:
- Простота реализации с обоих сторон.
- Отсекание запросов которые невозможно раскодировать что уменьшает количество запросов к базе данных.

## Шифрование и public_key
Для автоматической генерации ключей и шифрования мы рекомендуем использовать библиотеку [defuse/php-encryption](https://github.com/defuse/php-encryption) которой доверяют свыше 1,5 млн. разработчиков.
### Пример генерации ключей
``` php
// Генерируем public_key
$crypto = \Defuse\Crypto\Key::createNewRandomKey();
$crypto_key = $crypto->saveToAsciiSafeString();

echo $crypto_key;
```
ПРИМЕЧАНИЕ: И вы и пользователи вашего API должны использовать одну и туже библиотеку для шифрования !

### Пример аутентификации и передачи данных через POST запрос клиентом Guzzle
``` php

// Вы можете безопасно передавать любые данные
$data_arr = [
    'action' => "pay",
    'order_id' => "12345",
    'amount' => "10.01",
    'currency' => "UAH",
    'server_url' => "URL ответа"
];

// Преобразовать массив в json формат
$data_json = json_encode($data_arr);

// Взять crypto_key и public_key из конфигурации
$public_key = ''; // Публичный ключ - идентификатор пользователя API
$crypto_key = ''; // Ключ шифрования

// Шифруем данные
$data_post = \Defuse\Crypto\Crypto::encrypt($data_json, Key::loadFromAsciiSafeString($crypto_key));

// URL вашей API
$api_uri = "https://example.com/api/v1/json/pay";

// Подключаем Guzzle
$client = new \GuzzleHttp\Client();

// Отправляем данные
$response = $client->request('POST', $api_uri, ['public_key' => $public_key, 'data' => $data_post]);
```

### Получение и обработка данных на стороне REST API
``` php
// Полученный вами json массив в теле запроса
$json = '{public_key: "... ...", data: "... ..."}';

// Преобразовываем в массив
$arr = json_decode($json, true);

// Получаем public_key из запроса и по нему находим в своей базе user_key
$public_key = $arr["public_key"];

// Получаем user_key из базы данных
$user_key = "";

$decrypt_arr = \Defuse\Crypto\Crypto::decrypt($arr["data"], Key::loadFromAsciiSafeString($user_key));

echo $decrypt_arr;

/*
Array
(
    ["action"] => pay,
    ["order_id"] => 12345,
    ["amount"] => 10.01,
    ["currency"] => UAH,
    ["server_url"] => URL ответа
);
*/
```
 
