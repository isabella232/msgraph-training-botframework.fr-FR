---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347793"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fc6db-101">Dans cet exercice, vous allez créer un enregistrement de canaux de robots et un enregistrement d’application Web Azure AD à l’aide du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6db-101">In this exercise, you will create a new Bot Channels registration and an Azure AD web application registration using the Azure Portal.</span></span>

## <a name="create-a-bot-channels-registration"></a><span data-ttu-id="fc6db-102">Création d’un enregistrement de canaux de robots</span><span class="sxs-lookup"><span data-stu-id="fc6db-102">Create a Bot Channels registration</span></span>

1. <span data-ttu-id="fc6db-103">Ouvrez un navigateur et accédez au [portail Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fc6db-103">Open a browser and navigate to the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="fc6db-104">Connectez-vous à l’aide du compte associé à votre abonnement Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6db-104">Login using the account associated with your Azure subscription.</span></span>

1. <span data-ttu-id="fc6db-105">Sélectionnez le menu en haut à gauche, puis sélectionnez **créer une ressource**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-105">Select the upper-left menu, then select **Create a resource**.</span></span>

    ![Capture d’écran du menu du portail Azure](images/create-resource.png)

1. <span data-ttu-id="fc6db-107">Sur la **nouvelle** page, recherchez `Bot Channel` et sélectionnez **inscription des canaux des robots**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-107">On the **New** page, search for `Bot Channel` and select **Bot Channels Registration**.</span></span>

1. <span data-ttu-id="fc6db-108">Sur la page **inscription des canaux des robots** , sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-108">On the **Bot Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="fc6db-109">Renseignez les champs obligatoires et laissez le **point de terminaison de messagerie** vide.</span><span class="sxs-lookup"><span data-stu-id="fc6db-109">Fill in the required fields, and leave **Messaging endpoint** blank.</span></span> <span data-ttu-id="fc6db-110">Le champ de **descripteur de robot** doit être unique.</span><span class="sxs-lookup"><span data-stu-id="fc6db-110">The **Bot handle** field must be unique.</span></span> <span data-ttu-id="fc6db-111">Veillez à passer en revue les différents niveaux de tarification et à sélectionner ce qui est pertinent pour votre scénario.</span><span class="sxs-lookup"><span data-stu-id="fc6db-111">Be sure to review the different pricing tiers and select what makes sense for your scenario.</span></span> <span data-ttu-id="fc6db-112">S’il s’agit d’un simple exercice d’apprentissage, vous souhaiterez peut-être sélectionner l’option gratuit.</span><span class="sxs-lookup"><span data-stu-id="fc6db-112">If this is just a learning exercise, you may want to select the free option.</span></span>

1. <span data-ttu-id="fc6db-113">Sélectionnez l' **ID et le mot de passe de l’application Microsoft**, puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-113">Select the **Microsoft App ID and password**, then select **Create New**.</span></span>

1. <span data-ttu-id="fc6db-114">Sélectionnez **créer un ID d’application dans le portail d’inscription des applications**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-114">Select **Create App ID in the App Registration Portal**.</span></span> <span data-ttu-id="fc6db-115">Cette opération ouvre une nouvelle fenêtre ou un nouvel onglet pour le panneau **inscriptions des applications** dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6db-115">This will open a new window or tab to the **App registrations** blade in the Azure Portal.</span></span>

1. <span data-ttu-id="fc6db-116">Dans le panneau **inscriptions des applications** , sélectionnez **nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-116">In the **App registrations** blade, select **New registration**.</span></span>

1. <span data-ttu-id="fc6db-117">Définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="fc6db-117">Set the values as follows.</span></span>

    - <span data-ttu-id="fc6db-118">Définissez le **Nom** sur `Graph Calendar Bot`.</span><span class="sxs-lookup"><span data-stu-id="fc6db-118">Set **Name** to `Graph Calendar Bot`.</span></span>
    - <span data-ttu-id="fc6db-119">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="fc6db-120">Laissez **Redirect URI** vide.</span><span class="sxs-lookup"><span data-stu-id="fc6db-120">Leave **Redirect URI** empty.</span></span>

    ![Capture d’écran de la page Inscrire une application](./images/aad-register-an-app.png)

1. <span data-ttu-id="fc6db-122">Sélectionner **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-122">Select **Register**.</span></span> <span data-ttu-id="fc6db-123">Sur la page du **robot de calendrier Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc6db-123">On the **Graph Calendar Bot** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](./images/aad-application-id.png)

