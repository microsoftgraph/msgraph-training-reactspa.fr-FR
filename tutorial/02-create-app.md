<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="87e51-101">Dans cette section, vous allez créer une application React.</span><span class="sxs-lookup"><span data-stu-id="87e51-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="87e51-102">Ouvrez votre interface de ligne de commande( CLI), accédez à un répertoire dans lequel vous avez le droit de créer des fichiers et exécutez les commandes suivantes pour créer une application React.</span><span class="sxs-lookup"><span data-stu-id="87e51-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="87e51-103">Une fois la commande terminé, modifiez le répertoire dans votre CLI et exécutez la commande suivante pour démarrer `graph-tutorial` un serveur web local.</span><span class="sxs-lookup"><span data-stu-id="87e51-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > <span data-ttu-id="87e51-104">Si Ce n’est [pas le cas,](https://yarnpkg.com/) vous pouvez l’utiliser `npm start` à la place.</span><span class="sxs-lookup"><span data-stu-id="87e51-104">If you do not have [Yarn](https://yarnpkg.com/) installed, you can use `npm start` instead.</span></span>

<span data-ttu-id="87e51-105">Votre navigateur par défaut s’ouvre [https://localhost:3000/](https://localhost:3000) avec une page React par défaut.</span><span class="sxs-lookup"><span data-stu-id="87e51-105">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="87e51-106">Si votre navigateur ne s’ouvre pas, ouvrez-le et recherchez-le pour vérifier [https://localhost:3000/](https://localhost:3000) que la nouvelle application fonctionne.</span><span class="sxs-lookup"><span data-stu-id="87e51-106">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="87e51-107">Ajouter des packages de nœuds</span><span class="sxs-lookup"><span data-stu-id="87e51-107">Add Node packages</span></span>

<span data-ttu-id="87e51-108">Avant de passer à la suite, installez des packages supplémentaires que vous utiliserez ultérieurement :</span><span class="sxs-lookup"><span data-stu-id="87e51-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="87e51-109">[react-router-dom pour](https://github.com/ReactTraining/react-router) le routage déclaratif à l’intérieur de l’application React.</span><span class="sxs-lookup"><span data-stu-id="87e51-109">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="87e51-110">[bootstrap](https://github.com/twbs/bootstrap) pour les styles et les composants courants.</span><span class="sxs-lookup"><span data-stu-id="87e51-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="87e51-111">[reactstrap](https://github.com/reactstrap/reactstrap) pour les composants React basés sur Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="87e51-111">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="87e51-112">[sans fontawesome pour](https://github.com/FortAwesome/Font-Awesome) les icônes.</span><span class="sxs-lookup"><span data-stu-id="87e51-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="87e51-113">[moment de](https://github.com/moment/moment) mise en forme des dates et heures.</span><span class="sxs-lookup"><span data-stu-id="87e51-113">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="87e51-114">[windows-iana pour](https://github.com/rubenillodo/windows-iana) la traduction des fuseaux horaires Windows au format IANA.</span><span class="sxs-lookup"><span data-stu-id="87e51-114">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="87e51-115">[msal-browser pour](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) l’authentification à Azure Active Directory et la récupération des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="87e51-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="87e51-116">[microsoft-graph-client pour](https://github.com/microsoftgraph/msgraph-sdk-javascript) effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="87e51-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="87e51-117">Exécutez la commande suivante dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="87e51-117">Run the following command in your CLI.</span></span>

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a><span data-ttu-id="87e51-118">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="87e51-118">Design the app</span></span>

<span data-ttu-id="87e51-119">Commencez par créer une barre de navigation pour l’application.</span><span class="sxs-lookup"><span data-stu-id="87e51-119">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="87e51-120">Créez un fichier dans `./src` le répertoire nommé et `NavBar.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="87e51-120">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="87e51-121">Créez une page d’accueil pour l’application.</span><span class="sxs-lookup"><span data-stu-id="87e51-121">Create a home page for the app.</span></span> <span data-ttu-id="87e51-122">Créez un fichier dans `./src` le répertoire nommé et `Welcome.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="87e51-122">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="87e51-123">Créez un message d’erreur pour afficher les messages à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="87e51-123">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="87e51-124">Créez un fichier dans `./src` le répertoire nommé et `ErrorMessage.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="87e51-124">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="87e51-125">Ouvrez le fichier `./src/index.css` et remplacez l’intégralité de son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="87e51-125">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="87e51-126">Ouvrez `./src/App.tsx` et remplacez tout son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="87e51-126">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

    ```typescript
    import React, { Component } from 'react';
    import { BrowserRouter as Router, Route, Redirect } from 'react-router-dom';
    import { Container } from 'reactstrap';
    import NavBar from './NavBar';
    import ErrorMessage from './ErrorMessage';
    import Welcome from './Welcome';
    import 'bootstrap/dist/css/bootstrap.css';

    class App extends Component<any> {
      render() {
        let error = null;
        if (this.props.error) {
          error = <ErrorMessage
            message={this.props.error.message}
            debug={this.props.error.debug} />;
        }

        return (
          <Router>
            <div>
              <NavBar
                isAuthenticated={this.props.isAuthenticated}
                authButtonMethod={this.props.isAuthenticated ? this.props.logout : this.props.login}
                user={this.props.user} />
              <Container>
                {error}
                <Route exact path="/"
                  render={(props) =>
                    <Welcome {...props}
                      isAuthenticated={this.props.isAuthenticated}
                      user={this.props.user}
                      authButtonMethod={this.props.login} />
                  } />
              </Container>
            </div>
          </Router>
        );
      }
    }

    export default App;
    ```

1. <span data-ttu-id="87e51-127">Enregistrez toutes vos modifications et redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="87e51-127">Save all of your changes and restart the app.</span></span> <span data-ttu-id="87e51-128">L’application doit maintenant avoir une apparence très différente.</span><span class="sxs-lookup"><span data-stu-id="87e51-128">Now, the app should look very different.</span></span>

    ![Capture d’écran de la page d’accueil repensée](images/create-app-01.png)
