---
title: aaaHow toouse Azure Table storage s Pythonem | Microsoft Docs
description: "Ukládejte si strukturovaná data v cloudu hello pomocí Azure Table storage, úložiště dat typu NoSQL."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: marsma
ms.openlocfilehash: fd0e1b05cc12618f348eaf2d85d0dce5ac32702a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-in-python"></a><span data-ttu-id="bca41-103">Jak toouse úložiště tabulek v Pythonu</span><span class="sxs-lookup"><span data-stu-id="bca41-103">How toouse Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="bca41-104">Tento průvodce vám ukáže, jak tooperform běžné Azure Table storage scénáře v Pythonu pomocí hello [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="bca41-104">This guide shows you how tooperform common Azure Table storage scenarios in Python using hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="bca41-105">Hello pokryté scénáře zahrnují vytváření a odstraňování tabulek a vkládání a dotazování entity.</span><span class="sxs-lookup"><span data-stu-id="bca41-105">hello scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="bca41-106">Při práci prostřednictvím hello scénáře v tomto kurzu můžete toorefer toohello [sada SDK úložiště Azure pro referenční dokumentace rozhraní API Python](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="bca41-106">While working through hello scenarios in this tutorial, you may wish toorefer toohello [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a><span data-ttu-id="bca41-107">Nainstalujte hello sada SDK úložiště Azure pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="bca41-107">Install hello Azure Storage SDK for Python</span></span>

