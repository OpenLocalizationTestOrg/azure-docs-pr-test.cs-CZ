---
title: "aaaAdd odolnost proti chybám při aktivitě kopírování objektu pro vytváření dat Azure pomocí přeskočení nekompatibilní řádků | Microsoft Docs"
description: "Zjistěte, jak tooadd odolnost proti chybám při aktivitě kopírování objektu pro vytváření dat Azure pomocí přeskočení nekompatibilní řádky během kopírování"
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
ms.openlocfilehash: e7cf6117655910844b292d340674d8d631450a81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a><span data-ttu-id="a6d8b-103">Přidání odolnost proti chybám v aktivitě kopírování přeskočení nekompatibilní řádků</span><span class="sxs-lookup"><span data-stu-id="a6d8b-103">Add fault tolerance in Copy Activity by skipping incompatible rows</span></span>

<span data-ttu-id="a6d8b-104">Azure Data Factory [aktivity kopírování](data-factory-data-movement-activities.md) nabízí dva způsoby, jak toohandle nekompatibilní řádků při kopírování dat mezi zdroj a jímka úložišti dat:</span><span class="sxs-lookup"><span data-stu-id="a6d8b-104">Azure Data Factory [Copy Activity](data-factory-data-movement-activities.md) offers you two ways toohandle incompatible rows when copying data between source and sink data stores:</span></span>

