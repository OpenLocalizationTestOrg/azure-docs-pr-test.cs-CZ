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
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a><span data-ttu-id="456b8-104">Azure Cosmos DB: Kurz aplikace konzoly C++ pro hello DocumentDB rozhraní API</span><span class="sxs-lookup"><span data-stu-id="456b8-104">Azure Cosmos DB: C++ console application tutorial for hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="456b8-105">.NET</span><span class="sxs-lookup"><span data-stu-id="456b8-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="456b8-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="456b8-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="456b8-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="456b8-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="456b8-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="456b8-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="456b8-109">Java</span><span class="sxs-lookup"><span data-stu-id="456b8-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="456b8-110">C++</span><span class="sxs-lookup"><span data-stu-id="456b8-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="456b8-111">Vítejte toohello C++ kurz pro hello rozhraní API služby Azure Cosmos databáze DocumentDB schválené SDK pro jazyk C++!</span><span class="sxs-lookup"><span data-stu-id="456b8-111">Welcome toohello C++ tutorial for hello Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="456b8-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB, včetně databáze v jazyce C++, a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="456b8-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="456b8-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="456b8-113">We'll cover:</span></span>

* <span data-ttu-id="456b8-114">Vytvoření a připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="456b8-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="456b8-115">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="456b8-115">Setting up your application</span></span>
* <span data-ttu-id="456b8-116">Vytvoření databáze Azure Cosmos DB v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="456b8-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="456b8-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="456b8-117">Creating a collection</span></span>
* <span data-ttu-id="456b8-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="456b8-118">Creating JSON documents</span></span>
* <span data-ttu-id="456b8-119">Dotazování na kolekci hello</span><span class="sxs-lookup"><span data-stu-id="456b8-119">Querying hello collection</span></span>
* <span data-ttu-id="456b8-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="456b8-120">Replacing a document</span></span>
* <span data-ttu-id="456b8-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="456b8-121">Deleting a document</span></span>
* <span data-ttu-id="456b8-122">Odstraňování databáze aplikace C++ Azure Cosmos DB hello</span><span class="sxs-lookup"><span data-stu-id="456b8-122">Deleting hello C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="456b8-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="456b8-123">Don't have time?</span></span> <span data-ttu-id="456b8-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="456b8-124">Don't worry!</span></span> <span data-ttu-id="456b8-125">Hello úplné řešení je k dispozici na [Githubu](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="456b8-125">hello complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="456b8-126">V tématu [získání úplného řešení hello](#GetSolution) pro rychlé pokyny.</span><span class="sxs-lookup"><span data-stu-id="456b8-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="456b8-127">Po dokončení kurzu hello C++, prosím použijte hello hlasovací tlačítka v hello dolní části této stránky toogive nám zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="456b8-127">After you've completed hello C++ tutorial, please use hello voting buttons at hello bottom of this page toogive us feedback.</span></span> 

