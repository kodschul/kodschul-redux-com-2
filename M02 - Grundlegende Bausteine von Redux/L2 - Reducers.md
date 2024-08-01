## Redux Schulung: Reducers - Reine Funktionen für den Zustand

Reducers sind ein zentrales Konzept in Redux, das hilft, den Zustand einer Anwendung vorhersehbar und verwaltbar zu halten. Ein Verständnis für Reducers und deren Funktionsweise ist entscheidend für die effektive Nutzung von Redux.

### Was sind Reducers?

Reducers sind pure (reine) Funktionen, die den aktuellen Zustand und eine Aktion (Action) als Argumente akzeptieren und einen neuen Zustand zurückgeben. Sie beschreiben, wie der Zustand der Anwendung auf Basis von Aktionen verändert wird.

#### Eigenschaften von Reducers:

- **Pure Functions**: Ein Reducer ist eine reine Funktion, was bedeutet, dass der Rückgabewert nur von den Eingabewerten abhängt und keine Nebenwirkungen erzeugt.
- **Unveränderlichkeit**: Reducers mutieren den bestehenden Zustand nicht, sondern geben einen neuen Zustand zurück.

### Struktur eines Reducers

Ein Reducer nimmt zwei Parameter entgegen:

1. Den aktuellen Zustand (`state`)
2. Eine Aktion (`action`)

Der Reducer verwendet diese, um basierend auf dem Aktionstyp einen neuen Zustand zu erzeugen.

#### Beispiel eines einfachen Reducers:

```javascript
const initialState = {
  count: 0
};

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case 'INCREMENT':
      return {
        ...state,
        count: state.count + 1
      };
    case 'DECREMENT':
      return {
        ...state,
        count: state.count - 1
      };
    default:
      return state;
  }
}
```
In diesem Beispiel hat der Reducer zwei Aktionsarten: INCREMENT und DECREMENT, die den Zählerstand entsprechend erhöhen oder verringern.

### Eigenschaften von reinen Funktionen
Reducers müssen folgende Eigenschaften reiner Funktionen einhalten:

Keine Seiteneffekte: Ein Reducer darf keine Änderungen außerhalb seines Scopes vornehmen. Er sollte keine Daten mutieren, keine API-Aufrufe durchführen oder den Zustand auf andere Weise beeinflussen.
Deterministisch: Für dieselben Eingabewerte (aktueller Zustand und Aktion) sollte ein Reducer immer denselben Ausgabewert (neuen Zustand) zurückgeben.

Beispiel einer reinen Funktion:

```javascript
function add(a, b) {
  return a + b;
}

```

Beispiel einer Funktion mit Seiteneffekten (nicht rein):

```javascript
function addWithLogging(a, b) {
  console.log(a + b);
  return a + b;
}
```

### Kombinieren von Reducers
In komplexen Anwendungen können Sie mehrere Reducers verwenden, um verschiedene Teile des Zustands zu verwalten. Redux bietet die combineReducers-Funktion, um diese Reducers zu einem Haupt-Reducer zusammenzuführen.

Beispiel der Kombination von Reducers:

```javascript
import { combineReducers } from 'redux';

const userReducer = (state = {}, action) => {
  switch (action.type) {
    case 'SET_USER':
      return action.payload;
    default:
      return state;
  }
};

const postsReducer = (state = [], action) => {
  switch (action.type) {
    case 'ADD_POST':
      return [...state, action.payload];
    default:
      return state;
  }
};

const rootReducer = combineReducers({
  user: userReducer,
  posts: postsReducer
});

export default rootReducer;

```

In diesem Beispiel verwaltet userReducer den Benutzerzustand und postsReducer den Zustand der Posts. combineReducers erstellt einen Haupt-Reducer, der beide Zustände kombiniert.