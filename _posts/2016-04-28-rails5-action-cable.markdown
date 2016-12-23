---
layout: post
title:  "Rails 5 Action Cable"
date:   2016-04-26 00:37:19 -0300
categories: rails
---
Repositorio:
Una de las caracteristicas mas esperadas por los desarrolladores de rails es la integracion de action cable en rails 5.

Para entender action_cable, primero explicare algunas cosas: 

- Fundamentos de WebSockets.

- Action Cable API y un ejemplo de acción.

- Ventajas y desventajas Action Cable.


#### Fundamentos de WebSockets.

WebSockets son conexiones que están con estado. Esto significa que la conexión entre un cliente y un servidor se mantiene siempre conectado. En este escenario. cualquiera de las partes (el cliente o el servidor ) tiene la capacidad de iniciar una petición o un mensaje. El resultado final es la interacción directa entre el navegador y el servidor.

#### Action Cable API y un ejemplo de acción 

Lo primero que vamos hacer es instalar redis.

Linux:

```
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
make install
```

Mac:

```
brew install redis
```

Luego levantamos redis y nuestra aplicacion.

```
rails s
```

```
redis-server
```

Ahora lo divertido
La primera cosa que debe hacer es definir la clase ```ApplicationCable::Connection``` Este es el lugar donde se autoriza la conexión , y procederá a establecer conexión.
Aquí está el ejemplo más simple de comenzar con la clase de conexión del lado del servidor:

{% highlight ruby %}
# app/channels/application_cable/connection.rb
module ApplicationCable
  class Connection < ActionCable::Connection::Base
    identified_by :current_user

    def connect
      self.current_user = find_verified_user
    end

    protected
      def find_verified_user
        if current_user = User.find_by(id: cookies.signed[:user_id])
          current_user
        else
          reject_unauthorized_connection
        end
      end
  end
end
{% endhighlight %}

Aquí ```identified_by``` es el identificador de conexión, que se puede utilizar para encontrar la conexión específica de nuevo o posterior. Tenga en cuenta que cualquier cosa marcada como un identificador creará automáticamente un delegado por el mismo nombre en todas las instancias de los canales creados fuera de la conexión.

Esto se basa en el hecho de que ya se ha manejado la autenticación del usuario, y que una autenticación exitosa establece una cookie firmada con ```user_id```. Esta cookie es enviada automáticamente a la instancia de conexión cuando se intenta realizar una nueva conexión, y se lo utiliza para establecer el ```current_user```. La identificación de conexión es por el current_user, también se está asegurando de que posteriormente puede recuperar todas las conexiones abiertas por un usuario determinado (y potencialmente desconectar todos ellos si se elimina o desautorizado el usuario).

A continuación, debe definir la clase ActionCable::Channel en Ruby. Este es el lugar donde se coloca la lógica compartida entre los canales.

{% highlight ruby %}
# app/channels/application_cable/channel.rb
module ApplicationCable
  class Channel < ActionCable::Channel::Base
  end
end
{% endhighlight %}

El cliente necesita configurar una instancia para establecer conexión. Eso se hace de esta manera:

{% highlight ruby %}
# app/assets/javascripts/cable.coffee
#= require action_cable

@App = {}
App.cable = ActionCable.createConsumer("ws://cable.example.com")
{% endhighlight %}


El ```ws://cable.example.com``` debe apuntar a su servidor (s) ```action cable```, y tiene que compartir un espacio de nombres cookie con el resto de la aplicación (que puede vivir bajo http://example.com). Esto asegura que la cookie será enviada correctamente. Eso es todo lo que necesita para establecer la conexión! Pero, por supuesto, esto no es muy útil en sí mismo .Para hacer que las cosas ocurran, necesita contenido. Ese contenido está definido por la que se declara canales en el servidor y que permite al consumidor para suscribirse a ellos.

### ejemplo, el canal 1: las apariencias de los usuarios
He aquí un ejemplo simple de un canal que realiza un seguimiento de si un usuario está conectado o no, y también lo que la página que se encuentra actualmente. (Esto es útil para crear características de presencia como mostrar un punto verde junto al nombre de un usuario si están en línea). En primer lugar se declara el canal del lado del servidor:

{% highlight ruby %}
# app/channels/appearance_channel.rb
class AppearanceChannel < ApplicationCable::Channel
  def subscribed
    current_user.appear
  end

  def unsubscribed
    current_user.disappear
  end

  def appear(data)
    current_user.appear on: data['appearing_on']
  end

  def away
    current_user.away
  end
end
{% endhighlight %}

La devolución de llamada #subscribed se invoca cuando, como veremos a continuación mostramos, se inicia una suscripción de cliente. En este caso, se parte de esta oportunidad para decir "el usuario actual ha aparecido en verdad". API que aparecen / desaparecen podría ser respaldado por Redis o una base de datos o cualquier otra cosa. Esto es lo que el lado del cliente de que se parece a:

{% highlight ruby %}
# app/assets/javascripts/cable/subscriptions/appearance.coffee
App.cable.subscriptions.create "AppearanceChannel",
  # Called when the subscription is ready for use on the server
  connected: ->
    @install()
    @appear()

  # Called when the WebSocket connection is closed
  disconnected: ->
    @uninstall()

  # Called when the subscription is rejected by the server
  rejected: ->
    @uninstall()

  appear: ->
    # Calls `AppearanceChannel#appear(data)` on the server
    @perform("appear", appearing_on: $("main").data("appearing-on"))

  away: ->
    # Calls `AppearanceChannel#away` on the server
    @perform("away")


  buttonSelector = "[data-behavior~=appear_away]"

  install: ->
    $(document).on "page:change.appearance", =>
      @appear()

    $(document).on "click.appearance", buttonSelector, =>
      @away()
      false

    $(buttonSelector).show()

  uninstall: ->
    $(document).off(".appearance")
    $(buttonSelector).hide()
{% endhighlight %}

