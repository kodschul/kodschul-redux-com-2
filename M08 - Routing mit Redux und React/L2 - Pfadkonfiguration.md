## Redux Schulung: Pfadkonfiguration - Dynamische und verschachtelte Routen

In modernen Webanwendungen, die mit Redux und React Router arbeiten, ist die Konfiguration von Routen ein wesentlicher Bestandteil der Navigation. Die Möglichkeit, dynamische und verschachtelte Routen zu definieren, verbessert die Flexibilität und Struktur der Anwendung. In dieser Schulung werden wir die Grundlagen der Pfadkonfiguration, einschließlich dynamischer und verschachtelter Routen, behandeln.

### Dynamische Routen

Dynamische Routen ermöglichen es, Parameter in den Routenpfad einzufügen, die zur Laufzeit ausgefüllt werden. Dies ist besonders nützlich für Seiten, die auf spezifische Daten basieren, wie z.B. Benutzerdaten oder Artikel.

#### Beispiel: Dynamische Routen mit React Router

In React Router wird eine dynamische Route durch das Einfügen von Platzhaltern in den Routenpfad definiert.

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import UserProfile from './UserProfile';

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/user/:userId" component={UserProfile} />
        {/* Weitere Routen */}
      </Switch>
    </Router>
  );
}

export default App;
```

In diesem Beispiel wird der Platzhalter :userId verwendet, um eine Route zu definieren, die für jeden Benutzerprofilpfad passt. Die UserProfile-Komponente kann dann auf den userId-Parameter zugreifen.

### Zugriff auf Parameter in der Komponente
In der Komponente können die Parameter mithilfe der useParams-Hook aus react-router-dom abgerufen werden.

```jsx
import React from 'react';
import { useParams } from 'react-router-dom';

function UserProfile() {
  const { userId } = useParams();

  return <div>Profil des Benutzers: {userId}</div>;
}

export default UserProfile;

```

### Verschachtelte Routen
Verschachtelte Routen ermöglichen es, Unterrouten innerhalb einer Hauptroute zu definieren. Dies ist nützlich für komplexe Layouts, bei denen bestimmte Bereiche einer Seite ihre eigenen Routen haben.

Beispiel: Verschachtelte Routen mit React Router
Verschachtelte Routen können definiert werden, indem Routen innerhalb einer Komponente platziert werden.

```jsx
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Dashboard from './Dashboard';
import Settings from './Settings';
import Profile from './Profile';

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/dashboard" component={Dashboard} />
        <Route path="/settings" component={Settings} />
      </Switch>
    </Router>
  );
}

export default App;

```

Innerhalb der Dashboard-Komponente können weitere Routen definiert werden.

```jsx
import React from 'react';
import { Route, Switch, Link } from 'react-router-dom';
import Overview from './Overview';
import Details from './Details';

function Dashboard() {
  return (
    <div>
      <nav>
        <Link to="/dashboard/overview">Overview</Link>
        <Link to="/dashboard/details">Details</Link>
      </nav>
      <Switch>
        <Route path="/dashboard/overview" component={Overview} />
        <Route path="/dashboard/details" component={Details} />
      </Switch>
    </div>
  );
}

export default Dashboard;

```

In diesem Beispiel hat die Dashboard-Seite eigene Unterrouten für "Overview" und "Details", die innerhalb der Dashboard-Komponente gerendert werden.