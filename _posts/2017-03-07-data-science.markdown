---
layout: post
title: "Data Science"
date:   2017-03-06 00:37:19 -0300
categories: data
---

Llevar tus cosas a producción
Llevar tus cosas a producción significa tomar tu solución y construirle un sistema de ingeniería de modo que podamos olvidarlo e irnos a dormir o trabajar en un problema mejor.
Pongamos de ejemplo a Spotify
Imagina registrar semanalmente tu playlist de “Descubrimiento Semanal” en Spotify y clasificar esas predicciones en tu dispositivo. Mediante un cuaderno interactivo construiríamos un modelo matemático que consumiría la actividad que el usuario tiene en la plataforma, es decir, las canciones que ha escuchado, y, a partir de esa actividad, genera recomendaciones de otras canciones que también podría disfrutar. Así que este es en gran medida un problema matemático, de Machine Learning.
Todo es un largo proceso el que atravesamos para hacer un prototipo en un cuaderno interactivo y en teoría podemos hacerlo con lápiz y papel, pero de manera más práctica, lo hacemos en un cuaderno interactivo de Jupyter, un entorno interactivo de desarrollo.
La brecha entre tener resultados que nos gustan y realmente entregar esas playlists en una forma agradable y consumible a todos los dispositivos de los usuarios de Spotify es bastante grande.
Cuanto mejor entiendas un modelo matemático que estás llevando a producción, mejor será el sistema de producción que puedas crear. Si entiendes cómo funciona el algoritmo que, por ejemplo, está registrando las recomendaciones de Spotify, si entiendes sus posibilidades, su capacidad para ser paralelizado en diferentes servidores, etc. ese sistema de ingeniería que puedas construir a su alrededor será mejor.
No tienes que hacerlo todo como data scientist, pero si lo suficiente para guiar a tu equipo de ingeniería a llevar a cabo esto. Y esto es lo que significa “llevar tus cosas a producción”, construir un sistema de ingeniería
a partir de tu solución de data science.

Bases de datos: SQL o NoSQL
Cuando hablamos de bases de datos hablamos de bases de datos, o más coloquialmente, almacenes de datos SQL, o almacenes de datos NoSQL.
Un almacén de datos SQL se ocupa de datos más estructurados,
así que cuando hablamos de escribir datos en una base de datos hablamos de registros. En realidad un registro es cada cosa que ha pasado.
Por el contrario, una base de datos NoSQL no tiene la misma estructura implícita. De modo que cada registro podría contener o no cosas diferentes.
Al hablar de bases de datos SQL hablamos acerca de SQLite, MySQL, PostgreSQL y Redshift.
Al hablar de almacenes de datos NoSQL vemos MongoDB, que es un almacén de datos para documentos, Redis, un almacén clave-valor, y tanto Giraph como Neo4j son bases de datos gráficas.
Así que, al escoger una base de datos tenemos que hacernos algunas preguntas.
¿Qué tipo de consultasNharemos sobre esta base de datos?
¿Le estaremos pidiendo a nuestra base de datos pedazos grandes y planos de datos?
. ¿Estaremos haciendo un montón de agregaciones como suma, media o máximo al momento de la consulta?
¿O estaremos escribiendo sobre esta base de datosNmás que leyendo de ella?
Así que, al escoger una base de datos, cada situación es diferente. Tu problema tiene diferentes necesidades que el problema de alguien más.
Así que en realidad, para bases de datos,no existe una “talla única”.

ETL
ETL es una sigla en inglés para “Extraer, Transformar y Cargar”.
Por ejemplo, un proceso ETL podría consumir datos que se generaron en las cabinas de votación en la ciudad de Nueva York, así que quizás cada registro en estos datos en bruto contiene 50 campos diferentes acerca de ese voto. Pero realmente sólo nos interesan cuatro campos.
Así que quizás nos enganchamos a una API de esta cabina de votación, extraemos los datos en bruto, y, por supuesto, esa es la fase E, “Extraer”, de ETL.
La siguiente es transformar, así que de estos 50 campos podríamos decir: “OK, sólo queremos, sólo nos interesan cuatro de ellos, el nombre del votante, su edad, su partido político, y, desde luego, por quién votó.”
A continuación podemos enviar esto en un formato con el cual vamos a arrojar esos datos a nuestra base de datos. Así que esta es la fase T, “Transformar”. Y finalmente “Cargar”, lo cual implica tomar estos datos y persistirlos en nuestra base de datos.
Así que, al hablar de ETL y qué tanto hacemos en la labor de llevar nuestras cosas a producción.
Cuando creamos modelos matemáticos queremos datos limpios, datos actuales,
y la manera de obtenerlos es construyendo procesos ETL que efectivamente nos los traigan.
Nuevamente, como organización estamos tratando con una gran cantidad de datos que vienen de afuera y tomamos esos datos, los transformamos y luego los almacenamos para usarlos como data scientists.
Así que hasta ahora hemos hablado de los procesos ETL como nuestro brazo hacia el mundo exterior
Como data scientists nos encontramos escribiendo un buen número de procesos ETL, esta no es la parte sexy del trabajo, esta es la parte del trabajo que a las personas, digamos, les emociona menos hacer.
Sin embargo, tener datos limpios para tus modelos de machine learning, quiero decir, es más que indispensable. Con malos datos, ya sabes, hay un dicho en machine learning que dice:
“Si entra basura, sale basura”.
Si le alimentamos datos basura a nuestros modelos de machine learning no vamos a ser muy eficaces en nuestro trabajo.
Así que, de nuevo, la comunidad detrás de cada marco es un factor muy importante al hacer una decisión realmente inteligente para nuestra organización.

