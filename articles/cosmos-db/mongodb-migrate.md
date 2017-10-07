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
# <a name="azure-cosmos-db-import-mongodb-data"></a>Azure Cosmos DB: Data pro Import MongoDB 

toomigrate data z MongoDB tooan účet Azure Cosmos DB pro použití s hello rozhraní API pro MongoDB, musíte:

* Stáhněte si buď *mongoimport.exe* nebo *mongorestore.exe* z hello [MongoDB Download Center](https://www.mongodb.com/download-center).
* Získat vaše [rozhraní API pro MongoDB připojovací řetězec](connect-mongodb-account.md).

Pokud importujete data z MongoDB a plán toouse její hello Azure Cosmos DB, měli byste použít hello [nástroj pro migraci dat](import-data.md) tooimport data.

Tento kurz se zabývá hello následující úlohy:

> [!div class="checklist"]
> * Získávání připojovacího řetězce
> * Import dat MongoDB pomocí mongoimport
> * Import dat MongoDB pomocí mongorestore

## <a name="prerequisites"></a>Požadavky

* Zvýšit propustnost: délka hello migraci dat závisí na množství hello propustnost nastavení pro kolekce. Být, že tooincrease hello propustnost pro větší dat migrace. Po dokončení migrace hello, snížit náklady toosave propustnost hello. Další informace o zvýšení propustnosti v hello [portál Azure](https://portal.azure.com), najdete v části [úrovně výkonu a cenové úrovně v Azure Cosmos DB](performance-levels.md).

* Povolte protokol SSL: Azure Cosmos DB má vysokými nároky na zabezpečení a standardy. Pokud při práci s vaším účtem, být zda tooenable SSL. Zahrnout Hello postupy v hello zbývající části článku hello jak tooenable SSL pro mongoimport a mongorestore.

## <a name="find-your-connection-string-information-host-port-username-and-password"></a>Najít vaše informace o připojovacím řetězci (hostitelů, port, uživatelské jméno a heslo)

1. V hello [portál Azure](https://portal.azure.com), v levém podokně text hello, klikněte na hello **Azure Cosmos DB** položku.
2. V hello **odběry** podokně, vyberte název účtu.
3. V hello **připojovací řetězec** okně klikněte na tlačítko **připojovací řetězec**.  
pravé podokno obsahuje všechny informace hello, je nutné, aby toosuccessfully Hello připojit tooyour účet.

    ![Okno řetězec připojení](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="import-data-toohello-api-for-mongodb-by-using-mongoimport"></a>Importovat data toohello rozhraní API pro MongoDB pomocí mongoimport

tooimport data tooyour Azure Cosmos DB účet, použijte hello následující šablony. Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* hello hodnotami, které jsou specifické tooyour účet.  

Šablona:

    mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file C:\sample.json

Příklad:  

    mongoimport.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file C:\Users\anhoh\Desktop\*.json

## <a name="import-data-toohello-api-for-mongodb-by-using-mongorestore"></a>Importovat data toohello rozhraní API pro MongoDB pomocí mongorestore

toorestore data tooyour rozhraní API pro MongoDB účet, použijte následující importu hello tooexecute šablony hello. Vyplňte *hostitele*, *uživatelské jméno*, a *heslo* hello hodnotami, které jsou specifické tooyour účet.

Šablona:

    mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>

Příklad:

    mongorestore.exe --host anhoh-host.documents.azure.com:10255 -u anhoh-host -p tkvaVkp4Nnaoirnouenrgisuner2435qwefBH0z256Na24frio34LNQasfaefarfernoimczciqisAXw== --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
    
## <a name="guide-for-a-successful-migration"></a>Příručka pro úspěšné migrace

1. Předem vytvořit a škálovat vaše kolekce:
        
    * Ve výchozím nastavení zřídí Azure Cosmos DB nové kolekce MongoDB s 1 000 jednotek žádosti (ruština). Před zahájením migrace hello pomocí mongoimport, mongorestore nebo mongomirror, předem vytvořit všechny kolekce z hello [portál Azure](https://portal.azure.com) nebo z MongoDB ovladače a nástroje. Pokud vaše kolekce je větší než 10 GB, ujistěte se, že toocreate [horizontálně dělené/oddíly kolekce](partition-data.md) s klíčem odpovídající horizontálního oddílu.

    * Z hello [portál Azure](https://portal.azure.com), zvýšit propustnost vaší kolekce z 1 000 RUs pro kolekce tvořené jedním oddílem a odpovídající 2500 RUs pro horizontálně dělenou kolekci jenom pro migraci hello. S vyšší propustnost hello můžete vyhnout, omezení a migraci za kratší dobu. S každou hodinu fakturace v Azure Cosmos DB, můžete snížit propustnost hello ihned po hello migrace toosave náklady.

2. Vypočítejte hello přibližnou RU zdarma pro jednotlivý dokument zápisu:

    a. Připojte databázi Azure Cosmos DB MongoDB tooyour z hello prostředí MongoDB. Můžete najít pokyny v [připojit MongoDB aplikace tooAzure Cosmos DB](connect-mongodb-account.md).
    
    b. Spuštění příkazu insert ukázka pomocí jedné z ukázkových dokumentů z hello MongoDB prostředí:
    
        ```db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })```
        
    c. Spustit ```db.runCommand({getLastRequestStatistics: 1})``` a obdržíte odpověď podobné následujícímu:
     
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
        
    d. Poznamenejte si hello požadavek poplatků.
    
3. Určení hello latence z vašeho počítače toohello Azure Cosmos DB cloudové služby:
    
    a. Zapnutí podrobného protokolování z hello MongoDB prostředí pomocí tohoto příkazu:```setVerboseShell(true)```
    
    b. Spusťte jednoduchý dotaz na databázi hello: ```db.coll.find().limit(1)```. Zobrazí odpověď podobné následujícímu:

        ```
        Fetched 1 record(s) in 100(ms)
        ```
        
4. Odeberte hello vložit dokument před tooensure hello migrace, nejsou žádné duplicitní dokumenty. Dokumenty můžete odebrat pomocí tohoto příkazu:```db.coll.remove({})```

5. Vypočítat hello přibližnou *batchSize* a *numInsertionWorkers* hodnoty:

    * Pro *batchSize*, celkem hello dělení zřízený RUs podle hello RUs používán z vaší jednotlivý dokument zápisu v kroku 3.
    
    * Pokud hello vypočítat *batchSize* < = 24, použijte toto číslo jako vaše *batchSize* hodnotu.
    
    * Pokud hello vypočítat *batchSize* > 24, sada hello *batchSize* too24 hodnotu.
    
    * Pro *numInsertionWorkers*, použijte tento rovnice: *numInsertionWorkers = (zřízené propustnosti * čekací doby v sekundách) / (velikost dávky * využité RUs pro jeden zápis)*.
        
    |Vlastnost|Hodnota|
    |--------|-----|
    |batchSize| 24 |
    |RUs zřídit | 10000 |
    |Latence | 0.100 s |
    |RU účtovat poplatek za 1 doc zápisu | 10 RUs |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RUs x 0,1 s) / (24 × 10 RUs) = 4.1666*

6. Spusťte příkaz hello dokončení migrace:

   ```
   mongoimport.exe --host anhoh-mongodb.documents.azure.com:10255 -u anhoh-mongodb -p wzRJCyjtLPNuhm53yTwaefawuiefhbauwebhfuabweifbiauweb2YVdl2ZFNZNv8IU89LqFVm5U0bw== --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```

## <a name="next-steps"></a>Další kroky

Můžete pokračovat dalším kurzu toohello a zjistěte, jak tooquery MongoDB dat pomocí Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Jak tooquery MongoDB dat?](../cosmos-db/tutorial-query-mongodb.md)
