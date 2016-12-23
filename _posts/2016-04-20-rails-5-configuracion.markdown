---
layout: post
title:  "Rails 5 - Instalación y Configuración"
date:   2016-04-20 00:37:19 -0300
categories: rails
---

A continuación aprenderemos a como instalar una aplicación en rails 5, para contribuir.

```
git clone git@github.com:rails/rails.git
```

Para poder crear nuestra aplicación, comprobamos nuestra versión de rails

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
gem install rails -v 5.0.0.beta4
```

En caso contrario si queremos instalar sin documentación

```
gem install rails -v 5.0.0.beta2 --no-ri --no-rdoc
```

Ahora procedemos a crear el proyecto:

```
rails new my-proyect
```

Si queremos especificar el motor
```
rails new my-proyect -D database
```

