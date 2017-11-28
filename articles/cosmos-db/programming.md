---
title: "programování v jazyce JavaScript na straně aaaServer pro Azure Cosmos DB | Microsoft Docs"
description: "Zjistěte, jak Azure Cosmos DB toowrite toouse uložené procedury, triggery databáze a uživatelem definované funkce (UDF) v jazyce JavaScript. Získáte tipy programing databáze a další."
keywords: "Databáze aktivační události, uložené procedury, uložené procedury, program databáze, sproc, documentdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: aliuy
manager: jhubbard
editor: mimig
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: andrl
ms.openlocfilehash: 5a011d1c4b0b5908d5de73607a1bc328ed1711d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a><span data-ttu-id="71ea5-105">Azure programování na straně serveru Cosmos DB: uložené procedury, triggery databáze a UDF</span><span class="sxs-lookup"><span data-stu-id="71ea5-105">Azure Cosmos DB server-side programming: Stored procedures, database triggers, and UDFs</span></span>
<span data-ttu-id="71ea5-106">Zjistěte, jak integrovat Azure Cosmos DB jazyka, spouštění transakcí jazyka JavaScript umožňuje vývojářům zápisu **uložené procedury**, **aktivační události** a **funkce (UDF)definovanéuživatelem** nativně v [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-106">Learn how Azure Cosmos DB’s language integrated, transactional execution of JavaScript lets developers write **stored procedures**, **triggers** and **user defined functions (UDFs)** natively in an [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span></span> <span data-ttu-id="71ea5-107">To vám umožní toowrite databáze programu aplikační logiky, která může být dodána a provedeny přímo na oddíly úložiště databáze hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-107">This allows you toowrite database program application logic that can be shipped and executed directly on hello database storage partitions.</span></span> 

<span data-ttu-id="71ea5-108">Doporučujeme stáhnout spustí sledováním hello následující video, kde Andrew Liu poskytuje programovací model stručný úvod tooCosmos DB na straně serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="71ea5-108">We recommend getting started by watching hello following video, where Andrew Liu provides a brief introduction tooCosmos DB's server-side database programming model.</span></span> 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

<span data-ttu-id="71ea5-109">Pak se vraťte toothis článku, kde se dozvíte hello odpovědi toohello následující otázky:</span><span class="sxs-lookup"><span data-stu-id="71ea5-109">Then, return toothis article, where you'll learn hello answers toohello following questions:</span></span>  

* <span data-ttu-id="71ea5-110">Jak psát uložené procedury, aktivační události nebo pomocí jazyka JavaScript UDF?</span><span class="sxs-lookup"><span data-stu-id="71ea5-110">How do I write a a stored procedure, trigger, or UDF using JavaScript?</span></span>
* <span data-ttu-id="71ea5-111">Jak Cosmos DB zaručit kyseliny?</span><span class="sxs-lookup"><span data-stu-id="71ea5-111">How does Cosmos DB guarantee ACID?</span></span>
* <span data-ttu-id="71ea5-112">Jak fungují transakce v databázi Cosmos?</span><span class="sxs-lookup"><span data-stu-id="71ea5-112">How do transactions work in Cosmos DB?</span></span>
* <span data-ttu-id="71ea5-113">Co se aktivuje před a po aktivuje a jak jeden zápis?</span><span class="sxs-lookup"><span data-stu-id="71ea5-113">What are pre-triggers and post-triggers and how do I write one?</span></span>
* <span data-ttu-id="71ea5-114">Jak registrovat a spustit uloženou proceduru, aktivační události nebo UDF RESTful způsobem pomocí protokolu HTTP?</span><span class="sxs-lookup"><span data-stu-id="71ea5-114">How do I register and execute a stored procedure, trigger, or UDF in a RESTful manner by using HTTP?</span></span>
* <span data-ttu-id="71ea5-115">Co Cosmos DB sady SDK jsou k dispozici toocreate a spuštění uložené procedury, triggery a UDF?</span><span class="sxs-lookup"><span data-stu-id="71ea5-115">What Cosmos DB SDKs are available toocreate and execute stored procedures, triggers, and UDFs?</span></span>

## <a name="introduction-toostored-procedure-and-udf-programming"></a><span data-ttu-id="71ea5-116">Úvod tooStored postupu a UDF programování</span><span class="sxs-lookup"><span data-stu-id="71ea5-116">Introduction tooStored Procedure and UDF Programming</span></span>
<span data-ttu-id="71ea5-117">Tento přístup z *"JavaScript jako moderní den T-SQL"* uvolní vývojáři aplikací z hello složitosti neshody typu systému a relační objekt mapování technologie.</span><span class="sxs-lookup"><span data-stu-id="71ea5-117">This approach of *“JavaScript as a modern day T-SQL”* frees application developers from hello complexities of type system mismatches and object-relational mapping technologies.</span></span> <span data-ttu-id="71ea5-118">Má také počet vnitřní výhody, které mohou být využívané toobuild bohaté aplikace:</span><span class="sxs-lookup"><span data-stu-id="71ea5-118">It also has a number of intrinsic advantages that can be utilized toobuild rich applications:</span></span>  

* <span data-ttu-id="71ea5-119">**Procedurální logika:** JavaScript jako nejvyšší úrovni programovací jazyk, nabízí bohaté a známá rozhraní tooexpress obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="71ea5-119">**Procedural Logic:** JavaScript as a high level programming language, provides a rich and familiar interface tooexpress business logic.</span></span> <span data-ttu-id="71ea5-120">Komplexní pořadí operací blíže toohello dat, můžete provést.</span><span class="sxs-lookup"><span data-stu-id="71ea5-120">You can perform complex sequences of operations closer toohello data.</span></span>
* <span data-ttu-id="71ea5-121">**Jednotlivé transakce:** Cosmos DB záruky, které databáze operací provést v jedné uložené procedury nebo aktivační události jsou atomic.</span><span class="sxs-lookup"><span data-stu-id="71ea5-121">**Atomic Transactions:** Cosmos DB guarantees that database operations performed inside a single stored procedure or trigger are atomic.</span></span> <span data-ttu-id="71ea5-122">To umožní aplikaci kombinovat související operace v jedné dávkové tak, aby všechny úspěšné nebo žádná z nich úspěšné.</span><span class="sxs-lookup"><span data-stu-id="71ea5-122">This lets an application combine related operations in a single batch so that either all of them succeed or none of them succeed.</span></span> 
* <span data-ttu-id="71ea5-123">**Výkon:** hello skutečnost, že JSON je systém typů jazyka Javascript vnitřně namapované toohello a také je základní jednotkou hello úložiště v systému Cosmos DB umožňuje počet optimalizace jako opožděné materialization JSON dokumentů ve vyrovnávací paměti hello fond a přitom k dispozici na vyžádání toohello provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-123">**Performance:** hello fact that JSON is intrinsically mapped toohello Javascript language type system and is also hello basic unit of storage in Cosmos DB allows for a number of optimizations like lazy materialization of JSON documents in hello buffer pool and making them available on-demand toohello executing code.</span></span> <span data-ttu-id="71ea5-124">Existují další výhody výkonu přidružené k přenosů obchodní logiky toohello databázi:</span><span class="sxs-lookup"><span data-stu-id="71ea5-124">There are more performance benefits associated with shipping business logic toohello database:</span></span>
  
  * <span data-ttu-id="71ea5-125">Dávkování – vývojáři můžou skupiny operací, jako je vložení a odesílat je hromadně.</span><span class="sxs-lookup"><span data-stu-id="71ea5-125">Batching – Developers can group operations like inserts and submit them in bulk.</span></span> <span data-ttu-id="71ea5-126">latence provoz sítě Hello náklady a hello úložiště režijní toocreate samostatné transakce jsou podstatně snížit.</span><span class="sxs-lookup"><span data-stu-id="71ea5-126">hello network traffic latency cost and hello store overhead toocreate separate transactions are reduced significantly.</span></span> 
  * <span data-ttu-id="71ea5-127">Předběžné kompilace – Cosmos DB překompiluje uložené procedury, triggery a uživatelem definované funkce (UDF) tooavoid náklady kompilace jazyka JavaScript pro každé vyvolání.</span><span class="sxs-lookup"><span data-stu-id="71ea5-127">Pre-compilation – Cosmos DB precompiles stored procedures, triggers and user defined functions (UDFs) tooavoid JavaScript compilation cost for each invocation.</span></span> <span data-ttu-id="71ea5-128">Hello režijní náklady na vytváření kódu byte hello hello Procedurální logika je amortizovaný tooa minimální hodnota.</span><span class="sxs-lookup"><span data-stu-id="71ea5-128">hello overhead of building hello byte code for hello procedural logic is amortized tooa minimal value.</span></span>
  * <span data-ttu-id="71ea5-129">Sekvencování – mnoho operations nutné vedlejším účinkem (dále jen "aktivační události"), potenciálně zahrnuje jednu nebo více provádění operací sekundárního úložiště.</span><span class="sxs-lookup"><span data-stu-id="71ea5-129">Sequencing – Many operations need a side-effect (“trigger”) that potentially involves doing one or many secondary store operations.</span></span> <span data-ttu-id="71ea5-130">Kromě zajištění dostatečného nedělitelnost, to je další původce při přesunutí toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="71ea5-130">Aside from atomicity, this is more performant when moved toohello server.</span></span> 
* <span data-ttu-id="71ea5-131">**Zapouzdření:** uložené procedury lze použít toogroup obchodní logiky v jednom místě.</span><span class="sxs-lookup"><span data-stu-id="71ea5-131">**Encapsulation:** Stored procedures can be used toogroup business logic in one place.</span></span> <span data-ttu-id="71ea5-132">To má dvě výhody:</span><span class="sxs-lookup"><span data-stu-id="71ea5-132">This has two advantages:</span></span>
  * <span data-ttu-id="71ea5-133">Přidá abstraktní vrstvu nad hello nezpracovaná data, která umožňuje tooevolve architekty dat svých aplikací nezávisle hello data.</span><span class="sxs-lookup"><span data-stu-id="71ea5-133">It adds an abstraction layer on top of hello raw data, which enables data architects tooevolve their applications independently from hello data.</span></span> <span data-ttu-id="71ea5-134">To je zvlášť výhodné, pokud hello data bez schémat, z důvodu toohello křehká předpoklady, které může být nutné toobe zaručená do aplikace hello, pokud mají toodeal s daty přímo.</span><span class="sxs-lookup"><span data-stu-id="71ea5-134">This is particularly advantageous when hello data is schema-less, due toohello brittle assumptions that may need toobe baked into hello application if they have toodeal with data directly.</span></span>  
  * <span data-ttu-id="71ea5-135">Tato abstrakce umožňuje podnikům zabezpečit svá data pomocí zjednodušení hello přístup z hello skriptů.</span><span class="sxs-lookup"><span data-stu-id="71ea5-135">This abstraction lets enterprises keep their data secure by streamlining hello access from hello scripts.</span></span>  

<span data-ttu-id="71ea5-136">Hello vytváření a spouštění aktivační události databáze, uložené procedury a operátory vlastního dotazu je podporované prostřednictvím hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), a [klientskou sadu SDK](documentdb-sdk-dotnet.md) na spoustě platforem včetně .NET, Node.js a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-136">hello creation and execution of database triggers, stored procedure and custom query operators is supported through hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), and [client SDKs](documentdb-sdk-dotnet.md) in many platforms including .NET, Node.js and JavaScript.</span></span>

