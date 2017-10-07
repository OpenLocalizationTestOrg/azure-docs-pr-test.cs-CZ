---
title: "Azure Active Directory B2C: Začínáme s vlastními zásadami | Microsoft Docs"
description: "Jak tooget pracovat s Azure Active Directory B2C vlastní zásady"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja;parahk;gsacavdm
ms.openlocfilehash: 5ee133806395cddf18682769a6cad149889d82d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: Začínáme s vlastní zásady

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Po dokončení kroků hello v tomto článku, bude vaše vlastní zásada podporovat "místní účet" registrace nebo přihlášení pomocí služby e-mailovou adresu a heslo. Bude také Příprava prostředí pro přidání zprostředkovatelů identity (jako je Facebook nebo Azure Active Directory). Doporučujeme vám toocomplete tyto kroky před čtení o dalších používání hello Azure Active Directory (Azure AD) B2C Identity rozhraní Framework.

## <a name="prerequisites"></a>Požadavky

Než budete pokračovat, ujistěte se, že máte klienta Azure AD B2C, což je kontejner pro všechny uživatele, aplikace, zásady a další. Pokud nemáte již, je třeba příliš[vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md). Jsme důrazně podporují všechny vývojáře toocomplete hello návody předdefinovaných zásad Azure AD B2C a konfigurace aplikací pomocí předdefinovaných zásad než budete pokračovat. Vaše aplikace bude fungovat s oběma typy zásad po provedení méně zásadní změna toohello zásady název tooinvoke hello vlastní zásadu.

>[!NOTE]
>tooaccess vlastní zásady pro úpravy, je nutné klienta propojené tooyour platné předplatné Azure. Pokud jste to ještě [propojené vaší tooan klienta Azure AD B2C předplatného Azure](active-directory-b2c-how-to-enable-billing.md) nebo předplatné Azure je zakázáno, nebude tlačítko Identity rozhraní Framework hello k dispozici.

## <a name="add-signing-and-encryption-keys-tooyour-b2c-tenant-for-use-by-custom-policies"></a>Přidat vlastní zásady podepisování a šifrování klienta tooyour B2C klíče pro použití

1. Otevřete hello **Identity rozhraní Framework** okno v nastaveních klienta Azure AD B2C.
2. Vyberte **zásad klíče** tooview hello klíče k dispozici ve vašem klientovi.
3. Pokud neexistuje, vytvořte B2C_1A_TokenSigningKeyContainer:<br>
    a. Vyberte **Přidat**. <br>
    b. Vyberte **generovat**.<br>
    c. Pro **název**, použijte `TokenSigningKeyContainer`. <br> 
    Předpona Hello `B2C_1A_` může automaticky přidat.<br>
    d. Pro **typ klíče**, použijte **RSA**.<br>
    e. Pro **data**, použít výchozí hello. <br>
    f. Pro **použití klíče**, použijte **podpis**.<br>
    g. Vyberte **Vytvořit**.<br>
4. Pokud neexistuje, vytvořte B2C_1A_TokenEncryptionKeyContainer:<br>
 a. Vyberte **Přidat**.<br>
 b. Vyberte **generovat**.<br>
 c. Pro **název**, použijte `TokenEncryptionKeyContainer`. <br>
   Předpona Hello `B2C_1A`_ může automaticky přidat.<br>
 d. Pro **typ klíče**, použijte **RSA**.<br>
 e. Pro **data**, použít výchozí hello.<br>
 f. Pro **použití klíče**, použijte **šifrování**.<br>
 g. Vyberte **Vytvořit**.<br>
