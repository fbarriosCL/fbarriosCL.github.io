---
layout: post
title: "Implementando Statham Serverless on Rails"
date:   2017-03-27 00:37:19 -0300
categories: statham-serverless
---

## Statham Serverless

Statham es una aplicación open source desarrollada con Node.js sobre AWS Lambda

Statham actúa como middleware entre HTTP POST requests/responses, con un algoritmo que puedes especificar la cantidad de intentos que enviar un mensaje en particular. Si el mensaje no puede ser entregado después de todos los intentos, Statham te notificará a tu correo electrónico.

Documentacion oficial:

```
https://github.com/archdaily/statham-serverless/wiki
```

Bueno lo primero que hacemos es clonar el repositorio y vamos a instalar los requerimientos básicos para levantar nuestra aplicación.

```
git clone https://github.com/archdaily/statham-serverless.git
```

Debemos instalar:

```
brew install node
```

```
brew install npm
```

```
npm install serverless -g
```

Déspues de instalar serverless debemos configurar nuestras credenciales de AWS

```
 serverless config credentials --provider aws --key AWS_KEY --secret AWS_SECRET
```

Para testing

```
npm install mocha
```

Instalamos dependencias de statham 

```
npm install
```

Para que statham pueda funcionar correctamente debemos configurar dos archivos sumamente importantes ```config.json```
y ```credential.json```

Dentro del archivo de configuración ```config.json``` necesitamos configurar los siguientes parámetros:

```
{
  "CycleExpression": "rate(5 minutes)",
  "TriesNum": 5,
  "ToEmailNotification": "felipe.barrios@gmail.com",
  "FromEmailNotification": "no-reply@gmail.com"
}
```

y para ```credential.json```

```
{
 "accessKeyId": "AWS_KEY",
 "secretAccessKey": "AWS_SECRET",
 "secretToken": "",
 "passwordJWK": ""
}
```

Cabe destacar que detro del repositorio ```serverless.yml``` podemos encontrar dos endpoint, uno para obtener el token y el otro para enviar el mensaje:

```
functions:
  receiver:
    timeout: 3
    handler: handlers/receiver.receiveAndSendMessage
    events:
      - http:
          path: receive
          method: post
  receiverEmail:
    timeout: 3
    handler: handlers/receiver.emailResend
    events:
      - http:
          path: receive
          method: get
```


Bueno en mi ejemplo está en ROR y es un evento que se dispara despues de guardar un objeto, envia ciertos parámetros a otra aplicación mediante API.

Tengo un callback que después de guardar un objeto en mi aplicación A, lo enviará datos a aplicación B, para ello cree el método ```send_to_statham``` que es el encargado de instanciar mi servicio y enviar el mensaje a la aplicación B mediante statham.

Entonces mi ```after_save``` para llamar al método

```ruby
after_save :send_to_statham
```

Mi método en donde se hace la instancia del servicio y envió el mensaje, en DATA, se especifica el mensaje, los datos que queremos enviar, después el método GET, POST, PUT, DELETE, y en URL_APLICATION B va la url destino, hacia donde queremos enviar nuestro mensaje.

```ruby
def send_to_statham
  statham = StathamService.new
  statham.send_to_statham('DATA', 'POST', URL_APLICATION_B)
end
```

Y en StathamService, tenemos ```token```para obtener permisos y send_to_statham que es el método encargado para enviar el mensaje.

```ruby
class StathamService
  
  def initialize
  end

  def token
    url_token                 = Settings.statham.token
    uri                       = URI.parse(url_token)
    request                   = Net::HTTP::Get.new(uri.path)
    request['Authorization']  = Settings.statham.password
    http                      = Net::HTTP.new(uri.host, uri.port)
    http.use_ssl              = true if Rails.env.production?
    response                  = http.request(request)
    token                     = JSON.parse(response.body) if response.body.present?
    token['token'] if token.present?
  end

  def send_to_statham body, method, url
    data                      = { body: body, method: method, url: url }
    uri                       = URI.parse(Settings.statham.receive)
    http                      = Net::HTTP.new(uri.host, uri.port)
    http.use_ssl              = true if Rails.env.production?
    request                   = Net::HTTP::Post.new(uri.request_uri, 'Content-Type' =>'application/json')
    request['Authorization']  = token
    request.body              = data.to_json
    response                  = http.request(request)
    return response.body
  end

end

```


