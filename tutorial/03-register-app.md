---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347793"
---
<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez créer un enregistrement de canaux de robots et un enregistrement d’application Web Azure AD à l’aide du portail Azure.

## <a name="create-a-bot-channels-registration"></a>Création d’un enregistrement de canaux de robots

1. Ouvrez un navigateur et accédez au [portail Azure](https://portal.azure.com). Connectez-vous à l’aide du compte associé à votre abonnement Azure.

1. Sélectionnez le menu en haut à gauche, puis sélectionnez **créer une ressource**.

    ![Capture d’écran du menu du portail Azure](images/create-resource.png)

1. Sur la **nouvelle** page, recherchez `Bot Channel` et sélectionnez **inscription des canaux des robots**.

1. Sur la page **inscription des canaux des robots** , sélectionnez **créer**.

1. Renseignez les champs obligatoires et laissez le **point de terminaison de messagerie** vide. Le champ de **descripteur de robot** doit être unique. Veillez à passer en revue les différents niveaux de tarification et à sélectionner ce qui est pertinent pour votre scénario. S’il s’agit d’un simple exercice d’apprentissage, vous souhaiterez peut-être sélectionner l’option gratuit.

1. Sélectionnez l' **ID et le mot de passe de l’application Microsoft**, puis sélectionnez **créer**.

1. Sélectionnez **créer un ID d’application dans le portail d’inscription des applications**. Cette opération ouvre une nouvelle fenêtre ou un nouvel onglet pour le panneau **inscriptions des applications** dans le portail Azure.

1. Dans le panneau **inscriptions des applications** , sélectionnez **nouvelle inscription**.

1. Définissez les valeurs comme suit.

    - Définissez le **Nom** sur `Graph Calendar Bot`.
    - Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.
    - Laissez **Redirect URI** vide.

    ![Capture d’écran de la page Inscrire une application](./images/aad-register-an-app.png)

1. Sélectionner **Inscription**. Sur la page du **robot de calendrier Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin dans les étapes suivantes.

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](./images/aad-application-id.png)

1. Sélectionnez **Certificats et secrets** sous **Gérer**. Sélectionnez le bouton **Nouveau secret client**. Entrez une valeur dans **Description**, sélectionnez une des options pour **Expire le**, puis sélectionnez **Ajouter**.

1. Copiez la valeur due la clé secrète client avant de quitter cette page. Vous en aurez besoin dans les étapes suivantes.

    > [!IMPORTANT]
    > Cette clé secrète client n’apparaîtra plus jamais, aussi veillez à la copier maintenant. Vous devrez entrer cette valeur à plusieurs endroits afin de conserver la sécurité.

1. Revenez à la fenêtre d’enregistrement du canal bot dans votre navigateur et collez l’ID de l’application dans le champ ID de l’application **Microsoft** . Collez votre clé secrète client dans le champ **mot de passe** . Sélectionnez **OK**.

1. Sur la page **enregistrement des canaux des robots** , sélectionnez **créer**.

1. Attendez la création de l’enregistrement des canaux de robots. Une fois que vous avez créé, revenez à la page d’accueil dans le portail Azure, puis sélectionnez **services bot**. Sélectionnez votre nouvelle inscription de canal pour les robots pour afficher ses propriétés.

## <a name="create-a-web-app-registration"></a>Créer une inscription d’application Web

1. Revenez à la section **inscriptions des applications** du portail Azure.

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `Graph Calendar Bot Auth`.
    - Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.
    - Sous **URI de redirection**, définissez la première flèche déroulante sur `Web`, et la valeur sur `https://token.botframework.com/.auth/web/redirect`.

1. Sélectionnez **Inscrire**. Sur la page authentification de l’outil de **calendrier Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin dans les étapes suivantes.

1. Sélectionnez **Certificats et secrets** sous **Gérer**. Sélectionnez le bouton **Nouveau secret client**. Entrez une valeur dans **Description**, sélectionnez une des options pour **Expire le**, puis sélectionnez **Ajouter**.

1. Copiez la valeur due la clé secrète client avant de quitter cette page. Vous en aurez besoin dans les étapes suivantes.

1. Sélectionnez **autorisations d’API**, puis **Ajouter une autorisation**.

1. Sélectionnez **Microsoft Graph**, puis **autorisations déléguées**.

1. Sélectionnez les autorisations suivantes, puis **Ajouter des autorisations**.

    - **openid**
    - **profil**
    - **Calendars.ReadWrite**
    - **MailboxSettings.Read**

    ![Capture d’écran des autorisations configurées](images/configured-permissions.png)

### <a name="about-permissions"></a>À propos des autorisations

Tenez compte de ce que chacune de ces étendues d’autorisation autorise le bot et ce à quoi le robot les utilisera.

- **OpenID** et **Profil**: permet au bot de signer des utilisateurs et d’obtenir des informations de base d’Azure ad dans le jeton d’identité.
- **Calendars. ReadWrite**: permet au bot de lire le calendrier de l’utilisateur et d’ajouter de nouveaux événements au calendrier de l’utilisateur.
- **MailboxSettings. Read**: permet au bot de lire les paramètres de boîte aux lettres de l’utilisateur. Le bot l’utilisera pour obtenir le fuseau horaire sélectionné de l’utilisateur.
- **User. Read**: permet au bot d’obtenir le profil de l’utilisateur à partir de Microsoft Graph. Le bot l’utilisera pour obtenir le nom de l’utilisateur.

## <a name="add-oauth-connection-to-the-bot"></a>Ajouter une connexion OAuth au bot

1. Accédez à la page d’inscription des canaux de robots de votre robot sur le portail Azure. Sélectionnez **paramètres** sous **gestion des robots**.

1. Sous **paramètres de connexion OAuth** , dans la partie inférieure de la page, sélectionnez **Ajouter un paramètre**.

1. Remplissez le formulaire comme suit, puis sélectionnez **Enregistrer**.

    - **Nom**: `GraphBotAuth`
    - **Fournisseur**: **Azure Active Directory v2**
    - **ID client**: ID d’application de l’enregistrement d’authentification de votre **robot de calendrier Graph** .
    - **Clé secrète client**: la clé secrète client de l’enregistrement d’authentification de votre **robot de calendrier Graph** .
    - **URL d’échange de jetons**: laisser vide
    - **ID de client**: `common`
    - **Étendues**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`

1. Sélectionnez l’entrée **GraphBotAuth** sous **paramètres de connexion OAuth**.

1. Sélectionnez **tester la connexion**. Cette opération ouvre une nouvelle fenêtre de navigateur ou un nouvel onglet pour démarrer le flux OAuth.

1. Si nécessaire, connectez-vous. Passez en revue la liste des autorisations demandées, puis sélectionnez **accepter**.

1. Vous devriez voir une **connexion de test au message « GraphBotAuth »** .

> [!TIP]
> Vous pouvez sélectionner le bouton **copier le jeton** dans cette page, puis coller le jeton dans [https://jwt.ms](https://jwt.ms) pour afficher les revendications à l’intérieur du jeton. Cela est utile lors de la résolution des erreurs d’authentification.
