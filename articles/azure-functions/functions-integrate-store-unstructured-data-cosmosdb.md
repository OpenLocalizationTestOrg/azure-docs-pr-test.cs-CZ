---
title: "aaaStore nestrukturovaných dat s využitím Azure Functions a Cosmos DB"
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
ms.openlocfilehash: 48d6899c20d3e6f6b062725fca329972ead3c696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a><span data-ttu-id="2b4d4-104">Ukládání nestrukturovaných dat pomocí Azure Functions a databáze Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="2b4d4-104">Store unstructured data using Azure Functions and Cosmos DB</span></span>

<span data-ttu-id="2b4d4-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) je skvělým způsobem toostore nestrukturovaných a JSON data.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a great way toostore unstructured and JSON data.</span></span> <span data-ttu-id="2b4d4-106">Spolu s Azure Functions urychluje a zjednodušuje ukládání dat – ve srovnání s ukládáním dat v relační databáze budete potřebovat méně kódování.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-106">Combined with Azure Functions, Cosmos DB makes storing data quick and easy with much less code than required for storing data in a relational database.</span></span>

<span data-ttu-id="2b4d4-107">V Azure Functions poskytují vstupní a výstupní vazby dat deklarativní způsob tooconnect tooexternal služby z funkce.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-107">In Azure Functions, input and output bindings provide a declarative way tooconnect tooexternal service data from your function.</span></span> <span data-ttu-id="2b4d4-108">V tomto tématu se dozvíte, jak tooupdate existující C# fungovat tooadd vazbu výstup, která ukládá Nestrukturovaná data v dokumentu Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-108">In this topic, learn how tooupdate an existing C# function tooadd an output binding that stores unstructured data in a Cosmos DB document.</span></span> 

