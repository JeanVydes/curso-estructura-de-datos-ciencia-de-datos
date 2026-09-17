# Grafos y Modelado de Redes

Un grafo es una estructura de datos no lineal compuesta por un conjunto de nodos (o vertices) y un conjunto de aristas (o conexiones) que vinculan pares de nodos.

Los grafos son la herramienta principal cuando la informacion mas valiosa de un problema no esta en las entidades individuales, sino en las relaciones que existen entre ellas.

## Como se representan en memoria

Existen dos formas principales de representar un grafo en memoria: la matriz de adyacencia y la lista de adyacencia.

### Lista de Adyacencia

Es la forma mas comun en C# y ciencia de datos. Utiliza un diccionario donde cada llave es un nodo y su valor es una lista de los nodos con los que esta conectado.

```
Grafo de conexiones entre 3 usuarios:

[ Alice ] <-----> [ Bob ] <-----> [ Charlie ]

Representacion como Lista de Adyacencia en C#:

Diccionario:
  "Alice"   -> [ "Bob" ]
  "Bob"     -> [ "Alice", "Charlie" ]
  "Charlie" -> [ "Bob" ]

Esta representacion consume memoria O(V + E), donde V es el numero de vertices y E el numero de aristas. Es ideal para redes dispersas.
```

### Proceso de Recorrido BFS (Busqueda en Anchura) paso a paso

BFS recorre la red nivel por nivel para encontrar el camino mas corto o los grados de separacion entre dos personas. Utiliza una cola FIFO interna.

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

## Cuando usar un Grafo en Ciencia de Datos

1. Redes sociales y sistemas de recomendacion: Para modelar conexiones entre usuarios y productos (filtrado colaborativo basado en grafos).

2. Deteccion de fraudes financieros: Para analizar ciclos de transferencias y flujo de dinero entre cuentas bancarias.

3. Redes de transporte y mapas: Para calcular rutas mas cortas o eficientes en logistica.

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
        Console.WriteLine("Grados de separacion entre Alice y David: " + saltos + " saltos");
    }
}
```
