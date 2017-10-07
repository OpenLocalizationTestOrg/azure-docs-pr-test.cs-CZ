---
title: aaaHow toowork s hello .NET back-end serveru SDK pro Mobile Apps | Microsoft Docs
description: "Zjistěte, jak hello toowork s .NET back-end serveru SDK pro Azure App Service Mobile Apps."
keywords: "služby App service, služby azure app service, mobilní aplikace, mobilní služby, nasazení aplikací nasazení azure škálovatelné, aplikace stupnice"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a>Práce s hello .NET back-end serveru SDK pro Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Toto téma ukazuje, jak toouse hello .NET back-end serveru SDK v klíčové scénáře Azure App Service Mobile Apps. Hello Azure Mobile Apps SDK umožňuje pracovat s mobilních klientů z vaší aplikace ASP.NET.

> [!TIP]
> Hello [server .NET SDK pro Azure Mobile Apps] [ 2] je open source na Githubu. Hello úložiště obsahuje všechny zdrojový kód, včetně hello celý server SDK jednotky testovací sady a některé ukázkové projekty.
>
>

## <a name="reference-documentation"></a>Referenční dokumentace
Hello referenční dokumentaci k nástroji pro hello server SDK se nachází zde: [Azure Mobile Apps .NET – referenční informace][1].

## <a name="create-app"></a>Postupy: vytvoření back-endu mobilní aplikace .NET
Pokud spouštíte nový projekt, můžete vytvořit aplikaci služby App Service pomocí buď hello [portál Azure] nebo Visual Studio. Můžete spustit místně hello aplikace služby App Service nebo publikování hello projektu tooyour cloudové služby App Service mobilní aplikace.

