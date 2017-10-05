---
title: "Azure Cosmos DB: Vývoj s DocumentDB rozhraní API v rozhraní .NET | Microsoft Docs"
description: "Naučte se vyvíjet s rozhraním API Azure Cosmos DB DocumentDB pomocí rozhraní .NET"
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
ms.openlocfilehash: 2eed74ae9bd173b0944ec190dfe5d9a4bdc54c37
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmosdb-develop-with-the-documentdb-api-in-net"></a><span data-ttu-id="442ae-103">Azure CosmosDB: Vývoj s DocumentDB rozhraní API v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="442ae-103">Azure CosmosDB: Develop with the DocumentDB API in .NET</span></span>

<span data-ttu-id="442ae-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="442ae-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="442ae-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="442ae-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="442ae-106">Tento kurz ukazuje, jak vytvořit účet Azure Cosmos DB pomocí portálu Azure a pak vytvořte databázi dokumentů a kolekce s [klíč oddílu](documentdb-partition-data.md#partition-keys) pomocí [DocumentDB .NET API](documentdb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="442ae-106">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal, and then create a document database and collection with a [partition key](documentdb-partition-data.md#partition-keys) using the [DocumentDB .NET API](documentdb-introduction.md).</span></span> <span data-ttu-id="442ae-107">Definováním klíč oddílu, když vytvoříte kolekci, je připraven aplikace pak moci bez obtíží škálujte podle rozšiřujícího se vaše data.</span><span class="sxs-lookup"><span data-stu-id="442ae-107">By defining a partition key when you create a collection, your application is prepared to scale effortlessly as your data grows.</span></span> 

<span data-ttu-id="442ae-108">Tento kurz se zaměřuje na tyto úlohy pomocí [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="442ae-108">This tutorial covers the following tasks by using the [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="442ae-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="442ae-109">Create an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="442ae-110">Vytvoření databáze a kolekce s klíčem oddílu</span><span class="sxs-lookup"><span data-stu-id="442ae-110">Create a database and collection with a partition key</span></span>
> * <span data-ttu-id="442ae-111">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="442ae-111">Create JSON documents</span></span>
> * <span data-ttu-id="442ae-112">Aktualizace dokumentu</span><span class="sxs-lookup"><span data-stu-id="442ae-112">Update a document</span></span>
> * <span data-ttu-id="442ae-113">Dotaz na dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="442ae-113">Query partitioned collections</span></span>
> * <span data-ttu-id="442ae-114">Spuštění uložené procedury</span><span class="sxs-lookup"><span data-stu-id="442ae-114">Run stored procedures</span></span>
> * <span data-ttu-id="442ae-115">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="442ae-115">Delete a document</span></span>
> * <span data-ttu-id="442ae-116">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="442ae-116">Delete a database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="442ae-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="442ae-117">Prerequisites</span></span>
<span data-ttu-id="442ae-118">Ujistěte se prosím, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="442ae-118">Please make sure you have the following:</span></span>

* <span data-ttu-id="442ae-119">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="442ae-119">An active Azure account.</span></span> <span data-ttu-id="442ae-120">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="442ae-120">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="442ae-121">Alternativně můžete použít [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu, pokud chcete použít místní prostředí, které emuluje služby Azure DocumentDB pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="442ae-121">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial if you'd like to use a local environment that emulates the Azure DocumentDB service for development purposes.</span></span>
* <span data-ttu-id="442ae-122">Sadu [Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="442ae-122">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="442ae-123">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="442ae-123">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="442ae-124">Začněme vytvořením účtu Azure Cosmos DB na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="442ae-124">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>

> [!TIP]
> * <span data-ttu-id="442ae-125">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="442ae-125">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="442ae-126">Pokud ano, přeskočit na [nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="442ae-126">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="442ae-127">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="442ae-127">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="442ae-128">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit na [nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="442ae-128">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="442ae-129">Pokud používáte emulátor DB Cosmos Azure, postupujte podle kroků v [emulátoru DB Cosmos Azure](local-emulator.md) nastavit emulátoru a přeskočit na [nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="442ae-129">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="442ae-130"><a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="442ae-130"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="442ae-131">Otevřete na svém počítači sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="442ae-131">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="442ae-132">V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="442ae-132">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="442ae-133">V **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)** , pojmenujte svůj projekt a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="442ae-133">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="442ae-134">![Snímek obrazovky okna Nový projekt](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="442ae-134">![Screen shot of the New Project window](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span></span>

4. <span data-ttu-id="442ae-135">V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="442ae-135">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Snímek obrazovky místní nabídky projektu](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="442ae-137">V **NuGet** , klikněte na **Procházet**a typ **documentdb** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="442ae-137">In the **NuGet** tab, click **Browse**, and type **documentdb** in the search box.</span></span>
<!---stopped here--->
6. <span data-ttu-id="442ae-138">Najděte ve výsledcích **Microsoft.Azure.DocumentDB** a klikněte na **Nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="442ae-138">Within the results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="442ae-139">Je třeba ID balíčku klientské knihovny Azure Cosmos DB [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="442ae-139">The package ID for the Azure Cosmos DB Client Library is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span></span>
   <span data-ttu-id="442ae-140">![Snímek obrazovky nabídky NuGet pro vyhledání Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="442ae-140">![Screen shot of the NuGet Menu for finding Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="442ae-141">Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="442ae-141">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="442ae-142">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="442ae-142">If you get a message about license acceptance, click **I accept**.</span></span>

## <span data-ttu-id="442ae-143"><a id="Connect"></a>Přidejte odkazy na projekt</span><span class="sxs-lookup"><span data-stu-id="442ae-143"><a id="Connect"></a>Add references to your project</span></span>
<span data-ttu-id="442ae-144">Příklady zbývajících kroků v tomto kurzu poskytují fragmenty kódu rozhraní API DocumentDB nutná k vytváření a aktualizují Azure Cosmos DB prostředky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="442ae-144">The remaining steps in this tutorial provide the DocumentDB API code snippets required to create and update Azure Cosmos DB resources in your project.</span></span>

<span data-ttu-id="442ae-145">Nejprve přidejte tyto odkazy na aplikace.</span><span class="sxs-lookup"><span data-stu-id="442ae-145">First, add these references to your application.</span></span>
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <span data-ttu-id="442ae-146"><a id="add-references"></a>Připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="442ae-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="442ae-147">V dalším kroku přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="442ae-147">Next, add these two constants and your *client* variable in your application.</span></span>

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

<span data-ttu-id="442ae-148">Potom head zpátky [portál Azure](https://portal.azure.com) získat adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="442ae-148">Then, head back to the [Azure portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="442ae-149">Adresa URL koncového bodu a primární klíč jsou potřeba k tomu, aby aplikace věděla, kam se má připojit, a aby služba Azure Cosmos DB důvěřovala připojení aplikace.</span><span class="sxs-lookup"><span data-stu-id="442ae-149">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="442ae-150">Na portálu Azure přejděte ke svému účtu Azure Cosmos DB, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="442ae-150">In the Azure portal, navigate to your Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span>

<span data-ttu-id="442ae-151">Zkopírujte URI z portálu a vložte ji přes `<your endpoint URL>` v souboru program.cs.</span><span class="sxs-lookup"><span data-stu-id="442ae-151">Copy the URI from the portal and paste it over `<your endpoint URL>` in the program.cs file.</span></span> <span data-ttu-id="442ae-152">Poté zkopírujte primární klíč z portálu a vložte ji přes `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="442ae-152">Then copy the PRIMARY KEY from the portal and paste it over `<your primary key>`.</span></span> <span data-ttu-id="442ae-153">Nezapomeňte odebrat `<` a `>` z hodnoty.</span><span class="sxs-lookup"><span data-stu-id="442ae-153">Be sure to remove the `<` and `>` from your values.</span></span>

![Snímek obrazovky portálu Azure používá v kurzu NoSQL k vytvoření konzolové aplikace jazyka C#.](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <span data-ttu-id="442ae-156"><a id="instantiate"></a>Vytvoření instance DocumentClient</span><span class="sxs-lookup"><span data-stu-id="442ae-156"><a id="instantiate"></a>Instantiate the DocumentClient</span></span>

<span data-ttu-id="442ae-157">Teď vytvořte novou instanci třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="442ae-157">Now, create a new instance of the **DocumentClient**.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <span data-ttu-id="442ae-158"><a id="create-database"></a>Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="442ae-158"><a id="create-database"></a>Create a database</span></span>

<span data-ttu-id="442ae-159">Dále vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metodu  **DocumentClient** třídy z [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="442ae-159">Next, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class from the [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="442ae-160">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="442ae-160">A database is the logical container of JSON document storage partitioned across collections.</span></span>

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a><span data-ttu-id="442ae-161">Vyberte klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="442ae-161">Decide on a partition key</span></span> 

<span data-ttu-id="442ae-162">Kolekce jsou kontejnery pro ukládání dokumentů.</span><span class="sxs-lookup"><span data-stu-id="442ae-162">Collections are containers for storing documents.</span></span> <span data-ttu-id="442ae-163">Jsou logické prostředky a můžete [span jeden nebo více fyzických oddílů](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="442ae-163">They are logical resources and can [span one or more physical partitions](partition-data.md).</span></span> <span data-ttu-id="442ae-164">A [klíč oddílu](documentdb-partition-data.md) je vlastnost (nebo cestu) v rámci vaší dokumenty, které slouží k distribuci dat mezi servery nebo oddíly.</span><span class="sxs-lookup"><span data-stu-id="442ae-164">A [partition key](documentdb-partition-data.md) is a property (or path) within your documents that is used to distribute your data among the servers or partitions.</span></span> <span data-ttu-id="442ae-165">Všechny dokumenty se stejným klíčem oddílu ukládají do stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="442ae-165">All documents with the same partition key are stored in the same partition.</span></span> 

<span data-ttu-id="442ae-166">Rozhodnutí o důležité, aby předtím, než vytvoříte kolekci, je určení klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="442ae-166">Determining a partition key is an important decision to make before you create a collection.</span></span> <span data-ttu-id="442ae-167">Klíče oddílů jsou vlastnosti (nebo cestu) ve vaší dokumenty, které můžete používat k distribuci dat mezi několika servery nebo oddíly Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="442ae-167">Partition keys are a property (or path) within your documents that can be used by Azure Cosmos DB to distribute your data among multiple servers or partitions.</span></span> <span data-ttu-id="442ae-168">Cosmos DB hashuje hodnotu klíče oddílu a hash výsledek používá k určení oddílu pro uložení dokumentů.</span><span class="sxs-lookup"><span data-stu-id="442ae-168">Cosmos DB hashes the partition key value and uses the hashed result to determine the partition in which to store the document.</span></span> <span data-ttu-id="442ae-169">Všechny dokumenty se stejným klíčem oddílu ukládají do stejného oddílu a klíče oddílů nelze změnit po vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="442ae-169">All documents with the same partition key are stored in the same partition, and partition keys cannot be changed once a collection is created.</span></span> 

<span data-ttu-id="442ae-170">V tomto kurzu vytvoříme nastavit klíč oddílu na `/deviceId` tak, aby všechna data pro jedno zařízení je uložen v jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="442ae-170">For this tutorial, we're going to set the partition key to `/deviceId` so that the all the data for a single device is stored in a single partition.</span></span> <span data-ttu-id="442ae-171">Chcete vybrat klíč oddílu, který má velký počet hodnot, z nichž každý se používají v o stejnou frekvenci zajistit, že Cosmos DB můžete vyrovnávat zatížení, podle vašich dat roste a dosáhnout úplnou propustnost kolekce.</span><span class="sxs-lookup"><span data-stu-id="442ae-171">You want to choose a partition key that has a large number of values, each of which are used at about the same frequency to ensure Cosmos DB can load balance as your data grows and achieve the full throughput of the collection.</span></span> 

<span data-ttu-id="442ae-172">Další informace o oddílech najdete v tématu [postup oddílu a škálování v Azure Cosmos DB?](partition-data.md)</span><span class="sxs-lookup"><span data-stu-id="442ae-172">For more information about partitioning, see [How to partition and scale in Azure Cosmos DB?](partition-data.md)</span></span> 

## <span data-ttu-id="442ae-173"><a id="CreateColl"></a>Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="442ae-173"><a id="CreateColl"></a>Create a collection</span></span> 

<span data-ttu-id="442ae-174">Teď, když jsme si vědomi naše klíč oddílu `/deviceId`, umožňuje vytvořit [kolekce](documentdb-resources.md#collections) pomocí [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [CreateDocumentCollectionIfNotExistsAsync ](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metodu **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="442ae-174">Now that we know our partition key, `/deviceId`, lets create a [collection](documentdb-resources.md#collections) by using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="442ae-175">Kolekce je kontejner dokumentů JSON a všechny přidružené logiky Javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="442ae-175">A collection is a container of JSON documents and any associated JavaScript application logic.</span></span> 

> [!WARNING]
> <span data-ttu-id="442ae-176">Vytvoření kolekce hradí, jako jsou rezervování propustnost pro aplikace komunikovat s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="442ae-176">Creating a collection has pricing implications, as you are reserving throughput for the application to communicate with Azure Cosmos DB.</span></span> <span data-ttu-id="442ae-177">Další podrobnosti, navštivte naše [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="442ae-177">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
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

<span data-ttu-id="442ae-178">Tato metoda umožňuje volání Azure Cosmos DB a služby zřizuje počet oddílů podle propustnosti, požadované rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="442ae-178">This method makes a REST API call to Azure Cosmos DB, and the service provisions a number of partitions based on the requested throughput.</span></span> <span data-ttu-id="442ae-179">Propustnost kolekce můžete změnit, protože výkon musí momentální pomocí sady SDK nebo [portál Azure](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="442ae-179">You can change the throughput of a collection as your performance needs evolve using the SDK or the [Azure portal](set-throughput.md).</span></span>

## <span data-ttu-id="442ae-180"><a id="CreateDoc"></a>Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="442ae-180"><a id="CreateDoc"></a>Create JSON documents</span></span>
<span data-ttu-id="442ae-181">Umožňuje vložit některé dokumenty JSON do Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="442ae-181">Let's insert some JSON documents into Azure Cosmos DB.</span></span> <span data-ttu-id="442ae-182">[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) třídy **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="442ae-182">A [document](documentdb-resources.md#documents) can be created by using the [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="442ae-183">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="442ae-183">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="442ae-184">Tato ukázka třída obsahuje zařízení, čtení a volání CreateDocumentAsync vložit nového zařízení čtení do kolekce.</span><span class="sxs-lookup"><span data-stu-id="442ae-184">This sample class contains a device reading, and a call to CreateDocumentAsync to insert a new device reading into a collection.</span></span>

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

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
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
## <a name="read-data"></a><span data-ttu-id="442ae-185">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="442ae-185">Read data</span></span>

<span data-ttu-id="442ae-186">Umožňuje čtení dokumentu svým klíč oddílu a Id pomocí metody ReadDocumentAsync.</span><span class="sxs-lookup"><span data-stu-id="442ae-186">Let's read the document by its partition key and Id using the ReadDocumentAsync method.</span></span> <span data-ttu-id="442ae-187">Všimněte si, že čtení obsahují hodnotu PartitionKey (odpovídající `x-ms-documentdb-partitionkey` hlavička požadavku v rozhraní REST API).</span><span class="sxs-lookup"><span data-stu-id="442ae-187">Note that the reads include a PartitionKey value (corresponding to the `x-ms-documentdb-partitionkey` request header in the REST API).</span></span>

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a><span data-ttu-id="442ae-188">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="442ae-188">Update data</span></span>

<span data-ttu-id="442ae-189">Teď umožňuje aktualizovat některé data pomocí metody ReplaceDocumentAsync.</span><span class="sxs-lookup"><span data-stu-id="442ae-189">Now let's update some data using the ReplaceDocumentAsync method.</span></span>

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a><span data-ttu-id="442ae-190">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="442ae-190">Delete data</span></span>

<span data-ttu-id="442ae-191">Teď umožňuje odstranit dokument klíčem oddílu a id pomocí metody DeleteDocumentAsync.</span><span class="sxs-lookup"><span data-stu-id="442ae-191">Now lets delete a document by partition key and id by using the DeleteDocumentAsync method.</span></span>

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a><span data-ttu-id="442ae-192">Dotaz na dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="442ae-192">Query partitioned collections</span></span>

<span data-ttu-id="442ae-193">Pro dotazování dat v kolekcích oddílů Azure Cosmos DB automaticky směruje dotaz na oddíly odpovídající hodnoty klíče oddílu zadaných ve filtru (pokud existují).</span><span class="sxs-lookup"><span data-stu-id="442ae-193">When you query data in partitioned collections, Azure Cosmos DB automatically routes the query to the partitions corresponding to the partition key values specified in the filter (if there are any).</span></span> <span data-ttu-id="442ae-194">Například tento dotaz se směruje na právě oddílu klíč oddílu "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="442ae-194">For example, this query is routed to just the partition containing the partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="442ae-195">Následující dotaz na klíč oddílu (DeviceId) nemá filtr a je fanned pro všechny oddíly, kde je u indexu oddílu spustit.</span><span class="sxs-lookup"><span data-stu-id="442ae-195">The following query does not have a filter on the partition key (DeviceId) and is fanned out to all partitions where it is executed against the partition's index.</span></span> <span data-ttu-id="442ae-196">Všimněte si, že budete muset určit EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v rozhraní REST API) tak, aby měl sady SDK při spuštění dotazu napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="442ae-196">Note that you have to specify the EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in the REST API) to have the SDK to execute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a><span data-ttu-id="442ae-197">Provádění paralelního dotazu</span><span class="sxs-lookup"><span data-stu-id="442ae-197">Parallel query execution</span></span>
<span data-ttu-id="442ae-198">Azure Cosmos databáze DocumentDB SDK 1.9.0 a vyšší možnosti provádění paralelního dotazu podpory, které umožňují provádět dotazy s nízkou latencí pro dělené kolekce, i v případě, že potřebují k touch velký počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="442ae-198">The Azure Cosmos DB DocumentDB SDKs 1.9.0 and above support parallel query execution options, which allow you to perform low latency queries against partitioned collections, even when they need to touch a large number of partitions.</span></span> <span data-ttu-id="442ae-199">Například následující dotaz je nakonfigurována pro spuštění paralelně napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="442ae-199">For example, the following query is configured to run in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="442ae-200">Provádění paralelního dotazu můžete spravovat pomocí ladění následující parametry:</span><span class="sxs-lookup"><span data-stu-id="442ae-200">You can manage parallel query execution by tuning the following parameters:</span></span>

* <span data-ttu-id="442ae-201">Nastavením `MaxDegreeOfParallelism`, můžete řídit stupně paralelního zpracování tedy maximální počet souběžných síťové připojení k kolekce oddíly.</span><span class="sxs-lookup"><span data-stu-id="442ae-201">By setting `MaxDegreeOfParallelism`, you can control the degree of parallelism i.e., the maximum number of simultaneous network connections to the collection's partitions.</span></span> <span data-ttu-id="442ae-202">Pokud nastavíte na hodnotu -1, stupně paralelního zpracování spravuje sady SDK.</span><span class="sxs-lookup"><span data-stu-id="442ae-202">If you set this to -1, the degree of parallelism is managed by the SDK.</span></span> <span data-ttu-id="442ae-203">Pokud `MaxDegreeOfParallelism` není zadaný, nebo je nastavený na 0, což je výchozí hodnota, bude jedno síťové připojení k oddílům kolekce.</span><span class="sxs-lookup"><span data-stu-id="442ae-203">If the `MaxDegreeOfParallelism` is not specified or set to 0, which is the default value, there will be a single network connection to the collection's partitions.</span></span>
* <span data-ttu-id="442ae-204">Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="442ae-204">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="442ae-205">Pokud tento parametr vynecháte nebo tuto možnost nastavíte na hodnotu -1, počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje sady SDK.</span><span class="sxs-lookup"><span data-stu-id="442ae-205">If you omit this parameter or set this to -1, the number of items buffered during parallel query execution is managed by the SDK.</span></span>

<span data-ttu-id="442ae-206">Zadaný stav stejné kolekce, paralelní dotaz vrátí výsledky ve stejném pořadí jako sériové provádění.</span><span class="sxs-lookup"><span data-stu-id="442ae-206">Given the same state of the collection, a parallel query will return results in the same order as in serial execution.</span></span> <span data-ttu-id="442ae-207">Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), DocumentDB SDK vydá dotaz paralelně napříč oddíly a sloučí částečně seřazená výsledky na straně klienta k vytvoření globální seřazené výsledky.</span><span class="sxs-lookup"><span data-stu-id="442ae-207">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), the DocumentDB SDK issues the query in parallel across partitions and merges partially sorted results in the client side to produce globally ordered results.</span></span>

## <a name="execute-stored-procedures"></a><span data-ttu-id="442ae-208">Provedení uložené procedury</span><span class="sxs-lookup"><span data-stu-id="442ae-208">Execute stored procedures</span></span>
<span data-ttu-id="442ae-209">Nakonec můžete provést jednotlivé transakce na dokumenty se stejným ID zařízení, například pokud jste údržbě agregace nebo nejnovější stav zařízení do jednoho dokumentu přidáním následující kód do projektu.</span><span class="sxs-lookup"><span data-stu-id="442ae-209">Lastly, you can execute atomic transactions against documents with the same device ID, e.g. if you're maintaining aggregates or the latest state of a device in a single document by adding the following code to your project.</span></span>

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

<span data-ttu-id="442ae-210">A je to!</span><span class="sxs-lookup"><span data-stu-id="442ae-210">And that's it!</span></span> <span data-ttu-id="442ae-211">ty jsou hlavními součástmi Azure Cosmos DB aplikaci, která používá klíč oddílu efektivně škálovat distribuci dat mezi oddílů.</span><span class="sxs-lookup"><span data-stu-id="442ae-211">those are the main components of an Azure Cosmos DB application that uses a partition key to efficiently scale data distribution across partitions.</span></span>  

## <a name="clean-up-resources"></a><span data-ttu-id="442ae-212">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="442ae-212">Clean up resources</span></span>

<span data-ttu-id="442ae-213">Pokud nechcete pokračovat v používání této aplikace, odstraňte všechny prostředky, které jsou vytvořené v tomto kurzu na portálu Azure pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="442ae-213">If you're not going to continue to use this app, delete all resources created by this tutorial in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="442ae-214">Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **skupiny prostředků** a pak klikněte na jedinečný název prostředku, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="442ae-214">From the left-hand menu in the Azure portal, click **Resource groups** and then click the unique name of the resource you created.</span></span> 
2. <span data-ttu-id="442ae-215">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="442ae-215">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="442ae-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="442ae-216">Next steps</span></span>

<span data-ttu-id="442ae-217">V tomto kurzu jste provést následující:</span><span class="sxs-lookup"><span data-stu-id="442ae-217">In this tutorial, you've done the following:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="442ae-218">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="442ae-218">Created an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="442ae-219">Vytvořit databázi a kolekci s klíčem oddílu</span><span class="sxs-lookup"><span data-stu-id="442ae-219">Created a database and collection with a partition key</span></span>
> * <span data-ttu-id="442ae-220">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="442ae-220">Created JSON documents</span></span>
> * <span data-ttu-id="442ae-221">Aktualizovat dokumentu</span><span class="sxs-lookup"><span data-stu-id="442ae-221">Updated a document</span></span>
> * <span data-ttu-id="442ae-222">Dotaz dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="442ae-222">Queried partitioned collections</span></span>
> * <span data-ttu-id="442ae-223">Byla spuštěna uložené procedury</span><span class="sxs-lookup"><span data-stu-id="442ae-223">Ran a stored procedure</span></span>
> * <span data-ttu-id="442ae-224">Odstranit dokumentu</span><span class="sxs-lookup"><span data-stu-id="442ae-224">Deleted a document</span></span>
> * <span data-ttu-id="442ae-225">Odstranit databázi</span><span class="sxs-lookup"><span data-stu-id="442ae-225">Deleted a database</span></span>

<span data-ttu-id="442ae-226">Teď můžete pokračovat v dalším kurzu a další data importovat do účtu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="442ae-226">You can now proceed to the next tutorial and import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="442ae-227">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="442ae-227">Import data into Azure Cosmos DB</span></span>](import-data.md)
