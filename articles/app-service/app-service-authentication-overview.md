---
title: "aaaAuthentication a autorizace ve službě Azure App Service | Microsoft Docs"
description: "Reference konceptu a přehled hello ověřování / autorizace funkcí pro Azure App Service"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: dc2074b16cce47b72b78ea7afeda89dbc4832166
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Ověřování a autorizace v prostředí Azure App Service
## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / autorizace?
Aplikace služby ověřování / autorizace je funkce, která poskytuje způsob, jak pro vaše aplikace toosign mezi uživateli, takže nemáte toochange kód na back-end aplikace hello. Poskytne tooprotect snadný způsob, jak vaše aplikace a pracovní uživatelská data.

Služby App Service používá federovaných identit, ve kterém poskytovatel identity jiného výrobce uchovává účty a ověřuje uživatele. aplikace Hello spoléhá na informace o identitě hello poskytovatele tak, aby hello aplikace nemá toostore tyto informace sám sebe. App Service podporuje pět poskytovatelů identit předinstalované hello: Azure Active Directory, Facebook, Google, Microsoft Account a Twitter. Aplikace můžete použít libovolný počet tyto tooprovide zprostředkovatelé identity uživatele možnosti pro jak přihlášení. tooexpand hello integrovanou podporu, můžete integrovat jiného poskytovatele identity nebo [řešení vlastní identitu][custom-auth].

Pokud chcete spustit hned tooget, najdete v jednom z hello následující kurzy:

