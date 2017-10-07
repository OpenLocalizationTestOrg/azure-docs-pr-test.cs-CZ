---
title: "Azure Active Directory B2C: Přidáte účet Microsoft (MSA) jako zprostředkovatele identity pomocí vlastních zásad"
description: "Ukázka použití Microsoft jako zprostředkovatele identity pomocí protokolu OpenID Connect (OIDC)"
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
ms.openlocfilehash: 577ac612f69015e6790f2fa9f2cfb42c2b524b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="f1e2a-103">Azure Active Directory B2C: Přidáte účet Microsoft (MSA) jako zprostředkovatele identity pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="f1e2a-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="f1e2a-104">Tento článek ukazuje, jak tooenable přihlášení pro uživatele z účtu Microsoft (MSA) prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-104">This article shows you how tooenable sign-in for users from Microsoft account (MSA) through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1e2a-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f1e2a-105">Prerequisites</span></span>
<span data-ttu-id="f1e2a-106">Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="f1e2a-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-107">These steps include:</span></span>

1.  <span data-ttu-id="f1e2a-108">Vytvoření aplikace pro účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="f1e2a-109">Přidání hello Microsoft účet aplikace klíče tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="f1e2a-109">Adding hello Microsoft account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="f1e2a-110">Přidání zásad tooa poskytovatele deklarací identity</span><span class="sxs-lookup"><span data-stu-id="f1e2a-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="f1e2a-111">Registrace hello Account Microsoft deklarace identity zprostředkovatele tooa uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="f1e2a-111">Registering hello Microsoft Account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="f1e2a-112">Odesílání hello zásad tooan Azure AD B2C klienta a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="f1e2a-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="f1e2a-113">Vytvoření aplikace účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="f1e2a-113">Create a Microsoft account application</span></span>
<span data-ttu-id="f1e2a-114">toouse účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate k aplikaci účtu Microsoft a přiřaďte mu hello správné parametry.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-114">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="f1e2a-115">Potřebujete účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-115">You need a Microsoft account.</span></span> <span data-ttu-id="f1e2a-116">Pokud nemáte, navštivte [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="f1e2a-117">Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-117">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="f1e2a-118">Klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-118">Click **Add an app**.</span></span>

    ![Microsoft účet – přidat aplikaci](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="f1e2a-120">Zadejte **název** pro vaši aplikaci **kontaktovat e-mailu**, zrušte zaškrtnutí políčka **dejte nám vám pomohli začít** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Účet Microsoft - registrace vaší aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="f1e2a-122">Zkopírujte hodnotu hello **Id aplikace**. Budete ho potřebovat tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-122">Copy hello value of **Application Id**. You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>

    ![Účet Microsoft - kopie hello hodnota ID aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="f1e2a-124">Klikněte na **přidat platformy**</span><span class="sxs-lookup"><span data-stu-id="f1e2a-124">Click on **Add platform**</span></span>

    ![Microsoft účet – přidejte platformu](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="f1e2a-126">Z hello seznam platforem, zvolte **webové**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-126">From hello platform list, choose **Web**.</span></span>

    ![Účet Microsoft - hello platformy seznamu zvolte Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="f1e2a-128">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování** pole.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="f1e2a-129">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Účet Microsoft - Set přesměrování adresy URL](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="f1e2a-131">Klikněte na **generovat nové heslo** pod hello **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-131">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="f1e2a-132">Zkopírujte hello nové heslo zobrazené na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-132">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="f1e2a-133">Budete ho potřebovat tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-133">You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="f1e2a-134">Toto heslo je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-134">This password is an important security credential.</span></span>

    ![Microsoft účet - vytvořit nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Účet Microsoft - kopie hello nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="f1e2a-137">Zaškrtávací políčko hello, která uvádí, že **Live SDK podporu** pod hello **pokročilé možnosti** části.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-137">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="f1e2a-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-138">Click **Save**.</span></span>

    ![Účet Microsoft - Live SDK podpory](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-hello-microsoft-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="f1e2a-140">Přidat hello Microsoft účet aplikace klíče tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="f1e2a-140">Add hello Microsoft account application key tooAzure AD B2C</span></span>
<span data-ttu-id="f1e2a-141">Federace s účty Microsoft vyžaduje tajný klíč klienta pro tootrust účet Microsoft Azure AD B2C jménem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-141">Federation with Microsoft accounts requires a client secret for Microsoft account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="f1e2a-142">Je třeba toostore vaše secert Microsoft účet aplikace v Azure AD B2C klienta:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-142">You need toostore your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="f1e2a-143">Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="f1e2a-143">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="f1e2a-144">Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-144">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="f1e2a-145">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="f1e2a-146">Pro **možnosti**, použijte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="f1e2a-147">Pro **název**, použijte `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="f1e2a-148">Předpona Hello `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-148">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="f1e2a-149">V hello **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="f1e2a-149">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="f1e2a-150">Pro **použití klíče**, použijte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="f1e2a-151">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="f1e2a-151">Click **Create**</span></span>
9.  <span data-ttu-id="f1e2a-152">Potvrďte, že jste vytvořili klíč hello `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-152">Confirm that you've created hello key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="f1e2a-153">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="f1e2a-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="f1e2a-154">Pokud chcete uživatelům toosign v Account Microsoft, je třeba toodefine Account Microsoft jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-154">If you want users toosign in by using Microsoft Account, you need toodefine Microsoft Account as a claims provider.</span></span> <span data-ttu-id="f1e2a-155">Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-155">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="f1e2a-156">koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-156">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="f1e2a-157">Zadejte Microsoft Account jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="f1e2a-158">Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-158">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="f1e2a-159">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="f1e2a-160">Najde hello `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="f1e2a-160">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="f1e2a-161">Přidejte následující fragment kódu XML v části hello `ClaimsProviders` element:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-161">Add following XML snippet under hello `ClaimsProviders` element:</span></span>

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
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

4.  <span data-ttu-id="f1e2a-162">Nahraďte `client_id` hodnota se váš klient aplikace Microsoft Account Id</span><span class="sxs-lookup"><span data-stu-id="f1e2a-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="f1e2a-163">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-163">Save hello file.</span></span>

## <a name="register-hello-microsoft-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="f1e2a-164">Zaregistrovat hello Account Microsoft deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="f1e2a-164">Register hello Microsoft Account claims provider tooSign up or Sign-in user journey</span></span>

<span data-ttu-id="f1e2a-165">V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-165">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="f1e2a-166">Nyní je třeba tooadd hello Account Microsoft identity zprostředkovatele tooyour uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-166">Now you need tooadd hello Microsoft Account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="f1e2a-167">toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-167">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="f1e2a-168">Potom přidáme zprostředkovatele identity Account Microsoft hello:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-168">Then we add hello Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="f1e2a-169">Pokud jste dříve zkopírovali hello `<UserJourneys>` element ze základního souboru souboru rozšíření zásad toohello `TrustFrameworkExtensions.xml`, můžete toothis část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-169">If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file `TrustFrameworkExtensions.xml`, you can skip toothis section.</span></span>

1.  <span data-ttu-id="f1e2a-170">Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-170">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="f1e2a-171">Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-171">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="f1e2a-172">Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-172">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="f1e2a-173">Pokud hello element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-173">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="f1e2a-174">Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-174">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="f1e2a-175">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1e2a-175">Display hello button</span></span>
<span data-ttu-id="f1e2a-176">Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-176">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="f1e2a-177">`<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-177">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="f1e2a-178">Pokud přidáte `<ClaimsProviderSelection>` element pro účet Microsoft, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="f1e2a-179">tooadd tento element:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-179">tooadd this element:</span></span>

1.  <span data-ttu-id="f1e2a-180">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-180">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="f1e2a-181">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="f1e2a-181">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="f1e2a-182">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="f1e2a-183">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="f1e2a-183">Link hello button tooan action</span></span>
<span data-ttu-id="f1e2a-184">Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-184">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="f1e2a-185">pro Azure AD B2C toocommunicate s tooreceive Account Microsoft Hello akce v tomto případě je token.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-185">hello action, in this case, is for Azure AD B2C toocommunicate with Microsoft Account tooreceive a token.</span></span> <span data-ttu-id="f1e2a-186">Odkaz akce tooan tlačítka hello pomocí propojení hello technické profil pro poskytovatele deklarací identity Account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-186">Link hello button tooan action by linking hello technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="f1e2a-187">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-187">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="f1e2a-188">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="f1e2a-189">Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello</span><span class="sxs-lookup"><span data-stu-id="f1e2a-189">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
>   * <span data-ttu-id="f1e2a-190">Ujistěte se, `TechnicalProfileReferenceId` ID je nastavit toohello technické profil vytvoříte starší (MSA OIDC).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-190">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="f1e2a-191">Nahrát hello zásad tooyour klienta</span><span class="sxs-lookup"><span data-stu-id="f1e2a-191">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="f1e2a-192">V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-192">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="f1e2a-193">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="f1e2a-194">Otevřete hello **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-194">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="f1e2a-195">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="f1e2a-196">Zkontrolujte **přepsat zásady hello, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-196">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="f1e2a-197">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže</span><span class="sxs-lookup"><span data-stu-id="f1e2a-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="f1e2a-198">Otestovat vlastní zásady hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="f1e2a-198">Test hello custom policy by using Run Now</span></span>

1.  <span data-ttu-id="f1e2a-199">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-199">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="f1e2a-200">**Spustit nyní** vyžaduje alespoň jednu aplikaci toobe preregistered na hello klienta.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-200">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> <span data-ttu-id="f1e2a-201">jak zjistit, tooregister aplikace toolearn hello Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článku nebo hello [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-201">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="f1e2a-202">Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-202">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="f1e2a-203">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="f1e2a-204">Musí být schopný toosign pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-204">You should be able toosign in using Microsoft account.</span></span>

## <a name="optional-register-hello-microsoft-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="f1e2a-205">[Nepovinné] Zaregistrovat hello Account Microsoft deklarace identity zprostředkovatele tooProfile upravit uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="f1e2a-205">[Optional] Register hello Microsoft Account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="f1e2a-206">Může být vhodné zprostředkovatele identity Account Microsoft hello tooadd také tooyour uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-206">You may want tooadd hello Microsoft Account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="f1e2a-207">toomake je k dispozici, jsme opakujte hello poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-207">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="f1e2a-208">Tlačítko hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1e2a-208">Display hello button</span></span>
1.  <span data-ttu-id="f1e2a-209">Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="f1e2a-209">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="f1e2a-210">Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-210">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="f1e2a-211">Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="f1e2a-211">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="f1e2a-212">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="f1e2a-213">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="f1e2a-213">Link hello button tooan action</span></span>
1.  <span data-ttu-id="f1e2a-214">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-214">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="f1e2a-215">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="f1e2a-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="f1e2a-216">Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="f1e2a-216">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="f1e2a-217">Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-217">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="f1e2a-218">Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-218">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="f1e2a-219">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="f1e2a-220">Musí být schopný toosign pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-220">You should be able toosign in using Microsoft account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="f1e2a-221">Stáhnout soubory dokončení zásad hello</span><span class="sxs-lookup"><span data-stu-id="f1e2a-221">Download hello complete policy files</span></span>
<span data-ttu-id="f1e2a-222">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="f1e2a-222">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="f1e2a-223">Ukázkové soubory zásad pro – referenční informace</span><span class="sxs-lookup"><span data-stu-id="f1e2a-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
