# Ejercicios de Colas (Queue<T>)

En este tercer bloque nos enfocaremos en la estructura lineal FIFO (First In, First Out), donde el orden temporal de llegada debe ser estrictamente respetado para garantizar la equidad o la secuencia correcta de procesamiento.

## Caso 3: Buffer de Transmisiones Meteorologicas del IDEAM

Estaciones meteorologicas automaticas instaladas en la Sierra Nevada de Santa Marta envian paquetes de datos de precipitacion y humedad cada pocos segundos a un servidor central.

Debido a picos en la red, los paquetes llegan a mayor velocidad de la que el servidor puede procesar e insertar en la base de datos PostgreSQL.

```
Buffer de entrada FIFO:

Llegada de paquetes:
[ Paquete 101 (10:00:01) ] ---> [ Paquete 102 (10:00:02) ] ---> [ Paquete 103 (10:00:03) ]

El servidor debe retirar e insertar en la base de datos en el mismo orden exacto:
1. Procesa Paquete 101
2. Procesa Paquete 102
3. Procesa Paquete 103
```

## Preguntas de Racionalizacion

1. ¿Por que violaria los requerimientos del sistema si utilizáramos una Pila (Stack<T>) para este servidor de datos meteorologicos? Describe que le sucederia al Paquete 101 si siguen llegando paquetes continuamente.

2. Explica las tres operaciones primitivas de la clase Queue<T> en C# (Enqueue, Dequeue, Peek) y su complejidad algoritmica en notacion O grande.

3. Dibuja un diagrama ASCII mostrando el estado del frente (Front) y final (Rear) de una cola cuando realizas dos operaciones Enqueue y una operacion Dequeue.

```
Diagrama ASCII a completar:

Estado Inicial:
Front                          Rear
[ Dato A ] <--- [ Dato B ]

Paso 1: Enqueue(Dato C)            Paso 2: Dequeue()
Front                   Rear       Front        Rear
[   ?   ]               [ ? ]      [   ?   ]    [ ? ]
```

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionColas
{
    static void Main()
    {
        // JUSTIFICACION: Se elige Queue<string> porque la ingestion debe procesarse en estricto orden FIFO...
        Queue<string> bufferEstaciones = new Queue<string>();

        // Simular llegada de paquetes
        bufferEstaciones.Enqueue("Estacion San Lorenzo: 12mm lluvia");
        bufferEstaciones.Enqueue("Estacion El Campano: 8mm lluvia");
        bufferEstaciones.Enqueue("Estacion Minca: 15mm lluvia");

        Console.WriteLine("Paquetes pendientes en buffer: " + bufferEstaciones.Count);
        Console.WriteLine("Proximo a procesar (Peek): " + bufferEstaciones.Peek());

        // Consumir el buffer
        while (bufferEstaciones.Count > 0)
        {
            string paquete = bufferEstaciones.Dequeue();
            Console.WriteLine("Insertado en Base de Datos -> " + paquete);
        }
    }
}
```