1. <span data-ttu-id="fc6db-125">Sélectionnez **Certificats et secrets** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="fc6db-126">Sélectionnez le bouton **Nouveau secret client**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-126">Select the **New client secret** button.</span></span> <span data-ttu-id="fc6db-127">Entrez une valeur dans **Description**, sélectionnez une des options pour **Expire le**, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="fc6db-128">Copiez la valeur due la clé secrète client avant de quitter cette page.</span><span class="sxs-lookup"><span data-stu-id="fc6db-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="fc6db-129">Vous en aurez besoin dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc6db-129">You will need it in the following steps.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fc6db-130">Cette clé secrète client n’apparaîtra plus jamais, aussi veillez à la copier maintenant.</span><span class="sxs-lookup"><span data-stu-id="fc6db-130">This client secret is never shown again, so make sure you copy it now.</span></span> <span data-ttu-id="fc6db-131">Vous devrez entrer cette valeur à plusieurs endroits afin de conserver la sécurité.</span><span class="sxs-lookup"><span data-stu-id="fc6db-131">You will need to enter this value in multiple places so keep it safe.</span></span>

1. <span data-ttu-id="fc6db-132">Revenez à la fenêtre d’enregistrement du canal bot dans votre navigateur et collez l’ID de l’application dans le champ ID de l’application **Microsoft** .</span><span class="sxs-lookup"><span data-stu-id="fc6db-132">Return to the Bot Channel Registration window in your browser, and paste the application ID into the **Microsoft App ID** field.</span></span> <span data-ttu-id="fc6db-133">Collez votre clé secrète client dans le champ **mot de passe** .</span><span class="sxs-lookup"><span data-stu-id="fc6db-133">Paste your client secret into the **Password** field.</span></span> <span data-ttu-id="fc6db-134">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-134">Select **OK**.</span></span>

1. <span data-ttu-id="fc6db-135">Sur la page **enregistrement des canaux des robots** , sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-135">On the **Bots Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="fc6db-136">Attendez la création de l’enregistrement des canaux de robots.</span><span class="sxs-lookup"><span data-stu-id="fc6db-136">Wait for the Bot Channels registration to be created.</span></span> <span data-ttu-id="fc6db-137">Une fois que vous avez créé, revenez à la page d’accueil dans le portail Azure, puis sélectionnez **services bot**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-137">Once created, return to the Home page in the Azure Portal, then select **Bot Services**.</span></span> <span data-ttu-id="fc6db-138">Sélectionnez votre nouvelle inscription de canal pour les robots pour afficher ses propriétés.</span><span class="sxs-lookup"><span data-stu-id="fc6db-138">Select your new Bots Channel registration to view its properties.</span></span>

## <a name="create-a-web-app-registration"></a><span data-ttu-id="fc6db-139">Créer une inscription d’application Web</span><span class="sxs-lookup"><span data-stu-id="fc6db-139">Create a web app registration</span></span>

1. <span data-ttu-id="fc6db-140">Revenez à la section **inscriptions des applications** du portail Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6db-140">Return to the **App registrations** section of the Azure Portal.</span></span>

1. <span data-ttu-id="fc6db-141">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-141">Select **New registration**.</span></span> <span data-ttu-id="fc6db-142">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="fc6db-142">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="fc6db-143">Définissez le **Nom** sur `Graph Calendar Bot Auth`.</span><span class="sxs-lookup"><span data-stu-id="fc6db-143">Set **Name** to `Graph Calendar Bot Auth`.</span></span>
    - <span data-ttu-id="fc6db-144">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-144">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="fc6db-145">Sous **URI de redirection**, définissez la première flèche déroulante sur `Web`, et la valeur sur `https://token.botframework.com/.auth/web/redirect`.</span><span class="sxs-lookup"><span data-stu-id="fc6db-145">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://token.botframework.com/.auth/web/redirect`.</span></span>

1. <span data-ttu-id="fc6db-146">Sélectionnez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-146">Select **Register**.</span></span> <span data-ttu-id="fc6db-147">Sur la page authentification de l’outil de **calendrier Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc6db-147">On the **Graph Calendar Bot Auth** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

