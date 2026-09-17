# Ejercicios de Diccionarios y Tablas Hash

En este septimo bloque nos enfocaremos en estructuras basadas en Hashing: Dictionary<TKey, TValue> y HashSet<T>. Analizaremos como aprovechar el tiempo constante O(1) para deduplicar, contar frecuencias y cruzar datasets.

## Caso 7: Control de Peajes y Cruzado de Vehiculos en la Vía Ciénaga - Barranquilla

La concesion vial del peaje de Tasajera registra miles de vehiculos diariamente. Se presentan dos requerimientos criticos de procesamiento de datos:

### Requerimiento A: Deduplicacion en Tiempo Real
Los lectores automaticos de placas suelen fotografiar el mismo vehiculo multiple veces al pasar por la caseta. Necesitamos filtrar placas duplicadas en tiempo O(1) para contar cuantos vehiculos unicos pasaron en el dia.

### Requerimiento B: Hash Join de Datasets en RAM
Tenemos un dataset A con 100,000 registros de pasos por el peaje (Placa, Hora, Tarifa) y un dataset B con 50,000 registros de propietarios de vehiculos (Placa, Nombre, Telefono). Queremos unir ambos datasets para enviar notificaciones de cobro sin usar un bucle anidado O(N * M) que tardaria minutos.

```
Esquema del Hash Join en O(N + M):

Paso 1: Convertir Dataset B (Propietarios) en un Diccionario O(M)
Diccionario: { "AAA-123" : "Juan Perez", "BBB-456" : "Maria Gomez" }

Paso 2: Recorrer Dataset A (Pasos) en O(N) y buscar la placa en el Diccionario en O(1)
Paso "AAA-123" ---> Busca en Diccionario (Encontrado en O(1)) ---> Genera Notificacion para Juan Perez
```

## Preguntas de Racionalizacion

1. Explica la diferencia entre HashSet<T> y Dictionary<TKey, TValue> en C#. ¿En que se parecen internamente?

2. Si intentaras cruzar los dos datasets usando dos bucles foreach anidados, ¿cuantas comparaciones tendria que hacer la CPU en el peor escenario? ¿Cuantas hace el Hash Join en O(N + M)?

3. Dibuja un diagrama ASCII explicando como la funcion GetHashCode() transforma un texto de placa (ejemplo "AAA-123") en un indice entero dentro del arreglo interno de cubos (buckets).

```
Diagrama ASCII a completar:

Placa: "AAA-123" ---> [ GetHashCode() ] ---> Indice: 5 ---> Posicion 5 en Memoria RAM [ O(1) ]
```

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionDiccionarios
{
    static void Main()
    {
        // 1. Deduplicacion con HashSet<string>
        // JUSTIFICACION: HashSet.Add() retorna false en O(1) si la placa ya fue registrada
        HashSet<string> placasUnicas = new HashSet<string>();
        placasUnicas.Add("AAA-123");
        placasUnicas.Add("BBB-456");
        placasUnicas.Add("AAA-123"); // Duplicado omitido

        Console.WriteLine("Total vehiculos unicos: " + placasUnicas.Count);

        // 2. Hash Join entre Pasos y Propietarios
        Dictionary<string, string> mapaPropietarios = new Dictionary<string, string>();
        mapaPropietarios["AAA-123"] = "Juan Perez";
        mapaPropietarios["BBB-456"] = "Maria Gomez";

        List<(string Placa, double Tarifa)> pasosPeaje = new List<(string, double)>
        {
            ("AAA-123", 15000.0),
            ("BBB-456", 15000.0),
            ("AAA-123", 15000.0)
        };

        Console.WriteLine("\n=== NOTIFICACIONES DE PEAJE GENERADAS (HASH JOIN) ===");
        foreach (var paso in pasosPeaje)
        {
            if (mapaPropietarios.TryGetValue(paso.Placa, out string nombrePropietario))
            {
                Console.WriteLine("Cobro de $" + paso.Tarifa + " para: " + nombrePropietario + " (Placa " + paso.Placa + ")");
            }
        }
    }
}
```
