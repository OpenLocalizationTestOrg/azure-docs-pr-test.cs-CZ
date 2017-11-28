---
title: "Služba Azure Cosmos DB: Sestavení webové aplikace s ověřením přes Xamarin a Facebook | Dokumentace Microsoftu"
description: "Uvede ukázku kódu rozhraní .NET, můžete použít tooconnect tooand dotaz na databázi Azure Cosmos"
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
ms.openlocfilehash: 5f71dddd2b2f5bd117e481c96c17915fc58d2119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a><span data-ttu-id="f1984-103">Služba Azure Cosmos DB: Sestavení webové aplikace s rozhraním .NET a ověřením přes Xamarin a Facebook</span><span class="sxs-lookup"><span data-stu-id="f1984-103">Azure Cosmos DB: Build a web app with .NET, Xamarin, and Facebook authentication</span></span>

<span data-ttu-id="f1984-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="f1984-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="f1984-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1984-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="f1984-106">Tento rychlý start předvádí, jak hello toocreate účet Azure Cosmos DB, dokumentu databáze a kolekce pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f1984-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="f1984-107">Budete pak sestavení a nasazení webové aplikace seznamu úkolů založený na hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/)a modul autorizace Azure Cosmos DB hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), and hello Azure Cosmos DB authorization engine.</span></span> <span data-ttu-id="f1984-108">Hello todo seznamu webové aplikace implementuje vzor data na uživatele, který umožňuje uživatelům toologin pomocí ověřování sítě Facebook a spravovat své vlastní toodo položky.</span><span class="sxs-lookup"><span data-stu-id="f1984-108">hello todo list web app implements a per-user data pattern that enables users toologin using Facebook Auth and manage their own toodo items.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1984-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f1984-109">Prerequisites</span></span>

<span data-ttu-id="f1984-110">Pokud ještě nemáte nainstalované Visual Studio 2017, můžete stáhnout a použít hello **volné** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f1984-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="f1984-111">Ujistěte se, že povolíte **Azure development** při instalaci sady Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="f1984-112">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="f1984-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="f1984-113">Přidání kolekce</span><span class="sxs-lookup"><span data-stu-id="f1984-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="f1984-114">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f1984-114">Clone hello sample application</span></span>

<span data-ttu-id="f1984-115">Teď umožňuje nastavit připojovací řetězec hello klonování DocumentDB API aplikace z githubu a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="f1984-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="f1984-116">Uvidíte, jak je snadné toowork s daty prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="f1984-116">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="f1984-117">Otevřete okno terminálu git, jako je například git bash a `cd` tooa pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="f1984-117">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="f1984-118">Spusťte následující příkaz tooclone hello Ukázka úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. <span data-ttu-id="f1984-119">Pak otevřete soubor DocumentDBTodo.sln hello ze složky samples/xamarin/UserItems/xamarin.forms hello v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f1984-119">Then open hello DocumentDBTodo.sln file from hello samples/xamarin/UserItems/xamarin.forms folder in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="f1984-120">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="f1984-120">Review hello code</span></span>

<span data-ttu-id="f1984-121">Hello kódu ve složce Xamarin hello obsahuje:</span><span class="sxs-lookup"><span data-stu-id="f1984-121">hello code in hello Xamarin folder contains:</span></span>

* <span data-ttu-id="f1984-122">Aplikaci Xamarin.</span><span class="sxs-lookup"><span data-stu-id="f1984-122">Xamarin app.</span></span> <span data-ttu-id="f1984-123">aplikace Hello ukládá položky úkolů hello uživatele v dělenou kolekci s názvem UserItems.</span><span class="sxs-lookup"><span data-stu-id="f1984-123">hello app stores hello user's todo items in a partitioned collection named UserItems.</span></span>
* <span data-ttu-id="f1984-124">Rozhraní API zprostředkovatele tokenu prostředku.</span><span class="sxs-lookup"><span data-stu-id="f1984-124">Resource token broker API.</span></span> <span data-ttu-id="f1984-125">Prostředek Azure Cosmos DB toobroker jednoduché rozhraní ASP.NET Web API tokeny toohello přihlášení uživatele aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-125">A simple ASP.NET Web API toobroker Azure Cosmos DB resource tokens toohello logged in users of hello app.</span></span> <span data-ttu-id="f1984-126">Tokeny prostředků jsou krátkodobou přístupové tokeny, které aplikace hello poskytnout toohello přístup hello přihlášení uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="f1984-126">Resource tokens are short-lived access tokens that provide hello app with hello access toohello logged in user's data.</span></span>

