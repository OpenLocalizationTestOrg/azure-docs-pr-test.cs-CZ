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
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a><span data-ttu-id="0747d-103">Azure Cosmos DB: Vývoj pomocí hello rozhraní Graph API v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="0747d-103">Azure Cosmos DB: Develop with hello Graph API in .NET</span></span>
<span data-ttu-id="0747d-104">Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0747d-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="0747d-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0747d-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="0747d-106">Tento kurz ukazuje, jak hello toocreate účtu Azure Cosmos DB pomocí portálu Azure a jak toocreate grafu databáze a kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0747d-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal and how toocreate a graph database and container.</span></span> <span data-ttu-id="0747d-107">Hello aplikace se pak vytvoří jednoduchý sociálních sítí s čtyři lidmi pomocí hello [rozhraní Graph API](graph-sdk-dotnet.md) (preview), pak prochází a dotazy pomocí Gremlin grafu hello.</span><span class="sxs-lookup"><span data-stu-id="0747d-107">hello application then creates a simple social network with four people using hello [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries hello graph using Gremlin.</span></span>

<span data-ttu-id="0747d-108">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="0747d-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0747d-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0747d-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="0747d-110">Vytvoření grafu databáze a kontejneru</span><span class="sxs-lookup"><span data-stu-id="0747d-110">Create a graph database and container</span></span>
> * <span data-ttu-id="0747d-111">Serializovat objekty too.NET vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-111">Serialize vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="0747d-112">Přidejte vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-112">Add vertices and edges</span></span>
> * <span data-ttu-id="0747d-113">Graf hello dotazu pomocí Gremlin</span><span class="sxs-lookup"><span data-stu-id="0747d-113">Query hello graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="0747d-114">Grafy v Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0747d-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="0747d-115">Můžete použít Azure Cosmos DB toocreate, aktualizace a grafy dotazu pomocí hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) knihovny.</span><span class="sxs-lookup"><span data-stu-id="0747d-115">You can use Azure Cosmos DB toocreate, update, and query graphs using hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="0747d-116">Knihovna Microsoft.Azure.Graph Hello poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` nad hello `DocumentClient` třídy tooexecute Gremlin dotazy.</span><span class="sxs-lookup"><span data-stu-id="0747d-116">hello Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of hello `DocumentClient` class tooexecute Gremlin queries.</span></span>

<span data-ttu-id="0747d-117">Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal.</span><span class="sxs-lookup"><span data-stu-id="0747d-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="0747d-118">Několik příkladů v tento článek tooget nabídneme vaše Začínáme s Gremlin.</span><span class="sxs-lookup"><span data-stu-id="0747d-118">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="0747d-119">V tématu [Gremlin dotazy](gremlin-support.md) podrobný návod Gremlin možnosti dostupné v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0747d-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0747d-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0747d-120">Prerequisites</span></span>
<span data-ttu-id="0747d-121">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="0747d-121">Please make sure you have hello following:</span></span>

* <span data-ttu-id="0747d-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0747d-122">An active Azure account.</span></span> <span data-ttu-id="0747d-123">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="0747d-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="0747d-124">Alternativně můžete použít hello [emulátoru Azure DocumentDB](local-emulator.md) pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0747d-124">Alternatively, you can use hello [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="0747d-125">Sadu [Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="0747d-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="0747d-126">Vytvoření databázového účtu</span><span class="sxs-lookup"><span data-stu-id="0747d-126">Create database account</span></span>

<span data-ttu-id="0747d-127">Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0747d-127">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="0747d-128">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="0747d-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="0747d-129">Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="0747d-129">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="0747d-130">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="0747d-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="0747d-131">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="0747d-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="0747d-132">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="0747d-132">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="0747d-133"><a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0747d-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="0747d-134">Otevřete na svém počítači sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="0747d-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="0747d-135">Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="0747d-135">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="0747d-136">V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)**, pojmenujte svůj projekt a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0747d-136">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="0747d-137">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="0747d-137">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="0747d-138">V hello **NuGet** , klikněte na **Procházet**a typ **Microsoft.Azure.Graphs** v hello vyhledávacího pole a kontrola hello **zahrnují předprodejní verze**.</span><span class="sxs-lookup"><span data-stu-id="0747d-138">In hello **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in hello search box, and check hello **Include prerelease versions**.</span></span>
6. <span data-ttu-id="0747d-139">V rámci hello výsledky najít **Microsoft.Azure.Graphs** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="0747d-139">Within hello results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="0747d-140">Pokud se zobrazí zpráva o Kontrola řešení toohello změny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="0747d-140">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="0747d-141">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="0747d-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="0747d-142">Hello `Microsoft.Azure.Graphs` knihovna poskytuje jeden rozšiřující metodu `CreateGremlinQuery<T>` pro provádění operací Gremlin.</span><span class="sxs-lookup"><span data-stu-id="0747d-142">hello `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="0747d-143">Gremlin je funkční programovací jazyk, který podporuje zápis operace (DML) a operace dotazů a traversal.</span><span class="sxs-lookup"><span data-stu-id="0747d-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="0747d-144">Několik příkladů v tento článek tooget nabídneme vaše Začínáme s Gremlin.</span><span class="sxs-lookup"><span data-stu-id="0747d-144">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="0747d-145">[Dotazy gremlin](gremlin-support.md) obsahuje podrobný návod Gremlin funkcí v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0747d-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="0747d-146"><a id="add-references"></a>Připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="0747d-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="0747d-147">Přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0747d-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="0747d-148">V dalším kroku head zpět toohello [portál Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="0747d-148">Next, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="0747d-149">Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="0747d-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span> 

<span data-ttu-id="0747d-150">V hello portálu Azure, přejděte tooyour Azure Cosmos DB účet, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="0747d-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="0747d-151">Zkopírujte z portálu hello hello URI a vložte ho přes `Endpoint` ve vlastnosti koncový bod hello výše.</span><span class="sxs-lookup"><span data-stu-id="0747d-151">Copy hello URI from hello portal and paste it over `Endpoint` in hello endpoint property above.</span></span> <span data-ttu-id="0747d-152">Potom kopie hello primární klíč z portálu hello a vložte jej do hello `AuthKey` vlastnost výše.</span><span class="sxs-lookup"><span data-stu-id="0747d-152">Then copy hello PRIMARY KEY from hello portal and paste it into hello `AuthKey` property above.</span></span> 

<span data-ttu-id="0747d-153">! [Snímek obrazovky portálu Azure hello používá hello kurz toocreate aplikace v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="0747d-153">![Screen shot of hello Azure portal used by hello tutorial toocreate a C# application.</span></span> <span data-ttu-id="0747d-154">Zobrazuje hello účtu databázi Azure Cosmos, zvýrazněným tlačítkem klíče hello navigační Azure Cosmos DB a hodnotami URI a primární klíč hello zvýrazněným hello okna klíče] [klíče]</span><span class="sxs-lookup"><span data-stu-id="0747d-154">Shows an Azure Cosmos DB account hello KEYS button highlighted on hello Azure Cosmos DB navigation , and hello URI and PRIMARY KEY values highlighted on hello Keys blade][keys]</span></span> 
 
## <span data-ttu-id="0747d-155"><a id="instantiate"></a>Vytvoření instance DocumentClient hello</span><span class="sxs-lookup"><span data-stu-id="0747d-155"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span> 
<span data-ttu-id="0747d-156">Dál vytvořte novou instanci třídy hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="0747d-156">Next, create a new instance of hello **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="0747d-157"><a id="create-database"></a>Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="0747d-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="0747d-158">Teď vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello  **DocumentClient** třídy z hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0747d-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="0747d-159">Vytvoření grafu.</span><span class="sxs-lookup"><span data-stu-id="0747d-159">Create a graph</span></span> 

<span data-ttu-id="0747d-160">Dále vytvořte kontejner grafu pomocí hello pomocí hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient**  třídy.</span><span class="sxs-lookup"><span data-stu-id="0747d-160">Next, create a graph container by using hello using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="0747d-161">Kolekce je kontejner entit grafu.</span><span class="sxs-lookup"><span data-stu-id="0747d-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="0747d-162"><a id="serializing"></a>Serializovat objekty too.NET vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-162"><a id="serializing"></a>Serialize vertices and edges too.NET objects</span></span>
<span data-ttu-id="0747d-163">Azure Cosmos DB používá hello [GraphSON přenosový formát](gremlin-support.md), která definuje schéma JSON pro vrcholy, okraje a vlastností.</span><span class="sxs-lookup"><span data-stu-id="0747d-163">Azure Cosmos DB uses hello [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="0747d-164">Hello .NET SDK služby Azure Cosmos DB zahrnuje JSON.NET jako závislost, a to umožňuje nám tooserialize či deserializace GraphSON do objektů .NET, které jsme můžete pracovat v kódu.</span><span class="sxs-lookup"><span data-stu-id="0747d-164">hello Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us tooserialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="0747d-165">Jako příklad umožňuje pracovat s jednoduché sociálních sítí čtyři osoby.</span><span class="sxs-lookup"><span data-stu-id="0747d-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="0747d-166">Podíváme na to, jak toocreate `Person` vrcholy, přidejte `Knows` vztahy mezi nimi, potom dotaz a procházení hello grafu toofind "friend friend" relace.</span><span class="sxs-lookup"><span data-stu-id="0747d-166">We look at how toocreate `Person` vertices, add `Knows` relationships between them, then query and traverse hello graph toofind "friend of friend" relationships.</span></span> 

<span data-ttu-id="0747d-167">Hello `Microsoft.Azure.Graphs.Elements` obor názvů obsahuje `Vertex`, `Edge`, `Property` a `VertexProperty` třídy pro deserializaci objektů definovaných toowell .NET aplikace GraphSON odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0747d-167">hello `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses toowell-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="0747d-168">Spustit Gremlin pomocí CreateGremlinQuery</span><span class="sxs-lookup"><span data-stu-id="0747d-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="0747d-169">Gremlin, například SQL, podporuje čtení, zápisu a operace dotazů.</span><span class="sxs-lookup"><span data-stu-id="0747d-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="0747d-170">Například hello následující fragment kódu ukazuje, jak toocreate vrcholy, hranami, provádět některé ukázkové dotazy s pomocí `CreateGremlinQuery<T>`a asynchronně iteraci v rámci těchto výsledků pomocí `ExecuteNextAsync` a ' HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="0747d-170">For example, hello following snippet shows how toocreate vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

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

## <a name="add-vertices-and-edges"></a><span data-ttu-id="0747d-171">Přidejte vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-171">Add vertices and edges</span></span>

<span data-ttu-id="0747d-172">Podívejme se na hello Gremlin příkazy uvedené v předcházející části hello podrobněji.</span><span class="sxs-lookup"><span data-stu-id="0747d-172">Let's look at hello Gremlin statements shown in hello preceding section more detail.</span></span> <span data-ttu-id="0747d-173">První jsme některé vrcholy pomocí na Gremlin `addV` metoda.</span><span class="sxs-lookup"><span data-stu-id="0747d-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="0747d-174">Například hello následující fragment vytváří vrchol "Thomas rodinu" typu "Osoba", s vlastnostmi pro křestní jméno, příjmení a stáří.</span><span class="sxs-lookup"><span data-stu-id="0747d-174">For example, hello following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

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

<span data-ttu-id="0747d-175">Poté vytvoříme některé okraje mezi tyto vrcholy pomocí na Gremlin `addE` metoda.</span><span class="sxs-lookup"><span data-stu-id="0747d-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

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

<span data-ttu-id="0747d-176">Aktualizujeme existující vrchol pomocí `properties` krok v Gremlin.</span><span class="sxs-lookup"><span data-stu-id="0747d-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="0747d-177">Jsme přeskočit hello volání tooexecute hello dotazu prostřednictvím `HasMoreResults` a `ExecuteNextAsync` pro hello zbytek hello příklady.</span><span class="sxs-lookup"><span data-stu-id="0747d-177">We skip hello call tooexecute hello query via `HasMoreResults` and `ExecuteNextAsync` for hello rest of hello examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="0747d-178">Můžete vložit okraje a vrcholy pomocí Gremlin na `drop` krok.</span><span class="sxs-lookup"><span data-stu-id="0747d-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="0747d-179">Zde je fragment kódu, který ukazuje, jak toodelete a vrchol a okraj.</span><span class="sxs-lookup"><span data-stu-id="0747d-179">Here's a snippet that shows how toodelete a vertex and an edge.</span></span> <span data-ttu-id="0747d-180">Všimněte si, že vyřazení vrchol provede kaskádové odstranění hello související okraje.</span><span class="sxs-lookup"><span data-stu-id="0747d-180">Note that dropping a vertex performs a cascading delete of hello associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a><span data-ttu-id="0747d-181">Dotaz hello grafu</span><span class="sxs-lookup"><span data-stu-id="0747d-181">Query hello graph</span></span>

<span data-ttu-id="0747d-182">Můžete provádět dotazy a také pomocí Gremlin traversals.</span><span class="sxs-lookup"><span data-stu-id="0747d-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="0747d-183">Například hello následující fragment kódu ukazuje, jak toocount hello počet vrcholy v grafu hello:</span><span class="sxs-lookup"><span data-stu-id="0747d-183">For example, hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="0747d-184">Můžete nastavit filtry, pomocí na Gremlin `has` a `hasLabel` kroky a zkombinovat pomocí `and`, `or`, a `not` toobuild složitější filtry:</span><span class="sxs-lookup"><span data-stu-id="0747d-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="0747d-185">Můžete promítnout některé vlastnosti ve výsledcích dotazů hello pomocí hello `values` kroku:</span><span class="sxs-lookup"><span data-stu-id="0747d-185">You can project certain properties in hello query results using hello `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="0747d-186">Zatím jste pouze viděli operátory dotazu, které fungují v některé z databází.</span><span class="sxs-lookup"><span data-stu-id="0747d-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="0747d-187">Grafy jsou rychlé a efektivní pro operace průchodu, pokud potřebujete toonavigate toorelated okraje a vrcholy.</span><span class="sxs-lookup"><span data-stu-id="0747d-187">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="0747d-188">Umožňuje najít všechny přátelích Thomas.</span><span class="sxs-lookup"><span data-stu-id="0747d-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="0747d-189">Provedeme to pomocí na Gremlin `outE` krok toofind všechny hello odesílací okrajů z Thomas a pak z těchto hran pomocí Gremlin na procházení toohello v vrcholy `inV` krok:</span><span class="sxs-lookup"><span data-stu-id="0747d-189">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="0747d-190">Další dotaz Hello provádí dvěma segmenty směrování toofind všechny Thomas. "přátelích přátel,", voláním `outE` a `inV` dvakrát.</span><span class="sxs-lookup"><span data-stu-id="0747d-190">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="0747d-191">Můžete vytvořit složitější dotazy a implementovat logiku traversal výkonné grafu pomocí Gremlin, včetně směšovací filtru výrazů, provádění opakování ve smyčce pomocí hello `loop` kroku a implementuje podmíněného navigační pomocí hello `choose` krok.</span><span class="sxs-lookup"><span data-stu-id="0747d-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="0747d-192">Další informace o co můžete dělat s [Gremlin podporu](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="0747d-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="0747d-193">Je to, v tomto kurzu pro Azure Cosmos DB je dokončena!</span><span class="sxs-lookup"><span data-stu-id="0747d-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="0747d-194">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="0747d-194">Clean up resources</span></span>

<span data-ttu-id="0747d-195">Pokud ale nebudete toocontinue toouse tuto aplikaci, použijte následující kroky toodelete všechny prostředky vytvořené v tomto kurzu v portálu Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="0747d-195">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>  

1. <span data-ttu-id="0747d-196">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0747d-196">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="0747d-197">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="0747d-197">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0747d-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0747d-198">Next Steps</span></span>

<span data-ttu-id="0747d-199">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="0747d-199">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0747d-200">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0747d-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="0747d-201">Vytvoření grafu databáze a kontejneru</span><span class="sxs-lookup"><span data-stu-id="0747d-201">Created a graph database and container</span></span>
> * <span data-ttu-id="0747d-202">Serializovaných objektů too.NET vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-202">Serialized vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="0747d-203">Přidání vrcholy a okraje</span><span class="sxs-lookup"><span data-stu-id="0747d-203">Added vertices and edges</span></span>
> * <span data-ttu-id="0747d-204">Graf dotazované hello pomocí Gremlin</span><span class="sxs-lookup"><span data-stu-id="0747d-204">Queried hello graph using Gremlin</span></span>

<span data-ttu-id="0747d-205">Teď můžete pomocí konzoly Gremlin vytvářet složitější dotazy a implementovat účinnou logiku procházení grafů.</span><span class="sxs-lookup"><span data-stu-id="0747d-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="0747d-206">Dotazování pomocí konzoly Gremlin</span><span class="sxs-lookup"><span data-stu-id="0747d-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
