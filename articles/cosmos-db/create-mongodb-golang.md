---
title: "Azure Cosmos DB: Sestavení aplikace konzoly v jazyce Go s rozhraním API MongoDB na webu Azure Portal | Dokumentace Microsoftu"
description: "Obsahuje vzorový kód v jazyce Go, který můžete použít pro připojení ke službě Azure Cosmos DB a zadávání dotazů."
services: cosmos-db
author: Durgaprasad-Budhwani
manager: jhubbard
editor: mimig1
ms.service: cosmos-db
ms.topic: hero-article
ms.date: 07/21/2017
ms.author: mimig
ms.openlocfilehash: 9461a5d86b321fd02167379ba8751d44a861ebc2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-the-azure-portal"></a><span data-ttu-id="b5726-103">Azure Cosmos DB: Sestavení aplikace konzoly v jazyce Go s rozhraním API MongoDB na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5726-103">Azure Cosmos DB: Build a MongoDB API console app with Golang and the Azure portal</span></span>

<span data-ttu-id="b5726-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="b5726-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="b5726-105">Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5726-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span>

<span data-ttu-id="b5726-106">Tento rychlý start popisuje způsob použití stávající aplikace [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) napsané v jazyce [Go](https://golang.org/) a připojení k databázi Azure Cosmos DB, která podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b5726-106">This quick-start demonstrates how to use an existing [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) app written in [Golang](https://golang.org/) and connect it to your Azure Cosmos DB database, which supports MongoDB client connections.</span></span>

<span data-ttu-id="b5726-107">Jinými slovy: aplikace v jazyce Go ví pouze to, že se připojuje k databázi pomocí rozhraní API MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b5726-107">In other words, your Golang application only knows that it's connecting to a database using MongoDB APIs.</span></span> <span data-ttu-id="b5726-108">V aplikaci se transparentně zobrazuje, že data jsou uložena ve službě Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5726-108">It is transparent to the application that the data is stored in Azure Cosmos DB.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5726-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b5726-109">Prerequisites</span></span>

- <span data-ttu-id="b5726-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b5726-110">An Azure subscription.</span></span> <span data-ttu-id="b5726-111">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="b5726-111">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free) before you begin.</span></span>
- <span data-ttu-id="b5726-112">[Go](https://golang.org/dl/) a základní znalost jazyka [Go](https://golang.org/).</span><span class="sxs-lookup"><span data-stu-id="b5726-112">[Go](https://golang.org/dl/) and a basic knowledge of the [Go](https://golang.org/) language.</span></span>
- <span data-ttu-id="b5726-113">Integrované vývojové prostředí – [Gogland](https://www.jetbrains.com/go/) od JetBrains, [Visual Studio Code](https://code.visualstudio.com/) od Microsoftu nebo [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="b5726-113">An IDE — [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span> <span data-ttu-id="b5726-114">V tomto kurzu používáme Gogland.</span><span class="sxs-lookup"><span data-stu-id="b5726-114">In this tutorial, I'm using Goglang.</span></span>

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="b5726-115">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="b5726-115">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="b5726-116">Klonování ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="b5726-116">Clone the sample application</span></span>

<span data-ttu-id="b5726-117">Naklonujte ukázkovou aplikaci a nainstalujte požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="b5726-117">Clone the sample application and install the required packages.</span></span>

1. <span data-ttu-id="b5726-118">Vytvořte složku s názvem CosmosDBSample ve složce GOROOT\src, což je ve výchozím nastavení C:\Go\.</span><span class="sxs-lookup"><span data-stu-id="b5726-118">Create a folder named CosmosDBSample inside the GOROOT\src folder, which is C:\Go\ by default.</span></span>
2. <span data-ttu-id="b5726-119">V okně terminálu Git, jako je třeba Git Bash, spusťte následující příkaz, který naklonuje ukázkové úložiště do složky CosmosDBSample.</span><span class="sxs-lookup"><span data-stu-id="b5726-119">Run the following command using a git terminal window such as git bash to clone the sample repository into the CosmosDBSample folder.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  <span data-ttu-id="b5726-120">Spuštěním následujícího příkazu získejte balíček mgo.</span><span class="sxs-lookup"><span data-stu-id="b5726-120">Run the following command to get the mgo package.</span></span> 

    ```
    go get gopkg.in/mgo.v2
    ```

<span data-ttu-id="b5726-121">Ovladač [mgo](http://labix.org/mgo) (vyslovuje se jako *mango*) je ovladač [MongoDB](http://www.mongodb.org/) pro [jazyk Go](http://golang.org/), který implementuje bohatý a dobře otestovaný výběr funkcí v rámci velmi jednoduchého rozhraní API s využitím standardních frází jazyka Go.</span><span class="sxs-lookup"><span data-stu-id="b5726-121">The [mgo](http://labix.org/mgo) driver (pronounced as *mango*) is a [MongoDB](http://www.mongodb.org/) driver for the [Go language](http://golang.org/) that implements a rich and well tested selection of features under a very simple API following standard Go idioms.</span></span>

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a><span data-ttu-id="b5726-122">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="b5726-122">Update your connection string</span></span>

<span data-ttu-id="b5726-123">Teď se vraťte zpátky na web Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.</span><span class="sxs-lookup"><span data-stu-id="b5726-123">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="b5726-124">Informace o připojovacím řetězci požadované aplikací v jazyce Go zobrazíte tak, že v navigační nabídce vlevo kliknete na **Rychlý start** a pak na **Jiné**.</span><span class="sxs-lookup"><span data-stu-id="b5726-124">Click **Quick start** in the left navigation menu, and then click **Other** to view the connection string information required by the Go application.</span></span>

2. <span data-ttu-id="b5726-125">V prostředí Gogland otevřete soubor main.go v adresáři GOROOT\CosmosDBSample a aktualizujte následující řádky kódu s použitím informací o připojovacím řetězci z webu Azure Portal, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="b5726-125">In Goglang, open the main.go file in the GOROOT\CosmosDBSample directory and update the following lines of code using the connection string information from the Azure portal as shown in the following screenshot.</span></span> 

    <span data-ttu-id="b5726-126">Název databáze je předpona hodnoty **Hostitel** v podokně připojovacího řetězce na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b5726-126">The Database name is the prefix of the **Host** value in the Azure portal connection string pane.</span></span> <span data-ttu-id="b5726-127">Pro účet znázorněný v obrázku níže je název databáze golang-coach.</span><span class="sxs-lookup"><span data-stu-id="b5726-127">For the account shown in the image below, the Database name is golang-coach.</span></span>

    ```go
    Database: "The prefix of the Host value in the Azure portal",
    Username: "The Username in the Azure portal",
    Password: "The Password in the Azure portal",
    ```

    ![Podokno Rychlý start, karta Jiné na webu Azure Portal ukazující informace o připojovacím řetězci](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. <span data-ttu-id="b5726-129">Uložte soubor main.go.</span><span class="sxs-lookup"><span data-stu-id="b5726-129">Save the main.go file.</span></span>

## <a name="review-the-code"></a><span data-ttu-id="b5726-130">Kontrola kódu</span><span class="sxs-lookup"><span data-stu-id="b5726-130">Review the code</span></span>

<span data-ttu-id="b5726-131">Ještě jednou se stručně podívejme na to, co se v souboru main.go děje.</span><span class="sxs-lookup"><span data-stu-id="b5726-131">Let's make a quick review of what's happening in the main.go file.</span></span> 

### <a name="connecting-the-go-app-to-azure-cosmos-db"></a><span data-ttu-id="b5726-132">Připojení aplikace v jazyce Go ke službě Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b5726-132">Connecting the Go app to Azure Cosmos DB</span></span>

<span data-ttu-id="b5726-133">Azure Cosmos DB podporuje MongoDB s povoleným protokolem SSH.</span><span class="sxs-lookup"><span data-stu-id="b5726-133">Azure Cosmos DB supports the SSL-enabled MongoDB.</span></span> <span data-ttu-id="b5726-134">Pokud se chcete připojit k MongoDB s povoleným protokolem SSH, je potřeba ve třídě [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo) definovat funkci **DialServer** a pomocí funkce [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) provést připojení.</span><span class="sxs-lookup"><span data-stu-id="b5726-134">To connect to an SSL-enabled MongoDB, you need to define the **DialServer** function in [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo), and make use of the [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) function to perform the connection.</span></span>

<span data-ttu-id="b5726-135">Následující fragment kódu jazyka Go připojí aplikaci v jazyce Go pomocí rozhraní API MongoDB pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b5726-135">The following Golang code snippet connects the Go app with Azure Cosmos DB MongoDB API.</span></span> <span data-ttu-id="b5726-136">Třída *DialInfo* obsahuje možnosti pro vytvoření relace s clusterem MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b5726-136">The *DialInfo* class holds options for establishing a session with a MongoDB cluster.</span></span>

```go
// DialInfo holds options for establishing a session with a MongoDB cluster.
dialInfo := &mgo.DialInfo{
    Addrs:    []string{"golang-couch.documents.azure.com:10255"}, // Get HOST + PORT
    Timeout:  60 * time.Second,
    Database: "database", // It can be anything
    Username: "username", // Username
    Password: "Azure database connect password from Azure Portal", // PASSWORD
    DialServer: func(addr *mgo.ServerAddr) (net.Conn, error) {
        return tls.Dial("tcp", addr.String(), &tls.Config{})
    },
}

// Create a session which maintains a pool of socket connections
// to our Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect to mongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes the session safety mode.
// If the safe parameter is nil, the session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. The unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

<span data-ttu-id="b5726-137">Metoda **mgo.Dial()** se používá v případě, že není k dispozici připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="b5726-137">The **mgo.Dial()** method is used when there is no SSL connection.</span></span> <span data-ttu-id="b5726-138">Pro připojení SSL se vyžaduje metoda **mgo.DialWithInfo()**.</span><span class="sxs-lookup"><span data-stu-id="b5726-138">For an SSL connection, the **mgo.DialWithInfo()** method is required.</span></span>

<span data-ttu-id="b5726-139">Instance objektu **DialWIthInfo{}** slouží k vytvoření objektu relace.</span><span class="sxs-lookup"><span data-stu-id="b5726-139">An instance of the **DialWIthInfo{}** object is used to create the session object.</span></span> <span data-ttu-id="b5726-140">Po vytvoření relace můžete je kolekci přistupovat pomocí následujícího fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="b5726-140">Once the session is established, you can access the collection by using the following code snippet:</span></span>

```go
collection := session.DB(“database”).C(“package”)
```

<a id="create-document"></a>

### <a name="create-a-document"></a><span data-ttu-id="b5726-141">Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="b5726-141">Create a document</span></span>

```go
// Model
type Package struct {
    Id bson.ObjectId  `bson:"_id,omitempty"`
    FullName      string
    Description   string
    StarsCount    int
    ForksCount    int
    LastUpdatedBy string
}

// insert Document in collection
err = collection.Insert(&Package{
    FullName:"react",
    Description:"A framework for building native apps with React.",
    ForksCount: 11392,
    StarsCount:48794,
    LastUpdatedBy:"shergin",

})

if err != nil {
    log.Fatal("Problem inserting data: ", err)
    return
}
```

### <a name="query-or-read-a-document"></a><span data-ttu-id="b5726-142">Dotazování nebo čtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="b5726-142">Query or read a document</span></span>

<span data-ttu-id="b5726-143">Azure Cosmos DB podporuje bohaté dotazy na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="b5726-143">Azure Cosmos DB supports rich queries against JSON documents stored in each collection.</span></span> <span data-ttu-id="b5726-144">Následující ukázka kódu obsahuje dotaz, který je možné spustit proti dokumentům v kolekci.</span><span class="sxs-lookup"><span data-stu-id="b5726-144">The following sample code shows a query that you can run against the documents in your collection.</span></span>

```go
// Get a Document from the collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a><span data-ttu-id="b5726-145">Aktualizace dokumentu</span><span class="sxs-lookup"><span data-stu-id="b5726-145">Update a document</span></span>

```go
// Update a document
updateQuery := bson.M{"_id": result.Id}
change := bson.M{"$set": bson.M{"fullname": "react-native"}}
err = collection.Update(updateQuery, change)
if err != nil {
    log.Fatal("Error updating record: ", err)
    return
}
```

### <a name="delete-a-document"></a><span data-ttu-id="b5726-146">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="b5726-146">Delete a document</span></span>

<span data-ttu-id="b5726-147">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="b5726-147">Azure Cosmos DB supports deleting JSON documents.</span></span>

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-the-app"></a><span data-ttu-id="b5726-148">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="b5726-148">Run the app</span></span>

1. <span data-ttu-id="b5726-149">V prostředí Gogland ověřte, že cesta GOPATH (najdete ji v části **File** (Soubor), **Settings** (Nastavení), **Go**, **GOPATH**) zahrnuje umístění, ve kterém byl nainstalován balíček gopkg, což je ve výchozím nastavení PROFIL_UŽIVATELE\go.</span><span class="sxs-lookup"><span data-stu-id="b5726-149">In Goglang, ensure that your GOPATH (available under **File**, **Settings**, **Go**, **GOPATH**) include the location in which the gopkg was installed, which is USERPROFILE\go by default.</span></span> 
2. <span data-ttu-id="b5726-150">Okomentujte řádky pro odstranění dokumentu (řádky 91–96), abyste dokument po spuštění aplikace mohli zobrazit.</span><span class="sxs-lookup"><span data-stu-id="b5726-150">Comment out the lines that delete the document, lines 91-96, so that you can see the document after running the app.</span></span>
3. <span data-ttu-id="b5726-151">V prostředí Gogland klikněte na **Run** (Spustit) a pak na **Run 'Build main.go and run'** (Spustit akci „Sestavit a spustit main.go“).</span><span class="sxs-lookup"><span data-stu-id="b5726-151">In Goglang, click **Run**, and then click **Run 'Build main.go and run'**.</span></span>

    <span data-ttu-id="b5726-152">Aplikace dokončí dokument vytvořený v části [Vytvoření dokumentu](#create-document) a zobrazí jeho popis.</span><span class="sxs-lookup"><span data-stu-id="b5726-152">The app finishes and displays the description of the document created in [Create a document](#create-document).</span></span>
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Gogland zobrazující výstup aplikace](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a><span data-ttu-id="b5726-154">Kontrola dokumentu v Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="b5726-154">Review your document in Data Explorer</span></span>

<span data-ttu-id="b5726-155">Vraťte se na web Azure Portal a zobrazte dokument v Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="b5726-155">Go back to the Azure portal to see your document in Data Explorer.</span></span>

1. <span data-ttu-id="b5726-156">V navigační nabídce vlevo klikněte na **Průzkumník dat (Preview)**, rozbalte **golang-coach**, **balíček** a pak klikněte na **Dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="b5726-156">Click **Data Explorer (Preview)** in the left navigation menu, expand **golang-coach**, **package**, and then click **Documents**.</span></span> <span data-ttu-id="b5726-157">Na kartě **Dokumenty** klikněte na \_id a zobrazte dokument v pravém podokně.</span><span class="sxs-lookup"><span data-stu-id="b5726-157">In the **Documents** tab, click the \_id to display the document in the right pane.</span></span> 

    ![Průzkumník dat zobrazující nově vytvořený dokument](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. <span data-ttu-id="b5726-159">S dokumentem pak můžete přímo pracovat a kliknutím na **Aktualizovat** ho uložit.</span><span class="sxs-lookup"><span data-stu-id="b5726-159">You can then work with the document inline and click **Update** to save it.</span></span> <span data-ttu-id="b5726-160">Můžete také odstranit dokument nebo vytvořit nové dokumenty nebo dotazy.</span><span class="sxs-lookup"><span data-stu-id="b5726-160">You can also delete the document, or create new documents or queries.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="b5726-161">Ověření smluv SLA na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b5726-161">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="b5726-162">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="b5726-162">Clean up resources</span></span>

<span data-ttu-id="b5726-163">Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:</span><span class="sxs-lookup"><span data-stu-id="b5726-163">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="b5726-164">V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku.</span><span class="sxs-lookup"><span data-stu-id="b5726-164">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="b5726-165">Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="b5726-165">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5726-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b5726-166">Next steps</span></span>

<span data-ttu-id="b5726-167">V tomto rychlém startu jste se seznámili s postupem vytvoření účtu Azure Cosmos DB a spuštění aplikace v jazyce Go pomocí rozhraní API pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="b5726-167">In this quickstart, you've learned how to create an Azure Cosmos DB account and run a Golang app using the API for MongoDB.</span></span> <span data-ttu-id="b5726-168">Teď můžete do účtu databáze Cosmos importovat další data.</span><span class="sxs-lookup"><span data-stu-id="b5726-168">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b5726-169">Import dat do databáze Azure Cosmos pro rozhraní API MongoDB</span><span class="sxs-lookup"><span data-stu-id="b5726-169">Import data into Azure Cosmos DB for the MongoDB API</span></span>](mongodb-migrate.md)
