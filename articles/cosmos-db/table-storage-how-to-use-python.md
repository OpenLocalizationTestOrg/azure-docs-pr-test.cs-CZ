---
title: "Jak používat Azure Table storage s Pythonem | Microsoft Docs"
description: "Ukládejte si strukturovaná data v cloudu pomocí Azure Table Storage, úložiště dat typu NoSQL."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.openlocfilehash: 0c46f04786ba4b62bd7ca22c5e25643123e6e136
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-in-python"></a><span data-ttu-id="2ba4c-103">Postup používání úložiště Table v Pythonu</span><span class="sxs-lookup"><span data-stu-id="2ba4c-103">How to use Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="2ba4c-104">Tento průvodce vám ukáže, jak provádět běžné scénáře Azure Table storage pomocí Python [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="2ba4c-104">This guide shows you how to perform common Azure Table storage scenarios in Python using the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="2ba4c-105">Pokryté scénáře zahrnují vytváření a odstraňování tabulek a vkládání a dotazování entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-105">The scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="2ba4c-106">Při práci prostřednictvím scénáře v tomto kurzu, budete pravděpodobně chtít odkazovat [sada SDK úložiště Azure pro referenční dokumentace rozhraní API Python](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="2ba4c-106">While working through the scenarios in this tutorial, you may wish to refer to the [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a><span data-ttu-id="2ba4c-107">Nainstalovat úložiště Azure SDK pro Python</span><span class="sxs-lookup"><span data-stu-id="2ba4c-107">Install the Azure Storage SDK for Python</span></span>

<span data-ttu-id="2ba4c-108">Po vytvoření účtu úložiště, je dalším krokem k instalaci [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="2ba4c-108">Once you've created a storage account, your next step is to install the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="2ba4c-109">Podrobnosti o instalaci sady SDK najdete [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) soubor v sadě SDK úložiště pro Python úložišti na Githubu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-109">For details on installing the SDK, refer to the [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in the Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="2ba4c-110">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="2ba4c-110">Create a table</span></span>

<span data-ttu-id="2ba4c-111">Pro práci se službou Azure Table v Python, je nutné naimportovat [TableService] [ py_TableService] modulu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-111">To work with the Azure Table service in Python, you must import the [TableService][py_TableService] module.</span></span> <span data-ttu-id="2ba4c-112">Vzhledem k tomu, že je budete pracovat s entity tabulky, musíte taky [Entity] [ py_Entity] třídy.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-112">Since you'll be working with Table entities, you also need the [Entity][py_Entity] class.</span></span> <span data-ttu-id="2ba4c-113">Přidejte tento kód v horní Python soubor k importu obě:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-113">Add this code near the top your Python file to import both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="2ba4c-114">Vytvoření [TableService] [ py_TableService] objekt, předávání v klíč účet a název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="2ba4c-115">Nahraďte `myaccount` a `mykey` se název účtu a klíč a volání [create_table] [ py_create_table] pro vytvoření tabulky ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] to create the table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="2ba4c-116">Přidání entity do tabulky</span><span class="sxs-lookup"><span data-stu-id="2ba4c-116">Add an entity to a table</span></span>

<span data-ttu-id="2ba4c-117">Pokud chcete přidat entitu, nejprve vytvořit objekt, který představuje vaše entitu a předat objekt, který má [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-117">To add an entity, you first create an object that represents your entity, then pass the object to the [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="2ba4c-118">Objekt entity může být slovník nebo objekt typu [Entity][py_Entity]a definuje vlastnost názvy a hodnoty vaší entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-118">The entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="2ba4c-119">Každou entitu musíte zahrnout požadované [PartitionKey a RowKey](#partitionkey-and-rowkey) vlastnosti kromě všechny vlastnosti, které definujete pro entitu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-119">Every entity must include the required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition to any other properties you define for the entity.</span></span>

<span data-ttu-id="2ba4c-120">Tento příklad vytvoří objekt Slovník reprezentující entitu, potom předává jej do [insert_entity] [ py_insert_entity] metody přidat do tabulky:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-120">This example creates a dictionary object representing an entity, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="2ba4c-121">Tento příklad vytvoří [Entity] [ py_Entity] objekt a potom předává jej do [insert_entity] [ py_insert_entity] metody přidat do tabulky:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-121">This example creates an [Entity][py_Entity] object, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="2ba4c-122">PartitionKey a RowKey</span><span class="sxs-lookup"><span data-stu-id="2ba4c-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="2ba4c-123">Musíte zadat oba **PartitionKey** a **RowKey** vlastnosti pro každou entitu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="2ba4c-124">Toto jsou jedinečné identifikátory entity, jako společně tvoří primární klíč entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-124">These are the unique identifiers of your entities, as together they form the primary key of an entity.</span></span> <span data-ttu-id="2ba4c-125">Pomocí těchto hodnot mnohem rychlejší než dalších vlastností entity můžete dotazovat, protože pouze tyto vlastnosti jsou indexované se můžete dotazovat.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="2ba4c-126">Použití služby Table **PartitionKey** inteligentně entity tabulky distribuovat mezi uzly úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-126">The Table service uses **PartitionKey** to intelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="2ba4c-127">Entity, které mají stejnou **PartitionKey** jsou uložené na stejném uzlu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-127">Entities that have the same  **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="2ba4c-128">**RowKey** je jedinečný Identifikátor entity v oddílu patří do.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-128">**RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="2ba4c-129">Aktualizace entity</span><span class="sxs-lookup"><span data-stu-id="2ba4c-129">Update an entity</span></span>

<span data-ttu-id="2ba4c-130">Chcete-li aktualizovat všechny hodnoty vlastností entity, zavolejte [update_entity] [ py_update_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-130">To update all of an entity's property values, call the [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="2ba4c-131">Tento příklad ukazuje, jak nahradit stávající entity aktualizovaná verze:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-131">This example shows how to replace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="2ba4c-132">Pokud ještě neexistuje typ entity, která se právě aktualizuje, se nezdaří operace aktualizace.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-132">If the entity that is being updated doesn't already exist, then the update operation will fail.</span></span> <span data-ttu-id="2ba4c-133">Pokud chcete uložit entity toho, zda existuje nebo Ne, použít [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="2ba4c-133">If you want to store an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="2ba4c-134">V následujícím příkladu první volání nahradí stávající entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-134">In the following example, the first call will replace the existing entity.</span></span> <span data-ttu-id="2ba4c-135">Druhé volání bude vložit novou entitu, protože žádná entita s zadaný klíč oddílu a RowKey existuje v tabulce.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-135">The second call will insert a new entity, since no entity with the specified PartitionKey and RowKey exists in the table.</span></span>

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="2ba4c-136">[Update_entity] [ py_update_entity] metoda nahrazuje všechny vlastnosti a hodnoty existující entity, který můžete také použít k odebrání vlastnosti z existující entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-136">The [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use to remove properties from an existing entity.</span></span> <span data-ttu-id="2ba4c-137">Můžete použít [merge_entity] [ py_merge_entity] metodu za účelem aktualizace stávající entity s hodnotami vlastností nové nebo upravené bez úplného nahrazení entity.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-137">You can use the [merge_entity][py_merge_entity] method to update an existing entity with new or modified property values without completely replacing the entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="2ba4c-138">Úprava více entit</span><span class="sxs-lookup"><span data-stu-id="2ba4c-138">Modify multiple entities</span></span>

<span data-ttu-id="2ba4c-139">Aby atomic zpracování požadavku ve službě Table, můžete odeslat více operací společně v dávce.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-139">To ensure the atomic processing of a request by the Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="2ba4c-140">Nejprve pomocí [TableBatch] [ py_TableBatch] třídu a přidejte k jedné dávkové operace více.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-140">First, use the [TableBatch][py_TableBatch] class to add multiple operations to a single batch.</span></span> <span data-ttu-id="2ba4c-141">Pak zavolejte [TableService][py_TableService].[ commit_batch] [ py_commit_batch] k odeslání operace v atomické operace.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] to submit the operations in an atomic operation.</span></span> <span data-ttu-id="2ba4c-142">Všechny entity má být změněn ve službě batch musí být ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-142">All entities to be modified in batch must be in the same partition.</span></span>

<span data-ttu-id="2ba4c-143">Tento příklad přidá dvě entity společně v dávce:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="2ba4c-144">Dávky můžete použít taky s syntaxe kontextu správce:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-144">Batches can also be used with the context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="2ba4c-145">Dotaz pro entitu</span><span class="sxs-lookup"><span data-stu-id="2ba4c-145">Query for an entity</span></span>

<span data-ttu-id="2ba4c-146">Dotaz pro entitu v tabulce, předat jeho PartitionKey a RowKey k [TableService][py_TableService].[ get_entity] [ py_get_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-146">To query for an entity in a table, pass its PartitionKey and RowKey to the [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="2ba4c-147">Dotaz na sadu entit</span><span class="sxs-lookup"><span data-stu-id="2ba4c-147">Query a set of entities</span></span>

<span data-ttu-id="2ba4c-148">Můžete zadat dotaz pro sadu entit zadáním řetězec filtru s **filtru** parametr.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-148">You can query for a set of entities by supplying a filter string with the **filter** parameter.</span></span> <span data-ttu-id="2ba4c-149">Tento příklad vyhledá všechny úkoly v Praze použitím filtru na PartitionKey:</span><span class="sxs-lookup"><span data-stu-id="2ba4c-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="2ba4c-150">Dotaz na podmnožinu vlastností entity</span><span class="sxs-lookup"><span data-stu-id="2ba4c-150">Query a subset of entity properties</span></span>

<span data-ttu-id="2ba4c-151">Můžete taky omezit vlastnosti, které se vrátí pro každou entitu v dotazu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="2ba4c-152">Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entit nebo sad výsledků dotazu.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="2ba4c-153">Použití **vyberte** parametr a předejte jí názvy vlastností, které chcete vrácen do klienta.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-153">Use the **select** parameter and pass the names of the properties you want returned to the client.</span></span>

<span data-ttu-id="2ba4c-154">Dotaz v následujícím kódu vrátí pouze popisy entit v tabulce.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-154">The query in the following code returns only the descriptions of entities in the table.</span></span>

> [!NOTE]
> <span data-ttu-id="2ba4c-155">Následující fragment kódu funguje pouze s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-155">The following snippet works only against the Azure Storage.</span></span> <span data-ttu-id="2ba4c-156">Emulátor úložiště není podporován.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-156">It is not supported by the storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="2ba4c-157">Odstranění entity</span><span class="sxs-lookup"><span data-stu-id="2ba4c-157">Delete an entity</span></span>

<span data-ttu-id="2ba4c-158">Odstranění entity předáním jeho PartitionKey a RowKey k [delete_entity] [ py_delete_entity] metoda.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-158">Delete an entity by passing its PartitionKey and RowKey to the [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="2ba4c-159">Odstranění tabulky</span><span class="sxs-lookup"><span data-stu-id="2ba4c-159">Delete a table</span></span>

<span data-ttu-id="2ba4c-160">Pokud již nepotřebujete pro tabulku nebo některé z entit v něm, zavolejte [delete_table] [ py_delete_table] metoda trvale odstranit z úložiště Azure v tabulce.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-160">If you no longer need a table or any of the entities within it, call the [delete_table][py_delete_table] method to permanently delete the table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="2ba4c-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ba4c-161">Next steps</span></span>

* [<span data-ttu-id="2ba4c-162">Azure SDK úložiště pro Python API – referenční informace</span><span class="sxs-lookup"><span data-stu-id="2ba4c-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="2ba4c-163">Úložiště Azure SDK pro jazyk Python</span><span class="sxs-lookup"><span data-stu-id="2ba4c-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="2ba4c-164">Středisko pro vývojáře programující v Pythonu</span><span class="sxs-lookup"><span data-stu-id="2ba4c-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="2ba4c-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): bezplatná aplikace a platformy pro vizuální práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="2ba4c-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

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
