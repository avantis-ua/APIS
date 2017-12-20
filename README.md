# APIS-2018

«APIS-2018» — Стандарт обмена информацией между базами данных, торговыми платформами, интернет-магазинами, поставщиками товаров и сторонними платформами через RESTful API интерфейс.

## Теперь вам не нужно изобретать велосипед !

Вы можете писать свою API зная что другим API использующим стандарт APIS-2018 не придется тратиться на дополнительную доработку и интеграцию с вашим API. Для подключения к вашему API будет необходимо только получить данные аутентификации для доступа к учетной записи.

Этот стандарт используют: [«API Shop»](https://github.com/API-Shop/api-shop) и база данных [«API json DB»](https://github.com/pllano/api-json-db)

## Стандарт APIS-2018 стоит на двух основных принципах: 
- Адреса ресурсов и их назначение должны быть одинаковыми
- Структура ответов и запросов должна быть одинаковая

### 1. Адреса ресурсов и их назначение должны быть одинаковыми
Например:
- ресурс `/order` обрабатывает только `POST` `GET` `PUT` запросы и работает с заказами.
- ресурс `/price` обрабатывает только `GET` запросы и выдает информацию о ценах и наличии товара.
- ресурс `/product` обрабатывает только `GET` запросы и выдает информацию о товаре.
- ресурс `/search` обрабатывает только `GET` запросы и его основная задача обслуживание поисковых запросов.

### URL API
- URL: `https://example.com/{api}/{version}/{type}/{resource}/{id}`
- или
- URL: `https://{api}.example.com/{version}/{type}/{resource}/{id}`
- `{api}` - директория API по умолчанию `api`
- `{version}` - версия API по умолчанию `v1`
- `{type}` - тип данных `json`
- `{resource}` - название ресурса к которому обращаемся. Например `price` или `order`.
- `{id}` - уникальный индефикатор записи
- `{param}` - праметры запроса
#### Типы запросов: `POST` `GET` `PUT` `PATCH` `DELETE`
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
### Список всех поддерживаемых ресурсов
Использование всех ресурсов не является обязательным. Вы можете использовать только необходимые вам ресурсы.
- [`/order`](https://github.com/pllano/APIS-2018/blob/master/resource/order.md) `GET`, `POST`, `PUT`, `PATCH` - Заказы
- [`/user`](https://github.com/pllano/APIS-2018/blob/master/resource/user.md) `GET`, `POST` - Пользователи
- [`/price`](https://github.com/pllano/APIS-2018/blob/master/resource/price.md) `GET` - Каталог товаров продавца
- [`/warehouse`](https://github.com/pllano/APIS-2018/blob/master/resource/warehouse.md) `GET`, `POST`, `PUT`, `PATCH` - Склады, точки отгрузки, магазины
- [`/delivery`](https://github.com/pllano/APIS-2018/blob/master/resource/delivery.md) `POST` - Информация о статусе груза
- [`/pay`](https://github.com/pllano/APIS-2018/blob/master/resource/pay.md) `GET` - Платежи
- [`/checkout`](https://github.com/pllano/APIS-2018/blob/master/resource/checkout.md) `GET`, `POST` - Создание платежей
- [`/payment`](https://github.com/pllano/APIS-2018/blob/master/resource/payment.md) `POST` - Информация о статусе платежа
- [`/price-list`](https://github.com/pllano/APIS-2018/blob/master/resource/price-list.md) `GET`, `POST`, `PUT`, `PATCH`, `DELETE` - Каталог товаров от поставщиков
- [`/discount`](https://github.com/pllano/APIS-2018/blob/master/resource/discount.md) `GET` - Акции
- [`/category`](https://github.com/pllano/APIS-2018/blob/master/resource/category.md) `GET` - Категории товаров
- [`/brand`](https://github.com/pllano/APIS-2018/blob/master/resource/brand.md) `GET` - Бренды
- [`/sitemap`](https://github.com/pllano/APIS-2018/blob/master/resource/sitemap.md) `GET` - Карта сайта
- [`/export`](https://github.com/pllano/APIS-2018/blob/master/resource/export.md) `GET` - Экспорт
- [`/review`](https://github.com/pllano/APIS-2018/blob/master/resource/review.md) `GET`, `POST` - Отзывы
- [`/article`](https://github.com/pllano/APIS-2018/blob/master/resource/article.md) `GET` - Статьи
- [`/article-category`](https://github.com/pllano/APIS-2018/blob/master/resource/article-category.md) `GET` - Категории статей
- [`/site`](https://github.com/pllano/APIS-2018/blob/master/resource/site.md) `GET` - Конфигурация сайта
- [`/seller`](https://github.com/pllano/APIS-2018/blob/master/resource/seller.md) `GET`, `POST`, `PUT`, `PATCH` - Продавцы

### 2. Структура ответов и запросов должна быть одинаковая

Глобальная структура ответа ресурса /price должна иметь следующую форму
- [`headers`](https://github.com/pllano/APIS-2018/blob/master/structure/headers.md) - заголовки
- [`request`](https://github.com/pllano/APIS-2018/blob/master/structure/request.md) - запрос
- [`response`](https://github.com/pllano/APIS-2018/blob/master/structure/response.md) - ответ
- [`head`](https://github.com/pllano/APIS-2018/blob/master/structure/head.md) - голова (не обязательно)
- [`body`](https://github.com/pllano/APIS-2018/blob/master/structure/body.md) - тело
- [`footer`](https://github.com/pllano/APIS-2018/blob/master/structure/footer.md) - подвал (не обязательно)

### Структура ответа API при ошибке аутентификации
```json
{
    "header": {
        "status": "401 Unauthorized",
        "code": 401,
        "message": "Access is denied. The authentication method does not match.",
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/401.md"
    },
    "response": {
        "auth": "CryptoAuth",
        "total": 0
    }
}
```
[Коды состояния HTTP](https://github.com/pllano/APIS-2018/tree/master/http-codes)
### Минимальная структура ответа API
```json
{
    "header": {
        "status": "200 OK",
        "code": 200,
        "message": "RESTfull API json DB works!",
        "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/200.md"
    },
    "response": {
        "auth": "CryptoAuth",
        "total": 0
    }
}
```

### Параметр `relations`
`relations` - Очень важный параметр запроса позволяющий получать в ответе необходимые данные из других связанных ресурсов.

Используется четыри символа: `'` `.` `,` `:`

Сами ресурсы перечисляем через точку `'product'.'user'.'user_data'.'address'` экранируя одинарными кавычками
```
"relations" => "'product'.'user'.'user_data'.'address'"
```

Если необходимо запросить конкретные данные указываем их через двоеточие `'user:phone'` если нужно несколько через запятую `'product:type_id,brand_id,serie_id,articul'`

```
"relations" => "'product:type_id,brand_id,serie_id,articul'.'user:phone'.'address:street_id'"
```

В нашем запросе к ресурсу `order` мы хотим дополнительно получить: 
- `product` - товары в заказе
- `user` - данные покупателя
- `user_data` - данные покупателя
- `address` -  данные адреса покупателя

### Пример `GET` запроса к ресурсу `order` через HTTP клиент Guzzle
``` php
use GuzzleHttp\Client as Guzzle;

// Взять public_key из конфигурации
$public_key = $config->get("public_key");

// Наш запрос
$data = [
    "public_key" => $public_key,
    "limit" => 10,
    "offset" => 0,
    "order" => "DESC",
    "sort" => "created",
    "state" => 1,
    "date_from" => "2017-12-07",
    "date_to" => "2017-12-14",
    "relations" => "'product'.'user'.'user_data'.'address'"
];

// Массив в URL-кодированную строку запроса
$data_query = http_build_query($data) . "\n";

// Формируем URL запроса
$uri = "https://example.com/api/v1/json/order?".$data_query;

// Подключаем Guzzle
$client = new Guzzle();

// Отправляем запрос
$response = $client->request("GET", $uri);

// Получаем тело ответа
$output = $response->getBody();

// json в массив
$records = json_decode($output, true);

// Работаем с массивом
if (isset($records["headers"]["code"])) {
if ($records["headers"]["code"] == 200) {
	$count = count($records["body"]["items"]);
	if ($count >= 1) {
		foreach($records["body"]["items"] as $item)
		{
			print_r($item["item"]);
		}
	}
}
}

```
``` php
// Вывести на экран json
print_r($records);
```
### Структура ответа торговой площадки на наш `GET` запрос в json формате
```json
{
  "header": {
    "status": "200 OK",
    "code": "200",
    "message": "OK",
    "message_id": "https:\/\/github.com\/pllano\/APIS-2018\/tree\/master\/http-codes\/200.md"
  },
  "response": {
    "auth": "QueryKeyAuth",
    "total": "10"
  },
  "request": {
    "query": "GET",
    "resource": "order",
    "limit": "10",
    "offset": "0",
    "order": "DESC",
    "sort": "created",
    "state": "1",
    "date_from": "2017-12-07",
    "date_to": "2017-12-14",
    "relations": "product, user, address"
  },
  "body": {
    "items": [{
      "item": {
        "id": "1234567890",
        "created": "2017-12-11 10:30",
        "status": 1,
        "delivery": "Novaposhta",
        "delivery_code": "1234567890121",
        "total_amount": "10501.00",
        "currency_id": "UAH",
        "comment": "Могу принять после 17:00",
        "user": {
          "phone": "380670000001",
          "email": "user@example.com"
        },
        "user_data": {
          "fname": "Иванов",
          "iname": "Юрий",
          "oname": "Петрович"
        },
        "address": {
          "country": "Украина",
          "region": "Киевская область",
          "postal_code": 0,
          "city": "Киев",
          "district": "Позняки",
	  "street": "Бажана",
	  "number": "12а/17",
	  "parade": "0",
          "floor": "0",
	  "apartment": "75",
	  "additional": "Код на парадном 107"
        },
        "products": [
           {
             "product": {
               "product_id": "2001500",
               "status": 1,
               "name": "Type BrendName SerieName Articul",
               "price": "2500.50",
               "oldprice": "2500.50",
               "num": 2,
               "available": 20,
               "total_price": "5001.00",
               "currency_id": "UAH",
               "guarantee": 24,
               "pay_online": 1
             }
           }, {
             "product": {
               "product_id": "1000120",
               "status": 1,
               "name": "Ноутбук Asus X751NV (X751NV-TY001) Black",
               "type": "Ноутбук",
               "brand": "Asus",
               "type": "X751NV",
               "articul": "(X751NV-TY001) Black",
               "price": "5750.50",
               "oldprice": "5500.00",
               "num": 1,
               "available": 5,
               "total_price": "5500.00",
               "currency_id": "UAH",
               "guarantee": 36,
               "pay_online": 1
             }
           }
        ]
      }
    }]
  }
}
```
## Аутентификация
Вы должны поддерживать один из нижеуказанных методов аутентификации.

### LoginPasswordAuth

[«LoginPasswordAuth»](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md) — реализация базовой Http аутентификации. Пара логин пароль передаются в виде строки `login:password` в `Authorization` заголовке. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/LoginPasswordAuth.md)

### HttpTokenAuth
[«HttpTokenAuth»](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md) — реализация аутентификации по токену `HTTP Bearer token`. Для авторизации с помощью токена нужно передать его в `http` заголовке.  [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/HttpTokenAuth.md)

### QueryKeyAuth
[«QueryKeyAuth»](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md) — пожалуй самый простой из имеющихся способов авторизации. Работает по тому-же принципу, что и `HttpTokenAuth`, т.е. идентифицирует пользователя по токену, только в данном случае токен передается в строке запроса, как GET параметр. По умолчанию параметр называется `public_key`. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md)

### CryptoAuth
[«CryptoAuth»](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md) работает по тому-же принципу, что и [`QueryKeyAuth`](https://github.com/pllano/APIS-2018/blob/master/doc/QueryKeyAuth.md), т.е. токен передается как `GET` или `POST` параметр, только в данном случае данные передаются в зашифрованном виде с помощью `private_key` библиотекой [defuse/php-encryption](https://github.com/defuse/php-encryption) или подобной. [подробнее ...](https://github.com/pllano/APIS-2018/blob/master/doc/CryptoAuth.md)

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
