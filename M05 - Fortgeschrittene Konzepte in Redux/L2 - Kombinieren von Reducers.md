## Arbeiten mit mehreren Reducern in Redux

In Redux ist der Reducer eine reine Funktion, die den aktuellen Zustand und eine Aktion erhält und einen neuen Zustand zurückgibt. Bei größeren Anwendungen kann es jedoch unpraktisch sein, einen einzigen Reducer für den gesamten Zustand der Anwendung zu verwenden. Aus diesem Grund verwendet Redux die Technik der **Reducer-Komposition**, um mehrere Reducer zu kombinieren und so den Zustand effizient zu verwalten. Dieser Ansatz trägt zur Modularität und Übersichtlichkeit des Codes bei.

### Prinzipien der Reducer-Komposition

#### 1. Modularität

Durch die Verwendung mehrerer Reducer können Sie den Zustand in kleinere, überschaubare Teile aufteilen. Jeder Reducer ist für einen bestimmten Teil des Zustands verantwortlich und kümmert sich nur um die relevanten Aktionen.

- **Beispiel**: Eine Anwendung mit `todos`, `user` und `settings` könnte separate Reducer für jede dieser Teile haben:

```javascript
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.payload];
    default:
      return state;
  }
};

const userReducer = (state = null, action) => {
  switch (action.type) {
    case 'SET_USER':
      return action.payload;
    default:
      return state;
  }
};

const settingsReducer = (state = {}, action) => {
  switch (action.type) {
    case 'UPDATE_SETTING':
      return { ...state, [action.payload.key]: action.payload.value };
    default:
      return state;
  }
};
```

#### 2. Kombinieren von Reducern
Redux bietet die Funktion combineReducers aus dem redux-Paket, um mehrere Reducer zu einem einzelnen Root-Reducer zu kombinieren. Dieser Root-Reducer wird dann verwendet, um den gesamten Zustand der Anwendung zu verwalten.

Beispiel: Die combineReducers-Funktion kombiniert die oben definierten Reducer:


```javascript
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
  todos: todosReducer,
  user: userReducer,
  settings: settingsReducer
});

export default rootReducer;

```

In diesem Beispiel enthält der Root-Reducer drei Teilzustände (todos, user, settings), die jeweils von den entsprechenden Reducern verwaltet werden.

#### 3. Zugriff auf den Zustand
Nach der Kombination der Reducer können Sie im gesamten Zustand der Anwendung auf die einzelnen Teile zugreifen. Der Zustand der Anwendung wird in einem verschachtelten Objekt gespeichert, wobei jeder Schlüssel den Teilzustand eines Reducers darstellt.

Beispiel: Der Zustand könnte folgendermaßen aussehen:


```javascript
{
  todos: [{ id: 1, text: 'Learn Redux', completed: false }],
  user: { id: 1, name: 'Alice' },
  settings: { theme: 'dark' }
}

```

### Integration in die Anwendung
1. Erstellen des Stores
Verwenden Sie den kombinierten Root-Reducer, um den Redux-Store zu erstellen:

```javascript
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);

```

2. Dispatchen von Aktionen
Um den Zustand zu aktualisieren, verwenden Sie store.dispatch() und übergeben Sie Aktionen an die verschiedenen Reducer:

```javascript
store.dispatch({ type: 'ADD_TODO', payload: { id: 2, text: 'Read documentation', completed: false } });
store.dispatch({ type: 'SET_USER', payload: { id: 2, name: 'Bob' } });
store.dispatch({ type: 'UPDATE_SETTING', payload: { key: 'theme', value: 'light' } });
```