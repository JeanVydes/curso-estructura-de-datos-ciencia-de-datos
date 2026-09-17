# Listas y Arrays: Arrays Estáticos, List<T> y LinkedList<T>

Como estudiante de ingeniería de ciencia de datos, las estructuras de datos lineales serán tus herramientas de trabajo diarias para almacenar secuencias de información. En C# dispones de tres formas principales de trabajar con listas: arrays estáticos (T[]), listas dinámicas (List<T>) y listas enlazadas (LinkedList<T>).

Cada una gestiona la memoria RAM de forma distinta y responde de manera diferente a las operaciones de acceso, inserción y eliminación.

## Cómo Funcionan Internamente

### Array Estático (T[])

Un array estático reserva un bloque continuo de memoria RAM fija desde el momento de su creación.

```
Memoria RAM contigua para un array de 4 elementos:

Dirección: 1000     1004     1008     1012
Datos:     [ 23.5 ] [ 10.1 ] [ 45.0 ] [ 98.2 ]
Índice:       0        1        2        3

Como las posiciones están juntas en memoria, calcular la dirección del índice i es matemáticamente directo:
Dirección(i) = DirecciónInicial + (i * TamañoElemento)
Esto permite un acceso inmediato en tiempo O(1).
```

### Lista Dinámica (List<T>)

La clase List<T> en C# utiliza internamente un array estático. Cuando agregas elementos con el método Add() y alcanzas la capacidad máxima del array interno, C# realiza los siguientes pasos automáticamente en memoria:

```
Paso 1: La lista tiene capacidad 2 y está llena.
Array Interno: [ A ] [ B ]  (Capacidad: 2, Count: 2)

Paso 2: Queremos agregar C. C# crea un NUEVO array con el doble de capacidad (4).
Array Nuevo:   [   ] [   ] [   ] [   ]  (Capacidad: 4)

Paso 3: Copia los elementos del array viejo al nuevo y agrega C.
Array Nuevo:   [ A ] [ B ] [ C ] [   ]  (Capacidad: 4, Count: 3)

Paso 4: El array viejo se descarta para ser limpiado por el Garbage Collector.
```

Este proceso de redimensionar y copiar n elementos toma tiempo O(n), pero como solo ocurre esporádicamente, el costo promedio de insertar al final se considera O(1) amortizado.

### Lista Enlazada (LinkedList<T>)

Una lista enlazada no guarda sus elementos en posiciones contiguas de memoria. En su lugar, crea nodos dispersos en la RAM donde cada nodo guarda su valor y dos punteros: uno hacia el nodo anterior y otro hacia el nodo siguiente.

```
Insertar un nodo nuevo X entre B y C:

Estado Inicial:
[ Nodo A ] <---> [ Nodo B ] <---------------------> [ Nodo C ]

Paso 1: Se crea el nuevo Nodo X con sus punteros apuntando a B y a C.
                  [ Nodo X ]
                 /          \
                v            v
[ Nodo A ] <---> [ Nodo B ] <---------------------> [ Nodo C ]

Paso 2: Se actualizan los punteros de B y C para que apunten a X.
[ Nodo A ] <---> [ Nodo B ] <---> [ Nodo X ] <---> [ Nodo C ]

No hizo falta desplazar ningún elemento en memoria. La inserción se realiza en tiempo O(1) cambiando solo los punteros.
```

## Criterios de Selección para Ingeniería de Datos

Para seleccionar la estructura adecuada, evalúa cuál es la operación dominante en tu pipeline:

1. Utiliza Array Estático (T[]) cuando conozcas exactamente la cantidad de datos desde el inicio (como vectores de características de tamaño fijo en modelos numéricos). Es la opción más rápida en lectura gracias a la cercanía de los datos en la memoria caché del procesador.

2. Utiliza List<T> como opción por defecto para leer y acumular filas de archivos CSV o JSON donde la cantidad total no se conoce previamente. Si tienes una estimación de cuántas filas habrá, inicialízala especificando esa capacidad en el constructor: new List<double>(100000) para evitar que C# tenga que redimensionar el array interno múltiples veces.

3. Utiliza LinkedList<T> si necesitas continuamente agregar o eliminar elementos en los extremos o en posiciones intermedias, como en algoritmos de ventana deslizante sobre flujos continuos de datos.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploListas
{
    static void Main()
    {
        // Array Estático: Tamaño fijo
        double[] vectorSensor = new double[] { 12.4, 15.8, 20.1 };
        Console.WriteLine("Elemento en índice 0: " + vectorSensor[0]);

        // Lista Dinámica: Acumulador de registros
        List<double> lecturas = new List<double>(500);
        lecturas.Add(12.4);
        lecturas.Add(15.8);
        Console.WriteLine("Total lecturas registradas: " + lecturas.Count);

        // Lista Enlazada: Ventana deslizante manteniendo los últimos 3 elementos
        LinkedList<double> ventana = new LinkedList<double>();
        double[] datosStream = new double[] { 1.0, 2.0, 3.0, 4.0, 5.0 };

        foreach (double dato in datosStream)
        {
            ventana.AddLast(dato);
            if (ventana.Count > 3)
            {
                ventana.RemoveFirst(); // Eliminación en O(1) del más antiguo
            }

            Console.Write("Ventana actual: ");
            foreach (double v in ventana)
            {
                Console.Write(v + " ");
            }
            Console.WriteLine();
        }
    }
}
```
