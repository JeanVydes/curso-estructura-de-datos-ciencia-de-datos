# Ejercicios de Árboles y Búsqueda Jerárquica

En este cuarto bloque para Ingeniería de Ciencia de Datos trabajarás con estructuras no lineales jerárquicas, específicamente el Árbol Binario de Búsqueda (BST), analizando cómo mantener el invariante de orden y lograr búsquedas logarítmicas O(log n).

## Caso 4: Indexación de Especies Marinas en el Parque Nacional Natural Tayrona

El centro de investigación marina del Parque Tayrona clasifica avistamientos de especies mediante un código numérico de catálogo.

Necesitamos almacenar 5,000 códigos de especies de forma que se puedan realizar búsquedas inmediatas y, al mismo tiempo, imprimir el catálogo completo ordenado de menor a mayor sin necesidad de ordenar un arreglo desde cero.

```
Estructura del Árbol BST de especies:

                  ( 500 ) <-- Raíz
                 /       \
            ( 300 )     ( 800 )
            /     \
       ( 200 )   ( 400 )
```

## Preguntas de Racionalización

1. ¿Cuál es el invariante fundamental de un Árbol Binario de Búsqueda (BST) respecto a los nodos a la izquierda y derecha de cualquier nodo N?

2. Si insertamos los códigos en el siguiente orden: [500, 300, 800, 200, 400], observa el progreso paso a paso en el esquema ASCII.

```
Esquema ASCII de construcción:

Paso 1 (500):       Paso 2 (300):       Paso 3 (800):
   (500)               (500)               (500)
                      /                   /     \
                    (300)               (300)   (800)
```

3. Si por el contrario insertamos los códigos ya ordenados [100, 200, 300, 400, 500] en un BST simple sin autobalanceo, ¿en qué estructura se degrada el árbol y cuál sería el tiempo de búsqueda de un elemento?

4. ¿Qué tipo de recorrido de árbol (Pre-Order, In-Order o Post-Order) debes utilizar para imprimir todas las especies ordenadas numéricamente?

## Plantilla C# a Completar

```csharp
using System;

public class NodoEspecie
{
    public int Codigo;
    public NodoEspecie Izquierdo;
    public NodoEspecie Derecho;

    public NodoEspecie(int codigo)
    {
        Codigo = codigo;
    }
}

public class SolucionArboles
{
    private NodoEspecie raiz;

    public void Insertar(int codigo)
    {
        raiz = InsertarRec(raiz, codigo);
    }

    private NodoEspecie InsertarRec(NodoEspecie nodo, int codigo)
    {
        if (nodo == null) return new NodoEspecie(codigo);

        if (codigo < nodo.Codigo)
            nodo.Izquierdo = InsertarRec(nodo.Izquierdo, codigo);
        else if (codigo > nodo.Codigo)
            nodo.Derecho = InsertarRec(nodo.Derecho, codigo);

        return nodo;
    }

    // JUSTIFICACIÓN: Se usa recorrido In-Order para obtener la lista ordenada de menor a mayor
    public void ImprimirOrdenado(NodoEspecie nodo)
    {
        if (nodo != null)
        {
            ImprimirOrdenado(nodo.Izquierdo);
            Console.Write(nodo.Codigo + " ");
            ImprimirOrdenado(nodo.Derecho);
        }
    }

    static void Main()
    {
        SolucionArboles catalogo = new SolucionArboles();
        int[] codigos = new int[] { 500, 300, 800, 200, 400 };

        foreach (int c in codigos)
        {
            catalogo.Insertar(c);
        }

        Console.Write("Catálogo ordenado: ");
        catalogo.ImprimirOrdenado(catalogo.raiz);
        Console.WriteLine();
    }
}
```
