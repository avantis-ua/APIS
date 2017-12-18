# Ответ `response`
```json
{
    "response": {
        "auth": "CryptoAuth",
        "total": "100",
        "id": "10",
        "resource_param": "",
    }
}
```
### total
`total` - `Обязательным параметром` является общее количество найденых записей соответствующих запросу. Зная общее количество и количество выведенных на страницу можно организовать пагинацию или цикл запросов с желаемым количеством записей на странице.
### auth
`auth` - `Обязательным параметром` является информация о типе аутентификации. Вы должны указать один из нижеуказанных методов аутентификации.
#### NoneAuth
Без аутентификации
```json
"auth": 0
```
#### LoginPasswordAuth
```json
"auth": "LoginPasswordAuth"
```
«[LoginPasswordAuth](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)» — реализация базовой Http аутентификации. Пара логин пароль передаются в виде строки `login:password` в `Authorization` заголовке. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)
#### HttpTokenAuth
```json
"auth": "HttpTokenAuth"
```
«[HttpTokenAuth](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)» — реализация аутентификации по токену `HTTP Bearer token`. Для авторизации с помощью токена нужно передать его в `http` заголовке.  [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)
#### QueryKeyAuth
```json
"auth": "QueryKeyAuth"
```
«[QueryKeyAuth](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)» — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и `HttpTokenAuth`, т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)
#### CryptoAuth
```json
"auth": "CryptoAuth"
```
«[CryptoAuth](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)» работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption) или подобной. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)

### resource_param
Дополнительно можно отдавать любую информацию разрешенную данным ресурсом.