Chcete-li přidat existující projekt tooan mobilní funkce, najdete v části hello [stáhnout a inicializace hello SDK](#install-sdk) části.

### <a name="create-a-net-backend-using-hello-azure-portal"></a>Vytvořit rozhraní .NET back-end pomocí hello portálu Azure
toocreate backendu mobilní služby App Service, buď použijte hello [rychlý úvodní kurz] [ 3] nebo postupujte podle těchto kroků:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Zpět v hello *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **C#** jako vaše **back-end jazyk**. Klikněte na tlačítko **Stáhnout**, extrahujte komprimované projektu soubory tooyour místního počítače a otevřete hello řešení v sadě Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Vytvořit rozhraní .NET back-end pomocí Visual Studio 2013 a Visual Studio 2015
Nainstalujte hello [Azure SDK for .NET] [ 4] (verze 2.9.0 nebo novější) toocreate projektu Azure Mobile Apps v sadě Visual Studio. Po instalaci hello SDK, vytvořte aplikaci ASP.NET pomocí hello následující kroky:

1. Otevřete hello **nový projekt** dialogové okno (z *soubor* > **nový** > **projekt...** ).
2. Rozbalte položku **šablony** > **Visual C#**a vyberte **webové**.
3. Vyberte **webové aplikace ASP.NET**.
4. Zadejte název projektu hello. Pak klikněte na **OK**.
5. V části *ASP.NET 4.5.2 šablony*, vyberte **mobilní aplikace Azure**. Zkontrolujte **hostitel v cloudu hello** toocreate mobilního back-endu hello cloudu toowhich můžete publikovat tento projekt.
6. Klikněte na **OK**.

## <a name="install-sdk"></a>Postupy: stažení a inicializace hello SDK
Hello SDK je k dispozici na [NuGet.org]. Tento balíček obsahuje tooget základní funkci požadovanou hello pomocí hello SDK spuštěna. tooinitialize hello SDK, musíte tooperform akce na hello **HttpConfiguration** objektu.

### <a name="install-hello-sdk"></a>Nainstalujte hello SDK
hello tooinstall SDK, klikněte pravým tlačítkem na hello serverový projekt v sadě Visual Studio, vyberte **spravovat balíčky NuGet**, vyhledejte hello [Microsoft.Azure.Mobile.Server] balíček a potom klikněte na  **Nainstalujte**.

### <a name="server-project-setup"></a>Inicializace hello serverový projekt
Projekt .NET back-end server, je inicializovaného podobné tooother projekty ASP.NET, včetně třídy pro spuštění OWIN. Ujistěte se, že máte odkazuje balíček NuGet hello `Microsoft.Owin.Host.SystemWeb`. tooadd této třídy v sadě Visual Studio, klikněte pravým tlačítkem myši na serverový projekt a vyberte **přidat** >
**nová položka**, pak **webové**  >  ** Obecné** > **třídy pro spuštění OWIN**.  Třída je generována hello následující atribut:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

V hello `Configuration()` metoda vaší třídy pro spuštění OWIN, použijte **HttpConfiguration** objekt tooconfigure hello Azure Mobile Apps prostředí.
Následující ukázka Hello inicializuje serverový projekt hello se žádné nové funkce:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

tooenable jednotlivé funkce, musí volat metody rozšíření pro hello **MobileAppConfiguration** objekt před voláním **metodu**. Například následující kód hello přidá výchozí hello směruje řadiče tooall rozhraní API, které mají atribut hello `[MobileAppController]` během inicializace:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Rychlý start server Hello z hello Azure portálu volání **UseDefaultConfiguration()**. Tato ekvivalentní toohello následující nastavení:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

Hello rozšiřující metody používané jsou:

* `AddMobileAppHomeController()`poskytuje hello výchozí Azure Mobile Apps domovskou stránku.
* `MapApiControllers()`poskytuje možnosti vlastního rozhraní API pro WebAPI řadiče označených pomocí hello `[MobileAppController]` atribut.
* `AddTables()`poskytuje mapování hello `/tables` tootable řadiče koncové body.
* `AddTablesWithEntityFramework()`je sdružený pro mapování hello `/tables` používající rozhraní Entity Framework koncových bodů na základě řadiče.
* `AddPushNotifications()`poskytuje jednoduchý způsob registrace zařízení pro centra oznámení.
* `MapLegacyCrossDomainController()`poskytuje standardní hlavičky CORS pro místní vývoj.

### <a name="sdk-extensions"></a>Rozšíření sady SDK
Hello následující balíčky NuGet prostřednictvím rozšíření poskytují různé mobilní funkce, které mohou být využívána vaší aplikace. Povolit rozšíření během inicializace s použitím hello **MobileAppConfiguration** objektu.

* [Microsoft.Azure.Mobile.Server.Quickstart] podporuje hello základní nastavení mobilní aplikace. Konfigurace přidané toohello pomocí volání hello **UseDefaultConfiguration** metoda rozšíření během inicializace. Toto rozšíření zahrnuje následující rozšíření: oznámení, ověřování, entita, tabulky, mezi doménami a domovské balíčky. Tento balíček je používán hello mobilní aplikace Quickstart na hello portál Azure k dispozici.
* [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementuje výchozí hello *tuto aplikaci je spuštěný a funkční stránky* pro kořenový adresář webu hello. Přidat konfiguraci toohello voláním **AddMobileAppHomeController** metoda rozšíření.
* [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) obsahuje třídy pro práci s dat a nastaví až hello datovém kanálu. Přidat konfiguraci toohello volání hello **AddTables** metoda rozšíření.
* [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) umožňuje hello Entity Framework tooaccess data v hello databáze SQL. Přidat konfiguraci toohello volání hello **AddTablesWithEntityFramework** metoda rozšíření.
* [Microsoft.Azure.Mobile.Server.Authentication] umožňuje ověřování a nastaví up hello middlewaru OWIN, který používá toovalidate tokeny. Přidat konfiguraci toohello volání hello **AddAppServiceAuthentication** a **IAppBuilder**. **UseAppServiceAuthentication** rozšiřující metody.
* [Microsoft.Azure.Mobile.Server.Notifications] povoluje nabízená oznámení a definuje koncový bod nabízené registrace. Přidat konfiguraci toohello volání hello **AddPushNotifications** metoda rozšíření.
* [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) vytvoří kontroler, který slouží data toolegacy webových prohlížečů z mobilní aplikace. Přidat konfiguraci toohello voláním **MapLegacyCrossDomainController** metoda rozšíření.
* [Microsoft.Azure.Mobile.Server.Login] poskytuje hello AppServiceLoginHandler.CreateToken() metodu, která se používají při ověřování vlastní scénáře statickou metodu.

## <a name="publish-server-project"></a>Postupy: publikování projektu server hello
Tato část uvádí, jak toopublish váš back-end .NET projektu ze sady Visual Studio. Můžete taky nasadit projektu back-end pomocí Git nebo žádné z hello jiné metody popsané v hello [dokumentaci k nasazení služby Azure App Service](../app-service-web/web-sites-deploy.md).

1. V sadě Visual Studio znovu sestavte balíčky NuGet toorestore hello projektu.
2. V Průzkumníku řešení klikněte pravým tlačítkem na projekt hello, klikněte na tlačítko **publikovat**. Hello prvním publikujete, je nutné toodefine profil publikování. Pokud již máte profil definované, můžete jej vybrat a klikněte na tlačítko **publikovat**.
3. Pokud se zobrazí dotaz tooselect cíl publikování, klikněte na tlačítko **Microsoft Azure App Service** > **Další**, pak (v případě potřeby) přihlásit se pomocí svých přihlašovacích údajů Azure.
   Visual Studio stáhne a bezpečně uloží vaše nastavení publikování přímo z Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Zvolte vaše **předplatné**, vyberte **typ prostředku** z **zobrazení**, rozbalte položku **mobilní aplikace**a klikněte na váš back-end mobilní aplikace a pak klikněte na **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Ověřte hello publikovat informace o profilu a klikněte na tlačítko **publikovat**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Pokud váš back-end mobilní aplikace byla úspěšně publikována, zobrazí se cílová stránka udávající úspěch.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a>Postupy: definování řadič tabulky
Definujte tooexpose tabulky řadiče klienti toomobile tabulky SQL.  Konfigurace řadič tabulky vyžaduje tři kroky:

1. Vytvořte třídu objektu pro přenos dat (DTO).
2. Nakonfigurujte odkaz na tabulku v hello třídy Mobile DbContext.
3. Vytvořte řadič tabulky.

Objekt pro přenos dat (DTO) je prostý C# objekt, který dědí z `EntityData`.  Například:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Hello DTO je použité toodefine hello tabulky v databázi SQL hello.  toocreate hello databáze položky, přidejte `DbSet<>` vlastnost hello DbContext, kterou používáte.  V hello výchozí šablona projektu pro Azure Mobile Apps, hello DbContext nazývá `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Pokud máte hello instalace Azure SDK, nyní můžete vytvořit řadič tabulky šablony následujícím způsobem:

1. Klikněte pravým tlačítkem na složku hello řadiče a vyberte **přidat** > **řadiče...** .
2. Vyberte hello **Azure Mobile Apps tabulky řadič** možnost a potom klikněte na **přidat**.
3. V hello **přidat kontroler** dialogové okno:
   * V hello **třída modelu** rozevíracího seznamu, vyberte vaše nové DTO.
   * V hello **DbContext** rozevíracím třídy DbContext služby Mobile vyberte hello.
   * název řadiče Hello je vytvořen.
4. Klikněte na tlačítko **Přidat**.

obsahuje Hello rychlý start serverový projekt pro jednoduchý příklad **TodoItemController**.

### <a name="adjust-pagesize"></a>Postupy: přizpůsobení velikosti stránkování tabulky hello
Ve výchozím nastavení vrátí Azure Mobile Apps 50 záznamů na základě požadavku.  Stránkování zajišťuje, že klient hello neblokuje jejich uživatelského rozhraní vlákno ani hello server příliš dlouho, zajišťuje vhodné uživatelské prostředí. velikost toochange hello tabulky stránkování, zvýšení na straně serveru hello "povolená velikost dotazu" a hello stránky na straně klienta velikost hello straně serveru "povolená velikost dotazu" je upravit pomocí hello `EnableQuery` atribut:

    [EnableQuery(PageSize = 500)]

Zkontrolujte, zda text hello PageSize hello stejné nebo větší než požadovaná klientem hello velikost hello.  Další informace o změně velikosti stránky hello klienta naleznete v toohello konkrétního klienta postupy dokumentaci.

## <a name="how-to-define-a-custom-api-controller"></a>Postupy: definování vlastní kontroler API
kontroler API vlastní Hello poskytuje hello nejzákladnější funkce tooyour mobilní back-end aplikace tím, že vystavení koncový bod. Můžete zaregistrovat řadič specifické mobilní rozhraní API pomocí atributu hello [MobileAppController]. Hello `MobileAppController` atribut zaregistruje trasu hello, nastaví serializátor JSON mobilní aplikace hello a zapne [kontrolu verzí klienta](app-service-mobile-client-and-server-versioning.md).

1. V sadě Visual Studio, klikněte pravým tlačítkem na složku řadiče hello, pak klikněte na tlačítko **přidat** > **řadič**, vyberte **kontroler Web API 2&mdash;prázdný** a Klikněte na tlačítko **přidat**.
2. Zadejte **názvu Kontroleru**, jako například `CustomController`a klikněte na tlačítko **přidat**.
3. Hello nový soubor třídy kontroleru, přidejte hello následující příkaz using:

        using Microsoft.Azure.Mobile.Server.Config;
4. Použít hello **[MobileAppController]** atribut definice třídy řadiče toohello rozhraní API, stejně jako hello následující ukázka:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. V souboru App_Start/Startup.MobileApp.cs přidejte volání toohello **MapApiControllers** – metoda rozšíření, jako hello následující ukázka:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Můžete taky hello `UseDefaultConfiguration()` metoda rozšíření místo `MapApiControllers()`. Každý kontroler, který nemá **MobileAppControllerAttribute** použít můžete i nadále přistupovat klienti, ale nemusí být zpracován správně klienty pomocí libovolného mobilní aplikace klienta SDK.

## <a name="how-to-work-with-authentication"></a>Postupy: práce s ověřováním
Azure Mobile Apps používá aplikace služby ověřování / autorizace toosecure mobilního back-endu.  V této části se dozvíte, jak tooperform hello následující úkolech týkajících se ověřování v rozhraní .NET back-end serveru projektu:

* [Postupy: přidání ověřování tooa serverový projekt](#add-auth)
* [Postupy: použití vlastního ověřování pro aplikaci](#custom-auth)
* [Postupy: načtení ověřit informace o uživateli](#user-info)
* [Postupy: omezení přístupu k datům Autorizovaní uživatelé](#authorize)

### <a name="add-auth"></a>Postupy: přidání ověřování tooa serverový projekt
Můžete přidat ověřování tooyour serverový projekt rozšířením hello **MobileAppConfiguration** objekt a konfiguraci middlewaru OWIN. Při instalaci hello [Microsoft.Azure.Mobile.Server.Quickstart] balíčku a volání hello **UseDefaultConfiguration** metoda rozšíření, můžete přeskočit toostep 3.

1. V sadě Visual Studio, nainstalujte hello [Microsoft.Azure.Mobile.Server.Authentication] balíčku.
2. V souboru Startup.cs projektu hello, přidejte následující řádek kódu na začátku hello hello hello **konfigurace** metoda:

        app.UseAppServiceAuthentication(config);

    Tato komponenta middlewaru OWIN ověřuje tokeny vydané bránou hello související služby App Service.
3. Přidat hello `[Authorize]` atribut tooany kontroler nebo metodu, která vyžaduje ověření.

toolearn o tooauthenticate klienti tooyour back-end mobilní aplikace, najdete v části [přidat ověřování tooyour aplikace](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Postupy: použití vlastního ověřování pro aplikaci
Pokud nechcete, aby toouse jednoho z poskytovatelů hello ověřování/autorizace služby App Service, můžete implementovat systému přihlášení. Nainstalujte hello [Microsoft.Azure.Mobile.Server.Login] balíček tooassist s generování tokenů ověřování.  Zadejte vlastní kód pro ověření pověření uživatele. Můžete například zkontrolovat proti solené a hash hesla v databázi. V příkladu hello níže hello `isValidAssertion()` – metoda (definovanou jinde) zodpovídá za tyto kontroly.

vlastní ověřování Hello je zveřejněný prostřednictvím objektu ApiController vytvoření a vystavení `register` a `login` akce. Hello klient musí použít vlastní informace hello toocollect uživatelského rozhraní od uživatele hello.  informace o Hello je zavolat odeslaná toohello rozhraní API s standardní HTTP POST. Jakmile hello server ověří hello assertion, pomocí hello token je vydat `AppServiceLoginHandler.CreateToken()` metoda.  Hello objektu ApiController **neměli** použít hello `[MobileAppController]` atribut.

Příklad `login` akce:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

V předchozím příkladu hello LoginResult a LoginResultUser jsou serializovatelné objekty vystavení požadované vlastnosti. Klient Hello očekává toobe odpovědi přihlášení vrátí jako objekty JSON hello formuláře:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Hello `AppServiceLoginHandler.CreateToken()` metoda zahrnuje *cílovou skupinu* a *vystavitele* parametr. Oba tyto parametry se nastavují toohello URL kořenového adresáře aplikace pomocí hello schéma HTTPS. Podobně byste měli nastavit *secretKey* toobe hello hodnotu vaší aplikace je podpisový klíč. Nedistribuovat hello podpisového klíče v klientovi jako může být použité toomint klíče a zosobnit uživatele. Můžete získat hello podpisového klíče při odkazování na hello hostované ve službě App Service *webu\_AUTH\_PODPISOVÁNÍ\_klíč* proměnné prostředí. V případě potřeby v kontextu místního ladění, postupujte podle pokynů hello v hello [místní ladění pomocí ověřování](#local-debug) části tooretrieve hello klíč a uložte ho jako nastavení aplikace.

token vydán Hello zahrnovat také další deklarace identity a datum vypršení platnosti.  Minimálně hello vydán token musí obsahovat předmět (**sub**) deklarace identity.

Může podporovat standard klienta hello `loginAsync()` metoda podle přetížení hello ověřování trasy.  Pokud klient hello volá `client.loginAsync('custom');` toolog ve vašem cesty musí být `/.auth/login/custom`.  Můžete nastavit hello trasu pro hello vlastní ověřování řadiče pomocí `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Pomocí hello `loginAsync()` přístup zajišťuje, že ověřovací token hello je připojen tooevery následných volání toohello služby.
>
>

### <a name="user-info"></a>Postupy: načtení ověřit informace o uživateli
Pokud je uživatel ověřen službou App Service, můžete přistupovat hello přiřazené ID uživatele a další informace v rozhraní .NET back-end kódu. informace o uživateli Hello slouží pro při autorizačním rozhodování v back-end hello. Hello následující kód získá ID uživatele hello přidružený k požadavku:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

Hello SID je odvozen od ID uživatele specifický pro zprostředkovatele hello a je statický pro daného uživatele a zprostředkovatele přihlášení.  Hello SID má hodnotu null pro tokeny neplatný ověřování.

Služby App Service umožňuje taky deklarace identity specifické požadavky na poskytovatele přihlášení. Každý poskytovatel identity může poskytovat další informace pomocí zprostředkovatele identity SDK.  Například můžete hello Graph API sítě Facebook informace přátel.  Zadávat lze deklarace identity, které jsou požadovány v okně zprostředkovatele hello v hello portálu Azure. Některé deklarace identity vyžadovat dodatečnou konfiguraci pomocí zprostředkovatele identity hello.

Hello následující kód volání hello **GetAppServiceIdentityAsync** rozšíření metoda tooget hello přihlašovacích údajů, které zahrnují požadavky na přístup tokenu potřebné toomake hello proti hello Graph API sítě Facebook:

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Přidat pomocí příkazu pro `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** metoda rozšíření.

### <a name="authorize"></a>Postupy: omezení přístupu k datům Autorizovaní uživatelé
V předchozí části hello jsme vám ukázal, jak tooretrieve hello ID uživatele ověřeného uživatele. Můžete omezit přístup toodata a dalším prostředkům na základě této hodnoty. Jednoduchý způsob toolimit, vrácená data jenom tooauthorized uživatelé je například přidávání tootables sloupec ID uživatele a filtrování výsledků dotazu hello podle ID uživatele hello. Hello následující kód vrátí řádky dat pouze v případě hello SID odpovídá hodnotě v hello UserId sloupce v tabulce TodoItem hello:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

Hello `Query()` metoda vrátí `IQueryable` který smí uživatel manipulovat LINQ toohandle filtrování.

## <a name="how-to-add-push-notifications-tooa-server-project"></a>Postupy: Přidání nabízených oznámení tooa serverový projekt
Přidat nabízená oznámení tooyour serverový projekt rozšířením hello **MobileAppConfiguration** objekt a vytvoření centra oznámení klienta.

1. V sadě Visual Studio, klikněte pravým tlačítkem na hello serverový projekt a klikněte na tlačítko **spravovat balíčky NuGet**, vyhledejte `Microsoft.Azure.Mobile.Server.Notifications`, pak klikněte na tlačítko **nainstalovat**.
2. Opakujte tento krok tooinstall hello `Microsoft.Azure.NotificationHubs` balíček, která zahrnuje klientské knihovny pro hello centra oznámení.
3. V App_Start/Startup.MobileApp.cs a přidejte volání toohello **AddPushNotifications()** metoda rozšíření během inicializace:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Přidejte následující kód, který vytvoří klienta Notification Hubs hello:

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Teď můžete použít hello centra oznámení klienta toosend nabízená oznámení tooregistered zařízení. Další informace najdete v tématu [přidat nabízená oznámení tooyour aplikace](app-service-mobile-ios-get-started-push.md). toolearn Další informace o Notification Hubs najdete v části [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Postupy: povolení cílové nabízené pomocí značek
Centra oznámení můžete odeslat toospecific registrace cílená oznámení pomocí značek. Automaticky vytvoří několik značky:

* Hello ID instalace identifikuje konkrétní zařízení.
* Hello Id uživatele na základě hello ověřený SID identifikuje konkrétního uživatele.

Hello instalace ID je přístupná z hello **installationId** vlastnost hello **MobileServiceClient**.  Hello následující příklad ukazuje, jak používat tooadd ID instalace instalaci specifické tooa značky v Notification Hubs:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Všechny značky poskytl hello klienta během registrace nabízených oznámení se ignorují hello back-end, při vytváření hello instalace. tooenable tooadd klienta značky toohello instalace, musíte vytvořit vlastní rozhraní API, které přidá značky pomocí hello předcházející vzor.

V tématu [klienta přidat nabízená oznámení značky] [ 5] v ukázce dokončené rychlý start App Service Mobile Apps hello příklad.

## <a name="push-user"></a>Postupy: odeslání nabízených oznámení tooan ověřeného uživatele
Pokud ověřený uživatel zaregistruje pro nabízená oznámení, značku ID uživatele se automaticky přidá toohello registrace. Pomocí této značky může odesílat nabízená oznámení tooall zařízení registrovaná touto osobou. Hello následující kód získá hello identifikátor SID uživatele, který vytvořil žádost a odešle registrace zařízení k tooevery šablony nabízených oznámení pro tuto osobu:

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování. Další informace najdete v tématu [Push toousers] [ 6] v ukázce dokončené rychlý start hello App Service Mobile Apps pro rozhraní .NET back-end.

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a>Postupy: ladění a odstraňování potíží hello sady SDK pro .NET serveru
Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace ASP.NET:

* [Monitorování služby Azure App Service](../app-service-web/web-sites-monitor.md)
* [Povolit protokolování diagnostiky v Azure App Service](../app-service-web/web-sites-enable-diagnostic-log.md)
* [Řešení potíží s Azure App Service v sadě Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Protokolování
Diagnostické protokoly služby tooApp můžete zapsat pomocí hello zápis trasování standardní technologie ASP.NET. Než napíšete toohello protokoly, je nutné povolit diagnostiky v váš back-end mobilní aplikace.

tooenable zápisu a Diagnostika toohello protokoly:

1. Postupujte podle kroků hello v [jak Diagnostika tooenable](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).
2. Přidejte následující hello pomocí příkazu v souboru kódu:

        using System.Web.Http.Tracing;
3. Vytvoření toowrite zapisovač trasování ze hello .NET back-end toohello diagnostické protokoly, následujícím způsobem:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Znovu publikovat serverový projekt a přístup k hello mobilní aplikace back-end tooexecute hello kód cestě s protokolováním hello.
5. Stáhněte si a vyhodnocovat u nich hello protokoly, jak je popsáno v [postupy: stažení protokolů](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Místní ladění pomocí ověřování
Aplikaci můžete spustit místně tootest změní před publikováním toohello cloudu. Většina Azure Mobile Apps back-EndY, stiskněte klávesu *F5* při v sadě Visual Studio. Existují však některé další aspekty při použití ověřování.

Musí mít mobilní aplikaci s ověřování/autorizace služby App Service nakonfigurované založené na cloudu a vašeho klienta musí mít zadaný jako hostitele alternativního přihlašovacího hello hello koncového bodu cloudu. Vaše klientská platforma hello konkrétní kroky potřebné naleznete v dokumentaci hello.

Zajistěte, aby mobilního back-endu [Microsoft.Azure.Mobile.Server.Authentication] nainstalována. Potom v třídy pro spuštění OWIN vaší aplikace, přidat následující hello po `MobileAppConfiguration` bylo použité tooyour `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

V předchozím příkladu hello, měli byste nakonfigurovat hello *authAudience* a *authIssuer* nastavení aplikace v souboru Web.config souboru tooeach být adresa URL kořenového adresáře aplikace pomocí hello HTTPS schéma. Podobně byste měli nastavit *authSigningKey* toobe hello hodnotu vaší aplikace je podpisový klíč.
tooobtain hello podpisový klíč:

1. Přejděte tooyour aplikace v rámci hello [portál Azure]
2. Klikněte na tlačítko **nástroje**, **Kudu**, **přejděte**.
3. V lokalitě hello Kudu správy, klikněte na tlačítko **prostředí**.
4. Najít hodnotu hello *webu\_AUTH\_PODPISOVÁNÍ\_klíč*.

Použití hello podpisového klíče pro hello *authSigningKey* parametr v konfiguraci vaší místní aplikace.  Mobilního back-endu je nyní vybavené toovalidate tokeny při spuštění místně, které hello klient obdrží hello tokenu z koncového bodu cloudu hello.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[portál Azure]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
