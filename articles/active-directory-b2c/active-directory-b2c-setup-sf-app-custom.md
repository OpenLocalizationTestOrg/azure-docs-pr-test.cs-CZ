---
title: "Azure Active Directory B2C: Přidání poskytovatele služby Salesforce SAML pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o tom toocreate a spravovat vlastní zásady Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: f14c9d96980ff124110db7cfb58bf7cd81750b7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a><span data-ttu-id="345ff-103">Azure Active Directory B2C: Přihlaste se pomocí účtů služby Salesforce pomocí SAML</span><span class="sxs-lookup"><span data-stu-id="345ff-103">Azure Active Directory B2C: Sign in by using Salesforce accounts via SAML</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="345ff-104">Tento článek ukazuje, jak toouse [vlastní zásady](active-directory-b2c-overview-custom.md) tooset až přihlášení pro uživatele z konkrétní organizace služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-104">This article shows you how toouse [custom policies](active-directory-b2c-overview-custom.md) tooset up sign-in for users from a specific Salesforce organization.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="345ff-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="345ff-105">Prerequisites</span></span>

### <a name="azure-ad-b2c-setup"></a><span data-ttu-id="345ff-106">Instalace nástroje Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="345ff-106">Azure AD B2C setup</span></span>

<span data-ttu-id="345ff-107">Ujistěte se, že jste dokončili všechny kroky hello, které ukazují, jak příliš[začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) v Azure Active Directory B2C (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="345ff-107">Ensure that you have completed all of hello steps that show you how too[get started with custom policies](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="345ff-108">Mezi ně patří:</span><span class="sxs-lookup"><span data-stu-id="345ff-108">These include:</span></span>

* <span data-ttu-id="345ff-109">Vytvoření klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-109">Create an Azure AD B2C tenant.</span></span>
* <span data-ttu-id="345ff-110">Vytvoření aplikace Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-110">Create an Azure AD B2C application.</span></span>
* <span data-ttu-id="345ff-111">Zaregistrujte dvě aplikace modul zásad.</span><span class="sxs-lookup"><span data-stu-id="345ff-111">Register two policy engine applications.</span></span>
* <span data-ttu-id="345ff-112">Nastavení klíčů.</span><span class="sxs-lookup"><span data-stu-id="345ff-112">Set up keys.</span></span>
* <span data-ttu-id="345ff-113">Nastavte hello starter pack.</span><span class="sxs-lookup"><span data-stu-id="345ff-113">Set up hello starter pack.</span></span>

### <a name="salesforce-setup"></a><span data-ttu-id="345ff-114">Instalační program služby Salesforce</span><span class="sxs-lookup"><span data-stu-id="345ff-114">Salesforce setup</span></span>

<span data-ttu-id="345ff-115">V tomto článku předpokládáme, že jste již dokončili hello následující:</span><span class="sxs-lookup"><span data-stu-id="345ff-115">In this article, we assume that you have already done hello following:</span></span>

* <span data-ttu-id="345ff-116">Registraci účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-116">Signed up for a Salesforce account.</span></span> <span data-ttu-id="345ff-117">Můžete si zaregistrovat [bezplatný účet Developer Edition](https://developer.salesforce.com/signup).</span><span class="sxs-lookup"><span data-stu-id="345ff-117">You can sign up for a [free Developer Edition account](https://developer.salesforce.com/signup).</span></span>
* <span data-ttu-id="345ff-118">[Nastavit Moje domény](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) pro vaši organizaci Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-118">[Set up a My Domain](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) for your Salesforce organization.</span></span>

## <a name="set-up-salesforce-so-users-can-federate"></a><span data-ttu-id="345ff-119">Nastavení Salesforce, aby uživatelé mohli Federovat</span><span class="sxs-lookup"><span data-stu-id="345ff-119">Set up Salesforce so users can federate</span></span>

<span data-ttu-id="345ff-120">toohelp Azure AD B2C komunikovat s Salesforce, potřebujete adresu URL metadat tooget hello Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-120">toohelp Azure AD B2C communicate with Salesforce, you need tooget hello Salesforce metadata URL.</span></span>

### <a name="set-up-salesforce-as-an-identity-provider"></a><span data-ttu-id="345ff-121">Nastavení Salesforce jako zprostředkovatel identity</span><span class="sxs-lookup"><span data-stu-id="345ff-121">Set up Salesforce as an identity provider</span></span>

> [!NOTE]
> <span data-ttu-id="345ff-122">V tomto článku předpokládáme, že používáte [Salesforce Lightning prostředí](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span><span class="sxs-lookup"><span data-stu-id="345ff-122">In this article, we assume you are using [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span></span>

1. <span data-ttu-id="345ff-123">[Přihlaste se tooSalesforce](https://login.salesforce.com/).</span><span class="sxs-lookup"><span data-stu-id="345ff-123">[Sign in tooSalesforce](https://login.salesforce.com/).</span></span> 
2. <span data-ttu-id="345ff-124">Na hello levé nabídce v části **nastavení**, rozbalte položku **Identity**a potom klikněte na **zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="345ff-124">On hello left menu, under **Settings**, expand **Identity**, and then click **Identity Provider**.</span></span>
3. <span data-ttu-id="345ff-125">Klikněte na tlačítko **povolit zprostředkovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="345ff-125">Click **Enable Identity Provider**.</span></span>
4. <span data-ttu-id="345ff-126">V části **certifikátu vyberte hello**, vyberte certifikát hello chcete Salesforce toouse toocommunicate s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-126">Under **Select hello certificate**, select hello certificate you want Salesforce toouse toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="345ff-127">(Můžete hello výchozí certifikát.) Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="345ff-127">(You can use hello default certificate.) Click **Save**.</span></span> 

### <a name="create-a-connected-app-in-salesforce"></a><span data-ttu-id="345ff-128">Vytvoření připojené aplikaci v Salesforce</span><span class="sxs-lookup"><span data-stu-id="345ff-128">Create a connected app in Salesforce</span></span>

1. <span data-ttu-id="345ff-129">Na hello **zprostředkovatele Identity** stránky, přejděte příliš**poskytovatelé služeb**.</span><span class="sxs-lookup"><span data-stu-id="345ff-129">On hello **Identity Provider** page, go too**Service Providers**.</span></span>
2. <span data-ttu-id="345ff-130">Klikněte na tlačítko **poskytovatelé služeb jsou teď vytvořené prostřednictvím připojené aplikace. Kliknutím sem.**</span><span class="sxs-lookup"><span data-stu-id="345ff-130">Click **Service Providers are now created via Connected Apps. Click here.**</span></span>
3. <span data-ttu-id="345ff-131">V části **základní informace**, zadejte hello požadované hodnoty pro připojené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="345ff-131">Under **Basic Information**,  enter hello required values for your connected app.</span></span>
4. <span data-ttu-id="345ff-132">V části **nastavení webové aplikace**, vyberte hello **povolit SAML** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="345ff-132">Under **Web App Settings**, select hello **Enable SAML** check box.</span></span>
5. <span data-ttu-id="345ff-133">V hello **Entity ID** pole, zadejte následující adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-133">In hello **Entity ID** field, enter hello following URL.</span></span> <span data-ttu-id="345ff-134">Ujistěte se, že nahradíte hodnotu hello `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="345ff-134">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. <span data-ttu-id="345ff-135">V hello **adresa URL služby ACS** pole, zadejte následující adresu URL hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-135">In hello **ACS URL** field, enter hello following URL.</span></span> <span data-ttu-id="345ff-136">Ujistěte se, že nahradíte hodnotu hello `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="345ff-136">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. <span data-ttu-id="345ff-137">Ponechte hello výchozí hodnoty pro všechna ostatní nastavení.</span><span class="sxs-lookup"><span data-stu-id="345ff-137">Leave hello default values for all other settings.</span></span>
8. <span data-ttu-id="345ff-138">Posuňte se toohello dolní části seznamu hello a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="345ff-138">Scroll toohello bottom of hello list, and then click **Save**.</span></span>

### <a name="get-hello-metadata-url"></a><span data-ttu-id="345ff-139">Získat adresu URL metadat hello</span><span class="sxs-lookup"><span data-stu-id="345ff-139">Get hello metadata URL</span></span>

1. <span data-ttu-id="345ff-140">Na stránce Přehled hello připojené aplikaci, klikněte na tlačítko **spravovat**.</span><span class="sxs-lookup"><span data-stu-id="345ff-140">On hello overview page of your connected app, click **Manage**.</span></span>
2. <span data-ttu-id="345ff-141">Zkopírujte hodnotu hello **koncový bod zjišťování metadat**a pak ho uložte.</span><span class="sxs-lookup"><span data-stu-id="345ff-141">Copy hello value for **Metadata Discovery Endpoint**, and then save it.</span></span> <span data-ttu-id="345ff-142">Použijete ho později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="345ff-142">You'll use it later in this article.</span></span>

### <a name="set-up-salesforce-users-toofederate"></a><span data-ttu-id="345ff-143">Nastavení Salesforce uživatelé toofederate</span><span class="sxs-lookup"><span data-stu-id="345ff-143">Set up Salesforce users toofederate</span></span>

1. <span data-ttu-id="345ff-144">Na hello **spravovat** stránky připojené aplikace, přejděte příliš**profily**.</span><span class="sxs-lookup"><span data-stu-id="345ff-144">On hello **Manage** page of your connected app, go too**Profiles**.</span></span>
2. <span data-ttu-id="345ff-145">Klikněte na tlačítko **spravovat profily**.</span><span class="sxs-lookup"><span data-stu-id="345ff-145">Click **Manage Profiles**.</span></span>
3. <span data-ttu-id="345ff-146">Vyberte profily hello (nebo skupiny uživatelů), které chcete toofederate s Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-146">Select hello profiles (or groups of users) that you want toofederate with Azure AD B2C.</span></span> <span data-ttu-id="345ff-147">Jako správce systému, vyberte hello **správce systému** zaškrtnutí políčka, takže můžete vytvořit federaci s použitím účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-147">As a system administrator, select hello **System Administrator** check box, so that you can federate by using your Salesforce account.</span></span>

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a><span data-ttu-id="345ff-148">Generovat podpisový certifikát pro Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="345ff-148">Generate a signing certificate for Azure AD B2C</span></span>

<span data-ttu-id="345ff-149">Odeslání požadavků toobe nutné tooSalesforce podepsané pomocí Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-149">Requests sent tooSalesforce need toobe signed by Azure AD B2C.</span></span> <span data-ttu-id="345ff-150">toogenerate podpisový certifikát, otevřete prostředí Azure PowerShell a spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-150">toogenerate a signing certificate, open Azure PowerShell, and then run hello following commands.</span></span>

> [!NOTE]
> <span data-ttu-id="345ff-151">Zkontrolujte, zda je aktualizovat hello klienta jméno a heslo v horní dva řádky hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-151">Make sure that you update hello tenant name and password in hello top two lines.</span></span>

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a><span data-ttu-id="345ff-152">Přidat hello SAML podpisový certifikát tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="345ff-152">Add hello SAML signing certificate tooAzure AD B2C</span></span>

<span data-ttu-id="345ff-153">Odešlete hello podpisový certifikát tooyour Azure AD B2C klienta:</span><span class="sxs-lookup"><span data-stu-id="345ff-153">Upload hello signing certificate tooyour Azure AD B2C tenant:</span></span> 

1. <span data-ttu-id="345ff-154">Přejděte klienta tooyour Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="345ff-154">Go tooyour Azure AD B2C tenant.</span></span> <span data-ttu-id="345ff-155">Klikněte na tlačítko **nastavení** > **Identity rozhraní Framework** > **zásad klíče**.</span><span class="sxs-lookup"><span data-stu-id="345ff-155">Click **Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
2. <span data-ttu-id="345ff-156">Klikněte na tlačítko **+ přidat**a pak:</span><span class="sxs-lookup"><span data-stu-id="345ff-156">Click **+Add**, and then:</span></span>
    1. <span data-ttu-id="345ff-157">Klikněte na tlačítko **možnosti** > **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="345ff-157">Click **Options** > **Upload**.</span></span>
    2. <span data-ttu-id="345ff-158">Zadejte **název** (například SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="345ff-158">Enter a **Name** (for example, SAMLSigningCert).</span></span> <span data-ttu-id="345ff-159">Předpona Hello *B2C_1A_* se automaticky přidá toohello název vašeho klíče.</span><span class="sxs-lookup"><span data-stu-id="345ff-159">hello prefix *B2C_1A_* is automatically added toohello name of your key.</span></span>
    3. <span data-ttu-id="345ff-160">tooselect certifikát, vyberte **nahrát ovládacího prvku se souborovým**.</span><span class="sxs-lookup"><span data-stu-id="345ff-160">tooselect your certificate, select **upload file control**.</span></span> 
    4. <span data-ttu-id="345ff-161">Zadejte heslo hello certifikátu, který nastavíte ve skriptu PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-161">Enter hello certificate's password that you set in hello PowerShell script.</span></span>
3. <span data-ttu-id="345ff-162">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="345ff-162">Click **Create**.</span></span>
4. <span data-ttu-id="345ff-163">Ověřte, že jste vytvořili klíč (například B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="345ff-163">Verify that you've created a key (for example, B2C_1A_SAMLSigningCert).</span></span> <span data-ttu-id="345ff-164">Poznamenejte si hello celý název (včetně *B2C_1A_*).</span><span class="sxs-lookup"><span data-stu-id="345ff-164">Take note of hello full name (including *B2C_1A_*).</span></span> <span data-ttu-id="345ff-165">Klíč toothis později v hello zásady budou vztahovat.</span><span class="sxs-lookup"><span data-stu-id="345ff-165">You will refer toothis key later in hello policy.</span></span>

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a><span data-ttu-id="345ff-166">Vytvoření hello poskytovatele deklarací identity Salesforce SAML v základní zásady</span><span class="sxs-lookup"><span data-stu-id="345ff-166">Create hello Salesforce SAML claims provider in your base policy</span></span>

<span data-ttu-id="345ff-167">Je nutné toodefine Salesforce jako poskytovatele deklarací identity tak moct uživatel přihlásit pomocí služby Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-167">You need toodefine Salesforce as a claims provider so users can sign in by using Salesforce.</span></span> <span data-ttu-id="345ff-168">Jinými slovy budete potřebovat hello koncový bod toospecify, který Azure AD B2C bude komunikovat se službou.</span><span class="sxs-lookup"><span data-stu-id="345ff-168">In other words, you need toospecify hello endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="345ff-169">koncový bod Hello bude *poskytují* sadu *deklarace identity* , Azure AD B2C používá tooverify, který byl ověřen konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="345ff-169">hello endpoint will *provide* a set of *claims* that Azure AD B2C uses tooverify that a specific user has authenticated.</span></span> <span data-ttu-id="345ff-170">toodo, přidejte `<ClaimsProvider>` pro služby Salesforce v souboru rozšíření hello zásad:</span><span class="sxs-lookup"><span data-stu-id="345ff-170">toodo this, add a `<ClaimsProvider>` for Salesforce in hello extension file of your policy:</span></span>

1. <span data-ttu-id="345ff-171">V pracovním adresáři otevřete soubor rozšíření hello (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="345ff-171">In your working directory, open hello extension file (TrustFrameworkExtensions.xml).</span></span>
2. <span data-ttu-id="345ff-172">Najde hello `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="345ff-172">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="345ff-173">Pokud neexistuje, vytvořte ho v části hello kořenový uzel.</span><span class="sxs-lookup"><span data-stu-id="345ff-173">If it does not exist, create it under hello root node.</span></span>
3. <span data-ttu-id="345ff-174">Přidejte nový `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="345ff-174">Add a new `<ClaimsProvider>`:</span></span>

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

<span data-ttu-id="345ff-175">V části hello `<ClaimsProvider>` uzlu:</span><span class="sxs-lookup"><span data-stu-id="345ff-175">Under hello `<ClaimsProvider>` node:</span></span>

1. <span data-ttu-id="345ff-176">Změnit hodnotu hello pro `<Domain>` tooa jedinečnou hodnotu, která rozlišuje `<ClaimsProvider>` od jiných poskytovatelů identit.</span><span class="sxs-lookup"><span data-stu-id="345ff-176">Change hello value for `<Domain>` tooa unique value that distinguishes `<ClaimsProvider>` from other identity providers.</span></span>
2. <span data-ttu-id="345ff-177">Aktualizujte hello hodnotu pro `<DisplayName>` tooa zobrazovaný název pro hello deklarací zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="345ff-177">Update hello value for `<DisplayName>` tooa display name for hello claims provider.</span></span> <span data-ttu-id="345ff-178">Tato hodnota není v současné době používá.</span><span class="sxs-lookup"><span data-stu-id="345ff-178">Currently, this value is not used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="345ff-179">Aktualizovat profil technické hello</span><span class="sxs-lookup"><span data-stu-id="345ff-179">Update hello technical profile</span></span>

<span data-ttu-id="345ff-180">tooget tokenu SAML ze služby Salesforce, definujte hello protokoly, Azure AD B2C použijete toocommunicate službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="345ff-180">tooget a SAML token from Salesforce, define hello protocols that Azure AD B2C will use toocommunicate with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="345ff-181">K tomu hello `<TechnicalProfile>` element `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="345ff-181">Do this in hello `<TechnicalProfile>` element of `<ClaimsProvider>`:</span></span>

1. <span data-ttu-id="345ff-182">Aktualizovat hello ID hello `<TechnicalProfile>` uzlu.</span><span class="sxs-lookup"><span data-stu-id="345ff-182">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="345ff-183">Toto ID je použité toorefer toothis technické profil z různých částí hello zásad.</span><span class="sxs-lookup"><span data-stu-id="345ff-183">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
2. <span data-ttu-id="345ff-184">Aktualizujte hello hodnotu pro `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="345ff-184">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="345ff-185">Tato hodnota se zobrazí na hello přihlašovací tlačítko na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="345ff-185">This value is displayed on hello sign-in button on your sign-in page.</span></span>
3. <span data-ttu-id="345ff-186">Aktualizujte hello hodnotu pro `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="345ff-186">Update hello value for `<Description>`.</span></span>
4. <span data-ttu-id="345ff-187">Salesforce používá protokol hello SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="345ff-187">Salesforce uses hello SAML 2.0 protocol.</span></span> <span data-ttu-id="345ff-188">Ujistěte se, že hello hodnotu pro `<Protocol>` je **typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="345ff-188">Ensure that hello value for `<Protocol>` is **SAML2**.</span></span>

<span data-ttu-id="345ff-189">Aktualizace hello `<Metadata>` část v hello předcházející XML tooreflect hello nastavení pro konkrétní účtu Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-189">Update hello `<Metadata>` section in hello preceding XML tooreflect hello settings for your specific Salesforce account.</span></span> <span data-ttu-id="345ff-190">V hello XML aktualizujte hodnoty metadata hello:</span><span class="sxs-lookup"><span data-stu-id="345ff-190">In hello XML, update hello metadata values:</span></span>

1. <span data-ttu-id="345ff-191">Aktualizujte hodnotu hello `<Item Key="PartnerEntity">` s Salesforce adresu URL metadat hello jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="345ff-191">Update hello value of `<Item Key="PartnerEntity">` with hello Salesforce metadata URL you copied earlier.</span></span> <span data-ttu-id="345ff-192">Má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="345ff-192">It has hello following format:</span></span> 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. <span data-ttu-id="345ff-193">V hello `<CryptographicKeys>` část aktualizace hello hodnota pro obě instance `StorageReferenceId` toohello název klíče hello podpisového certifikátu (například B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="345ff-193">In hello `<CryptographicKeys>` section, update hello value for both instances of `StorageReferenceId` toohello name of hello key of your signing certificate (for example, B2C_1A_SAMLSigningCert).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="345ff-194">Nahrát soubor hello rozšíření pro ověření</span><span class="sxs-lookup"><span data-stu-id="345ff-194">Upload hello extension file for verification</span></span>

<span data-ttu-id="345ff-195">Zásady je nyní nakonfigurována tak, aby Azure AD B2C zná jak toocommunicate s Salesforce.</span><span class="sxs-lookup"><span data-stu-id="345ff-195">Your policy is now configured so that Azure AD B2C knows how toocommunicate with Salesforce.</span></span> <span data-ttu-id="345ff-196">Zkuste odeslat soubor hello rozšíření zásad, že nejsou k dispozici všechny problémy, pokud tooverify.</span><span class="sxs-lookup"><span data-stu-id="345ff-196">Try uploading hello extension file of your policy, tooverify that there aren't any issues so far.</span></span> <span data-ttu-id="345ff-197">tooupload hello rozšíření soubor zásad:</span><span class="sxs-lookup"><span data-stu-id="345ff-197">tooupload hello extension file of your policy:</span></span>

1. <span data-ttu-id="345ff-198">Ve vašem klientu Azure AD B2C, přejděte toohello **všechny zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="345ff-198">In your Azure AD B2C tenant, go toohello **All Policies** blade.</span></span>
2. <span data-ttu-id="345ff-199">Vyberte hello **přepsat zásady hello, pokud existuje** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="345ff-199">Select hello **Overwrite hello policy if it exists** check box.</span></span>
3. <span data-ttu-id="345ff-200">Nahrajte soubor rozšíření hello (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="345ff-200">Upload hello extension file (TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="345ff-201">Ujistěte se, že neselže ověření.</span><span class="sxs-lookup"><span data-stu-id="345ff-201">Ensure that it doesn't fail validation.</span></span>

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a><span data-ttu-id="345ff-202">Zaregistrovat hello Salesforce SAML deklarace identity zprostředkovatele tooa uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="345ff-202">Register hello Salesforce SAML claims provider tooa user journey</span></span>

<span data-ttu-id="345ff-203">Dál přidejte hello tooone poskytovatele identity Salesforce SAML z vaší uživatelské cesty.</span><span class="sxs-lookup"><span data-stu-id="345ff-203">Next, add hello Salesforce SAML identity provider tooone of your user journeys.</span></span> <span data-ttu-id="345ff-204">V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici na všech hello stránek registrace nebo přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="345ff-204">At this point, hello identity provider has been set up, but it’s not available on any of hello user sign-up or sign-in pages.</span></span> <span data-ttu-id="345ff-205">tooadd hello identity tooa přihlašovací stránka zprostředkovatele, nejprve vytvořte duplicitní existující uživatele cesty šablony.</span><span class="sxs-lookup"><span data-stu-id="345ff-205">tooadd hello identity provider tooa sign-in page, first, create a duplicate of an existing template user journey.</span></span> <span data-ttu-id="345ff-206">Potom upravte hello šablony tak, aby je také hello poskytovatele identit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="345ff-206">Then, modify hello template so that it also has hello Azure AD identity provider.</span></span>

1. <span data-ttu-id="345ff-207">Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="345ff-207">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="345ff-208">Najde hello `<UserJourneys>` elementu a kopírování hello celý `<UserJourney>` hodnoty, včetně Id = "SignUpOrSignIn".</span><span class="sxs-lookup"><span data-stu-id="345ff-208">Find hello `<UserJourneys>` element, and then copy hello entire `<UserJourney>` value, including Id=”SignUpOrSignIn”.</span></span>
3. <span data-ttu-id="345ff-209">Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="345ff-209">Open hello extension file (for example, TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="345ff-210">Najde hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="345ff-210">Find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="345ff-211">Pokud hello element neexistuje, vytvořte ji.</span><span class="sxs-lookup"><span data-stu-id="345ff-211">If hello element doesn't exist, create one.</span></span>
4. <span data-ttu-id="345ff-212">Vložení hello celý zkopírovat `<UserJourney>` jako podřízenou hello `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="345ff-212">Paste hello entire copied `<UserJourney>` as a child of hello `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="345ff-213">Přejmenujte hello ID hello nové `<UserJourney>` (například SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="345ff-213">Rename hello ID of hello new `<UserJourney>` (for example, SignUpOrSignUsingContoso).</span></span>

### <a name="display-hello-identity-provider-button"></a><span data-ttu-id="345ff-214">Tlačítko zprostředkovatele identity hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="345ff-214">Display hello identity provider button</span></span>

<span data-ttu-id="345ff-215">Hello `<ClaimsProviderSelection>` element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="345ff-215">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="345ff-216">Přidáním `<ClaimsProviderSelection>` element pro Salesforce nové tlačítko se zobrazí, když uživatel přejde toothis stránky.</span><span class="sxs-lookup"><span data-stu-id="345ff-216">By adding an `<ClaimsProviderSelection>` element for Salesforce, a new button appears when a user goes toothis page.</span></span> <span data-ttu-id="345ff-217">tlačítko zprostředkovatele identity toodisplay hello:</span><span class="sxs-lookup"><span data-stu-id="345ff-217">toodisplay hello identity provider button:</span></span>

1. <span data-ttu-id="345ff-218">V hello `<UserJourney>` , kterou jste vytvořili, najde hello `<OrchestrationStep>` s `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="345ff-218">In hello `<UserJourney>` that you created, find hello `<OrchestrationStep>` with `Order="1"`.</span></span>
2. <span data-ttu-id="345ff-219">Přidejte následující XML hello:</span><span class="sxs-lookup"><span data-stu-id="345ff-219">Add hello following XML:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. <span data-ttu-id="345ff-220">Nastavit `TargetClaimsExchangeId` tooa logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="345ff-220">Set `TargetClaimsExchangeId` tooa logical value.</span></span> <span data-ttu-id="345ff-221">Doporučujeme následující hello stejné konvence jako jiné (například  *\[ClaimProviderName\]Exchange*).</span><span class="sxs-lookup"><span data-stu-id="345ff-221">We recommend following hello same convention as others (for example, *\[ClaimProviderName\]Exchange*).</span></span>

### <a name="link-hello-identity-provider-button-tooan-action"></a><span data-ttu-id="345ff-222">Odkaz akce tooan tlačítka zprostředkovatele identity hello</span><span class="sxs-lookup"><span data-stu-id="345ff-222">Link hello identity provider button tooan action</span></span>

<span data-ttu-id="345ff-223">Nyní když máte tlačítko zprostředkovatele identity na místě, propojte tooan akce.</span><span class="sxs-lookup"><span data-stu-id="345ff-223">Now that you have an identity provider button in place, link it tooan action.</span></span> <span data-ttu-id="345ff-224">V takovém případě není hello akce pro Azure AD B2C toocommunicate s Salesforce tooreceive tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="345ff-224">In this case, hello action is for Azure AD B2C toocommunicate with Salesforce tooreceive a SAML token.</span></span> <span data-ttu-id="345ff-225">To provedete pomocí propojení hello technické profil pro vaše Salesforce SAML poskytovatele deklarací identity:</span><span class="sxs-lookup"><span data-stu-id="345ff-225">You can do this by linking hello technical profile for your Salesforce SAML claims provider:</span></span>

1. <span data-ttu-id="345ff-226">V hello `<UserJourney>` uzlu, najít hello `<OrchestrationStep>` s `Order="2"`.</span><span class="sxs-lookup"><span data-stu-id="345ff-226">In hello `<UserJourney>` node, find hello `<OrchestrationStep>` with `Order="2"`.</span></span>
2. <span data-ttu-id="345ff-227">Přidejte následující XML hello:</span><span class="sxs-lookup"><span data-stu-id="345ff-227">Add hello following XML:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. <span data-ttu-id="345ff-228">Aktualizace `Id` toohello, stejnou který jste dříve použít pro hodnotu `TargetClaimsExchangeId`.</span><span class="sxs-lookup"><span data-stu-id="345ff-228">Update `Id` toohello same value that you used earlier for `TargetClaimsExchangeId`.</span></span>
4. <span data-ttu-id="345ff-229">Aktualizace `TechnicalProfileReferenceId` toohello `Id` z hello technické profilu, kterou jste vytvořili dříve (například ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="345ff-229">Update `TechnicalProfileReferenceId` toohello `Id` of hello technical profile you created earlier (for example, ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="345ff-230">Nahrát soubor aktualizované rozšíření hello</span><span class="sxs-lookup"><span data-stu-id="345ff-230">Upload hello updated extension file</span></span>

<span data-ttu-id="345ff-231">Dokončení změny souboru rozšíření bylo hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-231">You are done modifying hello extension file.</span></span> <span data-ttu-id="345ff-232">Uložte a nahrajte tento soubor.</span><span class="sxs-lookup"><span data-stu-id="345ff-232">Save and upload this file.</span></span> <span data-ttu-id="345ff-233">Ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="345ff-233">Ensure that all validations succeed.</span></span>

### <a name="update-hello-relying-party-file"></a><span data-ttu-id="345ff-234">Aktualizovat hello předávající strany soubor</span><span class="sxs-lookup"><span data-stu-id="345ff-234">Update hello relying party file</span></span>

<span data-ttu-id="345ff-235">Potom aktualizujte hello předávající stranu soubor, který iniciuje cesty hello uživatele, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="345ff-235">Next, update hello relying party (RP) file that initiates hello user journey that you created:</span></span>

1. <span data-ttu-id="345ff-236">Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="345ff-236">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="345ff-237">Potom přejmenujte ji (například SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="345ff-237">Then, rename it (for example, SignUpOrSignInWithAAD.xml).</span></span>
2. <span data-ttu-id="345ff-238">Otevřete hello nový soubor a aktualizace hello `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="345ff-238">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="345ff-239">Toto je název hello zásad (například SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="345ff-239">This is hello name of your policy (for example, SignUpOrSignInWithAAD).</span></span>
3. <span data-ttu-id="345ff-240">Upravit hello `ReferenceId` atribut `<DefaultUserJourney>` toomatch hello `Id` hello nové uživatele cesty vytvořený (například SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="345ff-240">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello `Id` of hello new user journey that you created (for example, SignUpOrSignUsingContoso).</span></span>
4. <span data-ttu-id="345ff-241">Uložte změny a pak nahrajte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="345ff-241">Save your changes, and then upload hello file.</span></span>

## <a name="test-and-troubleshoot"></a><span data-ttu-id="345ff-242">Testování a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="345ff-242">Test and troubleshoot</span></span>

<span data-ttu-id="345ff-243">hello tootest vlastní se zásady, které jste právě nahráli, zvolte v hello portál Azure, přejděte toohello okna zásady a pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="345ff-243">tootest hello custom policy that you just uploaded, in hello Azure portal, go toohello policy blade, and then click **Run now**.</span></span> <span data-ttu-id="345ff-244">Pokud se nezdaří, najdete v části [řešení potíží se zásadami vlastní](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="345ff-244">If it fails, see [Troubleshoot custom policies](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="345ff-245">Další kroky</span><span class="sxs-lookup"><span data-stu-id="345ff-245">Next steps</span></span>

<span data-ttu-id="345ff-246">Poskytnutí zpětné vazby příliš[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="345ff-246">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
