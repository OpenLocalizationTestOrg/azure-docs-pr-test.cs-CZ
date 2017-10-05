---
title: "Opakovatelných kopie v Azure Data Factory | Microsoft Docs"
description: "Jak se vyhnout duplikáty, i když je více než jednou spustit řez, který kopíruje data."
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
ms.openlocfilehash: 5b88a235915bf35fec701eee4a5f80beb4a67632
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a><span data-ttu-id="56191-103">Opakovatelných kopie v Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="56191-103">Repeatable copy in Azure Data Factory</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="56191-104">Opakovatelných číst z relační zdrojů</span><span class="sxs-lookup"><span data-stu-id="56191-104">Repeatable read from relational sources</span></span>
<span data-ttu-id="56191-105">Při kopírování dat z relačních dat ukládá, uvědomte si, aby se zabránilo neúmyslnému výstupy opakovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="56191-105">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="56191-106">V Azure Data Factory může řez znovu ručně.</span><span class="sxs-lookup"><span data-stu-id="56191-106">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="56191-107">Zásady opakovaných pokusů pro datovou sadu můžete také nakonfigurovat tak, aby řez se znovu spustí, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="56191-107">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="56191-108">Řez se znovu spustí, buď způsobem, musíte zajistit, že stejná data je pro čtení bez ohledu na to kolikrát řez je spustit.</span><span class="sxs-lookup"><span data-stu-id="56191-108">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span>  
 
> [!NOTE]
> <span data-ttu-id="56191-109">Následující ukázky jsou pro Azure SQL, ale platí pro všechny datové úložiště, které podporuje obdélníková datové sady.</span><span class="sxs-lookup"><span data-stu-id="56191-109">The following samples are for Azure SQL but are applicable to any data store that supports rectangular datasets.</span></span> <span data-ttu-id="56191-110">Možná budete muset upravit **typ** zdroje a **dotazu** vlastnosti (například: dotazu místo sqlReaderQuery) pro data uložit.</span><span class="sxs-lookup"><span data-stu-id="56191-110">You may have to adjust the **type** of source and the **query** property (for example: query instead of sqlReaderQuery) for the data store.</span></span>   

<span data-ttu-id="56191-111">Obvykle při čtení z relační úložiště, kterou chcete číst pouze data odpovídající této řez.</span><span class="sxs-lookup"><span data-stu-id="56191-111">Usually, when reading from relational stores, you want to read only the data corresponding to that slice.</span></span> <span data-ttu-id="56191-112">Způsob, jak tomu by pomocí WindowStart a WindowEnd systémové proměnné dostupné v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="56191-112">A way to do so would be by using the WindowStart and WindowEnd system variables available in Azure Data Factory.</span></span> <span data-ttu-id="56191-113">Přečtěte si informace o proměnných a funkce v Azure Data Factory zde v [Azure Data Factory – funkce a systémové proměnné](data-factory-functions-variables.md) článku.</span><span class="sxs-lookup"><span data-stu-id="56191-113">Read about the variables and functions in Azure Data Factory here in the [Azure Data Factory - Functions and System Variables](data-factory-functions-variables.md) article.</span></span> <span data-ttu-id="56191-114">Příklad:</span><span class="sxs-lookup"><span data-stu-id="56191-114">Example:</span></span> 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
<span data-ttu-id="56191-115">Tento dotaz načte data, která spadá do rozsahu řez doba trvání (WindowStart -> WindowEnd) z tabulky MyTable.</span><span class="sxs-lookup"><span data-stu-id="56191-115">This query reads data that falls in the slice duration range (WindowStart -> WindowEnd) from the table MyTable.</span></span> <span data-ttu-id="56191-116">Spusťte tento řez by také vždy zajistěte, že je stejná data pro čtení.</span><span class="sxs-lookup"><span data-stu-id="56191-116">Rerun of this slice would also always ensure that the same data is read.</span></span> 

<span data-ttu-id="56191-117">V jiných případech může chtít přečíst celou tabulku a může sqlReaderQuery definujte takto:</span><span class="sxs-lookup"><span data-stu-id="56191-117">In other cases, you may wish to read the entire table and may define the sqlReaderQuery as follows:</span></span>

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a><span data-ttu-id="56191-118">Opakovatelných při zápisu do SqlSink</span><span class="sxs-lookup"><span data-stu-id="56191-118">Repeatable write to SqlSink</span></span>
<span data-ttu-id="56191-119">Při kopírování dat **Azure SQL nebo SQL Server** z jiným úložištím dat, budete muset opakovatelnosti mějte na paměti, aby se zabránilo neúmyslnému výstupy.</span><span class="sxs-lookup"><span data-stu-id="56191-119">When copying data to **Azure SQL/SQL Server** from other data stores, you need to keep repeatability in mind to avoid unintended outcomes.</span></span> 

