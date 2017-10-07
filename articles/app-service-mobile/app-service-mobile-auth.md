---
title: aaaAuthentication a autorizaci v Azure Mobile Apps | Microsoft Docs
description: "Reference konceptu a přehled hello ověřování / autorizace funkcí pro Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5255734481ada11afb65982aebe45c2a349402fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Ověřování a autorizace pro Azure Mobile Apps
## <a name="what-is-app-service-authentication--authorization"></a>Co je aplikace služby ověřování / autorizace?
> [!NOTE]
> Toto téma bude migrované tooa konsolidovat [aplikace služby ověřování / autorizace](../app-service/app-service-authentication-overview.md) téma, které obsahuje webové, mobilní a aplikace API.
> 
> 

Aplikace služby ověřování / autorizace je funkce, která umožňuje toolog vaší aplikace v uživatelé beze změn kódu na back-end aplikace hello vyžaduje. Poskytne tooprotect snadný způsob, jak vaše aplikace a pracovní uživatelská data.

Služby App Service používá federovaných identit, ve kterém 3. stran **zprostředkovatele identity** ("IDP") ukládá účty a ověřuje uživatele a aplikace hello používá tuto identitu místo vlastní. App Service podporuje pět poskytovatelů identit předinstalované hello: *Azure Active Directory*, *Facebook*, *Google*, *Account Microsoft*, a *Twitter*. Tato podpora pro vaše aplikace můžete také rozšířit integrací jiný zprostředkovatel identity nebo řešení vlastní identitu.

Aplikace můžete využít libovolný počet těchto poskytovatelů identit vaši koncoví uživatelé můžou poskytovat s možnostmi pro jak se přihlásit.

Pokud chcete spustit hned tooget, naleznete v jednom hello následující kurzy:

* [Přidání ověřování tooyour iOS aplikace]
* [Přidat aplikaci Xamarin.iOS tooyour ověřování]
* [Přidat aplikaci Xamarin.Android tooyour ověřování]
* [Přidat aplikace pro Windows tooyour ověřování]

## <a name="how-authentication-works"></a>Jak funguje ověřování
V pořadí tooauthenticate pomocí jednoho z poskytovatelů identity hello musíte nejdřív tooknow zprostředkovatele identity hello tooconfigure o vaší aplikaci. poskytovatel identity Hello vám potom poskytne ID a tajné klíče zadat back toohello aplikace. To dokončí hello vztah důvěryhodnosti a umožňuje tooit identity poskytuje toovalidate služby App Service.

Tyto kroky jsou podrobně popsané v následujících tématech hello:

* [Jak tooconfigure vaše aplikace toouse Azure Active Directory přihlášení]
* [Jak tooconfigure vaše aplikace toouse Facebook přihlášení]
* [Jak tooconfigure vaše aplikace toouse Google přihlášení]
* [Jak tooconfigure vaše aplikace toouse Account Microsoft přihlášení]
* [Jak tooconfigure vaše aplikace toouse Twitter přihlášení]

Jakmile je všechno nastavené na back-end hello, můžete upravit vašeho klienta toolog v. Existují dva přístupy tady:

* Použijete jeden řádek kódu, umožní hello Mobile Apps klienta SDK přihlášení uživatele.
* Využívejte sady SDK, které zveřejnil identitu danou identitu zprostředkovatele tooestablish a potom získat přístup k tooApp služby.

> [!TIP]
> Většina aplikace by měly používat tooget SDK na poskytovatele, přihlašování nativní pocit a podpora tooleverage aktualizace a jiné výhody specifický pro zprostředkovatele.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Jak funguje ověřování bez poskytovatele sady SDK
Pokud nechcete, aby tooset si poskytovatele sady SDK, můžete povolit přihlášení hello tooperform Mobile Apps pro vás. Hello Mobile Apps klienta SDK se otevře toohello poskytovatele webové zvolíte a dokončení hello přihlašovacích údajů. Příležitostně na blogy a fóra, které se zobrazí tato označuje tooas hello "toku serveru" nebo "směrované serveru pohybu" od serveru hello spravuje hello přihlášení a hello klienta SDK nikdy přijme token zprostředkovatele hello.

