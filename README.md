# APIS-2018

APIS-2018 - Стандарт для интернет-магазинов работающих без своей базы данных и использующих REST API торговых платформ для хранения и обработки информации по типу движка с открытым исходным кодом [API Shop](https://github.com/API-Shop/api-shop) разрабатываемого компанией PLLANO.com

## Цель 
Разработать стандарт для обмена информацией между интернет-магазинами, поставщиками и сторонними сервисами.

`Стандарт APIS-2018 - не является общепринятым !` Стандарт является взглядом в будущее и рекомендацией для унификации построения легких движков интернет-магазинов нового поколения.

## Мы ищем единомышленников ! Присоединяйтесь.

Мы конечно же хотим достичь чего то подобного на [schema.org](http://schema.org) но учитывая специфику E-commerce этот стандарт нам не совсем подходит.

## Стандарт APIS-2018 стоит на двух основных принципах: 
Адреса ресурсов и основная структура ответов и запросов должны быть одинаковыми

### 1. Адреса ресурсов и типы запросов с которыми работает [API Shop](https://github.com/API-Shop/api-shop) должны быть одинаковыми

Например:

ресурс `/price` обрабатывает только GET запросы и выдает информацию о товаре, ценах и наличии.

ресурс `/search` обрабатывает только GET запросы и его основная задача обслуживание поисковых запросов.

#### Поставщики
Каталог товаров - supplier-product GET, POST, PUT, DELETE
Цена товара - product->price
Наличие товара - product->available или product->stock
Описание товара (Тексты) product->description и description->text
Характеристики товара (Свойства) - product->params->param и params->param
Изображения товара (Картинки) - product->images->image и images->image
Заказы - supplier-order GET, POST, PUT
Дилеры - supplier-seller GET, POST, PUT

#### Платежные сервисы
Информация о статусе платежа - payment POST

#### Транспортные компании
Информация о статусе груза - delivery POST

#### Торговые площадки
Каталог товаров с ценами - marketplace-price GET
Заказы покупателей - marketplace-order GET, POST, PUT

#### Сайты работающие на REST API
Конфигурация сайта - site GET
Каталог товаров с ценами - price GET
Пользователи - account GET, POST
Заказы покупателей - order GET, POST
Создание платежей - checkout GET, POST
Акции - discount GET
Категории товаров - category GET
Бренды - brand GET
Статьи - article GET
Категории статей - article-category GET
Карта сайта - sitemap GET
Экспорт - export GET
Отзывы - review GET, POST

#### Транзитная API сайта работающего на PLLANO API
Информация о статусе платежа - payment POST
Информация о статусе груза - delivery POST

### 2. Структура ответов и запросов к ресурсам должна соответствовать стандарту APIS-2018

Глобальная структура ответа ресурса /price должна иметь следующую форму

`header` - заголовоки

`response` - ответ

`request` - запрос

`head` - голова

`body` - тело

`footer` - подвал



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
