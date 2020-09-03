<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la bibliothèque [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ouvrez `./src/GraphService.ts` et ajoutez la fonction suivante.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    Que fait ce code ?

    - L’URL qui sera appelée est `/me/calendarview`.
    - La `header` méthode ajoute l' `Prefer: outlook.timezone=""` en-tête à la demande, ce qui entraîne des temps dans la réponse qui se trouve dans le fuseau horaire préféré de l’utilisateur.
    - La `query` méthode ajoute les `startDateTime` `endDateTime` paramètres et, définissant la fenêtre de temps pour l’affichage Calendrier.
    - La `select` méthode limite les champs renvoyés pour chaque événement à ceux que l’affichage utilise réellement.
    - La `orderby` méthode trie les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.
    - La `top` méthode limite les résultats aux premiers événements 50.
    - Si la réponse contient une `@odata.nextLink` valeur, indiquant que davantage de résultats sont disponibles, un `PageIterator` objet est utilisé pour [Parcourir la collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) et obtenir tous les résultats.

1. Créez un composant REACT pour afficher les résultats de l’appel. Créez un fichier dans le `./src` répertoire nommé `Calendar.tsx` et ajoutez le code suivant.

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment from 'moment-timezone';
    import { findOneIana } from "windows-iana";
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getUserWeekCalendar } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      eventsLoaded: boolean;
      events: Event[];
      startOfWeek: Moment | undefined;
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          eventsLoaded: false,
          events: [],
          startOfWeek: undefined
        };
      }

      async componentDidUpdate() {
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          var ianaTimeZone = findOneIana(this.props.user.timeZone);

          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(ianaTimeZone!.valueOf()).startOf('week').utc();

          // Get the user's events
          var events = await getUserWeekCalendar(accessToken, this.props.user.timeZone, startOfWeek);

          // Update the array of events in state
          this.setState({
            eventsLoaded: true,
            events: events,
            startOfWeek: startOfWeek
          });
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

1. Ajoutez ce nouveau composant à l’application. Ouvrez `./src/App.tsx` et ajoutez l' `import` instruction suivante en haut du fichier.

    ```typescript
    import Calendar from './Calendar';
    ```

1. Ajoutez le composant suivant juste après le `<Route>` .

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

À présent, vous pouvez mettre à jour le `Calendar` composant pour afficher les événements de manière plus conviviale.

1. Créez un fichier dans le `./src` répertoire nommé `Calendar.css` et ajoutez le code suivant.

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. Créez un composant REACT pour afficher les événements sous forme de lignes de tableau. Créez un fichier dans le `./src` répertoire nommé `CalendarDayRow.tsx` et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. Ajoutez les `import` instructions suivantes en haut de **Calendar. TSX**.

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. Remplacez la `render` fonction existante à l’aide de `./src/Calendar.tsx` la fonction suivante.

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    Cela divise les événements en jours et affiche une section de tableau pour chaque jour.

1. Enregistrez les modifications et redémarrez l’application. Cliquez sur le lien **calendrier** et l’application doit maintenant afficher un tableau d’événements.

    ![Capture d’écran du tableau des événements](./images/add-msgraph-01.png)
