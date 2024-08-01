## Orchestrierung von Seiteneffekten mit Thunk, Sagas oder Observables

In Redux-Anwendungen müssen oft Nebenwirkungen (Seiteneffekte) wie Datenabfragen, API-Aufrufe oder asynchrone Operationen verwaltet werden. Redux allein ist nicht für die Handhabung von Nebenwirkungen ausgelegt, daher werden Middleware-Lösungen verwendet, um diese zu orchestrieren. Die gängigsten Methoden sind Redux Thunk, Redux Sagas und Redux Observables. Im Folgenden werden diese Ansätze vorgestellt und ihre Grundlagen erklärt.

### 1. Redux Thunk

**Redux Thunk** ist eine Middleware, die es ermöglicht, Action Creators als Funktionen zu definieren, die anstelle eines Action-Objekts eine Funktion zurückgeben. Diese Funktionen können dann asynchrone Logik enthalten, um Nebenwirkungen zu behandeln.

- **Vorteil**: Redux Thunk ist einfach zu verwenden und erfordert keine tiefgreifenden Änderungen an der bestehenden Redux-Architektur. Es ermöglicht das Dispatchen von Funktionen anstelle von nur Action-Objekten.

- **Beispiel**: Eine Thunk-Funktion, die Daten von einer API abruft, könnte so aussehen:

```javascript
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

const store = createStore(rootReducer, applyMiddleware(thunk));

```

Verwendung: Thunk wird durch die Middleware in den Store integriert:

```javascript
// actionCreators.js
export function fetchTodos() {
  return async (dispatch) => {
    dispatch({ type: 'FETCH_TODOS_REQUEST' });
    try {
      const response = await fetch('https://api.example.com/todos');
      const data = await response.json();
      dispatch({ type: 'FETCH_TODOS_SUCCESS', payload: data });
    } catch (error) {
      dispatch({ type: 'FETCH_TODOS_FAILURE', payload: error });
    }
  };
}
```

### 2. Redux Sagas
Redux Sagas ist eine Middleware, die auf Generator-Funktionen basiert und eine deklarative Methode zur Handhabung von Nebenwirkungen bietet. Sagas werden verwendet, um komplexe Asynchronität und Nebenwirkungen zu orchestrieren.

Vorteil: Redux Sagas ermöglicht die Verwaltung von komplexen Nebenwirkungen und asynchronen Flows in einer modularen und testbaren Weise. Es bietet mächtige Funktionen für die Handhabung von Fehlern und Wiederholungslogik.

Beispiel: Eine Saga für den API-Aufruf könnte so aussehen:

```javascript
import { call, put, takeEvery } from 'redux-saga/effects';

function* fetchTodos() {
  try {
    const response = yield call(fetch, 'https://api.example.com/todos');
    const data = yield response.json();
    yield put({ type: 'FETCH_TODOS_SUCCESS', payload: data });
  } catch (error) {
    yield put({ type: 'FETCH_TODOS_FAILURE', payload: error });
  }
}

function* watchFetchTodos() {
  yield takeEvery('FETCH_TODOS_REQUEST', fetchTodos);
}

export default function* rootSaga() {
  yield all([
    watchFetchTodos(),
  ]);
}

```

Verwendung: Sagas werden im Store eingerichtet:

```javascript
import createSagaMiddleware from 'redux-saga';
import { createStore, applyMiddleware } from 'redux';
import rootReducer from './reducers';
import rootSaga from './sagas';

const sagaMiddleware = createSagaMiddleware();
const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);

```

### 3. Redux Observables
Redux Observables ist eine Middleware, die RxJS-Operatoren verwendet, um asynchrone Nebenwirkungen zu orchestrieren. Sie basiert auf Observables und bietet eine reaktive Programmierung für die Verwaltung von Nebenwirkungen.

Vorteil: Redux Observables ermöglicht eine reaktive Programmierung, die besonders bei der Handhabung von komplexen asynchronen Streams und Ereignissen nützlich ist. Es bietet leistungsfähige Operatoren zur Transformation und Kombination von Streams.

Beispiel: Eine Observable für den API-Aufruf könnte so aussehen:

```javascript
import { ofType } from 'redux-observable';
import { from } from 'rxjs';
import { mergeMap, catchError, map } from 'rxjs/operators';

const fetchTodosEpic = action$ => action$.pipe(
  ofType('FETCH_TODOS_REQUEST'),
  mergeMap(() =>
    from(fetch('https://api.example.com/todos')).pipe(
      mergeMap(response => response.json()),
      map(data => ({ type: 'FETCH_TODOS_SUCCESS', payload: data })),
      catchError(error => ({ type: 'FETCH_TODOS_FAILURE', payload: error }))
    )
  )
);

export const rootEpic = fetchTodosEpic;

```

Verwendung: Observables werden im Store eingerichtet:

```javascript
import { createStore, applyMiddleware } from 'redux';
import { createEpicMiddleware } from 'redux-observable';
import rootReducer from './reducers';
import { rootEpic } from './epics';

const epicMiddleware = createEpicMiddleware();
const store = createStore(rootReducer, applyMiddleware(epicMiddleware));

epicMiddleware.run(rootEpic);

```