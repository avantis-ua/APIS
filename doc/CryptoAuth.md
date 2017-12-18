# CryptoAuth
«CryptoAuth» — работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption)

## Основные преимущества данного метода:
- Простота реализации с обоих сторон.
- Отсекание запросов которые невозможно раскодировать что уменьшает количество запросов к базе данных.

### private_key
Для автоматической генерации ключей и шифрования мы рекомендуем использовать библиотеку [defuse/php-encryption](https://github.com/defuse/php-encryption) которой доверяют свыше 1,5 млн. разработчиков.
#### Пример генерации ключей
``` php
use Defuse\Crypto\Key;

// Генерируем private_key
$crypto = Key::createNewRandomKey();
$private_key = $crypto->saveToAsciiSafeString();

echo $private_key;

```
Пример сгенерированного ключа `private_key`: ` def0000068b662b915532b74bb9784c61b87984d62be71672d62f3cb7cf392dde16deebfe0a4ed34dd26a2b4c3ae3c84d3a987a820f74d9228b69a36a6e2993385cb4268 `

ПРИМЕЧАНИЕ: И вы и пользователи вашего API должны использовать одну и туже библиотеку для шифрования !
### public_key
Для создания public_key не рекомендуется использовать тот же механизм что и для private_key. Вы можете использовать следующую функцию для генерации public_key.
``` php
// Функция генерации public_key 32 символов
// Вы можете передать функции параметр $length
function random_public_key($length = 16)
{
    if(!isset($length) || intval($length) <= 8 ){
    $length = 16;
}
    if (function_exists('random_bytes')) {
    return bin2hex(random_bytes($length));
}
    if (function_exists('mcrypt_create_iv')) {
    return bin2hex(mcrypt_create_iv($length, MCRYPT_DEV_URANDOM));
}
    if (function_exists('openssl_random_pseudo_bytes')) {
    return bin2hex(openssl_random_pseudo_bytes($length));
}
}

// Сгенерировать public_key
$public_key = random_public_key(32);
```
Пример сгенерированного ключа `public_key` длиной 64 символа: ` 3903f7b3fb82c2e609b3f07ccfa119352f1d26c55723c3f7f8fb36a0d0e31dae `

### Пример аутентификации и передачи данных через POST запрос клиентом Guzzle
``` php
use Defuse\Crypto\Crypto;
use GuzzleHttp\Client as Guzzle;

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

// Взять private_key и public_key из конфигурации
$public_key = $config->get('public_key'); // Публичный ключ - идентификатор пользователя API
$private_key = $config->get('private_key'); // Ключ шифрования

// Шифруем данные
$data_post = Crypto::encrypt(
    $data_json,
    Key::loadFromAsciiSafeString($private_key)
);

// URL вашей API
$api_uri = "https://example.com/api/v1/json/pay";

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем данные
$response = $client->request('POST', $api_uri, [
    'public_key' => base64_encode($public_key),
    'data' => base64_encode($data_post)
    ]);
```

### Получение и обработка данных на стороне REST API
``` php
use Defuse\Crypto\Crypto;
use Defuse\Crypto\Exception\WrongKeyOrModifiedCiphertextException as cException;

// Полученный вами json массив в теле запроса
$json = '{public_key: public_key", data: "data"}';

// Преобразовываем в массив
$arr = json_decode($json, true);

// Получаем public_key из запроса и по нему находим в своей базе user_key
$public_key = base64_decode($arr["public_key"]);

// Проверяем длину строки. Должна быть 64 символа.
if (strlen($public_key) == 64) {

    // Получаем user_key из базы данных
    $private_key = "";

    // Расшифровываем
    try {
        $decrypt_json = Crypto::decrypt(
            base64_decode($arr["data"]),
            Key::loadFromAsciiSafeString($private_key)
        );

        // Преобразование расшифрованного json в массив
        $data = json_decode($decrypt_json, true);

        echo $data;

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

    } catch(cException $ex){
 
        // Не могу расшифровать данные
 
    }

} else {
 
    // Длина строки не равняется 64 символа !
 
}
```
 
