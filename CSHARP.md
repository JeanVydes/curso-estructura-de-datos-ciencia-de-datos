# C#: Una introduccion necesaria

Este curso se trabaja a base de C#, entonces como ingenieros deberiamos conocer como manejar primero el lenguaje y la sintaxis, antes de aplicar las estructuras de datos.

C# parte de conceptos similares a JAVA. Entonces si usted ya esta familiarizado con JAVA, le resultara bastante intuitivo esta seccion.

C# es un lenguaje de programacion basado en clases. Es decir hereda todo lo que ya conocemos como polimorfismo, abstracciones, herencia, encapsulamiento, constructores, etc.

Para la ciencia de datos, C# incluye una libreria estandar de colecciones en System.Collections.Generic que nos ahorra tener que implementar las estructuras a mano. Nuestro trabajo consiste en saber cual coleccion elegir y como usarla en nuestros pipelines de datos.

```csharp
// using lo utilizamos para importar modulos externos, es decir, aumentar las capacidades de nuestro programa a traves de librerias.
using System;
using System.Collections.Generic;

public class Clinica
{
    static void Main()
    {
        // Inicializamos una cola de prioridad que se va a componer de dos elementos, un string que hace referencia al paciente, y int que hace referencia a su prioridad en el triaje.
        PriorityQueue<string, int> urgencias = new PriorityQueue<string, int>();

        // Ahora procedemos a ingresar pacientes a triaje.
        urgencias.Enqueue("Juan Perez: Gripa", 10);
        urgencias.Enqueue("Juana Perez: Brazo Roto", 2);
        urgencias.Enqueue("Robert Perez: Infarto", 1);

        // Ahora desencolamos para atenderlos segun su prioridad. Hasta que no haya mas pacientes que atender.
        while (urgencias.Count > 0)
        {
            string paciente = urgencias.Dequeue();
            Console.WriteLine("Atendiendo a: " + paciente);
        }
    }
}
```