---
title: "Kurz jazyka C++ pro službu Azure Cosmos DB | Dokumentace Microsoftu"
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
ms.openlocfilehash: 7d8de973765830ccd7983182bc1bb19b1e01e505
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-documentdb-api"></a><span data-ttu-id="a8a91-104">Azure Cosmos DB: Kurz konzolové aplikace v jazyce C++ pro rozhraní DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="a8a91-104">Azure Cosmos DB: C++ console application tutorial for the DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a8a91-105">.NET</span><span class="sxs-lookup"><span data-stu-id="a8a91-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="a8a91-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8a91-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="a8a91-107">Node.js pro MongoDB</span><span class="sxs-lookup"><span data-stu-id="a8a91-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="a8a91-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="a8a91-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="a8a91-109">Java</span><span class="sxs-lookup"><span data-stu-id="a8a91-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="a8a91-110">C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="a8a91-111">Vítejte v kurzu jazyka C++ pro rozhraní Azure Cosmos DB DocumentDB API se schválenou sadou SDK pro jazyk C++!</span><span class="sxs-lookup"><span data-stu-id="a8a91-111">Welcome to the C++ tutorial for the Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="a8a91-112">Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky Azure Cosmos DB, včetně databáze v jazyce C++, a dotazuje se na ně.</span><span class="sxs-lookup"><span data-stu-id="a8a91-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="a8a91-113">Budeme se zabývat těmito tématy:</span><span class="sxs-lookup"><span data-stu-id="a8a91-113">We'll cover:</span></span>

* <span data-ttu-id="a8a91-114">Vytvoření účtu služby Azure Cosmos DB a připojení k němu</span><span class="sxs-lookup"><span data-stu-id="a8a91-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="a8a91-115">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="a8a91-115">Setting up your application</span></span>
* <span data-ttu-id="a8a91-116">Vytvoření databáze Azure Cosmos DB v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="a8a91-117">Vytvoření kolekce</span><span class="sxs-lookup"><span data-stu-id="a8a91-117">Creating a collection</span></span>
* <span data-ttu-id="a8a91-118">Vytvoření dokumentů JSON</span><span class="sxs-lookup"><span data-stu-id="a8a91-118">Creating JSON documents</span></span>
* <span data-ttu-id="a8a91-119">Dotazování na kolekci</span><span class="sxs-lookup"><span data-stu-id="a8a91-119">Querying the collection</span></span>
* <span data-ttu-id="a8a91-120">Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="a8a91-120">Replacing a document</span></span>
* <span data-ttu-id="a8a91-121">Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="a8a91-121">Deleting a document</span></span>
* <span data-ttu-id="a8a91-122">Odstranění databáze Azure Cosmos DB v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-122">Deleting the C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="a8a91-123">Nemáte čas?</span><span class="sxs-lookup"><span data-stu-id="a8a91-123">Don't have time?</span></span> <span data-ttu-id="a8a91-124">Nevadí!</span><span class="sxs-lookup"><span data-stu-id="a8a91-124">Don't worry!</span></span> <span data-ttu-id="a8a91-125">Úplné řešení je k dispozici na [GitHubu](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="a8a91-125">The complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="a8a91-126">Rychlé pokyny najdete v části [Získání úplného řešení](#GetSolution).</span><span class="sxs-lookup"><span data-stu-id="a8a91-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="a8a91-127">Až tento kurz k C++ dokončíte, sdělte nám prosím svůj názor pomocí hlasovacích tlačítek v dolní části této stránky.</span><span class="sxs-lookup"><span data-stu-id="a8a91-127">After you've completed the C++ tutorial, please use the voting buttons at the bottom of this page to give us feedback.</span></span> 

