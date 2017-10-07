---
title: aaaAuthentication a autorizace API Apps v Azure App Service | Microsoft Docs
description: "Další informace o hello ověřování a autorizace služby, které poskytuje Azure App Service API Apps."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: d620b53a-5a6f-41c9-84c7-f7ef5ff02ae7
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: alkarche
ms.openlocfilehash: 6d26754b8b606ec232a74768787922ea80577c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Ověřování a autorizace API Apps v Azure App Service
## <a name="overview"></a>Přehled
> [!NOTE]
> Toto téma bude migrované tooa konsolidovat [aplikace služby ověřování / autorizace](../app-service/app-service-authentication-overview.md) téma, které obsahuje webové, mobilní a aplikace API.
> 
> 

Aplikační služba Azure nabízí integrované ověřování a autorizace služby, které implementují [OAuth 2.0](#oauth) a [OpenID Connect](#oauth). Tento článek popisuje hello služeb a možnosti, které jsou k dispozici pro API Apps v Azure App Service.

Hello následující diagram znázorňuje některé klíčové vlastnosti ověřování služby App Service:

* Se upraví příchozích žádostí o rozhraní API, což znamená, že funguje s libovolném jazyce nebo rozhraní podporované službou App Service.
* Nabízí, které mají několik možností, kolik ověřování můžete použít toodo v kódu.
* Funguje pro koncové uživatele a ověřování účtu služby. 
* Podporuje pět poskytovatelů identit: Azure Active Directory, Facebook, Google, Twitter a Account Microsoft.
* Ho hello stejné funguje pro aplikace API, webové aplikace a mobilní aplikace.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Bez ohledu na jazyk
Zpracování ověřování služby App Service se stane, než požadavky spojit aplikace API, což znamená, že funkce ověřování hello fungovat pro aplikace API, které jsou napsané v libovolném jazyce nebo rozhraní.  Rozhraní API může být založen na technologii ASP.NET, Java, Node.js nebo libovolnou architekturu, který podporuje služby App Service.

Předá služby App Service na hello JSON web token (JWT) v hello autorizační hlavičky požadavku HTTP a kód napsaný v libovolném jazyce nebo rozhraní můžete získat hello informace, které je nutné z hello tokenu. Kromě toho služby App Service umožňuje snazší přístup toohello nejčastěji používaná deklarací nastavením některé speciální hlavičky, jako je například hello následující:

* X-MS-KLIENTA HLAVNÍ NÁZEV
* X-MS-CLIENT-HLAVNÍ ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

V rozhraní .NET API, můžete použít hello `Authorize` atribut a pro jemně odstupňovaných autorizace můžete napsat snadno kódu na základě deklarace identity, protože informace deklarace identity se importují pro vás v rozhraní .NET třídy.

## <a name="multiple-protection-options"></a>Více možností ochrany
Služby App Service může zabránit anonymních požadavků protokolu HTTP v dosažení aplikace API, můžete předat na všechny požadavky a ověřovat tokeny pro požadavky, které je obsahují, nebo ji nechat přes všechny požadavky bez provedení akce na ně:

1. Povolit pouze ověřené žádosti tooreach aplikace API.
   
    Pokud byl přijat požadavek na anonymní z prohlížeče služby App Service přesměruje tooa přihlašovací stránku hello zprostředkovatele ověřování (Azure AD, Google, Twitter, atd.), které zvolíte. 
   
    Tato možnost není nutné toowrite žádné ověřovací kód vůbec ve vaší aplikaci a autorizační kód je jednodušší, protože hello nejdůležitější deklarace identity jsou uvedeny v záhlaví hello HTTP.
2. Povolit všechny tooreach požadavků vaší aplikace API, ale ověření ověřené žádosti a předávají informace o ověřování v hlavičkách hello HTTP.
   
    Tato možnost vám dává větší flexibilitu při zpracování anonymních požadavků, ale máte toowrite kódu, pokud chcete, aby anonymní uživatelé tooprevent pomocí rozhraní API. Vzhledem k tomu, že hello nejoblíbenější deklarace identity se předávají v hello hlaviček požadavků HTTP, je relativně jednoduché autorizační kód.
3. Povolit všechny požadavky tooreach vaše rozhraní API, provádět žádnou akci na informace o ověřování v žádostech o hello.
   
    Tato možnost zanechává hello úlohy ověřování a autorizace zcela až tooyour kódu aplikace.

V hello [portál Azure](https://portal.azure.com/), vyberte možnost hello, kterou chcete na hello **ověřování / autorizace** okno.

![](./media/app-service-api-authentication/authblade.png)

Možnosti 1 a 2, zapnout **ověřování služby aplikace**a v hello **tootake akce, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit** nebo **Povolit požadavku (žádná akce)**.  Pokud se rozhodnete **přihlásit**, máte toochoose zprostředkovatele ověřování a konfiguraci tohoto zprostředkovatele.

![](./media/app-service-api-authentication/actiontotake.png)

Podrobné informace o ověřování tooconfigure, najdete v části [jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Hello článek se týká tooAPI aplikace a také mobilní aplikace, a propojí tooother články pro hello dalších zprostředkovatelů ověření.

## <a id="internal"></a>Ověřování účtu služby
Ověřování služby App Service funguje pro interní scénáře, jako pro volání z jedné aplikace tooanother rozhraní API aplikace API. V tomto scénáři můžete získat token pomocí přihlašovacích údajů pro účet služby místo přihlašovacích údajů koncového uživatele. Je také označován jako účet služby *instanční objekt* v Azure Active Directory a ověřování pomocí uvedený účet je také označován jako scénáři service-to-service. 

Pro scénáře service-to-service chránit hello volá aplikaci API pomocí Azure Active Directory a poskytovat služby hlavní autorizační token AAD při volání aplikace hello rozhraní API. Získat token zadáním hello ID klienta a klient tajný z hello AAD aplikace. Žádné speciální jen Azure kód je povinné, například využité toobe hodnotu true pro zpracování tokenu hello záhlaví Zumo Mobile Services. Příklad tohoto scénáře použití aplikací rozhraní API technologie ASP.NET je popsaná v kurzu hello [objekt zabezpečení ověřování služby pro aplikace API](app-service-api-dotnet-service-principal-auth.md).

Pokud chcete toohandle scénáři service-to-service bez použití ověřování služby App Service, můžete použít klientské certifikáty nebo základní ověřování. Informace o klientské certifikáty v Azure najdete v tématu [jak tooConfigure vzájemné ověřování TLS pro webové aplikace](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Informace o základní ověřování v technologii ASP.NET najdete v tématu [filtry ověřování ve webovém rozhraní API 2 ASP.NET](http://www.asp.net/web-api/overview/security/authentication-filters).

Ověřování účtu služby z aplikace služby App Service logic app tooan rozhraní API je zvláštní případ, která je vysvětlená v [používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Ověření mobilního klienta
Informace o ověřování toohandle z mobilních klientů, najdete v části hello [dokumentaci o ověřování pro mobilní aplikace](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Ověřování služby App Service funguje hello stejným způsobem jako pro mobilní aplikace a aplikace API.

## <a name="more-information"></a>Další informace
Další informace o ověřování a autorizace ve službě Azure App Service najdete v tématu hello následující prostředky:

* [Se zvětšující služby App Service ověřování / autorizace](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)
* [Jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (obsahuje odkazy pro ostatní zprostředkovatelé ověřování v horní části hello hello stránky.) 

Další informace o protokolu OAuth 2.0, OpenID Connect a webové tokeny JSON (JWT) najdete v tématu hello následující prostředky.

* [Začínáme s OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "Začínáme s OAuth 2.0") 
* [Úvod tooOAuth2, OpenID Connect a JSON webové tokeny (JWT) - Pluralsight kurzu](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Vytváření a zabezpečení rozhraní RESTful API pro více klientů v technologii ASP.NET - Pluralsight kurzu](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Další informace o službě Azure Active Directory najdete v tématu hello následující prostředky.

* [Scénáře služby Azure AD](http://aka.ms/aadscenarios)
* [Průvodce Azure AD vývojářům](http://aka.ms/aaddev)
* [Ukázek Azure AD](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Další kroky
Tento článek obsahuje vysvětlení funkce ověřování a autorizace služby App Service, který můžete použít pro aplikace API. Další kurz Hello při získávání hello spuštění řady ukazuje jak tooimplement [ověřování uživatelů v App Service API Apps](app-service-api-dotnet-user-principal-auth.md).

