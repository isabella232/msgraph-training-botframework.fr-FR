---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347813"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4f05e-101">Dans cette section, vous allez créer un projet d’infrastructure de robot.</span><span class="sxs-lookup"><span data-stu-id="4f05e-101">In this section you'll create a Bot Framework project.</span></span>

1. <span data-ttu-id="4f05e-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="4f05e-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="4f05e-103">Exécutez la commande suivante pour créer un projet à l’aide du modèle **Microsoft. Bot. Framework. CSharp. EchoBot** .</span><span class="sxs-lookup"><span data-stu-id="4f05e-103">Run the following command to create new project using the **Microsoft.Bot.Framework.CSharp.EchoBot** template.</span></span>

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > <span data-ttu-id="4f05e-104">Si une erreur s’affiche `No templates matched the input template name: echobot.` , installez le modèle à l’aide de la commande suivante et réexécutez la commande précédente.</span><span class="sxs-lookup"><span data-stu-id="4f05e-104">If you receive an `No templates matched the input template name: echobot.` error, install the template with the following command and re-run the previous command.</span></span>
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. <span data-ttu-id="4f05e-105">Renommez la classe **EchoBot** par défaut en **CalendarBot**.</span><span class="sxs-lookup"><span data-stu-id="4f05e-105">Rename the default **EchoBot** class to **CalendarBot**.</span></span> <span data-ttu-id="4f05e-106">Ouvrez **./bots/EchoBot.cs** et remplacez toutes les instances de `EchoBot` par `CalendarBot` .</span><span class="sxs-lookup"><span data-stu-id="4f05e-106">Open **./Bots/EchoBot.cs** and replace all instances of `EchoBot` with `CalendarBot`.</span></span> <span data-ttu-id="4f05e-107">Renommez le fichier en **CalendarBot.cs**.</span><span class="sxs-lookup"><span data-stu-id="4f05e-107">Rename the file to **CalendarBot.cs**.</span></span>

1. <span data-ttu-id="4f05e-108">Remplacez toutes les instances de `EchoBot` par `CalendarBot` dans les fichiers **. cs** restants.</span><span class="sxs-lookup"><span data-stu-id="4f05e-108">Replace all instances of `EchoBot` with `CalendarBot` in the remaining **.cs** files.</span></span>

1. <span data-ttu-id="4f05e-109">Dans votre interface CLI, remplacez le répertoire actif par le répertoire **GraphCalendarBot** et exécutez la commande suivante pour confirmer la génération du projet.</span><span class="sxs-lookup"><span data-stu-id="4f05e-109">In your CLI, change the current directory to the **GraphCalendarBot** directory and run the following command to confirm the project builds.</span></span>

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a><span data-ttu-id="4f05e-110">Ajouter des packages NuGet</span><span class="sxs-lookup"><span data-stu-id="4f05e-110">Add NuGet packages</span></span>

<span data-ttu-id="4f05e-111">Avant de poursuivre, installez des packages NuGet supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="4f05e-111">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="4f05e-112">[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) pour permettre au bot d’envoyer des cartes adaptatives dans les réponses.</span><span class="sxs-lookup"><span data-stu-id="4f05e-112">[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) to allow the bot to send Adaptive Cards in responses.</span></span>
- <span data-ttu-id="4f05e-113">[Microsoft. Bot. Builder. dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) pour ajouter la prise en charge de la boîte de dialogue au bot.</span><span class="sxs-lookup"><span data-stu-id="4f05e-113">[Microsoft.Bot.Builder.Dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) to add dialog support to the bot.</span></span>
- <span data-ttu-id="4f05e-114">[Microsoft. Recognizers. Text. Datatypes. TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) pour convertir les expressions Timex retournées à partir des invites bot en objets **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="4f05e-114">[Microsoft.Recognizers.Text.DataTypes.TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) to convert the TIMEX expressions returned from bot prompts into **DateTime** objects.</span></span>
- <span data-ttu-id="4f05e-115">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) pour effectuer des appels Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4f05e-115">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="4f05e-116">Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="4f05e-116">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a><span data-ttu-id="4f05e-117">Tester le robot</span><span class="sxs-lookup"><span data-stu-id="4f05e-117">Test the bot</span></span>

<span data-ttu-id="4f05e-118">Avant d’ajouter du code, testez-le pour vous assurer qu’il fonctionne correctement et que l’émulateur de l’infrastructure bot est configuré pour le tester.</span><span class="sxs-lookup"><span data-stu-id="4f05e-118">Before adding any code, test the bot to make sure that it works correctly, and that the Bot Framework Emulator is configured to test it.</span></span>

1. <span data-ttu-id="4f05e-119">Démarrez le bot en exécutant la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="4f05e-119">Start the bot by running the following command.</span></span>

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > <span data-ttu-id="4f05e-120">Bien que vous puissiez utiliser n’importe quel éditeur de texte pour modifier les fichiers sources dans le projet, nous vous recommandons d’utiliser [Visual Studio code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4f05e-120">While you can use any text editor to edit the source files in the project, we recommend using [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="4f05e-121">Visual Studio code offre une prise en charge du débogage, IntelliSense et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="4f05e-121">Visual Studio Code offers debugging support, Intellisense, and more.</span></span> <span data-ttu-id="4f05e-122">Si vous utilisez Visual Studio code, vous pouvez démarrer le bot à l’aide du menu **exécuter**  ->  **Démarrer le débogage** .</span><span class="sxs-lookup"><span data-stu-id="4f05e-122">If using Visual Studio Code, you can start the bot using the **Run** -> **Start Debugging** menu.</span></span>

1. <span data-ttu-id="4f05e-123">Vérifiez que le robot est en cours d’exécution en ouvrant votre navigateur et en accédant à `http://localhost:3978` .</span><span class="sxs-lookup"><span data-stu-id="4f05e-123">Confirm the bot is running by opening your browser and going to `http://localhost:3978`.</span></span> <span data-ttu-id="4f05e-124">Vous devriez voir que **votre robot est prêt !**</span><span class="sxs-lookup"><span data-stu-id="4f05e-124">You should see a **Your bot is ready!**</span></span> <span data-ttu-id="4f05e-125">Message.</span><span class="sxs-lookup"><span data-stu-id="4f05e-125">message.</span></span>

1. <span data-ttu-id="4f05e-126">Ouvrez l’émulateur de l’infrastructure bot.</span><span class="sxs-lookup"><span data-stu-id="4f05e-126">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="4f05e-127">Sélectionnez le menu **fichier** , puis **ouvrez bot**.</span><span class="sxs-lookup"><span data-stu-id="4f05e-127">Choose the **File** menu, then **Open Bot**.</span></span>

1. <span data-ttu-id="4f05e-128">Entrez `http://localhost:3978/api/messages` dans l' **URL du robot**, puis sélectionnez **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="4f05e-128">Enter `http://localhost:3978/api/messages` in the **Bot URL**, then select **Connect**.</span></span>

1. <span data-ttu-id="4f05e-129">Le bot répond avec `Hello and welcome!` dans la fenêtre de conversation.</span><span class="sxs-lookup"><span data-stu-id="4f05e-129">The bot responds with `Hello and welcome!` in the chat window.</span></span> <span data-ttu-id="4f05e-130">Envoyez un message au bot et confirmez qu’il renvoie un écho.</span><span class="sxs-lookup"><span data-stu-id="4f05e-130">Send a message to the bot and confirm it echoes it back.</span></span>

    ![Capture d’écran de l’émulateur de l’infrastructure bot connecté au bot](images/test-emulator.png)
