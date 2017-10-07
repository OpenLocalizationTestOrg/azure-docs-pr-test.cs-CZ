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
# <a name="how-toouse-table-storage-in-python"></a>Jak toouse úložiště tabulek v Pythonu

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Tento průvodce vám ukáže, jak tooperform běžné Azure Table storage scénáře v Pythonu pomocí hello [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python). Hello pokryté scénáře zahrnují vytváření a odstraňování tabulek a vkládání a dotazování entity.

Při práci prostřednictvím hello scénáře v tomto kurzu můžete toorefer toohello [sada SDK úložiště Azure pro referenční dokumentace rozhraní API Python](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a>Nainstalujte hello sada SDK úložiště Azure pro jazyk Python

Po vytvoření účtu úložiště, dalším krokem je tooinstall hello [Microsoft Azure SDK úložiště pro jazyk Python](https://github.com/Azure/azure-storage-python). Podrobnosti o instalaci hello SDK, naleznete toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) souboru v hello sada SDK úložiště pro Python úložišti na Githubu.

## <a name="create-a-table"></a>Vytvoření tabulky

toowork s hello služby Azure Table v Pythonu, je nutné naimportovat hello [TableService] [ py_TableService] modulu. Vzhledem k tomu, že je budete pracovat s entity tabulky, musíte taky hello [Entity] [ py_Entity] třídy. Přidejte tento kód v horní hello vaší Python souboru tooimport obou:

```python
from azure.storage.table import TableService, Entity
```

Vytvoření [TableService] [ py_TableService] objekt, předávání v klíč účet a název účtu úložiště. Nahraďte `myaccount` a `mykey` se název účtu a klíč a volání [create_table] [ py_create_table] toocreate hello tabulky ve službě Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a>Přidat tooa tabulka entity

tooadd entity, je nejprve vytvořit objekt, který představuje entitu, pak předejte hello objekt toohello [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metoda. objekt entity Hello může být slovník nebo objekt typu [Entity][py_Entity]a definuje vlastnost názvy a hodnoty vaší entity. Každou entitu musí zahrnovat hello požadované [PartitionKey a RowKey](#partitionkey-and-rowkey) vlastností v tooany přidání dalších vlastností definujete pro entitu hello.

Tento příklad vytvoří objekt slovníku reprezentující entitu, pak předá ho toohello [insert_entity] [ py_insert_entity] tooadd metoda ji toohello tabulky:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Tento příklad vytvoří [Entity] [ py_Entity] objektu, a poté ho předá toohello [insert_entity] [ py_insert_entity] tooadd metoda ji toohello tabulky:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey a RowKey

Musíte zadat oba **PartitionKey** a **RowKey** vlastnosti pro každou entitu. Toto jsou jedinečné identifikátory hello entit, jako společně tvoří hello primární klíč entity. Pomocí těchto hodnot mnohem rychlejší než dalších vlastností entity můžete dotazovat, protože pouze tyto vlastnosti jsou indexované se můžete dotazovat.

Hello používá služby Table **PartitionKey** toointelligently entity tabulky distribuovat mezi uzly úložiště. Entity, které mají stejný hello **PartitionKey** jsou uložené na hello stejného uzlu. **RowKey** je jedinečné ID hello hello entity v rámci oddílu hello patří do.

## <a name="update-an-entity"></a>Aktualizace entity

tooupdate všech hodnot vlastností entity volání hello [update_entity] [ py_update_entity] metoda. Tento příklad ukazuje, jak tooreplace stávající entity aktualizovanou verzí:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Pokud ještě neexistuje hello entity, který se právě aktualizuje, se nezdaří operace aktualizace hello. Pokud chcete toostore entity, zda existuje nebo Ne, použijte [insert_or_replace_entity][py_insert_or_replace_entity]. V následujícím příkladu hello první volání hello nahradí stávající entity hello. druhé volání Hello vloží novou entitu, protože není zadané žádné entity s hello PartitionKey a RowKey existuje v tabulce hello.

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> Hello [update_entity] [ py_update_entity] metoda nahrazuje všechny vlastnosti a hodnoty existující entity, které můžete také použít tooremove vlastnosti z existující entity. Můžete použít hello [merge_entity] [ py_merge_entity] metoda tooupdate stávající entitu s hodnotami vlastností nové nebo upravené bez úplného nahrazení hello entity.

## <a name="modify-multiple-entities"></a>Úprava více entit

tooensure hello atomic zpracování požadavku službou hello tabulky, můžete odeslat více operací společně v dávce. Nejprve pomocí hello [TableBatch] [ py_TableBatch] třídy tooadd více jedné dávkové operace tooa. Pak zavolejte [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello operace v atomické operace. Všechny entity toobe upravit ve službě batch musí být v hello stejného oddílu.

Tento příklad přidá dvě entity společně v dávce:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Dávky můžete použít taky s hello kontextu manager syntaxe:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Dotaz pro entitu

tooquery pro entitu v tabulce předávání jeho PartitionKey a RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metoda.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Dotaz na sadu entit

Můžete zadat dotaz pro sadu entit zadáním řetězec filtru s hello **filtru** parametr. Tento příklad vyhledá všechny úkoly v Praze použitím filtru na PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Dotaz na podmnožinu vlastností entity

Můžete taky omezit vlastnosti, které se vrátí pro každou entitu v dotazu. Tento postup volá *projekce*, omezuje šířku pásma a může zlepšit výkon dotazů, hlavně pro velké entit nebo sad výsledků dotazu. Použití hello **vyberte** parametr a předejte jí hello názvy vlastností hello chcete, aby vrátila toohello klienta.

dotaz Hello v hello následující kód vrátí pouze hello popisy entit v tabulce hello.

> [!NOTE]
> Následující fragment kódu funguje pouze s hello Azure Storage Hello. Emulátor úložiště hello není podporován.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Odstranění entity

Odstranění entity předáním jeho PartitionKey a RowKey toohello [delete_entity] [ py_delete_entity] metoda.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Odstranění tabulky

Pokud již nepotřebujete pro tabulku nebo žádné hello entit v něm, zavolejte hello [delete_table] [ py_delete_table] metoda toopermanently odstranit tabulku hello ze služby Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Další kroky

* [Azure SDK úložiště pro Python API – referenční informace](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Úložiště Azure SDK pro jazyk Python](https://github.com/Azure/azure-storage-python)
* [Středisko pro vývojáře programující v Pythonu](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): bezplatná aplikace a platformy pro vizuální práci s daty Azure Storage ve Windows, systému macOS a Linux.

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
