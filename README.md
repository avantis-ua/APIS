# APIS-2018

«APIS-2018» — Стандарт обмена информацией между базами данных, торговыми платформами, интернет-магазинами, поставщиками товаров и сторонними платформами через RESTful API интерфейс.

Стандарт «APIS-2018» используют: E-commerce Platform [«API Shop»](https://github.com/API-Shop/api-shop) и база данных на JSON файлах [«API json DB»](https://github.com/pllano/api-json-db) разработки компании PLLANO.com

### `Стандарт APIS-2018 - не является общепринятым !`
Стандарт является взглядом в будущее и рекомендацией для унификации обмена данными сервер-сервер.

Если вы начнете использовать формат APIS-2018 сейчас, вы можете быть уверенны в том что в будущем вам будет намного легче настроить работу со сторонними RESTful API

### `Мы ищем единомышленников — Присоединяйтесь!`

Мы конечно же хотим достичь чего то подобного на [schema.org](http://schema.org) но учитывая специфику E-commerce этот стандарт нам не совсем подходит.

## Стандарт APIS-2018 стоит на двух основных принципах: 
- Адреса ресурсов должны быть одинаковыми
- Структура ответов и запросов должны быть одинаковыми

Таким образом вы можете писать свою API зная что другим API использующим наш стандарт не придется тратиться на дополнительную доработку и интеграцию с вашим API.

### 1. Адреса ресурсов и типы запросов с которыми работает [API Shop](https://github.com/API-Shop/api-shop) должны быть одинаковыми

Например:
- ресурс `/price` обрабатывает только GET запросы и выдает информацию о ценах и наличии товара.
- ресурс `/product` обрабатывает только GET запросы и выдает информацию о товаре.
- ресурс `/search` обрабатывает только GET запросы и его основная задача обслуживание поисковых запросов.

## Аутентификация
Вы должны поддерживать один из нижеуказанных методов аутентификации.

### LoginPasswordAuth

«[LoginPasswordAuth](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)» — реализация базовой Http аутентификации. Пара логин пароль передаются в виде строки `login:password` в `Authorization` заголовке. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)

### HttpTokenAut
«[HttpTokenAuth](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)» — реализация аутентификации по токену `HTTP Bearer token`. Для авторизации с помощью токена нужно передать его в `http` заголовке.  [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)

### QueryKeyAuth
«[QueryKeyAuth](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)» — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и `HttpTokenAuth`, т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)

### CryptoAuth
«[CryptoAuth](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)» работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption). [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)

### URL и типы запросов
- URL: `https://example.com/{api}/{version}/{resource}/{id}`
- или
- URL: `https://{api}.example.com/{version}/{resource}/{id}`
- `{api}` - директория API по умолчанию `api`
- `{version}` - версия API по умолчанию `v1`
- `{resource}` - название ресурса к которому обращаемся. Например price или user.
- `{id}` - уникальный индефикатор
- `{param}` - праметры запроса
#### Необходимо потдерживать запросы: `POST` `GET` `PUT` `PATCH` `DELETE`
- `POST /{table_name}` Создание записи 
- `POST /{table_name}/{id}` Ошибка
- `GET /{table_name}` Список всех записей
- `GET /{table_name}?{param}` Список всех записей с фильтром по параметрам
- `GET /{table_name}/{id}` Данные конкретной записи
- `PUT /{table_name}` Обновить данные записей
- `PUT /{table_name}/{id}` Обновить данные конкретной записи
- `PATCH /{table_name}` Обновить данные записей
- `PATCH /{table_name}/{id}` Обновить данные конкретной записи
- `DELETE /{table_name}` Удалить все записи
- `DELETE /{table_name}/{id}` Удалить конкретную запись
#### Параметры GET запроса
`?offset={offset}&limit={limit}&order={order}&sort={sort}&key={key}`
- `{key}` - Ключ доступа к API
- `{limit}` - Записей на страницу. По умолчанию 10
- `{offset}` - Страница. По умолчанию 0
- `{order}` - Тип сортировки. По умолчанию asc
- `{sort}` - Поле сортировки. По умолчанию id
- `{*}` - Пользовательский параметр
#### При использовании только `GET` запросов : Не обязательно !
Если вы хотите дать возможность обращаться к вашему API только с GET запросами, логика URL должна быть следующей:
- `GET /_post/{table_name}?{param}` Создание записи 
- `GET /_post/{table_name}/{id}` Ошибка
- `GET /_get/{table_name}?{param}` Список всех записей с фильтром по параметрам
- `GET /_get/{table_name}/{id}` Данные конкретной записи
- `GET /_put/{table_name}?{param}` Обновить данные записей
- `GET /_put/{table_name}/{id}?{param}` Обновить данные конкретной записи
- `GET /_patch/{table_name}?{param}` Обновить данные записей
- `GET /_patch/{table_name}/{id}?{param}` Обновить данные конкретной записи
- `GET /_delete/{table_name}` Удалить все записи
- `GET /_delete/{table_name}/{id}` Удалить конкретную запись

