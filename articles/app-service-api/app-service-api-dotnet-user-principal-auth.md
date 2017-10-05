---
title: "Ověřování uživatelů pro aplikace API v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak chránit aplikace API v Azure App Service, povolí přístup pouze ověřené uživatele."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 3896760d-46ff-4b67-b98a-edd233f24758
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: a5b713f3a60b3886d813438496d704f464d914e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Ověřování uživatelů pro aplikace API v Azure App Service
## <a name="overview"></a>Přehled
Tento článek ukazuje, jak do aplikace Azure API jsou chráněny, aby ji volat pouze ověřené uživatele. Článek předpokládá, že jste četli [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md).

Naučíte se:

* Postup konfigurace zprostředkovatele ověřování, s podrobnostmi o službě Azure Active Directory (Azure AD).
* Jak používat chráněné aplikace API přes [Active Directory Authentication Library (ADAL) pro JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Článek obsahuje dvě části:

* [Postup konfigurace ověřování uživatelů ve službě Azure App Service](#authconfig) část popisuje obecně konfiguraci ověřování uživatelů pro libovolnou aplikaci API a vztahuje stejnou měrou na všech rozhraní podporovaných službou App Service, včetně .NET, Node.js a Java.
* Od verze [pokračováním kurzů k .NET API Apps](#tutorialstart) části článek vás provede konfiguraci ukázkové aplikace s .NET back-end a AngularJS front-end tak, aby používala Azure Active Directory pro ověřování uživatelů. 

## <a id="authconfig"></a>Postup konfigurace ověřování uživatelů ve službě Azure App Service
Tato část obsahuje obecné pokyny, které platí pro všechny aplikace API. Konkrétní kroky pro ukázkovou aplikaci udělat rozhraní seznamu .NET přejděte na [pokračování úvodních kurzů k .NET](#tutorialstart).

1. V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, který chcete chránit, vyhledejte **funkce** části a potom klikněte na **ověřování / autorizace**.
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. Vyberte jednu z možností z **akci provést, když požadavek nebude ověřený** rozevíracího seznamu.
   
   * Pokud chcete pouze ověřené volání k dosažení aplikace API, vyberte jednu z **protokolu s...**  možnosti. Tato možnost umožňuje chránit aplikace API bez nutnosti napsat kód, který běží v ní.
   * Pokud chcete, aby všechna volání k dosažení aplikace API, zvolte **povolit požadavku (žádná akce)**. Tuto možnost můžete použít k přímé neověřené volající k vybrané zprostředkovatele ověřování. Pomocí této možnosti budete muset napsat kód pro autorizaci.
     
     Další informace najdete v tématu [Ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md#multiple-protection-options).
4. Vyberte jeden nebo více **zprostředkovatele ověřování**.
   
    Obrázek ukazuje možnosti, které vyžadují všechny volající ověřovány službou Azure AD.
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    Když zvolíte zprostředkovatele ověřování, na portálu zobrazí okno konfigurace pro tohoto zprostředkovatele. 
   
    Podrobné pokyny, které vysvětlují, jak nakonfigurovat oken konfigurace zprostředkovatele ověřování najdete v tématu [postup konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Tento odkaz je odkazem na článek o službě Azure AD, ale samotný článek obsahuje karty, které odkazují na články pro ostatní zprostředkovatelé ověřování.)
5. Když jste hotovi s okno Konfigurace zprostředkovatele ověřování, klikněte na tlačítko **OK**.
6. V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.

Když to uděláte, ověřuje služby App Service všechna volání rozhraní API, než dosáhnou aplikace API. Ověřovací služby fungovat stejně pro všechny jazyky, které podporuje služby App service, včetně .NET, Node.js a Java. 

Chcete-li ověřených volání rozhraní API, volající zahrnuje zprostředkovatele ověřování tokenu nosiče OAuth 2.0 v hlavičce autorizace požadavků HTTP. Pomocí sady SDK pro zprostředkovatele ověřování, může získat token.

## <a id="tutorialstart"></a>Pokračováním kurzů k .NET API Apps
Pokud postupujete podle Node.js nebo Java kurzy pro aplikace API, přejděte k další článek, [objekt zabezpečení ověřování služby pro aplikace API](app-service-api-dotnet-service-principal-auth.md). 

Pokud postupujete podle řady kurz .NET pro aplikace API a už jste nasadili ukázkovou aplikaci podle pokynů v [první](app-service-api-dotnet-get-started.md) a [druhý](app-service-api-cors-consume-javascript.md) kurzy, pokračujte [nastavení ověřování v App Service a Azure AD](#azureauth) části.

Pokud chcete v tomto kurzu bez průchodu přes kurzy první a druhé, proveďte následující kroky, které popisují, jak začít pracovat s použitím automatizovaného procesu nasazení ukázkové aplikace.

> [!NOTE]
> Následující kroky vám pomůžou na stejné počáteční bod jako, pokud jste to absolvovali první dva kurzy, s jednou výjimkou: Visual Studio nebude již víte, které webové aplikace nebo aplikace API, který získá každý projekt nasazen. To znamená, že nebudete mít přesné pokyny v tomto kurzu, které vysvětlují, jak nasadit do správné cíle. Pokud si nejste tedy se nad tím, jak to provést kroky nasazení sami, je lepší postupujte podle kurzu řady z [první kurz](app-service-api-dotnet-get-started.md) než začínat to automatizované procesu nasazení.
> 
> 

1. Ujistěte se, že máte všechny požadavky uvedené v [první kurz](app-service-api-dotnet-get-started.md). Kromě předpoklady uvedené tato kurzech ověřování předpokládají, jste už pracovali s webové aplikace aplikační služby a aplikace API v sadě Visual Studio a portálu Azure.
2. Klikněte na tlačítko **nasadit do Azure** v tlačítko [soubor readme úložiště seznam úkolů ukázkové](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) k nasazení aplikace API a webové aplikace. Skupina prostředků Azure, která je vytvořena, jak to můžete později k vyhledání webové aplikace a API aplikace názvy si poznamenejte.
3. Stáhnout nebo klonovat [seznam úkolů ukázkové úložiště](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) získat kód, který budete pracujete s místně v sadě Visual Studio.

## <a id="azureauth"></a>Nastavení ověřování v App Service a Azure AD
Nyní máte aplikace běžící v Azure App Service bez nutnosti ověřit uživatele. V této části přidáte ověřování provedením následujících úloh:

* Nakonfigurujte aplikační služby a chcete vyžadovat ověřování Azure Active Directory (Azure AD) pro volání metody aplikace API střední vrstvy.
* Vytvořte aplikaci Azure AD.
* Nakonfigurujte aplikace Azure AD k odeslání token nosiče po přihlášení do AngularJS front-endu. 

Pokud narazíte na problémy při následujícím kurzu pokynů, najdete v článku [Poradce při potížích s](#troubleshooting) části na konci tohoto kurzu. 

### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfigurovat ověřování pro aplikaci API střední vrstvy
1. V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, které jste vytvořili pro projekt ToDoListAPI najít **funkce** části a potom klikněte na **ověřování / autorizace**.
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. V **akci provést, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory**.
   
    Tato možnost zajistí, že žádné žádosti o unauathenticated dosáhne aplikace API. 
4. V části **zprostředkovatele ověřování**, klikněte na tlačítko **Azure Active Directory**.
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. V **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**
   
    ![Možnost Azure Express ověřování/autorizace služby portálu](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    Pomocí **Express** možnosti služby App Service můžete automaticky vytvořit aplikaci Azure AD ve službě Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nemusíte vytvořit klienta, protože každý účet Azure automaticky jeden má.
6. V části **režim správy**, klikněte na tlačítko **vytvořit novou aplikaci AD** Pokud již není vybrána a Všimněte si, hodnotu, která je v **vytvořit aplikaci** textového pole; budete vyhledání této AAD aplikace na portálu Azure classic později.
   
    ![Nastavení Azure portálu Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure automaticky vytvoří aplikaci Azure AD s názvem uvedené v klientovi služby Azure AD. Ve výchozím nastavení, aplikace Azure AD se stejným názvem jako aplikace API. Pokud dáváte přednost, můžete zadat jiný název.
7. Klikněte na **OK**.
8. V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Nyní jenom uživatelé v klientovi služby Azure AD můžete volat aplikaci API.

### <a name="optional-test-the-api-app"></a>Volitelné: Testovací aplikace API
1. V prohlížeči přejděte na adresu URL aplikace API: v **aplikace API** okno na portálu Azure klikněte na odkaz v části **URL**.  
   
    Budete přesměrováni na přihlašovací obrazovku, protože neověřené požadavky nejsou povoleny dosáhne aplikace API.
   
    Pokud prohlížeč přejde na stránku "byla úspěšně vytvořena.", prohlížeče mohou již být zaznamenány na – v takovém případě otevřete okno InPrivate nebo Incognito, přejděte na adresu URL aplikace API.
2. Přihlaste se pomocí přihlašovacích údajů pro uživatele v klientovi služby Azure AD.
   
    Pokud jste přihlášeni, "úspěšně vytvořil" stránka se zobrazí v prohlížeči.
3. Zavřete prohlížeč.

### <a name="configure-the-azure-ad-application"></a>Konfigurace aplikace Azure AD
Když jste nakonfigurovali ověřování Azure AD, služby App Service vytvoří aplikaci Azure AD. Ve výchozím nastavení nové služby Azure AD aplikace byla nakonfigurována k poskytování token nosiče adresu URL aplikace API. V této části můžete nakonfigurovat aplikace Azure AD zajistit token nosiče, který má AngularJS front-endu místo přímo do aplikace API střední vrstvy. Front-endu AngularJS odešle token do aplikace API, když volá aplikaci API.

> [!NOTE]
> Aplikace pro front-endu a středu zachovat proces jako jednoduchý nejblíže, tento kurz používá jednu Azure AD vrstvy aplikace API. Další možností je použití dvou aplikací Azure AD. V takovém případě je třeba udělit oprávnění aplikace Azure AD části front end volat aplikaci Azure AD střední vrstvy. Tento přístup více aplikacemi, získáte podrobnější kontrolu nad oprávnění pro každou úroveň.
> 
> 

1. V [portál Azure classic](https://manage.windowsazure.com/), přejděte na **Azure Active Directory**.
   
   Budete muset použít klasický portál, protože některá nastavení Azure Active Directory, kteří potřebují přístup k ještě nejsou k dispozici v aktuální portál Azure.
2. Na **Directory** , klikněte na vašem tenantovi AAD.
   
   ![Azure AD na portálu classic](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Klikněte na tlačítko **aplikace > Moje společnost vlastní aplikace**a klikněte na symbol zaškrtnutí.
   
   Budete také muset aktualizací stránky zobrazíte nové aplikace.
4. V seznamu aplikací klikněte na název ten, který vytvořil Azure za vás, pokud jste povolili ověřování pro vaše aplikace API.
   
   ![Karta Azure AD aplikace](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Klikněte na **Konfigurovat**.
   
   ![Azure AD konfigurovat karta](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Nastavit **přihlašovací adresa URL** na adresu URL pro webové aplikace AngularJS, žádné koncové lomítko.
   
   Příklad: https://todolistangular.azurewebsites.net
   
   ![Adresa URL přihlašování](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Nastavit **adresa URL odpovědi** na adresu URL pro webovou aplikaci, žádné koncové lomítko.
   
   Příklad: https://todolistsangular.azurewebsites.net
8. Klikněte na **Uložit**.
   
   ![Adresa URL odpovědi](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. V dolní části stránky klikněte na tlačítko **spravovat manifest > stáhnout manifest**.
   
   ![Stáhněte manifest](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Stáhněte soubor do umístění, kde můžete upravit.
11. Vyhledejte v souboru manifestu stažený `oauth2AllowImplicitFlow` vlastnost. Změna hodnoty této vlastnosti z `false` k `true`a pak soubor uložte.
    
    Toto nastavení je povinné pro přístup z jednostránkové aplikace JavaScript. Umožňuje tokenu nosiče Oauth 2.0, který se má vrátit v fragment adresy URL.
12. Klikněte na tlačítko **spravovat manifest > nahrávání manifest**a odešlete soubor, který jste aktualizovali v předchozím kroku.
    
    ![Nahrát manifestu](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Kopírování **ID klienta** hodnotu a uložte ho někde můžete získat z později.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfigurace projektu ToDoListAngular používat ověřování
V této části můžete změnit AngularJS front-endu, tak, aby používala Active Directory Authentication Library (ADAL) pro JS získat token nosiče pro přihlášeného uživatele z Azure AD. Kód bude obsahovat token v požadavky HTTP odeslané na střední vrstvy, jak je znázorněno v následujícím diagramu. 

![Diagram ověřování uživatele](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Proveďte následující změny do souborů projektu ToDoListAngular.

1. Otevřete *index.cshtml* souboru.
2. Zrušením komentáře u řádků, které odkazují Active Directory Authentication Library (ADAL) pro skripty JS.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Otevřete *app/scripts/app.js* souboru.
4. Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".
   
    Tato změna odkazuje zprostředkovatele ověřování ADAL JS a poskytuje hodnoty konfigurace do ní. V následujícím postupu nastavíte hodnoty konfigurace pro vaše aplikace API a aplikaci Azure AD.
5. V kódu, který nastaví `endpoints` proměnnou, nastavte adresu URL rozhraní API na adresu URL aplikace API, že jste vytvořili pro projekt ToDoListAPI a nastavit ID aplikace Azure AD na ID klienta, který jste zkopírovali z portálu Azure classic.
   
    Podobně jako v následujícím příkladu je kód.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. Ve volání `adalProvider.init`, nastavte `tenant` na název vašeho klienta a `clientId` stejnou hodnotu, kterou jste použili v předchozím kroku.
   
    Podobně jako v následujícím příkladu je kód.
   
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );
   
    Tyto změny `app.js` zadejte, zda kód volání a názvem rozhraní API jsou ve stejné aplikaci, Azure AD.
7. Otevřete *app/scripts/homeCtrl.js* souboru.
8. Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".
9. Otevřete *app/scripts/indexCtrl.js* souboru.
10. Komentář blok kódu označen pro "ověřování bez" a zrušte komentář u blok kódu označen pro "ověřování s".

### <a name="deploy-the-todolistangular-project-to-azure"></a>Nasazení projektu ToDoListAngular do Azure
1. V **Průzkumníku řešení** klikněte pravým tlačítkem na projekt ToDoListAngular a potom klikněte na **Publikovat**.
2. Klikněte na **Publikovat**.
   
    Visual Studio nasadí projekt a otevře prohlížeč na základní adresu URL webové aplikace. Zobrazí 403 chybovou stránku, což je normální pro pokus o přechod z prohlížeče do webového rozhraní API základní adresu URL.
   
    Máte k provedení změny do aplikace API střední vrstvy, musíte před testováním aplikace.
3. Zavřete prohlížeč.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfigurace projektu ToDoListAPI používat ověřování
Aktuálně projekt ToDoListAPI odešle "*" jako `owner` hodnotu ToDoListDataAPI. V této části můžete změnit kód odeslat ID přihlášeného uživatele.

Proveďte následující změny v projektu ToDoListAPI.

1. Otevřete *Controllers/ToDoListController.cs* souboru a zrušte komentář u řádku v každé metody akce, která nastaví `owner` do služby Azure AD `NameIdentifier` hodnoty deklarace identity. Například:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Důležité**: nemáte zrušte komentář u kód `ToDoListDataAPI` metoda; můžete to udělat později kurzu ověření objektu služby.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Nasazení projektu ToDoListAPI do Azure
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAPI a potom klikněte na **publikovat**.
2. Klikněte na **Publikovat**.
   
    Visual Studio nasadí projekt a otevře prohlížeč na základní adresu URL aplikace API.
3. Zavřete prohlížeč.

### <a name="test-the-application"></a>Testování aplikace
1. Přejděte na adresu URL webové aplikace, **pomocí protokolu HTTPS, není HTTP**.
2. Klikněte **seznam úkolů** kartě.
   
    Zobrazí se výzva k přihlášení.
3. Přihlaste se pomocí přihlašovacích údajů uživatele ve vašem tenantovi AAD.
4. **Seznam úkolů** se zobrazí stránka.
   
   ![Seznam úkolů stránky](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Žádné položky seznamu úkolů se nezobrazují, protože dosud všechny nejsou pro vlastníka "*". Nyní bude prostřední vrstva požaduje položek pro přihlášeného uživatele a žádné ještě nejsou vytvořené.
5. Přidání nových položek úkolů, chcete-li ověřit, zda aplikace pracuje.
6. V jiném okně prohlížeče, přejděte na adresu URL uživatelského rozhraní Swagger pro aplikaci ToDoListDataAPI API a klikněte na tlačítko **ToDoList > získat**. Zadejte hvězdičku pro `owner` parametr a pak klikněte na tlačítko **vyzkoušení**.
   
   Odpověď ukazuje, že mají nové položky seznamu úkolů skutečnou ID uživatele Azure AD ve vlastnosti vlastníka.
   
   ![ID vlastníka v odpovědi JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-the-projects-from-scratch"></a>Sestavení projektů od začátku
Dva projekty webového rozhraní API, které byly vytvořeny pomocí **aplikace API Azure** šablony a nahrazení řadiče hodnoty výchozí řadič seznamu úkolů projektu. 

Informace o tom, jak vytvořit jednostránková aplikace AngularJS s back-end webovém rozhraní API 2 najdete v tématu [rukou na testovacím: sestavení jedné stránce aplikace (SPA) pomocí rozhraní ASP.NET Web API a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informace o tom, jak přidat kód pro ověřování Azure AD najdete v následujících zdrojích informací:

* [Zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Představení ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nepleťte ToDoListAPI (střední úroveň) a ToDoListDataAPI (datové vrstvy). Ověřte například, že jste přidali ověřování do aplikace API střední vrstvy, ne na datovou vrstvu. 
* Ujistěte se, že zdrojový kód AngularJS odkazuje na adresu URL aplikace API střední vrstvy (ToDoListAPI, ToDoListDataAPI není) a správné Azure AD ID klienta. 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak používat ověřování služby App Service pro aplikaci API a jak volat aplikaci API pomocí knihovny ADAL JS. V dalším kurzu se dozvíte jak [zabezpečit přístup do vaší aplikace API pro scénáře service-to-service](app-service-api-dotnet-service-principal-auth.md).

