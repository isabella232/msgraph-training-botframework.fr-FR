---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347833"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez utiliser le kit de développement logiciel (SDK) Microsoft Graph pour obtenir l’utilisateur connecté.

## <a name="create-a-graph-service"></a>Créer un service Graph

Commencez par implémenter un service que le bot peut utiliser pour obtenir une **GraphServiceClient** du kit de développement logiciel (SDK) de Microsoft Graph, puis rendez ce service disponible pour le bot via l’injection de dépendance.

1. Créez un répertoire dans la racine du projet nommé **Graph**. Créez un fichier dans le répertoire **./Graph** nommé **IGraphClientService.cs** et ajoutez le code suivant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. Créez un fichier dans le répertoire **./Graph** nommé **GraphClientService.cs** et ajoutez le code suivant.

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. Ouvrez **./Startup.cs** et ajoutez le code suivant à la fin de la `ConfigureServices` fonction.

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. Ouvrez **./dialogs/MainDialog.cs**. Ajoutez les `using` instructions suivantes en haut du fichier.

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. Ajoutez la propriété suivante à la classe **MainDialog** .

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. Recherchez le constructeur de la classe **MainDialog** et mettez à jour sa signature afin de prendre un paramètre **IGraphServiceClient** .

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. Ajoutez le code suivant au constructeur.

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a>Obtenir l’utilisateur connecté

Dans cette section, vous allez utiliser Microsoft Graph pour obtenir le nom, l’adresse de messagerie et la photo de l’utilisateur. Ensuite, vous allez créer une carte adaptative pour afficher les informations.

1. Créez un fichier à la racine du projet nommé **CardHelper.cs**. Ajoutez le code suivant au fichier.

    ```csharp
    using AdaptiveCards;
    using Microsoft.Graph;
    using System;
    using System.IO;

    namespace CalendarBot
    {
        public class CardHelper
        {
            public static AdaptiveCard GetUserCard(User user, Stream photo)
            {
              // Create an Adaptive Card to display the user
                // See https://adaptivecards.io/designer/ for possibilities
                var userCard = new AdaptiveCard("1.2");

                var columns = new AdaptiveColumnSet();
                userCard.Body.Add(columns);

                var userPhotoColumn = new AdaptiveColumn { Width = AdaptiveColumnWidth.Auto };
                columns.Columns.Add(userPhotoColumn);

                userPhotoColumn.Items.Add(new AdaptiveImage {
                    Style = AdaptiveImageStyle.Person,
                    Size = AdaptiveImageSize.Small,
                    Url = GetDataUriFromPhoto(photo)
                });

                var userInfoColumn = new AdaptiveColumn {Width = AdaptiveColumnWidth.Stretch };
                columns.Columns.Add(userInfoColumn);

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Weight = AdaptiveTextWeight.Bolder,
                    Wrap = true,
                    Text = user.DisplayName
                });

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Spacing = AdaptiveSpacing.None,
                    IsSubtle = true,
                    Wrap = true,
                    Text = user.Mail ?? user.UserPrincipalName
                });

                return userCard;
            }

            private static Uri GetDataUriFromPhoto(Stream photo)
            {
                // Copy to a MemoryStream to get access to bytes
                var photoStream = new MemoryStream();
                photo.CopyTo(photoStream);

                var photoBytes = photoStream.ToArray();

                return new Uri($"data:image/png;base64,{Convert.ToBase64String(photoBytes)}");
            }
        }
    }
    ```

    Ce code utilise le package NuGet **AdaptiveCard** pour créer une carte adaptative permettant d’afficher l’utilisateur.

1. Ajoutez la fonction suivante à la classe **MainDialog** .

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    Examinez ce que fait ce code.

    - Il utilise le **graphClient** pour [obtenir l’utilisateur connecté](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).
        - Il utilise la `Select` méthode pour limiter les champs qui sont renvoyés.
    - Il utilise le **graphClient** pour [obtenir la photo](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0)de l’utilisateur, en demandant la plus petite taille prise en charge de 48 x 48 pixels.
    - Il utilise la classe **CardHelper** pour construire une carte adaptative et l’envoyer sous forme de pièce jointe.

1. Remplacez le code à l’intérieur du `else if (command.StartsWith("show me"))` bloc `ProcessStepAsync` par ce qui suit.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. Enregistrez toutes vos modifications et redémarrez le robot.

1. Utilisez l’émulateur de l’infrastructure bot pour vous connecter au bot et vous connecter. Sélectionnez le bouton **Afficher** pour afficher l’utilisateur connecté.

    ![Capture d’écran de la carte adaptative illustrant l’utilisateur](images/user-card.png)
