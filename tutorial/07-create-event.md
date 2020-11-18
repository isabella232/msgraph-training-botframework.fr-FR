---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347786"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez utiliser le kit de développement logiciel (SDK) Microsoft Graph pour ajouter un événement au calendrier de l’utilisateur.

## <a name="implement-a-dialog"></a>Implémentation d’une boîte de dialogue

Commencez par créer une boîte de dialogue personnalisée pour inviter l’utilisateur à entrer les valeurs nécessaires à l’ajout d’un événement à son calendrier. Cette boîte de dialogue utilise un **WaterfallDialog** pour effectuer les étapes suivantes.

- Demander un objet
- Demander à l’utilisateur s’il souhaite inviter des personnes
- Demander aux participants (si l’utilisateur a répondu oui à l’étape précédente)
- Demander une date et une heure de début
- Demander une date et une heure de fin
- Afficher toutes les valeurs collectées et demander à l’utilisateur de confirmer
- Si l’utilisateur confirme, obtenir le jeton d’accès à partir de **OAuthPrompt**
- Créer l’événement

1. Créez un fichier dans le répertoire **./dialogs** nommé **NewEventDialog.cs** et ajoutez le code suivant.

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

    Il s’agit de l’environnement Shell d’une nouvelle boîte de dialogue qui invite l’utilisateur à entrer les valeurs nécessaires à l’ajout d’un événement à son calendrier.

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour inviter l’utilisateur à entrer un objet.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker l’objet que l’utilisateur a donné à l’étape précédente et demandez-lui s’il souhaite ajouter des participants.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour vérifier la réponse de l’utilisateur à partir de l’étape précédente et inviter l’utilisateur à fournir une liste de participants, le cas échéant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la liste des participants de l’étape précédente (le cas échéant) et inviter l’utilisateur à entrer une date et une heure de début.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la valeur de début de l’étape précédente et inviter l’utilisateur à indiquer une date et une heure de fin.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour stocker la valeur de fin de l’étape précédente et demandez à l’utilisateur de confirmer toutes les entrées.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour vérifier la réponse de l’utilisateur à l’étape précédente. Si l’utilisateur confirme les entrées, utilisez la classe **OAuthPrompt** pour obtenir un jeton d’accès. Sinon, terminez la boîte de dialogue.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour utiliser le kit de développement logiciel (SDK) Microsoft Graph afin de créer le nouvel événement.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    Examinez ce que fait ce code.

    - Il obtient le **MailboxSettings** de l’utilisateur pour déterminer le fuseau horaire préféré de l’utilisateur.
    - Il crée un objet **Event** à l’aide des valeurs fournies par l’utilisateur. Notez que les propriétés **Start** et **end** sont définies avec le fuseau horaire de l’utilisateur.
    - Il crée l’événement sur le calendrier de l’utilisateur.

## <a name="add-validation"></a>Ajouter une validation

À présent, ajoutez une validation à l’entrée de l’utilisateur afin d’éviter les erreurs lors de la création de l’événement avec Microsoft Graph. Nous souhaitons s’assurer que :

- Si l’utilisateur donne une liste de participants, il doit s’agir d’une liste d’adresses de messagerie valides, séparées par des points-virgules.
- La date et l’heure de début doivent être une date et une heure valides.
- La date et l’heure de fin doivent être une date et une heure valides, et doivent être postérieures au début.

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour les participants.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. Ajoutez les fonctions suivantes à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour la date et l’heure de début.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. Ajoutez la fonction suivante à la classe **NewEventDialog** pour valider l’entrée de l’utilisateur pour la date et l’heure de fin.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a>Ajouter des étapes à WaterfallDialog

Maintenant que vous avez toutes les « étapes » de la boîte de dialogue, la dernière étape consiste à les ajouter à un **WaterfallDialog** dans le constructeur, puis à ajouter l' **NewEventDialog** à l' **MainDialog**.

1. Recherchez le constructeur **NewEventDialog** et ajoutez-y le code suivant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    Cette opération ajoute toutes les boîtes de dialogue utilisées et ajoute toutes les fonctions que vous avez implémentées en tant qu’étapes dans le **WaterfallDialog**.

1. Ouvrez **./dialogs/MainDialog.cs** et ajoutez la ligne suivante au constructeur.

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. Remplacez le code à l’intérieur du `else if (command.StartsWith("add event"))` bloc `ProcessStepAsync` par ce qui suit.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. Enregistrez toutes vos modifications et redémarrez le robot.

1. Utilisez l’émulateur de l’infrastructure bot pour vous connecter au bot et vous connecter. Sélectionnez le bouton **Ajouter un événement** .

1. Répondez aux invites pour créer un événement. Notez que lorsque vous êtes invité à entrer les valeurs de début et de fin, vous pouvez utiliser des expressions telles que « aujourd’hui à 3PM » ou « maintenant ».

    ![Capture d’écran de la boîte de dialogue Ajouter un événement dans l’émulateur de l’infrastructure bot](images/add-event.png)
