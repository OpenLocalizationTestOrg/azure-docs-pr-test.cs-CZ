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
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a><span data-ttu-id="4b1d0-103">Azure CosmosDB: Vývoj pomocí hello DocumentDB rozhraní API v rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="4b1d0-103">Azure CosmosDB: Develop with hello DocumentDB API in .NET</span></span>

<span data-ttu-id="4b1d0-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="4b1d0-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="4b1d0-106">Tento kurz ukazuje, jak toocreate účtu Azure Cosmos DB pomocí hello portálu Azure a pak vytvořte databázi dokumentů a kolekce s [klíč oddílu](documentdb-partition-data.md#partition-keys) pomocí hello [DocumentDB .NET API](documentdb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and then create a document database and collection with a [partition key](documentdb-partition-data.md#partition-keys) using hello [DocumentDB .NET API](documentdb-introduction.md).</span></span> <span data-ttu-id="4b1d0-107">Definováním klíč oddílu, když vytvoříte kolekci, vaše aplikace je připravená tooscale pak moci bez obtíží s růstem vaše data.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-107">By defining a partition key when you create a collection, your application is prepared tooscale effortlessly as your data grows.</span></span> 

<span data-ttu-id="4b1d0-108">Tento kurz zahrnuje hello následující úlohy pomocí hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="4b1d0-108">This tutorial covers hello following tasks by using hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4b1d0-109">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4b1d0-109">Create an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="4b1d0-110">Vytvoření databáze a kolekce s klíčem oddílu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-110">Create a database and collection with a partition key</span></span>
> * <span data-ttu-id="4b1d0-111">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="4b1d0-111">Create JSON documents</span></span>
> * <span data-ttu-id="4b1d0-112">Aktualizace dokumentu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-112">Update a document</span></span>
> * <span data-ttu-id="4b1d0-113">Dotaz na dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="4b1d0-113">Query partitioned collections</span></span>
> * <span data-ttu-id="4b1d0-114">Spuštění uložené procedury</span><span class="sxs-lookup"><span data-stu-id="4b1d0-114">Run stored procedures</span></span>
> * <span data-ttu-id="4b1d0-115">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-115">Delete a document</span></span>
> * <span data-ttu-id="4b1d0-116">Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="4b1d0-116">Delete a database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b1d0-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4b1d0-117">Prerequisites</span></span>
<span data-ttu-id="4b1d0-118">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="4b1d0-118">Please make sure you have hello following:</span></span>

* <span data-ttu-id="4b1d0-119">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-119">An active Azure account.</span></span> <span data-ttu-id="4b1d0-120">Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-120">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="4b1d0-121">Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu, pokud chcete toouse místní prostředí, které emuluje služby Azure DocumentDB hello pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-121">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial if you'd like toouse a local environment that emulates hello Azure DocumentDB service for development purposes.</span></span>
* <span data-ttu-id="4b1d0-122">Sadu [Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-122">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="4b1d0-123">Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4b1d0-123">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="4b1d0-124">Začněme vytvořením účtu Azure Cosmos DB v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-124">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>

> [!TIP]
> * <span data-ttu-id="4b1d0-125">Již máte účet Azure Cosmos DB?</span><span class="sxs-lookup"><span data-stu-id="4b1d0-125">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="4b1d0-126">Pokud ano, přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="4b1d0-126">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="4b1d0-127">Měli jste účet Azure DocumentDB?</span><span class="sxs-lookup"><span data-stu-id="4b1d0-127">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="4b1d0-128">Pokud ano, váš účet je teď účet Azure Cosmos DB a můžete přeskočit příliš[nastavit řešení sady Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-128">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="4b1d0-129">Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení řešení v nástroji Visual Studio](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-129">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="4b1d0-130"><a id="SetupVS"></a>Nastavení řešení v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b1d0-130"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="4b1d0-131">Otevřete na svém počítači sadu **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-131">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="4b1d0-132">Na hello **soubor** nabídce vyberte možnost **nový**a potom zvolte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-132">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="4b1d0-133">V hello **nový projekt** dialogovém okně, vyberte **šablony** / **Visual C#** / **konzolovou aplikaci (rozhraní .NET Framework)**, pojmenujte svůj projekt a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-133">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="4b1d0-134">![Snímek obrazovky okna Nový projekt hello](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="4b1d0-134">![Screen shot of hello New Project window](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span></span>

4. <span data-ttu-id="4b1d0-135">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na novou konzolovou aplikaci, která je v části řešení sady Visual Studio, a pak klikněte na tlačítko **spravovat balíčky NuGet...**</span><span class="sxs-lookup"><span data-stu-id="4b1d0-135">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Snímek obrazovky znázorňující hello právo místní nabídky pro hello projektu](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="4b1d0-137">V hello **NuGet** , klikněte na **Procházet**a typ **documentdb** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-137">In hello **NuGet** tab, click **Browse**, and type **documentdb** in hello search box.</span></span>
<!---stopped here--->
6. <span data-ttu-id="4b1d0-138">V rámci hello výsledky najít **Microsoft.Azure.DocumentDB** a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-138">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="4b1d0-139">ID balíčku Hello hello Azure Cosmos DB klientské knihovny je [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-139">hello package ID for hello Azure Cosmos DB Client Library is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span></span>
   <span data-ttu-id="4b1d0-140">![Snímek obrazovky hello nabídky NuGet pro vyhledání Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="4b1d0-140">![Screen shot of hello NuGet Menu for finding Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="4b1d0-141">Pokud se zobrazí zpráva o Kontrola řešení toohello změny, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-141">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="4b1d0-142">Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-142">If you get a message about license acceptance, click **I accept**.</span></span>

## <span data-ttu-id="4b1d0-143"><a id="Connect"></a>Přidání odkazů tooyour projektu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-143"><a id="Connect"></a>Add references tooyour project</span></span>
<span data-ttu-id="4b1d0-144">Hello zbývající kroky v tento kurz zadejte hello DocumentDB API kód fragmenty požadované toocreate a aktualizace Azure Cosmos DB prostředky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-144">hello remaining steps in this tutorial provide hello DocumentDB API code snippets required toocreate and update Azure Cosmos DB resources in your project.</span></span>

<span data-ttu-id="4b1d0-145">Nejprve přidejte tyto odkazy tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-145">First, add these references tooyour application.</span></span>
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <span data-ttu-id="4b1d0-146"><a id="add-references"></a>Připojení aplikace</span><span class="sxs-lookup"><span data-stu-id="4b1d0-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="4b1d0-147">V dalším kroku přidejte tyto dvě konstanty a *klienta* proměnné ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-147">Next, add these two constants and your *client* variable in your application.</span></span>

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

<span data-ttu-id="4b1d0-148">Potom, head zpět toohello [portál Azure](https://portal.azure.com) tooretrieve adresu URL koncového bodu a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-148">Then, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="4b1d0-149">Adresa URL koncového bodu Hello a primární klíč jsou nutné pro vaše aplikace toounderstand kde tooconnect a u Azure Cosmos DB tootrust připojení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="4b1d0-150">V hello portálu Azure, přejděte tooyour Azure Cosmos DB účet, klikněte na **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span>

<span data-ttu-id="4b1d0-151">Zkopírujte z portálu hello hello URI a vložte ho přes `<your endpoint URL>` v souboru program.cs hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-151">Copy hello URI from hello portal and paste it over `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="4b1d0-152">Pak hello primární klíč z portálu hello kopírování a vložení prostřednictvím `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-152">Then copy hello PRIMARY KEY from hello portal and paste it over `<your primary key>`.</span></span> <span data-ttu-id="4b1d0-153">Se, zda text hello tooremove `<` a `>` z hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-153">Be sure tooremove hello `<` and `>` from your values.</span></span>

![Snímek obrazovky portálu Azure hello používá toocreate kurzu NoSQL hello konzolovou aplikaci C#.](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <span data-ttu-id="4b1d0-156"><a id="instantiate"></a>Vytvoření instance DocumentClient hello</span><span class="sxs-lookup"><span data-stu-id="4b1d0-156"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span>

<span data-ttu-id="4b1d0-157">Teď vytvořte novou instanci třídy hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-157">Now, create a new instance of hello **DocumentClient**.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <span data-ttu-id="4b1d0-158"><a id="create-database"></a>Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="4b1d0-158"><a id="create-database"></a>Create a database</span></span>

<span data-ttu-id="4b1d0-159">Dále vytvořte Azure DB Cosmos [databáze](documentdb-resources.md#databases) pomocí hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metoda nebo [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metoda hello  **DocumentClient** třídy z hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-159">Next, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="4b1d0-160">Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-160">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a><span data-ttu-id="4b1d0-161">Vyberte klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-161">Decide on a partition key</span></span> 

<span data-ttu-id="4b1d0-162">Kolekce jsou kontejnery pro ukládání dokumentů.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-162">Collections are containers for storing documents.</span></span> <span data-ttu-id="4b1d0-163">Jsou logické prostředky a můžete [span jeden nebo více fyzických oddílů](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-163">They are logical resources and can [span one or more physical partitions](partition-data.md).</span></span> <span data-ttu-id="4b1d0-164">A [klíč oddílu](documentdb-partition-data.md) je vlastnost (nebo cestu) v rámci vaší dokumenty, které je použité toodistribute data mezi servery hello nebo oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-164">A [partition key](documentdb-partition-data.md) is a property (or path) within your documents that is used toodistribute your data among hello servers or partitions.</span></span> <span data-ttu-id="4b1d0-165">Všechny dokumenty se stejným klíčem oddílu jsou uložené v hello hello stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-165">All documents with hello same partition key are stored in hello same partition.</span></span> 

<span data-ttu-id="4b1d0-166">Klíč oddílu je určení důležité rozhodnutí toomake před vytvořením kolekce.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-166">Determining a partition key is an important decision toomake before you create a collection.</span></span> <span data-ttu-id="4b1d0-167">Klíče oddílů jsou vlastnosti (nebo cestu) v rámci vaší dokumenty, které se dají používat Azure Cosmos DB toodistribute data mezi více servery nebo oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-167">Partition keys are a property (or path) within your documents that can be used by Azure Cosmos DB toodistribute your data among multiple servers or partitions.</span></span> <span data-ttu-id="4b1d0-168">Cosmos DB hashuje hodnotu klíče oddílu hello a používá hello rozdělí výsledek toodetermine hello oddílu v které toostore hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-168">Cosmos DB hashes hello partition key value and uses hello hashed result toodetermine hello partition in which toostore hello document.</span></span> <span data-ttu-id="4b1d0-169">Všechny dokumenty se stejným klíčem oddílu jsou uložené v hello hello stejného oddílu a klíče oddílů nelze změnit po vytvoření kolekce.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-169">All documents with hello same partition key are stored in hello same partition, and partition keys cannot be changed once a collection is created.</span></span> 

<span data-ttu-id="4b1d0-170">V tomto kurzu vytvoříme klíč oddílu hello tooset příliš`/deviceId` , který hello všechna data hello pro jedno zařízení je uložený v jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-170">For this tutorial, we're going tooset hello partition key too`/deviceId` so that hello all hello data for a single device is stored in a single partition.</span></span> <span data-ttu-id="4b1d0-171">Chcete-li toochoose klíč oddílu, který má velký počet hodnot, z nichž každý se používají v o hello stejnou frekvenci tooensure Cosmos DB můžete vyrovnávat zatížení jako data zvětšování a dosáhnout úplné propustnosti hello hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-171">You want toochoose a partition key that has a large number of values, each of which are used at about hello same frequency tooensure Cosmos DB can load balance as your data grows and achieve hello full throughput of hello collection.</span></span> 

<span data-ttu-id="4b1d0-172">Další informace o oddílech najdete v tématu [jak toopartition a škálování v Azure Cosmos DB?](partition-data.md)</span><span class="sxs-lookup"><span data-stu-id="4b1d0-172">For more information about partitioning, see [How toopartition and scale in Azure Cosmos DB?](partition-data.md)</span></span> 

## <span data-ttu-id="4b1d0-173"><a id="CreateColl"></a>Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="4b1d0-173"><a id="CreateColl"></a>Create a collection</span></span> 

<span data-ttu-id="4b1d0-174">Teď, když jsme si vědomi naše klíč oddílu `/deviceId`, umožňuje vytvořit [kolekce](documentdb-resources.md#collections) pomocí hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metoda nebo [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-174">Now that we know our partition key, `/deviceId`, lets create a [collection](documentdb-resources.md#collections) by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="4b1d0-175">Kolekce je kontejner dokumentů JSON a všechny přidružené logiky Javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-175">A collection is a container of JSON documents and any associated JavaScript application logic.</span></span> 

> [!WARNING]
> <span data-ttu-id="4b1d0-176">Vytvoření kolekce hradí, jako jsou rezervování propustnost pro hello toocommunicate aplikací s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-176">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="4b1d0-177">Další podrobnosti, navštivte naše [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="4b1d0-177">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
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

<span data-ttu-id="4b1d0-178">Tato metoda díky rozhraní REST API volání tooAzure Cosmos DB a hello služby zřizuje počet oddílů založené na požadovaný propustnost hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-178">This method makes a REST API call tooAzure Cosmos DB, and hello service provisions a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="4b1d0-179">Výkon vašich potřeb momentální pomocí hello SDK nebo hello můžete změnit hello propustnost kolekce [portál Azure](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-179">You can change hello throughput of a collection as your performance needs evolve using hello SDK or hello [Azure portal](set-throughput.md).</span></span>

## <span data-ttu-id="4b1d0-180"><a id="CreateDoc"></a>Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="4b1d0-180"><a id="CreateDoc"></a>Create JSON documents</span></span>
<span data-ttu-id="4b1d0-181">Umožňuje vložit některé dokumenty JSON do Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-181">Let's insert some JSON documents into Azure Cosmos DB.</span></span> <span data-ttu-id="4b1d0-182">A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metoda hello **DocumentClient** třídy.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-182">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="4b1d0-183">Dokumenty představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-183">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="4b1d0-184">Tato ukázka třída obsahuje čtení zařízení a tooinsert tooCreateDocumentAsync volání nové zařízení čtení do kolekce.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-184">This sample class contains a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a collection.</span></span>

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
## <a name="read-data"></a><span data-ttu-id="4b1d0-185">Čtení dat</span><span class="sxs-lookup"><span data-stu-id="4b1d0-185">Read data</span></span>

<span data-ttu-id="4b1d0-186">Umožňuje číst dokument hello svým klíč oddílu a Id pomocí metody ReadDocumentAsync hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-186">Let's read hello document by its partition key and Id using hello ReadDocumentAsync method.</span></span> <span data-ttu-id="4b1d0-187">Všimněte si, že čtení hello obsahují hodnotu PartitionKey (odpovídající toohello `x-ms-documentdb-partitionkey` hlavička požadavku v hello REST API).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-187">Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a><span data-ttu-id="4b1d0-188">Aktualizace dat</span><span class="sxs-lookup"><span data-stu-id="4b1d0-188">Update data</span></span>

<span data-ttu-id="4b1d0-189">Teď umožňuje aktualizovat některé data pomocí metody ReplaceDocumentAsync hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-189">Now let's update some data using hello ReplaceDocumentAsync method.</span></span>

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a><span data-ttu-id="4b1d0-190">Odstranění dat</span><span class="sxs-lookup"><span data-stu-id="4b1d0-190">Delete data</span></span>

<span data-ttu-id="4b1d0-191">Teď umožňuje odstranit dokument klíčem oddílu a id pomocí metody DeleteDocumentAsync hello.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-191">Now lets delete a document by partition key and id by using hello DeleteDocumentAsync method.</span></span>

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a><span data-ttu-id="4b1d0-192">Dotaz na dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="4b1d0-192">Query partitioned collections</span></span>

<span data-ttu-id="4b1d0-193">Když dotazujete dat v kolekcích oddílů, Azure Cosmos DB automaticky trasy hello oddíly query toohello odpovídající hodnoty klíče toohello oddílu zadané ve filtru hello (pokud existují).</span><span class="sxs-lookup"><span data-stu-id="4b1d0-193">When you query data in partitioned collections, Azure Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="4b1d0-194">Tento dotaz je například směrované toojust hello oddílu obsahující hello klíč oddílu "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="4b1d0-194">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="4b1d0-195">Hello následující dotaz na klíč oddílu hello (DeviceId) nemá filtr a je fanned na oddíly tooall němž se spustí před hello oddílu indexu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-195">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="4b1d0-196">Všimněte si, že máte toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` v hello REST API) toohave hello SDK tooexecute dotazu napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-196">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a><span data-ttu-id="4b1d0-197">Provádění paralelního dotazu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-197">Parallel query execution</span></span>
<span data-ttu-id="4b1d0-198">Hello Azure Cosmos databáze DocumentDB SDK 1.9.0 a výše podpory možnosti provádění paralelního dotazu, které umožňují s nízkou latencí tooperform dotazy proti dělené kolekce, i v případě, že potřebují tootouch velký počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-198">hello Azure Cosmos DB DocumentDB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="4b1d0-199">Následující dotaz hello je například nakonfigurované toorun paralelně napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-199">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="4b1d0-200">Provádění paralelního dotazu můžete spravovat pomocí ladění hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="4b1d0-200">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="4b1d0-201">Nastavením `MaxDegreeOfParallelism`, můžete řídit hello stupně paralelního zpracování tedy hello maximální počet souběžných síťové připojení toohello kolekce oddílů.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-201">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello collection's partitions.</span></span> <span data-ttu-id="4b1d0-202">Pokud nastavíte příliš-1, hello stupně paralelního zpracování spravuje hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-202">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="4b1d0-203">Pokud hello `MaxDegreeOfParallelism` není zadaný, nebo nastavte too0, což je výchozí hodnota hello, bude oddíly kolekce toohello na jedné síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-203">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello collection's partitions.</span></span>
* <span data-ttu-id="4b1d0-204">Nastavením `MaxBufferedItemCount`, můžete kompromisy využití paměti dotazu latence a na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-204">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="4b1d0-205">Pokud tento parametr vynecháte, nebo nastavte příliš-1, hello počet položek do vyrovnávací paměti při provádění paralelního dotazu, které spravuje hello SDK.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-205">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="4b1d0-206">Zadané hello stejného stavu hello kolekce paralelní dotaz vrátí výsledky v hello stejné pořadí jako sériové provádění.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-206">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="4b1d0-207">Při provádění dotazu mezi oddílu, který zahrnuje řazení (ORDER BY a/nebo horní), hello DocumentDB SDK vydá dotaz hello paralelně napříč oddíly a sloučí částečně seřazená má za následek hello klientské straně tooproduce globálně řazení výsledků.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-207">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello DocumentDB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

## <a name="execute-stored-procedures"></a><span data-ttu-id="4b1d0-208">Provedení uložené procedury</span><span class="sxs-lookup"><span data-stu-id="4b1d0-208">Execute stored procedures</span></span>
<span data-ttu-id="4b1d0-209">Nakonec můžete provést jednotlivé transakce na dokumenty s hello stejné ID zařízení, například pokud jste zachování agregace nebo hello nejnovější stav zařízení do jednoho dokumentu přidáním hello následující kód tooyour projektu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-209">Lastly, you can execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single document by adding hello following code tooyour project.</span></span>

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

<span data-ttu-id="4b1d0-210">A je to!</span><span class="sxs-lookup"><span data-stu-id="4b1d0-210">And that's it!</span></span> <span data-ttu-id="4b1d0-211">ty jsou hello hlavními součástmi Azure Cosmos DB aplikaci, která používá distribuční oddílu klíče tooefficiently škálování dat napříč oddíly.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-211">those are hello main components of an Azure Cosmos DB application that uses a partition key tooefficiently scale data distribution across partitions.</span></span>  

## <a name="clean-up-resources"></a><span data-ttu-id="4b1d0-212">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="4b1d0-212">Clean up resources</span></span>

<span data-ttu-id="4b1d0-213">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené v tomto kurzu v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4b1d0-213">If you're not going toocontinue toouse this app, delete all resources created by this tutorial in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="4b1d0-214">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na tlačítko hello jedinečný název hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-214">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello unique name of hello resource you created.</span></span> 
2. <span data-ttu-id="4b1d0-215">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-215">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b1d0-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b1d0-216">Next steps</span></span>

<span data-ttu-id="4b1d0-217">V tomto kurzu provedete krok hello následující:</span><span class="sxs-lookup"><span data-stu-id="4b1d0-217">In this tutorial, you've done hello following:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="4b1d0-218">Vytvoření účtu Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4b1d0-218">Created an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="4b1d0-219">Vytvořit databázi a kolekci s klíčem oddílu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-219">Created a database and collection with a partition key</span></span>
> * <span data-ttu-id="4b1d0-220">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="4b1d0-220">Created JSON documents</span></span>
> * <span data-ttu-id="4b1d0-221">Aktualizovat dokumentu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-221">Updated a document</span></span>
> * <span data-ttu-id="4b1d0-222">Dotaz dělené kolekce</span><span class="sxs-lookup"><span data-stu-id="4b1d0-222">Queried partitioned collections</span></span>
> * <span data-ttu-id="4b1d0-223">Byla spuštěna uložené procedury</span><span class="sxs-lookup"><span data-stu-id="4b1d0-223">Ran a stored procedure</span></span>
> * <span data-ttu-id="4b1d0-224">Odstranit dokumentu</span><span class="sxs-lookup"><span data-stu-id="4b1d0-224">Deleted a document</span></span>
> * <span data-ttu-id="4b1d0-225">Odstranit databázi</span><span class="sxs-lookup"><span data-stu-id="4b1d0-225">Deleted a database</span></span>

<span data-ttu-id="4b1d0-226">Teď můžete pokračovat dalším kurzu toohello a importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="4b1d0-226">You can now proceed toohello next tutorial and import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="4b1d0-227">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4b1d0-227">Import data into Azure Cosmos DB</span></span>](import-data.md)
