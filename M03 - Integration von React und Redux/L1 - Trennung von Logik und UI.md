## Redux Schulung: Trennung von Logik und UI

Eine der Kernideen bei der Verwendung von Redux in React-Anwendungen ist die klare Trennung von Logik und Benutzeroberfläche (UI). Diese Trennung verbessert die Wartbarkeit, Lesbarkeit und Testbarkeit des Codes. Im Folgenden wird beschrieben, wie diese Trennung erreicht wird und welche Vorteile sie bietet.

### Trennung von Logik und UI

#### 1. Logik in Redux-Containern

Die Geschäftslogik sollte in sogenannten Containern oder Smart Components enthalten sein. Diese Komponenten sind direkt mit dem Redux-Store verbunden und sind dafür verantwortlich, Daten aus dem Store abzurufen und Aktionen zu dispatchen.

- **Vorteil**: Die Logik ist zentralisiert, was die Verwaltung und das Debuggen von Zustandsänderungen erleichtert.

- **Beispiel**: Ein Container für eine To-Do-Liste könnte folgendermaßen aussehen:

```javascript
import React from 'react';
import { connect } from 'react-redux';
import { addTodo } from '../actions';
import TodoList from '../components/TodoList';

class TodoListContainer extends React.Component {
  render() {
    const { todos, addTodo } = this.props;
    return <TodoList todos={todos} onAddTodo={addTodo} />;
  }
}

const mapStateToProps = state => ({
  todos: state.todos
});

const mapDispatchToProps = {
  addTodo
};

export default connect(mapStateToProps, mapDispatchToProps)(TodoListContainer);
```

### 2. Präsentationskomponenten
Die Darstellung der Daten erfolgt in Präsentationskomponenten oder Dumb Components. Diese Komponenten sind zustandslos und beziehen alle notwendigen Daten und Callback-Funktionen über Props. Sie kümmern sich ausschließlich um die Darstellung der Daten.

- **Vorteil**: Präsentationskomponenten sind wiederverwendbar und leicht zu testen, da sie keine Abhängigkeiten zum Zustand oder Store haben.

- **Beispiel**: Eine Präsentationskomponente für die To-Do-Liste könnte so aussehen:


```javascript
import React from 'react';
import PropTypes from 'prop-types';

const TodoList = ({ todos, onAddTodo }) => (
  <div>
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
    <button onClick={onAddTodo}>Add Todo</button>
  </div>
);

TodoList.propTypes = {
  todos: PropTypes.arrayOf(
    PropTypes.shape({
      id: PropTypes.number.isRequired,
      text: PropTypes.string.isRequired
    })
  ).isRequired,
  onAddTodo: PropTypes.func.isRequired
};

export default TodoList;

```

### Vorteile der Trennung von Logik und UI
1. Verbesserte Wartbarkeit
Durch die Trennung der Logik von der Darstellung wird der Code modularer und leichter zu verstehen. Änderungen in der Geschäftslogik oder der Benutzeroberfläche können unabhängig voneinander vorgenommen werden.

2. Leichtere Testbarkeit
Präsentationskomponenten sind einfacher zu testen, da sie keine Abhängigkeiten zu Redux oder dem Zustand haben. Tests können sich auf die Darstellung und das Verhalten der Komponenten konzentrieren, ohne sich um den Zustand zu kümmern.

3. Wiederverwendbarkeit
Präsentationskomponenten sind wiederverwendbar, da sie keine spezifische Logik enthalten. Sie können in verschiedenen Teilen der Anwendung verwendet werden, was den Entwicklungsaufwand reduziert und die Konsistenz der Benutzeroberfläche erhöht.

4. Klare Verantwortlichkeiten
Die klare Trennung der Verantwortlichkeiten führt zu einer besseren Organisation des Codes. Container kümmern sich um die Daten und die Logik, während Präsentationskomponenten sich auf die Darstellung konzentrieren.