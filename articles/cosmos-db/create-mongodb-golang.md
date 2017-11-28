---
<span data-ttu-id="2a2fc-101">Title: aaa "Azure Cosmos DB: vytvoření aplikace konzoly MongoDB rozhraní API s Golang a hello portálu Azure | Popis Microsoft Docs": uvede ukázku kódu Golang tooconnect tooand můžete dotazovat služby Azure Cosmos DB: cosmos-db Autor: Durgaprasad Budhwani manager: jhubbard editor: mimig1</span><span class="sxs-lookup"><span data-stu-id="2a2fc-101">title: aaa"Azure Cosmos DB: Build a MongoDB API console app with Golang and hello Azure portal | Microsoft Docs" description: Presents a Golang code sample you can use tooconnect tooand query Azure Cosmos DB services: cosmos-db author: Durgaprasad-Budhwani manager: jhubbard editor: mimig1</span></span>

<span data-ttu-id="2a2fc-102">MS.Service: cosmos-db ms.topic: hero-article ms.date: 07/21/2017 ms.author: mimig</span><span class="sxs-lookup"><span data-stu-id="2a2fc-102">ms.service: cosmos-db ms.topic: hero-article ms.date: 07/21/2017 ms.author: mimig</span></span>
---

# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-golang-and-hello-azure-portal"></a><span data-ttu-id="2a2fc-103">Azure Cosmos DB: Vytvoření aplikace konzoly MongoDB rozhraní API s Golang a hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2a2fc-103">Azure Cosmos DB: Build a MongoDB API console app with Golang and hello Azure portal</span></span>

<span data-ttu-id="2a2fc-104">Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="2a2fc-105">Můžete rychle vytvořit a dotazovat dokumentu, klíč/hodnota a graf databází, které těžit z globální distribuční hello a možnosti vodorovné škálování jádrem hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span>

