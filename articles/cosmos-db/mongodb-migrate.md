---
title: "Použít mongoimport a mongorestore s rozhraním API pro Azure Cosmos DB pro MongoDB | Microsoft Docs"
description: "Další informace o použití mongoimport a mongorestore pro import dat do rozhraní API pro MongoDB účet"
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
ms.openlocfilehash: 1555f13c3ea88b61be0ea240b51218b83f6f9724
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-import-mongodb-data"></a><span data-ttu-id="2c017-104">Azure Cosmos DB: Data pro Import MongoDB</span><span class="sxs-lookup"><span data-stu-id="2c017-104">Azure Cosmos DB: Import MongoDB data</span></span> 

<span data-ttu-id="2c017-105">Chcete-li migrovat data z MongoDB k účtu Azure Cosmos DB pro použití s rozhraním API pro MongoDB, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2c017-105">To migrate data from MongoDB to an Azure Cosmos DB account for use with the API for MongoDB, you must:</span></span>

* <span data-ttu-id="2c017-106">Stáhněte si buď *mongoimport.exe* nebo *mongorestore.exe* z [MongoDB stažení softwaru](https://www.mongodb.com/download-center).</span><span class="sxs-lookup"><span data-stu-id="2c017-106">Download either *mongoimport.exe* or *mongorestore.exe* from the [MongoDB Download Center](https://www.mongodb.com/download-center).</span></span>
* <span data-ttu-id="2c017-107">Získat vaše [rozhraní API pro MongoDB připojovací řetězec](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="2c017-107">Get your [API for MongoDB connection string](connect-mongodb-account.md).</span></span>

<span data-ttu-id="2c017-108">Pokud importujete data z MongoDB a plánujete používat s Azure Cosmos DB, měli byste použít [nástroj pro migraci dat](import-data.md) k importu dat.</span><span class="sxs-lookup"><span data-stu-id="2c017-108">If you are importing data from MongoDB and plan to use it with the Azure Cosmos DB, you should use the [Data Migration tool](import-data.md) to import data.</span></span>

<span data-ttu-id="2c017-109">Tento kurz obsahuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="2c017-109">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c017-110">Získávání připojovacího řetězce</span><span class="sxs-lookup"><span data-stu-id="2c017-110">Retrieving your connection string</span></span>
> * <span data-ttu-id="2c017-111">Import dat MongoDB pomocí mongoimport</span><span class="sxs-lookup"><span data-stu-id="2c017-111">Importing MongoDB data by using mongoimport</span></span>
> * <span data-ttu-id="2c017-112">Import dat MongoDB pomocí mongorestore</span><span class="sxs-lookup"><span data-stu-id="2c017-112">Importing MongoDB data by using mongorestore</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c017-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c017-113">Prerequisites</span></span>

* <span data-ttu-id="2c017-114">Zvýšit propustnost: trvání migrace dat závisí na množství propustnost nastavení pro kolekce.</span><span class="sxs-lookup"><span data-stu-id="2c017-114">Increase throughput: The duration of your data migration depends on the amount of throughput you set up for your collections.</span></span> <span data-ttu-id="2c017-115">Ujistěte se, že zvýšit propustnost pro větší dat migrace.</span><span class="sxs-lookup"><span data-stu-id="2c017-115">Be sure to increase the throughput for larger data migrations.</span></span> <span data-ttu-id="2c017-116">Po dokončení migrace, snížit propustnosti, abyste ušetřili náklady.</span><span class="sxs-lookup"><span data-stu-id="2c017-116">After you've completed the migration, decrease the throughput to save costs.</span></span> <span data-ttu-id="2c017-117">Další informace o zvýšení propustnosti v [portál Azure](https://portal.azure.com), najdete v části [úrovně výkonu a cenové úrovně v Azure Cosmos DB](performance-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2c017-117">For more information about increasing throughput in the [Azure portal](https://portal.azure.com), see [Performance levels and pricing tiers in Azure Cosmos DB](performance-levels.md).</span></span>

* <span data-ttu-id="2c017-118">Povolte protokol SSL: Azure Cosmos DB má vysokými nároky na zabezpečení a standardy.</span><span class="sxs-lookup"><span data-stu-id="2c017-118">Enable SSL: Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="2c017-119">Je nutné povolit protokol SSL, pokud při práci s vaším účtem.</span><span class="sxs-lookup"><span data-stu-id="2c017-119">Be sure to enable SSL when you interact with your account.</span></span> <span data-ttu-id="2c017-120">Postupy ve zbývající části článku zahrnují jak povolit SSL pro mongoimport a mongorestore.</span><span class="sxs-lookup"><span data-stu-id="2c017-120">The procedures in the rest of the article include how to enable SSL for mongoimport and mongorestore.</span></span>

## <a name="find-your-connection-string-information-host-port-username-and-password"></a><span data-ttu-id="2c017-121">Najít vaše informace o připojovacím řetězci (hostitelů, port, uživatelské jméno a heslo)</span><span class="sxs-lookup"><span data-stu-id="2c017-121">Find your connection string information (host, port, username, and password)</span></span>

1. <span data-ttu-id="2c017-122">V [portál Azure](https://portal.azure.com), v levém podokně klikněte na tlačítko **Azure Cosmos DB** položku.</span><span class="sxs-lookup"><span data-stu-id="2c017-122">In the [Azure portal](https://portal.azure.com), in the left pane, click the **Azure Cosmos DB** entry.</span></span>
2. <span data-ttu-id="2c017-123">V **odběry** podokně, vyberte název účtu.</span><span class="sxs-lookup"><span data-stu-id="2c017-123">In the **Subscriptions** pane, select your account name.</span></span>
3. <span data-ttu-id="2c017-124">V **připojovací řetězec** okně klikněte na tlačítko **připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="2c017-124">In the **Connection String** blade, click **Connection String**.</span></span>  
<span data-ttu-id="2c017-125">V pravém podokně obsahuje všechny informace, které potřebujete k úspěšnému připojení k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="2c017-125">The right pane contains all the information that you need to successfully connect to your account.</span></span>

    ![Okno řetězec připojení](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-to-the-api-for-mongodb-by-using-mongoimport"></a><span data-ttu-id="2c017-127">Import dat do rozhraní API pro MongoDB pomocí mongoimport</span><span class="sxs-lookup"><span data-stu-id="2c017-127">Import data to the API for MongoDB by using mongoimport</span></span>

<span data-ttu-id="2c017-128">K importu dat do účtu Azure Cosmos DB, použijte následující šablonu.</span><span class="sxs-lookup"><span data-stu-id="2c017-128">To import data to your Azure Cosmos DB account, use the following template.</span></span> <span data-ttu-id="2c017-129">Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* s hodnotami, které jsou specifické pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="2c017-129">Fill in *host*, *username*, and *password* with the values that are specific to your account.</span></span>  

<span data-ttu-id="2c017-130">Šablona:</span><span class="sxs-lookup"><span data-stu-id="2c017-130">Template:</span></span>

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

<span data-ttu-id="2c017-131">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2c017-131">Example:</span></span>  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-to-the-api-for-mongodb-by-using-mongorestore"></a><span data-ttu-id="2c017-132">Import dat do rozhraní API pro MongoDB pomocí mongorestore</span><span class="sxs-lookup"><span data-stu-id="2c017-132">Import data to the API for MongoDB by using mongorestore</span></span>

<span data-ttu-id="2c017-133">K obnovení dat do vašeho rozhraní API pro účet MongoDB, použijte následující šablonu provést import.</span><span class="sxs-lookup"><span data-stu-id="2c017-133">To restore data to your API for MongoDB account, use the following template to execute the import.</span></span> <span data-ttu-id="2c017-134">Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* s hodnotami, které jsou specifické pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="2c017-134">Fill in *host*, *username*, and *password* with the values that are specific to your account.</span></span>

<span data-ttu-id="2c017-135">Šablona:</span><span class="sxs-lookup"><span data-stu-id="2c017-135">Template:</span></span>

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

<span data-ttu-id="2c017-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2c017-136">Example:</span></span>

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a><span data-ttu-id="2c017-137">Příručka pro úspěšné migrace</span><span class="sxs-lookup"><span data-stu-id="2c017-137">Guide for a successful migration</span></span>

1. <span data-ttu-id="2c017-138">Předem vytvořit a škálovat vaše kolekce:</span><span class="sxs-lookup"><span data-stu-id="2c017-138">Pre-create and scale your collections:</span></span>
        
    * <span data-ttu-id="2c017-139">Ve výchozím nastavení zřídí Azure Cosmos DB nové kolekce MongoDB s 1 000 jednotek žádosti (ruština).</span><span class="sxs-lookup"><span data-stu-id="2c017-139">By default, Azure Cosmos DB provisions a new MongoDB collection with 1,000 request units (RUs).</span></span> <span data-ttu-id="2c017-140">Před zahájením migrace pomocí mongoimport, mongorestore nebo mongomirror, předem vytvořit všechny kolekce z [portál Azure](https://portal.azure.com) nebo z MongoDB ovladače a nástroje.</span><span class="sxs-lookup"><span data-stu-id="2c017-140">Before you start the migration by using mongoimport, mongorestore, or mongomirror, pre-create all your collections from the [Azure portal](https://portal.azure.com) or from MongoDB drivers and tools.</span></span> <span data-ttu-id="2c017-141">Pokud vaše kolekce je větší než 10 GB, nezapomeňte vytvořit [horizontálně dělené/oddíly kolekce](partition-data.md) s klíčem odpovídající horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="2c017-141">If your collection is greater than 10 GB, make sure to create a [sharded/partitioned collection](partition-data.md) with an appropriate shard key.</span></span>

    * <span data-ttu-id="2c017-142">Z [portál Azure](https://portal.azure.com), zvýšit propustnost vaší kolekce z 1 000 RUs pro kolekce tvořené jedním oddílem a odpovídající 2500 RUs pro horizontálně dělenou kolekci jenom pro migraci.</span><span class="sxs-lookup"><span data-stu-id="2c017-142">From the [Azure portal](https://portal.azure.com), increase your collections' throughput from 1,000 RUs for a single partition collection and 2,500 RUs for a sharded collection just for the migration.</span></span> <span data-ttu-id="2c017-143">S vyšší propustnost můžete vyhnout, omezení a migraci za kratší dobu.</span><span class="sxs-lookup"><span data-stu-id="2c017-143">With the higher throughput, you can avoid throttling and migrate in less time.</span></span> <span data-ttu-id="2c017-144">S každou hodinu fakturace v Azure Cosmos DB, můžete snížit propustnost ihned po migraci tak, aby ušetřili náklady.</span><span class="sxs-lookup"><span data-stu-id="2c017-144">With hourly billing in Azure Cosmos DB, you can reduce the throughput immediately after the migration to save costs.</span></span>

2. <span data-ttu-id="2c017-145">Přibližná RU náklady pro jednotlivý dokument zápis vypočítejte:</span><span class="sxs-lookup"><span data-stu-id="2c017-145">Calculate the approximate RU charge for a single document write:</span></span>

    <span data-ttu-id="2c017-146">a.</span><span class="sxs-lookup"><span data-stu-id="2c017-146">a.</span></span> <span data-ttu-id="2c017-147">Připojení k vaší databázi Azure Cosmos DB MongoDB z prostředí MongoDB.</span><span class="sxs-lookup"><span data-stu-id="2c017-147">Connect to your Azure Cosmos DB MongoDB database from the MongoDB Shell.</span></span> <span data-ttu-id="2c017-148">Můžete najít pokyny v [připojit MongoDB aplikace pro Azure Cosmos DB](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="2c017-148">You can find instructions in [Connect a MongoDB application to Azure Cosmos DB](connect-mongodb-account.md).</span></span>
    
    <span data-ttu-id="2c017-149">b.</span><span class="sxs-lookup"><span data-stu-id="2c017-149">b.</span></span> <span data-ttu-id="2c017-150">Spuštění příkazu insert ukázka pomocí jedné z ukázkových dokumentů z prostředí MongoDB:</span><span class="sxs-lookup"><span data-stu-id="2c017-150">Run a sample insert command by using one of your sample documents from the MongoDB Shell:</span></span>
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    <span data-ttu-id="2c017-151">c.</span><span class="sxs-lookup"><span data-stu-id="2c017-151">c.</span></span> <span data-ttu-id="2c017-152">Spustit ```db.runCommand({getLastRequestStatistics: 1})``` a obdržíte odpověď podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="2c017-152">Run ```db.runCommand({getLastRequestStatistics: 1})``` and you will receive a response like this one:</span></span>
     
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
        
    <span data-ttu-id="2c017-153">d.</span><span class="sxs-lookup"><span data-stu-id="2c017-153">d.</span></span> <span data-ttu-id="2c017-154">Poznamenejte si zdarma požadavku.</span><span class="sxs-lookup"><span data-stu-id="2c017-154">Take note of the request charge.</span></span>
    
3. <span data-ttu-id="2c017-155">Určení latence z vašeho počítače ke cloudové službě Azure Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="2c017-155">Determine the latency from your machine to the Azure Cosmos DB cloud service:</span></span>
    
    <span data-ttu-id="2c017-156">a.</span><span class="sxs-lookup"><span data-stu-id="2c017-156">a.</span></span> <span data-ttu-id="2c017-157">Zapnutí podrobného protokolování z prostředí MongoDB pomocí tohoto příkazu:```setVerboseShell(true)```</span><span class="sxs-lookup"><span data-stu-id="2c017-157">Enable verbose logging from the MongoDB Shell by using this command: ```setVerboseShell(true)```</span></span>
    
    <span data-ttu-id="2c017-158">b.</span><span class="sxs-lookup"><span data-stu-id="2c017-158">b.</span></span> <span data-ttu-id="2c017-159">Spusťte jednoduchý dotaz pro databázi: ```db.coll.find().limit(1)```.</span><span class="sxs-lookup"><span data-stu-id="2c017-159">Run a simple query against the database: ```db.coll.find().limit(1)```.</span></span> <span data-ttu-id="2c017-160">Zobrazí odpověď podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="2c017-160">You will receive a response like this one:</span></span>

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. <span data-ttu-id="2c017-161">Odeberte vloženého dokumentu před migrací zajistit, že neexistují žádné duplicitní dokumenty.</span><span class="sxs-lookup"><span data-stu-id="2c017-161">Remove the inserted document before the migration to ensure that there are no duplicate documents.</span></span> <span data-ttu-id="2c017-162">Dokumenty můžete odebrat pomocí tohoto příkazu:```db.coll.remove({})```</span><span class="sxs-lookup"><span data-stu-id="2c017-162">You can remove documents by using this command: ```db.coll.remove({})```</span></span>

5. <span data-ttu-id="2c017-163">Vypočítat přibližnou *batchSize* a *numInsertionWorkers* hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2c017-163">Calculate the approximate *batchSize* and *numInsertionWorkers* values:</span></span>

    * <span data-ttu-id="2c017-164">Pro *batchSize*, dělení celkový počet zřízení RUs podle RUs používán z vaší jednotlivý dokument zápisu v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="2c017-164">For *batchSize*, divide the total provisioned RUs by the RUs consumed from your single document write in step 3.</span></span>
    
    * <span data-ttu-id="2c017-165">Pokud počítaný *batchSize* < = 24, použijte toto číslo jako vaše *batchSize* hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2c017-165">If the calculated *batchSize* <= 24, use that number as your *batchSize* value.</span></span>
    
    * <span data-ttu-id="2c017-166">Pokud počítaný *batchSize* > 24, nastavte *batchSize* hodnotu 24.</span><span class="sxs-lookup"><span data-stu-id="2c017-166">If the calculated *batchSize* > 24, set the *batchSize* value to 24.</span></span>
    
    * <span data-ttu-id="2c017-167">Pro *numInsertionWorkers*, použijte tento rovnice: *numInsertionWorkers = (zřízené propustnosti * čekací doby v sekundách) / (velikost dávky * využité RUs pro jeden zápis)*.</span><span class="sxs-lookup"><span data-stu-id="2c017-167">For *numInsertionWorkers*, use this equation:   *numInsertionWorkers =  (provisioned throughput * latency in seconds) / (batch size * consumed RUs for a single write)*.</span></span>
        
    |<span data-ttu-id="2c017-168">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="2c017-168">Property</span></span>|<span data-ttu-id="2c017-169">Hodnota</span><span class="sxs-lookup"><span data-stu-id="2c017-169">Value</span></span>|
    |--------|-----|
    |<span data-ttu-id="2c017-170">batchSize</span><span class="sxs-lookup"><span data-stu-id="2c017-170">batchSize</span></span>| <span data-ttu-id="2c017-171">24</span><span class="sxs-lookup"><span data-stu-id="2c017-171">24</span></span> |
    |<span data-ttu-id="2c017-172">RUs zřídit</span><span class="sxs-lookup"><span data-stu-id="2c017-172">RUs provisioned</span></span> | <span data-ttu-id="2c017-173">10000</span><span class="sxs-lookup"><span data-stu-id="2c017-173">10000</span></span> |
    |<span data-ttu-id="2c017-174">Latence</span><span class="sxs-lookup"><span data-stu-id="2c017-174">Latency</span></span> | <span data-ttu-id="2c017-175">0.100 s</span><span class="sxs-lookup"><span data-stu-id="2c017-175">0.100 s</span></span> |
    |<span data-ttu-id="2c017-176">RU účtovat poplatek za 1 doc zápisu</span><span class="sxs-lookup"><span data-stu-id="2c017-176">RU charged for 1 doc write</span></span> | <span data-ttu-id="2c017-177">10 RUs</span><span class="sxs-lookup"><span data-stu-id="2c017-177">10 RUs</span></span> |
    |<span data-ttu-id="2c017-178">numInsertionWorkers</span><span class="sxs-lookup"><span data-stu-id="2c017-178">numInsertionWorkers</span></span> | <span data-ttu-id="2c017-179">?</span><span class="sxs-lookup"><span data-stu-id="2c017-179">?</span></span> |
    
    <span data-ttu-id="2c017-180">*numInsertionWorkers = (10000 RUs x 0,1 s) / (24 × 10 RUs) = 4.1666*</span><span class="sxs-lookup"><span data-stu-id="2c017-180">*numInsertionWorkers = (10000 RUs x 0.1 s) / (24 x 10 RUs) = 4.1666*</span></span>

6. <span data-ttu-id="2c017-181">Spusťte příkaz dokončení migrace:</span><span class="sxs-lookup"><span data-stu-id="2c017-181">Run the final migration command:</span></span>

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a><span data-ttu-id="2c017-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2c017-182">Next steps</span></span>

<span data-ttu-id="2c017-183">Můžete pokračovat v dalším kurzu a zjistěte, jak dotazovat MongoDB dat pomocí Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2c017-183">You can proceed to the next tutorial and learn how to query MongoDB data by using Azure Cosmos DB.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="2c017-184">Postup dotazování MongoDB dat?</span><span class="sxs-lookup"><span data-stu-id="2c017-184">How to query MongoDB data?</span></span>](../cosmos-db/tutorial-query-mongodb.md)
