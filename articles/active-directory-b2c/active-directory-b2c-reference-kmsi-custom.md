---
title: 'Azure Active Directory B2C: KMSI | Microsoft Docs'
description: "Téma, který ukazuje, jak nastavit 'zajistit mi přihlášení."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: mtillman
ms.assetid: 926e9711-71c0-49e8-b658-146ffb7386c0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: a3d78945f862d1ae12cec05da0cf0ea7df511f43
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a><span data-ttu-id="048fb-103">Azure Active Directory B2C: Povolte 'zůstat přihlášeni (KMSI).</span><span class="sxs-lookup"><span data-stu-id="048fb-103">Azure Active Directory B2C: Enable 'Keep me signed in (KMSI)'</span></span>  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="048fb-104">Azure AD B2C teď umožňuje web a nativních aplikací k povolení funkcí, zůstat přihlášeni (KMSI)'.</span><span class="sxs-lookup"><span data-stu-id="048fb-104">Azure AD B2C now allows your web and native applications to enable the 'Keep me signed in (KMSI)' functionality.</span></span> <span data-ttu-id="048fb-105">Tato funkce uděluje přístup k vrácení uživatelé aplikaci bez zobrazení výzvy k znovu zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="048fb-105">This feature grants access to returning users to application without prompting to reenter the username and password.</span></span> <span data-ttu-id="048fb-106">Tento přístup odvolaný při odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="048fb-106">This access is revoked when the user logs out.</span></span> 

<span data-ttu-id="048fb-107">Doporučujeme, abyste před uživatelé kontrola této možnosti na veřejné počítače.</span><span class="sxs-lookup"><span data-stu-id="048fb-107">We recommend against users checking this option on public computers.</span></span> 

![obrázek](images/kmsi.PNG)


## <a name="prerequisites"></a><span data-ttu-id="048fb-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="048fb-109">Prerequisites</span></span>

<span data-ttu-id="048fb-110">Klient služby Azure AD B2C nakonfigurovat tak, aby místní účet registrace-množství nebo přihlášení, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="048fb-110">An Azure AD B2C tenant configured to allow local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="how-to-enable-kmsi"></a><span data-ttu-id="048fb-111">Postup povolení KMSI</span><span class="sxs-lookup"><span data-stu-id="048fb-111">How to enable KMSI</span></span>

<span data-ttu-id="048fb-112">Proveďte následující změny v vaše zásady důvěryhodnosti framework rozšíření.</span><span class="sxs-lookup"><span data-stu-id="048fb-112">Make the following changes in your trust framework extensions policy.</span></span>

## <a name="adding-a-content-definition-element"></a><span data-ttu-id="048fb-113">Přidání obsahu definice elementu</span><span class="sxs-lookup"><span data-stu-id="048fb-113">Adding a content definition element</span></span> 

<span data-ttu-id="048fb-114">`BuildingBlocks` Musí zahrnovat uzlu souboru rozšíření `ContentDefinitions` elementu.</span><span class="sxs-lookup"><span data-stu-id="048fb-114">The `BuildingBlocks` node of your extention file must include a `ContentDefinitions` element.</span></span> 

1. <span data-ttu-id="048fb-115">V `ContentDefinitions` části, zadejte nový `ContentDefinition` s ID `api.signuporsigninwithkmsi`.</span><span class="sxs-lookup"><span data-stu-id="048fb-115">In the `ContentDefinitions` section, define a new `ContentDefinition` with ID `api.signuporsigninwithkmsi`.</span></span>
2. <span data-ttu-id="048fb-116">Nové `ContentDefinition` musí obsahovat `LoadUri`, `RecoveryUri` a `DataUri` následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="048fb-116">Your new `ContentDefinition` must include a `LoadUri`, `RecoveryUri` and `DataUri` as follows.</span></span>
3. <span data-ttu-id="048fb-117">Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` je identifikátor nerozumí počítač, který zobrazí zaškrtávací políčko KMSI v přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="048fb-117">Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` is a machine understandable identifier that brings up a KMSI check box in the sign-in pages.</span></span> <span data-ttu-id="048fb-118">Zkontrolujte prosím, že nemáte tuto hodnotu změnit.</span><span class="sxs-lookup"><span data-stu-id="048fb-118">Please make sure you don't change this value.</span></span> 

