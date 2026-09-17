# Colas (Queue<T>)

La cola es una estructura de datos lineal que responde a la regla FIFO (First In, First Out). Esto garantiza que el primer elemento en ingresar sea estrictamente el primer elemento en ser procesado y retirado.

Como mencionamos en los conceptos generales, la analogia intuitiva es la fila para comprar un tique o un cafe: a la persona que llega primero se le atiende primero.

## Como funciona internamente una Cola

La cola restringe sus operaciones a dos extremos bien definidos: los elementos ingresan por la parte trasera (Rear) y se retiran por la parte delantera (Front).

### Operacion Enqueue (Encolar)

```
Estado inicial de la cola con 2 elementos:

Front                             Rear
[ Elemento 1 ] <--- [ Elemento 2 ]

Ejecutamos Enqueue(Elemento 3):

Front                             Rear
[ Elemento 1 ] <--- [ Elemento 2 ] <--- [ Elemento 3 ]

El nuevo elemento se agrega al final de la cola en tiempo O(1).
```

### Operacion Dequeue (Desencolar)

```
Estado inicial con 3 elementos:

Front                             Rear
[ Elemento 1 ] <--- [ Elemento 2 ] <--- [ Elemento 3 ]

Ejecutamos Dequeue():

Retorna Elemento 1 y la cola queda:

Front              Rear
[ Elemento 2 ] <--- [ Elemento 3 ]

Retirar el elemento del frente se realiza en tiempo O(1).
```

## Cuando usar una Cola en Ciencia de Datos

Elegimos una cola cuando necesitamos procesar la informacion respetando estrictamente el orden secuencial de llegada:

1. Buffers de streaming en tiempo real: Cuando recibimos eventos continuos de sensores IoT o APIs y el sistema necesita acumularlos temporalmente para ir procesandolos a la velocidad disponible.

2. Planificacion de tareas en lote (Batch jobs): Para coordinar la ejecucion secuencial de scripts ETL (Extraccion, Transformacion y Carga).

3. Recorridos por niveles en grafos o arboles (BFS): Para procesar redes y relaciones nivel por nivel.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploCola
{
    static void Main()
    {
        Queue<string> bufferEventos = new Queue<string>();

        // Llegan eventos de streaming
        bufferEventos.Enqueue("Evento 1: Lectura Temp 24.5 C");
        bufferEventos.Enqueue("Evento 2: Lectura Temp 25.1 C");
        bufferEventos.Enqueue("Evento 3: Lectura Temp 24.8 C");

        Console.WriteLine("Eventos acumulados en el buffer: " + bufferEventos.Count);
        Console.WriteLine("Siguiente evento a procesar: " + bufferEventos.Peek());

        // Procesamos los eventos en orden de llegada (FIFO)
        Console.WriteLine("\nProcesando eventos:");
        while (bufferEventos.Count > 0)
        {
            string eventoActual = bufferEventos.Dequeue();
            Console.WriteLine("Procesado -> " + eventoActual);
        }
    }
}
```
