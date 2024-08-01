## Redux mit React verbinden: Erste Schritte

Redux ist ein leistungsstarkes Tool zur Zustandsverwaltung in JavaScript-Anwendungen. In Kombination mit React kann es helfen, den Zustand Ihrer Anwendung zentral zu verwalten und vorhersehbar zu machen. Diese Anleitung führt Sie durch die ersten Schritte, um Redux mit React zu verbinden.

### Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie Node.js und npm installiert haben. Sie sollten auch mit den Grundlagen von React und Redux vertraut sein.

### Schritt 1: Installation von Redux und React-Redux

Zunächst müssen Sie Redux und das React-Redux-Bindungspaket installieren. Dies geschieht mit npm oder yarn.

```bash
npm install redux react-redux
```

### Schritt 2: Einrichten des Redux Stores
Erstellen Sie einen Store, der den Zustand Ihrer Anwendung verwaltet. Der Store wird durch den createStore-Funktion von Redux erstellt und mit einem Reducer konfiguriert.

```js
// src/store.js
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);

export default store;

```

### Schritt 3: Erstellen eines Root Reducers
Reducer sind Funktionen, die den Zustand Ihrer Anwendung basierend auf den erhaltenen Aktionen verändern. Sie kombinieren mehrere Reducer zu einem Root Reducer.

```js
// src/reducers/index.js
import { combineReducers } from 'redux';
import todoReducer from './todoReducer';

const rootReducer = combineReducers({
  todos: todoReducer
});

export default rootReducer;

```

### Schritt 4: Definieren von Actions und Reducern
Definieren Sie Actions und den dazugehörigen Reducer, um den Zustand der Anwendung zu verwalten.

```js
// src/actions/todoActions.js
export const addTodo = (todo) => ({
  type: 'ADD_TODO',
  payload: todo
});
```


```js
// src/reducers/todoReducer.js
const initialState = {
  todos: []
};

const todoReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    default:
      return state;
  }
};

export default todoReducer;

```

### Schritt 5: Verbinden von React mit Redux
Verwenden Sie den Provider-Komponenten von react-redux, um den Store Ihrer Anwendung zur Verfügung zu stellen. Dadurch können alle Komponenten auf den Store zugreifen.

```js
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```

### Schritt 6: Verwenden von connect zum Verbinden von Komponenten
Verwenden Sie die connect-Funktion von react-redux, um Komponenten mit dem Redux-Store zu verbinden. Dies ermöglicht es Ihnen, den Zustand und die Actions als Props zu verwenden.

```js
// src/components/TodoList.js
import React from 'react';
import { connect } from 'react-redux';
import { addTodo } from '../actions/todoActions';

const TodoList = ({ todos, addTodo }) => {
  const handleAddTodo = () => {
    const todo = { id: todos.length + 1, text: 'New Todo' };
    addTodo(todo);
  };

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
      <button onClick={handleAddTodo}>Add Todo</button>
    </div>
  );
};

const mapStateToProps = (state) => ({
  todos: state.todos.todos
});

const mapDispatchToProps = {
  addTodo
};

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);

```
