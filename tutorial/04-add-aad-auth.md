---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347806"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3bc97-101">Dans cet exercice, vous allez utiliser le **OAuthPrompt** de l’infrastructure bot pour implémenter l’authentification dans le bot et acquérir des jetons d’accès pour appeler l’API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="3bc97-101">In this exercise you will use the Bot Framework's **OAuthPrompt** to implement authentication in the bot, and acquire access tokens for calling the Microsoft Graph API.</span></span>

1. <span data-ttu-id="3bc97-102">Ouvrez **./appsettings.jssur** et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="3bc97-102">Open **./appsettings.json** and make the following changes.</span></span>

    - <span data-ttu-id="3bc97-103">Remplacez la valeur de `MicrosoftAppId` par l’ID d’application de l’inscription de l’application bot de votre **calendrier Graph** .</span><span class="sxs-lookup"><span data-stu-id="3bc97-103">Change the value of `MicrosoftAppId` to the application ID of your **Graph Calendar Bot** app registration.</span></span>
    - <span data-ttu-id="3bc97-104">Modifiez la valeur de `MicrosoftAppPassword` à la clé secrète client de votre **calendrier Graph** .</span><span class="sxs-lookup"><span data-stu-id="3bc97-104">Change the value of `MicrosoftAppPassword` to your **Graph Calendar Bot** client secret.</span></span>
    - <span data-ttu-id="3bc97-105">Ajoutez une valeur nommée `ConnectionName` avec la valeur `GraphBotAuth` .</span><span class="sxs-lookup"><span data-stu-id="3bc97-105">Add a value named `ConnectionName` with a value of `GraphBotAuth`.</span></span>

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > <span data-ttu-id="3bc97-106">Si vous avez utilisé une valeur autre que `GraphBotAuth` pour le nom de votre entrée dans les **paramètres de connexion OAuth** dans le portail Azure, utilisez cette valeur pour l' `ConnectionName` entrée.</span><span class="sxs-lookup"><span data-stu-id="3bc97-106">If you used a value other than `GraphBotAuth` for the name of your entry in **OAuth Connection Settings** in the Azure Portal, use that value for the `ConnectionName` entry.</span></span>

## <a name="implement-dialogs"></a><span data-ttu-id="3bc97-107">Mettre en œuvre des boîtes de dialogue</span><span class="sxs-lookup"><span data-stu-id="3bc97-107">Implement dialogs</span></span>

1. <span data-ttu-id="3bc97-108">Créez un répertoire dans la racine du projet nommé **Dialogs**.</span><span class="sxs-lookup"><span data-stu-id="3bc97-108">Create a new directory in the root of the project named **Dialogs**.</span></span> <span data-ttu-id="3bc97-109">Créez un fichier dans le répertoire **./dialogs** nommé **LogoutDialog.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="3bc97-109">Create a new file in the **./Dialogs** directory named **LogoutDialog.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    <span data-ttu-id="3bc97-110">Cette boîte de dialogue fournit une classe de base pour toutes les autres boîtes de dialogue du bot à partir desquelles dériver.</span><span class="sxs-lookup"><span data-stu-id="3bc97-110">This dialog provides a base class for all of the other dialogs in the bot to derive from.</span></span> <span data-ttu-id="3bc97-111">Cela permet à l’utilisateur de se déconnecter, quel que soit l’endroit où il se trouve dans les boîtes de dialogue du robot.</span><span class="sxs-lookup"><span data-stu-id="3bc97-111">This allows the user to log out no matter where they are in the bot's dialogs.</span></span>

