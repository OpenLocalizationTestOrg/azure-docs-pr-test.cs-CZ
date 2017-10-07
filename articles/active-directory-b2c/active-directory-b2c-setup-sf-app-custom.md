---
title: "Azure Active Directory B2C: Přidání poskytovatele služby Salesforce SAML pomocí vlastních zásad | Microsoft Docs"
description: "Další informace o tom toocreate a spravovat vlastní zásady Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: f14c9d96980ff124110db7cfb58bf7cd81750b7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: Přihlaste se pomocí účtů služby Salesforce pomocí SAML

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Tento článek ukazuje, jak toouse [vlastní zásady](active-directory-b2c-overview-custom.md) tooset až přihlášení pro uživatele z konkrétní organizace služby Salesforce.

## <a name="prerequisites"></a>Požadavky

### <a name="azure-ad-b2c-setup"></a>Instalace nástroje Azure AD B2C

Ujistěte se, že jste dokončili všechny kroky hello, které ukazují, jak příliš[začít pracovat s vlastními zásadami](active-directory-b2c-get-started-custom.md) v Azure Active Directory B2C (Azure AD B2C).

Mezi ně patří:

* Vytvoření klienta Azure AD B2C.
* Vytvoření aplikace Azure AD B2C.
* Zaregistrujte dvě aplikace modul zásad.
* Nastavení klíčů.
* Nastavte hello starter pack.

### <a name="salesforce-setup"></a>Instalační program služby Salesforce

V tomto článku předpokládáme, že jste již dokončili hello následující:

