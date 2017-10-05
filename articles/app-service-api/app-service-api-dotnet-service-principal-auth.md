---
title: "Objekt zabezpečení ověřování služby pro aplikace API v Azure App Service | Microsoft Docs"
description: "Zjistěte, jak do aplikace API v Azure App Service pro scénáře service-to-service jsou chráněny."
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
ms.openlocfilehash: 95653287546bbe358111ed16af0c30a53caff2b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Objekt zabezpečení ověřování služby pro aplikace API v Azure App Service
## <a name="overview"></a>Přehled
Tento článek vysvětluje, jak používat ověřování služby App Service pro *interní* přístup k aplikacím API. Vnitřní scénář je, kdy máte aplikaci API, která mají být použití pouze vlastního kódu aplikace. Doporučeným způsobem, jak implementovat tento scénář ve službě App Service je používat k ochraně názvem aplikace API Azure AD. Volání chráněné aplikace API s token nosiče, který můžete získat z Azure AD tím, že poskytuje identitu aplikace (instanční objekt) přihlašovací údaje. Alternativy k používání služby Azure AD, najdete v článku **Service-to-service ověřování** části [Přehled ověřování služby Azure App Service](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

V tomto článku se dozvíte:

* Jak používat Azure Active Directory (Azure AD) pro aplikace API jsou chráněny před neověřeným přístupem.
* Jak používat chráněné aplikace API z aplikace API, webovou aplikaci nebo mobilní aplikace přes přihlašovací údaje Azure AD služba objektu zabezpečení (identita aplikace). Informace o tom, jak využívat z aplikace logiky najdete v tématu [používání vlastního rozhraní API hostovaného v App Service pomocí aplikací logiky](../logic-apps/logic-apps-custom-hosted-api.md).
* Jak zajistit, že chráněné aplikace API nelze volat z prohlížeče pomocí přihlášeného uživatele.
* Jak zajistit, že chráněné aplikace API, lze volat pouze konkrétní Azure AD služba objektu zabezpečení.

Článek obsahuje dvě části:

* [Postup konfigurace ověřování hlavní služby ve službě Azure App Service](#authconfig) část popisuje obecně konfiguraci ověřování pro všechny aplikace API a jak využívat chráněné aplikace API. Tato část se vztahuje stejnou měrou na všech rozhraní podporovaných službou App Service, včetně .NET, Node.js a Java.
* Od verze [pokračování úvodních kurzů k .NET](#tutorialstart) části kurzu vás provede konfiguraci "interní přístup" scénáře pro ukázkové aplikace .NET spuštěna ve službě App Service. 

## <a id="authconfig"></a>Postup konfigurace ověřování hlavní služby ve službě Azure App Service
Tato část obsahuje obecné pokyny, které platí pro všechny aplikace API. Konkrétní kroky pro ukázkovou aplikaci udělat rozhraní seznamu .NET přejděte na [pokračováním řady kurz .NET API Apps](#tutorialstart).

1. V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, který chcete chránit a pak vyhledejte **funkce** části a klikněte na tlačítko  **Ověřování / autorizace**.
   
    ![Ověřování/autorizace služby na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)
2. V **ověřování / autorizace** okně klikněte na tlačítko **na**.
3. V **akci provést, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory** .
4. V části **zprostředkovatele ověřování**, vyberte **Azure Active Directory**.
   
    ![Ověřování/autorizace okno na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
5. Konfigurace **nastavení Azure Active Directory** okno a vytvořte novou aplikaci Azure AD, nebo použijte existující aplikaci Azure AD Pokud již účet máte, kterou chcete použít.
   
    Interní scénáře jsou obvykle aplikace API volání aplikace API. Můžete použít samostatné Azure AD aplikací pro každou aplikaci API nebo pouze jednu aplikaci Azure AD.
   
    Podrobné pokyny v tomto okně najdete v tématu [postup konfigurace aplikace služby App Service pomocí přihlášení Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
6. Když jste hotovi s okno Konfigurace zprostředkovatele ověřování, klikněte na tlačítko **OK**.
7. V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Když to uděláte, služby App Service umožňuje pouze požadavky z volající v konfigurovaného klienta Azure AD. Bez ověřování nebo autorizace kódu se vyžaduje v chráněném aplikaci API. Token nosiče je předán do aplikace API společně s běžně používané deklarací identity v hlavičkách protokolu HTTP, a můžete si přečíst tyto informace v kódu k ověření, že žádosti jsou z konkrétní volající, jako je například hlavní název služby.

Tato funkce ověřování funguje stejným způsobem jako pro všechny jazyky, které podporuje služby App service, včetně .NET, Node.js a Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Jak používat chráněné aplikace API
Volající musí poskytnout volání rozhraní API tokenu nosiče Azure AD. Chcete-li získat token nosiče pomocí přihlašovacích údajů hlavní služby, volající používá služby Active Directory Authentication Library (ADAL pro [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs), nebo [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Chcete-li získat token, obsahuje kód, který volá ADAL adal následující informace:

* Název vašeho klienta Azure AD.
* ID klienta a tajný klíč klienta (klíč aplikace) aplikace Azure AD, které jsou spojené s volajícím.
* ID klienta aplikace Azure AD přidružené k chráněné aplikace API. (Pokud se používá pouze jednu aplikaci Azure AD, jedná se stejným ID klienta jako pro volající.)

Tyto hodnoty jsou k dispozici na stránkách Azure AD [portál Azure classic](https://manage.windowsazure.com/).

Jakmile získal token volající ji obsahuje u žádostí protokolu HTTP v hlavičce autorizace.  Služba App Service ověří token a umožňuje žádosti, které chcete dosáhnout chráněné aplikace API.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Jak chránit aplikace API z přístup uživatelů ve stejném klientovi
Nosné tokeny pro uživatele ve stejném klientovi jsou považovány za platné pro chráněné aplikace API.  Pokud chcete zajistit, že pouze objekt služby můžete volat chráněné aplikace API, přidejte kód v chráněné aplikace API pro ověření následující deklarací z tokenu:

* `appid`ID klienta aplikace Azure AD, která souvisí s volající by měl být. 
* `oid`(`objectidentifier`) by měla být ID objektu zabezpečení služby volajícího. 

Služba App Service také poskytuje `objectidentifier` deklarací identity v záhlaví X-MS-CLIENT-hlavní-ID.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Jak chránit aplikace API z přístup z prohlížeče
Pokud nemáte ověřit deklarace identity v kódu v chráněném aplikace API, a pokud používáte samostatnou aplikaci Azure AD pro chráněné aplikace API, ujistěte se, že adresa URL odpovědi aplikace Azure AD není stejný jako základní adresu URL aplikace API. Pokud adresa URL odpovědi odkazuje přímo na chráněném aplikace API, uživatel ve stejném klientovi Azure AD přejděte do aplikace API, přihlaste se a úspěšně volat rozhraní API.

## <a id="tutorialstart"></a>Pokračováním řady kurz .NET API Apps
Pokud postupujete podle Node.js nebo Java kurz řady pro aplikace API, pokračujte [další kroky](#next-steps) části. 

Zbývající část tohoto článku pokračuje řady kurz .NET API Apps a předpokládá, že jste dokončili [kurzu ověřování uživatele](app-service-api-dotnet-user-principal-auth.md) a ukázkové aplikace běžící v Azure s ověřováním uživatele povolena.

## <a name="set-up-authentication-in-azure"></a>Nastavení ověřování v Azure
V této části můžete nakonfigurovat App Service tak, aby pouze požadavky HTTP umožňuje připojit aplikaci API datové vrstvy jsou ty, které mají platný Azure AD nosné tokeny. 

V následující části můžete nakonfigurovat aplikaci API střední vrstvy odeslat přihlašovací údaje aplikací do Azure AD, získáte zpět token nosiče, a odeslat token nosiče aplikaci API datové vrstvy. Tento proces je znázorněn v diagramu.

![Diagram ověřování služby](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Pokud narazíte na problémy při následujícím kurzu pokynů, najdete v článku [Poradce při potížích s](#troubleshooting) části na konci tohoto kurzu. 

1. V [portál Azure](https://portal.azure.com/), přejděte na **nastavení** okno aplikace API, který jste vytvořili pro aplikaci API ToDoListDataAPI (datové vrstvy) a pak klikněte na tlačítko **nastavení**.
2. V **nastavení** okně Najít **funkce** části a potom klikněte na **ověřování / autorizace**.
   
    ![Ověřování/autorizace služby na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)
3. V **ověřování / autorizace** okně klikněte na tlačítko **na**.
4. V **akci provést, když požadavek nebude ověřený** rozevíracího seznamu vyberte **přihlásit se přes Azure Active Directory**.
   
    Toto je nastavení, která způsobí, že služby App Service k zajištění, pouze ověření reach požadavky aplikace API. Služby App Service pro požadavky, které mají platný nosné tokeny, předá tokeny podél do aplikace API a naplní hlavičky protokolu HTTP s běžně používané deklarace identity, aby tyto informace snadno dostupné pro váš kód.
5. V části **zprostředkovatele ověřování**, klikněte na tlačítko **Azure Active Directory**.
   
    ![Ověřování/autorizace okno na portálu Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)
6. V **nastavení Azure Active Directory** okně klikněte na tlačítko **Express**.
   
    Pomocí **Express** možnost Azure můžete automaticky vytvořit aplikaci AAD ve službě Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 
   
    Nemusíte vytvořit klienta, protože každý účet Azure automaticky jeden má.
7. V části **režim správy**, klikněte na tlačítko **vytvořit novou aplikaci AD** Pokud již není vybrána.
   
    Připojí na portál **vytvořit aplikaci** vstupní pole výchozí hodnotu. Ve výchozím nastavení, aplikace Azure AD se stejným názvem jako aplikace API. Pokud dáváte přednost, můžete zadat jiný název.
   
    ![Nastavení služby Azure AD](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)
   
    **Poznámka:**: jako alternativu, můžete použít jednu Azure AD pro volání rozhraní API app a chráněné aplikace API. Pokud jste vybrali tento alternativa, nebude potřeba **vytvořit novou aplikaci AD** možnost zde, protože jste již vytvořili aplikaci Azure AD v tomto kurzu ověřování uživatele. V tomto kurzu budete používat samostatné aplikace Azure AD pro volání rozhraní API app a chráněné aplikace API.
8. Poznamenejte si hodnotu, která je v **vytvořit aplikaci** vstupní pole; budete vyhledání této AAD aplikace na portálu Azure classic později.
9. Klikněte na **OK**.
10. V **ověřování / autorizace** okně klikněte na tlačítko **Uložit**.
    
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)
    
    Služby App Service vytvoří aplikaci služby Azure Active Directory s **přihlašovací adresa URL** a **adresa URL odpovědi** automaticky nastaví na adresu URL aplikace API. Tato hodnota umožňuje uživatelům ve vašem tenantovi AAD se přihlaste a přístup k aplikaci API.

### <a name="verify-that-the-api-app-is-protected"></a>Ověřte, jestli chráněný aplikace API
1. V prohlížeči přejděte na adresu URL aplikace API: v **aplikace API** okno na portálu Azure klikněte na odkaz v části **URL**. 
   
    Budete přesměrováni na přihlašovací obrazovku, protože neověřené požadavky nejsou povoleny dosáhne aplikace API. 
   
    Pokud váš prohlížeč, přejít na uživatelské rozhraní Swagger, prohlížeč mohou již být zaznamenány na – v takovém případě otevřete okno InPrivate nebo Incognito, přejděte na adresu URL uživatelského rozhraní Swagger.
2. Přihlaste se pomocí přihlašovacích údajů uživatele ve vašem tenantovi AAD.
   
   Pokud jste přihlášeni, "úspěšně vytvořil" stránka se zobrazí v prohlížeči.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurace projektu ToDoListAPI k získání a odeslání tokenu Azure AD
V této části můžete provést následující úkoly:

* Přidání kódu v aplikaci API střední vrstvy, která používá přihlašovací údaje Azure AD aplikace k získání tokenu a jeho odeslání u žádostí protokolu HTTP pro aplikaci API datové vrstvy.
* Přihlašovací údaje, které potřebujete získáte z Azure AD.
* Zadejte přihlašovací údaje do nastavení prostředí runtime Azure App Service v aplikaci API střední vrstvy. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurace projektu ToDoListAPI k získání a odeslání tokenu Azure AD
Proveďte následující změny v projektu ToDoListAPI v sadě Visual Studio.

1. Zrušením komentáře u všech kód *ServicePrincipal.cs* souboru.
   
    Toto je kód, který využívá ADAL pro .NET k získání tokenu nosiče Azure AD.  Používá několik hodnoty konfigurace, která je budete později nastavit v Azure běhové prostředí. Zde je kód: 
   
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
   
    **Poznámka:** tento kód vyžaduje knihovnu ADAL pro balíček NuGet pro rozhraní .NET (Microsoft.IdentityModel.Clients.ActiveDirectory), který je již nainstalován v projektu. Při vytváření tohoto projektu od začátku, museli byste instalaci tohoto balíčku. Tento balíček není automaticky instalován nový projekt šablony aplikace API.
2. V *řadiče nebo ToDoListController*, zrušte komentář kódu v `NewDataAPIClient` metoda, která přidává token do HTTP požadavky v hlavičce autorizace.
   
        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);
3. Nasaďte projekt ToDoListAPI. (Klikněte pravým tlačítkem na projekt a pak klikněte na **publikovat > Publikovat**.)
   
    Visual Studio nasadí projekt a otevře prohlížeč na základní adresu URL webové aplikace. Zobrazí 403 chybovou stránku, což je normální pro pokus o přechod z prohlížeče do webového rozhraní API základní adresu URL.
4. Zavřete prohlížeč.

### <a name="get-azure-ad-configuration-values"></a>Získat hodnoty konfigurace Azure AD
1. V [portál Azure classic](https://manage.windowsazure.com/), přejděte na **Azure Active Directory**.
2. Na **Directory** , klikněte na vašem tenantovi AAD.
3. Klikněte na tlačítko **aplikace > Moje společnost vlastní aplikace**a klikněte na symbol zaškrtnutí.
4. V seznamu aplikací klikněte na název ten, který vytvořil Azure za vás, pokud jste povolili ověřování pro aplikaci API ToDoListDataAPI (datové vrstvy).
5. Klikněte na kartu **KONFIGUROVAT**.
6. Kopírování **ID klienta** hodnotu a uložte ho někde můžete získat z později. 
7. V Azure classic portálu přejděte zpět do seznamu **aplikace Moje společnost vlastní**a klikněte na aplikaci AAD, kterou jste vytvořili pro (ten, který jste vytvořili v předchozí kurzu, nikoli k tomu, které jste vytvořili aplikaci ToDoListAPI API střední vrstvy v tomto kurzu).
8. Klikněte na kartu **KONFIGUROVAT**.
9. Kopírování **ID klienta** hodnotu a uložte ho někde můžete získat z později.
10. V části **klíče**, vyberte **1 rok** z **vyberte dobu trvání** rozevíracího seznamu.
11. Klikněte na **Uložit**.
    
     ![Vygenerování klíče aplikace](./media/app-service-api-dotnet-service-principal-auth/genkey.png)
12. Zkopírujte hodnotu klíče a uložte někde, které můžete získat z později.
    
     ![Zkopírujte nový klíč aplikace](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Konfigurace nastavení služby Azure AD v aplikaci API střední vrstvy běhového prostředí
1. Přejděte na [portál Azure](https://portal.azure.com/)a potom přejděte na **aplikace API** okno pro aplikaci API, který je hostitelem projektu TodoListAPI (střední úroveň).
2. Klikněte na **Nastavení > Nastavení aplikace**.
3. V **nastavení aplikace** přidejte následující klíče a hodnoty:
   
   | **Klíč** | IDA: autority |
   | --- | --- |
   | **Hodnota** |https://Login.microsoftonline.com/ {název klienta Azure AD} |
   | **Příklad** |https://Login.microsoftonline.com/contoso.onmicrosoft.com |
   
   | **Klíč** | IDA: ClientId |
   | --- | --- |
   | **Hodnota** |ID klienta je volající aplikace (střední úroveň - ToDoListAPI) |
   | **Příklad** |960adec2-b74a-484A-960adec2-b74a-484A |
   
   | **Klíč** | IDA: ClientSecret |
   | --- | --- |
   | **Hodnota** |Klíč aplikace z volající aplikace (střední úroveň - ToDoListAPI) |
   | **Příklad** |e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |
   
   | **Klíč** | IDA: prostředků |
   | --- | --- |
   | **Hodnota** |ID klienta názvem aplikace (datovou vrstvu - ToDoListDataAPI) |
   | **Příklad** |e65e8fc9-5f6b-48E8-e65e8fc9-5f6b-48E8 |
   
    **Poznámka:**: pro `ida:Resource`, nezapomeňte použít názvem aplikace **ID klienta** a ne její **identifikátor ID URI aplikace**.
   
    `ida:ClientId`a `ida:Resource` jsou různé hodnoty pro tento kurz vzhledem k tomu, že používáte samostatné applicaations Azure AD pro střední vrstvy a datové vrstvy. Pokud jste používali jedné aplikaci Azure AD pro volání rozhraní API app a chráněné aplikace API, použijete stejnou hodnotu v obou `ida:ClientId` a `ida:Resource`.
   
    Kód používá ConfigurationManager k získání těchto hodnot, tak může být uložený v souboru Web.config projektu nebo v Azure běhové prostředí. Při spuštěné aplikaci ASP.NET ve službě Azure App Service přepsat nastavení prostředí automaticky nastavení ze souboru Web.config. Nastavení prostředí jsou obecně [víc zabezpečit uložit citlivé informace, které jsou ve srovnání se souborem Web.config](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
4. Klikněte na **Uložit**.
   
    ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Testování aplikace
1. V prohlížeči přejděte na adresu URL HTTPS webové aplikace front-endu AngularJS.
2. Klikněte **seznam úkolů** kartě a přihlaste se pomocí pověření pro uživatele v klientovi služby Azure AD. 
3. Přidání položek seznamu úkolů pro ověřte, zda aplikace funguje.
   
    ![Seznam úkolů stránky](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)
   
    Pokud aplikace nebude fungovat podle očekávání, zkontrolujte všechna nastavení, které jste zadali v portálu Azure. Pokud všechna nastavení zdají být správná, přečtěte si téma [Poradce při potížích s](#troubleshooting) později v tomto kurzu.

## <a name="protect-the-api-app-from-browser-access"></a>Aplikace API jsou chráněny před přístup z prohlížeče
V tomto kurzu jste vytvořili samostatné aplikaci Azure AD pro aplikaci API ToDoListDataAPI (datové vrstvy). Jak jste se seznámili, když služby App Service vytvoří aplikaci AAD, nakonfiguruje aplikaci AAD způsobem, který umožňuje uživatelům přejít na adresu URL aplikace API v prohlížeči a přihlaste se. To znamená, že je možné pro koncového uživatele v klientovi Azure AD, ne jenom instančního objektu, pro přístup k rozhraní API. 

Pokud chcete, aby se zabránilo přístup z prohlížeče bez psaní kódu v chráněném aplikaci API, můžete změnit **adresa URL odpovědi** v aplikaci AAD, aby se liší od aplikace API základní adresu URL. 

### <a name="disable-browser-access"></a>Zakázat přístup z prohlížeče
1. Na portálu classic **konfigurace** kartě pro AAD aplikace, které bylo vytvořeno za TodoListService, změňte hodnotu v **adresa URL odpovědi** pole tak, aby se platná adresa URL, ale není adresa URL aplikace API.
2. Klikněte na **Uložit**.

### <a name="verify-browser-access-no-longer-works"></a>Ověřte, že přístup z prohlížeče přestane fungovat.
Dříve jste ověřili, že můžete přejít na adresu URL aplikace API z prohlížeče pomocí přihlášení s přihlašovacími údaji pro jednotlivé uživatele. V této části ověřte, že to není možné. 

1. V nové okno prohlížeče přejděte na adresu URL aplikace API znovu.
2. Přihlaste se při výzvě.
3. Přihlášení úspěšné, ale vede k chybovou stránku.
   
    Aplikace AAD jste nakonfigurovali tak, aby uživatelé v tenantovi AAD nemůže přihlásit a přístup k rozhraní API z prohlížeče. Aplikace API můžete stále přístup pomocí tokenu hlavní služby, které můžete ověřit tak, že přejdete na adresu URL webové aplikace a přidání více položkami seznamu úkolů.

## <a name="restrict-access-to-a-particular-service-principal"></a>Omezení přístupu k objektu konkrétní služby
Teď, všechny volající, který můžete získat token pro uživatele nebo instančního objektu v klientovi služby Azure AD můžete volat aplikaci API TodoListDataAPI (datové vrstvy). Můžete chtít Ujistěte se, že aplikaci API datové vrstvy přijímá pouze volání z aplikace API TodoListAPI (střední úroveň) a pouze z konkrétní instanční objekt. 

Můžete přidat tato omezení přidání kódu k ověření `appid` a `objectidentifier` deklarace identity na příchozí volání.

Pro účely tohoto kurzu umístíte kód, který ověří ID aplikace a ID objektu zabezpečení služby přímo v vaše akce kontroleru.  Alternativy se mají použít vlastní `Authorize` atribut, nebo pokud chcete provést toto ověření v po spuštění pořadí (např. middleware OWIN). Příklad k tomu, najdete v části [této ukázkové aplikaci](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Proveďte následující změny na projekt TodoListDataAPI.

1. Otevřete *Controllers/TodoListController.cs* souboru.
2. Zrušením komentáře u řádků, které nastavit `trustedCallerClientId` a `trustedCallerServicePrincipalId`.
   
        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];
3. Zrušením komentáře u kód v metodě CheckCallerId. Tato metoda je volána na začátku každou metodu akce v kontroleru. 
   
        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }
4. Znovu nasaďte projekt ToDoListDataAPI do služby Azure App Service.
5. V prohlížeči přejděte na adresu URL HTTPS AngularJS front-endu webové aplikace a na domovské stránce klikněte **seznam úkolů** kartě.
   
    Aplikace nebude fungovat, protože selhávají volání back-end. Nový kód je kontrola skutečné appid a které, ale ještě nemá správné hodnoty zkontrolovat je proti. V prohlížeči konzolu nástroje pro vývojáře hlásí, že server vrací chybu HTTP 401.
   
    ![Došlo k chybě v vývojáře nástrojů konzoly](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)
   
    V následujících krocích nakonfigurujete očekávaných hodnot.
6. Pomocí Azure AD PowerShell, získáte hodnotu objektu služby pro aplikaci Azure AD, kterou jste vytvořili pro projekt TodoListWebApp.
   
    a. Pokyny o tom, jak nainstalovat Azure PowerShell a připojte se k předplatnému najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).
   
    b. Pokud chcete získat seznam objekty služby, spusťte `Login-AzureRmAccount` příkaz a potom `Get-AzureRmADServicePrincipal` příkaz.
   
    c. Najít objectid pro objekt služby TodoListAPI aplikace a uložit ho do umístění, které můžete zkopírovat z později.
7. Na portálu Azure přejděte do okna aplikace API pro aplikaci API, které jste nasadili projekt ToDoListDataAPI.
8. Klikněte na tlačítko **Nastavení > Nastavení aplikace**.
9. V **nastavení aplikace** přidejte následující klíče a hodnoty:
   
   | **Klíč** | TODO:TrustedCallerServicePrincipalId |
   | --- | --- |
   | **Hodnota** |Id objektu zabezpečení služby volání aplikace |
   | **Příklad** |4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |
   
   | **Klíč** | TODO:TrustedCallerClientId |
   | --- | --- |
   | **Hodnota** |ID klienta volání aplikace – zkopírovaných z aplikace TodoListAPI Azure AD |
   | **Příklad** |960adec2-b74a-484A-960adec2-b74a-484A |
10. Klikněte na **Uložit**.
    
     ![Kliknutí na Uložit](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)
11. V prohlížeči, vraťte se na adresu URL webové aplikace a na domovské stránce klikněte **seznam úkolů** kartě.
    
     Tentokrát aplikace funguje podle očekávání, protože aplikace důvěryhodné volající ID a ID objektu zabezpečení služby jsou očekávaných hodnot.
    
     ![Seznam úkolů stránky](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Sestavení projektů od začátku
Dva projekty webového rozhraní API, které byly vytvořeny pomocí **aplikace API Azure** šablony a nahrazení řadiče hodnoty výchozí řadič seznamu úkolů projektu. Pro získání hlavní tokeny služby Azure AD v projektu ToDoListAPI [Active Directory Authentication Library (ADAL) pro rozhraní .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) byl nainstalován balíček NuGet.

Informace o tom, jak vytvořit jednostránková aplikace AngularJS s webového rozhraní API back-end, jako je ToDoListAngular najdete v tématu [rukou na testovacím: sestavení jedné stránce aplikace (SPA) pomocí rozhraní ASP.NET Web API a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informace o tom, jak přidat kód pro ověřování Azure AD najdete v tématu [zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nepleťte ToDoListAPI (střední úroveň) a ToDoListDataAPI (datové vrstvy). Například v tomto kurzu přidáte ověřování pro aplikaci API datové vrstvy, **klíč aplikace musí pocházet z aplikace Azure AD, kterou jste vytvořili pro aplikaci API střední vrstvy, ale**.

## <a name="next-steps"></a>Další kroky
Toto je poslední kurz v řadě aplikace API. 

Další informace o službě Azure Active Directory najdete v následujících materiálech.

* [Průvodce Azure AD vývojářům](http://aka.ms/aaddev)
* [Scénáře služby Azure AD](http://aka.ms/aadscenarios)
* [Ukázek Azure AD](http://aka.ms/aadsamples)
  
    [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) ukázka je podobná informace zobrazené v tomto kurzu, ale bez použití ověřování služby App Service.

Informace o jiných způsobech nasazení projektů sady Visual Studio do aplikace API, pomocí sady Visual Studio nebo [automatizace nasazení](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) z [systému správy zdrojů](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), najdete v části [nasazení Azure Aplikační služby](../app-service-web/web-sites-deploy.md).