<span data-ttu-id="71ea5-137">Tento kurz používá hello [Node.js SDK Q lišící](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntaxi a použití uložené procedury, triggery a UDF.</span><span class="sxs-lookup"><span data-stu-id="71ea5-137">This tutorial uses hello [Node.js SDK with Q Promises](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntax and usage of stored procedures, triggers, and UDFs.</span></span>   

## <a name="stored-procedures"></a><span data-ttu-id="71ea5-138">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="71ea5-138">Stored procedures</span></span>
### <a name="example-write-a-simple-stored-procedure"></a><span data-ttu-id="71ea5-139">Příklad: Zápis jednoduché uložené procedury</span><span class="sxs-lookup"><span data-stu-id="71ea5-139">Example: Write a simple stored procedure</span></span>
<span data-ttu-id="71ea5-140">Začněme jednoduché uložené procedury, jež vrátí odpověď "Hello World".</span><span class="sxs-lookup"><span data-stu-id="71ea5-140">Let’s start with a simple stored procedure that returns a “Hello World” response.</span></span>

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


<span data-ttu-id="71ea5-141">Uložené procedury jsou registrované na kolekci a mohou pracovat na všechny dokumentů a příloh, které se nachází v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="71ea5-141">Stored procedures are registered per collection, and can operate on any document and attachment present in that collection.</span></span> <span data-ttu-id="71ea5-142">Hello následující fragment kódu ukazuje, jak Hello World hello tooregister uložené procedury s kolekcí.</span><span class="sxs-lookup"><span data-stu-id="71ea5-142">hello following snippet shows how tooregister hello helloWorld stored procedure with a collection.</span></span> 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


<span data-ttu-id="71ea5-143">Po registraci hello uložené procedury můžeme provést proti kolekci hello a přečtěte si výsledky hello zpět na klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-143">Once hello stored procedure is registered, we can execute it against hello collection, and read hello results back at hello client.</span></span> 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


<span data-ttu-id="71ea5-144">objekt kontextu Hello poskytuje přístup tooall operace, které lze provést na úložiště Cosmos databáze a také přístup k objektům toohello žádostí a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="71ea5-144">hello context object provides access tooall operations that can be performed on Cosmos DB storage, as well as access toohello request and response objects.</span></span> <span data-ttu-id="71ea5-145">V takovém případě jsme použili hello objekt tooset hello odpovědi hello odpovědi, který vám byl zaslán zpět toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="71ea5-145">In this case, we used hello response object tooset hello body of hello response that was sent back toohello client.</span></span> <span data-ttu-id="71ea5-146">Další podrobnosti najdete v části toohello [server Azure Cosmos DB JavaScript dokumentaci k sadě SDK](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="71ea5-146">For more details, refer toohello [Azure Cosmos DB JavaScript server SDK documentation](http://azure.github.io/azure-documentdb-js-server/).</span></span>  

<span data-ttu-id="71ea5-147">Dejte nám rozbalte v tomto příkladu a přidat další databáze související funkce toohello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="71ea5-147">Let us expand on this example and add more database related functionality toohello stored procedure.</span></span> <span data-ttu-id="71ea5-148">Uložené procedury můžete vytvářet, aktualizovat, číst, dotaz a odstraňovat dokumentů a příloh uvnitř hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-148">Stored procedures can create, update, read, query and delete documents and attachments inside hello collection.</span></span>    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a><span data-ttu-id="71ea5-149">Příklad: Zápis uložené procedury toocreate dokumentu</span><span class="sxs-lookup"><span data-stu-id="71ea5-149">Example: Write a stored procedure toocreate a document</span></span>
<span data-ttu-id="71ea5-150">Hello další fragment kódu ukazuje, jak toouse hello kontextu objektu toointeract s prostředky Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="71ea5-150">hello next snippet shows how toouse hello context object toointeract with Cosmos DB resources.</span></span>

    var createDocumentStoredProc = {
        id: "createMyDocument",
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


<span data-ttu-id="71ea5-151">Tuto uloženou proceduru vezme jako vstupní documentToCreate hello text dokumentu toobe, vytvořené v aktuální kolekci hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-151">This stored procedure takes as input documentToCreate, hello body of a document toobe created in hello current collection.</span></span> <span data-ttu-id="71ea5-152">Všechny tyto operace jsou asynchronní a závisí na zpětná volání funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-152">All such operations are asynchronous and depend on JavaScript function callbacks.</span></span> <span data-ttu-id="71ea5-153">Funkce zpětného volání Hello má dva parametry, jeden pro objekt error hello v případě, že dojde k selhání operace hello a jeden pro hello vytvořený objekt.</span><span class="sxs-lookup"><span data-stu-id="71ea5-153">hello callback function has two parameters, one for hello error object in case hello operation fails, and one for hello created object.</span></span> <span data-ttu-id="71ea5-154">Uvnitř hello zpětné volání uživatelé mohou zpracování výjimky hello nebo vyvolána chyba.</span><span class="sxs-lookup"><span data-stu-id="71ea5-154">Inside hello callback, users can either handle hello exception or throw an error.</span></span> <span data-ttu-id="71ea5-155">V případě, že není k dispozici zpětné volání a dojde k chybě, runtime Azure Cosmos DB hello vyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-155">In case a callback is not provided and there is an error, hello Azure Cosmos DB runtime throws an error.</span></span>   

<span data-ttu-id="71ea5-156">V předchozím příkladu hello zpětného volání hello vrátí chybu, pokud hello operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="71ea5-156">In hello example above, hello callback throws an error if hello operation failed.</span></span> <span data-ttu-id="71ea5-157">Jinak nastaví hello id hello vytvořili dokument jako text hello hello odpovědi toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="71ea5-157">Otherwise, it sets hello id of hello created document as hello body of hello response toohello client.</span></span> <span data-ttu-id="71ea5-158">Zde je, jak se spustí tuto uloženou proceduru s vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="71ea5-158">Here is how this stored procedure is executed with input parameters.</span></span>

    // register hello stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure toocreate a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "hello Hitchhiker’s Guide toohello Galaxy",
                author: "Douglas Adams"
            };

            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });


<span data-ttu-id="71ea5-159">Všimněte si, že to uložené procedury lze upravené tootake pole těla dokumentu jako vstup a vytvořit všechny ve stejné uložené hello provádění procedury místo více síti požadavky toocreate každý z nich jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="71ea5-159">Note that this stored procedure can be modified tootake an array of document bodies as input and create them all in hello same stored procedure execution instead of multiple network requests toocreate each of them individually.</span></span> <span data-ttu-id="71ea5-160">To může být tooimplement použít efektivní hromadné – Importér pro DB Cosmos (popsané později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="71ea5-160">This can be used tooimplement an efficient bulk importer for Cosmos DB (discussed later in this tutorial).</span></span>   

<span data-ttu-id="71ea5-161">Příklad Hello popsané ukázán, jak toouse uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="71ea5-161">hello example described demonstrated how toouse stored procedures.</span></span> <span data-ttu-id="71ea5-162">Později v kurzu hello obsahuje triggery a uživatelem definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="71ea5-162">We will cover triggers and user defined functions (UDFs) later in hello tutorial.</span></span>

## <a name="database-program-transactions"></a><span data-ttu-id="71ea5-163">Databáze programu transakce</span><span class="sxs-lookup"><span data-stu-id="71ea5-163">Database program transactions</span></span>
<span data-ttu-id="71ea5-164">Transakce v typické databáze může být definováno jako posloupnost operací provést jako jednu logickou jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-164">Transaction in a typical database can be defined as a sequence of operations performed as a single logical unit of work.</span></span> <span data-ttu-id="71ea5-165">Poskytuje každou transakci **ACID záruky**.</span><span class="sxs-lookup"><span data-stu-id="71ea5-165">Each transaction provides **ACID guarantees**.</span></span> <span data-ttu-id="71ea5-166">Je kyselina dobře známé zkratku, který zastupuje čtyři vlastnosti - nedělitelnost, konzistence, izolace a odolnost.</span><span class="sxs-lookup"><span data-stu-id="71ea5-166">ACID is a well-known acronym that stands for four properties -  Atomicity, Consistency, Isolation and Durability.</span></span>  

<span data-ttu-id="71ea5-167">Stručně řečeno, nedělitelnost zaručuje, že všechny hello práci v transakci je považován za jednu jednotku kde buď všechny klade nebo hodnotu none.</span><span class="sxs-lookup"><span data-stu-id="71ea5-167">Briefly, atomicity guarantees that all hello work done inside a transaction is treated as a single unit where either all of it is committed or none.</span></span> <span data-ttu-id="71ea5-168">Konzistence zajišťuje, že hello data je vždy ve funkčním stavu interní napříč transakce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-168">Consistency makes sure that hello data is always in a good internal state across transactions.</span></span> <span data-ttu-id="71ea5-169">Izolace zaručuje, že žádné dvě transakce konfliktu s jinými – obecně, většina komerčních systémy poskytují více úrovní izolace, které lze použít podle potřeb aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-169">Isolation guarantees that no two transactions interfere with each other – generally, most commercial systems provide multiple isolation levels that can be used based on hello application needs.</span></span> <span data-ttu-id="71ea5-170">Odolnost zajistí, že všechny změny, která je potvrzena v databázi hello vždy bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="71ea5-170">Durability ensures that any change that’s committed in hello database will always be present.</span></span>   

<span data-ttu-id="71ea5-171">V databázi Cosmos JavaScript je hostován v hello stejné místo paměti jako hello databáze.</span><span class="sxs-lookup"><span data-stu-id="71ea5-171">In Cosmos DB, JavaScript is hosted in hello same memory space as hello database.</span></span> <span data-ttu-id="71ea5-172">Proto požadavky v rámci uložené procedury a triggery spustit v hello stejný rozsah relace databáze.</span><span class="sxs-lookup"><span data-stu-id="71ea5-172">Hence, requests made within stored procedures and triggers execute in hello same scope of a database session.</span></span> <span data-ttu-id="71ea5-173">To umožňuje Cosmos DB tooguarantee kyseliny pro všechny operace, které jsou součástí jedné uložené procedury nebo aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="71ea5-173">This enables Cosmos DB tooguarantee ACID for all operations that are part of a single stored procedure/trigger.</span></span> <span data-ttu-id="71ea5-174">Vezměte v úvahu následující hello uložené procedury definice:</span><span class="sxs-lookup"><span data-stu-id="71ea5-174">Consider hello following stored procedure definition:</span></span>

    // JavaScript source code
    var exchangeItemsSproc = {
        id: "exchangeItems",
        serverScript: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            var player1Document, player2Document;

            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);

                    if (documents.length != 1) throw "Unable toofind both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable toofind both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable tooread player details, abort ";
                });

            if (!accept) throw "Unable tooread player details, abort ";

            // swap hello two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable tooupdate player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable tooupdate player 2, abort"
                            });

                        if (!accept2) throw "Unable tooupdate player 2, abort";
                    });

                if (!accept) throw "Unable tooupdate player 1, abort";
            }
        }
    }

    // register hello stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