5. Vytvořte B2C_1A_FacebookSecret. <br>
Pokud již máte tajný klíč aplikace Facebook, přidejte ji jako klient klíče tooyour zásad. Hello klíč, jinak hodnota musíte vytvořit s hodnotu zástupného symbolu, tak, aby vaše zásady projít ověřením.<br>
 a. Vyberte **Přidat**.<br>
 b. Pro **možnosti**, použijte **ruční**.<br>
 c. Pro **název**, použijte `FacebookSecret`. <br>
 Předpona Hello `B2C_1A_` může automaticky přidat.<br>
 d. V hello **tajný klíč** zadejte vaše FacebookSecret z developers.facebook.com nebo `0` jako zástupný znak. *Toto není vaše ID aplikace sítě Facebook* <br>
 e. Pro **použití klíče**, použijte **podpis**. <br>
 f. Vyberte **vytvořit** a potvrďte vytvoření.

## <a name="register-identity-experience-framework-applications"></a>Registrace aplikací Identity rozhraní Framework

Azure AD B2C vyžaduje tooregister dva další aplikace, které jsou používány hello modul toosign nahoru a přihlášení uživatelů.

>[!NOTE]
>Je nutné vytvořit dvě aplikace této povolit přihlášení pomocí místních účtů: IdentityExperienceFramework (webové aplikace) a ProxyIdentityExperienceFramework (nativní aplikaci) s delegovaná oprávnění z aplikace IdentityExperienceFramework hello. Místní účty existovat pouze ve vašem klientovi. Vaši uživatelé přihlásit tooaccess kombinace jedinečnou e-mailovou adresu a heslo aplikace klient zaregistrovaný.

### <a name="create-hello-identityexperienceframework-application"></a>Vytvoření aplikace IdentityExperienceFramework hello

1. V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md).
2. Otevřete hello **Azure Active Directory** okno (není hello **Azure AD B2C** okno). Může být nutné tooselect **více služeb** toofind ho.
3. Vyberte **registrace aplikace**.
4. Vyberte **nové registrace aplikace**.
   * Pro **název**, použijte `IdentityExperienceFramework`.
   * Pro **typ aplikace**, použijte **webové aplikace nebo rozhraní API**.
   * Pro **přihlašovací adresa URL**, použijte `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, kde `yourtenant` je název domény klienta Azure AD B2C.
5. Vyberte **Vytvořit**.
6. Jakmile je vytvořen, vyberte aplikaci hello nově vytvořený **IdentityExperienceFramework**.<br>
   * Vyberte **vlastnosti**.<br>
   * ID aplikace hello zkopírovat a uložit pro pozdější použití.

### <a name="create-hello-proxyidentityexperienceframework-application"></a>Vytvoření aplikace ProxyIdentityExperienceFramework hello

1. Vyberte **registrace aplikace**.
1. Vyberte **nové registrace aplikace**.
   * Pro **název**, použijte `ProxyIdentityExperienceFramework`.
   * Pro **typ aplikace**, použijte **nativní**.
   * Pro **identifikátor URI pro přesměrování**, použijte `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, kde `yourtenant` je váš klient Azure AD B2C.
1. Vyberte **Vytvořit**.
1. Po vytvoření, vyberte aplikaci hello **ProxyIdentityExperienceFramework**.<br>
   * Vyberte **vlastnosti**. <br>
   * ID aplikace hello zkopírovat a uložit pro pozdější použití.
1. Vyberte **požadovaná oprávnění**.
1. Vyberte **Přidat**.
1. Vyberte **vybrat rozhraní API**.
1. Vyhledávání pro název hello IdentityExperienceFramework. Vyberte **IdentityExperienceFramework** v hello výsledky a potom klikněte na **vyberte**.
1. Hello zaškrtněte políčko vedle příliš**přístup IdentityExperienceFramework**a potom klikněte na **vyberte**.
1. Vyberte **provádí**.
1. Vyberte **udělit oprávnění**a pak potvrďte výběrem **Ano**.

## <a name="download-starter-pack-and-modify-policies"></a>Stáhnout starter pack a úpravám zásad