Kód Hello potřeba toostart, které tento tok je popsaná v kurzu hello ověřování pro každou platformu. Na konci hello hello toku má klient hello SDK tokenu služby App Service a hello token je automaticky připojené tooall požadavky toohello back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Jak funguje ověřování u poskytovatele sady SDK
Práci s poskytovatelem SDK umožňuje hello přihlašování toointeract více úzce s hello platformy, které běží aplikace hello operačního systému. To také umožňuje token zprostředkovatele a některé informace o uživateli na hello klienta, což umožňuje mnohem jednodušší graf tooconsume rozhraní API a přizpůsobit hello uživatelské prostředí. Čas od času na blogy a fóra uvidíte toto odkazuje tooas hello "tok klienta" nebo "přesměruje klienta pohybu" od kódu v klientovi hello zpracovává hello přihlášení a kód klienta hello má přístup tooa zprostředkovatele tokenu.

Po získání tokenu zprostředkovatele musí toobe odeslané tooApp služby pro ověření. Na konci hello hello toku má klient hello SDK tokenu služby App Service a hello token je automaticky připojené tooall požadavky toohello back-end. Hello vývojáře můžete také ponechat token odkazu toohello zprostředkovatele, pokud se rozhodne.

## <a name="how-authorization-works"></a>Jak funguje autorizace
Aplikace služby ověřování / autorizace zpřístupňuje několik možností **tootake akce, když požadavek nebude ověřený**. Než kódu obdrží daného požadavku, můžete mít toosee zkontrolujte služby App Service, pokud ověření žádosti o hello a pokud ne, odmítněte ho a pokusit se toohave hello přihlášení uživatele než to zkusíte znovu.

Jednou z možností je, že toohave neověřené požadavky přesměrování tooone zprostředkovatelů identity hello. Ve webovém prohlížeči to by ve skutečnosti trvat hello uživatele tooa nová stránka. Však tímto způsobem nelze přesměrovat mobilního klienta a obdrží neověřené odpovědi HTTP *401 – Neověřeno* odpovědi. To, první požadavek hello díky vašeho klienta by mělo být vždy koncový bod toohello přihlášení, a poté můžete provést volání tooany jiná rozhraní API. Pokud pokus toocall jiná rozhraní API před protokolování na vašeho klienta dojde k chybě.

Pokud chcete toohave podrobnější řízení, přes které koncové body vyžadují ověřování, můžete také vybrat "žádná akce (povolit žádosti o)" pro neověřené požadavky. V takovém případě jsou všechna rozhodnutí ověřování odložení tooyour kódu aplikace. To vám také umožní tooallow přístup toospecific uživatele na základě pravidel autorizace.

## <a name="documentation"></a>Dokumentace
Dobrý den, jak následující kurzy zobrazit tooadd ověřování tooyour mobilních klientů pomocí služby App Service:

* [Přidání ověřování tooyour iOS aplikace]
* [Přidat aplikaci Xamarin.iOS tooyour ověřování]
* [Přidat aplikaci Xamarin.Android tooyour ověřování]
* [Přidat aplikace pro Windows tooyour ověřování]

Dobrý den, jak následující kurzy zobrazit tooconfigure služby App Service tooleverage různá ověřovací zprostředkovatele:

* [Jak tooconfigure vaše aplikace toouse Azure Active Directory přihlášení]
* [Jak tooconfigure vaše aplikace toouse Facebook přihlášení]
* [Jak tooconfigure vaše aplikace toouse Google přihlášení]
* [Jak tooconfigure vaše aplikace toouse Account Microsoft přihlášení]
* [Jak tooconfigure vaše aplikace toouse Twitter přihlášení]

Nechcete-li toouse s identity systémem jiným než ty, které hello zadaný zde, můžete využít i hello [náhled podpora vlastní ověřování v rozhraní .NET server hello SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Přidání ověřování tooyour iOS aplikace]: app-service-mobile-ios-get-started-users.md
[Přidat aplikaci Xamarin.iOS tooyour ověřování]: app-service-mobile-xamarin-ios-get-started-users.md
[Přidat aplikaci Xamarin.Android tooyour ověřování]: app-service-mobile-xamarin-android-get-started-users.md
[Přidat aplikace pro Windows tooyour ověřování]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Jak tooconfigure vaše aplikace toouse Azure Active Directory přihlášení]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak tooconfigure vaše aplikace toouse Facebook přihlášení]: app-service-mobile-how-to-configure-facebook-authentication.md
[Jak tooconfigure vaše aplikace toouse Google přihlášení]: app-service-mobile-how-to-configure-google-authentication.md
[Jak tooconfigure vaše aplikace toouse Account Microsoft přihlášení]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak tooconfigure vaše aplikace toouse Twitter přihlášení]: app-service-mobile-how-to-configure-twitter-authentication.md
