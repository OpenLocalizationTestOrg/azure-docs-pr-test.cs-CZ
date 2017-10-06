---
title: "Azure Active Directory B2C: REST API deklarací výměnu jako ověření | Microsoft Docs"
description: "Téma na Azure Active Directory B2C vlastní zásady"
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
ms.openlocfilehash: cec6c6e110514a8bbe0e0780f36738ff21ae2f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako ověření na vstup uživatele

Hello Identity rozhraní Framework (IEF) podkladovou Azure Active Directory B2C (Azure AD B2C) umožňuje hello identity vývojáře toointegrate interakci s rozhraní RESTful API cesty uživatele.  

Na konci hello tohoto návodu bude možné toocreate cesty uživatele Azure AD B2C, která interaguje s služby RESTful.

Hello IEF odesílá data v deklaracích identity a přijímá data zpět v deklaracích identity. Hello interakci s hello rozhraní API:

- Můžete třeba navrhnout jako deklarace identity systému exchange REST API nebo jako profil ověření, který se stane uvnitř na krok orchestration.
- Obvykle ověří vstup od uživatele hello. Pokud je hodnota hello od uživatele hello zamítnutí, hello uživatele můžete to zkusit znovu tooenter platnou hodnotu s hello možnost tooreturn chybovou zprávu.

Můžete taky navrhnout hello interakce jako krok orchestration. Další informace najdete v tématu [návod: integrace rozhraní API REST deklarací výměn v vám dobře slouží uživatele Azure AD B2C jako krok orchestration](active-directory-b2c-rest-api-step-custom.md).

Například profil hello ověření použijeme hello profil upravit uživatele cesty v souboru pack starter hello ProfileEdit.xml.

Zadaný uživatelem hello v profilu hello upravit není součástí seznamu vyloučení, abychom mohli ověřit tento název hello.

## <a name="prerequisites"></a>Požadavky

- Toocomplete nakonfigurovat klienta Azure AD B2C místní účet registrace-množství nebo přihlášení, jak je popsáno v [Začínáme](active-directory-b2c-get-started-custom.md).
- Toointeract koncový bod REST API s. V tomto návodu, jsme zřídili ukázkový web s názvem [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) službou REST API.

## <a name="step-1-prepare-hello-rest-api-function"></a>Krok 1: Příprava funkce rozhraní API REST hello

> [!NOTE]
> Instalace funkce rozhraní API REST je mimo rámec tohoto článku hello. [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) poskytuje vynikající toolkit toocreate RESTful služby v cloudu hello.

Vytvořili jsme Azure funkce, která obdrží deklarace identity, která se očekává, že jako `playerTag`. Funkce Hello ověřuje, zda existuje tuto deklaraci. Dostanete kód dokončení Azure funkce hello v [Githubu](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"hello player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

Hello IEF očekává hello `userMessage` deklarací identity vrátí tuto hello Azure funkce. Toto tvrzení zobrazí jako uživatel toohello řetězec Pokud hello ověření selže, například když 409 – konflikt stavu je vrácený v předchozím příkladu hello.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Krok 2: Konfigurace exchange deklarace identity rozhraní RESTful API hello jako technické profil v souboru TrustFrameworkExtensions.xml

Technické profil je hello úplná konfigurace systému exchange hello požadovaného s hello služba RESTful. Otevřete soubor TrustFrameworkExtensions.xml hello a přidejte následující fragment kódu XML uvnitř hello hello `<ClaimsProviders>` elementu.

> [!NOTE]
> V následující XML, RESTful zprostředkovatele hello `Version=1.0.0.0` označen jako protokol hello. Považuje za jako hello funkce, která bude komunikovat s externí služba hello. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Hello `InputClaims` element definuje hello deklarace identity, které budou odesílané z hello IEF toohello REST služby. V tomto příkladu hello obsah deklarace identity hello `givenName` odešle služba REST toohello jako `playerTag`. V tomto příkladu hello IEF neočekává deklarací zpět. Místo toho čeká na odpověď od služby REST hello a jednání podle hello stavové kódy, které obdrží.

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a>Krok 3: Zahrnout exchange deklarace identity služba RESTful hello samoobslužné uplatňovaná technické profil místo toovalidate hello uživatelský vstup

v hello interakce s uživatelem se nejčastěji používá Hello hello ověření kroku. Všechny interakce, kde uživatel hello je očekávané tooprovide vstupní jsou *samoobslužné prohlašovanou technické profily*. V tomto příkladu přidáme hello ověření toohello samoobslužných Asserted ProfileUpdate technické profilu. Toto je hello technické profil, který hello soubor zásad předávající stranu `Profile Edit` používá.

toohello exchange deklarace identity hello tooadd samoobslužné prohlašovanou technické profil:

1. Otevřete soubor TrustFrameworkBase.xml hello a vyhledejte `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Zkontrolujte konfiguraci hello tohoto technické profilu. Sledujte, jak je hello exchange s hello uživatelem definovaný jako deklarace identity, které se dotaz uživatele hello (vstupních deklarací identity) a deklarací identity, které se budou zpět od zprostředkovatele samoobslužné uplatňovaná hello (výstup deklarace identity).
3. Vyhledejte `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`a Všimněte si, že tento profil je vyvolána jako orchestration krok 6 `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a>Krok 4: Nahrání a testovací soubor zásad RP upravit profil hello

1. Nahrajte hello nová verze souboru TrustFrameworkExtensions.xml hello.
2. Použití **spustit nyní** tootest hello profil upravit soubor zásad RP.
3. Test ověření hello některým z existující názvy hello (například mcvinny) v hello **křestní jméno** pole. Pokud všechno, co je správně nastavena, měli byste obdržet zprávu, která upozorní uživatele hello že hello player značky se již používá.

## <a name="next-steps"></a>Další kroky

[Upravit hello profil upravit a uživatel toogather Další informace o registraci od uživatelů](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Návod: Integrace rozhraní REST API deklarace identity výměn v vám dobře slouží Azure AD B2C uživatele jako krok orchestration](active-directory-b2c-rest-api-step-custom.md)
