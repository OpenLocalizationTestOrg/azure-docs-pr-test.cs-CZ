---
title: "Azure Active Directory B2C: Přidáte účet Microsoft (MSA) jako zprostředkovatele identity pomocí vlastních zásad"
description: "Ukázka použití Microsoft jako zprostředkovatele identity pomocí protokolu OpenID Connect (OIDC)"
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
ms.openlocfilehash: 577ac612f69015e6790f2fa9f2cfb42c2b524b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Přidáte účet Microsoft (MSA) jako zprostředkovatele identity pomocí vlastních zásad

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Tento článek ukazuje, jak tooenable přihlášení pro uživatele z účtu Microsoft (MSA) prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Požadavky
Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.

K těmto krokům patří:

1.  Vytvoření aplikace pro účet Microsoft.
2.  Přidání hello Microsoft účet aplikace klíče tooAzure AD B2C
3.  Přidání zásad tooa poskytovatele deklarací identity
4.  Registrace hello Account Microsoft deklarace identity zprostředkovatele tooa uživatele cesty
5.  Odesílání hello zásad tooan Azure AD B2C klienta a otestovat ji

## <a name="create-a-microsoft-account-application"></a>Vytvoření aplikace účtu Microsoft
toouse účet Microsoft jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba toocreate k aplikaci účtu Microsoft a přiřaďte mu hello správné parametry. Potřebujete účet Microsoft. Pokud nemáte, navštivte [https://www.live.com/](https://www.live.com/).

1.  Přejděte toohello [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) a přihlaste se pomocí přihlašovacích údajů účtu Microsoft.
2.  Klikněte na tlačítko **přidat aplikaci**.

    ![Microsoft účet – přidat aplikaci](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Zadejte **název** pro vaši aplikaci **kontaktovat e-mailu**, zrušte zaškrtnutí políčka **dejte nám vám pomohli začít** a klikněte na tlačítko **vytvořit**.

    ![Účet Microsoft - registrace vaší aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Zkopírujte hodnotu hello **Id aplikace**. Budete ho potřebovat tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi.

    ![Účet Microsoft - kopie hello hodnota ID aplikace](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Klikněte na **přidat platformy**

    ![Microsoft účet – přidejte platformu](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Z hello seznam platforem, zvolte **webové**.

    ![Účet Microsoft - hello platformy seznamu zvolte Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Zadejte `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` v hello **identifikátory URI přesměrování** pole. Nahraďte **{klient}** s názvem vašeho klienta (například contosob2c.onmicrosoft.com).

    ![Účet Microsoft - Set přesměrování adresy URL](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Klikněte na **generovat nové heslo** pod hello **tajné klíče aplikace** části. Zkopírujte hello nové heslo zobrazené na obrazovce. Budete ho potřebovat tooconfigure účet Microsoft jako zprostředkovatel identity ve vašem klientovi. Toto heslo je důležitým bezpečnostním pověřením.

    ![Microsoft účet - vytvořit nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Účet Microsoft - kopie hello nové heslo](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Zaškrtávací políčko hello, která uvádí, že **Live SDK podporu** pod hello **pokročilé možnosti** části. Klikněte na **Uložit**.

    ![Účet Microsoft - Live SDK podpory](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-hello-microsoft-account-application-key-tooazure-ad-b2c"></a>Přidat hello Microsoft účet aplikace klíče tooAzure AD B2C
Federace s účty Microsoft vyžaduje tajný klíč klienta pro tootrust účet Microsoft Azure AD B2C jménem aplikace hello. Je třeba toostore vaše secert Microsoft účet aplikace v Azure AD B2C klienta:   

1.  Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**
2.  Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.
3.  Klikněte na tlačítko **+ přidat**.
4.  Pro **možnosti**, použijte **ruční**.
5.  Pro **název**, použijte `MSASecret`.  
    Předpona Hello `B2C_1A_` může automaticky přidat.
6.  V hello **tajný klíč** zadejte váš tajný klíč aplikace Microsoft z https://apps.dev.microsoft.com
7.  Pro **použití klíče**, použijte **podpis**.
8.  Klikněte na **Vytvořit**
9.  Potvrďte, že jste vytvořili klíč hello `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Přidání poskytovatele deklarací identity v rozšíření zásady
Pokud chcete uživatelům toosign v Account Microsoft, je třeba toodefine Account Microsoft jako poskytovatele deklarací identity. Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C. koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.

Zadejte Microsoft Account jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:

1.  Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře. Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.
2.  Najde hello `<ClaimsProviders>` části
3.  Přidejte následující fragment kódu XML v části hello `ClaimsProviders` element:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Nahraďte `client_id` hodnota se váš klient aplikace Microsoft Account Id

5.  Uložte soubor hello.

## <a name="register-hello-microsoft-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Zaregistrovat hello Account Microsoft deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty

V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky. Nyní je třeba tooadd hello Account Microsoft identity zprostředkovatele tooyour uživatele `SignUpOrSignIn` cesty uživatele. toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.  Potom přidáme zprostředkovatele identity Account Microsoft hello:

> [!NOTE]
>
>Pokud jste dříve zkopírovali hello `<UserJourneys>` element ze základního souboru souboru rozšíření zásad toohello `TrustFrameworkExtensions.xml`, můžete toothis část přeskočit.

1.  Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).
2.  Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.
3.  Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu. Pokud hello element neexistuje, přidejte jedno.
4.  Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.  `<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení. Pokud přidáte `<ClaimsProviderSelection>` element pro účet Microsoft, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello. tooadd tento element:

1.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.
2.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
3.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz
Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce. pro Azure AD B2C toocommunicate s tooreceive Account Microsoft Hello akce v tomto případě je token. Odkaz akce tooan tlačítka hello pomocí propojení hello technické profil pro poskytovatele deklarací identity Account Microsoft:

1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello
>   * Ujistěte se, `TechnicalProfileReferenceId` ID je nastavit toohello technické profil vytvoříte starší (MSA OIDC).

## <a name="upload-hello-policy-tooyour-tenant"></a>Nahrát hello zásad tooyour klienta
1.  V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.
2.  Vyberte **Identity rozhraní Framework**.
3.  Otevřete hello **všechny zásady** okno.
4.  Vyberte **nahrát zásady**.
5.  Zkontrolujte **přepsat zásady hello, pokud existuje** pole.
6.  **Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže

## <a name="test-hello-custom-policy-by-using-run-now"></a>Otestovat vlastní zásady hello pomocí spustit nyní

1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.
> [!NOTE]
>
>**Spustit nyní** vyžaduje alespoň jednu aplikaci toobe preregistered na hello klienta. jak zjistit, tooregister aplikace toolearn hello Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článku nebo hello [registrace aplikace](active-directory-b2c-app-registration.md) článku.
2.  Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí účtu Microsoft.

## <a name="optional-register-hello-microsoft-account-claims-provider-tooprofile-edit-user-journey"></a>[Nepovinné] Zaregistrovat hello Account Microsoft deklarace identity zprostředkovatele tooProfile upravit uživatele cesty
Může být vhodné zprostředkovatele identity Account Microsoft hello tooadd také tooyour uživatele `ProfileEdit` cesty uživatele. toomake je k dispozici, jsme opakujte hello poslední dva kroky:

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
1.  Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).
2.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.
3.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
4.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz
1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní
1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.
2.  Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí účtu Microsoft.

## <a name="download-hello-complete-policy-files"></a>Stáhnout soubory dokončení zásad hello
Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede namísto použití tyto ukázkové soubory.  [Ukázkové soubory zásad pro – referenční informace](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
