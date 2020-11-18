---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347837"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez utiliser le kit de développement logiciel (SDK) Microsoft Graph pour obtenir les 3 prochains événements à venir sur le calendrier de l’utilisateur pour la semaine en cours.

## <a name="get-a-calendar-view"></a>Obtenir un affichage Calendrier

Un affichage Calendrier est une liste d’événements sur le calendrier d’un utilisateur qui se situent entre deux valeurs de date/heure. L’avantage de l’utilisation d’un affichage Calendrier est qu’il inclut toutes les occurrences de réunions périodiques.

1. Ouvrez **./CardHelper.cs** et ajoutez la fonction suivante à la classe **CardHelper** .

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    Ce code génère une carte adaptative pour afficher un événement de calendrier.

1. Ouvrez **./dialogs/MainDialog.cs** et ajoutez la fonction suivante à la classe **MainDialog** .

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    Examinez ce que fait ce code.

    - Il obtient le **MailboxSettings** de l’utilisateur pour déterminer le fuseau horaire et les formats de date/heure préférés de l’utilisateur.
    - Il définit les valeurs **startDateTime** et **endDateTime** sur Now et la fin de la semaine, respectivement. Cela définit la fenêtre de temps utilisée par l’affichage Calendrier.
    - Il est appelé `graphClient.Me.CalendarView` avec les détails suivants.
        - Il définit l' `Prefer: outlook.timezone` en-tête sur le fuseau horaire préféré de l’utilisateur, ce qui entraîne les heures de début et de fin des événements dans le fuseau horaire de l’utilisateur.
        - Il utilise la `Top(3)` méthode pour limiter les résultats aux trois premiers événements.
        - Il utilise `Select` pour limiter les champs renvoyés uniquement aux champs utilisés par le bot.
        - Il utilise `OrderBy` pour trier les événements par heure de début.
    - Elle ajoute une carte adaptative pour chaque événement au message de réponse.

1. Remplacez le code à l’intérieur du `else if (command.StartsWith("show calendar"))` bloc `ProcessStepAsync` par ce qui suit.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. Enregistrez toutes vos modifications et redémarrez le robot.

1. Utilisez l’émulateur de l’infrastructure bot pour vous connecter au bot et vous connecter. Sélectionnez le bouton **afficher le calendrier** pour afficher l’affichage Calendrier.

    ![Capture d’écran de la carte adaptative illustrant les trois événements suivants](images/calendar-view.png)
