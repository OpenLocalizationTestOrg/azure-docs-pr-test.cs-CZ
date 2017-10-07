---
title: "Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS"
description: "Jak tooarticle na nastavení služby AD FS 2016 pomocí protokolu SAML a vlastní zásady"
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
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Přidáte jako poskytovatele identity SAML, používat vlastní zásady služby AD FS

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Tento článek ukazuje, jak tooenable přihlášení pro uživatele z účtu služby AD FS prostřednictvím použití hello [vlastní zásady](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Požadavky

Kroky dokončení hello hello [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md) článku.

K těmto krokům patří:

1.  Vytvoření AD FS vztah důvěryhodnosti předávající strany.
2.  Přidání hello služby AD FS vztah důvěryhodnosti předávající strany certifikát tooAzure AD B2C.
3.  Přidání zásad tooa poskytovatele deklarací identity.
4.  Registrace účtu služby AD FS hello deklarací zprostředkovatele tooa uživatele cesty.
5.  Odesílání hello zásad tooan Azure AD B2C klienta a otestovat.

## <a name="toocreate-a-claims-aware-relying-party-trust"></a>toocreate deklaracemi vztahu důvěryhodnosti předávající strany

toouse služby AD FS jako poskytovatel identit v Azure Active Directory (Azure AD) B2C, třeba služby ADFS vztah důvěryhodnosti předávající strany toocreate a přiřaďte mu hello správné parametry.

tooadd nové předávající straně důvěryhodnosti pomocí modulu snap-in Správa služby FS hello AD a ručně nakonfigurujte nastavení hello, proveďte následující postup na federační server hello.

Členství ve skupině **správci**, nebo ekvivalentní na místním počítači hello je minimální požadovaná toocomplete hello tento postup. Podrobnosti o používání hello příslušných účtů a členství ve skupinách v [místní a doménové výchozí skupiny](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  Ve Správci serveru klikněte na tlačítko **nástroje**a potom vyberte **správu služby AD FS**.

2.  Klikněte na **přidat vztah důvěryhodnosti předávající strany**.
    ![Přidat vztah důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Na hello **úvodní** vyberte **deklarace identity vědět** a klikněte na tlačítko **spustit**.
    ![Na úvodní stránku hello zvolte vědět deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Na hello **vybrat zdroj dat** klikněte na tlačítko **ručně zadat data o předávající straně hello**a potom klikněte na **Další**.
    ![Zadat data o předávající straně hello](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Na hello **zadat zobrazovaný název** stránky, zadejte název do **zobrazovaný název**v části **poznámky** zadejte popis tohoto vztahu důvěryhodnosti předávající strany a pak klikněte na tlačítko **další **.
    ![Zadejte zobrazovaný název a poznámky](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Volitelné. Pokud máte volitelné tokenu šifrovací certifikát, pak na hello **konfigurace certifikátu** klikněte na tlačítko **Procházet** toolocate soubor certifikátu a pak klikněte na tlačítko **Další** .
    ![Konfigurace certifikátu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Na hello **konfigurace adresy URL** stránky, vyberte hello **povolit podporu protokolu SAML 2.0 WebSSO hello** zaškrtávací políčko. V části **URL služby SAML 2.0 SSO předávající strany**hello adresu URL koncového bodu služby Security (Assertion Markup Language SAML) pro tento vztah důvěryhodnosti předávající strany a pak klikněte na **Další**.  Pro hello **URL služby SAML 2.0 SSO předávající strany**, vložte hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. {Klient} nahraďte názvem vašeho klienta (například contosob2c.onmicrosoft.com) a nahraďte název zásady přípony (například B2C_1A_TrustFrameworkExtensions) hello {zásad}.
    > [!IMPORTANT]
    >Název zásady Hello je hello jeden, který dědí zásady signup_or_signin, v tomto případě je: `B2C_1A_TrustFrameworkExtensions`.
    >Například může být adresa URL hello: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Předávající strany adresa URL služby SAML 2.0 SSO](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Na hello **konfigurovat identifikátory** hello zadejte stejnou adresu URL jako předchozí krok text hello, klikněte na tlačítko **přidat** tooadd je toohello seznamu a pak klikněte na tlačítko **Další**.
    ![Předávající strany důvěryhodnosti identifikátory](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Na hello **zvolte zásad řízení přístupu** vyberte zásadu a klikněte na tlačítko **Další**.
    ![Zvolte zásad řízení přístupu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Na hello **připraveni tooAdd důvěryhodnosti** zkontrolujte hello nastavení a pak klikněte na tlačítko **Další** toosave informace o vztahu důvěryhodnosti předávající strany.
    ![Uložit informace o vztazích důvěryhodnosti předávající strany](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Na hello **Dokončit** klikněte na tlačítko **Zavřít**, tato akce se automaticky zobrazí hello **upravit pravidla deklarací identity** dialogové okno.
    ![Upravit pravidla deklarace identity](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Klikněte na tlačítko **přidat pravidlo**.  
      ![Přidat nové pravidlo](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  V **šablony pravidla deklarace identity**, vyberte **odesílat atributy LDAP jako deklarace identity**.
    ![Vyberte možnost odesílat atributy LDAP jako deklarace šablony](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Zadejte **název pravidla deklarací**. Pro hello **úložiště atributů** vyberte **služby Active Directory vyberte** přidejte následující deklarace identity hello a pak klikněte na tlačítko **Dokončit** a **OK**.
    ![Umožňuje nastavit vlastnosti pravidla](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Ve Správci serveru vyberte **vztahy důvěryhodnosti předávající strany** poté vyberte hello vztahu důvěryhodnosti předávající strany jste vytvořili a klikněte na tlačítko **vlastnosti**.
    ![Předávající strany upravit vlastnosti](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Jeden hello předávající strany vztahu důvěryhodnosti (B2C ukázku) vlastnosti klikněte na **podpis** a klikněte na **přidat**.  
    ![Sada podpis](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Přidání vaší podpis certifikátu (bez soukromého klíče soubor .cert).  
    ![Přidejte svůj certifikát podpisu](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  V okně Vlastnosti vztahu důvěryhodnosti (B2C ukázku) předávající strany hello klikněte na tlačítko **Upřesnit** kartě a změňte hello **zabezpečený algoritmus hash** příliš**SHA-1**, klikněte na tlačítko **Ok**.  
    ![Algoritmus hash zabezpečené sady tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a>Přidat hello služby AD FS účet aplikace klíče tooAzure AD B2C
Federace s účty služby AD FS vyžaduje tajný klíč klienta služby AD FS tootrust účet Azure AD B2C jménem aplikace hello. Budete potřebovat toostore vaší služby AD FS certifikát v klientovi služby Azure AD B2C. 

1.  Přejděte tooyour Azure AD B2C klienta a vyberte možnost **nastavení B2C** > **Identity rozhraní Framework**
2.  Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.
3.  Klikněte na tlačítko **+ přidat**.
4.  Pro **možnosti**, použijte **nahrát**.
5.  Pro **název**, použijte `ADFSSamlCert`.  
    Předpona Hello `B2C_1A_` může automaticky přidat.
6.  V hello nahrávání souborů ** vyberte soubor .pfx certifikátu s privátním klíčem. Poznámka: Tento certifikát (s privátním klíčem hello) by měl být hello stejné, který vydala a použít hello služby AD FS, předávající strany.
![Odešlete klíč zásad](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Klikněte na **Vytvořit**
8.  Potvrďte, že jste vytvořili klíč hello `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Přidání poskytovatele deklarací identity v rozšíření zásady
Pokud chcete uživatelům toosign pomocí účtu služby AD FS, potřebujete účet služby AD FS toodefine jako poskytovatele deklarací identity. Jinými slovy budete potřebovat toospecify koncový bod, který komunikuje se službou Azure AD B2C. koncový bod Hello obsahuje sadu deklarací identity, které jsou používány tooverify Azure AD B2C, který byl ověřen konkrétního uživatele.

Definování služby AD FS jako poskytovatele deklarací identity, přidáním `<ClaimsProvider>` uzlu v souboru rozšíření zásad:

1. Otevřete soubor hello rozšíření zásad (TrustFrameworkExtensions.xml) z pracovního adresáře. Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.
2. Najde hello `<ClaimsProviders>` části
3. Přidejte následující fragment kódu XML v části hello hello `ClaimsProviders` elementu a nahraďte `identityProvider` s DNS (libovolná hodnota, kterou určuje doménu) a uložte soubor hello. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Zaregistrovat hello služby AD FS účet deklarace identity zprostředkovatele tooSign nahoru nebo přihlášení uživatele cesty
V tomto okamžiku zprostředkovatele identity hello má nastaven.  Však není k dispozici v žádném z hello registrace-množství/přihlášení obrazovky. Nyní je třeba tooyour uživatele zprostředkovatele účtu identity tooadd hello služby AD FS `SignUpOrSignIn` cesty uživatele. toomake je k dispozici, vytvoříme duplicitní existující uživatele cesty šablony.  Potom jsme upravit ji tak, aby zahrnovala hello zprostředkovatele identity služby AD FS:
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).
2.  Najde hello `<UserJourneys>` elementu a zkopírujte hello celý obsah `<UserJourneys>` uzlu.
3.  Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml) a najděte hello `<UserJourneys>` elementu. Pokud hello element neexistuje, přidejte jedno.
4.  Vložte hello celý obsah `<UserJournesy>` uzlu, který jste zkopírovali jako podřízenou hello `<UserJourneys>` elementu.

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
Hello `<ClaimsProviderSelections>` element definuje hello seznam možností výběru poskytovatele deklarací identity a jejich pořadí.  `<ClaimsProviderSelection>`Element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace-množství nebo přihlášení. Pokud přidáte `<ClaimsProviderSelection>` element pro účet služby AD FS, nové tlačítko se zobrazí při pojmenováváme uživatele na stránku hello. tooadd tento element:

1.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="SignUpOrSignIn"` v cesty hello uživatele, který jste zkopírovali.
2.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
3.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz

Nyní když máte tlačítka na místě, je nutné toolink ho tooan akce. pro Azure AD B2C toocommunicate s tooreceive účet služby AD FS Hello akce v tomto případě je token. Odkaz hello tlačítko tooan akce pomocí propojení hello technické profil pro poskytovatele deklarací identity účtu služby AD FS:

1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Ujistěte se, hello `Id` má stejnou hodnotu jako hello `TargetClaimsExchangeId` v předcházející části hello.
> * Ujistěte se, `TechnicalProfileReferenceId` nastavena toohello technické profil vytvoříte starší (Contoso-typu SAML2).

## <a name="upload-hello-policy-tooyour-tenant"></a>Nahrát hello zásad tooyour klienta
1.  V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.
2.  Vyberte **Identity rozhraní Framework**.
3.  Otevřete hello **všechny zásady** okno.
4.  Vyberte **nahrát zásady**.
5.  Zkontrolujte **přepsat zásady hello, pokud existuje** pole.
6.  **Nahrát** TrustFrameworkExtensions.xml a ujistěte se, zda text hello ověření neselže

## <a name="test-hello-custom-policy-by-using-run-now"></a>Otestovat vlastní zásady hello pomocí spustit nyní
1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.
2.  Otevřete **B2C_1A_signup_signin**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí účtu služby AD FS.

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a>[Nepovinné] Zaregistrovat hello služby AD FS účet deklarace identity zprostředkovatele tooProfile upravit uživatele cesty
Může být vhodné zprostředkovatele identity účtu služby AD FS hello tooadd také tooyour uživatele `ProfileEdit` cesty uživatele. toomake je k dispozici, jsme opakujte hello poslední dva kroky:

### <a name="display-hello-button"></a>Tlačítko hello zobrazení
1.  Otevřete soubor hello rozšíření zásad (například TrustFrameworkExtensions.xml).
2.  Najde hello `<UserJourney>` uzlu, který zahrnuje `Id="ProfileEdit"` v cesty hello uživatele, který jste zkopírovali.
3.  Vyhledejte hello `<OrchestrationStep>` uzlu, který obsahuje`Order="1"`
4.  Přidejte následující fragment kódu XML v části `<ClaimsProviderSelections>` uzlu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Akce tooan tlačítka hello odkaz
1.  Najde hello `<OrchestrationStep>` , který obsahuje `Order="2"` v hello `<UserJourney>` uzlu.
2.  Přidejte následující fragment kódu XML v části `<ClaimsExchanges>` uzlu:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Otestovat vlastní zásady pro úpravy profilu hello pomocí spustit nyní
1.  Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.
2.  Otevřete **B2C_1A_ProfileEdit**, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.
3.  Musí být schopný toosign pomocí účtu služby AD FS.

## <a name="download-hello-complete-policy-files"></a>Stáhnout soubory dokončení zásad hello
Volitelné: Doporučujeme že vytvořit váš scénář pomocí vlastní soubory zásad vlastní po dokončení hello Začínáme se zásadami vlastní provede. [Ukázkové soubory zásad jenom jako reference](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
