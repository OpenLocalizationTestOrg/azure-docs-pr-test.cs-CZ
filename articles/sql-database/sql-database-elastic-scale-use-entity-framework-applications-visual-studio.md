---
title: "Klientská knihovna pro aaaUsing elastické databáze s platformou Entity Framework | Microsoft Docs"
description: "Pomocí klientské knihovny pro elastické databáze a Entity Framework pro kódování databází"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: b9c3065b-cb92-41be-aa7f-deba23e7e159
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: torsteng
ms.openlocfilehash: 917f6d28d9855c0b42afe2c008613a9bbb3ec6b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Klientská knihovna pro elastické databáze s platformou Entity Framework
Tento dokument ukazuje hello změny v aplikaci rozhraní Entity Framework, které jsou potřebné toointegrate s hello [nástroje elastické databáze](sql-database-elastic-scale-introduction.md). Hello zaměřuje se na skládání [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md) a [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md) s hello Entity Framework **Code First** přístup. Hello [Code nejprve - novou databázi](http://msdn.microsoft.com/data/jj193542.aspx) kurz pro EF slouží jako našem příkladu spuštěné v tomto dokumentu. Ukázkový kód Hello doplňujícími tento dokument je součástí nástroje elastické databáze sada ukázky v hello Visual Studio – ukázky kódu.

## <a name="downloading-and-running-hello-sample-code"></a>Stažení a spuštění hello ukázkový kód
toodownload hello kód v tomto článku:

* Visual Studio 2012 nebo novější je povinný. 
* Stáhnout hello [elastické databáze nástroje pro Azure SQL – ukázka integrace Entity Framework](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) z webu MSDN. Rozbalte hello ukázka tooa umístění dle vlastního výběru.
* Spusťte Visual Studio. 
* V sadě Visual Studio vyberte soubor -> Otevřít projekt nebo řešení. 
* V hello **otevřeného projektu** dialogové okno, přejděte toohello ukázka jste stáhli a vyberte **EntityFrameworkCodeFirst.sln** tooopen hello ukázka. 

Ukázka hello toorun, je třeba toocreate tři prázdné databáze ve službě Azure SQL Database:

* Horizontálního oddílu mapa správce databáze
* Databáze horizontálního oddílu 1
* Databáze horizontálního oddílu 2

Po vytvoření těchto databází, vyplňte hello zástupného v **Program.cs** se název serveru Azure SQL DB, hello názvy databáze a databáze toohello tooconnect přihlašovací údaje. Vytvoření hello řešení v sadě Visual Studio. Visual Studio stáhne hello požadované balíčky NuGet pro klientské knihovny pro elastické databáze hello Entity Framework a přechodná chyba zpracování jako součást procesu sestavení hello. Ujistěte se, že probíhá obnovení balíčků NuGet je povolena pro vaše řešení. Toto nastavení můžete povolit kliknutím pravým tlačítkem na soubor řešení hello v hello Průzkumníka řešení Visual Studio. 

## <a name="entity-framework-workflows"></a>Pracovní postupy Entity Framework
Vývojáři Entity Framework spoléhají na jednom z hello následující čtyři pracovních toobuild aplikace a trvalost tooensure pro objekty aplikací: 

* **Code First (nová databáze)**: hello EF vývojáře vytvoří v kódu aplikace hello hello model a pak EF generuje hello databáze z něj. 
* **Code First (existující databáze)**: hello vývojáře umožňuje EF generování kódu aplikace hello hello modelu z existující databáze.
* **Model první**: hello vývojáře vytvoří hello model v EF designeru hello a pak EF vytvoří hello databázi z modelu hello.
* **Databáze první**: hello developer používá EF tooling tooinfer hello modelu z existující databáze. 

Spravovat všechny tyto přístupy spoléhají na tootransparently třídy DbContext hello připojení k databázi a schéma databáze pro aplikaci. Jak se budeme zabývat podrobněji později v dokumentu hello jiné konstruktory na základní třídy DbContext hello povolit pro různé úrovně kontroly nad vytváření připojení, databáze vytváření zavádění a schéma. Problémy jsou vyvolány především hello skutečnost, že správa připojení databáze hello poskytovaná v rámci EF protíná s hello data závislé směrování rozhraní poskytuje možnosti správy připojení hello pomocí klientské knihovny pro elastické databáze hello. 

## <a name="elastic-database-tools-assumptions"></a>Předpoklady nástroje elastické databáze
Definice podmínek, najdete v části [Glosář nástroje elastické databáze](sql-database-elastic-scale-glossary.md).

Klientská knihovna pro elastické databáze definovat oddíly názvem shardlets data aplikací. Shardlets jsou identifikovány klíč horizontálního dělení a jsou namapované toospecific databáze. Aplikace může mít libovolný počet databází, podle potřeby a distribuci hello shardlets tooprovide dost kapacity nebo výkonu zadána aktuální obchodní požadavky. mapování Hello horizontálního dělení hodnoty klíče toohello databází ukládá horizontálního oddílu mapu poskytované hello elastické databáze klientských rozhraní API. Říkáme tato funkce **horizontálního oddílu mapy správu**, nebo pro zkrácení SMM. mapování horizontálních Hello slouží také jako hello zprostředkovatele připojení k databázi pro požadavky, které zajišťují klíč horizontálního dělení. Označujeme toothis funkce jako **závislé na data směrování**. 

správce mapy horizontálního oddílu Hello chrání uživatelé z nekonzistentní zobrazení do shardlet data, která může dojít, když se děje souběžných shardlet operace správy (například přemístění dat z jedné horizontálních tooanother). toodo tedy hello horizontálního oddílu mapy spravuje hello knihovně zprostředkovatele hello databáze připojení klienta pro aplikaci. To umožňuje hello horizontálního oddílu mapy funkce tooautomatically kill připojení k databázi při operacích správy horizontálního oddílu by mohlo mít vliv hello shardlet, který byl vytvořen hello připojení pro. Tento postup musí toointegrate s některými EF na funkce, jako je například vytváření nových připojení z existující jeden toocheck existence databáze. Obecně platí, naše pozorování bylo, že standardní konstruktory DbContext hello pouze fungovat spolehlivě pro připojení uzavřené databáze, které se dají bezpečně klonovat pro EF práci. Princip návrhu Hello elastické databáze místo toho je tooonly zprostředkovatele otevřít připojení. Může být jeden vezměte v úvahu uzavřením připojení zprostředkované pomocí klientské knihovny hello před blokováním přes toohello EF DbContext může vyřešit tento problém. Ale zavřením hello připojení a spoléhat na EF toore otevřete ho, jeden foregoes kontroly ověřování a konzistence hello provádí hello knihovně. funkce migrace Hello v EF, ale používá tyto hello toomanage připojení základní schéma databáze tak, že je transparentní toohello aplikace. V ideálním případě by jsme jako tooretain a kombinace všechny tyto funkce z klientské knihovny pro elastické databáze hello i EF v hello stejná aplikace. Hello následující část popisuje tyto vlastnosti a požadavky podrobněji. 

## <a name="requirements"></a>Požadavky
Při práci s hello klientské knihovny pro elastické databáze a Entity Framework rozhraní API, chceme tooretain hello následující vlastnosti: 

* **Škálováním na více systémů**: tooadd nebo odebrat databáze z hello datové vrstvy hello horizontálně dělené aplikace podle potřeby pro požadavky na kapacitu hello aplikace hello. To znamená kontrolu nad hello hello vytváření a odstraňování databází a databází toomanage rozhraní API map hello elastické databáze horizontálního oddílu manager a mapování shardlets jeho použití. 
* **Konzistence**: aplikace hello používá horizontálního dělení a používá hello závislé směrování funkce dat Klientská knihovna pro hello. tooavoid poškození nebo výsledky dotazu nesprávný připojení jsou zprostředkované přes správce mapy hello horizontálního oddílu. Zachová také ověření a konzistence.
* **Code First**: tooretain hello pohodlím, které představuje první zlepší EF na kódu. V Code First jsou třídy v aplikaci hello namapované transparentně toohello základní struktury databáze. kód aplikace Hello komunikuje s DbSets, který maskování většinu aspektů hello základní zpracování databáze.
* **Schéma**: rozhraní Entity Framework zpracovává vytvoření schématu počáteční databáze a následné schématu vývoj pomocí migrace. Zachováním tyto funkce, je snadné jako hello zpracovaní dat přizpůsobení vaší aplikace. 

Hello pokynů dá pokyn, jak toosatisfy tyto požadavky pro Code First aplikací pomocí nástroje elastické databáze. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Data závislé směrování pomocí EF DbContext
Databázová připojení s platformou Entity Framework se obvykle spravují prostřednictvím měly podtřídy **DbContext**. Vytvořit tyto podtřídy odvozené z **DbContext**. Toto je, kde můžete definovat vaše **DbSets** které implementují hello databáze zálohována kolekce objektů CLR pro vaši aplikaci. V kontextu hello data závislé směrování abychom mohli identifikovat několik užitečné vlastnosti, které nemají nutně další EF code první aplikaci scénáře: 

* Hello databáze již existuje a je zaregistrován v mapě horizontálního oddílu hello elastické databáze. 
* schéma Hello hello aplikace je již nasazené toohello databáze (vysvětlení níže). 
* Jsou závislé na data směrování připojení toohello databáze zprostředkované podle hello horizontálního oddílu mapy. 

toointegrate **DbContexts** s závislé na data směrování pro Škálováním na více systémů:

1. Vytvořit fyzická databáze připojení prostřednictvím rozhraní klienta elastické databáze hello hello horizontálního oddílu mapa správce, 
2. Zabalení hello připojení s hello **DbContext** podtřídy
3. Předat připojení hello do hello **DbContext** základní třídy tooensure všechny hello zpracování na straně EF hello se také stane. 

Hello následující příklad kódu ukazuje tento přístup. (Tento kód se taky v hello doplňujícími projektu sady Visual Studio)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed toohello proper shard by hello shard map manager. 
        // Note that hello base class c'tor call will fail for an open connection
        // if migrations need toobe done and SQL credentials are used. This is hello reason for hello 
        // separation of c'tors into hello data-dependent routing case (this c'tor) and hello internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map toobroker a validated connection for hello given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Hlavní body
* Nový konstruktor nahrazuje hello výchozí konstruktor v podtřídami DbContext hello 
* new – konstruktor Hello trvá hello argumenty, které jsou požadovány pro závislé směrování dat přes klientské knihovny pro elastické databáze:
  
  * Hello horizontálního oddílu mapy tooaccess hello rozhraní směrování závislé na data
  * Hello horizontálního dělení klíče tooidentify hello shardlet,
  * připojovací řetězec s hello přihlašovací údaje pro hello závislé na data směrování připojení toohello horizontálních. 
* konstruktor základní třídy toohello volání Hello bere detour v statickou metodu, která provádí všechny kroky hello potřebné pro směrování závislé na data. 
  
  * Hello OpenConnectionForKey volání rozhraní klienta elastické databáze hello používá na hello horizontálního oddílu mapy tooestablish otevřené připojení.
  * mapování horizontálních Hello vytvoří horizontálního hello otevřené připojení toohello oddílu, který obsahuje hello shardlet pro zadaný klíč horizontálního dělení hello.
  * Toto otevřené připojení je předán zpět toohello základní třída konstruktoru DbContext tooindicate, aby toto připojení není toobe používané EF místo EF automaticky vytvořit nové připojení. Toto připojení hello způsob, jak má byla označené klientem elastické databáze hello rozhraní API, tak, aby ho může zaručit konzistenci v rámci operace správy mapy horizontálního oddílu.

Použijte konstruktor nové hello podtřídy DbContext místo hello výchozí konstruktor v kódu. Zde naleznete příklad: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

new – konstruktor Hello otevře hello horizontálních toohello připojení, která obsahuje data hello pro hello shardlet identifikovaný hello hodnotu **tenantid1**. Hello kód v hello **pomocí** bloku zůstává beze změny tooaccess hello **DbSet** pro blogy pomocí EF na hello horizontálního oddílu pro **tenantid1**. Tato operace změní sémantiku pro hello kód v hello použití bloku tak, že všechny databázové operace jsou nyní obor jeden horizontálního oddílu toohello kde **tenantid1** je uložen. Například dotaz LINQ přes hello blogy **DbSet** by vrátit pouze uložené na aktuální horizontálního oddílu hello blogy, ale není hello těch, které jsou uložené na jiné horizontálních oddílů.  

#### <a name="transient-faults-handling"></a>Zpracování přechodné chyby
Hello postupy společnosti Microsoft Patterns team publikované hello [hello přechodné chyby zpracování bloku aplikace](https://msdn.microsoft.com/library/dn440719.aspx). Hello knihovně se používá v kombinaci s EF Klientská knihovna pro elastické škálování. Však zajistěte, že všechny přechodný výjimka vrátí tooa místě, kde jsme můžete zajistit, že tento nový konstruktor hello je používána po přechodná chyba tak, aby všechny nové připojení pokus se uskuteční pomocí hello konstruktory, které jsme mít tweaked. Správné není zaručena horizontálního oddílu, a neexistují žádné záruky hello připojení připojení toohello, jinak hodnota zachovaný jako změny dojít toohello horizontálního oddílu mapy. 

Hello následující příklad kódu ukazuje, jak zásady opakování SQL lze použít kolem hello nové **DbContext** podtřídami konstruktory: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** v hello výše uvedený kód je definovaný jako **SqlDatabaseTransientErrorDetectionStrategy** s počtem opakování 10 a 5 sekund čekací dobu mezi opakovanými pokusy. Tento přístup je podobné toohello pokyny pro EF a uživatel spustil transakce (viz [omezení s opakováním strategie provádění (EF6 a vyšší)](http://msdn.microsoft.com/data/dn307226). Obě situace vyžadují tohoto programu hello aplikace řídí hello oboru toowhich hello přechodný výjimka vrátí: tooeither znovu otevřete hello transakce, nebo (jak je znázorněno) vytvořte znovu hello kontext z hello správné konstruktor, používá hello elastické databáze Klientská knihovna.

Hello toocontrol třeba kde přechodné výjimky trvat nám zpět v oboru také vylučuje hello použití předdefinované hello **SqlAzureExecutionStrategy** dodávaný s EF. **SqlAzureExecutionStrategy** by znovu otevřít připojení, ale nechcete použít **OpenConnectionForKey** a proto všechny hello ověření, které se provádí v rámci hello obejít **OpenConnectionForKey** volání. Místo toho ukázka kódu hello používá integrované hello **DefaultExecutionStrategy** také dodávaný s EF. Na rozdíl od svazků příliš**SqlAzureExecutionStrategy**, funguje správně v kombinaci s hello zásady opakování z přechodných chyb. Zásady spouštění Hello je nastavena v hello **ElasticScaleDbConfiguration** třídy. Všimněte si, že jsme se rozhodli není toouse **DefaultSqlExecutionStrategy** vzhledem k tomu, že ho navrhuje toouse **SqlAzureExecutionStrategy** Pokud dojde k přechodné výjimky - který vede toowrong chování, jak je popsáno. Další informace o různých opakování zásady hello a EF najdete v tématu [odolnost připojení v EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktor přepisů
Hello výše uvedené příklady kódu ilustrují hello výchozí konstruktor znovu zapíše potřebné pro vaši aplikaci v pořadí toouse datech závislé směrování s hello Entity Framework. Následující tabulka Hello umožňuje zobecnit konstruktory tooother tento přístup. 

| Aktuální – konstruktor | Přepsaná konstruktor pro data | Základní – konstruktor | Poznámky |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |Hello připojení musí toobe funkce hello horizontálního oddílu mapy a hello závislé na data směrování klíče. Vytvoření automatického připojení k průchodu tooby správcem EF a místo toho pomocí hello horizontálního oddílu mapy toobroker hello připojení. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |připojení Hello je funkce hello horizontálního oddílu mapy a hello závislé na data směrování klíče. Pevné databázové název nebo připojovací řetězec nebude fungovat jako jejich obejít ověřování pomocí mapování horizontálních hello. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logická hodnota) |Hello připojení budou vytvořeny pro zadaný klíč mapy a horizontálního dělení horizontálního oddílu s modelem hello poskytuje hello. kompilované modelu Hello se předají na základní c'tor toohello. |
| MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap TKey, logická hodnota) |DbContext (DbConnection, bool) |Hello připojení musí toobe odvodit z mapy hello horizontálního oddílu a klíč hello. Jako vstup nemůže být zadaný (Pokud je tento vstup byl již pomocí mapy hello horizontálního oddílu a klíč hello). Hello logickou hodnotu, budou předány. |
| MyContext (string, DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logická hodnota) |Hello připojení musí toobe odvodit z mapy hello horizontálního oddílu a klíč hello. Jako vstup nemůže být zadaný (Pokud je tento vstup byl pomocí mapy hello horizontálního oddílu a klíč hello). kompilované modelu Hello se předají. |
| MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |new – konstruktor Hello musí tooensure, který jakékoli připojení, v hello ObjectContext předán jako vstup je znovu směrované tooa připojení spravuje elastické škálování. Podrobnou diskuzi o ObjectContexts je nad rámec tohoto dokumentu hello. |
| MyContext (DbConnection, DbCompiledModel, logická hodnota) |ElasticScaleContext (ShardMap, bool TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, logická hodnota); |Hello připojení musí toobe odvodit z mapy hello horizontálního oddílu a klíč hello. Hello připojení nelze zadat jako vstup (Pokud je tento vstup byl již pomocí mapy hello horizontálního oddílu a klíč hello). Model a logickou hodnotu jsou předány na toohello – základní třída – konstruktor. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>Nasazení schématu horizontálního oddílu pomocí EF migrace
Správa automatického schématu je pro vaše pohodlí poskytované hello Entity Framework. V kontextu hello aplikací pomocí nástroje elastické databáze chceme tooretain této schopnosti tooautomatically zřídit hello schématu toonewly vytvořit horizontálních oddílů po přidání aplikace horizontálně dělené toohello databáze. případem primárního použití Hello je kapacita tooincrease na datové vrstvě hello pro horizontálně dělenou aplikací s využitím EF. S horizontálně dělené aplikací postavené na EF spoléhat na EF na funkce pro správu schématu snižuje hello databáze správy úsilí. 

Nasazení schématu pomocí migrace EF funguje nejlépe na **nebyla otevřena připojení**. Toto je na rozdíl od scénář toohello pro data závislé směrování, které jsou závislé na hello otevřít připojení poskytované hello elastické databáze klientského rozhraní API. Další rozdíl je požadavek konzistence hello: při žádoucí tooensure konzistence pro všechny závislé na data směrování připojení tooprotect proti manipulaci mapy souběžných horizontálního oddílu, se nejedná o problém s novou databází tooa počáteční schématu nasazení který má ještě není zaregistrována v mapě hello horizontálního oddílu a ještě není byl přidělen toohold shardlets. Jsme můžete proto spoléhají na standardní databázi připojení pro tento scénáře, jako názvem na rozdíl od směrování toodata závislé.  

To vede tooan přístup, kde nasazení schématu pomocí EF migrace je úzce spojeny s hello registrace nové databáze hello jako horizontálního oddílu v mapě horizontálního oddílu aplikace hello. To závisí na hello následující požadavky: 

* Hello databáze již existuje. 
* Hello databáze je prázdná – drží žádné schéma uživatele a žádná uživatelská data.
* Hello databáze nelze ještě přistupovat prostřednictvím rozhraní API hello elastické databáze klienta pro směrování závislé na data. 

Tyto požadavky splněny, můžeme vytvořit běžný zrušení otevřenou **SqlConnection** tookick vypnout EF migrace pro nasazení schématu. Následující ukázka kódu Hello ukazuje tento přístup. 

        // Enter a new shard - i.e. an empty database - toohello shard map, allocate a first tenant tooit  
        // and kick off EF intialization of hello database toodeploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext tootrigger migrations and schema deployment for hello new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query tooengage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register hello mapping of hello tenant toohello shard in hello shard map. 
            // After this step, data-dependent routing on hello shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 


Tento příklad ukazuje hello metoda **RegisterNewShard** , registry hello horizontálního oddílu v mapě hello horizontálního oddílu, nasadí hello schématu prostřednictvím EF migrace a ukládá mapování horizontálních klíče toohello horizontálního dělení. Přitom spoléhá na konstruktoru hello **DbContext** podtřídami (**ElasticScaleContext** v ukázce hello), která má jako vstup připojovací řetězec SQL. Kód Hello tento konstruktor je jednoduché, jako následující příklad ukazuje hello: 

        // C'tor toodeploy schema and migrations tooa new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that hello schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 

Jeden použili hello verzi hello konstruktor zděděn ze základní třídy hello. Ale hello tooensure potřebám kód, který hello výchozí inicializátoru pro EF se používá při připojování. Proto hello krátké detour do hello statickou metodu před voláním do konstruktoru základní třídy hello s hello připojovací řetězec. Všimněte si, že hello registrace horizontálních oddílů měly být spuštěny v jiné aplikace domény nebo proces tooensure které hello inicializátoru nastavení pro EF nejsou v konfliktu. 

## <a name="limitations"></a>Omezení
přístupy Hello uvedených v tomto dokumentu za následek několik omezení: 

* EF aplikace, které používají **LocalDb** nejprve toomigrate tooa standardní databázi systému SQL Server před použitím klientské knihovny pro elastické databáze. Horizontální navýšení kapacity aplikace prostřednictvím horizontálního dělení s elastickým Škálováním není možné pomocí **LocalDb**. Všimněte si, že vývoj můžete nadále používat **LocalDb**. 
* Jakékoli změny toohello aplikace, která implikují změny schématu databáze potřebovat toogo prostřednictvím EF migrace na všechny horizontálních oddílů. Hello ukázkový kód pro tento dokument není ukazují, jak toodo to. Zvažte použití Update-Database s ConnectionString parametr tooiterate přes všechny horizontálních oddílů; nebo extrakce hello T-SQL skriptu pro hello čeká na migraci pomocí Update-Database s hello - možnost skript a použijte hello T-SQL skriptu tooyour horizontálních oddílů.  
* Zadaný požadavek, se předpokládá, že určeno klíčem horizontálního dělení hello poskytované hello žádost, všechny její zpracování databáze je obsažena v jedné horizontálního oddílu. Však tento předpoklad vždy nemá hodnotu true. Například když není možné toomake horizontálního dělení klíč, který je k dispozici. tooaddress se hello klientské knihovny poskytuje hello **MultiShardQuery** třídu, která implementuje abstraktní připojení pro dotazování přes několik horizontálních oddílů. Učení toouse hello **MultiShardQuery** v kombinaci s EF je nad rámec tohoto dokumentu hello

## <a name="conclusion"></a>Závěr
EF aplikace může prostřednictvím hello kroků uvedených v tomto dokumentu, použít schopností hello elastické databáze klientské knihovny pro data závislé směrování podle refaktoring konstruktory hello **DbContext** podtřídy použít v hello EF aplikace. Toto omezení hello změny požadované toothose umístí kde **DbContext** třídy již existují. Kromě toho EF aplikace můžete pokračovat toobenefit z nasazení automatické schéma kombinací hello kroky, které vyvolání hello nezbytné EF migrace s registrací hello nové horizontálních oddílů a mapování v mapě hello horizontálního oddílu. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
