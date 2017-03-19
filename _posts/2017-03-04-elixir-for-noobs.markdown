---
layout: post
title: "Conociendo elixir en profundidad"
date:   2017-02-03 00:37:19 -0300
categories: elixir
---

#### ¿Qué es Elixir?
Elixir creado por José Valim, comenta: "Me gustó todo lo que vi en Erlang, pero odiaba las cosas que no vi". Elixir tiene su propio sistema de gestión de paquetes -> hex.pm.

Elixir es un lenguaje que se ejecuta en la máquina virtual de Erlang. Por lo que tiene todas las ventajas, probado en batalla y puede usar las bibliotecas existentes Erlang sin penalización en el rendimiento. Elixir y añade un montón de sutilezas. Uno de ellos es ```pipe operator```.

La diferencia entre Erlang y elixir no es sólo la sintaxis. Sin embargo, la sintaxis es importante, y especialmente para desarrolladores de Ruby la sintaxis Elixir será muy familiar.


#### ¿ Qué es Erlang ?

Erlang es un lenguaje funcional y tiene una historia aún más larga con manejo de concurrencia. Erlang fue creado en Ericsson en la década de 1980 para permitir un mejor desarrollo de aplicaciones de telefonía.
Acualmente WhatsApp tiene cientos de millones de usuarios. con más de diez mil conexiones simultáneas en una sola máquina, lo cual se ve como un gran reto , pero Whatsapp tener servidores individuales con más de 2 millones de conexiones simultáneas. 2 millones de conexiones en un solo servidor, manejados por Erlang.

Erlang también es popular para los servidores del juego. Millones de usuarios juegan con la infraestructura Erlang tález como Call of Duty y Juego de la Guerra.

#### Inmutabilidad y la programación funcional
Elixir es un lenguaje funcional con la inmutabilidad.
La programación funcional es un paradigma de <b>programación declarativa</b> basado en el uso de <b>funciones matemáticas</b>, en contraste con la <b>programación imperativa</b>, que enfatiza los <b>cambios de estado</b> mediante la <b>mutación</b> de variables. <b>La programación funcional</b> tiene sus raíces en el cálculo <b>lambda</b>, un sistema formal desarrollado en los años 1930 para investigar la definición de función, la aplicación de las funciones y la recursión. Muchos lenguajes de programación funcionales pueden ser vistos como elaboraciones del cálculo lambda.

Me he dado cuenta de lo beneficioso que es. Incluso para la programación de un solo subproceso, mutabilidad trae una incertidumbre acerca de cómo va a ejecutar un programa. Y que no se pierda la mutabilidad en absoluto. La programación funcional y la inmutabilidad ayuda a aclarar las cosas y hace que sea más fácil de razonar sobre el código.

#### Concurrencia
Otro punto fuerte para Elixir es la simultaneidad. Concurrencia está aquí para quedarse, por varias razones. Para mencionar algunos: La tendencia en hardware es cada vez más núcleos de CPU. Los fabricantes de CPU no van a mejorar el rendimiento de un solo núcleo a la misma velocidad que antes. En su lugar, son la adición de más núcleos. También el mundo es concurrente y no desea que los usuarios finales u otros servicios de software que esperen innecesariamente una respuesta.

En la mayoría de los idiomas de concurrencia es un poco de dolor. No sólo es peligroso y difícil de hacer la sincronización. Con el estado mutable y bifurcación de procesos nativos también es a menudo lento y utiliza una gran cantidad de memoria.

Haciendo concurrencia en Erlang o Elixir otros idiomas es un poco como hacer ramas en Git contra la subversión . En la subversión era muy complicado de hacer - y nunca lo hice. En Git es mucho más fácil y lo hago todo el tiempo.

Bien mucho blabla y ahora un poco de acción ,para continuar vamos a necesitar instalar:

* Elixir: Phoenix está escrito en Elixir, y nuestro código de la aplicación también será escrito en Elixir. No vamos a entrar ahora en una aplicación de Phoenix sin él!
* Erlang: Elixir se compila a código de bytes de Erlang para ejecutar en la máquina virtual de Erlang. Sin 
Erlang, Elixir tiene ninguna máquina virtual para ejecutar en, por lo que necesitamos para instalar Erlang también.
* Phoenix
* Plug, Cowboy, and Ecto
* node.js (>= 5.0.0)
* PostgreSQL

#### Tips:

• La orientación de objetos no es la única manera de diseñar código.
• La programación funcional no necesita ser compleja o matemática.
• Los fundamentos de la programación no son asignaciones, si declaraciones, y
Bucles
• La concurrencia no necesita cerraduras, semáforos, monitores y similares.
• Los procesos no son necesariamente recursos costosos.
• La metaprogramación no es sólo algo pegado a un lenguaje.
• Incluso si es trabajo, la programación debe ser divertida.

Por supuesto, no estoy diciendo que Elixir es una poción mágica (bueno, técnicamente lo es, pero ya sabes a lo que me refiero). No es la única manera de escribir código. Pero es bastante diferente de la corriente principal que el aprendizaje le dará más perspectiva y abrirá su mente a nuevas formas de pensar sobre la programación.

