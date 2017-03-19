sudo apt-get update


wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/deb/elasticsearch/2.3.1/elasticsearch-2.3.1.deb


sudo dpkg -i elasticsearch-2.3.1.deb


cd /usr/share/elasticsearch

```
rails@bta-search:/usr/share/elasticsearch$ sudo bin/plugin install license
-> Installing license...
Trying https://download.elastic.co/elasticsearch/release/org/elasticsearch/plugin/license/2.3.1/license-2.3.1.zip ...
Downloading .......DONE
Verifying https://download.elastic.co/elasticsearch/release/org/elasticsearch/plugin/license/2.3.1/license-2.3.1.zip checksums if available ...
Downloading .DONE
Installed license into /usr/share/elasticsearch/plugins/license
```

```
rails@bta-search:/usr/share/elasticsearch$ sudo bin/plugin install shield
-> Installing shield...
Trying https://download.elastic.co/elasticsearch/release/org/elasticsearch/plugin/shield/2.3.1/shield-2.3.1.zip ...
Downloading .............................................................................................................................................DONE
Verifying https://download.elastic.co/elasticsearch/release/org/elasticsearch/plugin/shield/2.3.1/shield-2.3.1.zip checksums if available ...
Downloading .DONE
Installed shield into /usr/share/elasticsearch/plugins/shield
```

para crear un usuario en es

bin/shield/esusers useradd es_admin -r nameuser