---
title: aaaScale na Azure SQL database | Microsoft Docs
description: "Jak toouse hello ShardMapManager klientské knihovny pro elastické databáze"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 4cf670d42ad7ded98fb8d6f0830154587dd2c6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-out-databases-with-hello-shard-map-manager"></a>Horizontální navýšení kapacity databáze s hello horizontálního oddílu mapa správce
tooeasily škálování databáze na SQL Azure, použijte správce mapy horizontálního oddílu. Hello horizontálního oddílu mapa správce je speciální databáze, která udržuje globální mapování informace o všech horizontálních oddílů (databáze) v sadě horizontálního oddílu. Hello metadata umožňuje aplikace tooconnect toohello správnou databázi na základě hello hodnotu hello **horizontálního dělení klíč**. Kromě toho každých horizontálního oddílu v sadě hello obsahuje mapy, které sledují hello místní sdílení dat (označované jako **shardlets**). 

![Správa mapování horizontálních](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Pochopení, jak se vytvářejí tyto mapy je nezbytné tooshard mapy management. To se provádí pomocí hello [ShardMapManager třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx), který se nachází v hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md) toomanage horizontálních mapy.  

## <a name="shard-maps-and-shard-mappings"></a>Mapování horizontálních a horizontálního oddílu mapy
Pro každý horizontálního oddílu je třeba vybrat hello typ toocreate mapy horizontálního oddílu. Volba Hello závisí na architektuře databáze hello: 

1. Jednoho klienta na databázi  
2. Několik klientů na databázi (dva typy):
   1. Seznam mapování
   2. Mapování rozsahu

Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy. model jednoho klienta Hello přiřadí jednu databázi na každého klienta. Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.

![Seznam mapování][1]

Hello víceklientského modelu přiřadí několik klientů tooa jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází). Pomocí tohoto modelu, pokud očekáváte, že je každý klient toohave malá data. V tomto modelu jsme přiřadit rozsah klientů tooa databázi pomocí **rozsah mapování**. 

![Mapování rozsahu][2]

Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* tooassign více klientů tooa jedné databáze. Například DB1 je použité toostore informace o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10. 

![Vnořením klientů na jednom DB][3] 

### <a name="supported-net-types-for-sharding-keys"></a>Podporované typy .net pro klíče horizontálního dělení
Elastické škálování podporu hello následující rozhraní .net Framework typy jako horizontálního dělení klíče:

* celé číslo
* dlouhá
* Identifikátor GUID
* Byte]  
* Data a času
* Časový interval
* Datový typ DateTimeOffset

### <a name="list-and-range-shard-maps"></a>Seznam a rozsah horizontálního oddílu mapy
Mapování horizontálních se dá vytvořit pomocí **seznam jednotlivých horizontálního dělení hodnoty klíče**, nebo se dá vytvořit pomocí **rozsahy horizontálního dělení hodnoty klíče**. 

### <a name="list-shard-maps"></a>Seznam horizontálních mapy
**Horizontálních oddílů** obsahovat **shardlets** a hello mapování shardlets tooshards je možný díky mapu horizontálního oddílu. A **seznam horizontálních mapy** je přidružení mezi hello jednotlivé hodnoty klíče identifikují hello shardlets a hello databázemi, které slouží jako horizontálních oddílů.  **Seznam mapování** jsou explicitní a jiné hodnoty klíče může být namapované toohello stejné databáze. Například klíč 1 mapuje tooDatabase A a B. databáze odkazovat na klíčové hodnoty 3 a 6

| Klíč | Umístění horizontálního oddílu |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| Tlačítka ... |Tlačítka ... |

### <a name="range-shard-maps"></a>Rozsah horizontálního oddílu mapy
V **rozsah horizontálního oddílu mapy**, klíče rozsah hello je popsán pár **[hodnota nízká, vysoká hodnota)** kde hello *nízká hodnota* je hello minimální klíč v rozsahu hello a hello *Vysoké hodnoty* je vyšší než hello rozsah hello první hodnota. 

