<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="03b0e-101">Dans cette section, vous allez créer une nouvelle application REACT.</span><span class="sxs-lookup"><span data-stu-id="03b0e-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="03b0e-102">Ouvrez votre interface de ligne de commande (CLI), accédez à un répertoire où vous disposez de droits pour créer des fichiers, puis exécutez les commandes suivantes pour créer une nouvelle application REACT.</span><span class="sxs-lookup"><span data-stu-id="03b0e-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@3.4.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="03b0e-103">Une fois la commande terminée, accédez au `graph-tutorial` répertoire dans votre interface CLI et exécutez la commande suivante pour démarrer un serveur Web local.</span><span class="sxs-lookup"><span data-stu-id="03b0e-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

<span data-ttu-id="03b0e-104">Votre navigateur par défaut s’ouvre sur [https://localhost:3000/](https://localhost:3000) avec une page REACT par défaut.</span><span class="sxs-lookup"><span data-stu-id="03b0e-104">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="03b0e-105">Si votre navigateur ne s’ouvre pas, ouvrez-le et accédez à [https://localhost:3000/](https://localhost:3000) pour vérifier que la nouvelle application fonctionne.</span><span class="sxs-lookup"><span data-stu-id="03b0e-105">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="03b0e-106">Ajouter des packages de nœuds</span><span class="sxs-lookup"><span data-stu-id="03b0e-106">Add Node packages</span></span>

<span data-ttu-id="03b0e-107">Avant de poursuivre, installez des packages supplémentaires que vous utiliserez plus tard :</span><span class="sxs-lookup"><span data-stu-id="03b0e-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="03b0e-108">[REACT-Router-DOM](https://github.com/ReactTraining/react-router) pour le routage déclaratif au sein de l’application REACT.</span><span class="sxs-lookup"><span data-stu-id="03b0e-108">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="03b0e-109">[bootstrap](https://github.com/twbs/bootstrap) pour le style et les composants communs.</span><span class="sxs-lookup"><span data-stu-id="03b0e-109">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="03b0e-110">[reactstrap](https://github.com/reactstrap/reactstrap) pour les composants REACT basés sur bootstrap.</span><span class="sxs-lookup"><span data-stu-id="03b0e-110">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="03b0e-111">[fontawesome](https://github.com/FortAwesome/Font-Awesome) n’est pas disponible pour les icônes.</span><span class="sxs-lookup"><span data-stu-id="03b0e-111">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="03b0e-112">[moment](https://github.com/moment/moment) de mise en forme des dates et des heures.</span><span class="sxs-lookup"><span data-stu-id="03b0e-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="03b0e-113">[Windows-IANA](https://github.com/rubenillodo/windows-iana) pour traduire les fuseaux horaires Windows au format IANA.</span><span class="sxs-lookup"><span data-stu-id="03b0e-113">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="03b0e-114">[MSAL-navigateur](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) pour l’authentification auprès d’Azure Active Directory et pour la récupération des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="03b0e-114">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="03b0e-115">[Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="03b0e-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="03b0e-116">Exécutez la commande suivante dans votre interface CLI.</span><span class="sxs-lookup"><span data-stu-id="03b0e-116">Run the following command in your CLI.</span></span>

```Shell
npm install react-router-dom@5.2.0 @types/react-router-dom@5.1.5 bootstrap@4.5.2 reactstrap@8.5.1 @types/reactstrap@8.5.1 @fortawesome/fontawesome-free@5.14.0
npm install moment@2.27.0 moment-timezone@0.5.31 windows-iana@4.2.1 @azure/msal-browser@2.1.0 @microsoft/microsoft-graph-client@2.0.0 @types/microsoft-graph@1.18.0
```

## <a name="design-the-app"></a><span data-ttu-id="03b0e-117">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="03b0e-117">Design the app</span></span>

<span data-ttu-id="03b0e-118">Commencez par créer une barre de navigation pour l’application.</span><span class="sxs-lookup"><span data-stu-id="03b0e-118">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="03b0e-119">Créez un fichier dans le `./src` répertoire nommé `NavBar.tsx` et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="03b0e-119">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="03b0e-120">Créez une page d’accueil pour l’application.</span><span class="sxs-lookup"><span data-stu-id="03b0e-120">Create a home page for the app.</span></span> <span data-ttu-id="03b0e-121">Créez un fichier dans le `./src` répertoire nommé `Welcome.tsx` et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="03b0e-121">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="03b0e-122">Créer un affichage de message d’erreur pour afficher les messages destinés à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="03b0e-122">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="03b0e-123">Créez un fichier dans le `./src` répertoire nommé `ErrorMessage.tsx` et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="03b0e-123">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="03b0e-124">Ouvrez le fichier `./src/index.css` et remplacez l’intégralité de son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="03b0e-124">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="03b0e-125">Ouvrez `./src/App.tsx` et remplacez l’intégralité de son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="03b0e-125">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

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
                user={this.props.user}/>
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

1. <span data-ttu-id="03b0e-126">Enregistrez toutes vos modifications et actualisez la page.</span><span class="sxs-lookup"><span data-stu-id="03b0e-126">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="03b0e-127">À présent, l’application doit être très différente.</span><span class="sxs-lookup"><span data-stu-id="03b0e-127">Now, the app should look very different.</span></span>

    ![Capture d’écran de la page d’accueil repensée](images/create-app-01.png)