<span data-ttu-id="2a2fc-106">Tento úvodní ukazuje, jak toouse existující [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) aplikace napsané v [Golang](https://golang.org/) a připojte ho tooyour Azure Cosmos DB databáze, která podporuje připojení klienta MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-106">This quick-start demonstrates how toouse an existing [MongoDB](https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-introduction) app written in [Golang](https://golang.org/) and connect it tooyour Azure Cosmos DB database, which supports MongoDB client connections.</span></span>

<span data-ttu-id="2a2fc-107">Jinými slovy aplikace Golang pouze ví, že se počítač připojuje tooa databáze pomocí rozhraní API MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-107">In other words, your Golang application only knows that it's connecting tooa database using MongoDB APIs.</span></span> <span data-ttu-id="2a2fc-108">Je transparentní toohello aplikace, která hello dat je uložen v Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-108">It is transparent toohello application that hello data is stored in Azure Cosmos DB.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a2fc-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a2fc-109">Prerequisites</span></span>

- <span data-ttu-id="2a2fc-110">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-110">An Azure subscription.</span></span> <span data-ttu-id="2a2fc-111">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-111">If you don’t have an Azure subscription, create a [free account](https://azure.microsoft.com/free) before you begin.</span></span>
- <span data-ttu-id="2a2fc-112">[Přejděte](https://golang.org/dl/) a základní znalosti o hello [přejděte](https://golang.org/) jazyk.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-112">[Go](https://golang.org/dl/) and a basic knowledge of hello [Go](https://golang.org/) language.</span></span>
- <span data-ttu-id="2a2fc-113">Integrované vývojové prostředí – [Gogland](https://www.jetbrains.com/go/) od JetBrains, [Visual Studio Code](https://code.visualstudio.com/) od Microsoftu nebo [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="2a2fc-113">An IDE — [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span> <span data-ttu-id="2a2fc-114">V tomto kurzu používáme Gogland.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-114">In this tutorial, I'm using Goglang.</span></span>

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="2a2fc-115">Vytvoření účtu databáze</span><span class="sxs-lookup"><span data-stu-id="2a2fc-115">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="2a2fc-116">Klonování hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2a2fc-116">Clone hello sample application</span></span>

<span data-ttu-id="2a2fc-117">Klonování hello ukázkovou aplikaci a nainstalujte hello požadované balíčky.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-117">Clone hello sample application and install hello required packages.</span></span>

1. <span data-ttu-id="2a2fc-118">Vytvořte složku s názvem CosmosDBSample hello GOROOT\src naleznete ve složce, ve výchozím nastavení je C:\Go\.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-118">Create a folder named CosmosDBSample inside hello GOROOT\src folder, which is C:\Go\ by default.</span></span>
2. <span data-ttu-id="2a2fc-119">Spusťte následující příkaz pomocí okna terminálu git například git bash tooclone hello Ukázka úložiště do složky CosmosDBSample hello hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-119">Run hello following command using a git terminal window such as git bash tooclone hello sample repository into hello CosmosDBSample folder.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-golang-getting-started.git
    ```
3.  <span data-ttu-id="2a2fc-120">Spusťte následující příkaz tooget hello mgo balíček hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-120">Run hello following command tooget hello mgo package.</span></span> 

    ```
    go get gopkg.in/mgo.v2
    ```

<span data-ttu-id="2a2fc-121">Hello [mgo](http://labix.org/mgo) ovladačů (vyslovováno jako *mango*) je [MongoDB](http://www.mongodb.org/) ovladač pro hello [přejděte jazyk](http://golang.org/) , implementuje s formátováním a dobře testována Výběr funkcí v části velmi jednoduché rozhraní API následující standardní idioms přejděte.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-121">hello [mgo](http://labix.org/mgo) driver (pronounced as *mango*) is a [MongoDB](http://www.mongodb.org/) driver for hello [Go language](http://golang.org/) that implements a rich and well tested selection of features under a very simple API following standard Go idioms.</span></span>

<a id="connection-string"></a>

## <a name="update-your-connection-string"></a><span data-ttu-id="2a2fc-122">Aktualizace připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="2a2fc-122">Update your connection string</span></span>

<span data-ttu-id="2a2fc-123">Nyní přejděte zpět toohello Azure portálu tooget vaše informace o připojovacím řetězci a zkopírujte jej do aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-123">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="2a2fc-124">Klikněte na tlačítko **úvodní** v hello levé navigační nabídku a pak klikněte na **jiných** tooview hello informace o připojovacím řetězci vyžadovanou hello aplikaci přejděte.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-124">Click **Quick start** in hello left navigation menu, and then click **Other** tooview hello connection string information required by hello Go application.</span></span>

2. <span data-ttu-id="2a2fc-125">V Goglang otevřete hello main.go soubor v adresáři GOROOT\CosmosDBSample hello a aktualizujte následující řádky kódu pomocí hello připojovacím řetězcem z hello portálu Azure, jak ukazuje následující snímek obrazovky hello hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-125">In Goglang, open hello main.go file in hello GOROOT\CosmosDBSample directory and update hello following lines of code using hello connection string information from hello Azure portal as shown in hello following screenshot.</span></span> 

    <span data-ttu-id="2a2fc-126">Název databáze Hello je předpona hello hello **hostitele** hodnotu v podokně řetězec hello připojení portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-126">hello Database name is hello prefix of hello **Host** value in hello Azure portal connection string pane.</span></span> <span data-ttu-id="2a2fc-127">Pro účet hello znázorňuje následující obrázek hello je název databáze hello golang školit.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-127">For hello account shown in hello image below, hello Database name is golang-coach.</span></span>

    ```go
    Database: "hello prefix of hello Host value in hello Azure portal",
    Username: "hello Username in hello Azure portal",
    Password: "hello Password in hello Azure portal",
    ```

    ![Rychlý start podokně kartu jiné v informace o připojovacím řetězci hello portálu Azure znázorňující hello](./media/create-mongodb-golang/cosmos-db-golang-connection-string.png)

3. <span data-ttu-id="2a2fc-129">Uložte soubor main.go hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-129">Save hello main.go file.</span></span>

## <a name="review-hello-code"></a><span data-ttu-id="2a2fc-130">Zkontrolujte hello kódu</span><span class="sxs-lookup"><span data-stu-id="2a2fc-130">Review hello code</span></span>

<span data-ttu-id="2a2fc-131">Provedeme jejich stručný přehled o dění v souboru main.go hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-131">Let's make a quick review of what's happening in hello main.go file.</span></span> 

### <a name="connecting-hello-go-app-tooazure-cosmos-db"></a><span data-ttu-id="2a2fc-132">Připojení hello tooAzure aplikace přejděte Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2a2fc-132">Connecting hello Go app tooAzure Cosmos DB</span></span>

<span data-ttu-id="2a2fc-133">Azure Cosmos DB podporuje hello MongoDB povolen protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-133">Azure Cosmos DB supports hello SSL-enabled MongoDB.</span></span> <span data-ttu-id="2a2fc-134">tooconnect tooan MongoDB povolen protokol SSL, je nutné toodefine hello **DialServer** fungovat v [mgo. DialInfo](http://gopkg.in/mgo.v2#DialInfo)a ujistěte se, použití hello [tls. *Vytočit* ](http://golang.org/pkg/crypto/tls#Dial) funkce tooperform hello připojení.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-134">tooconnect tooan SSL-enabled MongoDB, you need toodefine hello **DialServer** function in [mgo.DialInfo](http://gopkg.in/mgo.v2#DialInfo), and make use of hello [tls.*Dial*](http://golang.org/pkg/crypto/tls#Dial) function tooperform hello connection.</span></span>

<span data-ttu-id="2a2fc-135">Následující fragment kódu Golang Hello připojí hello přejděte aplikace pomocí rozhraní API služby Azure Cosmos DB MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-135">hello following Golang code snippet connects hello Go app with Azure Cosmos DB MongoDB API.</span></span> <span data-ttu-id="2a2fc-136">Hello *DialInfo* třída obsahuje možnosti pro vytvoření relace s clusterem s podporou MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-136">hello *DialInfo* class holds options for establishing a session with a MongoDB cluster.</span></span>

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
// tooour Azure Cosmos DB MongoDB database.
session, err := mgo.DialWithInfo(dialInfo)

if err != nil {
    fmt.Printf("Can't connect toomongo, go error %v\n", err)
    os.Exit(1)
}

defer session.Close()

// SetSafe changes hello session safety mode.
// If hello safe parameter is nil, hello session is put in unsafe mode, 
// and writes become fire-and-forget,
// without error checking. hello unsafe mode is faster since operations won't hold on waiting for a confirmation.
// 
session.SetSafe(&mgo.Safe{})
```

<span data-ttu-id="2a2fc-137">Hello **mgo. Dial()** metoda se používá, když není připojení SSL.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-137">hello **mgo.Dial()** method is used when there is no SSL connection.</span></span> <span data-ttu-id="2a2fc-138">Pro připojení SSL, hello **mgo. DialWithInfo()** se metoda.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-138">For an SSL connection, hello **mgo.DialWithInfo()** method is required.</span></span>

<span data-ttu-id="2a2fc-139">Instance hello **{DialWIthInfo}** objekt je použité toocreate objektu session hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-139">An instance of hello **DialWIthInfo{}** object is used toocreate hello session object.</span></span> <span data-ttu-id="2a2fc-140">Po vytvoření relace hello dostanete hello kolekce pomocí hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="2a2fc-140">Once hello session is established, you can access hello collection by using hello following code snippet:</span></span>

```go
collection := session.DB(“database”).C(“package”)
```

<a id="create-document"></a>

### <a name="create-a-document"></a><span data-ttu-id="2a2fc-141">Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="2a2fc-141">Create a document</span></span>

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

### <a name="query-or-read-a-document"></a><span data-ttu-id="2a2fc-142">Dotazování nebo čtení dokumentu</span><span class="sxs-lookup"><span data-stu-id="2a2fc-142">Query or read a document</span></span>

<span data-ttu-id="2a2fc-143">Azure Cosmos DB podporuje bohaté dotazy na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-143">Azure Cosmos DB supports rich queries against JSON documents stored in each collection.</span></span> <span data-ttu-id="2a2fc-144">Hello následující vzorový kód ukazuje dotaz, který můžete spustit na hello dokumenty v kolekci.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-144">hello following sample code shows a query that you can run against hello documents in your collection.</span></span>

```go
// Get a Document from hello collection
result := Package{}
err = collection.Find(bson.M{"fullname": "react"}).One(&result)
if err != nil {
    log.Fatal("Error finding record: ", err)
    return
}

fmt.Println("Description:", result.Description)
```


### <a name="update-a-document"></a><span data-ttu-id="2a2fc-145">Aktualizace dokumentu</span><span class="sxs-lookup"><span data-stu-id="2a2fc-145">Update a document</span></span>

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

### <a name="delete-a-document"></a><span data-ttu-id="2a2fc-146">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="2a2fc-146">Delete a document</span></span>

<span data-ttu-id="2a2fc-147">Azure Cosmos DB podporuje odstraňování dokumentů JSON.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-147">Azure Cosmos DB supports deleting JSON documents.</span></span>

```go
// Delete a document
query := bson.M{"_id": result.Id}
err = collection.Remove(query)
if err != nil {
   log.Fatal("Error deleting record: ", err)
   return
}
```
    
## <a name="run-hello-app"></a><span data-ttu-id="2a2fc-148">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="2a2fc-148">Run hello app</span></span>

1. <span data-ttu-id="2a2fc-149">V Goglang, ověřte, že vaše GOPATH (k dispozici v části **soubor**, **nastavení**, **přejděte**, **GOPATH**) hello umístění zahrnout jako které hello gopkg byla nainstalována, což je USERPROFILE\go ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-149">In Goglang, ensure that your GOPATH (available under **File**, **Settings**, **Go**, **GOPATH**) include hello location in which hello gopkg was installed, which is USERPROFILE\go by default.</span></span> 
2. <span data-ttu-id="2a2fc-150">Komentář hello řádky, které odstranit řádky 91-96 hello dokumentu hello dokumentu viděli po spuštěné aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-150">Comment out hello lines that delete hello document, lines 91-96, so that you can see hello document after running hello app.</span></span>
3. <span data-ttu-id="2a2fc-151">V prostředí Gogland klikněte na **Run** (Spustit) a pak na **Run 'Build main.go and run'** (Spustit akci „Sestavit a spustit main.go“).</span><span class="sxs-lookup"><span data-stu-id="2a2fc-151">In Goglang, click **Run**, and then click **Run 'Build main.go and run'**.</span></span>

    <span data-ttu-id="2a2fc-152">aplikace Hello dokončí a zobrazí popis hello vytvořené v dokumentu hello [vytvořit dokument](#create-document).</span><span class="sxs-lookup"><span data-stu-id="2a2fc-152">hello app finishes and displays hello description of hello document created in [Create a document](#create-document).</span></span>
    
    ```
    Description: A framework for building native apps with React.
    
    Process finished with exit code 0
    ```

    ![Goglang zobrazující hello výstup hello aplikace](./media/create-mongodb-golang/goglang-cosmos-db.png)
    
## <a name="review-your-document-in-data-explorer"></a><span data-ttu-id="2a2fc-154">Kontrola dokumentu v Průzkumníku dat</span><span class="sxs-lookup"><span data-stu-id="2a2fc-154">Review your document in Data Explorer</span></span>

<span data-ttu-id="2a2fc-155">Vraťte se zpátky toohello Azure portálu toosee vašeho dokumentu v Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-155">Go back toohello Azure portal toosee your document in Data Explorer.</span></span>

1. <span data-ttu-id="2a2fc-156">Klikněte na tlačítko **Data Explorer (Preview)** v levé navigační nabídce hello rozbalte **golang školit**, **balíček**a potom klikněte na **dokumenty**.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-156">Click **Data Explorer (Preview)** in hello left navigation menu, expand **golang-coach**, **package**, and then click **Documents**.</span></span> <span data-ttu-id="2a2fc-157">V hello **dokumenty** , klikněte na hello \_id toodisplay hello dokumentu v pravém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-157">In hello **Documents** tab, click hello \_id toodisplay hello document in hello right pane.</span></span> 

    ![Data Explorer zobrazující hello nově vytvořeného dokumentu](./media/create-mongodb-golang/golang-cosmos-db-data-explorer.png)
    
2. <span data-ttu-id="2a2fc-159">Můžete pracovat s vloženého dokumentu hello a klikněte na tlačítko **aktualizace** toosave ho.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-159">You can then work with hello document inline and click **Update** toosave it.</span></span> <span data-ttu-id="2a2fc-160">Můžete také odstranit hello dokumentu nebo vytvořit nové dokumenty nebo dotazy.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-160">You can also delete hello document, or create new documents or queries.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="2a2fc-161">Zkontrolujte SLA v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2a2fc-161">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="2a2fc-162">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="2a2fc-162">Clean up resources</span></span>

<span data-ttu-id="2a2fc-163">Pokud ale nebudete toocontinue toouse této aplikace, odstraňte všechny prostředky, které jsou vytvořené tento rychlý start v hello portál Azure s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2a2fc-163">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="2a2fc-164">V levé nabídce hello v hello portálu Azure klikněte na **skupiny prostředků** a pak klikněte na název hello hello prostředků, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-164">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="2a2fc-165">Na stránce skupiny prostředků, klikněte na tlačítko **odstranit**hello textového pole zadejte název hello toodelete hello prostředků a pak klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-165">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a2fc-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2a2fc-166">Next steps</span></span>

<span data-ttu-id="2a2fc-167">V tento rychlý start když jste se naučili jak toocreate účet Azure Cosmos DB a spusťte Golang aplikace pomocí hello rozhraní API pro MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-167">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and run a Golang app using hello API for MongoDB.</span></span> <span data-ttu-id="2a2fc-168">Nyní můžete importovat další data tooyour Cosmos DB účtu.</span><span class="sxs-lookup"><span data-stu-id="2a2fc-168">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="2a2fc-169">Importovat data do Azure Cosmos DB pro hello MongoDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2a2fc-169">Import data into Azure Cosmos DB for hello MongoDB API</span></span>](mongodb-migrate.md)
