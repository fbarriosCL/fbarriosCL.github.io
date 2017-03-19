---
layout: post
title: "Data Science - Parte II"
date:   2017-03-06 00:37:19 -0300
categories: data
---

Encontrar conectores de claves

Tenemos una lista de usuarios, cada uno representado por un ```id``` y un nombre:

```
users = [
 { "id": 0, "name": "Hero" },
 { "id": 1, "name": "Dunn" },
 { "id": 2, "name": "Sue" },
 { "id": 3, "name": "Chi" },
 { "id": 4, "name": "Thor" },
 { "id": 5, "name": "Clive" },
 { "id": 6, "name": "Hicks" },
 { "id": 7, "name": "Devin" },
 { "id": 8, "name": "Kate" },
 { "id": 9, "name": "Klein" }
]
```

También le da los datos de "amistad", representados como una lista de pares de IDs:

```
friendships = [(0, 1), (0, 2), (1, 2), (1, 3), (2, 3), (3, 4),(4, 5), (5, 6), (5, 7), (6, 8), (7, 8), (8, 9)]
```

Por ejemplo, agregar una lista de amigos para cada usuario. 
Primero fijamos cada propiedad de amigos del usuario a una lista vacía:

```python
for user in users:
 user["friends"] = []
```

Y luego poblamos las listas usando los datos de las amistades:

```python
for i, j in friendships:
 # this works because users[i] is the user whose id is i
 users[i]["friends"].append(users[j]) # add i as a friend of j
 users[j]["friends"].append(users[i]) # add j as a friend of i
```

Una vez que cada dict de usuario contiene una lista de amigos, podemos hacer fácilmente preguntas de nuestro
Gráfico, como "¿cuál es el número promedio de conexiones?"
Primero encontramos el número total de conexiones, sumando las longitudes de todas las
Listas de amigos:

```
def number_of_friends(user):
 """how many friends does _user_ have?"""
 return len(user["friends"]) # length of friend_ids list
total_connections = sum(number_of_friends(user)
 for user in users) # 24
```

Y luego nos dividimos por el número de usuarios:

```
from __future__ import division # integer division is lame
num_users = len(users) # length of the users list
avg_connections = total_connections / num_users # 2.4
```

También es fácil encontrar a la gente más conectada: son las personas que tienen
Es el número de amigos.
Puesto que no hay muchos usuarios, podemos clasificarlos de "la mayoría de los amigos" a "menos
amigos":

```
# create a list (user_id, number_of_friends)
num_friends_by_id = [(user["id"], number_of_friends(user))
 for user in users]
sorted(num_friends_by_id, # get it sorted
 key=lambda (user_id, num_friends): num_friends, # by num_friends
 reverse=True) # largest to smallest
# each pair is (user_id, num_friends)
# [(1, 3), (2, 3), (3, 3), (5, 3), (8, 3),
# (0, 2), (4, 2), (6, 2), (7, 2), (9, 1)]
```


## Ejemplo 2

Que un usuario pueda conocer a los amigos de amigos. Estas
Son fáciles de calcular: para cada uno de los amigos de un usuario, iterar sobre los amigos de esa persona, y
Recoger todos los resultados:

```
def friends_of_friend_ids_bad(user):
 # "foaf" is short for "friend of a friend"
 return [foaf["id"]
 for friend in user["friends"] # for each of user's friends
 for foaf in friend["friends"]] # get each of _their_ friends
```

Cuando lo llamamos a los usuarios [0] (Héroe), retorna:

```
[0, 2, 3, 0, 1, 3]
```

It includes user 0 (twice), since Hero is indeed friends with both of his friends. It
includes users 1 and 2, although they are both friends with Hero already. And it
includes user 3 twice, as Chi is reachable through two different friends:

```
print [friend["id"] for friend in users[0]["friends"]] # [1, 2]
print [friend["id"] for friend in users[1]["friends"]] # [0, 2, 3]
print [friend["id"] for friend in users[2]["friends"]] # [0, 1, 3]
```

