---
layout: post
title:  "Instalando Elasticsearch en rails Parte III - Config"
date:   2016-05-08 00:37:19 -0300
categories: elasticsearch
---

Agregamos las siguientes gemas a nuestro archivo ```Gemfile```

```
gem 'elasticsearch-model', git: 'git://github.com/elasticsearch/elasticsearch-rails.git'

gem 'elasticsearch-rails', git: 'git://github.com/elasticsearch/elasticsearch-rails.git'
```
run

```
bundle install
```

Instalamos elasticsearch mediante brew

```
brew install elasticsearch
```

Para comprobar http://localhost:9200/

Configuramos nuestro proyecto, creamos archivo:

```
/config/initializers/elasticsearch.rb
```

```ruby
ELASTICSEARCH_URL = ENV['ELASTICSEARCH_URL'] || 'http://localhost:9200'

Elasticsearch::Model.client = Elasticsearch::Client.new host: ELASTICSEARCH_URL
```

