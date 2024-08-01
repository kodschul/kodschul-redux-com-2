## React-Redux: Einbindung leicht gemacht

React-Redux ist eine offizielle Bindings-Bibliothek, die React-Komponenten mit einem Redux-Store verbindet. Sie erleichtert die Integration von Redux in eine React-Anwendung und bietet eine effiziente Möglichkeit, den Anwendungszustand zu verwalten. Im Folgenden werden die grundlegenden Schritte und Konzepte für die Einbindung von React-Redux beschrieben:

### 1. Installation

Bevor Sie React-Redux in Ihrer Anwendung verwenden können, müssen Sie die erforderlichen Bibliotheken installieren. Dazu gehören `redux`, `react-redux`, und eventuell `@reduxjs/toolkit`.

```bash
npm install redux react-redux @reduxjs/toolkit
```

### 2. Einrichten des Redux Stores
Erstellen Sie den Redux-Store, indem Sie einen oder mehrere Reducer verwenden. Verwenden Sie den configureStore-Aufruf von @reduxjs/toolkit für eine einfache Konfiguration.

```js
import { configureStore } from '@reduxjs/toolkit';
import rootReducer from './reducers';

const store = configureStore({
  reducer: rootReducer,
});

export default store;

```

### 3. Bereitstellen des Stores
Um den Redux-Store in Ihrer React-Anwendung verfügbar zu machen, verwenden Sie den Provider-Komponenten, der den Store an alle Komponenten in Ihrer Anwendung weitergibt.

```js
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

### 4. Verbinden von Komponenten mit dem Store
Verwenden Sie die Hooks useSelector und useDispatch, um React-Komponenten mit dem Redux-Store zu verbinden. useSelector ermöglicht den Zugriff auf den Zustand, während useDispatch Aktionen an den Store sendet.

Beispiel: Verwenden von useSelector
Mit useSelector können Sie Teile des Zustands aus dem Redux-Store in einer Komponente auswählen.

```js
import React from 'react';
import { useSelector } from 'react-redux';

const TodoList = () => {
  const todos = useSelector((state) => state.todos);

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
};

export default TodoList;

```

#### Beispiel: Verwenden von useDispatch
Mit useDispatch können Sie Aktionen auslösen, um den Zustand im Redux-Store zu ändern.

```js
import React from 'react';
import { useDispatch } from 'react-redux';

const AddTodo = () => {
  const dispatch = useDispatch();

  const addTodo = () => {
    dispatch({
      type: 'ADD_TODO',
      payload: {
        id: new Date().getTime(),
        text: 'Neues Todo',
      },
    });
  };

  return <button onClick={addTodo}>Todo hinzufügen</button>;
};

export default AddTodo;

```

### 5. Kombinieren von Reducern
In einer größeren Anwendung haben Sie möglicherweise mehrere Reducer, die verschiedene Teile des Zustands verwalten. Verwenden Sie combineReducers, um diese Reducer zu einem Root-Reducer zu kombinieren.

```js
import { combineReducers } from '@reduxjs/toolkit';
import todosReducer from './todosReducer';
import userReducer from './userReducer';

const rootReducer = combineReducers({
  todos: todosReducer,
  user: userReducer,
});

export default rootReducer;

```

### 6. Middleware hinzufügen
Middleware in Redux bietet eine Möglichkeit, Aktionen zu erweitern oder abzufangen. Verwenden Sie die getDefaultMiddleware-Funktion von @reduxjs/toolkit, um Standard-Middleware hinzuzufügen, und fügen Sie bei Bedarf benutzerdefinierte Middleware hinzu.

```js
import { configureStore, getDefaultMiddleware } from '@reduxjs/toolkit';
import rootReducer from './reducers';
import logger from 'redux-logger';

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
});

export default store;

```