---
title: "Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad"
description: "Ukázku pomocí Google + jako zprostředkovatele identity pomocí protokolu OAuth2"
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
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: d389a44ce38d84e510060f3b0a53cda58513dee5
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a><span data-ttu-id="67ea6-103">Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="67ea6-103">Azure Active Directory B2C: Add Google+ as an OAuth2 identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="67ea6-104">Tento průvodce vám ukáže, jak povolit přihlášení pro uživatele z účtu Google + prostřednictvím [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="67ea6-104">This guide shows you how to enable sign-in for users from Google+ account through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67ea6-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67ea6-105">Prerequisites</span></span>

<span data-ttu-id="67ea6-106">Proveďte kroky v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="67ea6-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="67ea6-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="67ea6-107">These steps include:</span></span>

1.  <span data-ttu-id="67ea6-108">Vytvoření aplikace pro účet Google +.</span><span class="sxs-lookup"><span data-stu-id="67ea6-108">Creating a Google+ account application.</span></span>
2.  <span data-ttu-id="67ea6-109">Přidání aplikace klíč účtu Google + do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="67ea6-109">Adding the Google+ account application key to Azure AD B2C</span></span>
3.  <span data-ttu-id="67ea6-110">Přidání poskytovatele deklarací identity k zásadě</span><span class="sxs-lookup"><span data-stu-id="67ea6-110">Adding claims provider to a policy</span></span>
4.  <span data-ttu-id="67ea6-111">Registrace Google + účet zprostředkovatele deklarací identity pro uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="67ea6-111">Registering the Google+ account claims provider to a user journey</span></span>
5.  <span data-ttu-id="67ea6-112">Odeslání zásady do Azure AD B2C klienta a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="67ea6-112">Uploading the policy to an Azure AD B2C tenant and test it</span></span>

## <a name="create-a-google-account-application"></a><span data-ttu-id="67ea6-113">Vytvoření aplikace účet Google +</span><span class="sxs-lookup"><span data-stu-id="67ea6-113">Create a Google+ account application</span></span>
<span data-ttu-id="67ea6-114">Pokud chcete používat Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci Google + a zadat se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="67ea6-114">To use Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Google+ application and supply it with the right parameters.</span></span> <span data-ttu-id="67ea6-115">Můžete zaregistrovat Google aplikace + zde: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span><span class="sxs-lookup"><span data-stu-id="67ea6-115">You can register a Google+ application here: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span></span>

1.  <span data-ttu-id="67ea6-116">Přejděte na [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="67ea6-116">Go to the [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2.  <span data-ttu-id="67ea6-117">Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-117">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>

3.  <span data-ttu-id="67ea6-118">Klikněte na **projekty nabídky**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-118">Click on the **Projects menu**.</span></span>

    ![Google + účet – vyberte projektu](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  <span data-ttu-id="67ea6-120">Klikněte na  **+**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="67ea6-120">Click on the **+** button.</span></span>

    ![Google + účet – vytvoření nového projektu](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  <span data-ttu-id="67ea6-122">Zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-122">Enter a **Project name**, and then click **Create**.</span></span>

    ![Google + účet – nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  <span data-ttu-id="67ea6-124">Počkejte, dokud není připraven na projekt a klikněte na **projekty nabídky**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-124">Wait until the project is ready and click on the **Projects menu**.</span></span>

    ![Google + účet – Počkejte, až je připravený k použití nového projektu](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  <span data-ttu-id="67ea6-126">Klikněte na název projektu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-126">Click on your project name.</span></span>

    ![Google + účet – vyberte nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  <span data-ttu-id="67ea6-128">Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-128">Click **API Manager** and then click **Credentials** in the left navigation.</span></span>
9.  <span data-ttu-id="67ea6-129">Klikněte **obrazovky souhlas OAuth** v horní části.</span><span class="sxs-lookup"><span data-stu-id="67ea6-129">Click the **OAuth consent screen** tab at the top.</span></span>

    ![Google + účet – nastavte OAuth souhlasu obrazovky](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  <span data-ttu-id="67ea6-131">Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-131">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>

    ![Google + - přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  <span data-ttu-id="67ea6-133">Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-133">Click **New credentials** and then choose **OAuth client ID**.</span></span>

    ![Google + - vytvořit nové přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  <span data-ttu-id="67ea6-135">V části **typ aplikace**, vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-135">Under **Application type**, select **Web application**.</span></span>

    ![Google + - výběr typu aplikace](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  <span data-ttu-id="67ea6-137">Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **autorizováno přesměrování identifikátory URI** pole.</span><span class="sxs-lookup"><span data-stu-id="67ea6-137">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in the **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized redirect URIs** field.</span></span> <span data-ttu-id="67ea6-138">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="67ea6-138">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="67ea6-139">**{Klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="67ea6-139">The **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="67ea6-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-140">Click **Create**.</span></span>

    ![Google + - zadejte oprávnění JavaScript zdroje a identifikátory URI pro přesměrování](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  <span data-ttu-id="67ea6-142">Zkopírujte hodnoty z **Id klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-142">Copy the values of **Client Id** and **Client secret**.</span></span> <span data-ttu-id="67ea6-143">Je nutné obě konfigurace Google + jako zprostředkovatele identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="67ea6-143">You need both to configure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="67ea6-144">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="67ea6-144">**Client secret** is an important security credential.</span></span>

    ![Google + - zkopírujte hodnoty Id a klienta sdílený tajný klíč klienta](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="67ea6-146">Klíč aplikace účet Google + přidat do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="67ea6-146">Add the Google+ account application key to Azure AD B2C</span></span>
<span data-ttu-id="67ea6-147">Federace se Google + účty vyžaduje tajný klíč klienta pro účet Google + do vztahu důvěryhodnosti Azure AD B2C jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="67ea6-147">Federation with Google+ accounts requires a client secret for Google+ account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="67ea6-148">Je třeba uložit vaše Google + tajný klíč aplikace v Azure AD B2C klienta:</span><span class="sxs-lookup"><span data-stu-id="67ea6-148">You need to store your Google+ application secret in Azure AD B2C tenant:</span></span>  

1.  <span data-ttu-id="67ea6-149">Přejděte ke klientovi Azure AD B2C a vyberte **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="67ea6-149">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="67ea6-150">Vyberte **zásad klíče** zobrazíte klíče, které jsou k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="67ea6-150">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="67ea6-151">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-151">Click **+Add**.</span></span>
4.  <span data-ttu-id="67ea6-152">Pro **možnosti**, použijte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-152">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="67ea6-153">Pro **název**, použijte `GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="67ea6-153">For **Name**, use `GoogleSecret`.</span></span>  
    <span data-ttu-id="67ea6-154">Předpona `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="67ea6-154">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="67ea6-155">V **tajný klíč** zadejte tajný klíč Google vaší aplikace z [konzole pro vývojáře Google](https://console.developers.google.com/) zkopírovaného výše.</span><span class="sxs-lookup"><span data-stu-id="67ea6-155">In the **Secret** box, enter your Google application secret from the [Google Developers Console](https://console.developers.google.com/) that you copied above.</span></span>
7.  <span data-ttu-id="67ea6-156">Pro **použití klíče**, použijte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-156">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="67ea6-157">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="67ea6-157">Click **Create**</span></span>
9.  <span data-ttu-id="67ea6-158">Potvrďte, že jste vytvořili klíč `B2C_1A_GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="67ea6-158">Confirm that you've created the key `B2C_1A_GoogleSecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="67ea6-159">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="67ea6-159">Add a claims provider in your extension policy</span></span>

<span data-ttu-id="67ea6-160">Pokud chcete uživatelům přihlášení pomocí účtu Google +, musíte zadat účet Google + jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="67ea6-160">If you want users to sign in by using Google+ account, you need to define Google+ account as a claims provider.</span></span> <span data-ttu-id="67ea6-161">Jinými slovy budete muset zadat koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="67ea6-161">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="67ea6-162">Koncový bod poskytuje sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="67ea6-162">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="67ea6-163">Zadejte účet Google + jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="67ea6-163">Define Google+ Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="67ea6-164">Otevřete soubor rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="67ea6-164">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="67ea6-165">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="67ea6-165">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="67ea6-166">Najít `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="67ea6-166">Find the `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="67ea6-167">Přidejte následující fragment kódu XML v části `ClaimsProviders` elementu a nahraďte `client_id` hodnotu s vaší Google + účet ID klienta aplikace před uložením souboru.</span><span class="sxs-lookup"><span data-stu-id="67ea6-167">Add the following XML snippet under the `ClaimsProviders` element and replace `client_id` value with your Google+ account application client ID before saving the file.</span></span>  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="67ea6-168">Zaregistrovat Google + účet zprostředkovatele deklarací identity registrace nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="67ea6-168">Register the Google+ account claims provider to Sign up or Sign in user journey</span></span>

<span data-ttu-id="67ea6-169">Poskytovatel identity nastavit.</span><span class="sxs-lookup"><span data-stu-id="67ea6-169">The identity provider has been set up.</span></span>  <span data-ttu-id="67ea6-170">Však není k dispozici v žádném z obrazovky registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="67ea6-170">However, it is not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="67ea6-171">Přidejte zprostředkovatele identity účet Google + pro vaše uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="67ea6-171">Add the Google+ account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="67ea6-172">Chcete-li k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="67ea6-172">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="67ea6-173">Potom přidáme zprostředkovatele identity účet Google +:</span><span class="sxs-lookup"><span data-stu-id="67ea6-173">Then we add the Google+ account identity provider:</span></span>

>[!NOTE]
>
><span data-ttu-id="67ea6-174">Pokud jste zkopírovali `<UserJourneys>` element ze základního souboru zásad k rozšíření souboru (TrustFrameworkExtensions.xml), můžete přeskočit k této části.</span><span class="sxs-lookup"><span data-stu-id="67ea6-174">If you copied the `<UserJourneys>` element from base file of your policy to the extension file (TrustFrameworkExtensions.xml), you can skip to this section.</span></span>

1.  <span data-ttu-id="67ea6-175">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="67ea6-175">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="67ea6-176">Najít `<UserJourneys>` elementu a zkopírujte celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-176">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="67ea6-177">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a najděte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-177">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="67ea6-178">Pokud element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="67ea6-178">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="67ea6-179">Vložte celý obsah `<UserJourney>` uzlu, který jste zkopírovali jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-179">Paste the entire content of `<UserJourney>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="67ea6-180">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="67ea6-180">Display the button</span></span>
<span data-ttu-id="67ea6-181">`<ClaimsProviderSelections>` Element definuje seznam možnosti výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="67ea6-181">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="67ea6-182">`<ClaimsProviderSelection>` Element je obdobou tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="67ea6-182">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="67ea6-183">Pokud přidáte `<ClaimsProviderSelection>` element pro účet Google + nové tlačítko se zobrazí při pojmenováváme uživatele na stránce.</span><span class="sxs-lookup"><span data-stu-id="67ea6-183">If you add a `<ClaimsProviderSelection>` element for Google+ account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="67ea6-184">Chcete-li přidat tento element:</span><span class="sxs-lookup"><span data-stu-id="67ea6-184">To add this element:</span></span>

1.  <span data-ttu-id="67ea6-185">Najít `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="67ea6-185">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="67ea6-186">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje `Order="1"`</span><span class="sxs-lookup"><span data-stu-id="67ea6-186">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="67ea6-187">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="67ea6-187">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="67ea6-188">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="67ea6-188">Link the button to an action</span></span>
<span data-ttu-id="67ea6-189">Nyní když máte tlačítka na místě, budete muset propojit akce.</span><span class="sxs-lookup"><span data-stu-id="67ea6-189">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="67ea6-190">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s účet Google + přijmout token.</span><span class="sxs-lookup"><span data-stu-id="67ea6-190">The action, in this case, is for Azure AD B2C to communicate with Google+ account to receive a token.</span></span>

1.  <span data-ttu-id="67ea6-191">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-191">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="67ea6-192">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="67ea6-192">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * <span data-ttu-id="67ea6-193">Ujistěte se, `Id` mají stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části</span><span class="sxs-lookup"><span data-stu-id="67ea6-193">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section</span></span>
> * <span data-ttu-id="67ea6-194">Ujistěte se, `TechnicalProfileReferenceId` ID je nastaveno na technické profil vytvoříte starší (Google OAUTH).</span><span class="sxs-lookup"><span data-stu-id="67ea6-194">Ensure `TechnicalProfileReferenceId` ID is set to the technical profile you created earlier (Google-OAUTH).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="67ea6-195">Nahrát zásady klienta</span><span class="sxs-lookup"><span data-stu-id="67ea6-195">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="67ea6-196">V [portál Azure](https://portal.azure.com), přepněte do [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="67ea6-196">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="67ea6-197">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-197">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="67ea6-198">Otevřete **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="67ea6-198">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="67ea6-199">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-199">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="67ea6-200">Zkontrolujte **přepsat zásady, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="67ea6-200">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="67ea6-201">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, že neselže ověření</span><span class="sxs-lookup"><span data-stu-id="67ea6-201">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="67ea6-202">Otestovat vlastní zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="67ea6-202">Test the custom policy by using Run Now</span></span>
1.  <span data-ttu-id="67ea6-203">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-203">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>

    >[!NOTE]
    >
    >    <span data-ttu-id="67ea6-204">**Spustit nyní** vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="67ea6-204">**Run now** requires at least one application to be preregistered on the tenant.</span></span> 
    >    <span data-ttu-id="67ea6-205">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="67ea6-205">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>


2.  <span data-ttu-id="67ea6-206">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="67ea6-206">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="67ea6-207">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-207">Select **Run now**.</span></span>
3.  <span data-ttu-id="67ea6-208">Nyní byste měli mít k přihlášení pomocí účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="67ea6-208">You should be able to sign in using Google+ account.</span></span>

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="67ea6-209">[Nepovinné] Zaregistrovat Google + účet zprostředkovatele deklarací identity pro úpravy profilu uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="67ea6-209">[Optional] Register the Google+ account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="67ea6-210">Můžete také přidat zprostředkovatele identity účet Google + pro vaše uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="67ea6-210">You may want to add the Google+ account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="67ea6-211">Chcete-li k dispozici, jsme zopakujte poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="67ea6-211">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="67ea6-212">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="67ea6-212">Display the button</span></span>
1.  <span data-ttu-id="67ea6-213">Otevřete soubor rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="67ea6-213">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="67ea6-214">Najít `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="67ea6-214">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="67ea6-215">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje `Order="1"`</span><span class="sxs-lookup"><span data-stu-id="67ea6-215">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="67ea6-216">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="67ea6-216">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="67ea6-217">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="67ea6-217">Link the button to an action</span></span>
1.  <span data-ttu-id="67ea6-218">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="67ea6-218">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="67ea6-219">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="67ea6-219">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="67ea6-220">Nahrát zásady klienta</span><span class="sxs-lookup"><span data-stu-id="67ea6-220">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="67ea6-221">V [portál Azure](https://portal.azure.com), přepněte do [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="67ea6-221">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="67ea6-222">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-222">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="67ea6-223">Otevřete **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="67ea6-223">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="67ea6-224">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-224">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="67ea6-225">Zkontrolujte **přepsat zásady, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="67ea6-225">Check the **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="67ea6-226">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, že ověření neselže.</span><span class="sxs-lookup"><span data-stu-id="67ea6-226">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation.</span></span>

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="67ea6-227">Otestovat vlastní úpravy profilu zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="67ea6-227">Test the custom Profile-Edit policy by using Run Now</span></span>

1.  <span data-ttu-id="67ea6-228">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-228">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="67ea6-229">Otevřete **B2C_1A_ProfileEdit**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="67ea6-229">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="67ea6-230">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="67ea6-230">Select **Run now**.</span></span>
3.  <span data-ttu-id="67ea6-231">Nyní byste měli mít k přihlášení pomocí účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="67ea6-231">You should be able to sign in using Google+ account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="67ea6-232">Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="67ea6-232">Download the complete policy files</span></span>
<span data-ttu-id="67ea6-233">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní vlastní zásady, které soubory po dokončení Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="67ea6-233">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="67ea6-234">Ukázkové soubory zásad pro – referenční informace</span><span class="sxs-lookup"><span data-stu-id="67ea6-234">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
