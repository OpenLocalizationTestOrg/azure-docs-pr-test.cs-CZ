---
title: "kopírování aaaRepeatable v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak tooavoid duplicitních to i v případě, že je řez, který kopíruje data spustit více než jednou."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a><span data-ttu-id="e0a96-103">Opakovatelných kopie v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="e0a96-103">Repeatable copy in Azure Data Factory</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="e0a96-104">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="e0a96-104">Repeatable read from relational sources</span></span>
<span data-ttu-id="e0a96-105">Při kopírování dat z relační datové úložiště, mějte opakovatelnosti pamatovat tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="e0a96-105">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="e0a96-106">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="e0a96-106">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="e0a96-107">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e0a96-107">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="e0a96-108">Pokud v obou případech se znovu spustí řez, je potřeba toomake jisti, který hello stejných dat je pro čtení bez ohledu na to jak mnohokrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="e0a96-108">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span>  
 
> [!NOTE]
> <span data-ttu-id="e0a96-109">Hello následující ukázky jsou pro Azure SQL, ale jsou příslušné tooany datové úložiště, které podporuje obdélníková datové sady.</span><span class="sxs-lookup"><span data-stu-id="e0a96-109">hello following samples are for Azure SQL but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="e0a96-110">Můžete mít tooadjust hello **typ** zdroje a hello **dotazu** vlastnosti (například: dotazu místo sqlReaderQuery) pro hello data uložit.</span><span class="sxs-lookup"><span data-stu-id="e0a96-110">You may have tooadjust hello **type** of source and hello **query** property (for example: query instead of sqlReaderQuery) for hello data store.</span></span>   

