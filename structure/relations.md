# Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.
 
Для передачи дополнительных параметров в `json` формате с последующим кодированием данных в формат MIME base64 функцией base64_encode
 
Параметры: Название связаного ресурса равно `"all"` или строка с параметрами
```php
// Массив с параметрами запроса
$arr = [
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1,
    "relations" => base64_encode('{
        "product": ["name","type","brand","serie","articul"],
        "user": "all",
        "address": "all"
    }')
];
```
```php
// Декодируем relations в массив
$relations = json_decode(base64_decode('$arr["relations"]'), true);
```
