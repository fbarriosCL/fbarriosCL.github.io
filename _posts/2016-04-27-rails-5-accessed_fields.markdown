---
layout: post
title:  "Rails 5 - accessed_fields"
date:   2016-04-26 00:37:19 -0300
categories: rails
---
 
En el siguiente ejemplo vamos a conocer una nueva funcionalidad que trae Rails5 para poder encontrar los campos que realmente estamos utilizando.

Para ello vamos a crear un proyecto en Rails5.

Una vez instalada y configurada nuestra aplicación, rails nos proporciona una manera muy fácil de seleccionar todos los campos de una base de datos.

{% highlight ruby %}
@users = User.all
{% endhighlight %}

En la mayoria de los casos esto esta correcto, pero en alguna ocacines es posible que desee seleccionar  ciertas columnas para motivos de rendimiento y optimización.La tarea difícil es encontrar lo que todas las columnas se utilizan realmente en una solicitud.

Para esto, Rails5 ha añadido ```accessed_fields``` método que enumera los atributos que realmente se utilizaron en la operación.

Esto es útil en desarrollo cuando todos los campos están siendo utilizados por la aplicación.

{% highlight ruby %}
class UsersController < ApplicationController
  def index
    @users = User.all
  end
end
{% endhighlight %}

```
# app/views/users/index.html.erb

<table>
  <tr>
    <th>Name</th>
    <th>Email</th>
  </tr>
  <% @users.each do |user| %>
    <tr>
      <td><%= user.name %></td>
      <td><%= user.email %></td>
    </tr>
  <% end %>
</table>
```

Ahora, con la finalidad de encontrar todos los campos que realmente se utilizaron, vamos a añadir  callback ```after_action``` en nuestro controlador.

{% highlight ruby %}
class UsersController < ApplicationController
  after_action :print_accessed_fields
  
  def index
    @users = User.all
  end

  private
  def print_accessed_fields
    p @users.first.accessed_fields
  end
end
{% endhighlight %}

Si revisamos el log de nuestra consola

``` 
Processing by UsersController#index as HTML
User Load (0.1ms) SELECT "users".* FROM "users"
Rendered users/index.html.erb within layouts/application (1.0ms)
["name", "email"]
``` 

Como podemos ver, devuelve [ "nombre", "email"] como atributos que se utilizaron en realidad. Si tabla de usuarios tiene 20 columnas entonces no tenga que cargar los valores de todas esas otras columnas. Estamos utilizando sólo dos columnas. Así que vamos a cambiar el código para reflejar eso.

{% highlight ruby %}
class UsersController < ApplicationController
  def index
    @users = User.select(:name, :email)
  end
end
{% endhighlight %}