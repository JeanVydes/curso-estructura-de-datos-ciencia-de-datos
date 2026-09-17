# Listas y Arrays: Arrays Estaticos, List<T> y LinkedList<T>

Las estructuras de datos lineales organizan la informacion en una secuencia contigua o enlazada. En C# disponemos de tres formas principales de trabajar con listas: arrays estaticos (T[]), listas dinamicas (List<T>) y listas enlazadas (LinkedList<T>).

Cada una tiene comportamientos diferentes en cuanto a como gestionan la memoria RAM y como responden a las operaciones de acceso, insercion y eliminacion.

## Como funcionan internamente

### Array Estatico (T[])

Un array estatico reserva un bloque continuo de memoria RAM fija desde el momento de su creacion.

```
Memoria RAM contigua para un array de 4 elementos:

Direccion: 1000     1004     1008     1012
Data:      [ 23.5 ] [ 10.1 ] [ 45.0 ] [ 98.2 ]
Indice:       0        1        2        3

Como las posiciones estan juntas en memoria, calcular la direccion del indice i es matematicamente directo:
Direccion(i) = DireccionInicial + (i * TamañoElemento)
Esto permite un acceso inmediato O(1).
```

### Lista Dinamica (List<T>)

La clase List<T> en C# utiliza internamente un array estatico. Cuando agregamos elementos con el metodo Add() y alcanzamos la capacidad maxima del array interno, C# realiza los siguientes pasos automaticamente:

```
Paso 1: La lista tiene capacidad 2 y esta llena.
Array Interno: [ A ] [ B ]  (Capacidad: 2, Count: 2)

Paso 2: Queremos agregar C. C# crea un NUEVO array con el doble de capacidad (4).
Array Nuevo:   [   ] [   ] [   ] [   ]  (Capacidad: 4)

Paso 3: Copia los elementos del array viejo al nuevo y agrega C.
Array Nuevo:   [ A ] [ B ] [ C ] [   ]  (Capacidad: 4, Count: 3)

Paso 4: El array viejo se descarta para ser limpiado por el Garbage Collector.
```

Este proceso de redimensionar copiar n elementos toma tiempo O(n), pero como solo ocurre de forma esporadica, el costo promedio de insertar al final se considera O(1) amortizado.

### Lista Enlazada (LinkedList<T>)

Una lista enlazada no guarda sus elementos en posiciones contiguas de memoria. En su lugar, crea nodos dispersos donde cada nodo guarda su valor y dos punteros: uno hacia el nodo anterior y otro hacia el nodo siguiente.

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

No hizo falta desplazar ningun elemento en memoria. La insercion se realiza en O(1) cambiando solo los punteros.
```

## Cuando elegir cada una

Para ciencia de datos, elegimos basandonos en la operacion dominante:

1. Usamos Array Estatico cuando conocemos exactamente la cantidad de datos desde el inicio (como vectores de caracteristicas de tamaño fijo). Es la opcion mas rapida en lectura por la cercania de los datos en la memoria cache del procesador.

2. Usamos List<T> como opcion por defecto para leer y acumular filas de archivos CSV o JSON donde la cantidad total no se conoce previamente. Si tenemos una estimacion de cuantas filas habra, podemos inicializarla con esa capacidad: new List<double>(100000) para evitar que C# tenga que redimensionar el array interno varias veces.

3. Usamos LinkedList<T> si necesitamos continuamente agregar o eliminar elementos en los extremos o en posiciones intermedias, como en algoritmos de ventana deslizante sobre flujos de datos.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploListas
{
    static void Main()
    {
        // Array Estatico: Tamaño fijo
        double[] vectorSensor = new double[] { 12.4, 15.8, 20.1 };
        Console.WriteLine("Elemento en indice 0: " + vectorSensor[0]);

        // Lista Dinamica: Acumulador de registros
        List<double> lecturas = new List<double>(500);
        lecturas.Add(12.4);
        lecturas.Add(15.8);
        Console.WriteLine("Total lecturas registradas: " + lecturas.Count);

        // Lista Enlazada: Ventana deslizante manteniendo los ultimos 3 elementos
        LinkedList<double> ventana = new LinkedList<double>();
        double[] datosStream = new double[] { 1.0, 2.0, 3.0, 4.0, 5.0 };

        foreach (double dato in datosStream)
        {
            ventana.AddLast(dato);
            if (ventana.Count > 3)
            {
                ventana.RemoveFirst(); // Eliminacion O(1) del mas antiguo
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
