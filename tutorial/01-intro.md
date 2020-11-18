---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347817"
---
<!-- markdownlint-disable MD002 MD041 -->

Ce didacticiel vous apprend à créer un bot de l’infrastructure bot qui utilise l’API Microsoft Graph pour récupérer des informations de calendrier pour un utilisateur.

> [!TIP]
> Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-botframework). Consultez le fichier Lisez-moi dans le dossier **Demo** pour obtenir des instructions sur la configuration de l’application avec un ID d’application et une clé secrète.

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer ce didacticiel, les éléments suivants doivent être installés sur votre ordinateur de développement.

- [Kit de développement logiciel .NET Core](https://dotnet.microsoft.com/download)
- [Émulateur de l’infrastructure bot](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

Vous devez également disposer d’un compte Microsoft personnel disposant d’une boîte aux lettres sur Outlook.com ou d’un compte professionnel ou scolaire Microsoft. Si vous n’avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit :

- Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.
- Un abonnement Azure. Si vous n’en avez pas, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

> [!NOTE]
> Ce didacticiel a été écrit avec .NET Core SDK version 3.1.401. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.

## <a name="feedback"></a>Commentaires

Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-botframework).
