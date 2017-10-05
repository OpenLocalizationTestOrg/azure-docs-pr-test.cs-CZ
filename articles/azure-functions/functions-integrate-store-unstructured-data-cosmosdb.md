---
title: "Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB"
description: "Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure functions, functions, zpracování událostí, Cosmos DB, dynamické výpočty, architektura bez serverů"
ms.assetid: 
ms.service: functions
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7c18676ff94ec7da17094abc5f33fb3c6a79895f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a><span data-ttu-id="452da-104">Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="452da-104">Store unstructured data using Azure Functions and Cosmos DB</span></span>

<span data-ttu-id="452da-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) nabízí skvělou možnost pro ukládání nestrukturovaných dat a dat JSON.</span><span class="sxs-lookup"><span data-stu-id="452da-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a great way to store unstructured and JSON data.</span></span> <span data-ttu-id="452da-106">Spolu s Azure Functions urychluje a zjednodušuje ukládání dat – ve srovnání s ukládáním dat v relační databáze budete potřebovat méně kódování.</span><span class="sxs-lookup"><span data-stu-id="452da-106">Combined with Azure Functions, Cosmos DB makes storing data quick and easy with much less code than required for storing data in a relational database.</span></span>

<span data-ttu-id="452da-107">Ve službě Azure Functions poskytují vstupní a výstupní vazby deklarativní způsob připojení k datům externí služby z funkce.</span><span class="sxs-lookup"><span data-stu-id="452da-107">In Azure Functions, input and output bindings provide a declarative way to connect to external service data from your function.</span></span> <span data-ttu-id="452da-108">V tomto tématu se dozvíte, jak aktualizovat stávající funkci jazyka C# pro přidání výstupní vazby, která ukládá nestrukturovaná data v dokumentu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="452da-108">In this topic, learn how to update an existing C# function to add an output binding that stores unstructured data in a Cosmos DB document.</span></span> 

