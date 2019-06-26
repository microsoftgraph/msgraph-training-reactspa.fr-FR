<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8ddf8-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="8ddf8-102">Pour cette application, vous allez utiliser la bibliothèque [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="8ddf8-103">Obtenir des événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="8ddf8-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="8ddf8-104">Commencez par ajouter une nouvelle méthode au `./src/GraphService.js` fichier pour obtenir les événements à partir du calendrier.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-104">Start by adding a new method to the `./src/GraphService.js` file to get the events from the calendar.</span></span> <span data-ttu-id="8ddf8-105">Ajoutez la fonction suivante.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-105">Add the following function.</span></span>

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

<span data-ttu-id="8ddf8-106">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="8ddf8-107">L’URL qui sera appelée est `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="8ddf8-108">La `select` méthode limite les champs renvoyés pour chaque événement à ceux que l’affichage utilise réellement.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="8ddf8-109">La `orderby` méthode trie les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="8ddf8-110">À présent, créez un composant REACT pour afficher les résultats de l’appel.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-110">Now create a React component to display the results of the call.</span></span> <span data-ttu-id="8ddf8-111">Créez un fichier dans le `./src` répertoire nommé `Calendar.js` et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-111">Create a new file in the `./src` directory named `Calendar.js` and add the following code.</span></span>

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
      var accessToken = await window.msal.acquireTokenSilent({
        scopes: config.scopes
      });
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

<span data-ttu-id="8ddf8-112">Pour le moment, cela restitue simplement le tableau d’événements dans JSON sur la page.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-112">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="8ddf8-113">Ajoutez ce nouveau composant à l’application.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-113">Add this new component to the app.</span></span> <span data-ttu-id="8ddf8-114">Ouvrez `./src/App.js` et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-114">Open `./src/App.js` and add the following `import` statement to the top of the file.</span></span>

```js
import Calendar from './Calendar';
```

<span data-ttu-id="8ddf8-115">Ajoutez ensuite le composant suivant juste après le `<Route>`.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-115">Then add the following component just after the existing `<Route>`.</span></span>

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

<span data-ttu-id="8ddf8-116">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-116">Save your changes and restart the app.</span></span> <span data-ttu-id="8ddf8-117">Connectez-vous, puis cliquez sur le lien **calendrier** dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="8ddf8-118">Si tout fonctionne, vous devez voir un vidage JSON des événements sur le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="8ddf8-119">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="8ddf8-119">Display the results</span></span>

<span data-ttu-id="8ddf8-120">À présent, vous pouvez `Calendar` mettre à jour le composant pour afficher les événements de manière plus conviviale.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-120">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="8ddf8-121">Remplacez la fonction `render` existante à `./src/Calendar.js` l’aide de la fonction suivante.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-121">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

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

<span data-ttu-id="8ddf8-122">Cette méthode effectue une boucle dans la collection d’événements et ajoute une ligne de tableau pour chacun d’eux.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-122">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="8ddf8-123">Enregistrez les modifications et redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-123">Save the changes and restart the app.</span></span> <span data-ttu-id="8ddf8-124">Cliquez sur le lien **calendrier** et l’application doit maintenant afficher un tableau d’événements.</span><span class="sxs-lookup"><span data-stu-id="8ddf8-124">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Capture d’écran du tableau des événements](./images/add-msgraph-01.png)
