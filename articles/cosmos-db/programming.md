---
title: "Programování v jazyce JavaScript na straně serveru pro databázi Azure Cosmos | Microsoft Docs"
description: "Naučte se používat Azure Cosmos DB zápis uložené procedury, triggery databáze a uživatelem definované funkce (UDF) v jazyce JavaScript. Získáte tipy programing databáze a další."
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
ms.openlocfilehash: 8cddc7a8c9aa677b9c93bee3a7e05c226cc1f655
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a><span data-ttu-id="2086b-105">Azure programování na straně serveru Cosmos DB: uložené procedury, triggery databáze a UDF</span><span class="sxs-lookup"><span data-stu-id="2086b-105">Azure Cosmos DB server-side programming: Stored procedures, database triggers, and UDFs</span></span>
<span data-ttu-id="2086b-106">Zjistěte, jak integrovat Azure Cosmos DB jazyka, spouštění transakcí jazyka JavaScript umožňuje vývojářům zápisu **uložené procedury**, **aktivační události** a **funkce (UDF)definovanéuživatelem** nativně v [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-106">Learn how Azure Cosmos DB’s language integrated, transactional execution of JavaScript lets developers write **stored procedures**, **triggers** and **user defined functions (UDFs)** natively in an [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span></span> <span data-ttu-id="2086b-107">To umožňuje psát logiku aplikace program databáze, který může být dodána a provedeny přímo na databázi oddílů pro úložiště.</span><span class="sxs-lookup"><span data-stu-id="2086b-107">This allows you to write database program application logic that can be shipped and executed directly on the database storage partitions.</span></span> 

<span data-ttu-id="2086b-108">Doporučujeme začít následujícím videem, kde Andrew Liu poskytuje stručný úvod do Cosmos DB serverové databáze programovací model.</span><span class="sxs-lookup"><span data-stu-id="2086b-108">We recommend getting started by watching the following video, where Andrew Liu provides a brief introduction to Cosmos DB's server-side database programming model.</span></span> 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

<span data-ttu-id="2086b-109">Pak se vraťte k tomuto článku, kde se dozvíte odpovědi na tyto otázky:</span><span class="sxs-lookup"><span data-stu-id="2086b-109">Then, return to this article, where you'll learn the answers to the following questions:</span></span>  

* <span data-ttu-id="2086b-110">Jak psát uložené procedury, aktivační události nebo pomocí jazyka JavaScript UDF?</span><span class="sxs-lookup"><span data-stu-id="2086b-110">How do I write a a stored procedure, trigger, or UDF using JavaScript?</span></span>
* <span data-ttu-id="2086b-111">Jak Cosmos DB zaručit kyseliny?</span><span class="sxs-lookup"><span data-stu-id="2086b-111">How does Cosmos DB guarantee ACID?</span></span>
* <span data-ttu-id="2086b-112">Jak fungují transakce v databázi Cosmos?</span><span class="sxs-lookup"><span data-stu-id="2086b-112">How do transactions work in Cosmos DB?</span></span>
* <span data-ttu-id="2086b-113">Co se aktivuje před a po aktivuje a jak jeden zápis?</span><span class="sxs-lookup"><span data-stu-id="2086b-113">What are pre-triggers and post-triggers and how do I write one?</span></span>
* <span data-ttu-id="2086b-114">Jak registrovat a spustit uloženou proceduru, aktivační události nebo UDF RESTful způsobem pomocí protokolu HTTP?</span><span class="sxs-lookup"><span data-stu-id="2086b-114">How do I register and execute a stored procedure, trigger, or UDF in a RESTful manner by using HTTP?</span></span>
* <span data-ttu-id="2086b-115">Jaké Cosmos DB sady SDK jsou k dispozici pro vytvoření a provedení uložené procedury, triggery a UDF?</span><span class="sxs-lookup"><span data-stu-id="2086b-115">What Cosmos DB SDKs are available to create and execute stored procedures, triggers, and UDFs?</span></span>

## <a name="introduction-to-stored-procedure-and-udf-programming"></a><span data-ttu-id="2086b-116">Úvod do uložené procedury a UDF programování</span><span class="sxs-lookup"><span data-stu-id="2086b-116">Introduction to Stored Procedure and UDF Programming</span></span>
<span data-ttu-id="2086b-117">Tento přístup z *"JavaScript jako moderní den T-SQL"* uvolní vývojáři aplikací z složitosti neshody typu systému a relační objekt mapování technologie.</span><span class="sxs-lookup"><span data-stu-id="2086b-117">This approach of *“JavaScript as a modern day T-SQL”* frees application developers from the complexities of type system mismatches and object-relational mapping technologies.</span></span> <span data-ttu-id="2086b-118">Má také počet vnitřní výhody, které můžete použít k vytvoření bohaté aplikace:</span><span class="sxs-lookup"><span data-stu-id="2086b-118">It also has a number of intrinsic advantages that can be utilized to build rich applications:</span></span>  

* <span data-ttu-id="2086b-119">**Procedurální logika:** JavaScript jako nejvyšší úrovni programovací jazyk, poskytuje bohatou a známá rozhraní pro express obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="2086b-119">**Procedural Logic:** JavaScript as a high level programming language, provides a rich and familiar interface to express business logic.</span></span> <span data-ttu-id="2086b-120">Můžete provádět komplexní pořadí operací blíže k datům.</span><span class="sxs-lookup"><span data-stu-id="2086b-120">You can perform complex sequences of operations closer to the data.</span></span>
* <span data-ttu-id="2086b-121">**Jednotlivé transakce:** Cosmos DB záruky, které databáze operací provést v jedné uložené procedury nebo aktivační události jsou atomic.</span><span class="sxs-lookup"><span data-stu-id="2086b-121">**Atomic Transactions:** Cosmos DB guarantees that database operations performed inside a single stored procedure or trigger are atomic.</span></span> <span data-ttu-id="2086b-122">To umožní aplikaci kombinovat související operace v jedné dávkové tak, aby všechny úspěšné nebo žádná z nich úspěšné.</span><span class="sxs-lookup"><span data-stu-id="2086b-122">This lets an application combine related operations in a single batch so that either all of them succeed or none of them succeed.</span></span> 
* <span data-ttu-id="2086b-123">**Výkon:** skutečnost, že JSON je vnitřně namapovaný na systém typů jazyka Javascript a také je základní jednotkou úložiště v systému Cosmos DB umožňuje počet optimalizace jako opožděné materialization dokumentů JSON ve fondu vyrovnávací paměti a přitom k dispozici na vyžádání provádění kódu.</span><span class="sxs-lookup"><span data-stu-id="2086b-123">**Performance:** The fact that JSON is intrinsically mapped to the Javascript language type system and is also the basic unit of storage in Cosmos DB allows for a number of optimizations like lazy materialization of JSON documents in the buffer pool and making them available on-demand to the executing code.</span></span> <span data-ttu-id="2086b-124">Existují další výhody výkonu související s přenosů obchodní logiky k databázi:</span><span class="sxs-lookup"><span data-stu-id="2086b-124">There are more performance benefits associated with shipping business logic to the database:</span></span>
  
  * <span data-ttu-id="2086b-125">Dávkování – vývojáři můžou skupiny operací, jako je vložení a odesílat je hromadně.</span><span class="sxs-lookup"><span data-stu-id="2086b-125">Batching – Developers can group operations like inserts and submit them in bulk.</span></span> <span data-ttu-id="2086b-126">Latence provoz sítě, náklady a nároky na úložiště k vytvoření samostatné transakce jsou výrazně snížit.</span><span class="sxs-lookup"><span data-stu-id="2086b-126">The network traffic latency cost and the store overhead to create separate transactions are reduced significantly.</span></span> 
  * <span data-ttu-id="2086b-127">Předběžné kompilace – Cosmos DB překompiluje uložené procedury, triggery a uživatelem definované funkce (UDF), aby se zabránilo náklady kompilace jazyka JavaScript pro každé vyvolání.</span><span class="sxs-lookup"><span data-stu-id="2086b-127">Pre-compilation – Cosmos DB precompiles stored procedures, triggers and user defined functions (UDFs) to avoid JavaScript compilation cost for each invocation.</span></span> <span data-ttu-id="2086b-128">Nároky na vytváření kód bajtů pro Procedurální logika je amortizovaný na minimální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2086b-128">The overhead of building the byte code for the procedural logic is amortized to a minimal value.</span></span>
  * <span data-ttu-id="2086b-129">Sekvencování – mnoho operations nutné vedlejším účinkem (dále jen "aktivační události"), potenciálně zahrnuje jednu nebo více provádění operací sekundárního úložiště.</span><span class="sxs-lookup"><span data-stu-id="2086b-129">Sequencing – Many operations need a side-effect (“trigger”) that potentially involves doing one or many secondary store operations.</span></span> <span data-ttu-id="2086b-130">Kromě zajištění dostatečného nedělitelnost to je další původce při přesunu na server.</span><span class="sxs-lookup"><span data-stu-id="2086b-130">Aside from atomicity, this is more performant when moved to the server.</span></span> 
* <span data-ttu-id="2086b-131">**Zapouzdření:** uložené procedury lze použít k seskupení obchodní logiky v jednom místě.</span><span class="sxs-lookup"><span data-stu-id="2086b-131">**Encapsulation:** Stored procedures can be used to group business logic in one place.</span></span> <span data-ttu-id="2086b-132">To má dvě výhody:</span><span class="sxs-lookup"><span data-stu-id="2086b-132">This has two advantages:</span></span>
  * <span data-ttu-id="2086b-133">Přidá abstraktní vrstvu nad nezpracovaná data, která umožňuje data architekty vyvíjí svých aplikací nezávisle data.</span><span class="sxs-lookup"><span data-stu-id="2086b-133">It adds an abstraction layer on top of the raw data, which enables data architects to evolve their applications independently from the data.</span></span> <span data-ttu-id="2086b-134">To je zvlášť výhodné, pokud data bez schémat, z důvodu křehká předpokladů, které možná muset zaručená do aplikace, pokud mají jak nakládat s daty přímo.</span><span class="sxs-lookup"><span data-stu-id="2086b-134">This is particularly advantageous when the data is schema-less, due to the brittle assumptions that may need to be baked into the application if they have to deal with data directly.</span></span>  
  * <span data-ttu-id="2086b-135">Tato abstrakce umožňuje podnikům zabezpečit svá data pomocí zjednodušení přístup z skripty.</span><span class="sxs-lookup"><span data-stu-id="2086b-135">This abstraction lets enterprises keep their data secure by streamlining the access from the scripts.</span></span>  

<span data-ttu-id="2086b-136">Vytvoření a spuštění databáze aktivační události, uložené procedury a operátory vlastního dotazu jsou podporovány prostřednictvím [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), a [klientskou sadu SDK](documentdb-sdk-dotnet.md)na spoustě platforem včetně .NET, Node.js a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-136">The creation and execution of database triggers, stored procedure and custom query operators is supported through the [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), and [client SDKs](documentdb-sdk-dotnet.md) in many platforms including .NET, Node.js and JavaScript.</span></span>

<span data-ttu-id="2086b-137">Tento kurz používá [Node.js SDK Q lišící](http://azure.github.io/azure-documentdb-node-q/) pro ilustraci syntaxi a použití uložené procedury, triggery a UDF.</span><span class="sxs-lookup"><span data-stu-id="2086b-137">This tutorial uses the [Node.js SDK with Q Promises](http://azure.github.io/azure-documentdb-node-q/) to illustrate syntax and usage of stored procedures, triggers, and UDFs.</span></span>   

## <a name="stored-procedures"></a><span data-ttu-id="2086b-138">Uložené procedury</span><span class="sxs-lookup"><span data-stu-id="2086b-138">Stored procedures</span></span>
### <a name="example-write-a-simple-stored-procedure"></a><span data-ttu-id="2086b-139">Příklad: Zápis jednoduché uložené procedury</span><span class="sxs-lookup"><span data-stu-id="2086b-139">Example: Write a simple stored procedure</span></span>
<span data-ttu-id="2086b-140">Začněme jednoduché uložené procedury, jež vrátí odpověď "Hello World".</span><span class="sxs-lookup"><span data-stu-id="2086b-140">Let’s start with a simple stored procedure that returns a “Hello World” response.</span></span>

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


<span data-ttu-id="2086b-141">Uložené procedury jsou registrované na kolekci a mohou pracovat na všechny dokumentů a příloh, které se nachází v dané kolekci.</span><span class="sxs-lookup"><span data-stu-id="2086b-141">Stored procedures are registered per collection, and can operate on any document and attachment present in that collection.</span></span> <span data-ttu-id="2086b-142">Následující fragment kódu ukazuje, jak zaregistrovat helloWorld uložené procedury s kolekcí.</span><span class="sxs-lookup"><span data-stu-id="2086b-142">The following snippet shows how to register the helloWorld stored procedure with a collection.</span></span> 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


<span data-ttu-id="2086b-143">Po registraci uloženou proceduru můžeme provést proti kolekci a přečtěte si výsledky zpět na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="2086b-143">Once the stored procedure is registered, we can execute it against the collection, and read the results back at the client.</span></span> 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


<span data-ttu-id="2086b-144">Objekt context poskytuje přístup ke všem operacím, které lze provést na úložiště Cosmos databáze, a také přístup k objektům požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2086b-144">The context object provides access to all operations that can be performed on Cosmos DB storage, as well as access to the request and response objects.</span></span> <span data-ttu-id="2086b-145">V takovém případě jsme použili objektu odpovědi nastavit text odpovědi, která byla odeslána zpět klientovi.</span><span class="sxs-lookup"><span data-stu-id="2086b-145">In this case, we used the response object to set the body of the response that was sent back to the client.</span></span> <span data-ttu-id="2086b-146">Další podrobnosti najdete [server Azure Cosmos DB JavaScript dokumentaci k sadě SDK](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="2086b-146">For more details, refer to the [Azure Cosmos DB JavaScript server SDK documentation](http://azure.github.io/azure-documentdb-js-server/).</span></span>  

<span data-ttu-id="2086b-147">Dejte nám rozbalte v tomto příkladu, přidejte že další databáze související funkce uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2086b-147">Let us expand on this example and add more database related functionality to the stored procedure.</span></span> <span data-ttu-id="2086b-148">Uložené procedury můžete vytvářet, aktualizovat, číst, dotaz a odstraňovat dokumenty a přílohy v kolekci.</span><span class="sxs-lookup"><span data-stu-id="2086b-148">Stored procedures can create, update, read, query and delete documents and attachments inside the collection.</span></span>    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a><span data-ttu-id="2086b-149">Příklad: Zápis uložené procedury k vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="2086b-149">Example: Write a stored procedure to create a document</span></span>
<span data-ttu-id="2086b-150">Další fragment kódu ukazuje, jak pomocí objektu context můžete pracovat s prostředky Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2086b-150">The next snippet shows how to use the context object to interact with Cosmos DB resources.</span></span>

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


<span data-ttu-id="2086b-151">Tuto uloženou proceduru vezme jako vstupní documentToCreate textu dokumentu má být vytvořen v aktuální kolekci.</span><span class="sxs-lookup"><span data-stu-id="2086b-151">This stored procedure takes as input documentToCreate, the body of a document to be created in the current collection.</span></span> <span data-ttu-id="2086b-152">Všechny tyto operace jsou asynchronní a závisí na zpětná volání funkce jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-152">All such operations are asynchronous and depend on JavaScript function callbacks.</span></span> <span data-ttu-id="2086b-153">Funkce zpětného volání má dva parametry, jeden pro objekt chyby v případě, kdy se operace nezdaří a jeden pro vytvořený objekt.</span><span class="sxs-lookup"><span data-stu-id="2086b-153">The callback function has two parameters, one for the error object in case the operation fails, and one for the created object.</span></span> <span data-ttu-id="2086b-154">Uvnitř zpětné volání uživatelé mohou zpracovat výjimku nebo vyvolána chyba.</span><span class="sxs-lookup"><span data-stu-id="2086b-154">Inside the callback, users can either handle the exception or throw an error.</span></span> <span data-ttu-id="2086b-155">V případě, že není k dispozici zpětné volání a dojde k chybě, modul runtime Azure Cosmos DB vyvolá chybu.</span><span class="sxs-lookup"><span data-stu-id="2086b-155">In case a callback is not provided and there is an error, the Azure Cosmos DB runtime throws an error.</span></span>   

<span data-ttu-id="2086b-156">V předchozím příkladu zpětné volání vrátí chybu, pokud operace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="2086b-156">In the example above, the callback throws an error if the operation failed.</span></span> <span data-ttu-id="2086b-157">Jinak nastaví id vytvořený dokumentu jako text odpovědi klientovi.</span><span class="sxs-lookup"><span data-stu-id="2086b-157">Otherwise, it sets the id of the created document as the body of the response to the client.</span></span> <span data-ttu-id="2086b-158">Zde je, jak se spustí tuto uloženou proceduru s vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="2086b-158">Here is how this stored procedure is executed with input parameters.</span></span>

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
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


<span data-ttu-id="2086b-159">Všimněte si, že tuto uloženou proceduru lze upravit tak, aby pole těla dokumentu jako vstup a k jejich vytvoření všechno ve stejném spuštění uložené procedury místo více síťové požadavky pro každý z nich vytvoření jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="2086b-159">Note that this stored procedure can be modified to take an array of document bodies as input and create them all in the same stored procedure execution instead of multiple network requests to create each of them individually.</span></span> <span data-ttu-id="2086b-160">To lze použít k implementaci efektivní hromadné – Importér pro DB Cosmos (popsané později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="2086b-160">This can be used to implement an efficient bulk importer for Cosmos DB (discussed later in this tutorial).</span></span>   

<span data-ttu-id="2086b-161">Popisuje příklad ukázal, jak lze pomocí uložených procedur.</span><span class="sxs-lookup"><span data-stu-id="2086b-161">The example described demonstrated how to use stored procedures.</span></span> <span data-ttu-id="2086b-162">Později v tomto kurzu obsahuje triggery a uživatelem definované funkce (UDF).</span><span class="sxs-lookup"><span data-stu-id="2086b-162">We will cover triggers and user defined functions (UDFs) later in the tutorial.</span></span>

## <a name="database-program-transactions"></a><span data-ttu-id="2086b-163">Databáze programu transakce</span><span class="sxs-lookup"><span data-stu-id="2086b-163">Database program transactions</span></span>
<span data-ttu-id="2086b-164">Transakce v typické databáze může být definováno jako posloupnost operací provést jako jednu logickou jednotku práce.</span><span class="sxs-lookup"><span data-stu-id="2086b-164">Transaction in a typical database can be defined as a sequence of operations performed as a single logical unit of work.</span></span> <span data-ttu-id="2086b-165">Poskytuje každou transakci **ACID záruky**.</span><span class="sxs-lookup"><span data-stu-id="2086b-165">Each transaction provides **ACID guarantees**.</span></span> <span data-ttu-id="2086b-166">Je kyselina dobře známé zkratku, který zastupuje čtyři vlastnosti - nedělitelnost, konzistence, izolace a odolnost.</span><span class="sxs-lookup"><span data-stu-id="2086b-166">ACID is a well-known acronym that stands for four properties -  Atomicity, Consistency, Isolation and Durability.</span></span>  

<span data-ttu-id="2086b-167">Stručně řečeno, nedělitelnost zaručuje, že všechny práci v transakci je považován za jednu jednotku kde buď všechny klade nebo hodnotu none.</span><span class="sxs-lookup"><span data-stu-id="2086b-167">Briefly, atomicity guarantees that all the work done inside a transaction is treated as a single unit where either all of it is committed or none.</span></span> <span data-ttu-id="2086b-168">Konzistence zajišťuje, že data jsou vždycky ve funkčním stavu interní napříč transakce.</span><span class="sxs-lookup"><span data-stu-id="2086b-168">Consistency makes sure that the data is always in a good internal state across transactions.</span></span> <span data-ttu-id="2086b-169">Izolace zaručuje, že žádné dvě transakce konfliktu s jinými – obecně, většina komerčních systémy poskytují více úrovní izolace, které lze použít podle potřeb aplikace.</span><span class="sxs-lookup"><span data-stu-id="2086b-169">Isolation guarantees that no two transactions interfere with each other – generally, most commercial systems provide multiple isolation levels that can be used based on the application needs.</span></span> <span data-ttu-id="2086b-170">Odolnost zajistí, že všechny změny, která je potvrzena v databázi vždy bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2086b-170">Durability ensures that any change that’s committed in the database will always be present.</span></span>   

<span data-ttu-id="2086b-171">V Cosmos databáze JavaScript je hostován ve stejném prostoru paměti jako databáze.</span><span class="sxs-lookup"><span data-stu-id="2086b-171">In Cosmos DB, JavaScript is hosted in the same memory space as the database.</span></span> <span data-ttu-id="2086b-172">Proto požadavky provedené v rámci uložené procedury a triggery provést ve stejném oboru relace databáze.</span><span class="sxs-lookup"><span data-stu-id="2086b-172">Hence, requests made within stored procedures and triggers execute in the same scope of a database session.</span></span> <span data-ttu-id="2086b-173">To umožňuje Cosmos DB zaručit kyseliny pro všechny operace, které jsou součástí jedné uložené procedury nebo aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="2086b-173">This enables Cosmos DB to guarantee ACID for all operations that are part of a single stored procedure/trigger.</span></span> <span data-ttu-id="2086b-174">Vezměte v úvahu následující uložené procedury definice:</span><span class="sxs-lookup"><span data-stu-id="2086b-174">Consider the following stored procedure definition:</span></span>

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

                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });

            if (!accept) throw "Unable to read player details, abort ";

            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });

                        if (!accept2) throw "Unable to update player 2, abort";
                    });

                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }

    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

