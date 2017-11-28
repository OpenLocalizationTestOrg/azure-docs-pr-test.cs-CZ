---
title: "Azure Active Directory B2C: Přidejte zprostředkovatele služby Azure AD pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o vlastních zásad Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 9b0c32086cebc171d91da2e7bfb48136723ccd4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="01fee-103">Azure Active Directory B2C: Přihlaste se pomocí účtů služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="01fee-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="01fee-104">Tento článek ukazuje, jak tooenable přihlášení pro uživatele z konkrétní organizace Azure Active Directory (Azure AD) prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="01fee-104">This article shows you how tooenable sign-in for users from a specific Azure Active Directory (Azure AD) organization through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01fee-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="01fee-105">Prerequisites</span></span>

<span data-ttu-id="01fee-106">Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="01fee-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="01fee-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="01fee-107">These steps include:</span></span>

1. <span data-ttu-id="01fee-108">Vytvoření Azure Active Directory B2C klienta (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="01fee-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="01fee-109">Vytvoření aplikace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="01fee-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="01fee-110">Probíhá registrace dvě aplikace modul zásad.</span><span class="sxs-lookup"><span data-stu-id="01fee-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="01fee-111">Nastavení klíče.</span><span class="sxs-lookup"><span data-stu-id="01fee-111">Setting up keys.</span></span>
5. <span data-ttu-id="01fee-112">Nastavení úvodní pack hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-112">Setting up hello starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="01fee-113">Vytvoření aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="01fee-113">Create an Azure AD app</span></span>

<span data-ttu-id="01fee-114">přihlášení tooenable pro uživatele z konkrétní organizace Azure AD, je nutné tooregister aplikaci v rámci klienta hello organizace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01fee-114">tooenable sign-in for users from a specific Azure AD organization, you need tooregister an application within hello organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="01fee-115">Používáme "contoso.com" pro klienta hello organizace Azure AD a "fabrikamb2c.onmicrosoft.com" jako klienta hello Azure AD B2C v hello pokynů.</span><span class="sxs-lookup"><span data-stu-id="01fee-115">We use "contoso.com" for hello organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as hello Azure AD B2C tenant in hello following instructions.</span></span>

1. <span data-ttu-id="01fee-116">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="01fee-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="01fee-117">Na horním panelu hello vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="01fee-117">On hello top bar, select your account.</span></span> <span data-ttu-id="01fee-118">Z hello **Directory** vyberte místo, kam chcete tooregister klienta hello organizace Azure AD vaší aplikace (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="01fee-118">From hello **Directory** list, choose hello organizational Azure AD tenant where you want tooregister your application (contoso.com).</span></span>
1. <span data-ttu-id="01fee-119">Vyberte **další služby** v levém podokně hello a vyhledejte "Aplikace registrace."</span><span class="sxs-lookup"><span data-stu-id="01fee-119">Select **More services** in hello left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="01fee-120">Vyberte **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="01fee-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="01fee-121">Zadejte název pro vaši aplikaci (například `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="01fee-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="01fee-122">Vyberte **webovou aplikaci nebo API** pro typ aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-122">Select **Web app / API** for hello application type.</span></span>
1. <span data-ttu-id="01fee-123">Pro **přihlašovací adresa URL**, zadejte hello následující adresu URL, kde `yourtenant` je nahrazena hello název klienta služby Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="01fee-123">For **Sign-on URL**, enter hello following URL, where `yourtenant` is replaced by hello name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="01fee-124">Uložit ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="01fee-124">Save hello application ID.</span></span>
1. <span data-ttu-id="01fee-125">Vyberte aplikaci hello nově vytvořený.</span><span class="sxs-lookup"><span data-stu-id="01fee-125">Select hello newly created application.</span></span>
1. <span data-ttu-id="01fee-126">V části hello **nastavení** vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="01fee-126">Under hello **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="01fee-127">Vytvořte nový klíč a uložte jej.</span><span class="sxs-lookup"><span data-stu-id="01fee-127">Create a new key, and save it.</span></span> <span data-ttu-id="01fee-128">Budete ho používat v hello kroků v další části hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-128">You will use it in hello steps in hello next section.</span></span>

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a><span data-ttu-id="01fee-129">Přidání klíče tooAzure hello Azure AD AD B2C</span><span class="sxs-lookup"><span data-stu-id="01fee-129">Add hello Azure AD key tooAzure AD B2C</span></span>

<span data-ttu-id="01fee-130">Je nutné klíč toostore hello contoso.com aplikace v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="01fee-130">You need toostore hello contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="01fee-131">toodo toto:</span><span class="sxs-lookup"><span data-stu-id="01fee-131">toodo this:</span></span>
1. <span data-ttu-id="01fee-132">Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework** > **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="01fee-132">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="01fee-133">Vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="01fee-133">Select **+Add**.</span></span>
1. <span data-ttu-id="01fee-134">Vyberte nebo zadejte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="01fee-134">Select or enter these options:</span></span>
   * <span data-ttu-id="01fee-135">Vyberte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="01fee-135">Select **Manual**.</span></span>
   * <span data-ttu-id="01fee-136">Pro **název**, zvolte název, který odpovídá název vašeho klienta Azure AD (například `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="01fee-136">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="01fee-137">Předpona Hello `B2C_1A_` se automaticky přidá toohello název vašeho klíče.</span><span class="sxs-lookup"><span data-stu-id="01fee-137">hello prefix `B2C_1A_` is added automatically toohello name of your key.</span></span>
   * <span data-ttu-id="01fee-138">Vložte klíč vaší aplikace hello **tajný klíč** pole.</span><span class="sxs-lookup"><span data-stu-id="01fee-138">Paste your application key in hello **Secret** box.</span></span>
   * <span data-ttu-id="01fee-139">Vyberte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="01fee-139">Select **Signature**.</span></span>
1. <span data-ttu-id="01fee-140">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01fee-140">Select **Create**.</span></span>
1. <span data-ttu-id="01fee-141">Potvrďte, že jste vytvořili klíč hello `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="01fee-141">Confirm that you've created hello key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="01fee-142">Přidání poskytovatele deklarací identity v základní zásady</span><span class="sxs-lookup"><span data-stu-id="01fee-142">Add a claims provider in your base policy</span></span>

<span data-ttu-id="01fee-143">Pokud chcete uživatelům toosign pomocí Azure AD, je nutné toodefine Azure AD jako zprostředkovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="01fee-143">If you want users toosign in by using Azure AD, you need toodefine Azure AD as a claims provider.</span></span> <span data-ttu-id="01fee-144">Jinými slovy budete potřebovat toospecify koncový bod, který Azure AD B2C bude komunikovat se.</span><span class="sxs-lookup"><span data-stu-id="01fee-144">In other words, you need toospecify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="01fee-145">koncový bod Hello poskytne sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="01fee-145">hello endpoint will provide a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span> 

<span data-ttu-id="01fee-146">Azure AD jako zprostředkovatele deklarací identity můžete definovat přidáním Azure AD toohello `<ClaimsProvider>` uzel v souboru rozšíření bylo hello zásad:</span><span class="sxs-lookup"><span data-stu-id="01fee-146">You can define Azure AD as a claims provider by adding Azure AD toohello `<ClaimsProvider>` node in hello extension file of your policy:</span></span>

1. <span data-ttu-id="01fee-147">Otevřete soubor rozšíření hello (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="01fee-147">Open hello extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="01fee-148">Najde hello `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="01fee-148">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="01fee-149">Pokud neexistuje, přidejte ji pod hello kořenový uzel.</span><span class="sxs-lookup"><span data-stu-id="01fee-149">If it does not exist, add it under hello root node.</span></span>
1. <span data-ttu-id="01fee-150">Přidejte nový `<ClaimsProvider>` uzlu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="01fee-150">Add a new `<ClaimsProvider>` node as follows:</span></span>

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
                </OutputClaims>
                <OutputClaimsTransformations>
                    <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
                    <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
                    <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
                    <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
                </OutputClaimsTransformations>
                <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
            </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. <span data-ttu-id="01fee-151">V části hello `<ClaimsProvider>` uzel, hodnota hello aktualizace `<Domain>` tooa jedinečnou hodnotu, kterou lze použít toodistinguish ho od jiných poskytovatelů identit.</span><span class="sxs-lookup"><span data-stu-id="01fee-151">Under hello `<ClaimsProvider>` node, update hello value for `<Domain>` tooa unique value that can be used toodistinguish it from other identity providers.</span></span>
1. <span data-ttu-id="01fee-152">V části hello `<ClaimsProvider>` uzel, hodnota hello aktualizace `<DisplayName>` tooa popisný název hello deklarací zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="01fee-152">Under hello `<ClaimsProvider>` node, update hello value for `<DisplayName>` tooa friendly name for hello claims provider.</span></span> <span data-ttu-id="01fee-153">Tato hodnota není právě používána.</span><span class="sxs-lookup"><span data-stu-id="01fee-153">This value is not currently used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="01fee-154">Aktualizovat profil technické hello</span><span class="sxs-lookup"><span data-stu-id="01fee-154">Update hello technical profile</span></span>

<span data-ttu-id="01fee-155">tooget tokenu z koncového bodu hello Azure AD, je nutné toodefine hello protokoly, že Azure AD B2C mají používat toocommunicate s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01fee-155">tooget a token from hello Azure AD endpoint, you need toodefine hello protocols that Azure AD B2C should use toocommunicate with Azure AD.</span></span> <span data-ttu-id="01fee-156">To se provádí uvnitř hello `<TechnicalProfile>` element `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="01fee-156">This is done inside hello `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="01fee-157">Aktualizovat hello ID hello `<TechnicalProfile>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="01fee-157">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="01fee-158">Toto ID je použité toorefer toothis technické profil z různých částí hello zásad.</span><span class="sxs-lookup"><span data-stu-id="01fee-158">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
1. <span data-ttu-id="01fee-159">Aktualizujte hello hodnotu pro `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="01fee-159">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="01fee-160">Tato hodnota se zobrazí na hello přihlašovací tlačítko na obrazovce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="01fee-160">This value will be displayed on hello sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="01fee-161">Aktualizujte hello hodnotu pro `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="01fee-161">Update hello value for `<Description>`.</span></span>
1. <span data-ttu-id="01fee-162">Azure AD používá protokol OpenID Connect hello, zajistěte proto tuto hodnotu hello pro `<Protocol>` je `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="01fee-162">Azure AD uses hello OpenID Connect protocol, so ensure that hello value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="01fee-163">Je třeba tooupdate hello `<Metadata>` oddíl v souboru XML hello označuje tooearlier tooreflect hello konfigurační nastavení pro konkrétní klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01fee-163">You need tooupdate hello `<Metadata>` section in hello XML file referred tooearlier tooreflect hello configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="01fee-164">V souboru XML hello aktualizujte hodnoty metadata hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="01fee-164">In hello XML file, update hello metadata values as follows:</span></span>

1. <span data-ttu-id="01fee-165">Nastavit `<Item Key="METADATA">` příliš`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, kde `yourAzureADtenant` je název klienta Azure AD (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="01fee-165">Set `<Item Key="METADATA">` too`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="01fee-166">Otevřete váš prohlížeč a přejděte toohello `METADATA` adresu URL, kterou jste aktualizovali.</span><span class="sxs-lookup"><span data-stu-id="01fee-166">Open your browser and go toohello `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="01fee-167">V prohlížeči hello vyhledejte objekt "vystavitele" hello a zkopírujte jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="01fee-167">In hello browser, look for hello 'issuer' object and copy its value.</span></span> <span data-ttu-id="01fee-168">By měl vypadat jako následující hello: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="01fee-168">It should look like hello following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="01fee-169">Vložte hodnotu hello `<Item Key="ProviderName">` v souboru XML hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-169">Paste hello value for `<Item Key="ProviderName">` in hello XML file.</span></span>
1. <span data-ttu-id="01fee-170">Nastavit `<Item Key="client_id">` toohello ID aplikace z registrace aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-170">Set `<Item Key="client_id">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="01fee-171">Nastavit `<Item Key="IdTokenAudience">` toohello ID aplikace z registrace aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-171">Set `<Item Key="IdTokenAudience">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="01fee-172">Ujistěte se, že `<Item Key="response_types">` je nastaven příliš`id_token`.</span><span class="sxs-lookup"><span data-stu-id="01fee-172">Ensure that `<Item Key="response_types">` is set too`id_token`.</span></span>
1. <span data-ttu-id="01fee-173">Ujistěte se, že `<Item Key="UsePolicyInRedirectUri">` je nastaven příliš`false`.</span><span class="sxs-lookup"><span data-stu-id="01fee-173">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set too`false`.</span></span>

<span data-ttu-id="01fee-174">Musíte taky tajný klíč toolink hello Azure AD, který je zaregistrovaný v vaší toohello klienta Azure AD B2C Azure AD `<ClaimsProvider>` informace:</span><span class="sxs-lookup"><span data-stu-id="01fee-174">You also need toolink hello Azure AD secret that you registered in your Azure AD B2C tenant toohello Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="01fee-175">V hello `<CryptographicKeys>` kapitoly hello předcházející soubor XML, aktualizujte hello hodnotu pro `StorageReferenceId` toohello referenční ID hello tajný klíč, který jste definovali (například `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="01fee-175">In hello `<CryptographicKeys>` section in hello preceding XML file, update hello value for `StorageReferenceId` toohello reference ID of hello secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="01fee-176">Nahrát soubor hello rozšíření pro ověření</span><span class="sxs-lookup"><span data-stu-id="01fee-176">Upload hello extension file for verification</span></span>

<span data-ttu-id="01fee-177">Nyní by jste nakonfigurovali zásady tak, aby Azure AD B2C zná jak toocommunicate s adresářem služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="01fee-177">By now, you have configured your policy so that Azure AD B2C knows how toocommunicate with your Azure AD directory.</span></span> <span data-ttu-id="01fee-178">Zkuste odeslat soubor rozšíření hello jenom tooconfirm vaše zásady, nemá žádné problémy, pokud.</span><span class="sxs-lookup"><span data-stu-id="01fee-178">Try uploading hello extension file of your policy just tooconfirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="01fee-179">toodo tak:</span><span class="sxs-lookup"><span data-stu-id="01fee-179">toodo so:</span></span>

1. <span data-ttu-id="01fee-180">Otevřete hello **všechny zásady** okno v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="01fee-180">Open hello **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="01fee-181">Zkontrolujte hello **přepsat zásady hello, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="01fee-181">Check hello **Overwrite hello policy if it exists** box.</span></span>
1. <span data-ttu-id="01fee-182">Nahrání souboru rozšíření bylo hello (TrustFrameworkExtensions.xml) a ujistěte se, zda text hello ověření neselže.</span><span class="sxs-lookup"><span data-stu-id="01fee-182">Upload hello extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail hello validation.</span></span>

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a><span data-ttu-id="01fee-183">Zaregistrovat hello Azure AD deklarace identity zprostředkovatele tooa uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="01fee-183">Register hello Azure AD claims provider tooa user journey</span></span>

<span data-ttu-id="01fee-184">Teď musíte tooadd hello Azure AD identity zprostředkovatele tooone z vaší uživatelské cesty.</span><span class="sxs-lookup"><span data-stu-id="01fee-184">You now need tooadd hello Azure AD identity provider tooone of your user journeys.</span></span> <span data-ttu-id="01fee-185">V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky.</span><span class="sxs-lookup"><span data-stu-id="01fee-185">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="01fee-186">toomake k dispozici, jsme vytvoří duplicitní existující uživatele cesty šablony a upravit ho tak, aby je také zprostředkovatele identity hello Azure AD:</span><span class="sxs-lookup"><span data-stu-id="01fee-186">toomake it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has hello Azure AD identity provider:</span></span>

1. <span data-ttu-id="01fee-187">Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="01fee-187">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="01fee-188">Najde hello `<UserJourneys>` elementu a zkopírujte hello celý `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="01fee-188">Find hello `<UserJourneys>` element and copy hello entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="01fee-189">Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="01fee-189">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="01fee-190">Pokud hello element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="01fee-190">If hello element doesn't exist, add one.</span></span>
1. <span data-ttu-id="01fee-191">Vložení hello celý `<UserJourney>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="01fee-191">Paste hello entire `<UserJourney>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="01fee-192">Přejmenujte hello ID hello nové uživatele cesty (například přejmenování jako `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="01fee-192">Rename hello ID of hello new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="01fee-193">Zobrazení hello "button"</span><span class="sxs-lookup"><span data-stu-id="01fee-193">Display hello "button"</span></span>

<span data-ttu-id="01fee-194">Hello `<ClaimsProviderSelection>` element je obdobou tooan tlačítko zprostředkovatele identity na obrazovce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="01fee-194">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="01fee-195">Pokud přidáte `<ClaimsProviderSelection>` element pro Azure AD, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-195">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="01fee-196">tooadd tento element:</span><span class="sxs-lookup"><span data-stu-id="01fee-196">tooadd this element:</span></span>

1. <span data-ttu-id="01fee-197">Najde hello `<OrchestrationStep>` uzlu, který zahrnuje `Order="1"` v hello uživatele cestu, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="01fee-197">Find hello `<OrchestrationStep>` node that includes `Order="1"` in hello user journey that you just created.</span></span>
1. <span data-ttu-id="01fee-198">Přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="01fee-198">Add hello following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="01fee-199">Nastavit `TargetClaimsExchangeId` tooan odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="01fee-199">Set `TargetClaimsExchangeId` tooan appropriate value.</span></span> <span data-ttu-id="01fee-200">Doporučujeme následující hello stejné konvence jako ostatní:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="01fee-200">We recommend following hello same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="01fee-201">Akce tooan tlačítka hello odkaz</span><span class="sxs-lookup"><span data-stu-id="01fee-201">Link hello button tooan action</span></span>

<span data-ttu-id="01fee-202">Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce.</span><span class="sxs-lookup"><span data-stu-id="01fee-202">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="01fee-203">pro Azure AD B2C toocommunicate s Azure AD tooreceive Hello akce v tomto případě je token.</span><span class="sxs-lookup"><span data-stu-id="01fee-203">hello action, in this case, is for Azure AD B2C toocommunicate with Azure AD tooreceive a token.</span></span> <span data-ttu-id="01fee-204">Odkaz hello tlačítko tooan akce pomocí propojení hello technické profil pro poskytovatele deklarací identity služby Azure AD:</span><span class="sxs-lookup"><span data-stu-id="01fee-204">Link hello button tooan action by linking hello technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="01fee-205">Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="01fee-205">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
1. <span data-ttu-id="01fee-206">Přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="01fee-206">Add hello following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="01fee-207">Aktualizace `Id` toohello stejnou hodnotu jako `TargetClaimsExchangeId` v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-207">Update `Id` toohello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
1. <span data-ttu-id="01fee-208">Aktualizace `TechnicalProfileReferenceId` toohello ID hello technické profilu, kterou jste vytvořili dříve (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="01fee-208">Update `TechnicalProfileReferenceId` toohello ID of hello technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="01fee-209">Nahrát soubor aktualizované rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="01fee-209">Upload hello updated extension file</span></span>

<span data-ttu-id="01fee-210">Dokončení změny souboru rozšíření bylo hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-210">You are done modifying hello extension file.</span></span> <span data-ttu-id="01fee-211">Uložte jej.</span><span class="sxs-lookup"><span data-stu-id="01fee-211">Save it.</span></span> <span data-ttu-id="01fee-212">Nahrajte soubor hello a ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="01fee-212">Then upload hello file, and ensure that all validations succeed.</span></span>

### <a name="update-hello-rp-file"></a><span data-ttu-id="01fee-213">Aktualizujte soubor RP hello</span><span class="sxs-lookup"><span data-stu-id="01fee-213">Update hello RP file</span></span>

<span data-ttu-id="01fee-214">Teď musíte tooupdate hello předávající stranu soubor, který zahájí hello uživatele cestu, kterou jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="01fee-214">You now need tooupdate hello relying party (RP) file that will initiate hello user journey that you just created:</span></span>

1. <span data-ttu-id="01fee-215">Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář a přejmenujte ho (například přejmenujte ji tooSignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="01fee-215">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it tooSignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="01fee-216">Otevřete hello nový soubor a aktualizace hello `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu (například SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="01fee-216">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="01fee-217">To bude název hello zásad (například B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="01fee-217">This will be hello name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="01fee-218">Upravit hello `ReferenceId` atribut `<DefaultUserJourney>` toomatch hello ID hello nové uživatele cesty vytvořený (SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="01fee-218">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello ID of hello new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="01fee-219">Změny uložte a nahrajte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="01fee-219">Save your changes, and upload hello file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="01fee-220">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="01fee-220">Troubleshooting</span></span>

<span data-ttu-id="01fee-221">Testování hello vlastních zásad, které jste právě nahráli otevřením její okno a kliknutím na **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="01fee-221">Test hello custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="01fee-222">toodiagnose problémy, přečtěte si informace o [řešení potíží s](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="01fee-222">toodiagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="01fee-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="01fee-223">Next steps</span></span>

<span data-ttu-id="01fee-224">Poskytnutí zpětné vazby příliš[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="01fee-224">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
