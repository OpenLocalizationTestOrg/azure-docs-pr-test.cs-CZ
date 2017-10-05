---
title: "Azure Cosmos DB: Vývoj pomocí Graph API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB DocumentDB pomocí rozhraní .NET"
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
ms.openlocfilehash: eeaa0c4f84a408815371742334d2ba7ce600b72f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a><span data-ttu-id="69d88-103">Azure Cosmos DB: Vývoj pomocí Graph API v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="69d88-103">Azure Cosmos DB: Develop with the Graph API in .NET</span></span>
<span data-ttu-id="69d88-104">Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="69d88-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="69d88-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="69d88-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="69d88-106">Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a vytvoření grafu databáze a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="69d88-106">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal and how to create a graph database and container.</span></span> <span data-ttu-id="69d88-107">Aplikace se pak vytvoří jednoduchý sociálních sítí s čtyři lidmi pomocí [rozhraní Graph API](graph-sdk-dotnet.md) (preview), pak prochází a dotazy pomocí Gremlin grafu.</span><span class="sxs-lookup"><span data-stu-id="69d88-107">The application then creates a simple social network with four people using the [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries the graph using Gremlin.</span></span>

<span data-ttu-id="69d88-108">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="69d88-108">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="69d88-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="69d88-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="69d88-110">Vytvoření grafu databáze a kontejneru</span><span class="sxs-lookup"><span data-stu-id="69d88-110">Create a graph database and container</span></span>
> * <span data-ttu-id="69d88-111">Serializovat vrcholy a okrajů pro objekty .NET</span><span class="sxs-lookup"><span data-stu-id="69d88-111">Serialize vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="69d88-112">Přidejte vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="69d88-112">Add vertices and edges</span></span>
> * <span data-ttu-id="69d88-113">Dotaz pomocí Gremlin grafu</span><span class="sxs-lookup"><span data-stu-id="69d88-113">Query the graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="69d88-114">Grafy v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="69d88-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="69d88-115">Můžete vytvářet, aktualizovat a dotaz grafy pomocí Azure Cosmos DB [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) knihovny.</span><span class="sxs-lookup"><span data-stu-id="69d88-115">You can use Azure Cosmos DB to create, update, and query graphs using the [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="69d88-116">Knihovna Microsoft.Azure.Graph poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` na `DocumentClient` třída na provedení dotazů Gremlin.</span><span class="sxs-lookup"><span data-stu-id="69d88-116">The Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of the `DocumentClient` class to execute Gremlin queries.</span></span>

<span data-ttu-id="69d88-117">Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal.</span><span class="sxs-lookup"><span data-stu-id="69d88-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="69d88-118">Mezi příklady v tomto článku získat vaše Začínáme s Gremlin nabídneme.</span><span class="sxs-lookup"><span data-stu-id="69d88-118">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="69d88-119">V tématu [Gremlin dotazy](gremlin-support.md) podrobný návod Gremlin možnosti dostupné v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="69d88-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="69d88-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="69d88-120">Prerequisites</span></span>
<span data-ttu-id="69d88-121">Ujistěte se prosím, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="69d88-121">Please make sure you have the following:</span></span>