<span data-ttu-id="56191-120">Při kopírování dat do databáze serveru SQL/SQL Azure, připojí aktivitě kopírování dat do tabulky jímky ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="56191-120">When copying data to Azure SQL/SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="56191-121">Řekněme, jsou kopírování dat ze souboru CSV (hodnoty oddělené čárkami) obsahující dva záznamy v databázi Azure SQL nebo SQL Server v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="56191-121">Say, you are copying data from a CSV (comma-separated values) file containing two records to the following table in an Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="56191-122">Při spuštění se řez, dva záznamy se zkopírují do tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-122">When a slice runs, the two records are copied to the SQL table.</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

<span data-ttu-id="56191-123">Předpokládejme, že byly nalezeny chyby ve zdrojovém souboru a aktualizované množství dolů trubice od 2 do 4.</span><span class="sxs-lookup"><span data-stu-id="56191-123">Suppose you found errors in source file and updated the quantity of Down Tube from 2 to 4.</span></span> <span data-ttu-id="56191-124">Pokud datový řez pro toto období ručně, najdete v ní dva nové záznamy připojenou k databázi Azure SQL nebo SQL Server.</span><span class="sxs-lookup"><span data-stu-id="56191-124">If you rerun the data slice for that period manually, you’ll find two new records appended to Azure SQL/SQL Server Database.</span></span> <span data-ttu-id="56191-125">Tento příklad předpokládá, že žádný sloupců v tabulce nemá omezení primárního klíče.</span><span class="sxs-lookup"><span data-stu-id="56191-125">This example assumes that none of the columns in the table has the primary key constraint.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="56191-126">Chcete-li tomu zabránit, budete muset zadat UPSERT sémantiku pomocí jedné z následujících dvou mechanismů:</span><span class="sxs-lookup"><span data-stu-id="56191-126">To avoid this behavior, you need to specify UPSERT semantics by using one of the following two mechanisms:</span></span>

### <a name="mechanism-1-using-sqlwritercleanupscript"></a><span data-ttu-id="56191-127">Mechanismus 1: použití sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="56191-127">Mechanism 1: using sqlWriterCleanupScript</span></span>
<span data-ttu-id="56191-128">Můžete použít **sqlWriterCleanupScript** vlastnost vyčistit data z tabulky jímky před vložením dat při spuštění řez.</span><span class="sxs-lookup"><span data-stu-id="56191-128">You can use the **sqlWriterCleanupScript** property to clean up data from the sink table before inserting the data when a slice is run.</span></span> 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

<span data-ttu-id="56191-129">Při spuštění se řez, je nejprve spustit skript vyčištění odstranit data, která odpovídá řez z tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-129">When a slice runs, the cleanup script is run first to delete data that corresponds to the slice from the SQL table.</span></span> <span data-ttu-id="56191-130">Aktivitě kopírování vkládá data do tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-130">The copy activity then inserts data into the SQL Table.</span></span> <span data-ttu-id="56191-131">Pokud je řez je znovu spustit, množství se aktualizuje podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="56191-131">If the slice is rerun, the quantity is updated as desired.</span></span>

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="56191-132">Předpokládejme, že odebrání záznamu ploché podložku z původní sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="56191-132">Suppose the Flat Washer record is removed from the original csv.</span></span> <span data-ttu-id="56191-133">Pak znovu spustit řez vznikna následující výsledek:</span><span class="sxs-lookup"><span data-stu-id="56191-133">Then rerunning the slice would produce the following result:</span></span> 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

<span data-ttu-id="56191-134">Aktivita kopírování spustili čištění skript, který chcete odstranit data odpovídající této řez.</span><span class="sxs-lookup"><span data-stu-id="56191-134">The copy activity ran the cleanup script to delete the corresponding data for that slice.</span></span> <span data-ttu-id="56191-135">Pak jej číst vstupní ze souboru csv (což je obsaženy jenom jeden záznam) a vložit do tabulky.</span><span class="sxs-lookup"><span data-stu-id="56191-135">Then it read the input from the csv (which then contained only one record) and inserted it into the Table.</span></span> 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a><span data-ttu-id="56191-136">Mechanismus 2: pomocí sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="56191-136">Mechanism 2: using sliceIdentifierColumnName</span></span>
> [!IMPORTANT]
> <span data-ttu-id="56191-137">V současné době sliceIdentifierColumnName není podporována pro Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="56191-137">Currently, sliceIdentifierColumnName is not supported for Azure SQL Data Warehouse.</span></span> 