Saber que las personas son amigos de amigos de múltiples maneras parece interesante
Información, así que quizás en su lugar deberíamos producir un recuento de amigos mutuos. Y nosotros
Definitivamente debe utilizar una función de ayuda para excluir a las personas ya conocidas por el usuario:

```
from collections import Counter # not loaded by default
def not_the_same(user, other_user):
 """two users are not the same if they have different ids"""
 return user["id"] != other_user["id"]
def not_friends(user, other_user):
 """other_user is not a friend if he's not in user["friends"];
 that is, if he's not_the_same as all the people in user["friends"]"""
 return all(not_the_same(friend, other_user)
 for friend in user["friends"])
def friends_of_friend_ids(user):
 return Counter(foaf["id"]
 for friend in user["friends"] # for each of my friends
 for foaf in friend["friends"] # count *their* friends
 if not_the_same(user, foaf) # who aren't me
 and not_friends(user, foaf)) # and aren't my friends
print friends_of_friend_ids(users[3]) # Counter({0: 2, 5: 1})
```

Esto indica correctamente a Chi (id 3) que ella tiene dos amigos mutuos con Hero (id 0) pero
Sólo un amigo común con Clive (id 5).
Como científico de datos, sabe que también podría disfrutar de reuniones con usuarios similares
intereses. (Este es un buen ejemplo del aspecto de la "experiencia sustantiva" de la ciencia
Después de preguntar por ahí, se las arregla para poner sus manos en estos datos, como una lista de
pairs(user_id, interés):

```
interests = [
 (0, "Hadoop"), (0, "Big Data"), (0, "HBase"), (0, "Java"),
 (0, "Spark"), (0, "Storm"), (0, "Cassandra"),
 (1, "NoSQL"), (1, "MongoDB"), (1, "Cassandra"), (1, "HBase"),
 (1, "Postgres"), (2, "Python"), (2, "scikit-learn"), (2, "scipy"),
 (2, "numpy"), (2, "statsmodels"), (2, "pandas"), (3, "R"), (3, "Python"),
 (3, "statistics"), (3, "regression"), (3, "probability"),
 (4, "machine learning"), (4, "regression"), (4, "decision trees"),
 (4, "libsvm"), (5, "Python"), (5, "R"), (5, "Java"), (5, "C++"),
 (5, "Haskell"), (5, "programming languages"), (6, "statistics"),
 (6, "probability"), (6, "mathematics"), (6, "theory"),
 (7, "machine learning"), (7, "scikit-learn"), (7, "Mahout"),
 (7, "neural networks"), (8, "neural networks"), (8, "deep learning"),
 (8, "Big Data"), (8, "artificial intelligence"), (9, "Hadoop"),
 (9, "Java"), (9, "MapReduce"), (9, "Big Data")
]
```

Por ejemplo, Thor (id 4) no tiene amigos en común con Devin (id 7), pero comparten
Un interés en el aprendizaje automático.
Es fácil crear una función que encuentre usuarios con cierto interés:

```
def data_scientists_who_like(target_interest):
 return [user_id
 for user_id, user_interest in interests
 if user_interest == target_interest]
```

```
from collections import defaultdict
# keys are interests, values are lists of user_ids with that interest
user_ids_by_interest = defaultdict(list)
for user_id, interest in interests:
 user_ids_by_interest[interest].append(user_id)
And another from users to interests:
# keys are user_ids, values are lists of interests for that user_id
interests_by_user_id = defaultdict(list)
for user_id, interest in interests:
 interests_by_user_id[user_id].append(interest)

```

Ahora es fácil encontrar quién tiene más intereses en común con un usuario dado:
• Iterar sobre los intereses del usuario.
• Para cada interés, iterar sobre los otros usuarios con ese interés.
• Mantenga la cuenta de cuántas veces nos vemos entre nosotros.

```
def most_common_interests_with(user):
 return Counter(interested_user_id
 for interest in interests_by_user_id[user["id"]]
 for interested_user_id in user_ids_by_interest[interest]
 if interested_user_id != user["id"])
```













