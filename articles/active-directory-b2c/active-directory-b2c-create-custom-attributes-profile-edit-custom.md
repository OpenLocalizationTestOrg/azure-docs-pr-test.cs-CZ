---
title: "Azure Active Directory B2C: Přidat vlastní atributy toocustom zásady a použít v profilu upravit | Microsoft Docs"
description: "Návod na pomocí vlastnosti rozšíření, vlastní atributy a jejich včetně hello uživatelské rozhraní"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="ef749-103">Azure Active Directory B2C: Vytváření a používání vlastních atributů v vlastního profilu upravit zásady</span><span class="sxs-lookup"><span data-stu-id="ef749-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="ef749-104">V tomto článku můžete vytvořit vlastní atribut v adresáři Azure AD B2C a používat tento nový atribut jako vlastních deklarací identity v hello profil upravit uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="ef749-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in hello profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef749-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef749-105">Prerequisites</span></span>

<span data-ttu-id="ef749-106">Dokončení hello kroků v článku hello [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="ef749-106">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="ef749-107">Použít vlastní atributy toocollect informace o zákazníky v Azure Active Directory B2C pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="ef749-107">Use custom attributes toocollect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="ef749-108">Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu atributů: zadané jméno, příjmení, Město, PSČ, userPrincipalName, atd.  Často potřebujete toocreate vlastní atributy.</span><span class="sxs-lookup"><span data-stu-id="ef749-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need toocreate your own attributes.</span></span>  <span data-ttu-id="ef749-109">Například:</span><span class="sxs-lookup"><span data-stu-id="ef749-109">For example:</span></span>
* <span data-ttu-id="ef749-110">Zákazník směřujících aplikací musí toopersist atribut jako je například "LoyaltyNumber."</span><span class="sxs-lookup"><span data-stu-id="ef749-110">A customer-facing application needs toopersist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="ef749-111">Zprostředkovatele identity má jedinečný identifikátor uživatele, musí být uložena jako je například "uniqueUserGUID"."</span><span class="sxs-lookup"><span data-stu-id="ef749-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="ef749-112">Vlastní uživatelské cesty musí toopersist hello stavu uživatele, jako je například "migrationStatus."</span><span class="sxs-lookup"><span data-stu-id="ef749-112">A custom user journey needs toopersist hello state of user such as "migrationStatus."</span></span>

<span data-ttu-id="ef749-113">S Azure AD B2C můžete rozšířit hello sadu atributů uložené u každého uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="ef749-113">With Azure AD B2C, you can extend hello set of attributes stored on each user account.</span></span> <span data-ttu-id="ef749-114">Můžete také číst a zapsat tyto atributy pomocí hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ef749-114">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="ef749-115">Vlastnosti rozšíření rozšířit schéma hello hello uživatelských objektů v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-115">Extension properties extend hello schema of hello user objects in hello directory.</span></span>  <span data-ttu-id="ef749-116">Vlastnost rozšíření Hello podmínky, vlastních atributů a vlastních deklarací identity najdete v toohello, které samé v kontextu hello tento název článku a hello se liší v závislosti na kontextu hello (aplikace, objekt, zásady).</span><span class="sxs-lookup"><span data-stu-id="ef749-116">hello terms extension property, custom attribute and custom claim refer toohello same thing in hello context of this article and hello name varies depending on hello context (application, object, policy).</span></span>

<span data-ttu-id="ef749-117">Vlastnosti rozšíření se dají registrovat jenom na objekt aplikace i v případě, že mohou obsahovat data pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef749-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="ef749-118">Vlastnost Hello je připojený toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef749-118">hello property is attached toohello application.</span></span> <span data-ttu-id="ef749-119">objekt aplikace Hello musí být tooregister udělí oprávnění k zápisu ve vlastnosti rozšíření.</span><span class="sxs-lookup"><span data-stu-id="ef749-119">hello Application object must be granted write access tooregister an extension property.</span></span> <span data-ttu-id="ef749-120">100 – vlastnosti rozšíření (v rámci všech typů a všechny aplikace), může být napsán tooany jednoho objektu.</span><span class="sxs-lookup"><span data-stu-id="ef749-120">100 Extension properties (across ALL types and ALL applications) can be written tooany single object.</span></span> <span data-ttu-id="ef749-121">Vlastnosti rozšíření přidají toohello cílový adresář typ a že bude okamžitě dostupný v klientovi directory hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ef749-121">Extension properties are added toohello target directory type and becomes immediately accessible in hello Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="ef749-122">Pokud aplikace hello je odstraněna, budou odebrány také tyto vlastnosti rozšíření spolu s daty v nich obsažené pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef749-122">If hello application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="ef749-123">Pokud ve vlastnosti rozšíření je odstraní hello aplikace, bude odebrán na hello cílových objektů adresáře a hello hodnoty odstranit.</span><span class="sxs-lookup"><span data-stu-id="ef749-123">If an extension property is deleted by hello Application, it is removed on hello target directory objects, and hello values deleted.</span></span>

<span data-ttu-id="ef749-124">Vlastnosti rozšíření existují pouze v kontextu hello zaregistrovanou aplikaci v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-124">Extension properties exist only in hello context of a registered  Application in hello tenant.</span></span> <span data-ttu-id="ef749-125">id objektu Hello této aplikace musí být součástí hello TechnicalProfile, který ho použít.</span><span class="sxs-lookup"><span data-stu-id="ef749-125">hello object id of that Application must be included in hello TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="ef749-126">Hello Azure AD B2C directory obvykle zahrnuje webovou aplikaci s názvem `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="ef749-126">hello Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="ef749-127">Tato aplikace je používána především hello b2c předdefinované zásady pro vlastní deklarace hello vytvořené prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef749-127">This application is primarily used by hello b2c built-in  policies for hello custom claims created via hello Azure portal.</span></span>  <span data-ttu-id="ef749-128">Pomocí této aplikace tooregister rozšíření pro vlastní zásady b2c se doporučuje jenom pro zkušené uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef749-128">Using this application tooregister extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="ef749-129">Pokyny k tomuto jsou součástí hello části Další kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="ef749-129">Instructions for this are included in hello Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a><span data-ttu-id="ef749-130">Vytvoření nové aplikace toostore hello – vlastnosti rozšíření</span><span class="sxs-lookup"><span data-stu-id="ef749-130">Creating a new application toostore hello extension properties</span></span>

1. <span data-ttu-id="ef749-131">Spustí relaci prohlížeče a přejděte toohello [Azure Portal](https://portal.azure.com) a přihlaste se přihlašovacími údaji správce hello chcete tooconfigure adresáři B2C.</span><span class="sxs-lookup"><span data-stu-id="ef749-131">Open a browsing session and navigate toohello [Azure porta](https://portal.azure.com) and sign in with administrative credentials of hello B2C Directory you wish tooconfigure.</span></span>
1. <span data-ttu-id="ef749-132">Klikněte na tlačítko **Azure Active Directory** v levém navigačním nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-132">Click **Azure Active Directory** on hello left navigation menu.</span></span> <span data-ttu-id="ef749-133">Může být nutné ji tak, že vyberete více služeb toofind >.</span><span class="sxs-lookup"><span data-stu-id="ef749-133">You may need toofind it by selecting More services>.</span></span>
1. <span data-ttu-id="ef749-134">Vyberte **registrace aplikace** a klikněte na tlačítko **novou registraci aplikace**</span><span class="sxs-lookup"><span data-stu-id="ef749-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="ef749-135">Poskytnout doporučené hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ef749-135">Provide hello following recommended entries:</span></span>
  * <span data-ttu-id="ef749-136">Zadejte název webové aplikace hello: **WebApp. GraphAPI DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="ef749-136">Specify a name for hello web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="ef749-137">Typ aplikace: webové aplikace nebo rozhraní API</span><span class="sxs-lookup"><span data-stu-id="ef749-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="ef749-138">URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions přihlašování</span><span class="sxs-lookup"><span data-stu-id="ef749-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="ef749-139">Vyberte ** vytvořit.</span><span class="sxs-lookup"><span data-stu-id="ef749-139">Select **Create.</span></span> <span data-ttu-id="ef749-140">Úspěšné dokončení se zobrazí v hello **oznámení**</span><span class="sxs-lookup"><span data-stu-id="ef749-140">Successful completion appears in hello **notifications**</span></span>
1. <span data-ttu-id="ef749-141">Vyberte nově vytvořený hello webové aplikace: **WebApp. GraphAPI DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="ef749-141">Select hello newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="ef749-142">Vyberte nastavení: **požadovaná oprávnění**</span><span class="sxs-lookup"><span data-stu-id="ef749-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="ef749-143">Vybrat rozhraní API **služby Windows Active Directory**</span><span class="sxs-lookup"><span data-stu-id="ef749-143">Select API **Windows Active Directory**</span></span>
1. <span data-ttu-id="ef749-144">Oprávnění aplikací, zaškrtněte: **pro čtení a zápis dat adresáře**, a **uložit**</span><span class="sxs-lookup"><span data-stu-id="ef749-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="ef749-145">Zvolte **udělit oprávnění** a potvrďte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ef749-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="ef749-146">Tooyour schránky zkopírujte a uložte hello následujících identifikátory z webové aplikace. GraphAPI DirectoryExtensions > Nastavení > Vlastnosti ></span><span class="sxs-lookup"><span data-stu-id="ef749-146">Copy tooyour clipboard and save hello following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="ef749-147">**ID aplikace** .</span><span class="sxs-lookup"><span data-stu-id="ef749-147">**Application ID** .</span></span> <span data-ttu-id="ef749-148">Příklad:`103ee0e6-f92d-4183-b576-8c3739027780`</span><span class="sxs-lookup"><span data-stu-id="ef749-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="ef749-149">**ID objektu**.</span><span class="sxs-lookup"><span data-stu-id="ef749-149">**Object ID**.</span></span> <span data-ttu-id="ef749-150">Příklad:`80d8296a-da0a-49ee-b6ab-fd232aa45201`</span><span class="sxs-lookup"><span data-stu-id="ef749-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a><span data-ttu-id="ef749-151">Úprava vaše vlastní zásady tooadd hello ApplicationObjectId</span><span class="sxs-lookup"><span data-stu-id="ef749-151">Modifying your custom policy tooadd hello ApplicationObjectId</span></span>

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
><span data-ttu-id="ef749-152">Hello <TechnicalProfile Id="AAD-Common"> je odkazované tooas "běžné", protože jeho prvky jsou součástí a opakovaně používat ve všech hello Azure Active Directory TechnicalProfiles pomocí elementu hello:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="ef749-152">hello <TechnicalProfile Id="AAD-Common"> is referred tooas "common" because its elements are included in and reused in all hello Azure Active Directory TechnicalProfiles by using hello element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="ef749-153">Když hello TechnicalProfile zapíše pro vlastnost rozšíření toohello nově vytvořený první čas hello, může docházet k chybě jednorázové.</span><span class="sxs-lookup"><span data-stu-id="ef749-153">When hello TechnicalProfile writes for hello first time toohello newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="ef749-154">Vlastnost rozšíření Hello se vytvoří hello poprvé, co se používá.</span><span class="sxs-lookup"><span data-stu-id="ef749-154">hello extension property is created hello first time it is used.</span></span>  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="ef749-155">Pomocí nové vlastnosti rozšíření hello / vlastní atribut cesty uživatele</span><span class="sxs-lookup"><span data-stu-id="ef749-155">Using hello new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="ef749-156">Otevřete hello předávající Party(RP) soubor, který popisuje vaše zásady upravit uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="ef749-156">Open hello Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="ef749-157">Pokud jsou začínáte, může být vhodné toodownload vaší už nakonfigurované verzi hello RP PolicyEdit souboru přímo z hello části vlastní zásady pro Azure B2C v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ef749-157">If you are starting out, it may be advisable toodownload your already configured version of hello RP-PolicyEdit file directly from hello Azure B2C Custom Policy section in hello Azure portal.</span></span>  <span data-ttu-id="ef749-158">Alternativně můžete otevřete soubor XML ze složky úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef749-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="ef749-159">Přidat vlastní deklarace identity `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="ef749-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="ef749-160">Zahrnutím hello vlastní deklarace identity v hello `<RelyingParty>` elementu, je předaná jako parametr toohello UserJourney TechnicalProfiles a součástí hello token pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-160">By including hello custom claim in hello `<RelyingParty>` element, it is passed as a parameter toohello UserJourney TechnicalProfiles and included in hello token for hello application.</span></span>
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
3. <span data-ttu-id="ef749-161">Přidání souboru deklarace identity definice toohello rozšíření zásad `TrustFrameworkExtensions.xml` uvnitř hello `<ClaimsSchema>` element, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="ef749-161">Add a claim definition toohello Extension policy file  `TrustFrameworkExtensions.xml` inside hello `<ClaimsSchema>` element as shown.</span></span>
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
4. <span data-ttu-id="ef749-162">Přidat hello stejný soubor základní zásad toohello definice deklarace `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="ef749-162">Add hello same claim definition toohello Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="ef749-163">Přidání `ClaimType` definice v základní hello i hello rozšíření soubor není obvykle nutné, ale vzhledem k tomu, že další kroky hello přidá hello extension_loyaltyId tooTechnicalProfiles hello základního souboru, program pro ověření zásad hello odmítnou nahrávání hello hello základního souboru bez ní.</span><span class="sxs-lookup"><span data-stu-id="ef749-163">Adding a `ClaimType` definition in both hello base and hello extensions file is normally not necessary, however since hello next steps will add hello extension_loyaltyId tooTechnicalProfiles in hello Base file, hello policy validator will reject hello upload of hello base file without it.</span></span>
><span data-ttu-id="ef749-164">To může být užitečné tootrace hello provádění hello cesty uživatele s názvem "ProfileEdit" v souboru TrustFrameworkBase.xml hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-164">It may be useful tootrace hello execution of hello user journey named "ProfileEdit" in hello TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="ef749-165">Vyhledejte cestu uživatele hello hello stejný název ve svém editoru a pozorovat, že krok 5 Orchestration vyvolá hello TechnicalProfileReferenceID = "SelfAsserted-ProfileUpdate".</span><span class="sxs-lookup"><span data-stu-id="ef749-165">Search for hello user journey of hello same name in your editor and observe that Orchestration Step 5 invokes hello TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="ef749-166">Hledání a zkontrolovat tento TechnicalProfile toofamiliarize s tokem hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-166">Search and inspect this TechnicalProfile toofamiliarize yourself with hello flow.</span></span>
5. <span data-ttu-id="ef749-167">Přidat loyaltyId jako vstupní a výstupní deklarací identity v hello TechnicalProfile "SelfAsserted-ProfileUpdate"</span><span class="sxs-lookup"><span data-stu-id="ef749-167">Add loyaltyId as input and output claim in hello TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
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

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="ef749-168">Přidání deklarace identity v TechnicalProfile "AAD-UserWriteProfileUsingObjectId" toopersist hello hodnoty deklarace identity hello ve vlastnosti rozšíření hello, pro hello aktuálního uživatele v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" toopersist hello value of hello claim in hello extension property, for hello current user in hello directory.</span></span>
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
7. <span data-ttu-id="ef749-169">Přidání deklarace identity v TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello hodnotu atributu rozšíření hello pokaždé, když se uživatel přihlásí.</span><span class="sxs-lookup"><span data-stu-id="ef749-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello value of hello extension attribute every time a user logs in.</span></span> <span data-ttu-id="ef749-170">Doposud hello TechnicalProfiles byly změněny v toku hello pouze místní účty.</span><span class="sxs-lookup"><span data-stu-id="ef749-170">Thus far hello TechnicalProfiles have been changed in hello flow of local accounts only.</span></span>  <span data-ttu-id="ef749-171">Pokud nový atribut hello se požaduje v toku hello sociálního nebo federované účtu, musí jinou sadu TechnicalProfiles toobe změnit.</span><span class="sxs-lookup"><span data-stu-id="ef749-171">If hello new attribute is desired in hello flow of a social/federated account, a different set of TechnicalProfiles needs toobe changed.</span></span> <span data-ttu-id="ef749-172">Najdete v části Další kroky.</span><span class="sxs-lookup"><span data-stu-id="ef749-172">See Next Steps.</span></span>

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
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
><span data-ttu-id="ef749-173">Hello IncludeTechnicalProfile element přidá všechny elementy hello AAD běžné toothis TechnicalProfile.</span><span class="sxs-lookup"><span data-stu-id="ef749-173">hello IncludeTechnicalProfile element adds all hello elements of AAD-Common toothis TechnicalProfile.</span></span>

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="ef749-174">Testování hello "Spustit nyní" pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="ef749-174">Test hello custom policy using "Run Now"</span></span>
1. <span data-ttu-id="ef749-175">Otevřete hello **okno Azure AD B2C** a přejděte příliš**Identity rozhraní Framework > vlastní zásady**.</span><span class="sxs-lookup"><span data-stu-id="ef749-175">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="ef749-176">Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ef749-176">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
1. <span data-ttu-id="ef749-177">Musí být schopný toosign díky e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="ef749-177">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="ef749-178">Hello id token odeslána zpět tooyour aplikace obsahuje novou vlastnost rozšíření hello jako vlastní deklaraci extension_loyaltyId před sebou.</span><span class="sxs-lookup"><span data-stu-id="ef749-178">hello  id token sent back tooyour application includes hello new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="ef749-179">Podívejte se na příklad.</span><span class="sxs-lookup"><span data-stu-id="ef749-179">See example.</span></span>

```
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

## <a name="next-steps"></a><span data-ttu-id="ef749-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef749-180">Next steps</span></span>

<span data-ttu-id="ef749-181">Přidejte hello novou deklaraci identity toohello toky pro sociálních účet přihlášení změnou hello TechnicalProfiles nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="ef749-181">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed.</span></span> <span data-ttu-id="ef749-182">Tyto dvě TechnicalProfiles jsou používané toowrite sociálního/federovaný účet přihlášení a čtení dat uživatele hello pomocí hello alternativeSecurityId jako hello Lokátor hello objekt uživatele.</span><span class="sxs-lookup"><span data-stu-id="ef749-182">These two TechnicalProfiles are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator of hello user object.</span></span>
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="ef749-183">Pomocí hello stejné atributy rozšíření mezi předdefinované a vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="ef749-183">Using hello same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="ef749-184">Když přidáte rozšíření atributy (neboli vlastní atributy) prostřednictvím portálu prostředí hello, jsou tyto atributy registrovat pomocí hello ** b2c-rozšíření aplikaci, která existuje v každé klienta b2c.</span><span class="sxs-lookup"><span data-stu-id="ef749-184">When you add extension attributes (aka custom attributes) via hello portal experience, those attributes are registered using hello **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="ef749-185">toouse tyto atributy rozšíření ve vlastních zásadách:</span><span class="sxs-lookup"><span data-stu-id="ef749-185">toouse these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="ef749-186">V rámci svého klienta b2c na stránce portal.azure.com přejděte příliš**Azure Active Directory** a vyberte **registrace aplikace**</span><span class="sxs-lookup"><span data-stu-id="ef749-186">Within your b2c tenant in portal.azure.com, navigate too**Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="ef749-187">Najít váš **aplikace b2c rozšíření** a vyberte ho</span><span class="sxs-lookup"><span data-stu-id="ef749-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="ef749-188">V části 'Essentials' záznam hello **ID aplikace** a hello **ID objektu**</span><span class="sxs-lookup"><span data-stu-id="ef749-188">Under 'Essentials' record hello **Application ID** and hello **Object ID**</span></span>
4. <span data-ttu-id="ef749-189">Zahrnout do AAD běžné technické metadata profil jako následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ef749-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="ef749-190">konzistence tookeep hello portálu prostředí, vytvořte tyto atributy pomocí portálu hello uživatelského rozhraní *před* je použijete v vlastní zásady.</span><span class="sxs-lookup"><span data-stu-id="ef749-190">tookeep consistency with hello portal experience, create these attributes using hello portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="ef749-191">Při vytváření atributu "ActivationStatus" hello portálu musí odkazovat tooit následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ef749-191">When you create an attribute "ActivationStatus" in hello portal, you must refer tooit as follows:</span></span>

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a><span data-ttu-id="ef749-192">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ef749-192">Reference</span></span>

* <span data-ttu-id="ef749-193">A **technické profilu (TP)** je typ elementu, který lze považovat za *funkce* název koncového bodu, jeho metadata, používá protokol, který definuje a podrobnosti hello výměny deklarací identity, které hello Identity Rozhraní Framework má být provedena.</span><span class="sxs-lookup"><span data-stu-id="ef749-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details hello exchange of claims that hello Identity Experience Framework should perform.</span></span>  <span data-ttu-id="ef749-194">Pokud to *funkce* je volána v kroku orchestration nebo z jiného TechnicalProfile hello InputClaims a OutputClaims jsou poskytovány jako parametry volající hello.</span><span class="sxs-lookup"><span data-stu-id="ef749-194">When this *function* is called in an orchestration step or from another TechnicalProfile, hello InputClaims and OutputClaims are provided as parameters by hello caller.</span></span>


* <span data-ttu-id="ef749-195">Úplné zpracování u rozšíření vlastností, najdete v článku hello [rozšíření schématu adresáře | KONCEPTY GRAPH API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="ef749-195">For full treatment on extension properties, see hello article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="ef749-196">Rozšíření atributů v rozhraní Graph API jsou pojmenované pomocí konvencí hello `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="ef749-196">Extension attributes in Graph API are named using hello convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="ef749-197">Vlastní zásady odkazovat tooextensions atributy jako extension_attributename, proto vynechání hello ApplicationObjectId v hello XML</span><span class="sxs-lookup"><span data-stu-id="ef749-197">Custom policies refer tooextensions attributes as extension_attributename, thus omitting hello ApplicationObjectId in hello XML</span></span>