![Databáze Cosmos](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a><span data-ttu-id="452da-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="452da-110">Prerequisites</span></span>

<span data-ttu-id="452da-111">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="452da-111">To complete this tutorial:</span></span>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a><span data-ttu-id="452da-112">Přidání výstupní vazby</span><span class="sxs-lookup"><span data-stu-id="452da-112">Add an output binding</span></span>

1. <span data-ttu-id="452da-113">Rozbalte aplikaci Function App i funkci.</span><span class="sxs-lookup"><span data-stu-id="452da-113">Expand both your function app and your function.</span></span>

1. <span data-ttu-id="452da-114">Vyberte **Integrace** a v pravém horním rohu stránky **+ Nový výstup**.</span><span class="sxs-lookup"><span data-stu-id="452da-114">Select **Integrate** and **+ New Output**, which is at the top right of the page.</span></span> <span data-ttu-id="452da-115">Zvolte **Azure Cosmos DB** a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="452da-115">Choose **Azure Cosmos DB**, and click **Select**.</span></span>

    ![Přidání výstupní vazby služby Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. <span data-ttu-id="452da-117">Použijte nastavení pro **Výstup služby Azure Cosmos DB** uvedená v tabulce:</span><span class="sxs-lookup"><span data-stu-id="452da-117">Use the **Azure Cosmos DB output** settings as specified in the table:</span></span> 

    ![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | <span data-ttu-id="452da-119">Nastavení</span><span class="sxs-lookup"><span data-stu-id="452da-119">Setting</span></span>      | <span data-ttu-id="452da-120">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="452da-120">Suggested value</span></span>  | <span data-ttu-id="452da-121">Popis</span><span class="sxs-lookup"><span data-stu-id="452da-121">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="452da-122">**Název parametru dokumentu**</span><span class="sxs-lookup"><span data-stu-id="452da-122">**Document parameter name**</span></span> | <span data-ttu-id="452da-123">taskDocument</span><span class="sxs-lookup"><span data-stu-id="452da-123">taskDocument</span></span> | <span data-ttu-id="452da-124">Název, který odkazuje na objekt Cosmos DB v kódu.</span><span class="sxs-lookup"><span data-stu-id="452da-124">Name that refers to the Cosmos DB object in code.</span></span> |
    | <span data-ttu-id="452da-125">**Název databáze**</span><span class="sxs-lookup"><span data-stu-id="452da-125">**Database name**</span></span> | <span data-ttu-id="452da-126">taskDatabase</span><span class="sxs-lookup"><span data-stu-id="452da-126">taskDatabase</span></span> | <span data-ttu-id="452da-127">Název databáze pro uložení dokumentů.</span><span class="sxs-lookup"><span data-stu-id="452da-127">Name of database to save documents.</span></span> |
    | <span data-ttu-id="452da-128">**Název kolekce**</span><span class="sxs-lookup"><span data-stu-id="452da-128">**Collection name**</span></span> | <span data-ttu-id="452da-129">TaskCollection</span><span class="sxs-lookup"><span data-stu-id="452da-129">TaskCollection</span></span> | <span data-ttu-id="452da-130">Název kolekce databází Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="452da-130">Name of collection of Cosmos DB databases.</span></span> |
    | <span data-ttu-id="452da-131">**Je-li nastavená hodnota true, vytvoří se databáze a kolekce Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="452da-131">**If true, creates the Cosmos DB database and collection**</span></span> | <span data-ttu-id="452da-132">Zaškrtnuté</span><span class="sxs-lookup"><span data-stu-id="452da-132">Checked</span></span> | <span data-ttu-id="452da-133">Kolekce ještě neexistuje, takže ji vytvořte.</span><span class="sxs-lookup"><span data-stu-id="452da-133">The collection doesn't already exist, so create it.</span></span> |

4. <span data-ttu-id="452da-134">Vedle popisku **Připojení dokumentu Cosmos DB** vyberte **Nové** a potom vyberte **+ Vytvořit nové**.</span><span class="sxs-lookup"><span data-stu-id="452da-134">Select **New** next to the **Cosmos DB document connection** label, and select **+ Create new**.</span></span> 

5. <span data-ttu-id="452da-135">Použijte nastavení pro **Nový účet** uvedená v tabulce:</span><span class="sxs-lookup"><span data-stu-id="452da-135">Use the **New account** settings as specified in the table:</span></span> 

    ![Konfigurace připojení Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | <span data-ttu-id="452da-137">Nastavení</span><span class="sxs-lookup"><span data-stu-id="452da-137">Setting</span></span>      | <span data-ttu-id="452da-138">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="452da-138">Suggested value</span></span>  | <span data-ttu-id="452da-139">Popis</span><span class="sxs-lookup"><span data-stu-id="452da-139">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="452da-140">**ID**</span><span class="sxs-lookup"><span data-stu-id="452da-140">**ID**</span></span> | <span data-ttu-id="452da-141">Název databáze</span><span class="sxs-lookup"><span data-stu-id="452da-141">Name of database</span></span> | <span data-ttu-id="452da-142">Jedinečné ID databáze Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="452da-142">Unique ID for the Cosmos DB database</span></span>  |
    | <span data-ttu-id="452da-143">**Rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="452da-143">**API**</span></span> | <span data-ttu-id="452da-144">SQL (DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="452da-144">SQL (DocumentDB)</span></span> | <span data-ttu-id="452da-145">Vyberte rozhraní API databáze dokumentů.</span><span class="sxs-lookup"><span data-stu-id="452da-145">Select the document database API.</span></span>  |
    | <span data-ttu-id="452da-146">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="452da-146">**Subscription**</span></span> | <span data-ttu-id="452da-147">předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="452da-147">Azure Subscription</span></span> | <span data-ttu-id="452da-148">předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="452da-148">Azure Subscription</span></span>  |
    | <span data-ttu-id="452da-149">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="452da-149">**Resource Group**</span></span> | <span data-ttu-id="452da-150">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="452da-150">myResourceGroup</span></span> |  <span data-ttu-id="452da-151">Použijte existující skupinu prostředků, která obsahuje vaši aplikací funkcí.</span><span class="sxs-lookup"><span data-stu-id="452da-151">Use the existing resource group that contains your function app.</span></span> |
    | <span data-ttu-id="452da-152">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="452da-152">**Location**</span></span>  | <span data-ttu-id="452da-153">WestEurope</span><span class="sxs-lookup"><span data-stu-id="452da-153">WestEurope</span></span> | <span data-ttu-id="452da-154">Vyberte umístění blízko vaší aplikaci funkcí nebo jiným aplikacím, které používají uložené dokumenty.</span><span class="sxs-lookup"><span data-stu-id="452da-154">Select a location near to either your function app or to other apps that use the stored documents.</span></span>  |

6. <span data-ttu-id="452da-155">Kliknutím na **OK** vytvořte databázi.</span><span class="sxs-lookup"><span data-stu-id="452da-155">Click **OK** to create the database.</span></span> <span data-ttu-id="452da-156">Vytvoření databáze může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="452da-156">It may take a few minutes to create the database.</span></span> <span data-ttu-id="452da-157">Po vytvoření databáze se připojovací řetězec databáze uloží jako nastavení aplikace funkcí.</span><span class="sxs-lookup"><span data-stu-id="452da-157">After the database is created, the database connection string is stored as a function app setting.</span></span> <span data-ttu-id="452da-158">Název tohoto nastavení aplikace se vloží do **Připojení účtu Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="452da-158">The name of this app setting is inserted in **Cosmos DB account connection**.</span></span> 
 
8. <span data-ttu-id="452da-159">Po nastavení připojovacího řetězce vyberte **Uložit** a vytvořte vazbu.</span><span class="sxs-lookup"><span data-stu-id="452da-159">After the connection string is set, select **Save** to create the binding.</span></span>

## <a name="update-the-function-code"></a><span data-ttu-id="452da-160">Aktualizace kódu funkce</span><span class="sxs-lookup"><span data-stu-id="452da-160">Update the function code</span></span>

<span data-ttu-id="452da-161">Nahraďte stávající kód funkce jazyka C# následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="452da-161">Replace the existing C# function code with the following code:</span></span>

```csharp
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, out object taskDocument, TraceWriter log)
{
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    string task = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "task", true) == 0)
        .Value;

    string duedate = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "duedate", true) == 0)
        .Value;

    taskDocument = new {
        name = name,
        duedate = duedate.ToString(),
        task = task
    };

    if (name != "" && task != "") {
        return req.CreateResponse(HttpStatusCode.OK);
    }
    else {
        return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}

```
<span data-ttu-id="452da-162">Tento vzorový kód přečte řetězce dotazů požadavků HTTP a přiřadí je do polí v objektu `taskDocument`.</span><span class="sxs-lookup"><span data-stu-id="452da-162">This code sample reads the HTTP Request query strings and assigns them to fields in the `taskDocument` object.</span></span> <span data-ttu-id="452da-163">Vazba `taskDocument` odešle data objektu z tohoto parametru vazby k uložení ve vázané databázi dokumentů.</span><span class="sxs-lookup"><span data-stu-id="452da-163">The `taskDocument` binding sends the object data from this binding parameter to be stored in the bound document database.</span></span> <span data-ttu-id="452da-164">Tato databáze se vytvoří při prvním spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="452da-164">The database is created the first time the function runs.</span></span>

## <a name="test-the-function-and-database"></a><span data-ttu-id="452da-165">Testování funkce a databáze</span><span class="sxs-lookup"><span data-stu-id="452da-165">Test the function and database</span></span>

1. <span data-ttu-id="452da-166">Rozbalte okno vpravo a vyberte **Test**.</span><span class="sxs-lookup"><span data-stu-id="452da-166">Expand the right window and select **Test**.</span></span> <span data-ttu-id="452da-167">V části **Dotaz** klikněte na **+ Přidat parametr** a přidejte do řetězce dotazu následující parametry:</span><span class="sxs-lookup"><span data-stu-id="452da-167">Under **Query**, click **+ Add parameter** and add the following parameters to the query string:</span></span>

    + `name`
    + `task`
    + `duedate`

2. <span data-ttu-id="452da-168">Klikněte na **Spustit** a ověřte, že se vrátí stavový kód 200.</span><span class="sxs-lookup"><span data-stu-id="452da-168">Click **Run** and verify that a 200 status is returned.</span></span>

    ![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. <span data-ttu-id="452da-170">Na levé straně webu Azure Portal rozbalte pruh ikon, do vyhledávacího pole zadejte `cosmos` a vyberte **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="452da-170">On the left side of the Azure portal, expand the icon bar, type `cosmos` in the search field, and select **Azure Cosmos DB**.</span></span>

    ![Vyhledání služby Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. <span data-ttu-id="452da-172">Vyberte databázi, kterou jste vytvořili a potom vyberte **Průzkumník dat**.</span><span class="sxs-lookup"><span data-stu-id="452da-172">Select the database you created, then select **Data Explorer**.</span></span> <span data-ttu-id="452da-173">Rozbalte uzly **Kolekce**, vyberte nový dokument a potvrďte, že dokument obsahuje vaše hodnoty řetězce dotazu spolu s dalšími metadaty.</span><span class="sxs-lookup"><span data-stu-id="452da-173">Expand the **Collections** nodes, select the new document, and confirm that the document contains your query string values, along with some additional metadata.</span></span> 

    ![Kontrola záznamu v databázi Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

<span data-ttu-id="452da-175">Úspěšně jste přidali vazbu na váš trigger HTTP, který ukládá nestrukturovaná data v databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="452da-175">You have successfully added a binding to your HTTP trigger that stores unstructured data in a Cosmos DB database.</span></span>

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="452da-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="452da-176">Next steps</span></span>

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="452da-177">Další informace o vazbách na databázi Cosmos DB najdete v tématu [Vazby Cosmos DB ve službě Azure Functions](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="452da-177">For more information about binding to a Cosmos DB database, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>
