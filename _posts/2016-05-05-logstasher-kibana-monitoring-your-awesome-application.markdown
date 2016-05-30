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


En tu ```<environment>.rb```
```
# Enable logging of controller params
config.logstasher.log_controller_parameters = true
```

Empezamos
logstash:
Descargamos https://www.elastic.co/downloads/logstash

Crear un archivo dentro del directorio Logstash (todos los plugins) que se ha descargado en el paso anterior, quickstart.conf - establecer la ruta de registro de su aplicación a monitorizar.

```
input {
  file { 
    type => "rails logs"
    path => "/Users/shadab/test_project/logstasher.log"
    codec =>   json {
      charset => "UTF-8"
    }
  } 
}

output {
  # Print each event to stdout.
  stdout {
    codec => rubydebug
  }
  
  elasticsearch {
    # Setting 'embedded' will run  a real elasticsearch server inside logstash.
    # This option below saves you from having to run a separate process just
    # for ElasticSearch, so you can get started quicker!
    embedded => true
  }
}
```

logstasher.log será su nuevo archivo de registro creado por la gema logstasher.

start servicio:

```
bin/logstash agent -f quickstart.conf
```

Bueno en mi caso me lanzo un error

```
➜  logstash-2.3.2 bin/logstash agent -f quickstart.conf
Settings: Default pipeline workers: 4
Unknown setting 'embedded' for elasticsearch {:level=>:error}
Pipeline aborted due to error {:exception=>#<LogStash::ConfigurationError: Something is wrong with your configuration.>, :backtrace=>["/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/config/mixin.rb:134:in `config_init'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/outputs/base.rb:63:in `initialize'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/output_delegator.rb:74:in `register'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/pipeline.rb:181:in `start_workers'", "org/jruby/RubyArray.java:1613:in `each'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/pipeline.rb:181:in `start_workers'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/pipeline.rb:136:in `run'", "/Users/fbarrios/Documents/logstash-2.3.2/vendor/bundle/jruby/1.9/gems/logstash-core-2.3.2-java/lib/logstash/agent.rb:465:in `start_pipeline'"], :level=>:error}
stopping pipeline {:id=>"main"}
```

Solución

```
➜  logstash-2.3.2 sudo bin/plugin install logstash-output-elasticsearch-ec2
Password:
The use of bin/plugin is deprecated and will be removed in a feature release. Please use bin/logstash-plugin.
Validating logstash-output-elasticsearch-ec2
Installing logstash-output-elasticsearch-ec2
Installation successful
```

Listo para que kibana consuma

Start the configured logstash agent.
bin/logstash agent -f quickstart.conf

Start Kibana for Inferface and visit 0.0.0.0:5601

bin/kibana


Kivana
https://www.youtube.com/watch?v=SuDQ3-FihQk