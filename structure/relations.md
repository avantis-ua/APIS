# Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется json формат

Параметры:
- Название связаного ресурса и `true` или строка с параметрами
```
// Пример массива запроса
$arr = [
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1,
    "relations" => base64_encode('{
        "product": ["type_id","brand_id","serie_id","articul"],
        "user": true,
        "address": true
    }')
];
```
```
// Конвертируем relations в массив
$relations = json_decode(base64_decode('$arr["relations"]'), true);
```
