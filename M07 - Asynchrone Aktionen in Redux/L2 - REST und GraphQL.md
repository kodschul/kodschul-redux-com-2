## REST und GraphQL: Datenabruf mit der Fetch-API

Die Fetch-API ist eine moderne Schnittstelle für das Abrufen von Daten über HTTP in JavaScript-Anwendungen. Sie bietet eine einfache Möglichkeit, Netzwerkanfragen zu senden und Antworten zu verarbeiten. Die Fetch-API kann sowohl für den Abruf von Daten über REST- als auch über GraphQL-APIs verwendet werden. Im Folgenden werden die Grundlagen der Verwendung der Fetch-API für beide Ansätze beschrieben.

### REST API

REST (Representational State Transfer) ist ein Architekturprinzip für die Entwicklung von Webdiensten. REST-APIs verwenden HTTP-Anfragen (GET, POST, PUT, DELETE) zur Interaktion mit Ressourcen.

#### Abrufen von Daten mit einer REST-API

Um Daten von einer REST-API abzurufen, verwenden Sie die Fetch-API, um eine HTTP-GET-Anfrage zu senden.

```javascript
// URL der REST-API
const apiUrl = 'https://api.example.com/data';

// Abrufen der Daten von der REST-API
fetch(apiUrl)
  .then(response => {
    if (!response.ok) {
      throw new Error('Netzwerkantwort war nicht ok.');
    }
    return response.json(); // Parsing der JSON-Antwort
  })
  .then(data => {
    console.log('Daten:', data); // Verarbeitung der Daten
  })
  .catch(error => {
    console.error('Es gab ein Problem mit der Fetch-Operation:', error);
  });
```

Beschreibung: In diesem Beispiel wird eine GET-Anfrage an die angegebene API-URL gesendet. Die Antwort wird als JSON geparst und die Daten werden verarbeitet.

#### Senden von Daten an eine REST-API
Um Daten an eine REST-API zu senden, verwenden Sie die Fetch-API mit der HTTP-POST-Methode.

```javascript
// URL der REST-API
const apiUrl = 'https://api.example.com/data';

// Daten, die gesendet werden sollen
const postData = {
  name: 'Beispiel',
  value: 123
};

// Senden der Daten an die REST-API
fetch(apiUrl, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(postData) // Konvertieren der Daten in JSON
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Netzwerkantwort war nicht ok.');
    }
    return response.json(); // Parsing der JSON-Antwort
  })
  .then(data => {
    console.log('Antwort der API:', data); // Verarbeitung der Antwort
  })
  .catch(error => {
    console.error('Es gab ein Problem mit der Fetch-Operation:', error);
  });

```

Beschreibung: Hier wird eine POST-Anfrage an die API gesendet, die JSON-Daten enthält. Die Antwort wird ebenfalls als JSON geparst.
### GraphQL API
GraphQL ist eine Abfragesprache für APIs, die es ermöglicht, genau die Daten abzurufen, die benötigt werden. Im Gegensatz zu REST, das auf Endpunkten basiert, verwendet GraphQL eine einzige Endpunkt-URL, um Anfragen zu senden.

Abrufen von Daten mit einer GraphQL-API
Um Daten von einer GraphQL-API abzurufen, senden Sie eine POST-Anfrage mit einer Abfrage (Query) im Anfragekörper.

```javascript
// URL der GraphQL-API
const graphqlUrl = 'https://api.example.com/graphql';

// GraphQL-Abfrage
const query = `
  query {
    users {
      id
      name
      email
    }
  }
`;

// Abrufen der Daten von der GraphQL-API
fetch(graphqlUrl, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ query }) // Konvertieren der Abfrage in JSON
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Netzwerkantwort war nicht ok.');
    }
    return response.json(); // Parsing der JSON-Antwort
  })
  .then(data => {
    console.log('Daten:', data.data); // Verarbeitung der Daten
  })
  .catch(error => {
    console.error('Es gab ein Problem mit der Fetch-Operation:', error);
  });

```

Beschreibung: In diesem Beispiel wird eine POST-Anfrage an die GraphQL-API gesendet, die eine Abfrage zur Rückgabe von Benutzerdaten enthält. Die Antwort wird als JSON geparst.

#### Senden von Mutationen an eine GraphQL-API
Um Daten zu ändern, verwenden Sie Mutationen in GraphQL.

```javascript
// URL der GraphQL-API
const graphqlUrl = 'https://api.example.com/graphql';

// GraphQL-Mutation
const mutation = `
  mutation {
    addUser(name: "Beispiel", email: "example@example.com") {
      id
      name
    }
  }
`;

// Senden der Mutation an die GraphQL-API
fetch(graphqlUrl, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ query: mutation }) // Konvertieren der Mutation in JSON
})
  .then(response => {
    if (!response.ok) {
      throw new Error('Netzwerkantwort war nicht ok.');
    }
    return response.json(); // Parsing der JSON-Antwort
  })
  .then(data => {
    console.log('Antwort der API:', data.data); // Verarbeitung der Antwort
  })
  .catch(error => {
    console.error('Es gab ein Problem mit der Fetch-Operation:', error);
  });

```

Beschreibung: Hier wird eine Mutation an die GraphQL-API gesendet, um einen neuen Benutzer hinzuzufügen. Die Antwort wird als JSON geparst.