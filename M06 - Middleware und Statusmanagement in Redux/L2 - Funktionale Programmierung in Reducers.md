## Funktionale Programmierung in Reducers

In Redux spielen Reducer eine zentrale Rolle bei der Zustandsverwaltung. Reducer sind Funktionen, die den aktuellen Zustand und eine Aktion als Eingaben nehmen und einen neuen Zustand zurückgeben. Die funktionale Programmierung bietet wertvolle Prinzipien und Techniken, die helfen können, Reducer effizient und robust zu gestalten. Im Folgenden werden die wichtigsten Aspekte der funktionalen Programmierung in Reducern erläutert.

### Grundprinzipien der Funktionalen Programmierung

Funktionale Programmierung basiert auf einigen wesentlichen Prinzipien, die auch in der Implementierung von Reducern angewendet werden können:

#### 1. Unveränderlichkeit

In der funktionalen Programmierung wird der Zustand nicht direkt verändert. Stattdessen wird ein neuer Zustand basierend auf dem alten Zustand und der Aktion erstellt.

- **Vorteil**: Dies vermeidet Nebenwirkungen und macht den Code leichter nachvollziehbar und testbar.

- **Beispiel**: Anstatt den Zustand direkt zu ändern, wird ein neuer Zustand erstellt, indem der alte Zustand unverändert bleibt:

```javascript
const initialState = {
  todos: []
};

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state, // Zustand bleibt unverändert
        todos: [...state.todos, action.payload] // Neuer Zustand wird erstellt
      };
    default:
      return state;
  }
}
```

#### 2. Reine Funktionen
Reducer sind reine Funktionen, was bedeutet, dass sie keine Nebenwirkungen haben und immer dasselbe Ergebnis für dieselben Eingaben liefern.

- **Vorteil**: Reine Funktionen sind leicht zu testen und zu debuggen, da sie keine unerwarteten Änderungen oder Effekte verursachen.

- **Beispiel**: Ein Reducer, der eine Aufgabe hinzufügt, ist rein, da er nur den Zustand basierend auf der Aktion berechnet und keinen externen Zustand beeinflusst:

```javascript
function addTodo(state, todo) {
  return {
    ...state,
    todos: [...state.todos, todo]
  };
}

```

#### 3. Komposition
Funktionale Programmierung fördert die Komposition von Funktionen. In Reducern können Funktionen zur Bearbeitung von Aktionen oder Zuständen komposiert werden, um komplexe Logik zu kapseln und wiederverwendbar zu machen.

- **Vorteil**: Durch die Komposition von Funktionen wird der Code modular und wiederverwendbar.

- **Beispiel**: Separate Funktionen für das Hinzufügen und Entfernen von Aufgaben können in einem Reducer kombiniert werden:

```javascript
function handleAddTodo(state, action) {
  return {
    ...state,
    todos: [...state.todos, action.payload]
  };
}

function handleRemoveTodo(state, action) {
  return {
    ...state,
    todos: state.todos.filter(todo => todo.id !== action.payload.id)
  };
}

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return handleAddTodo(state, action);
    case 'REMOVE_TODO':
      return handleRemoveTodo(state, action);
    default:
      return state;
  }
}

```

4. Immutability Helpers
Bei der Arbeit mit verschachtelten Zuständen kann die Unveränderlichkeit durch spezielle Hilfsfunktionen oder Bibliotheken unterstützt werden, die dabei helfen, tief verschachtelte Strukturen zu kopieren und zu aktualisieren.

- **Beispiel**: Bibliotheken wie immutability-helper oder die native Spread-Syntax können verwendet werden, um unveränderliche Updates durchzuführen:

```javascript
import update from 'immutability-helper';

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case 'UPDATE_TODO':
      return update(state, {
        todos: {
          [action.index]: {
            text: { $set: action.payload.text }
          }
        }
      });
    default:
      return state;
  }
}

```