1. <span data-ttu-id="3bc97-112">Créez un fichier dans le répertoire **./dialogs** nommé **MainDialog.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="3bc97-112">Create a new file in the **./Dialogs** directory named **MainDialog.cs** and add the following code.</span></span>

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

    <span data-ttu-id="3bc97-113">Prenez un moment pour passer en revue ce code.</span><span class="sxs-lookup"><span data-stu-id="3bc97-113">Take a moment to review this code.</span></span>

    - <span data-ttu-id="3bc97-114">Dans le constructeur, il configure un [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) avec un ensemble d’étapes qui se produisent dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="3bc97-114">In the constructor, it sets up a [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) with a set of steps that occur in order.</span></span>
        - <span data-ttu-id="3bc97-115">`LoginPromptStepAsync`Il envoie un **OAuthPrompt**.</span><span class="sxs-lookup"><span data-stu-id="3bc97-115">In `LoginPromptStepAsync` it sends an **OAuthPrompt**.</span></span> <span data-ttu-id="3bc97-116">Si l’utilisateur n’est pas connecté, il envoie une invite d’interface utilisateur à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3bc97-116">If the user isn't logged in, this will send a UI prompt to the user.</span></span>
        - <span data-ttu-id="3bc97-117">`ProcessLoginStepAsync`Il vérifie si la connexion a réussi et envoie une confirmation.</span><span class="sxs-lookup"><span data-stu-id="3bc97-117">In `ProcessLoginStepAsync` it checks if the login was successful and sends a confirmation.</span></span>
        - <span data-ttu-id="3bc97-118">`PromptUserStepAsync`Il envoie un **ChoicePrompt** avec les commandes disponibles.</span><span class="sxs-lookup"><span data-stu-id="3bc97-118">In `PromptUserStepAsync` it sends a **ChoicePrompt** with the available commands.</span></span>
        - <span data-ttu-id="3bc97-119">`CommandStepAsync`Il enregistre le choix de l’utilisateur, puis renvoie un **OAuthPrompt**.</span><span class="sxs-lookup"><span data-stu-id="3bc97-119">In `CommandStepAsync` it saves the user's choice, then resends an **OAuthPrompt**.</span></span>
        - <span data-ttu-id="3bc97-120">Dans `ProcessStepAsync` ce dernier, une action est effectuée en fonction de la commande reçue.</span><span class="sxs-lookup"><span data-stu-id="3bc97-120">In `ProcessStepAsync` it takes action based on the command received.</span></span>
        - <span data-ttu-id="3bc97-121">Au `ReturnToPromptStepAsync` début, il démarre l’opération en cascade, mais transmet un indicateur pour ignorer la connexion initiale de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3bc97-121">In `ReturnToPromptStepAsync` it starts the waterfall over, but passes a flag to skip the initial user login.</span></span>

## <a name="update-calendarbot"></a><span data-ttu-id="3bc97-122">Mettre à jour CalendarBot</span><span class="sxs-lookup"><span data-stu-id="3bc97-122">Update CalendarBot</span></span>

<span data-ttu-id="3bc97-123">L’étape suivante consiste à mettre à jour **CalendarBot** afin d’utiliser ces nouvelles boîtes de dialogue.</span><span class="sxs-lookup"><span data-stu-id="3bc97-123">The next step is to update **CalendarBot** to use these new dialogs.</span></span>

1. <span data-ttu-id="3bc97-124">Ouvrez **./bots/CalendarBot.cs** et remplacez l’intégralité de son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="3bc97-124">Open **./Bots/CalendarBot.cs** and replace its entire contents with the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    <span data-ttu-id="3bc97-125">Voici un bref résumé des modifications.</span><span class="sxs-lookup"><span data-stu-id="3bc97-125">Here's a brief summary of the changes.</span></span>

    - <span data-ttu-id="3bc97-126">Modification de la classe **CalendarBot** en classe de modèle, réception d’une **boîte de dialogue**.</span><span class="sxs-lookup"><span data-stu-id="3bc97-126">Changed the **CalendarBot** class to be a template class, receiving a **Dialog**.</span></span>
    - <span data-ttu-id="3bc97-127">Modification de la classe **CalendarBot** pour étendre **TeamsActivityHandler**, ce qui permet de se connecter à Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="3bc97-127">Changed the **CalendarBot** class to extend **TeamsActivityHandler**, allowing it to sign in in Microsoft Teams.</span></span>
    - <span data-ttu-id="3bc97-128">Ajout de substitutions de méthode supplémentaires pour activer l’authentification.</span><span class="sxs-lookup"><span data-stu-id="3bc97-128">Added additional method overrides to enable authentication.</span></span>

