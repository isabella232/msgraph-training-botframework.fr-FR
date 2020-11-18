---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347818"
---
# <a name="graphcalendarbot"></a><span data-ttu-id="1b429-101">GraphCalendarBot</span><span class="sxs-lookup"><span data-stu-id="1b429-101">GraphCalendarBot</span></span>

<span data-ttu-id="1b429-102">Exemple d’écho v4 v4 de robot.</span><span class="sxs-lookup"><span data-stu-id="1b429-102">Bot Framework v4 echo bot sample.</span></span>

<span data-ttu-id="1b429-103">Ce robot a été créé à l’aide de l' [infrastructure de robot](https://dev.botframework.com), il montre comment créer un robot simple qui accepte l’entrée de l’utilisateur et le renvoie en écho.</span><span class="sxs-lookup"><span data-stu-id="1b429-103">This bot has been created using [Bot Framework](https://dev.botframework.com), it shows how to create a simple bot that accepts input from the user and echoes it back.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1b429-104">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="1b429-104">Prerequisites</span></span>

- <span data-ttu-id="1b429-105">[Kit de développement .net Core SDK](https://dotnet.microsoft.com/download) version 3,1</span><span class="sxs-lookup"><span data-stu-id="1b429-105">[.NET Core SDK](https://dotnet.microsoft.com/download) version 3.1</span></span>

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a><span data-ttu-id="1b429-106">Pour essayer cet exemple</span><span class="sxs-lookup"><span data-stu-id="1b429-106">To try this sample</span></span>

- <span data-ttu-id="1b429-107">Dans un terminal, accédez à `GraphCalendarBot`</span><span class="sxs-lookup"><span data-stu-id="1b429-107">In a terminal, navigate to `GraphCalendarBot`</span></span>

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- <span data-ttu-id="1b429-108">Exécuter le bot à partir d’un terminal ou de Visual Studio, choisissez l’option A ou B.</span><span class="sxs-lookup"><span data-stu-id="1b429-108">Run the bot from a terminal or from Visual Studio, choose option A or B.</span></span>

  <span data-ttu-id="1b429-109">A) à partir d’un terminal</span><span class="sxs-lookup"><span data-stu-id="1b429-109">A) From a terminal</span></span>

  ```bash
  # run the bot
  dotnet run
  ```

  <span data-ttu-id="1b429-110">B) ou à partir de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b429-110">B) Or from Visual Studio</span></span>

  - <span data-ttu-id="1b429-111">Lancer Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b429-111">Launch Visual Studio</span></span>
  - <span data-ttu-id="1b429-112">Fichier-> > projet/solution</span><span class="sxs-lookup"><span data-stu-id="1b429-112">File -> Open -> Project/Solution</span></span>
  - <span data-ttu-id="1b429-113">Accéder au `GraphCalendarBot` dossier</span><span class="sxs-lookup"><span data-stu-id="1b429-113">Navigate to `GraphCalendarBot` folder</span></span>
  - <span data-ttu-id="1b429-114">Sélectionner un `GraphCalendarBot.csproj` fichier</span><span class="sxs-lookup"><span data-stu-id="1b429-114">Select `GraphCalendarBot.csproj` file</span></span>
  - <span data-ttu-id="1b429-115">Appuyez sur `F5` pour exécuter le projet</span><span class="sxs-lookup"><span data-stu-id="1b429-115">Press `F5` to run the project</span></span>

## <a name="testing-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="1b429-116">Test du robot à l’aide de l’émulateur de l’infrastructure bot</span><span class="sxs-lookup"><span data-stu-id="1b429-116">Testing the bot using Bot Framework Emulator</span></span>

<span data-ttu-id="1b429-117">L’émulateur de l' [infrastructure bot](https://github.com/microsoft/botframework-emulator) est une application de bureau qui permet aux développeurs de moteurs de tester et de déboguer leurs robots sur localhost ou de s’exécuter à distance via un tunnel.</span><span class="sxs-lookup"><span data-stu-id="1b429-117">[Bot Framework Emulator](https://github.com/microsoft/botframework-emulator) is a desktop application that allows bot developers to test and debug their bots on localhost or running remotely through a tunnel.</span></span>

