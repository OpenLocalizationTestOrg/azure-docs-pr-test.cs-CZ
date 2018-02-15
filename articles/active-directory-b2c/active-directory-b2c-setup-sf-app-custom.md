---
title: "Azure Active Directory B2C: Přidání poskytovatele služby Salesforce SAML pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o tom, jak vytvořit a spravovat vlastní zásady Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: 16f7c5708b479f18de17a612a733a2be6e97ad01
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a><span data-ttu-id="5c7f4-103">Azure Active Directory B2C: Přihlaste se pomocí účtů služby Salesforce pomocí SAML</span><span class="sxs-lookup"><span data-stu-id="5c7f4-103">Azure Active Directory B2C: Sign in by using Salesforce accounts via SAML</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="5c7f4-104">V tomto článku se dozvíte, jak používat [vlastní zásady](active-directory-b2c-overview-custom.md) nastavení přihlášení pro uživatele z konkrétní organizace služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-104">This article shows you how to use [custom policies](active-directory-b2c-overview-custom.md) to set up sign-in for users from a specific Salesforce organization.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c7f4-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5c7f4-105">Prerequisites</span></span>

### <a name="azure-ad-b2c-setup"></a><span data-ttu-id="5c7f4-106">Instalace nástroje Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5c7f4-106">Azure AD B2C setup</span></span>