<span data-ttu-id="71ea5-175">Tuto uloženou proceduru používá transakce v rámci herní aplikace tootrade položky mezi dvěma přehrávače v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="71ea5-175">This stored procedure uses transactions within a gaming app tootrade items between two players in a single operation.</span></span> <span data-ttu-id="71ea5-176">Hello uložené procedury pokusy o tooread dva dokumenty, které každý odpovídající toohello player ID předaná jako argument.</span><span class="sxs-lookup"><span data-stu-id="71ea5-176">hello stored procedure attempts tooread two documents each corresponding toohello player IDs passed in as an argument.</span></span> <span data-ttu-id="71ea5-177">Pokud se oba dokumenty player, pak hello uložené procedury aktualizace podle odkládací jejich položky hello dokumenty.</span><span class="sxs-lookup"><span data-stu-id="71ea5-177">If both player documents are found, then hello stored procedure updates hello documents by swapping their items.</span></span> <span data-ttu-id="71ea5-178">Pokud společně hello způsob došlo k chybám, vyvolá výjimku JavaScript, která implicitně zruší hello transakce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-178">If any errors are encountered along hello way, it throws a JavaScript exception that implicitly aborts hello transaction.</span></span>

<span data-ttu-id="71ea5-179">Pokud hello kolekce hello uložené procedury je registrován proti je jedním oddílem kolekce a potom hello transakce je vymezená tooall hello dokumenty v rámci kolekce hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-179">If hello collection hello stored procedure is registered against is a single-partition collection, then hello transaction is scoped tooall hello documents within hello collection.</span></span> <span data-ttu-id="71ea5-180">Pokud hello kolekce rozdělena na oddíly, uložené procedury jsou spouštěny v oboru transakce hello klíče jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="71ea5-180">If hello collection is partitioned, then stored procedures are executed in hello transaction scope of a single partition key.</span></span> <span data-ttu-id="71ea5-181">Každý uložené provádění procedury pak musí obsahovat hodnotu klíče oddílu musí být spuštěna pod odpovídající toohello oboru hello transakce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-181">Each stored procedure execution must then include a partition key value corresponding toohello scope hello transaction must run under.</span></span> <span data-ttu-id="71ea5-182">Další podrobnosti najdete v tématu [Azure Cosmos DB dělení](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="71ea5-182">For more details, see [Azure Cosmos DB Partitioning](partition-data.md).</span></span>

### <a name="commit-and-rollback"></a><span data-ttu-id="71ea5-183">Potvrzení a vrácení zpět</span><span class="sxs-lookup"><span data-stu-id="71ea5-183">Commit and rollback</span></span>
<span data-ttu-id="71ea5-184">Transakce je úzce a nativně integrováno programovací model Cosmos DB jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-184">Transactions are deeply and natively integrated into Cosmos DB’s JavaScript programming model.</span></span> <span data-ttu-id="71ea5-185">Uvnitř funkce jazyka JavaScript jsou automaticky zabalená všechny operace v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-185">Inside a JavaScript function, all operations are automatically wrapped under a single transaction.</span></span> <span data-ttu-id="71ea5-186">Pokud hello JavaScript se dokončí bez jakékoli výjimky, se potvrdí hello provozní toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="71ea5-186">If hello JavaScript completes without any exception, hello operations toohello database are committed.</span></span> <span data-ttu-id="71ea5-187">V důsledku toho jsou implicitní v Cosmos DB hello "BEGIN TRANSACTION" a "Potvrzení transakcí" příkazy v relačních databází.</span><span class="sxs-lookup"><span data-stu-id="71ea5-187">In effect, hello “BEGIN TRANSACTION” and “COMMIT TRANSACTION” statements in relational databases are implicit in Cosmos DB.</span></span>  

<span data-ttu-id="71ea5-188">Pokud žádné výjimka, která je rozšířena z hello skriptu, bude Cosmos DB JavaScript runtime vrátit zpět hello celá transakce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-188">If there is any exception that’s propagated from hello script, Cosmos DB’s JavaScript runtime will roll back hello whole transaction.</span></span> <span data-ttu-id="71ea5-189">Jak je znázorněno v hello dříve je příklad, došlo k výjimce efektivně ekvivalentní tooa "Vrácení transakce" v databázi Cosmos.</span><span class="sxs-lookup"><span data-stu-id="71ea5-189">As shown in hello earlier example, throwing an exception is effectively equivalent tooa “ROLLBACK TRANSACTION” in Cosmos DB.</span></span>

### <a name="data-consistency"></a><span data-ttu-id="71ea5-190">Konzistence dat</span><span class="sxs-lookup"><span data-stu-id="71ea5-190">Data consistency</span></span>
<span data-ttu-id="71ea5-191">Uložené procedury a triggery jsou vždy provést u primární repliky hello hello Azure Cosmos DB kontejneru.</span><span class="sxs-lookup"><span data-stu-id="71ea5-191">Stored procedures and triggers are always executed on hello primary replica of hello Azure Cosmos DB container.</span></span> <span data-ttu-id="71ea5-192">Tím se zajistí, že čtení z uvnitř uložené procedury nabídka silnou konzistenci.</span><span class="sxs-lookup"><span data-stu-id="71ea5-192">This ensures that reads from inside stored procedures offer strong consistency.</span></span> <span data-ttu-id="71ea5-193">Dotazy pomocí uživatelem definované funkce mohou být provedeny u hello primární nebo sekundární repliku, ale je zajištěno toomeet hello požadovanou úroveň konzistence výběrem příslušné repliky hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-193">Queries using user defined functions can be executed on hello primary or any secondary replica, but we ensure toomeet hello requested consistency level by choosing hello appropriate replica.</span></span>

## <a name="bounded-execution"></a><span data-ttu-id="71ea5-194">Ohraničené provádění</span><span class="sxs-lookup"><span data-stu-id="71ea5-194">Bounded execution</span></span>
<span data-ttu-id="71ea5-195">Všechny operace Cosmos DB musí dokončit v povoleném zadaný server hello dobu trvání časového limitu požadavku.</span><span class="sxs-lookup"><span data-stu-id="71ea5-195">All Cosmos DB operations must complete within hello server specified request timeout duration.</span></span> <span data-ttu-id="71ea5-196">Toto omezení platí i funkce tooJavaScript (uložené procedury, triggery a uživatelem definované funkce).</span><span class="sxs-lookup"><span data-stu-id="71ea5-196">This constraint also applies tooJavaScript functions (stored procedures, triggers and user-defined functions).</span></span> <span data-ttu-id="71ea5-197">Pokud se tento časový limit operace nebude dokončena, hello transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="71ea5-197">If an operation does not complete with that time limit, hello transaction is rolled back.</span></span> <span data-ttu-id="71ea5-198">Funkce jazyka JavaScript musí dokončit v časovém limitu hello nebo implementovat pokračování na základě modelu toobatch nebo pokračovat v provádění.</span><span class="sxs-lookup"><span data-stu-id="71ea5-198">JavaScript functions must finish within hello time limit or implement a continuation based model toobatch/resume execution.</span></span>  

<span data-ttu-id="71ea5-199">V pořadí toosimplify vývoj uložené procedury a triggery toohandle časových limitů, všechny funkce v rámci objektu kolekce hello (pro vytvořit, číst, nahraďte a odstranění dokumentů a přílohy) vrátit logickou hodnotu, která představuje zda, operace dokončí.</span><span class="sxs-lookup"><span data-stu-id="71ea5-199">In order toosimplify development of stored procedures and triggers toohandle time limits, all functions under hello collection object (for create, read, replace, and delete of documents and attachments) return a Boolean value that represents whether that operation will complete.</span></span> <span data-ttu-id="71ea5-200">Pokud je tato hodnota false, je jako ukazatel toho, že hello časový limit je o tooexpire a hello postupu musíte zabalit až provádění.</span><span class="sxs-lookup"><span data-stu-id="71ea5-200">If this value is false, it is an indication that hello time limit is about tooexpire and that hello procedure must wrap up execution.</span></span>  <span data-ttu-id="71ea5-201">Operace první nepřijatelného úložiště operací ve frontě předchozí toohello je zaručeno toocomplete, pokud hello uložené procedury dokončení v čase a fronty nejsou žádné další žádosti.</span><span class="sxs-lookup"><span data-stu-id="71ea5-201">Operations queued prior toohello first unaccepted store operation are guaranteed toocomplete if hello stored procedure completes in time and does not queue any more requests.</span></span>  

<span data-ttu-id="71ea5-202">Funkce jazyka JavaScript jsou také vázaný na spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="71ea5-202">JavaScript functions are also bounded on resource consumption.</span></span> <span data-ttu-id="71ea5-203">Cosmos DB si vyhrazuje propustnosti na kolekci na základě velikosti hello zřízení účtu databáze.</span><span class="sxs-lookup"><span data-stu-id="71ea5-203">Cosmos DB reserves throughput per collection based on hello provisioned size of a database account.</span></span> <span data-ttu-id="71ea5-204">Propustnost se vyjadřuje jako normalizované jednotka procesoru, paměti a jednotek žádosti nebo RUs spotřeba vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="71ea5-204">Throughput is expressed in terms of a normalized unit of CPU, memory and IO consumption called request units or RUs.</span></span> <span data-ttu-id="71ea5-205">Funkce jazyka JavaScript může potenciálně spotřebovávat velký počet RUs během krátké doby a může získat míra limited, pokud bude dosažen limit hello kolekce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-205">JavaScript functions can potentially use up a large number of RUs within a short time, and might get rate-limited if hello collection’s limit is reached.</span></span> <span data-ttu-id="71ea5-206">V karanténě tooensure dostupnost primitivní databázové operace může být také prostředků náročné uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="71ea5-206">Resource intensive stored procedures might also be quarantined tooensure availability of primitive database operations.</span></span>  

### <a name="example-bulk-importing-data-into-a-database-program"></a><span data-ttu-id="71ea5-207">Příklad: Hromadný import dat do databáze programu</span><span class="sxs-lookup"><span data-stu-id="71ea5-207">Example: Bulk importing data into a database program</span></span>
<span data-ttu-id="71ea5-208">Dole je příklad uložené procedury, jež jsou zapsána toobulk import dokumenty do kolekce.</span><span class="sxs-lookup"><span data-stu-id="71ea5-208">Below is an example of a stored procedure that is written toobulk-import documents into a collection.</span></span> <span data-ttu-id="71ea5-209">Poznámka: jak hello uložené provádění procedury popisovače ohraničenou kontrolou hello logická hodnota návratová hodnota z createDocument a pak používá hello počet dokumentů v každé vyvolání hello uložené procedury tootrack a obnovit průběh vložit napříč dávky.</span><span class="sxs-lookup"><span data-stu-id="71ea5-209">Note how hello stored procedure handles bounded execution by checking hello Boolean return value from createDocument, and then uses hello count of documents inserted in each invocation of hello stored procedure tootrack and resume progress across batches.</span></span>

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // hello count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("hello array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call hello create API toocreate a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) hello createDocument request was not accepted. 
        //    In this case hello callback will not be called, we just call setBody and we are done.
        // 2) hello callback was called docs.length times.
        //    In this case all documents were created and we don’t need toocall tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If hello request was accepted, callback will be called.
            // Otherwise report current count back toohello client, 
            // which will call hello script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order tooprocess hello result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment hello count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set hello response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <span data-ttu-id="71ea5-210"><a id="trigger"></a>Aktivační události databáze</span><span class="sxs-lookup"><span data-stu-id="71ea5-210"><a id="trigger"></a> Database triggers</span></span>
### <a name="database-pre-triggers"></a><span data-ttu-id="71ea5-211">Databáze před aktivační události</span><span class="sxs-lookup"><span data-stu-id="71ea5-211">Database pre-triggers</span></span>
<span data-ttu-id="71ea5-212">Cosmos DB poskytuje triggery, které jsou provedeny nebo aktivovány operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-212">Cosmos DB provides triggers that are executed or triggered by an operation on a document.</span></span> <span data-ttu-id="71ea5-213">Například můžete zadat před aktivační událost, když vytváříte dokumentu – Tato předběžná aktivační událost se spustí před vytvořením hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-213">For example, you can specify a pre-trigger when you are creating a document – this pre-trigger will run before hello document is created.</span></span> <span data-ttu-id="71ea5-214">Hello následuje příklad jak před aktivační události lze použít toovalidate hello vlastností dokumentu, který je právě vytvářena:</span><span class="sxs-lookup"><span data-stu-id="71ea5-214">hello following is an example of how pre-triggers can be used toovalidate hello properties of a document that is being created:</span></span>

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document toobe created in hello current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update hello document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


<span data-ttu-id="71ea5-215">A hello odpovídající registrace klienta kódu Node.js hello aktivační události:</span><span class="sxs-lookup"><span data-stu-id="71ea5-215">And hello corresponding Node.js client-side registration code for hello trigger:</span></span>

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };

            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


