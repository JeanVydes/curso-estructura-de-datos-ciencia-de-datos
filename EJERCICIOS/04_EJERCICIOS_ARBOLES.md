# Ejercicios de Arboles y Busqueda Jerarquica

En este cuarto bloque trabajaremos con estructuras no lineales jerarquicas, especificamente el Arbol Binario de Busqueda (BST), analizando como mantener el invariante de orden y lograr busquedas logaritmicas O(log n).

## Caso 4: Indexacion de Especies Marinas en el Parque Nacional Natural Tayrona

El centro de investigacion marina del Parque Tayrona clasifica avistamientos de especies mediante un codigo numerico de catalogo.

Necesitamos almacenar 5,000 codigos de especies de forma que se puedan realizar busquedas inmediatas y, al mismo tiempo, imprimir el catalogo completo ordenado de menor a mayor sin necesidad de ordenar un arreglo desde cero.

```
Estructura del Arbol BST de especies:

                  ( 500 ) <-- Raiz
                 /       \
            ( 300 )     ( 800 )
            /     \
       ( 200 )   ( 400 )
```

## Preguntas de Racionalizacion

1. ¿Cual es el invariante fundamental de un Arbol Binario de Busqueda (BST) respecto a los nodos a la izquierda y derecha de cualquier nodo N?

2. Si insertamos los codigos en el siguiente orden: [500, 300, 800, 200, 400], dibuja paso a paso el diagrama ASCII del arbol resultante.

```
Diagrama ASCII a completar:

Paso 1 (500):       Paso 2 (300):       Paso 3 (800):
   (500)               (500)               (500)
                      /                   /     \
                    ( ? )               ( ? )   ( ? )
```

3. Si por el contrario insertamos los codigos ya ordenados [100, 200, 300, 400, 500] en un BST simple sin autobalanceo, ¿en que estructura se degrada el arbol y cual seria el tiempo de busqueda de un elemento?

4. ¿Que tipo de recorrido de arbol (Pre-Order, In-Order o Post-Order) debes utilizar para imprimir todas las especies ordenadas numericamente?

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

    // JUSTIFICACION: Se usa recorrido In-Order para obtener la lista ordenada de menor a mayor
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

        Console.Write("Catalogo ordenado: ");
        catalogo.ImprimirOrdenado(catalogo.raiz);
        Console.WriteLine();
    }
}
```