## <a name="update-startupcs"></a><span data-ttu-id="3bc97-129">Mettre à jour Startup.cs</span><span class="sxs-lookup"><span data-stu-id="3bc97-129">Update Startup.cs</span></span>

<span data-ttu-id="3bc97-130">La dernière étape consiste à mettre à jour la `ConfigureServices` méthode pour ajouter les services nécessaires à l’authentification et la nouvelle boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="3bc97-130">The final step is to update the `ConfigureServices` method to add the services needed for authentication and the new dialog.</span></span>

1. <span data-ttu-id="3bc97-131">Ouvrez **./Startup.cs** et supprimez la `services.AddTransient<IBot, Bots.CalendarBot>();` ligne de la `ConfigureServices` méthode.</span><span class="sxs-lookup"><span data-stu-id="3bc97-131">Open **./Startup.cs** and remove the `services.AddTransient<IBot, Bots.CalendarBot>();` line from the `ConfigureServices` method.</span></span>

1. <span data-ttu-id="3bc97-132">Insérez le code suivant à la fin de la `ConfigureServices` méthode.</span><span class="sxs-lookup"><span data-stu-id="3bc97-132">Insert the following code at the end of the `ConfigureServices` method.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a><span data-ttu-id="3bc97-133">Authentification de test</span><span class="sxs-lookup"><span data-stu-id="3bc97-133">Test authentication</span></span>

1. <span data-ttu-id="3bc97-134">Enregistrez toutes vos modifications et démarrez le robot avec `dotnet run` .</span><span class="sxs-lookup"><span data-stu-id="3bc97-134">Save all of your changes and start the bot with `dotnet run`.</span></span>

1. <span data-ttu-id="3bc97-135">Ouvrez l’émulateur de l’infrastructure bot.</span><span class="sxs-lookup"><span data-stu-id="3bc97-135">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="3bc97-136">Sélectionnez le menu **fichier** , puis **nouvelle configuration de bot..**..</span><span class="sxs-lookup"><span data-stu-id="3bc97-136">Select the **File** menu, then **New Bot Configuration...**.</span></span>

1. <span data-ttu-id="3bc97-137">Renseignez les champs comme suit.</span><span class="sxs-lookup"><span data-stu-id="3bc97-137">Fill in the fields as follows.</span></span>

    - <span data-ttu-id="3bc97-138">**Nom du robot :**`CalendarBot`</span><span class="sxs-lookup"><span data-stu-id="3bc97-138">**Bot name:** `CalendarBot`</span></span>
    - <span data-ttu-id="3bc97-139">**URL du point de terminaison :**`https://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="3bc97-139">**Endpoint URL:** `https://localhost:3978/api/messages`</span></span>
    - <span data-ttu-id="3bc97-140">**ID d’application Microsoft :** ID d’application de l’inscription de l’application bot de votre **calendrier Graph**</span><span class="sxs-lookup"><span data-stu-id="3bc97-140">**Microsoft App ID:** the application ID of your **Graph Calendar Bot** app registration</span></span>
    - <span data-ttu-id="3bc97-141">**Mot de passe Microsoft App :** clé secrète du client de votre **calendrier Graph**</span><span class="sxs-lookup"><span data-stu-id="3bc97-141">**Microsoft App password:** your **Graph Calendar Bot** client secret</span></span>
    - <span data-ttu-id="3bc97-142">**Chiffrez les clés stockées dans la configuration de votre bot :** Activé</span><span class="sxs-lookup"><span data-stu-id="3bc97-142">**Encrypt keys stored in your bot configuration:** Enabled</span></span>

    ![Capture d’écran de la boîte de dialogue nouvelle configuration de bot](images/new-bot-config.png)

