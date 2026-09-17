# Benchmarks y Pruebas de Rendimiento

Una vez que entendemos y elegimos las estructuras de datos adecuadas para nuestro problema, el ultimo paso consiste en verificar experimentalmente su impacto en rendimiento y asegurar que nuestro codigo cumple con las invariantes establecidas.

Medimos dos recursos computacionales esenciales: el tiempo de ejecucion en milisegundos mediante la clase Stopwatch y el consumo de memoria RAM mediante el recolector de basura (GC).

## Como medir el rendimiento paso a paso

```
Proceso de medicion de un benchmark en C#:

Paso 1: Forzamos la limpieza inicial de memoria para obtener una linea base limpia.
        GC.Collect() -> Memoria Inicial: M1 MB

Paso 2: Iniciamos el cronometro con Stopwatch.StartNew()

Paso 3: Ejecutamos el procesamiento o llenado de la estructura de datos (N iteraciones)

Paso 4: Detenemos el cronometro (Stopwatch.Stop()) y tomamos la memoria final (M2 MB)

Resultado:
  Tiempo Transcurrido = Cronometro.ElapsedMilliseconds
  Memoria Consumida   = M2 - M1
```

## Pruebas Unitarias para Validar Invariantes

Las pruebas unitarias son metodos automaticos que comprueban si una regla (invariante) se cumple siempre. Por ejemplo, probar que una Cola mantenga el orden FIFO o que un HashSet rechace elementos duplicados.

```
Esquema de una prueba de invariante FIFO:

Accion 1: Encolar(10)
Accion 2: Encolar(20)
Accion 3: Desencolar() -> Valor retornado: X

Verificacion: ¿X == 10?
Si X == 10 -> La prueba PASA (Invariante FIFO cumplido)
Si X != 10 -> La prueba FALLA (Invariante violado)
```

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;

public class PruebasYRendimiento
{
    // Metodo de prueba unitaria para validar invariante FIFO
    static void ProbarInvarianteCola()
    {
        Queue<int> cola = new Queue<int>();
        cola.Enqueue(100);
        cola.Enqueue(200);

        int resultado = cola.Dequeue();
        if (resultado != 100)
        {
            Console.WriteLine("FALLO EN PRUEBA: La cola no cumplio el invariante FIFO.");
        }
        else
        {
            Console.WriteLine("PRUEBA EXITOSA: La cola cumple el invariante FIFO.");
        }
    }

    static void Main()
    {
        // Ejecutamos pruebas unitarias
        Console.WriteLine("--- Ejecutando Pruebas Unitarias ---");
        ProbarInvarianteCola();

        // Benchmark de rendimiento
        Console.WriteLine("\n--- Ejecutando Benchmark de Rendimiento ---");
        int n = 2000000;

        // Medicion 1: Lista sin capacidad inicial
        GC.Collect();
        long memInicial = GC.GetTotalMemory(true);
        Stopwatch crono = Stopwatch.StartNew();

        List<double> lista1 = new List<double>();
        for (int i = 0; i < n; i++)
        {
            lista1.Add(i * 1.0);
        }

        crono.Stop();
        long memFinal = GC.GetTotalMemory(false);
        Console.WriteLine("Sin capacidad inicial: " + crono.ElapsedMilliseconds + " ms | Memoria: " + ((memFinal - memInicial) / 1024 / 1024) + " MB");

        // Medicion 2: Lista con capacidad inicial preallocada
        GC.Collect();
        memInicial = GC.GetTotalMemory(true);
        crono.Restart();

        List<double> lista2 = new List<double>(n); // Pre-asignacion O(1)
        for (int i = 0; i < n; i++)
        {
            lista2.Add(i * 1.0);
        }

        crono.Stop();
        memFinal = GC.GetTotalMemory(false);
        Console.WriteLine("Con capacidad preallocada: " + crono.ElapsedMilliseconds + " ms | Memoria: " + ((memFinal - memInicial) / 1024 / 1024) + " MB");
    }
}
```
