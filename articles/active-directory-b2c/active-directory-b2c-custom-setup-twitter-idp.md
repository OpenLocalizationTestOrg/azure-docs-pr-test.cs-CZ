---
title: "Azure Active Directory B2C: Přidání služby Twitter jako zprostředkovatel identity OAuth1 pomocí vlastních zásad"
description: "Pomocí služby Twitter jako poskytovatel identit pomocí protokolu OAuth1"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 10/23/2017
ms.author: yoelh
ms.openlocfilehash: 629e0bbaa7c62ef5d381085588c6a99c203c41cb
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a><span data-ttu-id="7d735-103">Azure Active Directory B2C: Přidání služby Twitter jako zprostředkovatel identity OAuth1 pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="7d735-103">Azure Active Directory B2C: Add Twitter as an OAuth1 identity provider by using custom policies</span></span>
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="7d735-104">Tento článek ukazuje, jak povolit přihlášení pro uživatele služby Twitter účtu pomocí [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7d735-104">This article shows you how to enable sign-in for users of a Twitter account by using [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d735-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7d735-105">Prerequisites</span></span>
<span data-ttu-id="7d735-106">Proveďte kroky v [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7d735-106">Complete the steps in the [Get started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

## <a name="step-1-create-a-twitter-account-application"></a><span data-ttu-id="7d735-107">Krok 1: Vytvoření aplikace účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="7d735-107">Step 1: Create a Twitter account application</span></span>
<span data-ttu-id="7d735-108">Použití služby Twitter jako poskytovatel identit v Azure Active Directory B2C (Azure AD B2C), musíte vytvořit aplikaci služby Twitter a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="7d735-108">To use Twitter as an identity provider in Azure Active Directory B2C (Azure AD B2C), you must create a Twitter application and supply it with the right parameters.</span></span> <span data-ttu-id="7d735-109">Můžete zaregistrovat aplikaci služby Twitter přechodem na [stránku pro přihlášení služby Twitter](https://twitter.com/signup).</span><span class="sxs-lookup"><span data-stu-id="7d735-109">You can register a Twitter application by going to the [Twitter sign-up page](https://twitter.com/signup).</span></span>

1. <span data-ttu-id="7d735-110">Přejděte na [Twitter vývojáři](https://apps.twitter.com/) web, přihlaste se pomocí přihlašovacích údajů účtu služby Twitter a pak vyberte **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="7d735-110">Go to the [Twitter Developers](https://apps.twitter.com/) website, sign in with your Twitter account credentials, and then select  **Create New App**.</span></span>

    ![Účet služby Twitter – vytvoření nové aplikace](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. <span data-ttu-id="7d735-112">V **vytvořit aplikaci** okno, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7d735-112">In the **Create an application** window, do the following:</span></span>
 
    <span data-ttu-id="7d735-113">a.</span><span class="sxs-lookup"><span data-stu-id="7d735-113">a.</span></span> <span data-ttu-id="7d735-114">Typ **název** a **popis** pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7d735-114">Type the **Name** and a **Description** for your new app.</span></span> 

    <span data-ttu-id="7d735-115">b.</span><span class="sxs-lookup"><span data-stu-id="7d735-115">b.</span></span> <span data-ttu-id="7d735-116">V **webu** pole, vložte **https://login.microsoftonline.com**.</span><span class="sxs-lookup"><span data-stu-id="7d735-116">In the **Website** box, paste **https://login.microsoftonline.com**.</span></span> 

    <span data-ttu-id="7d735-117">c.</span><span class="sxs-lookup"><span data-stu-id="7d735-117">c.</span></span> <span data-ttu-id="7d735-118">V **adresu URL zpětné volání** pole, vložte **https://login.microsoftonline.com/te/ {tenant}.onmicrosoft.com/oauth2/authresp**.</span><span class="sxs-lookup"><span data-stu-id="7d735-118">In the **Callback URL** box, paste **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp**.</span></span> <span data-ttu-id="7d735-119">Nahraďte {*klienta*} s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="7d735-119">Replace {*tenant*} with your tenant name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="7d735-120">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7d735-120">Make sure that you are using the HTTPS scheme.</span></span> 

    <span data-ttu-id="7d735-121">d.</span><span class="sxs-lookup"><span data-stu-id="7d735-121">d.</span></span> <span data-ttu-id="7d735-122">V dolní části stránky, přečtěte si a přijměte podmínky a potom vyberte **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="7d735-122">At the bottom of the page, read and accept the terms, and then select **Create your Twitter application**.</span></span>

    ![Účet služby Twitter – přidání nové aplikace](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. <span data-ttu-id="7d735-124">V **B2C ukázku** vyberte **nastavení**, vyberte **povolit této aplikace, který se má použít k přihlášení pomocí služby Twitter** zaškrtněte políčko a potom vyberte **aktualizace Nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7d735-124">In the **B2C demo** window, select **Settings**, select the **Allow this application to be used to sign in with Twitter** check box, and then select **Update Settings**.</span></span>

4. <span data-ttu-id="7d735-125">Vyberte **klíče a přístupové tokeny**a poznamenejte si **uživatelský klíč (klíč rozhraní API)** a **uživatelský tajný klíč (tajný klíč rozhraní API)** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7d735-125">Select **Keys and Access Tokens**, and note the **Consumer Key (API Key)** and **Consumer Secret (API Secret)** values.</span></span>

    ![Účet služby Twitter – nastavení vlastností aplikací](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    ><span data-ttu-id="7d735-127">Uživatelský tajný klíč je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="7d735-127">The consumer secret is an important security credential.</span></span> <span data-ttu-id="7d735-128">S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="7d735-128">Do not share this secret with anyone or distribute it with your app.</span></span>

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="7d735-129">Krok 2: Přidáte váš klíč pro Twitter účet aplikace do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="7d735-129">Step 2: Add your Twitter account application key to Azure AD B2C</span></span>
<span data-ttu-id="7d735-130">Federace s účty služby Twitter vyžaduje uživatelský tajný klíč pro účet služby Twitter do vztahu důvěryhodnosti Azure AD B2C jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="7d735-130">Federation with Twitter accounts requires a consumer secret for the Twitter account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="7d735-131">K uložení uživatelský tajný klíč pro Twitter aplikace v klientovi služby Azure AD B2C, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7d735-131">To store the Twitter application's consumer secret in your Azure AD B2C tenant, do the following:</span></span> 

1. <span data-ttu-id="7d735-132">Ve vašem klientu Azure AD B2C, vyberte **nastavení B2C** > **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="7d735-132">In your Azure AD B2C tenant, select **B2C Settings** > **Identity Experience Framework**.</span></span>

2. <span data-ttu-id="7d735-133">Chcete-li zobrazit klíčů, které jsou k dispozici ve vašem klientovi, vyberte **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="7d735-133">To view the keys that are available in your tenant, select **Policy Keys**.</span></span>

3. <span data-ttu-id="7d735-134">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="7d735-134">Select **Add**.</span></span>

4. <span data-ttu-id="7d735-135">V **možnosti** vyberte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="7d735-135">In the **Options** box, select **Manual**.</span></span>

5. <span data-ttu-id="7d735-136">V **název** vyberte **TwitterSecret**.</span><span class="sxs-lookup"><span data-stu-id="7d735-136">In the **Name** box, select **TwitterSecret**.</span></span>  
    <span data-ttu-id="7d735-137">Předpona *B2C_1A_* může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="7d735-137">The prefix *B2C_1A_* might be added automatically.</span></span>

6. <span data-ttu-id="7d735-138">V **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z [portálu pro registraci aplikace](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7d735-138">In the **Secret** box, enter your Microsoft application secret from the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span>

7. <span data-ttu-id="7d735-139">Pro **použití klíče**, použijte **šifrování**.</span><span class="sxs-lookup"><span data-stu-id="7d735-139">For **Key usage**, use **Encryption**.</span></span>

8. <span data-ttu-id="7d735-140">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7d735-140">Select **Create**.</span></span>

9. <span data-ttu-id="7d735-141">Potvrďte, že jste vytvořili `B2C_1A_TwitterSecret` klíč.</span><span class="sxs-lookup"><span data-stu-id="7d735-141">Confirm that you've created the `B2C_1A_TwitterSecret` key.</span></span>

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="7d735-142">Krok 3: Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="7d735-142">Step 3: Add a claims provider in your extension policy</span></span>

<span data-ttu-id="7d735-143">Pokud chcete uživatelům přihlášení pomocí účtu sítě Twitter, je nutné zadat Twitter jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="7d735-143">If you want users to sign in by using Twitter account, you must define Twitter as a claims provider.</span></span> <span data-ttu-id="7d735-144">Jinými slovy je nutné zadat koncové body, které komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="7d735-144">In other words, you must specify the endpoints that Azure AD B2C communicates with.</span></span> <span data-ttu-id="7d735-145">Koncové body poskytují sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d735-145">The endpoints provide a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="7d735-146">Definování Twitter jako poskytovatele deklarací identity tak, že přidáte `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="7d735-146">Define Twitter as a claims provider by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="7d735-147">V pracovním adresáři, otevřete *TrustFrameworkExtensions.xml* soubor rozšíření zásad.</span><span class="sxs-lookup"><span data-stu-id="7d735-147">In your working directory, open the *TrustFrameworkExtensions.xml* extension policy file.</span></span> 

2. <span data-ttu-id="7d735-148">Vyhledejte `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="7d735-148">Search for the `<ClaimsProviders>` section.</span></span>

3. <span data-ttu-id="7d735-149">V `<ClaimsProviders>` uzlu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="7d735-149">In the `<ClaimsProviders>` node, add the following XML snippet:</span></span>  

    ```xml
    <ClaimsProvider>
        <Domain>twitter.com</Domain>
        <DisplayName>Twitter</DisplayName>
        <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAUTH1">
            <DisplayName>Twitter</DisplayName>
            <Protocol Name="OAuth1" />
            <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application consumer key</Item>
            </Metadata>
            <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
            </CryptographicKeys>
            <InputClaims />
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            </OutputClaims>
            <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
            </OutputClaimsTransformations>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. <span data-ttu-id="7d735-150">Nahraďte *client_id*' hodnotu s vaší aplikací uživatelský klíč pro Twitter účtu.</span><span class="sxs-lookup"><span data-stu-id="7d735-150">Replace the *client_id*\` value with your Twitter account application consumer key.</span></span>

5. <span data-ttu-id="7d735-151">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="7d735-151">Save the file.</span></span>

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="7d735-152">Krok 4: Registrace zprostředkovatele deklarací identity účtu služby Twitter k vám dobře slouží registrace nebo přihlášení uživatele</span><span class="sxs-lookup"><span data-stu-id="7d735-152">Step 4: Register the Twitter account claims provider to your sign-up or sign-in user journey</span></span>
<span data-ttu-id="7d735-153">Nastavili jste si poskytovatele identit.</span><span class="sxs-lookup"><span data-stu-id="7d735-153">You've set up the identity provider.</span></span> <span data-ttu-id="7d735-154">Ale ho dosud nejsou k dispozici v žádném z registrace nebo přihlášení systému windows.</span><span class="sxs-lookup"><span data-stu-id="7d735-154">However, it is not yet available in any of the sign-up or sign-in windows.</span></span> <span data-ttu-id="7d735-155">Nyní je nutné přidat zprostředkovatele identity účtu služby Twitter pro vaše uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d735-155">Now you must add the Twitter account identity provider to your user `SignUpOrSignIn` user journey.</span></span>

### <a name="step-41-make-a-copy-of-the-user-journey"></a><span data-ttu-id="7d735-156">Krok 4.1: Vytvoření kopie cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="7d735-156">Step 4.1: Make a copy of the user journey</span></span>
<span data-ttu-id="7d735-157">Aby cesty uživatele dostupné, vytvořte duplicitní existující šablony cesty uživatele a pak přidejte zprostředkovatele identity Twitter:</span><span class="sxs-lookup"><span data-stu-id="7d735-157">To make the user journey available, you create a duplicate of an existing user journey template and then add the Twitter identity provider:</span></span>

>[!NOTE]
><span data-ttu-id="7d735-158">Pokud jste zkopírovali `<UserJourneys>` element ze základního souboru v zásadách *TrustFrameworkExtensions.xml* souboru rozšíření, můžete přeskočit k další části.</span><span class="sxs-lookup"><span data-stu-id="7d735-158">If you copied the `<UserJourneys>` element from the base file of your policy to the *TrustFrameworkExtensions.xml* extension file, you can skip to the next section.</span></span>

1. <span data-ttu-id="7d735-159">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="7d735-159">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>

2. <span data-ttu-id="7d735-160">Vyhledejte `<UserJourneys>` element, vyberte celý obsah `<UserJourney>` uzel a potom vyberte **Vyjmout** přesunout vybraný text do schránky.</span><span class="sxs-lookup"><span data-stu-id="7d735-160">Search for the `<UserJourneys>` element, select the entire contents of the `<UserJourney>` node, and then select **Cut** to move the selected text to the clipboard.</span></span>

3. <span data-ttu-id="7d735-161">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a poté vyhledejte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7d735-161">Open the extension file (for example, TrustFrameworkExtensions.xml), and then search for the `<UserJourneys>` element.</span></span> <span data-ttu-id="7d735-162">Pokud element neexistuje, přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="7d735-162">If the element doesn't exist, add it.</span></span>

4. <span data-ttu-id="7d735-163">Vložte celý obsah `<UserJourney>` uzlu, který jste přesunuli do schránky v kroku 2, do `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="7d735-163">Paste the entire contents of the `<UserJourney>` node, which you moved to the clipboard in step 2, into the `<UserJourneys>` element.</span></span>

### <a name="step-42-display-the-button"></a><span data-ttu-id="7d735-164">Krok 4.2: Zobrazit "button"</span><span class="sxs-lookup"><span data-stu-id="7d735-164">Step 4.2: Display the "button"</span></span>
<span data-ttu-id="7d735-165">`<ClaimsProviderSelections>` Element definuje seznam možnosti výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="7d735-165">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span> <span data-ttu-id="7d735-166">`<ClaimsProviderSelection>` Uzel je obdobou tlačítko zprostředkovatele identity na stránce registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7d735-166">The `<ClaimsProviderSelection>` node is analogous to an identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="7d735-167">Pokud přidáte `<ClaimsProviderSelection>` uzel pro účet služby Twitter, nové tlačítko se zobrazí, když uživatel pojmenováváme na stránce.</span><span class="sxs-lookup"><span data-stu-id="7d735-167">If you add a `<ClaimsProviderSelection>` node for a Twitter account, a new button is displayed when a user lands on the page.</span></span> <span data-ttu-id="7d735-168">Pokud chcete přidat tento element, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="7d735-168">To add this element, do the following:</span></span>

1. <span data-ttu-id="7d735-169">Vyhledejte `<UserJourney>` uzlu, který obsahuje `Id="SignUpOrSignIn"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="7d735-169">Search for the `<UserJourney>` node that contains `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>

2. <span data-ttu-id="7d735-170">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="7d735-170">Locate the `<OrchestrationStep>` node that contains `Order="1"`.</span></span>

3. <span data-ttu-id="7d735-171">V `<ClaimsProviderSelections>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="7d735-171">In the `<ClaimsProviderSelections>` element, add the following XML snippet:</span></span>

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a><span data-ttu-id="7d735-172">Krok 4.3: Odkaz na tlačítko akce</span><span class="sxs-lookup"><span data-stu-id="7d735-172">Step 4.3: Link the button to an action</span></span>
<span data-ttu-id="7d735-173">Nyní když máte tlačítka na místě, musíte ho propojit akce.</span><span class="sxs-lookup"><span data-stu-id="7d735-173">Now that you have a button in place, you must link it to an action.</span></span> <span data-ttu-id="7d735-174">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s účtem služby Twitter přijmout token.</span><span class="sxs-lookup"><span data-stu-id="7d735-174">The action, in this case, is for Azure AD B2C to communicate with the Twitter account to receive a token.</span></span> <span data-ttu-id="7d735-175">Pomocí propojení technické profil pro poskytovatele deklarací identity účtu služby Twitter odkazu na tlačítko akce:</span><span class="sxs-lookup"><span data-stu-id="7d735-175">Link the button to an action by linking the technical profile for your Twitter account claims provider:</span></span>

1. <span data-ttu-id="7d735-176">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="7d735-176">Search for the `<OrchestrationStep>` node that contains `Order="2"` in the `<UserJourney>` node.</span></span>
2. <span data-ttu-id="7d735-177">V `<ClaimsExchanges>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="7d735-177">In the `<ClaimsExchanges>` element, add the following XML snippet:</span></span>

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* <span data-ttu-id="7d735-178">Ujistěte se, že `Id` mají stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="7d735-178">Ensure that `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
    >* <span data-ttu-id="7d735-179">Ujistěte se, že `TechnicalProfileReferenceId` ID nastavena na technické profil, který jste vytvořili starší (Twitter OAUTH1).</span><span class="sxs-lookup"><span data-stu-id="7d735-179">Ensure that the `TechnicalProfileReferenceId` ID is set to the technical profile that you created earlier (Twitter-OAUTH1).</span></span>

## <a name="step-5-upload-the-policy-to-your-tenant"></a><span data-ttu-id="7d735-180">Krok 5: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="7d735-180">Step 5: Upload the policy to your tenant</span></span>
1. <span data-ttu-id="7d735-181">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a potom vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="7d735-181">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then select **Azure AD B2C**.</span></span>

2. <span data-ttu-id="7d735-182">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="7d735-182">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="7d735-183">Vyberte **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="7d735-183">Select **All Policies**.</span></span>

4. <span data-ttu-id="7d735-184">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="7d735-184">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="7d735-185">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7d735-185">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="7d735-186">Nahrát *TrustFrameworkBase.xml* a *TrustFrameworkExtensions.xml* soubory a ujistěte se, že jejich úspěšné ověření.</span><span class="sxs-lookup"><span data-stu-id="7d735-186">Upload the *TrustFrameworkBase.xml* and *TrustFrameworkExtensions.xml* files, and ensure that they pass validation.</span></span>

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="7d735-187">Krok 6: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="7d735-187">Step 6: Test the custom policy by using Run Now</span></span>

1. <span data-ttu-id="7d735-188">Vyberte **nastavení Azure AD B2C**a potom vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="7d735-188">Select **Azure AD B2C Settings**, and then select **Identity Experience Framework**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="7d735-189">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="7d735-189">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="7d735-190">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="7d735-190">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="7d735-191">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="7d735-191">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>  
    <span data-ttu-id="7d735-192">Teď by měla být neúspěšné přihlášení pomocí účtu sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="7d735-192">You should now be able to sign in by using the Twitter account.</span></span>

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a><span data-ttu-id="7d735-193">Krok 7: Zprostředkovatele pro úpravy profilu uživatele cesty deklarace identity (volitelné) zaregistrovat účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="7d735-193">Step 7: (Optional) Register the Twitter account claims provider to the profile-edit user journey</span></span>
<span data-ttu-id="7d735-194">Můžete také přidat zprostředkovatele identity účtu služby Twitter na váš `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="7d735-194">You might also want to add the Twitter account identity provider to your `ProfileEdit` user journey.</span></span> <span data-ttu-id="7d735-195">Aby se uživatel přepravě k dispozici, opakování "krok 4."</span><span class="sxs-lookup"><span data-stu-id="7d735-195">To make the user journey available, repeat "Step 4."</span></span> <span data-ttu-id="7d735-196">Tentokrát vyberte `<UserJourney>` uzlu, který obsahuje `Id="ProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="7d735-196">This time, select the `<UserJourney>` node that contains `Id="ProfileEdit"`.</span></span> <span data-ttu-id="7d735-197">Uložit, odeslání a testování vaší zásady.</span><span class="sxs-lookup"><span data-stu-id="7d735-197">Save, upload, and test your policy.</span></span>


## <a name="optional-download-the-complete-policy-files"></a><span data-ttu-id="7d735-198">(Volitelné) Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="7d735-198">(Optional) Download the complete policy files</span></span>
<span data-ttu-id="7d735-199">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="7d735-199">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="7d735-200">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).</span><span class="sxs-lookup"><span data-stu-id="7d735-200">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).</span></span>
