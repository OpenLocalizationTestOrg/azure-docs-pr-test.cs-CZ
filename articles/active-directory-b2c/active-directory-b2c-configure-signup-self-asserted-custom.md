---
title: "Azure Active Directory B2C: Upravit přihlašovací nahoru v vlastní zásady a nakonfigurovat vlastní prohlašovanou zprostředkovatele"
description: "Návod k přidání deklarací toosign nahoru a nakonfigurovat hello uživatelský vstup"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: tbd
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/29/2017
ms.author: joroja
ms.openlocfilehash: c31d737263fef3e771bdf451b809b0ca522c8fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-tooadd-new-claims-and-configure-user-input"></a><span data-ttu-id="1478e-103">Azure Active Directory B2C: Upravte registrace tooadd nových deklarací identity a konfigurovat uživatelský vstup.</span><span class="sxs-lookup"><span data-stu-id="1478e-103">Azure Active Directory B2C: Modify sign up tooadd new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="1478e-104">V tomto článku budete přidávat nové registrace uživatele cesty tooyour položka (deklarace identity) uživatel zadal.</span><span class="sxs-lookup"><span data-stu-id="1478e-104">In this article, you will add a new user provided entry (a claim) tooyour signup user journey.</span></span>  <span data-ttu-id="1478e-105">Bude nakonfigurovat hello položku jako rozevírací seznam a definovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="1478e-105">You will configure hello entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="1478e-106">Upravená Sipi tootrigger testovací předání.</span><span class="sxs-lookup"><span data-stu-id="1478e-106">Edited by Sipi tootrigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1478e-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1478e-107">Prerequisites</span></span>

