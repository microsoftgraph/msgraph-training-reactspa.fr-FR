# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="2ba73-101">Exécution du projet terminé</span><span class="sxs-lookup"><span data-stu-id="2ba73-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ba73-102">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="2ba73-102">Prerequisites</span></span>

<span data-ttu-id="2ba73-103">Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants:</span><span class="sxs-lookup"><span data-stu-id="2ba73-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="2ba73-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="2ba73-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="2ba73-105">Si vous ne disposez pas de Visual Studio, reportez-vous au lien précédent pour obtenir les options de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="2ba73-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="2ba73-106">(**Remarque:** ce didacticiel a été écrit avec Visual Studio 2017 version 15,81.</span><span class="sxs-lookup"><span data-stu-id="2ba73-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="2ba73-107">Les étapes de ce guide peuvent fonctionner avec d'autres versions, mais cela n'a pas été testé.)</span><span class="sxs-lookup"><span data-stu-id="2ba73-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="2ba73-108">Soit un compte Microsoft personnel avec une boîte aux lettres sur Outlook.com, soit un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2ba73-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="2ba73-109">Si vous n'avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit:</span><span class="sxs-lookup"><span data-stu-id="2ba73-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="2ba73-110">Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="2ba73-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="2ba73-111">Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="2ba73-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="2ba73-112">Enregistrer une application Web avec le centre d'administration Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2ba73-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="2ba73-113">Ouvrez un navigateur et accédez au [Centre d'administration Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2ba73-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="2ba73-114">Connectez-vous à l'aide d'un compte **personnel** (alias Microsoft) ou **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="2ba73-115">Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications (aperçu)** sous **gérer**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="2ba73-116">Capture d'écran des inscriptions d'application</span><span class="sxs-lookup"><span data-stu-id="2ba73-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="2ba73-117">Sélectionnez **nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-117">Select **New registration**.</span></span> <span data-ttu-id="2ba73-118">Sur la page **inscrire une application** , définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="2ba73-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="2ba73-119">Définissez **nom** sur `React Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="2ba73-119">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="2ba73-120">Définissez les types de comptes **pris en charge** sur **les comptes de tous les comptes d'annuaire et de Microsoft personnels**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="2ba73-121">Sous **URI**de redirection, définissez la première liste déroulante sur `Web` et définissez la `http://localhost:3000`valeur sur.</span><span class="sxs-lookup"><span data-stu-id="2ba73-121">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000`.</span></span>

    ![Capture d'écran de la page inscrire une application](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="2ba73-123">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-123">Choose **Register**.</span></span> <span data-ttu-id="2ba73-124">Sur la page **didacticiel de graphique angulaire** , copiez la valeur de l' **ID d'application (client)** et enregistrez-la, vous en aurez besoin à l'étape suivante.</span><span class="sxs-lookup"><span data-stu-id="2ba73-124">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Capture d'écran de l'ID d'application de la nouvelle inscription de l'application](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="2ba73-126">Sélectionnez **authentification** sous **gérer**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="2ba73-127">Recherchez la section **Grant implicite** et activez les **jetons d'accès** et les **jetons ID**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-127">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="2ba73-128">Cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="2ba73-128">Choose **Save**.</span></span>

    ![Capture d'écran de la section Grant implicite](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="2ba73-130">Configurer l'exemple</span><span class="sxs-lookup"><span data-stu-id="2ba73-130">Configure the sample</span></span>

1. <span data-ttu-id="2ba73-131">Renommez `./graph-tutorial/src/Config.js.example` le fichier `./graph-tutorial/src/Config.js`.</span><span class="sxs-lookup"><span data-stu-id="2ba73-131">Rename the `./graph-tutorial/src/Config.js.example` file to `./graph-tutorial/src/Config.js`.</span></span>
1. <span data-ttu-id="2ba73-132">Modifiez le `./graph-tutorial/src/Config.js` fichier et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="2ba73-132">Edit the `./graph-tutorial/src/Config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="2ba73-133">Remplacez `YOUR_APP_ID_HERE` par l' **ID d'application** que vous avez obtenu à partir du portail d'inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="2ba73-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="2ba73-134">Dans votre interface de ligne de commande (CLI), accédez au `graph-tutorial` répertoire et exécutez la commande suivante pour installer les conditions requises.</span><span class="sxs-lookup"><span data-stu-id="2ba73-134">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="2ba73-135">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="2ba73-135">Run the sample</span></span>

1. <span data-ttu-id="2ba73-136">Exécutez la commande suivante dans votre interface CLI pour démarrer l'application.</span><span class="sxs-lookup"><span data-stu-id="2ba73-136">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="2ba73-137">Ouvrez un navigateur et accédez à `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="2ba73-137">Open a browser and browse to `http://localhost:3000`.</span></span>