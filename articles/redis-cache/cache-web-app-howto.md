---
title: "aaaHow toocreate webové aplikace s Redis Cache | Microsoft Docs"
description: "Zjistěte, jak toocreate webové aplikace s Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 05/09/2017
ms.author: sdanie
ms.openlocfilehash: d3e6df97b06fdf9032570dc360944be4bd7715de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-web-app-with-redis-cache"></a>Jak toocreate webové aplikace s Redis Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Tento kurz ukazuje, jak toocreate a nasazení webové aplikace tooa webové aplikace ASP.NET ve službě Azure App Service pomocí Visual Studio 2017. Hello ukázková aplikace zobrazí seznam týmových statistik z databáze a uvádí různé způsoby toouse Azure Redis Cache toostore a načtení dat z mezipaměti hello. Po dokončení kurzu hello budete mít funkční webovou aplikaci, která čte a zapisuje tooa databáze, optimalizované s Azure Redis Cache a je hostovaná v Azure.

Naučíte se:

* Jak toocreate ASP.NET MVC 5 webovou aplikaci v sadě Visual Studio.
* Jak tooaccess dat z databáze používající rozhraní Entity Framework.
* Jak tooimprove propustnost dat a snížit zatížení databáze díky ukládání a načítání dat pomocí Azure Redis Cache.
* Jak toouse Redis seřazené sady tooretrieve hello top 5 týmů.
* Jak tooprovision hello prostředků Azure pro aplikace hello pomocí šablony Resource Manageru.
* Jak toopublish hello tooAzure aplikace pomocí sady Visual Studio.

## <a name="prerequisites"></a>Požadavky
toocomplete hello kurz, musíte mít hello následující požadavky.

