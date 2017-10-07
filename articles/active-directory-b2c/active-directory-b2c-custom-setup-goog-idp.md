---
title: "Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad"
description: "Ukázku pomocí Google + jako zprostředkovatele identity pomocí protokolu OAuth2"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 4feff21979c9c3b3b12c7a1cae4db0121d1bd79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a><span data-ttu-id="821c5-103">Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="821c5-103">Azure Active Directory B2C: Add Google+ as an OAuth2 identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="821c5-104">Tento průvodce vám ukáže, jak tooenable přihlášení pro uživatele z účtu Google + prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="821c5-104">This guide shows you how tooenable sign-in for users from Google+ account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="821c5-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="821c5-105">Prerequisites</span></span>

<span data-ttu-id="821c5-106">Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="821c5-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="821c5-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="821c5-107">These steps include:</span></span>

1.  <span data-ttu-id="821c5-108">Vytvoření aplikace pro účet Google +.</span><span class="sxs-lookup"><span data-stu-id="821c5-108">Creating a Google+ account application.</span></span>
2.  <span data-ttu-id="821c5-109">Přidání hello Google + účet aplikace klíče tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="821c5-109">Adding hello Google+ account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="821c5-110">Přidání zásad tooa poskytovatele deklarací identity</span><span class="sxs-lookup"><span data-stu-id="821c5-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="821c5-111">Registrace hello Google + účet deklarace identity zprostředkovatele tooa uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="821c5-111">Registering hello Google+ account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="821c5-112">Odesílání hello zásad tooan Azure AD B2C klienta a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="821c5-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-google-account-application"></a><span data-ttu-id="821c5-113">Vytvoření aplikace účet Google +</span><span class="sxs-lookup"><span data-stu-id="821c5-113">Create a Google+ account application</span></span>
<span data-ttu-id="821c5-114">toouse Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Google + a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="821c5-114">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="821c5-115">Můžete zaregistrovat Google aplikace + zde: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span><span class="sxs-lookup"><span data-stu-id="821c5-115">You can register a Google+ application here: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span></span>

