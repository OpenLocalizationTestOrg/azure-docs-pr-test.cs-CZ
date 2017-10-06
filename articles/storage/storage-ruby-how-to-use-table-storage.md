---
title: "aaaHow toouse úložiště Azure Table z Ruby | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 9c77ff9f384a776c9bc075b60b351685c61acc36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a><span data-ttu-id="8072e-103">Jak toouse úložiště Azure Table z Ruby</span><span class="sxs-lookup"><span data-stu-id="8072e-103">How toouse Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="8072e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8072e-104">Overview</span></span>
<span data-ttu-id="8072e-105">Tento průvodce vám ukáže, jak hello tooperform běžné scénáře s využitím služby Azure Table.</span><span class="sxs-lookup"><span data-stu-id="8072e-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="8072e-106">Ukázky Hello jsou zapsány pomocí hello Ruby rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8072e-106">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="8072e-107">Hello pokryté scénáře zahrnují **vytváření a odstraňování tabulek, vkládání a dotazování entity v tabulce**.</span><span class="sxs-lookup"><span data-stu-id="8072e-107">hello scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="8072e-108">Vytvoření Ruby aplikace</span><span class="sxs-lookup"><span data-stu-id="8072e-108">Create a Ruby application</span></span>
<span data-ttu-id="8072e-109">Pokyny jak toocreate Ruby aplikace, najdete v části [Ruby, na které webové aplikace ve virtuálním počítači Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="8072e-109">For instructions how toocreate a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="8072e-110">Konfigurace vaší aplikace tooaccess úložiště</span><span class="sxs-lookup"><span data-stu-id="8072e-110">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="8072e-111">toouse Azure Storage, potřebujete toodownload a použití hello Ruby balíček azure, který obsahuje sadu knihoven pohodlí, které komunikují s hello REST úložiště služby.</span><span class="sxs-lookup"><span data-stu-id="8072e-111">toouse Azure Storage, you need toodownload and use hello Ruby azure package which includes a set of convenience libraries that communicate with hello Storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="8072e-112">Použijte RubyGems tooobtain hello balíček</span><span class="sxs-lookup"><span data-stu-id="8072e-112">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="8072e-113">Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="8072e-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="8072e-114">Typ **gem instalace azure** v hello příkazového okna tooinstall hello gem a závislosti.</span><span class="sxs-lookup"><span data-stu-id="8072e-114">Type **gem install azure** in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="8072e-115">Importovat balíček hello</span><span class="sxs-lookup"><span data-stu-id="8072e-115">Import hello package</span></span>
<span data-ttu-id="8072e-116">Použít svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby souboru, kde chcete toouse úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="8072e-116">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="8072e-117">Nastavit připojení k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8072e-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="8072e-118">Hello modulu azure, bude číst proměnné prostředí hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč**informace vyžaduje účet služby Azure Storage tooyour tooconnect.</span><span class="sxs-lookup"><span data-stu-id="8072e-118">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required tooconnect tooyour Azure Storage account.</span></span> <span data-ttu-id="8072e-119">Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu hello před použitím **Azure::TableService** s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="8072e-119">If these environment variables are not set, you must specify hello account information before using **Azure::TableService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="8072e-120">tooobtain tyto hodnoty ze Správce prostředků úložiště nebo klasický účet v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="8072e-120">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="8072e-121">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8072e-121">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8072e-122">Přejděte toohello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="8072e-122">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="8072e-123">V okně Nastavení hello na hello správné, klikněte na tlačítko **přístupové klíče**.</span><span class="sxs-lookup"><span data-stu-id="8072e-123">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="8072e-124">V okně klíče přístup hello který se zobrazí uvidíte hello přístupový klíč 1 a 2 přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="8072e-124">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="8072e-125">Můžete použít kteroukoli z nich.</span><span class="sxs-lookup"><span data-stu-id="8072e-125">You can use either of these.</span></span>
5. <span data-ttu-id="8072e-126">Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="8072e-126">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

<span data-ttu-id="8072e-127">tooobtain tyto hodnoty z úložiště classic účet na portálu Azure classic hello:</span><span class="sxs-lookup"><span data-stu-id="8072e-127">tooobtain these values from a classic storage account in hello classic Azure portal:</span></span>

1. <span data-ttu-id="8072e-128">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="8072e-128">Log in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="8072e-129">Přejděte toohello úložiště účet, že který má toouse.</span><span class="sxs-lookup"><span data-stu-id="8072e-129">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="8072e-130">Klikněte na tlačítko **SPRAVOVAT přístupové klíče** na hello dolní části navigačního podokna hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-130">Click **MANAGE ACCESS KEYS** at hello bottom of hello navigation pane.</span></span>
4. <span data-ttu-id="8072e-131">V místním dialogovém okně hello zobrazí se název účtu úložiště hello, primární přístupový klíč a sekundární přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="8072e-131">In hello pop-up dialog, you'll see hello storage account name, primary access key and secondary access key.</span></span> <span data-ttu-id="8072e-132">Pro přístupový klíč můžete použít buď hello primární nebo sekundární jeden hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-132">For access key, you can use either hello primary one or hello secondary one.</span></span>
5. <span data-ttu-id="8072e-133">Klikněte na tlačítko hello kopírování ikonu toocopy hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="8072e-133">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="8072e-134">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="8072e-134">Create a table</span></span>
<span data-ttu-id="8072e-135">Hello **Azure::TableService** objekt vám umožňuje spolupracovat s tabulkami a entity.</span><span class="sxs-lookup"><span data-stu-id="8072e-135">hello **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="8072e-136">toocreate tabulky, použijte hello **vytvořit\_table()** metoda.</span><span class="sxs-lookup"><span data-stu-id="8072e-136">toocreate a table, use hello **create\_table()** method.</span></span> <span data-ttu-id="8072e-137">Hello následující příklad vytvoří tabulku nebo výtisků hello chyby, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="8072e-137">hello following example creates a table or prints hello error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="8072e-138">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="8072e-138">Add an entity tooa table</span></span>
<span data-ttu-id="8072e-139">tooadd entity, nejprve vytvořit objekt algoritmu hash, která definuje vlastnosti vaší entity.</span><span class="sxs-lookup"><span data-stu-id="8072e-139">tooadd an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="8072e-140">Všimněte si, že pro každou entitu musíte zadat **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="8072e-140">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="8072e-141">Tyto jsou hello jedinečné identifikátory vaší entit a jsou hodnoty, které může být dotazován mnohem rychleji než vaše jiných vlastností.</span><span class="sxs-lookup"><span data-stu-id="8072e-141">These are hello unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="8072e-142">Azure Storage používá **PartitionKey** tooautomatically distribuovat entity tabulky hello přes mnoho uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="8072e-142">Azure Storage uses **PartitionKey** tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="8072e-143">Entity s hello stejné **PartitionKey** jsou uložené na hello stejného uzlu.</span><span class="sxs-lookup"><span data-stu-id="8072e-143">Entities with hello same **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="8072e-144">Hello **RowKey** je jedinečné ID hello hello entity v rámci oddílu hello patří do.</span><span class="sxs-lookup"><span data-stu-id="8072e-144">hello **RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="8072e-145">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="8072e-145">Update an entity</span></span>
<span data-ttu-id="8072e-146">Existuje několik metod k dispozici tooupdate stávající entity:</span><span class="sxs-lookup"><span data-stu-id="8072e-146">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="8072e-147">**Aktualizovat\_entity():** aktualizace stávající entity nahrazením ho.</span><span class="sxs-lookup"><span data-stu-id="8072e-147">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="8072e-148">**sloučení\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-148">**merge\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span>
* <span data-ttu-id="8072e-149">**Vložit\_nebo\_sloučení\_entity():** aktualizace stávající entity podle jeho nahrazení.</span><span class="sxs-lookup"><span data-stu-id="8072e-149">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="8072e-150">Pokud žádná entita existuje, bude vložen nový:</span><span class="sxs-lookup"><span data-stu-id="8072e-150">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="8072e-151">**Vložit\_nebo\_nahradit\_entity():** aktualizace stávající entity sloučením nové hodnoty vlastností do stávající entity hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-151">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span> <span data-ttu-id="8072e-152">Pokud existuje žádné entity, se vloží novou.</span><span class="sxs-lookup"><span data-stu-id="8072e-152">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="8072e-153">Hello následující příklad ukazuje, aktualizuje entitu s využitím **aktualizace\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="8072e-153">hello following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="8072e-154">S **aktualizace\_entity()** a **sloučení\_entity()**, pokud neexistuje hello entita, která jsou aktualizace, hello operace aktualizace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8072e-154">With **update\_entity()** and **merge\_entity()**, if hello entity that you are updating doesn't exist then hello update operation will fail.</span></span> <span data-ttu-id="8072e-155">Proto pokud chcete toostore entity bez ohledu na to, jestli už existuje, měli byste místo toho použít **vložit\_nebo\_nahradit\_entity()** nebo **vložit\_nebo \_sloučení\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="8072e-155">Therefore if you wish toostore an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="8072e-156">Práce se skupinami entit</span><span class="sxs-lookup"><span data-stu-id="8072e-156">Work with groups of entities</span></span>
<span data-ttu-id="8072e-157">Někdy má smysl toosubmit více operací společně v batch tooensure atomic zpracování serverem hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-157">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="8072e-158">tooaccomplish, jestli jste nejdřív vytvořili **Batch** objekt a potom pomocí hello **provést\_batch()** metodu **TableService**.</span><span class="sxs-lookup"><span data-stu-id="8072e-158">tooaccomplish that, you first create a **Batch** object and then use hello **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="8072e-159">Hello následující příklad ukazuje odesílání dvě entity s RowKey 2 a 3 v dávce.</span><span class="sxs-lookup"><span data-stu-id="8072e-159">hello following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="8072e-160">Všimněte si to jen funguje u entit s hello stejný klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="8072e-160">Notice that it only works for entities with hello same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="8072e-161">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="8072e-161">Query for an entity</span></span>
<span data-ttu-id="8072e-162">tooquery entitu v tabulce, použijte hello **získat\_entity()** metoda předáním hello název tabulky, **PartitionKey** a **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="8072e-162">tooquery an entity in a table, use hello **get\_entity()** method, by passing hello table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="8072e-163">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="8072e-163">Query a set of entities</span></span>
<span data-ttu-id="8072e-164">tooquery sady entit v tabulce, vytvořit objekt algoritmu hash dotaz a použít hello **dotazu\_entities()** metoda.</span><span class="sxs-lookup"><span data-stu-id="8072e-164">tooquery a set of entities in a table, create a query hash object and use hello **query\_entities()** method.</span></span> <span data-ttu-id="8072e-165">Hello následující příklad ukazuje získání všech entit hello s hello stejné **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="8072e-165">hello following example demonstrates getting all hello entities with hello same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="8072e-166">Pokud sadu výsledků hello je příliš velký pro jeden dotaz tooreturn, token pokračování, bude vrácen který můžete použít následující stránky tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="8072e-166">If hello result set is too large for a single query tooreturn, a continuation token will be returned which you can use tooretrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="8072e-167">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="8072e-167">Query a subset of entity properties</span></span>
<span data-ttu-id="8072e-168">Tabulku tooa dotazu můžete načíst jenom několik z entity.</span><span class="sxs-lookup"><span data-stu-id="8072e-168">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="8072e-169">Tímto způsobem, nazývá "projekce" omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entity.</span><span class="sxs-lookup"><span data-stu-id="8072e-169">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="8072e-170">Použití klauzule select hello a názvy hello průchodu hello vlastnosti, které byste chtěli toobring přes toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="8072e-170">Use hello select clause and pass hello names of hello properties you would like toobring over toohello client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="8072e-171">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="8072e-171">Delete an entity</span></span>
<span data-ttu-id="8072e-172">toodelete entitu, použijte hello **odstranit\_entity()** metoda.</span><span class="sxs-lookup"><span data-stu-id="8072e-172">toodelete an entity, use hello **delete\_entity()** method.</span></span> <span data-ttu-id="8072e-173">Je nutné toopass v názvu hello hello tabulky obsahující hello entita, hello PartitionKey a RowKey hello entity.</span><span class="sxs-lookup"><span data-stu-id="8072e-173">You need toopass in hello name of hello table which contains hello entity, hello PartitionKey and RowKey of hello entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="8072e-174">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="8072e-174">Delete a table</span></span>
<span data-ttu-id="8072e-175">toodelete tabulky, použijte hello **odstranit\_table()** metoda a předejte jí hello název tabulky, které chcete toodelete hello.</span><span class="sxs-lookup"><span data-stu-id="8072e-175">toodelete a table, use hello **delete\_table()** method and pass in hello name of hello table you want toodelete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="8072e-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8072e-176">Next steps</span></span>

* <span data-ttu-id="8072e-177">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, volná, od společnosti Microsoft, která vám umožní toowork vizuálně s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="8072e-177">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="8072e-178">[Azure SDK pro Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu</span><span class="sxs-lookup"><span data-stu-id="8072e-178">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

