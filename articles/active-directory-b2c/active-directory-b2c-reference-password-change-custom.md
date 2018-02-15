---
title: "Azure Active Directory B2C: Změna hesla pomocí samoobslužné služby | Microsoft Docs"
description: "Téma, který ukazuje, jak nastavit změnu hesla pomocí samoobslužné služby pro uživatele v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: mtillman
ms.assetid: 712a7128-5788-4914-8a52-24e200aa4de1
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: 76e7ed328716d09dc57e25f15c411f07fda77bb9
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a><span data-ttu-id="78e54-103">Azure Active Directory B2C: Konfigurace Změna hesla v vlastní zásady</span><span class="sxs-lookup"><span data-stu-id="78e54-103">Azure Active Directory B2C: Configure password change in custom policies</span></span>  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="78e54-104">Funkce změnu hesla s přihlášeného příjemci (pomocí místní účty) můžete změnit jejich hesla bez nutnosti potvrzení jejich pravosti pomocí ověření e-mailu, jak je popsáno v [toku resetování hesla pomocí samoobslužné služby.](active-directory-b2c-reference-sspr.md)</span><span class="sxs-lookup"><span data-stu-id="78e54-104">With the password change feature, signed-in consumers (using local accounts) can change their passwords without having to prove their authenticity by email verification as described in the [self-service password reset flow.](active-directory-b2c-reference-sspr.md)</span></span> <span data-ttu-id="78e54-105">Pokud relace vyprší v čase příjemce získá heslo změnit toku, uživatel je vyzván, aby znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="78e54-105">If the session expires by the time the consumer gets to password change flow, user is prompted to sign in again.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="78e54-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="78e54-106">Prerequisites</span></span>

<span data-ttu-id="78e54-107">Klient služby Azure AD B2C, nakonfigurované k dokončení registrace-množství nebo přihlášení, místní účet, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="78e54-107">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="how-to-configure-password-change-in-custom-policy"></a><span data-ttu-id="78e54-108">Postup konfigurace ve vlastních zásadách pro změnu hesla</span><span class="sxs-lookup"><span data-stu-id="78e54-108">How to configure password change in custom policy</span></span>

<span data-ttu-id="78e54-109">Změna hesla ve vlastních zásadách pro konfigurace proveďte následující změny v zásadách rozšíření framework vztahu důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="78e54-109">To configure password change in custom policy make the following changes in your trust framework extensions policy,</span></span> 

## <a name="define-a-claimtype-oldpassword"></a><span data-ttu-id="78e54-110">Definování typ ClaimType 'Původní_heslo.</span><span class="sxs-lookup"><span data-stu-id="78e54-110">Define a ClaimType 'oldPassword'</span></span>

<span data-ttu-id="78e54-111">Celková struktura vaše vlastní zásada musí obsahovat `ClaimsSchema`a definovat novou `ClaimType` 'Původní_heslo, jak je uvedeno níže,</span><span class="sxs-lookup"><span data-stu-id="78e54-111">The overall structure of your custom policy must include a `ClaimsSchema`and define a new `ClaimType` 'oldPassword' as below,</span></span> 

```XML
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="oldPassword">
        <DisplayName>Old Password</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>Enter password</UserHelpText>
        <UserInputType>Password</UserInputType>
      </ClaimType>
    </ClaimsSchema>
  </BuildingBlocks>
```

<span data-ttu-id="78e54-112">Účelem těchto prvků vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="78e54-112">The purpose of these elements is as follows:</span></span>

- <span data-ttu-id="78e54-113">`ClaimsSchema` Definuje, které deklarace identity je ověřován.</span><span class="sxs-lookup"><span data-stu-id="78e54-113">The `ClaimsSchema` defines which claim is being validated.</span></span>  <span data-ttu-id="78e54-114">V takovém případě bude ověřeno staré heslo.</span><span class="sxs-lookup"><span data-stu-id="78e54-114">In this case, the 'old password' will be validated.</span></span> 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a><span data-ttu-id="78e54-115">Přidání poskytovatele deklarací identity změnu hesla s jeho podpůrné elementy</span><span class="sxs-lookup"><span data-stu-id="78e54-115">Add a password change claims provider with its supporting elements</span></span>

<span data-ttu-id="78e54-116">Deklarace identity, které bude zprostředkovatel pro změnu hesla</span><span class="sxs-lookup"><span data-stu-id="78e54-116">Password change claims provider will</span></span>

1. <span data-ttu-id="78e54-117">Ověření uživatele vůči staré heslo</span><span class="sxs-lookup"><span data-stu-id="78e54-117">Authenticate the user against the old password</span></span>
2. <span data-ttu-id="78e54-118">A pokud 'nové heslo, odpovídá "potvrzení nového hesla", tato hodnota je uložena v úložišti dat B2C, a proto je heslo úspěšně změněno.</span><span class="sxs-lookup"><span data-stu-id="78e54-118">And if 'new password' matches 'confirm new password', this value is stored in B2C datastore and hence the password is successfully changed.</span></span> 

![obrázek](images/passwordchange.jpg)

<span data-ttu-id="78e54-120">Přidejte následující zprostředkovatele deklarací identity do vaší zásady rozšíření.</span><span class="sxs-lookup"><span data-stu-id="78e54-120">Add the following claims provider to your extensions policy.</span></span> 

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
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="login-NonInteractive-PasswordChange">
          <DisplayName>Local Account SignIn</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
            <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
            <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
            <Item Key="ProviderName">https://sts.windows.net/</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
            <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
            <Item Key="response_types">id_token</Item>
            <Item Key="response_mode">query</Item>
            <Item Key="scope">email openid</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
            <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
            <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
            <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
            <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Local Account Password Change</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
          <DisplayName>Change password (username)</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
          </CryptographicKeys>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
            <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
