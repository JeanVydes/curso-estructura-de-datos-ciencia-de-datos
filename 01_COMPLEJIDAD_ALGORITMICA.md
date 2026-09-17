# Complejidad Algorítmica O(n)

Como estudiante de ingeniería de ciencia de datos, uno de los errores más comunes al iniciar es pensar que si un algoritmo funciona bien con 10 filas de datos, funcionará igual cuando enfrentes 10 millones de filas. Aquí es donde entra la complejidad algorítmica, que nos ayuda a medir y predecir cómo se comportará tu código a medida que el volumen de datos crece.

En la ingeniería analizamos dos aspectos principales: tiempo de ejecución (cuántas operaciones realiza la CPU) y espacio en memoria (cuánta memoria RAM consume el algoritmo durante la ejecución).

## Entendiendo la Notación O grande

La notación O grande nos permite clasificar los algoritmos según su peor escenario posible. Veamos cómo se comportan las complejidades más comunes a través de esquemas ASCII y ejemplos prácticos.

### O(1) - Tiempo Constante

Sin importar si tu conjunto de datos tiene 10 elementos o 100 millones, la operación toma exactamente el mismo tiempo. Ocurre cuando accedemos directamente a una posición de memoria por su índice o mediante una clave hash.

```
Paso 1: Queremos acceder al índice 2 del arreglo
Índice:    0       1       2       3
Datos:  [ 10.5 ] [ 40.2 ] [ 99.1 ] [ 15.0 ]
                           ^
                           |-- Acceso directo en 1 solo paso (O(1))
```

### O(n) - Tiempo Lineal

El tiempo de ejecución crece de forma directamente proporcional al número de elementos n. Si los datos se duplican, el tiempo de ejecución también se duplica. Ocurre cuando recorremos una lista elemento por elemento.

```
Buscando el valor 99.1 en una lista no ordenada de n = 4 elementos:

Paso 1: Revisa índice 0 -> [ 10.5 ] != 99.1
Paso 2: Revisa índice 1 -> [ 40.2 ] != 99.1
Paso 3: Revisa índice 2 -> [ 99.1 ] == 99.1 (Encontrado)

Si la lista tuviera 1,000,000 de elementos y el valor estuviera al final, realizaría 1,000,000 de revisiones.
```

### O(n^2) - Tiempo Cuadrático

Ocurre típicamente cuando anidamos bucles. Por cada elemento del conjunto, recorremos nuevamente todo el conjunto. Si tenemos n = 1,000 datos, realizaremos 1,000,000 de operaciones. En la ingeniería de ciencia de datos, los algoritmos O(n^2) se consideran peligrosos porque colapsan los servidores al escalar los datasets.

```
Comparando todos contra todos en un dataset de n = 3:

Elemento A -> Compara con A, luego con B, luego con C (3 pasos)
Elemento B -> Compara con A, luego con B, luego con C (3 pasos)
Elemento C -> Compara con A, luego con B, luego con C (3 pasos)

Total operaciones: 3 x 3 = 9 operaciones (n * n = n^2)
```

## Ejemplo en C#: Comparando O(n^2) frente a O(n)

Imagina que como ingeniero de datos necesitas comparar dos listas de identificadores de sensores y encontrar los elementos coincidentes en ambas.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;

public class EjemploComplejidad
{
    static void Main()
    {
        int n = 30000;
        List<int> listaA = new List<int>();
        List<int> listaB = new List<int>();
        Random rand = new Random(42);

        for (int i = 0; i < n; i++)
        {
            listaA.Add(rand.Next(0, 50000));
            listaB.Add(rand.Next(25000, 75000));
        }

        // Forma ineficiente O(n^2) con bucles anidados
        Stopwatch crono = Stopwatch.StartNew();
        int coincidenciasN2 = 0;
        for (int i = 0; i < listaA.Count; i++)
        {
            for (int j = 0; j < listaB.Count; j++)
            {
                if (listaA[i] == listaB[j])
                {
                    coincidenciasN2++;
                    break;
                }
            }
        }
        crono.Stop();
        Console.WriteLine("Tiempo O(n^2): " + crono.ElapsedMilliseconds + " ms");

        // Forma eficiente O(n) usando un HashSet en memoria
        crono.Restart();
        int coincidenciasN = 0;
        HashSet<int> conjuntoB = new HashSet<int>(listaB);
        for (int i = 0; i < listaA.Count; i++)
        {
            if (conjuntoB.Contains(listaA[i]))
            {
                coincidenciasN++;
            }
        }
        crono.Stop();
        Console.WriteLine("Tiempo O(n): " + crono.ElapsedMilliseconds + " ms");
    }
}
```

En este experimento notarás la diferencia: la solución O(n) responde en cuestión de milisegundos porque intercambiamos un poco de memoria para almacenar el HashSet a cambio de reducir la complejidad temporal de lineal a constante en las búsquedas.
