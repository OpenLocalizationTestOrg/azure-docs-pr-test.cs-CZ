---
title: "Azure Active Directory B2C: Přidejte zprostředkovatele služby Azure AD pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o vlastních zásad Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 9b0c32086cebc171d91da2e7bfb48136723ccd4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Přihlaste se pomocí účtů služby Azure AD

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Tento článek ukazuje, jak tooenable přihlášení pro uživatele z konkrétní organizace Azure Active Directory (Azure AD) prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Požadavky

Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.

K těmto krokům patří:

1. Vytvoření Azure Active Directory B2C klienta (Azure AD B2C).
2. Vytvoření aplikace Azure AD B2C.
3. Probíhá registrace dvě aplikace modul zásad.
4. Nastavení klíče.
5. Nastavení úvodní pack hello.

## <a name="create-an-azure-ad-app"></a>Vytvoření aplikace Azure AD

přihlášení tooenable pro uživatele z konkrétní organizace Azure AD, je nutné tooregister aplikaci v rámci klienta hello organizace Azure AD.

>[!NOTE]
> Používáme "contoso.com" pro klienta hello organizace Azure AD a "fabrikamb2c.onmicrosoft.com" jako klienta hello Azure AD B2C v hello pokynů.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
1. Na horním panelu hello vyberte svůj účet. Z hello **Directory** vyberte místo, kam chcete tooregister klienta hello organizace Azure AD vaší aplikace (contoso.com).
1. Vyberte **další služby** v levém podokně hello a vyhledejte "Aplikace registrace."
1. Vyberte **nové registrace aplikace**.
1. Zadejte název pro vaši aplikaci (například `Azure AD B2C App`).
1. Vyberte **webovou aplikaci nebo API** pro typ aplikace hello.
1. Pro **přihlašovací adresa URL**, zadejte hello následující adresu URL, kde `yourtenant` je nahrazena hello název klienta služby Azure AD B2C (`fabrikamb2c.onmicrosoft.com`):

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Uložit ID hello aplikace.
1. Vyberte aplikaci hello nově vytvořený.
1. V části hello **nastavení** vyberte **klíče**.
1. Vytvořte nový klíč a uložte jej. Budete ho používat v hello kroků v další části hello.

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a>Přidání klíče tooAzure hello Azure AD AD B2C

Je nutné klíč toostore hello contoso.com aplikace v klientovi služby Azure AD B2C. toodo toto:
1. Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework** > **zásad klíče**.
1. Vyberte **+ přidat**.
1. Vyberte nebo zadejte tyto možnosti:
   * Vyberte **ruční**.
   * Pro **název**, zvolte název, který odpovídá název vašeho klienta Azure AD (například `ContosoAppSecret`).  Předpona Hello `B2C_1A_` se automaticky přidá toohello název vašeho klíče.
   * Vložte klíč vaší aplikace hello **tajný klíč** pole.
   * Vyberte **podpis**.
1. Vyberte **Vytvořit**.
1. Potvrďte, že jste vytvořili klíč hello `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Přidání poskytovatele deklarací identity v základní zásady

Pokud chcete uživatelům toosign pomocí Azure AD, je nutné toodefine Azure AD jako zprostředkovatele deklarací identity. Jinými slovy budete potřebovat toospecify koncový bod, který Azure AD B2C bude komunikovat se. koncový bod Hello poskytne sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele. 

Azure AD jako zprostředkovatele deklarací identity můžete definovat přidáním Azure AD toohello `<ClaimsProvider>` uzel v souboru rozšíření bylo hello zásad:

1. Otevřete soubor rozšíření hello (TrustFrameworkExtensions.xml) z pracovního adresáře.
1. Najde hello `<ClaimsProviders>` části. Pokud neexistuje, přidejte ji pod hello kořenový uzel.
1. Přidejte nový `<ClaimsProvider>` uzlu následujícím způsobem:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
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

1. V části hello `<ClaimsProvider>` uzel, hodnota hello aktualizace `<Domain>` tooa jedinečnou hodnotu, kterou lze použít toodistinguish ho od jiných poskytovatelů identit.
1. V části hello `<ClaimsProvider>` uzel, hodnota hello aktualizace `<DisplayName>` tooa popisný název hello deklarací zprostředkovatele. Tato hodnota není právě používána.

### <a name="update-hello-technical-profile"></a>Aktualizovat profil technické hello

tooget tokenu z koncového bodu hello Azure AD, je nutné toodefine hello protokoly, že Azure AD B2C mají používat toocommunicate s Azure AD. To se provádí uvnitř hello `<TechnicalProfile>` element `<ClaimsProvider>`.
1. Aktualizovat hello ID hello `<TechnicalProfile>` uzlu. Toto ID je použité toorefer toothis technické profil z různých částí hello zásad.
1. Aktualizujte hello hodnotu pro `<DisplayName>`. Tato hodnota se zobrazí na hello přihlašovací tlačítko na obrazovce přihlášení.
1. Aktualizujte hello hodnotu pro `<Description>`.
1. Azure AD používá protokol OpenID Connect hello, zajistěte proto tuto hodnotu hello pro `<Protocol>` je `"OpenIdConnect"`.

Je třeba tooupdate hello `<Metadata>` oddíl v souboru XML hello označuje tooearlier tooreflect hello konfigurační nastavení pro konkrétní klienta Azure AD. V souboru XML hello aktualizujte hodnoty metadata hello následujícím způsobem:

1. Nastavit `<Item Key="METADATA">` příliš`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, kde `yourAzureADtenant` je název klienta Azure AD (contoso.com).
1. Otevřete váš prohlížeč a přejděte toohello `METADATA` adresu URL, kterou jste aktualizovali.
1. V prohlížeči hello vyhledejte objekt "vystavitele" hello a zkopírujte jeho hodnotu. By měl vypadat jako následující hello: `https://sts.windows.net/{tenantId}/`.
1. Vložte hodnotu hello `<Item Key="ProviderName">` v souboru XML hello.
1. Nastavit `<Item Key="client_id">` toohello ID aplikace z registrace aplikace hello.
1. Nastavit `<Item Key="IdTokenAudience">` toohello ID aplikace z registrace aplikace hello.
1. Ujistěte se, že `<Item Key="response_types">` je nastaven příliš`id_token`.
1. Ujistěte se, že `<Item Key="UsePolicyInRedirectUri">` je nastaven příliš`false`.

