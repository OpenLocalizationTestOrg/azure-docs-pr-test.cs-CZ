---
title: "Azure Cosmos DB: Vývoj pomocí hello DocumentDB rozhraní API v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toodevelop s rozhraním API Azure Cosmos DB DocumentDB pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 0d3d17afa782054c8fdf3cbac421e5a5d0a6800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a>Azure CosmosDB: Vývoj pomocí hello DocumentDB rozhraní API v rozhraní .NET

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento kurz ukazuje, jak toocreate účtu Azure Cosmos DB pomocí hello portálu Azure a pak vytvořte databázi dokumentů a kolekce s [klíč oddílu](documentdb-partition-data.md#partition-keys) pomocí hello [DocumentDB .NET API](documentdb-introduction.md). Definováním klíč oddílu, když vytvoříte kolekci, vaše aplikace je připravená tooscale pak moci bez obtíží s růstem vaše data. 

Tento kurz zahrnuje hello následující úlohy pomocí hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB
> * Vytvoření databáze a kolekce s klíčem oddílu
> * Vytvoření dokumentů JSON
> * Aktualizace dokumentu
> * Dotaz na dělené kolekce
> * Spuštění uložené procedury
> * Odstranění dokumentu
> * Odstranění databáze

## <a name="prerequisites"></a>Požadavky
Přesvědčte se, že máte následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 
    * Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu, pokud chcete toouse místní prostředí, které emuluje služby Azure DocumentDB hello pro účely vývoje.
* Sadu [Visual Studio](http://www.visualstudio.com/).

## <a name="create-an-azure-cosmos-db-account"></a>Vytvoření účtu služby Azure Cosmos DB

Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)
> * Měli jste účet Azure DocumentDB? Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).  
> * Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio
1. Otevřete na svém počítači sadu **Visual Studio**.
2. Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.
3. V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)**, pojmenujte svůj projekt a pak klikněte na tlačítko **OK**.
   ![Snímek obrazovky okna Nový projekt hello](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)

4. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**
    
    ![Snímek obrazovky znázorňující hello právo místní nabídky pro hello projektu](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. V hello **NuGet** , klikněte na **Procházet**a typ **documentdb** hello vyhledávacího pole.
<!---stopped here--->
6. V rámci hello výsledky najít **Microsoft.Azure.DocumentDB** a klikněte na tlačítko **nainstalovat**.
   ID balíčku Hello hello Azure Cosmos DB klientské knihovny je [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Snímek obrazovky hello nabídky NuGet pro vyhledání Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Pokud se zobrazí zpráva o Kontrola řešení toohello změny, klikněte na tlačítko **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.

## <a id="Connect"></a>Přidání odkazů tooyour projektu
Hello zbývající kroky v tento kurz zadejte hello DocumentDB API kód fragmenty požadované toocreate a aktualizace Azure Cosmos DB prostředky ve vašem projektu.

Nejprve přidejte tyto odkazy tooyour aplikace.
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Připojení aplikace

V dalším kroku přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Potom, head zpět toohello [portál Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč. Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.

V hello portálu Azure, přejděte tooyour Azure Cosmos DB účet, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.

Zkopírujte z portálu hello hello URI a vložte ho přes `<your endpoint URL>` v souboru program.cs hello. Pak hello primární klíč z portálu hello kopírování a vložení prostřednictvím `<your primary key>`. Se, zda text hello tooremove `<` a `>` z hodnoty.

![Snímek obrazovky portálu Azure hello používá toocreate kurzu NoSQL hello konzolovou aplikaci C#. Zobrazuje účet Azure Cosmos DB s hello klíče v okně účtu Azure Cosmos DB hello a hello URI a primární klíč zvýrazněnými hodnotami v okně klíče hello](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Vytvoření instance DocumentClient hello

Teď vytvořte novou instanci třídy hello **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <a id="create-database"></a>Vytvoření databáze

Dále vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello  **DocumentClient** třídy z hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md). Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Vyberte klíč oddílu 

Kolekce jsou kontejnery pro ukládání dokumentů. Jsou logické prostředky a můžete [span jeden nebo více fyzických oddílů](partition-data.md). A [klíč oddílu](documentdb-partition-data.md) je vlastnost (nebo cestu) v rámci vaší dokumenty, které je použité toodistribute data mezi servery hello nebo oddíly. Všechny dokumenty se stejným klíčem oddílu jsou uložené v hello hello stejného oddílu. 

Klíč oddílu je určení důležité rozhodnutí toomake před vytvořením kolekce. Klíče oddílů jsou vlastnosti (nebo cestu) v rámci vaší dokumenty, které se dají používat Azure Cosmos DB toodistribute data mezi více servery nebo oddíly. Cosmos DB hashuje hodnotu klíče oddílu hello a používá hello rozdělí výsledek toodetermine hello oddílu v které toostore hello dokumentu. Všechny dokumenty se stejným klíčem oddílu jsou uložené v hello hello stejného oddílu a klíče oddílů nelze změnit po vytvoření kolekce. 

V tomto kurzu vytvoříme klíč oddílu hello tooset příliš`/deviceId` , který hello všechna data hello pro jedno zařízení je uložený v jeden oddíl. Chcete-li toochoose klíč oddílu, který má velký počet hodnot, z nichž každý se používají v o hello stejnou frekvenci tooensure Cosmos DB můžete vyrovnávat zatížení jako data zvětšování a dosáhnout úplné propustnosti hello hello kolekce. 

Další informace o oddílech najdete v tématu [jak toopartition a škálování v Azure Cosmos DB?](partition-data.md) 

## <a id="CreateColl"></a>Vytvoření kolekce 

Teď, když jsme si vědomi naše klíč oddílu `/deviceId`, umožňuje vytvořit [kolekce](documentdb-resources.md#collections) pomocí hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient** třídy. Kolekce je kontejner dokumentů JSON a všechny přidružené logiky Javascriptové aplikace. 

> [!WARNING]
> Vytvoření kolekce hradí, jako jsou rezervování propustnost pro hello toocommunicate aplikací s Azure Cosmos DB. Další podrobnosti, navštivte naše [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here hello JSON property deviceId is used  
// as hello partition key toospread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Tato metoda díky rozhraní REST API volání tooAzure Cosmos DB a hello služby zřizuje počet oddílů založené na požadovaný propustnost hello. Výkon vašich potřeb momentální pomocí hello SDK nebo hello můžete změnit hello propustnost kolekce [portál Azure](set-throughput.md).

## <a id="CreateDoc"></a>Vytvoření dokumentů JSON
Umožňuje vložit některé dokumenty JSON do Azure Cosmos DB. A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metoda hello **DocumentClient** třídy. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Tato ukázka třída obsahuje čtení zařízení a tooinsert tooCreateDocumentAsync volání nové zařízení čtení do kolekce.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted 
// as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Čtení dat

Umožňuje číst dokument hello svým klíč oddílu a Id pomocí metody ReadDocumentAsync hello. Všimněte si, že čtení hello obsahují hodnotu PartitionKey (odpovídající toohello `x-ms-documentdb-partitionkey` hlavička požadavku v hello REST API).

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Aktualizace dat

Teď umožňuje aktualizovat některé data pomocí metody ReplaceDocumentAsync hello.

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Odstranění dat

Teď umožňuje odstranit dokument klíčem oddílu a id pomocí metody DeleteDocumentAsync hello.

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Dotaz na dělené kolekce

Když dotazujete dat v kolekcích oddílů, Azure Cosmos DB automaticky trasy hello oddíly query toohello odpovídající hodnoty klíče toohello oddílu zadané ve filtru hello (pokud existují). Tento dotaz je například směrované toojust hello oddílu obsahující hello klíč oddílu "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
Hello následující dotaz na klíč oddílu hello (DeviceId) nemá filtr a je fanned na oddíly tooall němž se spustí před hello oddílu indexu. Všimněte si, že máte toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v hello REST API) toohave hello SDK tooexecute dotazu napříč oddíly.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Provádění paralelního dotazu
Hello Azure Cosmos databáze DocumentDB SDK 1.9.0 a výše podpory možnosti provádění paralelního dotazu, které umožňují s nízkou latencí tooperform dotazy proti dělené kolekce, i v případě, že potřebují tootouch velký počet oddílů. Následující dotaz hello je například nakonfigurované toorun paralelně napříč oddíly.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
Provádění paralelního dotazu můžete spravovat pomocí ladění hello následující parametry:

* Nastavením `MaxDegreeOfParallelism`, můžete řídit hello stupně paralelního zpracování tedy hello maximální počet souběžných síťové připojení toohello kolekce oddílů. Pokud nastavíte příliš-1, hello stupně paralelního zpracování spravuje hello SDK. Pokud hello `MaxDegreeOfParallelism` není zadaný, nebo nastavte too0, což je výchozí hodnota hello, bude oddíly kolekce toohello na jedné síťové připojení.
* Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta. Pokud tento parametr vynecháte, nebo nastavte příliš-1, hello počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje hello SDK.

Zadané hello stejného stavu hello kolekce paralelní dotaz vrátí výsledky v hello stejné pořadí jako sériové provádění. Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), hello DocumentDB SDK vydá dotaz hello paralelně napříč oddíly a sloučí částečně seřazená má za následek hello klientské straně tooproduce globálně řazení výsledků.

## <a name="execute-stored-procedures"></a>Provedení uložené procedury
Nakonec můžete provést jednotlivé transakce na dokumenty s hello stejné ID zařízení, například pokud jste zachování agregace nebo hello nejnovější stav zařízení do jednoho dokumentu přidáním hello následující kód tooyour projektu.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

A je to! ty jsou hello hlavními součástmi Azure Cosmos DB aplikaci, která používá distribuční oddílu klíče tooefficiently škálování dat napříč oddíly.  

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené v tomto kurzu v hello portál Azure s hello následující kroky:

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na tlačítko hello jedinečný název hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující: 

> [!div class="checklist"]
> * Vytvoření účtu Azure Cosmos DB
> * Vytvořit databázi a kolekci s klíčem oddílu
> * Vytvoření dokumentů JSON
> * Aktualizovat dokumentu
> * Dotaz dělené kolekce
> * Byla spuštěna uložené procedury
> * Odstranit dokumentu
> * Odstranit databázi

Teď můžete pokračovat dalším kurzu toohello a importovat další data tooyour Cosmos DB účtu. 

> [!div class="nextstepaction"]
> [Importování dat do služby Azure Cosmos DB](import-data.md)