<span data-ttu-id="71ea5-216">Předběžné aktivační události nemůže mít žádné vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="71ea5-216">Pre-triggers cannot have any input parameters.</span></span> <span data-ttu-id="71ea5-217">objekt žádosti Hello lze použít toomanipulate hello požadavek zpráva přidružená k operaci hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-217">hello request object can be used toomanipulate hello request message associated with hello operation.</span></span> <span data-ttu-id="71ea5-218">Zde hello předběžné aktivační události je spuštěn s hello vytvoření dokumentu a tělo zprávy požadavku hello obsahuje toobe dokumentu hello vytvořená ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="71ea5-218">Here, hello pre-trigger is being run with hello creation of a document, and hello request message body contains hello document toobe created in JSON format.</span></span>   

<span data-ttu-id="71ea5-219">Když jsou registrované aktivačních událostí, mohou uživatelé zadat hello operace, které můžete spustit s.</span><span class="sxs-lookup"><span data-stu-id="71ea5-219">When triggers are registered, users can specify hello operations that it can run with.</span></span> <span data-ttu-id="71ea5-220">Této aktivační události byl vytvořen s TriggerOperation.Create, což znamená, že hello následující není povoleno.</span><span class="sxs-lookup"><span data-stu-id="71ea5-220">This trigger was created with TriggerOperation.Create, which means hello following is not permitted.</span></span>

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a><span data-ttu-id="71ea5-221">Databáze po aktivační události</span><span class="sxs-lookup"><span data-stu-id="71ea5-221">Database post-triggers</span></span>
<span data-ttu-id="71ea5-222">Po aktivační události, například před aktivační události, jsou spojena s operací na dokument a nechcete provést žádné vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="71ea5-222">Post-triggers, like pre-triggers, are associated with an operation on a document and don’t take any input parameters.</span></span> <span data-ttu-id="71ea5-223">Mohou být spuštěny **po** hello operace byla dokončena a mít přístup toohello odpověď odeslaná zpráva toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="71ea5-223">They run **after** hello operation has completed, and have access toohello response message that is sent toohello client.</span></span>   