<span data-ttu-id="456b8-128">Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích nebo [oslovení toous sem](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="456b8-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments or [reach out toous here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="456b8-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="456b8-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-c-tutorial"></a><span data-ttu-id="456b8-130">Předpoklady pro kurz hello C++</span><span class="sxs-lookup"><span data-stu-id="456b8-130">Prerequisites for hello C++ tutorial</span></span>
<span data-ttu-id="456b8-131">Přesvědčte se, že máte následující hello:</span><span class="sxs-lookup"><span data-stu-id="456b8-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="456b8-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="456b8-132">An active Azure account.</span></span> <span data-ttu-id="456b8-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="456b8-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="456b8-134">[Visual Studio](https://www.visualstudio.com/downloads/), s nainstalovanými komponentami jazyka hello C++.</span><span class="sxs-lookup"><span data-stu-id="456b8-134">[Visual Studio](https://www.visualstudio.com/downloads/), with hello C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="456b8-135">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="456b8-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="456b8-136">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456b8-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="456b8-137">Pokud již máte účet, který chcete toouse, můžete přeskočit příliš[nastavení aplikace C++](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="456b8-137">If you already have an account you want toouse, you can skip ahead too[Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="456b8-138"><a id="SetupC++"></a>Krok 2: Nastavení aplikace C++</span><span class="sxs-lookup"><span data-stu-id="456b8-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="456b8-139">Otevřete Visual Studio a pak na hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="456b8-139">Open Visual Studio, and then on hello **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="456b8-140">V hello **nový projekt** okno, v hello **nainstalovaná** podokně rozbalte **Visual C++**, klikněte na tlačítko **Win32**a pak klikněte na tlačítko  **Konzolové aplikace Win32**.</span><span class="sxs-lookup"><span data-stu-id="456b8-140">In hello **New Project** window, in hello **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="456b8-141">Hellodocumentdb hello projektu a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="456b8-141">Name hello project hellodocumentdb and then click **OK**.</span></span> 
   
    ![Snímek obrazovky hello Průvodci vytvořením nového projektu](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="456b8-143">Když se spustí hello Win32 – Průvodce aplikací, klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="456b8-143">When hello Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="456b8-144">Po vytvoření projektu hello, otevřete Správce balíčků NuGet hello tak, že kliknete pravým tlačítkem na hello **hellodocumentdb** projektu v **Průzkumníku řešení** a kliknutím na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="456b8-144">Once hello project has been created, open hello NuGet package manager by right-clicking hello **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![Snímek obrazovky zobrazující Správa balíčků NuGet v nabídce projektu hello](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="456b8-146">V hello **NuGet: hellodocumentdb** , klikněte na **Procházet**a poté vyhledejte *documentdbcpp*.</span><span class="sxs-lookup"><span data-stu-id="456b8-146">In hello **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="456b8-147">Ve výsledcích hello vyberte DocumentDbCPP, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-147">In hello results, select DocumentDbCPP, as shown in hello following screenshot.</span></span> <span data-ttu-id="456b8-148">Tento balíček nainstaluje odkazy tooC ++ REST SDK, což je závislost pro hello DocumentDbCPP.</span><span class="sxs-lookup"><span data-stu-id="456b8-148">This package installs references tooC++ REST SDK, which is a dependency for hello DocumentDbCPP.</span></span>  
   
    ![Snímek obrazovky zobrazující hello DocumentDbCpp balíček zvýrazněná](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="456b8-150">Jakmile hello balíčky nebyly přidané tooyour projektu, se všechny sady toostart zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="456b8-150">Once hello packages have been added tooyour project, we are all set toostart writing some code.</span></span>   

## <span data-ttu-id="456b8-151"><a id="Config"></a>Krok 3: Zkopírování podrobností o připojení z webu Azure Portal pro databázi Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="456b8-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="456b8-152">Zprovoznit [portál Azure](https://portal.azure.com) a procházení toohello Azure Cosmos DB databázového účtu jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="456b8-152">Bring up [Azure portal](https://portal.azure.com) and traverse toohello Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="456b8-153">Z našich fragment kódu C++ budeme potřebovat hello URI a primary key hello z portálu Azure v hello další krok tooestablish připojení.</span><span class="sxs-lookup"><span data-stu-id="456b8-153">We will need hello URI and hello primary key from Azure portal in hello next step tooestablish a connection from our C++ code snippet.</span></span> 

![Azure Cosmos DB URI a klíče v hello portálu Azure](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="456b8-155"><a id="Connect"></a>Krok 4: Připojení účtu Azure Cosmos DB tooan</span><span class="sxs-lookup"><span data-stu-id="456b8-155"><a id="Connect"></a>Step 4: Connect tooan Azure Cosmos DB account</span></span>
1. <span data-ttu-id="456b8-156">Přidejte následující hlavičky a obory názvů tooyour zdrojový kód, po hello `#include "stdafx.h"`.</span><span class="sxs-lookup"><span data-stu-id="456b8-156">Add hello following headers and namespaces tooyour source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="456b8-157">Dále přidejte následující hello kód tooyour hlavní funkce a nahraďte hello konfiguraci účtu a primární klíče toomatch nastavení Azure Cosmos DB od kroku 3.</span><span class="sxs-lookup"><span data-stu-id="456b8-157">Next add hello following code tooyour main function and replace hello account configuration and primary key toomatch your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="456b8-158">Teď, když máte hello kód tooinitialize hello documentdb klienta, Podívejme se na práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456b8-158">Now that you have hello code tooinitialize hello documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="456b8-159"><a id="CreateDBColl"></a>Krok 5: Vytvoření databáze a kolekce v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="456b8-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="456b8-160">Dříve než jsme provést tento krok, přejděte přes způsob interakce pro ty, kdo jsou nové tooAzure Cosmos DB z databáze, kolekce a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="456b8-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new tooAzure Cosmos DB.</span></span> <span data-ttu-id="456b8-161">[Databáze](documentdb-resources.md#databases) je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="456b8-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="456b8-162">A [kolekce](documentdb-resources.md#collections) je kontejner dokumentů JSON a přidružené logiky Javascriptové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and hello associated JavaScript application logic.</span></span> <span data-ttu-id="456b8-163">Můžete další informace o model hierarchické prostředků Azure Cosmos DB hello a koncepty v [Azure Cosmos DB model hierarchické prostředků a koncepty](documentdb-resources.md).</span><span class="sxs-lookup"><span data-stu-id="456b8-163">You can learn more about hello Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="456b8-164">toocreate a databázi a kolekci odpovídající přidejte následující kód toohello konec hlavní funkce hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-164">toocreate a database and a corresponding collection add hello following code toohello end of your main function.</span></span> <span data-ttu-id="456b8-165">Tím se vytvoří databázi označované jako 'FamilyRegistry' a kolekce s názvem FamilyCollection pomocí konfigurace klienta hello, deklarovaného v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using hello client configuration you declared in hello previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="456b8-166"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="456b8-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="456b8-167">[Dokumenty](documentdb-resources.md#documents) představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="456b8-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="456b8-168">Nyní můžete do služby Azure Cosmos DB vložit dokument.</span><span class="sxs-lookup"><span data-stu-id="456b8-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="456b8-169">Dokument můžete vytvořit tak, že zkopírujete následující kód do konce hello hlavní funkce hello hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-169">You can create a document by copying hello following code into hello end of hello main function.</span></span> 

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

<span data-ttu-id="456b8-170">toosummarize, tento kód vytvoří databázi Azure Cosmos databáze, kolekce a dokumenty, které můžete zadat dotaz v Průzkumníka dokumentů na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="456b8-170">toosummarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![Kurz pro C++ – Diagram ilustrující hierarchický vztah hello mezi hello účet, hello databáze, kolekce hello a dokumenty hello](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="456b8-172"><a id="QueryDB"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="456b8-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="456b8-173">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="456b8-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="456b8-174">Hello následující vzorový kód ukazuje dotaz vytvořené pomocí syntaxe SQL, který můžete spustit na hello dokumenty, že jsme vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-174">hello following sample code shows a query made using SQL syntax that you can run against hello documents we created in hello previous step.</span></span>

<span data-ttu-id="456b8-175">Funkce Hello přebírá jako argumenty hello jedinečný identifikátor nebo id prostředku pro hello databázi a kolekci hello společně s hello dokumentu klienta.</span><span class="sxs-lookup"><span data-stu-id="456b8-175">hello function takes in as arguments hello unique identifier or resource id for hello database and hello collection along with hello document client.</span></span> <span data-ttu-id="456b8-176">Vložte tento kód před hlavní funkci.</span><span class="sxs-lookup"><span data-stu-id="456b8-176">Add this code before main function.</span></span>

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

## <span data-ttu-id="456b8-177"><a id="Replace"></a>Krok 8: Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="456b8-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="456b8-178">Azure Cosmos DB podporuje nahrazování dokumentů JSON, jak je ukázáno v následujícím kódu hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in hello following code.</span></span> <span data-ttu-id="456b8-179">Přidejte tento kód po hello executesimplequery funkce.</span><span class="sxs-lookup"><span data-stu-id="456b8-179">Add this code after hello executesimplequery function.</span></span>

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

## <span data-ttu-id="456b8-180"><a id="Delete"></a>Krok 9: Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="456b8-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="456b8-181">Azure Cosmos DB podporuje odstraňování dokumentů JSON, můžete tak učinit pomocí kopírování a vkládání hello následující kód po hello replacedocument funkce.</span><span class="sxs-lookup"><span data-stu-id="456b8-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting hello following code after hello replacedocument function.</span></span> 

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

## <span data-ttu-id="456b8-182"><a id="DeleteDB"></a>Krok 10: Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="456b8-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="456b8-183">Odstraňování databáze hello vytvořili odebere hello databáze a všechny podřízené prostředky (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="456b8-183">Deleting hello created database removes hello database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="456b8-184">Zkopírujte a vložte následující fragment kódu (funkce čištění) po hello deletedocument funkce tooremove hello databáze a všechny podřízené prostředky hello hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-184">Copy and paste hello following code snippet (function cleanup) after hello deletedocument function tooremove hello database and all hello child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="456b8-185"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="456b8-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="456b8-186">Jsme právě jste přidali kód toocreate, dotaz, upravit a odstranit různými prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="456b8-186">We have now added code toocreate, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="456b8-187">Dejte nám teď propojit to si tak, že přidáte volání toothese různé funkce z našich hlavní funkce v hellodocumentdb.cpp společně s některé diagnostické zprávy.</span><span class="sxs-lookup"><span data-stu-id="456b8-187">Let us now wire this up by adding calls toothese different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="456b8-188">Můžete tak učinit nahrazením hello hlavní funkce aplikace hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="456b8-188">You can do so by replacing hello main function of your application with hello following code.</span></span> <span data-ttu-id="456b8-189">Tato zápisy přes hello account_configuration_uri a primary_key, které jste zkopírovali do kódu hello v kroku 3, takže uložit hello hodnoty v tomto řádku nebo kopírování znovu z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="456b8-189">This writes over hello account_configuration_uri and primary_key you copied into hello code in Step 3, so save that line or copy hello values in again from hello portal.</span></span> 

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

<span data-ttu-id="456b8-190">By měl nyní být schopný toobuild a spuštění kódu v sadě Visual Studio stisknutím klávesy F5 nebo případně v okno terminálu hello hledání hello aplikace a spuštěním hello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="456b8-190">You should now be able toobuild and run your code in Visual Studio by pressing F5 or alternatively in hello terminal window by locating hello application and running hello executable.</span></span> 

<span data-ttu-id="456b8-191">Měli byste vidět hello výstup počáteční aplikace.</span><span class="sxs-lookup"><span data-stu-id="456b8-191">You should see hello output of your get started app.</span></span> <span data-ttu-id="456b8-192">výstup Hello by měl odpovídat hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="456b8-192">hello output should match hello following screenshot.</span></span>

![Výstup aplikace v jazyce C++ využívající službu Azure Cosmos DB](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="456b8-194">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="456b8-194">Congratulations!</span></span> <span data-ttu-id="456b8-195">Po dokončení kurzu hello C++ a mít vaší první aplikace Azure Cosmos DB konzoly!</span><span class="sxs-lookup"><span data-stu-id="456b8-195">You've completed hello C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="456b8-196"><a id="GetSolution"></a>Získání hello dokončení C++ řešení kurzu</span><span class="sxs-lookup"><span data-stu-id="456b8-196"><a id="GetSolution"></a>Get hello complete C++ tutorial solution</span></span>
<span data-ttu-id="456b8-197">toobuild hello řešení GetStarted, které obsahuje všechny ukázky hello v tomto článku, budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="456b8-197">toobuild hello GetStarted solution that contains all hello samples in this article, you need hello following:</span></span>

* <span data-ttu-id="456b8-198">[Účet služby Azure Cosmos DB][create-account]</span><span class="sxs-lookup"><span data-stu-id="456b8-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="456b8-199">Hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) řešení, které jsou dostupné na Githubu.</span><span class="sxs-lookup"><span data-stu-id="456b8-199">hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="456b8-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="456b8-200">Next steps</span></span>
* <span data-ttu-id="456b8-201">Zjistěte, jak příliš[monitorovat účet Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="456b8-201">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="456b8-202">Spouštění dotazů na našem ukázkovou datovou sadu v hello [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="456b8-202">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="456b8-203">Další informace o programovacím modelu hello hello vývoj části hello [stránce dokumentace Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="456b8-203">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


