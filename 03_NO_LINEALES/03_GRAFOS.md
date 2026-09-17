# Grafos y Modelado de Redes

Un grafo es una estructura de datos no lineal compuesta por un conjunto de nodos (o vértices) y un conjunto de aristas (o conexiones) que vinculan pares de nodos.

Los grafos son la herramienta principal cuando la información más valiosa de un problema no está en las entidades individuales, sino en las relaciones que existen entre ellas.

## Cómo se Representan en Memoria

Existen dos formas principales de representar un grafo en memoria: la matriz de adyacencia y la lista de adyacencia.

### Lista de Adyacencia

Es la forma más común en C# y en ingeniería de ciencia de datos. Utiliza un diccionario donde cada llave es un nodo y su valor es una lista de los nodos con los que está conectado.

```
Grafo de conexiones entre 3 usuarios:

[ Alice ] <-----> [ Bob ] <-----> [ Charlie ]

Representación como Lista de Adyacencia en C#:

Diccionario:
  "Alice"   -> [ "Bob" ]
  "Bob"     -> [ "Alice", "Charlie" ]
  "Charlie" -> [ "Bob" ]

Esta representación consume memoria O(V + E), donde V es el número de vértices y E el número de aristas. Es ideal para redes dispersas.
```

### Proceso de Recorrido BFS (Búsqueda en Anchura) Paso a Paso

BFS recorre la red nivel por nivel para encontrar el camino más corto o los grados de separación entre dos entidades. Utiliza una cola FIFO interna.

```
Buscando la distancia entre Alice y Charlie:

Paso 1: Iniciamos en Alice (Nivel 0). La agregamos a la cola y la marcamos como visitada.
Cola: [ Alice ]
Visitados: { Alice }

Paso 2: Desencolamos Alice. Miramos sus vecinos: Bob.
Agregamos Bob a la cola (Nivel 1).
Cola: [ Bob ]
Visitados: { Alice, Bob }

Paso 3: Desencolamos Bob. Miramos sus vecinos no visitados: Charlie.
Charlie es nuestro destino. Hemos encontrado que la distancia es 2 saltos.
```

## Cuándo Usar un Grafo en Ingeniería de Ciencia de Datos

1. Redes sociales y sistemas de recomendación: Para modelar conexiones entre usuarios y productos (filtrado colaborativo basado en grafos).

2. Detección de fraudes financieros: Para analizar ciclos de transferencias y flujo de dinero entre cuentas bancarias.

3. Redes de transporte y mapas: Para calcular rutas más cortas o eficientes en logística.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class RedSocial
{
    private Dictionary<string, List<string>> adyacencia = new Dictionary<string, List<string>>();

    public void AgregarConexion(string u1, string u2)
    {
        if (!adyacencia.ContainsKey(u1)) adyacencia[u1] = new List<string>();
        if (!adyacencia.ContainsKey(u2)) adyacencia[u2] = new List<string>();

        adyacencia[u1].Add(u2);
        adyacencia[u2].Add(u1);
    }

    public int ContarSaltos(string inicio, string destino)
    {
        if (!adyacencia.ContainsKey(inicio) || !adyacencia.ContainsKey(destino)) return -1;
        if (inicio == destino) return 0;

        Queue<(string Usuario, int Nivel)> cola = new Queue<(string, int)>();
        HashSet<string> visitados = new HashSet<string>();

        cola.Enqueue((inicio, 0));
        visitados.Add(inicio);

        while (cola.Count > 0)
        {
            var actual = cola.Dequeue();

            foreach (string vecino in adyacencia[actual.Usuario])
            {
                if (vecino == destino) return actual.Nivel + 1;

                if (!visitados.Contains(vecino))
                {
                    visitados.Add(vecino);
                    cola.Enqueue((vecino, actual.Nivel + 1));
                }
            }
        }

        return -1;
    }
}

public class EjemploGrafos
{
    static void Main()
    {
        RedSocial red = new RedSocial();
        red.AgregarConexion("Alice", "Bob");
        red.AgregarConexion("Bob", "Charlie");
        red.AgregarConexion("Charlie", "David");

        int saltos = red.ContarSaltos("Alice", "David");
        Console.WriteLine("Grados de separación entre Alice y David: " + saltos + " saltos");
    }
}
```