Například **[0, 100)** zahrnuje všechny celá čísla větší než nebo rovna 0 a menší než 100. Všimněte si, že se podporují více rozsahů může bod toohello stejné databáze a nesouvislé rozsahy (například [100,200) a [400,600) i C tooDatabase bodu v následujícím příkladu hello.)

| Klíč | Umístění horizontálního oddílu |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| Tlačítka ... |Tlačítka ... |

Každý hello tabulky uvedené výše je koncepční příklad **ShardMap** objektu. Každý řádek je zjednodušená příklad jednotlivce **PointMapping** (pro hello seznam horizontálních map) nebo **RangeMapping** (pro mapu horizontálního oddílu rozsah hello) objektu.

## <a name="shard-map-manager"></a>Správce horizontálního oddílu mapy
V hello klientské knihovny správce mapy horizontálního oddílu hello je kolekce horizontálního oddílu mapy. Hello data spravovaná přes **ShardMapManager** instance je udržováno na třech místech: 

1. **Globální horizontálního oddílu mapy (GSM)**: Zadejte tooserve databáze jako hello úložiště pro všechny její horizontálního oddílu mapy a mapování. Speciální tabulek a uložených procedur se automaticky vytvoří toomanage hello informace. To je obvykle s malou databází a lehkým získat přístup, a nesmí být použita pro jiné potřebám aplikace hello. Hello tabulky jsou v speciální schéma s názvem **__ShardManagement**. 
2. **Místní ID horizontálního oddílu mapy (LSM)**: každou databázi, která zadáte toobe horizontálního oddílu je upravena toocontain několik malých tabulky a speciální uložené procedury, které obsahují a spravovat horizontálního oddílu mapy informace o konkrétní toothat horizontálního oddílu. Tyto informace jsou redundantní s informacemi o hello v hello GSM a umožňuje hello informace o mapování horizontálních aplikaci toovalidate do mezipaměti bez uvedení žádné zatížení na hello GSM; aplikace Hello používá hello LSM toodetermine, pokud v mezipaměti mapování je stále platný. Hello tabulky odpovídající toohello LSM na každý horizontálního oddílu jsou i ve schématu hello **__ShardManagement**.
3. **Mezipaměť aplikací**: každý přístup instance aplikace **ShardMapManager** objekt udržuje místní mezipaměť v paměti její mapování. Uloží informace o směrování, která nedávno byla načtena. 

## <a name="constructing-a-shardmapmanager"></a>Vytváření ShardMapManager
A **ShardMapManager** objektu je vytvořený [factory](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx) vzor. Hello  **[ShardMapManagerFactory.GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**  metoda přebírá přihlašovací údaje (včetně hello název serveru a název databáze, která uchovává hello GSM) hello formu  **ConnectionString** a vrací instanci třídy **ShardMapManager**.  

**Poznámka:** hello **ShardMapManager** by měla být vytvořena instance pouze jednou v každé doméně aplikace hello inicializace kódu pro aplikaci. Vytvoření dalších instancí ShardMapManager v hello stejné appdomain způsobí vyšší paměť a využití CPU aplikace hello. A **ShardMapManager** může obsahovat libovolný počet horizontálních mapy. Mapu jeden horizontálního oddílu může být dostatečné pro mnoho aplikací, jsou časy při použití různé sady databází pro jiné schéma nebo pro jedinečné účely; v těchto případech může být vhodnější než více mapami horizontálního oddílu. 

V tomto kódu aplikace pokusí tooopen existující **ShardMapManager** s hello [TryGetSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).  Pokud objekty, které představují globální **ShardMapManager** (GSM) není dosud neexistuje uvnitř hello databáze, hello klientské knihovny vytvoří je existuje pomocí hello [CreateSqlShardMapManager metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx).

    // Try tooget a reference toohello Shard Map Manager 
     // via hello Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create hello Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // hello connectionString contains server name, database name, and admin credentials 
        // for privileges on both hello GSM and hello shards themselves.
    } 

