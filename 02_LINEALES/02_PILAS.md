# Pilas (Stack<T>)

La pila es una estructura de datos lineal que sigue el principio LIFO (Last In, First Out). Esto significa que el ultimo elemento en ingresar es estrictamente el primero en salir.

Como vimos en los conceptos generales, la analogia mas clara es la de una pila de platos: el ultimo plato que colocamos arriba es el primero que tomamos para lavar.

## Como funciona internamente una Pila

Una pila restringe el acceso a un unico punto llamado tope (top). No podemos modificar ni retirar elementos que esten debajo del tope sin retirar primero los que se encuentran encima.

### Operacion Push (Insertar)

```
Estado inicial de la pila con 2 elementos:

|           |
|  Dato B   | <-- Tope
|  Dato A   |
+-----------+

Ejecutamos Push(Dato C):

|  Dato C   | <-- Nuevo Tope
|  Dato B   |
|  Dato A   |
+-----------+

El nuevo elemento pasa a ser el tope de la pila en tiempo O(1).
```

### Operacion Pop (Extraer)

```
Estado inicial con 3 elementos:

|  Dato C   | <-- Tope
|  Dato B   |
|  Dato A   |
+-----------+

Ejecutamos Pop():

Retorna Dato C y la pila queda:

|  Dato B   | <-- Nuevo Tope
|  Dato A   |
+-----------+

Extraer el elemento del tope se realiza en tiempo O(1).
```

## Cuando usar una Pila en Ciencia de Datos

Elegimos una pila cuando el problema requiere revertir un flujo de eventos o inspeccionar hacia atras la ultima accion realizada:

1. Historial de modificaciones en datos: Si construimos una herramienta para la limpieza interactiva de datasets y el usuario aplica filtros o imputaciones, usamos una pila para permitir deshacer (undo) la ultima transformacion efectuada.

2. Verificacion de balanceo de parentesis o parsing de expresiones: Al evaluar formulas matematicas o validar esquemas sintacticos (como archivos JSON o queries SQL), usamos una pila para verificar que cada parentesis que abre tenga su correspondiente parentesis de cierre en el orden correcto.

## Ejemplo en C#

```csharp
using System;
using System.Collections.Generic;

public class EjemploPila
{
    static bool ValidarParentesis(string expresion)
    {
        Stack<char> pila = new Stack<char>();

        foreach (char c in expresion)
        {
            if (c == '(' || c == '[' || c == '{')
            {
                pila.Push(c);
            }
            else if (c == ')' || c == ']' || c == '}')
            {
                if (pila.Count == 0) return false;

                char tope = pila.Pop();
                if ((c == ')' && tope != '(') ||
                    (c == ']' && tope != '[') ||
                    (c == '}' && tope != '{'))
                {
                    return false;
                }
            }
        }

        return pila.Count == 0;
    }

    static void Main()
    {
        // Validacion de formulas matematicas
        string formulaCorrecta = "SUM(A1:A10) * [10 + (2 - 1)]";
        string formulaIncorrecta = "AVG(A1:A10) * [10 + 2 - 1)]";

        Console.WriteLine("Formula 1 es valida: " + ValidarParentesis(formulaCorrecta));
        Console.WriteLine("Formula 2 es valida: " + ValidarParentesis(formulaIncorrecta));

        // Ejemplo de historial de deshacer (Undo)
        Stack<string> historialAcciones = new Stack<string>();
        historialAcciones.Push("Cargar archivo dataset.csv");
        historialAcciones.Push("Eliminar filas con nulos");
        historialAcciones.Push("Normalizar columna Edad");

        Console.WriteLine("\nUltima accion realizada: " + historialAcciones.Peek());

        // El usuario presiona Deshacer
        string accionDeshecha = historialAcciones.Pop();
        Console.WriteLine("Se deshizo la accion: " + accionDeshecha);
        Console.WriteLine("Accion activa actual: " + historialAcciones.Peek());
    }
}
```
