<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="11e2c-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11e2c-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="11e2c-102">Cette étape est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="11e2c-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="11e2c-103">Dans cette étape, vous allez intégrer la bibliothèque bibliothèque d’authentification [Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="11e2c-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

1. <span data-ttu-id="11e2c-104">Créez un fichier dans `./src` le répertoire nommé et `Config.ts` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="11e2c-104">Create a new file in the `./src` directory named `Config.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    <span data-ttu-id="11e2c-105">`YOUR_APP_ID_HERE`Remplacez-le par l’ID de l’application à partir du portail d’inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="11e2c-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="11e2c-106">Si vous utilisez un contrôle source tel que Git, il est temps d’exclure le fichier du contrôle source afin d’éviter toute fuite accidentelle de votre `Config.ts` ID d’application.</span><span class="sxs-lookup"><span data-stu-id="11e2c-106">If you're using source control such as git, now would be a good time to exclude the `Config.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="11e2c-107">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="11e2c-107">Implement sign-in</span></span>

<span data-ttu-id="11e2c-108">Dans cette section, vous allez créer un fournisseur d’authentification et implémenter la sign-in et la sign-out.</span><span class="sxs-lookup"><span data-stu-id="11e2c-108">In this section you'll create an authentication provider and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="11e2c-109">Créez un fichier dans `./src` le répertoire nommé et `AuthProvider.tsx` ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="11e2c-109">Create a new file in the `./src` directory named `AuthProvider.tsx` and add the following code.</span></span>

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

1. <span data-ttu-id="11e2c-110">Ouvrez `./src/App.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="11e2c-110">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. <span data-ttu-id="11e2c-111">Remplacez la `class App extends Component<any> {` ligne par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="11e2c-111">Replace the line `class App extends Component<any> {` with the following.</span></span>

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. <span data-ttu-id="11e2c-112">Remplacez la `export default App;` ligne par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="11e2c-112">Replace the line `export default App;` with the following.</span></span>

    ```typescript
    export default withAuthProvider(App);
    ```

1. <span data-ttu-id="11e2c-113">Enregistrez vos modifications et actualisez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="11e2c-113">Save your changes and refresh the browser.</span></span> <span data-ttu-id="11e2c-114">Cliquez sur le bouton de connectez-vous et vous devriez voir une fenêtre int er qui se `https://login.microsoftonline.com` charge.</span><span class="sxs-lookup"><span data-stu-id="11e2c-114">Click the sign-in button and you should see a pop-up window that loads `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="11e2c-115">Connectez-vous avec votre compte Microsoft et consentez aux autorisations demandées.</span><span class="sxs-lookup"><span data-stu-id="11e2c-115">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="11e2c-116">La page de l’application doit s’actualiser et afficher le jeton.</span><span class="sxs-lookup"><span data-stu-id="11e2c-116">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="11e2c-117">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="11e2c-117">Get user details</span></span>

<span data-ttu-id="11e2c-118">Dans cette section, vous allez obtenir les détails de l’utilisateur à partir de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="11e2c-118">In this section you will get the user's details from Microsoft Graph.</span></span>

1. <span data-ttu-id="11e2c-119">Créez un fichier dans le `./src` répertoire appelé `GraphService.ts` et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="11e2c-119">Create a new file in the `./src` directory called `GraphService.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    <span data-ttu-id="11e2c-120">Cette opération implémente la fonction `getUserDetails`, qui utilise le kit de développement logiciel Microsoft Graph pour appeler le point de terminaison `/me` et renvoyer le résultat.</span><span class="sxs-lookup"><span data-stu-id="11e2c-120">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="11e2c-121">Ouvrez `./src/AuthProvider.tsx` et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="11e2c-121">Open `./src/AuthProvider.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. <span data-ttu-id="11e2c-122">Remplacez la fonction `getUserProfile` existante par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="11e2c-122">Replace the existing `getUserProfile` function with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. <span data-ttu-id="11e2c-123">Enregistrez vos modifications et démarrez l’application, une fois que vous vous êtes inscrit, vous devez revenir sur la page d’accueil, mais l’interface utilisateur doit changer pour indiquer que vous êtes connecté.</span><span class="sxs-lookup"><span data-stu-id="11e2c-123">Save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Capture d’écran de la page d’accueil après la connexion](./images/add-aad-auth-01.png)

1. <span data-ttu-id="11e2c-125">Cliquez sur l’avatar de l’utilisateur dans le coin supérieur droit pour accéder au lien **de** connexion.</span><span class="sxs-lookup"><span data-stu-id="11e2c-125">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="11e2c-126">Le fait de cliquer sur **Se déconnecter** réinitialise la session et vous ramène à la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="11e2c-126">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Capture d’écran du menu déroulant avec le lien de déconnexion](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="11e2c-128">Stockage et actualisation des jetons</span><span class="sxs-lookup"><span data-stu-id="11e2c-128">Storing and refreshing tokens</span></span>

<span data-ttu-id="11e2c-129">À ce stade, votre application dispose d’un jeton d’accès, qui est envoyé dans l’en-tête des `Authorization` appels d’API.</span><span class="sxs-lookup"><span data-stu-id="11e2c-129">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="11e2c-130">Il s’agit du jeton qui permet à l’application d’accéder à Microsoft Graph au nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="11e2c-130">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="11e2c-131">Cependant, ce jeton est de courte durée.</span><span class="sxs-lookup"><span data-stu-id="11e2c-131">However, this token is short-lived.</span></span> <span data-ttu-id="11e2c-132">Le jeton expire une heure après son émission.</span><span class="sxs-lookup"><span data-stu-id="11e2c-132">The token expires an hour after it is issued.</span></span> <span data-ttu-id="11e2c-133">C’est là que le jeton d’actualisation devient utile.</span><span class="sxs-lookup"><span data-stu-id="11e2c-133">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="11e2c-134">Le jeton d’actualisation permet à l’application de demander un nouveau jeton d’accès sans obliger l’utilisateur à se reconnecter.</span><span class="sxs-lookup"><span data-stu-id="11e2c-134">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="11e2c-135">Étant donné que l’application utilise la bibliothèque MSAL, vous n’avez pas besoin d’implémenter de logique de stockage ou d’actualisation de jeton.</span><span class="sxs-lookup"><span data-stu-id="11e2c-135">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="11e2c-136">Le `PublicClientApplication` jeton est mis en cache dans la session du navigateur.</span><span class="sxs-lookup"><span data-stu-id="11e2c-136">The `PublicClientApplication` caches the token in the browser session.</span></span> <span data-ttu-id="11e2c-137">La méthode vérifie d’abord le jeton mis en cache et, s’il `acquireTokenSilent` n’a pas expiré, elle le renvoie.</span><span class="sxs-lookup"><span data-stu-id="11e2c-137">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="11e2c-138">S’il a expiré, il utilise le jeton d’actualisation mis en cache pour en obtenir un nouveau.</span><span class="sxs-lookup"><span data-stu-id="11e2c-138">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="11e2c-139">Vous utiliserez davantage cette méthode dans le module suivant.</span><span class="sxs-lookup"><span data-stu-id="11e2c-139">You'll use this method more in the following module.</span></span>
