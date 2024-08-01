## Store: Das Herzstück von Redux

In Redux ist der Store das Herzstück des Zustandsmanagements. Er speichert den gesamten Zustand der Anwendung und bietet Methoden zum Lesen des Zustands, zum Senden von Aktionen und zum Abonnieren von Zustandsänderungen. In dieser Schulung erfahren Sie mehr über die Bedeutung des Stores, seine Methoden und wie er in einer Redux-Anwendung verwendet wird.

### Erstellung des Stores

Der Store wird mit der Funktion `createStore` erstellt, die den Root-Reducer als Argument nimmt. Der Root-Reducer ist eine Kombination aller Reducer der Anwendung.

```javascript
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);
```

### Zugriff auf den Zustand
Der Zustand der Anwendung kann mit der Methode getState abgerufen werden. Diese Methode gibt den aktuellen Zustand zurück.

```javascript
const currentState = store.getState();
console.log(currentState);
```

### Senden von Aktionen
Aktionen sind JavaScript-Objekte, die eine Beschreibung der Zustandsänderung enthalten. Aktionen werden mit der Methode dispatch an den Store gesendet. Der Store leitet die Aktion an den Reducer weiter, der dann den neuen Zustand berechnet.

```javascript
const addAction = {
  type: 'ADD_TODO',
  payload: {
    id: 1,
    text: 'Learn Redux',
    completed: false
  }
};

store.dispatch(addAction);

```

### Abonnieren von Zustandsänderungen
Die Methode subscribe ermöglicht es, einen Listener zu registrieren, der aufgerufen wird, wenn eine Zustandsänderung eintritt. Dies ist nützlich, um die Benutzeroberfläche zu aktualisieren oder andere Seiteneffekte auszulösen.

```javascript
const unsubscribe = store.subscribe(() => {
  console.log('State changed:', store.getState());
});

// Um das Abonnement zu beenden:
unsubscribe();

```

### Beispiel: Einfache To-Do-App
Hier ist ein vollständiges Beispiel, das die Erstellung eines Stores, das Senden von Aktionen und das Abonnieren von Zustandsänderungen zeigt.

1. Definieren des Reducers

```javascript
const initialState = {
  todos: []
};

function todoReducer(state = initialState, action) {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    default:
      return state;
  }
}

```

2. Erstellen des Stores

```javascript
import { createStore } from 'redux';

const store = createStore(todoReducer);
```

3. Senden einer Aktion

```javascript
const addAction = {
  type: 'ADD_TODO',
  payload: {
    id: 1,
    text: 'Learn Redux',
    completed: false
  }
};

store.dispatch(addAction);
```

4. Abonnieren von Zustandsänderungen

```javascript
const unsubscribe = store.subscribe(() => {
  console.log('State changed:', store.getState());
});
```