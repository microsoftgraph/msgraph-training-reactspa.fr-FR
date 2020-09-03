<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="12466-101">Dans cet exercice, vous allez créer une inscription de l’application Web Azure AD à l’aide du centre d’administration Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="12466-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="12466-102">Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="12466-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="12466-103">Connectez-vous à l’aide d’un **compte personnel** (compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="12466-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="12466-104">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="12466-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="12466-105">Une capture d’écran des inscriptions d’applications</span><span class="sxs-lookup"><span data-stu-id="12466-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

    > [!NOTE]
    > <span data-ttu-id="12466-106">Les utilisateurs Azure AD B2C peuvent uniquement voir les **inscriptions des applications (héritées)**.</span><span class="sxs-lookup"><span data-stu-id="12466-106">Azure AD B2C users may only see **App registrations (legacy)**.</span></span> <span data-ttu-id="12466-107">Dans ce cas, accédez directement à [https://aka.ms/appregistrations](https://aka.ms/appregistrations) .</span><span class="sxs-lookup"><span data-stu-id="12466-107">In this case, please go directly to [https://aka.ms/appregistrations](https://aka.ms/appregistrations).</span></span>

1. <span data-ttu-id="12466-108">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="12466-108">Select **New registration**.</span></span> <span data-ttu-id="12466-109">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="12466-109">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="12466-110">Définissez le **Nom** sur `React Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="12466-110">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="12466-111">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="12466-111">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="12466-112">Sous **URI de redirection**, définissez la première flèche déroulante sur `Single-page application (SPA)`, et la valeur sur `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="12466-112">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:3000`.</span></span>

    ![Capture d’écran de la page Inscrire une application](./images/aad-register-an-app.png)

1. <span data-ttu-id="12466-114">Choisissez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="12466-114">Choose **Register**.</span></span> <span data-ttu-id="12466-115">Sur la page **didacticiel du graphique REACT** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="12466-115">On the **React Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](./images/aad-application-id.png)
