# Diccionarios y Tablas Hash

Las tablas hash son estructuras de datos diseñadas para ofrecer búsquedas, inserciones y eliminaciones en tiempo promedio constante O(1). En C#, se implementan mediante dos clases principales en System.Collections.Generic: Dictionary<TKey, TValue> y HashSet<T>.

Su objetivo principal es permitirte acceder a un elemento directamente mediante una clave sin tener que recorrer toda la colección.

## Cómo Funciona el Hash Paso a Paso

Una tabla hash utiliza una función hash (GetHashCode()) que transforma una clave (como un texto o identificador) en un índice numérico de un arreglo interno.

```
Proceso de guardado y búsqueda en tiempo O(1):

Paso 1: Queremos guardar la temperatura de "SensorA" = 24.5

  Clave: "SensorA" ---> [ Función Hash ] ---> Genera Índice: 3

Paso 2: El sistema guarda el valor directamente en la casilla 3 del arreglo interno.

  Índice 0: [     ]
  Índice 1: [     ]
  Índice 2: [     ]
  Índice 3: [ "SensorA" : 24.5 ]  <--- Guardado en 1 solo paso

Paso 3: Cuando consultamos "SensorA", la función hash recalcula el Índice 3 y salta directamente a esa posición de memoria en tiempo O(1).
```

### Proceso de un Hash Join O(N + M) entre dos tablas

En ingeniería de ciencia de datos, cuando quieres unir dos listas de registros por una clave común (como un UserID), realizar un Hash Join evita el costo cuadrático O(N * M) de usar dos bucles anidados.

```
Tabla Usuarios (N = 2):              Tabla Ventas (M = 3):
ID: 101, Nombre: "Ana"               UsuarioID: 102, Monto: 50.0
ID: 102, Nombre: "Carlos"            UsuarioID: 101, Monto: 120.0
                                     UsuarioID: 102, Monto: 30.0

Paso 1: Convertimos la tabla de Usuarios en un Diccionario O(N)
Diccionario = { 101: "Ana", 102: "Carlos" }

Paso 2: Recorremos las Ventas O(M) y buscamos cada UsuarioID en el diccionario en O(1)
Venta 1: UsuarioID 102 -> Encuentra "Carlos" en O(1)
Venta 2: UsuarioID 101 -> Encuentra "Ana" en O(1)
Venta 3: UsuarioID 102 -> Encuentra "Carlos" en O(1)

Tiempo total: O(N + M). Si N y M valen 100,000, pasa de 10,000,000,000 operaciones a solo 200,000 operaciones.
```

## Cuándo Usar Diccionarios y HashSet en Ingeniería de Ciencia de Datos

1. Deduplicación rápida con HashSet<T>: Para filtrar registros o identificadores repetidos en O(1).

2. Tablas de frecuencia e histogramas: Para contar cuántas veces aparece cada palabra o categoría en un dataset.

3. Hash Joins e indexación en memoria: Para relacionar dos datasets mediante una clave sin penalizar el tiempo de ejecución.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploHash
{
    static void Main()
    {
        // HashSet para deduplicar IDs
        HashSet<string> usuariosUnicos = new HashSet<string>();
        usuariosUnicos.Add("USR_100");
        usuariosUnicos.Add("USR_200");
        bool agregadoDeNuevo = usuariosUnicos.Add("USR_100"); // Retorna false en O(1)

        Console.WriteLine("¿Se pudo agregar el duplicado USR_100?: " + agregadoDeNuevo);
        Console.WriteLine("Total usuarios únicos: " + usuariosUnicos.Count);

        // Dictionary para contar frecuencias de palabras
        string texto = "datos ciencia datos algoritmo datos ciencia";
        string[] palabras = texto.Split(' ');

        Dictionary<string, int> frecuencias = new Dictionary<string, int>();
        foreach (string p in palabras)
        {
            if (frecuencias.ContainsKey(p))
                frecuencias[p]++;
            else
                frecuencias[p] = 1;
        }

        Console.WriteLine("\nFrecuencia de palabras:");
        foreach (var par in frecuencias)
        {
            Console.WriteLine(par.Key + ": " + par.Value);
        }
    }
}
```
