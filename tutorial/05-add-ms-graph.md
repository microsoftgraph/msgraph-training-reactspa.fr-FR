<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a07ae-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="a07ae-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="a07ae-102">Pour cette application, vous allez utiliser la bibliothèque [cliente microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a07ae-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="a07ae-103">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="a07ae-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="a07ae-104">Ouvrez `./src/GraphService.ts` et ajoutez la fonction suivante.</span><span class="sxs-lookup"><span data-stu-id="a07ae-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="a07ae-105">Que fait ce code ?</span><span class="sxs-lookup"><span data-stu-id="a07ae-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="a07ae-106">L’URL qui sera appelée est `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="a07ae-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="a07ae-107">La méthode ajoute l’en-tête à la demande, ce qui entraîne la place des heures dans la réponse dans le fuseau horaire préféré de `header` `Prefer: outlook.timezone=""` l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a07ae-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="a07ae-108">La méthode ajoute les paramètres et définit la fenêtre de temps `query` `startDateTime` pour `endDateTime` l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="a07ae-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="a07ae-109">La méthode limite les champs renvoyés pour chaque événement à ceux que `select` l’affichage utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="a07ae-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="a07ae-110">La méthode trie les résultats par date et heure de création, l’élément le plus `orderby` récent étant le premier.</span><span class="sxs-lookup"><span data-stu-id="a07ae-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="a07ae-111">La `top` méthode limite les résultats d’une seule page à 25 événements.</span><span class="sxs-lookup"><span data-stu-id="a07ae-111">The `top` method limits the results in a single page to 25 events.</span></span>
    - <span data-ttu-id="a07ae-112">Si la réponse contient une valeur, indiquant que d’autres résultats sont disponibles, un objet est utilisé pour passer en page dans la collection afin d’obtenir `@odata.nextLink` `PageIterator` tous les résultats. [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)</span><span class="sxs-lookup"><span data-stu-id="a07ae-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="a07ae-113">Créez un composant React pour afficher les résultats de l’appel.</span><span class="sxs-lookup"><span data-stu-id="a07ae-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="a07ae-114">Créez un fichier dans `./src` le répertoire nommé et `Calendar.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a07ae-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
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
        if (this.props.user && !this.state.eventsLoaded)
        {
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
          catch (err) {
            this.props.setError('ERROR', JSON.stringify(err));
          }
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

    <span data-ttu-id="a07ae-115">Pour l’instant, cela restituera simplement le tableau des événements dans JSON sur la page.</span><span class="sxs-lookup"><span data-stu-id="a07ae-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="a07ae-116">Ajoutez ce nouveau composant à l’application.</span><span class="sxs-lookup"><span data-stu-id="a07ae-116">Add this new component to the app.</span></span> <span data-ttu-id="a07ae-117">Ouvrez `./src/App.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="a07ae-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="a07ae-118">Ajoutez le composant suivant juste après le composant `<Route>` existant.</span><span class="sxs-lookup"><span data-stu-id="a07ae-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="a07ae-119">Enregistrez vos modifications, puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="a07ae-119">Save your changes and restart the app.</span></span> <span data-ttu-id="a07ae-120">Connectez-vous et cliquez **sur le lien** Calendrier dans la barre de navigation.</span><span class="sxs-lookup"><span data-stu-id="a07ae-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="a07ae-121">Si tout fonctionne, vous devriez voir une image mémoire JSON des événements dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a07ae-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="a07ae-122">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="a07ae-122">Display the results</span></span>

<span data-ttu-id="a07ae-123">Vous pouvez maintenant mettre à jour le composant pour afficher les événements `Calendar` de manière plus conviviale.</span><span class="sxs-lookup"><span data-stu-id="a07ae-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="a07ae-124">Créez un fichier dans `./src` le répertoire nommé et `Calendar.css` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a07ae-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="a07ae-125">Créez un composant React pour restituer les événements en une seule journée en tant que lignes de tableau.</span><span class="sxs-lookup"><span data-stu-id="a07ae-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="a07ae-126">Créez un fichier dans `./src` le répertoire nommé et `CalendarDayRow.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="a07ae-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="a07ae-127">Ajoutez les `import` instructions suivantes en haut de **Calendar.tsx**.</span><span class="sxs-lookup"><span data-stu-id="a07ae-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="a07ae-128">Remplacez la fonction `render` existante par la fonction `./src/Calendar.tsx` suivante.</span><span class="sxs-lookup"><span data-stu-id="a07ae-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="a07ae-129">Cela divise les événements en jours respectifs et rend une section de tableau pour chaque jour.</span><span class="sxs-lookup"><span data-stu-id="a07ae-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="a07ae-130">Enregistrez les modifications et redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="a07ae-130">Save the changes and restart the app.</span></span> <span data-ttu-id="a07ae-131">Cliquez sur le **lien Calendrier** et l’application doit maintenant restituer une table des événements.</span><span class="sxs-lookup"><span data-stu-id="a07ae-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Capture d’écran du tableau des événements](./images/add-msgraph-01.png)