<span data-ttu-id="bca41-108">Po vytvoření účtu úložiště, dalším krokem je tooinstall hello [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="bca41-108">Once you've created a storage account, your next step is tooinstall hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="bca41-109">Podrobnosti o instalaci hello SDK, naleznete toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) souboru v hello sada SDK úložiště pro Python úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="bca41-109">For details on installing hello SDK, refer toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in hello Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="bca41-110">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="bca41-110">Create a table</span></span>

<span data-ttu-id="bca41-111">toowork s hello služby Azure Table v Pythonu, je nutné naimportovat hello [TableService] [ py_TableService] modulu.</span><span class="sxs-lookup"><span data-stu-id="bca41-111">toowork with hello Azure Table service in Python, you must import hello [TableService][py_TableService] module.</span></span> <span data-ttu-id="bca41-112">Vzhledem k tomu, že je budete pracovat s entity tabulky, musíte taky hello [Entity] [ py_Entity] třídy.</span><span class="sxs-lookup"><span data-stu-id="bca41-112">Since you'll be working with Table entities, you also need hello [Entity][py_Entity] class.</span></span> <span data-ttu-id="bca41-113">Přidejte tento kód v horní hello vaší Python souboru tooimport obou:</span><span class="sxs-lookup"><span data-stu-id="bca41-113">Add this code near hello top your Python file tooimport both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="bca41-114">Vytvoření [TableService] [ py_TableService] objekt, předávání v klíč účet a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bca41-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="bca41-115">Nahraďte `myaccount` a `mykey` se název účtu a klíč a volání [create_table] [ py_create_table] toocreate hello tabulky ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bca41-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] toocreate hello table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="bca41-116">Přidat tooa tabulka entity</span><span class="sxs-lookup"><span data-stu-id="bca41-116">Add an entity tooa table</span></span>

<span data-ttu-id="bca41-117">tooadd entity, je nejprve vytvořit objekt, který představuje entitu, pak předejte hello objekt toohello [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="bca41-117">tooadd an entity, you first create an object that represents your entity, then pass hello object toohello [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="bca41-118">objekt entity Hello může být slovník nebo objekt typu [Entity][py_Entity]a definuje vlastnost názvy a hodnoty vaší entity.</span><span class="sxs-lookup"><span data-stu-id="bca41-118">hello entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="bca41-119">Každou entitu musí zahrnovat hello požadované [PartitionKey a RowKey](#partitionkey-and-rowkey) vlastností v tooany přidání dalších vlastností definujete pro entitu hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-119">Every entity must include hello required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition tooany other properties you define for hello entity.</span></span>

<span data-ttu-id="bca41-120">Tento příklad vytvoří objekt slovníku reprezentující entitu, pak předá ho toohello [insert_entity] [ py_insert_entity] tooadd metoda ji toohello tabulky:</span><span class="sxs-lookup"><span data-stu-id="bca41-120">This example creates a dictionary object representing an entity, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="bca41-121">Tento příklad vytvoří [Entity] [ py_Entity] objektu, a poté ho předá toohello [insert_entity] [ py_insert_entity] tooadd metoda ji toohello tabulky:</span><span class="sxs-lookup"><span data-stu-id="bca41-121">This example creates an [Entity][py_Entity] object, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="bca41-122">PartitionKey a RowKey</span><span class="sxs-lookup"><span data-stu-id="bca41-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="bca41-123">Musíte zadat oba **PartitionKey** a **RowKey** vlastnosti pro každou entitu.</span><span class="sxs-lookup"><span data-stu-id="bca41-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="bca41-124">Toto jsou jedinečné identifikátory hello entit, jako společně tvoří hello primární klíč entity.</span><span class="sxs-lookup"><span data-stu-id="bca41-124">These are hello unique identifiers of your entities, as together they form hello primary key of an entity.</span></span> <span data-ttu-id="bca41-125">Pomocí těchto hodnot mnohem rychlejší než dalších vlastností entity můžete dotazovat, protože pouze tyto vlastnosti jsou indexované se můžete dotazovat.</span><span class="sxs-lookup"><span data-stu-id="bca41-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="bca41-126">Hello používá služby Table **PartitionKey** toointelligently entity tabulky distribuovat mezi uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="bca41-126">hello Table service uses **PartitionKey** toointelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="bca41-127">Entity, které mají stejný hello **PartitionKey** jsou uložené na hello stejného uzlu.</span><span class="sxs-lookup"><span data-stu-id="bca41-127">Entities that have hello same  **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="bca41-128">**RowKey** je jedinečné ID hello hello entity v rámci oddílu hello patří do.</span><span class="sxs-lookup"><span data-stu-id="bca41-128">**RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="bca41-129">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="bca41-129">Update an entity</span></span>

<span data-ttu-id="bca41-130">tooupdate všech hodnot vlastností entity volání hello [update_entity] [ py_update_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="bca41-130">tooupdate all of an entity's property values, call hello [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="bca41-131">Tento příklad ukazuje, jak tooreplace stávající entity aktualizovanou verzí:</span><span class="sxs-lookup"><span data-stu-id="bca41-131">This example shows how tooreplace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="bca41-132">Pokud ještě neexistuje hello entity, který se právě aktualizuje, se nezdaří operace aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-132">If hello entity that is being updated doesn't already exist, then hello update operation will fail.</span></span> <span data-ttu-id="bca41-133">Pokud chcete toostore entity, zda existuje nebo Ne, použijte [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="bca41-133">If you want toostore an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="bca41-134">V následujícím příkladu hello první volání hello nahradí stávající entity hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-134">In hello following example, hello first call will replace hello existing entity.</span></span> <span data-ttu-id="bca41-135">druhé volání Hello vloží novou entitu, protože není zadané žádné entity s hello PartitionKey a RowKey existuje v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-135">hello second call will insert a new entity, since no entity with hello specified PartitionKey and RowKey exists in hello table.</span></span>

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="bca41-136">Hello [update_entity] [ py_update_entity] metoda nahrazuje všechny vlastnosti a hodnoty existující entity, které můžete také použít tooremove vlastnosti z existující entity.</span><span class="sxs-lookup"><span data-stu-id="bca41-136">hello [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use tooremove properties from an existing entity.</span></span> <span data-ttu-id="bca41-137">Můžete použít hello [merge_entity] [ py_merge_entity] metoda tooupdate stávající entitu s hodnotami vlastností nové nebo upravené bez úplného nahrazení hello entity.</span><span class="sxs-lookup"><span data-stu-id="bca41-137">You can use hello [merge_entity][py_merge_entity] method tooupdate an existing entity with new or modified property values without completely replacing hello entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="bca41-138">Úprava více entit</span><span class="sxs-lookup"><span data-stu-id="bca41-138">Modify multiple entities</span></span>

<span data-ttu-id="bca41-139">tooensure hello atomic zpracování požadavku službou hello tabulky, můžete odeslat více operací společně v dávce.</span><span class="sxs-lookup"><span data-stu-id="bca41-139">tooensure hello atomic processing of a request by hello Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="bca41-140">Nejprve pomocí hello [TableBatch] [ py_TableBatch] třídy tooadd více jedné dávkové operace tooa.</span><span class="sxs-lookup"><span data-stu-id="bca41-140">First, use hello [TableBatch][py_TableBatch] class tooadd multiple operations tooa single batch.</span></span> <span data-ttu-id="bca41-141">Pak zavolejte [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello operace v atomické operace.</span><span class="sxs-lookup"><span data-stu-id="bca41-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] toosubmit hello operations in an atomic operation.</span></span> <span data-ttu-id="bca41-142">Všechny entity toobe upravit ve službě batch musí být v hello stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="bca41-142">All entities toobe modified in batch must be in hello same partition.</span></span>

<span data-ttu-id="bca41-143">Tento příklad přidá dvě entity společně v dávce:</span><span class="sxs-lookup"><span data-stu-id="bca41-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="bca41-144">Dávky můžete použít taky s hello kontextu manager syntaxe:</span><span class="sxs-lookup"><span data-stu-id="bca41-144">Batches can also be used with hello context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="bca41-145">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="bca41-145">Query for an entity</span></span>

<span data-ttu-id="bca41-146">tooquery pro entitu v tabulce předávání jeho PartitionKey a RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="bca41-146">tooquery for an entity in a table, pass its PartitionKey and RowKey toohello [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="bca41-147">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="bca41-147">Query a set of entities</span></span>

<span data-ttu-id="bca41-148">Můžete zadat dotaz pro sadu entit zadáním řetězec filtru s hello **filtru** parametr.</span><span class="sxs-lookup"><span data-stu-id="bca41-148">You can query for a set of entities by supplying a filter string with hello **filter** parameter.</span></span> <span data-ttu-id="bca41-149">Tento příklad vyhledá všechny úkoly v Praze použitím filtru na PartitionKey:</span><span class="sxs-lookup"><span data-stu-id="bca41-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="bca41-150">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="bca41-150">Query a subset of entity properties</span></span>

<span data-ttu-id="bca41-151">Můžete taky omezit vlastnosti, které se vrátí pro každou entitu v dotazu.</span><span class="sxs-lookup"><span data-stu-id="bca41-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="bca41-152">Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entit nebo sad výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="bca41-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="bca41-153">Použití hello **vyberte** parametr a předejte jí hello názvy vlastností hello chcete, aby vrátila toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="bca41-153">Use hello **select** parameter and pass hello names of hello properties you want returned toohello client.</span></span>

<span data-ttu-id="bca41-154">dotaz Hello v hello následující kód vrátí pouze hello popisy entit v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-154">hello query in hello following code returns only hello descriptions of entities in hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="bca41-155">Následující fragment kódu funguje pouze s hello Azure Storage Hello.</span><span class="sxs-lookup"><span data-stu-id="bca41-155">hello following snippet works only against hello Azure Storage.</span></span> <span data-ttu-id="bca41-156">Emulátor úložiště hello není podporován.</span><span class="sxs-lookup"><span data-stu-id="bca41-156">It is not supported by hello storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="bca41-157">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="bca41-157">Delete an entity</span></span>

<span data-ttu-id="bca41-158">Odstranění entity předáním jeho PartitionKey a RowKey toohello [delete_entity] [ py_delete_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="bca41-158">Delete an entity by passing its PartitionKey and RowKey toohello [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="bca41-159">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="bca41-159">Delete a table</span></span>

<span data-ttu-id="bca41-160">Pokud již nepotřebujete pro tabulku nebo žádné hello entit v něm, zavolejte hello [delete_table] [ py_delete_table] metoda toopermanently odstranit tabulku hello ze služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bca41-160">If you no longer need a table or any of hello entities within it, call hello [delete_table][py_delete_table] method toopermanently delete hello table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="bca41-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bca41-161">Next steps</span></span>

* [<span data-ttu-id="bca41-162">Azure SDK úložiště pro Python API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="bca41-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="bca41-163">Úložiště Azure SDK pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="bca41-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="bca41-164">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="bca41-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="bca41-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): bezplatná aplikace a platformy pro vizuální práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="bca41-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch
