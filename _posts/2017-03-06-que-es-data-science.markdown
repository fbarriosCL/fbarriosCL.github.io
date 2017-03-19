---
layout: post
title: "¿Qué es un Data Scientist? "
date:   2017-02-06 00:37:19 -0300
categories: elixir
---

¿Qué es un Data Scientist? 
Una persona que construye sistemas que intentan encontrar patrones en datos.
Debemos conocer de código, poder entender el problema que se necesitas trabajar y traducirlo en términos matemáticos.
Programación + Matemáticas + Negocios

Un workflow típico:
-- Formular la pregunta.
-- Recopilar datos.
-- Limpiar datos.
-- Dormir con tus datos.
-- Construir un modelo.
-- Validar su modelo.
-- Predecir el futuro.
-- Comunicar resultados.
-- Hacer que las máquinas hagan todo.


La red neuronal convolucional: Algoritmo que intenta imitar a lo que haga un ser humano mientras conduce.

http://quorumdigital.uy/los-superordenadores-de-la-cia-ya-predicen-agitaciones-sociales-dias-antes-que-sucedan/

Resultados de búsqueda en Google

Para conocer un poco más del PageRank, el modelo matemático bajo el motor de búsqueda de Google: https://es.wikipedia.org/wiki/PageRank

https://www.amazon.com/Machine-Learning-Probabilistic-Perspective-Computation/dp/0262018020

¿Cuáles habilidades y herramientas usamos?
Matemáticas: estadísticas, machine learning, optimización.
Ingeniería: Python, R, Scala.

Antes de escribir código deberíamos respondernos.

 * ¿Cuál es nuestro objetivo?
 * ¿Cuál es la pregunta exacta?
 * ¿Cómo definimos el éxito?

Dormir tus Datos:

Debemos familiarizarnos con los datos, conocerlos y visualizarlos de las formas que sea necesario.
Esta etapa puede incluir:
Computar estadisticos sumarios
Crear visualizaciones
Visualizar la relación de una variable contra otra

Modelos de datos
Típicamente construiremos modelos matemáticos para
Investigación: nos enterarnos de la relación entre un input y un output de un sistema.
Predicción: predecir un comportamiento de output de acuerdo a inputs.

Validar su modelo y predecir el futuro

Una vez tenemos un modelo construido debemos asegurarnos que este funciona, un método para validar un modelo es la regresión lineal con el cual computando un coheficiente de determinación, este nos dice el porcentaje de la variación de una variable del modelo.
En un modelo que nos muestre la cantidad de medicamento que ha tomado un paciente, para validarlo con regresión lineal el coeficiente tendríamos que multiplicarlo por la variable que es el peso del paciente.
Validando este modelo tendremos la confianza si pasamos otro peso al modelo nos pueda predecir cuanto medicamento debe suministrarse a un paciente.
Predicción: predecir la dosis de medicamento que debería suministrarse
Vamos a tener el modelo entrenado y con nuevos datos debemos predecir un output con nuevos inputs, para evaluarlo nos toca juzgar su precisión con datos que no fueron usado usados en el entrenamiento del modelo.

Comunicar los resultados
Comunicar resultados no solo con otros data scientist, también con otros interesados en estos resultados
