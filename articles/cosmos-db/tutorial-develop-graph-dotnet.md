---
title: "Azure Cosmos DB: Vývoj pomocí hello rozhraní Graph API v rozhraní .NET | Microsoft Docs"
description: "Zjistěte, jak toodevelop s rozhraním API Azure Cosmos DB DocumentDB pomocí rozhraní .NET"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: 12e435d8169aeee6e818dac4a3b66c7a0ec5f2d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a>Azure Cosmos DB: Vývoj pomocí hello rozhraní Graph API v rozhraní .NET
Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft. Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB. 

Tento kurz ukazuje, jak hello toocreate účtu Azure Cosmos DB pomocí portálu Azure a jak toocreate grafu databáze a kontejneru. Hello aplikace se pak vytvoří jednoduchý sociálních sítí s čtyři lidmi pomocí hello [rozhraní Graph API](graph-sdk-dotnet.md) (preview), pak prochází a dotazy pomocí Gremlin grafu hello.

Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Vytvoření účtu služby Azure Cosmos DB 
> * Vytvoření grafu databáze a kontejneru
> * Serializovat objekty too.NET vrcholy a okraje
> * Přidejte vrcholy a okraje
> * Graf hello dotazu pomocí Gremlin

## <a name="graphs-in-azure-cosmos-db"></a>Grafy v Azure Cosmos DB
Můžete použít Azure Cosmos DB toocreate, aktualizace a grafy dotazu pomocí hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) knihovny. Knihovna Microsoft.Azure.Graph Hello poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` nad hello `DocumentClient` třídy tooexecute Gremlin dotazy.

Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal. Několik příkladů v tento článek tooget nabídneme vaše Začínáme s Gremlin. V tématu [Gremlin dotazy](gremlin-support.md) podrobný návod Gremlin možnosti dostupné v Azure Cosmos DB. 

## <a name="prerequisites"></a>Požadavky
Přesvědčte se, že máte následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 
    * Alternativně můžete použít hello [emulátoru Azure DocumentDB](local-emulator.md) pro účely tohoto kurzu.
* Sadu [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Vytvoření databázového účtu

Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.  

> [!TIP]
> * Již máte účet Azure Cosmos DB? Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)
> * Měli jste účet Azure DocumentDB? Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).  
> * Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio
1. Otevřete na svém počítači sadu **Visual Studio**.
2. Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.
3. V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)**, pojmenujte svůj projekt a pak klikněte na tlačítko **OK**.
4. V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**
5. V hello **NuGet** , klikněte na **Procházet**a typ **Microsoft.Azure.Graphs** v hello vyhledávacího pole a kontrola hello **zahrnují předprodejní verze**.
6. V rámci hello výsledky najít **Microsoft.Azure.Graphs** a klikněte na tlačítko **nainstalovat**.
   
   Pokud se zobrazí zpráva o Kontrola řešení toohello změny, klikněte na tlačítko **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.
   
    Hello `Microsoft.Azure.Graphs` knihovna poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` pro provádění operací Gremlin. Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal. Několik příkladů v tento článek tooget nabídneme vaše Začínáme s Gremlin. [Dotazy gremlin](gremlin-support.md) obsahuje podrobný návod Gremlin funkcí v Azure Cosmos DB.

## <a id="add-references"></a>Připojení aplikace

Přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
V dalším kroku head zpět toohello [portál Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč. Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace. 

V hello portálu Azure, přejděte tooyour Azure Cosmos DB účet, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**. 

Zkopírujte z portálu hello hello URI a vložte ho přes `Endpoint` ve vlastnosti koncový bod hello výše. Potom kopie hello primární klíč z portálu hello a vložte jej do hello `AuthKey` vlastnost výše. 

! [Snímek obrazovky portálu Azure hello používá hello kurz toocreate aplikace v jazyce C#. Zobrazuje hello účtu databázi Azure Cosmos, zvýrazněným tlačítkem klíče hello navigační Azure Cosmos DB a hodnotami URI a primární klíč hello zvýrazněným hello okna klíče] [klíče] 
 
## <a id="instantiate"></a>Vytvoření instance DocumentClient hello 
Dál vytvořte novou instanci třídy hello **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Vytvoření databáze 

Teď vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello  **DocumentClient** třídy z hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Vytvoření grafu. 

Dále vytvořte kontejner grafu pomocí hello pomocí hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient**  třídy. Kolekce je kontejner entit grafu. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Serializovat objekty too.NET vrcholy a okraje
Azure Cosmos DB používá hello [GraphSON přenosový formát](gremlin-support.md), která definuje schéma JSON pro vrcholy, okraje a vlastností. Hello .NET SDK služby Azure Cosmos DB zahrnuje JSON.NET jako závislost, a to umožňuje nám tooserialize či deserializace GraphSON do objektů .NET, které jsme můžete pracovat v kódu.

Jako příklad umožňuje pracovat s jednoduché sociálních sítí čtyři osoby. Podíváme na to, jak toocreate `Person` vrcholy, přidejte `Knows` vztahy mezi nimi, potom dotaz a procházení hello grafu toofind "friend friend" relace. 

Hello `Microsoft.Azure.Graphs.Elements` obor názvů obsahuje `Vertex`, `Edge`, `Property` a `VertexProperty` třídy pro deserializaci objektů definovaných toowell .NET aplikace GraphSON odpovědi.

## <a name="run-gremlin-using-creategremlinquery"></a>Spustit Gremlin pomocí CreateGremlinQuery
Gremlin, například SQL, podporuje čtení, zápisu a operace dotazů. Například hello následující fragment kódu ukazuje, jak toocreate vrcholy, hranami, provádět některé ukázkové dotazy s pomocí `CreateGremlinQuery<T>`a asynchronně iteraci v rámci těchto výsledků pomocí `ExecuteNextAsync` a ' HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Přidejte vrcholy a okraje

Podívejme se na hello Gremlin příkazy uvedené v předcházející části hello podrobněji. První jsme některé vrcholy pomocí na Gremlin `addV` metoda. Například hello následující fragment vytváří vrchol "Thomas rodinu" typu "Osoba", s vlastnostmi pro křestní jméno, příjmení a stáří.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Poté vytvoříme některé okraje mezi tyto vrcholy pomocí na Gremlin `addE` metoda. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Aktualizujeme existující vrchol pomocí `properties` krok v Gremlin. Jsme přeskočit hello volání tooexecute hello dotazu prostřednictvím `HasMoreResults` a `ExecuteNextAsync` pro hello zbytek hello příklady.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Můžete vložit okraje a vrcholy pomocí Gremlin na `drop` krok. Zde je fragment kódu, který ukazuje, jak toodelete a vrchol a okraj. Všimněte si, že vyřazení vrchol provede kaskádové odstranění hello související okraje.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a>Dotaz hello grafu

Můžete provádět dotazy a také pomocí Gremlin traversals. Například hello následující fragment kódu ukazuje, jak toocount hello počet vrcholy v grafu hello:

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` toobuild složitější filtry:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Můžete promítnout některé vlastnosti ve výsledcích dotazů hello pomocí hello `values` kroku:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází. Grafy jsou rychlé a efektivní pro operace průchodu, pokud potřebujete toonavigate toorelated okraje a vrcholy. Umožňuje najít všechny přátelích Thomas. Provedeme to pomocí na Gremlin `outE` krok toofind všechny hello odesílací okrajů z Thomas a pak z těchto hran pomocí Gremlin na procházení toohello v vrcholy `inV` krok:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Další dotaz Hello provádí dvěma segmenty směrování toofind všechny Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně směšovací filtru výrazů, provádění opakování ve smyčce pomocí hello `loop` kroku a implementuje podmíněného navigační pomocí hello `choose` krok. Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!

Je to, v tomto kurzu pro Azure Cosmos DB je dokončena! 

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello.  

1. V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili. 
2. Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu provedete krok hello následující:

> [!div class="checklist"]
> * Vytvoření účtu Azure Cosmos DB 
> * Vytvoření grafu databáze a kontejneru
> * Serializovaných objektů too.NET vrcholy a okraje
> * Přidání vrcholy a okraje
> * Graf dotazované hello pomocí Gremlin

Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů. 

> [!div class="nextstepaction"]
> [Dotazování pomocí konzoly Gremlin](tutorial-query-graph.md)
