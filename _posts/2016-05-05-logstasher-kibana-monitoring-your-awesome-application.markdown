---
layout: post
title:  "Logstasher y Kibana - Monitorinr your awesome application"
date:   2016-05-05 00:37:19 -0300
categories: jekyll update
---

Productos https://www.elastic.co/products

Paso 1: Instalar Elasticsearch e iniciar automáticamente durante el arranque.

Paso 2: Instalar Logstash y siga los pasos de instalación para todos los plugins.
https://www.elastic.co/products/logstash

Paso 3: Descarga de archivos Kibana siga los pasos de instalación.

Información completa sobre https://www.elastic.co/downloads

https://www.elastic.co/downloads


Instalación:
En tu archivo Gemfile:

```
gem 'logstasher'
```

Configure your ```<environment>.rb``` e.g. ```development.rb```

```
# Enable the logstasher logs for the current environment
config.logstasher.enabled = true

# Each of the following lines are optional. If you want to selectively disable log subscribers.
config.logstasher.controller_enabled = false
config.logstasher.mailer_enabled = false
config.logstasher.record_enabled = false
config.logstasher.view_enabled = false

# This line is optional if you do not want to suppress app logs in your <environment>.log
config.logstasher.suppress_app_log = false

# This line is optional, it allows you to set a custom value for the @source field of the log event
config.logstasher.source = 'your.arbitrary.source'

# This line is optional if you do not want to log the backtrace of exceptions
config.logstasher.backtrace = false

# This line is optional, defaults to log/logstasher_<environment>.log
config.logstasher.logger_path = 'log/logstasher.log'
```
Logging params hash

Logstasher se puede configurar para registrar los contenidos del hash params. Cuando está activado, el contenido del hash params (menos los parametros internos ActionController) se agregan al registro como un hash de profundidad. Esto puede causar conflictos dentro de las asignaciones Elasticsearch sin embargo, así debe ser habilitado con cuidado. Los conflictos se producirán si las diferentes acciones (o incluso diferentes aplicaciones de registro al mismo grupo Elasticsearch) utilizan la misma clave params, pero con un tipo de datos diferente (por ejemplo, una cadena contra un hash). Esto puede dar lugar a entradas de registro perdidas. Permitiendo esto también puede aumentar significativamente el tamaño de los índices Elasticsearch.

```
# Enable logging of controller params
config.logstasher.log_controller_parameters = true
``


Kivana
https://www.youtube.com/watch?v=SuDQ3-FihQk