```



### <a name="add-the-application-ids-to-your-custom-policy"></a><span data-ttu-id="78e54-121">Přidat ID aplikace do vlastních zásad</span><span class="sxs-lookup"><span data-stu-id="78e54-121">Add the application IDs to your custom policy</span></span>

<span data-ttu-id="78e54-122">Přidat ID aplikace souboru rozšíření (`TrustFrameworkExtensions.xml`):</span><span class="sxs-lookup"><span data-stu-id="78e54-122">Add the application IDs to the extensions file (`TrustFrameworkExtensions.xml`):</span></span>

1. <span data-ttu-id="78e54-123">V souboru rozšíření (TrustFrameworkExtensions.xml) nalezen element `<TechnicalProfile Id="login-NonInteractive">` a `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`</span><span class="sxs-lookup"><span data-stu-id="78e54-123">In the extensions file (TrustFrameworkExtensions.xml), find the element `<TechnicalProfile Id="login-NonInteractive">` and `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`</span></span>

2. <span data-ttu-id="78e54-124">Nahraďte všechny výskyty `IdentityExperienceFrameworkAppId` s ID aplikace Framework prostředí Identity aplikace, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="78e54-124">Replace all instances of `IdentityExperienceFrameworkAppId` with the application ID of the Identity Experience Framework application as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="78e54-125">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="78e54-125">Here is an example:</span></span>

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. <span data-ttu-id="78e54-126">Nahraďte všechny výskyty `ProxyIdentityExperienceFrameworkAppId` s ID aplikace Framework prostředí Identity Proxy aplikace, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="78e54-126">Replace all instances of `ProxyIdentityExperienceFrameworkAppId` with the application ID of the Proxy Identity Experience Framework application as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>

4. <span data-ttu-id="78e54-127">Uložte soubor rozšíření.</span><span class="sxs-lookup"><span data-stu-id="78e54-127">Save your extensions file.</span></span>



## <a name="create-a-password-change-user-journey"></a><span data-ttu-id="78e54-128">Vytvoření cesty uživatel změnit heslo</span><span class="sxs-lookup"><span data-stu-id="78e54-128">Create a password change user journey</span></span>

```XML
 <UserJourneys>
    <UserJourney Id="PasswordChange">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

<span data-ttu-id="78e54-129">Dokončení úpravy souboru rozšíření.</span><span class="sxs-lookup"><span data-stu-id="78e54-129">You are done modifying the extension file.</span></span> <span data-ttu-id="78e54-130">Uložte a nahrajte tento soubor.</span><span class="sxs-lookup"><span data-stu-id="78e54-130">Save and upload this file.</span></span> <span data-ttu-id="78e54-131">Ujistěte se, že všechny ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="78e54-131">Ensure that all validations succeed.</span></span>



## <a name="create-a-relying-party-rp-file"></a><span data-ttu-id="78e54-132">Vytvořte soubor předávající stranu</span><span class="sxs-lookup"><span data-stu-id="78e54-132">Create a relying party (RP) file</span></span>

<span data-ttu-id="78e54-133">Potom aktualizujte soubor předávající stranu, který iniciuje cesty uživatele, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="78e54-133">Next, update the relying party (RP) file that initiates the user journey that you created:</span></span>

1. <span data-ttu-id="78e54-134">Vytvořte kopii ProfileEdit.xml v pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="78e54-134">Make a copy of ProfileEdit.xml in your working directory.</span></span> <span data-ttu-id="78e54-135">Potom přejmenujte ji (například PasswordChange.xml).</span><span class="sxs-lookup"><span data-stu-id="78e54-135">Then, rename it (for example, PasswordChange.xml).</span></span>
2. <span data-ttu-id="78e54-136">Otevřete nový soubor a aktualizace `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="78e54-136">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="78e54-137">Toto je název vaší zásady (například PasswordChange).</span><span class="sxs-lookup"><span data-stu-id="78e54-137">This is the name of your policy (for example, PasswordChange).</span></span>
3. <span data-ttu-id="78e54-138">Změnit `ReferenceId` atribut `<DefaultUserJourney>` tak, aby odpovídala `Id` nové cesty uživatele, který jste vytvořili (například PasswordChange).</span><span class="sxs-lookup"><span data-stu-id="78e54-138">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the `Id` of the new user journey that you created (for example, PasswordChange).</span></span>
4. <span data-ttu-id="78e54-139">Uložte změny a potom soubor odešlete.</span><span class="sxs-lookup"><span data-stu-id="78e54-139">Save your changes, and then upload the file.</span></span>
5. <span data-ttu-id="78e54-140">Pokud chcete otestovat vlastní zásady, který jste nahráli, na portálu Azure, přejděte do okna zásady a pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="78e54-140">To test the custom policy that you uploaded, in the Azure portal, go to the policy blade, and then click **Run now**.</span></span>




## <a name="link-to-password-change-sample-policy"></a><span data-ttu-id="78e54-141">Odkaz na heslo změnit ukázkové zásady</span><span class="sxs-lookup"><span data-stu-id="78e54-141">Link to password change sample policy</span></span>

<span data-ttu-id="78e54-142">Můžete najít ukázkové zásady [zde](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change).</span><span class="sxs-lookup"><span data-stu-id="78e54-142">You can find the sample policy [here](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change).</span></span> 










