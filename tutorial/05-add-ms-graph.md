<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la bibliothèque [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ouvrez `./src/GraphService.ts` et ajoutez la fonction suivante.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getEventsSnippet":::

    Que fait ce code ?

    - L’URL qui sera appelée est `/me/events`.
    - La `select` méthode limite les champs renvoyés pour chaque événement à ceux que l’affichage utilise réellement.
    - La `orderby` méthode trie les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.

1. Créez un composant REACT pour afficher les résultats de l’appel. Créez un fichier dans le `./src` répertoire nommé `Calendar.tsx` et ajoutez le code suivant.

    ```typescript
    import React from 'react';
    import { Table } from 'reactstrap';
    import moment from 'moment';
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getEvents } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      events: Event[];
    }

    // Helper function to format Graph date/time
    function formatDateTime(dateTime: string | undefined) {
      if (dateTime !== undefined) {
        return moment.utc(dateTime).local().format('M/D/YY h:mm A');
      }
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          events: []
        };
      }

      async componentDidMount() {
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Get the user's events
          var events = await getEvents(accessToken);
          // Update the array of events in state
          this.setState({events: events.value});
        }
        catch(err) {
          this.props.setError('ERROR', JSON.stringify(err));
        }
      }

      render() {
        return (
          <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
        );
      }
    }

    export default withAuthProvider(Calendar);
    ```

    Pour le moment, cela restitue simplement le tableau d’événements dans JSON sur la page.

1. Ajoutez ce nouveau composant à l’application. Ouvrez `./src/App.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import Calendar from './Calendar';
    ```

1. Ajoutez le composant suivant juste après le `<Route>`.

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. Enregistrez vos modifications, puis redémarrez l’application. Connectez-vous, puis cliquez sur le lien **calendrier** dans la barre de navigation. Si tout fonctionne, vous devriez voir une image mémoire JSON des événements dans le calendrier de l’utilisateur.

## <a name="display-the-results"></a>Afficher les résultats

À présent, vous pouvez `Calendar` mettre à jour le composant pour afficher les événements de manière plus conviviale.

1. Remplacez la fonction `render` existante à `./src/Calendar.js` l’aide de la fonction suivante.

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    Cette méthode effectue une boucle dans la collection d’événements et ajoute une ligne de tableau pour chacun d’eux.

1. Enregistrez les modifications et redémarrez l’application. Cliquez sur le lien **calendrier** et l’application doit maintenant afficher un tableau d’événements.

    ![Capture d’écran du tableau des événements](./images/add-msgraph-01.png)