* [Přidat aplikaci iOS tooyour ověřování] [ iOS] (nebo [Android], [Windows], [Xamarin.iOS], [ Xamarin.Android], [Xamarin.Forms], nebo [Cordova])
* [Ověřování uživatelů pro aplikace API v Azure App Service][apia-user]
* [Začínáme s Azure App Service – část 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Princip ověřování ve službě App Service
V pořadí tooauthenticate pomocí jednoho z poskytovatelů identit hello musíte nejdřív tooknow zprostředkovatele identity hello tooconfigure o vaší aplikaci. Zprostředkovatel identity Hello bude potom zadejte ID a tajné klíče, že zadáte tooApp služby. Vztah důvěryhodnosti hello tím dokončíte tak, aby služby App Service můžete ověřit kontrolní výrazy uživatele, jako je například ověřování tokenů, od poskytovatele identity hello.

toosign v uživatele pomocí jednoho z těchto poskytovatelů přesměrovaného tooan koncový bod, který se přihlásí uživatele pro tohoto zprostředkovatele musí být uživatel hello. Pokud zákazníci používají webový prohlížeč, může mít všechny koncový bod neověřené uživatele toohello, který se přihlásí uživatele automaticky nasměrovat služby App Service. Jinak, budete potřebovat toodirect vašim zákazníkům příliš`{your App Service base URL}/.auth/login/<provider>`, kde `<provider>` je jedním z následujících hodnot hello: aad, facebook, google, microsoft nebo twitteru. Mobilní aplikace a API scénáře jsou vysvětlené v částech později v tomto článku.

Uživatelé, kteří pracují s vaší aplikací prostřednictvím webového prohlížeče, bude mít soubor cookie nastavte tak, aby se může zůstat ověřený jako procházení vaší aplikace. U jiných typů klientů, jako je například mobile, webového tokenu JSON (JWT), která by měla zobrazit v hello `X-ZUMO-AUTH` záhlaví, budou vydávat toohello klienta. Hello Mobile Apps klientskou sadu SDK bude zpracovávat to pro vás. Alternativně tokenu identity Azure Active Directory nebo přístupový token může být přímo součástí hello `Authorization` záhlaví jako [tokenu nosiče](https://tools.ietf.org/html/rfc6750).

Služby App Service ověří všechny soubor cookie nebo token, že vaše aplikace problémy tooauthenticate uživatele. toorestrict, kdo má přístup k vaší aplikaci, najdete v části hello [autorizace](#authorization) později v tomto článku.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobilní ověření u poskytovatele sady SDK
Jakmile je všechno nastavené na back-end hello, můžete upravit toosign mobilních klientů pomocí služby App Service. Existují dva přístupy tady:

* Pomocí sady SDK, že danou identitu zprostředkovatele publikuje tooestablish identity a potom získat přístup k tooApp služby.
* Použijte jeden řádek kódu tak, že hello Mobile Apps klientské sady SDK můžete přihlásit uživatele.

> [!TIP]
> Většina aplikace by měly používat poskytovatele sady SDK tooget jednotnější prostředí po přihlášení uživatelé, podpora aktualizace toouse a tooget Určuje další výhody tohoto zprostředkovatele hello.
> 
> 

Pokud používáte poskytovatele sady SDK, uživatelé se mohou přihlásit tooan prostředí, který se integruje s hello operačního systému více úzce aplikaci hello běží na. To také umožňuje token zprostředkovatele a některé informace o uživateli na hello klienta, což umožňuje mnohem jednodušší graf tooconsume rozhraní API a přizpůsobit hello uživatelské prostředí. Příležitostně na blogy a fóra zobrazí se tato odkazované tooas hello "tok klienta" nebo "přesměruje klienta pohybu" vzhledem k tomu, že kód na klientovi hello přihlášení uživatelů a kódem klientské hello má přístup tooa zprostředkovatele tokenu.

Po získání tokenu zprostředkovatele musí toobe odeslané tooApp služby pro ověření. Po služby App Service ověří hello token, služby App Service vytvoří nový token služby App Service, která je vrácena toohello klienta. Hello Mobile Apps klienta SDK obsahuje pomocné metody toomanage toto exchange a automaticky připojit hello tokenu tooall požadavky toohello aplikace back-end. Vývojáři mohou také ponechat token odkazu toohello zprostředkovatele, pokud se tak rozhodne.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobilní ověřování bez poskytovatele sady SDK
Pokud nechcete, aby tooset si poskytovatele sady SDK, můžete povolit funkce Mobile Apps hello toosign Azure App Service v za vás. Hello Mobile Apps klienta SDK bude otevřete poskytovatele webových zobrazení toohello dle vašeho výběru a přihlášení uživatele hello. Čas od času na blogy a fóra, uvidíte tento odkazované tooas hello "serveru pohybu" nebo "pohybu směrované serveru" protože hello server spravuje hello proces, který se přihlásí uživatele a klient hello SDK nikdy obdrží hello zprostředkovatele tokenu.

Kód toostart tento tok je zahrnuta v kurzu hello ověřování pro každou platformu. Na konci hello hello toku má klient hello SDK tokenu služby App Service a hello token je automaticky připojené tooall požadavky toohello aplikace back-end.

### <a name="service-to-service-authentication"></a>Ověřování služba-služba
I když můžete udělit uživatelům přístup tooyour aplikace, můžete také důvěřovat jiné aplikace toocall vlastní rozhraní API. Například můžete mít jednu webovou aplikaci volat rozhraní API v jiné webové aplikace. V tomto scénáři použijte přihlašovací údaje pro účet služby místo tooget přihlašovací údaje uživatele token. Je také označován jako účet služby *instanční objekt* v Azure Active Directory slangu a ověřování, který používá uvedený účet je také označován jako scénáři service-to-service.

> [!IMPORTANT]
> Vzhledem k tomu mobilní aplikace běží v zařízení zákazníka, mobilní aplikace se *není* se počítají jako důvěryhodné aplikace a neměli používat k hlavní toku služby. Místo toho měli používat toku uživatele, který byl dříve podrobné.
> 
> 

Pro scénáře service to service služby App Service chrání vaše aplikace pomocí služby Azure Active Directory. volající aplikace Hello právě musí tooprovide Azure Active Directory service hlavní autorizační token, který byl získán zadáním hello ID klienta a klient tajný z Azure Active Directory. Příklad tohoto scénáře, který používá rozhraní API ASP.NET aplikace je vysvětleno v kurzu hello [objekt zabezpečení ověřování služby pro aplikace API][apia-service].

Pokud chcete toouse toohandle ověřování služby App Service scénáři service-to-service, můžete použít klientské certifikáty nebo základní ověřování. Informace o klientské certifikáty v Azure najdete v tématu [jak tooConfigure vzájemné ověřování TLS pro webové aplikace](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informace o základní ověřování v technologii ASP.NET najdete v tématu [filtry ověřování ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/authentication-filters).

Ověřování účtu služby z aplikace služby App Service logic app tooan rozhraní API je zvláštní případ, který je podrobně popsán v [používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Princip ověřování ve službě App Service
Máte plnou kontrolu nad hello požadavků, které můžete přístup k aplikaci. Aplikace služby ověřování / autorizace můžete nakonfigurovat pomocí některé z hello následující chování:

* Povolte pouze ověřené žádosti tooreach vaší aplikace.
  
    Pokud prohlížeč obdrží požadavek na anonymní, služby App Service přesměruje tooa stránku hello zprostředkovatele identity, který zvolíte, aby uživatelé mohli podepsat. Pokud hello požadavek pochází z mobilního zařízení, hello vrátí odpověď HTTP je *401 – Neověřeno* odpovědi.
  
    Tato možnost není nutné toowrite žádné ověřovací kód vůbec ve vaší aplikaci. Pokud potřebujete jemnějšího autorizace, informace o uživateli hello je k dispozici tooyour kódu.
* Tooreach všechny požadavky, aby vaše aplikace, ale ověření ověřené žádosti a předávají informace o ověřování v hlavičkách hello protokolu HTTP.
  
    Tato možnost odkládat údaje kódu aplikace tooyour autorizační rozhodnutí. Poskytuje větší flexibilitu při zpracování anonymních požadavků, ale máte toowrite kódu.
* Všechny požadavky tooreach, aby vaše aplikace a provádět žádnou akci na informace o ověřování v žádostech o hello.
  
    V takovém případě hello ověřování / autorizace funkce je vypnutá. Hello úlohy ověřování a autorizace jsou zcela tooyour kódu aplikace.

předchozí chování Hello jsou řízeny hello **tootake akce, když požadavek nebude ověřený** možnost v hello portálu Azure. Pokud se rozhodnete ** protokolu s *název zprostředkovatele* **, všechny požadavky mít toobe ověření. **Povolit požadavku (žádná akce)** odkládat údaje hello autorizační rozhodnutí tooyour kód, ale stále poskytuje informace o ověřování. Pokud chcete, aby toohave váš kód zpracovat vše, můžete zakázat hello ověřování / autorizace funkce.

## <a name="working-with-user-identities-in-your-application"></a>Práce s identit uživatelů ve vaší aplikaci
Služby App Service předá některé uživatelská informace tooyour aplikace pomocí speciálními záhlavími. Externí požadavky zakázat tyto hlavičky a se nachází v případě nastavit pouze pomocí aplikace služby ověřování / autorizace. Některé příklad hlavičky zahrnují:

* X-MS-KLIENTA HLAVNÍ NÁZEV
* X-MS-CLIENT-HLAVNÍ ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kód, který je napsán v libovolném jazyce nebo rozhraní můžete získat z těchto hlavičky hello informace, které potřebuje. Pro technologii ASP.NET 4.6 aplikace hello **ClaimsPrincipal** bude automaticky nastavena s příslušnými hodnotami hello.

Aplikace také můžete získat podrobnosti o další uživatele prostřednictvím HTTP GET na hello `/.auth/me` koncový bod vaší aplikace. Neplatný token, který je součástí požadavku hello se vrátit datové části JSON s podrobnostmi o hello zprostředkovatele, který se používá, hello základní zprostředkovatel tokenu a některé další informace o uživateli. Hello Mobile Apps server SDK zadejte pomocné metody toowork s těmito daty. Další informace najdete v tématu [jak toouse hello Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), a [pracovat s hello .NET back-end serveru SDK pro Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentace a další prostředky
### <a name="identity-providers"></a>Zprostředkovatelé identity
Dobrý den, jak následující kurzy zobrazit tooconfigure služby App Service toouse různá ověřovací zprostředkovatele:

* [Jak tooconfigure vaše aplikace toouse Azure Active Directory přihlášení][AAD]
* [Jak tooconfigure vaše aplikace toouse Facebook přihlášení][Facebook]
* [Jak tooconfigure vaše aplikace toouse Google přihlášení][Google]
* [Jak tooconfigure vaše aplikace toouse Account Microsoft přihlášení][MSA]
* [Jak tooconfigure vaše aplikace toouse Twitter přihlášení][Twitter]

Pokud chcete toouse s identity systémem jiným než ty, které hello zadaný zde, můžete také použít hello [náhled podpora vlastní ověřování v server hello mobilní aplikace .NET SDK][custom-auth], který může být použit ve službě web apps mobilní aplikace, nebo rozhraní API.

### <a name="web-applications"></a>Webové aplikace
Hello následující kurzy ukazují, jak tooadd ověřování tooa webové aplikace:

* [Začínáme s Azure App Service – část 2][web-getstarted]

### <a name="mobile-applications"></a>Mobilní aplikace
Hello následující kurzy ukazují, jak tooadd ověřování tooyour mobilních klientů pomocí hello toku směrované serveru:

* [Přidání ověřování tooyour iOS aplikace][iOS]
* [Přidat aplikaci pro Android ověřování tooyour][Android]
* [Přidat aplikace pro Windows ověřování tooyour][Windows]
* [Přidat ověřování aplikace Xamarin.iOS tooyour][Xamarin.iOS]
* [Přidat ověřování aplikace Xamarin.Android tooyour][ Xamarin.Android]
* [Přidat aplikaci Xamarin.Forms tooyour ověřování][Xamarin.Forms]
* [Přidat aplikaci Cordova tooyour ověřování][Cordova]

Použijte hello následující prostředky, pokud chcete, aby toouse hello přesměruje klienta tok pro Azure Active Directory:

* [Použití hello Active Directory Authentication Library pro iOS][ADAL-iOS]
* [Použití hello Active Directory Authentication Library pro Android][ADAL-Android]
* [Použít hello Active Directory Authentication knihovny pro systém Windows a Xamarin][ADAL-dotnet]

Použijte následující prostředky, pokud chcete, aby toouse hello přesměruje klienta tok pro Facebook hello:

* [Použití hello Facebook SDK pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Použijte následující prostředky, pokud chcete, aby toouse hello přesměruje klienta tok pro Twitter hello:

* [Pomocí služby Twitter prostředků infrastruktury pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Použijte následující prostředky, pokud chcete, aby toouse hello přesměruje klienta tok pro Google hello:

* [Použití hello Google přihlášení SDK pro iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>Aplikace rozhraní API
Dobrý den, jak následující kurzy zobrazit tooprotect vaše aplikace API:

* [Ověřování uživatelů pro aplikace API v Azure App Service][apia-user]
* [Objekt zabezpečení ověřování služby pro aplikace API v Azure App Service][apia-service]

[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[ Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
