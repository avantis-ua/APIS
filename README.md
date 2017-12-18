# APIS-2018

«APIS-2018» — Стандарт обмена информацией между базами данных, торговыми платформами, интернет-магазинами, поставщиками товаров и сторонними платформами через RESTful API интерфейс.

Этот стандарт используют: [«API Shop»](https://github.com/pllano/api-shop) и база данных [«API json DB»](https://github.com/pllano/api-json-db)

### `Стандарт APIS-2018 - не является общепринятым !`
Стандарт является взглядом в будущее и рекомендацией для унификации обмена данными сервер-сервер.

Если вы начнете использовать формат APIS-2018 сейчас, вы можете быть уверенны в том что в будущем вам будет намного легче настроить работу со сторонними RESTful API

Мы конечно же хотим достичь чего то подобного на [schema.org](http://schema.org) но учитывая специфику E-commerce этот стандарт нам не совсем подходит.

## Стандарт APIS-2018 стоит на двух основных принципах: 
- Адреса ресурсов должны быть одинаковыми
- Структура ответов и запросов должны быть одинаковыми

Таким образом вы можете писать свою API зная что другим API использующим наш стандарт не придется тратиться на дополнительную доработку и интеграцию с вашим API.

### 1. Адреса ресурсов и типы запросов с которыми работает [API Shop](https://github.com/pllano/api-shop) должны быть одинаковыми

Например:
- ресурс `/price` обрабатывает только GET запросы и выдает информацию о ценах и наличии товара.
- ресурс `/product` обрабатывает только GET запросы и выдает информацию о товаре.
- ресурс `/search` обрабатывает только GET запросы и его основная задача обслуживание поисковых запросов.

## Аутентификация
Вы должны поддерживать один из нижеуказанных методов аутентификации.

### LoginPasswordAuth

«[LoginPasswordAuth](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)» — реализация базовой Http аутентификации. Пара логин пароль передаются в виде строки `login:password` в `Authorization` заголовке. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)

### HttpTokenAuth
«[HttpTokenAuth](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)» — реализация аутентификации по токену `HTTP Bearer token`. Для авторизации с помощью токена нужно передать его в `http` заголовке.  [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)

### QueryKeyAuth
«[QueryKeyAuth](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)» — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и `HttpTokenAuth`, т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)

### CryptoAuth
«[CryptoAuth](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)» работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption) или подобной. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)

### URL и типы запросов
- URL: `https://example.com/{api}/{version}/{type}/{resource}/{id}`
- или
- URL: `https://{api}.example.com/{version}/{type}/{resource}/{id}`
- `{api}` - директория API по умолчанию `api`
- `{version}` - версия API по умолчанию `v1`
- `{type}` - тип данных `json`
- `{resource}` - название ресурса к которому обращаемся. Например price или user.
- `{id}` - уникальный индефикатор
- `{param}` - праметры запроса
#### Необходимо потдерживать запросы: `POST` `GET` `PUT` `PATCH` `DELETE`
- `POST /{resource}` Создание записи 
- `POST /{resource}/{id}` Ошибка
- `GET /{resource}` Список всех записей
- `GET /{resource}?{param}` Список всех записей с фильтром по параметрам
- `GET /{resource}/{id}` Данные конкретной записи
- `PUT /{resource}` Обновить данные записей
- `PUT /{resource}/{id}` Обновить данные конкретной записи
- `PATCH /{resource}` Обновить данные записей
- `PATCH /{resource}/{id}` Обновить данные конкретной записи
- `DELETE /{resource}` Удалить все записи
- `DELETE /{resource}/{id}` Удалить конкретную запись
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
- `GET /_post/{resource}?{param}` Создание записи 
- `GET /_post/{resource}/{id}` Ошибка
- `GET /_get/{resource}?{param}` Список всех записей с фильтром по параметрам
- `GET /_get/{resource}/{id}` Данные конкретной записи
- `GET /_put/{resource}?{param}` Обновить данные записей
- `GET /_put/{resource}/{id}?{param}` Обновить данные конкретной записи
- `GET /_patch/{resource}?{param}` Обновить данные записей
- `GET /_patch/{resource}/{id}?{param}` Обновить данные конкретной записи
- `GET /_delete/{resource}` Удалить все записи
- `GET /_delete/{resource}/{id}` Удалить конкретную запись

## Ресурсы
Список всех поддерживаемых ресурсов

