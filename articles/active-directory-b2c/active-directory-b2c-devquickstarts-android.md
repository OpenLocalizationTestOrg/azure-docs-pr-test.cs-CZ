---
title: "Azure Active Directory B2C: Získávání tokenu pomocí aplikace pro Android | Microsoft Docs"
description: "Tento článek vám ukáže, jak toocreate Android aplikaci, která používá AppAuth s Azure Active Directory B2C toomanage uživatelských identit a ověření uživatelů."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: 0236398673115a34951f035cb1e73e89417abf86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Přihlaste se pomocí aplikace pro Android

Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect. To umožňuje vývojáři tooleverage žádnou knihovnu si přejí toointegrate s našich služeb. Vývojáři tooaid pomocí naše platforma s další knihovny, jsme jste zapsána několik návody jako tento jeden toodemonstrate jak tooconfigure 3. stran knihovny tooconnect toohello identity platformu Microsoft. Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) bude mít tooconnect toohello Microsoft Identity platformy.

> [!WARNING]
> Společnost Microsoft neposkytuje opravy pro 3. stran knihovny a nebyl provádí kontrolu těchto knihoven. Tato ukázka používá knihovnu 3. stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s hello Azure AD B2C. Problémy a žádosti o funkce by měla být směrovanou toohello knihovny open-source projekt. Najdete v tématu [v tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) Další informace.  
>
>

Pokud jste nový tooOAuth2 nebo OpenID Connect mnohem této ukázkové konfigurace nemusí mít mnoho tooyou smysl. Doporučujeme, abyste si prohlédnete brief [přehled protokolu hello jsme jsme tady popisujeme](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace

Dále musíte toocreate aplikace ve svém adresáři B2C. Díky tomu získá informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací. toocreate mobilní aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

* Zahrnout **nativního klienta** v aplikaci hello.
* Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. Budete potřebovat později.
* Nastavit nativního klienta **identifikátor URI pro přesměrování** (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). To také budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení. Je třeba toocreate tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Když vytvoříte hello zásady, nezapomeňte:

* Zvolte hello **zobrazovaný název** jako atribut registrace ve svojí zásadě.
* Zvolte hello **zobrazovaný název** a **ID objektu** deklarace identity aplikace v každé zásadě. Můžete zvolit i další deklarace identity.
* Kopírování hello **název** po jejím vytvoření každé zásady. Měl by mít předponu hello `b2c_1_`.  Název zásady hello budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření zásad jste připravené toobuild vaší aplikace.

## <a name="download-hello-sample-code"></a>Stáhněte si ukázkový kód hello

Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Můžete stáhnout hello kód a spusťte ho. Můžete rychle začít pracovat s vlastní aplikace pomocí Azure AD B2C konfiguraci podle pokynů hello v hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

Ukázka Hello je změna hello ukázky poskytované [AppAuth](https://openid.github.io/AppAuth-Android/). Navštivte toolearn jejich stránky, další informace o AppAuth a jejich funkce.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Úprava aplikace toouse Azure AD B2C s AppAuth

> [!NOTE]
> AppAuth podporuje Android API 16 (Jellybean) a vyšší. Doporučujeme používat rozhraní API 23 a vyšší.
>

### <a name="configuration"></a>Konfigurace

Komunikaci s Azure AD B2C můžete nakonfigurovat buď zadání hello zjišťování URI nebo zadáním koncový bod autorizace hello a koncový bod tokenu identifikátory URI. V obou případech budete potřebovat hello následující informace:

* ID klienta (např. contoso.onmicrosoft.com)
* Název zásady (například B2C\_1\_SignUpIn)

Pokud zvolíte tooautomatically zjistit hello autorizace a token koncový bod identifikátory URI, budete potřebovat toofetch informace ze zjišťování hello identifikátor URI. identifikátor URI může být generována nahrazení hello klienta zjišťování Hello\_ID a hello zásad\_název v hello následující adresu URL:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Potom můžete získat hello autorizace a koncový bod tokenu identifikátory URI a vytvořit objekt AuthorizationServiceConfiguration spuštěním hello následující:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed tooretrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed tooauthorization...
        }
      }
  });
```

Místo použití zjišťování tooobtain hello autorizace a koncový bod tokenu identifikátory URI, je můžete také zadat explicitně nahrazením hello klienta\_ID a hello zásad\_názvem v adrese URL hello níže:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Spusťte následující kód toocreate hello AuthorizationServiceConfiguration objektu:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a>Autorizace

Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace. požádat o toocreate hello, budete potřebovat hello následující informace:

* ID klienta (například 00000000-0000-0000-0000-000000000000)
* Identifikátor URI pro přesměrování s vlastní schéma (např. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Podrobnosti najdete toohello [AppAuth průvodce](https://openid.github.io/AppAuth-Android/) na tom, jak toocomplete hello celého procesu hello. Pokud potřebujete tooquickly začít pracovat s pracovní aplikace, podívejte se na [naše ukázka](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Postupujte podle kroků hello v hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter konfiguraci Azure AD B2C.

Snažíme se vždy otevřete toofeedback a návrhy! Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello. Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

