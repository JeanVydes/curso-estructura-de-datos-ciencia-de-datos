# Heaps y Colas de Prioridad

Un Heap (o Montículo) es una estructura de datos basada en un árbol binario casi completo que satisface la propiedad del heap:
En un Min-Heap, el valor de cada nodo padre es menor o igual que el de sus nodos hijos. Por lo tanto, el elemento más pequeño de toda la estructura se encuentra siempre en la raíz.

En C#, disponemos de la clase nativa PriorityQueue<TElement, TPriority> que implementa internamente un Min-Heap.

## Cómo Funciona un Min-Heap Internamente

Veamos cómo funciona la propiedad del heap y qué sucede cuando insertamos y extraemos elementos:

```
Estado inicial de un Min-Heap con prioridades 1, 3, 5:

       ( 1 ) <-- Raíz (Siempre el elemento con menor valor/prioridad)
      /     \
   ( 3 )   ( 5 )

Insertamos un nuevo elemento con prioridad 2:

Paso 1: Se coloca temporalmente al final del árbol.
       ( 1 )
      /     \
   ( 3 )   ( 5 )
   /
( 2 )

Paso 2: Como 2 < 3, viola la propiedad del Min-Heap con su padre (3). Intercambiamos 2 y 3 (proceso llamado Heapify Up).
       ( 1 )
      /     \
   ( 2 )   ( 5 )
  /
( 3 )

Ahora el árbol vuelve a cumplir la propiedad del heap en O(log n).
```

### Proceso de Desencolar (Dequeue)

```
Al llamar Dequeue(), extraemos la raíz (1), que es el elemento prioritario.
El último elemento del árbol (3) pasa a la raíz y se reacomoda hacia abajo (Heapify Down) comparando con sus hijos hasta recuperar la propiedad.
Este proceso toma tiempo O(log n).
```

## Cuándo Usar un Heap en Ingeniería de Ciencia de Datos

1. Algoritmo de selección Top-K en datasets grandes: Si quieres encontrar los 5 productos más vendidos entre 10 millones de registros, no necesitas ordenar los 10 millones de elementos en O(n log n). Puedes mantener un Min-Heap de tamaño K = 5. A medida que recorres el dataset, si el elemento nuevo es mayor que la raíz del heap, eliminas la raíz y colocas el nuevo. Esto toma tiempo O(n log K) y usa una cantidad mínima de memoria.

2. Atención prioritaria de tareas o alertas: Cuando tienes eventos de diferentes niveles de gravedad y las emergencias deben procesarse antes que los mensajes normales.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploHeaps
{
    // Algoritmo Top-K usando PriorityQueue como Min-Heap
    static List<string> ObtenerTopK(List<(string Producto, double Venta)> ventas, int k)
    {
        // El segundo parámetro de PriorityQueue define la prioridad (en este caso el valor de venta)
        PriorityQueue<string, double> heapMinimo = new PriorityQueue<string, double>();

        foreach (var registro in ventas)
        {
            heapMinimo.Enqueue(registro.Producto, registro.Venta);

            // Mantener el heap con tamaño máximo K
            if (heapMinimo.Count > k)
            {
                heapMinimo.Dequeue(); // Elimina el elemento con menor venta
            }
        }

        List<string> resultado = new List<string>();
        while (heapMinimo.Count > 0)
        {
            resultado.Add(heapMinimo.Dequeue());
        }

        return resultado;
    }

    static void Main()
    {
        var datasetVentas = new List<(string, double)>
        {
            ("Teclado", 50.0),
            ("Laptop", 1200.0),
            ("Mouse", 20.0),
            ("Monitor", 350.0),
            ("Servidor", 5000.0)
        };

        int k = 2;
        List<string> top2 = ObtenerTopK(datasetVentas, k);

        Console.WriteLine("Top " + k + " productos con mayores ventas:");
        foreach (string prod in top2)
        {
            Console.WriteLine("- " + prod);
        }
    }
}
```
