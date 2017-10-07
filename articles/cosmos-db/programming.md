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
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure programování na straně serveru Cosmos DB: uložené procedury, triggery databáze a UDF
Zjistěte, jak integrovat Azure Cosmos DB jazyka, spouštění transakcí jazyka JavaScript umožňuje vývojářům zápisu **uložené procedury**, **aktivační události** a **funkce (UDF)definovanéuživatelem** nativně v [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. To vám umožní toowrite databáze programu aplikační logiky, která může být dodána a provedeny přímo na oddíly úložiště databáze hello. 

Doporučujeme stáhnout spustí sledováním hello následující video, kde Andrew Liu poskytuje programovací model stručný úvod tooCosmos DB na straně serveru databáze. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

Pak se vraťte toothis článku, kde se dozvíte hello odpovědi toohello následující otázky:  

* Jak psát uložené procedury, aktivační události nebo pomocí jazyka JavaScript UDF?
* Jak Cosmos DB zaručit kyseliny?
* Jak fungují transakce v databázi Cosmos?
* Co se aktivuje před a po aktivuje a jak jeden zápis?
* Jak registrovat a spustit uloženou proceduru, aktivační události nebo UDF RESTful způsobem pomocí protokolu HTTP?
* Co Cosmos DB sady SDK jsou k dispozici toocreate a spuštění uložené procedury, triggery a UDF?

## <a name="introduction-toostored-procedure-and-udf-programming"></a>Úvod tooStored postupu a UDF programování
Tento přístup z *"JavaScript jako moderní den T-SQL"* uvolní vývojáři aplikací z hello složitosti neshody typu systému a relační objekt mapování technologie. Má také počet vnitřní výhody, které mohou být využívané toobuild bohaté aplikace:  

* **Procedurální logika:** JavaScript jako nejvyšší úrovni programovací jazyk, nabízí bohaté a známá rozhraní tooexpress obchodní logiku. Komplexní pořadí operací blíže toohello dat, můžete provést.
* **Jednotlivé transakce:** Cosmos DB záruky, které databáze operací provést v jedné uložené procedury nebo aktivační události jsou atomic. To umožní aplikaci kombinovat související operace v jedné dávkové tak, aby všechny úspěšné nebo žádná z nich úspěšné. 
* **Výkon:** hello skutečnost, že JSON je systém typů jazyka Javascript vnitřně namapované toohello a také je základní jednotkou hello úložiště v systému Cosmos DB umožňuje počet optimalizace jako opožděné materialization JSON dokumentů ve vyrovnávací paměti hello fond a přitom k dispozici na vyžádání toohello provádění kódu. Existují další výhody výkonu přidružené k přenosů obchodní logiky toohello databázi:
  
  * Dávkování – vývojáři můžou skupiny operací, jako je vložení a odesílat je hromadně. latence provoz sítě Hello náklady a hello úložiště režijní toocreate samostatné transakce jsou podstatně snížit. 
  * Předběžné kompilace – Cosmos DB překompiluje uložené procedury, triggery a uživatelem definované funkce (UDF) tooavoid náklady kompilace jazyka JavaScript pro každé vyvolání. Hello režijní náklady na vytváření kódu byte hello hello Procedurální logika je amortizovaný tooa minimální hodnota.
  * Sekvencování – mnoho operations nutné vedlejším účinkem (dále jen "aktivační události"), potenciálně zahrnuje jednu nebo více provádění operací sekundárního úložiště. Kromě zajištění dostatečného nedělitelnost, to je další původce při přesunutí toohello serveru. 
* **Zapouzdření:** uložené procedury lze použít toogroup obchodní logiky v jednom místě. To má dvě výhody:
  * Přidá abstraktní vrstvu nad hello nezpracovaná data, která umožňuje tooevolve architekty dat svých aplikací nezávisle hello data. To je zvlášť výhodné, pokud hello data bez schémat, z důvodu toohello křehká předpoklady, které může být nutné toobe zaručená do aplikace hello, pokud mají toodeal s daty přímo.  
  * Tato abstrakce umožňuje podnikům zabezpečit svá data pomocí zjednodušení hello přístup z hello skriptů.  

Hello vytváření a spouštění aktivační události databáze, uložené procedury a operátory vlastního dotazu je podporované prostřednictvím hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), a [klientskou sadu SDK](documentdb-sdk-dotnet.md) na spoustě platforem včetně .NET, Node.js a JavaScript.