Parte practica:
Bueno lo primero que vamos hacer es instalar elixir, en mi caso lo hare mediante brew

```
brew install elixir
```

y a continuacion vamos a necesitar erlang

```
brew install erlang
```

Acontinuacion ejecutaremos para comprobar que la instación fue un exito; ```iex``` Iex es una herramienta sorprendentemente potente. Utilícelo para compilar y ejecutar proyectos completos, iniciar sesión en máquinas remotas y acceder a las aplicaciones Elixir en ejecución.

Una ves vamos a definir un ejemplo sencillo

```
iex> 3 + 1
4
```

```
iex>  String.reverse "Backtrack Academy"
"ymedacA kcartkcaB"
```

Elixir tiene funciones auxiliares. por ejemplo comando para obtener ayuda ```h```

```
iex(20)> h
warning: variable "h" does not exist and is being expanded to "h()", please use parentheses to remove the ambiguity or change the variable name
  iex:20


                                  IEx.Helpers

Welcome to Interactive Elixir. You are currently seeing the documentation for
the module IEx.Helpers which provides many helpers to make Elixir's shell more
joyful to work with.

This message was triggered by invoking the helper h(), usually referred to as
h/0 (since it expects 0 arguments).

You can use the h/1 function to invoke the documentation for any Elixir module
or function:

    iex> h Enum
    iex> h Enum.map
    iex> h Enum.reverse/1

You can also use the i/1 function to introspect any value you have in the
shell:

    iex> i "hello"

There are many other helpers available:

  • b/1           - prints callbacks info and docs for a given module
  • c/1           - compiles a file into the current directory
  • c/2           - compiles a file to the given path
  • cd/1          - changes the current directory
  • clear/0       - clears the screen
  • flush/0       - flushes all messages sent to the shell
  • h/0           - prints this help message
  • h/1           - prints help for the given module, function or macro
  • i/1           - prints information about the data type of any given
    term
  • import_file/1 - evaluates the given file in the shell's context
  • l/1           - loads the given module's BEAM code
  • ls/0          - lists the contents of the current directory
  • ls/1          - lists the contents of the specified directory
  • nl/2          - deploys local BEAM code to a list of nodes
  • pid/1         - creates a PID from a string
  • pid/3         - creates a PID with the 3 integer arguments passed
  • pwd/0         - prints the current working directory
  • r/1           - recompiles the given module's source file
  • recompile/0   - recompiles the current project
  • respawn/0     - respawns the current shell
  • s/1           - prints spec information
  • t/1           - prints type information
  • v/0           - retrieves the last value from the history
  • v/1           - retrieves the nth value from the history

Help for all of those functions can be consulted directly from the command line
using the h/1 helper itself. Try:

    iex> h(v/0)

To learn more about IEx as a whole, type h(IEx).
```

Probablemente la más útil es h misma. Con un argumento, le ofrece ayuda sobre módulos Elixir o funciones individuales en un módulo.
Otros ejemplos

```
h String 
h Integer
h IO.puts
```

Otro auxiliar informativo es i, que muestra información sobre un valor:

```
iex(22)> i 'Backtrack Academy'
Term
  'Backtrack Academy'
Data type
  List
Description
  This is a list of integers that is printed as a sequence of characters
  delimited by single quotes because all the integers in it represent valid
  ASCII characters. Conventionally, such lists of integers are referred to as
  "charlists" (more precisely, a charlist is a list of Unicode codepoints,
  and ASCII is a subset of Unicode).
Raw representation
  [66, 97, 99, 107, 116, 114, 97, 99, 107, 32, 65, 99, 97, 100, 101, 109, 121]
Reference modules
  List
Implemented protocols
  IEx.Info, Collectable, Enumerable, Inspect, List.Chars, String.Chars
iex(23)>
```

Reprogramando tu cerebro :O

Asignación:
No pienso que significa lo que usted piensa que significa.
Vamos a usar el shell Elixir interactivo, iex, para ver un pedazo de código realmente simple. (Recuerde, inicia iex en el indicador de comandos utilizando el comando iex. Introduce el código de Elixir en su prompt iex> y muestra los valores resultantes.)

Asignación: 

```
iex> a = 1 
1
```

```
iex> a + 3 
4
```

Como pensaria un developer: "OK, asignamos 1 a una variable a, luego en la siguiente línea añadimos 3 a a, dándonos 4."

Pero cuando se trata de Elixir, estarían equivocados. En Elixir, el signo de <b>igualdad =</b>b no es una <b>asignación</b>. En su lugar, es como una <b>afirmación</b>. Sucede si Elixir puede encontrar una manera de hacer que el <b>lado izquierdo sea igual al lado derecho</b>. Elixir llama = un operador de <b>coincidencia</b>.

```
iex> a = 1 
1
```

```
iex> 1 = a 
1
```

```
iex> 2 = a
** (MatchError) no match of right hand side value: 1
```

Elixir sólo cambiará el valor de una variable en el lado izquierdo de un signo igual a la derecha una variable se sustituye por su valor. Esta línea fallida de código es la misma que 2 = 1, lo que causa el error.


