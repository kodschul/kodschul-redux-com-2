## Redux Schulung: Actions - Was sie sind und wie sie funktionieren

In Redux sind Actions ein zentraler Bestandteil des State-Management-Systems. Sie beschreiben Zustandsänderungen und sind die einzige Möglichkeit, den Zustand in Redux zu verändern. In dieser Schulung werden wir uns mit den Grundlagen von Actions beschäftigen, verstehen, was sie sind, wie sie funktionieren und wie man sie in einer Anwendung verwendet.

### Was sind Actions?

Actions sind einfache JavaScript-Objekte, die eine Beschreibung der Zustandsänderung enthalten. Jede Action muss eine `type` Eigenschaft haben, die den Typ der Action angibt. Darüber hinaus können Actions zusätzliche Daten enthalten, die für die Zustandsänderung notwendig sind.

- **Beispiel einer Action**: Eine Action zum Hinzufügen eines To-Do-Elements in einer To-Do-App.

```javascript
const addAction = {
  type: 'ADD_TODO',
  payload: {
    id: 1,
    text: 'Learn Redux',
    completed: false
  }
};
```

### Wie funktionieren Actions?
Actions in Redux durchlaufen einen standardisierten Lebenszyklus. Hier sind die Schritte, wie Actions in Redux funktionieren:

1. Erstellen einer Action
Eine Action wird erstellt, um eine bestimmte Zustandsänderung zu beschreiben. Dies geschieht normalerweise durch Action Creators, die Funktionen sind, die Actions erzeugen.

Beispiel eines Action Creators:

```javascript
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    payload: {
      id: new Date().getTime(),
      text: text,
      completed: false
    }
  };
}

```

2. Versenden einer Action
Eine Action wird an den Redux Store gesendet (dispatched). Dies veranlasst den Store, die entsprechenden Reducer aufzurufen, um den Zustand basierend auf der Action zu aktualisieren.

Beispiel des Dispatchens einer Action:

```javascript
store.dispatch(addTodo('Learn Redux'));

```

3. Handhaben einer Action im Reducer
Der Reducer ist eine reine Funktion, die den aktuellen Zustand und die Action als Argumente nimmt und einen neuen Zustand zurückgibt. Der Reducer enthält die Logik, wie der Zustand basierend auf der Action geändert wird.

Beispiel eines Reducers:

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