<span data-ttu-id="f1984-127">tok ověřování a data Hello je znázorněno v následujícím diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-127">hello authentication and data flow is illustrated in hello diagram below.</span></span>

* <span data-ttu-id="f1984-128">Hello UserItems kolekce je vytvořena s klíčem oddílu hello "/ userid'.</span><span class="sxs-lookup"><span data-stu-id="f1984-128">hello UserItems collection is created with hello partition key '/userid'.</span></span> <span data-ttu-id="f1984-129">Určení, že klíč oddílu pro kolekci nekonečnou umožňuje tooscale Azure Cosmos DB jako hello počet uživatelů a položky zvětšování.</span><span class="sxs-lookup"><span data-stu-id="f1984-129">Specifying a partition key for a collection allows Azure Cosmos DB tooscale infinitely as hello number of users and items grows.</span></span>
* <span data-ttu-id="f1984-130">umožňuje aplikaci Xamarin Hello toologin uživatelů s přihlašovacími údaji Facebook.</span><span class="sxs-lookup"><span data-stu-id="f1984-130">hello Xamarin app allows users toologin with Facebook credentials.</span></span>
* <span data-ttu-id="f1984-131">aplikace Xamarin Hello používá tooauthenticate tokenu přístupu Facebook s rozhraním API ResourceTokenBroker</span><span class="sxs-lookup"><span data-stu-id="f1984-131">hello Xamarin app uses Facebook access token tooauthenticate with ResourceTokenBroker API</span></span>
* <span data-ttu-id="f1984-132">Hello prostředků zprostředkovatele tokenu rozhraní API ověří žádost o hello pomocí funkce ověřování aplikace služby a požadavky tokenu prostředků Azure Cosmos DB s dokumenty tooall přístup pro čtení a zápis sdílení klíč oddílu hello ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1984-132">hello resource token broker API authenticates hello request using App Service Auth feature, and requests an Azure Cosmos DB resource token with read/write access tooall documents sharing hello authenticated user's partition key.</span></span>
* <span data-ttu-id="f1984-133">Token zprostředkovatele prostředků vrátí hello prostředků tokenu toohello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1984-133">Resource token broker returns hello resource token toohello client app.</span></span>
* <span data-ttu-id="f1984-134">aplikace Hello přístup k položek todo hello uživatele pomocí tokenu hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="f1984-134">hello app accesses hello user's todo items using hello resource token.</span></span>