Vlastní zásady jsou sada souborů XML, které je třeba klienta toobe nahrán tooyour Azure AD B2C. Poskytujeme tooget starter balíčky můžete rychle přejdete. Jednotlivé sady starter v hello následující seznam obsahuje hello nejmenší počet technické profily a cesty uživatele potřeby tooachieve hello scénářů:
 * LocalAccounts. Umožňuje použití hello pouze místní účty.
 * SocialAccounts. Umožňuje použití hello sociálních (nebo federované) účtů.
 * **SocialAndLocalAccounts**. Tento soubor použít pro návod hello.
 * SocialAndLocalAccountsWithMFA. Možnosti sociálního, místní a multi-Factor Authentication jsou zde zahrnuty.

Jednotlivé sady starter obsahuje:

* Hello [základního souboru](active-directory-b2c-overview-custom.md#policy-files) hello zásad. Několik úpravy jsou požadované toohello základní.
* Hello [souboru rozšíření bylo](active-directory-b2c-overview-custom.md#policy-files) hello zásad.  Tento soubor je, kde jsou vytvářeny většinu změn konfigurace.
* [Předávající strany soubory](active-directory-b2c-overview-custom.md#policy-files) jsou specifické pro úlohy soubory aplikací.

>[!NOTE]
>Pokud XML editor podporuje ověřování, ověřte hello soubory pro schéma hello TrustFrameworkPolicy_0.3.0.0.xsd XML, který je umístěný v kořenovém adresáři hello sady starter hello. Ověření schématu XML označuje chyby před nahráním.

 Můžeme začít:

1. Stáhněte si active-directory-b2c-custom-policy-starterpack z Githubu. [Stáhněte si soubor .zip hello](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) nebo spustit

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Otevřete složku SocialAndLocalAccounts hello.  Hello základního souboru (TrustFrameworkBase.xml) v této složce obsahuje obsah potřebné pro místní i sociálního nebo podnikové účty. Hello sociálních obsah není v konfliktu s hello kroky pro místní účty zprovoznění.
3. Otevřete TrustFrameworkBase.xml. Pokud potřebujete editoru XML [zkuste Visual Studio Code](https://code.visualstudio.com/download), lightweight editor napříč platformami.
4. V kořenovém hello `TrustFrameworkPolicy` element, aktualizace hello `TenantId` a `PublicPolicyUri` atributy, nahraďte `yourtenant.onmicrosoft.com` hello název domény klienta služby Azure AD B2C:
   ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
   >[!NOTE]
   >`PolicyId`je název zásady hello zobrazí hello portálu a hello název, kterým je tento soubor zásad odkazují jiné soubory zásad.

5. Uložte soubor hello.
6. Otevřete TrustFrameworkExtensions.xml. Změnit hello stejné dva nahrazením `yourtenant.onmicrosoft.com` s vašeho klienta Azure AD B2C. Ujistěte se, hello stejné náhrada v hello `<TenantId>` element celkem tři změny. Uložte soubor hello.
7. Otevřete SignUpOrSignIn.xml. Provést stejné změny hello nahrazením `yourtenant.onmicrosoft.com` s vašeho klienta Azure AD B2C na třech místech. Uložte soubor hello.
8. Resetování hesla otevřete hello a profil úpravy souborů. Provést stejné změny hello nahrazením `yourtenant.onmicrosoft.com` s vašeho klienta Azure AD B2C na třech místech do každého souboru. Uložte soubory hello.

### <a name="add-hello-application-ids-tooyour-custom-policy"></a>Přidat hello aplikace ID tooyour vlastní zásady
Přidání souboru rozšíření hello aplikace ID toohello (`TrustFrameworkExtensions.xml`):

1. V souboru rozšíření hello (TrustFrameworkExtensions.xml), najít hello element `<TechnicalProfile Id="login-NonInteractive">`.
2. Nahraďte obou instancí `IdentityExperienceFrameworkAppId` s ID aplikace hello hello Framework prostředí Identity aplikace, kterou jste vytvořili dříve. Zde naleznete příklad:

   ```xml
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Nahraďte obou instancí `ProxyIdentityExperienceFrameworkAppId` s ID aplikace hello hello aplikaci Proxy Identity rozhraní Framework, kterou jste vytvořili dříve.
4. Uložte soubor rozšíření.

## <a name="upload-hello-policies-tooyour-tenant"></a>Nahrát hello zásady tooyour klienta

1. V hello [portál Azure](https://portal.azure.com), přepněte do hello [kontextu klienta služby Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a otevřete hello **Azure AD B2C** okno.
2. Vyberte **Identity rozhraní Framework**.
3. Vyberte **nahrát zásady**.

    >[!WARNING]
    >soubory Hello vlastní zásady, musí se nahrát v hello následující pořadí:

1. Nahrajte TrustFrameworkBase.xml.
2. Nahrajte TrustFrameworkExtensions.xml.
3. Nahrajte SignUpOrSignin.xml.
4. Odešlete další soubory zásad.

Při odeslání souboru hello hello zásad souboru začíná název `B2C_1A_`.

## <a name="test-hello-custom-policy-by-using-run-now"></a>Otestovat vlastní zásady hello pomocí spustit nyní

1. Otevřete **nastavení Azure AD B2C** a přejděte příliš**Identity rozhraní Framework**.

   >[!NOTE]
   >**Spustit nyní** vyžaduje alespoň jednu aplikaci toobe preregistered na hello klienta. Aplikace musí být zaregistrovaný v klientovi hello B2C, buď pomocí hello **aplikace** výběr nabídky v Azure AD B2C, nebo pomocí tooinvoke Identity rozhraní Framework hello předdefinované a vlastní zásady. Je potřeba pouze jedna registrace na aplikaci.<br><br>
   jak zjistit, tooregister aplikace toolearn hello Azure AD B2C [Začínáme](active-directory-b2c-get-started.md) článku nebo hello [registrace aplikace](active-directory-b2c-app-registration.md) článku.  

2. Otevřete B2C_1A_signup_signin, hello předávající vlastní zásady stranu, který jste nahráli. Vyberte **spustit nyní**.

3. Musí být schopný toosign díky e-mailovou adresu.

4. Přihlaste se pomocí hello účet tooconfirm, zda máte hello správnou konfiguraci.

>[!NOTE]
>Obvyklou příčinou selhání přihlášení je nesprávně nakonfigurovaný IdentityExperienceFramework aplikace.


## <a name="next-steps"></a>Další kroky

### <a name="add-facebook-as-an-identity-provider"></a>Přidat Facebook jako zprostředkovatele identity
tooset až Facebook:
1. [Konfigurace aplikace Facebook v developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [Přidat klienta tajný tooyour Azure AD B2C aplikace Facebook hello](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. V modulu snap-in soubor zásad TrustFrameworkExtensions hello nahradit hodnotu hello `client_id` s ID aplikace pro Facebook hello:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace hello value of client_id in this technical profile with hello Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Nahrajte hello TrustFrameworkExtensions.xml zásad souboru tooyour klienta.
5. Test pomocí **spustit nyní** nebo vyvoláním hello zásady přímo z zaregistrovanou aplikaci.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Přidat jako zprostředkovatele identity Azure Active Directory
Hello základního souboru již používán Tato příručka Začínáme obsahuje některé hello obsahu, který pro přidání jiných poskytovatelů identit. Informace o nastavení přihlášení najdete v tématu hello [Azure Active Directory B2C: Přihlaste se pomocí účtů služby Azure AD](active-directory-b2c-setup-aad-custom.md) článku.

Přehled vlastní zásady v Azure AD B2C, které používají hello Identity Framework prostředí najdete v tématu hello [Azure Active Directory B2C: vlastní zásady](active-directory-b2c-overview-custom.md) článku. 
