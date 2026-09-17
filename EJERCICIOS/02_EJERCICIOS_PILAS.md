# Ejercicios de Pilas (Stack<T>)

En este segundo bloque para Ingeniería de Ciencia de Datos trabajarás con el comportamiento LIFO (Last In, First Out). Tu objetivo es reconocer situaciones donde el orden inverso de procesamiento es indispensable.

## Caso 2: Sistema de Deshacer (Undo) en Encuestas Agrícolas de la Zona Bananera

Un grupo de encuestadores registra información sobre el estado de los cultivos de banano en la Zona Bananera del Magdalena mediante una aplicación móvil.

Durante la recolección de datos en campo, los encuestadores suelen cometer errores de digitación y necesitan una función de Deshacer (Undo) que revierta siempre la última modificación realizada.

```
Historial de modificaciones sobre el registro de una finca:

Paso 1: Se ingresa "Finca El Recreo: 50 hectáreas"
Paso 2: Se modifica a "Finca El Recreo: 55 hectáreas"
Paso 3: Se modifica a "Finca El Recreo: 60 hectáreas" (Error de digitación)

Al presionar el botón Deshacer:
El sistema debe eliminar "60 hectáreas" y restaurar "55 hectáreas" en tiempo O(1).
```

## Caso Adicional: Validación Sintáctica de Consultas de Filtro

La aplicación permite a los analistas escribir filtros mediante expresiones con paréntesis y corchetes, por ejemplo: `(Hectareas > 50 AND [Riego == "Goteo"])`.

Necesitamos un módulo que verifique si los paréntesis y corchetes de la fórmula están correctamente balanceados antes de ejecutar la consulta sobre la base de datos.

## Preguntas de Racionalización

1. Explica la propiedad LIFO de la clase Stack<T> en C# y por qué es la estructura idónea para implementar el historial de deshacer frente a una lista convencional.

2. ¿Cuál es la diferencia entre el método Pop() y el método Peek() en una pila? Dibuja un esquema ASCII mostrando la pila antes y después de ejecutar cada operación.

```
Esquema ASCII:

Pila Inicial:
|  Cambio C  | <-- Tope
|  Cambio B  |
|  Cambio A  |
+------------+

Después de Peek():                  Después de Pop():
|  Cambio C  | <-- Permanece        |  Cambio B  | <-- Nuevo Tope
|  Cambio B  |                      |  Cambio A  |
|  Cambio A  |                      +------------+
+------------+
```

3. Al validar la sintaxis de paréntesis, ¿qué ocurre en la pila cuando encuentras un carácter de apertura '(' frente a uno de cierre ')'?

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionPilas
{
    static bool ValidarSintaxis(string formula)
    {
        // JUSTIFICACIÓN: Se usa Stack<char> porque los paréntesis se deben cerrar en orden inverso LIFO
        Stack<char> pila = new Stack<char>();

        foreach (char c in formula)
        {
            if (c == '(' || c == '[')
            {
                pila.Push(c);
            }
            else if (c == ')' || c == ']')
            {
                if (pila.Count == 0) return false;
                char ultimo = pila.Pop();
                if ((c == ')' && ultimo != '(') || (c == ']' && ultimo != '[')) return false;
            }
        }

        return pila.Count == 0;
    }

    static void Main()
    {
        Stack<string> historialUndo = new Stack<string>();
        historialUndo.Push("Ingreso 50 Ha");
        historialUndo.Push("Ingreso 55 Ha");
        historialUndo.Push("Ingreso 60 Ha");

        Console.WriteLine("Estado actual en tope: " + historialUndo.Peek());
        historialUndo.Pop(); // Deshacer último
        Console.WriteLine("Estado después de deshacer: " + historialUndo.Peek());

        string consulta = "(Hectareas > 50 AND [Riego == 'Goteo'])";
        Console.WriteLine("Consulta válida: " + ValidarSintaxis(consulta));
    }
}
```
