<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cette étape est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la bibliothèque bibliothèque d’authentification [Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) dans l’application.

1. Créez un fichier dans `./src` le répertoire nommé et `Config.ts` ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    `YOUR_APP_ID_HERE`Remplacez-le par l’ID de l’application à partir du portail d’inscription des applications.

    > [!IMPORTANT]
    > Si vous utilisez un contrôle source tel que Git, il est temps d’exclure le fichier du contrôle source afin d’éviter toute fuite accidentelle de votre `Config.ts` ID d’application.

## <a name="implement-sign-in"></a>Implémentation de la connexion

Dans cette section, vous allez créer un fournisseur d’authentification et implémenter la sign-in et la sign-out.

1. Créez un fichier dans `./src` le répertoire nommé et `AuthProvider.tsx` ajoutez le code suivant.

    ```typescript
    import React from 'react';
    import { PublicClientApplication } from '@azure/msal-browser';

    import { config } from './Config';

    export interface AuthComponentProps {
      error: any;
      isAuthenticated: boolean;
      user: any;
      login: Function;
      logout: Function;
      getAccessToken: Function;
      setError: Function;
    }

    interface AuthProviderState {
      error: any;
      isAuthenticated: boolean;
      user: any;
    }

    export default function withAuthProvider<T extends React.Component<AuthComponentProps>>
      (WrappedComponent: new (props: AuthComponentProps, context?: any) => T): React.ComponentClass {
      return class extends React.Component<any, AuthProviderState> {
        private publicClientApplication: PublicClientApplication;

        constructor(props: any) {
          super(props);
          this.state = {
            error: null,
            isAuthenticated: false,
            user: {}
          };

          // Initialize the MSAL application object
          this.publicClientApplication = new PublicClientApplication({
            auth: {
              clientId: config.appId,
              redirectUri: config.redirectUri
            },
            cache: {
              cacheLocation: "sessionStorage",
              storeAuthStateInCookie: true
            }
          });
        }

        componentDidMount() {
          // If MSAL already has an account, the user
          // is already logged in
          const accounts = this.publicClientApplication.getAllAccounts();

          if (accounts && accounts.length > 0) {
            // Enhance user object with data from Graph
            this.getUserProfile();
          }
        }

        render() {
          return <WrappedComponent
            error={ this.state.error }
            isAuthenticated={ this.state.isAuthenticated }
            user={ this.state.user }
            login={ () => this.login() }
            logout={ () => this.logout() }
            getAccessToken={ (scopes: string[]) => this.getAccessToken(scopes) }
            setError={ (message: string, debug: string) => this.setErrorMessage(message, debug) }
            { ...this.props } />;
        }

        async login() {
          try {
            // Login via popup
            await this.publicClientApplication.loginPopup(
              {
                scopes: config.scopes,
                prompt: "select_account"
              });

            // After login, get the user's profile
            await this.getUserProfile();
          }
          catch (err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        logout() {
          this.publicClientApplication.logout();
        }

        async getAccessToken(scopes: string[]): Promise<string> {
          try {
            const accounts = this.publicClientApplication
              .getAllAccounts();

            if (accounts.length <= 0) throw new Error('login_required');
            // Get the access token silently
            // If the cache contains a non-expired token, this function
            // will just return the cached token. Otherwise, it will
            // make a request to the Azure OAuth endpoint to get a token
            var silentResult = await this.publicClientApplication
              .acquireTokenSilent({
                scopes: scopes,
                account: accounts[0]
              });

            return silentResult.accessToken;
          } catch (err) {
            // If a silent request fails, it may be because the user needs
            // to login or grant consent to one or more of the requested scopes
            if (this.isInteractionRequired(err)) {
              var interactiveResult = await this.publicClientApplication
                .acquireTokenPopup({
                  scopes: scopes
                });

              return interactiveResult.accessToken;
            } else {
              throw err;
            }
          }
        }

        async getUserProfile() {
          try {
            var accessToken = await this.getAccessToken(config.scopes);

            if (accessToken) {
              // TEMPORARY: Display the token in the error flash
              this.setState({
                isAuthenticated: true,
                error: { message: "Access token:", debug: accessToken }
              });
            }
          }
          catch(err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        setErrorMessage(message: string, debug: string) {
          this.setState({
            error: { message: message, debug: debug }
          });
        }

        normalizeError(error: string | Error): any {
          var normalizedError = {};
          if (typeof (error) === 'string') {
            var errParts = error.split('|');
            normalizedError = errParts.length > 1 ?
              { message: errParts[1], debug: errParts[0] } :
              { message: error };
          } else {
            normalizedError = {
              message: error.message,
              debug: JSON.stringify(error)
            };
          }
          return normalizedError;
        }

        isInteractionRequired(error: Error): boolean {
          if (!error.message || error.message.length <= 0) {
            return false;
          }

          return (
            error.message.indexOf('consent_required') > -1 ||
            error.message.indexOf('interaction_required') > -1 ||
            error.message.indexOf('login_required') > -1 ||
            error.message.indexOf('no_account_in_silent_request') > -1
          );
        }
      }
    }
    ```

