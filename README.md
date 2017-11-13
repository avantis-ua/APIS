# APIS-2018
APIS-2018 - Стандарт разметки для REST API магазинов разрабатываемый компанией PLLANO

## Стандарт APIS-2018 предусматривает два основных подстандарта:

### 1. Адреса ресурсов к которым обращаеться API Shop должны быть одинаковыми

Например ресурс /price обрабатывает только GET запросы

### 2. Структура ответов и запросов к ресурсам

Глобальная структура ответа ресурса /price должна иметь следующую форму

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
        "total": "1000"
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
    "price": {
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
