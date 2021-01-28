# <a name="getting-started-with-create-react-app"></a><span data-ttu-id="19fbc-101">Mise en place de la création d’une application React</span><span class="sxs-lookup"><span data-stu-id="19fbc-101">Getting Started with Create React App</span></span>

<span data-ttu-id="19fbc-102">Ce projet a été bootstrapped avec [Create React App](https://github.com/facebook/create-react-app).</span><span class="sxs-lookup"><span data-stu-id="19fbc-102">This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).</span></span>

## <a name="available-scripts"></a><span data-ttu-id="19fbc-103">Scripts disponibles</span><span class="sxs-lookup"><span data-stu-id="19fbc-103">Available Scripts</span></span>

<span data-ttu-id="19fbc-104">Dans le répertoire du projet, vous pouvez exécuter :</span><span class="sxs-lookup"><span data-stu-id="19fbc-104">In the project directory, you can run:</span></span>

### `yarn start`

<span data-ttu-id="19fbc-105">Exécute l’application en mode de développement.</span><span class="sxs-lookup"><span data-stu-id="19fbc-105">Runs the app in the development mode.</span></span>\
<span data-ttu-id="19fbc-106">[http://localhost:3000](http://localhost:3000)Ouvrez-le pour l’afficher dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="19fbc-106">Open [http://localhost:3000](http://localhost:3000) to view it in the browser.</span></span>

<span data-ttu-id="19fbc-107">La page se recharge si vous a modifiez.</span><span class="sxs-lookup"><span data-stu-id="19fbc-107">The page will reload if you make edits.</span></span>\
<span data-ttu-id="19fbc-108">Vous verrez également des erreurs de liaison dans la console.</span><span class="sxs-lookup"><span data-stu-id="19fbc-108">You will also see any lint errors in the console.</span></span>

### `yarn test`

<span data-ttu-id="19fbc-109">Lance le programme d’essai en mode d’observation interactif.</span><span class="sxs-lookup"><span data-stu-id="19fbc-109">Launches the test runner in the interactive watch mode.</span></span>\
<span data-ttu-id="19fbc-110">Pour plus d’informations, voir la section [sur l’exécution](https://facebook.github.io/create-react-app/docs/running-tests) des tests.</span><span class="sxs-lookup"><span data-stu-id="19fbc-110">See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.</span></span>

### `yarn build`

<span data-ttu-id="19fbc-111">Crée l’application pour la production dans le `build` dossier.</span><span class="sxs-lookup"><span data-stu-id="19fbc-111">Builds the app for production to the `build` folder.</span></span>\
<span data-ttu-id="19fbc-112">Il regroupe correctement React en mode production et optimise la build pour optimiser les performances.</span><span class="sxs-lookup"><span data-stu-id="19fbc-112">It correctly bundles React in production mode and optimizes the build for the best performance.</span></span>

<span data-ttu-id="19fbc-113">La build est minifiée et les noms de fichiers incluent les hés.</span><span class="sxs-lookup"><span data-stu-id="19fbc-113">The build is minified and the filenames include the hashes.</span></span>\
<span data-ttu-id="19fbc-114">Votre application est prête à être déployée !</span><span class="sxs-lookup"><span data-stu-id="19fbc-114">Your app is ready to be deployed!</span></span>

<span data-ttu-id="19fbc-115">Pour plus [d’informations, voir](https://facebook.github.io/create-react-app/docs/deployment) la section sur le déploiement.</span><span class="sxs-lookup"><span data-stu-id="19fbc-115">See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.</span></span>

### `yarn eject`

<span data-ttu-id="19fbc-116">**Remarque : il s’agit d’une opération à sens seul. Une fois `eject` que vous, vous ne pouvez pas revenir en arrière !**</span><span class="sxs-lookup"><span data-stu-id="19fbc-116">**Note: this is a one-way operation. Once you `eject`, you can’t go back!**</span></span>

<span data-ttu-id="19fbc-117">Si vous n’êtes pas satisfait de l’outil de build et des choix de configuration, vous pouvez `eject` le faire à tout moment.</span><span class="sxs-lookup"><span data-stu-id="19fbc-117">If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time.</span></span> <span data-ttu-id="19fbc-118">Cette commande supprime la dépendance de build unique de votre projet.</span><span class="sxs-lookup"><span data-stu-id="19fbc-118">This command will remove the single build dependency from your project.</span></span>

<span data-ttu-id="19fbc-119">Au lieu de cela, il copie tous les fichiers de configuration et les dépendances transitives (webpack, Contrôles, ESLint, etc.) directement dans votre projet pour que vous en copiiez le contrôle total.</span><span class="sxs-lookup"><span data-stu-id="19fbc-119">Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them.</span></span> <span data-ttu-id="19fbc-120">Toutes les commandes sauf fonctionneront toujours, mais elles pointent vers les scripts copiés afin de `eject` pouvoir les ajuster.</span><span class="sxs-lookup"><span data-stu-id="19fbc-120">All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them.</span></span> <span data-ttu-id="19fbc-121">À ce stade, vous êtes seul.</span><span class="sxs-lookup"><span data-stu-id="19fbc-121">At this point you’re on your own.</span></span>

<span data-ttu-id="19fbc-122">Vous n’avez pas besoin de `eject` l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="19fbc-122">You don’t have to ever use `eject`.</span></span> <span data-ttu-id="19fbc-123">L’ensemble de fonctionnalités organisées convient aux déploiements de petite et moyenne taille, et vous ne devez pas vous sentir obligé d’utiliser cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="19fbc-123">The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature.</span></span> <span data-ttu-id="19fbc-124">Toutefois, nous savons que cet outil ne serait pas utile si vous ne pouviez pas le personnaliser lorsque vous êtes prêt à l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="19fbc-124">However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.</span></span>

## <a name="learn-more"></a><span data-ttu-id="19fbc-125">En savoir plus</span><span class="sxs-lookup"><span data-stu-id="19fbc-125">Learn More</span></span>

<span data-ttu-id="19fbc-126">Vous pouvez en savoir plus dans la [documentation créer une application React.](https://facebook.github.io/create-react-app/docs/getting-started)</span><span class="sxs-lookup"><span data-stu-id="19fbc-126">You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).</span></span>

<span data-ttu-id="19fbc-127">Pour en savoir plus sur React, consultez [la documentation React.](https://reactjs.org/)</span><span class="sxs-lookup"><span data-stu-id="19fbc-127">To learn React, check out the [React documentation](https://reactjs.org/).</span></span>