<span data-ttu-id="2086b-175">Tuto uloženou proceduru používá transakce v rámci aplikace herní položkám obchodu mezi dvěma přehrávače v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="2086b-175">This stored procedure uses transactions within a gaming app to trade items between two players in a single operation.</span></span> <span data-ttu-id="2086b-176">Uložená procedura se pokusí přečíst dva dokumenty, které každý odpovídající ID player v předat jako argument.</span><span class="sxs-lookup"><span data-stu-id="2086b-176">The stored procedure attempts to read two documents each corresponding to the player IDs passed in as an argument.</span></span> <span data-ttu-id="2086b-177">Pokud jsou oba player dokumenty, uložené procedury aktualizuje dokumenty pomocí odkládací jejich položky.</span><span class="sxs-lookup"><span data-stu-id="2086b-177">If both player documents are found, then the stored procedure updates the documents by swapping their items.</span></span> <span data-ttu-id="2086b-178">Pokud na cestě došlo k chybám, vyvolá výjimku JavaScript, která implicitně zruší transakce.</span><span class="sxs-lookup"><span data-stu-id="2086b-178">If any errors are encountered along the way, it throws a JavaScript exception that implicitly aborts the transaction.</span></span>

<span data-ttu-id="2086b-179">Pokud je registrován kolekce uložené procedury proti je jedním oddílem kolekce a potom transakce je omezená na všechny dokumenty v rámci kolekce.</span><span class="sxs-lookup"><span data-stu-id="2086b-179">If the collection the stored procedure is registered against is a single-partition collection, then the transaction is scoped to all the documents within the collection.</span></span> <span data-ttu-id="2086b-180">Pokud je kolekce rozdělena na oddíly, uložené procedury jsou spouštěny v oboru transakce klíče jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="2086b-180">If the collection is partitioned, then stored procedures are executed in the transaction scope of a single partition key.</span></span> <span data-ttu-id="2086b-181">Každý uložené provádění procedury pak musí obsahovat hodnotu klíče oddílu odpovídající transakce musí běžet v rámci oboru.</span><span class="sxs-lookup"><span data-stu-id="2086b-181">Each stored procedure execution must then include a partition key value corresponding to the scope the transaction must run under.</span></span> <span data-ttu-id="2086b-182">Další podrobnosti najdete v tématu [Azure Cosmos DB dělení](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="2086b-182">For more details, see [Azure Cosmos DB Partitioning](partition-data.md).</span></span>

### <a name="commit-and-rollback"></a><span data-ttu-id="2086b-183">Potvrzení a vrácení zpět</span><span class="sxs-lookup"><span data-stu-id="2086b-183">Commit and rollback</span></span>
<span data-ttu-id="2086b-184">Transakce je úzce a nativně integrováno programovací model Cosmos DB jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-184">Transactions are deeply and natively integrated into Cosmos DB’s JavaScript programming model.</span></span> <span data-ttu-id="2086b-185">Uvnitř funkce jazyka JavaScript jsou automaticky zabalená všechny operace v rámci jedné transakce.</span><span class="sxs-lookup"><span data-stu-id="2086b-185">Inside a JavaScript function, all operations are automatically wrapped under a single transaction.</span></span> <span data-ttu-id="2086b-186">Pokud JavaScript se dokončí bez jakékoli výjimky, se potvrdí operací do databáze.</span><span class="sxs-lookup"><span data-stu-id="2086b-186">If the JavaScript completes without any exception, the operations to the database are committed.</span></span> <span data-ttu-id="2086b-187">V důsledku toho jsou implicitní v Cosmos DB "BEGIN TRANSACTION" a "Potvrzení transakcí" příkazy v relačních databází.</span><span class="sxs-lookup"><span data-stu-id="2086b-187">In effect, the “BEGIN TRANSACTION” and “COMMIT TRANSACTION” statements in relational databases are implicit in Cosmos DB.</span></span>  

<span data-ttu-id="2086b-188">Pokud žádné výjimka, která je rozšířena ze skriptu, bude Cosmos DB JavaScript runtime vrátit zpět celou transakci.</span><span class="sxs-lookup"><span data-stu-id="2086b-188">If there is any exception that’s propagated from the script, Cosmos DB’s JavaScript runtime will roll back the whole transaction.</span></span> <span data-ttu-id="2086b-189">Jak ukazuje předchozí příklad, došlo k výjimce je efektivně ekvivalentní "Transakce vrácení zpět" v Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2086b-189">As shown in the earlier example, throwing an exception is effectively equivalent to a “ROLLBACK TRANSACTION” in Cosmos DB.</span></span>

### <a name="data-consistency"></a><span data-ttu-id="2086b-190">Konzistence dat</span><span class="sxs-lookup"><span data-stu-id="2086b-190">Data consistency</span></span>
<span data-ttu-id="2086b-191">Uložené procedury a triggery jsou vždy provést u primární repliky kontejneru Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2086b-191">Stored procedures and triggers are always executed on the primary replica of the Azure Cosmos DB container.</span></span> <span data-ttu-id="2086b-192">Tím se zajistí, že čtení z uvnitř uložené procedury nabídka silnou konzistenci.</span><span class="sxs-lookup"><span data-stu-id="2086b-192">This ensures that reads from inside stored procedures offer strong consistency.</span></span> <span data-ttu-id="2086b-193">Dotazy pomocí uživatelem definované funkce mohou být provedeny u primární nebo sekundární repliku, ale je zajištěno pro splnění úrovně konzistence požadovaný výběrem příslušné repliky.</span><span class="sxs-lookup"><span data-stu-id="2086b-193">Queries using user defined functions can be executed on the primary or any secondary replica, but we ensure to meet the requested consistency level by choosing the appropriate replica.</span></span>

## <a name="bounded-execution"></a><span data-ttu-id="2086b-194">Ohraničené provádění</span><span class="sxs-lookup"><span data-stu-id="2086b-194">Bounded execution</span></span>
<span data-ttu-id="2086b-195">Všechny operace Cosmos DB musí dokončit v rámci serveru zadané trvání časového limitu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2086b-195">All Cosmos DB operations must complete within the server specified request timeout duration.</span></span> <span data-ttu-id="2086b-196">Toto omezení platí i pro funkce jazyka JavaScript (uložené procedury, triggery a uživatelem definované funkce).</span><span class="sxs-lookup"><span data-stu-id="2086b-196">This constraint also applies to JavaScript functions (stored procedures, triggers and user-defined functions).</span></span> <span data-ttu-id="2086b-197">Pokud se tento časový limit operace nebude dokončena, transakce je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="2086b-197">If an operation does not complete with that time limit, the transaction is rolled back.</span></span> <span data-ttu-id="2086b-198">Funkce jazyka JavaScript musí dokončit v časovém limitu nebo implementovat pokračování na základě modelu pro spuštění dávky nebo obnovení.</span><span class="sxs-lookup"><span data-stu-id="2086b-198">JavaScript functions must finish within the time limit or implement a continuation based model to batch/resume execution.</span></span>  

<span data-ttu-id="2086b-199">Aby bylo možné zjednodušit vývoj uložených procedur a aktivačních událostí ke zpracování časových limitů, všechny funkce v rámci kolekce objektu (vytvořit, číst, nahraďte a odstranění dokumentů a přílohy) vrátí logickou hodnotu hodnotu této představuje jestli tuto operaci dokončí.</span><span class="sxs-lookup"><span data-stu-id="2086b-199">In order to simplify development of stored procedures and triggers to handle time limits, all functions under the collection object (for create, read, replace, and delete of documents and attachments) return a Boolean value that represents whether that operation will complete.</span></span> <span data-ttu-id="2086b-200">Pokud je tato hodnota false, je to znamenat, že je časový limit vyprší a, musíte si provádění zabalit postupu.</span><span class="sxs-lookup"><span data-stu-id="2086b-200">If this value is false, it is an indication that the time limit is about to expire and that the procedure must wrap up execution.</span></span>  <span data-ttu-id="2086b-201">Operace zařazených do fronty před první operace nepřijatelného úložiště je zaručeno dokončení, pokud uložená procedura se dokončí za dobu a fronty nejsou žádné další žádosti.</span><span class="sxs-lookup"><span data-stu-id="2086b-201">Operations queued prior to the first unaccepted store operation are guaranteed to complete if the stored procedure completes in time and does not queue any more requests.</span></span>  

<span data-ttu-id="2086b-202">Funkce jazyka JavaScript jsou také vázaný na spotřeby prostředků.</span><span class="sxs-lookup"><span data-stu-id="2086b-202">JavaScript functions are also bounded on resource consumption.</span></span> <span data-ttu-id="2086b-203">Cosmos DB si vyhrazuje propustnosti na kolekci podle zřízené velikost databázového účtu.</span><span class="sxs-lookup"><span data-stu-id="2086b-203">Cosmos DB reserves throughput per collection based on the provisioned size of a database account.</span></span> <span data-ttu-id="2086b-204">Propustnost se vyjadřuje jako normalizované jednotka procesoru, paměti a jednotek žádosti nebo RUs spotřeba vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="2086b-204">Throughput is expressed in terms of a normalized unit of CPU, memory and IO consumption called request units or RUs.</span></span> <span data-ttu-id="2086b-205">Funkce jazyka JavaScript může potenciálně spotřebovávat velký počet RUs během krátké doby a může získat míra limited, pokud bude dosažen limit kolekce.</span><span class="sxs-lookup"><span data-stu-id="2086b-205">JavaScript functions can potentially use up a large number of RUs within a short time, and might get rate-limited if the collection’s limit is reached.</span></span> <span data-ttu-id="2086b-206">Prostředek náročné uložené procedury mohou také umístí do karantény k zajištění dostupnosti primitivní databázových operací.</span><span class="sxs-lookup"><span data-stu-id="2086b-206">Resource intensive stored procedures might also be quarantined to ensure availability of primitive database operations.</span></span>  

### <a name="example-bulk-importing-data-into-a-database-program"></a><span data-ttu-id="2086b-207">Příklad: Hromadný import dat do databáze programu</span><span class="sxs-lookup"><span data-stu-id="2086b-207">Example: Bulk importing data into a database program</span></span>
<span data-ttu-id="2086b-208">Dole je příklad uložené procedury, jež je zapsán do hromadného importu dokumenty do kolekce.</span><span class="sxs-lookup"><span data-stu-id="2086b-208">Below is an example of a stored procedure that is written to bulk-import documents into a collection.</span></span> <span data-ttu-id="2086b-209">Všimněte si, jak uloženou proceduru zpracovává ohraničené provádění kontrolou logickou hodnotu návratová hodnota z createDocument a pak používá k sledování a pokračovat v průběhu napříč dávek počet dokumentů vložen do každé volání uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2086b-209">Note how the stored procedure handles bounded execution by checking the Boolean return value from createDocument, and then uses the count of documents inserted in each invocation of the stored procedure to track and resume progress across batches.</span></span>

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // The count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call the create API to create a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment the count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <span data-ttu-id="2086b-210"><a id="trigger"></a>Aktivační události databáze</span><span class="sxs-lookup"><span data-stu-id="2086b-210"><a id="trigger"></a> Database triggers</span></span>
### <a name="database-pre-triggers"></a><span data-ttu-id="2086b-211">Databáze před aktivační události</span><span class="sxs-lookup"><span data-stu-id="2086b-211">Database pre-triggers</span></span>
<span data-ttu-id="2086b-212">Cosmos DB poskytuje triggery, které jsou provedeny nebo aktivovány operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-212">Cosmos DB provides triggers that are executed or triggered by an operation on a document.</span></span> <span data-ttu-id="2086b-213">Například můžete zadat před aktivační událost, když vytváříte dokumentu – Tato předběžná aktivační událost se spustí před vytvořením dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-213">For example, you can specify a pre-trigger when you are creating a document – this pre-trigger will run before the document is created.</span></span> <span data-ttu-id="2086b-214">Následuje příklad použití aktivační události předběžné ověření vlastností dokumentu, který je právě vytvářena:</span><span class="sxs-lookup"><span data-stu-id="2086b-214">The following is an example of how pre-triggers can be used to validate the properties of a document that is being created:</span></span>

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document to be created in the current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


<span data-ttu-id="2086b-215">A odpovídající kód Node.js registrace klienta pro aktivační událost:</span><span class="sxs-lookup"><span data-stu-id="2086b-215">And the corresponding Node.js client-side registration code for the trigger:</span></span>

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


<span data-ttu-id="2086b-216">Předběžné aktivační události nemůže mít žádné vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="2086b-216">Pre-triggers cannot have any input parameters.</span></span> <span data-ttu-id="2086b-217">Objekt požadavku můžete použít k manipulaci zprávu požadavku přidruženou operaci.</span><span class="sxs-lookup"><span data-stu-id="2086b-217">The request object can be used to manipulate the request message associated with the operation.</span></span> <span data-ttu-id="2086b-218">Zde předběžné aktivační události je spuštěn s vytvoření dokumentu a tělo zprávy požadavku obsahuje dokumentu, který má být vytvořen ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="2086b-218">Here, the pre-trigger is being run with the creation of a document, and the request message body contains the document to be created in JSON format.</span></span>   

<span data-ttu-id="2086b-219">Když jsou registrované aktivačních událostí, mohou uživatelé zadat operace, které můžete spustit s.</span><span class="sxs-lookup"><span data-stu-id="2086b-219">When triggers are registered, users can specify the operations that it can run with.</span></span> <span data-ttu-id="2086b-220">Této aktivační události byl vytvořen s TriggerOperation.Create, což znamená, že následující není povoleno.</span><span class="sxs-lookup"><span data-stu-id="2086b-220">This trigger was created with TriggerOperation.Create, which means the following is not permitted.</span></span>

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a><span data-ttu-id="2086b-221">Databáze po aktivační události</span><span class="sxs-lookup"><span data-stu-id="2086b-221">Database post-triggers</span></span>
<span data-ttu-id="2086b-222">Po aktivační události, například před aktivační události, jsou spojena s operací na dokument a nechcete provést žádné vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="2086b-222">Post-triggers, like pre-triggers, are associated with an operation on a document and don’t take any input parameters.</span></span> <span data-ttu-id="2086b-223">Mohou být spuštěny **po** operace byla dokončena a mít přístup k zprávu odpovědi, která je odeslána do klienta.</span><span class="sxs-lookup"><span data-stu-id="2086b-223">They run **after** the operation has completed, and have access to the response message that is sent to the client.</span></span>   

<span data-ttu-id="2086b-224">Následující příklad ukazuje po aktivační události v akci:</span><span class="sxs-lookup"><span data-stu-id="2086b-224">The following example shows post-triggers in action:</span></span>

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
            if(!accept) throw "Unable to update metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


<span data-ttu-id="2086b-225">Aktivační událost lze zaregistrovat, jak znázorňuje následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="2086b-225">The trigger can be registered as shown in the following sample.</span></span>

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
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


<span data-ttu-id="2086b-226">Této aktivační události dotazuje na dokument metadat a aktualizuje s podrobnostmi o nově vytvořený dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-226">This trigger queries for the metadata document and updates it with details about the newly created document.</span></span>  

<span data-ttu-id="2086b-227">Je jednou z věcí, je důležité si uvědomit **transakcí** provádění aktivační události do databáze. Cosmos.</span><span class="sxs-lookup"><span data-stu-id="2086b-227">One thing that is important to note is the **transactional** execution of triggers in Cosmos DB.</span></span> <span data-ttu-id="2086b-228">Této aktivační události po spuštění v rámci stejné transakci jako vytváření původního dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-228">This post-trigger runs as part of the same transaction as the creation of the original document.</span></span> <span data-ttu-id="2086b-229">Proto pokud jsme z po aktivační události (například pokud nelze aktualizovat dokument metadat) způsobí výjimku, celá transakce se nezdaří a vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="2086b-229">Therefore, if we throw an exception from the post-trigger (say if we are unable to update the metadata document), the whole transaction will fail and be rolled back.</span></span> <span data-ttu-id="2086b-230">Žádné dokumentu se vytvoří a bude vrácen výjimku.</span><span class="sxs-lookup"><span data-stu-id="2086b-230">No document will be created, and an exception will be returned.</span></span>  

## <span data-ttu-id="2086b-231"><a id="udf"></a>Uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="2086b-231"><a id="udf"></a>User-defined functions</span></span>
<span data-ttu-id="2086b-232">Uživatelem definované funkce (UDF) slouží k rozšíření gramatika jazyk dotazu DocumentDB SQL rozhraní API a implementovat vlastní obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="2086b-232">User-defined functions (UDFs) are used to extend the DocumentDB API SQL query language grammar and implement custom business logic.</span></span> <span data-ttu-id="2086b-233">Je možné volat jedině z uvnitř dotazy.</span><span class="sxs-lookup"><span data-stu-id="2086b-233">They can only be called from inside queries.</span></span> <span data-ttu-id="2086b-234">Tyto nemají přístup k objektu kontextu a jsou určené pro použití jako jen výpočetní JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-234">They do not have access to the context object and are meant to be used as compute-only JavaScript.</span></span> <span data-ttu-id="2086b-235">Proto UDF lze spustit na sekundárních replikách služby Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2086b-235">Therefore, UDFs can be run on secondary replicas of the Cosmos DB service.</span></span>  

<span data-ttu-id="2086b-236">Následující příklad vytvoří UDF k výpočtu daně z příjmu podle sazby za různé příjem závorky a používá je uvnitř dotazu najít všechny lidí, kteří v daně placené více než 20 000 $.</span><span class="sxs-lookup"><span data-stu-id="2086b-236">The following sample creates a UDF to calculate income tax based on rates for various income brackets, and then uses it inside a query to find all people who paid more than $20,000 in taxes.</span></span>

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


<span data-ttu-id="2086b-237">UDF lze následně použít v dotazech jako následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2086b-237">The UDF can subsequently be used in queries like in the following sample:</span></span>

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

## <a name="javascript-language-integrated-query-api"></a><span data-ttu-id="2086b-238">JavaScript language-integrated query rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2086b-238">JavaScript language-integrated query API</span></span>
<span data-ttu-id="2086b-239">Kromě vydávat dotazy pomocí DocumentDB SQL gramatika, sadu SDK na straně serveru umožňuje provádět optimalizované dotazy pomocí rozhraní fluent JavaScript bez znalostí SQL.</span><span class="sxs-lookup"><span data-stu-id="2086b-239">In addition to issuing queries using DocumentDB’s SQL grammar, the server-side SDK allows you to perform optimized queries using a fluent JavaScript interface without any knowledge of SQL.</span></span> <span data-ttu-id="2086b-240">Dotaz jazyka JavaScript, který rozhraní API umožňuje prostřednictvím kódu programu vytvořit dotazy předáním predikátem funkce do chainable funkce volá s pro pole built-ins a Oblíbené knihovny JavaScript jako lodash ECMAScript5 na syntaxi.</span><span class="sxs-lookup"><span data-stu-id="2086b-240">The JavaScript query API allows you to programmatically build queries by passing predicate functions into chainable function calls, with a syntax familiar to ECMAScript5's Array built-ins and popular JavaScript libraries like lodash.</span></span> <span data-ttu-id="2086b-241">Dotazy jsou analyzovat pomocí prostředí JavaScript runtime spouštění efektivně pomocí Azure Cosmos DB indexy.</span><span class="sxs-lookup"><span data-stu-id="2086b-241">Queries are parsed by the JavaScript runtime to be executed efficiently using Azure Cosmos DB’s indices.</span></span>

> [!NOTE]
> <span data-ttu-id="2086b-242">`__`(dvojité podtržítko) je alias `getContext().getCollection()`.</span><span class="sxs-lookup"><span data-stu-id="2086b-242">`__` (double-underscore) is an alias to `getContext().getCollection()`.</span></span>
> <br/>
> <span data-ttu-id="2086b-243">Jinými slovy, můžete použít `__` nebo `getContext().getCollection()` pro přístup k rozhraní API dotazu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-243">In other words, you can use `__` or `getContext().getCollection()` to access the JavaScript query API.</span></span>
> 
> 

<span data-ttu-id="2086b-244">Podporované funkce patří:</span><span class="sxs-lookup"><span data-stu-id="2086b-244">Supported functions include:</span></span>

<ul>
<li><span data-ttu-id="2086b-245">
<b>chain().... hodnota ([zpětného volání] [, možnosti])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-245">
<b>chain() ... .value([callback] [, options])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-246">Začíná value() zřetězené volání, které musí být ukončena.</span><span class="sxs-lookup"><span data-stu-id="2086b-246">Starts a chained call which must be terminated with value().</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-247">
<b>Filtr (predicateFunction [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-247">
<b>filter(predicateFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-248">Filtry vstupu pomocí funkce predikátu, která vrátí hodnotu true nebo false, chcete-li filtrovat a konec vstupu dokumenty do výsledné sady.</span><span class="sxs-lookup"><span data-stu-id="2086b-248">Filters the input using a predicate function which returns true/false in order to filter in/out input documents into the resulting set.</span></span> <span data-ttu-id="2086b-249">To se chová podobně jako klauzule WHERE v příkazu SQL.</span><span class="sxs-lookup"><span data-stu-id="2086b-249">This behaves similar to a WHERE clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-250">
<b>mapy (transformationFunction [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-250">
<b>map(transformationFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-251">Platí projekci zadané transformační funkce, která se mapuje na objekt jazyka JavaScript nebo hodnota každé vstupní položky.</span><span class="sxs-lookup"><span data-stu-id="2086b-251">Applies a projection given a transformation function which maps each input item to a JavaScript object or value.</span></span> <span data-ttu-id="2086b-252">To se chová podobně jako v klauzuli SELECT v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="2086b-252">This behaves similar to a SELECT clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-253">
<b>pluck ([propertyName] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-253">
<b>pluck([propertyName] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-254">Toto je zástupce pro mapu, která extrahuje hodnotu jedinou vlastnost z každé vstupní položky.</span><span class="sxs-lookup"><span data-stu-id="2086b-254">This is a shortcut for a map which extracts the value of a single property from each input item.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-255">
<b>vyrovnání ([isShallow] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-255">
<b>flatten([isShallow] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-256">Kombinuje a vyrovná pole z každé vstupní položky do jednoho pole.</span><span class="sxs-lookup"><span data-stu-id="2086b-256">Combines and flattens arrays from each input item in to a single array.</span></span> <span data-ttu-id="2086b-257">To se chová podobně jako označit více v technologii LINQ.</span><span class="sxs-lookup"><span data-stu-id="2086b-257">This behaves similar to SelectMany in LINQ.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-258">
<b>sortBy ([predikát] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-258">
<b>sortBy([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-259">Vytvořit novou sadu dokumentů podle řazení dokumenty v datovém proudu vstupní dokument ve vzestupném pořadí pomocí daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="2086b-259">Produce a new set of documents by sorting the documents in the input document stream in ascending order using the given predicate.</span></span> <span data-ttu-id="2086b-260">To se chová podobně jako klauzuli ORDER BY v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="2086b-260">This behaves similar to a ORDER BY clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="2086b-261">
<b>sortbydescending – ([predikát] [, možnosti] [, zpětného volání])</b>
</span><span class="sxs-lookup"><span data-stu-id="2086b-261">
<b>sortByDescending([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="2086b-262">Vytvořit novou sadu dokumentů podle řazení dokumenty v datovém proudu vstupní dokument v sestupném pořadí pomocí daného predikátu.</span><span class="sxs-lookup"><span data-stu-id="2086b-262">Produce a new set of documents by sorting the documents in the input document stream in descending order using the given predicate.</span></span> <span data-ttu-id="2086b-263">To se chová podobně jako klauzuli ORDER BY x DESC v systému SQL.</span><span class="sxs-lookup"><span data-stu-id="2086b-263">This behaves similar to a ORDER BY x DESC clause in SQL.</span></span>
</li>
</ul>
</li>
</ul>


<span data-ttu-id="2086b-264">Pokud je součástí uvnitř predikátu nebo pro výběr funkcí, následující konstrukce JavaScript získat automaticky optimalizovaná tak, aby spustit přímo v Azure Cosmos DB indexů:</span><span class="sxs-lookup"><span data-stu-id="2086b-264">When included inside predicate and/or selector functions, the following JavaScript constructs get automatically optimized to run directly on Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="2086b-265">Jednoduché operátory: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span><span class="sxs-lookup"><span data-stu-id="2086b-265">Simple operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span></span> ~
* <span data-ttu-id="2086b-266">Literály, včetně objekt literálu: {}</span><span class="sxs-lookup"><span data-stu-id="2086b-266">Literals, including the object literal: {}</span></span>
* <span data-ttu-id="2086b-267">var, návratový</span><span class="sxs-lookup"><span data-stu-id="2086b-267">var, return</span></span>

<span data-ttu-id="2086b-268">Následující konstrukce JavaScript získat není optimalizovaný pro Azure Cosmos DB indexů:</span><span class="sxs-lookup"><span data-stu-id="2086b-268">The following JavaScript constructs do not get optimized for Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="2086b-269">Řízení toku (například v případě, pro, zatímco)</span><span class="sxs-lookup"><span data-stu-id="2086b-269">Control flow (e.g. if, for, while)</span></span>
* <span data-ttu-id="2086b-270">Volání funkcí</span><span class="sxs-lookup"><span data-stu-id="2086b-270">Function calls</span></span>

<span data-ttu-id="2086b-271">Další informace najdete v tématu naše [serverové JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="2086b-271">For more information, please see our [Server-Side JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span></span>

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a><span data-ttu-id="2086b-272">Příklad: Zápis uložené procedury pomocí rozhraní API dotazu jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="2086b-272">Example: Write a stored procedure using the JavaScript query API</span></span>
<span data-ttu-id="2086b-273">Následující ukázka kódu je příklad použití rozhraní API dotazu jazyka JavaScript v rámci uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="2086b-273">The following code sample is an example of how the JavaScript Query API can be used in the context of a stored procedure.</span></span> <span data-ttu-id="2086b-274">Uložená procedura vloží dokumentu, poskytují vstupní parametr a metadat aktualizací dokumentů, pomocí `__.filter()` metoda s minSize, maxSize a totalSize podle vlastnosti velikosti vstupní dokument.</span><span class="sxs-lookup"><span data-stu-id="2086b-274">The stored procedure inserts a document, given by an input parameter, and updates a metadata document, using the `__.filter()` method, with minSize, maxSize, and totalSize based upon the input document's size property.</span></span>

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a><span data-ttu-id="2086b-275">Server SQL k Javascript – tahák</span><span class="sxs-lookup"><span data-stu-id="2086b-275">SQL to Javascript cheat sheet</span></span>
<span data-ttu-id="2086b-276">Následující tabulka uvádí různé dotazy SQL a odpovídající dotazy jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2086b-276">The following table presents various SQL queries and the corresponding JavaScript queries.</span></span>

<span data-ttu-id="2086b-277">Jako s dotazy SQL dokumentu vlastnosti klíče (například `doc.id`) malých a velkých písmen.</span><span class="sxs-lookup"><span data-stu-id="2086b-277">As with SQL queries, document property keys (e.g. `doc.id`) are case-sensitive.</span></span>

|<span data-ttu-id="2086b-278">SQL</span><span class="sxs-lookup"><span data-stu-id="2086b-278">SQL</span></span>| <span data-ttu-id="2086b-279">Rozhraní API dotazu jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="2086b-279">JavaScript Query API</span></span>|<span data-ttu-id="2086b-280">Popis dole</span><span class="sxs-lookup"><span data-stu-id="2086b-280">Description below</span></span>|
|---|---|---|
|<span data-ttu-id="2086b-281">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="2086b-281">SELECT *</span></span><br><span data-ttu-id="2086b-282">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-282">FROM docs</span></span>| <span data-ttu-id="2086b-283">__.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-283">__.map(function(doc) {</span></span> <br><span data-ttu-id="2086b-284">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc;</span><span class="sxs-lookup"><span data-stu-id="2086b-284">&nbsp;&nbsp;&nbsp;&nbsp;return doc;</span></span><br><span data-ttu-id="2086b-285">});</span><span class="sxs-lookup"><span data-stu-id="2086b-285">});</span></span>|<span data-ttu-id="2086b-286">1</span><span class="sxs-lookup"><span data-stu-id="2086b-286">1</span></span>|
|<span data-ttu-id="2086b-287">Vyberte docs.id, docs.message jako msg, docs.actions</span><span class="sxs-lookup"><span data-stu-id="2086b-287">SELECT docs.id, docs.message AS msg, docs.actions</span></span> <br><span data-ttu-id="2086b-288">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-288">FROM docs</span></span>|<span data-ttu-id="2086b-289">__.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-289">__.map(function(doc) {</span></span><br><span data-ttu-id="2086b-290">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {</span><span class="sxs-lookup"><span data-stu-id="2086b-290">&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="2086b-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="2086b-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="2086b-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span><span class="sxs-lookup"><span data-stu-id="2086b-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span></span><br><span data-ttu-id="2086b-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions</span><span class="sxs-lookup"><span data-stu-id="2086b-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions</span></span><br><span data-ttu-id="2086b-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="2086b-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="2086b-295">});</span><span class="sxs-lookup"><span data-stu-id="2086b-295">});</span></span>|<span data-ttu-id="2086b-296">2</span><span class="sxs-lookup"><span data-stu-id="2086b-296">2</span></span>|
|<span data-ttu-id="2086b-297">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="2086b-297">SELECT *</span></span><br><span data-ttu-id="2086b-298">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-298">FROM docs</span></span><br><span data-ttu-id="2086b-299">KDE docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="2086b-299">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="2086b-300">__.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-300">__.filter(function(doc) {</span></span><br><span data-ttu-id="2086b-301">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="2086b-301">&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="2086b-302">});</span><span class="sxs-lookup"><span data-stu-id="2086b-302">});</span></span>|<span data-ttu-id="2086b-303">3</span><span class="sxs-lookup"><span data-stu-id="2086b-303">3</span></span>|
|<span data-ttu-id="2086b-304">VYBERTE *</span><span class="sxs-lookup"><span data-stu-id="2086b-304">SELECT *</span></span><br><span data-ttu-id="2086b-305">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-305">FROM docs</span></span><br><span data-ttu-id="2086b-306">KDE ARRAY_CONTAINS (dokumentace. Značky, 123)</span><span class="sxs-lookup"><span data-stu-id="2086b-306">WHERE ARRAY_CONTAINS(docs.Tags, 123)</span></span>|<span data-ttu-id="2086b-307">__.Filter(Function(x) {</span><span class="sxs-lookup"><span data-stu-id="2086b-307">__.filter(function(x) {</span></span><br><span data-ttu-id="2086b-308">&nbsp;&nbsp;&nbsp;&nbsp;Vrátí x.Tags & & x.Tags.indexOf(123) > -1;</span><span class="sxs-lookup"><span data-stu-id="2086b-308">&nbsp;&nbsp;&nbsp;&nbsp;return x.Tags && x.Tags.indexOf(123) > -1;</span></span><br><span data-ttu-id="2086b-309">});</span><span class="sxs-lookup"><span data-stu-id="2086b-309">});</span></span>|<span data-ttu-id="2086b-310">4</span><span class="sxs-lookup"><span data-stu-id="2086b-310">4</span></span>|
|<span data-ttu-id="2086b-311">Vyberte docs.id, docs.message jako msg</span><span class="sxs-lookup"><span data-stu-id="2086b-311">SELECT docs.id, docs.message AS msg</span></span><br><span data-ttu-id="2086b-312">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-312">FROM docs</span></span><br><span data-ttu-id="2086b-313">KDE docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="2086b-313">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="2086b-314">__.chain()</span><span class="sxs-lookup"><span data-stu-id="2086b-314">__.chain()</span></span><br><span data-ttu-id="2086b-315">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-315">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="2086b-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="2086b-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="2086b-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="2086b-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="2086b-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {</span></span><br><span data-ttu-id="2086b-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {</span><span class="sxs-lookup"><span data-stu-id="2086b-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="2086b-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,</span><span class="sxs-lookup"><span data-stu-id="2086b-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="2086b-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span><span class="sxs-lookup"><span data-stu-id="2086b-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span></span><br><span data-ttu-id="2086b-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="2086b-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="2086b-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="2086b-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="2086b-324">.Value();</span><span class="sxs-lookup"><span data-stu-id="2086b-324">.value();</span></span>|<span data-ttu-id="2086b-325">5</span><span class="sxs-lookup"><span data-stu-id="2086b-325">5</span></span>|
|<span data-ttu-id="2086b-326">SELECT VALUE – značka</span><span class="sxs-lookup"><span data-stu-id="2086b-326">SELECT VALUE tag</span></span><br><span data-ttu-id="2086b-327">Z dokumentace</span><span class="sxs-lookup"><span data-stu-id="2086b-327">FROM docs</span></span><br><span data-ttu-id="2086b-328">Připojte značky v dokumentaci. Značky</span><span class="sxs-lookup"><span data-stu-id="2086b-328">JOIN tag IN docs.Tags</span></span><br><span data-ttu-id="2086b-329">Docs._ts ORDER BY</span><span class="sxs-lookup"><span data-stu-id="2086b-329">ORDER BY docs._ts</span></span>|<span data-ttu-id="2086b-330">__.chain()</span><span class="sxs-lookup"><span data-stu-id="2086b-330">__.chain()</span></span><br><span data-ttu-id="2086b-331">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {</span><span class="sxs-lookup"><span data-stu-id="2086b-331">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="2086b-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí dokumentů. Značky & & Array.IsArray – (dokumentů. Značky);</span><span class="sxs-lookup"><span data-stu-id="2086b-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.Tags && Array.isArray(doc.Tags);</span></span><br><span data-ttu-id="2086b-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="2086b-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="2086b-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span><span class="sxs-lookup"><span data-stu-id="2086b-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span></span><br><span data-ttu-id="2086b-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc._ts;</span><span class="sxs-lookup"><span data-stu-id="2086b-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc._ts;</span></span><br><span data-ttu-id="2086b-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="2086b-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="2086b-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span><span class="sxs-lookup"><span data-stu-id="2086b-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span></span><br><span data-ttu-id="2086b-338">&nbsp;&nbsp;&nbsp;&nbsp;.Flatten()</span><span class="sxs-lookup"><span data-stu-id="2086b-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span></span><br><span data-ttu-id="2086b-339">&nbsp;&nbsp;&nbsp;&nbsp;.Value()</span><span class="sxs-lookup"><span data-stu-id="2086b-339">&nbsp;&nbsp;&nbsp;&nbsp;.value()</span></span>|<span data-ttu-id="2086b-340">6</span><span class="sxs-lookup"><span data-stu-id="2086b-340">6</span></span>|

<span data-ttu-id="2086b-341">V následujících popisech popisují každý dotaz v předchozí tabulce.</span><span class="sxs-lookup"><span data-stu-id="2086b-341">The following descriptions explain each query in the table above.</span></span>
1. <span data-ttu-id="2086b-342">Výsledkem všechny dokumenty (čísla stránek vložena s token pokračování), jako je.</span><span class="sxs-lookup"><span data-stu-id="2086b-342">Results in all documents (paginated with continuation token) as is.</span></span>
2. <span data-ttu-id="2086b-343">Projekty id, zprávy (alias pro msg) a akce z všechny dokumenty.</span><span class="sxs-lookup"><span data-stu-id="2086b-343">Projects the id, message (aliased to msg), and action from all documents.</span></span>
3. <span data-ttu-id="2086b-344">Dotazy na dokumenty s predikát: id = "X998_Y998".</span><span class="sxs-lookup"><span data-stu-id="2086b-344">Queries for documents with the predicate: id = "X998_Y998".</span></span>
4. <span data-ttu-id="2086b-345">Dotazy pro dokumenty, které mají značky a vlastnost Tags je pole obsahující hodnotu 123.</span><span class="sxs-lookup"><span data-stu-id="2086b-345">Queries for documents that have a Tags property and Tags is an array containing the value 123.</span></span>
5. <span data-ttu-id="2086b-346">Dotazy na dokumenty s predikátem, id = "X998_Y998" a pak projekty id a zprávy (alias pro msg).</span><span class="sxs-lookup"><span data-stu-id="2086b-346">Queries for documents with a predicate, id = "X998_Y998", and then projects the id and message (aliased to msg).</span></span>
6. <span data-ttu-id="2086b-347">Filtry pro dokumenty, které mají ve vlastnosti pole značky, a seřadí výsledné dokumenty _ts časové razítko systému vlastnost a potom projekty + vyrovná pole značky.</span><span class="sxs-lookup"><span data-stu-id="2086b-347">Filters for documents which have an array property, Tags, and sorts the resulting documents by the _ts timestamp system property, and then projects + flattens the Tags array.</span></span>


## <a name="runtime-support"></a><span data-ttu-id="2086b-348">Podpora modulu runtime</span><span class="sxs-lookup"><span data-stu-id="2086b-348">Runtime support</span></span>
<span data-ttu-id="2086b-349">[Rozhraní API jazyka DocumentDB JavaScript serveru straně](http://azure.github.io/azure-documentdb-js-server/) poskytuje podporu pro většinu všeobecně funkce jazyka JavaScript jako standardizované podle [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span><span class="sxs-lookup"><span data-stu-id="2086b-349">[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) provides support for the most of the mainstream JavaScript language features as standardized by [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span></span>

### <a name="security"></a><span data-ttu-id="2086b-350">Zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2086b-350">Security</span></span>
<span data-ttu-id="2086b-351">JavaScript uložené procedury a triggery jsou v izolovaném prostoru tak, aby důsledky jeden skript není pronikly na druhý bez průchodu přes transakci izolace snímku na úrovni databáze.</span><span class="sxs-lookup"><span data-stu-id="2086b-351">JavaScript stored procedures and triggers are sandboxed so that the effects of one script do not leak to the other without going through the snapshot transaction isolation at the database level.</span></span> <span data-ttu-id="2086b-352">Prostředí runtime jsou ve fondu, ale čištění kontextu po každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="2086b-352">The runtime environments are pooled but cleaned of the context after each run.</span></span> <span data-ttu-id="2086b-353">Proto jsou zaručeno bezpečné z jakékoli nezamýšleným vedlejší účinky od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="2086b-353">Hence they are guaranteed to be safe of any unintended side effects from each other.</span></span>

### <a name="pre-compilation"></a><span data-ttu-id="2086b-354">Předkompilace</span><span class="sxs-lookup"><span data-stu-id="2086b-354">Pre-compilation</span></span>
<span data-ttu-id="2086b-355">Uložené procedury, triggery a UDF jsou implicitně předkompilovaných na formát kódu byte předejdete tak náklady na kompilace v době každé vyvolání skriptu.</span><span class="sxs-lookup"><span data-stu-id="2086b-355">Stored procedures, triggers and UDFs are implicitly precompiled to the byte code format in order to avoid compilation cost at the time of each script invocation.</span></span> <span data-ttu-id="2086b-356">To zajišťuje volání uložené procedury jsou rychlé a nízkým nárokům mít.</span><span class="sxs-lookup"><span data-stu-id="2086b-356">This ensures invocations of stored procedures are fast and have a low footprint.</span></span>

## <a name="client-sdk-support"></a><span data-ttu-id="2086b-357">Podpora klienta SDK</span><span class="sxs-lookup"><span data-stu-id="2086b-357">Client SDK support</span></span>
<span data-ttu-id="2086b-358">Kromě rozhraní API služby DocumentDB [Node.js](documentdb-sdk-node.md) má klient Azure Cosmos DB [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), a [Python SDK](documentdb-sdk-python.md) pro DocumentDB rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2086b-358">In addition to the DocumentDB API for [Node.js](documentdb-sdk-node.md) client, Azure Cosmos DB has [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/), and [Python SDKs](documentdb-sdk-python.md) for the DocumentDB API.</span></span> <span data-ttu-id="2086b-359">Uložené procedury, triggery a UDF lze vytvořit a spustit některé z těchto sad SDK také používá.</span><span class="sxs-lookup"><span data-stu-id="2086b-359">Stored procedures, triggers and UDFs can be created and executed using any of these SDKs as well.</span></span> <span data-ttu-id="2086b-360">Následující příklad ukazuje postup vytvoření a provedení uložené procedury pomocí klienta rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="2086b-360">The following example shows how to create and execute a stored procedure using the .NET client.</span></span> <span data-ttu-id="2086b-361">Všimněte si, jak jsou typy .NET předaný do uložené procedury jako JSON a čtení zpět.</span><span class="sxs-lookup"><span data-stu-id="2086b-361">Note how the .NET types are passed into the stored procedure as JSON and read back.</span></span>

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


<span data-ttu-id="2086b-362">Tento příklad ukazuje způsob použití [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) vytvořit před aktivační události a vytvořit dokument s aktivační událost povolena.</span><span class="sxs-lookup"><span data-stu-id="2086b-362">This sample shows how to use the [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) to create a pre-trigger and create a document with the trigger enabled.</span></span> 

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


<span data-ttu-id="2086b-363">A následující příklad ukazuje, jak vytvořit uživatelsky definované funkce (UDF) a použít ho [dotazu DocumentDB API SQL](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="2086b-363">And the following example shows how to create a user defined function (UDF) and use it in a [DocumentDB API SQL query](documentdb-sql-query.md).</span></span>

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

## <a name="rest-api"></a><span data-ttu-id="2086b-364">REST API</span><span class="sxs-lookup"><span data-stu-id="2086b-364">REST API</span></span>
<span data-ttu-id="2086b-365">Všechny operace Azure Cosmos databáze lze provést RESTful způsobem.</span><span class="sxs-lookup"><span data-stu-id="2086b-365">All Azure Cosmos DB operations can be performed in a RESTful manner.</span></span> <span data-ttu-id="2086b-366">Uložené procedury, triggery a uživatelem definované funkce může být registrováno v rámci kolekce pomocí HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="2086b-366">Stored procedures, triggers and user-defined functions can be registered under a collection by using HTTP POST.</span></span> <span data-ttu-id="2086b-367">Následuje příklad toho, jak zaregistrovat uložené procedury:</span><span class="sxs-lookup"><span data-stu-id="2086b-367">The following is an example of how to register a stored procedure:</span></span>

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


<span data-ttu-id="2086b-368">Uložená procedura je zaregistrován spuštěním požadavek POST URI databází nebo testdb/colls/testColl/sprocs spolu s textem obsahující uložené procedury k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="2086b-368">The stored procedure is registered by executing a POST request against the URI dbs/testdb/colls/testColl/sprocs with the body containing the stored procedure to create.</span></span> <span data-ttu-id="2086b-369">Aktivační události a funkcí UDF lze registrovat podobně vydáním POST proti/aktivační události a /udfs v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="2086b-369">Triggers and UDFs can be registered similarly by issuing a POST against /triggers and /udfs respectively.</span></span>
<span data-ttu-id="2086b-370">Tato uložená procedura může poté provést po vydání požadavku POST s jeho odkazu prostředku:</span><span class="sxs-lookup"><span data-stu-id="2086b-370">This stored procedure can then be executed by issuing a POST request against its resource link:</span></span>

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


<span data-ttu-id="2086b-371">Zde vstup uložené procedury je předán v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2086b-371">Here, the input to the stored procedure is passed in the request body.</span></span> <span data-ttu-id="2086b-372">Všimněte si, že vstup je předán jako pole JSON vstupních parametrů.</span><span class="sxs-lookup"><span data-stu-id="2086b-372">Note that the input is passed as a JSON array of input parameters.</span></span> <span data-ttu-id="2086b-373">Uložená procedura přijímá první vstup jako dokument, který je text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="2086b-373">The stored procedure takes the first input as a document that is a response body.</span></span> <span data-ttu-id="2086b-374">Odpovědi, které obdržíme, vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2086b-374">The response we receive is as follows:</span></span>

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


<span data-ttu-id="2086b-375">Na rozdíl od uložené procedury, aktivační události nelze spustit přímo.</span><span class="sxs-lookup"><span data-stu-id="2086b-375">Triggers, unlike stored procedures, cannot be executed directly.</span></span> <span data-ttu-id="2086b-376">Místo toho jsou spouštěny jako součást operace v dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-376">Instead they are executed as part of an operation on a document.</span></span> <span data-ttu-id="2086b-377">Můžeme určit aktivačních událostí ke spuštění s žádostí pomocí hlaviček protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="2086b-377">We can specify the triggers to run with a request using HTTP headers.</span></span> <span data-ttu-id="2086b-378">Toto je požadavek na vytvoření dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2086b-378">The following is request to create a document.</span></span>

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


<span data-ttu-id="2086b-379">Předběžné aktivační událost ke spuštění s požadavkem je zde uvedená v hlavičce x-ms-documentdb-pre-trigger-include.</span><span class="sxs-lookup"><span data-stu-id="2086b-379">Here the pre-trigger to be run with the request is specified in the x-ms-documentdb-pre-trigger-include header.</span></span> <span data-ttu-id="2086b-380">Žádné aktivační události po odpovídajícím způsobem, jsou uvedeny v hlavičce x-ms-documentdb-post-trigger-include.</span><span class="sxs-lookup"><span data-stu-id="2086b-380">Correspondingly, any post-triggers are given in the x-ms-documentdb-post-trigger-include header.</span></span> <span data-ttu-id="2086b-381">Všimněte si, že oba před a po aktivační události lze zadat pro daný požadavek.</span><span class="sxs-lookup"><span data-stu-id="2086b-381">Note that both pre- and post-triggers can be specified for a given request.</span></span>

## <a name="sample-code"></a><span data-ttu-id="2086b-382">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="2086b-382">Sample code</span></span>
<span data-ttu-id="2086b-383">Můžete najít další příklady kódu na straně serveru (včetně [hromadné odstranění](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), a [aktualizace](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) na našem [úložiště GitHub](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="2086b-383">You can find more server-side code examples (including [bulk-delete](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), and [update](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) on our [GitHub repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span></span>

<span data-ttu-id="2086b-384">Chcete sdílet vaše Super uložené procedury?</span><span class="sxs-lookup"><span data-stu-id="2086b-384">Want to share your awesome stored procedure?</span></span> <span data-ttu-id="2086b-385">Nám prosím pošlete žádost o přijetí změn!</span><span class="sxs-lookup"><span data-stu-id="2086b-385">Please, send us a pull-request!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2086b-386">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2086b-386">Next steps</span></span>
<span data-ttu-id="2086b-387">Až budete mít jeden nebo více uložené procedury, triggery a uživatelem definované funkce vytvoření, můžete je načíst a zobrazit je na portálu Azure pomocí Průzkumníku dat.</span><span class="sxs-lookup"><span data-stu-id="2086b-387">Once you have one or more stored procedures, triggers, and user-defined functions created, you can load them and view them in the Azure portal using Data Explorer.</span></span>

<span data-ttu-id="2086b-388">Můžete také zjistit následující odkazy a prostředky užitečné ve své cestě Další informace o programování na straně serveru dB Azure Cosmos:</span><span class="sxs-lookup"><span data-stu-id="2086b-388">You may also find the following references and resources useful in your path to learn more about Azure Cosmos dB server-side programming:</span></span>

* [<span data-ttu-id="2086b-389">Sady SDK služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2086b-389">Azure Cosmos DB SDKs</span></span>](documentdb-sdk-dotnet.md)
* [<span data-ttu-id="2086b-390">DocumentDB Studio</span><span class="sxs-lookup"><span data-stu-id="2086b-390">DocumentDB Studio</span></span>](https://github.com/mingaliu/DocumentDBStudio/releases)
* [<span data-ttu-id="2086b-391">JSON</span><span class="sxs-lookup"><span data-stu-id="2086b-391">JSON</span></span>](http://www.json.org/) 
* [<span data-ttu-id="2086b-392">JavaScript ECMA-262</span><span class="sxs-lookup"><span data-stu-id="2086b-392">JavaScript ECMA-262</span></span>](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [<span data-ttu-id="2086b-393">Rozšiřitelnost zabezpečení a přenosné databáze</span><span class="sxs-lookup"><span data-stu-id="2086b-393">Secure and Portable Database Extensibility</span></span>](http://dl.acm.org/citation.cfm?id=276339) 
* [<span data-ttu-id="2086b-394">Služba zaměřené na konkrétní architektura databáze</span><span class="sxs-lookup"><span data-stu-id="2086b-394">Service Oriented Database Architecture</span></span>](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [<span data-ttu-id="2086b-395">Hostování prostředí .NET Runtime v systému Microsoft SQL server</span><span class="sxs-lookup"><span data-stu-id="2086b-395">Hosting the .NET Runtime in Microsoft SQL server</span></span>](http://dl.acm.org/citation.cfm?id=1007669)

