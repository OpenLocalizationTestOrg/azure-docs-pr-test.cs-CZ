---
title: "Azure Active Directory B2C: Přidejte LinkedIn jako zprostředkovatel identity OAuth2 pomocí vlastních zásad"
description: "Postupy: článek o nastavení aplikace LinkedIn pomocí OAuth2 protokol a vlastní zásady"
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
ms.openlocfilehash: 77e2b9b283e4051370ffb905681135c27512834e
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a><span data-ttu-id="ad455-103">Azure Active Directory B2C: Přidejte LinkedIn jako zprostředkovatel identity pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="ad455-103">Azure Active Directory B2C: Add LinkedIn as an identity provider by using custom policies</span></span>
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="ad455-104">Tento článek ukazuje, jak povolit přihlášení pro uživatele účtu LinkedIn pomocí [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="ad455-104">This article shows you how to enable sign-in for users of a LinkedIn account by using [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad455-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad455-105">Prerequisites</span></span>
<span data-ttu-id="ad455-106">Proveďte kroky v [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ad455-106">Complete the steps in the [Get started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

## <a name="step-1-create-a-linkedin-account-application"></a><span data-ttu-id="ad455-107">Krok 1: Vytvoření aplikace účet LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ad455-107">Step 1: Create a LinkedIn account application</span></span>
<span data-ttu-id="ad455-108">Použít LinkedIn jako poskytovatel identit v Azure Active Directory B2C (Azure AD B2C), musíte vytvořit aplikaci LinkedIn a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="ad455-108">To use LinkedIn as an identity provider in Azure Active Directory B2C (Azure AD B2C), you must create a LinkedIn application and supply it with the right parameters.</span></span> <span data-ttu-id="ad455-109">Můžete zaregistrovat aplikaci LinkedIn přechodem na [stránku pro přihlášení LinkedIn](https://LinkedIn.com/signup).</span><span class="sxs-lookup"><span data-stu-id="ad455-109">You can register a LinkedIn application by going to the [LinkedIn sign-up page](https://LinkedIn.com/signup).</span></span>

1. <span data-ttu-id="ad455-110">Přejděte na [Správa aplikací LinkedIn](https://www.linkedin.com/secure/developer?newapp=) web, přihlaste se pomocí přihlašovacích údajů účtu LinkedIn a pak vyberte **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="ad455-110">Go to the [LinkedIn application management](https://www.linkedin.com/secure/developer?newapp=) website, sign in with your LinkedIn account credentials, and then select **Create Application**.</span></span>

    ![LinkedIn účet – vytvoření aplikace](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. <span data-ttu-id="ad455-112">Na **vytvořte novou aplikaci** proveďte následující:</span><span class="sxs-lookup"><span data-stu-id="ad455-112">On the **Create a New Application** page, do the following:</span></span>

    <span data-ttu-id="ad455-113">a.</span><span class="sxs-lookup"><span data-stu-id="ad455-113">a.</span></span> <span data-ttu-id="ad455-114">Typ vaše **název společnosti**, popisný **název** pro společnosti a **popis** nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad455-114">Type your **Company Name**, a descriptive **Name** for the company, and a **Description** of your new app.</span></span>

    <span data-ttu-id="ad455-115">b.</span><span class="sxs-lookup"><span data-stu-id="ad455-115">b.</span></span> <span data-ttu-id="ad455-116">Nahrát váš **Logo aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ad455-116">Upload your **Application Logo**.</span></span>

    <span data-ttu-id="ad455-117">c.</span><span class="sxs-lookup"><span data-stu-id="ad455-117">c.</span></span> <span data-ttu-id="ad455-118">Vyberte **využívání aplikací**.</span><span class="sxs-lookup"><span data-stu-id="ad455-118">Select an **Application Use**.</span></span>

    <span data-ttu-id="ad455-119">d.</span><span class="sxs-lookup"><span data-stu-id="ad455-119">d.</span></span> <span data-ttu-id="ad455-120">V **adresu URL webu** pole, vložte **https://login.microsoftonline.com**.</span><span class="sxs-lookup"><span data-stu-id="ad455-120">In the **Website URL** box, paste **https://login.microsoftonline.com**.</span></span>

    <span data-ttu-id="ad455-121">e.</span><span class="sxs-lookup"><span data-stu-id="ad455-121">e.</span></span> <span data-ttu-id="ad455-122">Typ vaše **e-mailová adresa** adresu a **Telefon do zaměstnání** číslo.</span><span class="sxs-lookup"><span data-stu-id="ad455-122">Type your **Business Email** address and **Business Phone** number.</span></span>

    <span data-ttu-id="ad455-123">f.</span><span class="sxs-lookup"><span data-stu-id="ad455-123">f.</span></span> <span data-ttu-id="ad455-124">V dolní části stránky, přečtěte si a přijměte podmínky použití a pak vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="ad455-124">At the bottom of the page, read and accept the terms of use, and then select **Submit**.</span></span>

    ![LinkedIn účet - konfigurovat vlastnosti aplikace](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. <span data-ttu-id="ad455-126">Vyberte **ověřování**a poznamenejte si **ID klienta** a **tajný klíč klienta** hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ad455-126">Select **Authentication**, and then note the **Client ID** and **Client Secret** values.</span></span>

4. <span data-ttu-id="ad455-127">V **oprávnění adres URL pro přesměrování** pole, vložte **https://login.microsoftonline.com/te/ {tenant}.onmicrosoft.com/oauth2/authresp**.</span><span class="sxs-lookup"><span data-stu-id="ad455-127">In the **Authorized Redirect URLs** box, paste **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp**.</span></span> <span data-ttu-id="ad455-128">Nahraďte {*klienta*} s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="ad455-128">Replace {*tenant*} with your tenant name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="ad455-129">Ujistěte se, že používáte schéma HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ad455-129">Make sure that you are using the HTTPS scheme.</span></span> 

    ![Účet LinkedIn – sadu oprávnění přesměrování adresy URL](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    ><span data-ttu-id="ad455-131">Tajný klíč klienta je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="ad455-131">The client secret is an important security credential.</span></span> <span data-ttu-id="ad455-132">S kýmkoli sdílet tento tajný klíč nebo distribuovat s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="ad455-132">Do not share this secret with anyone or distribute it with your app.</span></span>

5. <span data-ttu-id="ad455-133">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ad455-133">Select **Add**.</span></span>

6. <span data-ttu-id="ad455-134">Vyberte **nastavení**, změnit **stav aplikace** k **živé**a potom vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="ad455-134">Select **Settings**, change the **Application status** to **Live**, and then select **Update**.</span></span>

    ![Účet LinkedIn – nastavit stav aplikace](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a><span data-ttu-id="ad455-136">Krok 2: Přidejte svůj klíč LinkedIn aplikace do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="ad455-136">Step 2: Add your LinkedIn application key to Azure AD B2C</span></span>
<span data-ttu-id="ad455-137">Federace s účty LinkedIn vyžaduje tajný klíč klienta pro účet LinkedIn do vztahu důvěryhodnosti Azure AD B2C jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="ad455-137">Federation with LinkedIn accounts requires a client secret for the LinkedIn account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="ad455-138">K uložení tajný klíč aplikace LinkedIn v klientovi služby Azure AD B2C, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ad455-138">To store the LinkedIn application secret in your Azure AD B2C tenant, do the following:</span></span>  

1. <span data-ttu-id="ad455-139">Ve vašem klientu Azure AD B2C, vyberte **nastavení B2C** > **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="ad455-139">In your Azure AD B2C tenant, select **B2C Settings** > **Identity Experience Framework**.</span></span>

2. <span data-ttu-id="ad455-140">Chcete-li zobrazit klíčů, které jsou k dispozici ve vašem klientovi, vyberte **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="ad455-140">To view the keys that are available in your tenant, select **Policy Keys**.</span></span>

3. <span data-ttu-id="ad455-141">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ad455-141">Select **Add**.</span></span>

4. <span data-ttu-id="ad455-142">V **možnosti** vyberte **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="ad455-142">In the **Options** box, select **Upload**.</span></span>

5. <span data-ttu-id="ad455-143">V **název** zadejte **B2cRestClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="ad455-143">In the **Name** box, type **B2cRestClientCertificate**.</span></span>  
    <span data-ttu-id="ad455-144">Předpona *B2C_1A_* může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="ad455-144">The prefix *B2C_1A_* might be added automatically.</span></span>

6. <span data-ttu-id="ad455-145">V **tajný klíč** zadejte váš tajný klíč aplikace LinkedIn z [portálu pro registraci aplikace](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="ad455-145">In the **Secret** box, enter your LinkedIn application secret from the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span>

7. <span data-ttu-id="ad455-146">Pro **použití klíče**, vyberte **šifrování**.</span><span class="sxs-lookup"><span data-stu-id="ad455-146">For **Key usage**, select **Encryption**.</span></span>

8. <span data-ttu-id="ad455-147">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ad455-147">Select **Create**.</span></span> 

9. <span data-ttu-id="ad455-148">Potvrďte, že jste vytvořili `B2C_1A_LinkedInSecret`klíč.</span><span class="sxs-lookup"><span data-stu-id="ad455-148">Confirm that you've created the `B2C_1A_LinkedInSecret`key.</span></span>

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="ad455-149">Krok 3: Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="ad455-149">Step 3: Add a claims provider in your extension policy</span></span>
<span data-ttu-id="ad455-150">Pokud chcete uživatelům přihlásit pomocí svého účtu LinkedIn, je nutné zadat LinkedIn jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="ad455-150">If you want users to sign in by using their LinkedIn account, you must define LinkedIn as a claims provider.</span></span> <span data-ttu-id="ad455-151">Jinými slovy je nutné zadat koncové body, které komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ad455-151">In other words, you must specify the endpoints that Azure AD B2C communicates with.</span></span> <span data-ttu-id="ad455-152">Koncové body poskytují sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad455-152">The endpoints provide a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="ad455-153">Definování LinkedIn jako poskytovatele deklarací identity tak, že přidáte `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="ad455-153">Define LinkedIn as a claims provider by adding a `<ClaimsProvider>` node in your extension policy file:</span></span>

1. <span data-ttu-id="ad455-154">V pracovním adresáři, otevřete *TrustFrameworkExtensions.xml* soubor rozšíření zásad.</span><span class="sxs-lookup"><span data-stu-id="ad455-154">In your working directory, open the *TrustFrameworkExtensions.xml* extension policy file.</span></span> 

2. <span data-ttu-id="ad455-155">Vyhledejte `<ClaimsProviders>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ad455-155">Search for the `<ClaimsProviders>` element.</span></span>

3. <span data-ttu-id="ad455-156">V `<ClaimsProviders>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="ad455-156">In the `<ClaimsProviders>` element, add the following XML snippet:</span></span> 

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
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

4. <span data-ttu-id="ad455-157">Nahraďte *client_id* hodnotu s vaším ID klienta aplikace LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="ad455-157">Replace the *client_id* value with your LinkedIn application client ID.</span></span>

5. <span data-ttu-id="ad455-158">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ad455-158">Save the file.</span></span>

## <a name="step-4-register-the-linkedin-account-claims-provider"></a><span data-ttu-id="ad455-159">Krok 4: Registrace zprostředkovatele deklarací identity účtu LinkedIn</span><span class="sxs-lookup"><span data-stu-id="ad455-159">Step 4: Register the LinkedIn account claims provider</span></span>
<span data-ttu-id="ad455-160">Nastavili jste si poskytovatele identit.</span><span class="sxs-lookup"><span data-stu-id="ad455-160">You've set up the identity provider.</span></span> <span data-ttu-id="ad455-161">Ale ho dosud nejsou k dispozici v žádném z registrace nebo přihlášení systému windows.</span><span class="sxs-lookup"><span data-stu-id="ad455-161">However, it is not yet available in any of the sign-up or sign-in windows.</span></span> <span data-ttu-id="ad455-162">Nyní je nutné přidat zprostředkovatele identity účtu LinkedIn pro vaše uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad455-162">Now you must add the LinkedIn account identity provider to your user `SignUpOrSignIn` user journey.</span></span>

### <a name="step-41-make-a-copy-of-the-user-journey"></a><span data-ttu-id="ad455-163">Krok 4.1: Vytvoření kopie cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="ad455-163">Step 4.1: Make a copy of the user journey</span></span>
<span data-ttu-id="ad455-164">Chcete-li k dispozici cesty uživatele, vytvořte duplicitní existující šablony cesty uživatele a pak přidejte zprostředkovatele identity LinkedIn:</span><span class="sxs-lookup"><span data-stu-id="ad455-164">To make the user journey available, you create a duplicate of an existing user journey template and then add the LinkedIn identity provider:</span></span>

>[!NOTE]
><span data-ttu-id="ad455-165">Pokud jste zkopírovali `<UserJourneys>` element ze základního souboru v zásadách *TrustFrameworkExtensions.xml* souboru rozšíření, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="ad455-165">If you copied the `<UserJourneys>` element from the base file of your policy to the *TrustFrameworkExtensions.xml* extension file, you can skip this section.</span></span>

1. <span data-ttu-id="ad455-166">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="ad455-166">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>

2. <span data-ttu-id="ad455-167">Vyhledejte `<UserJourneys>` element, vyberte celý obsah `<UserJourney>` uzel a potom vyberte **Vyjmout** přesunout vybraný text do schránky.</span><span class="sxs-lookup"><span data-stu-id="ad455-167">Search for the `<UserJourneys>` element, select the entire contents of the `<UserJourney>` node, and then select **Cut** to move the selected text to the clipboard.</span></span>

3. <span data-ttu-id="ad455-168">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a vyhledejte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ad455-168">Open the extension file (for example, TrustFrameworkExtensions.xml), and search for the `<UserJourneys>` element.</span></span> <span data-ttu-id="ad455-169">Pokud element neexistuje, přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="ad455-169">If the element doesn't exist, add it.</span></span>

4. <span data-ttu-id="ad455-170">Vložte celý obsah `<UserJourney>` uzlu, který jste přesunuli do schránky v kroku 2, do `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="ad455-170">Paste the entire contents of the `<UserJourney>` node, which you moved to the clipboard in step 2, into the `<UserJourneys>` element.</span></span>

### <a name="step-42-display-the-button"></a><span data-ttu-id="ad455-171">Krok 4.2: Zobrazit "button"</span><span class="sxs-lookup"><span data-stu-id="ad455-171">Step 4.2: Display the "button"</span></span>
<span data-ttu-id="ad455-172">`<ClaimsProviderSelections>` Element definuje seznam možnosti výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="ad455-172">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span> <span data-ttu-id="ad455-173">`<ClaimsProviderSelection>` Uzel je obdobou tlačítko zprostředkovatele identity na stránce registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ad455-173">The `<ClaimsProviderSelection>` node is analogous to an identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="ad455-174">Pokud přidáte `<ClaimsProviderSelection>` uzel pro účet LinkedIn nové tlačítko se zobrazí, když uživatel pojmenováváme na stránce.</span><span class="sxs-lookup"><span data-stu-id="ad455-174">If you add a `<ClaimsProviderSelection>` node for a LinkedIn account, a new button is displayed when a user lands on the page.</span></span> <span data-ttu-id="ad455-175">Pokud chcete přidat tento element, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ad455-175">To add this element, do the following:</span></span>

1. <span data-ttu-id="ad455-176">Vyhledejte `<UserJourney>` uzlu, který obsahuje `Id="SignUpOrSignIn"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="ad455-176">Search for the `<UserJourney>` node that contains `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>

2. <span data-ttu-id="ad455-177">Vyhledejte `<OrchestrationStep>` uzlu, který zahrnuje `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="ad455-177">Locate the `<OrchestrationStep>` node that includes `Order="1"`.</span></span>

3. <span data-ttu-id="ad455-178">V `<ClaimsProviderSelections>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="ad455-178">In the `<ClaimsProviderSelections>` element, add the following XML snippet:</span></span>

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a><span data-ttu-id="ad455-179">Krok 4.3: Odkaz na tlačítko akce</span><span class="sxs-lookup"><span data-stu-id="ad455-179">Step 4.3: Link the button to an action</span></span>
<span data-ttu-id="ad455-180">Nyní když máte tlačítka na místě, musíte ho propojit akce.</span><span class="sxs-lookup"><span data-stu-id="ad455-180">Now that you have a button in place, you must link it to an action.</span></span> <span data-ttu-id="ad455-181">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s účtem LinkedIn přijmout token.</span><span class="sxs-lookup"><span data-stu-id="ad455-181">The action, in this case, is for Azure AD B2C to communicate with the LinkedIn account to receive a token.</span></span> <span data-ttu-id="ad455-182">Pomocí propojení technické profil pro poskytovatele deklarací identity účtu LinkedIn odkazu na tlačítko akce:</span><span class="sxs-lookup"><span data-stu-id="ad455-182">Link the button to an action by linking the technical profile for your LinkedIn account claims provider:</span></span>

1. <span data-ttu-id="ad455-183">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="ad455-183">Search for the `<OrchestrationStep>` node that contains `Order="2"` in the `<UserJourney>` node.</span></span>

2. <span data-ttu-id="ad455-184">V `<ClaimsExchanges>` elementu, přidejte následující fragment kódu XML:</span><span class="sxs-lookup"><span data-stu-id="ad455-184">In the `<ClaimsExchanges>` element, add the following XML snippet:</span></span>

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* <span data-ttu-id="ad455-185">Ujistěte se, že `Id` mají stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="ad455-185">Ensure that `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
    >* <span data-ttu-id="ad455-186">Ujistěte se, že `TechnicalProfileReferenceId` ID je nastaven na technické profil vytvořený starší (-OAuth pro LinkedIn).</span><span class="sxs-lookup"><span data-stu-id="ad455-186">Ensure that the `TechnicalProfileReferenceId` ID is set to the technical profile that you created earlier (LinkedIn-OAuth).</span></span>

## <a name="step-5-upload-the-policy-to-your-tenant"></a><span data-ttu-id="ad455-187">Krok 5: Nahrajte zásady klienta</span><span class="sxs-lookup"><span data-stu-id="ad455-187">Step 5: Upload the policy to your tenant</span></span>
1. <span data-ttu-id="ad455-188">V [portál Azure](https://portal.azure.com), přepnout [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a potom vyberte **Azure AD B2C**.</span><span class="sxs-lookup"><span data-stu-id="ad455-188">In the [Azure portal](https://portal.azure.com), switch to the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then select **Azure AD B2C**.</span></span>

2. <span data-ttu-id="ad455-189">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="ad455-189">Select **Identity Experience Framework**.</span></span>

3. <span data-ttu-id="ad455-190">Vyberte **všechny zásady**.</span><span class="sxs-lookup"><span data-stu-id="ad455-190">Select **All Policies**.</span></span>

4. <span data-ttu-id="ad455-191">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="ad455-191">Select **Upload Policy**.</span></span>

5. <span data-ttu-id="ad455-192">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="ad455-192">Select the **Overwrite the policy if it exists** check box.</span></span>

6. <span data-ttu-id="ad455-193">Nahrát *TrustFrameworkBase.xml* a *TrustFrameworkExtensions.xml* soubory a ujistěte se, že jejich úspěšné ověření.</span><span class="sxs-lookup"><span data-stu-id="ad455-193">Upload the *TrustFrameworkBase.xml* and *TrustFrameworkExtensions.xml* files, and ensure that they pass validation.</span></span>

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="ad455-194">Krok 6: Testování spustit pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="ad455-194">Step 6: Test the custom policy by using Run Now</span></span>
1. <span data-ttu-id="ad455-195">Vyberte **nastavení Azure AD B2C**a potom vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="ad455-195">Select **Azure AD B2C Settings**, and then select **Identity Experience Framework**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="ad455-196">Spustit nyní vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="ad455-196">Run Now requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="ad455-197">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ad455-197">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>

2. <span data-ttu-id="ad455-198">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, které jste nahráli a potom vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="ad455-198">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded, and then select **Run now**.</span></span>  
    <span data-ttu-id="ad455-199">Nyní byste měli být moct přihlásit pomocí účtu LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="ad455-199">You should now be able to sign in by using the LinkedIn account.</span></span>

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a><span data-ttu-id="ad455-200">Krok 7: (Volitelné) zaregistrovat účet LinkedIn deklarací zprostředkovatele, který se cesty úpravy profilu uživatele</span><span class="sxs-lookup"><span data-stu-id="ad455-200">Step 7: (Optional) Register the LinkedIn account claims provider to the Profile-Edit user journey</span></span>
<span data-ttu-id="ad455-201">Můžete také přidat zprostředkovatele identity LinkedIn účet, který má vaše `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad455-201">You might also want to add the LinkedIn account identity provider to your `ProfileEdit` user journey.</span></span> <span data-ttu-id="ad455-202">Aby se uživatel přepravě k dispozici, opakování "krok 4."</span><span class="sxs-lookup"><span data-stu-id="ad455-202">To make the user journey available, repeat "Step 4."</span></span> <span data-ttu-id="ad455-203">Tentokrát vyberte `<UserJourney>` uzlu, který obsahuje `Id="ProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="ad455-203">This time, select the `<UserJourney>` node that contains `Id="ProfileEdit"`.</span></span> <span data-ttu-id="ad455-204">Uložit, odeslání a testování vaší zásady.</span><span class="sxs-lookup"><span data-stu-id="ad455-204">Save, upload, and test your policy.</span></span>

## <a name="optional-download-the-complete-policy-files"></a><span data-ttu-id="ad455-205">(Volitelné) Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="ad455-205">(Optional) Download the complete policy files</span></span>
<span data-ttu-id="ad455-206">Po dokončení [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) návod, doporučujeme vám vytvořit váš scénář pomocí vlastních zásad pro soubory.</span><span class="sxs-lookup"><span data-stu-id="ad455-206">After you complete the [Get started with custom policies](active-directory-b2c-get-started-custom.md) walkthrough, we recommend that you build your scenario by using your own custom policy files.</span></span> <span data-ttu-id="ad455-207">Pro vaši informaci uvádíme [ukázkové soubory zásad](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).</span><span class="sxs-lookup"><span data-stu-id="ad455-207">For your reference, we have provided [Sample policy files](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).</span></span>
