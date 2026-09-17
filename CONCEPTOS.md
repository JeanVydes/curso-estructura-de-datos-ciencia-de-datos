## Conceptos Clave

A la hora de adentrarte en las estructuras de datos como estudiante de ingeniería de ciencia de datos, te encontrarás con términos a los que de pronto no estás acostumbrado cotidianamente. Antes de entrar de lleno al mundo de las estructuras, debemos dominar algunos conceptos fundamentales que te guiarán a lo largo de todo tu aprendizaje.

### ¿FIFO y LIFO?

Algunas estructuras de datos tienen comportamientos específicos que definen y limitan cómo accedemos a la información. 

FIFO (First In, First Out) hace referencia a las estructuras donde el primer elemento en ingresar debe ser estrictamente el primer elemento en salir. Una analogía cotidiana es hacer fila en la cafetería de la universidad: si tú llegas primero, a ti deben atenderte primero. En la ingeniería de datos, esto se utiliza cuando recibes un flujo continuo de datos en tiempo real (streaming) o en colas de procesamiento por lotes donde debes respetar el orden de llegada.

Por otra parte, tenemos las estructuras LIFO (Last In, First Out). La más conocida es la pila, una estructura donde el último elemento en entrar es el primero en salir. Un ejemplo de la vida real es cuando vas a lavar platos: los platos sucios se acumulan en una pila, y cuando empiezas a lavar, el primero que tomas es el de arriba, a pesar de que fue el último que pusiste. En la ingeniería de datos, esto se usa para mantener un historial de acciones que permita deshacer la última modificación realizada sobre un conjunto de datos o para evaluar expresiones sintácticas.

### Invariantes

Como su nombre lo indica, un invariante es una regla que no varía. En el contexto de las estructuras de datos, se refiere a reglas estrictas que cada familia de estructuras debe cumplir en todo momento. No son negociables, y su violación provoca que la estructura deje de funcionar correctamente y genere errores inesperados.

Las colas son un ejemplo perfecto: una cola es una estructura FIFO. Su invariante es que siempre se debe retirar el elemento más antiguo. Violar esta restricción haría que la cola deje de ser una cola.

### TDA (Tipo Abstracto de Datos)

Un Tipo Abstracto de Datos es un modelo matemático y conceptual que define un conjunto de valores y las operaciones que se pueden aplicar sobre ellos. Independientemente de cómo se implemente en el código, es básicamente un contrato que te dice qué acciones se pueden realizar.

Cada estructura de datos es un TDA, ya que define un grupo de datos con acciones específicas como insertar, eliminar o consultar el elemento superior. Cada TDA tiene una forma distinta de operar.

### Acciones Primitivas y Acciones Derivadas

Las acciones primitivas de un TDA definen sus operaciones mínimas e indispensables para funcionar, entre las cuales encontramos insertar, eliminar y consultar (peek).

Las acciones derivadas son operaciones auxiliares construidas a partir de las acciones primitivas. No son estrictamente necesarias para que la estructura exista conceptualmente, pero ayudan en el trabajo diario, como contar el total de elementos o imprimirlos en pantalla.