#### Поставщики
- [`/supplier-product`](https://github.com/pllano/APIS-2018/blob/master/resource/supplier-product.md) `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Каталог товаров
- [`/supplier-order`](https://github.com/pllano/APIS-2018/blob/master/resource/supplier-order.md) `GET`, `POST`, `PUT`, `PATCH` - Заказы
- [`/supplier-seller`](https://github.com/pllano/APIS-2018/blob/master/resource/supplier-seller.md) `GET`, `POST`, `PUT`, `PATCH` - Дилеры

#### Платежные сервисы
- [`/payment`](https://github.com/pllano/APIS-2018/blob/master/resource/payment.md) `POST` - Информация о статусе платежа

#### Транспортные компании
- [`/delivery`](https://github.com/pllano/APIS-2018/blob/master/resource/delivery.md) `POST` - Информация о статусе груза

#### Торговые площадки
- [`/marketplace-price`](https://github.com/pllano/APIS-2018/blob/master/resource/marketplace-price.md) `GET` - Каталог товаров с ценами
- [`/marketplace-order`](https://github.com/pllano/APIS-2018/blob/master/resource/marketplace-order.md) `GET`, `POST`, `PUT`, `PATCH` - Заказы покупателей

#### Сайты работающие через API
- [`/site`](https://github.com/pllano/APIS-2018/blob/master/resource/site.md) `GET` - Конфигурация сайта
- [`/price`](https://github.com/pllano/APIS-2018/blob/master/resource/price.md) `GET` - Каталог товаров с ценами
- [`/account`](https://github.com/pllano/APIS-2018/blob/master/resource/account.md) `GET`, `POST` - Пользователи
- [`/order`](https://github.com/pllano/APIS-2018/blob/master/resource/order.md) `GET`, `POST` - Заказы покупателей
- [`/checkout`](https://github.com/pllano/APIS-2018/blob/master/resource/checkout.md) `GET`, `POST` - Создание платежей
- [`/discount`](https://github.com/pllano/APIS-2018/blob/master/resource/discount.md) `GET` - Акции
- [`/category`](https://github.com/pllano/APIS-2018/blob/master/resource/category.md) `GET` - Категории товаров
- [`/brand`](https://github.com/pllano/APIS-2018/blob/master/resource/brand.md) `GET` - Бренды
- [`/article`](https://github.com/pllano/APIS-2018/blob/master/resource/article.md) `GET` - Статьи
- [`/article-category`](https://github.com/pllano/APIS-2018/blob/master/resource/article-category.md) `GET` - Категории статей
- [`/sitemap`](https://github.com/pllano/APIS-2018/blob/master/resource/sitemap.md) `GET` - Карта сайта
- [`/export`](https://github.com/pllano/APIS-2018/blob/master/resource/export.md) `GET` - Экспорт
- [`/review`](https://github.com/pllano/APIS-2018/blob/master/resource/review.md) `GET`, `POST` - Отзывы
- [`/pay`](https://github.com/pllano/APIS-2018/blob/master/resource/pay.md) `GET` - Платежи
#### Транзитная API сайта работающего через API
- [`/payment`](https://github.com/pllano/APIS-2018/blob/master/resource/payment.md) `POST` - Информация о статусе платежа
- [`/delivery`](https://github.com/pllano/APIS-2018/blob/master/resource/delivery.md) `POST` - Информация о статусе груза

### 2. Структура ответов и запросов к ресурсам должна соответствовать стандарту APIS-2018

Глобальная структура ответа ресурса /price должна иметь следующую форму
- [`headers`](https://github.com/pllano/APIS-2018/blob/master/structure/headers.md) - заголовки
- [`request`](https://github.com/pllano/APIS-2018/blob/master/structure/request.md) - запрос
- [`response`](https://github.com/pllano/APIS-2018/blob/master/structure/response.md) - ответ
- [`head`](https://github.com/pllano/APIS-2018/blob/master/structure/head.md) - голова (не обязательно)
- [`body`](https://github.com/pllano/APIS-2018/blob/master/structure/body.md) - тело
- [`footer`](https://github.com/pllano/APIS-2018/blob/master/structure/footer.md) - подвал (не обязательно)

### 3. Ускорении разработки сайта
Структура APIS-2018 чем то напоминает структуру сайта и это очень хорошо, так как дает возможность в одном ответе отдать всю структуру html страницы сайта. Получив всю необходимую информацию для формирования страницы и обрабатывать сразу шаблонизатором без дополнительной обработки PHP. Это способно свести разработку сайта к работе верстальщика.

#### Структура json
```json
[{
    "header": {
        "status": "200 OK",
        "code": "200",
        "message": "OK"
    },
    "response": {
        "auth": "CryptoAuth",
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
    },
    "footer": {
        "html": "",
        "link": "",
        "script": ""
    }
}]
```

## `Мы ищем единомышленников — Присоединяйтесь!`

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