![Databáze Cosmos](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a><span data-ttu-id="2b4d4-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2b4d4-110">Prerequisites</span></span>

<span data-ttu-id="2b4d4-111">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="2b4d4-111">toocomplete this tutorial:</span></span>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a><span data-ttu-id="2b4d4-112">Přidání výstupní vazby</span><span class="sxs-lookup"><span data-stu-id="2b4d4-112">Add an output binding</span></span>

1. <span data-ttu-id="2b4d4-113">Rozbalte aplikaci Function App i funkci.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-113">Expand both your function app and your function.</span></span>

1. <span data-ttu-id="2b4d4-114">Vyberte **integrací** a **+ nový výstupní**, což je v hello top pravé části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-114">Select **Integrate** and **+ New Output**, which is at hello top right of hello page.</span></span> <span data-ttu-id="2b4d4-115">Zvolte **Azure Cosmos DB** a klikněte na **Vybrat**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-115">Choose **Azure Cosmos DB**, and click **Select**.</span></span>

    ![Přidání výstupní vazby služby Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. <span data-ttu-id="2b4d4-117">Použití hello **Azure Cosmos DB výstup** nastavení uvedeného v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="2b4d4-117">Use hello **Azure Cosmos DB output** settings as specified in hello table:</span></span> 

    ![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | <span data-ttu-id="2b4d4-119">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2b4d4-119">Setting</span></span>      | <span data-ttu-id="2b4d4-120">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="2b4d4-120">Suggested value</span></span>  | <span data-ttu-id="2b4d4-121">Popis</span><span class="sxs-lookup"><span data-stu-id="2b4d4-121">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="2b4d4-122">**Název parametru dokumentu**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-122">**Document parameter name**</span></span> | <span data-ttu-id="2b4d4-123">taskDocument</span><span class="sxs-lookup"><span data-stu-id="2b4d4-123">taskDocument</span></span> | <span data-ttu-id="2b4d4-124">Název, který odkazuje objekt Cosmos DB toohello v kódu.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-124">Name that refers toohello Cosmos DB object in code.</span></span> |
    | <span data-ttu-id="2b4d4-125">**Název databáze**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-125">**Database name**</span></span> | <span data-ttu-id="2b4d4-126">taskDatabase</span><span class="sxs-lookup"><span data-stu-id="2b4d4-126">taskDatabase</span></span> | <span data-ttu-id="2b4d4-127">Název databáze toosave dokumentů.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-127">Name of database toosave documents.</span></span> |
    | <span data-ttu-id="2b4d4-128">**Název kolekce**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-128">**Collection name**</span></span> | <span data-ttu-id="2b4d4-129">TaskCollection</span><span class="sxs-lookup"><span data-stu-id="2b4d4-129">TaskCollection</span></span> | <span data-ttu-id="2b4d4-130">Název kolekce databází Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-130">Name of collection of Cosmos DB databases.</span></span> |
    | <span data-ttu-id="2b4d4-131">**V případě hodnoty true vytvoří hello Cosmos DB databáze a kolekce.**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-131">**If true, creates hello Cosmos DB database and collection**</span></span> | <span data-ttu-id="2b4d4-132">Zaškrtnuté</span><span class="sxs-lookup"><span data-stu-id="2b4d4-132">Checked</span></span> | <span data-ttu-id="2b4d4-133">kolekce Hello již neexistuje, takže ho vytvořte.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-133">hello collection doesn't already exist, so create it.</span></span> |

4. <span data-ttu-id="2b4d4-134">Vyberte **nový** další toohello **Cosmos DB dokumentu připojení** label a vyberte **+ vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-134">Select **New** next toohello **Cosmos DB document connection** label, and select **+ Create new**.</span></span> 

5. <span data-ttu-id="2b4d4-135">Použití hello **nový účet** nastavení uvedeného v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="2b4d4-135">Use hello **New account** settings as specified in hello table:</span></span> 

    ![Konfigurace připojení Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | <span data-ttu-id="2b4d4-137">Nastavení</span><span class="sxs-lookup"><span data-stu-id="2b4d4-137">Setting</span></span>      | <span data-ttu-id="2b4d4-138">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="2b4d4-138">Suggested value</span></span>  | <span data-ttu-id="2b4d4-139">Popis</span><span class="sxs-lookup"><span data-stu-id="2b4d4-139">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="2b4d4-140">**ID**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-140">**ID**</span></span> | <span data-ttu-id="2b4d4-141">Název databáze</span><span class="sxs-lookup"><span data-stu-id="2b4d4-141">Name of database</span></span> | <span data-ttu-id="2b4d4-142">Jedinečné ID pro databázi Cosmos DB hello</span><span class="sxs-lookup"><span data-stu-id="2b4d4-142">Unique ID for hello Cosmos DB database</span></span>  |
    | <span data-ttu-id="2b4d4-143">**Rozhraní API**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-143">**API**</span></span> | <span data-ttu-id="2b4d4-144">SQL (DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="2b4d4-144">SQL (DocumentDB)</span></span> | <span data-ttu-id="2b4d4-145">Vyberte rozhraní API databáze hello dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-145">Select hello document database API.</span></span>  |
    | <span data-ttu-id="2b4d4-146">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-146">**Subscription**</span></span> | <span data-ttu-id="2b4d4-147">předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="2b4d4-147">Azure Subscription</span></span> | <span data-ttu-id="2b4d4-148">předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="2b4d4-148">Azure Subscription</span></span>  |
    | <span data-ttu-id="2b4d4-149">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-149">**Resource Group**</span></span> | <span data-ttu-id="2b4d4-150">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="2b4d4-150">myResourceGroup</span></span> |  <span data-ttu-id="2b4d4-151">Použijte hello existující skupinu prostředků, která obsahuje aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-151">Use hello existing resource group that contains your function app.</span></span> |
    | <span data-ttu-id="2b4d4-152">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="2b4d4-152">**Location**</span></span>  | <span data-ttu-id="2b4d4-153">WestEurope</span><span class="sxs-lookup"><span data-stu-id="2b4d4-153">WestEurope</span></span> | <span data-ttu-id="2b4d4-154">Vyberte umístění téměř tooeither aplikace funkce nebo tooother aplikace, které používají hello uložené dokumenty.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-154">Select a location near tooeither your function app or tooother apps that use hello stored documents.</span></span>  |

6. <span data-ttu-id="2b4d4-155">Klikněte na tlačítko **OK** toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-155">Click **OK** toocreate hello database.</span></span> <span data-ttu-id="2b4d4-156">To může trvat několik minut toocreate hello databáze.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-156">It may take a few minutes toocreate hello database.</span></span> <span data-ttu-id="2b4d4-157">Po vytvoření databáze hello připojovací řetězec databáze hello je uložený jako funkce nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-157">After hello database is created, hello database connection string is stored as a function app setting.</span></span> <span data-ttu-id="2b4d4-158">Hello název tohoto nastavení aplikace je vložen do **účet připojení databáze Cosmos**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-158">hello name of this app setting is inserted in **Cosmos DB account connection**.</span></span> 
 
8. <span data-ttu-id="2b4d4-159">Po nastavení hello připojovací řetězec, vyberte **Uložit** toocreate hello vazby.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-159">After hello connection string is set, select **Save** toocreate hello binding.</span></span>

## <a name="update-hello-function-code"></a><span data-ttu-id="2b4d4-160">Aktualizovat kód funkce hello</span><span class="sxs-lookup"><span data-stu-id="2b4d4-160">Update hello function code</span></span>

<span data-ttu-id="2b4d4-161">Nahraďte hello existující kód C# funkce hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="2b4d4-161">Replace hello existing C# function code with hello following code:</span></span>

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
<span data-ttu-id="2b4d4-162">Tato ukázka kódu přečte hello požadavek HTTP řetězců dotazů a přiřadí je toofields v hello `taskDocument` objektu.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-162">This code sample reads hello HTTP Request query strings and assigns them toofields in hello `taskDocument` object.</span></span> <span data-ttu-id="2b4d4-163">Hello `taskDocument` vazby odešle data objektu hello z této vazby parametru toobe uložené v databázi dokumentů vázané hello.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-163">hello `taskDocument` binding sends hello object data from this binding parameter toobe stored in hello bound document database.</span></span> <span data-ttu-id="2b4d4-164">Hello databáze byla vytvořena hello při prvním spuštění funkce hello.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-164">hello database is created hello first time hello function runs.</span></span>

## <a name="test-hello-function-and-database"></a><span data-ttu-id="2b4d4-165">Test hello funkce a databáze</span><span class="sxs-lookup"><span data-stu-id="2b4d4-165">Test hello function and database</span></span>

1. <span data-ttu-id="2b4d4-166">Rozbalte okno správné hello a vyberte **Test**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-166">Expand hello right window and select **Test**.</span></span> <span data-ttu-id="2b4d4-167">V části **dotazu**, klikněte na tlačítko **+ přidat parametr** a přidejte následující parametry řetězce dotazu toohello hello:</span><span class="sxs-lookup"><span data-stu-id="2b4d4-167">Under **Query**, click **+ Add parameter** and add hello following parameters toohello query string:</span></span>

    + `name`
    + `task`
    + `duedate`

2. <span data-ttu-id="2b4d4-168">Klikněte na **Spustit** a ověřte, že se vrátí stavový kód 200.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-168">Click **Run** and verify that a 200 status is returned.</span></span>

    ![Konfigurace výstupní vazby databáze Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. <span data-ttu-id="2b4d4-170">Na levé straně hello portál Azure hello, rozbalte položku panelu ikonu hello, typ `cosmos` v hello vyhledávání pole a vyberte **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-170">On hello left side of hello Azure portal, expand hello icon bar, type `cosmos` in hello search field, and select **Azure Cosmos DB**.</span></span>

    ![Vyhledejte hello Cosmos DB služby](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. <span data-ttu-id="2b4d4-172">Vyberte hello databáze, které jste vytvořili, pak vyberte **Průzkumníku dat**.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-172">Select hello database you created, then select **Data Explorer**.</span></span> <span data-ttu-id="2b4d4-173">Rozbalte hello **kolekce** uzly, vyberte nový dokument hello a potvrďte, že hello dokument obsahuje vaše hodnoty řetězců dotazu, společně s některá další metadata.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-173">Expand hello **Collections** nodes, select hello new document, and confirm that hello document contains your query string values, along with some additional metadata.</span></span> 

    ![Kontrola záznamu v databázi Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

<span data-ttu-id="2b4d4-175">Úspěšně jste přidali aktivační událost HTTP tooyour vazby, která ukládá Nestrukturovaná data v databázi Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="2b4d4-175">You have successfully added a binding tooyour HTTP trigger that stores unstructured data in a Cosmos DB database.</span></span>

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="2b4d4-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b4d4-176">Next steps</span></span>

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="2b4d4-177">Další informace o databázi Cosmos DB tooa vazby najdete v tématu [Azure funkce Cosmos DB vazby](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="2b4d4-177">For more information about binding tooa Cosmos DB database, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>
