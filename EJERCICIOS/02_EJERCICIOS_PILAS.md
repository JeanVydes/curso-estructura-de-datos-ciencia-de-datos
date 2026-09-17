# Ejercicios de Pilas (Stack<T>)

En este segundo bloque trabajaremos con el comportamiento LIFO (Last In, First Out). Tu objetivo es reconocer situaciones donde el orden inverso de procesamiento es indispensable.

## Caso 2: Sistema de Deshacer (Undo) en Encuestas Agricolas de la Zona Bananera

Un grupo de encuestadores registra informacion sobre el estado de los cultivos de banano en la Zona Bananera del Magdalena mediante una aplicacion movil.

Durante la recoleccion de datos en campo, los encuestadores suelen cometer errores de ingreso y necesitan una funcion de Deshacer (Undo) que revierta siempre la ultima modificacion realizada.

```
Historial de modificaciones sobre el registro de una finca:

Paso 1: Se ingresa "Finca El Recreo: 50 hectareas"
Paso 2: Se modifica a "Finca El Recreo: 55 hectareas"
Paso 3: Se modifica a "Finca El Recreo: 60 hectareas" (Error de digitacion)

Al presionar el boton Deshacer:
El sistema debe eliminar "60 hectareas" y restaurar "55 hectareas" en tiempo O(1).
```

## Caso Adicional: Validacion Sintactica de Consultas de Filtro

La aplicacion permite a los analistas escribir filtros mediante expresiones con parentesis y corchetes, como por ejemplo: `(Hectareas > 50 AND [Riego == "Goteo"])`.

Necesitamos un modulo que verifique si los parentesis y corchetes de la formula estan correctamente balanceados antes de ejecutar la consulta sobre la base de datos.

## Preguntas de Racionalizacion

1. Explica la propiedad LIFO de la clase Stack<T> en C# y por que es la estructura idonea para implementar el historial de deshacer frente a una lista convencional.

2. ¿Cual es la diferencia entre el metodo Pop() y el metodo Peek() en una pila? Dibuja un diagrama ASCII mostrando la pila antes y despues de ejecutar cada operacion.

```
Diagrama ASCII a completar:

Pila Inicial:
|  Cambio C  | <-- Tope
|  Cambio B  |
|  Cambio A  |
+------------+

Despues de Peek():                   Despues de Pop():
|     ?      |                       |     ?      |
+------------+                       +------------+
```

3. Al validar la sintaxis de parentesis, ¿que ocurre en la pila cuando encontramos un caracter de apertura '(' frente a uno de cierre ')'?

## Plantilla C# a Completar

```csharp
using System;
using System.Collections.Generic;

public class SolucionPilas
{
    // Completar el algoritmo de validacion de parentesis usando Stack<char>
    static bool ValidarSintaxis(string formula)
    {
        // JUSTIFICACION: Se usa Stack<char> porque los parentesis se deben cerrar en orden inverso...
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
        historialUndo.Pop(); // Deshacer ultimo
        Console.WriteLine("Estado despues de deshacer: " + historialUndo.Peek());

        string consulta = "(Hectareas > 50 AND [Riego == 'Goteo'])";
        Console.WriteLine("Consulta valida: " + ValidarSintaxis(consulta));
    }
}
```
