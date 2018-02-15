---
title: "Azure Active Directory B2C: Přidejte zprostředkovatele služby Azure AD pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o vlastních zásad Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: f34326bcb8a7cbf5b5cf75e8f18f2843abc0b3ab
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="93187-103">Azure Active Directory B2C: Přihlaste se pomocí účtů služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="93187-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="93187-104">Tento článek ukazuje, jak povolit přihlášení pro uživatele z konkrétní organizace Azure Active Directory (Azure AD) prostřednictvím [vlastní zásady](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="93187-104">This article shows you how to enable sign-in for users from a specific Azure Active Directory (Azure AD) organization through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93187-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="93187-105">Prerequisites</span></span>

<span data-ttu-id="93187-106">Proveďte kroky v [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.</span><span class="sxs-lookup"><span data-stu-id="93187-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="93187-107">K těmto krokům patří:</span><span class="sxs-lookup"><span data-stu-id="93187-107">These steps include:</span></span>

1. <span data-ttu-id="93187-108">Vytvoření Azure Active Directory B2C klienta (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="93187-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="93187-109">Vytvoření aplikace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="93187-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="93187-110">Probíhá registrace dvě aplikace modul zásad.</span><span class="sxs-lookup"><span data-stu-id="93187-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="93187-111">Nastavení klíče.</span><span class="sxs-lookup"><span data-stu-id="93187-111">Setting up keys.</span></span>
5. <span data-ttu-id="93187-112">Nastavení úvodní pack.</span><span class="sxs-lookup"><span data-stu-id="93187-112">Setting up the starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="93187-113">Vytvoření aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="93187-113">Create an Azure AD app</span></span>

<span data-ttu-id="93187-114">Chcete-li povolit přihlášení pro uživatele z konkrétní organizace Azure AD, je nutné zaregistrovat aplikaci v rámci organizační klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93187-114">To enable sign-in for users from a specific Azure AD organization, you need to register an application within the organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="93187-115">Používáme "contoso.com" pro organizační klienta Azure AD a "fabrikamb2c.onmicrosoft.com" jako klienta Azure AD B2C v následujících pokynech.</span><span class="sxs-lookup"><span data-stu-id="93187-115">We use "contoso.com" for the organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as the Azure AD B2C tenant in the following instructions.</span></span>

1. <span data-ttu-id="93187-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="93187-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="93187-117">Na horním panelu vyberte svůj účet.</span><span class="sxs-lookup"><span data-stu-id="93187-117">On the top bar, select your account.</span></span> <span data-ttu-id="93187-118">Z **Directory** seznamu, vyberte organizační klienta Azure AD, ve které chcete zaregistrovat aplikaci (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="93187-118">From the **Directory** list, choose the organizational Azure AD tenant where you want to register your application (contoso.com).</span></span>
1. <span data-ttu-id="93187-119">Vyberte **další služby** v levém podokně a vyhledejte "Aplikace registrace."</span><span class="sxs-lookup"><span data-stu-id="93187-119">Select **More services** in the left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="93187-120">Vyberte **nové registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="93187-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="93187-121">Zadejte název pro vaši aplikaci (například `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="93187-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="93187-122">Jako typ aplikace vyberte položku **Webová aplikace / webové rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="93187-122">Select **Web app / API** for the application type.</span></span>
1. <span data-ttu-id="93187-123">Pro **přihlašovací adresa URL**, zadejte následující adresu URL, kde `yourtenant` je nahrazena název vašeho klienta Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="93187-123">For **Sign-on URL**, enter the following URL, where `yourtenant` is replaced by the name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    >[!NOTE]
    ><span data-ttu-id="93187-124">Hodnota pro "yourtenant" musí být psaný malými písmeny v **přihlašovací adresa URL**.</span><span class="sxs-lookup"><span data-stu-id="93187-124">The value for "yourtenant" must be all lowercase in the **Sign-on URL**.</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="93187-125">Uložit ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="93187-125">Save the application ID.</span></span>
1. <span data-ttu-id="93187-126">Vyberte nově vytvořenou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="93187-126">Select the newly created application.</span></span>
1. <span data-ttu-id="93187-127">V části **nastavení** vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="93187-127">Under the **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="93187-128">Vytvořte nový klíč a uložte jej.</span><span class="sxs-lookup"><span data-stu-id="93187-128">Create a new key, and save it.</span></span> <span data-ttu-id="93187-129">Budete ho používat v krocích v další části.</span><span class="sxs-lookup"><span data-stu-id="93187-129">You will use it in the steps in the next section.</span></span>

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a><span data-ttu-id="93187-130">Přidat klíč Azure AD do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="93187-130">Add the Azure AD key to Azure AD B2C</span></span>

<span data-ttu-id="93187-131">Budete muset uložit klíč contoso.com aplikace v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="93187-131">You need to store the contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="93187-132">Použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="93187-132">To do this:</span></span>
1. <span data-ttu-id="93187-133">Přejděte ke klientovi Azure AD B2C a vyberte **nastavení B2C** > **Identity rozhraní Framework** > **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="93187-133">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="93187-134">Vyberte **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="93187-134">Select **+Add**.</span></span>
1. <span data-ttu-id="93187-135">Vyberte nebo zadejte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="93187-135">Select or enter these options:</span></span>
   * <span data-ttu-id="93187-136">Vyberte **ruční**.</span><span class="sxs-lookup"><span data-stu-id="93187-136">Select **Manual**.</span></span>
   * <span data-ttu-id="93187-137">Pro **název**, zvolte název, který odpovídá název vašeho klienta Azure AD (například `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="93187-137">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="93187-138">Předpona `B2C_1A_` se automaticky přidá k názvu klíče.</span><span class="sxs-lookup"><span data-stu-id="93187-138">The prefix `B2C_1A_` is added automatically to the name of your key.</span></span>
   * <span data-ttu-id="93187-139">Vložte klíč vaší aplikace v **tajný klíč** pole.</span><span class="sxs-lookup"><span data-stu-id="93187-139">Paste your application key in the **Secret** box.</span></span>
   * <span data-ttu-id="93187-140">Vyberte **podpis**.</span><span class="sxs-lookup"><span data-stu-id="93187-140">Select **Signature**.</span></span>
1. <span data-ttu-id="93187-141">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="93187-141">Select **Create**.</span></span>
1. <span data-ttu-id="93187-142">Potvrďte, že jste vytvořili klíč `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="93187-142">Confirm that you've created the key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="93187-143">Přidání poskytovatele deklarací identity v základní zásady</span><span class="sxs-lookup"><span data-stu-id="93187-143">Add a claims provider in your base policy</span></span>

<span data-ttu-id="93187-144">Pokud chcete, aby uživatelé přihlásit pomocí služby Azure AD, budete muset definovat Azure AD jako zprostředkovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="93187-144">If you want users to sign in by using Azure AD, you need to define Azure AD as a claims provider.</span></span> <span data-ttu-id="93187-145">Jinými slovy budete muset zadat koncový bod, který Azure AD B2C bude komunikovat se.</span><span class="sxs-lookup"><span data-stu-id="93187-145">In other words, you need to specify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="93187-146">Koncový bod poskytne sadu deklarací identity, které používají Azure AD B2C k ověření, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="93187-146">The endpoint will provide a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span> 

<span data-ttu-id="93187-147">Azure AD jako zprostředkovatele deklarací identity můžete definovat přidáním Azure AD `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="93187-147">You can define Azure AD as a claims provider by adding Azure AD to the `<ClaimsProvider>` node in the extension file of your policy:</span></span>

1. <span data-ttu-id="93187-148">Otevřete soubor rozšíření (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="93187-148">Open the extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="93187-149">Najít `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="93187-149">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="93187-150">Pokud neexistuje, přidejte ji do kořenového uzlu.</span><span class="sxs-lookup"><span data-stu-id="93187-150">If it does not exist, add it under the root node.</span></span>
1. <span data-ttu-id="93187-151">Přidejte nový `<ClaimsProvider>` uzlu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93187-151">Add a new `<ClaimsProvider>` node as follows:</span></span>

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

1. <span data-ttu-id="93187-152">V části `<ClaimsProvider>` uzlu, aktualizujte hodnotu pro `<Domain>` k jedinečná hodnota, které je možné ho odlišuje od jiných zprostředkovatelů identity.</span><span class="sxs-lookup"><span data-stu-id="93187-152">Under the `<ClaimsProvider>` node, update the value for `<Domain>` to a unique value that can be used to distinguish it from other identity providers.</span></span>
1. <span data-ttu-id="93187-153">V části `<ClaimsProvider>` uzlu, aktualizujte hodnotu pro `<DisplayName>` pro popisný název zprostředkovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="93187-153">Under the `<ClaimsProvider>` node, update the value for `<DisplayName>` to a friendly name for the claims provider.</span></span> <span data-ttu-id="93187-154">Tato hodnota není právě používána.</span><span class="sxs-lookup"><span data-stu-id="93187-154">This value is not currently used.</span></span>

### <a name="update-the-technical-profile"></a><span data-ttu-id="93187-155">Technické profil aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="93187-155">Update the technical profile</span></span>

<span data-ttu-id="93187-156">K získání tokenu z koncového bodu Azure AD, budete muset definovat protokolů, které by měl použít Azure AD B2C ke komunikaci s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93187-156">To get a token from the Azure AD endpoint, you need to define the protocols that Azure AD B2C should use to communicate with Azure AD.</span></span> <span data-ttu-id="93187-157">To se provádí uvnitř `<TechnicalProfile>` element `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="93187-157">This is done inside the `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="93187-158">Aktualizovat ID `<TechnicalProfile>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="93187-158">Update the ID of the `<TechnicalProfile>` node.</span></span> <span data-ttu-id="93187-159">Toto ID použité k odkazování na tento profil technické z dalších částí zásad.</span><span class="sxs-lookup"><span data-stu-id="93187-159">This ID is used to refer to this technical profile from other parts of the policy.</span></span>
1. <span data-ttu-id="93187-160">Aktualizujte hodnotu pro `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="93187-160">Update the value for `<DisplayName>`.</span></span> <span data-ttu-id="93187-161">Tato hodnota se zobrazí na tlačítko přihlásit na obrazovce přihlášení.</span><span class="sxs-lookup"><span data-stu-id="93187-161">This value will be displayed on the sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="93187-162">Aktualizujte hodnotu pro `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="93187-162">Update the value for `<Description>`.</span></span>
1. <span data-ttu-id="93187-163">Azure AD používá protokol OpenID Connect, tak zajistěte, aby hodnota `<Protocol>` je `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="93187-163">Azure AD uses the OpenID Connect protocol, so ensure that the value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="93187-164">Je potřeba aktualizovat `<Metadata>` oddíl v souboru XML uvedené dříve tak, aby odrážela konfigurační nastavení pro konkrétní klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93187-164">You need to update the `<Metadata>` section in the XML file referred to earlier to reflect the configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="93187-165">V souboru XML aktualizujte metadata hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="93187-165">In the XML file, update the metadata values as follows:</span></span>

1. <span data-ttu-id="93187-166">Nastavit `<Item Key="METADATA">` k `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, kde `yourAzureADtenant` je název klienta Azure AD (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="93187-166">Set `<Item Key="METADATA">` to `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="93187-167">Otevřete prohlížeč a přejděte na `METADATA` adresu URL, kterou jste aktualizovali.</span><span class="sxs-lookup"><span data-stu-id="93187-167">Open your browser and go to the `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="93187-168">V prohlížeči vyhledejte objekt 'vystavitele' a zkopírujte jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="93187-168">In the browser, look for the 'issuer' object and copy its value.</span></span> <span data-ttu-id="93187-169">Měl by vypadat třeba takto: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="93187-169">It should look like the following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="93187-170">Vložit hodnotu pro `<Item Key="ProviderName">` v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="93187-170">Paste the value for `<Item Key="ProviderName">` in the XML file.</span></span>
1. <span data-ttu-id="93187-171">Nastavit `<Item Key="client_id">` ID aplikace z aplikace registrace.</span><span class="sxs-lookup"><span data-stu-id="93187-171">Set `<Item Key="client_id">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="93187-172">Nastavit `<Item Key="IdTokenAudience">` ID aplikace z aplikace registrace.</span><span class="sxs-lookup"><span data-stu-id="93187-172">Set `<Item Key="IdTokenAudience">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="93187-173">Ujistěte se, že `<Item Key="response_types">` je nastaven na `id_token`.</span><span class="sxs-lookup"><span data-stu-id="93187-173">Ensure that `<Item Key="response_types">` is set to `id_token`.</span></span>
1. <span data-ttu-id="93187-174">Ujistěte se, že `<Item Key="UsePolicyInRedirectUri">` je nastaven na `false`.</span><span class="sxs-lookup"><span data-stu-id="93187-174">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set to `false`.</span></span>

<span data-ttu-id="93187-175">Musíte také propojit Azure AD tajný klíč, který je zaregistrovaný v klientovi služby Azure AD B2C ke službě Azure AD `<ClaimsProvider>` informace:</span><span class="sxs-lookup"><span data-stu-id="93187-175">You also need to link the Azure AD secret that you registered in your Azure AD B2C tenant to the Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="93187-176">V `<CryptographicKeys>` tématu v předchozím soubor XML, aktualizujte hodnotu pro `StorageReferenceId` na odkaz na ID tajný klíč, který jste definovali (například `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="93187-176">In the `<CryptographicKeys>` section in the preceding XML file, update the value for `StorageReferenceId` to the reference ID of the secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-the-extension-file-for-verification"></a><span data-ttu-id="93187-177">Nahrát soubor rozšíření pro ověření</span><span class="sxs-lookup"><span data-stu-id="93187-177">Upload the extension file for verification</span></span>

<span data-ttu-id="93187-178">Nyní by jste nakonfigurovali zásady tak, aby Azure AD B2C umí ke komunikaci s adresáře Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93187-178">By now, you have configured your policy so that Azure AD B2C knows how to communicate with your Azure AD directory.</span></span> <span data-ttu-id="93187-179">Zkuste odeslat soubor rozšíření jenom k potvrzení, že nemá žádné problémy, pokud vaše zásady.</span><span class="sxs-lookup"><span data-stu-id="93187-179">Try uploading the extension file of your policy just to confirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="93187-180">Postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="93187-180">To do so:</span></span>

1. <span data-ttu-id="93187-181">Otevřete **všechny zásady** okno v klientovi služby Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="93187-181">Open the **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="93187-182">Zkontrolujte **přepsat zásady, pokud existuje** pole.</span><span class="sxs-lookup"><span data-stu-id="93187-182">Check the **Overwrite the policy if it exists** box.</span></span>
1. <span data-ttu-id="93187-183">Nahrát soubor rozšíření (TrustFrameworkExtensions.xml) a ujistěte se, že ověření neselže.</span><span class="sxs-lookup"><span data-stu-id="93187-183">Upload the extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail the validation.</span></span>

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a><span data-ttu-id="93187-184">Zaregistrujte poskytovatele deklarací identity Azure AD pro uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="93187-184">Register the Azure AD claims provider to a user journey</span></span>

<span data-ttu-id="93187-185">Teď je potřeba přidat poskytovatele identit Azure AD na jednu z vaší uživatelské cesty.</span><span class="sxs-lookup"><span data-stu-id="93187-185">You now need to add the Azure AD identity provider to one of your user journeys.</span></span> <span data-ttu-id="93187-186">V tomto okamžiku byla nastavena zprostředkovatele identity, ale není k dispozici v žádném z obrazovky registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="93187-186">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="93187-187">Chcete-li k dispozici, jsme se duplicitní existující uživatele cesty šablony vytvořit a upravit ho tak, aby je také poskytovatele identit Azure AD:</span><span class="sxs-lookup"><span data-stu-id="93187-187">To make it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has the Azure AD identity provider:</span></span>

1. <span data-ttu-id="93187-188">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="93187-188">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="93187-189">Najít `<UserJourneys>` elementu a zkopírujte celou `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="93187-189">Find the `<UserJourneys>` element and copy the entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="93187-190">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a najděte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="93187-190">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="93187-191">Pokud element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="93187-191">If the element doesn't exist, add one.</span></span>
1. <span data-ttu-id="93187-192">Vložte celý `<UserJourney>` uzlu, který jste zkopírovali jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="93187-192">Paste the entire `<UserJourney>` node that you copied as a child of the `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="93187-193">Přejmenujte ID nové cesty uživatele (například přejmenování jako `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="93187-193">Rename the ID of the new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-the-button"></a><span data-ttu-id="93187-194">Zobrazit "button"</span><span class="sxs-lookup"><span data-stu-id="93187-194">Display the "button"</span></span>

<span data-ttu-id="93187-195">`<ClaimsProviderSelection>` Element je obdobou tlačítko zprostředkovatele identity na obrazovce registrace-množství nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="93187-195">The `<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="93187-196">Pokud přidáte `<ClaimsProviderSelection>` element pro Azure AD, nové tlačítko se zobrazí při pojmenováváme uživatele na stránce.</span><span class="sxs-lookup"><span data-stu-id="93187-196">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="93187-197">Chcete-li přidat tento element:</span><span class="sxs-lookup"><span data-stu-id="93187-197">To add this element:</span></span>

1. <span data-ttu-id="93187-198">Najít `<OrchestrationStep>` uzlu, který zahrnuje `Order="1"` v cesty uživatele, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="93187-198">Find the `<OrchestrationStep>` node that includes `Order="1"` in the user journey that you just created.</span></span>
1. <span data-ttu-id="93187-199">Přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="93187-199">Add the following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="93187-200">Nastavit `TargetClaimsExchangeId` na odpovídající hodnotu.</span><span class="sxs-lookup"><span data-stu-id="93187-200">Set `TargetClaimsExchangeId` to an appropriate value.</span></span> <span data-ttu-id="93187-201">Doporučujeme následující stejné konvence jako ostatní:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="93187-201">We recommend following the same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="93187-202">Tlačítko propojit akce</span><span class="sxs-lookup"><span data-stu-id="93187-202">Link the button to an action</span></span>

<span data-ttu-id="93187-203">Nyní když máte tlačítka na místě, budete muset propojit akce.</span><span class="sxs-lookup"><span data-stu-id="93187-203">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="93187-204">Akce, v takovém případě je pro Azure AD B2C ke komunikaci s Azure AD přijmout token.</span><span class="sxs-lookup"><span data-stu-id="93187-204">The action, in this case, is for Azure AD B2C to communicate with Azure AD to receive a token.</span></span> <span data-ttu-id="93187-205">Odkaz na tlačítko akce pomocí propojení technické profil pro poskytovatele deklarací identity služby Azure AD:</span><span class="sxs-lookup"><span data-stu-id="93187-205">Link the button to an action by linking the technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="93187-206">Najít `<OrchestrationStep>` , který obsahuje `Order="2"` v `<UserJourney>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="93187-206">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
1. <span data-ttu-id="93187-207">Přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="93187-207">Add the following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="93187-208">Aktualizace `Id` na stejnou hodnotu jako `TargetClaimsExchangeId` v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="93187-208">Update `Id` to the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
1. <span data-ttu-id="93187-209">Aktualizace `TechnicalProfileReferenceId` na ID technické profil vytvoříte starší (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="93187-209">Update `TechnicalProfileReferenceId` to the ID of the technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-the-updated-extension-file"></a><span data-ttu-id="93187-210">Nahrát soubor aktualizované rozšíření</span><span class="sxs-lookup"><span data-stu-id="93187-210">Upload the updated extension file</span></span>

<span data-ttu-id="93187-211">Dokončení úpravy souboru rozšíření.</span><span class="sxs-lookup"><span data-stu-id="93187-211">You are done modifying the extension file.</span></span> <span data-ttu-id="93187-212">Uložte jej.</span><span class="sxs-lookup"><span data-stu-id="93187-212">Save it.</span></span> <span data-ttu-id="93187-213">Pak nahrajte soubor a ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="93187-213">Then upload the file, and ensure that all validations succeed.</span></span>

### <a name="update-the-rp-file"></a><span data-ttu-id="93187-214">Aktualizovat soubor RP</span><span class="sxs-lookup"><span data-stu-id="93187-214">Update the RP file</span></span>

<span data-ttu-id="93187-215">Teď je potřeba aktualizovat soubor předávající stranu, který zahájí cesty uživatele, který jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="93187-215">You now need to update the relying party (RP) file that will initiate the user journey that you just created:</span></span>

1. <span data-ttu-id="93187-216">Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář a přejmenujte ho (například přejmenujte jej na SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="93187-216">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it to SignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="93187-217">Otevřete nový soubor a aktualizace `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu (například SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="93187-217">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="93187-218">To bude název vaší zásady (například B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="93187-218">This will be the name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="93187-219">Změnit `ReferenceId` atribut `<DefaultUserJourney>` tak, aby odpovídaly ID nové cesty uživatele, který jste vytvořili (SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="93187-219">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the ID of the new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="93187-220">Uložte změny a soubor odešlete.</span><span class="sxs-lookup"><span data-stu-id="93187-220">Save your changes, and upload the file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="93187-221">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="93187-221">Troubleshooting</span></span>

<span data-ttu-id="93187-222">Testování vlastních zásad, které jste právě nahráli otevřením její okno a kliknutím na **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="93187-222">Test the custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="93187-223">K diagnostikování problémů, přečtěte si informace o [řešení potíží s](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="93187-223">To diagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="93187-224">Další postup</span><span class="sxs-lookup"><span data-stu-id="93187-224">Next steps</span></span>

<span data-ttu-id="93187-225">Poskytnutí zpětné vazby k [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="93187-225">Provide feedback to [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
