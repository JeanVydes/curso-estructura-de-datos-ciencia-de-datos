# Ejercicios de Heaps y Colas de Prioridad (PriorityQueue)

En este quinto bloque trabajaremos con la estructura de Heap (Monticulo) y la clase nativa PriorityQueue<TElement, TPriority> de C#. Analizaremos situaciones donde la prioridad manda sobre el orden de llegada.

## Caso 5: Triaje de Pacientes en la Sala de Urgencias del Hospital Universitario

El Hospital Universitario de Santa Marta atiende a cientos de pacientes diariamente en su sala de urgencias.

El sistema de triaje asigna un nivel de prioridad numerico a cada paciente segun su estado clinico:
- Prioridad 1: Emergencia Vital (Infarto, Trauma Severo).
- Prioridad 2: Urgencia Alta (Fractura expuesta).
- Prioridad 3: Urgencia Media (Fiebre alta).
- Prioridad 5: Atencion General (Sintoma leve).

El sistema de la sala de espera debe despachar siempre al paciente con la MENOR cifra numerica de prioridad (Prioridad 1 primero), sin importar la hora de llegada a la sala.

```
Llegada de pacientes a urgencias:

Llega Paciente A (Gripa, Prioridad 5)   at [08:00 AM]
Llega Paciente B (Fractura, Prioridad 2) at [08:05 AM]
Llega Paciente C (Infarto, Prioridad 1)  at [08:10 AM]

Atencion requerida por el hospital:
1. Atender a Paciente C (Infarto, Prioridad 1)
2. Atender a Paciente B (Fractura, Prioridad 2)
3. Atender a Paciente A (Gripa, Prioridad 5)
```

## Preguntas de Racionalizacion

1. ¿Por que una cola convencional FIFO (Queue<T>) resulta inaceptable para la gestion de urgencias medicas de este hospital?

2. La clase PriorityQueue<TElement, TPriority> de C# implementa internamente un Min-Heap. Explica que significa que la raiz del arbol contenga siempre el elemento con menor valor de prioridad.

3. Dibuja un diagrama ASCII mostrando que ocurre internamente en el Min-Heap cuando insertamos Paciente A (5), luego Paciente B (2) y finalmente Paciente C (1) con el proceso de reacomodo Heapify Up.

```
Diagrama ASCII a completar:

Paso 1 (A:5):       Paso 2 (B:2):                 Paso 3 (C:1):
   ( 5 )               ( 5 )                        ( 1 )
                      /                            /     \
                    ( 2 )  --> Heapify Up -->   ( 2 )   ( 5 )
```

4. Para resolver el problema de encontrar los K productos mas vendidos de un dataset de 10 millones de filas sin ordenar todo el arreglo, ¿por que un Min-Heap de tamaño K permite resolverlo en tiempo O(n log k)?

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionHeaps
{
    static void Main()
    {
        // JUSTIFICACION: Se utiliza PriorityQueue<string, int> porque implementa un Min-Heap que mantiene el paciente mas urgente en la raiz en O(log n)
        PriorityQueue<string, int> salaUrgencias = new PriorityQueue<string, int>();

        // Ingreso de pacientes
        salaUrgencias.Enqueue("Paciente A: Gripa", 5);
        salaUrgencias.Enqueue("Paciente B: Fractura", 2);
        salaUrgencias.Enqueue("Paciente C: Infarto", 1);

        Console.WriteLine("=== ATENCION EN SALA DE URGENCIAS ===");
        while (salaUrgencias.Count > 0)
        {
            string pacienteAtendido = salaUrgencias.Dequeue();
            Console.WriteLine("Atendiendo -> " + pacienteAtendido);
        }
    }
}
```
