# Ejercicios de Listas y Arrays

En este primer bloque de ejercicios para Ingeniería de Ciencia de Datos trabajarás con estructuras lineales básicas: Arrays Estáticos (T[]), Listas Dinámicas (List<T>) y Listas Doblemente Enlazadas (LinkedList<T>).

Tu objetivo como estudiante de ingeniería es analizar el problema, decidir cuál de las tres variaciones de listas utilizar y justificar formalmente tu decisión.

## Caso 1: Monitoreo de Salinidad en la Ciénaga Grande de Santa Marta

Un equipo de ingenieros ambientales ha desplegado sensores en la Ciénaga Grande para registrar niveles de salinidad cada hora.

Se presentan dos necesidades técnicas distintas:

### Escenario A: Registro de Muestras de Tamaño Conocido
El equipo realiza una jornada de campo de exactamente 24 horas y conoce de antemano que recolectará exactamente 24 lecturas. Necesitamos la estructura más ligera posible en memoria RAM y con el acceso más rápido por índice.

### Escenario B: Ventana Deslizante de Últimas Muestras
En la estación central se recibe un flujo continuo e indeterminado de lecturas. Queremos mantener una ventana deslizante de exactamente las últimas 5 muestras recibidas. Cada vez que llega una nueva muestra, se agrega al final y se elimina la más antigua del frente.

```
Proceso de ventana deslizante con 3 elementos máximo:

Estado Inicial:     [ 12.5 ] <---> [ 14.1 ] <---> [ 15.0 ]

Llega nuevo dato (18.2):
Paso 1: Agregar al final  -> [ 12.5 ] <---> [ 14.1 ] <---> [ 15.0 ] <---> [ 18.2 ]
Paso 2: Eliminar primero  -> [ 14.1 ] <---> [ 15.0 ] <---> [ 18.2 ]
```

## Preguntas de Racionalización

1. Para el Escenario A, ¿por qué es preferible utilizar un Array Estático (double[]) en lugar de una List<double>? Explica qué ocurre en la memoria caché del procesador.

2. Para el Escenario B, ¿por qué utilizar una List<double> para eliminar el primer elemento (índice 0) requiere tiempo O(n), mientras que con una LinkedList<double> se realiza en tiempo O(1)?

3. Si en lugar de 5 muestras tuvieras que acumular un archivo CSV de 500,000 filas usando una List<double>, ¿por qué es importante indicar la capacidad inicial en el constructor new List<double>(500000)? Explica qué ocurre cuando la lista redimensiona su array interno.

```
Esquema ASCII a analizar: Redimensionamiento interno de List<T>

Capacidad 2 llena:   [ dato1 ] [ dato2 ]
Agregar dato3:      [ dato1 ] [ dato2 ] [ dato3 ] [       ]  (Nueva capacidad: 4)
```

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionListas
{
    static void Main()
    {
        // 1. Escenario A: Implementar con la estructura de tamaño fijo elegida
        // JUSTIFICACIÓN: Se elige double[] porque la capacidad es conocida...
        double[] muestras24Horas = new double[24];
        muestras24Horas[0] = 15.4;

        // 2. Escenario B: Implementar la ventana deslizante con la estructura elegida
        // JUSTIFICACIÓN: Se elige LinkedList<double> porque eliminar al inicio toma O(1)...
        LinkedList<double> ventana = new LinkedList<double>();
        double[] datosNuevos = new double[] { 10.1, 12.3, 14.5, 16.7, 18.9, 20.2 };

        foreach (double d in datosNuevos)
        {
            ventana.AddLast(d);
            if (ventana.Count > 5)
            {
                ventana.RemoveFirst();
            }
        }

        Console.WriteLine("Ventana deslizante actual completada.");
    }
}
```
