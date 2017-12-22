# Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется четыри символа: `'` `.` `,` `:`

Сами ресурсы перечисляем через точку `'product'.'user'.'address'` экранируя одинарными кавычками
```
"relations" => "'product'.'user'.'address'"
```

Если необходимо запросить конкретные данные указываем их через двоеточие `'user:phone'` если нужно несколько через запятую `'product:type_id,brand_id,serie_id,articul'`

```
"relations" => "'product:type_id,brand_id,serie_id,articul'.'user:phone'.'address:street_id'"
```
