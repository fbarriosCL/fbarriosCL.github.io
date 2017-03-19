---
layout: post
title:  "Rails 4 Goodbye Mandrill Welcome To Amazon SES"
date:   2016-04-25 00:37:19 -0300
categories: aws
---

Hoy llegando al departamento @abelorian, me comenta que llegado un mail a Backtrack Academy notificando que hoy se cancela nuestra cuenta en Mandrill, un servicio que teníamos implementado para la notificación de cursos y artículos mediantes correo electrónico  y como buen Chileno dejamos todo para el final. Lo positivo que escribimos juntos este post.

Durante un año, Mandrill fue el servicio de correo que ocupamos en #BTA, en donde se enviaba unos 100.000 correos mensuales, comunicando los nuevos cursos, post, correos transaccionales, algo muy importante para mantenernos en comunicación con nuestros usuarios.

Así que nos toco migrar a un nuevo servicio, el servicio que escogimos fue Amazon SES, por qué? porque los costos son muy bajos y escalabilidad. Incluso en comparación con Mandrill, Amazon nos sale mucho más económico.
Bueno, ahora vamos a la parte entretenida. Lo práctico para ello vamos a  <a src="https://aws.amazon.com/es/">Amazon Web Services (AWS)</a>

En nuestro panel elegimos el siguiente servicio Amazon SES :

![alt text](/assets/posts/01/1.png)

Luego hacemos clic en ```domains``` para validar nuestro dominio.

![alt text](/assets/posts/01/2.png)

Asi que le damos clic al boton blue 

![alt text](/assets/posts/01/3.png)

Ingresamos nuestro dominio

![alt text](/assets/posts/01/4.png)

Esta verificación se hace mediante el DNS, en donde nos pide ingresar un registro.

![alt text](/assets/posts/01/5.png)

Acá es donde vamos a tener que agregar nueva zona


Copiamos  name, text, value y lo agregamos en nuestro panel de DNS. Acá va a depender de su proveedor la configuración.

Una vez ingresado vamos nuevamente al panel en Amazon SES hasta esperar que el estado sea *verified*, esto puede tardar un maximo de 24 horas.
Luego, debemos repetir la misma verificación para el DKIM en donde vamos agregar 3 registros de tipo CNAME.

Una vez comprobado nos vamos a ```emails_address``` en donde tenemos que ingresar un correo valido de origen,para eso agregamos un mail, medidas antispam. En mi ejemplo 3 pero basta con solo 1 email.


![alt text](/assets/posts/01/6.png)

Le damos clic nuevamente al botón azul ```Verify new email address```

![alt text](/assets/posts/01/7.png)

Te va a llegar un link con un codigó de verificación, le damos clic y queda listo el correo como verificado y habilitado para enviar mails desde esa direccion.

Ahora vamos a configurar el STMP el envío de correos salientes hijo, porq no hay correos entrantes.


![alt text](/assets/posts/01/9.png)

Nos dará nuestra credencial

![alt text](/assets/posts/01/10.png)

A continuación nos entregará STMP username y STMP password para asi poder realizar la configuración en rails que es de donde enviaremos los mails.

![alt text](/assets/posts/01/11.png)

Ahora en nuestro proyecto Rails vamos a ```config/environments/production.rb```

{% highlight ruby %}
  config.action_mailer.perform_deliveries = true
  config.action_mailer.raise_delivery_errors = true
  config.action_mailer.default_url_options = { :host => 'https://www.url.com' }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    address:              'email-smtp.us-west-2.amazonaws.com',
    port:                 587,
    user_name:            'SMTP username',
    password:             'SMTP password',
    authentication:       'login',
    enable_starttls_auto: true  
    }
{% endhighlight %}