Populares ETL

AWS Data pipeline 
Luigi -> Spotify
AirFlow -> airbnb 
Pinball -> Pinterest

Mostrar tus datos mediante dashboards
Creo que todos sabemos qué es un panel de control, Cuando hablamos de llevar nuestras cosas a producción, la brecha entre tener una solución que nos gusta, en un cuaderno interactivo de desarrollo, y crear ese panel de control, el paso en el medio es en realidad llevar tus cosas a producción.
La creación de paneles de control es sólo poner una especie de agradable capa visual encima de eso, una capa que hace muy fácil consumir el trabajo que acabamos de llevar a producción.
Hay varias grandes razones para usar paneles de control.
La primera es la inteligencia empresarial interna. Estos pueden mostrar cosas como ttiempo de actividad del servidor, pueden mostrar cosas como disponibilidad en atención al cliente.
Otra razón es reducir la carga cognitiva las fotos son bonitas y simplifican las cosas. Simplemente, como data scientists, eliminar la necesidad de escribir una consulta SQL
cada vez que queremos una cierta información es realmente un gran triunfo.
Muchas organizaciones de data science, en especial las más jóvenes, están en la fase en la que otros departamentos que tienen dudas acerca de los datos de su compañíales envían correos diciendo “oye, ¿puedes escribirme una consulta SQL?, porque no sé cómo hacerlo yo.”, eso no es nada escalable.
De hecho un panel de control es algo bastante simple. Y muchas personas han creado excelentes infraestructuras para paneles de control algunas son un poco diferentes, algunas son más para producción, algunas son más para creación de prototipos, algo que citas día de por medio
como data scientist. De nuevo, reducir la carga cognitiva, tener la capacidad de ver las cosas gráficamente y, en cierto modo, atar un lindo lazo a tu trabajo realmente hace más fácil enfocarse en otras cosas,Nen problemas que aún no has resuelto.
Así que construir tu propio panel de control, al menos desde ceros, muy probablemente es una pésima idea.

Looker https://looker.com/

Repaso por Jupyter Notebook
Cuando prototipos soluciones de data science a menudo nos encontramos utilizando cuadernos interactivos. Estos cuadernos interactivos nos permiten ejecutar código, sin ningún orden en particular. Lo que es muy bueno porque el proceso de construir soluciones de data science es a menudo muy iterativo.
En el caso puntual de python usamos un notebook llamado Jupyter. Desde la terminal ejecutamos:
pip install Jupyter
Una vez instalado, podemos ejecutar jupyter con:
jupyter notebook
Es realmente importante entender que Jupyer, de hecho el proyecto Jupyter, esta libreta es agnóstica al lenguaje. Es decir, podemos usarla con una gran variedad de lenguajes.
Entonces desde un cuaderno interactivo, podemos cargar datos y podemos inspeccionar esos datos. Podemos crear visualizaciones en línea, por supuesto podemos estas celdas fuera de orden también.
Como data scientist esto podría sonar muy natural de hecho, tener este ambiente de trabajo interactivo, flexible, iterativo.
Para muchos ingenieros de software de hecho esta herramienta les parece muy distante, para muchos ingenieros de software donde escribimos scripts, estamos escribiendo código que ejecutamos continuamente. es decir, escribimos algo de código, y ejecutamos el script entero,
entonces no tenemos la opción o incluso la noción de ser capaces de ejecutar tan fácil ese script, de alguna forma sin orden, ejecutando la primer linea y luego la cuarta linea y la tercera,
porque lo escribimos como tal para que funcione con cierta cronología.
Si no usas Jupyter existen otras opciones, Rodeo es una, Zeppelin es otra. Lo importante es que explores otras opciones y nos cuentes como te ha ido.

pip install pandas

pip install matplotlib

import numpy as np

df = pd.DataFrame({
  'a': np.random.randn(100),
  'b': np.random.randn(100)
});

df.head(5)

df.dtypes

df.shapes

df['a'].hist()

