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
# <a name="azure-active-directory-b2c-modify-sign-up-tooadd-new-claims-and-configure-user-input"></a>Azure Active Directory B2C: Upravte registrace tooadd nových deklarací identity a konfigurovat uživatelský vstup.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

V tomto článku budete přidávat nové registrace uživatele cesty tooyour položka (deklarace identity) uživatel zadal.  Bude nakonfigurovat hello položku jako rozevírací seznam a definovat v případě potřeby.

Upravená Sipi tootrigger testovací předání.

## <a name="prerequisites"></a>Požadavky

* Dokončení hello kroků v článku hello [Začínáme se zásadami vlastní](active-directory-b2c-get-started-custom.md).  Otestujte hello registrace nebo přihlášení uživatele cesty toosignup nové místní účet, než budete pokračovat.


Shromažďování dat prvotní od uživatelů se dosahuje prostřednictvím registrace nebo přihlášení.  Další deklarace identity se dají shromáždit později prostřednictvím profilu upravit uživatele cesty. Kdykoliv Azure AD B2C shromáždí informace o přímo od uživatele hello interaktivně, hello Identity Framework prostředí používá jeho `selfasserted provider`. Hello postup použít, kdykoliv se používá tohoto zprostředkovatele.


## <a name="define-hello-claim-its-display-name-and-hello-user-input-type"></a>Zadejte hello deklarace identity, jeho zobrazovaný název a hello typ vstupu uživatele
Umožňuje, požádejte hello uživatele pro jejich město.  Přidejte následující element toohello hello `<ClaimsSchema>` element v souboru zásad TrustFrameWorkExtensions hello:

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
Existují další možnosti, které můžete tady vytvořit toocustomize hello deklarací identity.  Úplné schéma, najdete v části toohello **Identity rozhraní Framework technické referenční příručka**.  Tato příručka brzy zveřejníme v části odkaz hello.

* `<DisplayName>`je řetězec, který definuje uživatelsky orientovaný hello *popisek*

* `<UserHelpText>`pomáhá hello uživateli pochopit, co je požadováno

* `<UserInputType>`hello následující čtyři možnosti zdůraznily níže:
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * `RadioSingleSelectduration`-Vynucuje jednoho výběru.
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

    * `DropdownSingleSelect`-Umožňuje výběr hello pouze platné hodnoty.

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


* `CheckboxMultiSelect`Umožňuje výběr hello jeden nebo více hodnot.

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

## <a name="add-hello-claim-toohello-sign-upsign-in-user-journey"></a>Přidat až hello deklarace identity toohello přihlášení nebo přihlášení uživatele cesty

1. Přidá deklaraci hello jako `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (nalezené v souboru zásad TrustFrameworkBase hello).  Všimněte si, že tento TechnicalProfile používá hello SelfAssertedAttributeProvider.

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

2. Přidání deklarace identity toohello hello AAD UserWriteUsingLogonEmail jako `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello deklarace identity toohello AAD directory po shromáždění od uživatele hello. Pokud dáváte přednost není toopersist hello deklarace identity v adresáři hello pro budoucí použití, může tento krok přeskočte.

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

3. Přidání deklarace identity toohello hello TechnicalProfile, který čte z adresáře hello, když se uživatel přihlásí jako`<OutputClaim ClaimTypeReferenceId="city" />`

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

4. Přidat hello `<OutputClaim ClaimTypeReferenceId="city" />` soubor zásad RP toohello SignUporSignIn.xml tak touto deklarací identity se odešlou toohello aplikace v tokenu hello po úspěšné uživatele cesty.

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

## <a name="test-hello-custom-policy-using-run-now"></a>Testování hello "Spustit nyní" pomocí vlastních zásad

1. Otevřete hello **okno Azure AD B2C** a přejděte příliš**Identity rozhraní Framework > vlastní zásady**.
2. Vyberte hello vlastní zásadu, kterou jste nahráli a klikněte na tlačítko hello **spustit nyní** tlačítko.
3. Musí být schopný toosign díky e-mailovou adresu.

úvodní obrazovka registrace v testovacím režimu by měl vypadat podobně jako toothis:

![Snímek obrazovky upravené možnost zápisu](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  Hello tokenu back tooyour aplikace bude teď obsahovat hello `city` deklarace identity, jak je uvedeno níže
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

## <a name="optional-remove-email-verification-from-signup-journey"></a>Volitelné: Ověření e-mailu odeberte z cesty registrace

ověření e-mailu tooskip, Autor zásad hello vybrat tooremove `PartnerClaimType="Verified.Email"`. Hello e-mailovou adresu bude potřeba, ale není ověřen, pokud: vyžaduje"= true se odebere.  Pečlivě zvažte, pokud tato možnost není pravé pro vaše případy použití!

Ověřit e-mailu je povoleno ve výchozím nastavení v hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` v souboru zásad TrustFrameworkBase hello sadě starter hello:
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a>Další kroky

Změnou hello níže uvedená TechnicalProfiles přidáte hello novou deklaraci identity toohello toky pro sociálních účet přihlášení. Jsou používané toowrite sociálního/federovaný účet přihlášení a čtení dat uživatele hello pomocí hello alternativeSecurityId jako hello lokátoru.
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
