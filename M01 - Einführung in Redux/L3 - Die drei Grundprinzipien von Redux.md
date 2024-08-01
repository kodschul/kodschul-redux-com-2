## Die drei Grundprinzipien von Redux

Redux ist ein vorhersehbarer Zustandscontainer für JavaScript-Anwendungen. Es hilft, Anwendungen konsistent zu gestalten und in verschiedenen Umgebungen (Client, Server, native Apps) auszuführen und dabei das Debugging und Testen zu erleichtern. Redux basiert auf drei zentralen Prinzipien, die seine Architektur und Funktionsweise prägen:

### 1. Ein einziger Store

Im Gegensatz zu anderen State-Management-Lösungen verwendet Redux einen einzigen Store, um den gesamten Zustand der Anwendung zu speichern. Dieser Store ist eine einzige Quelle der Wahrheit (Single Source of Truth).

- **Vorteil**: Durch die zentrale Speicherung des Zustands wird die Synchronisation zwischen verschiedenen Teilen der Anwendung vereinfacht und es entsteht ein klarer Überblick über den gesamten Anwendungszustand.

- **Beispiel**: Der Zustand einer To-Do-App mit den aktuellen Aufgaben und deren Status wird in einem einzigen Store verwaltet.

```javascript
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);
```

### 2. Zustand ist unveränderlich und wird durch Aktionen verändert
Der einzige Weg, den Zustand zu ändern, besteht darin, eine Aktion (Action) zu senden. Eine Aktion ist ein einfaches JavaScript-Objekt, das eine Beschreibung der Zustandsänderung enthält.

- **Vorteil**: Durch die Verwendung von Aktionen wird jede Zustandsänderung explizit und nachvollziehbar. Dies erleichtert das Debuggen und Verstehen der Anwendung.

- **Beispiel**: Eine Aktion zum Hinzufügen einer Aufgabe in einer To-Do-App könnte so aussehen:

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

### 3. Änderungen werden durch reine Funktionen (Reducer) beschrieben
Reducer sind reine Funktionen, die den aktuellen Zustand und eine Aktion als Argumente nehmen und einen neuen Zustand zurückgeben. Sie beschreiben, wie eine Aktion den Zustand verändert, ohne den alten Zustand zu mutieren.

- **Vorteil**: Reine Funktionen sind vorhersehbar und einfach zu testen. Sie garantieren, dass die Zustandsänderungen deterministisch sind und keine Nebenwirkungen haben.

- **Beispiel**: Ein Reducer für die To-Do-App könnte so aussehen:

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