<span data-ttu-id="71ea5-224">Hello následující příklad ukazuje po aktivační události v akci:</span><span class="sxs-lookup"><span data-stu-id="71ea5-224">hello following example shows post-triggers in action:</span></span>

    var updateMetadataTrigger = {
        id: "updateMetadata",
        serverScript: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            // document that was created
            var createdDocument = response.getBody();

            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable tooupdate metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable toofind metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable tooupdate metadata, abort";
                               });
                         if(!accept) throw "Unable tooupdate metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


<span data-ttu-id="71ea5-225">Hello aktivační událost lze zaregistrovat, jak je znázorněno v následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-225">hello trigger can be registered as shown in hello following sample.</span></span>

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "hello Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };

            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


<span data-ttu-id="71ea5-226">Této aktivační události dotazuje na dokument metadat hello a aktualizuje s podrobnostmi o hello nově vytvořeného dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-226">This trigger queries for hello metadata document and updates it with details about hello newly created document.</span></span>  

<span data-ttu-id="71ea5-227">Jednou z věcí, je důležité, toonote hello **transakcí** provádění aktivační události do databáze. Cosmos.</span><span class="sxs-lookup"><span data-stu-id="71ea5-227">One thing that is important toonote is hello **transactional** execution of triggers in Cosmos DB.</span></span> <span data-ttu-id="71ea5-228">Této aktivační události po spouští jako součást hello stejné transakci jako hello vytvoření hello původního dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-228">This post-trigger runs as part of hello same transaction as hello creation of hello original document.</span></span> <span data-ttu-id="71ea5-229">Proto pokud jsme vyvolat výjimku z hello po aktivační události (například pokud jsme dokument metadat nelze tooupdate hello), celá transakce hello se nezdaří a vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="71ea5-229">Therefore, if we throw an exception from hello post-trigger (say if we are unable tooupdate hello metadata document), hello whole transaction will fail and be rolled back.</span></span> <span data-ttu-id="71ea5-230">Žádné dokumentu se vytvoří a bude vrácen výjimku.</span><span class="sxs-lookup"><span data-stu-id="71ea5-230">No document will be created, and an exception will be returned.</span></span>  

## <span data-ttu-id="71ea5-231"><a id="udf"></a>Uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="71ea5-231"><a id="udf"></a>User-defined functions</span></span>
<span data-ttu-id="71ea5-232">Uživatelem definované funkce (UDF) používané tooextend hello DocumentDB API SQL dotazu jazyka gramatiky a implementovat vlastní obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="71ea5-232">User-defined functions (UDFs) are used tooextend hello DocumentDB API SQL query language grammar and implement custom business logic.</span></span> <span data-ttu-id="71ea5-233">Je možné volat jedině z uvnitř dotazy.</span><span class="sxs-lookup"><span data-stu-id="71ea5-233">They can only be called from inside queries.</span></span> <span data-ttu-id="71ea5-234">Tyto nemají přístup k objektu context toohello a jsou určené toobe použít jako jen výpočetní JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-234">They do not have access toohello context object and are meant toobe used as compute-only JavaScript.</span></span> <span data-ttu-id="71ea5-235">Proto UDF lze spustit na sekundárních replikách hello služby Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="71ea5-235">Therefore, UDFs can be run on secondary replicas of hello Cosmos DB service.</span></span>  

<span data-ttu-id="71ea5-236">Hello následující ukázka vytvoří daně z příjmu toocalculate UDF podle sazby za různé příjem závorky a potom pomocí jeho uvnitř dotazu toofind všichni uživatelé, kteří placené více než 20 000 $ v daně.</span><span class="sxs-lookup"><span data-stu-id="71ea5-236">hello following sample creates a UDF toocalculate income tax based on rates for various income brackets, and then uses it inside a query toofind all people who paid more than $20,000 in taxes.</span></span>

    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


<span data-ttu-id="71ea5-237">Hello UDF lze následně použít v dotazech jako v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="71ea5-237">hello UDF can subsequently be used in queries like in hello following sample:</span></span>

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);

            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a><span data-ttu-id="71ea5-238">JavaScript language-integrated query rozhraní API</span><span class="sxs-lookup"><span data-stu-id="71ea5-238">JavaScript language-integrated query API</span></span>
<span data-ttu-id="71ea5-239">Kromě toho tooissuing dotazy pomocí DocumentDB SQL gramatika, hello serverové SDK vám umožní tooperform optimalizované dotazy pomocí rozhraní fluent JavaScript bez znalostí SQL.</span><span class="sxs-lookup"><span data-stu-id="71ea5-239">In addition tooissuing queries using DocumentDB’s SQL grammar, hello server-side SDK allows you tooperform optimized queries using a fluent JavaScript interface without any knowledge of SQL.</span></span> <span data-ttu-id="71ea5-240">dotaz JavaScript Hello rozhraní API umožňuje tooprogrammatically sestavení dotazy předáním predikátem funkce do chainable funkce volá s built-ins pole a Oblíbené knihovny JavaScript jako lodash syntaxe známé tooECMAScript5 společnosti.</span><span class="sxs-lookup"><span data-stu-id="71ea5-240">hello JavaScript query API allows you tooprogrammatically build queries by passing predicate functions into chainable function calls, with a syntax familiar tooECMAScript5's Array built-ins and popular JavaScript libraries like lodash.</span></span> <span data-ttu-id="71ea5-241">Dotazy jsou analyzovat pomocí hello JavaScript runtime toobe spouštěných efektivně pomocí Azure Cosmos DB indexy.</span><span class="sxs-lookup"><span data-stu-id="71ea5-241">Queries are parsed by hello JavaScript runtime toobe executed efficiently using Azure Cosmos DB’s indices.</span></span>

> [!NOTE]
> <span data-ttu-id="71ea5-242">`__`(dvojité podtržítko) je příliš alias`getContext().getCollection()`.</span><span class="sxs-lookup"><span data-stu-id="71ea5-242">`__` (double-underscore) is an alias too`getContext().getCollection()`.</span></span>
> <br/>
> <span data-ttu-id="71ea5-243">Jinými slovy, můžete použít `__` nebo `getContext().getCollection()` tooaccess hello API dotazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71ea5-243">In other words, you can use `__` or `getContext().getCollection()` tooaccess hello JavaScript query API.</span></span>
> 
> 

<span data-ttu-id="71ea5-244">Podporované funkce patří:</span><span class="sxs-lookup"><span data-stu-id="71ea5-244">Supported functions include:</span></span>

