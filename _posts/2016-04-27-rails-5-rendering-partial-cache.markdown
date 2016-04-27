---
layout: post
title:  "Rails 5 render desde un partials cache"
date:   2016-04-26 00:37:19 -0300
categories: jekyll update
---
Vamos a echar un vistazo a un fragmento de código en rails 5 que hace uso de una colección parcial.

```
# index.html.erb
<%= render partial: 'todo', collection: @todos %>
```

```
# _todo.html.erb
<% cache todo do %>
  <%= todo.name %>
<% end %>
```

En el caso anterior Rails hará una peticion recuperar  la memoria caché para cada ```todo```

Fetch es por lo general bastante rápido con cualquier solución de almacenamiento en caché, sin embargo, FETCH por TODO puede hacer que la aplicación sea lenta.

La gema ```multi_fetch_fragments``` fijaron este problema utilizando la api ```read_multi``` proporcionada por Rails.

Con un solo request a la memoria caché, esta gema obtiene todos los fragmentos de caché para una colección. El autor de la gema vio una mejora de 78% en velocidad mediante el uso de esta gema.

Para obtener los beneficios de almacenamiento en caché de colección, sólo tiene que añadir en caché: true, como se muestra a continuación.

```
# index.html.erb
<%= render partial: 'todo', collection: @todos, cached: true %>

# _todo.html.erb
<% cache todo do %>
  <%= todo.name %>
<% end %>s
```

Con ```caché: true``` presente, Rails utiliza ```read_multi``` a la tienda de caché en lugar de leer de ella cada parcial. Rails también registrarán aciertos de caché en los registros de la siguiente manera.

```
Rendered collection of todos/_todo.html.erb [100 / 100 cache hits] (339.5ms)
```








