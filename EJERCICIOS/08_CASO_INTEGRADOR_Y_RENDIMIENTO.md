# Caso Integrador y Ejercicios de Rendimiento y Pruebas

En este último bloque de la materia para Ingeniería de Ciencia de Datos integrarás múltiples estructuras de datos en un sistema complejo, realizarás pruebas de rendimiento empírico (benchmarking) y escribirás pruebas unitarias para certificar que las invariantes se cumplen bajo carga.

## Caso Integrador Final: Rediseño del Pipeline de Datos de MagdalenaExpress

Como proyecto integrador del semestre, debes consolidar las soluciones individuales estudiadas en un pipeline completo para la empresa de transporte MagdalenaExpress.

El sistema debe coordinar cuatro componentes integrados:

```
Pipeline de procesamiento completo:

1. [ Escaneo e Ingestión ] ---> HashSet<string> para deduplicar placas/códigos en O(1)
2. [ Buffer de Despacho ]  ---> Queue<string> para mantener orden FIFO de carga
3. [ Envíos de Urgencia ]  ---> PriorityQueue<string, int> para despachos de emergencia médica
4. [ Auditoría & Undo ]    ---> Stack<string> para revertir errores de digitación
```

## Parte 1: Benchmarking de Memoria y Tiempo

El equipo de infraestructura duda si vale la pena especificar la capacidad inicial en las colecciones. Debes construir un script en C# con la clase Stopwatch y GC.GetTotalMemory() que compare:
- Experimento 1: Llenar 5,000,000 de registros en una List<double> sin capacidad inicial.
- Experimento 2: Llenar 5,000,000 de registros en una List<double>(5000000) con capacidad preallocada.

```
Medición de Benchmark:

[ Limpieza Inicial GC.Collect() ] ---> Medir RAM 1 ---> Cronómetro Start
                                                              │
                                                        Llenar Lista
                                                              │
[ Resultado: RAM 2 - RAM 1 | Tiempo ms ] <--- Medir RAM 2 <--- Cronómetro Stop
```

## Parte 2: Suite de Pruebas Unitarias para Validar Invariantes

Debes implementar dos pruebas unitarias automáticas:
1. Prueba de Invariante FIFO: Garantizar que al encolar "A" y luego "B", Dequeue() retorne estrictamente "A".
2. Prueba de Invariante HashSet: Garantizar que al intentar agregar dos veces el mismo elemento "COD-1", HashSet.Add() retorne false en la segunda llamada y la cuenta de elementos permanezca en 1.

## Preguntas de Racionalización Finales

1. Analiza los resultados del benchmark de capacidad preallocada. ¿Por qué preasignar la capacidad desde el inicio ahorra tiempo de CPU y reduce el trabajo del Garbage Collector (GC)?

2. ¿Por qué la suite de pruebas unitarias es fundamental en la producción de software de ingeniería de ciencia de datos antes de desplegar un algoritmo a un entorno real?

## Plantilla C# Completa a Ejecutar

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;

public class CasoIntegradorYRendimiento
{
    // 1. Suite de Pruebas Unitarias
    static void EjecutarPruebasUnitarias()
    {
        Console.WriteLine("=== EJECUTANDO SUITE DE PRUEBAS UNITARIAS ===");

        // Test FIFO
        Queue<string> colaPrueba = new Queue<string>();
        colaPrueba.Enqueue("Paquete_A");
        colaPrueba.Enqueue("Paquete_B");
        string retornado = colaPrueba.Dequeue();

        if (retornado != "Paquete_A")
            throw new Exception("FALLO: La cola violó el invariante FIFO.");
        Console.WriteLine("[PASS] Invariante FIFO en Queue verificado.");

        // Test HashSet
        HashSet<string> setPrueba = new HashSet<string>();
        setPrueba.Add("COD-1");
        bool duplicadoAceptado = setPrueba.Add("COD-1");

        if (duplicadoAceptado || setPrueba.Count != 1)
            throw new Exception("FALLO: HashSet aceptó un elemento duplicado.");
        Console.WriteLine("[PASS] Invariante de Deduplicación en HashSet verificado.");
    }

    // 2. Experimento de Benchmark
    static void EjecutarBenchmark()
    {
        int total = 5000000;
        Console.WriteLine("\n=== BENCHMARK DE RENDIMIENTO (N = " + total + ") ===");

        // Experimento 1: Sin capacidad
        GC.Collect();
        GC.WaitForPendingFinalizers();
        long mem1 = GC.GetTotalMemory(true);
        Stopwatch sw = Stopwatch.StartNew();

        List<double> listSinCapacidad = new List<double>();
        for (int i = 0; i < total; i++) listSinCapacidad.Add(i * 1.0);
        sw.Stop();

        long mem2 = GC.GetTotalMemory(false);
        Console.WriteLine("Sin Capacidad: " + sw.ElapsedMilliseconds + " ms | RAM Aprox: " + ((mem2 - mem1) / 1024 / 1024) + " MB");

        // Experimento 2: Con capacidad
        GC.Collect();
        GC.WaitForPendingFinalizers();
        mem1 = GC.GetTotalMemory(true);
        sw.Restart();

        List<double> listConCapacidad = new List<double>(total);
        for (int i = 0; i < total; i++) listConCapacidad.Add(i * 1.0);
        sw.Stop();

        mem2 = GC.GetTotalMemory(false);
        Console.WriteLine("Con Capacidad Preallocada: " + sw.ElapsedMilliseconds + " ms | RAM Aprox: " + ((mem2 - mem1) / 1024 / 1024) + " MB");
    }

    static void Main()
    {
        EjecutarPruebasUnitarias();
        EjecutarBenchmark();
    }
}
```
