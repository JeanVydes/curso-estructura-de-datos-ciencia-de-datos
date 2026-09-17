## Conceptos Claves

A la hora de adentrarnos en las estructuras de datos, nos encontramos con muchos terminos con los que coloquialmente no estamos acostumbrados, y antes de entrar de lleno al mundo de las estructuras de datos, debemos conocer algunos terminos clave que nos ayudaran en nuestro proceso.

### ¿FIFO y LIFO?

Algunas estructuras de datos tienen ciertos comportamientos que limitan a como accedemos a ciertos datos, FIFO (First In, First Out) hacen referencia a las estructuras que limitan a acceder a un dato dependiendo de cuando entro, y como su nombre lo indica, el primer elemento que ingreso DEBE ser el primer elemento en salir. Una alegoria seria hacer fila para una cafe, si yo llego primero, deberian atenderme primero. En ciencia de datos esto se utiliza cuando recibimos un flujo constante de datos en tiempo real (streaming) o en colas de tareas batch donde debemos procesar las cosas exactamente en el orden en que llegaron.

Por otra parte tenemos las estructuras LIFO (Last In, First Out). En este punto la mas conocida seria la pila, una estructura de datos donde el ultimo elemento en entrar debe ser el primero en salir; En la vida real estamos familiarizados con esto, un ejemplo basico seria cuando vamos a lavar platos, los platos primeros se acumulan, en una pila, y cuando ya queremos lavarlos, el primero que agarramos es el de mas arriba, a pesar de que fue el ultimo en ponerse. En ciencia de datos esto se usa cuando queremos mantener un historial de acciones que nos permita deshacer la ultima modificacion realizada sobre un dataset o analizar expresiones sintacticas.

### Invariantes

Como el nombre lo indica, es algo que no varia, en el contexto de las estructuras de datos esto se refiere a reglas strictly estrictas que cada familia o estructura deben seguir. No son negociables, y su violacion tiene como consecuencia que no sea lo estandar y provoque problemas inesperados.

Las colas son un ejemplo perfecto, una cola es una estructura de datos FIFO. Es decir, sus invariantes son que siempre se debe sacar el primer elemento que entro, violar en este caso esta restriccion, haria que la cola dejara de ser una cola.

### TDA

Un tipo abstracto de datos es un modelo matematico o conceptual que define un conjunto de valores y operaciones que se pueden aplicar sobre ellos. Independientemente de como se implemente, es basicamente un contrato que te dice como se deben hacer las cosas.

Cada estructura de datos es un TDA, ya que forman grupos de datos que tienen ciertas acciones, como insertar, eliminar, peek, etc. Cada TDA tiene una manera diferente de hacer cada una de ellas, y como operan.

### Acciones Primitivas y Acciones Derivadas

Las acciones primitivas de un TDA definen sus operaciones mas comunes y minimo requeridas para funcionar, entre estas podemos encontrar: insertar, eliminar, peek entre otros.

Pero tambien tenemos acciones derivadas, que surgen a raiz de metodos de ayuda BASADOS en acciones primitivas, pero que no son necesarios para que la estructura funcione. Como contar elementos o imprimirlos en pantalla.
