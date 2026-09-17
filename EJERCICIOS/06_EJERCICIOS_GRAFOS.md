# Ejercicios de Grafos y Redes

En este sexto bloque trabajaremos con estructuras no lineales orientadas al modelado de redes y relaciones complejas entre entidades.

## Caso 6: Red de Conectividad VIAL y Logistica en el Departamento del Magdalena

El Departamento de Planeacion del Magdalena necesita modelar la red de municipios interconectados por carreteras principales para optimizar la distribucion de insumos agricolas.

La red de municipios y carreteras bidireccionales es la siguiente:

```
Red de carreteras del Magdalena:

[ Santa Marta ] <-------> [ Cienaga ] <-------> [ Fundacion ]
       |                                              |
       v                                              v
  [ Aracataca ] <--------------------------------> [ Plato ]
```

Se requiere responder a dos consultas fundamentales:
1. Almacenar la red en memoria consumiendo la menor cantidad posible de RAM.
2. Calcular el numero MINIMO de municipios intermedios (saltos) para transportar carga desde Santa Marta hasta Plato.

## Preguntas de Racionalizacion

1. Compara las dos formas de representar un grafo en memoria (Matriz de Adyacencia vs Lista de Adyacencia). ¿Por que una Lista de Adyacencia (Dictionary<string, List<string>>) es la opcion correcta para esta red vial dispersa?

2. Para encontrar la ruta con la MENOR cantidad de paradas entre dos municipios, ¿debes utilizar el algoritmo de recorrido BFS (Busqueda en Anchura) o DFS (Busqueda en Profundidad)? Explica como explora los nodos cada algoritmo.

3. Dibuja el diagrama ASCII del recorrido BFS desde Santa Marta buscando llegar a Plato, indicando como evoluciona la cola FIFO de nodos pendientes y el conjunto de visitados.

```
Diagrama ASCII a completar del algoritmo BFS:

Paso 1 (Inicio):
Cola: [ Santa Marta ]
Visitados: { Santa Marta }

Paso 2 (Desencola Santa Marta):
Nuevos vecinos agregados a la cola: [ ? , ? ]
Visitados: { Santa Marta, ? , ? }
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

    // JUSTIFICACION: Se usa BFS con una Queue<T> auxiliar para garantizar encontrar la ruta con menor cantidad de saltos
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
        redVial.AgregarCarretera("Santa Marta", "Cienaga");
        redVial.AgregarCarretera("Cienaga", "Fundacion");
        redVial.AgregarCarretera("Santa Marta", "Aracataca");
        redVial.AgregarCarretera("Fundacion", "Plato");
        redVial.AgregarCarretera("Aracataca", "Plato");

        int saltos = redVial.CalcularMinimoSaltos("Santa Marta", "Plato");
        Console.WriteLine("Minimo de saltos entre Santa Marta y Plato: " + saltos);
    }
}
```
