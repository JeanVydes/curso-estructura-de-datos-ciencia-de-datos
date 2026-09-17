# Árboles y Búsqueda Jerárquica

A diferencia de las estructuras lineales como listas, pilas o colas, los árboles son estructuras de datos no lineales que organizan los elementos en forma de jerarquía.

Se componen de nodos conectados por ramas, donde existe un único nodo principal llamado raíz y cada nodo puede tener nodos hijos.

## Cómo Funciona un Árbol Binario de Búsqueda (BST)

Un Árbol Binario de Búsqueda impone una regla estricta conocida como el invariante de orden de un BST:
Para cualquier nodo N del árbol, todos los valores en su subárbol izquierdo son estrictamente menores que N, y todos los valores en su subárbol derecho son estrictamente mayores que N.

### Insertar Elementos Paso a Paso

Veamos cómo se construye un BST al insertar los valores 50, 30 y 70 en ese orden:

```
Paso 1: Insertar 50. Como el árbol está vacío, 50 se convierte en la raíz.

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

### Proceso de Búsqueda de un Valor (Ejemplo: Buscar 40)

```
Paso 1: Comparamos 40 con la raíz (50). Como 40 < 50, descartamos todo el subárbol derecho (70) y vamos a la izquierda.
Paso 2: Comparamos 40 con el nodo actual (30). Como 40 > 30, descartamos el subárbol izquierdo de 30 y vamos a la derecha.
Paso 3: Llegamos al nodo (40). Como 40 == 40, hemos encontrado el elemento.

En cada paso eliminamos la mitad de las opciones restantes. En un árbol balanceado esto nos da un tiempo de búsqueda de O(log n).
```

## Cuándo Usar un Árbol en Ingeniería de Ciencia de Datos

1. Indexación de búsquedas rápidas: Las bases de datos relacionales utilizan estructuras basadas en árboles para permitir encontrar registros en tiempo logarítmico O(log n) sobre millones de filas.

2. Modelos de Aprendizaje Automático: Algoritmos como los Árboles de Decisión dividen el espacio de características mediante preguntas binarias en cada nodo.

3. Estructuras de taxonomías y jerarquías de categorías: Para representar categorías y subcategorías de productos o clasificaciones taxonómicas.

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