Jako alternativu můžete použít Powershell toocreate nového správce mapy horizontálního oddílu. Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>Získání RangeShardMap nebo ListShardMap
Po vytvoření horizontálního oddílu mapa správce, můžete získat hello [RangeShardMap](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ListShardMap](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí hello [TryGetRangeShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ TryGetListShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo hello [GetShardMap](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda.

    /// <summary>
    /// Creates a new Range Shard Map with hello specified name, or gets hello Range Shard Map if it already exists.
    /// </summary>
    public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
    {
        // Try tooget a reference toohello Shard Map.
        RangeShardMap<T> shardMap;
        bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

        if (shardMapExists)
        {
            ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
        }
        else
        {
            // hello Shard Map does not exist, so create it
            shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
            ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
        }

        return shardMap;
    } 

### <a name="shard-map-administration-credentials"></a>Pověření správce horizontálního oddílu mapy
Aplikace, které Správa a zpracování horizontálního oddílu maps se liší od těch, které používají hello horizontálního oddílu mapy tooroute připojení. 

mapuje tooadminister horizontálních (Přidat nebo změnit horizontálních oddílů, map horizontálního oddílu, mapování horizontálních, atd.) je třeba vytvořit instanci hello **ShardMapManager** pomocí **přihlašovací údaje, které mají oprávnění na obou hello GSM databáze a na každém pro čtení a zápis databáze, která slouží jako horizontálního oddílu**. přihlašovací údaje Hello musí povolovat zápisu hello tabulek v obou hello GSM a LSM informace ID horizontálního oddílu mapy zadat nebo změnit, stejně jako u vytváření tabulek LSM na nové horizontálních oddílů.  

V tématu [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Vliv pouze metadata.
Metody používané pro vyplnění nebo změna hello **ShardMapManager** data nijak nemění hello uživatelská data uložená v horizontálních oddílů hello sami. Například metody, jako **CreateShard**, **DeleteShard**, **UpdateMapping**atd ovlivnit hello horizontálního oddílu mapy obsahují pouze metadata. Není odebrat, přidat nebo změnit uživatele data obsažená v hello horizontálních oddílů. Místo toho tyto metody jsou navrženou toobe používá ve spojení s samostatné operace můžete provádět toocreate nebo odebrání skutečná databáze, nebo že přesunutí řádky jeden horizontálních tooanother toorebalance horizontálně dělené prostředí.  (hello **rozdělení sloučení** nástroje dodávaná s nástroje elastické databáze využívá tato rozhraní API společně s Orchestrace vlastní přesun dat. mezi horizontálních oddílů.) V tématu [škálování, pomocí nástroje hello elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="populating-a-shard-map-example"></a>Sestavování příklad horizontálního oddílu mapy
Ukázková posloupnost operací toopopulate mapu konkrétní horizontálního oddílu jsou uvedeny níže. Kód Hello provádí tyto kroky: 

1. Nové mapování horizontálních je vytvořen v rámci správce mapy horizontálního oddílu. 
2. Hello metadata pro dva různé horizontálních oddílů je přidán toohello horizontálního oddílu mapy. 
3. Budou přidány různé klíče rozsah mapování a hello celkové mapy horizontálního oddílu hello se zobrazí obsah. 

Kód Hello je napsán tak, aby hello metoda může být znovu, pokud dojde k chybě. Každý požadavek testuje, jestli horizontálního oddílu nebo mapování už existuje, před pokusem o toocreate ho. Hello kód předpokládá, že databáze s názvem **sample_shard_0**, **sample_shard_1** a **sample_shard_2** již byly vytvořeny v odkazuje řetězec serverhello**shardServer**. 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // Check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(
                                     shardServer, 
                                     "sample_shard_0"), 
                                     out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(
                                            shardServer, 
                                            "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(
                                    shardServer, 
                                    "sample_shard_1"), 
                                    out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(
                                             shardServer, 
                                            "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                          new RangeMappingCreationInfo<long>
                          (new Range<long>(0, 50), 
                          shard0, 
                          MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(50, 100), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long>
                         (new Range<long>(100, 150), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(150, 200), 
                         shard1, 
                         MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(
                         new RangeMappingCreationInfo<long> 
                         (new Range<long>(200, 300), 
                         shard0, 
                         MappingStatus.Online)); 
            } 

            // List hello shards and mappings 
            foreach (Shard s in sm.GetShards()
                         .OrderBy(s => s.Location.DataSource)
                         .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 

Jako alternativu můžete použít PowerShell skriptů tooachieve hello stejný výsledek. Některé příklady prostředí PowerShell ukázkový hello jsou k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).     

Jakmile jsou vyplněna horizontálního oddílu mapy, data přístup k aplikacím je možné vytvořit nebo přizpůsobit toowork s hello mapy. Naplnění nebo manipulace s hello mapy nemusí dojít znovu, dokud **mapy rozložení** musí toochange.  

## <a name="data-dependent-routing"></a>Směrování závislé na datech
správce mapy horizontálního oddílu Hello nejvíce použije v aplikacích, které vyžadují databázi připojení tooperform hello data specifická pro aplikaci operations. Tato připojení musí být přidružen hello správné databázi. To se označuje jako **závislé směrování dat**. Pro tyto aplikace vytvořit instanci objektu manager mapy horizontálního oddílu z objektu pro vytváření hello pomocí přihlašovacích údajů, které mají oprávnění jen pro čtení na hello GSM databáze. Jednotlivých požadavků pro pozdější připojení zadat přihlašovací údaje potřebné pro připojení databáze toohello odpovídající horizontálního oddílu.

Všimněte si, že tyto aplikace (pomocí **ShardMapManager** otevřít s přihlašovacími údaji jen pro čtení) nelze provádět změny mapování nebo toohello mapy. Pro tyto potřeby vytvořte správu konkrétní aplikace nebo skripty prostředí PowerShell, které zadat přihlašovací údaje pro vyšší úrovní oprávnění, jak je popsáno výše. V tématu [použita pověření tooaccess hello elastické databáze klientské knihovny](sql-database-elastic-scale-manage-credentials.md).

Další podrobnosti najdete v tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>Úprava horizontálního oddílu mapy
Mapování horizontálních lze změnit různými způsoby. Všechny následující metody hello upravit hello metadat, která popisují hello horizontálních oddílů a jejich mapování, ale fyzicky nemění data v rámci hello horizontálních oddílů, ani se jejich vytvoření nebo odstranění hello skutečné databáze.  Některé operace hello na mapě horizontálního oddílu hello popsané dál může být nutné toobe koordinované s akce správy, který fyzicky přesunout data nebo který přidávat a odebírat databáze slouží jako horizontálních oddílů.

Tyto metody spolupracovat jako stavební bloky hello k dispozici pro úpravy hello celkové distribuci dat ve vašem prostředí horizontálně dělené databáze.  

* tooadd nebo odeberte horizontálních oddílů: použijte  **[CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)**  a  **[DeleteShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)**  z hello [Shardmap třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx). 
  
    Hello server a databáze představující horizontálního oddílu hello cíl již musí existovat pro tyto operace tooexecute. Tyto metody nemají žádný vliv na hello samotných databázích, jenom na metadata v mapě hello horizontálního oddílu.
* body toocreate nebo odeberte nebo rozsahy, které jsou namapované toohello horizontálních oddílů: použijte  **[CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn841993.aspx)**,  **[DeleteMapping](https://msdn.microsoft.com/library/azure/dn824200.aspx)**  z hello [RangeShardMapping třída](https://msdn.microsoft.com/library/azure/dn807318.aspx), a  **[CreatePointMapping](https://msdn.microsoft.com/library/azure/dn807218.aspx)**  z hello [ListShardMap](https://msdn.microsoft.com/library/azure/dn842123.aspx)
  
    Mnoho různých bodů nebo rozsahy lze mapovat toohello stejné ID horizontálního oddílu. Tyto metody ovlivňují jenom metadata – neovlivňují všechna data, která může být již přítomny v horizontálních oddílů. Pokud data musí odebrat z databáze hello v pořadí toobe konzistentní s toobe **DeleteMapping** operace, budete potřebovat tooperform tyto operace samostatně, ale ve spojení s těmito metodami.  
* existující rozsahy toosplit do dvou nebo sloučení sousedící oblasti do jedné: použijte  **[SplitMapping](https://msdn.microsoft.com/library/azure/dn824205.aspx)**  a  **[MergeMappings](https://msdn.microsoft.com/library/azure/dn824201.aspx)**.  
  
    Všimněte si, že rozdělení a operace sloučení **neměňte hello horizontálního oddílu toowhich klíč hodnoty, se namapují**. Rozdělení stávající rozsah dělí na dvě části, ale ponechá i jako namapované toohello stejné ID horizontálního oddílu. Sloučení funguje na dva přiléhající rozsahů, které jsou už namapované toohello stejné ID horizontálního oddílu, je slučování do jednoho rozsahu.  Hello pohybů bodů nebo rozsahy, sami mezi horizontálních oddílů musí toobe koordinované pomocí **UpdateMapping** ve spojení s vlastní přesun dat..  Můžete použít hello **rozdělení či sloučení** služba, která je součástí elastické databáze nástroje změny mapování horizontálních toocoordinate s přesun dat potřeby pohyb. 
* toore mapy (nebo přesunutí) jednotlivé body nebo rozsahy toodifferent horizontálních oddílů: použijte  **[UpdateMapping](https://msdn.microsoft.com/library/azure/dn824207.aspx)**.  
  
    Vzhledem k tomu, že dat může být nutné přesunout z jednoho horizontálních tooanother v pořadí toobe konzistentní s toobe **UpdateMapping** operace, budete potřebovat tooperform tento přesun samostatně, ale ve spojení s těmito metodami.
* mapování tootake online a offline: použijte  **[MarkMappingOffline](https://msdn.microsoft.com/library/azure/dn824202.aspx)**  a  **[MarkMappingOnline](https://msdn.microsoft.com/library/azure/dn807225.aspx)**  toocontrol hello online stavu mapování. 
  
    Mapování horizontálních určité operace jsou povoleny pouze při mapování je ve stavu "offline", včetně **UpdateMapping** a **DeleteMapping**. Při mapování je offline, žádost dat závisí na základě klíče, součástí mapování vrátí chybu. Kromě toho když rozsah je nejprve do režimu offline, ID horizontálního oddílu toohello vliv na všechny připojení jsou automaticky ukončená ve výsledcích nekonzistentní nebo neúplné tooprevent pořadí pro dotazy namířená proti rozsahy se změnil. 

Mapování jsou neměnné objekty v rozhraní .net.  Všechny metody hello výše, které změní mapování také zneplatnit toothem jakékoli odkazy v kódu. toomake it jednodušší tooperform pořadí operací, které změny stavu mapování, všechny hello metody, které změní mapování vrátit novou mapování odkaz, takže možné zřetězit operace. Například toodelete existující mapování v shardmap sm, který obsahuje klíč hello 25, můžete provést hello následující: 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## <a name="adding-a-shard"></a>Přidání horizontálního oddílu
Aplikace často potřebují toosimply přidat nová data toohandle horizontálních oddílů, kterou je očekáván z nových klíčů nebo rozsahy klíčů pro ID horizontálního oddílu mapu, která již existuje. Například aplikace horizontálně dělené podle ID klienta může potřebovat tooprovision nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před hello začátek každý nový měsíc. 

Pokud není hello nový rozsah hodnot klíče již součástí stávající mapování a žádné přesun dat je nezbytné, je velmi jednoduchý tooadd hello nové horizontálních a přidružit nový klíč hello nebo rozsah toothat komplikovaného skládání architektury. Podrobnosti o přidání nové horizontálních oddílů najdete v tématu [přidání nové horizontálního oddílu](sql-database-elastic-scale-add-a-shard.md).

Pro scénáře, které vyžadují přesun dat ale nástroj rozdělení sloučení hello je potřebné tooorchestrate hello přesun dat mezi horizontálních oddílů v kombinaci s aktualizace mapování hello nezbytné horizontálního oddílu. Podrobnosti o použití hello yool rozdělení sloučení, najdete v části [Přehled rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