<ul>
<li><span data-ttu-id="71ea5-245">
<b>chain().... hodnota ([zpětného volání] [, možnosti])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-245">
<b>chain() ... .value([callback] [, options])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-246">Začíná value() zřetězené volání, které musí být ukončena.</span><span class="sxs-lookup"><span data-stu-id="71ea5-246">Starts a chained call which must be terminated with value().</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-247">
<b>Filtr (predicateFunction [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-247">
<b>filter(predicateFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-248">Filtry hello vstup pomocí funkce predikátu, která vrátí hodnotu true nebo false v pořadí toofilter a konec vstupu dokumenty do hello výsledné sady.</span><span class="sxs-lookup"><span data-stu-id="71ea5-248">Filters hello input using a predicate function which returns true/false in order toofilter in/out input documents into hello resulting set.</span></span> <span data-ttu-id="71ea5-249">To se chová podobně jako tooa klauzule WHERE v příkazu SQL.</span><span class="sxs-lookup"><span data-stu-id="71ea5-249">This behaves similar tooa WHERE clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-250">
<b>mapy (transformationFunction [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-250">
<b>map(transformationFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-251">Platí projekci zadané funkce transformace, který mapuje každý objekt jazyka JavaScript tooa vstupní položka nebo hodnota.</span><span class="sxs-lookup"><span data-stu-id="71ea5-251">Applies a projection given a transformation function which maps each input item tooa JavaScript object or value.</span></span> <span data-ttu-id="71ea5-252">To se chová podobně jako klauzuli SELECT tooa v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="71ea5-252">This behaves similar tooa SELECT clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-253">
<b>pluck ([propertyName] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-253">
<b>pluck([propertyName] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-254">Toto je zástupce pro mapu, která extrahuje hello hodnotu jedinou vlastnost z každé vstupní položky.</span><span class="sxs-lookup"><span data-stu-id="71ea5-254">This is a shortcut for a map which extracts hello value of a single property from each input item.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-255">
<b>vyrovnání ([isShallow] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-255">
<b>flatten([isShallow] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-256">Kombinuje a vyrovná pole z každé vstupní položky v jednom poli tooa.</span><span class="sxs-lookup"><span data-stu-id="71ea5-256">Combines and flattens arrays from each input item in tooa single array.</span></span> <span data-ttu-id="71ea5-257">To se chová podobně jako tooSelectMany v technologii LINQ.</span><span class="sxs-lookup"><span data-stu-id="71ea5-257">This behaves similar tooSelectMany in LINQ.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-258">
<b>sortBy ([predikát] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-258">
<b>sortBy([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-259">Vytvořit novou sadu dokumentů podle řazení hello dokumenty v datovém proudu vstupní dokument hello ve vzestupném pořadí pomocí hello zadané predikátu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-259">Produce a new set of documents by sorting hello documents in hello input document stream in ascending order using hello given predicate.</span></span> <span data-ttu-id="71ea5-260">To se chová podobně jako tooa klauzule ORDER by v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="71ea5-260">This behaves similar tooa ORDER BY clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="71ea5-261">
<b>sortbydescending – ([predikát] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="71ea5-261">
<b>sortByDescending([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="71ea5-262">Vytvořit novou sadu dokumentů podle řazení hello dokumenty v datovém proudu vstupní dokument hello v sestupném pořadí pomocí hello zadané predikátu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-262">Produce a new set of documents by sorting hello documents in hello input document stream in descending order using hello given predicate.</span></span> <span data-ttu-id="71ea5-263">To se chová podobně jako klauzule ORDER BY x DESC tooa v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="71ea5-263">This behaves similar tooa ORDER BY x DESC clause in SQL.</span></span>
</li>
</ul>
</li>
</ul>


<span data-ttu-id="71ea5-264">Zahrnuté uvnitř predikátu nebo pro výběr funkcí, hello následující konstrukce JavaScript získat automaticky optimalizované toorun přímo v Azure Cosmos DB indexů:</span><span class="sxs-lookup"><span data-stu-id="71ea5-264">When included inside predicate and/or selector functions, hello following JavaScript constructs get automatically optimized toorun directly on Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="71ea5-265">Jednoduché operátory: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span><span class="sxs-lookup"><span data-stu-id="71ea5-265">Simple operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span></span> ~
* <span data-ttu-id="71ea5-266">Literály, včetně hello objekt literálu: {}</span><span class="sxs-lookup"><span data-stu-id="71ea5-266">Literals, including hello object literal: {}</span></span>
* <span data-ttu-id="71ea5-267">var, návratový</span><span class="sxs-lookup"><span data-stu-id="71ea5-267">var, return</span></span>

<span data-ttu-id="71ea5-268">Dobrý den, který vytvoří následující JavaScript získat není optimalizovaný pro Azure Cosmos DB indexy:</span><span class="sxs-lookup"><span data-stu-id="71ea5-268">hello following JavaScript constructs do not get optimized for Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="71ea5-269">Řízení toku (například v případě, pro, zatímco)</span><span class="sxs-lookup"><span data-stu-id="71ea5-269">Control flow (e.g. if, for, while)</span></span>
* <span data-ttu-id="71ea5-270">Volání funkcí</span><span class="sxs-lookup"><span data-stu-id="71ea5-270">Function calls</span></span>

<span data-ttu-id="71ea5-271">Další informace najdete v tématu naše [serverové JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="71ea5-271">For more information, please see our [Server-Side JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span></span>

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a><span data-ttu-id="71ea5-272">Příklad: Zápis uložené procedury pomocí rozhraní API dotazu hello JavaScript</span><span class="sxs-lookup"><span data-stu-id="71ea5-272">Example: Write a stored procedure using hello JavaScript query API</span></span>
<span data-ttu-id="71ea5-273">Hello následující ukázka kódu je příklad použití hello API dotazu jazyka JavaScript v kontextu hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="71ea5-273">hello following code sample is an example of how hello JavaScript Query API can be used in hello context of a stored procedure.</span></span> <span data-ttu-id="71ea5-274">Hello uložené procedury vloží dokumentu, poskytují vstupní parametr a aktualizuje dokumentu metadat, s použitím hello `__.filter()` metoda s minSize, maxSize a totalSize podle vlastnosti velikosti hello vstupní dokument.</span><span class="sxs-lookup"><span data-stu-id="71ea5-274">hello stored procedure inserts a document, given by an input parameter, and updates a metadata document, using hello `__.filter()` method, with minSize, maxSize, and totalSize based upon hello input document's size property.</span></span>

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent tooour callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check hello doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get hello meta document. We keep it in hello same collection. it's hello only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed toofind hello metadata document.");

            // hello metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all hello rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace hello metadata document in hello store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read hello meta again 
              //       and update again because due tooSnapshot isolation we will read same exact version (we are in same transaction).
              //       We have tootake care of that on hello client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-toojavascript-cheat-sheet"></a><span data-ttu-id="71ea5-275">SQL tooJavascript tahák</span><span class="sxs-lookup"><span data-stu-id="71ea5-275">SQL tooJavascript cheat sheet</span></span>
<span data-ttu-id="71ea5-276">Hello následující tabulka uvádí různé dotazy SQL a dotazy jazyka JavaScript odpovídající hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-276">hello following table presents various SQL queries and hello corresponding JavaScript queries.</span></span>

<span data-ttu-id="71ea5-277">Jako s dotazy SQL dokumentu vlastnosti klíče (například `doc.id`) malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="71ea5-277">As with SQL queries, document property keys (e.g. `doc.id`) are case-sensitive.</span></span>

|<span data-ttu-id="71ea5-278">SQL</span><span class="sxs-lookup"><span data-stu-id="71ea5-278">SQL</span></span>| <span data-ttu-id="71ea5-279">Rozhraní API dotazu jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="71ea5-279">JavaScript Query API</span></span>|<span data-ttu-id="71ea5-280">Popis dole</span><span class="sxs-lookup"><span data-stu-id="71ea5-280">Description below</span></span>|
|---|---|---|
|<span data-ttu-id="71ea5-281">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="71ea5-281">SELECT *</span></span><br><span data-ttu-id="71ea5-282">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-282">FROM docs</span></span>| <span data-ttu-id="71ea5-283">__.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-283">__.map(function(doc) {</span></span> <br><span data-ttu-id="71ea5-284">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc;</span><span class="sxs-lookup"><span data-stu-id="71ea5-284">&nbsp;&nbsp;&nbsp;&nbsp;return doc;</span></span><br><span data-ttu-id="71ea5-285">});</span><span class="sxs-lookup"><span data-stu-id="71ea5-285">});</span></span>|<span data-ttu-id="71ea5-286">1</span><span class="sxs-lookup"><span data-stu-id="71ea5-286">1</span></span>|
|<span data-ttu-id="71ea5-287">Vyberte docs.id, docs.message jako msg, docs.actions</span><span class="sxs-lookup"><span data-stu-id="71ea5-287">SELECT docs.id, docs.message AS msg, docs.actions</span></span> <br><span data-ttu-id="71ea5-288">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-288">FROM docs</span></span>|<span data-ttu-id="71ea5-289">__.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-289">__.map(function(doc) {</span></span><br><span data-ttu-id="71ea5-290">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {</span><span class="sxs-lookup"><span data-stu-id="71ea5-290">&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="71ea5-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="71ea5-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="71ea5-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span><span class="sxs-lookup"><span data-stu-id="71ea5-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span></span><br><span data-ttu-id="71ea5-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions</span><span class="sxs-lookup"><span data-stu-id="71ea5-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions</span></span><br><span data-ttu-id="71ea5-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="71ea5-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="71ea5-295">});</span><span class="sxs-lookup"><span data-stu-id="71ea5-295">});</span></span>|<span data-ttu-id="71ea5-296">2</span><span class="sxs-lookup"><span data-stu-id="71ea5-296">2</span></span>|
|<span data-ttu-id="71ea5-297">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="71ea5-297">SELECT *</span></span><br><span data-ttu-id="71ea5-298">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-298">FROM docs</span></span><br><span data-ttu-id="71ea5-299">KDE docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="71ea5-299">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="71ea5-300">__.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-300">__.filter(function(doc) {</span></span><br><span data-ttu-id="71ea5-301">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="71ea5-301">&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="71ea5-302">});</span><span class="sxs-lookup"><span data-stu-id="71ea5-302">});</span></span>|<span data-ttu-id="71ea5-303">3</span><span class="sxs-lookup"><span data-stu-id="71ea5-303">3</span></span>|
|<span data-ttu-id="71ea5-304">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="71ea5-304">SELECT *</span></span><br><span data-ttu-id="71ea5-305">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-305">FROM docs</span></span><br><span data-ttu-id="71ea5-306">KDE ARRAY_CONTAINS (dokumentace. Značky, 123)</span><span class="sxs-lookup"><span data-stu-id="71ea5-306">WHERE ARRAY_CONTAINS(docs.Tags, 123)</span></span>|<span data-ttu-id="71ea5-307">__.Filter(Function(x) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-307">__.filter(function(x) {</span></span><br><span data-ttu-id="71ea5-308">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí x.Tags & & x.Tags.indexOf(123) > -1;</span><span class="sxs-lookup"><span data-stu-id="71ea5-308">&nbsp;&nbsp;&nbsp;&nbsp;return x.Tags && x.Tags.indexOf(123) > -1;</span></span><br><span data-ttu-id="71ea5-309">});</span><span class="sxs-lookup"><span data-stu-id="71ea5-309">});</span></span>|<span data-ttu-id="71ea5-310">4</span><span class="sxs-lookup"><span data-stu-id="71ea5-310">4</span></span>|
|<span data-ttu-id="71ea5-311">Vyberte docs.id, docs.message jako msg</span><span class="sxs-lookup"><span data-stu-id="71ea5-311">SELECT docs.id, docs.message AS msg</span></span><br><span data-ttu-id="71ea5-312">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-312">FROM docs</span></span><br><span data-ttu-id="71ea5-313">KDE docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="71ea5-313">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="71ea5-314">__.chain()</span><span class="sxs-lookup"><span data-stu-id="71ea5-314">__.chain()</span></span><br><span data-ttu-id="71ea5-315">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-315">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="71ea5-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="71ea5-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="71ea5-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="71ea5-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="71ea5-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {</span></span><br><span data-ttu-id="71ea5-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {</span><span class="sxs-lookup"><span data-stu-id="71ea5-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="71ea5-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="71ea5-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="71ea5-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span><span class="sxs-lookup"><span data-stu-id="71ea5-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span></span><br><span data-ttu-id="71ea5-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="71ea5-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="71ea5-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="71ea5-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="71ea5-324">.Value();</span><span class="sxs-lookup"><span data-stu-id="71ea5-324">.value();</span></span>|<span data-ttu-id="71ea5-325">5</span><span class="sxs-lookup"><span data-stu-id="71ea5-325">5</span></span>|
|<span data-ttu-id="71ea5-326">SELECT VALUE – značka</span><span class="sxs-lookup"><span data-stu-id="71ea5-326">SELECT VALUE tag</span></span><br><span data-ttu-id="71ea5-327">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="71ea5-327">FROM docs</span></span><br><span data-ttu-id="71ea5-328">Připojte značky v dokumentaci. Značky</span><span class="sxs-lookup"><span data-stu-id="71ea5-328">JOIN tag IN docs.Tags</span></span><br><span data-ttu-id="71ea5-329">Docs._ts ORDER BY</span><span class="sxs-lookup"><span data-stu-id="71ea5-329">ORDER BY docs._ts</span></span>|<span data-ttu-id="71ea5-330">__.chain()</span><span class="sxs-lookup"><span data-stu-id="71ea5-330">__.chain()</span></span><br><span data-ttu-id="71ea5-331">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-331">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="71ea5-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí dokumentů. Značky & & Array.IsArray – (dokumentů. Značky);</span><span class="sxs-lookup"><span data-stu-id="71ea5-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.Tags && Array.isArray(doc.Tags);</span></span><br><span data-ttu-id="71ea5-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="71ea5-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="71ea5-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span><span class="sxs-lookup"><span data-stu-id="71ea5-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span></span><br><span data-ttu-id="71ea5-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc._ts;</span><span class="sxs-lookup"><span data-stu-id="71ea5-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc._ts;</span></span><br><span data-ttu-id="71ea5-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="71ea5-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="71ea5-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span><span class="sxs-lookup"><span data-stu-id="71ea5-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span></span><br><span data-ttu-id="71ea5-338">&nbsp;&nbsp;&nbsp;&nbsp;.Flatten()</span><span class="sxs-lookup"><span data-stu-id="71ea5-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span></span><br><span data-ttu-id="71ea5-339">&nbsp;&nbsp;&nbsp;&nbsp;.Value()</span><span class="sxs-lookup"><span data-stu-id="71ea5-339">&nbsp;&nbsp;&nbsp;&nbsp;.value()</span></span>|<span data-ttu-id="71ea5-340">6</span><span class="sxs-lookup"><span data-stu-id="71ea5-340">6</span></span>|

<span data-ttu-id="71ea5-341">Hello následujících popisech popisují každý dotaz ve výše uvedené tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-341">hello following descriptions explain each query in hello table above.</span></span>
1. <span data-ttu-id="71ea5-342">Výsledkem všechny dokumenty (čísla stránek vložena s token pokračování), jako je.</span><span class="sxs-lookup"><span data-stu-id="71ea5-342">Results in all documents (paginated with continuation token) as is.</span></span>
2. <span data-ttu-id="71ea5-343">Projekty hello id zprávy (alias toomsg) a akce z všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="71ea5-343">Projects hello id, message (aliased toomsg), and action from all documents.</span></span>
3. <span data-ttu-id="71ea5-344">Dotazy na dokumenty s predikát hello: id = "X998_Y998".</span><span class="sxs-lookup"><span data-stu-id="71ea5-344">Queries for documents with hello predicate: id = "X998_Y998".</span></span>
4. <span data-ttu-id="71ea5-345">Dotazy pro dokumenty, které mají značky a vlastnost Tags je pole obsahující hello hodnotu 123.</span><span class="sxs-lookup"><span data-stu-id="71ea5-345">Queries for documents that have a Tags property and Tags is an array containing hello value 123.</span></span>
5. <span data-ttu-id="71ea5-346">Dotazy na dokumenty s predikátem, id = "X998_Y998" a pak id hello projekty a zpráv (alias toomsg).</span><span class="sxs-lookup"><span data-stu-id="71ea5-346">Queries for documents with a predicate, id = "X998_Y998", and then projects hello id and message (aliased toomsg).</span></span>
6. <span data-ttu-id="71ea5-347">Filtry pro dokumenty, které mají ve vlastnosti pole značky, a seřadí výsledné dokumenty hello vlastností hello _ts časové razítko systému a pak projekty + vyrovná hello pole značky.</span><span class="sxs-lookup"><span data-stu-id="71ea5-347">Filters for documents which have an array property, Tags, and sorts hello resulting documents by hello _ts timestamp system property, and then projects + flattens hello Tags array.</span></span>


## <a name="runtime-support"></a><span data-ttu-id="71ea5-348">Podpora modulu runtime</span><span class="sxs-lookup"><span data-stu-id="71ea5-348">Runtime support</span></span>
<span data-ttu-id="71ea5-349">[Rozhraní API jazyka DocumentDB JavaScript serveru straně](http://azure.github.io/azure-documentdb-js-server/) poskytuje podporu pro hello většinu hello běžný funkce jazyka JavaScript jako standardizované podle [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span><span class="sxs-lookup"><span data-stu-id="71ea5-349">[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) provides support for hello most of hello mainstream JavaScript language features as standardized by [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span></span>

### <a name="security"></a><span data-ttu-id="71ea5-350">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="71ea5-350">Security</span></span>
<span data-ttu-id="71ea5-351">JavaScript uložené procedury a triggery jsou v izolovaném prostoru tak, aby hello důsledky jeden skript není úniku toohello jiných bez průchodu přes transakci izolace snímku hello na úrovni databáze hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-351">JavaScript stored procedures and triggers are sandboxed so that hello effects of one script do not leak toohello other without going through hello snapshot transaction isolation at hello database level.</span></span> <span data-ttu-id="71ea5-352">Hello prostředí runtime jsou ve fondu, ale čištění hello kontextu po každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="71ea5-352">hello runtime environments are pooled but cleaned of hello context after each run.</span></span> <span data-ttu-id="71ea5-353">Proto jsou zaručit toobe bezpečné z jakékoli nezamýšleným vedlejší účinky od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="71ea5-353">Hence they are guaranteed toobe safe of any unintended side effects from each other.</span></span>

### <a name="pre-compilation"></a><span data-ttu-id="71ea5-354">Předkompilace</span><span class="sxs-lookup"><span data-stu-id="71ea5-354">Pre-compilation</span></span>
<span data-ttu-id="71ea5-355">Uložené procedury, triggery a UDF jsou implicitně předkompilovaných toohello bajtovém formátu kódu v pořadí tooavoid kompilace náklady v době hello každé vyvolání skriptu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-355">Stored procedures, triggers and UDFs are implicitly precompiled toohello byte code format in order tooavoid compilation cost at hello time of each script invocation.</span></span> <span data-ttu-id="71ea5-356">To zajišťuje volání uložené procedury jsou rychlé a nízkým nárokům mít.</span><span class="sxs-lookup"><span data-stu-id="71ea5-356">This ensures invocations of stored procedures are fast and have a low footprint.</span></span>

## <a name="client-sdk-support"></a><span data-ttu-id="71ea5-357">Podpora klienta SDK</span><span class="sxs-lookup"><span data-stu-id="71ea5-357">Client SDK support</span></span>
<span data-ttu-id="71ea5-358">V toohello přidání DocumentDB rozhraní API pro [Node.js](documentdb-sdk-node.md) má klient Azure Cosmos DB [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), a [Python SDK](documentdb-sdk-python.md) pro hello DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="71ea5-358">In addition toohello DocumentDB API for [Node.js](documentdb-sdk-node.md) client, Azure Cosmos DB has [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/), and [Python SDKs](documentdb-sdk-python.md) for hello DocumentDB API.</span></span> <span data-ttu-id="71ea5-359">Uložené procedury, triggery a UDF lze vytvořit a spustit některé z těchto sad SDK také používá.</span><span class="sxs-lookup"><span data-stu-id="71ea5-359">Stored procedures, triggers and UDFs can be created and executed using any of these SDKs as well.</span></span> <span data-ttu-id="71ea5-360">Následující příklad ukazuje, jak Hello toocreate a provedení uložené procedury pomocí klienta rozhraní .NET hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-360">hello following example shows how toocreate and execute a stored procedure using hello .NET client.</span></span> <span data-ttu-id="71ea5-361">Všimněte si, jak jsou typy .NET hello předaný do hello uložený postup jako JSON a čtení zpět.</span><span class="sxs-lookup"><span data-stu-id="71ea5-361">Note how hello .NET types are passed into hello stored procedure as JSON and read back.</span></span>

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    

                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }

                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };

    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;

    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


<span data-ttu-id="71ea5-362">Tento příklad ukazuje, jak toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate před aktivační události a vytvořit dokument s hello aktivační události povolen.</span><span class="sxs-lookup"><span data-stu-id="71ea5-362">This sample shows how toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate a pre-trigger and create a document with hello trigger enabled.</span></span> 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };

    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


<span data-ttu-id="71ea5-363">A hello následující příklad ukazuje, jak toocreate uživatele definované funkce (UDF) a použít ho v [dotazu DocumentDB API SQL](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="71ea5-363">And hello following example shows how toocreate a user defined function (UDF) and use it in a [DocumentDB API SQL query](documentdb-sql-query.md).</span></span>

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };

    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a><span data-ttu-id="71ea5-364">REST API</span><span class="sxs-lookup"><span data-stu-id="71ea5-364">REST API</span></span>
<span data-ttu-id="71ea5-365">Všechny operace Azure Cosmos databáze lze provést RESTful způsobem.</span><span class="sxs-lookup"><span data-stu-id="71ea5-365">All Azure Cosmos DB operations can be performed in a RESTful manner.</span></span> <span data-ttu-id="71ea5-366">Uložené procedury, triggery a uživatelem definované funkce může být registrováno v rámci kolekce pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="71ea5-366">Stored procedures, triggers and user-defined functions can be registered under a collection by using HTTP POST.</span></span> <span data-ttu-id="71ea5-367">Hello tady je příklad toho, jak tooregister uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="71ea5-367">hello following is an example of how tooregister a stored procedure:</span></span>

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


<span data-ttu-id="71ea5-368">Hello uložené procedury je zaregistrován spuštěním požadavek POST hello URI databází nebo testdb/colls/testColl/sprocs s obsahující textu hello hello toocreate uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="71ea5-368">hello stored procedure is registered by executing a POST request against hello URI dbs/testdb/colls/testColl/sprocs with hello body containing hello stored procedure toocreate.</span></span> <span data-ttu-id="71ea5-369">Aktivační události a funkcí UDF lze registrovat podobně vydáním POST proti/aktivační události a /udfs v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="71ea5-369">Triggers and UDFs can be registered similarly by issuing a POST against /triggers and /udfs respectively.</span></span>
<span data-ttu-id="71ea5-370">Tato uložená procedura může poté provést po vydání požadavku POST s jeho odkazu prostředku:</span><span class="sxs-lookup"><span data-stu-id="71ea5-370">This stored procedure can then be executed by issuing a POST request against its resource link:</span></span>

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


<span data-ttu-id="71ea5-371">Zde je předán hello vstupní toohello uložené procedury v textu žádosti hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-371">Here, hello input toohello stored procedure is passed in hello request body.</span></span> <span data-ttu-id="71ea5-372">Všimněte si, že vstup hello se předá jako pole JSON vstupních parametrů.</span><span class="sxs-lookup"><span data-stu-id="71ea5-372">Note that hello input is passed as a JSON array of input parameters.</span></span> <span data-ttu-id="71ea5-373">Hello uložené procedury trvá hello první vstup jako dokument, který je text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="71ea5-373">hello stored procedure takes hello first input as a document that is a response body.</span></span> <span data-ttu-id="71ea5-374">Hello odpovědi, které obdržíme, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="71ea5-374">hello response we receive is as follows:</span></span>

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of hello Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


<span data-ttu-id="71ea5-375">Na rozdíl od uložené procedury, aktivační události nelze spustit přímo.</span><span class="sxs-lookup"><span data-stu-id="71ea5-375">Triggers, unlike stored procedures, cannot be executed directly.</span></span> <span data-ttu-id="71ea5-376">Místo toho jsou spouštěny jako součást operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-376">Instead they are executed as part of an operation on a document.</span></span> <span data-ttu-id="71ea5-377">Toorun hello aktivační události lze zadat s žádostí pomocí hlaviček protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="71ea5-377">We can specify hello triggers toorun with a request using HTTP headers.</span></span> <span data-ttu-id="71ea5-378">Hello následuje požadavek toocreate dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71ea5-378">hello following is request toocreate a document.</span></span>

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “hello Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


<span data-ttu-id="71ea5-379">Hello předběžné aktivační událost toobe spustit s žádostí hello je zde zadaným v hlavičce x-ms-documentdb-pre-trigger-include hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-379">Here hello pre-trigger toobe run with hello request is specified in hello x-ms-documentdb-pre-trigger-include header.</span></span> <span data-ttu-id="71ea5-380">Žádné aktivační události po odpovídajícím způsobem, jsou uvedeny v záhlaví x-ms-documentdb-post-trigger-include hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-380">Correspondingly, any post-triggers are given in hello x-ms-documentdb-post-trigger-include header.</span></span> <span data-ttu-id="71ea5-381">Všimněte si, že oba před a po aktivační události lze zadat pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="71ea5-381">Note that both pre- and post-triggers can be specified for a given request.</span></span>

## <a name="sample-code"></a><span data-ttu-id="71ea5-382">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="71ea5-382">Sample code</span></span>
<span data-ttu-id="71ea5-383">Můžete najít další příklady kódu na straně serveru (včetně [hromadné odstranění](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), a [aktualizace](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) na našem [úložiště GitHub](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="71ea5-383">You can find more server-side code examples (including [bulk-delete](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), and [update](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) on our [GitHub repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span></span>

<span data-ttu-id="71ea5-384">Má vaše společnost Super uložené procedury tooshare?</span><span class="sxs-lookup"><span data-stu-id="71ea5-384">Want tooshare your awesome stored procedure?</span></span> <span data-ttu-id="71ea5-385">Nám prosím pošlete žádost o přijetí změn!</span><span class="sxs-lookup"><span data-stu-id="71ea5-385">Please, send us a pull-request!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="71ea5-386">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71ea5-386">Next steps</span></span>
<span data-ttu-id="71ea5-387">Až budete mít jeden nebo více uložené procedury, triggery a uživatelem definované funkce vytvoření, můžete je načíst a zobrazit v portálu Azure pomocí Průzkumníku dat hello.</span><span class="sxs-lookup"><span data-stu-id="71ea5-387">Once you have one or more stored procedures, triggers, and user-defined functions created, you can load them and view them in hello Azure portal using Data Explorer.</span></span>

<span data-ttu-id="71ea5-388">Můžete také zjistit hello následující odkazy a prostředky, které jsou užitečné v vaše informace o programování na straně serveru dB Azure Cosmos toolearn cesta:</span><span class="sxs-lookup"><span data-stu-id="71ea5-388">You may also find hello following references and resources useful in your path toolearn more about Azure Cosmos dB server-side programming:</span></span>

* [<span data-ttu-id="71ea5-389">Sady SDK služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="71ea5-389">Azure Cosmos DB SDKs</span></span>](documentdb-sdk-dotnet.md)
* [<span data-ttu-id="71ea5-390">DocumentDB Studio</span><span class="sxs-lookup"><span data-stu-id="71ea5-390">DocumentDB Studio</span></span>](https://github.com/mingaliu/DocumentDBStudio/releases)
* [<span data-ttu-id="71ea5-391">JSON</span><span class="sxs-lookup"><span data-stu-id="71ea5-391">JSON</span></span>](http://www.json.org/) 
* [<span data-ttu-id="71ea5-392">JavaScript ECMA-262</span><span class="sxs-lookup"><span data-stu-id="71ea5-392">JavaScript ECMA-262</span></span>](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [<span data-ttu-id="71ea5-393">Rozšiřitelnost zabezpečení a přenosné databáze</span><span class="sxs-lookup"><span data-stu-id="71ea5-393">Secure and Portable Database Extensibility</span></span>](http://dl.acm.org/citation.cfm?id=276339) 
* [<span data-ttu-id="71ea5-394">Služba zaměřené na konkrétní architektura databáze</span><span class="sxs-lookup"><span data-stu-id="71ea5-394">Service Oriented Database Architecture</span></span>](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [<span data-ttu-id="71ea5-395">Hostování v systému Microsoft SQL server hello modul Runtime rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="71ea5-395">Hosting hello .NET Runtime in Microsoft SQL server</span></span>](http://dl.acm.org/citation.cfm?id=1007669)

