---
title: "kurz aaaNode.js pro hello DocumentDB rozhraní API pro Azure Cosmos DB | Microsoft Docs"
description: "Kurz Node.js, která vytvoří Cosmos DB s hello DocumentDB rozhraní API."
keywords: "kurz node.js, databáze uzlů"
services: cosmos-db
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: article
ms.date: 08/14/2017
ms.author: anhoh
ms.openlocfilehash: fce244c6a5f321608e82ca51a2c987e84b98bffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a>Kurz k Node.js: použití hello DocumentDB rozhraní API v Azure Cosmos DB toocreate konzolovou aplikaci Node.js
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Vítejte v kurzu Node.js toohello pro hello Azure Cosmos DB Node.js SDK! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB a dotazuje se na ně.

Budeme se zabývat těmito tématy:

* Vytvoření a připojení účtu Azure Cosmos DB tooan
* Nastavení aplikace
* Vytvoření databáze Node
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci hello
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstranění databáze node hello

Nemáte čas? Nevadí! Hello úplné řešení je k dispozici na [Githubu](https://github.com/Azure-Samples/documentdb-node-getting-started). V tématu [získání úplného řešení hello](#GetSolution) pro rychlé pokyny.

Po dokončení kurzu Node.js hello, prosím použijte hello hlasovací tlačítka v hello horní a dolní části této stránky toogive nám zpětnou vazbu. Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.

Můžeme začít!

## <a name="prerequisites-for-hello-nodejs-tutorial"></a>Předpoklady pro kurz k Node.js hello
Přesvědčte se, že máte následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
    * Alternativně můžete použít hello [emulátoru DB Cosmos Azure](local-emulator.md) pro účely tohoto kurzu.
* [Node.js](https://nodejs.org/) verze 0.10.29 nebo vyšší

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Krok 1: Vytvoření účtu služby Azure Cosmos DB
Vytvořme účet služby Azure Cosmos DB. Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení aplikace Node.js](#SetupNode). Pokud používáte hello emulátoru DB Cosmos Azure, postupujte podle kroků hello v [emulátoru DB Cosmos Azure](local-emulator.md) toosetup hello emulátoru a přeskočit příliš[nastavení aplikace Node.js](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Krok 2: Nastavení aplikace Node.js
1. Otevřete svůj oblíbený terminál.
2. Vyhledejte hello složku nebo adresář, kam chcete toosave aplikace Node.js.
3. Vytvořte dva prázdné Javascriptové soubory s hello následující příkazy:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux nebo OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Nainstalujte pomocí npm modul documentdb hello. Hello použijte následující příkaz:
   * ```npm install documentdb --save```

Výborně! Teď když jste dokončili nastavování, napišme nějaký kód.

## <a id="Config"></a>Krok 3: Nastavení konfigurací aplikace
Ve svém oblíbeném textovém editoru otevřete ```config.js```.

Potom, kopírování a vložení hello fragment kódu níže a nastavte vlastnosti ```config.endpoint``` a ```config.primaryKey``` tooyour Azure Cosmos DB koncový bod uri a primary key. Obě tyto konfigurace lze nalézt v hello [portál Azure](https://portal.azure.com).

![Kurz k Node.js – snímek obrazovky portálu Azure znázorňující účet Azure Cosmos DB s AKTIVNÍM centrem hello hello zvýrazněným tlačítkem klíče hello v okně účtu Azure Cosmos DB hello a hello identifikátor URI, primární a sekundární klíč zvýrazněnými hodnotami v hello Okna klíče – databáze Node][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Zkopírujte a vložte hello ```database id```, ```collection id```, a ```JSON documents``` tooyour ```config``` objekt níže kde nastavíte vaše ```config.endpoint``` a ```config.authKey``` vlastnosti. Pokud již máte data, které byste chtěli toostore v databázi, můžete použít Azure Cosmos DB [nástroj pro migraci dat](import-data.md) místo přidávání definic dokumentů hello.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART tooYOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Hello databáze, kolekce a definice dokumentů bude fungovat jako vaše Azure DB Cosmos ```database id```, ```collection id```a data dokumentů.

Nakonec exportujte vaše ```config``` objektu, takže můžete použít v rámci hello ```app.js``` souboru.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <a id="Connect"></a>Krok 4: Připojení účtu Azure Cosmos DB tooan
Otevřete prázdný ```app.js``` soubor v textovém editoru hello. Zkopírujte a vložte kód hello pod tooimport hello ```documentdb``` modulu a nově vytvořený ```config``` modulu.

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Zkopírujte a vložte hello kód toouse hello uložili dřív ```config.endpoint``` a ```config.primaryKey``` toocreate nové instance DocumentClient.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Teď, když máte hello kód tooinitialize hello Azure Cosmos DB klienta, Podívejme se na práci s prostředky Azure Cosmos DB.

## <a name="step-5-create-a-node-database"></a>Krok 5: Vytvoření databáze Node
Zkopírujte a vložte kód hello pod tooset hello stav protokolu HTTP pro nebyl nalezen, url databáze hello a adresa url kolekce hello. Tyto adresy URL se, jak bude klient Azure Cosmos DB hello najít hello správné databáze a kolekce.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

A [databáze](documentdb-resources.md#databases) lze vytvořit pomocí hello [metodu createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy. Databáze je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi hello.

Zkopírujte a vložte hello **getDatabase** funkci pro vytvoření nové databáze v souboru app.js hello s hello ```id``` zadaný v hello ```config``` objektu. Hello funkce zkontroluje, jestli hello databáze s hello stejné ```FamilyRegistry``` id již neexistuje. Pokud ano, místo vytvoření nové databáze se vrátí databáze s tímto ID.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART tooYOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Zkopírujte a vložte následující kód hello, kde nastavíte hello **getDatabase** funkce tooadd hello pomocné funkce **ukončete** ta vypíše hello závěrečnou zprávu a volání hello příliš**getDatabase** funkce.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key tooexit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste vytvořili databázi Azure Cosmos DB.

## <a id="CreateColl"></a>Krok 6: Vytvoření kolekce
> [!WARNING]
> **CreateDocumentCollectionAsync** vytvoří novou kolekci, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

A [kolekce](documentdb-resources.md#collections) lze vytvořit pomocí hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.

Zkopírujte a vložte hello **getCollection** funkce pod hello **getDatabase** fungovat v toocreate souboru app.js hello nové kolekce s hello ```id``` zadaný v hello ```config```objektu. Opět zkontrolujeme, toomake opravdu kolekce se hello stejné ```FamilyCollection``` id již neexistuje. Pokud existuje, místo vytvoření nové kolekce se vrátí kolekce s tímto ID.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Zkopírujte a vložte kód hello pod hello volání příliš**getDatabase** tooexecute hello **getCollection** funkce.

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste vytvořili kolekci Azure Cosmos DB.

## <a id="CreateDoc"></a>Krok 7: Vytvoření dokumentu
A [dokumentu](documentdb-resources.md#documents) lze vytvořit pomocí hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) funkce hello **DocumentClient** třídy. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete do služby Azure Cosmos DB vložit dokument.

Zkopírujte a vložte hello **getFamilyDocument** funkce pod hello **getCollection** funkce pro vytváření hello dokumenty obsahující data JSON hello uložené v hello ```config``` objektu. Opět zkontrolujeme toomake opravdu dokument s hello stejným id už neexistuje.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód hello pod hello volání příliš**getCollection** tooexecute hello **getFamilyDocument** funkce.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste vytvořili dokument Azure Cosmos DB.

![Kurz k Node.js – Diagram ilustrující hierarchický vztah hello mezi hello účet, hello databáze, kolekce hello a dokumenty hello – uzlu databáze](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>Krok 8: Dotazování prostředků Azure Cosmos DB
Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí. Hello následující vzorový kód ukazuje dotaz, který můžete spustit na hello dokumenty v kolekci.

Zkopírujte a vložte hello **queryCollection** funkce pod hello **getFamilyDocument** funkce v souboru app.js hello. Azure Cosmos DB podporuje dotazy podobné jazyku SQL, jak je uvedeno níže. Další informace o vytváření komplexních dotazů, podívejte se na hello [Query Playground](https://www.documentdb.com/sql/demo) a hello [dokumentaci k dotazům](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Hello následující diagram znázorňuje, jak se hello Azure Cosmos DB SQL dotazu, že se volá syntaxe proti kolekci hello jste vytvořili.

![Kurz k Node.js – Diagram ilustrující hello obor a význam dotazu hello – uzlu databáze](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Hello [FROM](documentdb-sql-query.md#FromClause) – klíčové slovo je volitelný hello dotazu, protože Azure Cosmos DB dotazy jsou již vymezená tooa jedinou kolekci. Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte. Azure Cosmos DB odvodí, že rodiny, root nebo název proměnné hello jste vybrali, odkaz na aktuální kolekci hello ve výchozím nastavení.

Zkopírujte a vložte kód hello pod hello volání příliš**getFamilyDocument** tooexecute hello **queryCollection** funkce.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste provedli dotaz na dokumenty Azure Cosmos DB.

## <a id="ReplaceDocument"></a>Krok 9: Nahrazení dokumentu
Azure Cosmos DB podporuje nahrazování dokumentů JSON.

Zkopírujte a vložte hello **replaceFamilyDocument** funkce pod hello **queryCollection** funkce v souboru app.js hello.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód hello pod hello volání příliš**queryCollection** tooexecute hello **replaceDocument** funkce. Navíc přidat hello kódu toocall **queryCollection** znovu tooverify hello dokument úspěšně změnil.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste nahradili dokument Azure Cosmos DB.

## <a id="DeleteDocument"></a>Krok 10: Odstranění dokumentu
Azure Cosmos DB podporuje odstraňování dokumentů JSON.

Zkopírujte a vložte hello **deleteFamilyDocument** funkce pod hello **replaceFamilyDocument** funkce.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Zkopírujte a vložte kód hello pod hello volání toohello druhý **queryCollection** tooexecute hello **deleteDocument** funkce.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Blahopřejeme! Úspěšně jste odstranili dokument Azure Cosmos DB.

## <a id="DeleteDatabase"></a>Krok 11: Odstranění databáze Node hello
Odstraňování hello vytvořené databáze dojde k odebrání hello databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

Zkopírujte a vložte hello **čištění** funkce pod hello **deleteFamilyDocument** funkce tooremove hello databázi a všechny podřízené prostředky hello.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Zkopírujte a vložte kód hello pod hello volání příliš**deleteFamilyDocument** tooexecute hello **čištění** funkce.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>Krok 12: Spuštění celé aplikace Node.js
Jako celek by pořadí hello volání funkcí by měl vypadat takto:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

V terminálu vyhledejte vaší ```app.js``` souboru a spusťte příkaz hello:```node app.js```

Měli byste vidět hello výstup počáteční aplikace. výstup Hello by měl odpovídat ukázkovému textu hello níže.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key tooexit

Blahopřejeme! Jste vytvořili jste jste dokončili kurz k Node.js hello a máte vaší první aplikace Azure Cosmos DB konzoly!

## <a id="GetSolution"></a>Získat hello kompletní řešení kurzu k Node.js
Pokud nebyly čas toocomplete hello kroky v tomto kurzu, nebo jenom toodownload hello kódu můžete získat z [Githubu](https://github.com/Azure-Samples/documentdb-node-getting-started).

toorun hello řešení GetStarted, které obsahuje všechny ukázky hello v tomto článku, budete potřebovat následující hello:

* [Účet služby Azure Cosmos DB][create-account]
* Hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) řešení, které jsou dostupné na Githubu.

Nainstalujte hello **documentdb** pomocí npm modul. Hello použijte následující příkaz:

* ```npm install documentdb --save```

Vedle hello ```config.js``` souboru, hodnoty config.endpoint a config.authKey hello aktualizace, jak je popsáno v [krok 3: nastavení konfigurací aplikace](#Config). 

V terminálu vyhledejte vaše ```app.js``` souboru a spusťte příkaz hello: ```node app.js```.

A to je vše, stačí sestavit a máte hotovo. 

## <a name="next-steps"></a>Další kroky
* Hledáte složitější ukázku Node.js? Viz [Sestavení webové aplikace Node.js využívající službu Azure Cosmos DB](documentdb-nodejs-application.md).
* Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).
* Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
* Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
