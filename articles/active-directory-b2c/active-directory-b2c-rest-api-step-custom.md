---
title: "Azure Active Directory B2C: REST API deklarací výměnu jako krok orchestration | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady, které se integrují s rozhraní API"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration

Hello Identity rozhraní Framework (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje hello identity vývojáře toointegrate interakci s rozhraní RESTful API cesty uživatele.  

Na konci hello tohoto návodu bude možné toocreate cesty uživatele Azure AD B2C, která interaguje s služby RESTful.

Hello IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity. Hello exchange deklarace identity rozhraní REST API:

- Můžete třeba navrhnout jako krok orchestration.
- Můžete aktivovat externí akce. Například se můžete přihlásit jako událost v externí databáze.
- Můžou být použité toofetch hodnota a uloží jej v uživatelské databázi hello.

Můžete použít deklarace identity hello přijatých novější toku toochange hello spouštění.

Můžete taky navrhnout hello interakce jako profil ověření. Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele](active-directory-b2c-rest-api-validation-custom.md).

scénář Hello je, že když uživatel provede úpravy profilu, chceme:

1. Vyhledání uživatele hello v externím systému.
2. Získáte hello Město, kde je tento uživatel zaregistrován.
3. Vrátí tuto aplikaci toohello atribut jako deklarace identity.

## <a name="prerequisites"></a>Požadavky

- Toocomplete nakonfigurovat klienta Azure AD B2C místní účet registrace-množství nebo přihlášení, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).
- Toointeract koncový bod REST API s. Tento návod používá jako příklad webhook, jehož jednoduché Azure funkce aplikace.
- *Doporučená*: dokončení hello [REST API deklarací exchange návod jako krok ověření](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-hello-rest-api-function"></a>Krok 1: Příprava funkce rozhraní API REST hello

> [!NOTE]
> Instalace funkce rozhraní API REST je mimo rámec tohoto článku hello. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající toolkit toocreate RESTful služby v cloudu hello.

Nastavíme Azure funkce, která obdrží deklarace identity názvem `email`, a potom vrátí hello deklarace identity `city` s hodnotou hello přiřazené `Redmond`. Ukázka Hello Azure funkce je na [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

Hello `userMessage` deklaraci identity, kterou vrátí funkce Azure hello je volitelná v tomto kontextu a hello IEF ho budou ignorovat. Potenciálně můžete ho jako zprávu předán toohello aplikace a později uvedené toohello uživatele.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

Aplikace Azure funkce umožňuje snadno tooget hello funkce URL, která obsahuje identifikátor hello hello konkrétní funkce. V takovém případě je adresa URL hello: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. Můžete ho použít pro testování.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API hello jako technické profil v souboru TrustFrameworExtensions.xml

Technické profil je hello úplná konfigurace systému exchange hello požadovaného s hello služba RESTful. Otevřete soubor TrustFrameworkExtensions.xml hello a přidejte následující fragment kódu XML uvnitř hello hello `<ClaimsProvider>` elementu.

> [!NOTE]
> V následující XML, RESTful zprostředkovatele hello `Version=1.0.0.0` označen jako protokol hello. Považuje za jako hello funkce, která bude komunikovat s externí služba hello. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Hello `<InputClaims>` element definuje hello deklarace identity, které budou odesílané z hello IEF toohello REST služby. V tomto příkladu hello obsah deklarace identity hello `givenName` se budou odesílat služby REST toohello jako deklarace identity hello `email`.  

Hello `<OutputClaims>` element definuje hello deklarace identity tohoto hello IEF bude od služby REST hello očekávat. Bez ohledu na číslo hello deklarací identity, které jsou přijaty hello IEF použije pouze ty, které jsou identifikovány sem. V tomto příkladu se deklarace identity jako přijata `city` bude volána deklarací identity mapovat tooan IEF `city`.

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a>Krok 3: Přidejte novou deklaraci hello `city` toohello schématu TrustFrameworkExtensions.xml souboru

deklarace identity Hello `city` není kdekoli definován zatím v našem schématu. Ano, přidat definici uvnitř elementu hello `<BuildingBlocks>`. Tento element na začátku hello hello TrustFrameworkExtensions.xml soubor můžete najít.

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>Krok 4: Zahrnují výměny deklarací identity služby REST hello jako krok orchestration ve vašem profilu upravit uživatele cesty ve TrustFrameworkExtensions.xml

Přidejte krok toohello profil upravit uživatele cesty po hello uživatel ověřen (postup orchestration 1 – 4 v následující XML hello) a hello uživatel zadal hello aktualizovat informace o profilu (krok 5).

> [!NOTE]
> Existuje mnoho případy použití, kde hello volání rozhraní API REST slouží jako krok orchestration. Krok orchestration ho lze použít jako externí systém tooan aktualizace po uživatele byla úspěšně dokončena úloha jako první registrace, nebo jako profil aktualizovat tookeep informace synchronizované. V takovém případě je použité tooaugment hello informací toohello aplikace po upravit profil hello.

Kopírování hello profil upravit kód XML cesty uživatele ze souboru TrustFrameworkExtensions.xml hello TrustFrameworkBase.xml souboru tooyour uvnitř hello `<UserJourneys>` elementu. Proveďte úpravy hello v kroku 6.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Pokud pořadí hello se neshoduje s vaší verzí, ujistěte se, vložit hello kód jako hello krok před hello `ClaimsExchange` typu `SendClaims`.

Hello konečné XML pro cestu hello uživatel by měl vypadat takto:

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a>Krok 5: Přidejte hello deklarace `city` tooyour předávající strany zásad souborů, deklarace identity hello se odesílají tooyour aplikace

Váš soubor ProfileEdit.xml předávající stranu a upravte hello `<TechnicalProfile Id="PolicyProfile">` element tooadd hello následující: `<OutputClaim ClaimTypeReferenceId="city" />`.

Po přidání novou deklaraci hello technické profil hello vypadá takto:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a>Krok 6: Odešlete své změny a testování

Přepište existující verze hello hello zásad.

1.  (Volitelné:) Uložte hello stávající verzi (podle stahování) souboru rozšíření předtím, než budete pokračovat. Doporučujeme tookeep hello počáteční složitost nízkou nenahrávejte více verzí souboru rozšíření hello.
2.  (Volitelné:) Přejmenujte hello novou verzi hello ID zásad pro soubor úpravy zásad hello změnou `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Nahrajte soubor rozšíření hello.
4.  Nahrajte soubor RP úpravy zásad hello.
5.  Použití **spustit nyní** tootest hello zásad. Zkontrolujte hello token, který hello IEF vrátí toohello aplikace.

Pokud všechno, co je správně nastavena, hello token bude obsahovat nová deklarace identity hello `city`, s hodnotou hello `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Další kroky

[Pomocí rozhraní API REST jako krok ověření](active-directory-b2c-rest-api-validation-custom.md)

[Upravte hello profil upravit toogather Další informace od uživatelů](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