```XML
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.signuporsigninwithkmsi">
        <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
        <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
        <Metadata>
          <Item Key="DisplayName">Signin and Signup</Item>
        </Metadata>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>                       
```



## <a name="add-a--local-account-sign-in-claims-provider"></a><span data-ttu-id="048fb-119">Přidání poskytovatele deklarací identity přihlášení místní účet</span><span class="sxs-lookup"><span data-stu-id="048fb-119">Add a  local account sign-in claims provider</span></span> 

<span data-ttu-id="048fb-120">Místní přihlášení účtu můžete definovat jako zprostředkovatele deklarací identity k `<ClaimsProvider>` uzlu v souboru rozšíření zásad:</span><span class="sxs-lookup"><span data-stu-id="048fb-120">You can define Local Account SignIn as a claims provider to the `<ClaimsProvider>` node in the extension file of your policy:</span></span>

1. <span data-ttu-id="048fb-121">Otevřete soubor rozšíření (TrustFrameworkExtensions.xml) z pracovního adresáře.</span><span class="sxs-lookup"><span data-stu-id="048fb-121">Open the extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span> 
2. <span data-ttu-id="048fb-122">Najít `<ClaimsProviders>` části.</span><span class="sxs-lookup"><span data-stu-id="048fb-122">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="048fb-123">Pokud neexistuje, přidejte ji do kořenového uzlu.</span><span class="sxs-lookup"><span data-stu-id="048fb-123">If it does not exist, add it under the root node.</span></span>
3. <span data-ttu-id="048fb-124">Startovní sady z [Začínáme](active-directory-b2c-get-started-custom.md) se dodává s poskytovatele deklarací identity, místní účet přihlášení".</span><span class="sxs-lookup"><span data-stu-id="048fb-124">The starter pack from  [Getting started](active-directory-b2c-get-started-custom.md) comes with a 'Local Account SignIn' claims provider.</span></span> 
4. <span data-ttu-id="048fb-125">Pokud ne, přidejte nový `<ClaimsProvider>` uzlu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="048fb-125">If not, add a new `<ClaimsProvider>` node as follows:</span></span>

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="login-NonInteractive">
           <Metadata>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
           </Metadata>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
           </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
 </ClaimsProviders>
