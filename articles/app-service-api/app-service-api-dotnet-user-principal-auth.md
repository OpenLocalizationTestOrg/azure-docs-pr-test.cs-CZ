---
title: "aaaUser ověřování pro aplikace API v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak tooprotect aplikace API v Azure App Service tím, že přístup pouze tooauthenticated uživatele."
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
ms.openlocfilehash: 4702fc77fcfe736405e22b033c35d22ae3c5a300
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Ověřování uživatelů pro aplikace API v Azure App Service
## <a name="overview"></a>Přehled
Tento článek ukazuje, jak tooprotect aplikace Azure API, která pouze ověření uživatelé ji volat. Hello článek předpokládá, že jste četli hello [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md).

Naučíte se:

* Jak tooconfigure zprostředkovatele ověřování, s podrobnostmi o službě Azure Active Directory (Azure AD).
* Jak tooconsume chráněné aplikace API pomocí hello [Active Directory Authentication Library (ADAL) pro JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Hello článek obsahuje dvě části:

* Hello [jak tooconfigure ověření uživatele v Azure App Service](#authconfig) část vysvětluje v obecné postupy tooconfigure ověřování uživatele pro všechny aplikace API a rozhraní tooall podporovaných službou App Service, včetně .NET, se vztahuje stejnou měrou Node.js a Java.
* Počínaje hello [pokračováním hello .NET API Apps kurzy](#tutorialstart) část, hello příručky článek vás provede konfiguraci ukázkové aplikace s .NET back end a AngularJS front-end tak, aby používala Azure Active Directory pro uživatele ověřování. 

## <a id="authconfig"></a>Jak tooconfigure ověření uživatele v Azure App Service
Tato část obsahuje obecné pokyny, které se vztahují tooany aplikace API. Pro konkrétní toohello tooDo kroky seznamu .NET ukázkovou aplikaci, přejděte příliš[pokračování úvodních kurzů .NET hello](#tutorialstart).

1. V hello [portál Azure](https://portal.azure.com/), přejděte toohello **nastavení** okno aplikace hello rozhraní API, které chcete tooprotect, najít hello **funkce** části a pak klikněte na tlačítko  **Ověřování / autorizace**.
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V hello **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. Vyberte jednu z možností hello hello **tootake akce, když požadavek nebude ověřený** rozevíracího seznamu.
   
   * Pokud chcete pouze ověřených volání tooreach aplikace API, vyberte jednu z hello **protokolu s...**  možnosti. Tato možnost umožňuje tooprotect hello rozhraní API aplikace bez nutnosti napsat kód, který běží v ní.
   * Pokud chcete všechny tooreach volá aplikaci API, zvolte **povolit požadavku (žádná akce)**. Tato možnost toodirect neověřené volající tooa volba zprostředkovatelů ověřování můžete použít. Pomocí této možnosti máte toowrite kód toohandle autorizace.
     
     Další informace najdete v tématu [Ověřování a autorizace API Apps v Azure App Service](app-service-api-authentication.md#multiple-protection-options).
4. Vyberte jednu nebo více hello **zprostředkovatele ověřování**.
   
    Hello obrázek ukazuje možnosti, které vyžadují všechny volající toobe ověření Azure AD.
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
   
    Když zvolíte zprostředkovatele ověřování, hello portál zobrazí okno konfigurace pro tohoto zprostředkovatele. 
   
    Podrobné pokyny, které vysvětlují, jak tooconfigure hello oken konfigurace zprostředkovatele ověřování najdete v tématu [jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (hello odkaz přejde tooan článek o službě Azure AD, ale samotný článek hello obsahuje karty, které odkazují tooarticles pro hello dalších zprostředkovatelů ověření.)
5. Když jste hotovi s hello ověřování zprostředkovatele konfigurace okna, klikněte na tlačítko **OK**.
6. V hello **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.

Když to uděláte, ověřuje služby App Service před nedostanou aplikace API hello všechna volání rozhraní API. Hello ověřování služby pracovní hello stejné pro všechny jazyky, které podporuje služby App service, včetně .NET, Node.js a Java. 

toomake ověřená volání rozhraní API, volající hello obsahuje token nosiče OAuth 2.0 zprostředkovatele ověřování hello v hello autorizační hlavičky požadavků HTTP. Hello token můžete získat pomocí sady SDK zprostředkovatele ověřování hello.

## <a id="tutorialstart"></a>Pokračováním kurzy hello .NET API Apps
Pokud postupujete podle hello Node.js nebo Java kurzy pro aplikace API, přeskočte toohello další článek, [objekt zabezpečení ověřování služby pro aplikace API](app-service-api-dotnet-service-principal-auth.md). 

Pokud postupujete podle hello řady kurz .NET pro aplikace API a už jste nasadili ukázkovou aplikaci hello podle pokynů v hello [první](app-service-api-dotnet-get-started.md) a [druhý](app-service-api-cors-consume-javascript.md) kurzy, přeskočte toohello [nastavení ověřování v App Service a Azure AD](#azureauth) části.

Pokud chcete toofollow v tomto kurzu bez průchodu přes první a druhý kurzy hello hello následující kroky, které popisují, jak tooget spuštěné pomocí automatizovaného procesu toodeploy hello ukázkové aplikace.

> [!NOTE]
> Hello následující kroky vám pomůžou toohello stejné výchozím bodem, jako kdyby hello první dva kurzy s jednou výjimkou: Visual Studio nebude již víte, které webové aplikace nebo aplikace API, který získá každý projekt nasazen. To znamená, že nebudete mít přesné pokyny v tomto kurzu, které vysvětlují, jak toodeploy toohello správné cíle. Pokud si nejste tedy se nad tím, jak kroky nasazení hello toodo sami, je lepší toofollow hello kurz řady z hello [první kurz](app-service-api-dotnet-get-started.md) než toostart s tímto automatizované procesu nasazení.
> 
> 

1. Ujistěte se, že máte všechny požadavky hello uvedené v hello [první kurz](app-service-api-dotnet-get-started.md). V přidání toohello předpoklady uvedené tato kurzech ověřování Předpokládejme, že jste pracovali s webové aplikace aplikační služby a aplikace API v sadě Visual Studio a hello portálu Azure.
2. Klikněte na tlačítko hello **nasazení tooAzure** tlačítka na hello [soubor readme úložiště ukázkové seznamu tooDo](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) toodeploy aplikace hello rozhraní API a hello webové aplikace. Poznamenejte si skupinu prostředků Azure hello, která je vytvořena, ujistěte se, jak můžete použít tento novější toolook webové aplikace a názvy aplikací API.
3. Stažení nebo klonování hello [tooDo seznamu Ukázka úložiště](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tooget hello kód, který budete pracujete s místně v sadě Visual Studio.

## <a id="azureauth"></a>Nastavení ověřování v App Service a Azure AD
Nyní máte hello aplikace běžící v Azure App Service bez nutnosti ověřit uživatele. V této části přidáte ověřování pomocí tohoto postupu hello následující úlohy:

* Nakonfigurujte ověřování Azure Active Directory (Azure AD) toorequire služby App Service pro volání hello aplikaci API střední vrstvy.
* Vytvořte aplikaci Azure AD.
* Po přihlášení toohello AngularJS front-endu nakonfigurujte aplikace Azure AD hello, toosend tokenu nosiče hello. 

Pokud narazíte na problémy při následující hello kurz pokynů, najdete v části hello [Poradce při potížích s](#troubleshooting) části na konci hello tohoto kurzu hello. 

### <a name="configure-authentication-for-hello-middle-tier-api-app"></a>Konfigurovat ověřování pro aplikaci API střední vrstvy hello
1. V hello [portál Azure](https://portal.azure.com/), přejděte toohello **nastavení** okno aplikace hello rozhraní API, kterou jste vytvořili pro projekt ToDoListAPI hello, najde hello **funkce** části a potom Klikněte na tlačítko **ověřování / autorizace**.
   
    ![Portál Azure ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V hello **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. V hello **tootake akce, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory**.
   
    Tato možnost zajišťuje, že žádné žádosti o unauathenticated bude dosáhnou hello aplikace API. 
4. V části **zprostředkovatele ověřování**, klikněte na tlačítko **Azure Active Directory**.
   
    ![Okno Azure portálu ověřování/autorizace](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. V hello **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**
   
    ![Možnost Azure Express ověřování/autorizace služby portálu](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)
   
    S hello **Express** možnosti služby App Service můžete automaticky vytvořit aplikaci Azure AD ve službě Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nemáte toocreate klienta, protože každý účet Azure automaticky jeden má.
6. V části **režim správy**, klikněte na tlačítko **vytvořit novou aplikaci AD** Pokud již není vybrána a Všimněte si hello hodnotu, která je v hello **vytvořit aplikaci** textového pole; budete vyhledání této AAD aplikace v hello portál Azure classic později.
   
    ![Nastavení Azure portálu Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)
   
    Azure automaticky vytvoří aplikaci Azure AD s názvem uvedené hello v klientovi služby Azure AD. Ve výchozím nastavení, je hello aplikaci Azure AD s názvem hello stejné jako aplikace hello rozhraní API. Pokud dáváte přednost, můžete zadat jiný název.
7. Klikněte na **OK**.
8. V hello **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Nyní jenom uživatelé v klientovi služby Azure AD můžete volat aplikaci API hello.

### <a name="optional-test-hello-api-app"></a>Volitelné: Testování aplikace hello rozhraní API
1. Přejděte v prohlížeči adresu URL toohello aplikace hello rozhraní API: v hello **aplikace API** v hello portál Azure, klikněte na odkaz hello pod **URL**.  
   
    Protože neověřené požadavky nejsou povoleny aplikace API hello tooreach jste přesměrovaného tooa přihlašovací obrazovku.
   
    Pokud prohlížeč přejde toohello "úspěšně vytvořen" stránky, hello prohlížeče mohou již být zaznamenány na – v takovém případě otevřete okno InPrivate nebo Incognito, přejděte na adresu URL aplikace toohello rozhraní API.
2. Přihlaste se pomocí přihlašovacích údajů pro uživatele v klientovi služby Azure AD.
   
    Pokud jste přihlášeni, zobrazí se stránka "úspěšně vytvořen" hello v prohlížeči hello.
3. Prohlížeč zavřít hello.

### <a name="configure-hello-azure-ad-application"></a>Konfigurace aplikace hello Azure AD
Když jste nakonfigurovali ověřování Azure AD, služby App Service vytvoří aplikaci Azure AD. Ve výchozím nastavení aplikace hello nové Azure AD se adresy URL nakonfigurované tooprovide hello nosiče tokenu toohello aplikace API. V této části nakonfigurujete hello Azure AD application tooprovide hello nosiče tokenu toohello AngularJS front-endu místo přímo toohello aplikaci API střední vrstvy. front-endu Hello AngularJS odešle hello tokenu toohello rozhraní API aplikace, když volá aplikaci API hello.

> [!NOTE]
> proces tookeep hello co nejjednodušší, tento kurz používá jednu Azure AD pro hello front-endu a aplikaci API střední vrstvy hello. Další možností je toouse dvě aplikace Azure AD. V takovém případě by mít toogive hello front-endu na Azure AD application oprávnění toocall hello střední vrstvy na aplikaci Azure AD. Tento přístup více aplikacemi, získáte podrobnější kontrolu nad tooeach úroveň oprávnění.
> 
> 

1. V hello [portál Azure classic](https://manage.windowsazure.com/), přejděte příliš**Azure Active Directory**.
   
   Portál classic hello toouse máte, protože některá nastavení Azure Active Directory, které budete potřebovat přístup k tooare ještě není k dispozici v aktuální portál Azure hello.
2. Na hello **Directory** , klikněte na vašem tenantovi AAD.
   
   ![Azure AD na portálu classic](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)
3. Klikněte na tlačítko **aplikace > Moje společnost vlastní aplikace**a pak klikněte na tlačítko zaškrtnutí hello.
   
   Také může mít toorefresh hello stránky toosee hello novou aplikaci.
4. Hello seznamu aplikací klikněte na název hello hello ten, který Azure vytvořili pro vás, pokud jste povolili ověřování pro vaše aplikace API.
   
   ![Karta Azure AD aplikace](./media/app-service-api-dotnet-user-principal-auth/appstab.png)
5. Klikněte na **Konfigurovat**.
   
   ![Azure AD konfigurovat karta](./media/app-service-api-dotnet-user-principal-auth/configure.png)
6. Nastavit **přihlašovací adresa URL** toohello adresa URL pro webové aplikace AngularJS, žádné koncové lomítko.
   
   Příklad: https://todolistangular.azurewebsites.net
   
   ![Adresa URL přihlašování](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)
7. Nastavit **adresa URL odpovědi** toohello adresu URL pro webovou aplikaci, žádné koncové lomítko.
   
   Příklad: https://todolistsangular.azurewebsites.net
8. Klikněte na **Uložit**.
   
   ![Adresa URL odpovědi](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)
9. V dolní části hello hello stránky, klikněte na tlačítko **spravovat manifest > stáhnout manifest**.
   
   ![Stáhněte manifest](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)
10. Stáhněte si hello souboru tooa umístění, kde můžete upravit.
11. V souboru manifestu hello stáhli, vyhledejte hello `oauth2AllowImplicitFlow` vlastnost. Změňte hello hodnotu této vlastnosti z `false` příliš`true`a pak hello soubor uložte.
    
    Toto nastavení je povinné pro přístup z jednostránkové aplikace JavaScript. Umožňuje hello Oauth 2.0 nosiče tokenu toobe, vrátí se v hello fragment adresy URL.
12. Klikněte na tlačítko **spravovat manifest > nahrávání manifest**a nahrávání hello soubor, který jste aktualizovali v předchozím kroku hello.
    
    ![Nahrát manifestu](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)
13. Kopírování hello **ID klienta** hodnotu a uložte ho někde můžete získat z později.

## <a name="configure-hello-todolistangular-project-toouse-authentication"></a>Konfigurace ověřování toouse projektu ToDoListAngular hello
V této části můžete změnit hello AngularJS front-endu, tak, aby používala Active Directory Authentication Library (ADAL) pro JS tooacquire nosný token pro hello přihlášeného uživatele z Azure AD. Hello kód bude obsahovat hello token v požadavky HTTP odeslané toohello střední vrstvy, jak je znázorněno v následujícím diagramu hello. 

![Diagram ověřování uživatele](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Proveďte následující změny toofiles v projektu ToDoListAngular hello hello.

1. Otevřete hello *index.cshtml* souboru.
2. Zrušením komentáře u hello řádku, které odkazují na hello Active Directory Authentication Library (ADAL) pro skripty JS.
   
        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>
3. Otevřete hello *app/scripts/app.js* souboru.
4. Komentář hello blok kódu označen pro "bez ověřování" a zrušte komentář u hello blok kódu označen pro "ověřování s".
   
    Tato změna odkazuje zprostředkovatele ověřování ADAL JS hello a poskytuje tooit hodnoty konfigurace. V následující kroky hello nastavíte hello konfigurační hodnoty pro vaše aplikace API a aplikaci Azure AD.
5. V hello kód, který nastaví hello `endpoints` hello proměnné, nastavte adresu URL rozhraní API toohello adresu URL aplikace hello rozhraní API, který jste vytvořili pro projekt ToDoListAPI hello a nastavit hello Azure AD ID toohello ID klienta aplikace, který jste zkopírovali ze hello portál Azure classic.
   
    Podobně jako toohello následující ukázka je kód Hello.
   
        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };
6. V hello volat příliš`adalProvider.init`, nastavte `tenant` tooyour název klienta a `clientId` toosame hodnotu, kterou jste použili v předchozím kroku hello.
   
    Podobně jako toohello následující ukázka je kód Hello.
   
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
   
    Tyto změny příliš`app.js` určit, že hello volací kód a hello volat rozhraní API jsou v aplikaci hello stejnou službou Azure AD.
7. Otevřete hello *app/scripts/homeCtrl.js* souboru.
8. Komentář hello blok kódu označen pro "bez ověřování" a zrušte komentář u hello blok kódu označen pro "ověřování s".
9. Otevřete hello *app/scripts/indexCtrl.js* souboru.
10. Komentář hello blok kódu označen pro "bez ověřování" a zrušte komentář u hello blok kódu označen pro "ověřování s".

### <a name="deploy-hello-todolistangular-project-tooazure"></a>Nasazení tooAzure projektu ToDoListAngular hello
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAngular hello a pak klikněte na tlačítko **publikovat**.
2. Klikněte na **Publikovat**.
   
    Visual Studio nasadí projekt hello a otevře základní adresu URL prohlížeče toohello webové aplikace. Zobrazí 403 chybovou stránku, což je normální pokus o toogo tooa webového rozhraní API základní adresy URL v prohlížeči.
   
    Musíte ještě toomake změnu toohello aplikaci API střední vrstvy musíte před testováním aplikace hello.
3. Prohlížeč zavřít hello.

## <a name="configure-hello-todolistapi-project-toouse-authentication"></a>Konfigurace ověřování toouse projekt ToDoListAPI hello
Aktuálně hello zasílá projekt ToDoListAPI "*" jako hello `owner` tooToDoListDataAPI hodnotu. V této části můžete změnit hello kód toosend hello ID hello přihlášeného uživatele.

Proveďte následující změny v projektu ToDoListAPI hello hello.

1. Otevřete hello *Controllers/ToDoListController.cs* souboru a zrušte komentář u hello řádku v každé metody akce, která nastaví `owner` toohello Azure AD `NameIdentifier` hodnoty deklarace identity. Například:
   
        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;
   
    **Důležité**: nemáte zrušte komentář u kódu v hello `ToDoListDataAPI` metoda; můžete to udělat později hello služby ověření objektu kurzu.

### <a name="deploy-hello-todolistapi-project-tooazure"></a>Nasazení tooAzure projekt ToDoListAPI hello
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ToDoListAPI hello a pak klikněte na tlačítko **publikovat**.
2. Klikněte na **Publikovat**.
   
    Visual Studio nasadí projekt hello a otevře základní adresu URL aplikace API pro prohlížeče toohello.
3. Prohlížeč zavřít hello.

### <a name="test-hello-application"></a>Testování aplikace hello
1. Přejděte na adresu URL toohello hello webové aplikace, **pomocí protokolu HTTPS, není HTTP**.
2. Klikněte na tlačítko hello **tooDo seznamu** kartě.
   
    Jste výzvami toolog v.
3. Přihlaste se pomocí přihlašovacích údajů uživatele ve vašem tenantovi AAD hello.
4. Hello **tooDo seznamu** se zobrazí stránka.
   
   ![Stránka seznamu tooDo](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)
   
   Žádné položky seznamu úkolů se nezobrazují, protože dosud všechny nejsou pro vlastníka "*". Nyní střední vrstvy hello požaduje položek pro hello přihlášeného uživatele a žádné ještě nejsou vytvořené.
5. Přidáte nové položky tooverify úkolů, které aplikace hello funguje.
6. V jiném okně prohlížeče přejděte toohello adresa URL Swaggeru uživatelského rozhraní pro aplikaci ToDoListDataAPI API hello a klikněte na tlačítko **ToDoList > získat**. Zadejte hvězdičku hello `owner` parametr a pak klikněte na tlačítko **vyzkoušení**.
   
   odpověď Hello ukazuje, že nové položky seznamu úkolů hello mají v hello vlastníka vlastnost ID uživatele hello skutečné Azure AD.
   
   ![ID vlastníka v odpovědi JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)

## <a name="building-hello-projects-from-scratch"></a>Sestavení projektů hello od začátku
Hello dva projekty webového rozhraní API, které byly vytvořeny pomocí hello **aplikace API Azure** šablony a nahraďte hello výchozí hodnoty řadiče s řadičem seznamu úkolů projektu. 

Informace o tom, příliš vytvořit jednostránková aplikace AngularJS s webovém rozhraní API 2 back-end, naleznete v tématu [rukou na testovacím: sestavení jedné stránce aplikace (SPA) pomocí rozhraní ASP.NET Web API a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informace o tooadd ověřovacího kódu Azure AD, najdete v části hello následující prostředky:

* [Zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Představení ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nepleťte ToDoListAPI (střední úroveň) a ToDoListDataAPI (datové vrstvy). Ověřte například, že jste přidali aplikaci API střední vrstvy toohello ověřování, není hello datové vrstvy. 
* Ujistěte se, že adresa URL aplikace hello AngularJS zdrojového kódu odkazy hello rozhraní API střední vrstvy (ToDoListAPI, ToDoListDataAPI není) a hello Opravte ID klienta Azure AD 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak toouse ověřování služby App Service pro aplikaci API a jak toocall hello aplikace API pomocí hello knihovnu ADAL JS. V dalším kurzu hello dozvíte jak příliš[zabezpečený přístup aplikace tooyour rozhraní API pro scénáře service-to-service](app-service-api-dotnet-service-principal-auth.md).