![Aplikace seznamu úkolů s ukázkovými daty](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a><span data-ttu-id="f1984-136">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="f1984-136">Update your connection string</span></span>

<span data-ttu-id="f1984-137">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-137">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="f1984-138">V hello [portál Azure](http://portal.azure.com/), ve vašem Azure Cosmos DB účet, klikněte v levé navigační hello **klíče**a potom klikněte na **klíče pro čtení a zápis**.</span><span class="sxs-lookup"><span data-stu-id="f1984-138">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="f1984-139">Do souboru Web.config hello v dalším kroku hello použijete hello tlačítka Kopírovat na pravé straně hello hello obrazovky toocopy hello URI a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="f1984-139">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello Web.config file in hello next step.</span></span>

    ![Zobrazení a zkopírování přístupový klíč v hello portálu Azure, okna klíče](./media/create-documentdb-xamarin-dotnet/keys.png)

2. <span data-ttu-id="f1984-141">V aplikaci Visual Studio 2017 otevřete soubor Web.config hello ve složce azure-documentdb-dotnet nebo ukázky nebo xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-141">In Visual Studio 2017, open hello Web.config file in hello azure-documentdb-dotnet/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker folder.</span></span> 

3. <span data-ttu-id="f1984-142">Zkopírujte URI hodnota z portálu hello (pomocí tlačítka kopírování hello) a nastavit jej jako hello hodnotu hello accountUrl v souboru Web.config.</span><span class="sxs-lookup"><span data-stu-id="f1984-142">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello accountUrl in Web.config.</span></span> 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. <span data-ttu-id="f1984-143">Poté zkopírujte primární klíč hodnota z portálu hello a nastavit jej jako hodnota hello accountKey v Web.congif hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-143">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello accountKey in Web.congif.</span></span> 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

<span data-ttu-id="f1984-144">Jste nyní aktualizovat vaši aplikaci s všechny údaje hello potřebuje toocommunicate s Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1984-144">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="build-and-deploy-hello-web-app"></a><span data-ttu-id="f1984-145">Vytvoření a nasazení webové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f1984-145">Build and deploy hello web app</span></span>

1. <span data-ttu-id="f1984-146">V hello portálu Azure vytvořte App Service Web toohost hello prostředků tokenu zprostředkovatele rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f1984-146">In hello Azure portal, create an App Service website toohost hello Resource token broker API.</span></span>
2. <span data-ttu-id="f1984-147">V hello portálu Azure otevřete okno nastavení aplikace hello hello prostředků zprostředkovatele tokenu web API.</span><span class="sxs-lookup"><span data-stu-id="f1984-147">In hello Azure portal, open hello App Settings blade of hello Resource token broker API website.</span></span> <span data-ttu-id="f1984-148">Vyplňte následující nastavení aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="f1984-148">Fill in hello following app settings:</span></span>

    * <span data-ttu-id="f1984-149">accountUrl – adresa URL účtu Azure Cosmos DB hello z karty hello klíčů účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1984-149">accountUrl - hello Azure Cosmos DB account URL from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="f1984-150">accountKey - hello Azure Cosmos DB účet hlavní klíč z karty hello klíčů účtu Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f1984-150">accountKey - hello Azure Cosmos DB account master key from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="f1984-151">Parametr DatabaseId a collectionId vytvořené databáze a kolekce</span><span class="sxs-lookup"><span data-stu-id="f1984-151">databaseId and collectionId of your created database and collection</span></span>

3. <span data-ttu-id="f1984-152">Publikujte hello tooyour řešení ResourceTokenBroker vytvořit web.</span><span class="sxs-lookup"><span data-stu-id="f1984-152">Publish hello ResourceTokenBroker solution tooyour created website.</span></span>

4. <span data-ttu-id="f1984-153">Otevřete projekt Xamarin hello a přejděte tooTodoItemManager.cs.</span><span class="sxs-lookup"><span data-stu-id="f1984-153">Open hello Xamarin project, and navigate tooTodoItemManager.cs.</span></span> <span data-ttu-id="f1984-154">Vyplňte hodnoty hello accountURL, collectionId, hodnotu databaseId, jakož i resourceTokenBrokerURL hello adresy url základní https pro web tokenu zprostředkovatele prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-154">Fill in hello values for accountURL, collectionId, databaseId, as well as resourceTokenBrokerURL as hello base https url for hello resource token broker website.</span></span>

5. <span data-ttu-id="f1984-155">Dokončení hello [jak tooconfigure přihlašování Facebook toouse aplikace služby App Service](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) ověřování Facebook kurz toosetup a nakonfigurovat web ResourceTokenBroker hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-155">Complete hello [How tooconfigure your App Service application toouse Facebook login](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) tutorial toosetup Facebook authentication and configure hello ResourceTokenBroker website.</span></span>

    <span data-ttu-id="f1984-156">Spuštění aplikace Xamarin hello.</span><span class="sxs-lookup"><span data-stu-id="f1984-156">Run hello Xamarin app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="f1984-157">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f1984-157">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="f1984-158">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="f1984-158">Clean up resources</span></span>

<span data-ttu-id="f1984-159">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f1984-159">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="f1984-160">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředku, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="f1984-160">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you just created.</span></span> 
2. <span data-ttu-id="f1984-161">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="f1984-161">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1984-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1984-162">Next steps</span></span>

<span data-ttu-id="f1984-163">V tento rychlý start když jste se naučili jak toocreate účtu Azure Cosmos DB, vytvořte kolekci pomocí hello Průzkumníku dat a sestavení a nasazení aplikace Xamarin.</span><span class="sxs-lookup"><span data-stu-id="f1984-163">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and build and deploy a Xamarin app.</span></span> <span data-ttu-id="f1984-164">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="f1984-164">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="f1984-165">Importování dat do služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f1984-165">Import data into Azure Cosmos DB</span></span>](import-data.md)