Musíte taky tajný klíč toolink hello Azure AD, který je zaregistrovaný v vaší toohello klienta Azure AD B2C Azure AD `<ClaimsProvider>` informace:

* V hello `<CryptographicKeys>` kapitoly hello předcházející soubor XML, aktualizujte hello hodnotu pro `StorageReferenceId` toohello referenční ID hello tajný klíč, který jste definovali (například `ContosoAppSecret`).

### <a name="upload-hello-extension-file-for-verification"></a>Nahrát soubor hello rozšíření pro ověření

Nyní by jste nakonfigurovali zásady tak, aby Azure AD B2C zná jak toocommunicate s adresářem služby Azure AD. Zkuste odeslat soubor rozšíření hello jenom tooconfirm vaše zásady, nemá žádné problémy, pokud. toodo tak:

1. Otevřete hello **všechny zásady** okno v klientovi služby Azure AD B2C.
1. Zkontrolujte hello **přepsat zásady hello, pokud existuje** pole.
1. Nahrání souboru rozšíření bylo hello (TrustFrameworkExtensions.xml) a ujistěte se, zda text hello ověření neselže.

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a>Zaregistrovat hello Azure AD deklarace identity zprostředkovatele tooa uživatele cesty

Teď musíte tooadd hello Azure AD identity zprostředkovatele tooone z vaší uživatelské cesty. V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky. toomake k dispozici, jsme vytvoří duplicitní existující uživatele cesty šablony a upravit ho tak, aby je také zprostředkovatele identity hello Azure AD:

1. Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).
1. Najde hello `<UserJourneys>` elementu a zkopírujte hello celý `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"`.
1. Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu. Pokud hello element neexistuje, přidejte jedno.
1. Vložení hello celý `<UserJourney>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.
1. Přejmenujte hello ID hello nové uživatele cesty (například přejmenování jako `SignUpOrSignUsingContoso`).

### <a name="display-hello-button"></a>Zobrazení hello "button"

Hello `<ClaimsProviderSelection>` element je obdobou tooan tlačítko zprostředkovatele identity na obrazovce registrace-množství nebo přihlášení. Pokud přidáte `<ClaimsProviderSelection>` element pro Azure AD, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello. tooadd tento element:

1. Najde hello `<OrchestrationStep>` uzlu, který zahrnuje `Order="1"` v hello uživatele cestu, kterou jste právě vytvořili.
1. Přidejte následující hello:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Nastavit `TargetClaimsExchangeId` tooan odpovídající hodnotu. Doporučujeme následující hello stejné konvence jako ostatní:  *\[ClaimProviderName\]Exchange*.

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz

Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce. pro Azure AD B2C toocommunicate s Azure AD tooreceive Hello akce v tomto případě je token. Odkaz hello tlačítko tooan akce pomocí propojení hello technické profil pro poskytovatele deklarací identity služby Azure AD:

1. Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
1. Přidejte následující hello:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Aktualizace `Id` toohello stejnou hodnotu jako `TargetClaimsExchangeId` v předcházející části hello.
1. Aktualizace `TechnicalProfileReferenceId` toohello ID hello technické profilu, kterou jste vytvořili dříve (ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Nahrát soubor aktualizované rozšíření hello

Dokončení změny souboru rozšíření bylo hello. Uložte jej. Nahrajte soubor hello a ujistěte se, že všechny ověření úspěšné.

### <a name="update-hello-rp-file"></a>Aktualizujte soubor RP hello

Teď musíte tooupdate hello předávající stranu soubor, který zahájí hello uživatele cestu, kterou jste právě vytvořili:

1. Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář a přejmenujte ho (například přejmenujte ji tooSignUpOrSignInWithAAD.xml).
1. Otevřete hello nový soubor a aktualizace hello `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu (například SignUpOrSignInWithAAD). <br> To bude název hello zásad (například B2C\_1A\_SignUpOrSignInWithAAD).
1. Upravit hello `ReferenceId` atribut `<DefaultUserJourney>` toomatch hello ID hello nové uživatele cesty vytvořený (SignUpOrSignUsingContoso).
1. Změny uložte a nahrajte soubor hello.

## <a name="troubleshooting"></a>Řešení potíží

Testování hello vlastních zásad, které jste právě nahráli otevřením její okno a kliknutím na **spustit nyní**. toodiagnose problémy, přečtěte si informace o [řešení potíží s](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Další kroky

Poskytnutí zpětné vazby příliš[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).