```

### <a name="add-the-application-ids-to-your-custom-policy"></a><span data-ttu-id="048fb-126">Přidat ID aplikace do vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="048fb-126">Add the application IDs to your custom policy</span></span>

<span data-ttu-id="048fb-127">Přidat ID aplikace souboru rozšíření (`TrustFrameworkExtensions.xml`):</span><span class="sxs-lookup"><span data-stu-id="048fb-127">Add the application IDs to the extensions file (`TrustFrameworkExtensions.xml`):</span></span>

1. <span data-ttu-id="048fb-128">V souboru rozšíření (TrustFrameworkExtensions.xml) nalezen element `<TechnicalProfile Id="login-NonInteractive">` a `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`</span><span class="sxs-lookup"><span data-stu-id="048fb-128">In the extensions file (TrustFrameworkExtensions.xml), find the element `<TechnicalProfile Id="login-NonInteractive">` and `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`</span></span>

2. <span data-ttu-id="048fb-129">Nahraďte všechny výskyty `IdentityExperienceFrameworkAppId` s ID aplikace Framework prostředí Identity aplikace, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="048fb-129">Replace all instances of `IdentityExperienceFrameworkAppId` with the application ID of the Identity Experience Framework application as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="048fb-130">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="048fb-130">Here is an example:</span></span>

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. <span data-ttu-id="048fb-131">Nahraďte všechny výskyty `ProxyIdentityExperienceFrameworkAppId` s ID aplikace Framework prostředí Identity Proxy aplikace, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="048fb-131">Replace all instances of `ProxyIdentityExperienceFrameworkAppId` with the application ID of the Proxy Identity Experience Framework application as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>

4. <span data-ttu-id="048fb-132">Uložte soubor rozšíření.</span><span class="sxs-lookup"><span data-stu-id="048fb-132">Save your extensions file.</span></span>

## <a name="create-a-kmsi-in-enabled-user-journey"></a><span data-ttu-id="048fb-133">Vytvoření KMSI v cesty povoleného uživatele</span><span class="sxs-lookup"><span data-stu-id="048fb-133">Create a KMSI in enabled user journey</span></span>

<span data-ttu-id="048fb-134">Teď je potřeba přidat poskytovatele deklarací identity místní účet přihlášení pro vaše uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="048fb-134">You now need to add Local Account SignIn claims provider to your user journey.</span></span> 

1. <span data-ttu-id="048fb-135">Otevřete soubor základní zásad (například TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="048fb-135">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="048fb-136">Najít `<UserJourneys>` elementu a zkopírujte celou `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="048fb-136">Find the `<UserJourneys>` element and copy the entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
3. <span data-ttu-id="048fb-137">Otevřete soubor rozšíření (například TrustFrameworkExtensions.xml) a najděte `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="048fb-137">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="048fb-138">Pokud element neexistuje, přidejte jedno.</span><span class="sxs-lookup"><span data-stu-id="048fb-138">If the element doesn't exist, add one.</span></span>
4. <span data-ttu-id="048fb-139">Vložte celý `<UserJourney>` uzlu, který jste zkopírovali jako podřízenou `<UserJourneys>` elementu.</span><span class="sxs-lookup"><span data-stu-id="048fb-139">Paste the entire `<UserJourney>` node that you copied as a child of the `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="048fb-140">Přejmenujte ID nové cesty uživatele (například přejmenování jako `SignUpOrSignInWithKmsi`).</span><span class="sxs-lookup"><span data-stu-id="048fb-140">Rename the ID of the new user journey (for example, rename as `SignUpOrSignInWithKmsi`).</span></span>
6. <span data-ttu-id="048fb-141">Nakonec v `OrchestrationStep 1` změnit `ContentDefinitionReferenceId` k `api.signuporsigninwithkmsi` , definována v dřívějších krocích.</span><span class="sxs-lookup"><span data-stu-id="048fb-141">Finally, in `OrchestrationStep 1` change the `ContentDefinitionReferenceId` to `api.signuporsigninwithkmsi` , you definied in the earlier steps.</span></span> <span data-ttu-id="048fb-142">To umožňuje políčka v cesty uživatele.</span><span class="sxs-lookup"><span data-stu-id="048fb-142">This enables the  checkbox in the user journey.</span></span> 
7. <span data-ttu-id="048fb-143">Dokončení úpravy souboru rozšíření.</span><span class="sxs-lookup"><span data-stu-id="048fb-143">You are done modifying the extension file.</span></span> <span data-ttu-id="048fb-144">Uložte a nahrajte tento soubor.</span><span class="sxs-lookup"><span data-stu-id="048fb-144">Save and upload this file.</span></span> <span data-ttu-id="048fb-145">Ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="048fb-145">Ensure that all validations succeed.</span></span>

```XML
<UserJourneys>
    <UserJourney Id="SignUpOrSignInWithKmsi">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <Preconditions>
            <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
              <Value>objectId</Value>
              <Action>SkipThisOrchestrationStep</Action>
            </Precondition>
          </Preconditions>
          <ClaimsExchanges>
            <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <!-- This step reads any user attributes that we may not have received when in the token. -->
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

## <a name="create-a-relying-party-rp-file"></a><span data-ttu-id="048fb-146">Vytvořte soubor předávající stranu</span><span class="sxs-lookup"><span data-stu-id="048fb-146">Create a relying party (RP) file</span></span>

<span data-ttu-id="048fb-147">Potom aktualizujte soubor předávající stranu, který iniciuje cesty uživatele, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="048fb-147">Next, update the relying party (RP) file that initiates the user journey that you created:</span></span>

1. <span data-ttu-id="048fb-148">Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="048fb-148">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="048fb-149">Potom přejmenujte ji (například SignUpOrSignInWithKmsi.xml).</span><span class="sxs-lookup"><span data-stu-id="048fb-149">Then, rename it (for example, SignUpOrSignInWithKmsi.xml).</span></span>

2. <span data-ttu-id="048fb-150">Otevřete nový soubor a aktualizace `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="048fb-150">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="048fb-151">Toto je název vaší zásady (například SignUpOrSignInWithKmsi).</span><span class="sxs-lookup"><span data-stu-id="048fb-151">This is the name of your policy (for example, SignUpOrSignInWithKmsi).</span></span>