* [Účet Azure](#azure-account)
* [Visual Studio 2017 s hello Azure SDK for .NET](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Účet Azure
Budete potřebovat účtu Azure toocomplete hello kurzu. Můžete:

* [Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Získáte kredity, které se dají použít tootry na placené služby Azure. I po hello kredity vyčerpáte, můžete zachovat hello účet a používat bezplatné služby Azure a funkce.
* [Aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Díky předplatnému MSDN každý měsíc získáváte kredity, které můžete použít pro placené služby Azure.

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a>Visual Studio 2017 s hello Azure SDK for .NET
Hello kurz je napsán pro Visual Studio 2017 s hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools). Hello Azure SDK 2.9.5 je součástí instalačního programu sady Visual Studio hello.

Pokud máte Visual Studio 2015, můžete postupovat podle kurzu hello s hello [Azure SDK for .NET](../dotnet-sdk.md) ve verzi 2.8.2 nebo novější. [Stažení hello nejnovější sadu Azure SDK pro Visual Studio 2015 tady](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio se automaticky nainstaluje s hello SDK, pokud ještě nemáte ho. Některé obrazovky se mohou lišit od ilustrací hello v tomto kurzu.

Pokud máte Visual Studio 2013, můžete [stažení hello nejnovější sadu Azure SDK pro Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Některé obrazovky se mohou lišit od ilustrací hello v tomto kurzu.

## <a name="create-hello-visual-studio-project"></a>Vytvoření projektu Visual Studio hello
1. Otevřete sadu Visual Studio a klikněte na **Soubor**, **Nový**, **Projekt**.
2. Rozbalte hello **Visual C#** uzel v hello **šablony** seznamu, vyberte **cloudu**a klikněte na tlačítko **webové aplikace ASP.NET**. Ujistěte se, že je vybrán **.NET Framework 4.5.2** nebo vyšší.  Typ **ContosoTeamStats** do hello **název** textové pole a klikněte na tlačítko **OK**.
   
    ![Vytvoření projektu][cache-create-project]
3. Vyberte **MVC** jako typ projektu hello. 

    Ujistěte se, že **bez ověřování** je zadán pro hello **ověřování** nastavení. V závislosti na vaší verzi sady Visual Studio může být hello výchozí nastavení toosomething else. toochange, klikněte na tlačítko **změna ověřování** a vyberte **bez ověřování**.

    Pokud postupujete podle společně s Visual Studio 2015, zrušte hello **hostitel v cloudu hello** zaškrtávací políčko. Budete [zřídit hello prostředků Azure](#provision-the-azure-resources) a [publikování tooAzure aplikace hello](#publish-the-application-to-azure) v dalších krocích hello kurzu. Příklad zřízení webové aplikace služby App Service ze sady Visual Studio ponecháním **hostitel v cloudu hello** najdete v tématu [Začínáme s webovými aplikacemi ve službě Azure App Service pomocí ASP.NET a Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).
   
    ![Výběr šablony projektu][cache-select-template]
4. Klikněte na tlačítko **OK** toocreate hello projektu.

## <a name="create-hello-aspnet-mvc-application"></a>Vytvoření hello aplikace ASP.NET MVC
V této části kurzu hello vytvoříte hello základní aplikaci, která načítá a zobrazuje týmové statistiky z databáze.

* [Přidání balíčku Entity Framework NuGet hello](#add-the-entity-framework-nuget-package)
* [Přidání modelu hello](#add-the-model)
* [Přidat řadič hello](#add-the-controller)
* [Konfigurace zobrazení hello](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a>Přidání balíčku Entity Framework NuGet hello

1. Klikněte na tlačítko **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.
2. Spuštění hello následující příkaz z hello **Konzola správce balíčků** okno.
    
    ```
    Install-Package EntityFramework
    ```

Další informace o tomto balíčku najdete v tématu hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet stránky.

### <a name="add-hello-model"></a>Přidání modelu hello
1. V **Průzkumníku řešení** klikněte pravým tlačítkem na **Modely** a vyberte **Přidat**, **Třídu**. 
   
    ![Přidání modelu][cache-model-add-class]
2. Zadejte `Team` pro název třídy hello a klikněte na tlačítko **přidat**.
   
    ![Přidání třídy modelu][cache-model-add-class-dialog]
3. Nahraďte hello `using` příkazy hello horní části hello `Team.cs` soubor s následující hello `using` příkazy.

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. Nahraďte definici hello hello `Team` se hello následující fragment kódu, který obsahuje kromě aktualizované `Team` třídy definice, jakož i některé další pomocné třídy Entity Frameworku. Další informace o hello kód první přístup tooEntity Framework, který se používá v tomto kurzu, najdete v části [kód první tooa novou databázi](https://msdn.microsoft.com/data/jj193542).

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. V **Průzkumníku řešení**, dvakrát klikněte na **web.config** tooopen ho.
   
    ![Soubor web.config][cache-web-config]
2. Přidejte následující hello `connectionStrings` části. Název Hello hello připojovací řetězec musí odpovídat názvu hello hello třídy kontextu databáze Entity Framework, který je `TeamContext`.

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Můžete přidat nové hello `connectionStrings` tak, aby postupuje `configSections`, jak ukazuje následující příklad hello.

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

    > [!NOTE]
    > Připojovací řetězec se může lišit v závislosti na verzi hello sady Visual Studio a edice systému SQL Server Express používat toocomplete hello kurzu. Hello web.config šablona by měla být nakonfigurovaná toomatch instalaci a může obsahovat `Data Source` jako položky `(LocalDB)\v11.0` (z SQL serveru Express 2012) nebo `Data Source=(LocalDB)\MSSQLLocalDB` (z SQL serveru Express 2014 a novější). Další informace o připojovacích řetězcích a verzích SQL Express najdete v tématu [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).

### <a name="add-hello-controller"></a>Přidat řadič hello
1. Stiskněte klávesu **F6** toobuild hello projektu. 
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **řadiče** složky a vyberte **přidat**, **řadič**.
   
    ![Přidání kontroleru][cache-add-controller]
3. Vyberte **Kontroler MVC 5 se zobrazeními, s použitím Entity Frameworku**, a klikněte na **Přidat**. Pokud dojde k chybě po kliknutí na **přidat**, ujistěte se, že jste vytvořili projekt hello nejdřív.
   
    ![Přidání třídy kontroleru][cache-add-controller-class]
4. Vyberte **Team (ContosoTeamStats.Models)** z hello **třída modelu** rozevíracího seznamu. Vyberte **TeamContext (ContosoTeamStats.Models)** z hello **třída kontextu dat** rozevíracího seznamu. Typ `TeamsController` v hello **řadič** textového pole název (Pokud není vyplněné automaticky). Klikněte na tlačítko **přidat** toocreate hello třídy kontroleru a přidáte výchozí zobrazení hello.
   
    ![Konfigurace kontroleru][cache-configure-controller]
5. V **Průzkumníku řešení**, rozbalte položku **Global.asax** a dvakrát klikněte na **Global.asax.cs** tooopen ho.
   
    ![Soubor Global.asax.cs][cache-global-asax]
6. Přidejte následující dvě hello `using` příkazy hello horní části souboru hello pod hello jiných `using` příkazy.

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. Přidejte následující řádek kódu na konci hello hello hello `Application_Start` metoda.

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. V **Průzkumníku řešení** rozbalte `App_Start` a dvakrát klikněte na soubor `RouteConfig.cs`.
   
    ![Soubor RouteConfig.cs][cache-RouteConfig-cs]
2. Nahraďte `controller = "Home"` v hello následující kód v hello `RegisterRoutes` metoda s `controller = "Teams"` jak ukazuje následující příklad hello.

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a>Konfigurace zobrazení hello
1. V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku a potom hello **sdílené** složku a dvojím kliknutím **_Layout.cshtml**. 
   
    ![Soubor _Layout.cshtml][cache-layout-cshtml]
2. Změnit obsah hello hello `title` elementu a nahraďte `My ASP.NET Application` s `Contoso Team Stats` jak ukazuje následující příklad hello.

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. V hello `body` část, nejdřív aktualizovat hello `Html.ActionLink` příkaz a nahraďte `Application name` s `Contoso Team Stats` a nahraďte `Home` s `Teams`.
   
   * Před: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Po: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Změny kódu][cache-layout-cshtml-code]
2. Stiskněte klávesu **Ctrl + F5** toobuild a spuštění aplikace hello. Tato verze aplikace hello čte hello výsledky přímo z databáze hello. Poznámka: hello **vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** akce, které byly automaticky přidá toohello aplikace hello **Kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** vygenerované uživatelské rozhraní. V další části kurzu hello hello přidáte Redis Cache toooptimize hello data přístup a poskytnutí dodatečných funkcí aplikaci toohello.

![Startovní aplikace][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a>Konfigurace aplikace toouse hello Redis Cache
V této části kurzu hello budete konfigurovat hello ukázkové aplikace toostore a načítání týmových statistik Contoso z instance Azure Redis Cache pomocí hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) mezipaměti klienta.

* [Konfigurace aplikace toouse hello StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
* [Aktualizaci hello TeamsController třída tooreturn výsledků z mezipaměti hello nebo hello databáze](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Hello vytvořit, upravit, aktualizovat a odstranit toowork metody s mezipamětí hello](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Aktualizovat toowork zobrazení Teams Index hello s mezipamětí hello](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a>Konfigurace aplikace toouse hello StackExchange.Redis
1. Klikněte na tlačítko tooconfigure klientskou aplikaci v sadě Visual Studio pomocí balíčku StackExchange.Redis NuGet, hello **Správce balíčků NuGet**, **Konzola správce balíčků** z hello **nástroje** nabídky.
2. Spuštění hello následující příkaz z hello `Package Manager Console` okno.
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    Hello NuGet balíček stáhne a přidá hello požadované odkazy na sestavení pro vašeho klienta aplikace tooaccess Azure Redis Cache pomocí klienta mezipaměti StackExchange.Redis hello. Pokud dáváte přednost toouse silným názvem verzi hello `StackExchange.Redis` klientské knihovny, instalace hello `StackExchange.Redis.StrongName` balíčku.
3. V **Průzkumníku řešení**, rozbalte položku hello **řadiče** složku a dvojím kliknutím **TeamsController.cs** tooopen ho.
   
    ![Kontroler Teams][cache-teamscontroller]
4. Přidejte následující dvě hello `using` příkazy příliš**TeamsController.cs**.

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. Přidejte následující dvě vlastnosti toohello hello `TeamsController` třídy.

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. Vytvořte soubor v počítači s názvem `WebAppPlusCacheAppSecrets.config` a jeho následné uložení do umístění, které nebudou navázalo kontakt se hello zdrojový kód ukázkovou aplikaci, musí rozhodnout toocheck ji někam registrovat. V tento příklad hello `AppSettingsSecrets.config` soubor se nachází v `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Upravit hello `WebAppPlusCacheAppSecrets.config` souboru a přidejte hello následující obsah. Pokud místní spuštění aplikace hello tyto informace jsou použité tooconnect tooyour Azure Redis Cache instance. Později v kurzu hello budete zřídíte instanci služby Azure Redis Cache a aktualizovat hello mezipaměti jméno a heslo. Pokud neplánujete toorun hello ukázkovou aplikaci místně, můžete přeskočit hello vytvoření tohoto souboru a hello následné kroky, které odkazují hello souboru, protože při nasazení aplikace hello tooAzure načítá informace o připojení hello mezipaměti z aplikace hello nastavení pro hello webovou aplikaci a ne z tohoto souboru. Od hello `WebAppPlusCacheAppSecrets.config` není nasazený tooAzure s vaší aplikací, nebudete ho potřebovat, pokud budete toorun hello aplikace místně.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. V **Průzkumníku řešení**, dvakrát klikněte na **web.config** tooopen ho.
   
    ![Soubor web.config][cache-web-config]
2. Přidejte následující hello `file` atribut toohello `appSettings` element. Pokud jste použili jiný název souboru nebo umístění, nahraďte těmito hodnotami hello ty, které jsou v příkladu hello.
   
   * Před: `<appSettings>`
   * Po: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
   Hello ASP.NET runtime sloučí obsah hello hello externího souboru se hello značek v hello `<appSettings>` elementu. Hello runtime ignoruje atribut souboru hello, pokud nelze nalézt zadaný soubor hello. Vaše tajné klíče (hello připojovací řetězec tooyour mezipaměti) nejsou součástí hello zdrojový kód aplikace hello. Při nasazení vaší webové aplikace tooAzure hello `WebAppPlusCacheAppSecrests.config` soubor nebude možné nasadit (to znamená, co chcete použít). Existuje několik způsobů toospecify těchto tajných klíčů v Azure, a v tomto kurzu jsou konfigurovány automaticky za vás při můžete [zřízení prostředků Azure hello](#provision-the-azure-resources) v dalším kroku kurzu. Další informace o práci s tajnými klíči v Azure najdete v tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat tooASP.NET a Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a>Aktualizaci hello TeamsController třída tooreturn výsledků z mezipaměti hello nebo hello databáze
V této ukázce lze týmové statistiky načíst z databáze hello nebo z mezipaměti hello. Týmové statistiky jsou uloženy v mezipaměti hello jako serializovaný seznam `List<Team>`a také jako seřazená sada pomocí datových typů Redis. Při načítání položek ze seřazené sady můžete načíst část položek, všechny položky nebo provézt dotaz na určité položky. V této ukázce dotazujete hello seřazené sady hello top 5 týmů podle počtu vítězství.

> [!NOTE]
> Není požadováno toostore hello týmové statistiky ve více formátech v mezipaměti hello v pořadí toouse Azure Redis Cache. Tento kurz používá více formátů toodemonstrate některé hello různé způsoby a různé datové typy toocache data můžete použít.
> 
> 

1. Přidejte následující hello `using` příkazy toohello `TeamsController.cs` souboru v horní části hello s hello jiných `using` příkazy.

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. Nahradit aktuální hello `public ActionResult Index()` implementace metod s hello následující implementaci.

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear hello results from hello cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild hello database with sample data.
                RebuildDB();
                break;
        }

        // Measure hello time it takes tooretrieve hello results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve hello top 5 teams from hello sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from hello cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from hello database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add hello elapsed time of hello operation toohello ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. Přidejte následující tři metody toohello hello `TeamsController` třída tooimplement hello `playGames`, `clearCache`, a `rebuildDB` typy akcí z hello switch – příkaz přidali v předchozím fragmentu kódu hello.
   
    Hello `PlayGames` metoda aktualizací hello týmové statistiky tak, že simuluje sezónu her, uloží hello výsledky toohello databáze a vymaže hello teď zastaralých dat z mezipaměti hello.

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hello `RebuildDB` metoda inicializaci hello databáze s hello výchozí sadou týmů, vygeneruje pro ně statistiky, a vymaže hello teď zastaralých dat z mezipaměti hello.

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize hello database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hello `ClearCachedTeams` metoda odebere všechny uložené týmové statistiky z mezipaměti hello.

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. Přidejte následující čtyři metody toohello hello `TeamsController` třída tooimplement hello různých způsobů načítání hello týmové statistiky z mezipaměti hello a hello databáze. Každá z těchto metod vrací `List<Team>` který se následně zobrazí hello zobrazení.
   
    Hello `GetFromDB` metoda čte hello týmové statistiky z databáze hello.
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    Hello `GetFromList` metoda čte hello týmové statistiky z mezipaměti jako serializovaný seznam `List<Team>`. Pokud dojde k neúspěšnému přístupu do mezipaměti, týmové statistiky hello jsou čtení z databáze hello a pak uloženy v mezipaměti hello pro další použití. V této ukázce používáme JSON.NET serializace tooserialize hello .NET objekty tooand z mezipaměti hello. Další informace najdete v tématu [jak toowork s .NET objekty ve službě Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    Hello `GetFromSortedSet` metoda přečte hello týmové statistiky ze seřazené sady v mezipaměti. Pokud dojde k neúspěšnému přístupu do mezipaměti, týmové statistiky hello jsou čtení z databáze hello a uloženy v hello mezipaměti jako seřazená sada.

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding toosorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    Hello `GetFromSortedSetTop5` metoda čte hello top 5 týmů z mezipaměti hello Seřazená sada. Začne tím hello mezipaměti hello existenci hello `teamsSortedSet` klíč. Pokud tento klíč neexistuje, hello `GetFromSortedSet` metoda je volána tooread hello týmové statistiky a uložit je do mezipaměti hello. V dalším kroku hello seřazené sady v mezipaměti je dotazován na hello top 5 týmů, které jsou vráceny.

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load hello entire sorted set into hello cache.
            GetFromSortedSet();

            // Retrieve hello top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get hello top 5 teams from hello sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a>Hello vytvořit, upravit, aktualizovat a odstranit toowork metody s mezipamětí hello
Kód generování uživatelského rozhraní Hello, který byl vytvořen jako součást této ukázky obsahuje metody tooadd, upravovat a odstraňovat týmy. Kdykoliv tým přidat, upravit nebo odebrat, hello data v hello mezipaměti stanou zastaralými. V této části, upravte tyto tři metody tooclear hello do mezipaměti týmy tak, aby hello mezipaměť nebude synchronizována s databází hello.

1. Procházet toohello `Create(Team team)` metoda v hello `TeamsController` třídy. Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.

    ```c#
    // POST: Teams/Create
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. Procházet toohello `Edit(Team team)` metoda v hello `TeamsController` třídy. Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.

    ```c#
    // POST: Teams/Edit/5
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. Procházet toohello `DeleteConfirmed(int id)` metoda v hello `TeamsController` třídy. Přidejte volání toohello `ClearCachedTeams` metoda, jak ukazuje následující příklad hello.

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, hello cache is out of date.
        // Clear hello cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a>Aktualizovat toowork zobrazení Teams Index hello s mezipamětí hello
1. V **Průzkumníku řešení**, rozbalte položku hello **zobrazení** složku a potom hello **týmy** složku a dvojím kliknutím **Index.cshtml**.
   
    ![Soubor Index.cshtml][cache-views-teams-index-cshtml]
2. V horní hello hello souboru vyhledejte následující element odstavce hello.
   
    ![Tabulka akcí][cache-teams-index-table]
   
    Toto je odkaz toocreate hello nový tým. Nahraďte element odstavce hello hello následující tabulka. Tato tabulka obsahuje odkazy na akce pro vytvoření nového týmu, hraní nové sezóny her, vymazání mezipaměti hello, načítání hello týmů z mezipaměti hello v různých formátech, načtení týmů hello z hello databáze a znovu sestavit hello databáze s novými vzorovými daty.

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. Posuv toohello dolní části hello **Index.cshtml** souboru a přidejte následující hello `tr` element tak, aby se hello hello posledním řádkem poslední tabulky v souboru hello.
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    Tento řádek zobrazuje hodnotu hello `ViewBag.Msg` obsahující zprávu o aktuální operace hello stavu. Hello `ViewBag.Msg` je nastaven, po kliknutí na tlačítko všechny odkazy na akce hello hello v předchozím kroku.   
   
    ![Zpráva o stavu][cache-status-message]
2. Stiskněte klávesu **F6** toobuild hello projektu.

## <a name="provision-hello-azure-resources"></a>Zřízení prostředků Azure hello
toohost aplikaci v Azure, musíte nejdříve zřídit služby Azure, které vaše aplikace vyžaduje hello. Ukázková aplikace Hello v tomto kurzu používá následující služby Azure hello.

* Azure Redis Cache
* Webová aplikace App Service
* SQL Database

toodeploy tyto služby tooa nový nebo existující prostředek skupiny podle svého výběru, klikněte na tlačítko hello následující **nasazení tooAzure** tlačítko.

[! [Nasazení tooAzure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

To **nasazení tooAzure** tlačítko používá hello [vytvoření webové aplikace s Redis Cache a SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) šablony tooprovision těchto služeb a sadu hello připojovací řetězec pro hello SQL Database a hello nastavení aplikace pro hello připojovacího řetězce Azure Redis Cache.

> [!NOTE]
> Pokud ještě nemáte účet Azure, můžete si během několika minut [zdarma vytvořit účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).
> 
> 

Kliknutím na hello **nasazení tooAzure** tlačítko přejdete toohello Azure portal a zahájí hello proces vytváření prostředků hello popsaných hello šablony.

![Nasazení tooAzure][cache-deploy-to-azure-step-1]

1. V hello **Základy** části vyberte hello toouse předplatné Azure a vyberte existující skupinu prostředků nebo vytvořte novou a zadejte umístění skupiny prostředků hello.
2. V hello **nastavení** , určete **přihlášení správce** (nepoužívejte **správce**), **heslo správce**a  **Název databáze**. Hello další parametry jsou nakonfigurovány pro hostování plán a možnosti nižší náklady hello SQL Database a Azure Redis Cache, které nejsou součástí úrovně free bezplatné služby App Service.

    ![Nasazení tooAzure][cache-deploy-to-azure-step-2]

3. Po dokončení konfigurace nastavení hello potřeby, posuňte se konec toohello stránku hello, přečtěte si hello podmínky a ujednání a zkontrolujte hello **souhlasím toohello podmínky a ujednání, které jsou uvedené výše** zaškrtávací políčko.
4. Klikněte na tlačítko toobegin zřizování hello prostředky **nákupu**.

Klikněte na ikonu oznámení hello tooview hello průběh nasazení a klikněte na tlačítko **nasazení začalo**.

![Nasazení začalo][cache-deployment-started]

Hello stav nasazení můžete zobrazit na hello **Microsoft.Template** okno.

![Nasazení tooAzure][cache-deploy-to-azure-step-3]

Jakmile je zřizování dokončeno, můžete publikovat tooAzure vaší aplikace z Visual Studia.

> [!NOTE]
> Na hello se zobrazí nějaké chyby, které mohou nastat během procesu zřizování hello **Microsoft.Template** okno. Mezi běžné chyby patří příliš mnoho SQL Serverů nebo příliš mnoho plánů hostování služby App Service úrovně Free na předplatné. Vyřešte všechny chyby a restartování procesu hello kliknutím **znovu nasaďte** na hello **Microsoft.Template** okno nebo hello **nasazení tooAzure** tlačítko v tomto kurzu.
> 
> 

## <a name="publish-hello-application-tooazure"></a>Publikování aplikace tooAzure hello
V tomto kroku kurzu hello budete publikovat tooAzure aplikace hello a spustíte ho v cloudu hello.

1. Klikněte pravým tlačítkem na hello **ContosoTeamStats** projektu v sadě Visual Studio a zvolte **publikovat**.
   
    ![Publikování][cache-publish-app]
2. Klikněte na **Microsoft Azure App Service**, zvolte **Vybrat existující** a klikněte na **Publikovat**.
   
    ![Publikování][cache-publish-to-app-service]
3. Vyberte předplatné hello použité při vytváření hello prostředky Azure, rozbalte skupinu prostředků hello obsahující hello prostředky, a vyberte hello potřeby webové aplikace. Pokud jste použili hello **nasazení tooAzure** tlačítko název vaší webové aplikace spustí s **webu** následuje některé další znaky.
   
    ![Výběr webové aplikace][cache-select-web-app]
4. Klikněte na tlačítko **OK** toobegin hello procesu publikování. Po chvíli hello proces publikování dokončí a spuštění prohlížeče s hello spuštění ukázkové aplikace. Pokud dojde chybě DNS při ověřování nebo publikování a hello zřizování pro hello prostředků Azure pro aplikace hello má právě k dokončení, počkejte a akci opakujte.
   
    ![Mezipaměť byla přidána][cache-added-to-application]

Hello následující tabulka popisuje každý odkaz na akci z ukázkové aplikace hello.

| Akce | Popis |
| --- | --- |
| Vytvořit nový |Vytvoření nového týmu. |
| Odehrát sezónu |Odehrání sezóny her, aktualizace hello týmových statistik a zrušte všechny zastaralé týmovými daty z mezipaměti hello. |
| Vymazat mezipaměť |Vymazat hello týmových statistik z mezipaměti hello. |
| Seznam z mezipaměti |Načíst hello týmových statistik z mezipaměti hello. Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti pro další použití. |
| Seřazená sada z mezipaměti |Načtení týmových statistik hello z hello mezipaměti jako seřazená sada. Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti jako seřazená sada. |
| Top 5 týmů z mezipaměti |Načtení top 5 týmů hello z hello mezipaměti jako seřazená sada. Pokud dojde k neúspěšnému přístupu do mezipaměti, načtou hello z hello databáze a uloží toohello mezipaměti jako seřazená sada. |
| Načíst z databáze |Hello týmových statistik z databáze získat z hello. |
| Znovu sestavit databázi |Znovu sestavte hello databáze a se vzorovými týmovými daty. |
| Upravit / Podrobnosti / Odstranit |Upravení týmu, zobrazení podrobností o týmu, odstranění týmu. |

Klikněte na některé hello akce a Experimentujte s načítáním dat hello z různých zdrojů hello. Není hello rozdíly v čase hello trvá toocomplete hello různých způsobů načítání hello data z databáze hello a hello mezipaměti.

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a>Odstranit hello prostředky, když jste hotovi s aplikací hello
Po skončení hello ukázkové aplikace, můžete odstranit hello Azure používá v pořadí tooconserve náklady a prostředků. Pokud používáte hello **nasazení tooAzure** tlačítka na hello [hello zřízení prostředků Azure](#provision-the-azure-resources) oddíl a všechny vaše prostředky jsou obsaženy v hello stejnou skupinu prostředků, můžete je odstranit společně v jednom operace odstraněním skupiny prostředků hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **skupiny prostředků**.
2. Název typu hello vaší skupiny prostředků do hello **filtrování položek...**  textové pole.
3. Klikněte na tlačítko **...**  toohello napravo od vaší skupiny prostředků.
4. Klikněte na **Odstranit**.
   
    ![Odstranění][cache-delete-resource-group]
5. Název typu hello vaší skupiny prostředků a klikněte na **odstranit**.
   
    ![Potvrzení odstranění][cache-delete-confirm]

Za pár chvil hello prostředků skupiny obsažených prostředků odstraněna.

> [!IMPORTANT]
> Všimněte si, že odstranění skupiny prostředků je nevratné a příslušné skupině prostředků hello a všechny hello prostředky v ní jsou trvale odstraněny. Ujistěte se, že neodstraníte omylem hello nesprávnou skupinu prostředků nebo prostředky. Pokud jste vytvořili hello prostředky pro hostování této ukázky ve stávající skupinu prostředků, můžete odstranit jednotlivé prostředky jednotlivě z jejich odpovídajících podoken.
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a>Spuštění ukázkové aplikace hello na místním počítači
aplikace hello toorun místně na počítači, potřebujete Azure Redis Cache instance v které toocache vaše data. 

* Pokud vaše aplikace tooAzure jste publikovali, jak je popsáno v předchozí části hello, můžete instanci Azure Redis Cache hello, zřízenou během tohoto kroku.
* Pokud máte jinou stávající instanci Azure Redis Cache, můžete tuto toorun této ukázky místně.
* Pokud potřebujete toocreate instanci služby Azure Redis Cache, můžete provést kroky hello v [vytvoření mezipaměti](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Po vybrání nebo vytvoření mezipaměti toouse hello, procházet toohello mezipaměti hello portál Azure a načíst hello [název hostitele](cache-configure.md#properties) a [přístupové klíče](cache-configure.md#access-keys) ke svojí mezipaměti. Pokyny najdete v oddílu [Konfigurace nastavení mezipaměti Redis](cache-configure.md#configure-redis-cache-settings).

1. Otevřete hello `WebAppPlusCacheAppSecrets.config` soubor, který jste vytvořili během hello [konfigurace toouse aplikace hello Redis Cache](#configure-the-application-to-use-redis-cache) krok v tomto kurzu pomocí zvoleného editoru hello.
2. Upravit hello `value` atribut a nahraďte `MyCache.redis.cache.windows.net` s hello [název hostitele](cache-configure.md#properties) vaší mezipaměti a zadejte buď hello [primární nebo sekundární klíč](cache-configure.md#access-keys) svojí mezipaměti jako hello heslo.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Stiskněte klávesu **Ctrl + F5** toorun hello aplikace.

> [!NOTE]
> Všimněte si, že protože hello aplikaci, včetně hello databáze, je spuštěn místně a hello Redis Cache je hostovaná v Azure, hello mezipaměti může se zobrazit toounder-provést hello databáze. Pro zajištění nejlepšího výkonu hello klientská aplikace a instance služby Azure Redis Cache by měla být v hello stejné umístění. 
> 
> 

## <a name="next-steps"></a>Další kroky
* Další informace o [Začínáme s ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) na hello [ASP.NET](http://asp.net/) lokality.
* Další příklady vytváření webové aplikace ASP.NET ve službě App Service naleznete v tématu [vytvořit a nasadit webové aplikace ASP.NET ve službě Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) z hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
  * Další quickstarts z hello HealthClinic.biz ukázku, najdete v části [Quickstarts nástroje pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
* Další informace o hello [kód první tooa novou databázi](https://msdn.microsoft.com/data/jj193542) přístupu tooEntity Framework, který se používá v tomto kurzu.
* Zjistěte více o [webových aplikacích v Azure App Service](../app-service-web/app-service-web-overview.md).
* Zjistěte, jak příliš[monitorování](cache-how-to-monitor.md) svoji mezipaměť hello portálu Azure.
* Prozkoumejte prémiové funkce Azure Redis Cache
  
  * [Jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md)
  * [Jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md)
  * [Jak podporuje tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md)
  * V tématu hello [Azure Redis Cache – nejčastější dotazy](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) další podrobnosti o velikosti, propustnosti a šířky pásma u prémiových mezipamětí.

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

