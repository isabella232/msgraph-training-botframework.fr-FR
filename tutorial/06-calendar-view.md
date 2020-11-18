---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347837"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5b86e-101">Dans cette section, vous allez utiliser le kit de développement logiciel (SDK) Microsoft Graph pour obtenir les 3 prochains événements à venir sur le calendrier de l’utilisateur pour la semaine en cours.</span><span class="sxs-lookup"><span data-stu-id="5b86e-101">In this section you'll use the Microsoft Graph SDK to get the next 3 upcoming events on the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="5b86e-102">Obtenir un affichage Calendrier</span><span class="sxs-lookup"><span data-stu-id="5b86e-102">Get a calendar view</span></span>

<span data-ttu-id="5b86e-103">Un affichage Calendrier est une liste d’événements sur le calendrier d’un utilisateur qui se situent entre deux valeurs de date/heure.</span><span class="sxs-lookup"><span data-stu-id="5b86e-103">A calendar view is a list of events on a user's calendar that fall between two date/time values.</span></span> <span data-ttu-id="5b86e-104">L’avantage de l’utilisation d’un affichage Calendrier est qu’il inclut toutes les occurrences de réunions périodiques.</span><span class="sxs-lookup"><span data-stu-id="5b86e-104">The advantage of using a calendar view is that it includes any occurrences of recurring meetings.</span></span>

1. <span data-ttu-id="5b86e-105">Ouvrez **./CardHelper.cs** et ajoutez la fonction suivante à la classe **CardHelper** .</span><span class="sxs-lookup"><span data-stu-id="5b86e-105">Open **./CardHelper.cs** and add the following function to the **CardHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    <span data-ttu-id="5b86e-106">Ce code génère une carte adaptative pour afficher un événement de calendrier.</span><span class="sxs-lookup"><span data-stu-id="5b86e-106">This code builds an Adaptive Card to render a calendar event.</span></span>

1. <span data-ttu-id="5b86e-107">Ouvrez **./dialogs/MainDialog.cs** et ajoutez la fonction suivante à la classe **MainDialog** .</span><span class="sxs-lookup"><span data-stu-id="5b86e-107">Open **./Dialogs/MainDialog.cs** and add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    <span data-ttu-id="5b86e-108">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="5b86e-108">Consider what this code does.</span></span>

    - <span data-ttu-id="5b86e-109">Il obtient le **MailboxSettings** de l’utilisateur pour déterminer le fuseau horaire et les formats de date/heure préférés de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5b86e-109">It gets the user's **MailboxSettings** to determine the user's preferred time zone and date/time formats.</span></span>
    - <span data-ttu-id="5b86e-110">Il définit les valeurs **startDateTime** et **endDateTime** sur Now et la fin de la semaine, respectivement.</span><span class="sxs-lookup"><span data-stu-id="5b86e-110">It sets the **startDateTime** and **endDateTime** values to now and the end of the week, respectively.</span></span> <span data-ttu-id="5b86e-111">Cela définit la fenêtre de temps utilisée par l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="5b86e-111">This defines the window of time that the calendar view uses.</span></span>
    - <span data-ttu-id="5b86e-112">Il est appelé `graphClient.Me.CalendarView` avec les détails suivants.</span><span class="sxs-lookup"><span data-stu-id="5b86e-112">It calls `graphClient.Me.CalendarView` with the following details.</span></span>
        - <span data-ttu-id="5b86e-113">Il définit l' `Prefer: outlook.timezone` en-tête sur le fuseau horaire préféré de l’utilisateur, ce qui entraîne les heures de début et de fin des événements dans le fuseau horaire de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5b86e-113">It sets the `Prefer: outlook.timezone` header to the user's preferred time zone, causing the start and end times for the events to be in the user's timezone.</span></span>
        - <span data-ttu-id="5b86e-114">Il utilise la `Top(3)` méthode pour limiter les résultats aux trois premiers événements.</span><span class="sxs-lookup"><span data-stu-id="5b86e-114">It uses the `Top(3)` method to limit the results to only the first 3 events.</span></span>
        - <span data-ttu-id="5b86e-115">Il utilise `Select` pour limiter les champs renvoyés uniquement aux champs utilisés par le bot.</span><span class="sxs-lookup"><span data-stu-id="5b86e-115">It uses `Select` to limit the fields returned to just those fields used by the bot.</span></span>
        - <span data-ttu-id="5b86e-116">Il utilise `OrderBy` pour trier les événements par heure de début.</span><span class="sxs-lookup"><span data-stu-id="5b86e-116">It uses `OrderBy` to sort the events by start time.</span></span>
    - <span data-ttu-id="5b86e-117">Elle ajoute une carte adaptative pour chaque événement au message de réponse.</span><span class="sxs-lookup"><span data-stu-id="5b86e-117">It adds an Adaptive Card for each event to the reply message.</span></span>

1. <span data-ttu-id="5b86e-118">Remplacez le code à l’intérieur du `else if (command.StartsWith("show calendar"))` bloc `ProcessStepAsync` par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="5b86e-118">Replace the code inside the `else if (command.StartsWith("show calendar"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. <span data-ttu-id="5b86e-119">Enregistrez toutes vos modifications et redémarrez le robot.</span><span class="sxs-lookup"><span data-stu-id="5b86e-119">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="5b86e-120">Utilisez l’émulateur de l’infrastructure bot pour vous connecter au bot et vous connecter.</span><span class="sxs-lookup"><span data-stu-id="5b86e-120">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="5b86e-121">Sélectionnez le bouton **afficher le calendrier** pour afficher l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="5b86e-121">Select the **Show calendar** button to display the calendar view.</span></span>

    ![Capture d’écran de la carte adaptative illustrant les trois événements suivants](images/calendar-view.png)
