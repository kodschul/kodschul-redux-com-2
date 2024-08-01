## Debugging mit der Redux Chrome Extension

Die Redux DevTools Extension ist ein leistungsstarkes Werkzeug für das Debugging von Anwendungen, die Redux für das State-Management verwenden. Diese Chrome-Extension bietet eine grafische Benutzeroberfläche zur Überwachung und Verwaltung des Redux-Stores, zur Überprüfung von Aktionen und Zustandsänderungen sowie zur Zeitreise-Debugging. Im Folgenden sind die Schritte zur Verwendung der Redux Chrome Extension beschrieben und erläutert, wie sie beim Debugging hilft.

### Installation der Redux DevTools Extension

1. **Chrome Web Store besuchen**: Öffnen Sie den Chrome Web Store und suchen Sie nach "Redux DevTools".
2. **Erweiterung hinzufügen**: Klicken Sie auf "Hinzufügen" und installieren Sie die Erweiterung in Ihrem Chrome-Browser.

### Integration in Ihre Redux-Anwendung

Um die Redux DevTools Extension in Ihre Anwendung zu integrieren, müssen Sie den Store mit der Unterstützung für DevTools konfigurieren. Hierzu nutzen Sie die `composeWithDevTools`-Funktion, die von der `redux-devtools-extension`-Bibliothek bereitgestellt wird.

1. **Bibliothek installieren**:

   ```bash
   npm install redux-devtools-extension
   ```

2. **Store konfigurieren**:

    ```js
   import { createStore } from 'redux';
    import { composeWithDevTools } from 'redux-devtools-extension';
    import rootReducer from './reducers';

    const store = createStore(
    rootReducer,
    composeWithDevTools()
    );
    ```
    Durch die Verwendung von composeWithDevTools() wird die Redux DevTools Extension automatisch mit Ihrem Redux-Store verbunden.

### Verwendung der Redux DevTools Extension
Nach der Integration können Sie die Redux DevTools Extension nutzen, um verschiedene Aspekte Ihrer Anwendung zu überwachen:

1. **Aktionen überwachen**
    Die Extension zeigt alle gesendeten Aktionen an, einschließlich des Zeitpunkts, zu dem sie gesendet wurden. Dies hilft, den Fluss der Zustandsänderungen zu verstehen.

    Aktionen: Zeigt eine Liste aller Aktionen, die im Redux-Store ausgeführt wurden.
2. **Zustandsänderungen untersuchen**
    Die Extension ermöglicht es, den Zustand der Anwendung zu jedem Zeitpunkt anzusehen. Sie können sehen, wie sich der Zustand vor und nach jeder Aktion verändert hat.

    State: Zeigt den aktuellen Zustand des Redux-Stores und erlaubt es, frühere Zustände zu überprüfen.
3. **Zeitreise-Debugging**
    Eine der leistungsfähigsten Funktionen der Redux DevTools ist die Zeitreise-Debugging-Funktion. Sie ermöglicht es Ihnen, zu einem früheren Zustand der Anwendung zurückzukehren, indem Sie durch die Historie der Zustandsänderungen navigieren.

    Time Travel: Ermöglicht es, zu früheren Zuständen zurückzukehren und den Zustand der Anwendung zu einem bestimmten Zeitpunkt zu überprüfen.
4. **Dispatching von Aktionen**
    Sie können Aktionen direkt in der DevTools-Oberfläche auslösen, um zu testen, wie Ihre Anwendung auf bestimmte Aktionen reagiert.

    Action Dispatching: Erlaubt das manuelle Senden von Aktionen und die sofortige Überprüfung der Auswirkungen auf den Zustand.