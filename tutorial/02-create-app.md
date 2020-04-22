<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez créer une nouvelle application REACT.

1. Ouvrez votre interface de ligne de commande (CLI), accédez à un répertoire où vous disposez de droits pour créer des fichiers, puis exécutez les commandes suivantes pour créer une nouvelle application REACT.

    ```Shell
    npx create-react-app@3.4.0 graph-tutorial --template typescript
    ```

1. Une fois la commande terminée, accédez au `graph-tutorial` répertoire dans votre interface CLI et exécutez la commande suivante pour démarrer un serveur Web local.

    ```Shell
    npm start
    ```

Votre navigateur par défaut s' [https://localhost:3000/](https://localhost:3000) ouvre sur avec une page REACT par défaut. Si votre navigateur ne s’ouvre pas, ouvrez-le [https://localhost:3000/](https://localhost:3000) et accédez à pour vérifier que la nouvelle application fonctionne.

## <a name="add-node-packages"></a>Ajouter des packages de nœuds

Avant de poursuivre, installez des packages supplémentaires que vous utiliserez plus tard :

- [REACT-Router-DOM](https://github.com/ReactTraining/react-router) pour le routage déclaratif au sein de l’application REACT.
- [bootstrap](https://github.com/twbs/bootstrap) pour le style et les composants communs.
- [reactstrap](https://github.com/reactstrap/reactstrap) pour les composants REACT basés sur bootstrap.
- [fontawesome](https://github.com/FortAwesome/Font-Awesome) n’est pas disponible pour les icônes.
- [moment](https://github.com/moment/moment) de mise en forme des dates et des heures.
- [MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-js) pour l’authentification auprès d’Azure Active Directory et pour la récupération des jetons d’accès.
- [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.

Exécutez la commande suivante dans votre interface CLI.

```Shell
npm install react-router-dom@5.1.2 @types/react-router-dom@5.1.3 bootstrap@4.4.1 reactstrap@8.4.1 @types/reactstrap@8.4.2
npm install @fortawesome/fontawesome-free@5.12.1 moment@2.24.0 msal@1.2.1 @microsoft/microsoft-graph-client@2.0.0 @types/microsoft-graph@1.12.0
```

## <a name="design-the-app"></a>Concevoir l’application

Commencez par créer une barre de navigation pour l’application.

1. Créez un fichier dans le `./src` répertoire nommé `NavBar.tsx` et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. Créez une page d’accueil pour l’application. Créez un fichier dans le `./src` répertoire nommé `Welcome.tsx` et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. Créer un affichage de message d’erreur pour afficher les messages destinés à l’utilisateur. Créez un fichier dans le `./src` répertoire nommé `ErrorMessage.tsx` et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. Ouvrez le fichier `./src/index.css` et remplacez l’intégralité de son contenu par ce qui suit.

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. Ouvrez `./src/App.tsx` et remplacez l’intégralité de son contenu par ce qui suit.

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

1. Enregistrez toutes vos modifications et actualisez la page. À présent, l’application doit être très différente.

    ![Capture d’écran de la page d’accueil repensée](images/create-app-01.png)