1. <span data-ttu-id="3bc97-144">Sélectionnez **enregistrer et connexion**.</span><span class="sxs-lookup"><span data-stu-id="3bc97-144">Select **Save and connect**.</span></span> <span data-ttu-id="3bc97-145">Une fois que l’émulateur s’est connecté, vous devriez voir `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span><span class="sxs-lookup"><span data-stu-id="3bc97-145">After the emulator connects, you should see `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span></span>

1. <span data-ttu-id="3bc97-146">Tapez du texte et envoyez-le au bot.</span><span class="sxs-lookup"><span data-stu-id="3bc97-146">Type some text and send it to the bot.</span></span> <span data-ttu-id="3bc97-147">Le bot répond avec une invite de connexion.</span><span class="sxs-lookup"><span data-stu-id="3bc97-147">The bot responds with a login prompt.</span></span>

1. <span data-ttu-id="3bc97-148">Sélectionnez le bouton **connexion** .</span><span class="sxs-lookup"><span data-stu-id="3bc97-148">Select the **Login** button.</span></span> <span data-ttu-id="3bc97-149">L’émulateur vous invite à confirmer l’URL qui commence par `oauthlink://https://token.botframeworkcom` .</span><span class="sxs-lookup"><span data-stu-id="3bc97-149">The emulator prompts you to confirm the URL that starts with `oauthlink://https://token.botframeworkcom`.</span></span> <span data-ttu-id="3bc97-150">Sélectionnez **confirmer** pour continuer.</span><span class="sxs-lookup"><span data-stu-id="3bc97-150">Select **Confirm** to continue.</span></span>

1. <span data-ttu-id="3bc97-151">Dans la fenêtre contextuelle, connectez-vous avec votre compte Microsoft 365.</span><span class="sxs-lookup"><span data-stu-id="3bc97-151">In the pop-up window, login with your Microsoft 365 account.</span></span> <span data-ttu-id="3bc97-152">Passez en revue les autorisations demandées et acceptez.</span><span class="sxs-lookup"><span data-stu-id="3bc97-152">Review the requested permissions and accept.</span></span>

1. <span data-ttu-id="3bc97-153">Une fois l’authentification et le consentement terminés, la fenêtre contextuelle fournit un code de validation.</span><span class="sxs-lookup"><span data-stu-id="3bc97-153">Once authentication and consent are complete, the pop-up window provides a validation code.</span></span> <span data-ttu-id="3bc97-154">Copiez le code et fermez la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="3bc97-154">Copy the code and close the window.</span></span>

    ![Capture d’écran du code de validation de l’émulateur de robot Framework](images/validation-code.png)

1. <span data-ttu-id="3bc97-156">Entrez le code de validation dans la fenêtre de conversation pour terminer la connexion.</span><span class="sxs-lookup"><span data-stu-id="3bc97-156">Enter the validation code in the chat window to complete the login.</span></span>

    ![Capture d’écran de la conversation de connexion avec l’exemple de robot](images/bot-login.png)

1. <span data-ttu-id="3bc97-158">Si vous sélectionnez le bouton **afficher le jeton** (ou le type `show token` ), le bot affiche le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="3bc97-158">If you select the **Show token** button (or type `show token`), the bot displays the access token.</span></span> <span data-ttu-id="3bc97-159">Le bouton **déconnexion** (ou saisie `log out` ) vous déconnecte.</span><span class="sxs-lookup"><span data-stu-id="3bc97-159">The **Log out** button (or typing `log out`) will log you out.</span></span>

> [!TIP]
> <span data-ttu-id="3bc97-160">Le message d’erreur suivant peut s’afficher dans l’émulateur de l’infrastructure bot lors du démarrage d’une conversation avec le bot.</span><span class="sxs-lookup"><span data-stu-id="3bc97-160">You may receive the following error message in the Bot Framework Emulator when starting a conversation with the bot.</span></span>
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> <span data-ttu-id="3bc97-161">Dans ce cas, fermez l’émulateur et redémarrez-le.</span><span class="sxs-lookup"><span data-stu-id="3bc97-161">If this happens, close the emulator and restart it.</span></span>