Tento kurz používá hello [Node.js SDK Q lišící](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntaxi a použití uložené procedury, triggery a UDF.   

## <a name="stored-procedures"></a>Uložené procedury
### <a name="example-write-a-simple-stored-procedure"></a>Příklad: Zápis jednoduché uložené procedury
Začněme jednoduché uložené procedury, jež vrátí odpověď "Hello World".

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


Uložené procedury jsou registrované na kolekci a mohou pracovat na všechny dokumentů a příloh, které se nachází v dané kolekci. Hello následující fragment kódu ukazuje, jak Hello World hello tooregister uložené procedury s kolekcí. 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Po registraci hello uložené procedury můžeme provést proti kolekci hello a přečtěte si výsledky hello zpět na klientovi hello. 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


objekt kontextu Hello poskytuje přístup tooall operace, které lze provést na úložiště Cosmos databáze a také přístup k objektům toohello žádostí a odpovědí. V takovém případě jsme použili hello objekt tooset hello odpovědi hello odpovědi, který vám byl zaslán zpět toohello klienta. Další podrobnosti najdete v části toohello [server Azure Cosmos DB JavaScript dokumentaci k sadě SDK](http://azure.github.io/azure-documentdb-js-server/).  

Dejte nám rozbalte v tomto příkladu a přidat další databáze související funkce toohello uložené procedury. Uložené procedury můžete vytvářet, aktualizovat, číst, dotaz a odstraňovat dokumentů a příloh uvnitř hello kolekce.    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a>Příklad: Zápis uložené procedury toocreate dokumentu
Hello další fragment kódu ukazuje, jak toouse hello kontextu objektu toointeract s prostředky Cosmos DB.

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


Tuto uloženou proceduru vezme jako vstupní documentToCreate hello text dokumentu toobe, vytvořené v aktuální kolekci hello. Všechny tyto operace jsou asynchronní a závisí na zpětná volání funkce jazyka JavaScript. Funkce zpětného volání Hello má dva parametry, jeden pro objekt error hello v případě, že dojde k selhání operace hello a jeden pro hello vytvořený objekt. Uvnitř hello zpětné volání uživatelé mohou zpracování výjimky hello nebo vyvolána chyba. V případě, že není k dispozici zpětné volání a dojde k chybě, runtime Azure Cosmos DB hello vyvolá chybu.   

V předchozím příkladu hello zpětného volání hello vrátí chybu, pokud hello operace se nezdařila. Jinak nastaví hello id hello vytvořili dokument jako text hello hello odpovědi toohello klienta. Zde je, jak se spustí tuto uloženou proceduru s vstupní parametry.

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


Všimněte si, že to uložené procedury lze upravené tootake pole těla dokumentu jako vstup a vytvořit všechny ve stejné uložené hello provádění procedury místo více síti požadavky toocreate každý z nich jednotlivě. To může být tooimplement použít efektivní hromadné – Importér pro DB Cosmos (popsané později v tomto kurzu).   

Příklad Hello popsané ukázán, jak toouse uložené procedury. Později v kurzu hello obsahuje triggery a uživatelem definované funkce (UDF).

## <a name="database-program-transactions"></a>Databáze programu transakce
Transakce v typické databáze může být definováno jako posloupnost operací provést jako jednu logickou jednotku práce. Poskytuje každou transakci **ACID záruky**. Je kyselina dobře známé zkratku, který zastupuje čtyři vlastnosti - nedělitelnost, konzistence, izolace a odolnost.  

Stručně řečeno, nedělitelnost zaručuje, že všechny hello práci v transakci je považován za jednu jednotku kde buď všechny klade nebo hodnotu none. Konzistence zajišťuje, že hello data je vždy ve funkčním stavu interní napříč transakce. Izolace zaručuje, že žádné dvě transakce konfliktu s jinými – obecně, většina komerčních systémy poskytují více úrovní izolace, které lze použít podle potřeb aplikace hello. Odolnost zajistí, že všechny změny, která je potvrzena v databázi hello vždy bude k dispozici.   

V databázi Cosmos JavaScript je hostován v hello stejné místo paměti jako hello databáze. Proto požadavky v rámci uložené procedury a triggery spustit v hello stejný rozsah relace databáze. To umožňuje Cosmos DB tooguarantee kyseliny pro všechny operace, které jsou součástí jedné uložené procedury nebo aktivační událost. Vezměte v úvahu následující hello uložené procedury definice:

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

Tuto uloženou proceduru používá transakce v rámci herní aplikace tootrade položky mezi dvěma přehrávače v rámci jedné operace. Hello uložené procedury pokusy o tooread dva dokumenty, které každý odpovídající toohello player ID předaná jako argument. Pokud se oba dokumenty player, pak hello uložené procedury aktualizace podle odkládací jejich položky hello dokumenty. Pokud společně hello způsob došlo k chybám, vyvolá výjimku JavaScript, která implicitně zruší hello transakce.

Pokud hello kolekce hello uložené procedury je registrován proti je jedním oddílem kolekce a potom hello transakce je vymezená tooall hello dokumenty v rámci kolekce hello. Pokud hello kolekce rozdělena na oddíly, uložené procedury jsou spouštěny v oboru transakce hello klíče jeden oddíl. Každý uložené provádění procedury pak musí obsahovat hodnotu klíče oddílu musí být spuštěna pod odpovídající toohello oboru hello transakce. Další podrobnosti najdete v tématu [Azure Cosmos DB dělení](partition-data.md).

### <a name="commit-and-rollback"></a>Potvrzení a vrácení zpět
Transakce je úzce a nativně integrováno programovací model Cosmos DB jazyka JavaScript. Uvnitř funkce jazyka JavaScript jsou automaticky zabalená všechny operace v rámci jedné transakce. Pokud hello JavaScript se dokončí bez jakékoli výjimky, se potvrdí hello provozní toohello databáze. V důsledku toho jsou implicitní v Cosmos DB hello "BEGIN TRANSACTION" a "Potvrzení transakcí" příkazy v relačních databází.  

Pokud žádné výjimka, která je rozšířena z hello skriptu, bude Cosmos DB JavaScript runtime vrátit zpět hello celá transakce. Jak je znázorněno v hello dříve je příklad, došlo k výjimce efektivně ekvivalentní tooa "Vrácení transakce" v databázi Cosmos.

### <a name="data-consistency"></a>Konzistence dat
Uložené procedury a triggery jsou vždy provést u primární repliky hello hello Azure Cosmos DB kontejneru. Tím se zajistí, že čtení z uvnitř uložené procedury nabídka silnou konzistenci. Dotazy pomocí uživatelem definované funkce mohou být provedeny u hello primární nebo sekundární repliku, ale je zajištěno toomeet hello požadovanou úroveň konzistence výběrem příslušné repliky hello.

## <a name="bounded-execution"></a>Ohraničené provádění
Všechny operace Cosmos DB musí dokončit v povoleném zadaný server hello dobu trvání časového limitu požadavku. Toto omezení platí i funkce tooJavaScript (uložené procedury, triggery a uživatelem definované funkce). Pokud se tento časový limit operace nebude dokončena, hello transakce je vrácena zpět. Funkce jazyka JavaScript musí dokončit v časovém limitu hello nebo implementovat pokračování na základě modelu toobatch nebo pokračovat v provádění.  

V pořadí toosimplify vývoj uložené procedury a triggery toohandle časových limitů, všechny funkce v rámci objektu kolekce hello (pro vytvořit, číst, nahraďte a odstranění dokumentů a přílohy) vrátit logickou hodnotu, která představuje zda, operace dokončí. Pokud je tato hodnota false, je jako ukazatel toho, že hello časový limit je o tooexpire a hello postupu musíte zabalit až provádění.  Operace první nepřijatelného úložiště operací ve frontě předchozí toohello je zaručeno toocomplete, pokud hello uložené procedury dokončení v čase a fronty nejsou žádné další žádosti.  

Funkce jazyka JavaScript jsou také vázaný na spotřeby prostředků. Cosmos DB si vyhrazuje propustnosti na kolekci na základě velikosti hello zřízení účtu databáze. Propustnost se vyjadřuje jako normalizované jednotka procesoru, paměti a jednotek žádosti nebo RUs spotřeba vstupně-výstupní operace. Funkce jazyka JavaScript může potenciálně spotřebovávat velký počet RUs během krátké doby a může získat míra limited, pokud bude dosažen limit hello kolekce. V karanténě tooensure dostupnost primitivní databázové operace může být také prostředků náročné uložené procedury.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Příklad: Hromadný import dat do databáze programu
Dole je příklad uložené procedury, jež jsou zapsána toobulk import dokumenty do kolekce. Poznámka: jak hello uložené provádění procedury popisovače ohraničenou kontrolou hello logická hodnota návratová hodnota z createDocument a pak používá hello počet dokumentů v každé vyvolání hello uložené procedury tootrack a obnovit průběh vložit napříč dávky.

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

## <a id="trigger"></a>Aktivační události databáze
### <a name="database-pre-triggers"></a>Databáze před aktivační události
Cosmos DB poskytuje triggery, které jsou provedeny nebo aktivovány operace v dokumentu. Například můžete zadat před aktivační událost, když vytváříte dokumentu – Tato předběžná aktivační událost se spustí před vytvořením hello dokumentu. Hello následuje příklad jak před aktivační události lze použít toovalidate hello vlastností dokumentu, který je právě vytvářena:

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


A hello odpovídající registrace klienta kódu Node.js hello aktivační události:

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


Předběžné aktivační události nemůže mít žádné vstupní parametry. objekt žádosti Hello lze použít toomanipulate hello požadavek zpráva přidružená k operaci hello. Zde hello předběžné aktivační události je spuštěn s hello vytvoření dokumentu a tělo zprávy požadavku hello obsahuje toobe dokumentu hello vytvořená ve formátu JSON.   

Když jsou registrované aktivačních událostí, mohou uživatelé zadat hello operace, které můžete spustit s. Této aktivační události byl vytvořen s TriggerOperation.Create, což znamená, že hello následující není povoleno.

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Databáze po aktivační události
Po aktivační události, například před aktivační události, jsou spojena s operací na dokument a nechcete provést žádné vstupní parametry. Mohou být spuštěny **po** hello operace byla dokončena a mít přístup toohello odpověď odeslaná zpráva toohello klienta.   

Hello následující příklad ukazuje po aktivační události v akci:

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


Hello aktivační událost lze zaregistrovat, jak je znázorněno v následující ukázka hello.

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


Této aktivační události dotazuje na dokument metadat hello a aktualizuje s podrobnostmi o hello nově vytvořeného dokumentu.  

Jednou z věcí, je důležité, toonote hello **transakcí** provádění aktivační události do databáze. Cosmos. Této aktivační události po spouští jako součást hello stejné transakci jako hello vytvoření hello původního dokumentu. Proto pokud jsme vyvolat výjimku z hello po aktivační události (například pokud jsme dokument metadat nelze tooupdate hello), celá transakce hello se nezdaří a vrácena zpět. Žádné dokumentu se vytvoří a bude vrácen výjimku.  

## <a id="udf"></a>Uživatelem definované funkce
Uživatelem definované funkce (UDF) používané tooextend hello DocumentDB API SQL dotazu jazyka gramatiky a implementovat vlastní obchodní logiku. Je možné volat jedině z uvnitř dotazy. Tyto nemají přístup k objektu context toohello a jsou určené toobe použít jako jen výpočetní JavaScript. Proto UDF lze spustit na sekundárních replikách hello služby Cosmos DB.  

Hello následující ukázka vytvoří daně z příjmu toocalculate UDF podle sazby za různé příjem závorky a potom pomocí jeho uvnitř dotazu toofind všichni uživatelé, kteří placené více než 20 000 $ v daně.

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


Hello UDF lze následně použít v dotazech jako v hello následující ukázka:

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

## <a name="javascript-language-integrated-query-api"></a>JavaScript language-integrated query rozhraní API
Kromě toho tooissuing dotazy pomocí DocumentDB SQL gramatika, hello serverové SDK vám umožní tooperform optimalizované dotazy pomocí rozhraní fluent JavaScript bez znalostí SQL. dotaz JavaScript Hello rozhraní API umožňuje tooprogrammatically sestavení dotazy předáním predikátem funkce do chainable funkce volá s built-ins pole a Oblíbené knihovny JavaScript jako lodash syntaxe známé tooECMAScript5 společnosti. Dotazy jsou analyzovat pomocí hello JavaScript runtime toobe spouštěných efektivně pomocí Azure Cosmos DB indexy.

> [!NOTE]
> `__`(dvojité podtržítko) je příliš alias`getContext().getCollection()`.
> <br/>
> Jinými slovy, můžete použít `__` nebo `getContext().getCollection()` tooaccess hello API dotazu jazyka JavaScript.
> 
> 

Podporované funkce patří:

<ul>
<li>
<b>chain().... hodnota ([zpětného volání] [, možnosti])</b>
<ul>
<li>
Začíná value() zřetězené volání, které musí být ukončena.
</li>
</ul>
</li>
<li>
<b>Filtr (predicateFunction [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Filtry hello vstup pomocí funkce predikátu, která vrátí hodnotu true nebo false v pořadí toofilter a konec vstupu dokumenty do hello výsledné sady. To se chová podobně jako tooa klauzule WHERE v příkazu SQL.
</li>
</ul>
</li>
<li>
<b>mapy (transformationFunction [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Platí projekci zadané funkce transformace, který mapuje každý objekt jazyka JavaScript tooa vstupní položka nebo hodnota. To se chová podobně jako klauzuli SELECT tooa v systému SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Toto je zástupce pro mapu, která extrahuje hello hodnotu jedinou vlastnost z každé vstupní položky.
</li>
</ul>
</li>
<li>
<b>vyrovnání ([isShallow] [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Kombinuje a vyrovná pole z každé vstupní položky v jednom poli tooa. To se chová podobně jako tooSelectMany v technologii LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predikát] [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Vytvořit novou sadu dokumentů podle řazení hello dokumenty v datovém proudu vstupní dokument hello ve vzestupném pořadí pomocí hello zadané predikátu. To se chová podobně jako tooa klauzule ORDER by v systému SQL.
</li>
</ul>
</li>
<li>
<b>sortbydescending – ([predikát] [, možnosti] [, zpětného volání])</b>
<ul>
<li>
Vytvořit novou sadu dokumentů podle řazení hello dokumenty v datovém proudu vstupní dokument hello v sestupném pořadí pomocí hello zadané predikátu. To se chová podobně jako klauzule ORDER BY x DESC tooa v systému SQL.
</li>
</ul>
</li>
</ul>


Zahrnuté uvnitř predikátu nebo pro výběr funkcí, hello následující konstrukce JavaScript získat automaticky optimalizované toorun přímo v Azure Cosmos DB indexů:

* Jednoduché operátory: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literály, včetně hello objekt literálu: {}
* var, návratový

Dobrý den, který vytvoří následující JavaScript získat není optimalizovaný pro Azure Cosmos DB indexy:

* Řízení toku (například v případě, pro, zatímco)
* Volání funkcí

Další informace najdete v tématu naše [serverové JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a>Příklad: Zápis uložené procedury pomocí rozhraní API dotazu hello JavaScript
Hello následující ukázka kódu je příklad použití hello API dotazu jazyka JavaScript v kontextu hello uložené procedury. Hello uložené procedury vloží dokumentu, poskytují vstupní parametr a aktualizuje dokumentu metadat, s použitím hello `__.filter()` metoda s minSize, maxSize a totalSize podle vlastnosti velikosti hello vstupní dokument.

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

## <a name="sql-toojavascript-cheat-sheet"></a>SQL tooJavascript tahák
Hello následující tabulka uvádí různé dotazy SQL a dotazy jazyka JavaScript odpovídající hello.

Jako s dotazy SQL dokumentu vlastnosti klíče (například `doc.id`) malých a velkých písmen.

|SQL| Rozhraní API dotazu jazyka JavaScript|Popis dole|
|---|---|---|
|VYBERTE *<br>Z dokumentace| __.map(Function(DOC) { <br>&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc;<br>});|1|
|Vyberte docs.id, docs.message jako msg, docs.actions <br>Z dokumentace|__.map(Function(DOC) {<br>&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|VYBERTE *<br>Z dokumentace<br>KDE docs.id="X998_Y998"|__.Filter(Function(DOC) {<br>&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";<br>});|3|
|VYBERTE *<br>Z dokumentace<br>KDE ARRAY_CONTAINS (dokumentace. Značky, 123)|__.Filter(Function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;Vrátí x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|Vyberte docs.id, docs.message jako msg<br>Z dokumentace<br>KDE docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc.id === "X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(Function(DOC) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|SELECT VALUE – značka<br>Z dokumentace<br>Připojte značky v dokumentaci. Značky<br>Docs._ts ORDER BY|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(DOC) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí dokumentů. Značky & & Array.IsArray – (dokumentů. Značky);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Vrátí doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.Flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

Hello následujících popisech popisují každý dotaz ve výše uvedené tabulce hello.
1. Výsledkem všechny dokumenty (čísla stránek vložena s token pokračování), jako je.
2. Projekty hello id zprávy (alias toomsg) a akce z všechny dokumenty.
3. Dotazy na dokumenty s predikát hello: id = "X998_Y998".
4. Dotazy pro dokumenty, které mají značky a vlastnost Tags je pole obsahující hello hodnotu 123.
5. Dotazy na dokumenty s predikátem, id = "X998_Y998" a pak id hello projekty a zpráv (alias toomsg).
6. Filtry pro dokumenty, které mají ve vlastnosti pole značky, a seřadí výsledné dokumenty hello vlastností hello _ts časové razítko systému a pak projekty + vyrovná hello pole značky.


## <a name="runtime-support"></a>Podpora modulu runtime
[Rozhraní API jazyka DocumentDB JavaScript serveru straně](http://azure.github.io/azure-documentdb-js-server/) poskytuje podporu pro hello většinu hello běžný funkce jazyka JavaScript jako standardizované podle [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Zabezpečení
JavaScript uložené procedury a triggery jsou v izolovaném prostoru tak, aby hello důsledky jeden skript není úniku toohello jiných bez průchodu přes transakci izolace snímku hello na úrovni databáze hello. Hello prostředí runtime jsou ve fondu, ale čištění hello kontextu po každé spuštění. Proto jsou zaručit toobe bezpečné z jakékoli nezamýšleným vedlejší účinky od sebe navzájem.

### <a name="pre-compilation"></a>Předkompilace
Uložené procedury, triggery a UDF jsou implicitně předkompilovaných toohello bajtovém formátu kódu v pořadí tooavoid kompilace náklady v době hello každé vyvolání skriptu. To zajišťuje volání uložené procedury jsou rychlé a nízkým nárokům mít.

## <a name="client-sdk-support"></a>Podpora klienta SDK
V toohello přidání DocumentDB rozhraní API pro [Node.js](documentdb-sdk-node.md) má klient Azure Cosmos DB [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), a [Python SDK](documentdb-sdk-python.md) pro hello DocumentDB rozhraní API. Uložené procedury, triggery a UDF lze vytvořit a spustit některé z těchto sad SDK také používá. Následující příklad ukazuje, jak Hello toocreate a provedení uložené procedury pomocí klienta rozhraní .NET hello. Všimněte si, jak jsou typy .NET hello předaný do hello uložený postup jako JSON a čtení zpět.

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


Tento příklad ukazuje, jak toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate před aktivační události a vytvořit dokument s hello aktivační události povolen. 

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


A hello následující příklad ukazuje, jak toocreate uživatele definované funkce (UDF) a použít ho v [dotazu DocumentDB API SQL](documentdb-sql-query.md).

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

## <a name="rest-api"></a>REST API
Všechny operace Azure Cosmos databáze lze provést RESTful způsobem. Uložené procedury, triggery a uživatelem definované funkce může být registrováno v rámci kolekce pomocí HTTP POST. Hello tady je příklad toho, jak tooregister uložené procedury:

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


Hello uložené procedury je zaregistrován spuštěním požadavek POST hello URI databází nebo testdb/colls/testColl/sprocs s obsahující textu hello hello toocreate uložené procedury. Aktivační události a funkcí UDF lze registrovat podobně vydáním POST proti/aktivační události a /udfs v uvedeném pořadí.
Tato uložená procedura může poté provést po vydání požadavku POST s jeho odkazu prostředku:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


Zde je předán hello vstupní toohello uložené procedury v textu žádosti hello. Všimněte si, že vstup hello se předá jako pole JSON vstupních parametrů. Hello uložené procedury trvá hello první vstup jako dokument, který je text odpovědi. Hello odpovědi, které obdržíme, vypadá takto:

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


Na rozdíl od uložené procedury, aktivační události nelze spustit přímo. Místo toho jsou spouštěny jako součást operace v dokumentu. Toorun hello aktivační události lze zadat s žádostí pomocí hlaviček protokolu HTTP. Hello následuje požadavek toocreate dokumentu.

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


Hello předběžné aktivační událost toobe spustit s žádostí hello je zde zadaným v hlavičce x-ms-documentdb-pre-trigger-include hello. Žádné aktivační události po odpovídajícím způsobem, jsou uvedeny v záhlaví x-ms-documentdb-post-trigger-include hello. Všimněte si, že oba před a po aktivační události lze zadat pro daný požadavek.

## <a name="sample-code"></a>Ukázka kódu
Můžete najít další příklady kódu na straně serveru (včetně [hromadné odstranění](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), a [aktualizace](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) na našem [úložiště GitHub](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Má vaše společnost Super uložené procedury tooshare? Nám prosím pošlete žádost o přijetí změn! 

## <a name="next-steps"></a>Další kroky
Až budete mít jeden nebo více uložené procedury, triggery a uživatelem definované funkce vytvoření, můžete je načíst a zobrazit v portálu Azure pomocí Průzkumníku dat hello.

Můžete také zjistit hello následující odkazy a prostředky, které jsou užitečné v vaše informace o programování na straně serveru dB Azure Cosmos toolearn cesta:

* [Sady SDK služby Azure Cosmos DB](documentdb-sdk-dotnet.md)
* [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Rozšiřitelnost zabezpečení a přenosné databáze](http://dl.acm.org/citation.cfm?id=276339) 
* [Služba zaměřené na konkrétní architektura databáze](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [Hostování v systému Microsoft SQL server hello modul Runtime rozhraní .NET](http://dl.acm.org/citation.cfm?id=1007669)