* <span data-ttu-id="1478e-108">Dokončení hello kroků v článku hello [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1478e-108">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="1478e-109">Otestujte hello registrace nebo přihlášení uživatele cesty toosignup nové místní účet, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="1478e-109">Test hello signup/signin user journey toosignup a new local account before proceeding.</span></span>


<span data-ttu-id="1478e-110">Shromažďování dat prvotní od uživatelů se dosahuje prostřednictvím registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1478e-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="1478e-111">Další deklarace identity se dají shromáždit později prostřednictvím profilu upravit uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="1478e-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="1478e-112">Kdykoliv Azure AD B2C shromáždí informace o přímo od uživatele hello interaktivně, hello Identity Framework prostředí používá jeho `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="1478e-112">Anytime Azure AD B2C gathers information directly from hello user interactively, hello Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="1478e-113">Hello postup použít, kdykoliv se používá tohoto zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="1478e-113">hello steps below apply anytime this provider is used.</span></span>


## <a name="define-hello-claim-its-display-name-and-hello-user-input-type"></a><span data-ttu-id="1478e-114">Zadejte hello deklarace identity, jeho zobrazovaný název a hello typ vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="1478e-114">Define hello claim, its display name and hello user input type</span></span>
<span data-ttu-id="1478e-115">Umožňuje, požádejte hello uživatele pro jejich město.</span><span class="sxs-lookup"><span data-stu-id="1478e-115">Lets ask hello user for their city.</span></span>  <span data-ttu-id="1478e-116">Přidejte následující element toohello hello `<ClaimsSchema>` element v souboru zásad TrustFrameWorkExtensions hello:</span><span class="sxs-lookup"><span data-stu-id="1478e-116">Add hello following element toohello `<ClaimsSchema>` element in hello TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="1478e-117">Existují další možnosti, které můžete tady vytvořit toocustomize hello deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="1478e-117">There are additional choices you can make here toocustomize hello claim.</span></span>  <span data-ttu-id="1478e-118">Úplné schéma, najdete v části toohello **Identity rozhraní Framework technické referenční příručka**.</span><span class="sxs-lookup"><span data-stu-id="1478e-118">For a full schema, refer toohello **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="1478e-119">Tato příručka brzy zveřejníme v části odkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1478e-119">This guide will be published soon in hello reference section.</span></span>

* <span data-ttu-id="1478e-120">`<DisplayName>`je řetězec, který definuje uživatelsky orientovaný hello *popisek*</span><span class="sxs-lookup"><span data-stu-id="1478e-120">`<DisplayName>` is a string that defines hello user-facing *label*</span></span>

* <span data-ttu-id="1478e-121">`<UserHelpText>`pomáhá hello uživateli pochopit, co je požadováno</span><span class="sxs-lookup"><span data-stu-id="1478e-121">`<UserHelpText>` helps hello user understand what is required</span></span>

* <span data-ttu-id="1478e-122">`<UserInputType>`hello následující čtyři možnosti zdůraznily níže:</span><span class="sxs-lookup"><span data-stu-id="1478e-122">`<UserInputType>` has hello following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="1478e-123">`RadioSingleSelectduration`-Vynucuje jednoho výběru.</span><span class="sxs-lookup"><span data-stu-id="1478e-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

    * <span data-ttu-id="1478e-124">`DropdownSingleSelect`-Umožňuje výběr hello pouze platné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1478e-124">`DropdownSingleSelect` - Allows hello selection of only valid value.</span></span>

![Snímek obrazovky možností rozevíracího seznamu](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```


* <span data-ttu-id="1478e-126">`CheckboxMultiSelect`Umožňuje výběr hello jeden nebo více hodnot.</span><span class="sxs-lookup"><span data-stu-id="1478e-126">`CheckboxMultiSelect` Allows for hello selection of one or more values.</span></span>

![Snímek obrazovky možnost vícenásobného výběru.](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>Receive updates from which cities?</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-hello-claim-toohello-sign-upsign-in-user-journey"></a><span data-ttu-id="1478e-128">Přidat až hello deklarace identity toohello přihlášení nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="1478e-128">Add hello claim toohello sign up/sign in user journey</span></span>

1. <span data-ttu-id="1478e-129">Přidá deklaraci hello jako `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (nalezené v souboru zásad TrustFrameworkBase hello).</span><span class="sxs-lookup"><span data-stu-id="1478e-129">Add hello claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in hello TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="1478e-130">Všimněte si, že tento TechnicalProfile používá hello SelfAssertedAttributeProvider.</span><span class="sxs-lookup"><span data-stu-id="1478e-130">Note this TechnicalProfile uses hello SelfAssertedAttributeProvider.</span></span>

  ```xml
  <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
    <DisplayName>Email signup</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
      <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
      <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
      <Item Key="language.button_continue">Create</Item>
    </Metadata>
    <CryptographicKeys>
      <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
    </CryptographicKeys>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
      <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" />
      <OutputClaim ClaimTypeReferenceId="newUser" />
      <!-- Optional claims, toobe collected from hello user -->
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surName" />
      <OutputClaim ClaimTypeReferenceId="city"/>
    </OutputClaims>
    <ValidationTechnicalProfiles>
      <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
    </ValidationTechnicalProfiles>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

2. <span data-ttu-id="1478e-131">Přidání deklarace identity toohello hello AAD UserWriteUsingLogonEmail jako `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello deklarace identity toohello AAD directory po shromáždění od uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="1478e-131">Add hello claim toohello AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello claim toohello AAD directory after collecting it from hello user.</span></span> <span data-ttu-id="1478e-132">Pokud dáváte přednost není toopersist hello deklarace identity v adresáři hello pro budoucí použití, může tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="1478e-132">You may skip this step if you prefer not toopersist hello claim in hello directory for future use.</span></span>

  ```xml
  <!-- Technical profiles for local accounts -->
  <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
    <Metadata>
      <Item Key="Operation">Write</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" Required="true" />
    </InputClaims>
    <PersistedClaims>
      <!-- Required claims -->
      <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
      <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password" />
      <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
      <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
      <!-- Optional claims. -->
      <PersistedClaim ClaimTypeReferenceId="givenName" />
      <PersistedClaim ClaimTypeReferenceId="surname" />
      <PersistedClaim ClaimTypeReferenceId="city" />
    </PersistedClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

3. <span data-ttu-id="1478e-133">Přidání deklarace identity toohello hello TechnicalProfile, který čte z adresáře hello, když se uživatel přihlásí jako`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="1478e-133">Add hello claim toohello TechnicalProfile that reads from hello directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for hello provided user ID.</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames" Required="true" />
    </InputClaims>
    <OutputClaims>
      <!-- Required claims -->
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <!-- Optional claims -->
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="otherMails" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  </TechnicalProfile>
  ```

4. <span data-ttu-id="1478e-134">Přidat hello `<OutputClaim ClaimTypeReferenceId="city" />` soubor zásad RP toohello SignUporSignIn.xml tak touto deklarací identity se odešlou toohello aplikace v tokenu hello po úspěšné uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="1478e-134">Add hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP policy file SignUporSignIn.xml so this claim is sent toohello application in hello token after a successful user journey.</span></span>

  ```xml
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ```

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="1478e-135">Testování hello "Spustit nyní" pomocí vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="1478e-135">Test hello custom policy using "Run Now"</span></span>

1. <span data-ttu-id="1478e-136">Otevřete hello **okno Azure AD B2C** a přejděte příliš**Identity rozhraní Framework > vlastní zásady**.</span><span class="sxs-lookup"><span data-stu-id="1478e-136">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="1478e-137">Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1478e-137">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="1478e-138">Musí být schopný toosign díky e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="1478e-138">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="1478e-139">úvodní obrazovka registrace v testovacím režimu by měl vypadat podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="1478e-139">hello signup screen in test mode should look similar toothis:</span></span>

![Snímek obrazovky upravené možnost zápisu](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="1478e-141">Hello tokenu back tooyour aplikace bude teď obsahovat hello `city` deklarace identity, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="1478e-141">hello token back tooyour application will now include hello `city` claim as shown below</span></span>
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "9c2a3a9e-ac65-4e46-a12d-9557b63033a9",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustf_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1493593222,
  "auth_time": 1493593222,
  "email": "joe@outlook.com",
  "given_name": "Joe",
  "family_name": "Ras",
  "city": "Bellevue",
  "name": "unknown"
}
```

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="1478e-142">Volitelné: Ověření e-mailu odeberte z cesty registrace</span><span class="sxs-lookup"><span data-stu-id="1478e-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="1478e-143">ověření e-mailu tooskip, Autor zásad hello vybrat tooremove `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="1478e-143">tooskip email verification, hello policy author can choose tooremove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="1478e-144">Hello e-mailovou adresu bude potřeba, ale není ověřen, pokud: vyžaduje"= true se odebere.</span><span class="sxs-lookup"><span data-stu-id="1478e-144">hello email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="1478e-145">Pečlivě zvažte, pokud tato možnost není pravé pro vaše případy použití!</span><span class="sxs-lookup"><span data-stu-id="1478e-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="1478e-146">Ověřit e-mailu je povoleno ve výchozím nastavení v hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` v souboru zásad TrustFrameworkBase hello sadě starter hello:</span><span class="sxs-lookup"><span data-stu-id="1478e-146">Verified email is enabled by default in hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in hello TrustFrameworkBase policy file in hello starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="1478e-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1478e-147">Next steps</span></span>

<span data-ttu-id="1478e-148">Změnou hello níže uvedená TechnicalProfiles přidáte hello novou deklaraci identity toohello toky pro sociálních účet přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1478e-148">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed below.</span></span> <span data-ttu-id="1478e-149">Jsou používané toowrite sociálního/federovaný účet přihlášení a čtení dat uživatele hello pomocí hello alternativeSecurityId jako hello lokátoru.</span><span class="sxs-lookup"><span data-stu-id="1478e-149">These are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