3. <span data-ttu-id="048fb-152">Změnit `ReferenceId` atribut `<DefaultUserJourney>` tak, aby odpovídala `Id` nové cesty uživatele, který jste vytvořili (například SignUpOrSignInWithKmsi).</span><span class="sxs-lookup"><span data-stu-id="048fb-152">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the `Id` of the new user journey that you created (for example, SignUpOrSignInWithKmsi).</span></span>

4. <span data-ttu-id="048fb-153">KMSI je nakonfigurovaný v `UserJourneyBehaviors`.</span><span class="sxs-lookup"><span data-stu-id="048fb-153">KMSI is configured in `UserJourneyBehaviors`.</span></span> 

5. <span data-ttu-id="048fb-154">**`KeepAliveInDays`** Určuje, jak dlouho zůstane přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="048fb-154">**`KeepAliveInDays`** controls how long the user remains signed in.</span></span> <span data-ttu-id="048fb-155">V následujícím příkladu KMSI relace automaticky vyprší 14 dnů bez ohledu na to, jak často uživatel provede bezobslužnou ověřování.</span><span class="sxs-lookup"><span data-stu-id="048fb-155">In the following example, KMSI session automatically expires after 14 days regardless of how often the user performs silent authentication.</span></span>

   <span data-ttu-id="048fb-156">Nastavení `KeepAliveInDays` hodnota 0 vypne KMSI funkce.</span><span class="sxs-lookup"><span data-stu-id="048fb-156">Setting `KeepAliveInDays`  value to 0 turns off KMSI functionality.</span></span> <span data-ttu-id="048fb-157">Ve výchozím nastavení tato hodnota je 0</span><span class="sxs-lookup"><span data-stu-id="048fb-157">By default, this value is 0</span></span>

6. <span data-ttu-id="048fb-158">Pokud  **`SessionExpiryType`**  je *kolejová*, pak KMSI relace je rozšířeno 14 dnů pokaždé, když uživatel provede bezobslužnou ověřování.</span><span class="sxs-lookup"><span data-stu-id="048fb-158">If **`SessionExpiryType`** is *Rolling*, then the KMSI session is extended by 14 days every time the user performs silent authentication.</span></span>  <span data-ttu-id="048fb-159">Pokud *kolejová* je vybraný, doporučujeme zachovat s minimální počet dnů.</span><span class="sxs-lookup"><span data-stu-id="048fb-159">If *Rolling* is selected, we recommend you to keep the number of days to minimum.</span></span> 

       <RelyingParty>
       <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
       <UserJourneyBehaviors>
         <SingleSignOn Scope="Tenant" KeepAliveInDays="14" />
         <SessionExpiryType>Absolute</SessionExpiryType>
         <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
       </UserJourneyBehaviors>
       <TechnicalProfile Id="PolicyProfile">
         <DisplayName>PolicyProfile</DisplayName>
         <Protocol Name="OpenIdConnect" />
         <OutputClaims>
           <OutputClaim ClaimTypeReferenceId="displayName" />
           <OutputClaim ClaimTypeReferenceId="givenName" />
           <OutputClaim ClaimTypeReferenceId="surname" />
           <OutputClaim ClaimTypeReferenceId="email" />
           <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
         </OutputClaims>
         <SubjectNamingInfo ClaimType="sub" />
       </TechnicalProfile>
       </RelyingParty>

7. <span data-ttu-id="048fb-160">Uložte změny a potom soubor odešlete.</span><span class="sxs-lookup"><span data-stu-id="048fb-160">Save your changes, and then upload the file.</span></span>

8. <span data-ttu-id="048fb-161">Pokud chcete otestovat vlastní zásady, který jste nahráli, na portálu Azure, přejděte do okna zásady a pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="048fb-161">To test the custom policy that you uploaded, in the Azure portal, go to the policy blade, and then click **Run now**.</span></span>


## <a name="link-to-sample-policy"></a><span data-ttu-id="048fb-162">Propojit ukázka zásad</span><span class="sxs-lookup"><span data-stu-id="048fb-162">Link to sample policy</span></span>

<span data-ttu-id="048fb-163">Můžete najít ukázkové zásady [zde](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).</span><span class="sxs-lookup"><span data-stu-id="048fb-163">You can find the sample policy [here](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).</span></span>








