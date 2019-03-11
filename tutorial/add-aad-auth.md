<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l'application de l'exercice précédent pour prendre en charge l'authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d'accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la bibliothèque de la [bibliothèque d'authentification Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) dans l'application.

Créez un fichier dans le `./src` répertoire nommé `Config.js` et ajoutez le code suivant.

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

Remplacez `YOUR_APP_ID_HERE` par l'ID de l'application dans le portail d'inscription des applications.

> [!IMPORTANT]
> Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d'exclure le `Config.js` fichier du contrôle de code source afin d'éviter une fuite accidentelle de votre ID d'application.

Ouvrez `./src/App.js` et ajoutez les instructions `import` suivantes en haut du fichier.

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a>Mettre en œuvre la connexion

Commencez par mettre à jour `constructor` le pour `App` la classe afin de créer une instance `UserAgentApplication` de la classe `getUser` et d'appeler pour voir s'il existe déjà un utilisateur connecté. Remplacez le existant `constructor` par ce qui suit.

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication(config.appId, null, null);

  var user = this.userAgentApplication.getUser();

  this.state = {
    isAuthenticated: (user !== null),
    user: {},
    error: null
  };

  if (user) {
    // Enhance user object with data from Graph
    this.getUserProfile();
  }
}
```

Ce code initialise la `UserAgentApplication` classe avec votre ID d'application et vérifie la présence d'un utilisateur. S'il existe un utilisateur, il `isAuthenticated` prend la valeur true. La `getUserProfile` méthode n'est pas encore implémentée. Vous allez implémenter un peu plus tard.

Ensuite, ajoutez une fonction à la `App` classe pour effectuer la connexion. Ajoutez la fonction suivante à la classe `App`.

```js
async login() {
  try {
    await this.userAgentApplication.loginPopup(config.scopes);
    await this.getUserProfile();
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Cette méthode appelle la `loginPopup` fonction pour effectuer la connexion, puis appelle la `getUserProfile` fonction.

À présent, ajoutez une fonction pour déconnecter. Ajoutez la fonction suivante à la classe `App`.

```js
logout() {
  this.userAgentApplication.logout();
}
```

À présent que `login` les `logout` méthodes et sont implémentées, `NavBar` mettez `Welcome` à jour les `render` éléments et dans `App` la méthode de la classe. Remplacez l'élément `NavBar` existant par ce qui suit.

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

Remplacez l'élément `Welcome` existant par ce qui suit.

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

Enfin, implémentez `getUserProfile` la fonction. Ajoutez la fonction suivante à la classe `App`.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent(config.scopes);

    if (accessToken) {
      // TEMPORARY: Display the token in the error flash
      this.setState({
        isAuthenticated: true,
        error: { message: "Access token:", debug: accessToken }
      });
    }
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Ce code appelle `acquireTokenSilent` pour obtenir un jeton d'accès, puis renvoie simplement le jeton en tant qu'erreur.

Enregistrez vos modifications et actualisez le navigateur. Cliquez sur le bouton de connexion et vous devez être redirigé vers `https://login.microsoftonline.com`. Connectez-vous avec votre compte Microsoft et acceptez les autorisations demandées. La page de l'application doit être actualisée, affichant le jeton.

### <a name="get-user-details"></a>Obtenir les détails de l'utilisateur

Commencez par créer un fichier qui contiendra tous vos appels Microsoft Graph. Créez un fichier dans le `./src` répertoire appelé `GraphService.js` et ajoutez le code suivant.

```js
var graph = require('@microsoft/microsoft-graph-client');

function getAuthenticatedClient(accessToken) {
  // Initialize Graph client
  const client = graph.Client.init({
    // Use the provided access token to authenticate
    // requests
    authProvider: (done) => {
      done(null, accessToken);
    }
  });

  return client;
}

export async function getUserDetails(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const user = await client.api('/me').get();
  return user;
}
```

Cette fonctionnalité implémente `getUserDetails` la fonction, qui utilise le kit de développement logiciel ( `/me` SDK) Microsoft Graph pour appeler le point de terminaison et renvoyer le résultat.

Mettez à `getUserProfile` jour la `./src/App.js` méthode dans pour appeler cette fonction. Tout d'abord, ajoutez `import` l'instruction suivante en haut du fichier.

```js
import { getUserDetails } from './GraphService';
```

Remplacez la fonction `getUserProfile` existante par le code suivant.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent(config.scopes);

    if (accessToken) {
      // Get the user's profile from Graph
      var user = await getUserDetails(accessToken);
      this.setState({
        isAuthenticated: true,
        user: {
          displayName: user.displayName,
          email: user.mail || user.userPrincipalName
        },
        error: null
      });
    }
  }
  catch(err) {
    var error = {};
    if (typeof(err) === 'string') {
      var errParts = err.split('|');
      error = errParts.length > 1 ?
        { message: errParts[1], debug: errParts[0] } :
        { message: err };
    } else {
      error = {
        message: err.message,
        debug: JSON.stringify(err)
      };
    }

    this.setState({
      isAuthenticated: false,
      user: {},
      error: error
    });
  }
}
```

Maintenant, si vous enregistrez vos modifications et démarrez l'application, après vous être connecté, vous devez revenir sur la page d'accueil, mais l'interface utilisateur doit changer pour indiquer que vous êtes connecté.

![Capture d'écran de la page d'accueil après la connexion](./images/add-aad-auth-01.png)

Cliquez sur Avatar de l'utilisateur dans le coin supérieur droit pour **** accéder au lien Déconnexion. Cliquez **** sur Déconnexion pour réinitialiser la session et revenir à la page d'accueil.

![Capture d'écran du menu déroulant avec le lien déConnexion](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Stockage et actualisation des jetons

À ce stade, votre application a un jeton d'accès, qui est envoyé `Authorization` dans l'en-tête des appels d'API. Il s'agit du jeton qui permet à l'application d'accéder à Microsoft Graph pour le compte de l'utilisateur.

Toutefois, ce jeton est éphémère. Le jeton expire une heure après son émission. C'est ici que le jeton d'actualisation devient utile. Le jeton d'actualisation permet à l'application de demander un nouveau jeton d'accès sans demander à l'utilisateur de se reconnecter.

Étant donné que l'application utilise la bibliothèque MSAL, vous n'avez pas besoin d'implémenter de logique d'actualisation ou de stockage de jetons. La `UserAgentApplication` méthode met en cache le jeton dans la session de navigateur. La `acquireTokenSilent` méthode vérifie d'abord le jeton mis en cache et, s'il n'a pas expiré, il le renvoie. Si elle a expiré, elle utilise le jeton d'actualisation mis en cache pour en obtenir une nouvelle. Vous utiliserez cette méthode plus dans le module suivant.