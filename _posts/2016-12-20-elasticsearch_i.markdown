---
layout: post
title:  "Elasticsearch Parte I - Conocimientos básicos"
date:   2016-05-07 00:37:19 -0300
categories: elasticsearch
---


## Introducción elasticsearch


Hay algunos conceptos que son fundamentales para Elasticsearch. Comprender estos conceptos desde el principio ayudará enormemente a facilitar el proceso de aprendizaje.

### arquitectura: 

Elasticsearch > Índices > Types > Documents > Fields

### nodo
Un nodo es un servidor que forma parte del clúster, almacena los datos, y participa en las capacidades de indexación y búsqueda del cluster.

### índice
Un índice es una colección de documentos que tienen características algo similares.

### tipo
Dentro de un índice, se pueden seleccionar uno o más tipos. Un tipo es una categoría / partición lógica de su índice de cuya semántica es totalmente suya. En general, un tipo se define para documentos con un conjunto de campos comunes.

### documento
Un documento es una unidad básica de información que puede ser indexado. Este documento se expresa en JSON (JavaScript Object Notation), que es un formato de intercambio de datos de Internet ubicua.

### shard y réplicas
Un índice  puede almacenar una gran cantidad de datos que puede exceder los límites de hardware de un solo nodo. 

Para resolver este problema, Elasticsearch ofrece la posibilidad de subdividir el índice en varios trozos llamados fragmentos. Cuando se crea un índice, sólo tiene que definir el número de fragmentos que desee. Cada fragmento es en sí mismo un "índice" totalmente funcional e independiente que se puede alojar en cualquier nodo del clúster.

Sharding es importante por dos razones principales:

* Le permite dividida horizontalmente / o reducir el volumen de contenido.
* Permite distribuir y paralelizar las operaciones a través de fragmentos (potencialmente en varios nodos) aumentando así el rendimiento.

La replicación es importante por dos razones principales:

* Proporciona alta disponibilidad en caso de que falle un fragmento / nodo. Por esta razón, es importante tener en cuenta que un fragmento de réplica no se asigna en el mismo nodo que el fragmento primario / original que fue copiado.
* Se le permite escalar a cabo volumen de búsquedas y / rendimiento puesto que las búsquedas se pueden ejecutar en todas las réplicas en paralelo.

Vamos a empezar con un chequeo, para comprobar el estado del clúster, vamos a utilizar la ```_catAPI```

Añadimos ```pretty``` final de la llamada para indicarle que debe bastante-imprimir la respuesta JSON (si lo hay).

ES agrega su propia metadata:

```
_id
_type
_source
_all
_timesstamp
_ttl
_size
```

Respuesta

201 created, se creo un nuevo documento
200 se actualizo
404 not found

```javascript
{
"ok" : true,  
"_index" :
"_type" :
"_id" : 
"_version" :
}
```

```javascript
curl -XGET 'search-es:9200/_cat/health?v&pretty'
```

```
curl -XGET 'search-es:9200/_cat/health?v&pretty'
epoch      timestamp cluster       status node.total node.data shards pri relo init unassign pending_tasks
1482455962 01:19:22  elasticsearch yellow          1         1     10  10    0    0       10             0
```

#### Obtener nodos

```javascript
curl -XGET 'search-es:9200/_cat/nodos?v&pretty'
```

#### Obtener indices

```javascript
curl -XGET 'search-es:9200/_cat/indices?v&pretty'
```

```
curl -XGET 'search-es:9200/_cat/indices?v&pretty'
health status index    pri rep docs.count docs.deleted store.size pri.store.size
yellow open   article    5   1          1            0      3.1kb          3.1kb
yellow open   products   5   1          1            0        3kb            3kb
```

## CRUD

#### Crear documento:

Al indexar, la parte ID es opcional. Si no se especifica, Elasticsearch generará un ID aleatorio y luego utilizarlo para indexar el documento. El actual ID Elasticsearch genera (o lo que sea que se especifica explícitamente en los ejemplos anteriores).

```javascript
curl -XPOST "search-es:9200/article/news/1" -d '{ "title" : "Ejemplo titulo" }'

or

curl -XPOST 'search-es:9200/article/news?' -d '{ "title" : "Jane Doe" }'

```

#### Obtener documento por id

```javascript
curl -XGET 'search-es:9200/article/news/1'
```

#### Borrar documento por ID

```javascript
curl -XDELETE 'search-es:9200/documents_development'
```

#### Actualizar documento

```javascript
curl -XPOST 'localhost:9200/article/news/1/_update?' -d'
{
  "doc": { "title": "Jane Doe" }
}'
```

#### Buscar documento por texto

```javascript
curl -XGET 'search-es:9200/article/_search?p=Ejemplo'
```