<span data-ttu-id="a8a91-128">Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích nebo [se na nás obraťte zde](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="a8a91-128">If you'd like us to contact you directly, feel free to include your email address in your comments or [reach out to us here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="a8a91-129">Můžeme začít!</span><span class="sxs-lookup"><span data-stu-id="a8a91-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-c-tutorial"></a><span data-ttu-id="a8a91-130">Předpoklady pro kurz k C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-130">Prerequisites for the C++ tutorial</span></span>
<span data-ttu-id="a8a91-131">Ujistěte se prosím, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="a8a91-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="a8a91-132">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="a8a91-132">An active Azure account.</span></span> <span data-ttu-id="a8a91-133">Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8a91-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a8a91-134">[Visual Studio](https://www.visualstudio.com/downloads/) s nainstalovanými komponentami jazyka C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-134">[Visual Studio](https://www.visualstudio.com/downloads/), with the C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="a8a91-135">Krok 1: Vytvoření účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a8a91-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="a8a91-136">Vytvořme účet služby Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a8a91-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="a8a91-137">Pokud už máte účet, který chcete použít, můžete přeskočit na [Nastavení aplikace C++](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="a8a91-137">If you already have an account you want to use, you can skip ahead to [Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="a8a91-138"><a id="SetupC++"></a>Krok 2: Nastavení aplikace C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="a8a91-139">Otevřete Visual Studio a v nabídce **Soubor** klikněte na **Nový** a potom na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="a8a91-139">Open Visual Studio, and then on the **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="a8a91-140">V okně **Nový projekt** v podokně **Nainstalováno** rozbalte nabídku **Visual C++**, klikněte na **Win32** a potom klikněte na **Konzolová aplikace Win32**.</span><span class="sxs-lookup"><span data-stu-id="a8a91-140">In the **New Project** window, in the **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="a8a91-141">Jako název dokumentu zadejte hellodocumentdb a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8a91-141">Name the project hellodocumentdb and then click **OK**.</span></span> 
   
    ![Snímek obrazovky s průvodcem novým projektem](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="a8a91-143">Jakmile se spustí průvodce aplikací Win32, klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a8a91-143">When the Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="a8a91-144">Po vytvoření projektu otevřete správce balíčku NuGet kliknutím pravým tlačítkem myši na projekt **hellodocumentdb** v **průzkumníku řešení** a potom na **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="a8a91-144">Once the project has been created, open the NuGet package manager by right-clicking the **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![Snímek obrazovky s možností Spravovat balíčky NuGet v nabídce projektu](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="a8a91-146">Na kartě **NuGet: hellodocumentdb** klikněte na **Procházet** a vyhledejte *documentdbcpp*.</span><span class="sxs-lookup"><span data-stu-id="a8a91-146">In the **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="a8a91-147">Ve výsledcích vyberte položku DocumentDbCPP, jak je znázorněno na následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a8a91-147">In the results, select DocumentDbCPP, as shown in the following screenshot.</span></span> <span data-ttu-id="a8a91-148">Tento balíček nainstaluje odkazy na sadu C++ REST SDK, která závislostí pro DocumentDbCPP.</span><span class="sxs-lookup"><span data-stu-id="a8a91-148">This package installs references to C++ REST SDK, which is a dependency for the DocumentDbCPP.</span></span>  
   
    ![Snímek obrazovky se zvýrazněným balíčkem DocumentDbCpp](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="a8a91-150">Jakmile se balíčky přidají do vašeho projektu, můžeme začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="a8a91-150">Once the packages have been added to your project, we are all set to start writing some code.</span></span>   

## <span data-ttu-id="a8a91-151"><a id="Config"></a>Krok 3: Zkopírování podrobností o připojení z webu Azure Portal pro databázi Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a8a91-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="a8a91-152">Otevřete web [Azure Portal](https://portal.azure.com) a přejděte do účtu databáze Azure Cosmos DB, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a8a91-152">Bring up [Azure portal](https://portal.azure.com) and traverse to the Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="a8a91-153">Pro další krok budete z webu Azure Portal potřebovat identifikátor URI a primární klíč, pomocí kterých navážete připojení z fragmentu kódu v jazyce C++.</span><span class="sxs-lookup"><span data-stu-id="a8a91-153">We will need the URI and the primary key from Azure portal in the next step to establish a connection from our C++ code snippet.</span></span> 

![Identifikátor URI a klíče pro službu Azure Cosmos DB na webu Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="a8a91-155"><a id="Connect"></a>Krok 4: Připojení k účtu služby Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a8a91-155"><a id="Connect"></a>Step 4: Connect to an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="a8a91-156">Do zdrojového kódu zadejte níže uvedené hlavičky a obory názvů za `#include "stdafx.h"`.</span><span class="sxs-lookup"><span data-stu-id="a8a91-156">Add the following headers and namespaces to your source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="a8a91-157">Dále do hlavní funkce přidejte následující kód a změňte konfiguraci účtu a primární klíč tak, aby se shodovaly s nastavením služby Azure Cosmos DB popsaným v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="a8a91-157">Next add the following code to your main function and replace the account configuration and primary key to match your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="a8a91-158">Nyní, když máte kód pro inicializaci klienta documentdb, se budeme věnovat práci s prostředky Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a8a91-158">Now that you have the code to initialize the documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="a8a91-159"><a id="CreateDBColl"></a>Krok 5: Vytvoření databáze a kolekce v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="a8a91-160">Pokud se službou Azure Cosmos DB začínáte, přečtěte si ještě před provedením tohoto kroku, jak databáze, kolekce a dokumenty vzájemně komunikují.</span><span class="sxs-lookup"><span data-stu-id="a8a91-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new to Azure Cosmos DB.</span></span> <span data-ttu-id="a8a91-161">[Databáze](documentdb-resources.md#databases) je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi.</span><span class="sxs-lookup"><span data-stu-id="a8a91-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="a8a91-162">[Kolekce](documentdb-resources.md#collections) je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8a91-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and the associated JavaScript application logic.</span></span> <span data-ttu-id="a8a91-163">Další informace o konceptech a hierarchickém modelu prostředků Azure Cosmos DB najdete v tématu [Koncepty a hierarchický model prostředků Azure Cosmos DB](documentdb-resources.md).</span><span class="sxs-lookup"><span data-stu-id="a8a91-163">You can learn more about the Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="a8a91-164">Databázi a odpovídající kolekci vytvoříte vložením níže uvedeného kódu na konec hlavní funkce.</span><span class="sxs-lookup"><span data-stu-id="a8a91-164">To create a database and a corresponding collection add the following code to the end of your main function.</span></span> <span data-ttu-id="a8a91-165">Prostřednictvím konfigurace klienta deklarované v předchozím kroku vznikne databáze „FamilyRegistry“ a kolekce „FamilyCollection“.</span><span class="sxs-lookup"><span data-stu-id="a8a91-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using the client configuration you declared in the previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="a8a91-166"><a id="CreateDoc"></a>Krok 6: Vytvoření dokumentu</span><span class="sxs-lookup"><span data-stu-id="a8a91-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="a8a91-167">[Dokumenty](documentdb-resources.md#documents) představují uživatelem definovaný (libovolný) obsah JSON.</span><span class="sxs-lookup"><span data-stu-id="a8a91-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="a8a91-168">Nyní můžete do služby Azure Cosmos DB vložit dokument.</span><span class="sxs-lookup"><span data-stu-id="a8a91-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="a8a91-169">Dokument můžete vytvořit zkopírováním níže uvedeného kódu na konec hlavní funkce.</span><span class="sxs-lookup"><span data-stu-id="a8a91-169">You can create a document by copying the following code into the end of the main function.</span></span> 

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

<span data-ttu-id="a8a91-170">Souhrnně řečeno, tento kód vytvoří databázi, kolekci a dokumenty Azure Cosmos DB, na které se můžete dotazovat v Průzkumníku dokumentů na webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a8a91-170">To summarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![Kurz k C++ – diagram ilustrující hierarchický vztah mezi účtem, databází, kolekcí a dokumenty](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="a8a91-172"><a id="QueryDB"></a>Krok 7: Dotazování prostředků Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="a8a91-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="a8a91-173">Azure Cosmos DB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.</span><span class="sxs-lookup"><span data-stu-id="a8a91-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="a8a91-174">Následující ukázkový kód ukazuje dotaz vytvořený pomocí syntaxe SQL, který můžete spouštět proti dokumentům vytvořeným v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="a8a91-174">The following sample code shows a query made using SQL syntax that you can run against the documents we created in the previous step.</span></span>

<span data-ttu-id="a8a91-175">Tato funkce využívá jako argumenty společně s klientem dokumentu i unikátní identifikátor nebo ID prostředku databáze a kolekce.</span><span class="sxs-lookup"><span data-stu-id="a8a91-175">The function takes in as arguments the unique identifier or resource id for the database and the collection along with the document client.</span></span> <span data-ttu-id="a8a91-176">Vložte tento kód před hlavní funkci.</span><span class="sxs-lookup"><span data-stu-id="a8a91-176">Add this code before main function.</span></span>

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

## <span data-ttu-id="a8a91-177"><a id="Replace"></a>Krok 8: Nahrazení dokumentu</span><span class="sxs-lookup"><span data-stu-id="a8a91-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="a8a91-178">Azure Cosmos DB podporuje nahrazování dokumentů JSON, jak můžete vidět na následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="a8a91-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in the following code.</span></span> <span data-ttu-id="a8a91-179">Tento kód vložte za funkci executesimplequery.</span><span class="sxs-lookup"><span data-stu-id="a8a91-179">Add this code after the executesimplequery function.</span></span>

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

## <span data-ttu-id="a8a91-180"><a id="Delete"></a>Krok 9: Odstranění dokumentu</span><span class="sxs-lookup"><span data-stu-id="a8a91-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="a8a91-181">Azure Cosmos DB podporuje odstraňování dokumentů JSON. Provedete to tak, že zkopírujete a vložíte následující kód za funkci replacedocument.</span><span class="sxs-lookup"><span data-stu-id="a8a91-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting the following code after the replacedocument function.</span></span> 

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

## <span data-ttu-id="a8a91-182"><a id="DeleteDB"></a>Krok 10: Odstranění databáze</span><span class="sxs-lookup"><span data-stu-id="a8a91-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="a8a91-183">Odstraněním vytvořené databáze dojde k odebrání databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).</span><span class="sxs-lookup"><span data-stu-id="a8a91-183">Deleting the created database removes the database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="a8a91-184">Zkopírujte a za funkci deletedocument vložte následující fragment kódu (vyčištění funkce), který odebere databázi a všechny podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="a8a91-184">Copy and paste the following code snippet (function cleanup) after the deletedocument function to remove the database and all the child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="a8a91-185"><a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#</span><span class="sxs-lookup"><span data-stu-id="a8a91-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="a8a91-186">Vložili jsme kód, pomocí kterého můžete vytvářet, upravovat a odstraňovat různé prostředky Azure Cosmos DB nebo se na ně dotazovat.</span><span class="sxs-lookup"><span data-stu-id="a8a91-186">We have now added code to create, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="a8a91-187">Pojďme ho uvést do provozu přidáním volání těchto funkcí z naší hlavní funkce v projektu hellodocumentdb.cpp a některých diagnostických zpráv.</span><span class="sxs-lookup"><span data-stu-id="a8a91-187">Let us now wire this up by adding calls to these different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="a8a91-188">Provedete to tak, že hlavní funkci vaší aplikace nahradíte níže uvedeným kódem.</span><span class="sxs-lookup"><span data-stu-id="a8a91-188">You can do so by replacing the main function of your application with the following code.</span></span> <span data-ttu-id="a8a91-189">account_configuration_uri a primary_key, které jste do kódu zkopírovali v kroku 3, se přepíší, takže tento řádek uložte nebo hodnoty znovu zkopírujte z webu.</span><span class="sxs-lookup"><span data-stu-id="a8a91-189">This writes over the account_configuration_uri and primary_key you copied into the code in Step 3, so save that line or copy the values in again from the portal.</span></span> 

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

<span data-ttu-id="a8a91-190">Teď už by mělo být možné vytvořit a spustit kód v sadě Visual Studio stisknutím klávesy F5, případně v okně terminálu vyhledáním aplikace a otevřením spustitelného souboru.</span><span class="sxs-lookup"><span data-stu-id="a8a91-190">You should now be able to build and run your code in Visual Studio by pressing F5 or alternatively in the terminal window by locating the application and running the executable.</span></span> 

<span data-ttu-id="a8a91-191">Měl by se zobrazit výstup počáteční aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8a91-191">You should see the output of your get started app.</span></span> <span data-ttu-id="a8a91-192">Tento výstup by se měl shodovat se snímkem obrazovky níže.</span><span class="sxs-lookup"><span data-stu-id="a8a91-192">The output should match the following screenshot.</span></span>

![Výstup aplikace v jazyce C++ využívající službu Azure Cosmos DB](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="a8a91-194">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="a8a91-194">Congratulations!</span></span> <span data-ttu-id="a8a91-195">Dokončili jste kurz jazyka C++ a máte svou první konzolovou aplikaci využívající službu Azure Cosmos DB!</span><span class="sxs-lookup"><span data-stu-id="a8a91-195">You've completed the C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="a8a91-196"><a id="GetSolution"></a>Získání úplného řešení kurzu k C++</span><span class="sxs-lookup"><span data-stu-id="a8a91-196"><a id="GetSolution"></a>Get the complete C++ tutorial solution</span></span>
<span data-ttu-id="a8a91-197">Abyste mohli sestavit řešení GetStarted, které obsahuje všechny ukázky tohoto článku, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="a8a91-197">To build the GetStarted solution that contains all the samples in this article, you need the following:</span></span>

* <span data-ttu-id="a8a91-198">[Účet služby Azure Cosmos DB][create-account]</span><span class="sxs-lookup"><span data-stu-id="a8a91-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="a8a91-199">Řešení [GetStarted](https://github.com/stalker314314/DocumentDBCpp) dostupné na GitHubu</span><span class="sxs-lookup"><span data-stu-id="a8a91-199">The [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8a91-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8a91-200">Next steps</span></span>
* <span data-ttu-id="a8a91-201">Zjistěte, jak [monitorovat účet služby Azure Cosmos DB](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="a8a91-201">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="a8a91-202">Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="a8a91-202">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="a8a91-203">Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace ke službě Azure Cosmos DB](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="a8a91-203">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


