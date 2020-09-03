# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="e89b6-101">Exécution du projet terminé</span><span class="sxs-lookup"><span data-stu-id="e89b6-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e89b6-102">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="e89b6-102">Prerequisites</span></span>

<span data-ttu-id="e89b6-103">Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="e89b6-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="e89b6-104">[Node.js](https://nodejs.org) installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="e89b6-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="e89b6-105">Si vous n’avez pas Node.js, reportez-vous au lien précédent pour obtenir les options de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="e89b6-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="e89b6-106">(**Remarque :** ce didacticiel a été écrit avec le nœud version 12.16.1.</span><span class="sxs-lookup"><span data-stu-id="e89b6-106">(**Note:** This tutorial was written with Node version 12.16.1.</span></span> <span data-ttu-id="e89b6-107">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)</span><span class="sxs-lookup"><span data-stu-id="e89b6-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="e89b6-108">Soit un compte Microsoft personnel avec une boîte aux lettres sur Outlook.com, soit un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e89b6-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="e89b6-109">Si vous n’avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit :</span><span class="sxs-lookup"><span data-stu-id="e89b6-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="e89b6-110">Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="e89b6-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="e89b6-111">Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="e89b6-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="e89b6-112">Enregistrer une application Web avec le centre d’administration Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e89b6-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="e89b6-113">Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e89b6-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="e89b6-114">Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="e89b6-115">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="e89b6-116">Une capture d’écran des inscriptions d’applications</span><span class="sxs-lookup"><span data-stu-id="e89b6-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

    > <span data-ttu-id="e89b6-117">**Remarque :** Les utilisateurs Azure AD B2C peuvent uniquement voir les **inscriptions des applications (héritées)**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-117">**Note:** Azure AD B2C users may only see **App registrations (legacy)**.</span></span> <span data-ttu-id="e89b6-118">Dans ce cas, accédez directement à [https://aka.ms/appregistrations](https://aka.ms/appregistrations) .</span><span class="sxs-lookup"><span data-stu-id="e89b6-118">In this case, please go directly to [https://aka.ms/appregistrations](https://aka.ms/appregistrations).</span></span>

1. <span data-ttu-id="e89b6-119">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-119">Select **New registration**.</span></span> <span data-ttu-id="e89b6-120">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="e89b6-120">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="e89b6-121">Définissez le **Nom** sur `React Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="e89b6-121">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="e89b6-122">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-122">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="e89b6-123">Sous **URI de redirection**, définissez la première flèche déroulante sur `Single-page application (SPA)`, et la valeur sur `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="e89b6-123">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:3000`.</span></span>

    ![Capture d’écran de la page Inscrire une application](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="e89b6-125">Choisissez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="e89b6-125">Choose **Register**.</span></span> <span data-ttu-id="e89b6-126">Sur la page **didacticiel de graphique angulaire** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="e89b6-126">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="e89b6-128">Configurer l’exemple</span><span class="sxs-lookup"><span data-stu-id="e89b6-128">Configure the sample</span></span>

1. <span data-ttu-id="e89b6-129">Renommez le `./graph-tutorial/src/Config.example.ts` fichier `./graph-tutorial/src/Config.ts` .</span><span class="sxs-lookup"><span data-stu-id="e89b6-129">Rename the `./graph-tutorial/src/Config.example.ts` file to `./graph-tutorial/src/Config.ts`.</span></span>
1. <span data-ttu-id="e89b6-130">Modifiez le `./graph-tutorial/src/Config.ts` fichier et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="e89b6-130">Edit the `./graph-tutorial/src/Config.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="e89b6-131">Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="e89b6-131">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="e89b6-132">Dans votre interface de ligne de commande (CLI), accédez au `graph-tutorial` répertoire et exécutez la commande suivante pour installer les conditions requises.</span><span class="sxs-lookup"><span data-stu-id="e89b6-132">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="e89b6-133">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="e89b6-133">Run the sample</span></span>

1. <span data-ttu-id="e89b6-134">Exécutez la commande suivante dans votre interface CLI pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="e89b6-134">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="e89b6-135">Ouvrez un navigateur et accédez à `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="e89b6-135">Open a browser and browse to `http://localhost:3000`.</span></span>
