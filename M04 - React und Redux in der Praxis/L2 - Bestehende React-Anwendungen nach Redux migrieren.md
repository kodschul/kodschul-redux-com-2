## Bestehende React-Anwendungen nach Redux migrieren

Das Migrieren einer bestehenden React-Anwendung zu Redux kann die Verwaltung des Anwendungszustands vereinfachen und die Skalierbarkeit verbessern. Redux bietet eine strukturierte Methode zur Verwaltung des Zustands und kann besonders nützlich werden, wenn eine Anwendung komplex wird. In dieser Schulung werden die wesentlichen Schritte und Überlegungen beschrieben, die bei der Migration einer bestehenden React-Anwendung zu Redux zu beachten sind.

### Schritte zur Migration

#### 1. Verstehen des aktuellen Zustandsmanagements

Bevor Sie mit der Migration beginnen, sollten Sie ein klares Verständnis davon haben, wie der Zustand aktuell in Ihrer Anwendung verwaltet wird. Dies umfasst:

- Identifizieren, wo der Zustand gespeichert und aktualisiert wird (z.B. in Komponentenstate, Context API, oder lokalen Speicher).
- Verstehen der aktuellen Datenflüsse und wie Komponenten miteinander kommunizieren.

#### 2. Installieren und Konfigurieren von Redux

Installieren Sie Redux und die benötigten Middleware-Pakete (falls erforderlich) in Ihrem Projekt.

```bash
npm install redux react-redux
```

Konfigurieren Sie den Redux-Store und integrieren Sie ihn in Ihre Anwendung.

```js
// store.js
import { createStore } from 'redux';
import rootReducer from './reducers';

const store = createStore(rootReducer);

export default store;

```

Fügen Sie den Provider von react-redux um Ihre Hauptkomponente, um den Store verfügbar zu machen.

```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import App from './App';
import store from './store';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```

#### 3. Erstellen von Actions und Reducern
Definieren Sie die Actions und Reducer, die für die Verwaltung des Zustands in Redux erforderlich sind.

Actions: Definieren Sie die Aktionen, die den Zustand verändern.

```js
// actions.js
export const ADD_TODO = 'ADD_TODO';

export const addTodo = (todo) => ({
  type: ADD_TODO,
  payload: todo
});

```

Reducer: Schreiben Sie die Reducer, die die Aktionen verarbeiten und den neuen Zustand berechnen.

```js
// store.js
// reducer.js
import { ADD_TODO } from './actions';

const initialState = {
  todos: []
};

const todoReducer = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
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

#### 4. Verbinden von Komponenten mit Redux
Verwenden Sie die connect-Funktion von react-redux, um Ihre Komponenten mit dem Redux-Store zu verbinden.

mapStateToProps: Bindet den Redux-Zustand an die Komponenten-Props.

```js
// mapStateToProps.js
const mapStateToProps = (state) => ({
  todos: state.todos
});
```

mapDispatchToProps: Bindet Action-Creator an die Komponenten-Props.

```js
// mapDispatchToProps.js
import { addTodo } from './actions';

const mapDispatchToProps = (dispatch) => ({
  addTodo: (todo) => dispatch(addTodo(todo))
});

```

Beispielkomponente: Verbinden Sie Ihre Komponente mit Redux.

```js
// TodoList.js
import React from 'react';
import { connect } from 'react-redux';
import { addTodo } from './actions';

const TodoList = ({ todos, addTodo }) => {
  const handleAdd = () => {
    const newTodo = { id: Date.now(), text: 'New Todo' };
    addTodo(newTodo);
  };

  return (
    <div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))}
      </ul>
      <button onClick={handleAdd}>Add Todo</button>
    </div>
  );
};

const mapStateToProps = (state) => ({
  todos: state.todos
});

const mapDispatchToProps = (dispatch) => ({
  addTodo: (todo) => dispatch(addTodo(todo))
});

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);

```

#### 5. Testen und Anpassen
Testen Sie Ihre Anwendung gründlich, um sicherzustellen, dass alle Funktionen wie erwartet arbeiten. Überprüfen Sie, ob:

Der Zustand korrekt aktualisiert wird.
Alle Komponenten richtig mit dem Redux-Store verbunden sind.
Der Zustand zwischen den Komponenten konsistent ist.