1. Ouvrez `./src/App.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. Remplacez la `class App extends Component<any> {` ligne par ce qui suit.

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. Remplacez la `export default App;` ligne par ce qui suit.

    ```typescript
    export default withAuthProvider(App);
    ```

1. Enregistrez vos modifications et actualisez le navigateur. Cliquez sur le bouton de connectez-vous et vous devriez voir une fenêtre int er qui se `https://login.microsoftonline.com` charge. Connectez-vous avec votre compte Microsoft et consentez aux autorisations demandées. La page de l’application doit s’actualiser et afficher le jeton.

### <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

Dans cette section, vous allez obtenir les détails de l’utilisateur à partir de Microsoft Graph.

1. Créez un fichier dans le `./src` répertoire appelé `GraphService.ts` et ajoutez le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    Cette opération implémente la fonction `getUserDetails`, qui utilise le kit de développement logiciel Microsoft Graph pour appeler le point de terminaison `/me` et renvoyer le résultat.

1. Ouvrez `./src/AuthProvider.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. Remplacez la fonction `getUserProfile` existante par le code suivant.

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. Enregistrez vos modifications et démarrez l’application, une fois que vous vous êtes inscrit, vous devez revenir sur la page d’accueil, mais l’interface utilisateur doit changer pour indiquer que vous êtes connecté.

    ![Capture d’écran de la page d’accueil après la connexion](./images/add-aad-auth-01.png)

1. Cliquez sur l’avatar de l’utilisateur dans le coin supérieur droit pour accéder au lien **de** connexion. Le fait de cliquer sur **Se déconnecter** réinitialise la session et vous ramène à la page d’accueil.

    ![Capture d’écran du menu déroulant avec le lien de déconnexion](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Stockage et actualisation des jetons

À ce stade, votre application dispose d’un jeton d’accès, qui est envoyé dans l’en-tête des `Authorization` appels d’API. Il s’agit du jeton qui permet à l’application d’accéder à Microsoft Graph au nom de l’utilisateur.

Cependant, ce jeton est de courte durée. Le jeton expire une heure après son émission. C’est là que le jeton d’actualisation devient utile. Le jeton d’actualisation permet à l’application de demander un nouveau jeton d’accès sans obliger l’utilisateur à se reconnecter.

Étant donné que l’application utilise la bibliothèque MSAL, vous n’avez pas besoin d’implémenter de logique de stockage ou d’actualisation de jeton. Le `PublicClientApplication` jeton est mis en cache dans la session du navigateur. La méthode vérifie d’abord le jeton mis en cache et, s’il `acquireTokenSilent` n’a pas expiré, elle le renvoie. S’il a expiré, il utilise le jeton d’actualisation mis en cache pour en obtenir un nouveau. Vous utiliserez davantage cette méthode dans le module suivant.
