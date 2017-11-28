---
title: "Azure Cosmos DB: Sestavení aplikace pomocí Python a hello rozhraní API DocumentDB | Microsoft Docs"
description: "Uvede ukázku kódu Python, můžete použít dotaz tooand tooconnect hello rozhraní API služby Azure Cosmos databáze DocumentDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.openlocfilehash: e66965ab493c6ef693e88a3767a401d39e1bde2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-hello-azure-portal"></a><span data-ttu-id="b671f-103">Azure Cosmos DB: Sestavení aplikace DocumentDB API s Pythonem a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b671f-103">Azure Cosmos DB: Build a DocumentDB API app with Python and hello Azure portal</span></span>

<span data-ttu-id="b671f-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="b671f-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="b671f-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b671f-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="b671f-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b671f-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="b671f-107">Potom sestavení a spuštění konzoly aplikace založená na hello [DocumentDB Python API](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="b671f-107">You then build and run a console app built on hello [DocumentDB Python API](documentdb-sdk-python.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b671f-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b671f-108">Prerequisites</span></span>

* <span data-ttu-id="b671f-109">Před spuštěním této ukázce, musíte mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b671f-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="b671f-110">[Visual Studio 2015](http://www.visualstudio.com/) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b671f-110">[Visual Studio 2015](http://www.visualstudio.com/) or higher.</span></span>
    * <span data-ttu-id="b671f-111">Python Tools for Visual Studio z [GitHubu](http://microsoft.github.io/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="b671f-111">Python Tools for Visual Studio from [GitHub](http://microsoft.github.io/PTVS/).</span></span> <span data-ttu-id="b671f-112">V tomto kurzu se používá Python Tools for VS 2015.</span><span class="sxs-lookup"><span data-stu-id="b671f-112">This tutorial uses Python Tools for VS 2015.</span></span>
    * <span data-ttu-id="b671f-113">Python 2.7 z webu [python.org](https://www.python.org/downloads/release/python-2712/)</span><span class="sxs-lookup"><span data-stu-id="b671f-113">Python 2.7 from [python.org](https://www.python.org/downloads/release/python-2712/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="b671f-114">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="b671f-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b671f-115">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="b671f-115">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="b671f-116">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b671f-116">Clone hello sample application</span></span>

<span data-ttu-id="b671f-117">Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="b671f-117">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="b671f-118">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="b671f-118">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="b671f-119">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="b671f-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="b671f-120">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="b671f-120">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## <a name="review-hello-code"></a><span data-ttu-id="b671f-121">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="b671f-121">Review hello code</span></span>

<span data-ttu-id="b671f-122">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="b671f-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="b671f-123">Otevřete hello DocumentDBGetStarted.py souboru a najdete v ní vytvořit tyto řádky kódu hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b671f-123">Open hello DocumentDBGetStarted.py file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 


* <span data-ttu-id="b671f-124">Hello DocumentClient je inicializován.</span><span class="sxs-lookup"><span data-stu-id="b671f-124">hello DocumentClient is initialized.</span></span>

    ```python
    # Initialize hello Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* <span data-ttu-id="b671f-125">Vytvoří se nová databáze.</span><span class="sxs-lookup"><span data-stu-id="b671f-125">A new database is created.</span></span>

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* <span data-ttu-id="b671f-126">Vytvoří se nová kolekce.</span><span class="sxs-lookup"><span data-stu-id="b671f-126">A new collection is created.</span></span>

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* <span data-ttu-id="b671f-127">Vytvoří se některé dokumenty.</span><span class="sxs-lookup"><span data-stu-id="b671f-127">Some documents are created.</span></span>

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* <span data-ttu-id="b671f-128">Provede se dotaz v jazyce SQL.</span><span class="sxs-lookup"><span data-stu-id="b671f-128">A query is performed using SQL</span></span>

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="b671f-129">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="b671f-129">Update your connection string</span></span>

<span data-ttu-id="b671f-130">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="b671f-130">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="b671f-131">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="b671f-131">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="b671f-132">Použijete tlačítka hello kopírovat na pravé straně hello hello obrazovky toocopy hello URI a Primary Key do hello `DocumentDBGetStarted.py` souboru v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="b671f-132">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `DocumentDBGetStarted.py` file in hello next step.</span></span>

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="b671f-134">V otevřené hello `DocumentDBGetStarted.py` souboru.</span><span class="sxs-lookup"><span data-stu-id="b671f-134">In Open hello `DocumentDBGetStarted.py` file.</span></span> 

3. <span data-ttu-id="b671f-135">Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu klíče hello koncového bodu v `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="b671f-135">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `DocumentDBGetStarted.py`.</span></span> 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="b671f-136">Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hello hodnotu hello `config.MASTERKEY` v `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="b671f-136">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.MASTERKEY` in `DocumentDBGetStarted.py`.</span></span> <span data-ttu-id="b671f-137">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b671f-137">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.MASTERKEY : "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="b671f-138">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="b671f-138">Run hello app</span></span>
1. <span data-ttu-id="b671f-139">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení**vyberte hello aktuální prostředí Python a pak klikněte pravým tlačítkem.</span><span class="sxs-lookup"><span data-stu-id="b671f-139">In Visual Studio, right-click on hello project in **Solution Explorer**, select hello current Python environment, then right click.</span></span>

2. <span data-ttu-id="b671f-140">Vyberte Možnost Instalovat balíček Pythonu a potom zadejte **pydocumentdb**.</span><span class="sxs-lookup"><span data-stu-id="b671f-140">Select Install Python Package, then type in **pydocumentdb**</span></span>

3. <span data-ttu-id="b671f-141">Spusťte aplikaci hello toorun F5.</span><span class="sxs-lookup"><span data-stu-id="b671f-141">Run F5 toorun hello application.</span></span> <span data-ttu-id="b671f-142">Aplikace se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b671f-142">Your app displays in your browser.</span></span> 

<span data-ttu-id="b671f-143">Teď můžete přejít zpět tooData Průzkumníka a zobrazit dotaz, upravit a pracovat s Tato nová data.</span><span class="sxs-lookup"><span data-stu-id="b671f-143">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="b671f-144">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b671f-144">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="b671f-145">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b671f-145">Clean up resources</span></span>

<span data-ttu-id="b671f-146">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b671f-146">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="b671f-147">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b671f-147">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="b671f-148">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b671f-148">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b671f-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b671f-149">Next steps</span></span>

<span data-ttu-id="b671f-150">V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b671f-150">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="b671f-151">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="b671f-151">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b671f-152">Importovat data do Azure Cosmos DB pro hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="b671f-152">Import data into Azure Cosmos DB for hello DocumentDB API</span></span>](import-data.md)


