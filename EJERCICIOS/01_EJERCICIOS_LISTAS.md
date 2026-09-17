# Ejercicios de Listas y Arrays

En este primer bloque de ejercicios trabajaremos con estructuras lineales basicas: Arrays Estaticos (T[]), Listas Dinamicas (List<T>) y Listas Doblemente Enlazadas (LinkedList<T>).

Tu objetivo como estudiante de Ciencia de Datos es analizar el problema, decidir cual de las tres variaciones de listas utilizar y justificar formalmente tu respuesta.

## Caso 1: Monitoreo de Salinidad en la Cienaga Grande de Santa Marta

Un equipo de investigadores ambientales ha desplegado sensores en la Cienaga Grande para registrar niveles de salinidad cada hora.

Se presentan dos necesidades tecnicas distintas:

### Escenario A: Registro de Muestras de Tamaño Conocido
El equipo realiza una jornada de campo de exactamente 24 horas y conoce de antemano que recolectara exactamente 24 lecturas de temperatura. Necesitamos la estructura mas ligera posible en memoria RAM y con el acceso mas rapido por indice.

### Escenario B: Ventana Deslizante de Ultimas Muestras
En la estacion central se recibe un flujo continuo e indeterminado de lecturas. Queremos mantener una ventana deslizante de exactamente las ultimas 5 muestras recibidas. Cada vez que llega una nueva muestra, se agrega al final y se elimina la mas antigua del frente.

```
Proceso de ventana deslizante con 3 elementos maximo:

Estado Inicial:     [ 12.5 ] <---> [ 14.1 ] <---> [ 15.0 ]

Llega nuevo dato (18.2):
Paso 1: Agregar al final -> [ 12.5 ] <---> [ 14.1 ] <---> [ 15.0 ] <---> [ 18.2 ]
Paso 2: Eliminar el primero -> [ 14.1 ] <---> [ 15.0 ] <---> [ 18.2 ]
```

## Preguntas de Racionalizacion

1. Para el Escenario A, ¿por que es preferible utilizar un Array Estatico (double[]) en lugar de una List<double>? Explica que ocurre en la memoria cache del procesador.

2. Para el Escenario B, ¿por que utilizar una List<double> para eliminar el primer elemento (indice 0) requiere tiempo O(n), mientras que con una LinkedList<double> se realiza en tiempo O(1)?

3. Si en lugar de 5 muestras tuvieras que acumular un archivo CSV de 500,000 filas usando una List<double>, ¿por que es importante pasar la capacidad inicial en el constructor new List<double>(500000)? Dibuja en ASCII que ocurre cuando la lista redimensiona su array interno.

```
Diagrama ASCII a completar: Redimensionamiento interno de List<T>

Capacidad 2 llena:   [ dato1 ] [ dato2 ]
Agregar dato3:      [ ? ] [ ? ] [ ? ] [ ? ]  <-- Explica que sucede aqui
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
        // JUSTIFICACION: Se elige __________ porque...
        double[] muestras24Horas = new double[24];
        muestras24Horas[0] = 15.4;

        // 2. Escenario B: Implementar la ventana deslizante con la estructura elegida
        // JUSTIFICACION: Se elige __________ porque eliminar al inicio toma O(1)...
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
