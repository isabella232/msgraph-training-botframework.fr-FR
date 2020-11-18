---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347818"
---
# <a name="graphcalendarbot"></a>GraphCalendarBot

Exemple d’écho v4 v4 de robot.

Ce robot a été créé à l’aide de l' [infrastructure de robot](https://dev.botframework.com), il montre comment créer un robot simple qui accepte l’entrée de l’utilisateur et le renvoie en écho.

## <a name="prerequisites"></a>Conditions préalables

- [Kit de développement .net Core SDK](https://dotnet.microsoft.com/download) version 3,1

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a>Pour essayer cet exemple

- Dans un terminal, accédez à `GraphCalendarBot`

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- Exécuter le bot à partir d’un terminal ou de Visual Studio, choisissez l’option A ou B.

  A) à partir d’un terminal

  ```bash
  # run the bot
  dotnet run
  ```

  B) ou à partir de Visual Studio

  - Lancer Visual Studio
  - Fichier-> > projet/solution
  - Accéder au `GraphCalendarBot` dossier
  - Sélectionner un `GraphCalendarBot.csproj` fichier
  - Appuyez sur `F5` pour exécuter le projet

## <a name="testing-the-bot-using-bot-framework-emulator"></a>Test du robot à l’aide de l’émulateur de l’infrastructure bot

L’émulateur de l' [infrastructure bot](https://github.com/microsoft/botframework-emulator) est une application de bureau qui permet aux développeurs de moteurs de tester et de déboguer leurs robots sur localhost ou de s’exécuter à distance via un tunnel.

- Installer l' [émulateur de l'](https://github.com/Microsoft/BotFramework-Emulator/releases) infrastructure bot 4.9.0 ou une version ultérieure

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a>Se connecter au bot à l’aide de l’émulateur de l’infrastructure bot

- Lancer l’émulateur de l’infrastructure bot
- Robot ouvert > fichier
- Entrez une URL de robot de `http://localhost:3978/api/messages`

## <a name="deploy-the-bot-to-azure"></a>Déployer le bot sur Azure

Pour en savoir plus sur le déploiement d’un bot sur Azure, voir [Deploy Your bot to Azure](https://aka.ms/azuredeployment) pour obtenir la liste complète des instructions de déploiement.

## <a name="further-reading"></a>Autres lectures

- [Documentation de l’infrastructure bot](https://docs.botframework.com)
- [Notions de base sur les robots](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [Traitement des activités](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [Présentation du service de robots Azure](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Documentation du service de robot Azure](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [Outils CLI .NET Core](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [Interface de commande de commande Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Portail Azure](https://portal.azure.com)
- [Compréhension de la langue à l’aide de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [Service de connecteur de canal et de robot](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

Généré à l’aide de `dotnet new echobot` v 4.10.2
