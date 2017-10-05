---
title: "Jak používat Azure Table Storage z Ruby | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 372bc89f75ad4730f0defbf9d6f9f041ae5ce1bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-ruby"></a><span data-ttu-id="93175-103">Jak používat Azure Table Storage z Ruby</span><span class="sxs-lookup"><span data-stu-id="93175-103">How to use Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="93175-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="93175-104">Overview</span></span>
<span data-ttu-id="93175-105">Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="93175-105">This guide shows you how to perform common scenarios using the Azure Table service.</span></span> <span data-ttu-id="93175-106">Ukázky jsou zapsány pomocí rozhraní API Ruby.</span><span class="sxs-lookup"><span data-stu-id="93175-106">The samples are written using the Ruby API.</span></span> <span data-ttu-id="93175-107">Pokryté scénáře zahrnují **vytváření a odstraňování tabulek, vkládání a dotazování entity v tabulce**.</span><span class="sxs-lookup"><span data-stu-id="93175-107">The scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="93175-108">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="93175-108">Create a Ruby application</span></span>
<span data-ttu-id="93175-109">Pokyny pro vytvoření Ruby aplikace naleznete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="93175-109">For instructions how to create a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="93175-110">Konfigurace aplikace pro přístup k úložišti</span><span class="sxs-lookup"><span data-stu-id="93175-110">Configure your application to access Storage</span></span>
<span data-ttu-id="93175-111">Pokud chcete používat Azure Storage, musíte stáhnout a použít Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují se službami REST úložiště.</span><span class="sxs-lookup"><span data-stu-id="93175-111">To use Azure Storage, you need to download and use the Ruby azure package which includes a set of convenience libraries that communicate with the Storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="93175-112">Použití RubyGems získat balíček</span><span class="sxs-lookup"><span data-stu-id="93175-112">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="93175-113">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="93175-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="93175-114">Typ **gem instalace azure** v příkazovém okně instalace gem a závislostí.</span><span class="sxs-lookup"><span data-stu-id="93175-114">Type **gem install azure** in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="93175-115">Import balíčku</span><span class="sxs-lookup"><span data-stu-id="93175-115">Import the package</span></span>
<span data-ttu-id="93175-116">Použít svém oblíbeném textovém editoru, přidejte na začátek souboru Ruby, kde máte v úmyslu používat úložiště následující:</span><span class="sxs-lookup"><span data-stu-id="93175-116">Use your favorite text editor, add the following to the top of the Ruby file where you intend to use Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="93175-117">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="93175-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="93175-118">Modul azure, bude číst proměnné prostředí **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** informace požadované pro připojení k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="93175-118">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required to connect to your Azure Storage account.</span></span> <span data-ttu-id="93175-119">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu před použitím **Azure::TableService** následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="93175-119">If these environment variables are not set, you must specify the account information before using **Azure::TableService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="93175-120">K získání těchto hodnot z klasický nebo účet správce prostředků úložiště na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="93175-120">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="93175-121">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="93175-121">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="93175-122">Přejděte na účet úložiště, který chcete použít.</span><span class="sxs-lookup"><span data-stu-id="93175-122">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="93175-123">V okně nastavení na pravé straně klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="93175-123">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="93175-124">V okně klíče přístup, který se zobrazí uvidíte přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="93175-124">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="93175-125">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="93175-125">You can use either of these.</span></span>
5. <span data-ttu-id="93175-126">Kliknutím na ikonu kopírování do schránky zkopírujte klíč.</span><span class="sxs-lookup"><span data-stu-id="93175-126">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="93175-127">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="93175-127">Create a table</span></span>
<span data-ttu-id="93175-128">**Azure::TableService** objekt vám umožňuje spolupracovat s tabulkami a entity.</span><span class="sxs-lookup"><span data-stu-id="93175-128">The **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="93175-129">Chcete-li vytvořit tabulku, použijte **vytvořit\_table()** metoda.</span><span class="sxs-lookup"><span data-stu-id="93175-129">To create a table, use the **create\_table()** method.</span></span> <span data-ttu-id="93175-130">Následující příklad vytvoří tabulku nebo vytiskne chybu, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="93175-130">The following example creates a table or prints the error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="93175-131">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="93175-131">Add an entity to a table</span></span>
<span data-ttu-id="93175-132">Chcete-li přidat entitu, nejprve vytvořte objekt algoritmu hash, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="93175-132">To add an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="93175-133">Všimněte si, že pro každou entitu musíte zadat **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="93175-133">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="93175-134">Jsou jedinečné identifikátory vaší entity a jsou hodnoty, které může být dotazován mnohem rychleji než vaše jiných vlastností.</span><span class="sxs-lookup"><span data-stu-id="93175-134">These are the unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="93175-135">Azure Storage používá **PartitionKey** automaticky distribuovat entity v tabulce přes mnoho uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="93175-135">Azure Storage uses **PartitionKey** to automatically distribute the table's entities over many storage nodes.</span></span> <span data-ttu-id="93175-136">Entity se stejným **PartitionKey** jsou uložené na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="93175-136">Entities with the same **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="93175-137">**RowKey** je jedinečný Identifikátor entity v oddílu patří do.</span><span class="sxs-lookup"><span data-stu-id="93175-137">The **RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="93175-138">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="93175-138">Update an entity</span></span>
<span data-ttu-id="93175-139">Existuje několik metod, které jsou k dispozici pro aktualizace stávající entity:</span><span class="sxs-lookup"><span data-stu-id="93175-139">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="93175-140">**Aktualizovat\_entity():** aktualizace stávající entity nahrazením ho.</span><span class="sxs-lookup"><span data-stu-id="93175-140">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="93175-141">**sloučení\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity.</span><span class="sxs-lookup"><span data-stu-id="93175-141">**merge\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span>
* <span data-ttu-id="93175-142">**Vložit\_nebo\_sloučení\_entity():** aktualizace stávající entity podle jeho nahrazení.</span><span class="sxs-lookup"><span data-stu-id="93175-142">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="93175-143">Pokud žádná entita existuje, bude vložen nový:</span><span class="sxs-lookup"><span data-stu-id="93175-143">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="93175-144">**Vložit\_nebo\_nahradit\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity.</span><span class="sxs-lookup"><span data-stu-id="93175-144">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span> <span data-ttu-id="93175-145">Pokud existuje žádné entity, se vloží novou.</span><span class="sxs-lookup"><span data-stu-id="93175-145">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="93175-146">Následující příklad ukazuje, aktualizuje entitu s využitím **aktualizace\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="93175-146">The following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="93175-147">S **aktualizace\_entity()** a **sloučení\_entity()**, pokud typ entity, která jsou aktualizace neexistuje, operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="93175-147">With **update\_entity()** and **merge\_entity()**, if the entity that you are updating doesn't exist then the update operation will fail.</span></span> <span data-ttu-id="93175-148">Proto pokud chcete uložit entity bez ohledu na to, jestli už existuje, měli byste místo toho použít **vložit\_nebo\_nahradit\_entity()** nebo **vložit\_nebo\_sloučení\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="93175-148">Therefore if you wish to store an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="93175-149">Práce se skupinami entit</span><span class="sxs-lookup"><span data-stu-id="93175-149">Work with groups of entities</span></span>
<span data-ttu-id="93175-150">Někdy má smysl odeslat více operací společně v dávce zajistit atomic zpracování serverem.</span><span class="sxs-lookup"><span data-stu-id="93175-150">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="93175-151">To provést, je třeba nejprve vytvořit **Batch** objekt a potom pomocí **provést\_batch()** metodu **TableService**.</span><span class="sxs-lookup"><span data-stu-id="93175-151">To accomplish that, you first create a **Batch** object and then use the **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="93175-152">Následující příklad ukazuje, odesílání dvě entity s RowKey 2 a 3 v dávce.</span><span class="sxs-lookup"><span data-stu-id="93175-152">The following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="93175-153">Všimněte si, že funguje pouze u entit s stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="93175-153">Notice that it only works for entities with the same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="93175-154">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="93175-154">Query for an entity</span></span>
<span data-ttu-id="93175-155">Chcete-li prohledávat entitu v tabulce, použijte **získat\_entity()** metoda předáním názvu tabulky **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="93175-155">To query an entity in a table, use the **get\_entity()** method, by passing the table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="93175-156">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="93175-156">Query a set of entities</span></span>
<span data-ttu-id="93175-157">Chcete-li prohledávat sady entit v tabulce, vytvoření objektu hash dotazu a použijte **dotazu\_entities()** metoda.</span><span class="sxs-lookup"><span data-stu-id="93175-157">To query a set of entities in a table, create a query hash object and use the **query\_entities()** method.</span></span> <span data-ttu-id="93175-158">Následující příklad ukazuje získání všech entit se stejným **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="93175-158">The following example demonstrates getting all the entities with the same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="93175-159">Je-li nastavit výsledek je příliš velký pro jediný dotaz vrátit, které můžete použít k načtení další stránky vrátí token pokračování.</span><span class="sxs-lookup"><span data-stu-id="93175-159">If the result set is too large for a single query to return, a continuation token will be returned which you can use to retrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="93175-160">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="93175-160">Query a subset of entity properties</span></span>
<span data-ttu-id="93175-161">Dotaz na tabulku můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="93175-161">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="93175-162">Tímto způsobem, nazývá "projekce" omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="93175-162">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="93175-163">Použijte klauzuli select a předejte názvy vlastností, které chcete převést klientovi.</span><span class="sxs-lookup"><span data-stu-id="93175-163">Use the select clause and pass the names of the properties you would like to bring over to the client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="93175-164">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="93175-164">Delete an entity</span></span>
<span data-ttu-id="93175-165">Pokud chcete odstranit entitu, použijte **odstranit\_entity()** metoda.</span><span class="sxs-lookup"><span data-stu-id="93175-165">To delete an entity, use the **delete\_entity()** method.</span></span> <span data-ttu-id="93175-166">Je třeba předat názvu tabulky a který obsahuje entitu, PartitionKey a RowKey entity.</span><span class="sxs-lookup"><span data-stu-id="93175-166">You need to pass in the name of the table which contains the entity, the PartitionKey and RowKey of the entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="93175-167">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="93175-167">Delete a table</span></span>
<span data-ttu-id="93175-168">Pokud chcete odstranit tabulku, použijte **odstranit\_table()** metoda a předejte jí název tabulky, které chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="93175-168">To delete a table, use the **delete\_table()** method and pass in the name of the table you want to delete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="93175-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93175-169">Next steps</span></span>

* <span data-ttu-id="93175-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.</span><span class="sxs-lookup"><span data-stu-id="93175-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="93175-171">[Azure SDK pro Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="93175-171">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