* <span data-ttu-id="69d88-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="69d88-122">An active Azure account.</span></span> <span data-ttu-id="69d88-123">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="69d88-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="69d88-124">Alternativně můžete pro tento kurz použít [emulátor Azure DocumentDB](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="69d88-124">Alternatively, you can use the [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="69d88-125">Sadu [Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="69d88-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="69d88-126">Vytvoření databázového účtu</span><span class="sxs-lookup"><span data-stu-id="69d88-126">Create database account</span></span>

<span data-ttu-id="69d88-127">Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="69d88-127">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="69d88-128">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="69d88-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="69d88-129">Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="69d88-129">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="69d88-130">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="69d88-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="69d88-131">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="69d88-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="69d88-132">Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="69d88-132">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="69d88-133"><a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69d88-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="69d88-134">Otevřete na svém počítači sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="69d88-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="69d88-135">V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="69d88-135">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="69d88-136">V **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)** , pojmenujte svůj projekt a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="69d88-136">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="69d88-137">V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="69d88-137">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="69d88-138">V **NuGet** , klikněte na **Procházet**a typ **Microsoft.Azure.Graphs** do vyhledávacího pole a kontroly **zahrnují předprodejní verze**.</span><span class="sxs-lookup"><span data-stu-id="69d88-138">In the **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in the search box, and check the **Include prerelease versions**.</span></span>
6. <span data-ttu-id="69d88-139">Ve výsledcích hledání **Microsoft.Azure.Graphs** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="69d88-139">Within the results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="69d88-140">Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="69d88-140">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="69d88-141">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="69d88-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="69d88-142">`Microsoft.Azure.Graphs` Knihovna poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` pro provádění operací Gremlin.</span><span class="sxs-lookup"><span data-stu-id="69d88-142">The `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="69d88-143">Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal.</span><span class="sxs-lookup"><span data-stu-id="69d88-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="69d88-144">Mezi příklady v tomto článku získat vaše Začínáme s Gremlin nabídneme.</span><span class="sxs-lookup"><span data-stu-id="69d88-144">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="69d88-145">[Dotazy gremlin](gremlin-support.md) obsahuje podrobný návod Gremlin funkcí v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="69d88-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="69d88-146"><a id="add-references"></a>Připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="69d88-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="69d88-147">Přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="69d88-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="69d88-148">V dalším kroku head zpět do [portál Azure](https://portal.azure.com) získat adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="69d88-148">Next, head back to the [Azure portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="69d88-149">Adresa URL koncového bodu a primární klíč jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="69d88-149">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span> 

<span data-ttu-id="69d88-150">Na portálu Azure přejděte ke svému účtu Azure Cosmos DB, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="69d88-150">In the Azure portal, navigate to your Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="69d88-151">Zkopírujte URI z portálu a vložte ji přes `Endpoint` ve výše uvedené vlastnosti koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="69d88-151">Copy the URI from the portal and paste it over `Endpoint` in the endpoint property above.</span></span> <span data-ttu-id="69d88-152">Poté zkopírujte primární klíč z portálu a vložte ji do `AuthKey` vlastnost výše.</span><span class="sxs-lookup"><span data-stu-id="69d88-152">Then copy the PRIMARY KEY from the portal and paste it into the `AuthKey` property above.</span></span> 

<span data-ttu-id="69d88-153">! [Snímek obrazovky portálu Azure používá v kurzu k vytvoření aplikace v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="69d88-153">![Screen shot of the Azure portal used by the tutorial to create a C# application.</span></span> <span data-ttu-id="69d88-154">Zobrazuje účet Azure DB Cosmos tlačítkem klíče v Azure Cosmos DB navigaci a hodnotami URI a primární klíč v okně klíče] [klíče]</span><span class="sxs-lookup"><span data-stu-id="69d88-154">Shows an Azure Cosmos DB account the KEYS button highlighted on the Azure Cosmos DB navigation , and the URI and PRIMARY KEY values highlighted on the Keys blade][keys]</span></span> 
 
## <span data-ttu-id="69d88-155"><a id="instantiate"></a>Vytvoření instance DocumentClient</span><span class="sxs-lookup"><span data-stu-id="69d88-155"><a id="instantiate"></a>Instantiate the DocumentClient</span></span> 
<span data-ttu-id="69d88-156">Dál vytvořte novou instanci třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="69d88-156">Next, create a new instance of the **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="69d88-157"><a id="create-database"></a>Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="69d88-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="69d88-158">Teď vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metodu  **DocumentClient** třídy z [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="69d88-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class from the [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="69d88-159">Vytvoření grafu.</span><span class="sxs-lookup"><span data-stu-id="69d88-159">Create a graph</span></span> 

<span data-ttu-id="69d88-160">Dále vytvořte kontejner grafu pomocí použití [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metodu **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="69d88-160">Next, create a graph container by using the using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="69d88-161">Kolekce je kontejner entit grafu.</span><span class="sxs-lookup"><span data-stu-id="69d88-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="69d88-162"><a id="serializing"></a>Serializovat vrcholy a okrajů pro objekty .NET</span><span class="sxs-lookup"><span data-stu-id="69d88-162"><a id="serializing"></a>Serialize vertices and edges to .NET objects</span></span>
<span data-ttu-id="69d88-163">Používá Azure Cosmos DB [GraphSON přenosový formát](gremlin-support.md), která definuje schéma JSON pro vrcholy, okraje a vlastností.</span><span class="sxs-lookup"><span data-stu-id="69d88-163">Azure Cosmos DB uses the [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="69d88-164">.NET SDK služby Azure Cosmos DB zahrnuje JSON.NET jako závislost, a to umožňuje nám k serializaci nebo deserializaci GraphSON do objektů .NET, které jsme můžete pracovat v kódu.</span><span class="sxs-lookup"><span data-stu-id="69d88-164">The Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us to serialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="69d88-165">Jako příklad umožňuje pracovat s jednoduché sociálních sítí čtyři osoby.</span><span class="sxs-lookup"><span data-stu-id="69d88-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="69d88-166">Podíváme na tom, jak vytvořit `Person` vrcholy, přidejte `Knows` vztahy mezi nimi, potom dotaz a procházení graf tak, aby najde "friend friend" relace.</span><span class="sxs-lookup"><span data-stu-id="69d88-166">We look at how to create `Person` vertices, add `Knows` relationships between them, then query and traverse the graph to find "friend of friend" relationships.</span></span> 

<span data-ttu-id="69d88-167">`Microsoft.Azure.Graphs.Elements` Obor názvů obsahuje `Vertex`, `Edge`, `Property` a `VertexProperty` třídy pro deserializaci GraphSON odpovědí dobře definované objekty .NET.</span><span class="sxs-lookup"><span data-stu-id="69d88-167">The `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses to well-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="69d88-168">Spustit Gremlin pomocí CreateGremlinQuery</span><span class="sxs-lookup"><span data-stu-id="69d88-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="69d88-169">Gremlin, například SQL, podporuje čtení, zápisu a operace dotazů.</span><span class="sxs-lookup"><span data-stu-id="69d88-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="69d88-170">Například následující fragment kódu ukazuje, jak vytvořit vrcholy, okraje, provádět některé ukázkové dotazy s pomocí `CreateGremlinQuery<T>`a asynchronně iteraci v rámci těchto výsledků pomocí `ExecuteNextAsync` a ' HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="69d88-170">For example, the following snippet shows how to create vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

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

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
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

## <a name="add-vertices-and-edges"></a><span data-ttu-id="69d88-171">Přidejte vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="69d88-171">Add vertices and edges</span></span>

<span data-ttu-id="69d88-172">Podívejme se na Gremlin příkazy uvedené v předchozí části podrobněji.</span><span class="sxs-lookup"><span data-stu-id="69d88-172">Let's look at the Gremlin statements shown in the preceding section more detail.</span></span> <span data-ttu-id="69d88-173">První jsme některé vrcholy pomocí na Gremlin `addV` metoda.</span><span class="sxs-lookup"><span data-stu-id="69d88-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="69d88-174">Například následující fragment vytváří vrchol "Thomas rodinu" typu "Osoba", s vlastnostmi pro křestní jméno, příjmení a stáří.</span><span class="sxs-lookup"><span data-stu-id="69d88-174">For example, the following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

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

<span data-ttu-id="69d88-175">Poté vytvoříme některé okraje mezi tyto vrcholy pomocí na Gremlin `addE` metoda.</span><span class="sxs-lookup"><span data-stu-id="69d88-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

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

<span data-ttu-id="69d88-176">Aktualizujeme existující vrchol pomocí `properties` krok v Gremlin.</span><span class="sxs-lookup"><span data-stu-id="69d88-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="69d88-177">Jsme přeskočit volání spuštění dotazu prostřednictvím `HasMoreResults` a `ExecuteNextAsync` pro zbytek příklady.</span><span class="sxs-lookup"><span data-stu-id="69d88-177">We skip the call to execute the query via `HasMoreResults` and `ExecuteNextAsync` for the rest of the examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="69d88-178">Můžete vložit okraje a vrcholy pomocí Gremlin na `drop` krok.</span><span class="sxs-lookup"><span data-stu-id="69d88-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="69d88-179">Zde je fragment kódu, který ukazuje, jak odstranit vrchol a okraj.</span><span class="sxs-lookup"><span data-stu-id="69d88-179">Here's a snippet that shows how to delete a vertex and an edge.</span></span> <span data-ttu-id="69d88-180">Pamatujte, že vyřazení vrchol kaskádové odstranění přidružené okrajů.</span><span class="sxs-lookup"><span data-stu-id="69d88-180">Note that dropping a vertex performs a cascading delete of the associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a><span data-ttu-id="69d88-181">Dotaz grafu</span><span class="sxs-lookup"><span data-stu-id="69d88-181">Query the graph</span></span>

<span data-ttu-id="69d88-182">Můžete provádět dotazy a také pomocí Gremlin traversals.</span><span class="sxs-lookup"><span data-stu-id="69d88-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="69d88-183">Například následující fragment kódu ukazuje, jak můžete zjistit, kolik vrcholy v grafu:</span><span class="sxs-lookup"><span data-stu-id="69d88-183">For example, the following snippet shows how to count the number of vertices in the graph:</span></span>

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="69d88-184">Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` k vytvoření složitějších filtrů:</span><span class="sxs-lookup"><span data-stu-id="69d88-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` to build more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="69d88-185">Můžete promítnout některé vlastnosti ve výsledcích dotazu pomocí `values` kroku:</span><span class="sxs-lookup"><span data-stu-id="69d88-185">You can project certain properties in the query results using the `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="69d88-186">Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází.</span><span class="sxs-lookup"><span data-stu-id="69d88-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="69d88-187">Grafy jsou rychlé a efektivní pro operace traversal, když potřebujete přejít na související okraje a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="69d88-187">Graphs are fast and efficient for traversal operations when you need to navigate to related edges and vertices.</span></span> <span data-ttu-id="69d88-188">Umožňuje najít všechny přátelích Thomas.</span><span class="sxs-lookup"><span data-stu-id="69d88-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="69d88-189">Provedeme to pomocí na Gremlin `outE` krok najdete všechny odesílací okrajů z Thomas pak procházení k v vrcholy z těchto hran pomocí Gremlin na `inV` krok:</span><span class="sxs-lookup"><span data-stu-id="69d88-189">We do this by using Gremlin's `outE` step to find all the out-edges from Thomas, then traversing to the in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="69d88-190">Další dotaz provádí dvěma segmenty směrování k vyhledání všech Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát.</span><span class="sxs-lookup"><span data-stu-id="69d88-190">The next query performs two hops to find all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="69d88-191">Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně kombinování filtru výrazů, provádění opakování pomocí `loop` kroku a implementuje pomocí podmíněného navigace `choose` krok.</span><span class="sxs-lookup"><span data-stu-id="69d88-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using the `loop` step, and implementing conditional navigation using the `choose` step.</span></span> <span data-ttu-id="69d88-192">Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="69d88-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="69d88-193">Je to, v tomto kurzu pro Azure Cosmos DB je dokončena!</span><span class="sxs-lookup"><span data-stu-id="69d88-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="69d88-194">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="69d88-194">Clean up resources</span></span>

<span data-ttu-id="69d88-195">Pokud nebudete tuto aplikaci nadále používat, pomocí následujícího postupu odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="69d88-195">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span>  

1. <span data-ttu-id="69d88-196">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="69d88-196">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="69d88-197">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="69d88-197">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69d88-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69d88-198">Next Steps</span></span>

<span data-ttu-id="69d88-199">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="69d88-199">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="69d88-200">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="69d88-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="69d88-201">Vytvoření grafu databáze a kontejneru</span><span class="sxs-lookup"><span data-stu-id="69d88-201">Created a graph database and container</span></span>
> * <span data-ttu-id="69d88-202">Serializovaná vrcholy a okrajů pro objekty .NET</span><span class="sxs-lookup"><span data-stu-id="69d88-202">Serialized vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="69d88-203">Přidání vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="69d88-203">Added vertices and edges</span></span>
> * <span data-ttu-id="69d88-204">Dotaz pomocí Gremlin grafu</span><span class="sxs-lookup"><span data-stu-id="69d88-204">Queried the graph using Gremlin</span></span>

<span data-ttu-id="69d88-205">Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů.</span><span class="sxs-lookup"><span data-stu-id="69d88-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="69d88-206">Dotazování pomocí konzoly Gremlin</span><span class="sxs-lookup"><span data-stu-id="69d88-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
