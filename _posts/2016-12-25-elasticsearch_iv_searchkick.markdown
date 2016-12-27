---
layout: post
title:  "Elasticsearch Parte IV - searchkick"
date:   2016-05-08 00:37:19 -0300
categories: elasticsearch
---

Agregamos a nuestro ```Gemfile```

```ruby
class Document < ActiveRecord::Base
  searchkick
end
```

```ruby
class Document < ActiveRecord::Base
  searchkick language: "spanish", wordnet: true

  def self.search_bta(query)
    search(query,
      fields: ['title^10', 'body', 'keyword'],
      misspellings: { edit_distance: 2 ,
                      below: 5
                    }
      )
  end
end
```

```
language: idioma de busquedas.
wordnet: diccionario de sinonimos.
fields: le damos prioridad.
misspellings: para falta ortografia.
bellow: Si hay menos de 5 resultados, se realiza una segunda búsqueda con errores ortográficos habilitados. Se devuelve el resultado de esta consulta.
```

### Ejemplos de query:

Por defecto los registros se obtienen de Elasticsearch se extraen de la base de datos. Para obtener todo de Elasticsearch:

```ruby
  load: false
```

```ruby
where: {
  expires_at: {gt: Time.now}, # lt, gte, lte also available
  orders_count: 1..10,        # equivalent to {gte: 1, lte: 10}
  aisle_id: [25, 30],         # in
  store_id: {not: 2},         # not
  aisle_id: {not: [25, 30]},  # not in
  user_ids: {all: [1, 3]},    # all elements in array
  category: /frozen .+/,      # regexp
  _or: [{in_stock: true}, {backordered: true}]
},
  limit: 10,
  offset: 50

```

### order 

ordenar por relevancia

```ruby 
order: {_score: :desc }
```

### Boosting
Impulsar campos importantes

```ruby
fields: ["title^10", "description"]
```

Aumentar el valor de un campo (Solo casos numéricos)

```ruby
boost_by: [:orders_count] # give popular documents a little boost
boost_by: {orders_count: {factor: 10}} # default factor is 1
```

Las conversiones también son una buena opcion para impulsar.

### Partial Matches

```ruby
:word # default
:word_start
:word_middle
:word_end
:text_start
:text_middle
:text_end
```

### Exact Matches

```ruby
User.search params[:q], fields: [{email: :exact}, :name]
```

### Language

```ruby
 searchkick language: "german"
```


## SEARCHJOY

add Gemfile:

```ruby
gem "searchjoy"
```

And run the generator.

```ruby
rails generate searchjoy:install
```

```ruby
rake db:migrate
```

A continuación, agregue  ```config/routes.rb```.

```ruby
mount Searchjoy::Engine, at: "admin/searchjoy"
```

Para comprobar

```
http://localhost:3000/admin/searchjoy/
```
