---
title: "Nástroj pro migraci aaaDatabase pro Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak toouse hello otevřete zdroj Azure Cosmos DB data migrace nástroje tooimport data tooAzure Cosmos DB z různých zdrojů, včetně souborů MongoDB, systému SQL Server, úložiště Table, Amazon DynamoDB, CSV a JSON. Převod tooJSON sdíleného svazku clusteru."
keywords: "CSV toojson, nástrojů pro migraci databáze, převést toojson sdíleného svazku clusteru"
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 997648a31602d854db75bb6ce4e2ecff36fc1069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a>Jak hello tooimport data do Azure DB Cosmos pro rozhraní API DocumentDB?

Tento kurz obsahuje instrukce k používání hello Azure Cosmos DB: nástroj pro migraci dat rozhraní API DocumentDB, který můžete importovat data z různých zdrojů, včetně souborů JSON, CSV soubory, SQL, MongoDB, Azure Table storage, Amazon DynamoDB a Azure Cosmos databáze DocumentDB Rozhraní API kolekce do kolekce pro použití v Azure Cosmos DB a hello DocumentDB rozhraní API. Nástroj pro migraci dat Hello mohou sloužit také při migraci z kolekce tvořené jedním oddílem kolekce tooa více oddílů pro hello DocumentDB rozhraní API.

Nástroj pro migraci dat Hello funguje pouze při import dat do Azure DB Cosmos pro použití s hello DocumentDB rozhraní API. V tuto chvíli není podporován import dat pro použití s hello tabulky API nebo rozhraní Graph API. 

tooimport dat pro použití s hello MongoDB rozhraní API, najdete v části [Cosmos databázi Azure: jak hello toomigrate data pro rozhraní API MongoDB?](mongodb-migrate.md).

Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Instalace nástroje pro migraci dat hello
> * Import dat z různých zdrojů dat.
> * Export z Azure Cosmos DB tooJSON

## <a id="Prerequisites"></a>Požadavky
Než budete postupovat hello pokyny v tomto článku, zajistěte, abyste měli hello nainstalované tyto položky:

