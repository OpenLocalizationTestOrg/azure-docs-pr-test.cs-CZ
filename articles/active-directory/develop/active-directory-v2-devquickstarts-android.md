---
title: "aplikace pro aaaAzure služby Active Directory v2.0 Android | Microsoft Docs"
description: "Jak toobuild aplikace pro Android s přihlašováním uživatelů s osobní účet Microsoft a pracovní nebo školní účty a volání hello rozhraní Graph API pomocí knihoven jiných dodavatelů."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1dd40bd3bcea28c629abce09abaed66b38774162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Přidat aplikaci pro Android tooan přihlášení pomocí rozhraní Graph API pomocí koncového bodu v2.0 hello knihovně třetích stran
Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect. Vývojáři mohou použít žádnou knihovnu chtějí toointegrate s našich služeb. Vývojáři toohelp používat naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure třetích stran knihovny tooconnect toohello Microsoft identity platformy. Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) můžete připojení toohello Microsoft identity platformy.

Hello aplikaci, která vytváří Tento názorný postup mohou uživatelé přihlásit tootheir organizace a poté vyhledejte sami ve své organizaci pomocí hello rozhraní Graph API.

Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít smysl tooyou. Doporučujeme, abyste si přečetli [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md) pozadí.

> [!NOTE]
> Některé funkce naše platformy, které mají výraz v hello OAuth2 nebo OpenID Connect standardy, jako je například podmíněný přístup a správu zásad Intune, vyžadují jste toouse naše s otevřeným zdrojem Identity pro knihovny Microsoft Azure.
> 
> 

koncový bod v2.0 Hello nepodporuje všechny scénáře Azure Active Directory a funkce.

