---
title: "Azure Active Directory B2C: Upravit přihlašovací nahoru v vlastní zásady a nakonfigurovat vlastní prohlašovanou zprostředkovatele"
description: "Návod k přidání deklarací zaregistrovat a nakonfigurovat uživatelský vstup"
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
ms.openlocfilehash: 64b9d904d7d070052e125b479f4719d208c9ff85
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a><span data-ttu-id="4a12d-103">Azure Active Directory B2C: Upravte přihlašovací nahoru k přidání nových deklarací identity a konfiguraci vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a12d-103">Azure Active Directory B2C: Modify sign up to add new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="4a12d-104">V tomto článku přidáte nový záznam uživatel zadal (deklarace identity) vám dobře slouží registrace uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a12d-104">In this article, you will add a new user provided entry (a claim) to your signup user journey.</span></span>  <span data-ttu-id="4a12d-105">Bude nakonfigurovat položku jako rozevírací seznam a definovat v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4a12d-105">You will configure the entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="4a12d-106">Upravená Sipi se spustit testovací předání.</span><span class="sxs-lookup"><span data-stu-id="4a12d-106">Edited by Sipi to trigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a12d-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4a12d-107">Prerequisites</span></span>

* <span data-ttu-id="4a12d-108">Proveďte kroky v následujícím článku [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="4a12d-108">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="4a12d-109">Otestujte cesty registrace nebo přihlášení uživatele k registraci nového místní účet, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="4a12d-109">Test the signup/signin user journey to signup a new local account before proceeding.</span></span>


<span data-ttu-id="4a12d-110">Shromažďování dat prvotní od uživatelů se dosahuje prostřednictvím registrace nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4a12d-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="4a12d-111">Další deklarace identity se dají shromáždit později prostřednictvím profilu upravit uživatele cesty.</span><span class="sxs-lookup"><span data-stu-id="4a12d-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="4a12d-112">Kdykoliv Azure AD B2C shromáždí informace o interaktivním přímo od uživatele, používá rozhraní prostředí Identity jeho `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="4a12d-112">Anytime Azure AD B2C gathers information directly from the user interactively, the Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="4a12d-113">Následující postup použít, kdykoliv se používá tohoto zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="4a12d-113">The steps below apply anytime this provider is used.</span></span>


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a><span data-ttu-id="4a12d-114">Zadejte deklarace identity, jeho zobrazovaný název a typ vstupu uživatele</span><span class="sxs-lookup"><span data-stu-id="4a12d-114">Define the claim, its display name and the user input type</span></span>
<span data-ttu-id="4a12d-115">Umožňuje požádat uživatele pro jejich město.</span><span class="sxs-lookup"><span data-stu-id="4a12d-115">Lets ask the user for their city.</span></span>  <span data-ttu-id="4a12d-116">Přidejte následující elementu, který chcete `<ClaimsSchema>` element v souboru TrustFrameWorkExtensions zásad:</span><span class="sxs-lookup"><span data-stu-id="4a12d-116">Add the following element to the `<ClaimsSchema>` element in the TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="4a12d-117">Existují další možnosti, které můžete provést zde přizpůsobit deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="4a12d-117">There are additional choices you can make here to customize the claim.</span></span>  <span data-ttu-id="4a12d-118">Úplné schéma, najdete v části **Identity rozhraní Framework technické referenční příručka**.</span><span class="sxs-lookup"><span data-stu-id="4a12d-118">For a full schema, refer to the **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="4a12d-119">Tato příručka brzy zveřejníme v části odkaz.</span><span class="sxs-lookup"><span data-stu-id="4a12d-119">This guide will be published soon in the reference section.</span></span>

* <span data-ttu-id="4a12d-120">`<DisplayName>`je řetězec, který definuje uživatelsky orientovaný *popisek*</span><span class="sxs-lookup"><span data-stu-id="4a12d-120">`<DisplayName>` is a string that defines the user-facing *label*</span></span>

* <span data-ttu-id="4a12d-121">`<UserHelpText>`pomáhá uživateli pochopit, co je požadováno</span><span class="sxs-lookup"><span data-stu-id="4a12d-121">`<UserHelpText>` helps the user understand what is required</span></span>

* <span data-ttu-id="4a12d-122">`<UserInputType>`obsahuje následující čtyři možnosti zvýrazněná níže:</span><span class="sxs-lookup"><span data-stu-id="4a12d-122">`<UserInputType>` has the following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="4a12d-123">`RadioSingleSelectduration`-Vynucuje jednoho výběru.</span><span class="sxs-lookup"><span data-stu-id="4a12d-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
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

    * <span data-ttu-id="4a12d-124">`DropdownSingleSelect`-Umožňuje výběr pouze platnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4a12d-124">`DropdownSingleSelect` - Allows the selection of only valid value.</span></span>

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


* <span data-ttu-id="4a12d-126">`CheckboxMultiSelect`Umožňuje výběr jednoho nebo více hodnot.</span><span class="sxs-lookup"><span data-stu-id="4a12d-126">`CheckboxMultiSelect` Allows for the selection of one or more values.</span></span>

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

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a><span data-ttu-id="4a12d-128">Přidá deklaraci k přihlašovací nahoru nebo přihlášení uživatele cesty</span><span class="sxs-lookup"><span data-stu-id="4a12d-128">Add the claim to the sign up/sign in user journey</span></span>

1. <span data-ttu-id="4a12d-129">Přidání deklarace identity jako `<OutputClaim ClaimTypeReferenceId="city"/>` k TechnicalProfile `LocalAccountSignUpWithLogonEmail` (nalezené v souboru TrustFrameworkBase zásad).</span><span class="sxs-lookup"><span data-stu-id="4a12d-129">Add the claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` to the TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in the TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="4a12d-130">Všimněte si, že tento TechnicalProfile používá SelfAssertedAttributeProvider.</span><span class="sxs-lookup"><span data-stu-id="4a12d-130">Note this TechnicalProfile uses the SelfAssertedAttributeProvider.</span></span>

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
      <!-- Optional claims, to be collected from the user -->
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

2. <span data-ttu-id="4a12d-131">Přidá deklaraci do AAD-UserWriteUsingLogonEmail jako `<PersistedClaim ClaimTypeReferenceId="city" />` zápis deklarace identity k adresáři AAD po shromáždění od uživatele.</span><span class="sxs-lookup"><span data-stu-id="4a12d-131">Add the claim to the AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` to write the claim to the AAD directory after collecting it from the user.</span></span> <span data-ttu-id="4a12d-132">Pokud nechcete zachovat deklarace identity v adresáři pro budoucí použití, může tento krok přeskočte.</span><span class="sxs-lookup"><span data-stu-id="4a12d-132">You may skip this step if you prefer not to persist the claim in the directory for future use.</span></span>

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

3. <span data-ttu-id="4a12d-133">Přidá deklaraci k TechnicalProfile, který čte z adresáře, když se uživatel přihlásí jako`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="4a12d-133">Add the claim to the TechnicalProfile that reads from the directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for the provided user ID.</Item>
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

4. <span data-ttu-id="4a12d-134">Přidat `<OutputClaim ClaimTypeReferenceId="city" />` zásad RP souboru SignUporSignIn.xml tak tuto deklaraci posílá po úspěšné uživatele cestu aplikace v tokenu.</span><span class="sxs-lookup"><span data-stu-id="4a12d-134">Add the `<OutputClaim ClaimTypeReferenceId="city" />` to the RP policy file SignUporSignIn.xml so this claim is sent to the application in the token after a successful user journey.</span></span>

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

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="4a12d-135">Otestovat vlastní zásady pomocí "Spustit nyní"</span><span class="sxs-lookup"><span data-stu-id="4a12d-135">Test the custom policy using "Run Now"</span></span>

1. <span data-ttu-id="4a12d-136">Otevřete **okno Azure AD B2C** a přejděte do **Identity rozhraní Framework > vlastní zásady**.</span><span class="sxs-lookup"><span data-stu-id="4a12d-136">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="4a12d-137">Vyberte vlastní zásady, který jste nahráli a klikněte na **spustit nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4a12d-137">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="4a12d-138">Nyní byste měli mít zaregistrovat pomocí e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="4a12d-138">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="4a12d-139">Na obrazovce registrace v testovacím režimu by měl vypadat podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="4a12d-139">The signup screen in test mode should look similar to this:</span></span>

![Snímek obrazovky upravené možnost zápisu](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="4a12d-141">Token zpět do aplikace bude teď obsahovat `city` deklarace identity, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="4a12d-141">The token back to your application will now include the `city` claim as shown below</span></span>
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

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="4a12d-142">Volitelné: Ověření e-mailu odeberte z cesty registrace</span><span class="sxs-lookup"><span data-stu-id="4a12d-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="4a12d-143">Pokud chcete přeskočit ověření e-mailu, Autor zásady můžete odebrat `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="4a12d-143">To skip email verification, the policy author can choose to remove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="4a12d-144">E-mailová adresa bude potřeba ale není ověřen, pokud "Požadavky" = true se odebere.</span><span class="sxs-lookup"><span data-stu-id="4a12d-144">The email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="4a12d-145">Pečlivě zvažte, pokud tato možnost není pravé pro vaše případy použití!</span><span class="sxs-lookup"><span data-stu-id="4a12d-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="4a12d-146">Ověřit e-mailu je povolena ve výchozím nastavení `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` v souboru TrustFrameworkBase zásad sadě starter:</span><span class="sxs-lookup"><span data-stu-id="4a12d-146">Verified email is enabled by default in the `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in the TrustFrameworkBase policy file in the starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="4a12d-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4a12d-147">Next steps</span></span>

<span data-ttu-id="4a12d-148">Přidejte novou deklaraci na toky pro sociálních účet přihlášení změnou TechnicalProfiles uvedené níže.</span><span class="sxs-lookup"><span data-stu-id="4a12d-148">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed below.</span></span> <span data-ttu-id="4a12d-149">Ty se používají sociálního/federovaný účet přihlášení k zápisu a čtení dat uživatele pomocí alternativeSecurityId jako Lokátor.</span><span class="sxs-lookup"><span data-stu-id="4a12d-149">These are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
