---
title: aaaC ++ kurz pro Azure Cosmos DB | Microsoft Docs
description: "Kurz jazyka C++, v rámci kterého se vytvoří databáze a konzolová aplikace v jazyce C++ pomocí služby Azure Cosmos DB se schválenou sadou SDK pro jazyk C++. Azure Cosmos DB je databázová služba na globální úrovni."
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: 2d5eeff349b7753e39936b7eb77557ad30c5830a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a>Azure Cosmos DB: Kurz aplikace konzoly C++ pro hello DocumentDB rozhraní API
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Vítejte toohello C++ kurz pro hello rozhraní API služby Azure Cosmos databáze DocumentDB schválené SDK pro jazyk C++! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB, včetně databáze v jazyce C++, a dotazuje se na ně.

Budeme se zabývat těmito tématy:

* Vytvoření a připojení účtu Azure Cosmos DB tooan
* Nastavení aplikace
* Vytvoření databáze Azure Cosmos DB v jazyce C++
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci hello
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstraňování databáze aplikace C++ Azure Cosmos DB hello

Nemáte čas? Nevadí! Hello úplné řešení je k dispozici na [Githubu](https://github.com/stalker314314/DocumentDBCpp). V tématu [získání úplného řešení hello](#GetSolution) pro rychlé pokyny.

Po dokončení kurzu hello C++, prosím použijte hello hlasovací tlačítka v hello dolní části této stránky toogive nám zpětnou vazbu. 

Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích nebo [oslovení toous sem](https://www.research.net/r/8BKRJ3Z). 

Můžeme začít!

## <a name="prerequisites-for-hello-c-tutorial"></a>Předpoklady pro kurz hello C++
Přesvědčte se, že máte následující hello:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/), s nainstalovanými komponentami jazyka hello C++.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Krok 1: Vytvoření účtu služby Azure Cosmos DB
Vytvořme účet služby Azure Cosmos DB. Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení aplikace C++](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupC++"></a>Krok 2: Nastavení aplikace C++
1. Otevřete Visual Studio a pak na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**. 
2. V hello **nový projekt** okno, v hello **nainstalovaná** podokně rozbalte **Visual C++**, klikněte na tlačítko **Win32**a pak klikněte na tlačítko  **Konzolové aplikace Win32**. Hellodocumentdb hello projektu a potom klikněte na **OK**. 
   
    ![Snímek obrazovky hello Průvodci vytvořením nového projektu](media/documentdb-cpp-get-started/hello.png)
3. Když se spustí hello Win32 – Průvodce aplikací, klikněte na tlačítko **Dokončit**.
4. Po vytvoření projektu hello, otevřete Správce balíčků NuGet hello tak, že kliknete pravým tlačítkem na hello **hellodocumentdb** projektu v **Průzkumníku řešení** a kliknutím na **spravovat balíčky NuGet**. 
   
    ![Snímek obrazovky zobrazující Správa balíčků NuGet v nabídce projektu hello](media/documentdb-cpp-get-started/nuget.png)
5. V hello **NuGet: hellodocumentdb** , klikněte na **Procházet**a poté vyhledejte *documentdbcpp*. Ve výsledcích hello vyberte DocumentDbCPP, jak ukazuje následující snímek obrazovky hello. Tento balíček nainstaluje odkazy tooC ++ REST SDK, což je závislost pro hello DocumentDbCPP.  
   
    ![Snímek obrazovky zobrazující hello DocumentDbCpp balíček zvýrazněná](media/documentdb-cpp-get-started/cpp.png)
   
    Jakmile hello balíčky nebyly přidané tooyour projektu, se všechny sady toostart zápis nějaký kód.   

## <a id="Config"></a>Krok 3: Zkopírování podrobností o připojení z webu Azure Portal pro databázi Azure Cosmos DB
Zprovoznit [portál Azure](https://portal.azure.com) a procházení toohello Azure Cosmos DB databázového účtu jste vytvořili. Z našich fragment kódu C++ budeme potřebovat hello URI a primary key hello z portálu Azure v hello další krok tooestablish připojení. 

![Azure Cosmos DB URI a klíče v hello portálu Azure](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Krok 4: Připojení účtu Azure Cosmos DB tooan
1. Přidejte následující hlavičky a obory názvů tooyour zdrojový kód, po hello `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Dále přidejte následující hello kód tooyour hlavní funkce a nahraďte hello konfiguraci účtu a primární klíče toomatch nastavení Azure Cosmos DB od kroku 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Teď, když máte hello kód tooinitialize hello documentdb klienta, Podívejme se na práci s prostředky Azure Cosmos DB.

## <a id="CreateDBColl"></a>Krok 5: Vytvoření databáze a kolekce v jazyce C++
Dříve než jsme provést tento krok, přejděte přes způsob interakce pro ty, kdo jsou nové tooAzure Cosmos DB z databáze, kolekce a dokumenty. [Databáze](documentdb-resources.md#databases) je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi. A [kolekce](documentdb-resources.md#collections) je kontejner dokumentů JSON a přidružené logiky Javascriptové aplikace hello. Můžete další informace o model hierarchické prostředků Azure Cosmos DB hello a koncepty v [Azure Cosmos DB model hierarchické prostředků a koncepty](documentdb-resources.md).

toocreate a databázi a kolekci odpovídající přidejte následující kód toohello konec hlavní funkce hello. Tím se vytvoří databázi označované jako 'FamilyRegistry' a kolekce s názvem FamilyCollection pomocí konfigurace klienta hello, deklarovaného v předchozím kroku hello.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Krok 6: Vytvoření dokumentu
[Dokumenty](documentdb-resources.md#documents) představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete do služby Azure Cosmos DB vložit dokument. Dokument můžete vytvořit tak, že zkopírujete následující kód do konce hello hlavní funkce hello hello. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

toosummarize, tento kód vytvoří databázi Azure Cosmos databáze, kolekce a dokumenty, které můžete zadat dotaz v Průzkumníka dokumentů na portálu Azure. 

![Kurz pro C++ – Diagram ilustrující hierarchický vztah hello mezi hello účet, hello databáze, kolekce hello a dokumenty hello](media/documentdb-cpp-get-started/docs.png)

## <a id="QueryDB"></a>Krok 7: Dotazování prostředků Azure Cosmos DB
Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí. Hello následující vzorový kód ukazuje dotaz vytvořené pomocí syntaxe SQL, který můžete spustit na hello dokumenty, že jsme vytvořili v předchozím kroku hello.

Funkce Hello přebírá jako argumenty hello jedinečný identifikátor nebo id prostředku pro hello databázi a kolekci hello společně s hello dokumentu klienta. Vložte tento kód před hlavní funkci.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>Krok 8: Nahrazení dokumentu
Azure Cosmos DB podporuje nahrazování dokumentů JSON, jak je ukázáno v následujícím kódu hello. Přidejte tento kód po hello executesimplequery funkce.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>Krok 9: Odstranění dokumentu
Azure Cosmos DB podporuje odstraňování dokumentů JSON, můžete tak učinit pomocí kopírování a vkládání hello následující kód po hello replacedocument funkce. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>Krok 10: Odstranění databáze
Odstraňování databáze hello vytvořili odebere hello databáze a všechny podřízené prostředky (kolekcí, dokumentů atd.).

Zkopírujte a vložte následující fragment kódu (funkce čištění) po hello deletedocument funkce tooremove hello databáze a všechny podřízené prostředky hello hello.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#
Jsme právě jste přidali kód toocreate, dotaz, upravit a odstranit různými prostředky Azure Cosmos DB.  Dejte nám teď propojit to si tak, že přidáte volání toothese různé funkce z našich hlavní funkce v hellodocumentdb.cpp společně s některé diagnostické zprávy.

Můžete tak učinit nahrazením hello hlavní funkce aplikace hello následující kód. Tato zápisy přes hello account_configuration_uri a primary_key, které jste zkopírovali do kódu hello v kroku 3, takže uložit hello hodnoty v tomto řádku nebo kopírování znovu z portálu hello. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

By měl nyní být schopný toobuild a spuštění kódu v sadě Visual Studio stisknutím klávesy F5 nebo případně v okno terminálu hello hledání hello aplikace a spuštěním hello spustitelný soubor. 

Měli byste vidět hello výstup počáteční aplikace. výstup Hello by měl odpovídat hello následující snímek obrazovky.

![Výstup aplikace v jazyce C++ využívající službu Azure Cosmos DB](media/documentdb-cpp-get-started/console.png)

Blahopřejeme! Po dokončení kurzu hello C++ a mít vaší první aplikace Azure Cosmos DB konzoly!

## <a id="GetSolution"></a>Získání hello dokončení C++ řešení kurzu
toobuild hello řešení GetStarted, které obsahuje všechny ukázky hello v tomto článku, budete potřebovat následující hello:

* [Účet služby Azure Cosmos DB][create-account]
* Hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) řešení, které jsou dostupné na Githubu.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).
* Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).
* Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account


