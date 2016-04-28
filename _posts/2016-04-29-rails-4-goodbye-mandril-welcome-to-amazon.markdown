---
layout: post
title:  "Rails 4 Migrando de Mandril con destino Amazon SES"
date:   2016-04-29 00:37:19 -0300
categories: jekyll update
---

Hoy llegando al departamento @abelorian, me comenta que llegado un mail a Backtrack Academy notificando que hoy se cancela nuestra cuenta en Mandrill, un servicio que teníamos implementado para la notificación de cursos y artículos mediantes correo electrónico  y como buen Chileno dejamos todo para el final. Lo positivo que escribimos juntos este post.

Durante un año, Mandrill fue el servicio de correo que ocupamos en #BTA, en donde se enviaba unos 100.000 correos mensuales, comunicando los cursos, post, correos transaccionales, algo muy importante para nuestra startup.

Así que nos toco migrar a un nuevo servicio, el servicio que escogimos fue Amazon SES, por qué? porque los costos son muy bajos. Incluso en comparación con Mandrill, Amazon nos sale más económico.
Bueno, ahora vamos a la parte entretenida. Lo práctico para ello vamos a Amazon Web Services (AWS)
En nuestro panel elegimos el siguiente servicio Amazon SES:



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

