# Pilas (Stack<T>)

La pila es una estructura de datos lineal que sigue el principio LIFO (Last In, First Out). Esto significa que el último elemento en ingresar es estrictamente el primero en salir.

Como vimos en los conceptos generales, la analogía más clara es la de una pila de platos: el último plato que colocas arriba es el primero que tomas para lavar.

## Cómo Funciona Internamente una Pila

Una pila restringe el acceso a un único punto llamado tope (top). No puedes modificar ni retirar elementos que estén debajo del tope sin retirar primero los que se encuentran encima.

### Operación Push (Insertar)

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

### Operación Pop (Extraer)

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

## Cuándo Usar una Pila en Ingeniería de Ciencia de Datos

Elegirás una pila cuando el problema requiera revertir un flujo de eventos o inspeccionar hacia atrás la última acción realizada:

1. Historial de modificaciones en datos: Si construyes una herramienta para la limpieza interactiva de datasets y el usuario aplica filtros o imputaciones, usas una pila para permitir deshacer (undo) la última transformación efectuada.

2. Verificación de balanceo de paréntesis o parsing de expresiones: Al evaluar fórmulas matemáticas o validar esquemas sintácticos (como archivos JSON o consultas SQL), usas una pila para verificar que cada paréntesis que abre tenga su correspondiente paréntesis de cierre en el orden correcto.

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
        // Validación de fórmulas matemáticas
        string formulaCorrecta = "SUM(A1:A10) * [10 + (2 - 1)]";
        string formulaIncorrecta = "AVG(A1:A10) * [10 + 2 - 1)]";

        Console.WriteLine("Fórmula 1 es válida: " + ValidarParentesis(formulaCorrecta));
        Console.WriteLine("Fórmula 2 es válida: " + ValidarParentesis(formulaIncorrecta));

        // Ejemplo de historial de deshacer (Undo)
        Stack<string> historialAcciones = new Stack<string>();
        historialAcciones.Push("Cargar archivo dataset.csv");
        historialAcciones.Push("Eliminar filas con nulos");
        historialAcciones.Push("Normalizar columna Edad");

        Console.WriteLine("\nÚltima acción realizada: " + historialAcciones.Peek());

        // El usuario presiona Deshacer
        string accionDeshecha = historialAcciones.Pop();
        Console.WriteLine("Se deshizo la acción: " + accionDeshecha);
        Console.WriteLine("Acción activa actual: " + historialAcciones.Peek());
    }
}
```
