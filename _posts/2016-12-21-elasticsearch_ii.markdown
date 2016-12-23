---
layout: post
title:  "Elasticsearch Parte II - Busquedas"
date:   2016-05-08 00:37:19 -0300
categories: elasticsearch
---

```javascript
curl -XGET 'search-es:9200/bank/_search?q=*&sort=account_number:asc&pretty&pretty'
```

Primero buscamos ( ```_search``` final) en el índice de bancos, y el ```q=*``` parámetro indica Elasticsearch para que coincida con todos los documentos en el índice. El ```sort=account_number:asc``` parámetro indica para ordenar los resultados usando el ```account_number``` campo de cada documento en un orden ascendente. El ```pretty``` parámetro, de nuevo, sólo le dice Elasticsearch para devolver resultados JSON bastante impresos.

Esta es la misma búsqueda exacta anteriormente utilizando el método de solicitud cuerpo alternativa:

``javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}'
```

La disección de lo anterior, la queryparte nos dice lo que es nuestra definición de la consulta y la match_allparte es simplemente el tipo de consulta que queremos ejecutar. La match_allconsulta es simplemente una búsqueda de todos los documentos en el índice especificado.

Además del queryparámetro, también podemos pasar a otros parámetros para influir en los resultados de búsqueda. En el ejemplo de la sección anterior pasamos en sort, aquí pasamos en size:


Tenga en cuenta que si size no se especifica, el valor predeterminado es 10.

En este ejemplo se hace una match_ally devuelve los documentos 11 a 20:

```javascript
curl -XGET 'localhost:9200/bank/_search?pretty' -d'
{
  "query": { "match_all": {} },
  "from": 10,
  "size": 10
}'

```

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

con must, ambas condiciones se deben cumplir


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

con should, cualquiera de las dos

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

con must_not, cualquiera de las dos











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









