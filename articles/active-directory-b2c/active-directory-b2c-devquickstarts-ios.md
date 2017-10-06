---
title: "Získávání tokenu pomocí aplikace pro iOS – Azure AD B2C | Microsoft Docs"
description: "Tento článek vám ukáže, jak toocreate aplikaci iOS, která používá AppAuth s Azure Active Directory B2C toomanage uživatelských identit a ověření uživatelů."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: e7cbe2de6e9ae3d45448cdd36292c457a0ef4887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Přihlaste se pomocí aplikace pro iOS

Platforma identity Microsoft Hello používá otevřete standardy, jako je OAuth2 a OpenID Connect. Pomocí otevřený standardní protokol nabízí více možností pro vývojáře, při výběru toointegrate knihovny pomocí našich služeb. Nabízíme tento návod a ostatní jako ho vývojáři tooaid s psaní aplikací, které se připojují toohello platformy Microsoft Identity. Většina knihovny, které implementují [specifikace hello RFC6749 OAuth2](https://tools.ietf.org/html/rfc6749) jsou možné tooconnect toohello Microsoft Identity platformy.

> [!WARNING]
> Společnost Microsoft neposkytuje opravy pro knihovny třetích stran a nebyl provádí kontrolu těchto knihoven. Tato ukázka je použití knihovny třetích stran volat AppAuth, který byl testován pro kompatibilitu v základní scénáře s hello Azure AD B2C. Problémy a žádosti o funkce by měla být směrovanou toohello knihovny open-source projekt. Další informace najdete v [tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Pokud jste nový tooOAuth2 nebo OpenID Connect, mnohem této ukázkové konfigurace nemusí mít mnoho tooyou smysl. Doporučujeme, abyste si prohlédnete brief [přehled protokolu hello jsme jsme tady popisujeme](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C
Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace
Dále musíte toocreate aplikace ve svém adresáři B2C. registrace aplikace Hello poskytuje informace o Azure AD, je nutné toocommunicate bezpečně s vaší aplikací. toocreate mobilní aplikace, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

* Zahrnout **nativního klienta** v aplikaci hello.
* Kopírování hello **ID aplikace** který je přiřazený tooyour aplikace. Tento identifikátor GUID potřebovat později.
* Nastavení **identifikátor URI pro přesměrování** s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Tento identifikátor URI potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Vytvořte svoje zásady
V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje jeden prostředí identit: například společné přihlášení a odhlášení. Vytvořte tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Když vytvoříte hello zásady, nezapomeňte:

* V části **atributy registrace**, vyberte atribut hello **zobrazovaný název**.  Můžete vybrat i další atributy.
* V části **deklarace identity aplikace**, vyberte hello deklarací **zobrazovaný název** a **ID objektu uživatele**. Můžete vybrat i další deklarace identity.
* Kopírování hello **název** po jejím vytvoření každé zásady. Je název vaší zásady předponu `b2c_1_` při ukládání zásad hello.  Název zásady hello potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření zásad jste připravené toobuild vaší aplikace.

## <a name="download-hello-sample-code"></a>Stáhněte si ukázkový kód hello
Uvádíme pracovní vzorku, který používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Můžete stáhnout hello kód a spusťte ho. toouse klienta vlastní Azure AD B2C, postupujte podle pokynů hello v hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Tato ukázka byla vytvořena podle pokynů README hello podle hello [iOS AppAuth projektu na Githubu](https://github.com/openid/AppAuth-iOS). Další informace o fungování hello knihovně a hello ukázková odkazovat hello README AppAuth na Githubu.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Úprava aplikace toouse Azure AD B2C s AppAuth

> [!NOTE]
> AppAuth podporuje iOS 7 a vyšší.  Ale je potřeba toosupport sociálních přihlášení na webu Google, SFSafariViewController, které vyžaduje iOS 9 nebo vyšší.
>

### <a name="configuration"></a>Konfigurace

Komunikaci s Azure AD B2C můžete nakonfigurovat tak, že zadáte koncový bod autorizace hello a koncový bod tokenu identifikátory URI.  toogenerate tyto identifikátory URI, budete potřebovat hello následující informace:
* ID klienta (například contoso.onmicrosoft.com)
* Název zásady (například B2C\_1\_SignUpIn)

Hello koncový bod tokenu může být generována URI nahrazení hello klienta\_ID a hello zásad\_název v hello následující adresu URL:

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Hello koncový bod autorizace URI může být generována nahrazení hello klienta\_ID a hello zásad\_název v hello následující adresu URL:

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Spusťte následující kód toocreate hello AuthorizationServiceConfiguration objektu:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a>Autorizace

Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit požadavek autorizace. požádat o toocreate hello, budete potřebovat hello následující informace:  
* ID klienta (například 00000000-0000-0000-0000-000000000000)
* Identifikátor URI pro přesměrování s vlastní schéma (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Obě položky by byly uloženy když jste byli [aplikaci zaregistrujete](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

tooset do vaší aplikace toohandle hello přesměrování toohello identifikátor URI s hello vlastní schéma, potřebujete tooupdate hello seznam 'schémata URL, ve vaší Info.pList:
* Otevřete Info.pList.
* Pozastavte ukazatel myši nad řádek jako kód sady operačního systému typu a klikněte na tlačítko hello \+ symbol.
* Přejmenujte hello nový řádek "adresa URL typů".
* Klikněte na tlačítko hello šipka doleva toohello "Adresa URL typů" tooopen hello stromu.
* Klikněte na tlačítko hello šipku toohello nalevo od '0 položky' tooopen hello stromu.
* Přejmenujte první položka pod položku 0 too'URL schémat.
* Klikněte na tlačítko hello šipka doleva toohello 'schémata URL, tooopen hello stromu.
* Ve sloupci 'Hodnota' hello je prázdné pole toohello nalevo od 'Položky 0' pod 'Adresy URL Schemes'.  Nastavte hello hodnota tooyour aplikace na jedinečné schéma.  Hello hodnota musí odpovídat používané v redirectURL při vytvoření objektu OIDAuthorizationRequest hello schéma hello.  V našem příkladu jsme použili hello schéma 'com.onmicrosoft.fabrikamb2c.exampleapp'.

Odkazovat toohello [AppAuth průvodce](https://openid.github.io/AppAuth-iOS/) na tom, jak toocomplete hello celého procesu hello. Pokud potřebujete tooquickly začít pracovat s pracovní aplikace, podívejte se na [naše ukázka](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Postupujte podle kroků hello v hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter konfiguraci Azure AD B2C.

Snažíme se vždy otevřete toofeedback a návrhy! Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello. Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