- <span data-ttu-id="1b429-118">Installer l' [émulateur de l'](https://github.com/Microsoft/BotFramework-Emulator/releases) infrastructure bot 4.9.0 ou une version ultérieure</span><span class="sxs-lookup"><span data-stu-id="1b429-118">Install the Bot Framework Emulator version 4.9.0 or greater from [here](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="1b429-119">Se connecter au bot à l’aide de l’émulateur de l’infrastructure bot</span><span class="sxs-lookup"><span data-stu-id="1b429-119">Connect to the bot using Bot Framework Emulator</span></span>

- <span data-ttu-id="1b429-120">Lancer l’émulateur de l’infrastructure bot</span><span class="sxs-lookup"><span data-stu-id="1b429-120">Launch Bot Framework Emulator</span></span>
- <span data-ttu-id="1b429-121">Robot ouvert > fichier</span><span class="sxs-lookup"><span data-stu-id="1b429-121">File -> Open Bot</span></span>
- <span data-ttu-id="1b429-122">Entrez une URL de robot de `http://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="1b429-122">Enter a Bot URL of `http://localhost:3978/api/messages`</span></span>

## <a name="deploy-the-bot-to-azure"></a><span data-ttu-id="1b429-123">Déployer le bot sur Azure</span><span class="sxs-lookup"><span data-stu-id="1b429-123">Deploy the bot to Azure</span></span>

<span data-ttu-id="1b429-124">Pour en savoir plus sur le déploiement d’un bot sur Azure, voir [Deploy Your bot to Azure](https://aka.ms/azuredeployment) pour obtenir la liste complète des instructions de déploiement.</span><span class="sxs-lookup"><span data-stu-id="1b429-124">To learn more about deploying a bot to Azure, see [Deploy your bot to Azure](https://aka.ms/azuredeployment) for a complete list of deployment instructions.</span></span>

## <a name="further-reading"></a><span data-ttu-id="1b429-125">Autres lectures</span><span class="sxs-lookup"><span data-stu-id="1b429-125">Further reading</span></span>

- [<span data-ttu-id="1b429-126">Documentation de l’infrastructure bot</span><span class="sxs-lookup"><span data-stu-id="1b429-126">Bot Framework Documentation</span></span>](https://docs.botframework.com)
- [<span data-ttu-id="1b429-127">Notions de base sur les robots</span><span class="sxs-lookup"><span data-stu-id="1b429-127">Bot Basics</span></span>](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [<span data-ttu-id="1b429-128">Traitement des activités</span><span class="sxs-lookup"><span data-stu-id="1b429-128">Activity processing</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [<span data-ttu-id="1b429-129">Présentation du service de robots Azure</span><span class="sxs-lookup"><span data-stu-id="1b429-129">Azure Bot Service Introduction</span></span>](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [<span data-ttu-id="1b429-130">Documentation du service de robot Azure</span><span class="sxs-lookup"><span data-stu-id="1b429-130">Azure Bot Service Documentation</span></span>](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [<span data-ttu-id="1b429-131">Outils CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="1b429-131">.NET Core CLI tools</span></span>](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [<span data-ttu-id="1b429-132">Interface de commande de commande Azure</span><span class="sxs-lookup"><span data-stu-id="1b429-132">Azure CLI</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="1b429-133">Portail Azure</span><span class="sxs-lookup"><span data-stu-id="1b429-133">Azure Portal</span></span>](https://portal.azure.com)
- [<span data-ttu-id="1b429-134">Compréhension de la langue à l’aide de LUIS</span><span class="sxs-lookup"><span data-stu-id="1b429-134">Language Understanding using LUIS</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [<span data-ttu-id="1b429-135">Service de connecteur de canal et de robot</span><span class="sxs-lookup"><span data-stu-id="1b429-135">Channels and Bot Connector Service</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

<span data-ttu-id="1b429-136">Généré à l’aide de `dotnet new echobot` v 4.10.2</span><span class="sxs-lookup"><span data-stu-id="1b429-136">Generated using `dotnet new echobot` v4.10.2</span></span>
