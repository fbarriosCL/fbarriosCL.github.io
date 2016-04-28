---
layout: post
title:  "Rails 5 - Instalación y Configuración"
date:   2016-04-20 00:37:19 -0300
categories: jekyll update
---

A continuación aprenderemos a como instalar una aplicación en rails 5.

```
git clone git@github.com:rails/rails.git
```

Comprobamos nuestra versión de rails

```
rails -v
```
Nos debería retornar un error porque aún no hemos instalado ninguna versión

```
You can then rerun your "rails" command
```


A continuación vamos a instalar Rails 5

Si queremos instalar nuestra aplicación junto con su documentación

```
gem install rails -v 5.0.0.beta2
```

En caso contrario si queremos instalar sin documentación

```
gem install rails -v 5.0.0.beta2 --no-ri --no-rdoc
```