<span data-ttu-id="56191-138">Druhý mechanismus k dosažení opakovatelnosti spočívá v použití vyhrazených sloupce (sliceIdentifierColumnName) v cílové tabulce.</span><span class="sxs-lookup"><span data-stu-id="56191-138">The second mechanism to achieve repeatability is by having a dedicated column (sliceIdentifierColumnName) in the target Table.</span></span> <span data-ttu-id="56191-139">V tomto sloupci se použije službou Azure Data Factory pro Ujistěte se, že zdrojový a cílový zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="56191-139">This column would be used by Azure Data Factory to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="56191-140">Tento přístup funguje, když je flexibilitu při změně nebo definování schématu cílové tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-140">This approach works when there is flexibility in changing or defining the destination SQL Table schema.</span></span> 

<span data-ttu-id="56191-141">V tomto sloupci se používá službou Azure Data Factory pro účely opakovatelnost a v procesu Azure Data Factory nebyly provedeny žádné změny schématu tabulky.</span><span class="sxs-lookup"><span data-stu-id="56191-141">This column is used by Azure Data Factory for repeatability purposes and in the process Azure Data Factory does not make any schema changes to the Table.</span></span> <span data-ttu-id="56191-142">Způsob použití tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="56191-142">Way to use this approach:</span></span>

1. <span data-ttu-id="56191-143">Definování sloupec typu **binárního souboru (32)** v cílové tabulce SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-143">Define a column of type **binary (32)** in the destination SQL Table.</span></span> <span data-ttu-id="56191-144">Měla by existovat žádné omezení na tomto sloupci.</span><span class="sxs-lookup"><span data-stu-id="56191-144">There should be no constraints on this column.</span></span> <span data-ttu-id="56191-145">Název v tomto sloupci jako AdfSliceIdentifier budeme v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="56191-145">Let's name this column as AdfSliceIdentifier for this example.</span></span>


    <span data-ttu-id="56191-146">Zdrojová tabulka:</span><span class="sxs-lookup"><span data-stu-id="56191-146">Source table:</span></span>

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    <span data-ttu-id="56191-147">Cílové tabulky:</span><span class="sxs-lookup"><span data-stu-id="56191-147">Destination table:</span></span> 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. <span data-ttu-id="56191-148">Ho použijte k aktivitě kopírování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56191-148">Use it in the copy activity as follows:</span></span>
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

<span data-ttu-id="56191-149">Azure Data Factory naplní tento sloupec podle jeho potřeba zajistit, že zdrojový a cílový zůstane synchronizovaná.</span><span class="sxs-lookup"><span data-stu-id="56191-149">Azure Data Factory populates this column as per its need to ensure the source and destination stay synchronized.</span></span> <span data-ttu-id="56191-150">Mimo tento kontext není vhodné používat hodnoty daného sloupce.</span><span class="sxs-lookup"><span data-stu-id="56191-150">The values of this column should not be used outside of this context.</span></span> 

<span data-ttu-id="56191-151">Podobně jako mechanismus 1, aktivity kopírování automaticky vyčistí data pro danou řez z cílového tabulky SQL.</span><span class="sxs-lookup"><span data-stu-id="56191-151">Similar to mechanism 1, Copy Activity automatically cleans up the data for the given slice from the destination SQL Table.</span></span> <span data-ttu-id="56191-152">Potom vloží data ze zdroje v do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="56191-152">It then inserts data from source in to the destination table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="56191-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56191-153">Next steps</span></span>
<span data-ttu-id="56191-154">Zkontrolujte konektor následující články, dokončení příklady JSON:</span><span class="sxs-lookup"><span data-stu-id="56191-154">Review the following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="56191-155">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="56191-155">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="56191-156">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="56191-156">Azure SQL Data Warehouse</span></span>](data-factory-azure-sql-data-warehouse-connector.md)
- [<span data-ttu-id="56191-157">SQL Server</span><span class="sxs-lookup"><span data-stu-id="56191-157">SQL Server</span></span>](data-factory-sqlserver-connector.md)