* Registraci účtu Salesforce. Můžete si zaregistrovat [bezplatný účet Developer Edition](https://developer.salesforce.com/signup).
* [Nastavit Moje domény](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) pro vaši organizaci Salesforce.

## <a name="set-up-salesforce-so-users-can-federate"></a>Nastavení Salesforce, aby uživatelé mohli Federovat

toohelp Azure AD B2C komunikovat s Salesforce, potřebujete adresu URL metadat tooget hello Salesforce.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Nastavení Salesforce jako zprostředkovatel identity

> [!NOTE]
> V tomto článku předpokládáme, že používáte [Salesforce Lightning prostředí](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Přihlaste se tooSalesforce](https://login.salesforce.com/). 
2. Na hello levé nabídce v části **nastavení**, rozbalte položku **Identity**a potom klikněte na **zprostředkovatele Identity**.
3. Klikněte na tlačítko **povolit zprostředkovatele Identity**.
4. V části **certifikátu vyberte hello**, vyberte certifikát hello chcete Salesforce toouse toocommunicate s Azure AD B2C. (Můžete hello výchozí certifikát.) Klikněte na **Uložit**. 

### <a name="create-a-connected-app-in-salesforce"></a>Vytvoření připojené aplikaci v Salesforce

1. Na hello **zprostředkovatele Identity** stránky, přejděte příliš**poskytovatelé služeb**.
2. Klikněte na tlačítko **poskytovatelé služeb jsou teď vytvořené prostřednictvím připojené aplikace. Kliknutím sem.**
3. V části **základní informace**, zadejte hello požadované hodnoty pro připojené aplikaci.
4. V části **nastavení webové aplikace**, vyberte hello **povolit SAML** zaškrtávací políčko.
5. V hello **Entity ID** pole, zadejte následující adresu URL hello. Ujistěte se, že nahradíte hodnotu hello `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. V hello **adresa URL služby ACS** pole, zadejte následující adresu URL hello. Ujistěte se, že nahradíte hodnotu hello `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Ponechte hello výchozí hodnoty pro všechna ostatní nastavení.
8. Posuňte se toohello dolní části seznamu hello a pak klikněte na **Uložit**.

### <a name="get-hello-metadata-url"></a>Získat adresu URL metadat hello

1. Na stránce Přehled hello připojené aplikaci, klikněte na tlačítko **spravovat**.
2. Zkopírujte hodnotu hello **koncový bod zjišťování metadat**a pak ho uložte. Použijete ho později v tomto článku.

### <a name="set-up-salesforce-users-toofederate"></a>Nastavení Salesforce uživatelé toofederate

1. Na hello **spravovat** stránky připojené aplikace, přejděte příliš**profily**.
2. Klikněte na tlačítko **spravovat profily**.
3. Vyberte profily hello (nebo skupiny uživatelů), které chcete toofederate s Azure AD B2C. Jako správce systému, vyberte hello **správce systému** zaškrtnutí políčka, takže můžete vytvořit federaci s použitím účtu Salesforce.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Generovat podpisový certifikát pro Azure AD B2C

Odeslání požadavků toobe nutné tooSalesforce podepsané pomocí Azure AD B2C. toogenerate podpisový certifikát, otevřete prostředí Azure PowerShell a spusťte následující příkazy hello.

> [!NOTE]
> Zkontrolujte, zda je aktualizovat hello klienta jméno a heslo v horní dva řádky hello.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a>Přidat hello SAML podpisový certifikát tooAzure AD B2C

Odešlete hello podpisový certifikát tooyour Azure AD B2C klienta: 

1. Přejděte klienta tooyour Azure AD B2C. Klikněte na tlačítko **nastavení** > **Identity rozhraní Framework** > **zásad klíče**.
2. Klikněte na tlačítko **+ přidat**a pak:
    1. Klikněte na tlačítko **možnosti** > **nahrát**.
    2. Zadejte **název** (například SAMLSigningCert). Předpona Hello *B2C_1A_* se automaticky přidá toohello název vašeho klíče.
    3. tooselect certifikát, vyberte **nahrát ovládacího prvku se souborovým**. 
    4. Zadejte heslo hello certifikátu, který nastavíte ve skriptu PowerShell hello.
3. Klikněte na možnost **Vytvořit**.
4. Ověřte, že jste vytvořili klíč (například B2C_1A_SAMLSigningCert). Poznamenejte si hello celý název (včetně *B2C_1A_*). Klíč toothis později v hello zásady budou vztahovat.

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a>Vytvoření hello poskytovatele deklarací identity Salesforce SAML v základní zásady

Je nutné toodefine Salesforce jako poskytovatele deklarací identity tak moct uživatel přihlásit pomocí služby Salesforce. Jinými slovy budete potřebovat hello koncový bod toospecify, který Azure AD B2C bude komunikovat se službou. koncový bod Hello bude *poskytují* sadu *deklarace identity* , Azure AD B2C používá tooverify, který byl ověřen konkrétního uživatele. toodo, přidejte `<ClaimsProvider>` pro služby Salesforce v souboru rozšíření hello zásad:

1. V pracovním adresáři otevřete soubor rozšíření hello (TrustFrameworkExtensions.xml).
2. Najde hello `<ClaimsProviders>` části. Pokud neexistuje, vytvořte ho v části hello kořenový uzel.
3. Přidejte nový `<ClaimsProvider>`:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

V části hello `<ClaimsProvider>` uzlu:

1. Změnit hodnotu hello pro `<Domain>` tooa jedinečnou hodnotu, která rozlišuje `<ClaimsProvider>` od jiných poskytovatelů identit.
2. Aktualizujte hello hodnotu pro `<DisplayName>` tooa zobrazovaný název pro hello deklarací zprostředkovatele. Tato hodnota není v současné době používá.

### <a name="update-hello-technical-profile"></a>Aktualizovat profil technické hello

tooget tokenu SAML ze služby Salesforce, definujte hello protokoly, Azure AD B2C použijete toocommunicate službou Azure Active Directory (Azure AD). K tomu hello `<TechnicalProfile>` element `<ClaimsProvider>`:

1. Aktualizovat hello ID hello `<TechnicalProfile>` uzlu. Toto ID je použité toorefer toothis technické profil z různých částí hello zásad.
2. Aktualizujte hello hodnotu pro `<DisplayName>`. Tato hodnota se zobrazí na hello přihlašovací tlačítko na přihlašovací stránku.
3. Aktualizujte hello hodnotu pro `<Description>`.
4. Salesforce používá protokol hello SAML 2.0. Ujistěte se, že hello hodnotu pro `<Protocol>` je **typu SAML2**.

Aktualizace hello `<Metadata>` část v hello předcházející XML tooreflect hello nastavení pro konkrétní účtu Salesforce. V hello XML aktualizujte hodnoty metadata hello:

1. Aktualizujte hodnotu hello `<Item Key="PartnerEntity">` s Salesforce adresu URL metadat hello jste zkopírovali dříve. Má hello následující formát: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. V hello `<CryptographicKeys>` část aktualizace hello hodnota pro obě instance `StorageReferenceId` toohello název klíče hello podpisového certifikátu (například B2C_1A_SAMLSigningCert).

### <a name="upload-hello-extension-file-for-verification"></a>Nahrát soubor hello rozšíření pro ověření

Zásady je nyní nakonfigurována tak, aby Azure AD B2C zná jak toocommunicate s Salesforce. Zkuste odeslat soubor hello rozšíření zásad, že nejsou k dispozici všechny problémy, pokud tooverify. tooupload hello rozšíření soubor zásad:

1. Ve vašem klientu Azure AD B2C, přejděte toohello **všechny zásady** okno.
2. Vyberte hello **přepsat zásady hello, pokud existuje** zaškrtávací políčko.
3. Nahrajte soubor rozšíření hello (TrustFrameworkExtensions.xml). Ujistěte se, že neselže ověření.

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a>Zaregistrovat hello Salesforce SAML deklarace identity zprostředkovatele tooa uživatele cesty

Dál přidejte hello tooone poskytovatele identity Salesforce SAML z vaší uživatelské cesty. V tomto okamžiku zprostředkovatele identity hello byl nastaven, ale není k dispozici na všech hello stránek registrace nebo přihlášení uživatele. tooadd hello identity tooa přihlašovací stránka zprostředkovatele, nejprve vytvořte duplicitní existující uživatele cesty šablony. Potom upravte hello šablony tak, aby je také hello poskytovatele identit Azure AD.

1. Otevřete soubor základní hello zásad (například TrustFrameworkBase.xml).
2. Najde hello `<UserJourneys>` elementu a kopírování hello celý `<UserJourney>` hodnoty, včetně Id = "SignUpOrSignIn".
3. Otevřete soubor rozšíření hello (například TrustFrameworkExtensions.xml). Najde hello `<UserJourneys>` elementu. Pokud hello element neexistuje, vytvořte ji.
4. Vložení hello celý zkopírovat `<UserJourney>` jako podřízenou hello `<UserJourneys>` elementu.
5. Přejmenujte hello ID hello nové `<UserJourney>` (například SignUpOrSignUsingContoso).

### <a name="display-hello-identity-provider-button"></a>Tlačítko zprostředkovatele identity hello zobrazení

Hello `<ClaimsProviderSelection>` element je obdobou tooan tlačítko zprostředkovatele identity na stránce registrace nebo přihlášení. Přidáním `<ClaimsProviderSelection>` element pro Salesforce nové tlačítko se zobrazí, když uživatel přejde toothis stránky. tlačítko zprostředkovatele identity toodisplay hello:

1. V hello `<UserJourney>` , kterou jste vytvořili, najde hello `<OrchestrationStep>` s `Order="1"`.
2. Přidejte následující XML hello:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Nastavit `TargetClaimsExchangeId` tooa logická hodnota. Doporučujeme následující hello stejné konvence jako jiné (například  *\[ClaimProviderName\]Exchange*).

### <a name="link-hello-identity-provider-button-tooan-action"></a>Odkaz akce tooan tlačítka zprostředkovatele identity hello

Nyní když máte tlačítko zprostředkovatele identity na místě, propojte tooan akce. V takovém případě není hello akce pro Azure AD B2C toocommunicate s Salesforce tooreceive tokenu SAML. To provedete pomocí propojení hello technické profil pro vaše Salesforce SAML poskytovatele deklarací identity:

1. V hello `<UserJourney>` uzlu, najít hello `<OrchestrationStep>` s `Order="2"`.
2. Přidejte následující XML hello:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Aktualizace `Id` toohello, stejnou který jste dříve použít pro hodnotu `TargetClaimsExchangeId`.
4. Aktualizace `TechnicalProfileReferenceId` toohello `Id` z hello technické profilu, kterou jste vytvořili dříve (například ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Nahrát soubor aktualizované rozšíření hello

Dokončení změny souboru rozšíření bylo hello. Uložte a nahrajte tento soubor. Ujistěte se, že všechny ověření úspěšné.

### <a name="update-hello-relying-party-file"></a>Aktualizovat hello předávající strany soubor

Potom aktualizujte hello předávající stranu soubor, který iniciuje cesty hello uživatele, který jste vytvořili:

1. Vytvořte kopii SignUpOrSignIn.xml v pracovní adresář. Potom přejmenujte ji (například SignUpOrSignInWithAAD.xml).
2. Otevřete hello nový soubor a aktualizace hello `PolicyId` atribut pro `<TrustFrameworkPolicy>` s jedinečnou hodnotu. Toto je název hello zásad (například SignUpOrSignInWithAAD).
3. Upravit hello `ReferenceId` atribut `<DefaultUserJourney>` toomatch hello `Id` hello nové uživatele cesty vytvořený (například SignUpOrSignUsingContoso).
4. Uložte změny a pak nahrajte soubor hello.

## <a name="test-and-troubleshoot"></a>Testování a řešení potíží

hello tootest vlastní se zásady, které jste právě nahráli, zvolte v hello portál Azure, přejděte toohello okna zásady a pak klikněte na tlačítko **spustit nyní**. Pokud se nezdaří, najdete v části [řešení potíží se zásadami vlastní](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Další kroky

Poskytnutí zpětné vazby příliš[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).