* [Rozhraní Microsoft .NET Framework 4.51](https://www.microsoft.com/download/developer-tools.aspx) nebo vyšší.

## <a id="Overviewl"></a>Přehled nástroje pro migraci dat hello
Nástroj pro migraci dat Hello je řešení open source, který umožňuje importovat data tooAzure Cosmos DB z různých zdrojů, včetně:

* Soubory JSON
* MongoDB
* SQL Server
* CSV soubory
* Azure Table Storage
* Amazon DynamoDB
* HBase
* Azure Cosmos DB kolekce

Když nástroj pro import hello zahrnuje grafické uživatelské rozhraní (dtui.exe), můžete také vycházejí z příkazového řádku hello (dt.exe). Po nastavení importu prostřednictvím hello uživatelského rozhraní je ve skutečnosti příkaz hello přidružené toooutput možnost. Tabulkové zdroje dat (např. SQL Server nebo CSV soubory) lze je transformovat tak, aby hierarchické vztahy (vnořené dokumenty) lze vytvořit při importu. Zachovat čtení toolearn informace o možnosti nastavení zdroje, tooimport příkazové řádky ukázkové z každého zdroje, target – možnosti a zobrazení pro import výsledků.

## <a id="Install"></a>Nainstalujte nástroj pro migraci dat hello
Hello migrace nástroj zdrojový kód je k dispozici na Githubu v [toto úložiště](https://github.com/azure/azure-documentdb-datamigrationtool) a je k dispozici z kompilované verze [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d). Může buď zkompilovat hello řešení nebo jednoduše stažení a extrakci hello zkompilovaná tooa adresáři podle vašeho výběru. Spusťte buď:

* **Dtui.exe**: verze grafického rozhraní nástroje hello
* **DT.exe**: příkazového řádku verze nástroje hello

## <a name="import-data"></a>Import dat

Jakmile jste nainstalovali nástroj hello, je čas tooimport vaše data. Jaký typ dat chcete tooimport?

* [Soubory JSON](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB exportní soubory](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV soubory](#CSV)
* [Azure Table storage](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [Objekt BLOB](#BlobImport)
* [Azure Cosmos DB kolekce](#DocumentDBSource)
* [HBase](#HBaseSource)
* [Azure Cosmos DB hromadného importu](#DocumentDBBulkImport)
* [Sekvenční záznam importu do Azure Cosmos DB](#DocumentDSeqTarget)


## <a id="JSON"></a>soubory JSON tooimport
Hello možnost program pro import zdrojového souboru JSON umožňuje tooimport jeden nebo více jednotlivý dokument JSON soubory nebo soubory JSON, že každý obsahovat pole dokumentů JSON. Při přidávání složek, které obsahují tooimport soubory JSON, máte možnost hello rekurzivní hledání souborů v podsložkách.

![Možnosti nastavení zdroje souboru snímek JSON - nástrojů pro migraci databáze](./media/import-data/jsonsource.png)

Zde jsou některé soubory JSON tooimport ukázky příkazového řádku:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition hello data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>tooimport z MongoDB

> [!IMPORTANT]
> Pokud importujete účet Azure Cosmos DB tooan s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).
> 
> 

Hello MongoDB zdrojový program pro import možnost vám umožní tooimport z kolekci jednotlivých MongoDB a volitelně filtrovat dokumentů pomocí dotazu nebo změna struktury dokumentu hello pomocí projekce.  

![Možnosti nastavení zdroje snímek MongoDB](./media/import-data/mongodbsource.png)

Hello připojovací řetězec je ve formátu MongoDB standard hello:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Je možné přistupovat pomocí hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci MongoDB.
> 
> 

Zadejte název hello hello kolekce, ze kterého budou importována data. Může volitelně zadejte nebo zadejte soubor pro dotaz (například {pop: {$gt: 5000}}) nebo projekce (například {loc:0}) tooboth filtru a tvar hello data toobe importovat.

Zde jsou některé tooimport ukázky příkazový řádek z MongoDB:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>tooimport MongoDB exportní soubory

> [!IMPORTANT]
> Pokud importujete účet Azure Cosmos DB tooan s podporou pro MongoDB, postupujte podle těchto [pokyny](mongodb-migrate.md).
> 
> 

Hello MongoDB export JSON souboru zdrojový program pro import možnost vám umožní tooimport jeden nebo více soubory JSON vytvořeného z hello mongoexport nástroj.  

![Možnosti nastavení zdroje export snímek MongoDB](./media/import-data/mongodbexportsource.png)

Při přidávání složek, které obsahují soubory JSON export MongoDB pro import, máte možnost hello rekurzivní hledání souborů v podsložkách.

Tady je příkazový řádek ukázkové tooimport MongoDB exportu soubory JSON:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>tooimport z SQL serveru
Hello možnost program pro import zdroje SQL můžete tooimport z jednotlivých databáze systému SQL Server a volitelně filtrování toobe záznamy hello importován pomocí dotazu. Kromě toho můžete změnit strukturu dokumentu hello zadáním vnoření oddělovače (podrobnosti o této za chvíli).  

![Možnosti nastavení zdroje snímek SQL – nástroje pro migraci databáze](./media/import-data/sqlexportsource.png)

Formát Hello hello připojovacího řetězce je formátu řetězce připojení standardní SQL hello.

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci serveru SQL je přístupná.
> 
> 

Hello vnoření vlastnost oddělovače je použité toocreate hierarchické vztahy (dílčí dokumenty) během importu. Vezměte v úvahu hello následující dotazu SQL:

*jako Id, názvu, AddressType jako [Address.AddressType], AddressLine1 jako [Address.AddressLine1], města jako [Address.Location.City], StateProvinceName jako [Address.Location.StateProvinceName], PostalCode jako [[[vyberte PŘETYPOVÁNÍ (BusinessEntityID AS varchar) Address.PostalCode] CountryRegionName jako [Address.CountryRegionName] z Sales.vStoreWithAddresses kde AddressType = "hlavní kanceláře.*

Která vrací hello následující výsledky (částečné):

![Snímek obrazovky SQL výsledky dotazu](./media/import-data/sqlqueryresults.png)

Poznámka: aliasy hello například Address.AddressType a Address.Location.StateProvinceName. Zadáním vnoření oddělovač '.', nástroj pro import hello vytvoří adres a import vnořené dokumenty Address.Location během hello. Tady je příklad výsledné dokumentu v Azure Cosmos DB:

*{"id": "956", "název": "Jemnějšího prodeje a služba", "Adres": {"AddressType": "Hlavní Office", "AddressLine1": "#500 75 O'Connor ulici", "Umístění": {"City": "Ottawa", "StateProvinceName": "Ontario"}, "PostalCode": "K4B 1S2", "CountryRegionName": " Kanada"}}*

Zde jsou některé tooimport ukázky příkazový řádek z SQL serveru:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>CSV soubory tooimport a tooJSON převést sdíleného svazku clusteru
Hello možnost program pro import zdrojového souboru CSV umožňuje tooimport je jeden nebo více souborů CSV. Při přidávání složek, které obsahují soubory sdíleného svazku clusteru pro import, máte možnost hello rekurzivní hledání souborů v podsložkách.

![Možnosti nastavení zdroje snímek CSV - tooJSON sdíleného svazku clusteru](media/import-data/csvsource.png)

Podobné toohello SQL zdroje, hello vnoření oddělovače vlastnost může být použité toocreate hierarchické vztahy (dílčí dokumenty) během importu. Vezměte v úvahu hello následující řádek záhlaví sdíleného svazku clusteru a řádků dat:

![Snímek obrazovky CSV ukázkové záznamy - tooJSON sdíleného svazku clusteru](./media/import-data/csvsample.png)

Poznámka: aliasy hello například DomainInfo.Domain_Name a RedirectInfo.Redirecting. Zadáním vnoření oddělovač '.', vytvoří nástroj pro import hello DomainInfo a vnořené dokumenty RedirectInfo během hello importovat. Tady je příklad výsledné dokumentu v Azure Cosmos DB:

*{"DomainInfo": {"Domain_Name": "ACUS.GOV", "Domain_Name_Address": "http://www. ACUS.GOV"},"Federal agenturou":"Správu konferencí hello USA","RedirectInfo": {"Přesměrování":"0","Redirect_Destination":" "},"id":"9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*

Nástroj pro import Hello pokusí tooinfer informací o typu nekotovaných hodnoty v souborů CSV (hodnoty v uvozovkách se vždy pracuje jako řetězce).  Typy jsou označeny v následujícím pořadí hello: číslo, datum a čas, logická hodnota.  

Existují dvě jiných věcí toonote o importu souboru CSV:

1. Ve výchozím nastavení, jsou nekotovaných hodnoty vždy oříznut pro karty a prostory, zatímco se zachovají hodnoty v uvozovkách jako-je. Toto chování lze přepsat pomocí hello Trim v uvozovkách hodnoty zaškrtávací políčko nebo hello /s.TrimQuoted možnosti příkazového řádku.
2. Ve výchozím nastavení nekotovaných null považován za hodnotu null. Toto chování můžete přepsat (tj. považovat za nekotovaných null řetězec "null") s hello zpracovávat nekotované NULL jako řetězec zaškrtávací políčko nebo hello /s.NoUnquotedNulls možnosti příkazového řádku.

Zde je ukázka příkazového řádku pro importu souboru CSV:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>tooimport z úložiště tabulek Azure
Hello možnost program pro import zdroje úložiště Azure Table vám umožní tooimport z jednotlivé tabulky Azure Table storage a volitelně filtrovat hello tabulka entity toobe importovat. Upozorňujeme, že nelze použít hello migrace dat nástroj tooimport Azure Table storage data do Azure Cosmos DB pro použití s hello tabulky rozhraní API. V tuto chvíli je podporována pouze import tooAzure Cosmos DB pro použití s hello DocumentDB rozhraní API.

![Možnosti nastavení zdroje snímek Azure Table storage](./media/import-data/azuretablesource.png)

Formát Hello hello Azure Table storage připojovacího řetězce je:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v pole řetězce připojení hello instanci Azure Table storage je přístupná.
> 
> 

Zadejte název hello hello Azure tabulka, ze kterého budou importována data. Volitelně můžete určit [filtru](https://msdn.microsoft.com/library/azure/ff683669.aspx).

Hello možnost program pro import zdroje úložiště Azure Table má hello následující další možnosti:

1. Zahrnutí interní polí
   1. Všechny - obsahovat všechny interní pole (klíč oddílu, RowKey a časové razítko)
   2. Žádný - vyloučit všechny interní pole
   3. RowKey – obsahovat pouze pole RowKey hello
2. Vyberte sloupce
   1. Azure Table storage filtry nepodporují projekce. Pokud chcete tooonly import specifické Azure Table entity vlastnosti, přidáte toohello výběr sloupců seznamu. Všechny ostatní vlastnosti entity, bude ignorována.

Tady je tooimport ukázka příkazový řádek z úložiště tabulek Azure:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>tooimport z Amazon DynamoDB
Hello Amazon DynamoDB zdrojový program pro import možnost vám umožní tooimport z jednotlivé tabulky Amazon DynamoDB a volitelně filtrování toobe entity hello importovat. Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource1.png)

![Možnosti nastavení zdroje DynamoDB Amazon – snímek obrazovky - nástrojů pro migraci databáze](./media/import-data/dynamodbsource2.png)

Formát Hello hello Amazon DynamoDB připojovacího řetězce je:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Amazon DynamoDB je přístupná.
> 
> 

Tady je tooimport ukázka příkazový řádek z Amazon DynamoDB:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>soubory tooimport z Azure Blob storage
Hello soubor JSON, MongoDB export souboru a možnosti program pro import zdrojového souboru CSV umožňují tooimport jeden nebo více souborů z úložiště objektů Blob Azure. Po zadání adresy URL kontejneru objektu Blob a klíč účtu, stačí zadejte tooimport soubory hello tooselect regulárním výrazem.

![Možnosti nastavení zdroje snímek objektu Blob souboru](./media/import-data/blobsource.png)

Tady je příkazového řádku ukázkové tooimport JSON soubory z Azure Blob storage:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="DocumentDBSource"></a>tooimport z kolekce rozhraní API služby Azure Cosmos databáze DocumentDB
Hello možnost program pro import zdroje Azure Cosmos DB vám umožní tooimport data z jedné nebo více kolekcí Azure Cosmos DB a volitelně filtrování dokumentů pomocí dotazu.  

![Možnosti nastavení zdroje snímek databázi Cosmos Azure](./media/import-data/documentdbsource.png)

Formát Hello hello Azure Cosmos DB připojovacího řetězce je:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:

    Database=<CosmosDB Database>;

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.
> 
> 

tooimport z jedné kolekce Azure Cosmos DB, zadejte název hello hello kolekce, ze kterého budou importována data. tooimport z více kolekcí Azure Cosmos DB, jeden nebo více názvy kolekce poskytují toomatch regulární výraz (například collection01 | collection02 | collection03). Může volitelně můžete zadat, nebo zadejte soubor pro tooboth filtru dotazu a utvářejí toobe hello data importovat.

> [!NOTE]
> Vzhledem k tomu, že hello kolekce pole přijímá regulární výrazy, při importu z jedné kolekce, jejichž název obsahuje regulární výraz znaků, musí tyto znaky uvozené odpovídajícím způsobem.
> 
> 

Hello Azure Cosmos DB zdrojový program pro import možnost má následující hello rozšířené možnosti:

1. Zahrnutí interní polí: Určuje, zda tooinclude Azure Cosmos DB dokumentu systému vlastnosti v hello exportovat (například _rid, _ts).
2. Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
3. Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
4. Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB. jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány. režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.

![Snímek obrazovky databázi Cosmos Azure zdroj rozšířené možnosti](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> Nástroj výchozí tooconnection režim DirectTcp import Hello. Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.
> 
> 

Zde jsou některé tooimport ukázky příkazový řádek z databáze Cosmos Azure:

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> Hello Azure Cosmos DB dat nástroj pro Import také podporuje import dat z hello [emulátoru DB Cosmos Azure](local-emulator.md). Při importu dat z místní emulátoru, nastavte koncový bod hello příliš`https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>tooimport z HBase
Hello možnost program pro import zdroje HBase můžete tooimport data z tabulky HBase a volitelně filtrování dat hello. Tak, aby nastavení importu se co největší jsou k dispozici několik šablon.

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource1.png)

![Možnosti nastavení zdroje snímek HBase](./media/import-data/hbasesource2.png)

Formát Hello hello HBase Stargate připojovacího řetězce je:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci HBase je přístupná.
> 
> 

Tady je tooimport ukázka příkazový řádek z HBase:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB rozhraní API (hromadným importem)
Program pro import Azure Cosmos DB hromadné Hello umožňuje tooimport ze všech dostupných možností hello pomocí Azure DB Cosmos uložené procedury pro efektivitu. Nástroj Hello podporuje import tooone oddíly jedním Azure Cosmos DB kolekce, jakož i horizontálně dělené importu, které je dat rozděleného mezi více kolekcí oddíly jedním Azure Cosmos DB. Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md). Nástroj Hello bude vytvořte, spuštění a potom odstraňte hello uložené procedury z kolekcí cíl hello.  

![Možnosti hromadné snímek databázi Cosmos Azure](./media/import-data/documentdbbulk.png)

Formát Hello hello Azure Cosmos DB připojovacího řetězce je:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:

    Database=<CosmosDB Database>;

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.
> 
> 

tooimport tooa jedna kolekce, zadejte název hello hello kolekce toowhich data budou naimportovány a klikněte na tlačítko Přidat hello. tooimport toomultiple kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi toospecify hello více kolekcí: *collection_prefix*[index - end index počátečního]. Při zadávání více kolekcí prostřednictvím hello výše uvedené syntaxe, mějte hello následující skutečnosti:

1. Jsou podporovány pouze celé číslo vzory názvů rozsahu. Například zadání kolekce [0-3] vytvoří hello následující kolekce: collection0, collection1, collection2, collection3.
2. Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.
3. Lze zadat více než jeden nahrazení. Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).

Jakmile hello kolekce názvy byly nastaveny, vyberte požadované propustnost hello hello kolekcí (400 RUs too10, 000 RUs). Pro nejlepší výkon import zvolte vyšší propustnost. Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md).

> [!NOTE]
> Hello výkonu propustnost nastavení se vztahuje pouze toocollection vytvoření. Pokud hello zadané kolekce už existuje, se nezmění jeho propustnost.
> 
> 

Při importu toomultiple kolekce, nástroj pro import hello podporuje hodnoty hash na základě horizontálního dělení. V tomto scénáři, určete vlastnost dokumentu hello chcete toouse jako klíč oddílu hello (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně napříč hello cílové kolekce).

Můžete volitelně můžete určit, které pole ve zdroji import hello má být použit jako hello vlastnost id dokumentu Azure Cosmos DB během importu hello (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import hello bude generovat identifikátor GUID jako hodnota vlastnosti id hello).

Během importu nejsou k dispozici několik upřesňujících možností. Nejprve při hello nástroj obsahuje výchozí hromadné importovat uložené procedury (BulkInsert.js), můžete se rozhodnout toospecify vlastní import uložené procedury:

 ![Snímek obrazovky databázi Cosmos Azure hromadné vložení sproc možnost](./media/import-data/bulkinsertsp.png)

Kromě toho při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* Řetězec: Zachovat jako hodnotu řetězce
* Epoch: Zachovat jako číslo hodnota Epoch
* Oba: Zachovat řetězec a Epoch číselné hodnoty. Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}

Program pro import Azure Cosmos DB hromadné Hello má hello následující další rozšířené možnosti:

1. Velikost dávky: hello nástroj výchozí tooa velikost dávky 50.  Pokud toobe dokumenty hello importovat velké, zvažte, což snižuje velikost dávky hello. Pokud toobe dokumenty hello importovat malé, zvažte naopak vyvolání hello velikost dávky.
2. Skript maximální velikost (bajty): nástroj hello výchozí velikost maximální skriptu tooa 512KB
3. Zakázat automatické generování Id: Pokud každý dokument toobe importovat obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon. Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.
4. Aktualizace stávající dokumenty: toonot výchozí nástroj hello nahrazení stávající dokumenty docházet ke konfliktům id. Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID. Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.
5. Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
6. Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
7. Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB. jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány. režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.

![Snímek obrazovky databázi Cosmos Azure hromadným importem rozšířené možnosti](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> Nástroj výchozí tooconnection režim DirectTcp import Hello. Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.
> 
> 

## <a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB rozhraní API (po sobě jdoucích Import záznamů)
Program pro import po sobě jdoucích záznam Hello Azure Cosmos DB vám umožní tooimport z jakéhokoli z dostupných možností hello na základě záznamu podle. Tuto možnost můžete zvolit, pokud importujete tooan existující kolekci, která dosáhla své kvóty uložené procedury. Hello nástroj podporuje import tooa jedna kolekce Azure Cosmos DB (jedním oddílem i více oddílu), stejně jako horizontálně dělené importu, které jsou data rozdělena mezi více kolekcí Azure Cosmos DB jedním oddílem nebo více oddílů. Další informace o segmentace dat naleznete v tématu [dělení a škálování v Azure Cosmos DB](partition-data.md).

![Snímek obrazovky databázi Cosmos Azure možnosti sekvenční záznamů importu](./media/import-data/documentdbsequential.png)

Formát Hello hello Azure Cosmos DB připojovacího řetězce je:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

Hello připojovací řetězce k účtu Azure Cosmos DB lze načíst z okna klíče hello hello portál Azure, jak je popsáno v [jak toomanage účet Azure Cosmos DB](manage-account.md), ale název hello hello databáze musí toohello toobe připojeno připojovací řetězec v hello následující formát:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Použití hello ověřte, zda příkaz tooensure, který hello zadané v poli řetězec připojení hello instanci Azure Cosmos databáze je přístupná.
> 
> 

tooimport tooa jedna kolekce, zadejte název hello hello kolekce toowhich data budou naimportovány a klikněte na tlačítko Přidat hello. tooimport toomultiple kolekcí, zadejte jednotlivé názvy kolekce jednotlivě nebo použijte následující syntaxi toospecify hello více kolekcí: *collection_prefix*[index - end index počátečního]. Při zadávání více kolekcí prostřednictvím hello výše uvedené syntaxe, mějte hello následující skutečnosti:

1. Jsou podporovány pouze celé číslo vzory názvů rozsahu. Například zadání kolekce [0-3] vytvoří hello následující kolekce: collection0, collection1, collection2, collection3.
2. Můžete použít zkrácené syntaxe: kolekce [3] se emitování stejnou sadu kolekcí uveden v kroku 1.
3. Lze zadat více než jeden nahrazení. Například kolekce [0-1] [0-9] vygeneruje 20 názvů kolekcí úvodními nulami (collection01,... 02... 03).

Jakmile hello kolekce názvy byly nastaveny, vyberte požadované propustnost hello hello kolekcí (400 RUs too250, 000 RUs). Pro nejlepší výkon import zvolte vyšší propustnost. Další informace o úrovně výkonu najdete v tématu [úrovně výkonu v Azure Cosmos DB](performance-levels.md). Žádné importovat toocollections s propustností > 10 000 RUs bude vyžadovat klíč oddílu. Pokud vyberete více než 250 000 RUs toohave, budete potřebovat toofile žádost v portálu toohave hello vyšší váš účet.

> [!NOTE]
> Hello propustnost nastavení se vztahuje pouze toocollection vytvoření. Pokud hello zadané kolekce už existuje, se nezmění jeho propustnost.
> 
> 

Při importu toomultiple kolekce, nástroj pro import hello podporuje hodnoty hash na základě horizontálního dělení. V tomto scénáři, určete vlastnost dokumentu hello chcete toouse jako klíč oddílu hello (Pokud klíč oddílu je prázdné, dokumenty bude horizontálně dělené náhodně napříč hello cílové kolekce).

Můžete volitelně můžete určit, které pole ve zdroji import hello má být použit jako hello vlastnost id dokumentu Azure Cosmos DB během importu hello (Všimněte si, že pokud dokumenty neobsahují tuto vlastnost, pak nástroj pro import hello bude generovat identifikátor GUID jako hodnota vlastnosti id hello).

Během importu nejsou k dispozici několik upřesňujících možností. První při importu datum typy (například ze systému SQL Server nebo MongoDB), můžete zvolit tři možnosti importu:

 ![Snímek obrazovky databázi Cosmos Azure datum čas import možnosti](./media/import-data/datetimeoptions.png)

* Řetězec: Zachovat jako hodnotu řetězce
* Epoch: Zachovat jako číslo hodnota Epoch
* Oba: Zachovat řetězec a Epoch číselné hodnoty. Tato možnost vytvořit vnořený dokument, třeba: "date_joined": {"Value": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}

Hello Azure Cosmos DB - program pro import po sobě jdoucích záznam má hello následující další rozšířené možnosti:

1. Počet požadavků na paralelní: nástroj hello výchozí too2 paralelní požadavky. Pokud toobe dokumenty hello importovat malé, zvažte vyvolání hello počet paralelní požadavků. Všimněte si, že pokud toto číslo se vyvolá příliš mnoho hello importu může dojít k omezení šířky pásma.
2. Zakázat automatické generování Id: Pokud každý dokument toobe importovat obsahuje id pole, pak výběrem této možnosti můžete zvýšit výkon. Chybí hodnota v poli jedinečné id dokumenty nebudou importovány.
3. Aktualizace stávající dokumenty: toonot výchozí nástroj hello nahrazení stávající dokumenty docházet ke konfliktům id. Výběrem této možnosti umožníte přepsal stávající dokumenty s odpovídajícím ID. Tato funkce je užitečná pro naplánované data migrace, které aktualizovat existující dokumenty.
4. Počet opakovaných pokusů o selhání: Určuje hello počet tooretry hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
5. Interval opakování: Určuje, jak dlouho toowait mezi opakování hello připojení tooAzure Cosmos DB v případě přechodných chyb (například přerušení připojení k síti).
6. Režim připojení: Určuje hello připojení režimu toouse s Azure Cosmos DB. jsou k dispozici možnosti Hello DirectTcp, DirectHttps a brány. režimy Hello přímé připojení jsou rychlejší, zatímco hello brány režim je další brány firewall popisný jako pouze používá port 443.

![Snímek obrazovky databázi Cosmos Azure import sekvenční záznam rozšířené možnosti](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> Nástroj výchozí tooconnection režim DirectTcp import Hello. Pokud máte brány firewall problémy, přepněte tooconnection režimu bránu, vyžaduje jenom port 443.
> 
> 

## <a id="IndexingPolicy"></a>Při vytváření kolekcí Azure Cosmos DB zadat zásady indexování
Když migrace hello povolíte během importu kolekce toocreate nástroj, můžete zadat zásady indexování hello hello kolekcí. V hello rozšířené možnosti části hello Azure Cosmos DB hromadného importu a možnosti Azure DB Cosmos po sobě jdoucích záznam přejděte části toohello zásady indexování.

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy1.png)

Pomocí hello možnost Upřesnit zásady indexování, můžete vybrat indexování soubor zásad, ručně zadat zásady indexování nebo vybrat ze sady výchozích šablon (kliknutím pravým tlačítkem v hello indexování textbox zásad).

šablony zásad Hello, poskytuje nástroj hello jsou:

* Výchozí hodnota. Tato zásada je vhodné, když jste provádění dotazy na rovnost pro řetězce a pomocí klauzule ORDER BY, rozsah a dotazy na rovnost pro čísla. Tato zásada má nižší režijní náklady úložiště index než rozsah.
* Rozsah. Tato zásada je nejvhodnější, že používáte dotazy ORDER BY, rozsah a rovnost na čísla i řetězce. Tato zásada nemá vyšší index režijní náklady na úložiště než výchozí nebo hodnoty Hash.

![Snímek obrazovky Azure Cosmos DB indexování zásad rozšířené možnosti](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Pokud nezadáte zásady indexování, bude použita hello výchozí zásady. Další informace o indexování zásad najdete v tématu [Azure DB Cosmos indexování zásady](indexing-policies.md).
> 
> 

## <a name="export-toojson-file"></a>TooJSON souboru exportu
Hello Azure Cosmos DB JSON exportu vám umožní tooexport žádné hello dostupných možností tooa JSON souboru, který obsahuje pole dokumentů JSON. Nástroj Hello zpracuje hello exportu pro vás, nebo můžete zvolit tooview hello výsledné migrace příkaz a spusťte příkaz hello sami. Výsledný soubor JSON Hello mohou být uloženy místně nebo v Azure Blob storage.

![Snímek obrazovky z Azure Cosmos DB JSON možností exportu místního souboru](./media/import-data/jsontarget.png)

![Možností exportu snímek z Azure Cosmos DB JSON Azure Blob storage](./media/import-data/jsontarget2.png)

Volitelně můžete tooprettify hello výsledná formát JSON, což zvýší velikost hello hello výsledné dokumentu, při vytváření hello obsah další číselné vyjádření.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places toosee in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places toosee in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a>Pokročilá konfigurace
Na obrazovce pokročilou konfiguraci hello zadejte umístění hello toowhich souboru protokolu hello chcete všechny chyby zapsána. Hello následující pravidla platí toothis stránky:

1. Pokud název souboru není k dispozici, pak všechny chyby, bude vrácen na stránce výsledky hello.
2. Pokud je název souboru zadaný bez adresáře, pak hello soubor se vytvoří (nebo přepsat) v aktuálním adresáři prostředí hello.
3. Pokud vyberete existující soubor, a soubor hello budou přepsány, není žádná možnost připojit.

Pak zvolte zda toolog všechny, je důležité, nebo žádná chybová zpráva. Nakonec rozhodnete, jak často hello na obrazovce přenos zprávy bude aktualizováno o jejím průběhu.

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Potvrďte nastavení pro import a zobrazení příkazového řádku
1. Po zadání informace o zdroji, informace o cílové a pokročilou konfiguraci, zkontrolujte souhrn hello migrace a volitelně, zobrazení nebo zkopírování hello výsledná příkaz migrace (kopírování příkaz hello je užitečné tooautomate import operations):
   
    ![Snímek obrazovky souhrn](./media/import-data/summary.png)
   
    ![Snímek obrazovky souhrn](./media/import-data/summarycommand.png)
2. Jakmile budete spokojeni s možnosti zdroje a cíle, klikněte na možnost **Import**. Hello uplynulý čas, přenášená počtu a informace o selhání (Pokud název souboru v pokročilé konfiguraci hello neposkytli) bude aktualizovat jako hello importu je v procesu. Po dokončení můžete exportovat výsledky hello (např. toodeal s jeho selhání import).
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/viewresults.png)
3. Nový import, může spustit také udržuje hello stávající nastavení (například připojovací řetězec informace, zdroje a cíle volba atd.) nebo resetování všechny hodnoty.
   
    ![Snímek obrazovky z Azure Cosmos DB JSON možností exportu](./media/import-data/newimport.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Nainstalovaný nástroj pro migraci dat hello
> * Importovaná data z různých zdrojů dat.
> * Export z Azure Cosmos DB tooJSON

Teď můžete pokračovat dalším kurzu toohello a zjistěte, jak tooquery dat pomocí Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Jak tooquery dat?](../cosmos-db/tutorial-query-documentdb.md)
