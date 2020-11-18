---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347786"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="16ef9-101">Dans cette section, vous allez utiliser le kit de développement logiciel (SDK) Microsoft Graph pour ajouter un événement au calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-101">In this section you'll use the Microsoft Graph SDK to add an event to the user's calendar.</span></span>

## <a name="implement-a-dialog"></a><span data-ttu-id="16ef9-102">Implémentation d’une boîte de dialogue</span><span class="sxs-lookup"><span data-stu-id="16ef9-102">Implement a dialog</span></span>

<span data-ttu-id="16ef9-103">Commencez par créer une boîte de dialogue personnalisée pour inviter l’utilisateur à entrer les valeurs nécessaires à l’ajout d’un événement à son calendrier.</span><span class="sxs-lookup"><span data-stu-id="16ef9-103">Start by creating a new custom dialog to prompt the user for the values needed to add an event to their calendar.</span></span> <span data-ttu-id="16ef9-104">Cette boîte de dialogue utilise un **WaterfallDialog** pour effectuer les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="16ef9-104">This dialog will use a **WaterfallDialog** to do the following steps.</span></span>

- <span data-ttu-id="16ef9-105">Demander un objet</span><span class="sxs-lookup"><span data-stu-id="16ef9-105">Prompt for a subject</span></span>
- <span data-ttu-id="16ef9-106">Demander à l’utilisateur s’il souhaite inviter des personnes</span><span class="sxs-lookup"><span data-stu-id="16ef9-106">Ask if the user wants to invite people</span></span>
- <span data-ttu-id="16ef9-107">Demander aux participants (si l’utilisateur a répondu oui à l’étape précédente)</span><span class="sxs-lookup"><span data-stu-id="16ef9-107">Prompt for attendees (if user said yes to previous step)</span></span>
- <span data-ttu-id="16ef9-108">Demander une date et une heure de début</span><span class="sxs-lookup"><span data-stu-id="16ef9-108">Prompt for a start date and time</span></span>
- <span data-ttu-id="16ef9-109">Demander une date et une heure de fin</span><span class="sxs-lookup"><span data-stu-id="16ef9-109">Prompt for an end date and time</span></span>
- <span data-ttu-id="16ef9-110">Afficher toutes les valeurs collectées et demander à l’utilisateur de confirmer</span><span class="sxs-lookup"><span data-stu-id="16ef9-110">Display all of the collected values and ask user to confirm</span></span>
- <span data-ttu-id="16ef9-111">Si l’utilisateur confirme, obtenir le jeton d’accès à partir de **OAuthPrompt**</span><span class="sxs-lookup"><span data-stu-id="16ef9-111">If user confirms, get access token from **OAuthPrompt**</span></span>
- <span data-ttu-id="16ef9-112">Créer l’événement</span><span class="sxs-lookup"><span data-stu-id="16ef9-112">Create the event</span></span>

