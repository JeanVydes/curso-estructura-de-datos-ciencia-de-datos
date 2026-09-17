# Complejidad Algoritmica O(n)

Cuando estamos trabajando en ciencia de datos, uno de los errores mas comunes al iniciar es pensar que si un algoritmo funciona con 10 filas de datos, funcionara igual de bien con 10 millones de filas. Aqui es donde entra la complejidad algoritmica, que nos ayuda a medir como se comporta nuestro codigo a medida que el volumen de datos crece.

Nos enfocamos en dos aspectos principales: tiempo de ejecucion (cuantas operaciones realiza la CPU) y espacio en memoria (cuanta memoria RAM consume el algoritmo).

## Entendiendo la Notacion O grande

La notacion O grande nos permite clasificar los algoritmos segun su peor escenario. Veamos como se comportan los casos mas comunes a traves de graficos ASCII y ejemplos.

### O(1) - Tiempo Constante

Sin importar si tu dataset tiene 10 elementos o 100 millones, la operacion toma exactamente el mismo tiempo. Ocurre cuando accedemos directamente a una posicion de memoria por su indice o por una llave hash.

```
Paso 1: Queremos acceder al indice 2 del arreglo
Indice:    0       1       2       3
Data:   [ 10.5 ] [ 40.2 ] [ 99.1 ] [ 15.0 ]
                           ^
                           |-- Acceso directo en 1 solo paso (O(1))
```

### O(n) - Tiempo Lineal

El tiempo de ejecucion crece de forma directamente proporcional al numero de elementos n. Si los datos se duplican, el tiempo de ejecucion se duplica. Ocurre cuando recorremos una lista elemento por elemento.

```
Buscando el valor 99.1 en una lista no ordenada de n = 4 elementos:

Paso 1: Revisa indice 0 -> [ 10.5 ] != 99.1
Paso 2: Revisa indice 1 -> [ 40.2 ] != 99.1
Paso 3: Revisa indice 2 -> [ 99.1 ] == 99.1 (Encontrado!)

Si la lista tuviera 1,000,000 elementos y el valor esta al final, haria 1,000,000 revisiones.
```

### O(n^2) - Tiempo Cuadratico

Ocurre tipicamente cuando anidamos bucles. Por cada elemento del conjunto, recorremos nuevamente todo el conjunto. Si tenemos n = 1,000 datos, realizaremos 1,000,000 de operaciones.

```
Comparando todos contra todos en un dataset de n = 3:

Elemento A -> Compara con A, luego con B, luego con C (3 pasos)
Elemento B -> Compara con A, luego con B, luego con C (3 pasos)
Elemento C -> Compara con A, luego con B, luego con C (3 pasos)

Total operaciones: 3 x 3 = 9 operaciones (n * n = n^2)
```

## Ejemplo en C#: Comparando O(n^2) frente a O(n)

Imaginemos que tenemos dos listas de identificadores de sensores y queremos encontrar los elementos que coinciden en ambas listas.

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

En este caso, la solucion O(n) tarda apenas unos milisegundos porque intercambiamos un poco de memoria para almacenar el HashSet a cambio de reducir el tiempo de busqueda de lineal a constante.
