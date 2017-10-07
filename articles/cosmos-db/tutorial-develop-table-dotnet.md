---
title: "Azure Cosmos DB: Vývoj pomocí hello rozhraní API pro tabulky v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toodevelop s rozhraním API Azure Cosmos DB Table pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a>Azure Cosmos DB: Vývoj pomocí hello rozhraní API pro tabulky v rozhraní .NET

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.

Tento kurz se zabývá hello následující úlohy: 

> [!div class="checklist"] 
> * Vytvoření účtu služby Azure Cosmos DB 
> * Povolit funkci v souboru app.config hello 
> * Vytvoření tabulky s použitím hello [tabulky API](table-introduction.md) (preview)
> * Přidat tooa tabulka entity 
> * Vložení dávky entit 
> * Načtení jedné entity 
> * Entity dotazu pomocí automatické sekundární indexy 
> * Nahrazení entity 
> * Odstranění entity 
> * Odstranění tabulky
 
## <a name="tables-in-azure-cosmos-db"></a>Tabulky v Azure Cosmos DB 

Azure Cosmos DB poskytuje hello [tabulky API](table-introduction.md) (Náhled) pro aplikace, které potřebují úložiště dvojic klíč hodnota s návrhem bez schématu. [Azure Table storage](../storage/common/storage-introduction.md) sady SDK a rozhraní REST API lze použít toowork s Azure Cosmos DB. Můžete použít Azure Cosmos DB toocreate tabulky s požadavky na vysoké propustnosti. Azure Cosmos DB podporuje tabulky s optimalizovanou propustností (neformálně označované jako „tabulky Premium“), aktuálně ve verzi Public Preview. 

Můžete pokračovat v toouse Azure Table storage pro tabulky s vysokou úložiště a nižší požadavky na propustnost. Azure Cosmos DB zavádí podporu pro úložiště optimalizované tabulky v budoucí aktualizaci a Azure Table stávající a nové účty úložiště bude plynule upgradovat tooAzure Cosmos DB.

Pokud aktuálně používáte Azure Table storage, získáte následující výhody s preview "premium tabulka" hello hello:

