# Ejercicios de Grafos y Redes

En este sexto bloque para Ingeniería de Ciencia de Datos trabajarás con estructuras no lineales orientadas al modelado de redes y relaciones complejas entre entidades.

## Caso 6: Red de Conectividad Vial y Logística en el Departamento del Magdalena

El Departamento de Planeación del Magdalena necesita modelar la red de municipios interconectados por carreteras principales para optimizar la distribución de insumos agrícolas.

La red de municipios y carreteras bidireccionales es la siguiente:

```
Red de carreteras del Magdalena:

[ Santa Marta ] <-------> [ Ciénaga ] <-------> [ Fundación ]
       |                                              |
       v                                              v
  [ Aracataca ] <--------------------------------> [ Plato ]
```

Se requiere responder a dos consultas fundamentales:
1. Almacenar la red en memoria consumiendo la menor cantidad posible de RAM.
2. Calcular el número MÍNIMO de municipios intermedios (saltos) para transportar carga desde Santa Marta hasta Plato.

## Preguntas de Racionalización

1. Compara las dos formas de representar un grafo en memoria (Matriz de Adyacencia vs Lista de Adyacencia). ¿Por qué una Lista de Adyacencia (Dictionary<string, List<string>>) es la opción correcta para esta red vial dispersa?

2. Para encontrar la ruta con la MENOR cantidad de paradas entre dos municipios, ¿debes utilizar el algoritmo de recorrido BFS (Búsqueda en Anchura) o DFS (Búsqueda en Profundidad)? Explica cómo explora los nodos cada algoritmo.

3. Observa la evolución del algoritmo BFS desde Santa Marta buscando llegar a Plato, indicando cómo evoluciona la cola FIFO de nodos pendientes y el conjunto de visitados.

```
Esquema ASCII del algoritmo BFS:

Paso 1 (Inicio):
Cola: [ Santa Marta ]
Visitados: { Santa Marta }

Paso 2 (Desencola Santa Marta):
Nuevos vecinos agregados a la cola: [ Ciénaga, Aracataca ]
Visitados: { Santa Marta, Ciénaga, Aracataca }
```

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionGrafos
{
    private Dictionary<string, List<string>> adyacencia = new Dictionary<string, List<string>>();

    public void AgregarCarretera(string m1, string m2)
    {
        if (!adyacencia.ContainsKey(m1)) adyacencia[m1] = new List<string>();
        if (!adyacencia.ContainsKey(m2)) adyacencia[m2] = new List<string>();

        adyacencia[m1].Add(m2);
        adyacencia[m2].Add(m1);
    }

    // JUSTIFICACIÓN: Se usa BFS con una Queue<T> auxiliar para garantizar encontrar la ruta con menor cantidad de saltos
    public int CalcularMinimoSaltos(string origen, string destino)
    {
        if (!adyacencia.ContainsKey(origen) || !adyacencia.ContainsKey(destino)) return -1;
        if (origen == destino) return 0;

        Queue<(string Municipio, int Saltos)> cola = new Queue<(string, int)>();
        HashSet<string> visitados = new HashSet<string>();

        cola.Enqueue((origen, 0));
        visitados.Add(origen);

        while (cola.Count > 0)
        {
            var actual = cola.Dequeue();

            foreach (string vecino in adyacencia[actual.Municipio])
            {
                if (vecino == destino) return actual.Saltos + 1;

                if (!visitados.Contains(vecino))
                {
                    visitados.Add(vecino);
                    cola.Enqueue((vecino, actual.Saltos + 1));
                }
            }
        }

        return -1;
    }

    static void Main()
    {
        SolucionGrafos redVial = new SolucionGrafos();
        redVial.AgregarCarretera("Santa Marta", "Ciénaga");
        redVial.AgregarCarretera("Ciénaga", "Fundación");
        redVial.AgregarCarretera("Santa Marta", "Aracataca");
        redVial.AgregarCarretera("Fundación", "Plato");
        redVial.AgregarCarretera("Aracataca", "Plato");

        int saltos = redVial.CalcularMinimoSaltos("Santa Marta", "Plato");
        Console.WriteLine("Mínimo de saltos entre Santa Marta y Plato: " + saltos);
    }
}
```
