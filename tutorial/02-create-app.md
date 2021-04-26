<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez créer une application React'application.

1. Ouvrez votre interface de ligne de commande (CLI), accédez à un répertoire dans lequel vous avez le droit de créer des fichiers et exécutez les commandes suivantes pour créer une application React de commande.

    ```Shell
    yarn create react-app graph-tutorial --template typescript
    ```

1. Une fois la commande terminé, modifiez le répertoire dans votre CLI et exécutez la commande suivante pour démarrer `graph-tutorial` un serveur web local.

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > Si Ce n'est [pas le cas,](https://yarnpkg.com/) vous pouvez l'utiliser `npm start` à la place.

Votre navigateur par défaut s'ouvre [https://localhost:3000/](https://localhost:3000) avec une page de React par défaut. Si votre navigateur ne s'ouvre pas, ouvrez-le et recherchez-le pour vérifier [https://localhost:3000/](https://localhost:3000) que la nouvelle application fonctionne.

## <a name="add-node-packages"></a>Ajouter des packages de nœuds

Avant de passer à la suite, installez des packages supplémentaires que vous utiliserez ultérieurement :

- [react-router-dom](https://github.com/ReactTraining/react-router) pour le routage déclaratif à l'intérieur de l React applet.
- [bootstrap](https://github.com/twbs/bootstrap) pour les styles et les composants courants.
- [reactstrap](https://github.com/reactstrap/reactstrap) pour React composants basés sur Bootstrap.
- [sans fontawesome pour](https://github.com/FortAwesome/Font-Awesome) les icônes.
- [moment de](https://github.com/moment/moment) mise en forme des dates et heures.
- [windows-iana pour](https://github.com/rubenillodo/windows-iana) la traduction Windows fuseaux horaires au format IANA.
- [msal-browser pour](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) l'authentification Azure Active Directory et la récupération des jetons d'accès.
- [microsoft-graph-client pour](https://github.com/microsoftgraph/msgraph-sdk-javascript) effectuer des appels à Microsoft Graph.

Exécutez la commande suivante dans votre CLI.

```Shell
yarn add react-router-dom@5.2.0 bootstrap@4.6.0 reactstrap@8.9.0 @fortawesome/fontawesome-free@5.15.3
yarn add moment-timezone@0.5.33 windows-iana@5.0.2 @azure/msal-browser@2.14.1 @microsoft/microsoft-graph-client@2.2.1
yarn add -D @types/react-router-dom@5.1.7 @types/microsoft-graph@1.36.0
```

## <a name="design-the-app"></a>Concevoir l’application

Commencez par créer une barre de navigation pour l'application.

1. Créez un fichier dans `./src` le répertoire nommé et `NavBar.tsx` ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. Créez une page d'accueil pour l'application. Créez un fichier dans `./src` le répertoire nommé et `Welcome.tsx` ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. Créez un message d'erreur pour afficher les messages à l'utilisateur. Créez un fichier dans `./src` le répertoire nommé et `ErrorMessage.tsx` ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. Ouvrez le fichier `./src/index.css` et remplacez l’intégralité de son contenu par ce qui suit.

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. Ouvrez `./src/App.tsx` et remplacez tout son contenu par ce qui suit.

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

1. Enregistrez toutes vos modifications et redémarrez l’application. L'application doit maintenant avoir une apparence très différente.

    ![Capture d’écran de la page d’accueil repensée](images/create-app-01.png)