1.  <span data-ttu-id="821c5-116">Přejděte toohello [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.</span><span class="sxs-lookup"><span data-stu-id="821c5-116">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2.  <span data-ttu-id="821c5-117">Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="821c5-117">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>

3.  <span data-ttu-id="821c5-118">Klikněte na hello **projekty nabídky**.</span><span class="sxs-lookup"><span data-stu-id="821c5-118">Click on hello **Projects menu**.</span></span>

    ![Google + účet – vyberte projektu](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  <span data-ttu-id="821c5-120">Klikněte na hello  **+**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="821c5-120">Click on hello **+** button.</span></span>

    ![Google + účet – vytvoření nového projektu](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  <span data-ttu-id="821c5-122">Zadejte **název projektu**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="821c5-122">Enter a **Project name**, and then click **Create**.</span></span>

    ![Google + účet – nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  <span data-ttu-id="821c5-124">Počkejte, dokud je připraven hello projekt a klikněte na hello **projekty nabídky**.</span><span class="sxs-lookup"><span data-stu-id="821c5-124">Wait until hello project is ready and click on hello **Projects menu**.</span></span>

    ![Google + účet – Počkejte, až je připraven toouse nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  <span data-ttu-id="821c5-126">Klikněte na název projektu.</span><span class="sxs-lookup"><span data-stu-id="821c5-126">Click on your project name.</span></span>

    ![Google + účet – nový projekt vyberte hello](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  <span data-ttu-id="821c5-128">Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levé navigační hello.</span><span class="sxs-lookup"><span data-stu-id="821c5-128">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
9.  <span data-ttu-id="821c5-129">Klikněte na tlačítko hello **obrazovky souhlas OAuth** karty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="821c5-129">Click hello **OAuth consent screen** tab at hello top.</span></span>

    ![Google + účet – nastavte OAuth souhlasu obrazovky](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  <span data-ttu-id="821c5-131">Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="821c5-131">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>

    ![Google + - přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  <span data-ttu-id="821c5-133">Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="821c5-133">Click **New credentials** and then choose **OAuth client ID**.</span></span>

    ![Google + - vytvořit nové přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  <span data-ttu-id="821c5-135">V části **typ aplikace**, vyberte **webové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="821c5-135">Under **Application type**, select **Web application**.</span></span>

    ![Google + - výběr typu aplikace](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  <span data-ttu-id="821c5-137">Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v hello **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **autorizováno přesměrování identifikátory URI**pole.</span><span class="sxs-lookup"><span data-stu-id="821c5-137">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="821c5-138">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="821c5-138">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="821c5-139">Hello **{klient}** hodnota je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="821c5-139">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="821c5-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="821c5-140">Click **Create**.</span></span>

    ![Google + - zadejte oprávnění JavaScript zdroje a identifikátory URI pro přesměrování](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  <span data-ttu-id="821c5-142">Zkopírujte hodnoty hello **Id klienta** a **tajný klíč klienta**.</span><span class="sxs-lookup"><span data-stu-id="821c5-142">Copy hello values of **Client Id** and **Client secret**.</span></span> <span data-ttu-id="821c5-143">Musíte obou tooconfigure Google + jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="821c5-143">You need both tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="821c5-144">**Tajný klíč klienta** je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="821c5-144">**Client secret** is an important security credential.</span></span>

    ![Google + - kopírování hodnot hello tajný klíč klienta Id a klienta](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="821c5-146">Přidat hello Google + účet aplikace klíče tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="821c5-146">Add hello Google+ account application key tooAzure AD B2C</span></span>
<span data-ttu-id="821c5-147">Federace se Google + účty vyžaduje tajný klíč klienta pro účet Google + tootrust Azure AD B2C jménem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="821c5-147">Federation with Google+ accounts requires a client secret for Google+ account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="821c5-148">Je třeba toostore vaše Google + tajný klíč aplikace v Azure AD B2C klienta:</span><span class="sxs-lookup"><span data-stu-id="821c5-148">You need toostore your Google+ application secret in Azure AD B2C tenant:</span></span>  

1.  <span data-ttu-id="821c5-149">Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="821c5-149">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="821c5-150">Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="821c5-150">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="821c5-151">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="821c5-151">Click **+Add**.</span></span>
4.  <span data-ttu-id="821c5-152">Pro **možnosti**, použijte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="821c5-152">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="821c5-153">Pro **název**, použijte `GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="821c5-153">For **Name**, use `GoogleSecret`.</span></span>  
    <span data-ttu-id="821c5-154">Předpona Hello `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="821c5-154">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="821c5-155">V hello **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="821c5-155">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="821c5-156">Pro **použití klíče**, použijte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="821c5-156">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="821c5-157">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="821c5-157">Click **Create**</span></span>
9.  <span data-ttu-id="821c5-158">Potvrďte, že jste vytvořili klíč hello `B2C_1A_GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="821c5-158">Confirm that you've created hello key `B2C_1A_GoogleSecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="821c5-159">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="821c5-159">Add a claims provider in your extension policy</span></span>

<span data-ttu-id="821c5-160">Pokud chcete, uživatelé toosign pomocí účtu Google +, musíte toodefine Google + účet jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="821c5-160">If you want users toosign in by using Google+ account, you need toodefine Google+ account as a claims provider.</span></span> <span data-ttu-id="821c5-161">Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="821c5-161">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="821c5-162">koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="821c5-162">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="821c5-163">Zadejte účet Google + jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="821c5-163">Define Google+ Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="821c5-164">Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="821c5-164">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="821c5-165">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="821c5-165">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="821c5-166">Najde hello `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="821c5-166">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="821c5-167">Přidejte následující fragment kódu XML v části hello hello `ClaimsProviders` elementu a nahraďte `client_id` hodnotu s vaší Google + účet ID klienta aplikace před uložením souboru hello.</span><span class="sxs-lookup"><span data-stu-id="821c5-167">Add hello following XML snippet under hello `ClaimsProviders` element and replace `client_id` value with your Google+ account application client ID before saving hello file.</span></span>  

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
            <!--In case of authorization code used error, we don't want hello user tooselect his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="821c5-168">Zaregistrovat hello Google + účet deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="821c5-168">Register hello Google+ account claims provider tooSign up or Sign in user journey</span></span>

<span data-ttu-id="821c5-169">Zprostředkovatel identity Hello nastavit.</span><span class="sxs-lookup"><span data-stu-id="821c5-169">hello identity provider has been set up.</span></span>  <span data-ttu-id="821c5-170">Však není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky.</span><span class="sxs-lookup"><span data-stu-id="821c5-170">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="821c5-171">Přidat hello Google + identity zprostředkovatele tooyour uživatelský účet `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="821c5-171">Add hello Google+ account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="821c5-172">toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="821c5-172">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="821c5-173">Potom přidáme hello Google + zprostředkovatele identity účtu:</span><span class="sxs-lookup"><span data-stu-id="821c5-173">Then we add hello Google+ account identity provider:</span></span>

>[!NOTE]
>
><span data-ttu-id="821c5-174">Pokud jste zkopírovali hello `<UserJourneys>` element ze základního souboru zásad toohello příponu souboru (TrustFrameworkExtensions.xml), můžete toothis část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="821c5-174">If you copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml), you can skip toothis section.</span></span>

1.  <span data-ttu-id="821c5-175">Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="821c5-175">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="821c5-176">Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="821c5-176">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="821c5-177">Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="821c5-177">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="821c5-178">Pokud hello element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="821c5-178">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="821c5-179">Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="821c5-179">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="821c5-180">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="821c5-180">Display hello button</span></span>
<span data-ttu-id="821c5-181">Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="821c5-181">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="821c5-182">`<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="821c5-182">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="821c5-183">Pokud přidáte `<ClaimsProviderSelection>` element pro účet Google + nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello.</span><span class="sxs-lookup"><span data-stu-id="821c5-183">If you add a `<ClaimsProviderSelection>` element for Google+ account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="821c5-184">tooadd tento element:</span><span class="sxs-lookup"><span data-stu-id="821c5-184">tooadd this element:</span></span>

1.  <span data-ttu-id="821c5-185">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="821c5-185">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="821c5-186">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="821c5-186">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="821c5-187">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="821c5-187">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="821c5-188">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="821c5-188">Link hello button tooan action</span></span>
<span data-ttu-id="821c5-189">Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce.</span><span class="sxs-lookup"><span data-stu-id="821c5-189">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="821c5-190">pro Azure AD B2C toocommunicate s tooreceive účet Google + Hello akce v tomto případě je token.</span><span class="sxs-lookup"><span data-stu-id="821c5-190">hello action, in this case, is for Azure AD B2C toocommunicate with Google+ account tooreceive a token.</span></span>

1.  <span data-ttu-id="821c5-191">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="821c5-191">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="821c5-192">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="821c5-192">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * <span data-ttu-id="821c5-193">Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello</span><span class="sxs-lookup"><span data-stu-id="821c5-193">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
> * <span data-ttu-id="821c5-194">Ujistěte se, `TechnicalProfileReferenceId` ID je nastavit toohello technické profil vytvoříte starší (Google OAUTH).</span><span class="sxs-lookup"><span data-stu-id="821c5-194">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (Google-OAUTH).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="821c5-195">Nahrát hello zásad tooyour klienta</span><span class="sxs-lookup"><span data-stu-id="821c5-195">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="821c5-196">V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="821c5-196">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="821c5-197">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="821c5-197">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="821c5-198">Otevřete hello **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="821c5-198">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="821c5-199">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="821c5-199">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="821c5-200">Zkontrolujte **přepsat zásady hello, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="821c5-200">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="821c5-201">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže</span><span class="sxs-lookup"><span data-stu-id="821c5-201">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="821c5-202">Otestovat vlastní zásady hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="821c5-202">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="821c5-203">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="821c5-203">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>

    >[!NOTE]
    >
    >    <span data-ttu-id="821c5-204">**Spustit nyní** vyžaduje alespoň jednu aplikaci toobe preregistered na hello klienta.</span><span class="sxs-lookup"><span data-stu-id="821c5-204">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> 
    >    <span data-ttu-id="821c5-205">jak zjistit, tooregister aplikace toolearn hello Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článku nebo hello [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="821c5-205">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>


2.  <span data-ttu-id="821c5-206">Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="821c5-206">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="821c5-207">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="821c5-207">Select **Run now**.</span></span>
3.  <span data-ttu-id="821c5-208">Musí být schopný toosign pomocí Google + účet.</span><span class="sxs-lookup"><span data-stu-id="821c5-208">You should be able toosign in using Google+ account.</span></span>

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="821c5-209">[Nepovinné] Zaregistrovat hello Google + účet deklarace identity zprostředkovatele tooProfile upravit uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="821c5-209">[Optional] Register hello Google+ account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="821c5-210">Může být vhodné tooadd hello Google + účet zprostředkovatele identity také tooyour uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="821c5-210">You may want tooadd hello Google+ account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="821c5-211">toomake je k dispozici, jsme opakujte hello poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="821c5-211">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="821c5-212">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="821c5-212">Display hello button</span></span>
1.  <span data-ttu-id="821c5-213">Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="821c5-213">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="821c5-214">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="821c5-214">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="821c5-215">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="821c5-215">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="821c5-216">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="821c5-216">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="821c5-217">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="821c5-217">Link hello button tooan action</span></span>
1.  <span data-ttu-id="821c5-218">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="821c5-218">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="821c5-219">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="821c5-219">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="821c5-220">Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="821c5-220">Test hello custom Profile-Edit policy by using Run Now</span></span>

1.  <span data-ttu-id="821c5-221">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="821c5-221">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="821c5-222">Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="821c5-222">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="821c5-223">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="821c5-223">Select **Run now**.</span></span>
3.  <span data-ttu-id="821c5-224">Musí být schopný toosign pomocí Google + účet.</span><span class="sxs-lookup"><span data-stu-id="821c5-224">You should be able toosign in using Google+ account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="821c5-225">Stáhnout soubory dokončení zásad hello</span><span class="sxs-lookup"><span data-stu-id="821c5-225">Download hello complete policy files</span></span>
<span data-ttu-id="821c5-226">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="821c5-226">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="821c5-227">Ukázkové soubory zásad pro – referenční informace</span><span class="sxs-lookup"><span data-stu-id="821c5-227">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
