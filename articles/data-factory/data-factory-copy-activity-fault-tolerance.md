---
title: "Přidání odolnost proti chybám v Azure Data Factory kopie aktivity přeskočení nekompatibilní řádků | Microsoft Docs"
description: "Informace o postupu přidání odolnost proti chybám při aktivitě kopírování objektu pro vytváření dat Azure pomocí přeskočení nekompatibilní řádky během kopírování"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: e2a108752259d5da3b401666c6bdbaad13b7ea90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a><span data-ttu-id="52a58-103">Přidání odolnost proti chybám v aktivitě kopírování přeskočení nekompatibilní řádků</span><span class="sxs-lookup"><span data-stu-id="52a58-103">Add fault tolerance in Copy Activity by skipping incompatible rows</span></span>

<span data-ttu-id="52a58-104">Azure Data Factory [aktivity kopírování](data-factory-data-movement-activities.md) nabízí dva způsoby, jak zpracovat nekompatibilní řádků při kopírování dat mezi zdroj a jímka úložišti dat:</span><span class="sxs-lookup"><span data-stu-id="52a58-104">Azure Data Factory [Copy Activity](data-factory-data-movement-activities.md) offers you two ways to handle incompatible rows when copying data between source and sink data stores:</span></span>

- <span data-ttu-id="52a58-105">Můžete přerušit a selhání kopie aktivity po nekompatibilní data došlo (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="52a58-105">You can abort and fail the copy activity when incompatible data is encountered (default behavior).</span></span>
- <span data-ttu-id="52a58-106">Kopírování všech dat přidáním odolnost proti chybám a přeskočení řádky nekompatibilní data můžete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="52a58-106">You can continue to copy all of the data by adding fault tolerance and skipping incompatible data rows.</span></span> <span data-ttu-id="52a58-107">Kromě toho se můžete přihlásit nekompatibilní řádky úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="52a58-107">In addition, you can log the incompatible rows in Azure Blob storage.</span></span> <span data-ttu-id="52a58-108">Poté můžete prozkoumat do protokolu a zjistěte příčinu selhání, opravte dat ve zdroji dat a zkuste to aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="52a58-108">You can then examine the log to learn the cause for the failure, fix the data on the data source, and retry the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="52a58-109">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="52a58-109">Supported scenarios</span></span>
<span data-ttu-id="52a58-110">Aktivita kopírování podporuje tři scénáře pro zjišťování, přeskočí a protokolování nekompatibilní data:</span><span class="sxs-lookup"><span data-stu-id="52a58-110">Copy Activity supports three scenarios for detecting, skipping, and logging incompatible data:</span></span>

- <span data-ttu-id="52a58-111">**Nekompatibilita mezi zdrojového datového typu a nativní typ jímky**</span><span class="sxs-lookup"><span data-stu-id="52a58-111">**Incompatibility between the source data type and the sink native type**</span></span>

    <span data-ttu-id="52a58-112">Příklad: kopírování dat ze souboru CSV v úložišti objektů Blob k databázi SQL s definici schématu, která obsahuje tři **INT** typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="52a58-112">For example: Copy data from a CSV file in Blob storage to a SQL database with a schema definition that contains three **INT** type columns.</span></span> <span data-ttu-id="52a58-113">Řádků souboru CSV, které obsahují číselné údaje, jako například `123,456,789` jsou úspěšně zkopírovat do úložiště jímky.</span><span class="sxs-lookup"><span data-stu-id="52a58-113">The CSV file rows that contain numeric data, such as `123,456,789` are copied successfully to the sink store.</span></span> <span data-ttu-id="52a58-114">Však řádky obsahující jiné než číselné hodnoty, například `123,456,abc` jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="52a58-114">However, the rows that contain non-numeric values, such as `123,456,abc` are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="52a58-115">**Neshoda mezi počtem sloupců mezi zdroj a jímka**</span><span class="sxs-lookup"><span data-stu-id="52a58-115">**Mismatch in the number of columns between the source and the sink**</span></span>

    <span data-ttu-id="52a58-116">Příklad: kopírování dat ze souboru CSV v úložišti objektů Blob k databázi SQL s definici schématu, která obsahuje šest sloupce.</span><span class="sxs-lookup"><span data-stu-id="52a58-116">For example: Copy data from a CSV file in Blob storage to a SQL database with a schema definition that contains six columns.</span></span> <span data-ttu-id="52a58-117">Řádky soubor CSV, které obsahují šesti sloupce jsou úspěšně zkopírovat do úložiště jímky.</span><span class="sxs-lookup"><span data-stu-id="52a58-117">The CSV file rows that contain six columns are copied successfully to the sink store.</span></span> <span data-ttu-id="52a58-118">Řádky soubor CSV, které obsahují více nebo méně než šest sloupce jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="52a58-118">The CSV file rows that contain more or fewer than six columns are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="52a58-119">**Porušení primárního klíče při zápisu do relační databáze**</span><span class="sxs-lookup"><span data-stu-id="52a58-119">**Primary key violation when writing to a relational database**</span></span>

    <span data-ttu-id="52a58-120">Příklad: kopírování dat z SQL serveru do databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="52a58-120">For example: Copy data from a SQL server to a SQL database.</span></span> <span data-ttu-id="52a58-121">Ve službě SQL database podřízený je definovaný primární klíč, ale na zdrojovém serveru SQL je definován žádný primární klíč.</span><span class="sxs-lookup"><span data-stu-id="52a58-121">A primary key is defined in the sink SQL database, but no such primary key is defined in the source SQL server.</span></span> <span data-ttu-id="52a58-122">Duplicitní řádky, na které existují ve zdroji nelze zkopírovat do jímky.</span><span class="sxs-lookup"><span data-stu-id="52a58-122">The duplicated rows that exist in the source cannot be copied to the sink.</span></span> <span data-ttu-id="52a58-123">Aktivita kopírování zkopíruje pouze první řádek zdrojová data do jímky.</span><span class="sxs-lookup"><span data-stu-id="52a58-123">Copy Activity copies only the first row of the source data into the sink.</span></span> <span data-ttu-id="52a58-124">Další zdroje řádky, které obsahují duplicitní hodnotu primárního klíče jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="52a58-124">The subsequent source rows that contain the duplicated primary key value are detected as incompatible and are skipped.</span></span>

## <a name="configuration"></a><span data-ttu-id="52a58-125">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="52a58-125">Configuration</span></span>
<span data-ttu-id="52a58-126">Následující příklad uvádí definici JSON konfigurace přeskočení nekompatibilní řádky v aktivitě kopírování:</span><span class="sxs-lookup"><span data-stu-id="52a58-126">The following example provides a JSON definition to configure skipping the incompatible rows in Copy Activity:</span></span>

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| <span data-ttu-id="52a58-127">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="52a58-127">Property</span></span> | <span data-ttu-id="52a58-128">Popis</span><span class="sxs-lookup"><span data-stu-id="52a58-128">Description</span></span> | <span data-ttu-id="52a58-129">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="52a58-129">Allowed values</span></span> | <span data-ttu-id="52a58-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="52a58-130">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="52a58-131">**enableSkipIncompatibleRow**</span><span class="sxs-lookup"><span data-stu-id="52a58-131">**enableSkipIncompatibleRow**</span></span> | <span data-ttu-id="52a58-132">Povolte přeskočení nekompatibilní řádků při kopírování nebo ne.</span><span class="sxs-lookup"><span data-stu-id="52a58-132">Enable skipping incompatible rows during copy or not.</span></span> | <span data-ttu-id="52a58-133">True</span><span class="sxs-lookup"><span data-stu-id="52a58-133">True</span></span><br/><span data-ttu-id="52a58-134">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="52a58-134">False (default)</span></span> | <span data-ttu-id="52a58-135">Ne</span><span class="sxs-lookup"><span data-stu-id="52a58-135">No</span></span> |
| <span data-ttu-id="52a58-136">**redirectIncompatibleRowSettings**</span><span class="sxs-lookup"><span data-stu-id="52a58-136">**redirectIncompatibleRowSettings**</span></span> | <span data-ttu-id="52a58-137">Skupina vlastností, které může být zadán, pokud chcete protokolovat nekompatibilní řádky.</span><span class="sxs-lookup"><span data-stu-id="52a58-137">A group of properties that can be specified when you want to log the incompatible rows.</span></span> | &nbsp; | <span data-ttu-id="52a58-138">Ne</span><span class="sxs-lookup"><span data-stu-id="52a58-138">No</span></span> |
| <span data-ttu-id="52a58-139">**linkedServiceName**</span><span class="sxs-lookup"><span data-stu-id="52a58-139">**linkedServiceName**</span></span> | <span data-ttu-id="52a58-140">Propojené služby Azure Storage k ukládání protokol, který obsahuje přeskočených řádků.</span><span class="sxs-lookup"><span data-stu-id="52a58-140">The linked service of Azure Storage to store the log that contains the skipped rows.</span></span> | <span data-ttu-id="52a58-141">Název [azurestorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) propojené služby, která odkazuje na instanci úložiště, který chcete použít k uložení souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="52a58-141">The name of an [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linked service, which refers to the storage instance that you want to use to store the log file.</span></span> | <span data-ttu-id="52a58-142">Ne</span><span class="sxs-lookup"><span data-stu-id="52a58-142">No</span></span> |
| <span data-ttu-id="52a58-143">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="52a58-143">**path**</span></span> | <span data-ttu-id="52a58-144">Cesta souboru protokolu, který obsahuje přeskočených řádků.</span><span class="sxs-lookup"><span data-stu-id="52a58-144">The path of the log file that contains the skipped rows.</span></span> | <span data-ttu-id="52a58-145">Zadejte cestu úložiště objektů Blob, které chcete používat k protokolování nekompatibilní data.</span><span class="sxs-lookup"><span data-stu-id="52a58-145">Specify the Blob storage path that you want to use to log the incompatible data.</span></span> <span data-ttu-id="52a58-146">Pokud nezadáte cestu, služby pro vás vytvoří kontejner.</span><span class="sxs-lookup"><span data-stu-id="52a58-146">If you do not provide a path, the service creates a container for you.</span></span> | <span data-ttu-id="52a58-147">Ne</span><span class="sxs-lookup"><span data-stu-id="52a58-147">No</span></span> |

## <a name="monitoring"></a><span data-ttu-id="52a58-148">Monitorování</span><span class="sxs-lookup"><span data-stu-id="52a58-148">Monitoring</span></span>
<span data-ttu-id="52a58-149">Po dokončení kopírování aktivity při spuštění, zobrazí se počet přeskočených řádků v části monitorování:</span><span class="sxs-lookup"><span data-stu-id="52a58-149">After the copy activity run completes, you can see the number of skipped rows in the monitoring section:</span></span>

![Monitorování přeskočen nekompatibilní řádků](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

<span data-ttu-id="52a58-151">Pokud nakonfigurujete protokolu nekompatibilní řádky, můžete nějakého najít soubor protokolu v této cestě: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` v souboru protokolu, můžete zobrazit na řádky, které byly přeskočeny a hlavní příčinu nekompatibilita.</span><span class="sxs-lookup"><span data-stu-id="52a58-151">If you configure to log the incompatible rows, you can find the log file at this path: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In the log file, you can see the rows that were skipped and the root cause of the incompatibility.</span></span>

<span data-ttu-id="52a58-152">Původní data a odpovídající chyby jsou protokolovány v souboru.</span><span class="sxs-lookup"><span data-stu-id="52a58-152">Both the original data and the corresponding error are logged in the file.</span></span> <span data-ttu-id="52a58-153">Příklad obsahu souboru protokolu je následující:</span><span class="sxs-lookup"><span data-stu-id="52a58-153">An example of the log file content is as follows:</span></span>
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a><span data-ttu-id="52a58-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52a58-154">Next steps</span></span>
<span data-ttu-id="52a58-155">Další informace o aktivitě kopírování objektu pro vytváření dat Azure, najdete v části [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="52a58-155">To learn more about Azure Data Factory Copy Activity, see [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>