> [!NOTE]
> toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="download-hello-code-from-github"></a>Stáhněte si kód hello z Githubu
Hello kód v tomto kurzu se udržuje [na Githubu](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  toofollow společně, můžete [stáhnout kostru aplikace hello jako ZIP](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) nebo hello kostru klonovat:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Můžete také právě stažení ukázky hello a rovnou začít:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registrace aplikace
Vytvoření nové aplikace v hello [portálu pro registraci aplikace](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), nebo postupujte podle hello podrobné kroky v [jak tooregister aplikace s koncovým bodem v2.0 hello](active-directory-v2-app-registration.md).  Zkontrolujte, že:

* Kopírování hello **Id aplikace** aplikace přiřazené tooyour důvodem je, že ho budete potřebovat brzy k dispozici.
* Přidat hello **Mobile** platformu pro vaši aplikaci.

> Poznámka: portál pro registraci aplikace hello poskytuje **identifikátor URI pro přesměrování** hodnotu. Ale v tomto příkladu musí používat výchozí hodnotu hello `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a>Stažení hello NXOAuth2 třetích stran knihovny a vytvořit pracovní prostor
V tomto návodu budete používat hello OIDCAndroidLib z Githubu, která je OAuth2 knihovny založené na hello kód Google OpenID Connect. Profil nativní aplikace hello implementuje a podporuje koncový bod autorizace hello hello uživatele. To je vše, co hello, že potřebujete toointegrate s platformou identity Microsoft hello.

Klonování hello OIDCAndroidLib úložišti tooyour počítače.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Nastavení prostředí Android Studio
1. Vytvořte nový projekt Android Studio a přijměte výchozí hodnoty hello v Průvodci hello.
   
    ![Vytvořit nový projekt v Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Cílové zařízení se systémem Android](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Přidat toomobile aktivity](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. tooset až moduly vašeho projektu přesunout umístění projektu toohello hello klonovat úložiště. Můžete také vytvořit hello projekt a poté klonovat ji přímo toohello umístění projektu.
   
    ![Moduly projektu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. Otevřete nastavení moduly hello projektu pomocí hello kontextové nabídky, nebo pomocí zástupce Ctrl + Alt + avní + S hello.
   
    ![Nastavení projektu moduly](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. Modul aplikace hello výchozí odeberte, protože chcete nastavení kontejneru projektu hello.
   
    ![modul aplikace výchozí Hello](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. Naimportovat moduly z hello klonovaný úložišti toohello aktuálního projektu.
   
    ![Import projektu gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![vytvořit novou stránku modulu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)
6. Opakujte tyto kroky u hello `oidlib-sample` modulu.
7. Zkontrolujte hello oidclib závislostí na hello `oidlib-sample` modulu.
   
    ![oidclib závislosti na modulu hello oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. Klikněte na tlačítko **OK** a počkejte, než pro synchronizaci gradle.
   
    Vaše settings.gradle by měl vypadat podobně jako:
   
    ![Snímek obrazovky settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. Sestavení hello ukázkové aplikace toomake se, že hello ukázka funguje správně.
   
    Můžete nebudou moct toouse to s Azure Active Directory ještě. Budeme potřebovat tooconfigure některé koncové body nejdřív. Toto je tooensure nemáte oprávnění ke Android Studio než začneme přizpůsobení hello ukázkovou aplikaci.
10. Sestavení a spuštění `oidlib-sample` jako cíl hello v Android Studio.
    
    ![Průběh sestavení oidlib – ukázka](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. Odstranit hello `app ` adresáře, které bylo při odebrání hello modul z projektu hello protože Android Studio neodstraní, je pro zabezpečení.
    
    ![Struktura souborů, která obsahuje adresář aplikace hello](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. Otevřete hello **upravit konfigurace** nabídky tooremove hello spustit konfigurace, které bylo také při odebrání hello modul z projektu hello.
    
    ![Upravit nabídku konfigurace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![spustit konfigurace aplikace](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-hello-endpoints-of-hello-sample"></a>Nakonfigurovat koncové body hello hello vzorku
Teď, když máte hello `oidlib-sample` spuštěna úspěšně, tady upravit některé koncové body tooget tato práce s Azure Active Directory.

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a>Nakonfigurujte klienta tak, že upravíte soubor oidc_clientconf.xml hello
1. Vzhledem k tomu, že používáte OAuth2 toky pouze tooget token a volání hello rozhraní Graph API, nastavte hello toodo klienta OAuth2 pouze. OIDC vrátí se v pozdější příkladu.
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. Nakonfigurujte vaše ID klienta, který jste obdrželi z portálu pro registraci hello.
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. Nakonfigurujte váš identifikátor URI přesměrování s hello jeden níže.
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. Konfigurace vaší oborů, že budete potřebovat v pořadí tooaccess hello rozhraní Graph API.
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Hello `User.Read` hodnotu `oidc_scopes` umožňuje vám tooread hello základní profil hello přihlášení uživatele.
Další informace o všech dostupných oborů hello v [obory oprávnění Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

Pokud chcete o vysvětlení `openid` nebo `offline_access` jako obory v OpenID Connect, najdete v části [2.0 protokoly - toku OAuth 2.0 autorizační kód](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a>Nakonfigurovat koncové body klienta úpravou souboru oidc_endpoints.xml hello
* Otevřete hello `oidc_endpoints.xml` souboru a nastavit hello následující změny:
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Tyto koncové body nikdy změnit v případě, že používáte OAuth2 jako protokol.

> [!NOTE]
> Hello koncové body pro `userInfoEndpoint` a `revocationEndpoint` nejsou aktuálně podporovány službou Azure Active Directory. Pokud tyto výchozí example.com hodnotou hello, budete upozorněni, že nejsou k dispozici v ukázce hello :-)
> 
> 

## <a name="configure-a-graph-api-call"></a>Konfigurace volání rozhraní Graph API
* Otevřete hello `HomeActivity.java` souboru a nastavit hello následující změny:
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Zde jednoduché volání rozhraní Graph API vrací naše informace.

Ty jsou všechny změny hello, je nutné, aby toodo. Spustit hello `oidlib-sample` aplikace a klikněte na tlačítko **přihlášení**.

Když jste úspěšně ověřen, vyberte hello **požadavku chráněných prostředků** tlačítko tootest toohello vaše volání rozhraní Graph API.

## <a name="get-security-updates-for-our-product"></a>Získejte bezpečnostní aktualizace našich produktů
Doporučujeme vám tooget oznámení o bezpečnostní incidenty v oblasti návštěvou hello [Web Security TechCenter](https://technet.microsoft.com/security/dd252948) a přihlášení k odběru tooSecurity Advisory Alerts.

