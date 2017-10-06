---
title: "ověření objektu aaaService pro API Apps v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak aplikace tooprotect rozhraní API v Azure App Service pro scénáře service-to-service."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 7ca0bab2-1d29-4d51-b779-dce0edd34f8b
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 06/30/2016
ms.author: alkarche
ms.openlocfilehash: 94d9ee11f38293df4a2fd815ef02c59cc6defed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Objekt zabezpečení ověřování služby pro aplikace API v Azure App Service
## <a name="overview"></a>Přehled
Tento článek vysvětluje, jak toouse ověřování služby App Service pro *interní* přístup k aplikacím tooAPI. Vnitřní scénář je, kdy máte aplikaci API, že chcete toobe použití pouze podle kódu aplikace. Hello doporučená tooimplement způsob, jak tento scénář ve službě App Service je toouse Azure AD tooprotect hello volá aplikaci API. Volání rozhraní API aplikace hello chráněné pomocí token nosiče, který můžete získat z Azure AD tím, že poskytuje identitu aplikace (instanční objekt) přihlašovací údaje. Alternativy toousing Azure AD, najdete v části hello **Service-to-service ověřování** části hello [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

V tomto článku se dozvíte:

* Jak aplikace tooprotect rozhraní API služby Azure Active Directory (Azure AD) toouse z neověřené přístup.
* Jak aplikace tooconsume chráněné rozhraní API z aplikace API, webovou aplikaci nebo mobilní aplikace pomocí pověření hlavní (identita aplikace) služby Azure AD. Informace o tom najdete v části tooconsume z aplikace logiky, [používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md).
* Jak se, že tento hello chráněné aplikace API toomake nelze volat z prohlížeče pomocí přihlášených uživatelů.
* Jak pouze toomake jisti, že hello chráněn aplikace API je možné volat v konkrétní objekt služby Azure AD.

Hello článek obsahuje dvě části:

* Hello [jak tooconfigure služby ověření objektu ve službě Azure App Service](#authconfig) část vysvětluje v obecné postupy tooconfigure ověřování pro všechny aplikace API a jak tooconsume hello chráněné aplikace API. Tato část se vztahuje stejnou měrou tooall rozhraní podporované službou App Service, včetně .NET, Node.js a Java.
* Počínaje hello [pokračování úvodních kurzů .NET hello](#tutorialstart) části hello kurz vás provede konfiguraci "interní přístup" scénáře pro ukázkové aplikace .NET spuštěna ve službě App Service. 

## <a id="authconfig"></a>Jak tooconfigure služby ověření objektu ve službě Azure App Service
Tato část obsahuje obecné pokyny, které se vztahují tooany aplikace API. Pro konkrétní toohello tooDo kroky seznamu .NET ukázkovou aplikaci, přejděte příliš[pokračováním řady kurz .NET API Apps hello](#tutorialstart).

1. V hello [portál Azure](https://portal.azure.com/), přejděte toohello **nastavení** okno aplikace hello rozhraní API, které chcete tooprotect a pak vyhledejte hello **funkce** části a klikněte na tlačítko **Ověřování / autorizace**.
   
    ![Ověřování/autorizace služby na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V hello **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. V hello **tootake akce, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory** .
4. V části **zprostředkovatele ověřování**, vyberte **Azure Active Directory**.
   
    ![Ověřování/autorizace okno na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Konfigurace hello **nastavení Azure Active Directory** toocreate okno Nový Azure AD aplikace, nebo použijte existující aplikaci Azure AD Pokud již účet máte, které chcete toouse.
   
    Interní scénáře jsou obvykle aplikace API volání aplikace API. Můžete použít samostatné Azure AD aplikací pro každou aplikaci API nebo pouze jednu aplikaci Azure AD.
   
    Podrobné pokyny v tomto okně najdete v tématu [jak tooconfigure přihlášení vaší služby App Service toouse aplikace Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
6. Když jste hotovi s hello ověřování zprostředkovatele konfigurace okna, klikněte na tlačítko **OK**.
7. V hello **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Když to uděláte, služby App Service umožňuje požadavků pouze z volající v klientovi Azure AD hello nakonfigurované. V aplikaci API hello chráněné je vyžadován žádný kód ověřování nebo autorizace. Hello tokenu nosiče je předán toohello rozhraní API aplikace společně se běžně používané deklarací identity v hlavičkách protokolu HTTP, a můžete si přečíst tyto informace v toovalidate kódu, které požadavky jsou z konkrétní volající, jako je například hlavní název služby.

Tato funkce ověřování funguje hello stejným způsobem jako pro všechny jazyky, podporuje aplikace služby, včetně .NET, Node.js a Java. 

#### <a name="how-tooconsume-hello-protected-api-app"></a>Tom, jak tooconsume hello chráněný aplikace API
Hello volající musí poskytnout volání rozhraní API tokenu nosiče Azure AD. tooget nosný token pomocí přihlašovacích údajů hlavní služby, volající hello používá služby Active Directory Authentication Library (ADAL pro [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs), nebo [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). tooget token hello kód, který volá ADAL poskytuje tooADAL hello následující informace:

* Hello název klienta služby Azure AD.
* Hello ID a klienta tajný klíč klienta (klíč aplikace) přidružené k hello volající aplikace hello Azure AD.
* ID klienta Hello hello aplikace Azure AD, které jsou spojené s hello chráněné aplikace API. (Pokud se používá pouze jednu aplikaci Azure AD, to je hello stejné ID klienta jako text hello, jeden pro volající hello.)

Tyto hodnoty jsou k dispozici hello Azure AD stránkách hello [portál Azure classic](https://manage.windowsazure.com/).

Jakmile získal hello token hello volající ji obsahuje u žádostí protokolu HTTP v hlavičce autorizace hello.  Služba App Service ověří hello token a umožňuje hello požadavky tooreach hello chráněné aplikace API.

#### <a name="how-tooprotect-hello-api-app-from-access-by-users-in-hello-same-tenant"></a>Jak se aplikace hello rozhraní API tooprotect z přístup uživatelům v hello stejné klienta
Nosné tokeny pro uživatele ve stejné klienta jsou považovány za platné pro hello hello chráněné aplikace API.  Pokud chcete tooensure, které lze volat pouze objekt služby hello chráněné aplikace API, přidat kód v hello chráněné rozhraní API aplikace toovalidate hello následující deklarací z tokenu hello:

* `appid`hello ID klienta aplikace Azure AD hello, který je přidružen hello volající by měl být. 
* `oid`(`objectidentifier`) by měla být hello ID objektu zabezpečení služby hello volajícího. 

App Service navíc poskytuje hello `objectidentifier` deklarace identity v záhlaví hello X-MS-CLIENT-hlavní-ID.

### <a name="how-tooprotect-hello-api-app-from-browser-access"></a>Jak tooprotect hello aplikace API z přístup z prohlížeče
Pokud nemáte ověřit deklarace identity v kódu v aplikaci API hello chráněný, a pokud používáte samostatnou aplikaci Azure AD hello chráněné aplikace API, ujistěte se, že hello je adresa URL odpovědi aplikaci Azure AD není hello stejná hodnota jako hello základní adresu URL aplikace API. Pokud adresa URL odpovědi hello odkazuje přímo toohello chráněné aplikace API, uživatel v klientovi hello stejnou službou Azure AD procházet toohello aplikace API, přihlaste se a úspěšně volat rozhraní API hello.

## <a id="tutorialstart"></a>Pokračováním řady kurz .NET API Apps hello
Pokud postupujete podle hello Node.js nebo Java kurz řady pro aplikace API, přeskočte toohello [další kroky](#next-steps) části. 

Hello zbývající část tohoto článku pokračuje kurz řady hello .NET API Apps a předpokládá, že jste dokončili hello [kurzu ověřování uživatele](app-service-api-dotnet-user-principal-auth.md) a mít hello ukázková aplikace běžící v Azure s ověřováním uživatele Povolit.

## <a name="set-up-authentication-in-azure"></a>Nastavení ověřování v Azure
V této části můžete nakonfigurovat App Service, že hello jen protokol HTTP požadavků, umožňuje aplikaci API datové vrstvy v tooreach hello jsou hello ty, které se platný Azure AD nosné tokeny. 

V následující části hello nakonfigurujte toosend aplikaci API střední vrstvy hello aplikace tooAzure pověření AD, získáte zpět token nosiče a odeslat aplikaci API datové vrstvy toohello tokenu nosiče hello. Tento proces je znázorněn v diagramu hello.

![Diagram ověřování služby](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Pokud narazíte na problémy při následující hello kurz pokynů, najdete v části hello [Poradce při potížích s](#troubleshooting) části na konci hello tohoto kurzu hello. 

1. V hello [portál Azure](https://portal.azure.com/), přejděte toohello **nastavení** okno aplikace hello rozhraní API, který jste vytvořili pro aplikaci API (datové vrstvy) hello ToDoListDataAPI a potom klikněte na **nastavení**.
2. V hello **nastavení** okno, najít hello **funkce** části a potom klikněte na **ověřování / autorizace**.
   
    ![Ověřování/autorizace služby na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)
3. V hello **ověřování / autorizace** okně klikněte na tlačítko **na**.
4. V hello **tootake akce, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory**.
   
    Toto je hello nastavení, které způsobí, že služby App Service tooensure, pouze k ověření aplikace API hello reach požadavky. Pro požadavky, které mají platný nosné tokeny, služby App Service předá hello tokeny podél toohello rozhraní API app a naplní hlavičky protokolu HTTP s běžně používanými deklarací toomake tyto informace snadno dostupné tooyour kódu.
5. V části **zprostředkovatele ověřování**, klikněte na tlačítko **Azure Active Directory**.
   
    ![Ověřování/autorizace okno na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
6. V hello **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**.
   
    S hello **Express** možnost Azure můžete automaticky vytvořit aplikaci AAD ve službě Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nemáte toocreate klienta, protože každý účet Azure automaticky jeden má.
7. V části **režim správy**, klikněte na tlačítko **vytvořit novou aplikaci AD** Pokud již není vybrána.
   
    Hello portál připojuje hello **vytvořit aplikaci** vstupní pole výchozí hodnotu. Ve výchozím nastavení, je hello aplikaci Azure AD s názvem hello stejné jako aplikace hello rozhraní API. Pokud dáváte přednost, můžete zadat jiný název.
   
    ![Nastavení služby Azure AD](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)
   
    **Poznámka:**: jako alternativu, můžete použít jednu Azure AD aplikace pro obě hello volání aplikace API a hello chráněné aplikace API. Pokud jste vybrali tento alternativa, nebude potřeba hello **vytvořit novou aplikaci AD** možnost zde, protože jste již vytvořili aplikaci Azure AD v kurzu ověřování uživatele hello. V tomto kurzu budete používat aplikace Azure AD oddělte hello volání aplikace API a hello chráněné aplikace API.
8. Poznamenejte si hodnotu hello v hello **vytvořit aplikaci** vstupní pole; budete vyhledávání tuto aplikaci AAD v hello portál Azure classic později.
9. Klikněte na **OK**.
10. V hello **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
    
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)
    
    Služby App Service vytvoří aplikaci služby Azure Active Directory s **přihlašovací adresa URL** a **adresa URL odpovědi** automaticky nastaví toohello adresu URL aplikace API. Hodnota pozdější Hello umožňuje uživatelům ve vaší toolog klienta AAD v a aplikace API hello přístup.

### <a name="verify-that-hello-api-app-is-protected"></a>Ověřte, zda že je chráněný aplikaci hello rozhraní API
1. Přejděte v prohlížeči adresu URL toohello aplikace hello rozhraní API: v hello **aplikace API** v hello portál Azure, klikněte na odkaz hello pod **URL**. 
   
    Protože neověřené požadavky nejsou povoleny aplikace API hello tooreach jste přesměrovaného tooa přihlašovací obrazovku. 
   
    Pokud váš prohlížeč přejděte toohello uživatelské rozhraní Swagger, váš prohlížeč pravděpodobně již přihlášeni – v takovém případě otevřete okno InPrivate nebo Incognito a přejděte toohello adresa URL Swaggeru uživatelského rozhraní.
2. Přihlaste se pomocí přihlašovacích údajů uživatele ve vašem tenantovi AAD.
   
   Pokud jste přihlášeni, zobrazí se stránka "úspěšně vytvořen" hello v prohlížeči hello.

## <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Nakonfigurujte tooacquire projekt ToDoListAPI hello a odeslání tokenu Azure AD hello
V této části hello následující úlohy:

* Přidejte kód v aplikaci API střední vrstvy hello, která používá Azure AD application pověření tooacquire token a odeslat ho s protokolem HTTP požadavky aplikaci API datové vrstvy toohello.
* Hello přihlašovací údaje, které potřebujete získáte z Azure AD.
* Zadejte přihlašovací údaje hello do nastavení prostředí runtime Azure App Service v aplikaci API střední vrstvy hello. 

### <a name="configure-hello-todolistapi-project-tooacquire-and-send-hello-azure-ad-token"></a>Nakonfigurujte tooacquire projekt ToDoListAPI hello a odeslání tokenu Azure AD hello
Proveďte následující změny v projektu ToDoListAPI hello v sadě Visual Studio hello.

1. Zrušením komentáře u všech hello kódu hello *ServicePrincipal.cs* souboru.
   
    Toto je hello kód, který využívá ADAL pro .NET tooacquire hello Azure AD nosný token.  Používá několik hodnoty konfigurace, které budete nastaveny v prostředí Azure runtime hello později. Tady je kód hello: 
   
        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
   
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
   
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }
   
    **Poznámka:** tento kód vyžaduje hello ADAL pro balíček NuGet pro rozhraní .NET (Microsoft.IdentityModel.Clients.ActiveDirectory), který je již nainstalován v projektu hello. Pokud tento projekt bylo vytváření od začátku, by měl tooinstall tento balíček. Tento balíček není automaticky instalován šablonou nový projekt aplikace API hello.
2. V *řadiče nebo ToDoListController*, zrušte komentář u hello kód v hello `NewDataAPIClient` metoda, která přidává hello tokenu tooHTTP požadavky v hlavičce autorizace hello.
   
        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
3. Nasazení projektu ToDoListAPI hello. (Klikněte pravým tlačítkem na projekt hello a pak klikněte na **publikovat > Publikovat**.)
   
    Visual Studio nasadí projekt hello a otevře základní adresu URL prohlížeče toohello webové aplikace. Zobrazí 403 chybovou stránku, což je normální pokus o toogo tooa webového rozhraní API základní adresy URL v prohlížeči.
4. Prohlížeč zavřít hello.

### <a name="get-azure-ad-configuration-values"></a>Získat hodnoty konfigurace Azure AD
1. V hello [portál Azure classic](https://manage.windowsazure.com/), přejděte příliš**Azure Active Directory**.
2. Na hello **Directory** , klikněte na vašem tenantovi AAD.
3. Klikněte na tlačítko **aplikace > Moje společnost vlastní aplikace**a pak klikněte na tlačítko zaškrtnutí hello.
4. V hello seznam aplikací klikněte na název hello hello ten, který Azure vytvořené pro vás při povolení ověřování pro aplikaci API (datové vrstvy) ToDoListDataAPI hello.
5. Klikněte na tlačítko hello **konfigurace** kartě.
6. Kopírování hello **ID klienta** hodnotu a uložte ho někde můžete získat z později. 
7. V hello portál Azure classic přejděte zpět toohello seznam **aplikace Moje společnost vlastní**a klikněte na tlačítko hello AAD aplikace, kterou jste vytvořili pro aplikaci ToDoListAPI API střední vrstvy hello (hello není vytvořené v předchozí kurzu hello Hello, jednu, kterou jste vytvořili v tomto kurzu).
8. Klikněte na tlačítko hello **konfigurace** kartě.
9. Kopírování hello **ID klienta** hodnotu a uložte ho někde můžete získat z později.
10. V části **klíče**, vyberte **1 rok** z hello **vyberte dobu trvání** rozevíracího seznamu.
11. Klikněte na **Uložit**.
    
     ![Vygenerování klíče aplikace](./media/app-service-api-dotnet-service-principal-auth/genkey.png)
12. Zkopírujte hodnotu klíče hello a uložte někde, které můžete získat z později.
    
     ![Zkopírujte nový klíč aplikace](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-hello-middle-tier-api-apps-runtime-environment"></a>Konfigurace nastavení služby Azure AD v aplikaci API střední vrstvy hello běhového prostředí
1. Přejděte toohello [portál Azure](https://portal.azure.com/)a potom přejděte toohello **aplikace API** aplikace hello rozhraní API, který je hostitelem hello projekt TodoListAPI (střední úroveň).
2. Klikněte na **Nastavení > Nastavení aplikace**.
3. V hello **nastavení aplikace** přidejte hello následující klíče a hodnoty:
   
   | **Klíč** | IDA: autority |
   | --- | --- |
   | **Hodnota** |https://Login.microsoftonline.com/ {název klienta Azure AD} |
   | **Příklad** |https://Login.microsoftonline.com/contoso.onmicrosoft.com |
   
   | **Klíč** | IDA: ClientId |
   | --- | --- |
   | **Hodnota** |ID klienta hello volání aplikace (střední úroveň - ToDoListAPI) |
   | **Příklad** |960adec2-b74a-484A-960adec2-b74a-484A |
   
   | **Klíč** | IDA: ClientSecret |
   | --- | --- |
   | **Hodnota** |Klíč aplikace hello volání aplikace (střední úroveň - ToDoListAPI) |
   | **Příklad** |e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |
   
   | **Klíč** | IDA: prostředků |
   | --- | --- |
   | **Hodnota** |ID klienta hello se nazývá aplikace (datovou vrstvu - ToDoListDataAPI) |
   | **Příklad** |e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |
   
    **Poznámka:**: pro `ida:Resource`, nezapomeňte použít hello názvem aplikace **ID klienta** a ne její **identifikátor ID URI aplikace**.
   
    `ida:ClientId`a `ida:Resource` jsou různé hodnoty pro tento kurz vzhledem k tomu, že používáte samostatné applicaations Azure AD pro hello střední vrstvy a datové vrstvy. Pokud jste používali jedné aplikaci Azure AD pro volání rozhraní API app a hello hello chráněné aplikace API, využije hello stejnou hodnotu v obou `ida:ClientId` a `ida:Resource`.
   
    Kód Hello používá ConfigurationManager tooget tak může být uložený v souboru Web.config hello projektu nebo v prostředí Azure runtime hello tyto hodnoty. Při spuštěné aplikaci ASP.NET ve službě Azure App Service přepsat nastavení prostředí automaticky nastavení ze souboru Web.config. Nastavení prostředí jsou obecně [bezpečnější způsob toostore citlivé informace porovnání souboru Web.config tooa](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
4. Klikněte na **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-hello-application"></a>Testování aplikace hello
1. Přejděte v prohlížeči toohello adresy URL HTTPS hello AngularJS front-endu webové aplikace.
2. Klikněte na tlačítko hello **tooDo seznamu** kartě a přihlaste se pomocí pověření pro uživatele v klientovi služby Azure AD. 
3. Přidáte položky tooverify úkolů, které aplikace hello funguje.
   
    ![Stránka seznamu tooDo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)
   
    Pokud aplikace hello nebude fungovat podle očekávání, zkontrolujte všechny hello nastavení, které jste zadali do hello portálu Azure. Pokud se zobrazí všechny hello nastavení toobe správný, najdete v části hello [Poradce při potížích s](#troubleshooting) později v tomto kurzu.

## <a name="protect-hello-api-app-from-browser-access"></a>Hello aplikace API jsou chráněny před přístup z prohlížeče
V tomto kurzu jste vytvořili samostatné aplikaci Azure AD pro aplikaci hello ToDoListDataAPI (datové vrstvy) rozhraní API. Jak jsme viděli, a když služby App Service vytvoří aplikaci AAD nakonfiguruje hello AAD aplikace způsobem, který umožňuje na adresu URL uživatel toogo toohello aplikace API v prohlížeči a protokolu. To znamená, že je možné pro koncového uživatele v klientovi služby Azure AD, ne jenom instančního objektu, tooaccess hello rozhraní API. 

Pokud chcete přístup z prohlížeče tooprevent bez psaní kódu v hello chráněné aplikace API, můžete změnit hello **adresa URL odpovědi** v hello AAD aplikace tak, aby se liší od aplikace hello rozhraní API základní adresu URL. 

### <a name="disable-browser-access"></a>Zakázat přístup z prohlížeče
1. V hello portálu classic **konfigurace** kartě hello AAD aplikace, která byla vytvořena pro hello TodoListService, změňte hodnotu hello v hello **adresa URL odpovědi** pole tak, aby se platná adresa URL, ale není hello rozhraní API aplikace ADRESA URL.
2. Klikněte na **Uložit**.

### <a name="verify-browser-access-no-longer-works"></a>Ověřte, že přístup z prohlížeče přestane fungovat.
Dříve jste ověřili, že můžete přejít toohello adresu URL aplikace API z prohlížeče pomocí přihlášení s přihlašovacími údaji pro jednotlivé uživatele. V této části ověřte, že to není možné. 

1. V nové okno prohlížeče přejděte znovu toohello adresu URL aplikace API hello.
2. Při přihlášení zobrazí výzva toodo tak.
3. Přihlášení úspěšné, ale vede tooan chybovou stránku.
   
    AAD aplikace hello jste nakonfigurovali tak, aby uživatelé v tenantovi AAD hello nemůže přihlásit a přístup k rozhraní API hello z prohlížeče. Můžete pořád přístup k aplikaci API hello pomocí tokenu hlavní služby, které můžete ověřit, přejdete na adresu URL toohello webové aplikace a přidáním další položkami seznamu úkolů.

## <a name="restrict-access-tooa-particular-service-principal"></a>Omezit přístup tooa konkrétní instančního objektu
Teď, všechny volající, který můžete získat token pro uživatele nebo instančního objektu v klientovi služby Azure AD můžete volat aplikace hello TodoListDataAPI (datové vrstvy) rozhraní API. Můžete chtít toomake se, že tuto aplikaci API hello datové vrstvy přijímá pouze volání z aplikace API TodoListAPI (střední vrstvy) hello a pouze z konkrétní instanční objekt. 

Tato omezení můžete přidat tak, že přidáte kód toovalidate hello `appid` a `objectidentifier` deklarace identity na příchozí volání.

Pro účely tohoto kurzu umístíte hello kód, který ověří ID aplikace a ID objektu zabezpečení služby přímo v vaše akce kontroleru.  Alternativy jsou toouse vlastní `Authorize` atribut nebo toodo toto ověření v pořadí spuštění (např. middleware OWIN). Příklad hello druhé, naleznete v části [této ukázkové aplikaci](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Proveďte následující změny projekt TodoListDataAPI toohello hello.

1. Otevřete hello *Controllers/TodoListController.cs* souboru.
2. Zrušením komentáře u hello řádku, které nastavit `trustedCallerClientId` a `trustedCallerServicePrincipalId`.
   
        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];
3. Zrušením komentáře u hello kód v hello CheckCallerId metoda. Tato metoda je volána při spuštění hello každou metodu akce v kontroleru hello. 
   
        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "hello appID or service principal ID is not hello expected value." });
            }
        }
4. Znovu nasaďte tooAzure projekt ToDoListDataAPI hello služby App Service.
5. Přejděte v prohlížeči adresu URL HTTPS toohello AngularJS front-endu webové aplikace a hello domovské stránce klikněte na tlačítko hello **tooDo seznamu** kartě.
   
    aplikace Hello nefunguje, protože selhávají volání ukončení toohello zpět. nový kód Hello kontroluje skutečné appid a které, ale ještě nemá správné hodnoty toocheck hello je proti. Prohlížeč Hello konzoly vývojářských nástrojů hlásí, že tento server hello je vrátila chybu HTTP 401.
   
    ![Došlo k chybě v vývojáře nástrojů konzoly](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)
   
    V hello následujících kroků můžete nakonfigurovat hello očekávaných hodnot.
6. Pomocí Azure AD PowerShell, získáte hello hodnotu služby hello hlavní hello aplikaci Azure AD, kterou jste vytvořili pro projekt TodoListWebApp hello.
   
    a. Návod, jak tooinstall prostředí Azure PowerShell a připojení tooyour odběru naleznete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).
   
    b. tooget seznam objekty služby, provést hello `Login-AzureRmAccount` příkaz a potom hello `Get-AzureRmADServicePrincipal` příkaz.
   
    c. Najít hello objectid pro hello instanční objekt hello TodoListAPI aplikace a uložit ho do umístění, které můžete zkopírovat z později.
7. V hello portálu Azure přejděte toohello okně aplikace API pro aplikaci API hello, který jste nasadili projekt ToDoListDataAPI hello.
8. Klikněte na tlačítko **Nastavení > Nastavení aplikace**.
9. V hello **nastavení aplikace** přidejte hello následující klíče a hodnoty:
   
   | **Klíč** | TODO:TrustedCallerServicePrincipalId |
   | --- | --- |
   | **Hodnota** |Id objektu zabezpečení služby volání aplikace |
   | **Příklad** |4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |
   
   | **Klíč** | TODO:TrustedCallerClientId |
   | --- | --- |
   | **Hodnota** |ID klienta volání aplikace – zkopírovat z aplikace hello TodoListAPI Azure AD |
   | **Příklad** |960adec2-b74a-484A-960adec2-b74a-484A |
10. Klikněte na **Uložit**.
    
     ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)
11. V prohlížeči návratovou adresu URL toohello webové aplikace a hello domovské stránce klikněte na tlačítko hello **tooDo seznamu** kartě.
    
     Tato aplikace hello čas funguje podle očekávání, protože hello důvěryhodné volající aplikace ID a ID objektu zabezpečení služby jsou hello očekávaných hodnot.
    
     ![Stránka seznamu tooDo](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-hello-projects-from-scratch"></a>Sestavení projektů hello od začátku
Hello dva projekty webového rozhraní API, které byly vytvořeny pomocí hello **aplikace API Azure** šablony a nahraďte hello výchozí hodnoty řadiče s řadičem seznamu úkolů projektu. Pro získání hlavní tokeny služby Azure AD v projektu ToDoListAPI hello, hello [Active Directory Authentication Library (ADAL) pro rozhraní .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) byl nainstalován balíček NuGet.

Informace o tom, příliš vytvořit jednostránková aplikace AngularJS pomocí webového rozhraní API back-end, jako je ToDoListAngular, naleznete v tématu [rukou na testovacím: sestavení jedné stránce aplikace (SPA) pomocí rozhraní ASP.NET Web API a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informace o tooadd ověřovacího kódu Azure AD, najdete v části [zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nepleťte ToDoListAPI (střední úroveň) a ToDoListDataAPI (datové vrstvy). Například v tomto kurzu, můžete přidat aplikaci pro rozhraní API ověřování toohello datové vrstvy, **ale klíč aplikace hello musí pocházet ze hello aplikaci Azure AD, kterou jste vytvořili pro aplikaci API střední vrstvy hello**.

## <a name="next-steps"></a>Další kroky
Toto je poslední kurz hello v hello řady aplikace API. 

Další informace o službě Azure Active Directory najdete v tématu hello následující prostředky.

* [Průvodce Azure AD vývojářům](http://aka.ms/aaddev)
* [Scénáře služby Azure AD](http://aka.ms/aadscenarios)
* [Ukázek Azure AD](http://aka.ms/aadsamples)
  
    Hello [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) ukázka je podobné toowhat se zobrazí v tomto kurzu, ale bez použití ověřování služby App Service.

Informace o jiných způsobech toodeploy Visual Studio projekty tooAPI aplikace pomocí sady Visual Studio nebo [automatizace nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) z [systému správy zdrojů](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), najdete v části [jak toodeploy Aplikační služby Azure](../app-service-web/web-sites-deploy.md).

