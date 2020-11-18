---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347813"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez créer un projet d’infrastructure de robot.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour créer un projet à l’aide du modèle **Microsoft. Bot. Framework. CSharp. EchoBot** .

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > Si une erreur s’affiche `No templates matched the input template name: echobot.` , installez le modèle à l’aide de la commande suivante et réexécutez la commande précédente.
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. Renommez la classe **EchoBot** par défaut en **CalendarBot**. Ouvrez **./bots/EchoBot.cs** et remplacez toutes les instances de `EchoBot` par `CalendarBot` . Renommez le fichier en **CalendarBot.cs**.

1. Remplacez toutes les instances de `EchoBot` par `CalendarBot` dans les fichiers **. cs** restants.

1. Dans votre interface CLI, remplacez le répertoire actif par le répertoire **GraphCalendarBot** et exécutez la commande suivante pour confirmer la génération du projet.

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a>Ajouter des packages NuGet

Avant de poursuivre, installez des packages NuGet supplémentaires que vous utiliserez plus tard.

- [AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) pour permettre au bot d’envoyer des cartes adaptatives dans les réponses.
- [Microsoft. Bot. Builder. dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) pour ajouter la prise en charge de la boîte de dialogue au bot.
- [Microsoft. Recognizers. Text. Datatypes. TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) pour convertir les expressions Timex retournées à partir des invites bot en objets **DateTime** .
- [Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) pour effectuer des appels Microsoft Graph.

1. Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a>Tester le robot

Avant d’ajouter du code, testez-le pour vous assurer qu’il fonctionne correctement et que l’émulateur de l’infrastructure bot est configuré pour le tester.

1. Démarrez le bot en exécutant la commande suivante.

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > Bien que vous puissiez utiliser n’importe quel éditeur de texte pour modifier les fichiers sources dans le projet, nous vous recommandons d’utiliser [Visual Studio code](https://code.visualstudio.com/). Visual Studio code offre une prise en charge du débogage, IntelliSense et bien plus encore. Si vous utilisez Visual Studio code, vous pouvez démarrer le bot à l’aide du menu **exécuter**  ->  **Démarrer le débogage** .

1. Vérifiez que le robot est en cours d’exécution en ouvrant votre navigateur et en accédant à `http://localhost:3978` . Vous devriez voir que **votre robot est prêt !** Message.

1. Ouvrez l’émulateur de l’infrastructure bot. Sélectionnez le menu **fichier** , puis **ouvrez bot**.

1. Entrez `http://localhost:3978/api/messages` dans l' **URL du robot**, puis sélectionnez **se connecter**.

1. Le bot répond avec `Hello and welcome!` dans la fenêtre de conversation. Envoyez un message au bot et confirmez qu’il renvoie un écho.

    ![Capture d’écran de l’émulateur de l’infrastructure bot connecté au bot](images/test-emulator.png)
