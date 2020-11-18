---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347806"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez utiliser le **OAuthPrompt** de l’infrastructure bot pour implémenter l’authentification dans le bot et acquérir des jetons d’accès pour appeler l’API Microsoft Graph.

1. Ouvrez **./appsettings.jssur** et effectuez les modifications suivantes.

    - Remplacez la valeur de `MicrosoftAppId` par l’ID d’application de l’inscription de l’application bot de votre **calendrier Graph** .
    - Modifiez la valeur de `MicrosoftAppPassword` à la clé secrète client de votre **calendrier Graph** .
    - Ajoutez une valeur nommée `ConnectionName` avec la valeur `GraphBotAuth` .

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > Si vous avez utilisé une valeur autre que `GraphBotAuth` pour le nom de votre entrée dans les **paramètres de connexion OAuth** dans le portail Azure, utilisez cette valeur pour l' `ConnectionName` entrée.

## <a name="implement-dialogs"></a>Mettre en œuvre des boîtes de dialogue

1. Créez un répertoire dans la racine du projet nommé **Dialogs**. Créez un fichier dans le répertoire **./dialogs** nommé **LogoutDialog.cs** et ajoutez le code suivant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    Cette boîte de dialogue fournit une classe de base pour toutes les autres boîtes de dialogue du bot à partir desquelles dériver. Cela permet à l’utilisateur de se déconnecter, quel que soit l’endroit où il se trouve dans les boîtes de dialogue du robot.

1. Créez un fichier dans le répertoire **./dialogs** nommé **MainDialog.cs** et ajoutez le code suivant.

    ```csharp
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Dialogs.Choices;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;

    namespace CalendarBot.Dialogs
    {
        public class MainDialog : LogoutDialog
        {
            const string NO_PROMPT = "no-prompt";
            protected readonly ILogger _logger;

            public MainDialog(IConfiguration configuration, ILogger<MainDialog> logger)
                : base(nameof(MainDialog), configuration["ConnectionName"])
            {
                _logger = logger;

                // OAuthPrompt dialog handles the authentication and token
                // acquisition
                AddDialog(new OAuthPrompt(
                    nameof(OAuthPrompt),
                    new OAuthPromptSettings
                    {
                        ConnectionName = ConnectionName,
                        Text = "Please login",
                        Title = "Login",
                        Timeout = 300000, // User has 5 minutes to login
                    }));

                AddDialog(new ChoicePrompt(nameof(ChoicePrompt)));

                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    LoginPromptStepAsync,
                    ProcessLoginStepAsync,
                    PromptUserStepAsync,
                    CommandStepAsync,
                    ProcessStepAsync,
                    ReturnToPromptStepAsync
                }));

                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }

            private async Task<DialogTurnResult> LoginPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessLoginStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                // Get the token from the previous step. If it's there, login was successful
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;
                    if (!string.IsNullOrEmpty(tokenResponse?.Token))
                    {
                        await stepContext.Context.SendActivityAsync(
                            MessageFactory.Text("You are now logged in."), cancellationToken);
                        return await stepContext.NextAsync(null, cancellationToken);
                    }
                }

                await stepContext.Context.SendActivityAsync(
                    MessageFactory.Text("Login was not successful please try again."), cancellationToken);
                return await stepContext.EndDialogAsync();
            }

            private async Task<DialogTurnResult> PromptUserStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                var options = new PromptOptions
                {
                    Prompt = MessageFactory.Text("Please choose an option below"),
                    Choices = new List<Choice> {
                        new Choice { Value = "Show token" },
                        new Choice { Value = "Show me" },
                        new Choice { Value = "Show calendar" },
                        new Choice { Value = "Add event" },
                        new Choice { Value = "Log out" },
                    }
                };

                return await stepContext.PromptAsync(
                    nameof(ChoicePrompt),
                    options,
                    cancellationToken);
            }

            private async Task<DialogTurnResult> CommandStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Save the command the user entered so we can get it back after
                // the OAuthPrompt completes
                var foundChoice = stepContext.Result as FoundChoice;
                // Result could be a FoundChoice (if user selected a choice button)
                // or a string (if user just typed something)
                stepContext.Values["command"] = foundChoice?.Value ?? stepContext.Result;

                // There is no reason to store the token locally in the bot because we can always just call
                // the OAuth prompt to get the token or get a new token if needed. The prompt completes silently
                // if the user is already signed in.
                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;

                    // If we have the token use the user is authenticated so we may use it to make API calls.
                    if (tokenResponse?.Token != null)
                    {
                        var command = ((string)stepContext.Values["command"] ?? string.Empty).ToLowerInvariant();

                        if (command.StartsWith("show token"))
                        {
                            // Show the user's token - for testing and troubleshooting
                            // Generally production apps should not display access tokens
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text($"Your token is: {tokenResponse.Token}"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show me"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show calendar"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("add event"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I'm sorry, I didn't understand. Please try again."),
                                cancellationToken);
                        }
                    }
                }
                else
                {
                    await stepContext.Context.SendActivityAsync(
                        MessageFactory.Text("We couldn't log you in. Please try again later."),
                        cancellationToken);
                    return await stepContext.EndDialogAsync(cancellationToken: cancellationToken);
                }

                // Go to the next step
                return await stepContext.NextAsync(cancellationToken: cancellationToken);
            }

            private async Task<DialogTurnResult> ReturnToPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Restart the dialog, but skip the initial login prompt
                return await stepContext.ReplaceDialogAsync(InitialDialogId, NO_PROMPT, cancellationToken);
            }
        }
    }
    ```

    Prenez un moment pour passer en revue ce code.

    - Dans le constructeur, il configure un [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) avec un ensemble d’étapes qui se produisent dans l’ordre.
        - `LoginPromptStepAsync`Il envoie un **OAuthPrompt**. Si l’utilisateur n’est pas connecté, il envoie une invite d’interface utilisateur à l’utilisateur.
        - `ProcessLoginStepAsync`Il vérifie si la connexion a réussi et envoie une confirmation.
        - `PromptUserStepAsync`Il envoie un **ChoicePrompt** avec les commandes disponibles.
        - `CommandStepAsync`Il enregistre le choix de l’utilisateur, puis renvoie un **OAuthPrompt**.
        - Dans `ProcessStepAsync` ce dernier, une action est effectuée en fonction de la commande reçue.
        - Au `ReturnToPromptStepAsync` début, il démarre l’opération en cascade, mais transmet un indicateur pour ignorer la connexion initiale de l’utilisateur.

