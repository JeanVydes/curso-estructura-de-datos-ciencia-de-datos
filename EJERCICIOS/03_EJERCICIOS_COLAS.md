# Ejercicios de Colas (Queue<T>)

En este tercer bloque nos enfocaremos en la estructura lineal FIFO (First In, First Out), donde el orden temporal de llegada debe ser estrictamente respetado para garantizar el procesamiento secuencial correcto.

## Caso 3: Buffer de Transmisiones Meteorológicas del IDEAM

Estaciones meteorológicas automáticas instaladas en la Sierra Nevada de Santa Marta envían paquetes de datos de precipitación y humedad cada pocos segundos a un servidor central.

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

## Preguntas de Racionalización

1. ¿Por qué violaría los requerimientos del sistema si utilizaras una Pila (Stack<T>) para este servidor de datos meteorológicos? Describe qué le sucedería al Paquete 101 si siguen llegando paquetes continuamente.

2. Explica las tres operaciones primitivas de la clase Queue<T> en C# (Enqueue, Dequeue, Peek) y su complejidad algorítmica en notación O grande.

3. Observa el siguiente esquema ASCII mostrando el estado del frente (Front) y final (Rear) de una cola cuando realizas operaciones Enqueue y Dequeue.

```
Esquema ASCII:

Estado Inicial:
Front                          Rear
[ Dato A ] <--- [ Dato B ]

Después de Enqueue(Dato C):
Front                                         Rear
[ Dato A ] <--- [ Dato B ] <--- [ Dato C ]

Después de Dequeue():
Front                          Rear
[ Dato B ] <--- [ Dato C ]
```

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionColas
{
    static void Main()
    {
        // JUSTIFICACIÓN: Se elige Queue<string> porque la ingestión debe procesarse en estricto orden FIFO
        Queue<string> bufferEstaciones = new Queue<string>();

        // Simular llegada de paquetes
        bufferEstaciones.Enqueue("Estación San Lorenzo: 12mm lluvia");
        bufferEstaciones.Enqueue("Estación El Campano: 8mm lluvia");
        bufferEstaciones.Enqueue("Estación Minca: 15mm lluvia");

        Console.WriteLine("Paquetes pendientes en buffer: " + bufferEstaciones.Count);
        Console.WriteLine("Próximo a procesar (Peek): " + bufferEstaciones.Peek());

        // Consumir el buffer
        while (bufferEstaciones.Count > 0)
        {
            string paquete = bufferEstaciones.Dequeue();
            Console.WriteLine("Insertado en Base de Datos -> " + paquete);
        }
    }
}
```