- Klíč [globální distribuční](distribute-data-globally.md) s více domovských stránek a [automatickou a ruční převzetí služeb při selhání](regional-failover.md)
- Podpora pro automatické indexování pro všechny vlastnosti (dále jen "sekundární indexy") a rychlé dotazy schématu vznikl 
- Podpora pro [nezávislé škálování úložiště a propustnost](partition-data.md), napříč libovolný počet oblastí
- Podpora pro [vyhrazené propustnost za tabulky](request-units.md) , je možné rozšířit stovek toomillions požadavků za sekundu
- Podpora pro [pět přizpůsobitelné úrovně konzistence](consistency-levels.md) tootrade vypnout dostupnost, latence a konzistence podle potřebám vaší aplikace
- 99,99 % dostupnost v rámci jedné oblasti a možnost tooadd další oblasti pro vyšší dostupnosti, a [komplexní SLA špičkový](https://azure.microsoft.com/support/legal/sla/cosmos-db/) na obecné dostupnosti
- Práce s hello existující úložiště Azure .NET SDK a žádná aplikace tooyour změny kódu

Během hello preview podporuje Azure Cosmos DB hello API tabulky pomocí hello .NET SDK. Si můžete stáhnout hello [SDK Preview úložiště Azure](https://aka.ms/premiumtablenuget) z NuGet, který má hello stejné třídy a metody podpisy jako hello [sada SDK úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale také můžete připojit tooAzure Cosmos DB účty pomocí hello Tabulka rozhraní API.

toolearn Další informace o složité úlohy Azure Table storage, najdete v části:

* [Úvod tooAzure Cosmos DB: tabulka rozhraní API](table-introduction.md)
* Hello tabulky referenční dokumentaci ke službě kompletní informace o dostupných rozhraních API [Klientská knihovna pro úložiště pro .NET – referenční informace](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

### <a name="about-this-tutorial"></a>O tomto kurzu
V tomto kurzu je pro vývojáře, kteří znají hello úložiště tabulek Azure SDK a chcete toouse hello prémiové funkce dostupné pomocí Azure Cosmos DB. Je založena na [Začínáme s Azure Table storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) a ukazuje, jak tootake využívat další funkce jako sekundární indexy, zřízená propustnost a více domovských stránek. Nabídneme jak toouse hello Azure portálu toocreate účet Azure Cosmos DB sestavení a nasazení aplikace tabulky. Můžeme také provede příklady .NET pro vytváření a odstraňování tabulek a vkládání, aktualizaci, odstranění a dotazování dat v tabulce. 

Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.  

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).
> * Měli jste účet Azure DocumentDB? Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).  
> * Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a>Klonování hello ukázkové aplikace

Nyní Pojďme klonovat aplikace tabulky z githubu, nastavte hello připojovací řetězec a spusťte ho.

1. Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.  

2. Spusťte následující příkaz tooclone hello Ukázka úložiště hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Poté otevřete soubor řešení hello v sadě Visual Studio.

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.

1. V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**. Kopírování tlačítek hello použijete na pravé straně hello hello obrazovky toocopy hello připojovací řetězec do souboru app.config hello v dalším kroku hello.

2. V sadě Visual Studio otevřete soubor app.config hello. 

3. Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíč účtu hello v souboru app.config. Použijte název účtu hello dříve vytvořili pro název účtu v souboru app.config.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> toouse této aplikaci pomocí standardní Azure Table Storage je nutné toochange hello připojovací řetězec v `app.config file`. Název účtu hello použijte jako název tabulky účtu a klíč jako Azure úložiště primární klíč. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a>Sestavení a nasazení aplikace hello
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**. 

2. V hello NuGet **Procházet** zadejte ***WindowsAzure.Storage PremiumTable***. Zkontrolujte **zahrnují předprodejní verze**.

3. Z výsledků hello nainstalovat hello **WindowsAzure.Storage PremiumTable** a zvolte buildu preview hello `0.0.1-preview`. Tato akce nainstaluje balíček úložiště Azure Table hello a všechny závislé součásti.

4. Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace. 

Teď můžete vrátit tooData Průzkumníka a zobrazit dotaz, upravit a práci s daty této tabulky. 

> [!NOTE]
> toouse této aplikace pomocí emulátoru DB Cosmos Azure, můžete jednoduše potřebovat toochange hello připojovací řetězec v `app.config file`. Použijte hello menší než hodnota pro emulátor. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Možnosti Azure Cosmos DB
Azure Cosmos DB podporuje několik možností, které nejsou k dispozici v hello Azure Table storage rozhraní API. Hello nové funkce se dá povolit buď hello následující `appSettings` hodnoty konfigurace. Zavedeme nejsou žádné nové podpisy nebo přetížení toohello preview SDK úložiště Azure. To vám umožní tooconnect tooboth standard a premium tabulky a práci s jinými službami Azure Storage jako objekty BLOB a fronty. 


| Klíč | Popis |
| --- | --- |
| TableConnectionMode  | Azure Cosmos DB podporuje dva režimy připojení. V `Gateway` režimu, požadavky se budou vždy provádět toohello Azure Cosmos DB brána, která předá ho toohello odpovídající data oddíly. V `Direct` režim připojení klienta hello načte hello mapování tabulek toopartitions a přímo na data oddíly jsou vytvářeny požadavky. Doporučujeme, abyste `Direct`, hello výchozí.  |
| TableConnectionProtocol | Azure Cosmos DB podporuje dva protokoly připojení - `Https` a `Tcp`. `Tcp`hello výchozí a doporučuje, protože je jednodušší. |
| TablePreferredLocations | Čárkami oddělený seznam upřednostňovaných (více funkci) umístění pro čtení. Každý účet Azure Cosmos DB lze přidružit 1 – 30 + oblasti. Každá instance klienta můžete určit podmnožinu těchto oblastí v hello upřednostňované pořadí pro čtení s nízkou latencí. Hello oblasti musí mít název pomocí jejich [zobrazované názvy](https://msdn.microsoft.com/library/azure/gg441293.aspx), například `West US`. Viz také [více domovských stránek rozhraní API](tutorial-global-distribution-table.md).
| TableConsistencyLevel | Můžete se kompromisy mezi latence, konzistence a dostupnosti výběrem mezi pěti dobře definované úrovně konzistence: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, a `Eventual`. Výchozí hodnota je `Session`. Hello Volba úrovně konzistence díky významně zvýšit výkon rozdíl ve více oblastech nastavení. V tématu [úrovně konzistence](consistency-levels.md) podrobnosti. |
| TableThroughput | Vyhrazenou propustností pro tabulku hello vyjádřené v jednotek žádosti (RU) za sekundu. Jedné tabulky může podporovat 100s miliony RU/s. V tématu [požadované jednotky](request-units.md). Výchozí hodnota je`400` |
| TableIndexingPolicy | Konzistentní a automatické sekundární indexování všech sloupců v rámci tabulky | JSON řetězec vyhovující toohello indexování specifikace zásad. V tématu [zásady indexování](indexing-policies.md) toosee jak můžete změnit indexování zásad tooinclude/vyloučit konkrétní sloupce. | Automatické indexování všech vlastností (hodnota hash pro řetězce) a rozsah čísel |
| TableQueryMaxItemCount | Nakonfigurujte hello maximální počet položek vrátí na dotaz tabulky v jednom dobu odezvy. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu hello za běhu. |
| TableQueryEnableScan | Pokud hello dotazu nelze použít pro libovolný filtr hello index, spusťte ji přesto prostřednictvím kontrolu. Výchozí hodnota je `false`.|
| TableQueryMaxDegreeOfParallelism | Hello stupně paralelního zpracování pro spuštění dotazu mezi oddílu. `0`sériový s žádné předem načítání, `1` je sériové s předem načítání a vyšší hodnoty zvýšit rychlost hello paralelního zpracování. Výchozí hodnota je `-1`, které umožní Azure DB Cosmos dynamicky určí hodnotu hello za běhu. |

toochange hello výchozí hodnotu, otevřete hello `app.config` soubor v Průzkumníku řešení v sadě Visual Studio. Přidat obsah hello hello `<appSettings>` níže uvedeného prvku. Nahraďte `account-name` hello název účtu úložiště a `account-key` nahraďte svým klíčem účtu přístup. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Provedeme jejich stručný přehled o dění v aplikaci hello. Otevřete hello `Program.cs` souboru a najít vytvořit tyto řádky kódu hello tabulky prostředky. 

## <a name="create-hello-table-client"></a>Vytvoření klienta tabulky hello
Inicializaci `CloudTableClient` tooconnect toohello tabulky účtu.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Tento klient je inicializována pomocí hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, a `TablePreferredLocations` konfigurační hodnoty, pokud zadaný v nastavení aplikace hello.
    
## <a name="create-a-table"></a>Vytvoření tabulky
Pak vytvořte tabulku pomocí `CloudTable`. Tabulky v databázi Cosmos Azure můžete nezávisle škálovat z hlediska úložiště a propustnost a dělení se automaticky zpracovává službou hello. Azure Cosmos DB podporuje pevné velikosti a neomezená tabulky. V tématu [vytváření oddílů v Azure Cosmos DB](partition-data.md) podrobnosti. 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Je důležité rozdíly v vytváření tabulek. Azure Cosmos DB si vyhrazuje propustnost, na rozdíl od modelu na základě spotřeby úložiště Azure pro transakce. model rezervace Hello přináší dvě klíčové výhody:

* Vaše propustnost je vyhrazené nebo vyhrazené, tak můžete nikdy získat omezeny Pokud je vaše četnost požadavků nebo zřízené propustnosti pod ní.
* model rezervace Hello je další [nákladově efektivní pro úlohy náročné propustnost](key-value-store-cost.md)

Propustnost výchozí hello můžete nakonfigurovat tak, že nakonfigurujete nastavení hello `TableThroughput` z hlediska RU (jednotek žádosti) za sekundu. 

Čtení entity 1 KB normalizována jako 1 RU a další operace jsou normalizovaný tooa pevná hodnota RU na základě jejich spotřeby procesoru, paměti a procesorů. Další informace o [požadované jednotky v Azure Cosmos DB](request-units.md).

> [!NOTE]
> Zatímco úložiště Table SDK aktuálně nepodporuje změny propustnosti, můžete změnit hello propustnost okamžitě kdykoli použití hello portál Azure nebo Azure CLI.

Potom provede hello jednoduché pro čtení a zápisu operace pomocí Azure Table storage hello SDK. Tento kurz představuje předvídatelný nízkou milisekundu jednociferné latenci a rychlé dotazy, které poskytuje Azure Cosmos DB.

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity
Entity v Azure Table storage vycházet z hello `TableEntity` třídy a musí mít `PartitionKey` a `RowKey` vlastnosti. Zde je ukázka definice pro entitu zákazníka.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Hello následující fragment kódu ukazuje, jak tooinsert entity s hello úložiště Azure SDK. Azure Cosmos DB je určená pro zaručit s nízkou latencí v jakémkoli měřítku napříč hello, world.

Dokončení zápisu < hello 15 ms v p99 a ms ~ 6 v p50 pro aplikace spuštěné stejné oblasti jako hello účet Azure Cosmos DB. A tato doba trvání účty pro hello fakt, který zapíše jsou potvrzené back toohello klienta, až po jejich synchronně replikovány, spolehlivě potvrzené, a veškerý obsah je indexovaný.

Hello tabulky rozhraní API pro Azure Cosmos DB je ve verzi preview. Při obecné dostupnosti hello p99 latence záruky jsou zajišťované SLA jako jiná rozhraní API Azure Cosmos DB. 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Vložení dávky entit
Azure Table storage podporuje rozhraní API dávkové operace, které vám umožní sloučit aktualizace, odstranění, a vloží v hello jedné dávkové operace. Azure Cosmos DB nemá některé hello omezení na rozhraní API pro dávkové hello jako Azure Table storage. Například můžete provádět vícenásobné čtení v dávce, můžete provést několik toohello zápisy stejné entity v dávce, a není nijak omezen na 100 operace na jednu dávku. 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Načtení jedné entity
Načte (získá) v Azure DB Cosmos dokončení < 10 ms v p99 a ~ 1 hello ms v p50 ve stejné oblasti Azure. Můžete přidat jako v mnoha oblastech tooyour účet pro čtení s nízkou latencí a nasadit aplikace tooread ze své místní oblast ("s více adresami") nastavením `TablePreferredLocations`. 

Můžete načíst jednu entitu pomocí hello následující fragment kódu:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Další informace o více funkci rozhraní API v [vývoj s několika oblastí](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Entity dotazu pomocí automatické sekundární indexy
Tabulky lze dotazovat pomocí hello `TableQuery` třídy. Azure Cosmos DB má optimalizované zápisu databázový stroj, který automaticky indexuje všechny sloupce v tabulce. Indexování v Azure Cosmos DB je lhostejné tooschema. Proto i v případě, že vaše schéma je odlišné mezi řádky nebo pokud schéma hello vyvíjí v průběhu času, je automaticky indexován. Vzhledem k tomu, že Azure Cosmos DB podporuje automatické sekundární indexy, dotazy pro vlastnost, můžete použít hello index a efektivně obsluhovat.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

Ve verzi preview podporuje Azure Cosmos DB hello stejné funkce jako úložiště Azure Table pro hello API tabulka dotazu. Azure Cosmos DB také podporuje řazení, agregace, geoprostorové dotazu, hierarchie a širokou škálu integrované funkce. Hello další funkce bude k dispozici v hello API tabulky v aktualizaci budoucí služby. V tématu [Azure Cosmos DB dotazu](documentdb-sql-query.md) přehled těchto funkcí. 

## <a name="replace-an-entity"></a>Nahrazení entity
tooupdate entity, načtěte ji ze služby Table hello, upravte objekt entity hello a potom uložte změny hello zpět toohello služby Table. Hello následující kód změní telefonní číslo stávajícího zákazníka. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Podobně můžete provádět `InsertOrMerge` nebo `Merge` operace.  

## <a name="delete-an-entity"></a>Odstranění entity
Po jejím načtení pomocí hello můžete snadno odstranit entity stejného vzoru zobrazovaného pro aktualizaci entity. Hello následující kód načte a odstraní entitu zákazníka.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Odstranění tabulky
Hello následující příklad kódu nakonec odstraní tabulku z účtu úložiště. Můžete odstranit a znovu vytvořte tabulku okamžitě s Azure Cosmos DB.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Vyčištění prostředků 

Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello.   

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.  
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jsme popsaná způsob tooget spuštění pomocí Azure Cosmos DB hello tabulky rozhraní API a uděláte hello následující: 

> [!div class="checklist"] 
> * Vytvoření účtu Azure Cosmos DB 
> * Povolená funkce v souboru app.config hello 
> * Vytvoření tabulky 
> * Přidat tooa tabulka entity 
> * Vložit dávku entit 
> * Načíst jednu entitu 
> * Dotazovaný entity pomocí automatické sekundární indexy 
> * Nahradí entitu 
> * Odstranit entity 
> * Odstranit tabulku  

Teď můžete pokračovat dalším kurzu toohello a další informace o dotazování dat v tabulce. 

> [!div class="nextstepaction"]
> [Dotaz s hello tabulky rozhraní API](tutorial-query-table.md)
