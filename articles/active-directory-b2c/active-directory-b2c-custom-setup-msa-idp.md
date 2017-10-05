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
ms.openlocfilehash: 8c981046ff41d3927ff60d6dc4f40366ae25ba74
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="25ee3-103">Azure Active Directory B2C: Přidáte účet Microsoft (MSA) jako zprostředkovatele identity pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="25ee3-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="25ee3-104">Tento článek ukazuje, jak povolit přihlášení pro uživatele z účtu Microsoft (MSA) prostřednictvím [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="25ee3-104">This article shows you how to enable sign-in for users from Microsoft account (MSA) through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25ee3-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="25ee3-105">Prerequisites</span></span>
<span data-ttu-id="25ee3-106">Proveďte kroky v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="25ee3-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="25ee3-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="25ee3-107">These steps include:</span></span>

1.  <span data-ttu-id="25ee3-108">Vytvoření aplikace pro účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25ee3-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="25ee3-109">Přidání klíče aplikace účtu Microsoft k Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="25ee3-109">Adding the Microsoft account application key to Azure AD B2C</span></span>
3.  <span data-ttu-id="25ee3-110">Přidání poskytovatele deklarací identity k zásadě</span><span class="sxs-lookup"><span data-stu-id="25ee3-110">Adding claims provider to a policy</span></span>
4.  <span data-ttu-id="25ee3-111">Registrace zprostředkovatele deklarací identity Account Microsoft k cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="25ee3-111">Registering the Microsoft Account claims provider to a user journey</span></span>
5.  <span data-ttu-id="25ee3-112">Odeslání zásady do Azure AD B2C klienta a otestovat ji</span><span class="sxs-lookup"><span data-stu-id="25ee3-112">Uploading the policy to an Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="25ee3-113">Vytvoření aplikace účtu Microsoft</span><span class="sxs-lookup"><span data-stu-id="25ee3-113">Create a Microsoft account application</span></span>
<span data-ttu-id="25ee3-114">Pokud chcete používat účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, musíte vytvořit aplikaci účtu Microsoft a zadejte se správné parametry.</span><span class="sxs-lookup"><span data-stu-id="25ee3-114">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="25ee3-115">Potřebujete účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25ee3-115">You need a Microsoft account.</span></span> <span data-ttu-id="25ee3-116">Pokud nemáte, navštivte [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="25ee3-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="25ee3-117">Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25ee3-117">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="25ee3-118">Klikněte na tlačítko **přidat aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-118">Click **Add an app**.</span></span>

    ![Microsoft účet – přidat aplikaci](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="25ee3-120">Zadejte **název** pro vaši aplikaci **kontaktovat e-mailu**, zrušte zaškrtnutí políčka **dejte nám vám pomohli začít** a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Účet Microsoft - registrace vaší aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="25ee3-122">Zkopírujte hodnotu **Id aplikace**. Je třeba ji nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="25ee3-122">Copy the value of **Application Id**. You need it to configure Microsoft account as an identity provider in your tenant.</span></span>

    ![Účet Microsoft - kopírování hodnoty ID aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="25ee3-124">Klikněte na **přidat platformy**</span><span class="sxs-lookup"><span data-stu-id="25ee3-124">Click on **Add platform**</span></span>

    ![Microsoft účet – přidejte platformu](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="25ee3-126">Vyberte ze seznamu platformy **webové**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-126">From the platform list, choose **Web**.</span></span>

    ![Účet Microsoft - ze seznamu platformy vyberte Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="25ee3-128">Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v **identifikátory URI přesměrování** pole.</span><span class="sxs-lookup"><span data-stu-id="25ee3-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="25ee3-129">Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="25ee3-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Účet Microsoft - Set přesměrování adresy URL](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="25ee3-131">Klikněte na **generovat nové heslo** pod **tajné klíče aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="25ee3-131">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="25ee3-132">Zkopírujte nové heslo, které jsou zobrazené na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="25ee3-132">Copy the new password displayed on screen.</span></span> <span data-ttu-id="25ee3-133">Je třeba ji nakonfigurovat účet Microsoft jako zprostředkovatel identity ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="25ee3-133">You need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="25ee3-134">Toto heslo je důležitým bezpečnostním pověřením.</span><span class="sxs-lookup"><span data-stu-id="25ee3-134">This password is an important security credential.</span></span>

    ![Microsoft účet - vytvořit nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Účet Microsoft - kopie nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="25ee3-137">Zaškrtněte políčko, která uvádí, že **Live SDK podporu** pod **pokročilé možnosti** části.</span><span class="sxs-lookup"><span data-stu-id="25ee3-137">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="25ee3-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-138">Click **Save**.</span></span>

    ![Účet Microsoft - Live SDK podpory](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="25ee3-140">Přidejte klíč aplikace účtu Microsoft k Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="25ee3-140">Add the Microsoft account application key to Azure AD B2C</span></span>
<span data-ttu-id="25ee3-141">Federace s účty Microsoft vyžaduje tajný klíč klienta pro účet Microsoft do vztahu důvěryhodnosti Azure AD B2C jménem aplikace.</span><span class="sxs-lookup"><span data-stu-id="25ee3-141">Federation with Microsoft accounts requires a client secret for Microsoft account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="25ee3-142">Je třeba uložit vaše secert aplikace účtu Microsoft v klientovi Azure AD B2C:</span><span class="sxs-lookup"><span data-stu-id="25ee3-142">You need to store your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="25ee3-143">Přejděte ke klientovi Azure AD B2C a vyberte **nastavení B2C** > **Identity rozhraní Framework**</span><span class="sxs-lookup"><span data-stu-id="25ee3-143">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="25ee3-144">Vyberte **zásad klíče** zobrazíte klíče, které jsou k dispozici ve vašem klientovi.</span><span class="sxs-lookup"><span data-stu-id="25ee3-144">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="25ee3-145">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="25ee3-146">Pro **možnosti**, použijte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="25ee3-147">Pro **název**, použijte `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="25ee3-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="25ee3-148">Předpona `B2C_1A_` může automaticky přidat.</span><span class="sxs-lookup"><span data-stu-id="25ee3-148">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="25ee3-149">V **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="25ee3-149">In the **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="25ee3-150">Pro **použití klíče**, použijte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="25ee3-151">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="25ee3-151">Click **Create**</span></span>
9.  <span data-ttu-id="25ee3-152">Potvrďte, že jste vytvořili klíč `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="25ee3-152">Confirm that you've created the key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="25ee3-153">Přidání poskytovatele deklarací identity v rozšíření zásady</span><span class="sxs-lookup"><span data-stu-id="25ee3-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="25ee3-154">Pokud chcete uživatelům se přihlásit pomocí Account Microsoft, budete muset definovat Account Microsoft jako poskytovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="25ee3-154">If you want users to sign in by using Microsoft Account, you need to define Microsoft Account as a claims provider.</span></span> <span data-ttu-id="25ee3-155">Jinými slovy budete muset zadat koncový bod, který komunikuje se službou Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="25ee3-155">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="25ee3-156">Koncový bod poskytuje sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="25ee3-156">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="25ee3-157">Zadejte Microsoft Account jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="25ee3-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="25ee3-158">Otevřete soubor rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="25ee3-158">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="25ee3-159">Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="25ee3-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="25ee3-160">Najít `<ClaimsProviders>` části</span><span class="sxs-lookup"><span data-stu-id="25ee3-160">Find the `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="25ee3-161">Přidejte následující fragment kódu XML v části `ClaimsProviders` element:</span><span class="sxs-lookup"><span data-stu-id="25ee3-161">Add following XML snippet under the `ClaimsProviders` element:</span></span>

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

4.  <span data-ttu-id="25ee3-162">Nahraďte `client_id` hodnota se váš klient aplikace Microsoft Account Id</span><span class="sxs-lookup"><span data-stu-id="25ee3-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="25ee3-163">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="25ee3-163">Save the file.</span></span>

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="25ee3-164">Registrace Microsoft Account deklarace zprostředkovatele podepsání nahoru nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="25ee3-164">Register the Microsoft Account claims provider to Sign up or Sign-in user journey</span></span>

<span data-ttu-id="25ee3-165">V tomto okamžiku byla nastavena zprostředkovatele identity, ale není k dispozici v žádném z obrazovky registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="25ee3-165">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="25ee3-166">Nyní je nutné přidat zprostředkovatele identity Account Microsoft pro vaše uživatele `SignUpOrSignIn` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="25ee3-166">Now you need to add the Microsoft Account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="25ee3-167">Chcete-li k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="25ee3-167">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="25ee3-168">Potom přidáme zprostředkovatele identity Account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="25ee3-168">Then we add the Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="25ee3-169">Pokud jste dříve zkopírovali `<UserJourneys>` element ze základního souboru zásad k souboru rozšíření `TrustFrameworkExtensions.xml`, můžete přeskočit k této části.</span><span class="sxs-lookup"><span data-stu-id="25ee3-169">If you previously copied the `<UserJourneys>` element from base file of your policy to the extension file `TrustFrameworkExtensions.xml`, you can skip to this section.</span></span>

1.  <span data-ttu-id="25ee3-170">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="25ee3-170">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="25ee3-171">Najít `<UserJourneys>` elementu a zkopírujte celý obsah `<UserJourneys>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="25ee3-171">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="25ee3-172">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a najděte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="25ee3-172">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="25ee3-173">Pokud element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="25ee3-173">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="25ee3-174">Vložte celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="25ee3-174">Paste the entire content of `<UserJournesy>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="25ee3-175">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="25ee3-175">Display the button</span></span>
<span data-ttu-id="25ee3-176">`<ClaimsProviderSelections>` Element definuje seznam možnosti výběru poskytovatele deklarací identity a jejich pořadí.</span><span class="sxs-lookup"><span data-stu-id="25ee3-176">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="25ee3-177">`<ClaimsProviderSelection>`Element je obdobou tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="25ee3-177">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="25ee3-178">Pokud přidáte `<ClaimsProviderSelection>` element pro účet Microsoft, nové tlačítko se zobrazí při pojmenováváme uživatele na stránce.</span><span class="sxs-lookup"><span data-stu-id="25ee3-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="25ee3-179">Chcete-li přidat tento element:</span><span class="sxs-lookup"><span data-stu-id="25ee3-179">To add this element:</span></span>

1.  <span data-ttu-id="25ee3-180">Najít `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="25ee3-180">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="25ee3-181">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="25ee3-181">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="25ee3-182">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="25ee3-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="25ee3-183">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="25ee3-183">Link the button to an action</span></span>
<span data-ttu-id="25ee3-184">Nyní když máte tlačítka na místě, budete muset propojit akce.</span><span class="sxs-lookup"><span data-stu-id="25ee3-184">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="25ee3-185">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s Account Microsoft přijmout token.</span><span class="sxs-lookup"><span data-stu-id="25ee3-185">The action, in this case, is for Azure AD B2C to communicate with Microsoft Account to receive a token.</span></span> <span data-ttu-id="25ee3-186">Pomocí propojení technické profil pro poskytovatele deklarací identity Microsoft Account odkazu na tlačítko akce:</span><span class="sxs-lookup"><span data-stu-id="25ee3-186">Link the button to an action by linking the technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="25ee3-187">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="25ee3-187">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="25ee3-188">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="25ee3-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="25ee3-189">Ujistěte se, `Id` mají stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části</span><span class="sxs-lookup"><span data-stu-id="25ee3-189">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section</span></span>
>   * <span data-ttu-id="25ee3-190">Ujistěte se, `TechnicalProfileReferenceId` ID je nastaveno na technické profil vytvoříte starší (MSA OIDC).</span><span class="sxs-lookup"><span data-stu-id="25ee3-190">Ensure `TechnicalProfileReferenceId` ID is set to the technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="25ee3-191">Nahrát zásady klienta</span><span class="sxs-lookup"><span data-stu-id="25ee3-191">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="25ee3-192">V [portál Azure](https://portal.azure.com), přepněte do [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete **Azure AD B2C** okno.</span><span class="sxs-lookup"><span data-stu-id="25ee3-192">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="25ee3-193">Vyberte **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="25ee3-194">Otevřete **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="25ee3-194">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="25ee3-195">Vyberte **nahrát zásady**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="25ee3-196">Zkontrolujte **přepsat zásady, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="25ee3-196">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="25ee3-197">**Nahrát** TrustFrameworkExtensions.xml a ujistěte se, že neselže ověření</span><span class="sxs-lookup"><span data-stu-id="25ee3-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="25ee3-198">Otestovat vlastní zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="25ee3-198">Test the custom policy by using Run Now</span></span>

1.  <span data-ttu-id="25ee3-199">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-199">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="25ee3-200">**Spustit nyní** vyžaduje alespoň jedné aplikace do být preregistered u klienta.</span><span class="sxs-lookup"><span data-stu-id="25ee3-200">**Run now** requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="25ee3-201">Další postup registrace aplikace najdete v tématu Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článek nebo [registrace aplikace](active-directory-b2c-app-registration.md) článku.</span><span class="sxs-lookup"><span data-stu-id="25ee3-201">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="25ee3-202">Otevřete **B2C_1A_signup_signin**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="25ee3-202">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="25ee3-203">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="25ee3-204">Byste měli být schopní se přihlásit pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25ee3-204">You should be able to sign in using Microsoft account.</span></span>

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="25ee3-205">[Nepovinné] Zaregistrujte poskytovatele deklarací identity Account Microsoft pro úpravy profilu uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="25ee3-205">[Optional] Register the Microsoft Account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="25ee3-206">Můžete také přidat zprostředkovatele identity Account Microsoft pro vaše uživatele `ProfileEdit` cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="25ee3-206">You may want to add the Microsoft Account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="25ee3-207">Chcete-li k dispozici, jsme zopakujte poslední dva kroky:</span><span class="sxs-lookup"><span data-stu-id="25ee3-207">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="25ee3-208">Zobrazení tlačítka</span><span class="sxs-lookup"><span data-stu-id="25ee3-208">Display the button</span></span>
1.  <span data-ttu-id="25ee3-209">Otevřete soubor rozšíření zásad (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="25ee3-209">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="25ee3-210">Najít `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty uživatele, který jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="25ee3-210">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="25ee3-211">Vyhledejte `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="25ee3-211">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="25ee3-212">Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="25ee3-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="25ee3-213">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="25ee3-213">Link the button to an action</span></span>
1.  <span data-ttu-id="25ee3-214">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="25ee3-214">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="25ee3-215">Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="25ee3-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="25ee3-216">Otestovat vlastní úpravy profilu zásady pomocí spustit nyní</span><span class="sxs-lookup"><span data-stu-id="25ee3-216">Test the custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="25ee3-217">Otevřete **nastavení Azure AD B2C** a přejděte na **Identity rozhraní Framework**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-217">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="25ee3-218">Otevřete **B2C_1A_ProfileEdit**, předávající stranu vlastních zásad, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="25ee3-218">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="25ee3-219">Vyberte **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="25ee3-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="25ee3-220">Byste měli být schopní se přihlásit pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25ee3-220">You should be able to sign in using Microsoft account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="25ee3-221">Stáhnout soubory dokončení zásad</span><span class="sxs-lookup"><span data-stu-id="25ee3-221">Download the complete policy files</span></span>
<span data-ttu-id="25ee3-222">Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní vlastní zásady, které soubory po dokončení Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.</span><span class="sxs-lookup"><span data-stu-id="25ee3-222">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="25ee3-223">Ukázkové soubory zásad pro – referenční informace</span><span class="sxs-lookup"><span data-stu-id="25ee3-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
