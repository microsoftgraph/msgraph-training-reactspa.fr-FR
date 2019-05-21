<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la bibliothèque [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtenir des événements de calendrier à partir d’Outlook

Commencez par ajouter une nouvelle méthode au `./src/GraphService.js` fichier pour obtenir les événements à partir du calendrier. Ajoutez la fonction suivante.

```js
export async function getEvents(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

Examinez ce que fait ce code.

- L’URL qui sera appelée est `/me/events`.
- La `select` méthode limite les champs renvoyés pour chaque événement à ceux que l’affichage utilise réellement.
- La `orderby` méthode trie les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.

À présent, créez un composant REACT pour afficher les résultats de l’appel. Créez un fichier dans le `./src` répertoire nommé `Calendar.js` et ajoutez le code suivant.

```JSX
import React from 'react';
import { Table } from 'reactstrap';
import moment from 'moment';
import config from './Config';
import { getEvents } from './GraphService';

// Helper function to format Graph date/time
function formatDateTime(dateTime) {
  return moment.utc(dateTime).local().format('M/D/YY h:mm A');
}

export default class Calendar extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      events: []
    };
  }

  async componentDidMount() {
    try {
      // Get the user's access token
      var accessToken = await window.msal.acquireTokenSilent(config.scopes);
      // Get the user's events
      var events = await getEvents(accessToken);
      // Update the array of events in state
      this.setState({events: events.value});
    }
    catch(err) {
      this.props.showError('ERROR', JSON.stringify(err));
    }
  }

  render() {
    return (
      <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
    );
  }
}
```

Pour le moment, cela restitue simplement le tableau d’événements dans JSON sur la page. Ajoutez ce nouveau composant à l’application. Ouvrez `./src/App.js` et ajoutez l’instruction `import` suivante en haut du fichier.

```js
import Calendar from './Calendar';
```

Ajoutez ensuite le composant suivant juste après le `<Route>`.

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

Enregistrez vos modifications, puis redémarrez l’application. Connectez-vous, puis cliquez sur le lien **calendrier** dans la barre de navigation. Si tout fonctionne, vous devez voir un vidage JSON des événements sur le calendrier de l’utilisateur.

## <a name="display-the-results"></a>Afficher les résultats

À présent, vous pouvez `Calendar` mettre à jour le composant pour afficher les événements de manière plus conviviale. Remplacez la fonction `render` existante à `./src/Calendar.js` l’aide de la fonction suivante.

```JSX
render() {
  return (
    <div>
      <h1>Calendar</h1>
      <Table>
        <thead>
          <tr>
            <th scope="col">Organizer</th>
            <th scope="col">Subject</th>
            <th scope="col">Start</th>
            <th scope="col">End</th>
          </tr>
        </thead>
        <tbody>
          {this.state.events.map(
            function(event){
              return(
                <tr key={event.id}>
                  <td>{event.organizer.emailAddress.name}</td>
                  <td>{event.subject}</td>
                  <td>{formatDateTime(event.start.dateTime)}</td>
                  <td>{formatDateTime(event.end.dateTime)}</td>
                </tr>
              );
            })}
        </tbody>
      </Table>
    </div>
  );
}
```

Cette méthode effectue une boucle dans la collection d’événements et ajoute une ligne de tableau pour chacun d’eux. Enregistrez les modifications et redémarrez l’application. Cliquez sur le lien **calendrier** et l’application doit maintenant afficher un tableau d’événements.

![Capture d’écran du tableau des événements](./images/add-msgraph-01.png)