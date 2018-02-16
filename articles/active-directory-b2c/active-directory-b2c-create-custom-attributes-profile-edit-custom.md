---
title: "Azure Active Directory B2C: Přidat vlastní atributy do vlastní zásady a použít v profilu upravit | Microsoft Docs"
description: "Návod na pomocí vlastnosti rozšíření, vlastní atributy a jejich včetně v uživatelském rozhraní"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 0d4ee064c15c914eea7353900c6bb5a77b3e3b3b
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="e8046-103">Azure Active Directory B2C: Vytváření a používání vlastních atributů v vlastního profilu upravit zásady</span><span class="sxs-lookup"><span data-stu-id="e8046-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="e8046-104">V tomto článku můžete vytvořit vlastní atribut v adresáři Azure AD B2C a používat tento nový atribut jako vlastních deklarací identity v cesty uživatelské úpravy profilu.</span><span class="sxs-lookup"><span data-stu-id="e8046-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in the profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8046-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e8046-105">Prerequisites</span></span>

<span data-ttu-id="e8046-106">Proveďte kroky v následujícím článku [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="e8046-106">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="e8046-107">Použití vlastních atributů ke shromažďování informací o zákazníky v Azure Active Directory B2C pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="e8046-107">Use custom attributes to collect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="e8046-108">Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu atributů: zadané jméno, příjmení, Město, PSČ, userPrincipalName, atd.  Často je potřeba vytvořit vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="e8046-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need to create your own attributes.</span></span>  <span data-ttu-id="e8046-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e8046-109">For example:</span></span>
* <span data-ttu-id="e8046-110">Zákazník směřujících aplikací musí zachovat atribut jako je například "LoyaltyNumber."</span><span class="sxs-lookup"><span data-stu-id="e8046-110">A customer-facing application needs to persist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="e8046-111">Zprostředkovatele identity má jedinečný identifikátor uživatele, musí být uložena jako je například "uniqueUserGUID"."</span><span class="sxs-lookup"><span data-stu-id="e8046-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="e8046-112">Vlastní uživatelské cesty musí zachovat stav uživatele, jako je například "migrationStatus."</span><span class="sxs-lookup"><span data-stu-id="e8046-112">A custom user journey needs to persist the state of user such as "migrationStatus."</span></span>

<span data-ttu-id="e8046-113">S Azure AD B2C můžete rozšířit sadu atributů uložené u každého uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="e8046-113">With Azure AD B2C, you can extend the set of attributes stored on each user account.</span></span> <span data-ttu-id="e8046-114">Můžete také číst a zapsat pomocí těchto atributů [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e8046-114">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="e8046-115">Vlastnosti rozšíření rozšířit schéma uživatelské objekty v adresáři.</span><span class="sxs-lookup"><span data-stu-id="e8046-115">Extension properties extend the schema of the user objects in the directory.</span></span>  <span data-ttu-id="e8046-116">Vlastnost rozšíření podmínky, vlastních atributů a vlastních deklarací identity označují totéž v kontextu tohoto článku a název se liší v závislosti na kontextu (aplikace, objekt, zásady).</span><span class="sxs-lookup"><span data-stu-id="e8046-116">The terms extension property, custom attribute and custom claim refer to the same thing in the context of this article and the name varies depending on the context (application, object, policy).</span></span>

<span data-ttu-id="e8046-117">Vlastnosti rozšíření se dají registrovat jenom na objekt aplikace i v případě, že mohou obsahovat data pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8046-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="e8046-118">Vlastnost je připojený k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e8046-118">The property is attached to the application.</span></span> <span data-ttu-id="e8046-119">Objekt aplikace musí mít udělen přístup k zápisu do zaregistrovat ve vlastnosti rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e8046-119">The Application object must be granted write access to register an extension property.</span></span> <span data-ttu-id="e8046-120">K žádnému jednotlivému objektu může být napsán 100 – vlastnosti rozšíření (v rámci všech typů a všechny aplikace).</span><span class="sxs-lookup"><span data-stu-id="e8046-120">100 Extension properties (across ALL types and ALL applications) can be written to any single object.</span></span> <span data-ttu-id="e8046-121">Vlastnosti rozšíření jsou přidány do adresáře typ cíle a že bude okamžitě dostupný v adresáři klienta Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="e8046-121">Extension properties are added to the target directory type and becomes immediately accessible in the Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="e8046-122">Pokud se aplikaci odstranit, budou odebrány také tyto vlastnosti rozšíření spolu s daty v nich obsažené pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8046-122">If the application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="e8046-123">Pokud ve vlastnosti rozšíření je odstraní aplikaci, odeberou se v cílové directory objekty a hodnoty odstranit.</span><span class="sxs-lookup"><span data-stu-id="e8046-123">If an extension property is deleted by the Application, it is removed on the target directory objects, and the values deleted.</span></span>

<span data-ttu-id="e8046-124">Vlastnosti rozšíření existují pouze v kontextu registrovaný aplikace v klientovi.</span><span class="sxs-lookup"><span data-stu-id="e8046-124">Extension properties exist only in the context of a registered  Application in the tenant.</span></span> <span data-ttu-id="e8046-125">Id objektu této aplikace musí být součástí TechnicalProfile, který ho použít.</span><span class="sxs-lookup"><span data-stu-id="e8046-125">The object id of that Application must be included in the TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="e8046-126">Adresář Azure AD B2C obvykle zahrnuje webovou aplikaci s názvem `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="e8046-126">The Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="e8046-127">Tato aplikace je používána především pomocí předdefinovaných zásad b2c pro vlastní deklarace vytvořené prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8046-127">This application is primarily used by the b2c built-in  policies for the custom claims created via the Azure portal.</span></span>  <span data-ttu-id="e8046-128">Pomocí této aplikace k registraci rozšíření pro vlastní zásady b2c se doporučuje jenom pro zkušené uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8046-128">Using this application to register extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="e8046-129">Pokyny pro tento jsou uvedené v části Další kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e8046-129">Instructions for this are included in the Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-to-store-the-extension-properties"></a><span data-ttu-id="e8046-130">Vytvoření nové aplikace a uložit vlastnosti rozšíření</span><span class="sxs-lookup"><span data-stu-id="e8046-130">Creating a new application to store the extension properties</span></span>

1. <span data-ttu-id="e8046-131">Spustí relaci prohlížeče a přejděte do [portál Azure](https://portal.azure.com) a přihlaste se pomocí pověření správce adresáře B2C, které chcete nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="e8046-131">Open a browsing session and navigate to the [Azure portal](https://portal.azure.com) and sign in with administrative credentials of the B2C Directory you wish to configure.</span></span>
1. <span data-ttu-id="e8046-132">Klikněte na tlačítko **Azure Active Directory** v levé navigační nabídce.</span><span class="sxs-lookup"><span data-stu-id="e8046-132">Click **Azure Active Directory** on the left navigation menu.</span></span> <span data-ttu-id="e8046-133">Budete muset najít tak, že vyberete další služby >.</span><span class="sxs-lookup"><span data-stu-id="e8046-133">You may need to find it by selecting More services>.</span></span>
1. <span data-ttu-id="e8046-134">Vyberte **registrace aplikace** a klikněte na tlačítko **novou registraci aplikace**</span><span class="sxs-lookup"><span data-stu-id="e8046-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="e8046-135">Zadejte následující doporučené položky:</span><span class="sxs-lookup"><span data-stu-id="e8046-135">Provide the following recommended entries:</span></span>
  * <span data-ttu-id="e8046-136">Zadejte název webové aplikace: **WebApp. GraphAPI DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="e8046-136">Specify a name for the web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="e8046-137">Typ aplikace: webové aplikace nebo rozhraní API</span><span class="sxs-lookup"><span data-stu-id="e8046-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="e8046-138">URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions přihlašování</span><span class="sxs-lookup"><span data-stu-id="e8046-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="e8046-139">Vyberte ** vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e8046-139">Select **Create.</span></span> <span data-ttu-id="e8046-140">Úspěšné dokončení se zobrazí v **oznámení**</span><span class="sxs-lookup"><span data-stu-id="e8046-140">Successful completion appears in the **notifications**</span></span>
1. <span data-ttu-id="e8046-141">Vyberte nově vytvořenou webovou aplikaci: **WebApp. GraphAPI DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="e8046-141">Select the newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="e8046-142">Vyberte nastavení: **požadovaná oprávnění**</span><span class="sxs-lookup"><span data-stu-id="e8046-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="e8046-143">Vybrat rozhraní API **Windows Azure Active Directory**</span><span class="sxs-lookup"><span data-stu-id="e8046-143">Select API **Windows Azure Active Directory**</span></span>
1. <span data-ttu-id="e8046-144">Oprávnění aplikací, zaškrtněte: **pro čtení a zápis dat adresáře**, a **uložit**</span><span class="sxs-lookup"><span data-stu-id="e8046-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="e8046-145">Zvolte **udělit oprávnění** a potvrďte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="e8046-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="e8046-146">Zkopírujte do schránky a uložte následující identifikátory z webové aplikace. GraphAPI DirectoryExtensions > Nastavení > Vlastnosti ></span><span class="sxs-lookup"><span data-stu-id="e8046-146">Copy to your clipboard and save the following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="e8046-147">**ID aplikace** .</span><span class="sxs-lookup"><span data-stu-id="e8046-147">**Application ID** .</span></span> <span data-ttu-id="e8046-148">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e8046-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="e8046-149">**ID objektu**.</span><span class="sxs-lookup"><span data-stu-id="e8046-149">**Object ID**.</span></span> <span data-ttu-id="e8046-150">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e8046-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a><span data-ttu-id="e8046-151">Úprava vaše vlastní zásady pro přidání ApplicationObjectId</span><span class="sxs-lookup"><span data-stu-id="e8046-151">Modifying your custom policy to add the ApplicationObjectId</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
><span data-ttu-id="e8046-152"><TechnicalProfile Id="AAD-Common"> Se označuje jako "běžné", protože jeho prvky jsou součástí a opakovaně používat ve všech Azure Active Directory TechnicalProfiles pomocí prvku: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="e8046-152">The <TechnicalProfile Id="AAD-Common"> is referred to as "common" because its elements are included in and reused in all the Azure Active Directory TechnicalProfiles by using the element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="e8046-153">Když TechnicalProfile zapíše první vlastnost nově vytvořený rozšíření, může docházet k chybě jednorázové.</span><span class="sxs-lookup"><span data-stu-id="e8046-153">When the TechnicalProfile writes for the first time to the newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="e8046-154">Vlastnost rozšíření se vytvoří při prvním se používá.</span><span class="sxs-lookup"><span data-stu-id="e8046-154">The extension property is created the first time it is used.</span></span>  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="e8046-155">Pomocí nové vlastnosti rozšíření / vlastní atribut cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="e8046-155">Using the new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="e8046-156">Otevřete úprav předávající Party(RP) soubor, který popisuje vaše zásady uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="e8046-156">Open the Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="e8046-157">Pokud jsou začínáte, může být vhodné vaší už nakonfigurované verzi souboru RP PolicyEdit stáhnout přímo z části vlastní zásady pro Azure B2C na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e8046-157">If you are starting out, it may be advisable to download your already configured version of the RP-PolicyEdit file directly from the Azure B2C Custom Policy section in the Azure portal.</span></span>  <span data-ttu-id="e8046-158">Alternativně můžete otevřete soubor XML ze složky úložiště.</span><span class="sxs-lookup"><span data-stu-id="e8046-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="e8046-159">Přidat vlastní deklarace identity `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="e8046-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="e8046-160">Zahrnutím vlastní deklarace identity v `<RelyingParty>` elementu, je jako parametr předaný UserJourney TechnicalProfiles a součástí token pro danou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e8046-160">By including the custom claim in the `<RelyingParty>` element, it is passed as a parameter to the UserJourney TechnicalProfiles and included in the token for the application.</span></span>
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. <span data-ttu-id="e8046-161">Přidejte definici deklarací identity na soubor zásad, rozšíření `TrustFrameworkExtensions.xml` uvnitř `<ClaimsSchema>` element, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="e8046-161">Add a claim definition to the Extension policy file  `TrustFrameworkExtensions.xml` inside the `<ClaimsSchema>` element as shown.</span></span>
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. <span data-ttu-id="e8046-162">Přidání stejné deklarace identity na soubor zásad základní definice `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="e8046-162">Add the same claim definition to the Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="e8046-163">Přidání `ClaimType` definice v základní a soubor rozšíření se obvykle nemusí, ale vzhledem k tomu, že další kroky přidá do TechnicalProfiles v základního souboru extension_loyaltyId, validátor zásad odmítnou nahrávání základního souboru bez.</span><span class="sxs-lookup"><span data-stu-id="e8046-163">Adding a `ClaimType` definition in both the base and the extensions file is normally not necessary, however since the next steps will add the extension_loyaltyId to TechnicalProfiles in the Base file, the policy validator will reject the upload of the base file without it.</span></span>
><span data-ttu-id="e8046-164">Může být užitečná pro sledování spuštění cesty uživatele s názvem "ProfileEdit" v souboru TrustFrameworkBase.xml.</span><span class="sxs-lookup"><span data-stu-id="e8046-164">It may be useful to trace the execution of the user journey named "ProfileEdit" in the TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="e8046-165">Vyhledejte uživatele cesty se stejným názvem ve svém editoru a pozorovat, že krok 5 Orchestration vyvolá TechnicalProfileReferenceID = "SelfAsserted-ProfileUpdate".</span><span class="sxs-lookup"><span data-stu-id="e8046-165">Search for the user journey of the same name in your editor and observe that Orchestration Step 5 invokes the TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="e8046-166">Hledání a zkontrolovat tento TechnicalProfile Seznamte se s toku.</span><span class="sxs-lookup"><span data-stu-id="e8046-166">Search and inspect this TechnicalProfile to familiarize yourself with the flow.</span></span>
5. <span data-ttu-id="e8046-167">Přidat loyaltyId jako vstupní a výstupní deklarací identity v TechnicalProfile "SelfAsserted-ProfileUpdate"</span><span class="sxs-lookup"><span data-stu-id="e8046-167">Add loyaltyId as input and output claim in the TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="e8046-168">Přidání deklarace identity v TechnicalProfile "AAD-UserWriteProfileUsingObjectId" udržení hodnoty deklarace identity ve vlastnosti rozšíření pro aktuálního uživatele v adresáři.</span><span class="sxs-lookup"><span data-stu-id="e8046-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" to persist the value of the claim in the extension property, for the current user in the directory.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. <span data-ttu-id="e8046-169">Přidání deklarace identity v TechnicalProfile "AAD-UserReadUsingObjectId" načíst hodnotu atributu rozšíření pokaždé, když se uživatel přihlásí.</span><span class="sxs-lookup"><span data-stu-id="e8046-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" to read the value of the extension attribute every time a user logs in.</span></span> <span data-ttu-id="e8046-170">Doposud byly změněny TechnicalProfiles toku pouze místní účty.</span><span class="sxs-lookup"><span data-stu-id="e8046-170">Thus far the TechnicalProfiles have been changed in the flow of local accounts only.</span></span>  <span data-ttu-id="e8046-171">Pokud nový atribut se požaduje toku účet sociálního/federovaný, je potřeba změnit jinou sadu TechnicalProfiles.</span><span class="sxs-lookup"><span data-stu-id="e8046-171">If the new attribute is desired in the flow of a social/federated account, a different set of TechnicalProfiles needs to be changed.</span></span> <span data-ttu-id="e8046-172">Najdete v části Další kroky.</span><span class="sxs-lookup"><span data-stu-id="e8046-172">See Next Steps.</span></span>

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
><span data-ttu-id="e8046-173">IncludeTechnicalProfile element přidá všechny elementy AAD běžné tento TechnicalProfile.</span><span class="sxs-lookup"><span data-stu-id="e8046-173">The IncludeTechnicalProfile element adds all the elements of AAD-Common to this TechnicalProfile.</span></span>

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="e8046-174">Otestovat vlastní zásady pomocí "Spustit nyní"</span><span class="sxs-lookup"><span data-stu-id="e8046-174">Test the custom policy using "Run Now"</span></span>
1. <span data-ttu-id="e8046-175">Otevřete **okno Azure AD B2C** a přejděte do **Identity rozhraní Framework > vlastní zásady**.</span><span class="sxs-lookup"><span data-stu-id="e8046-175">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="e8046-176">Vyberte vlastní zásady, který jste nahráli a klikněte na **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e8046-176">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
1. <span data-ttu-id="e8046-177">Nyní byste měli mít zaregistrovat pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="e8046-177">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="e8046-178">Id token odeslána zpět do vaší aplikace obsahuje nové vlastnosti rozšíření jako vlastních deklarací identity extension_loyaltyId před sebou.</span><span class="sxs-lookup"><span data-stu-id="e8046-178">The  id token sent back to your application includes the new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="e8046-179">Podívejte se na příklad.</span><span class="sxs-lookup"><span data-stu-id="e8046-179">See example.</span></span>

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="e8046-180">Další postup</span><span class="sxs-lookup"><span data-stu-id="e8046-180">Next steps</span></span>

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a><span data-ttu-id="e8046-181">Přidejte novou deklaraci na toky pro sociálních účet přihlášení změnou TechnicalProfiles uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="e8046-181">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed below.</span></span> <span data-ttu-id="e8046-182">Tyto dvě TechnicalProfiles jsou používány sociálního/federovaný účet přihlášení k zápisu a čtení dat uživatele pomocí alternativeSecurityId jako Lokátor objektu uživatele.</span><span class="sxs-lookup"><span data-stu-id="e8046-182">These two TechnicalProfiles are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator of the user object.</span></span>
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="e8046-183">Pomocí stejné atributy rozšíření mezi předdefinované a vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="e8046-183">Using the same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="e8046-184">Když přidáte rozšíření atributy (neboli vlastní atributy) prostřednictvím portálu prostředí, jsou tyto atributy registrovat pomocí ** b2c-rozšíření aplikaci, která existuje v každé klienta b2c.</span><span class="sxs-lookup"><span data-stu-id="e8046-184">When you add extension attributes (aka custom attributes) via the portal experience, those attributes are registered using the **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="e8046-185">Tyto atributy rozšíření použít vlastní zásady:</span><span class="sxs-lookup"><span data-stu-id="e8046-185">To use these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="e8046-186">V rámci svého klienta b2c na stránce portal.azure.com, přejděte na **Azure Active Directory** a vyberte **registrace aplikace**</span><span class="sxs-lookup"><span data-stu-id="e8046-186">Within your b2c tenant in portal.azure.com, navigate to **Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="e8046-187">Najít váš **aplikace b2c rozšíření** a vyberte ho</span><span class="sxs-lookup"><span data-stu-id="e8046-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="e8046-188">V části "Essentials, záznam **ID aplikace** a **ID objektu**</span><span class="sxs-lookup"><span data-stu-id="e8046-188">Under 'Essentials' record the **Application ID** and the **Object ID**</span></span>
4. <span data-ttu-id="e8046-189">Zahrnout do AAD běžné technické metadata profil jako následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e8046-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="e8046-190">Zachovat konzistenci s práce s portálem, vytvořte tyto atributy pomocí uživatelského rozhraní portálu *před* je použijete v vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="e8046-190">To keep consistency with the portal experience, create these attributes using the portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="e8046-191">Při vytváření atributu "ActivationStatus" na portálu můžete musí odkazovat na ho následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e8046-191">When you create an attribute "ActivationStatus" in the portal, you must refer to it as follows:</span></span>

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a><span data-ttu-id="e8046-192">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="e8046-192">Reference</span></span>

* <span data-ttu-id="e8046-193">A **technické profilu (TP)** je typ elementu, který lze považovat za *funkce* který definuje název koncového bodu, jeho metadata, používá protokol a podrobnosti exchange deklarací identity, které by měl provést rozhraní prostředí Identity.</span><span class="sxs-lookup"><span data-stu-id="e8046-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details the exchange of claims that the Identity Experience Framework should perform.</span></span>  <span data-ttu-id="e8046-194">Pokud to *funkce* je volána v kroku orchestration nebo z jiného TechnicalProfile, InputClaims a OutputClaims jsou poskytovány jako parametry volající.</span><span class="sxs-lookup"><span data-stu-id="e8046-194">When this *function* is called in an orchestration step or from another TechnicalProfile, the InputClaims and OutputClaims are provided as parameters by the caller.</span></span>


* <span data-ttu-id="e8046-195">Úplné zpracování u rozšíření vlastností, najdete v článku [rozšíření schématu adresáře | KONCEPTY GRAPH API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="e8046-195">For full treatment on extension properties, see the article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="e8046-196">Rozšíření atributů v rozhraní Graph API jsou pojmenované pomocí konvence `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="e8046-196">Extension attributes in Graph API are named using the convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="e8046-197">Vlastní zásady, které se odkazují na rozšíření atributy jako extension_attributename, proto vynechání ApplicationObjectId v souboru XML</span><span class="sxs-lookup"><span data-stu-id="e8046-197">Custom policies refer to extensions attributes as extension_attributename, thus omitting the ApplicationObjectId in the XML</span></span>