## <a name="update-calendarbot"></a>Mettre à jour CalendarBot

L’étape suivante consiste à mettre à jour **CalendarBot** afin d’utiliser ces nouvelles boîtes de dialogue.

1. Ouvrez **./bots/CalendarBot.cs** et remplacez l’intégralité de son contenu par le code suivant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    Voici un bref résumé des modifications.

    - Modification de la classe **CalendarBot** en classe de modèle, réception d’une **boîte de dialogue**.
    - Modification de la classe **CalendarBot** pour étendre **TeamsActivityHandler**, ce qui permet de se connecter à Microsoft Teams.
    - Ajout de substitutions de méthode supplémentaires pour activer l’authentification.

## <a name="update-startupcs"></a>Mettre à jour Startup.cs

La dernière étape consiste à mettre à jour la `ConfigureServices` méthode pour ajouter les services nécessaires à l’authentification et la nouvelle boîte de dialogue.

1. Ouvrez **./Startup.cs** et supprimez la `services.AddTransient<IBot, Bots.CalendarBot>();` ligne de la `ConfigureServices` méthode.

1. Insérez le code suivant à la fin de la `ConfigureServices` méthode.

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a>Authentification de test

1. Enregistrez toutes vos modifications et démarrez le robot avec `dotnet run` .

1. Ouvrez l’émulateur de l’infrastructure bot. Sélectionnez le menu **fichier** , puis **nouvelle configuration de bot..**..

1. Renseignez les champs comme suit.

    - **Nom du robot :**`CalendarBot`
    - **URL du point de terminaison :**`https://localhost:3978/api/messages`
    - **ID d’application Microsoft :** ID d’application de l’inscription de l’application bot de votre **calendrier Graph**
    - **Mot de passe Microsoft App :** clé secrète du client de votre **calendrier Graph**
    - **Chiffrez les clés stockées dans la configuration de votre bot :** Activé

    ![Capture d’écran de la boîte de dialogue nouvelle configuration de bot](images/new-bot-config.png)

1. Sélectionnez **enregistrer et connexion**. Une fois que l’émulateur s’est connecté, vous devriez voir `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`

1. Tapez du texte et envoyez-le au bot. Le bot répond avec une invite de connexion.

1. Sélectionnez le bouton **connexion** . L’émulateur vous invite à confirmer l’URL qui commence par `oauthlink://https://token.botframeworkcom` . Sélectionnez **confirmer** pour continuer.

1. Dans la fenêtre contextuelle, connectez-vous avec votre compte Microsoft 365. Passez en revue les autorisations demandées et acceptez.

1. Une fois l’authentification et le consentement terminés, la fenêtre contextuelle fournit un code de validation. Copiez le code et fermez la fenêtre.

    ![Capture d’écran du code de validation de l’émulateur de robot Framework](images/validation-code.png)

1. Entrez le code de validation dans la fenêtre de conversation pour terminer la connexion.

    ![Capture d’écran de la conversation de connexion avec l’exemple de robot](images/bot-login.png)

1. Si vous sélectionnez le bouton **afficher le jeton** (ou le type `show token` ), le bot affiche le jeton d’accès. Le bouton **déconnexion** (ou saisie `log out` ) vous déconnecte.

> [!TIP]
> Le message d’erreur suivant peut s’afficher dans l’émulateur de l’infrastructure bot lors du démarrage d’une conversation avec le bot.
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> Dans ce cas, fermez l’émulateur et redémarrez-le.