1. <span data-ttu-id="fc6db-148">Sélectionnez **Certificats et secrets** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-148">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="fc6db-149">Sélectionnez le bouton **Nouveau secret client**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-149">Select the **New client secret** button.</span></span> <span data-ttu-id="fc6db-150">Entrez une valeur dans **Description**, sélectionnez une des options pour **Expire le**, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-150">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="fc6db-151">Copiez la valeur due la clé secrète client avant de quitter cette page.</span><span class="sxs-lookup"><span data-stu-id="fc6db-151">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="fc6db-152">Vous en aurez besoin dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc6db-152">You will need it in the following steps.</span></span>

1. <span data-ttu-id="fc6db-153">Sélectionnez **autorisations d’API**, puis **Ajouter une autorisation**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-153">Select **API permissions**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="fc6db-154">Sélectionnez **Microsoft Graph**, puis **autorisations déléguées**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-154">Select **Microsoft Graph**, then select **Delegated permissions**.</span></span>

1. <span data-ttu-id="fc6db-155">Sélectionnez les autorisations suivantes, puis **Ajouter des autorisations**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-155">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="fc6db-156">**openid**</span><span class="sxs-lookup"><span data-stu-id="fc6db-156">**openid**</span></span>
    - <span data-ttu-id="fc6db-157">**profil**</span><span class="sxs-lookup"><span data-stu-id="fc6db-157">**profile**</span></span>
    - <span data-ttu-id="fc6db-158">**Calendars.ReadWrite**</span><span class="sxs-lookup"><span data-stu-id="fc6db-158">**Calendars.ReadWrite**</span></span>
    - <span data-ttu-id="fc6db-159">**MailboxSettings.Read**</span><span class="sxs-lookup"><span data-stu-id="fc6db-159">**MailboxSettings.Read**</span></span>

    ![Capture d’écran des autorisations configurées](images/configured-permissions.png)

### <a name="about-permissions"></a><span data-ttu-id="fc6db-161">À propos des autorisations</span><span class="sxs-lookup"><span data-stu-id="fc6db-161">About permissions</span></span>

<span data-ttu-id="fc6db-162">Tenez compte de ce que chacune de ces étendues d’autorisation autorise le bot et ce à quoi le robot les utilisera.</span><span class="sxs-lookup"><span data-stu-id="fc6db-162">Consider what each of those permission scopes allows the bot to do, and what the bot will use them for.</span></span>

- <span data-ttu-id="fc6db-163">**OpenID** et **Profil**: permet au bot de signer des utilisateurs et d’obtenir des informations de base d’Azure ad dans le jeton d’identité.</span><span class="sxs-lookup"><span data-stu-id="fc6db-163">**openid** and **profile**: allows the bot to sign users in and get basic information from Azure AD in the identity token.</span></span>
- <span data-ttu-id="fc6db-164">**Calendars. ReadWrite**: permet au bot de lire le calendrier de l’utilisateur et d’ajouter de nouveaux événements au calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc6db-164">**Calendars.ReadWrite**: allows the bot to read the user's calendar and to add new events to the user's calendar.</span></span>
- <span data-ttu-id="fc6db-165">**MailboxSettings. Read**: permet au bot de lire les paramètres de boîte aux lettres de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc6db-165">**MailboxSettings.Read**: allows the bot to read the user's mailbox settings.</span></span> <span data-ttu-id="fc6db-166">Le bot l’utilisera pour obtenir le fuseau horaire sélectionné de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc6db-166">The bot will use this to get the user's selected time zone.</span></span>
- <span data-ttu-id="fc6db-167">**User. Read**: permet au bot d’obtenir le profil de l’utilisateur à partir de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fc6db-167">**User.Read**: allows the bot to get the user's profile from Microsoft Graph.</span></span> <span data-ttu-id="fc6db-168">Le bot l’utilisera pour obtenir le nom de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc6db-168">The bot will use this to get the user's name.</span></span>

## <a name="add-oauth-connection-to-the-bot"></a><span data-ttu-id="fc6db-169">Ajouter une connexion OAuth au bot</span><span class="sxs-lookup"><span data-stu-id="fc6db-169">Add OAuth connection to the bot</span></span>

