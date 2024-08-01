## Redux Schulung: Asynchrone Aktionen – Arbeiten mit Nebenwirkungen

Redux selbst ist synchron, was bedeutet, dass es nicht direkt mit asynchronen Aktionen oder Nebenwirkungen umgehen kann. Um asynchrone Logik zu integrieren, verwendet man Middleware, die die asynchrone Natur von APIs, Datenbankoperationen und anderen Nebenwirkungen handhabt. Zwei der häufigsten Middleware-Lösungen für asynchrone Aktionen in Redux sind `redux-thunk` und `redux-saga`. In dieser Schulung konzentrieren wir uns auf das Konzept von asynchronen Aktionen und wie sie in Redux implementiert werden können.

### Grundkonzept der asynchronen Aktionen

Asynchrone Aktionen sind notwendig, wenn Sie Operationen ausführen, die nicht sofort abgeschlossen sind, wie z.B. das Abrufen von Daten von einem Server. Da Redux von Natur aus synchron ist, werden asynchrone Aktionen mit Middleware behandelt, um diese Operationen zu ermöglichen.

### 1. Verwendung von `redux-thunk`

`redux-thunk` ist eine Middleware, die es Ihnen ermöglicht, Aktions-Creator-Funktionen zu schreiben, die anstelle eines Aktionsobjekts eine Funktion zurückgeben können. Diese Funktion kann dann asynchrone Operationen ausführen und erst dann eine Aktion dispatchen, wenn die Operation abgeschlossen ist.

#### Installation

```bash
npm install redux-thunk
```

Beispiel: Asynchrone Datenabfrage

```js
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';

// Erstellen des Stores mit redux-thunk Middleware
const store = createStore(rootReducer, applyMiddleware(thunk));

// Aktion zum Abrufen von Daten
const fetchData = () => {
  return (dispatch) => {
    dispatch({ type: 'FETCH_DATA_REQUEST' });

    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => {
        dispatch({ type: 'FETCH_DATA_SUCCESS', payload: data });
      })
      .catch(error => {
        dispatch({ type: 'FETCH_DATA_FAILURE', payload: error });
      });
  };
};

// Beispiel Reducer zur Handhabung der asynchronen Aktionen
const dataReducer = (state = { loading: false, data: [], error: null }, action) => {
  switch (action.type) {
    case 'FETCH_DATA_REQUEST':
      return { ...state, loading: true };
    case 'FETCH_DATA_SUCCESS':
      return { ...state, loading: false, data: action.payload };
    case 'FETCH_DATA_FAILURE':
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
};

```

### 2. Verwendung von redux-saga
redux-saga ist eine Middleware, die auf Generator-Funktionen basiert und eine deklarative Art der Handhabung von Nebenwirkungen bietet. Sagas ermöglichen es Ihnen, komplexe asynchrone Logik, wie das Abwarten mehrerer API-Anfragen, zu verwalten und den Zustand auf vorhersehbare Weise zu ändern.

Installation

```bash
npm install redux-saga

```

Beispiel: Asynchrone Datenabfrage mit redux-saga

```js
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import rootReducer from './reducers';
import { call, put, takeEvery } from 'redux-saga/effects';

// Erstellen des Saga-Middleware
const sagaMiddleware = createSagaMiddleware();
const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

// Saga zum Abrufen von Daten
function* fetchDataSaga() {
  try {
    yield put({ type: 'FETCH_DATA_REQUEST' });
    const response = yield call(fetch, 'https://api.example.com/data');
    const data = yield response.json();
    yield put({ type: 'FETCH_DATA_SUCCESS', payload: data });
  } catch (error) {
    yield put({ type: 'FETCH_DATA_FAILURE', payload: error });
  }
}

// Beobachten der 'FETCH_DATA' Aktion
function* watchFetchData() {
  yield takeEvery('FETCH_DATA', fetchDataSaga);
}

// Starten der Sagas
sagaMiddleware.run(watchFetchData);

```

### 3. Vergleich von redux-thunk und redux-saga
redux-thunk: Einfach zu verwenden und gut für einfache asynchrone Logik. Ideal, wenn Sie einfache API-Calls oder asynchrone Logik handhaben müssen.

redux-saga: Mächtiger und besser geeignet für komplexere Szenarien wie das Handling von mehreren parallelen oder abhängigen Asynchronous Requests. Bietet eine höhere Kontrolle und bessere Handhabung von Fehlern und Nebenwirkungen.
