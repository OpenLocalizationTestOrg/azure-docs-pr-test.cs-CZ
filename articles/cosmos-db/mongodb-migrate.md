---
title: "aaaUse mongoimport a mongorestore s hello rozhraní API služby Azure Cosmos DB pro MongoDB | Microsoft Docs"
description: "Zjistěte, jak toouse mongoimport a mongorestore tooimport data tooan rozhraní API pro MongoDB účet"
keywords: mongoimport mongorestore
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 921354bc7b09a076a73e0cbf5e4aabcc9e83d5a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a><span data-ttu-id="c563e-104">Azure Cosmos DB: Data pro Import MongoDB</span><span class="sxs-lookup"><span data-stu-id="c563e-104">Azure Cosmos DB: Import MongoDB data</span></span> 

<span data-ttu-id="c563e-105">toomigrate data z MongoDB tooan účet Azure Cosmos DB pro použití s hello rozhraní API pro MongoDB, musíte:</span><span class="sxs-lookup"><span data-stu-id="c563e-105">toomigrate data from MongoDB tooan Azure Cosmos DB account for use with hello API for MongoDB, you must:</span></span>

* <span data-ttu-id="c563e-106">Stáhněte si buď *mongoimport.exe* nebo *mongorestore.exe* z hello [MongoDB Download Center](https://www.mongodb.com/download-center).</span><span class="sxs-lookup"><span data-stu-id="c563e-106">Download either *mongoimport.exe* or *mongorestore.exe* from hello [MongoDB Download Center](https://www.mongodb.com/download-center).</span></span>
* <span data-ttu-id="c563e-107">Získat vaše [rozhraní API pro MongoDB připojovací řetězec](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="c563e-107">Get your [API for MongoDB connection string](connect-mongodb-account.md).</span></span>

<span data-ttu-id="c563e-108">Pokud importujete data z MongoDB a plán toouse její hello Azure Cosmos DB, měli byste použít hello [nástroj pro migraci dat](import-data.md) tooimport data.</span><span class="sxs-lookup"><span data-stu-id="c563e-108">If you are importing data from MongoDB and plan toouse it with hello Azure Cosmos DB, you should use hello [Data Migration tool](import-data.md) tooimport data.</span></span>

<span data-ttu-id="c563e-109">Tento kurz se zabývá hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c563e-109">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c563e-110">Získávání připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="c563e-110">Retrieving your connection string</span></span>
> * <span data-ttu-id="c563e-111">Import dat MongoDB pomocí mongoimport</span><span class="sxs-lookup"><span data-stu-id="c563e-111">Importing MongoDB data by using mongoimport</span></span>
> * <span data-ttu-id="c563e-112">Import dat MongoDB pomocí mongorestore</span><span class="sxs-lookup"><span data-stu-id="c563e-112">Importing MongoDB data by using mongorestore</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c563e-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c563e-113">Prerequisites</span></span>

* <span data-ttu-id="c563e-114">Zvýšit propustnost: délka hello migraci dat závisí na množství hello propustnost nastavení pro kolekce.</span><span class="sxs-lookup"><span data-stu-id="c563e-114">Increase throughput: hello duration of your data migration depends on hello amount of throughput you set up for your collections.</span></span> <span data-ttu-id="c563e-115">Být, že tooincrease hello propustnost pro větší dat migrace.</span><span class="sxs-lookup"><span data-stu-id="c563e-115">Be sure tooincrease hello throughput for larger data migrations.</span></span> <span data-ttu-id="c563e-116">Po dokončení migrace hello, snížit náklady toosave propustnost hello.</span><span class="sxs-lookup"><span data-stu-id="c563e-116">After you've completed hello migration, decrease hello throughput toosave costs.</span></span> <span data-ttu-id="c563e-117">Další informace o zvýšení propustnosti v hello [portál Azure](https://portal.azure.com), najdete v části [úrovně výkonu a cenové úrovně v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="c563e-117">For more information about increasing throughput in hello [Azure portal](https://portal.azure.com), see [Performance levels and pricing tiers in Azure Cosmos DB](performance-levels.md).</span></span>

* <span data-ttu-id="c563e-118">Povolte protokol SSL: Azure Cosmos DB má vysokými nároky na zabezpečení a standardy.</span><span class="sxs-lookup"><span data-stu-id="c563e-118">Enable SSL: Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="c563e-119">Pokud při práci s vaším účtem, být zda tooenable SSL.</span><span class="sxs-lookup"><span data-stu-id="c563e-119">Be sure tooenable SSL when you interact with your account.</span></span> <span data-ttu-id="c563e-120">Zahrnout Hello postupy v hello zbývající části článku hello jak tooenable SSL pro mongoimport a mongorestore.</span><span class="sxs-lookup"><span data-stu-id="c563e-120">hello procedures in hello rest of hello article include how tooenable SSL for mongoimport and mongorestore.</span></span>

## <a name="find-your-connection-string-information-host-port-username-and-password"></a><span data-ttu-id="c563e-121">Najít vaše informace o připojovacím řetězci (hostitelů, port, uživatelské jméno a heslo)</span><span class="sxs-lookup"><span data-stu-id="c563e-121">Find your connection string information (host, port, username, and password)</span></span>

1. <span data-ttu-id="c563e-122">V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Cosmos DB** položku.</span><span class="sxs-lookup"><span data-stu-id="c563e-122">In hello [Azure portal](https://portal.azure.com), in hello left pane, click hello **Azure Cosmos DB** entry.</span></span>
2. <span data-ttu-id="c563e-123">V hello **odběry** podokně, vyberte název účtu.</span><span class="sxs-lookup"><span data-stu-id="c563e-123">In hello **Subscriptions** pane, select your account name.</span></span>
3. <span data-ttu-id="c563e-124">V hello **připojovací řetězec** okně klikněte na tlačítko **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="c563e-124">In hello **Connection String** blade, click **Connection String**.</span></span>  
<span data-ttu-id="c563e-125">pravé podokno obsahuje všechny informace hello, je nutné, aby toosuccessfully Hello připojit tooyour účet.</span><span class="sxs-lookup"><span data-stu-id="c563e-125">hello right pane contains all hello information that you need toosuccessfully connect tooyour account.</span></span>

    ![Okno řetězec připojení](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a><span data-ttu-id="c563e-127">Importovat data toohello rozhraní API pro MongoDB pomocí mongoimport</span><span class="sxs-lookup"><span data-stu-id="c563e-127">Import data toohello API for MongoDB by using mongoimport</span></span>

<span data-ttu-id="c563e-128">tooimport data tooyour Azure Cosmos DB účet, použijte hello následující šablony.</span><span class="sxs-lookup"><span data-stu-id="c563e-128">tooimport data tooyour Azure Cosmos DB account, use hello following template.</span></span> <span data-ttu-id="c563e-129">Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* hello hodnotami, které jsou specifické tooyour účet.</span><span class="sxs-lookup"><span data-stu-id="c563e-129">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>  

<span data-ttu-id="c563e-130">Šablona:</span><span class="sxs-lookup"><span data-stu-id="c563e-130">Template:</span></span>

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

<span data-ttu-id="c563e-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c563e-131">Example:</span></span>  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a><span data-ttu-id="c563e-132">Importovat data toohello rozhraní API pro MongoDB pomocí mongorestore</span><span class="sxs-lookup"><span data-stu-id="c563e-132">Import data toohello API for MongoDB by using mongorestore</span></span>

<span data-ttu-id="c563e-133">toorestore data tooyour rozhraní API pro MongoDB účet, použijte následující importu hello tooexecute šablony hello.</span><span class="sxs-lookup"><span data-stu-id="c563e-133">toorestore data tooyour API for MongoDB account, use hello following template tooexecute hello import.</span></span> <span data-ttu-id="c563e-134">Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* hello hodnotami, které jsou specifické tooyour účet.</span><span class="sxs-lookup"><span data-stu-id="c563e-134">Fill in *host*, *username*, and *password* with hello values that are specific tooyour account.</span></span>

<span data-ttu-id="c563e-135">Šablona:</span><span class="sxs-lookup"><span data-stu-id="c563e-135">Template:</span></span>

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

<span data-ttu-id="c563e-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c563e-136">Example:</span></span>

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a><span data-ttu-id="c563e-137">Příručka pro úspěšné migrace</span><span class="sxs-lookup"><span data-stu-id="c563e-137">Guide for a successful migration</span></span>

1. <span data-ttu-id="c563e-138">Předem vytvořit a škálovat vaše kolekce:</span><span class="sxs-lookup"><span data-stu-id="c563e-138">Pre-create and scale your collections:</span></span>
        
    * <span data-ttu-id="c563e-139">Ve výchozím nastavení zřídí Azure Cosmos DB nové kolekce MongoDB s 1 000 jednotek žádosti (ruština).</span><span class="sxs-lookup"><span data-stu-id="c563e-139">By default, Azure Cosmos DB provisions a new MongoDB collection with 1,000 request units (RUs).</span></span> <span data-ttu-id="c563e-140">Před zahájením migrace hello pomocí mongoimport, mongorestore nebo mongomirror, předem vytvořit všechny kolekce z hello [portál Azure](https://portal.azure.com) nebo z MongoDB ovladače a nástroje.</span><span class="sxs-lookup"><span data-stu-id="c563e-140">Before you start hello migration by using mongoimport, mongorestore, or mongomirror, pre-create all your collections from hello [Azure portal](https://portal.azure.com) or from MongoDB drivers and tools.</span></span> <span data-ttu-id="c563e-141">Pokud vaše kolekce je větší než 10 GB, ujistěte se, že toocreate [horizontálně dělené/oddíly kolekce](partition-data.md) s klíčem odpovídající horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="c563e-141">If your collection is greater than 10 GB, make sure toocreate a [sharded/partitioned collection](partition-data.md) with an appropriate shard key.</span></span>

    * <span data-ttu-id="c563e-142">Z hello [portál Azure](https://portal.azure.com), zvýšit propustnost vaší kolekce z 1 000 RUs pro kolekce tvořené jedním oddílem a odpovídající 2500 RUs pro horizontálně dělenou kolekci jenom pro migraci hello.</span><span class="sxs-lookup"><span data-stu-id="c563e-142">From hello [Azure portal](https://portal.azure.com), increase your collections' throughput from 1,000 RUs for a single partition collection and 2,500 RUs for a sharded collection just for hello migration.</span></span> <span data-ttu-id="c563e-143">S vyšší propustnost hello můžete vyhnout, omezení a migraci za kratší dobu.</span><span class="sxs-lookup"><span data-stu-id="c563e-143">With hello higher throughput, you can avoid throttling and migrate in less time.</span></span> <span data-ttu-id="c563e-144">S každou hodinu fakturace v Azure Cosmos DB, můžete snížit propustnost hello ihned po hello migrace toosave náklady.</span><span class="sxs-lookup"><span data-stu-id="c563e-144">With hourly billing in Azure Cosmos DB, you can reduce hello throughput immediately after hello migration toosave costs.</span></span>

2. <span data-ttu-id="c563e-145">Vypočítejte hello přibližnou RU zdarma pro jednotlivý dokument zápisu:</span><span class="sxs-lookup"><span data-stu-id="c563e-145">Calculate hello approximate RU charge for a single document write:</span></span>

    <span data-ttu-id="c563e-146">a.</span><span class="sxs-lookup"><span data-stu-id="c563e-146">a.</span></span> <span data-ttu-id="c563e-147">Připojte databázi Azure Cosmos DB MongoDB tooyour z hello prostředí MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c563e-147">Connect tooyour Azure Cosmos DB MongoDB database from hello MongoDB Shell.</span></span> <span data-ttu-id="c563e-148">Můžete najít pokyny v [připojit MongoDB aplikace tooAzure Cosmos DB](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="c563e-148">You can find instructions in [Connect a MongoDB application tooAzure Cosmos DB](connect-mongodb-account.md).</span></span>
    
    <span data-ttu-id="c563e-149">b.</span><span class="sxs-lookup"><span data-stu-id="c563e-149">b.</span></span> <span data-ttu-id="c563e-150">Spuštění příkazu insert ukázka pomocí jedné z ukázkových dokumentů z hello MongoDB prostředí:</span><span class="sxs-lookup"><span data-stu-id="c563e-150">Run a sample insert command by using one of your sample documents from hello MongoDB Shell:</span></span>
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    <span data-ttu-id="c563e-151">c.</span><span class="sxs-lookup"><span data-stu-id="c563e-151">c.</span></span> <span data-ttu-id="c563e-152">Spustit ```db.runCommand({getLastRequestStatistics: 1})``` a obdržíte odpověď podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c563e-152">Run ```db.runCommand({getLastRequestStatistics: 1})``` and you will receive a response like this one:</span></span>
     
        ```
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
        ```
        
    <span data-ttu-id="c563e-153">d.</span><span class="sxs-lookup"><span data-stu-id="c563e-153">d.</span></span> <span data-ttu-id="c563e-154">Poznamenejte si hello požadavek poplatků.</span><span class="sxs-lookup"><span data-stu-id="c563e-154">Take note of hello request charge.</span></span>
    
3. <span data-ttu-id="c563e-155">Určení hello latence z vašeho počítače toohello Azure Cosmos DB cloudové služby:</span><span class="sxs-lookup"><span data-stu-id="c563e-155">Determine hello latency from your machine toohello Azure Cosmos DB cloud service:</span></span>
    
    <span data-ttu-id="c563e-156">a.</span><span class="sxs-lookup"><span data-stu-id="c563e-156">a.</span></span> <span data-ttu-id="c563e-157">Zapnutí podrobného protokolování z hello MongoDB prostředí pomocí tohoto příkazu:```setVerboseShell(true)```</span><span class="sxs-lookup"><span data-stu-id="c563e-157">Enable verbose logging from hello MongoDB Shell by using this command: ```setVerboseShell(true)```</span></span>
    
    <span data-ttu-id="c563e-158">b.</span><span class="sxs-lookup"><span data-stu-id="c563e-158">b.</span></span> <span data-ttu-id="c563e-159">Spusťte jednoduchý dotaz na databázi hello: ```db.coll.find().limit(1)```.</span><span class="sxs-lookup"><span data-stu-id="c563e-159">Run a simple query against hello database: ```db.coll.find().limit(1)```.</span></span> <span data-ttu-id="c563e-160">Zobrazí odpověď podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c563e-160">You will receive a response like this one:</span></span>

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. <span data-ttu-id="c563e-161">Odeberte hello vložit dokument před tooensure hello migrace, nejsou žádné duplicitní dokumenty.</span><span class="sxs-lookup"><span data-stu-id="c563e-161">Remove hello inserted document before hello migration tooensure that there are no duplicate documents.</span></span> <span data-ttu-id="c563e-162">Dokumenty můžete odebrat pomocí tohoto příkazu:```db.coll.remove({})```</span><span class="sxs-lookup"><span data-stu-id="c563e-162">You can remove documents by using this command: ```db.coll.remove({})```</span></span>

5. <span data-ttu-id="c563e-163">Vypočítat hello přibližnou *batchSize* a *numInsertionWorkers* hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c563e-163">Calculate hello approximate *batchSize* and *numInsertionWorkers* values:</span></span>

    * <span data-ttu-id="c563e-164">Pro *batchSize*, celkem hello dělení zřízený RUs podle hello RUs používán z vaší jednotlivý dokument zápisu v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="c563e-164">For *batchSize*, divide hello total provisioned RUs by hello RUs consumed from your single document write in step 3.</span></span>
    
    * <span data-ttu-id="c563e-165">Pokud hello vypočítat *batchSize* < = 24, použijte toto číslo jako vaše *batchSize* hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c563e-165">If hello calculated *batchSize* <= 24, use that number as your *batchSize* value.</span></span>
    
    * <span data-ttu-id="c563e-166">Pokud hello vypočítat *batchSize* > 24, sada hello *batchSize* too24 hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c563e-166">If hello calculated *batchSize* > 24, set hello *batchSize* value too24.</span></span>
    
    * <span data-ttu-id="c563e-167">Pro *numInsertionWorkers*, použijte tento rovnice: *numInsertionWorkers = (zřízené propustnosti * čekací doby v sekundách) / (velikost dávky * využité RUs pro jeden zápis)*.</span><span class="sxs-lookup"><span data-stu-id="c563e-167">For *numInsertionWorkers*, use this equation:   *numInsertionWorkers =  (provisioned throughput * latency in seconds) / (batch size * consumed RUs for a single write)*.</span></span>
        
    |<span data-ttu-id="c563e-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c563e-168">Property</span></span>|<span data-ttu-id="c563e-169">Hodnota</span><span class="sxs-lookup"><span data-stu-id="c563e-169">Value</span></span>|
    |--------|-----|
    |<span data-ttu-id="c563e-170">batchSize</span><span class="sxs-lookup"><span data-stu-id="c563e-170">batchSize</span></span>| <span data-ttu-id="c563e-171">24</span><span class="sxs-lookup"><span data-stu-id="c563e-171">24</span></span> |
    |<span data-ttu-id="c563e-172">RUs zřídit</span><span class="sxs-lookup"><span data-stu-id="c563e-172">RUs provisioned</span></span> | <span data-ttu-id="c563e-173">10000</span><span class="sxs-lookup"><span data-stu-id="c563e-173">10000</span></span> |
    |<span data-ttu-id="c563e-174">Latence</span><span class="sxs-lookup"><span data-stu-id="c563e-174">Latency</span></span> | <span data-ttu-id="c563e-175">0.100 s</span><span class="sxs-lookup"><span data-stu-id="c563e-175">0.100 s</span></span> |
    |<span data-ttu-id="c563e-176">RU účtovat poplatek za 1 doc zápisu</span><span class="sxs-lookup"><span data-stu-id="c563e-176">RU charged for 1 doc write</span></span> | <span data-ttu-id="c563e-177">10 RUs</span><span class="sxs-lookup"><span data-stu-id="c563e-177">10 RUs</span></span> |
    |<span data-ttu-id="c563e-178">numInsertionWorkers</span><span class="sxs-lookup"><span data-stu-id="c563e-178">numInsertionWorkers</span></span> | <span data-ttu-id="c563e-179">?</span><span class="sxs-lookup"><span data-stu-id="c563e-179">?</span></span> |
    
    <span data-ttu-id="c563e-180">*numInsertionWorkers = (10000 RUs x 0,1 s) / (24 × 10 RUs) = 4.1666*</span><span class="sxs-lookup"><span data-stu-id="c563e-180">*numInsertionWorkers = (10000 RUs x 0.1 s) / (24 x 10 RUs) = 4.1666*</span></span>

6. <span data-ttu-id="c563e-181">Spusťte příkaz hello dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="c563e-181">Run hello final migration command:</span></span>

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a><span data-ttu-id="c563e-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c563e-182">Next steps</span></span>

<span data-ttu-id="c563e-183">Můžete pokračovat dalším kurzu toohello a zjistěte, jak tooquery MongoDB dat pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c563e-183">You can proceed toohello next tutorial and learn how tooquery MongoDB data by using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="c563e-184">Jak tooquery MongoDB dat?</span><span class="sxs-lookup"><span data-stu-id="c563e-184">How tooquery MongoDB data?</span></span>](../cosmos-db/tutorial-query-mongodb.md)
