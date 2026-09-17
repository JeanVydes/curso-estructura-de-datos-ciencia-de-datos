# C#: Una Introducción Necesaria para Ingeniería de Ciencia de Datos

Como estudiante de Ingeniería de Ciencia de Datos, antes de manipular estructuras avanzadas debes conocer el lenguaje y las herramientas con las que vas a construir tus soluciones. En este curso trabajaremos con C#.

C# es un lenguaje moderno, fuertemente tipado, orientado a objetos y de muy alto rendimiento desarrollado sobre la plataforma .NET. Si ya estás familiarizado con lenguajes como Java, C++ o TypeScript, te resultará muy intuitivo. Si vienes de Python, notarás que C# requiere definir explícitamente el tipo de cada variable, lo cual te brinda un control de errores superior en tiempo de compilación y un consumo de memoria mucho más predecible.

En la ingeniería de ciencia de datos no necesitamos reimplementar cada estructura desde cero. .NET incluye el espacio de nombres System.Collections.Generic, una librería altamente optimizada que pone a nuestra disposición las estructuras de datos nativas. Tu tarea principal como futuro ingeniero es comprender cómo funcionan internamente para saber cuál elegir en cada problema de procesamiento de datos.

## Conceptos Fundamentales de C# para Ingeniería de Datos

### Tipos de Datos Primitivos y Nulos

En la ingeniería de datos es común enfrentar valores faltantes (Missing Data). C# permite manejar esto mediante tipos nulos (Nullable Types) agregando el símbolo de interrogación (?) al tipo de dato:

```csharp
int edad = 25;                  // Entero normal, no puede ser nulo
double salinidad = 14.8;         // Flotante de doble precisión
bool esValido = true;            // Booleano

double? lecturaOpcional = null;  // Permite representar un dato nulo o faltante
if (lecturaOpcional.HasValue)
{
    Console.WriteLine("La lectura es: " + lecturaOpcional.Value);
}
else
{
    Console.WriteLine("Dato faltante detectado en el sensor.");
}
```

### Modelado de Registros con Clases y Propiedades

Para representar las filas de un dataset, creamos clases con propiedades. Esto nos permite agrupar atributos heterogéneos bajo una misma estructura:

```csharp
public class RegistroSensor
{
    public int Id { get; set; }
    public string Ubicacion { get; set; }
    public double Valor { get; set; }
    public DateTime FechaHora { get; set; }

    public RegistroSensor(int id, string ubicacion, double valor)
    {
        Id = id;
        Ubicacion = ubicacion;
        Valor = valor;
        FechaHora = DateTime.Now;
    }
}
```

### Genericismos (<T>)

El parámetro de tipo genérico <T> permite que las colecciones almacenen cualquier tipo de dato específico sin perder el tipado fuerte y evitando costo de conversión en memoria:

```csharp
List<int> listaEnteros = new List<int>();
List<RegistroSensor> listaSensores = new List<RegistroSensor>();
```

## Colecciones Nativas en System.Collections.Generic

A continuación se resumen las estructuras nativas más utilizadas en C# para la ciencia de datos:

* Array Estático (T[]): Arreglo continuo en memoria RAM con tamaño fijo. Acceso por índice en tiempo O(1).
* Lista Dinámica (List<T>): Arreglo redimensionable automáticamente. Inserción al final en tiempo O(1) amortizado.
* Lista Enlazada (LinkedList<T>): Nodos doblemente enlazados dispersos en memoria. Inserción y eliminación en O(1) en posiciones conocidas.
* Pila (Stack<T>): Colección LIFO para manejo de historiales, deshechos (undo) y parsing sintáctico.
* Cola (Queue<T>): Colección FIFO para buffers de streaming y procesamiento secuencial.
* Cola de Prioridad (PriorityQueue<TElement, TPriority>): Heap Binario nativo para atención prioritaria y algoritmos Top-K.
* Diccionario (Dictionary<TKey, TValue>): Tabla Hash para asociar clave y valor con acceso en tiempo O(1).
* Conjunto (HashSet<T>): Colección de elementos únicos basada en Hash para deduplicación instantánea en O(1).

## Introducción a LINQ (Language Integrated Query)

C# incluye LINQ, una herramienta potente para consultar, filtrar y transformar colecciones de datos con una sintaxis declarativa limpia similar a SQL:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class EjemploLINQ
{
    static void Main()
    {
        List<double> lecturas = new List<double> { 12.5, 45.0, 8.2, 99.1, 23.4 };

        // Filtrar lecturas mayores a 20.0 y ordenarlas de mayor a menor
        var lecturasAltas = lecturas
            .Where(l => l > 20.0)
            .OrderByDescending(l => l)
            .ToList();

        Console.WriteLine("Lecturas altas ordenadas:");
        foreach (var l in lecturasAltas)
        {
            Console.WriteLine("- " + l);
        }

        // Obtener el promedio de las lecturas
        double promedio = lecturas.Average();
        Console.WriteLine("Promedio general: " + promedio);
    }
}
```

## Ejemplo Completo: Pipeline de Ingestión y Triaje de Alertas

El siguiente ejemplo muestra cómo un ingeniero de datos combina diferentes colecciones de C# para procesar registros, eliminar duplicados y atender alertas prioritarias:

```csharp
using System;
using System.Collections.Generic;

public class ProgramaIngenieriaDatos
{
    static void Main()
    {
        // 1. HashSet para deduplicación rápida de sensores escaneados en O(1)
        HashSet<string> sensoresRegistrados = new HashSet<string>();

        // 2. PriorityQueue para atender alertas críticas según su prioridad (menor número = mayor prioridad)
        PriorityQueue<string, int> alertasPrioritarias = new PriorityQueue<string, int>();

        string[] datosEntrada = new string[] {
            "SENSOR_01: Temperatura Normal",
            "SENSOR_02: Sobrecalentamiento Critico",
            "SENSOR_01: Temperatura Normal", // Duplicado
            "SENSOR_03: Presion Alta"
        };

        Console.WriteLine("=== INGESTIÓN Y DEDUPLICACIÓN DE DATOS ===");
        foreach (string registro in datosEntrada)
        {
            string idSensor = registro.Split(':')[0];

            if (sensoresRegistrados.Add(idSensor))
            {
                Console.WriteLine("NUEVO REGISTRO: " + registro);

                // Asignación de nivel de prioridad
                int prioridad = 3; // Normal por defecto
                if (registro.Contains("Critico")) prioridad = 1;
                else if (registro.Contains("Alta")) prioridad = 2;

                alertasPrioritarias.Enqueue(registro, prioridad);
            }
            else
            {
                Console.WriteLine("DUPLICADO OMITIDO: " + idSensor);
            }
        }

        Console.WriteLine("\n=== PROCESAMIENTO DE ALERTAS POR PRIORIDAD (HEAP) ===");
        while (alertasPrioritarias.Count > 0)
        {
            string alerta = alertasPrioritarias.Dequeue();
            Console.WriteLine("Procesando: " + alerta);
        }
    }
}
```