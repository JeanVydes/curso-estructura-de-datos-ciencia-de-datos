# Arboles y Busqueda Jerarquica

A diferencia de las estructuras lineales como listas, pilas o colas, los arboles son estructuras de datos no lineales que organizan los elementos en forma de jerarquia.

Se componen de nodos conectados por ramas, donde existe un unico nodo principal llamado raiz y cada nodo puede tener nodos hijos.

## Como funciona un Arbol Binario de Busqueda (BST)

Un Arbol Binario de Busqueda impone una regla estricta conocida como el invariante de orden de un BST:
Para cualquier nodo N del arbol, todos los valores en su subarbol izquierdo son estrictamente menores que N, y todos los valores en su subarbol derecho son estrictamente mayores que N.

### Insertar elementos paso a paso

Veamos como se construye un BST al insertar los valores 50, 30 y 70 en ese orden:

```
Paso 1: Insertar 50. Como el arbol esta vacio, 50 se convierte en la raiz.

       ( 50 )

Paso 2: Insertar 30. Comparamos 30 < 50, por lo que va a la izquierda de 50.

       ( 50 )
       /
    ( 30 )

Paso 3: Insertar 70. Comparamos 70 > 50, por lo que va a la derecha de 50.

       ( 50 )
       /    \
    ( 30 )  ( 70 )

Paso 4: Insertar 40. Comparamos 40 < 50 (izquierda), luego 40 > 30 (derecha).

       ( 50 )
       /    \
    ( 30 )  ( 70 )
       \
       ( 40 )
```

### Proceso de Busqueda de un valor (ejemplo buscar 40)

```
Paso 1: Comparamos 40 con la raiz (50). Como 40 < 50, descartamos todo el subarbol derecho (70) y vamos a la izquierda.
Paso 2: Comparamos 40 con el nodo actual (30). Como 40 > 30, descartamos el subarbol izquierdo de 30 y vamos a la derecha.
Paso 3: Llegamos al nodo (40). Como 40 == 40, hemos encontrado el elemento.

En cada paso eliminamos la mitad de las opciones restantes. En un arbol balanceado esto nos da un tiempo de busqueda de O(log n).
```

## Cuando usar un Arbol en Ciencia de Datos

1. Indexacion de busquedas rapidas: Las bases de datos relacionales utilizan estructuras basadas en arboles para permitir encontrar registros en tiempo logaritmico O(log n) sobre millones de filas.

2. Modelos de Machine Learning: Algoritmos como los Arboles de Decision dividen el espacio de caracteristicas mediante preguntas binarias en cada nodo.

3. Estructuras de taxonomias y jerarquias de categorias: Para representar categorias y subcategorias de productos o clasificaciones taxonomicas.

## Ejemplo en C#

```csharp
using System;

public class NodoArbol
{
    public int Valor;
    public NodoArbol Izquierdo;
    public NodoArbol Derecho;

    public NodoArbol(int valor)
    {
        Valor = valor;
        Izquierdo = null;
        Derecho = null;
    }
}

public class ArbolBinarioBusqueda
{
    public NodoArbol Raiz;

    public void Insertar(int valor)
    {
        Raiz = InsertarRecursivo(Raiz, valor);
    }

    private NodoArbol InsertarRecursivo(NodoArbol nodo, int valor)
    {
        if (nodo == null) return new NodoArbol(valor);

        if (valor < nodo.Valor)
            nodo.Izquierdo = InsertarRecursivo(nodo.Izquierdo, valor);
        else if (valor > nodo.Valor)
            nodo.Derecho = InsertarRecursivo(nodo.Derecho, valor);

        return nodo;
    }

    public bool Buscar(int valorBuscado)
    {
        return BuscarRecursivo(Raiz, valorBuscado);
    }

    private bool BuscarRecursivo(NodoArbol nodo, int valorBuscado)
    {
        if (nodo == null) return false;
        if (nodo.Valor == valorBuscado) return true;

        if (valorBuscado < nodo.Valor)
            return BuscarRecursivo(nodo.Izquierdo, valorBuscado);

        return BuscarRecursivo(nodo.Derecho, valorBuscado);
    }

    // Recorrido In-Order imprime los datos ordenados
    public void ImprimirInOrder(NodoArbol nodo)
    {
        if (nodo != null)
        {
            ImprimirInOrder(nodo.Izquierdo);
            Console.Write(nodo.Valor + " ");
            ImprimirInOrder(nodo.Derecho);
        }
    }
}

public class EjemploArboles
{
    static void Main()
    {
        ArbolBinarioBusqueda bst = new ArbolBinarioBusqueda();
        bst.Insertar(50);
        bst.Insertar(30);
        bst.Insertar(70);
        bst.Insertar(40);

        Console.Write("Datos ordenados (In-Order): ");
        bst.ImprimirInOrder(bst.Raiz);
        Console.WriteLine();

        Console.WriteLine("¿Existe el valor 40?: " + bst.Buscar(40));
        Console.WriteLine("¿Existe el valor 99?: " + bst.Buscar(99));
    }
}
```