1. <span data-ttu-id="16ef9-113">Créez un fichier dans le répertoire **./dialogs** nommé **NewEventDialog.cs** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="16ef9-113">Create a new file in the **./Dialogs** directory named **NewEventDialog.cs** and add the following code.</span></span>

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;
    using CalendarBot.Graph;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    using TimexTypes = Microsoft.Recognizers.Text.DataTypes.TimexExpression.Constants.TimexTypes;

    namespace CalendarBot.Dialogs
    {
        public class NewEventDialog : LogoutDialog
        {
            protected readonly ILogger _logger;
            private readonly IGraphClientService _graphClientService;

            public NewEventDialog(
                IConfiguration configuration,
                IGraphClientService graphClientService)
                : base(nameof(NewEventDialog), configuration["ConnectionName"])
            {

            }

            // Generate a DateTime from the list of
            // DateTimeResolutions provided by the DateTimePrompt
            private static DateTime GetDateTimeFromResolutions(IList<DateTimeResolution> resolutions)
            {
                var timex = new TimexProperty(resolutions[0].Timex);

                // Handle the "now" case
                if (timex.Now ?? false)
                {
                    return DateTime.Now;
                }

                // Otherwise generate a DateTime
                return TimexHelpers.DateFromTimex(timex);
            }
        }
    }
    ```

    <span data-ttu-id="16ef9-114">Il s’agit de l’environnement Shell d’une nouvelle boîte de dialogue qui invite l’utilisateur à entrer les valeurs nécessaires à l’ajout d’un événement à son calendrier.</span><span class="sxs-lookup"><span data-stu-id="16ef9-114">This is the shell of a new dialog that will prompt the user for the values needed to add an event to their calendar.</span></span>

1. <span data-ttu-id="16ef9-115">Ajoutez la fonction suivante à la classe **NewEventDialog** pour inviter l’utilisateur à entrer un objet.</span><span class="sxs-lookup"><span data-stu-id="16ef9-115">Add the following function to the **NewEventDialog** class to prompt the user for a subject.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. <span data-ttu-id="16ef9-116">Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker l’objet que l’utilisateur a donné à l’étape précédente et demandez-lui s’il souhaite ajouter des participants.</span><span class="sxs-lookup"><span data-stu-id="16ef9-116">Add the following function to the **NewEventDialog** class to store the subject the user gave in the previous step, and to ask if they want to add attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. <span data-ttu-id="16ef9-117">Ajoutez la fonction suivante à la classe **NewEventDialog** pour vérifier la réponse de l’utilisateur à partir de l’étape précédente et inviter l’utilisateur à fournir une liste de participants, le cas échéant.</span><span class="sxs-lookup"><span data-stu-id="16ef9-117">Add the following function to the **NewEventDialog** class to check the user's response from the previous step and prompt the user for a list of attendees if needed.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. <span data-ttu-id="16ef9-118">Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la liste des participants de l’étape précédente (le cas échéant) et inviter l’utilisateur à entrer une date et une heure de début.</span><span class="sxs-lookup"><span data-stu-id="16ef9-118">Add the following function to the **NewEventDialog** class to store the list of attendees from the previous step (if present) and prompt the user for a start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. <span data-ttu-id="16ef9-119">Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la valeur de début de l’étape précédente et inviter l’utilisateur à indiquer une date et une heure de fin.</span><span class="sxs-lookup"><span data-stu-id="16ef9-119">Add the following function to the **NewEventDialog** class to store the start value from the previous step and prompt the user for an end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. <span data-ttu-id="16ef9-120">Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la valeur de fin de l’étape précédente et demandez à l’utilisateur de confirmer toutes les entrées.</span><span class="sxs-lookup"><span data-stu-id="16ef9-120">Add the following function to the **NewEventDialog** class to store the end value from the previous step and ask the user to confirm all of the inputs.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. <span data-ttu-id="16ef9-121">Ajoutez la fonction suivante à la classe **NewEventDialog** pour vérifier la réponse de l’utilisateur à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="16ef9-121">Add the following function to the **NewEventDialog** class to check the user's response from the previous step.</span></span> <span data-ttu-id="16ef9-122">Si l’utilisateur confirme les entrées, utilisez la classe **OAuthPrompt** pour obtenir un jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="16ef9-122">If the user confirms the inputs, use the **OAuthPrompt** class to get an access token.</span></span> <span data-ttu-id="16ef9-123">Sinon, terminez la boîte de dialogue.</span><span class="sxs-lookup"><span data-stu-id="16ef9-123">Otherwise, end the dialog.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. <span data-ttu-id="16ef9-124">Ajoutez la fonction suivante à la classe **NewEventDialog** pour utiliser le kit de développement logiciel (SDK) Microsoft Graph afin de créer le nouvel événement.</span><span class="sxs-lookup"><span data-stu-id="16ef9-124">Add the following function to the **NewEventDialog** class to use the Microsoft Graph SDK to create the new event.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    <span data-ttu-id="16ef9-125">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="16ef9-125">Consider what this code does.</span></span>

    - <span data-ttu-id="16ef9-126">Il obtient le **MailboxSettings** de l’utilisateur pour déterminer le fuseau horaire préféré de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-126">It gets the user's **MailboxSettings** to determine the user's preferred time zone.</span></span>
    - <span data-ttu-id="16ef9-127">Il crée un objet **Event** à l’aide des valeurs fournies par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-127">It creates an **Event** object using the values provided by the user.</span></span> <span data-ttu-id="16ef9-128">Notez que les propriétés **Start** et **end** sont définies avec le fuseau horaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-128">Note that the **Start** and **End** properties are set with the user's time zone.</span></span>
    - <span data-ttu-id="16ef9-129">Il crée l’événement sur le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-129">It creates the event on the user's calendar.</span></span>

## <a name="add-validation"></a><span data-ttu-id="16ef9-130">Ajouter une validation</span><span class="sxs-lookup"><span data-stu-id="16ef9-130">Add validation</span></span>

<span data-ttu-id="16ef9-131">À présent, ajoutez une validation à l’entrée de l’utilisateur afin d’éviter les erreurs lors de la création de l’événement avec Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="16ef9-131">Now add validation to the user's input to avoid errors when creating the event with Microsoft Graph.</span></span> <span data-ttu-id="16ef9-132">Nous souhaitons s’assurer que :</span><span class="sxs-lookup"><span data-stu-id="16ef9-132">We want to make sure that:</span></span>

- <span data-ttu-id="16ef9-133">Si l’utilisateur donne une liste de participants, il doit s’agir d’une liste d’adresses de messagerie valides, séparées par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="16ef9-133">If the user gives an attendee list, it should be a semicolon-delimited list of valid email addresses.</span></span>
- <span data-ttu-id="16ef9-134">La date et l’heure de début doivent être une date et une heure valides.</span><span class="sxs-lookup"><span data-stu-id="16ef9-134">The start date/time should be a valid date and time.</span></span>
- <span data-ttu-id="16ef9-135">La date et l’heure de fin doivent être une date et une heure valides, et doivent être postérieures au début.</span><span class="sxs-lookup"><span data-stu-id="16ef9-135">The end date/time should be a valid date and time, and should be later than the start.</span></span>

1. <span data-ttu-id="16ef9-136">Ajoutez la fonction suivante à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour les participants.</span><span class="sxs-lookup"><span data-stu-id="16ef9-136">Add the following function to the **NewEventDialog** class to validate the user's entry for attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. <span data-ttu-id="16ef9-137">Ajoutez les fonctions suivantes à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour la date et l’heure de début.</span><span class="sxs-lookup"><span data-stu-id="16ef9-137">Add the following functions to the **NewEventDialog** class to validate the user's entry for start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. <span data-ttu-id="16ef9-138">Ajoutez la fonction suivante à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour la date et l’heure de fin.</span><span class="sxs-lookup"><span data-stu-id="16ef9-138">Add the following function to the **NewEventDialog** class to validate the user's entry for end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a><span data-ttu-id="16ef9-139">Ajouter des étapes à WaterfallDialog</span><span class="sxs-lookup"><span data-stu-id="16ef9-139">Add steps to WaterfallDialog</span></span>

<span data-ttu-id="16ef9-140">Maintenant que vous avez toutes les « étapes » de la boîte de dialogue, la dernière étape consiste à les ajouter à un **WaterfallDialog** dans le constructeur, puis à ajouter l' **NewEventDialog** à l' **MainDialog**.</span><span class="sxs-lookup"><span data-stu-id="16ef9-140">Now that you have all of the "steps" for the dialog, the last step is to add them to a **WaterfallDialog** in the constructor, then add the **NewEventDialog** to the **MainDialog**.</span></span>

1. <span data-ttu-id="16ef9-141">Recherchez le constructeur **NewEventDialog** et ajoutez-y le code suivant.</span><span class="sxs-lookup"><span data-stu-id="16ef9-141">Locate the **NewEventDialog** constructor and add the following code to it.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    <span data-ttu-id="16ef9-142">Cette opération ajoute toutes les boîtes de dialogue utilisées et ajoute toutes les fonctions que vous avez implémentées en tant qu’étapes dans le **WaterfallDialog**.</span><span class="sxs-lookup"><span data-stu-id="16ef9-142">This adds all of the dialogs that are used, and adds all of the functions you implemented as steps in the **WaterfallDialog**.</span></span>

1. <span data-ttu-id="16ef9-143">Ouvrez **./dialogs/MainDialog.cs** et ajoutez la ligne suivante au constructeur.</span><span class="sxs-lookup"><span data-stu-id="16ef9-143">Open **./Dialogs/MainDialog.cs** and add the following line to the constructor.</span></span>

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. <span data-ttu-id="16ef9-144">Remplacez le code à l’intérieur du `else if (command.StartsWith("add event"))` bloc `ProcessStepAsync` par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="16ef9-144">Replace the code inside the `else if (command.StartsWith("add event"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. <span data-ttu-id="16ef9-145">Enregistrez toutes vos modifications et redémarrez le robot.</span><span class="sxs-lookup"><span data-stu-id="16ef9-145">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="16ef9-146">Utilisez l’émulateur de l’infrastructure bot pour vous connecter au bot et vous connecter.</span><span class="sxs-lookup"><span data-stu-id="16ef9-146">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="16ef9-147">Sélectionnez le bouton **Ajouter un événement** .</span><span class="sxs-lookup"><span data-stu-id="16ef9-147">Select the **Add event** button.</span></span>

1. <span data-ttu-id="16ef9-148">Répondez aux invites pour créer un événement.</span><span class="sxs-lookup"><span data-stu-id="16ef9-148">Respond to the prompts to create a new event.</span></span> <span data-ttu-id="16ef9-149">Notez que lorsque vous êtes invité à entrer les valeurs de début et de fin, vous pouvez utiliser des expressions telles que « aujourd’hui à 3PM » ou « maintenant ».</span><span class="sxs-lookup"><span data-stu-id="16ef9-149">Note that when prompted for start and end values, you can use phrases like "today at 3PM", or "now".</span></span>

    ![Capture d’écran de la boîte de dialogue Ajouter un événement dans l’émulateur de l’infrastructure bot](images/add-event.png)