<span data-ttu-id="e0a96-111">Obvykle při čtení z relační úložiště, budete chtít tooread pouze hello data odpovídající toothat řez.</span><span class="sxs-lookup"><span data-stu-id="e0a96-111">Usually, when reading from relational stores, you want tooread only hello data corresponding toothat slice.</span></span> <span data-ttu-id="e0a96-112">Způsob toodo tak bude pomocí hello WindowStart WindowEnd systémové proměnné a k dispozici v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="e0a96-112">A way toodo so would be by using hello WindowStart and WindowEnd system variables available in Azure Data Factory.</span></span> <span data-ttu-id="e0a96-113">Přečtěte si informace o proměnných hello a funkcí v datové továrně Azure v hello [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku.</span><span class="sxs-lookup"><span data-stu-id="e0a96-113">Read about hello variables and functions in Azure Data Factory here in hello [Azure Data Factory - Functions and System Variables](data-factory-functions-variables.md) article.</span></span> <span data-ttu-id="e0a96-114">Příklad:</span><span class="sxs-lookup"><span data-stu-id="e0a96-114">Example:</span></span> 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
<span data-ttu-id="e0a96-115">Tento dotaz načte data, která se nachází ve hello řez trvání rozsahu (WindowStart -> WindowEnd) z tabulky hello MyTable.</span><span class="sxs-lookup"><span data-stu-id="e0a96-115">This query reads data that falls in hello slice duration range (WindowStart -> WindowEnd) from hello table MyTable.</span></span> <span data-ttu-id="e0a96-116">Spusťte tento řez by také vždy ujistěte se, že hello je čtení stejná data.</span><span class="sxs-lookup"><span data-stu-id="e0a96-116">Rerun of this slice would also always ensure that hello same data is read.</span></span> 

<span data-ttu-id="e0a96-117">V jiných případech může přát tooread hello celou tabulku a může definovat hello sqlReaderQuery následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e0a96-117">In other cases, you may wish tooread hello entire table and may define hello sqlReaderQuery as follows:</span></span>

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a><span data-ttu-id="e0a96-118">TooSqlSink opakovatelných zápisu</span><span class="sxs-lookup"><span data-stu-id="e0a96-118">Repeatable write tooSqlSink</span></span>
<span data-ttu-id="e0a96-119">Při kopírování dat příliš**Azure SQL nebo SQL Server** z jiných úložišť dat je třeba tookeep opakovatelnosti v paměti tooavoid nezamýšleným výstupy.</span><span class="sxs-lookup"><span data-stu-id="e0a96-119">When copying data too**Azure SQL/SQL Server** from other data stores, you need tookeep repeatability in mind tooavoid unintended outcomes.</span></span> 

<span data-ttu-id="e0a96-120">Při kopírování dat tooAzure databázi SQL nebo SQL Server, aktivity kopírování hello připojí tabulky jímky toohello data ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e0a96-120">When copying data tooAzure SQL/SQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="e0a96-121">Vyslovte jsou kopírování dat z CSV (hodnoty oddělené čárkami) soubor obsahující dva záznamy toohello následující tabulka v databázi Azure SQL nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e0a96-121">Say, you are copying data from a CSV (comma-separated values) file containing two records toohello following table in an Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="e0a96-122">Při spuštění se řez, jsou dva záznamy hello zkopírovaný toohello tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="e0a96-122">When a slice runs, hello two records are copied toohello SQL table.</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="e0a96-123">Předpokládejme, že byly nalezeny chyby ve zdrojovém souboru a aktualizovat hello objemu trubice dolů z 2 too4.</span><span class="sxs-lookup"><span data-stu-id="e0a96-123">Suppose you found errors in source file and updated hello quantity of Down Tube from 2 too4.</span></span> <span data-ttu-id="e0a96-124">Pokud hello datový řez pro toto období ručně, zjistíte, že dva nové záznamy připojí tooAzure databázi SQL nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e0a96-124">If you rerun hello data slice for that period manually, you’ll find two new records appended tooAzure SQL/SQL Server Database.</span></span> <span data-ttu-id="e0a96-125">Tento příklad předpokládá, že žádný hello sloupců v tabulce hello nemá hello omezení primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="e0a96-125">This example assumes that none of hello columns in hello table has hello primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="e0a96-126">tooavoid toto chování je třeba toospecify UPSERT sémantiku pomocí jedné z následujících dvou mechanismů hello:</span><span class="sxs-lookup"><span data-stu-id="e0a96-126">tooavoid this behavior, you need toospecify UPSERT semantics by using one of hello following two mechanisms:</span></span>

### <a name="mechanism-1-using-sqlwritercleanupscript"></a><span data-ttu-id="e0a96-127">Mechanismus 1: použití sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="e0a96-127">Mechanism 1: using sqlWriterCleanupScript</span></span>
<span data-ttu-id="e0a96-128">Můžete použít hello **sqlWriterCleanupScript** tooclean vlastnost zálohovat data z tabulky jímky hello před vložením dat hello při spuštění řez.</span><span class="sxs-lookup"><span data-stu-id="e0a96-128">You can use hello **sqlWriterCleanupScript** property tooclean up data from hello sink table before inserting hello data when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="e0a96-129">Při spuštění se řez, je spustit skript pro vyčištění hello první toodelete data, která odpovídá toohello řez z tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="e0a96-129">When a slice runs, hello cleanup script is run first toodelete data that corresponds toohello slice from hello SQL table.</span></span> <span data-ttu-id="e0a96-130">Aktivita kopírování Hello vkládá data do hello tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="e0a96-130">hello copy activity then inserts data into hello SQL Table.</span></span> <span data-ttu-id="e0a96-131">Pokud se znovu spustí hello řez, množství hello je aktualizováno podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e0a96-131">If hello slice is rerun, hello quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="e0a96-132">Předpokládejme, že hello ploché podložku záznam se odebere z původní csv hello.</span><span class="sxs-lookup"><span data-stu-id="e0a96-132">Suppose hello Flat Washer record is removed from hello original csv.</span></span> <span data-ttu-id="e0a96-133">Znovu spustit řez hello by pak vytváří hello následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="e0a96-133">Then rerunning hello slice would produce hello following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="e0a96-134">Aktivita kopírování Hello spustili hello čištění toodelete hello odpovídající data skriptu pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="e0a96-134">hello copy activity ran hello cleanup script toodelete hello corresponding data for that slice.</span></span> <span data-ttu-id="e0a96-135">Pak jej číst hello vstup ze souboru csv hello (což je obsaženy jenom jeden záznam) a vložit do hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0a96-135">Then it read hello input from hello csv (which then contained only one record) and inserted it into hello Table.</span></span> 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a><span data-ttu-id="e0a96-136">Mechanismus 2: pomocí sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="e0a96-136">Mechanism 2: using sliceIdentifierColumnName</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e0a96-137">V současné době sliceIdentifierColumnName není podporována pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e0a96-137">Currently, sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="e0a96-138">Hello druhý mechanismus tooachieve opakovatelnosti spočívá v použití vyhrazených sloupce (sliceIdentifierColumnName) v hello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0a96-138">hello second mechanism tooachieve repeatability is by having a dedicated column (sliceIdentifierColumnName) in hello target Table.</span></span> <span data-ttu-id="e0a96-139">V tomto sloupci se použije pomocí Azure Data Factory tooensure hello zdrojové a cílové synchronní.</span><span class="sxs-lookup"><span data-stu-id="e0a96-139">This column would be used by Azure Data Factory tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="e0a96-140">Tento přístup funguje, když je flexibilitu při změně nebo definice schématu tabulky SQL cílové hello.</span><span class="sxs-lookup"><span data-stu-id="e0a96-140">This approach works when there is flexibility in changing or defining hello destination SQL Table schema.</span></span> 

<span data-ttu-id="e0a96-141">V tomto sloupci se používá službou Azure Data Factory pro účely opakovatelnost a v procesu hello Azure Data Factory zajistěte, aby nebyly žádné schéma změny toohello tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0a96-141">This column is used by Azure Data Factory for repeatability purposes and in hello process Azure Data Factory does not make any schema changes toohello Table.</span></span> <span data-ttu-id="e0a96-142">Způsob toouse tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="e0a96-142">Way toouse this approach:</span></span>

1. <span data-ttu-id="e0a96-143">Definování sloupec typu **binárního souboru (32)** v hello cílové tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="e0a96-143">Define a column of type **binary (32)** in hello destination SQL Table.</span></span> <span data-ttu-id="e0a96-144">Měla by existovat žádné omezení na tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="e0a96-144">There should be no constraints on this column.</span></span> <span data-ttu-id="e0a96-145">Název v tomto sloupci jako AdfSliceIdentifier budeme v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="e0a96-145">Let's name this column as AdfSliceIdentifier for this example.</span></span>


    <span data-ttu-id="e0a96-146">Zdrojová tabulka:</span><span class="sxs-lookup"><span data-stu-id="e0a96-146">Source table:</span></span>

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    <span data-ttu-id="e0a96-147">Cílové tabulky:</span><span class="sxs-lookup"><span data-stu-id="e0a96-147">Destination table:</span></span> 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. <span data-ttu-id="e0a96-148">Ho použijte k aktivitě kopírování hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e0a96-148">Use it in hello copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

<span data-ttu-id="e0a96-149">Azure Data Factory naplní tento sloupec podle jeho nutné tooensure hello zdrojové a cílové zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="e0a96-149">Azure Data Factory populates this column as per its need tooensure hello source and destination stay synchronized.</span></span> <span data-ttu-id="e0a96-150">mimo tento kontext není vhodné používat Hello hodnoty daného sloupce.</span><span class="sxs-lookup"><span data-stu-id="e0a96-150">hello values of this column should not be used outside of this context.</span></span> 

<span data-ttu-id="e0a96-151">Podobně jako toomechanism 1, aktivity kopírování automaticky vyčistí hello dat pro danou řez z cílového hello tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="e0a96-151">Similar toomechanism 1, Copy Activity automatically cleans up hello data for hello given slice from hello destination SQL Table.</span></span> <span data-ttu-id="e0a96-152">Potom vloží data ze zdroje v toohello cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0a96-152">It then inserts data from source in toohello destination table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="e0a96-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0a96-153">Next steps</span></span>
<span data-ttu-id="e0a96-154">Projděte si následující články konektoru, které pro dokončení JSON příklady hello:</span><span class="sxs-lookup"><span data-stu-id="e0a96-154">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="e0a96-155">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="e0a96-155">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="e0a96-156">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="e0a96-156">Azure SQL Data Warehouse</span></span>](data-factory-azure-sql-data-warehouse-connector.md)
- [<span data-ttu-id="e0a96-157">SQL Server</span><span class="sxs-lookup"><span data-stu-id="e0a96-157">SQL Server</span></span>](data-factory-sqlserver-connector.md)