<span data-ttu-id="5c7f4-107">Ujistěte se, že jste dokončili všechny kroky, které ukazují, jak k [začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) v Azure Active Directory B2C (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-107">Ensure that you have completed all of the steps that show you how to [get started with custom policies](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="5c7f4-108">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-108">These include:</span></span>

* <span data-ttu-id="5c7f4-109">Vytvoření klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-109">Create an Azure AD B2C tenant.</span></span>
* <span data-ttu-id="5c7f4-110">Vytvoření aplikace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-110">Create an Azure AD B2C application.</span></span>
* <span data-ttu-id="5c7f4-111">Zaregistrujte dvě aplikace modul zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-111">Register two policy engine applications.</span></span>
* <span data-ttu-id="5c7f4-112">Nastavení klíčů.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-112">Set up keys.</span></span>
* <span data-ttu-id="5c7f4-113">Nastavte starter pack.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-113">Set up the starter pack.</span></span>

### <a name="salesforce-setup"></a><span data-ttu-id="5c7f4-114">Instalační program služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="5c7f4-114">Salesforce setup</span></span>

<span data-ttu-id="5c7f4-115">V tomto článku předpokládáme, že jste již dokončili následující:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-115">In this article, we assume that you have already done the following:</span></span>

* <span data-ttu-id="5c7f4-116">Registraci účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-116">Signed up for a Salesforce account.</span></span> <span data-ttu-id="5c7f4-117">Můžete si zaregistrovat [bezplatný účet Developer Edition](https://developer.salesforce.com/signup).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-117">You can sign up for a [free Developer Edition account](https://developer.salesforce.com/signup).</span></span>
* <span data-ttu-id="5c7f4-118">[Nastavit Moje domény](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) pro vaši organizaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-118">[Set up a My Domain](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) for your Salesforce organization.</span></span>

## <a name="set-up-salesforce-so-users-can-federate"></a><span data-ttu-id="5c7f4-119">Nastavení Salesforce, aby uživatelé mohli Federovat</span><span class="sxs-lookup"><span data-stu-id="5c7f4-119">Set up Salesforce so users can federate</span></span>

<span data-ttu-id="5c7f4-120">Pomoc při komunikaci s Salesforce Azure AD B2C, musíte získat adresu URL metadat služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-120">To help Azure AD B2C communicate with Salesforce, you need to get the Salesforce metadata URL.</span></span>

### <a name="set-up-salesforce-as-an-identity-provider"></a><span data-ttu-id="5c7f4-121">Nastavení Salesforce jako zprostředkovatel identity</span><span class="sxs-lookup"><span data-stu-id="5c7f4-121">Set up Salesforce as an identity provider</span></span>

> [!NOTE]
> <span data-ttu-id="5c7f4-122">V tomto článku předpokládáme, že používáte [Salesforce Lightning prostředí](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-122">In this article, we assume you are using [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span></span>

1. <span data-ttu-id="5c7f4-123">[Přihlaste se k Salesforce](https://login.salesforce.com/).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-123">[Sign in to Salesforce](https://login.salesforce.com/).</span></span> 
2. <span data-ttu-id="5c7f4-124">V nabídce vlevo pod **nastavení**, rozbalte položku **Identity**a potom klikněte na **zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-124">On the left menu, under **Settings**, expand **Identity**, and then click **Identity Provider**.</span></span>
3. <span data-ttu-id="5c7f4-125">Klikněte na tlačítko **povolit zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-125">Click **Enable Identity Provider**.</span></span>
4. <span data-ttu-id="5c7f4-126">V části **vyberte certifikát**, vyberte certifikát, který chcete podporu Salesforce se používají ke komunikaci s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-126">Under **Select the certificate**, select the certificate you want Salesforce to use to communicate with Azure AD B2C.</span></span> <span data-ttu-id="5c7f4-127">(Můžete použít výchozí certifikát.) Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-127">(You can use the default certificate.) Click **Save**.</span></span> 

### <a name="create-a-connected-app-in-salesforce"></a><span data-ttu-id="5c7f4-128">Vytvoření připojené aplikaci v Salesforce</span><span class="sxs-lookup"><span data-stu-id="5c7f4-128">Create a connected app in Salesforce</span></span>

1. <span data-ttu-id="5c7f4-129">Na **zprostředkovatele Identity** stránky, přejděte na **poskytovatelé služeb**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-129">On the **Identity Provider** page, go to **Service Providers**.</span></span>
2. <span data-ttu-id="5c7f4-130">Klikněte na tlačítko **poskytovatelé služeb jsou teď vytvořené prostřednictvím připojené aplikace. Kliknutím sem.**</span><span class="sxs-lookup"><span data-stu-id="5c7f4-130">Click **Service Providers are now created via Connected Apps. Click here.**</span></span>
3. <span data-ttu-id="5c7f4-131">V části **základní informace**, zadejte požadované hodnoty pro připojené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-131">Under **Basic Information**,  enter the required values for your connected app.</span></span>
4. <span data-ttu-id="5c7f4-132">V části **nastavení webové aplikace**, vyberte **povolit SAML** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-132">Under **Web App Settings**, select the **Enable SAML** check box.</span></span>
5. <span data-ttu-id="5c7f4-133">V **Entity ID** pole, zadejte následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-133">In the **Entity ID** field, enter the following URL.</span></span> <span data-ttu-id="5c7f4-134">Ujistěte se, že nahradíte hodnotu `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-134">Ensure that you replace the value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. <span data-ttu-id="5c7f4-135">V **adresa URL služby ACS** pole, zadejte následující adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-135">In the **ACS URL** field, enter the following URL.</span></span> <span data-ttu-id="5c7f4-136">Ujistěte se, že nahradíte hodnotu `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-136">Ensure that you replace the value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. <span data-ttu-id="5c7f4-137">Ponechte výchozí hodnoty pro všechna ostatní nastavení.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-137">Leave the default values for all other settings.</span></span>
8. <span data-ttu-id="5c7f4-138">Přejděte do dolní části seznamu a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-138">Scroll to the bottom of the list, and then click **Save**.</span></span>

### <a name="get-the-metadata-url"></a><span data-ttu-id="5c7f4-139">Získat adresu URL metadat</span><span class="sxs-lookup"><span data-stu-id="5c7f4-139">Get the metadata URL</span></span>

1. <span data-ttu-id="5c7f4-140">Na stránce Přehled připojené aplikaci klikněte na **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-140">On the overview page of your connected app, click **Manage**.</span></span>
2. <span data-ttu-id="5c7f4-141">Zkopírujte hodnotu pro **koncový bod zjišťování metadat**a pak ho uložte.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-141">Copy the value for **Metadata Discovery Endpoint**, and then save it.</span></span> <span data-ttu-id="5c7f4-142">Použijete ho později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-142">You'll use it later in this article.</span></span>

### <a name="set-up-salesforce-users-to-federate"></a><span data-ttu-id="5c7f4-143">Nastavení Salesforce uživatelů pro vytvoření federace</span><span class="sxs-lookup"><span data-stu-id="5c7f4-143">Set up Salesforce users to federate</span></span>

1. <span data-ttu-id="5c7f4-144">Na **spravovat** stránku vaší připojené aplikaci, přejděte na **profily**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-144">On the **Manage** page of your connected app, go to **Profiles**.</span></span>
2. <span data-ttu-id="5c7f4-145">Klikněte na tlačítko **spravovat profily**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-145">Click **Manage Profiles**.</span></span>
3. <span data-ttu-id="5c7f4-146">Vyberte profily (nebo skupiny uživatelů), kterou chcete vytvořit federaci s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-146">Select the profiles (or groups of users) that you want to federate with Azure AD B2C.</span></span> <span data-ttu-id="5c7f4-147">Jako správce systému, vyberte **správce systému** zaškrtnutí políčka, takže můžete vytvořit federaci s použitím účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-147">As a system administrator, select the **System Administrator** check box, so that you can federate by using your Salesforce account.</span></span>

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a><span data-ttu-id="5c7f4-148">Generovat podpisový certifikát pro Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5c7f4-148">Generate a signing certificate for Azure AD B2C</span></span>

<span data-ttu-id="5c7f4-149">Požadavky odeslané do služby Salesforce musí být podepsány pomocí Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-149">Requests sent to Salesforce need to be signed by Azure AD B2C.</span></span> <span data-ttu-id="5c7f4-150">Pokud chcete vygenerovat podpisový certifikát, otevřete prostředí Azure PowerShell a spusťte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-150">To generate a signing certificate, open Azure PowerShell, and then run the following commands.</span></span>

> [!NOTE]
> <span data-ttu-id="5c7f4-151">Zajistěte, aby aktualizovat název klienta a heslo v horních dvou řádcích.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-151">Make sure that you update the tenant name and password in the top two lines.</span></span>

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a><span data-ttu-id="5c7f4-152">Přidání podpisového certifikátu SAML do Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="5c7f4-152">Add the SAML signing certificate to Azure AD B2C</span></span>

<span data-ttu-id="5c7f4-153">Podpisový certifikát odešlete ke klientovi Azure AD B2C:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-153">Upload the signing certificate to your Azure AD B2C tenant:</span></span> 

1. <span data-ttu-id="5c7f4-154">Přejděte ke klientovi Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-154">Go to your Azure AD B2C tenant.</span></span> <span data-ttu-id="5c7f4-155">Klikněte na tlačítko **nastavení** > **Identity rozhraní Framework** > **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-155">Click **Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
2. <span data-ttu-id="5c7f4-156">Klikněte na tlačítko **+ přidat**a pak:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-156">Click **+Add**, and then:</span></span>
    1. <span data-ttu-id="5c7f4-157">Klikněte na tlačítko **možnosti** > **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-157">Click **Options** > **Upload**.</span></span>
    2. <span data-ttu-id="5c7f4-158">Zadejte **název** (například SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-158">Enter a **Name** (for example, SAMLSigningCert).</span></span> <span data-ttu-id="5c7f4-159">Předpona *B2C_1A_* se automaticky přidá k názvu klíče.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-159">The prefix *B2C_1A_* is automatically added to the name of your key.</span></span>
    3. <span data-ttu-id="5c7f4-160">Chcete-li vybrat certifikát, vyberte **nahrát ovládacího prvku se souborovým**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-160">To select your certificate, select **upload file control**.</span></span> 
    4. <span data-ttu-id="5c7f4-161">Zadejte heslo k certifikátu, který nastavíte ve skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-161">Enter the certificate's password that you set in the PowerShell script.</span></span>
3. <span data-ttu-id="5c7f4-162">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-162">Click **Create**.</span></span>
4. <span data-ttu-id="5c7f4-163">Ověřte, že jste vytvořili klíč (například B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-163">Verify that you've created a key (for example, B2C_1A_SAMLSigningCert).</span></span> <span data-ttu-id="5c7f4-164">Poznamenejte si úplný název (včetně *B2C_1A_*).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-164">Take note of the full name (including *B2C_1A_*).</span></span> <span data-ttu-id="5c7f4-165">Je bude odkazovat na tento klíč později v zásadách.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-165">You will refer to this key later in the policy.</span></span>

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a><span data-ttu-id="5c7f4-166">Vytvoření zprostředkovatele deklarací identity Salesforce SAML v základní zásady</span><span class="sxs-lookup"><span data-stu-id="5c7f4-166">Create the Salesforce SAML claims provider in your base policy</span></span>

<span data-ttu-id="5c7f4-167">Je třeba definovat Salesforce jako poskytovatele deklarací identity, takže uživatelům se můžete přihlásit pomocí služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-167">You need to define Salesforce as a claims provider so users can sign in by using Salesforce.</span></span> <span data-ttu-id="5c7f4-168">Jinými slovy budete muset zadat koncový bod, který Azure AD B2C bude komunikovat se službou.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-168">In other words, you need to specify the endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="5c7f4-169">Koncový bod se *poskytují* sadu *deklarace identity* využívající Azure AD B2C, chcete-li ověřit, že byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-169">The endpoint will *provide* a set of *claims* that Azure AD B2C uses to verify that a specific user has authenticated.</span></span> <span data-ttu-id="5c7f4-170">Chcete-li to provést, přidejte `<ClaimsProvider>` pro služby Salesforce v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-170">To do this, add a `<ClaimsProvider>` for Salesforce in the extension file of your policy:</span></span>

1. <span data-ttu-id="5c7f4-171">V pracovním adresáři otevřete soubor rozšíření (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-171">In your working directory, open the extension file (TrustFrameworkExtensions.xml).</span></span>
2. <span data-ttu-id="5c7f4-172">Najít `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-172">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="5c7f4-173">Pokud neexistuje, vytvořte ho do kořenového uzlu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-173">If it does not exist, create it under the root node.</span></span>
3. <span data-ttu-id="5c7f4-174">Přidejte nový `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-174">Add a new `<ClaimsProvider>`:</span></span>

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

<span data-ttu-id="5c7f4-175">V části `<ClaimsProvider>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-175">Under the `<ClaimsProvider>` node:</span></span>

1. <span data-ttu-id="5c7f4-176">Změňte hodnotu `<Domain>` na jedinečnou hodnotu, která rozlišuje `<ClaimsProvider>` od jiných poskytovatelů identit.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-176">Change the value for `<Domain>` to a unique value that distinguishes `<ClaimsProvider>` from other identity providers.</span></span>
2. <span data-ttu-id="5c7f4-177">Aktualizujte hodnotu pro `<DisplayName>` na název zobrazení pro zprostředkovatele deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-177">Update the value for `<DisplayName>` to a display name for the claims provider.</span></span> <span data-ttu-id="5c7f4-178">Tato hodnota není v současné době používá.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-178">Currently, this value is not used.</span></span>

### <a name="update-the-technical-profile"></a><span data-ttu-id="5c7f4-179">Technické profil aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-179">Update the technical profile</span></span>

<span data-ttu-id="5c7f4-180">Chcete-li získat token SAML ze služby Salesforce, definujte protokolů, které Azure AD B2C bude používat pro komunikaci se službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-180">To get a SAML token from Salesforce, define the protocols that Azure AD B2C will use to communicate with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="5c7f4-181">K tomu `<TechnicalProfile>` element `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-181">Do this in the `<TechnicalProfile>` element of `<ClaimsProvider>`:</span></span>

1. <span data-ttu-id="5c7f4-182">Aktualizovat ID `<TechnicalProfile>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-182">Update the ID of the `<TechnicalProfile>` node.</span></span> <span data-ttu-id="5c7f4-183">Toto ID použité k odkazování na tento profil technické z dalších částí zásad.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-183">This ID is used to refer to this technical profile from other parts of the policy.</span></span>
2. <span data-ttu-id="5c7f4-184">Aktualizujte hodnotu pro `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-184">Update the value for `<DisplayName>`.</span></span> <span data-ttu-id="5c7f4-185">Tato hodnota se zobrazí na tlačítko přihlásit na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-185">This value is displayed on the sign-in button on your sign-in page.</span></span>
3. <span data-ttu-id="5c7f4-186">Aktualizujte hodnotu pro `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-186">Update the value for `<Description>`.</span></span>
4. <span data-ttu-id="5c7f4-187">Salesforce používá protokol SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-187">Salesforce uses the SAML 2.0 protocol.</span></span> <span data-ttu-id="5c7f4-188">Ujistěte se, že hodnota `<Protocol>` je **typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-188">Ensure that the value for `<Protocol>` is **SAML2**.</span></span>

<span data-ttu-id="5c7f4-189">Aktualizace `<Metadata>` část v předchozím XML tak, aby odrážela nastavení pro konkrétní účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-189">Update the `<Metadata>` section in the preceding XML to reflect the settings for your specific Salesforce account.</span></span> <span data-ttu-id="5c7f4-190">V souboru XML aktualizujte metadata hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-190">In the XML, update the metadata values:</span></span>

1. <span data-ttu-id="5c7f4-191">Aktualizujte hodnotu `<Item Key="PartnerEntity">` s adresou URL metadat Salesforce jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-191">Update the value of `<Item Key="PartnerEntity">` with the Salesforce metadata URL you copied earlier.</span></span> <span data-ttu-id="5c7f4-192">Má následující formát:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-192">It has the following format:</span></span> 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. <span data-ttu-id="5c7f4-193">V `<CryptographicKeys>` část, aktualizujte hodnotu v obou instancí `StorageReferenceId` k názvu klíče podpisového certifikátu (například B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-193">In the `<CryptographicKeys>` section, update the value for both instances of `StorageReferenceId` to the name of the key of your signing certificate (for example, B2C_1A_SAMLSigningCert).</span></span>

### <a name="upload-the-extension-file-for-verification"></a><span data-ttu-id="5c7f4-194">Nahrát soubor rozšíření pro ověření</span><span class="sxs-lookup"><span data-stu-id="5c7f4-194">Upload the extension file for verification</span></span>

<span data-ttu-id="5c7f4-195">Vaše zásada je nyní nakonfigurován tak, aby Azure AD B2C umí ke komunikaci s Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-195">Your policy is now configured so that Azure AD B2C knows how to communicate with Salesforce.</span></span> <span data-ttu-id="5c7f4-196">Zkuste odeslat soubor rozšíření zásad, chcete-li ověřit, že nejsou k dispozici všechny problémy, pokud.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-196">Try uploading the extension file of your policy, to verify that there aren't any issues so far.</span></span> <span data-ttu-id="5c7f4-197">Nahrát soubor rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-197">To upload the extension file of your policy:</span></span>

1. <span data-ttu-id="5c7f4-198">Ve vašem klientu Azure AD B2C, přejděte na **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-198">In your Azure AD B2C tenant, go to the **All Policies** blade.</span></span>
2. <span data-ttu-id="5c7f4-199">Vyberte **přepsat zásady, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-199">Select the **Overwrite the policy if it exists** check box.</span></span>
3. <span data-ttu-id="5c7f4-200">Nahrajte soubor rozšíření (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-200">Upload the extension file (TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="5c7f4-201">Ujistěte se, že neselže ověření.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-201">Ensure that it doesn't fail validation.</span></span>

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a><span data-ttu-id="5c7f4-202">Zaregistrujte zprostředkovatele deklarací identity Salesforce SAML k cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="5c7f4-202">Register the Salesforce SAML claims provider to a user journey</span></span>

<span data-ttu-id="5c7f4-203">Dál přidejte poskytovatele identity Salesforce SAML na jednu z vaší uživatelské cesty.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-203">Next, add the Salesforce SAML identity provider to one of your user journeys.</span></span> <span data-ttu-id="5c7f4-204">V tomto okamžiku byla nastavena zprostředkovatele identity, ale není k dispozici na všech stránkách registrace nebo přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-204">At this point, the identity provider has been set up, but it’s not available on any of the user sign-up or sign-in pages.</span></span> <span data-ttu-id="5c7f4-205">Pro přidání poskytovatele identity na přihlašovací stránku, nejprve vytvořte duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-205">To add the identity provider to a sign-in page, first, create a duplicate of an existing template user journey.</span></span> <span data-ttu-id="5c7f4-206">Potom upravte šablonu tak, aby je také poskytovatele identit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-206">Then, modify the template so that it also has the Azure AD identity provider.</span></span>

1. <span data-ttu-id="5c7f4-207">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-207">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="5c7f4-208">Najít `<UserJourneys>` elementu a zkopírujte celou `<UserJourney>` hodnoty, včetně Id = "SignUpOrSignIn".</span><span class="sxs-lookup"><span data-stu-id="5c7f4-208">Find the `<UserJourneys>` element, and then copy the entire `<UserJourney>` value, including Id=”SignUpOrSignIn”.</span></span>
3. <span data-ttu-id="5c7f4-209">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-209">Open the extension file (for example, TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="5c7f4-210">Najít `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-210">Find the `<UserJourneys>` element.</span></span> <span data-ttu-id="5c7f4-211">Pokud element neexistuje, vytvořte ji.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-211">If the element doesn't exist, create one.</span></span>
4. <span data-ttu-id="5c7f4-212">Vložte zkopírovali celý `<UserJourney>` jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-212">Paste the entire copied `<UserJourney>` as a child of the `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="5c7f4-213">Přejmenujte ID nové `<UserJourney>` (například SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-213">Rename the ID of the new `<UserJourney>` (for example, SignUpOrSignUsingContoso).</span></span>

### <a name="display-the-identity-provider-button"></a><span data-ttu-id="5c7f4-214">Zobrazení tlačítka zprostředkovatele identity</span><span class="sxs-lookup"><span data-stu-id="5c7f4-214">Display the identity provider button</span></span>

<span data-ttu-id="5c7f4-215">`<ClaimsProviderSelection>` Element je obdobou tlačítko zprostředkovatele identity na stránce registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-215">The `<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="5c7f4-216">Přidáním `<ClaimsProviderSelection>` element pro Salesforce nové tlačítko se zobrazí, když uživatel přejde na této stránce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-216">By adding an `<ClaimsProviderSelection>` element for Salesforce, a new button appears when a user goes to this page.</span></span> <span data-ttu-id="5c7f4-217">Chcete-li zobrazit tlačítko zprostředkovatele identity:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-217">To display the identity provider button:</span></span>

1. <span data-ttu-id="5c7f4-218">V `<UserJourney>` , kterou jste vytvořili, vyhledejte `<OrchestrationStep>` s `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-218">In the `<UserJourney>` that you created, find the `<OrchestrationStep>` with `Order="1"`.</span></span>
2. <span data-ttu-id="5c7f4-219">Přidejte následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-219">Add the following XML:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. <span data-ttu-id="5c7f4-220">Nastavit `TargetClaimsExchangeId` na logickou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-220">Set `TargetClaimsExchangeId` to a logical value.</span></span> <span data-ttu-id="5c7f4-221">Doporučujeme následující stejné konvence jako jiné (například  *\[ClaimProviderName\]Exchange*).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-221">We recommend following the same convention as others (for example, *\[ClaimProviderName\]Exchange*).</span></span>

### <a name="link-the-identity-provider-button-to-an-action"></a><span data-ttu-id="5c7f4-222">Tlačítko zprostředkovatele identity propojit akce</span><span class="sxs-lookup"><span data-stu-id="5c7f4-222">Link the identity provider button to an action</span></span>

<span data-ttu-id="5c7f4-223">Nyní když máte tlačítko zprostředkovatele identity na místě, propojte akce.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-223">Now that you have an identity provider button in place, link it to an action.</span></span> <span data-ttu-id="5c7f4-224">V takovém případě je akce pro Azure AD B2C ke komunikaci s Salesforce přijímat tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-224">In this case, the action is for Azure AD B2C to communicate with Salesforce to receive a SAML token.</span></span> <span data-ttu-id="5c7f4-225">To provedete pomocí propojení technické profil pro vaše Salesforce SAML poskytovatele deklarací identity:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-225">You can do this by linking the technical profile for your Salesforce SAML claims provider:</span></span>

1. <span data-ttu-id="5c7f4-226">V `<UserJourney>` uzlu najít `<OrchestrationStep>` s `Order="2"`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-226">In the `<UserJourney>` node, find the `<OrchestrationStep>` with `Order="2"`.</span></span>
2. <span data-ttu-id="5c7f4-227">Přidejte následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-227">Add the following XML:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. <span data-ttu-id="5c7f4-228">Aktualizace `Id` na stejnou hodnotu, který jste použili předtím pro `TargetClaimsExchangeId`.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-228">Update `Id` to the same value that you used earlier for `TargetClaimsExchangeId`.</span></span>
4. <span data-ttu-id="5c7f4-229">Aktualizace `TechnicalProfileReferenceId` k `Id` technických profilu, kterou jste vytvořili dříve (například ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-229">Update `TechnicalProfileReferenceId` to the `Id` of the technical profile you created earlier (for example, ContosoProfile).</span></span>

### <a name="upload-the-updated-extension-file"></a><span data-ttu-id="5c7f4-230">Nahrát soubor aktualizované rozšíření</span><span class="sxs-lookup"><span data-stu-id="5c7f4-230">Upload the updated extension file</span></span>

<span data-ttu-id="5c7f4-231">Dokončení úpravy souboru rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-231">You are done modifying the extension file.</span></span> <span data-ttu-id="5c7f4-232">Uložte a nahrajte tento soubor.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-232">Save and upload this file.</span></span> <span data-ttu-id="5c7f4-233">Ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-233">Ensure that all validations succeed.</span></span>

### <a name="update-the-relying-party-file"></a><span data-ttu-id="5c7f4-234">Aktualizace souboru předávající strany</span><span class="sxs-lookup"><span data-stu-id="5c7f4-234">Update the relying party file</span></span>

<span data-ttu-id="5c7f4-235">Potom aktualizujte soubor předávající stranu, který iniciuje cesty uživatele, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5c7f4-235">Next, update the relying party (RP) file that initiates the user journey that you created:</span></span>

1. <span data-ttu-id="5c7f4-236">Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-236">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="5c7f4-237">Potom přejmenujte ji (například SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-237">Then, rename it (for example, SignUpOrSignInWithAAD.xml).</span></span>
2. <span data-ttu-id="5c7f4-238">Otevřete nový soubor a aktualizace `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-238">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="5c7f4-239">Toto je název vaší zásady (například SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-239">This is the name of your policy (for example, SignUpOrSignInWithAAD).</span></span>
3. <span data-ttu-id="5c7f4-240">Změnit `ReferenceId` atribut `<DefaultUserJourney>` tak, aby odpovídala `Id` nové cesty uživatele, který jste vytvořili (například SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-240">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the `Id` of the new user journey that you created (for example, SignUpOrSignUsingContoso).</span></span>
4. <span data-ttu-id="5c7f4-241">Uložte změny a potom soubor odešlete.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-241">Save your changes, and then upload the file.</span></span>

## <a name="test-and-troubleshoot"></a><span data-ttu-id="5c7f4-242">Testování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5c7f4-242">Test and troubleshoot</span></span>

<span data-ttu-id="5c7f4-243">K testování vlastních zásad, které jste právě nahráli, na portálu Azure, přejděte do okna zásady a pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="5c7f4-243">To test the custom policy that you just uploaded, in the Azure portal, go to the policy blade, and then click **Run now**.</span></span> <span data-ttu-id="5c7f4-244">Pokud se nezdaří, najdete v části [řešení potíží se zásadami vlastní](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-244">If it fails, see [Troubleshoot custom policies](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c7f4-245">Další postup</span><span class="sxs-lookup"><span data-stu-id="5c7f4-245">Next steps</span></span>

<span data-ttu-id="5c7f4-246">Poskytnutí zpětné vazby k [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="5c7f4-246">Provide feedback to [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