## Ресурсы

#### Поставщики
- `/supplier-product` `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Каталог товаров
- `/supplier-order` `GET`, `POST`, `PUT`, `PATCH` - Заказы
- `/supplier-seller` `GET`, `POST`, `PUT`, `PATCH` - Дилеры
#### Платежные сервисы
- `/payment` `POST` - Информация о статусе платежа
#### Транспортные компании
- `/delivery` `POST` - Информация о статусе груза
#### Торговые площадки
- `/marketplace-price` `GET` - Каталог товаров с ценами
- `/marketplace-order` `GET`, `POST`, `PUT`, `PATCH` - Заказы покупателей
#### Сайты работающие на REST API
- `/site` `GET` - Конфигурация сайта
- `/price` `GET` - Каталог товаров с ценами
- `/account` `GET`, `POST` - Пользователи
- `/order` `GET`, `POST` - Заказы покупателей
- `/checkout` `GET`, `POST` - Создание платежей
- `/discount` `GET` - Акции
- `/category` `GET` - Категории товаров
- `/brand` `GET` - Бренды
- `/article` `GET` - Статьи
- `/article-category` `GET` - Категории статей
- `/sitemap` `GET` - Карта сайта
- `/export` `GET` - Экспорт
- `/review` `GET`, `POST` - Отзывы
- `/pay` `GET` - Платежи
#### Транзитная API сайта работающего на PLLANO API
- `/payment` `POST` - Информация о статусе платежа
- `/delivery` `POST` - Информация о статусе груза

### 2. Структура ответов и запросов к ресурсам должна соответствовать стандарту APIS-2018

Глобальная структура ответа ресурса /price должна иметь следующую форму
- `headers` - заголовки
- `request` - запрос
- `response` - ответ
- `head` - голова (не обязательно)
- `body` - тело
- `footer` - подвал (не обязательно)

### 3. Ускорении разработки сайта
Структура APIS-2018 чем то напоминает структуру сайта и это очень хорошо, так как дает возможность в одном ответе отдать всю структуру html страницы сайта. Получив всю необходимую информацию для формирования страницы и обрабатывать сразу шаблонизатором без дополнительной обработки PHP. Это способно свести разработку сайта к работе верстальщика.

#### Структура json
```json
{
    "header": {
        "status": "200 OK",
        "code": "200",
        "message": "OK"
    },
    "response": {
        "api": "v1.0",
        "total": "100"
    },
    "request": {
        "query": "GET",
        "model": "price",
        "limit": "10",
        "offset": "0",
        "order": "DESC",
        "sort": "id",
        "state": "1",
        "type": "",
        "brand": "",
        "serie": "",
        "articul": "",
        "brand_id": "",
        "product_id": "",
        "search": ""
    },
    "head": {
        "meta": "",
        "link": "",
        "script": ""
    },
    "footer": {
        "html": "",
        "link": "",
        "script": ""
    },
    "body": {
        "items": [
            {
                "item": {
                    "id": "",
                    "product_id": "",
                    "parent_id": "",
                    "brand_id": "",
                    "price": "",
                    "oldprice": "",
                    "available": "",
                    "guarantee": "",
                    "ean": "",
                    "category": {
                        "id": "",
                        "parent_id": "",
                        "name": "",
                        "alias": ""
                    },
                    "supplier": {
                        "id": "",
                        "dropshipping": "",
                        "pay_online": ""
                    },
                    "seller": {
                        "id": "",
                        "name": ""
                    },
                    "delivery": {
                        "terms": ""
                    },
                    "currency": {
                        "currency_id": "UAH",
                        "short_sign": "₴",
                        "name": "грн.",
                    },
                    "name": "",
                    "type": "",
                    "brand": "",
                    "serie": "",
                    "articul": "",
                    "url": "",
                    "image": {
                        "1": "",
                        "2": ""
                    },
                    "description": "-",
                    "param": {
                        "Гарантия": "12 месяцев",
                        "Страна производитель": "Украина"
                    }
		}
            }
         ]
    }
}
```
#### Структура xml

Структура xml очень сильно напоминает стандартные xml для выгрузки на торговые площадки, 
за исключением того что содержит свои блоки: header, response, request

```xml
<?xml version="1.0" encoding="utf-8"?>
<price>
    <date>2017-11-01 12:59:59</date>
    <header>
        <status>200 OK</status>
        <code>200</code>
        <message>OK</message>
    </header>
    <response>
        <platform>pllano.com</platform>
        <api>v1.0</api>
        <date>2017-11-13 04:51:18</date>
        <encoding>utf-8</encoding>
        <total>26</total>
    </response>
    <request>
        <query>GET</query>
        <model>price</model>
        <limit>10</limit>
        <offset>0</offset>
        <order>DESC</order>
        <sort>id</sort>
        <state>1</state>
        <type></type>
        <brand></brand>
        <serie></serie>
        <articul></articul>
        <brand_id></brand_id>
        <product_id></product_id>
        <search></search>
    </request>
    <items>
        <item>
	    <id></id>
            <product_id></product_id>
            <parent_id></parent_id>
            <price></price>
            <oldprice></oldprice>
            <available></available>
            <guarantee></guarantee>
            <ean>0</ean>
            <category>
                <categoryId></categoryId>
                <parentId></parentId>
                <name></name>
                <alias></alias>
            </category>
            <supplier>
                <id></id>
                <dropshipping></dropshipping>
                <pay_online></pay_online>
            </supplier>
            <seller>
                <id>0</id>
                <name></name>
            </seller>
            <delivery>
                <terms>Бесплатная по Украине.</terms>
            </delivery>
            <currency>
                <currency_id>UAH</currency_id>
                <short_sign>₴</short_sign>
                <currency_name>грн.</currency_name>
            </currency>
            <name></name>
            <type></type>
            <brand brand_id=""></brand>
            <vendor vendor_id=""></vendor>
            <serie></serie>
            <articul></articul>
            <url></url>
            <images>
	        <image sort="1"></image>
                <image sort="2"></image>
            </images>
            <description>Описание</description>
            	<param name=""></param>
            	<param name=""></param>
        </item>
    </items>
</price>
```

<a name="feedback"></a>
## Поддержка, обратная связь, новости

Общайтесь с нами через почту open.source@pllano.com

Если вы нашли баг в APIS-2018 загляните в
[issues](https://github.com/pllano/APIS-2018/issues), возможно, про него мы уже знаем и
чиним. Если нет, лучше всего сообщить о нём там. Там же вы можете оставлять свои
пожелания и предложения.

За новостями вы можете следить по
[коммитам](https://github.com/pllano/APIS-2018/commits/master) в этом репозитории.
[RSS](https://github.com/pllano/APIS-2018/commits/master.atom).

Лицензия APIS-2018
-------

The MIT License (MIT). Please see [LICENSE](LICENSE.md) for more information.
