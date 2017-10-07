---
title: "Azure Cosmos DB: Sestavení webové aplikace pomocí rozhraní .NET a hello MongoDB API | Microsoft Docs"
description: "Uvede ukázku kódu .NET pomocí dotazu tooand tooconnect hello rozhraní API služby Azure Cosmos DB MongoDB"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: c85cc47f772a19aaa7181611b75a8acaedbc4c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="13b5c-103">Azure Cosmos DB: Sestavení rozhraní API MongoDB webové aplikace pomocí rozhraní .NET a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="13b5c-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="13b5c-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="13b5c-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="13b5c-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b5c-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="13b5c-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="13b5c-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="13b5c-107">Budete pak sestavení a nasazení webové aplikace seznamu úkolů založený na hello [MongoDB .NET ovladač](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span><span class="sxs-lookup"><span data-stu-id="13b5c-107">You'll then build and deploy a tasks list web app built on hello [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="13b5c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="13b5c-108">Prerequisites</span></span>

<span data-ttu-id="13b5c-109">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="13b5c-109">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="13b5c-110">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="13b5c-110">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="13b5c-111">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="13b5c-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="13b5c-112">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="13b5c-112">Clone hello sample application</span></span>

<span data-ttu-id="13b5c-113">Teď umožňuje nastavit připojovací řetězec hello klonování MongoDB API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="13b5c-113">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="13b5c-114">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="13b5c-114">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="13b5c-115">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="13b5c-115">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="13b5c-116">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="13b5c-116">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="13b5c-117">Poté otevřete soubor řešení hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13b5c-117">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="13b5c-118">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="13b5c-118">Review hello code</span></span>

<span data-ttu-id="13b5c-119">Provedeme jejich stručný přehled o dění v aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="13b5c-119">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="13b5c-120">Otevřete hello **Dal.cs** souboru pod hello **DAL** directory a najdete, že tyto řádky kódu vytvořit hello prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b5c-120">Open hello **Dal.cs** file under hello **DAL** directory and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="13b5c-121">Inicializujte hello Mongo klienta.</span><span class="sxs-lookup"><span data-stu-id="13b5c-121">Initialize hello Mongo Client.</span></span>

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* <span data-ttu-id="13b5c-122">Načíst hello databázi a kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="13b5c-122">Retrieve hello database and hello collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="13b5c-123">Načtou se všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="13b5c-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="13b5c-124">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="13b5c-124">Update your connection string</span></span>

<span data-ttu-id="13b5c-125">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="13b5c-125">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="13b5c-126">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **připojovací řetězec**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="13b5c-126">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="13b5c-127">Do souboru Dal.cs hello v dalším kroku hello použijete hello tlačítka Kopírovat na pravé straně hello hello obrazovky toocopy hello uživatelské jméno, heslo a hostitele.</span><span class="sxs-lookup"><span data-stu-id="13b5c-127">You'll use hello copy buttons on hello right side of hello screen toocopy hello Username, Password, and Host into hello Dal.cs file in hello next step.</span></span>

2. <span data-ttu-id="13b5c-128">Otevřete hello **Dal.cs** souboru v hello **DAL** adresáře.</span><span class="sxs-lookup"><span data-stu-id="13b5c-128">Open hello **Dal.cs** file in hello **DAL** directory.</span></span> 

3. <span data-ttu-id="13b5c-129">Kopírování vaše **uživatelské jméno** hodnoty z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu hello **uživatelské jméno** ve vaší **Dal.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="13b5c-129">Copy your **username** value from hello portal (using hello copy button) and make it hello value of hello **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="13b5c-130">Zkopírujte vaše **hostitele** hodnoty z portálu hello a nastavit jej jako hello hodnotu hello **hostitele** ve vaší **Dal.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="13b5c-130">Then copy your **host** value from hello portal and make it hello value of hello **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="13b5c-131">Nakonec zkopírujte vaše **heslo** hodnoty z portálu hello a nastavit jej jako hello hodnotu hello **heslo** v vaše **Dal.cs** souboru.</span><span class="sxs-lookup"><span data-stu-id="13b5c-131">Finally copy your **password** value from hello portal and make it hello value of hello **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="13b5c-132">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="13b5c-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-web-app"></a><span data-ttu-id="13b5c-133">Spouštění hello webové aplikace</span><span class="sxs-lookup"><span data-stu-id="13b5c-133">Run hello web app</span></span>

1. <span data-ttu-id="13b5c-134">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello v **Průzkumníku řešení** a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="13b5c-134">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="13b5c-135">V hello NuGet **Procházet** zadejte *MongoDB.Driver*.</span><span class="sxs-lookup"><span data-stu-id="13b5c-135">In hello NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="13b5c-136">Z výsledků hello nainstalovat hello **MongoDB.Driver** knihovny.</span><span class="sxs-lookup"><span data-stu-id="13b5c-136">From hello results, install hello **MongoDB.Driver** library.</span></span> <span data-ttu-id="13b5c-137">Tím se nainstaluje balíček MongoDB.Driver hello a také všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="13b5c-137">This installs hello MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="13b5c-138">Klikněte na kombinaci kláves CTRL + F5 toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="13b5c-138">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="13b5c-139">Aplikace se zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="13b5c-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="13b5c-140">Klikněte na tlačítko **vytvořit** v hello prohlížeče a vytvořit pár nové úlohy ve vaší aplikaci seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="13b5c-140">Click **Create** in hello browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="13b5c-141">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="13b5c-141">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="13b5c-142">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="13b5c-142">Clean up resources</span></span>

<span data-ttu-id="13b5c-143">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="13b5c-143">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="13b5c-144">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="13b5c-144">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="13b5c-145">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="13b5c-145">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13b5c-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="13b5c-146">Next steps</span></span>

<span data-ttu-id="13b5c-147">V tento rychlý start když jste se naučili jak toocreate účet Azure Cosmos DB a spusťte webovou aplikaci pomocí hello rozhraní API pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="13b5c-147">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and run a web app using hello API for MongoDB.</span></span> <span data-ttu-id="13b5c-148">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="13b5c-148">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="13b5c-149">Importovat data do Azure Cosmos DB pro hello MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="13b5c-149">Import data into Azure Cosmos DB for hello MongoDB API</span></span>](mongodb-migrate.md)

