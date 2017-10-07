---
title: "Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad"
description: "Ukázku pomocí Google + jako zprostředkovatele identity pomocí protokolu OAuth2"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 4feff21979c9c3b3b12c7a1cae4db0121d1bd79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Přidejte Google + jako zprostředkovatele identity OAuth2 pomocí vlastních zásad

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Tento průvodce vám ukáže, jak tooenable přihlášení pro uživatele z účtu Google + prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Požadavky

Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.

K těmto krokům patří:

1.  Vytvoření aplikace pro účet Google +.
2.  Přidání hello Google + účet aplikace klíče tooAzure AD B2C
3.  Přidání zásad tooa poskytovatele deklarací identity
4.  Registrace hello Google + účet deklarace identity zprostředkovatele tooa uživatele cesty
5.  Odesílání hello zásad tooan Azure AD B2C klienta a otestovat ji

## <a name="create-a-google-account-application"></a>Vytvoření aplikace účet Google +
toouse Google + jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, potřebujete toocreate aplikace Google + a přiřaďte mu hello správné parametry. Můžete zaregistrovat Google aplikace + zde: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Přejděte toohello [konzole pro vývojáře Google](https://console.developers.google.com/) a přihlaste se pomocí přihlašovací údaje účtu Google +.
2.  Klikněte na tlačítko **vytvořit projekt**, zadejte **název projektu**a potom klikněte na **vytvořit**.

3.  Klikněte na hello **projekty nabídky**.

    ![Google + účet – vyberte projektu](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klikněte na hello  **+**  tlačítko.

    ![Google + účet – vytvoření nového projektu](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Zadejte **název projektu**a potom klikněte na **vytvořit**.

    ![Google + účet – nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Počkejte, dokud je připraven hello projekt a klikněte na hello **projekty nabídky**.

    ![Google + účet – Počkejte, až je připraven toouse nový projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klikněte na název projektu.

    ![Google + účet – nový projekt vyberte hello](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klikněte na tlačítko **rozhraní API Správce** a pak klikněte na **pověření** v levé navigační hello.
9.  Klikněte na tlačítko hello **obrazovky souhlas OAuth** karty v horní části hello.

    ![Google + účet – nastavte OAuth souhlasu obrazovky](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Vyberte nebo zadejte platný **e-mailová adresa**, poskytovat **název produktu**a klikněte na tlačítko **Uložit**.

    ![Google + - přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klikněte na tlačítko **nové přihlašovací údaje** a potom zvolte **ID klienta OAuth**.

    ![Google + - vytvořit nové přihlašovací údaje aplikací](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  V části **typ aplikace**, vyberte **webové aplikace**.

    ![Google + - výběr typu aplikace](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Zadejte **název** pro vaši aplikaci, zadejte `https://login.microsoftonline.com` v hello **zdroje oprávnění JavaScript** pole, a `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **autorizováno přesměrování identifikátory URI**pole. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com). Hello **{klient}** hodnota je malá a velká písmena. Klikněte na možnost **Vytvořit**.

    ![Google + - zadejte oprávnění JavaScript zdroje a identifikátory URI pro přesměrování](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Zkopírujte hodnoty hello **Id klienta** a **tajný klíč klienta**. Musíte obou tooconfigure Google + jako zprostředkovatel identity ve vašem klientovi. **Tajný klíč klienta** je důležitým bezpečnostním pověřením.

    ![Google + - kopírování hodnot hello tajný klíč klienta Id a klienta](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a>Přidat hello Google + účet aplikace klíče tooAzure AD B2C
Federace se Google + účty vyžaduje tajný klíč klienta pro účet Google + tootrust Azure AD B2C jménem aplikace hello. Je třeba toostore vaše Google + tajný klíč aplikace v Azure AD B2C klienta:  

1.  Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**
2.  Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.
3.  Klikněte na tlačítko **+ přidat**.
4.  Pro **možnosti**, použijte **ruční**.
5.  Pro **název**, použijte `GoogleSecret`.  
    Předpona Hello `B2C_1A_` může automaticky přidat.
6.  V hello **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z https://apps.dev.microsoft.com
7.  Pro **použití klíče**, použijte **podpis**.
8.  Klikněte na **Vytvořit**
9.  Potvrďte, že jste vytvořili klíč hello `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Přidání poskytovatele deklarací identity v rozšíření zásady

Pokud chcete, uživatelé toosign pomocí účtu Google +, musíte toodefine Google + účet jako poskytovatele deklarací identity. Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C. koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.

Zadejte účet Google + jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:

1.  Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře. Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.
2.  Najde hello `<ClaimsProviders>` části
3.  Přidejte následující fragment kódu XML v části hello hello `ClaimsProviders` elementu a nahraďte `client_id` hodnotu s vaší Google + účet ID klienta aplikace před uložením souboru hello.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want hello user tooselect his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Zaregistrovat hello Google + účet deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty

Zprostředkovatel identity Hello nastavit.  Však není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky. Přidat hello Google + identity zprostředkovatele tooyour uživatelský účet `SignUpOrSignIn` cesty uživatele. toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.  Potom přidáme hello Google + zprostředkovatele identity účtu:

>[!NOTE]
>
>Pokud jste zkopírovali hello `<UserJourneys>` element ze základního souboru zásad toohello příponu souboru (TrustFrameworkExtensions.xml), můžete toothis část přeskočit.

1.  Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).
2.  Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.
3.  Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu. Pokud hello element neexistuje, přidejte jedno.
4.  Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.  `<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení. Pokud přidáte `<ClaimsProviderSelection>` element pro účet Google + nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello. tooadd tento element:

1.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.
2.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
3.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz
Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce. pro Azure AD B2C toocommunicate s tooreceive účet Google + Hello akce v tomto případě je token.

1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello
> * Ujistěte se, `TechnicalProfileReferenceId` ID je nastavit toohello technické profil vytvoříte starší (Google OAUTH).

## <a name="upload-hello-policy-tooyour-tenant"></a>Nahrát hello zásad tooyour klienta
1.  V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.
2.  Vyberte **Identity rozhraní Framework**.
3.  Otevřete hello **všechny zásady** okno.
4.  Vyberte **nahrát zásady**.
5.  Zkontrolujte **přepsat zásady hello, pokud existuje** pole.
6.  **Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže

## <a name="test-hello-custom-policy-by-using-run-now"></a>Otestovat vlastní zásady hello pomocí spustit nyní
1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.

    >[!NOTE]
    >
    >    **Spustit nyní** vyžaduje alespoň jednu aplikaci toobe preregistered na hello klienta. 
    >    jak zjistit, tooregister aplikace toolearn hello Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článku nebo hello [registrace aplikace](active-directory-b2c-app-registration.md) článku.


2.  Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí Google + účet.

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a>[Nepovinné] Zaregistrovat hello Google + účet deklarace identity zprostředkovatele tooProfile upravit uživatele cesty
Může být vhodné tooadd hello Google + účet zprostředkovatele identity také tooyour uživatele `ProfileEdit` cesty uživatele. toomake je k dispozici, jsme opakujte hello poslední dva kroky:

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
1.  Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).
2.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.
3.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
4.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz
1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní

1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.
2.  Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí Google + účet.

## <a name="download-hello-complete-policy-files"></a>Stáhnout soubory dokončení zásad hello
Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.  [Ukázkové soubory zásad pro – referenční informace](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
