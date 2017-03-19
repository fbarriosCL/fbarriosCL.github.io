---
layout: post
title:  "Elasticsearch Parte II - Busquedas"
date:   2016-05-08 00:37:19 -0300
categories: elasticsearch
---
## Search API

```javascript
curl -XGET 'search-es:9200/bank/_search?q=*&sort=account_number:asc&pretty&pretty'
```

Primero buscamos ( ```_search``` final) en el índice de bancos, y el ```q=*``` parámetro indica Elasticsearch para que coincida con todos los documentos en el índice. El ```sort=account_number:asc``` parámetro indica para ordenar los resultados usando el ```account_number``` campo de cada documento en un orden ascendente. El ```pretty``` parámetro, de nuevo, sólo le dice Elasticsearch para devolver resultados JSON bastante impresos.

## Introducctión a Query Language
<hr>
### Query DSL

```query``` definición de la consulta.

```match_all``` consulta es una búsqueda en todos los documentos en el índice especificado.

También podemos pasar a otros parámetros para influir en los resultados de búsqueda. En el ejemplo de la sección anterior pasamos en ```sort```, ```size```:

El parámetro ```from``` especifica el índice de documentos para iniciar desde y al size parámetro especifica el número de documentos para volver a partir de la del parámetro. Esta función es útil cuando se implementa paginación de resultados de búsqueda. Tenga en cuenta que si from no se especifica, el valor predeterminado es 0.

Este ejemplo devolvera "_source": ["account_number", "balance"]


```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match_all": {} },
  "sort": { "balance": { "order": "desc" } },
  "from": 10,
  "size": 10
  "_source": ["account_number", "balance"]
}'
```

### Match query

Retorna todos los ```account_number``` cuando su valor es 20

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match": { "account_number": 20 } }
}'
```

Este ejemplo devuelve todas las cuentas que contienen el término "mill" o "lane" en ```address```:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match": { "address": "mill lane" } }
}'
```

```match_phrase``` devuelve todas las cuentas que contienen la frase "carril del molino" en  ```address```:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match_phrase": { "address": "mill lane" } }
}'
```

### bool

La consulta ```bool``` nos permite componer consultas más grandes en consultas más pequeñas que utilizan lógica booleana.

Ejemplo: Devuelve todas las cuentas que contienen "mill" AND "lane" en ```address```:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": {
    "bool": {
      "must": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```

```must``` == AND 

Ejemplo: Devuelve todas las cuentas que contienen "molino" OR "carril" en ```adress```:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": {
    "bool": {
      "should": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```

```should``` == OR 


Ejemplo: se compone de dos consultas match y devuelve todas las cuentas que no contienen ni "molino" ni "Lane" en ```adress```:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": {
    "bool": {
      "must_not": [
        { "match": { "address": "mill" } },
        { "match": { "address": "lane" } }
      ]
    }
  }
}'
```

## Filtros

```_score``` es la relevancia de nuestro documento

```filter``` permite utilizar una consulta para restringir los documentos que serán igualados por otras cláusulas.

En este ejemplo se utiliza una consulta ```bool``` para devolver todas las cuentas con saldos entre 20000 y 30000, ambos inclusive. En otras palabras, queremos encontrar cuentas con un equilibrio que es mayor que o igual a 20.000 e inferior o igual a 30.000.

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}'

```

## Executing Aggregations

```javascript
curl -XGET 'localhost:9200/ banco / _search?pretty' -d'
{
  "Tamaño": 0,
  "aggs": {
    "Group_by_state": {
      "términos": {
        "Campo": "state.keyword"
      }
    }
  }
}'
```

```
SELECT state, COUNT(*) FROM bank GROUP BY state ORDER BY COUNT(*) DESC
```

otro ejemplo 

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}'
```



<!-- Querys mas especificas

curl -XGET "http://search-es:9200/articles/article/A1?
						_soruce_include=*id&_source_explude=*pidcture"


Para comprobar si existe el documento, evitar el overhead de transferir el JSON

curl -XHEAD 'http://search-es:9200/articles/article/A1'

Sin metadatos extra

curl -XHEAD 'http://search-es:9200/articles/article/A1/_source'

curl -XPOST 'http://search-es:9200/articles/article/A1/_update' -d {
	"id" : ""
	"title" : ""
}

## SEARCH API

Relevancia(Scoring) : define que tan importante es un documento en un conjunto de resultados.

Spellchecker : Permite interpretar una busqueda aunque tenga errores ortograficos

Multi-lenguaje

autocomplete


### Busquedas de Texto:

busquedas booleanas

curl -XGET 'http://search-es:9200/articles/A1/_search?q=sony+OR+nikon'

busquedas por rango

curl -XGET 'http://search-es:9200/articles/A1/_search?q=proce:[10+TO+*]'


SHARD ROUTING

curl -XPUT 'http://search-es:9200/articles/A1/_routing=MLB' -d {...}


QUERY DSL
{
	"query" : {...}
	"filter" : {...}

}

busqueda de texto

{ 
 	"match" : { "titulo" : "as"}
}

buscando frases

{

}

max_expansion

multi_match buscar en muchos campos

match_all
 -->