- <span data-ttu-id="a6d8b-105">Můžete přerušit a selhání hello kopie aktivity po nekompatibilní data došlo (výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="a6d8b-105">You can abort and fail hello copy activity when incompatible data is encountered (default behavior).</span></span>
- <span data-ttu-id="a6d8b-106">Můžete pokračovat v toocopy všechna data hello přidáním odolnost proti chybám a přeskočení nekompatibilní data řádků.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-106">You can continue toocopy all of hello data by adding fault tolerance and skipping incompatible data rows.</span></span> <span data-ttu-id="a6d8b-107">Kromě toho se můžete přihlásit nekompatibilní řádky hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-107">In addition, you can log hello incompatible rows in Azure Blob storage.</span></span> <span data-ttu-id="a6d8b-108">Můžete pak zkontrolujte hello protokolu toolearn hello příčinou selhání hello, opravte hello dat na zdroji dat hello a opakujte aktivity kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-108">You can then examine hello log toolearn hello cause for hello failure, fix hello data on hello data source, and retry hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="a6d8b-109">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="a6d8b-109">Supported scenarios</span></span>
<span data-ttu-id="a6d8b-110">Aktivita kopírování podporuje tři scénáře pro zjišťování, přeskočí a protokolování nekompatibilní data:</span><span class="sxs-lookup"><span data-stu-id="a6d8b-110">Copy Activity supports three scenarios for detecting, skipping, and logging incompatible data:</span></span>

- <span data-ttu-id="a6d8b-111">**Nekompatibilita mezi hello zdrojového datového typu a nativní typ jímky hello**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-111">**Incompatibility between hello source data type and hello sink native type**</span></span>

    <span data-ttu-id="a6d8b-112">Příklad: kopírování dat ze souboru CSV v objektu Blob úložiště tooa SQL databáze s definici schématu, která obsahuje tři **INT** typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-112">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains three **INT** type columns.</span></span> <span data-ttu-id="a6d8b-113">Hello řádků souboru CSV, obsahující číselná data, jako například `123,456,789` zkopírují úspěšně toohello podřízený úložiště.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-113">hello CSV file rows that contain numeric data, such as `123,456,789` are copied successfully toohello sink store.</span></span> <span data-ttu-id="a6d8b-114">Ale hello řádky, které obsahují jiné než číselné hodnoty, jako například `123,456,abc` jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-114">However, hello rows that contain non-numeric values, such as `123,456,abc` are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="a6d8b-115">**Došlo k neshodě v hello počet sloupců mezi hello zdroj a jímka hello**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-115">**Mismatch in hello number of columns between hello source and hello sink**</span></span>

    <span data-ttu-id="a6d8b-116">Příklad: kopírování dat ze souboru CSV v objektu Blob úložiště tooa SQL databáze s definici schématu, která obsahuje šest sloupce.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-116">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains six columns.</span></span> <span data-ttu-id="a6d8b-117">Hello souboru CSV, který řádky, které obsahují šesti sloupce jsou úspěšně zkopírovány toohello podřízený úložiště.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-117">hello CSV file rows that contain six columns are copied successfully toohello sink store.</span></span> <span data-ttu-id="a6d8b-118">Hello CSV souboru řádky, které obsahují více nebo méně než šest sloupce jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-118">hello CSV file rows that contain more or fewer than six columns are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="a6d8b-119">**Porušení primárního klíče při zápisu tooa relační databáze**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-119">**Primary key violation when writing tooa relational database**</span></span>

    <span data-ttu-id="a6d8b-120">Příklad: kopírování dat z databáze SQL tooa systému SQL server.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-120">For example: Copy data from a SQL server tooa SQL database.</span></span> <span data-ttu-id="a6d8b-121">Primární klíč je definována v hello jímku SQL databáze, ale žádný primární klíč je definována v systému SQL server hello zdroje.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-121">A primary key is defined in hello sink SQL database, but no such primary key is defined in hello source SQL server.</span></span> <span data-ttu-id="a6d8b-122">Hello duplicitní řádky, které existují v hello zdroj nesmí být zkopírovaný toohello jímky.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-122">hello duplicated rows that exist in hello source cannot be copied toohello sink.</span></span> <span data-ttu-id="a6d8b-123">Aktivita kopírování zkopíruje pouze hello první řádek hello zdroje dat do hello jímky.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-123">Copy Activity copies only hello first row of hello source data into hello sink.</span></span> <span data-ttu-id="a6d8b-124">Hello řádky další zdroje, které obsahují hello duplicitní hodnotu primárního klíče jsou rozpoznána jako nekompatibilní a se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-124">hello subsequent source rows that contain hello duplicated primary key value are detected as incompatible and are skipped.</span></span>

## <a name="configuration"></a><span data-ttu-id="a6d8b-125">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="a6d8b-125">Configuration</span></span>
<span data-ttu-id="a6d8b-126">Hello následující příklad uvádí tooconfigure definici JSON přeskočení hello nekompatibilní řádků v aktivitě kopírování:</span><span class="sxs-lookup"><span data-stu-id="a6d8b-126">hello following example provides a JSON definition tooconfigure skipping hello incompatible rows in Copy Activity:</span></span>

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

| <span data-ttu-id="a6d8b-127">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="a6d8b-127">Property</span></span> | <span data-ttu-id="a6d8b-128">Popis</span><span class="sxs-lookup"><span data-stu-id="a6d8b-128">Description</span></span> | <span data-ttu-id="a6d8b-129">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="a6d8b-129">Allowed values</span></span> | <span data-ttu-id="a6d8b-130">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a6d8b-130">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a6d8b-131">**enableSkipIncompatibleRow**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-131">**enableSkipIncompatibleRow**</span></span> | <span data-ttu-id="a6d8b-132">Povolte přeskočení nekompatibilní řádků při kopírování nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-132">Enable skipping incompatible rows during copy or not.</span></span> | <span data-ttu-id="a6d8b-133">True</span><span class="sxs-lookup"><span data-stu-id="a6d8b-133">True</span></span><br/><span data-ttu-id="a6d8b-134">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="a6d8b-134">False (default)</span></span> | <span data-ttu-id="a6d8b-135">Ne</span><span class="sxs-lookup"><span data-stu-id="a6d8b-135">No</span></span> |
| <span data-ttu-id="a6d8b-136">**redirectIncompatibleRowSettings**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-136">**redirectIncompatibleRowSettings**</span></span> | <span data-ttu-id="a6d8b-137">Skupinu vlastností, které lze zadat, když chcete toolog hello nekompatibilní řádků.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-137">A group of properties that can be specified when you want toolog hello incompatible rows.</span></span> | &nbsp; | <span data-ttu-id="a6d8b-138">Ne</span><span class="sxs-lookup"><span data-stu-id="a6d8b-138">No</span></span> |
| <span data-ttu-id="a6d8b-139">**linkedServiceName**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-139">**linkedServiceName**</span></span> | <span data-ttu-id="a6d8b-140">Hello propojená služba Azure Storage toostore hello protokolu, který obsahuje řádky hello přeskočena.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-140">hello linked service of Azure Storage toostore hello log that contains hello skipped rows.</span></span> | <span data-ttu-id="a6d8b-141">Název Hello [azurestorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) propojené služby, která odkazuje toohello úložiště instance, že chcete soubor protokolu hello toostore toouse.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-141">hello name of an [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linked service, which refers toohello storage instance that you want toouse toostore hello log file.</span></span> | <span data-ttu-id="a6d8b-142">Ne</span><span class="sxs-lookup"><span data-stu-id="a6d8b-142">No</span></span> |
| <span data-ttu-id="a6d8b-143">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="a6d8b-143">**path**</span></span> | <span data-ttu-id="a6d8b-144">Hello cesta souboru protokolu hello, který obsahuje hello přeskočen řádků.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-144">hello path of hello log file that contains hello skipped rows.</span></span> | <span data-ttu-id="a6d8b-145">Zadejte cestu úložiště objektů Blob hello má toouse toolog hello nekompatibilní data.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-145">Specify hello Blob storage path that you want toouse toolog hello incompatible data.</span></span> <span data-ttu-id="a6d8b-146">Pokud nezadáte cestu, služba hello vytvoří kontejner pro vás.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-146">If you do not provide a path, hello service creates a container for you.</span></span> | <span data-ttu-id="a6d8b-147">Ne</span><span class="sxs-lookup"><span data-stu-id="a6d8b-147">No</span></span> |

## <a name="monitoring"></a><span data-ttu-id="a6d8b-148">Monitorování</span><span class="sxs-lookup"><span data-stu-id="a6d8b-148">Monitoring</span></span>
<span data-ttu-id="a6d8b-149">Po dokončení aktivity kopírování hello při spuštění, můžete zjistit hello Počet přeskočených řádků v části Sledování hello:</span><span class="sxs-lookup"><span data-stu-id="a6d8b-149">After hello copy activity run completes, you can see hello number of skipped rows in hello monitoring section:</span></span>

![Monitorování přeskočen nekompatibilní řádků](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

<span data-ttu-id="a6d8b-151">Pokud nakonfigurujete toolog hello nekompatibilní řádků, můžete najít soubor protokolu hello v této cestě: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` v souboru protokolu hello, můžete zobrazit hello řádky, které byly přeskočeny a hello hlavní příčinu nekompatibilita hello.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-151">If you configure toolog hello incompatible rows, you can find hello log file at this path: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In hello log file, you can see hello rows that were skipped and hello root cause of hello incompatibility.</span></span>

<span data-ttu-id="a6d8b-152">Původní data hello a odpovídající chyba hello jsou protokolovány v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="a6d8b-152">Both hello original data and hello corresponding error are logged in hello file.</span></span> <span data-ttu-id="a6d8b-153">Příklad obsahu souboru protokolu hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a6d8b-153">An example of hello log file content is as follows:</span></span>
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a><span data-ttu-id="a6d8b-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a6d8b-154">Next steps</span></span>
<span data-ttu-id="a6d8b-155">toolearn Další informace o aktivitě kopírování Azure Data Factory najdete v části [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="a6d8b-155">toolearn more about Azure Data Factory Copy Activity, see [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>