1. <span data-ttu-id="fc6db-170">Accédez à la page d’inscription des canaux de robots de votre robot sur le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6db-170">Navigate to your bot's Bot Channels Registration page on the Azure Portal.</span></span> <span data-ttu-id="fc6db-171">Sélectionnez **paramètres** sous **gestion des robots**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-171">Select **Settings** under **Bot Management**.</span></span>

1. <span data-ttu-id="fc6db-172">Sous **paramètres de connexion OAuth** , dans la partie inférieure de la page, sélectionnez **Ajouter un paramètre**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-172">Under **OAuth Connection Settings** near the bottom of the page, select **Add Setting**.</span></span>

1. <span data-ttu-id="fc6db-173">Remplissez le formulaire comme suit, puis sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-173">Fill in the form as follows, then select **Save**.</span></span>

    - <span data-ttu-id="fc6db-174">**Nom**: `GraphBotAuth`</span><span class="sxs-lookup"><span data-stu-id="fc6db-174">**Name**: `GraphBotAuth`</span></span>
    - <span data-ttu-id="fc6db-175">**Fournisseur**: **Azure Active Directory v2**</span><span class="sxs-lookup"><span data-stu-id="fc6db-175">**Provider**: **Azure Active Directory v2**</span></span>
    - <span data-ttu-id="fc6db-176">**ID client**: ID d’application de l’enregistrement d’authentification de votre **robot de calendrier Graph** .</span><span class="sxs-lookup"><span data-stu-id="fc6db-176">**Client id**: The application ID of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="fc6db-177">**Clé secrète client**: la clé secrète client de l’enregistrement d’authentification de votre **robot de calendrier Graph** .</span><span class="sxs-lookup"><span data-stu-id="fc6db-177">**Client secret**: The client secret of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="fc6db-178">**URL d’échange de jetons**: laisser vide</span><span class="sxs-lookup"><span data-stu-id="fc6db-178">**Token Exchange URL**: Leave blank</span></span>
    - <span data-ttu-id="fc6db-179">**ID de client**: `common`</span><span class="sxs-lookup"><span data-stu-id="fc6db-179">**Tenant ID**: `common`</span></span>
    - <span data-ttu-id="fc6db-180">**Étendues**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span><span class="sxs-lookup"><span data-stu-id="fc6db-180">**Scopes**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span></span>

1. <span data-ttu-id="fc6db-181">Sélectionnez l’entrée **GraphBotAuth** sous **paramètres de connexion OAuth**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-181">Select the **GraphBotAuth** entry under **OAuth Connection Settings**.</span></span>

1. <span data-ttu-id="fc6db-182">Sélectionnez **tester la connexion**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-182">Select **Test Connection**.</span></span> <span data-ttu-id="fc6db-183">Cette opération ouvre une nouvelle fenêtre de navigateur ou un nouvel onglet pour démarrer le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="fc6db-183">This opens a new browser window or tab to start the OAuth flow.</span></span>

1. <span data-ttu-id="fc6db-184">Si nécessaire, connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="fc6db-184">If necessary, sign in.</span></span> <span data-ttu-id="fc6db-185">Passez en revue la liste des autorisations demandées, puis sélectionnez **accepter**.</span><span class="sxs-lookup"><span data-stu-id="fc6db-185">Review the list of requested permissions, then select **Accept**.</span></span>

1. <span data-ttu-id="fc6db-186">Vous devriez voir une **connexion de test au message « GraphBotAuth »** .</span><span class="sxs-lookup"><span data-stu-id="fc6db-186">You should see a **Test Connection to 'GraphBotAuth' Succeeded** message.</span></span>

> [!TIP]
> <span data-ttu-id="fc6db-187">Vous pouvez sélectionner le bouton **copier le jeton** dans cette page, puis coller le jeton dans [https://jwt.ms](https://jwt.ms) pour afficher les revendications à l’intérieur du jeton.</span><span class="sxs-lookup"><span data-stu-id="fc6db-187">You can select the **Copy Token** button on this page, and paste the token into [https://jwt.ms](https://jwt.ms) to see the claims inside the token.</span></span> <span data-ttu-id="fc6db-188">Cela est utile lors de la résolution des erreurs d’authentification.</span><span class="sxs-lookup"><span data-stu-id="fc6db-188">This is useful when troubleshooting authentication errors.</span></span>
