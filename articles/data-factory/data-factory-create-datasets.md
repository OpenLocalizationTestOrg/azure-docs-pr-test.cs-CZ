---
title: "Vytvoření datových sad v Azure Data Factory | Microsoft Docs"
description: "Postup vytvoření datových sad v Azure Data Factory s příklady, které používají vlastnosti, například posun a anchorDateTime."
keywords: "Vytvoření datové sady, například datové sady, posun příklad"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: shlo
ms.openlocfilehash: 6fd58edd830df8ea3f77a68e8dfcaf6de055b17c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="datasets-in-azure-data-factory"></a><span data-ttu-id="c101d-104">Datové sady ve službě Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c101d-104">Datasets in Azure Data Factory</span></span>
<span data-ttu-id="c101d-105">Tento článek popisuje, jaké datové sady se, jak jsou definovány ve formátu JSON, a způsobu jejich použití v Azure Data Factory kanálů.</span><span class="sxs-lookup"><span data-stu-id="c101d-105">This article describes what datasets are, how they are defined in JSON format, and how they are used in Azure Data Factory pipelines.</span></span> <span data-ttu-id="c101d-106">Poskytuje podrobnosti o jednotlivých částech (například struktura, dostupnost a zásad) v definici JSON datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-106">It provides details about each section (for example, structure, availability, and policy) in the dataset JSON definition.</span></span> <span data-ttu-id="c101d-107">Tento článek také poskytuje příklady pro použití **posun**, **anchorDateTime**, a **styl** vlastnosti v definici JSON datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-107">The article also provides examples for using the **offset**, **anchorDateTime**, and **style** properties in a dataset JSON definition.</span></span>

> [!NOTE]
> <span data-ttu-id="c101d-108">Pokud jste nový objekt pro vytváření dat, najdete v části [Úvod do Azure Data Factory](data-factory-introduction.md) Přehled.</span><span class="sxs-lookup"><span data-stu-id="c101d-108">If you are new to Data Factory, see [Introduction to Azure Data Factory](data-factory-introduction.md) for an overview.</span></span> <span data-ttu-id="c101d-109">Pokud nemáte praktických zkušeností s vytvářením objektů pro vytváření dat, vám může pomoci lépe porozumět načtením [kurzu transformaci dat](data-factory-build-your-first-pipeline.md) a [kurzu přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-109">If you do not have hands-on experience with creating data factories, you can gain a better understanding by reading the [data transformation tutorial](data-factory-build-your-first-pipeline.md) and the [data movement tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="c101d-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="c101d-110">Overview</span></span>
<span data-ttu-id="c101d-111">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="c101d-111">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="c101d-112">A **kanálu** je logické seskupení **aktivity** společně provádějí úlohy.</span><span class="sxs-lookup"><span data-stu-id="c101d-112">A **pipeline** is a logical grouping of **activities** that together perform a task.</span></span> <span data-ttu-id="c101d-113">Aktivity v kanálu definovat akce lze provádět na vaše data.</span><span class="sxs-lookup"><span data-stu-id="c101d-113">The activities in a pipeline define actions to perform on your data.</span></span> <span data-ttu-id="c101d-114">Může například pomocí aktivity kopírování zkopírovat data z místního serveru SQL do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="c101d-114">For example, you might use a copy activity to copy data from an on-premises SQL Server to Azure Blob storage.</span></span> <span data-ttu-id="c101d-115">Pak můžete použít aktivitu Hive, která se spouští skript Hive v clusteru Azure HDInsight pro zpracování dat z úložiště objektů Blob nevytvořila výstupní data.</span><span class="sxs-lookup"><span data-stu-id="c101d-115">Then, you might use a Hive activity that runs a Hive script on an Azure HDInsight cluster to process data from Blob storage to produce output data.</span></span> <span data-ttu-id="c101d-116">Nakonec můžete použít druhý aktivity kopírování zkopírovat výstupní data do Azure SQL Data Warehouse, na které business intelligence (BI), vytváření sestav jsou integrované řešení.</span><span class="sxs-lookup"><span data-stu-id="c101d-116">Finally, you might use a second copy activity to copy the output data to Azure SQL Data Warehouse, on top of which business intelligence (BI) reporting solutions are built.</span></span> <span data-ttu-id="c101d-117">Další informace o kanály a aktivity, najdete v části [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-117">For more information about pipelines and activities, see [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md).</span></span>

<span data-ttu-id="c101d-118">Aktivita může trvat vstup nula nebo více **datové sady**a vytvoří výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-118">An activity can take zero or more input **datasets**, and produce one or more output datasets.</span></span> <span data-ttu-id="c101d-119">Představuje vstupní datová sada vstupem pro aktivitu v kanálu a výstupní datové reprezentuje výstup aktivity.</span><span class="sxs-lookup"><span data-stu-id="c101d-119">An input dataset represents the input for an activity in the pipeline, and an output dataset represents the output for the activity.</span></span> <span data-ttu-id="c101d-120">Datové sady identifikují dat v rámci různých úložišť dat, jako je například tabulek, souborů, složek a dokumentů.</span><span class="sxs-lookup"><span data-stu-id="c101d-120">Datasets identify data within different data stores, such as tables, files, folders, and documents.</span></span> <span data-ttu-id="c101d-121">Například datové sadě služby Azure Blob Určuje kontejner objektů blob a složky v úložišti objektů Blob, ze kterého by měl kanálu číst data.</span><span class="sxs-lookup"><span data-stu-id="c101d-121">For example, an Azure Blob dataset specifies the blob container and folder in Blob storage from which the pipeline should read the data.</span></span> 

<span data-ttu-id="c101d-122">Než vytvoříte datové sady, vytvořit **propojená služba** propojit data store k objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="c101d-122">Before you create a dataset, create a **linked service** to link your data store to the data factory.</span></span> <span data-ttu-id="c101d-123">Propojené služby jsou velmi podobné připojovacím řetězcům, které definují informace o připojení, které služba Data Factory potřebuje pro připojení k externím prostředkům.</span><span class="sxs-lookup"><span data-stu-id="c101d-123">Linked services are much like connection strings, which define the connection information needed for Data Factory to connect to external resources.</span></span> <span data-ttu-id="c101d-124">Datové sady identifikují dat v rámci propojených úložištích dat, jako jsou tabulky SQL, souborů, složek a dokumentů.</span><span class="sxs-lookup"><span data-stu-id="c101d-124">Datasets identify data within the linked data stores, such as SQL tables, files, folders, and documents.</span></span> <span data-ttu-id="c101d-125">Například Azure Storage propojená služba propojuje účet úložiště do služby data factory.</span><span class="sxs-lookup"><span data-stu-id="c101d-125">For example, an Azure Storage linked service links a storage account to the data factory.</span></span> <span data-ttu-id="c101d-126">Datové sadě služby Azure Blob představuje kontejner objektů blob a složky, která obsahuje vstupní objekty BLOB ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="c101d-126">An Azure Blob dataset represents the blob container and the folder that contains the input blobs to be processed.</span></span> 

<span data-ttu-id="c101d-127">Tady je ukázkový scénář.</span><span class="sxs-lookup"><span data-stu-id="c101d-127">Here is a sample scenario.</span></span> <span data-ttu-id="c101d-128">Ke zkopírování dat z úložiště objektů Blob k databázi SQL, vytvoříte dvě propojené služby: úložiště Azure a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c101d-128">To copy data from Blob storage to a SQL database, you create two linked services: Azure Storage and Azure SQL Database.</span></span> <span data-ttu-id="c101d-129">Pak vytvořte dvě datové sady: datovou sadu objektu Blob Azure, (který odkazuje na službu Azure Storage, propojené) a tabulky SQL Azure datovou sadu (odkazuje na službu Azure SQL Database propojené).</span><span class="sxs-lookup"><span data-stu-id="c101d-129">Then, create two datasets: Azure Blob dataset (which refers to the Azure Storage linked service) and Azure SQL Table dataset (which refers to the Azure SQL Database linked service).</span></span> <span data-ttu-id="c101d-130">Azure Storage a Azure SQL Database propojené služby obsahovat připojovací řetězce, které objekt pro vytváření dat používá za běhu k připojení k Azure Storage a Azure SQL Database, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c101d-130">The Azure Storage and Azure SQL Database linked services contain connection strings that Data Factory uses at runtime to connect to your Azure Storage and Azure SQL Database, respectively.</span></span> <span data-ttu-id="c101d-131">Datovou sadu objektu Blob Azure Určuje kontejner objektů blob a objektů blob složku, která obsahuje vstupní objekty BLOB ve službě Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c101d-131">The Azure Blob dataset specifies the blob container and blob folder that contains the input blobs in your Blob storage.</span></span> <span data-ttu-id="c101d-132">Datová sada tabulky SQL Azure Určuje tabulku SQL ve vaší databázi SQL, na které má zkopírovat data.</span><span class="sxs-lookup"><span data-stu-id="c101d-132">The Azure SQL Table dataset specifies the SQL table in your SQL database to which the data is to be copied.</span></span>

<span data-ttu-id="c101d-133">Následující obrázek znázorňuje vztahy mezi kanálu, aktivity, datové sady a propojené služby objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="c101d-133">The following diagram shows the relationships among pipeline, activity, dataset, and linked service in Data Factory:</span></span> 

![Vztah mezi kanálu, aktivity, datové sady, propojených služeb](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a><span data-ttu-id="c101d-135">JSON datové sady</span><span class="sxs-lookup"><span data-stu-id="c101d-135">Dataset JSON</span></span>
<span data-ttu-id="c101d-136">Datové sady ve službě Data Factory je definováno ve formátu JSON následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c101d-136">A dataset in Data Factory is defined in JSON format as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="c101d-137">Následující tabulka popisuje vlastnosti v výše uvedený kód JSON:</span><span class="sxs-lookup"><span data-stu-id="c101d-137">The following table describes properties in the above JSON:</span></span>   

| <span data-ttu-id="c101d-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c101d-138">Property</span></span> | <span data-ttu-id="c101d-139">Popis</span><span class="sxs-lookup"><span data-stu-id="c101d-139">Description</span></span> | <span data-ttu-id="c101d-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c101d-140">Required</span></span> | <span data-ttu-id="c101d-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c101d-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c101d-142">jméno</span><span class="sxs-lookup"><span data-stu-id="c101d-142">name</span></span> |<span data-ttu-id="c101d-143">Název datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-143">Name of the dataset.</span></span> <span data-ttu-id="c101d-144">V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování.</span><span class="sxs-lookup"><span data-stu-id="c101d-144">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="c101d-145">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-145">Yes</span></span> |<span data-ttu-id="c101d-146">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-146">NA</span></span> |
| <span data-ttu-id="c101d-147">type</span><span class="sxs-lookup"><span data-stu-id="c101d-147">type</span></span> |<span data-ttu-id="c101d-148">Typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-148">Type of the dataset.</span></span> <span data-ttu-id="c101d-149">Zadejte jeden z typů podporovaných službou Data Factory (například: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="c101d-149">Specify one of the types supported by Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <br/><br/><span data-ttu-id="c101d-150">Podrobnosti najdete v tématu [typ sady](#Type).</span><span class="sxs-lookup"><span data-stu-id="c101d-150">For details, see [Dataset type](#Type).</span></span> |<span data-ttu-id="c101d-151">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-151">Yes</span></span> |<span data-ttu-id="c101d-152">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-152">NA</span></span> |
| <span data-ttu-id="c101d-153">Struktura</span><span class="sxs-lookup"><span data-stu-id="c101d-153">structure</span></span> |<span data-ttu-id="c101d-154">Schéma datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-154">Schema of the dataset.</span></span><br/><br/><span data-ttu-id="c101d-155">Podrobnosti najdete v tématu [strukturu datové sady](#Structure).</span><span class="sxs-lookup"><span data-stu-id="c101d-155">For details, see [Dataset structure](#Structure).</span></span> |<span data-ttu-id="c101d-156">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-156">No</span></span> |<span data-ttu-id="c101d-157">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-157">NA</span></span> |
| <span data-ttu-id="c101d-158">rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="c101d-158">typeProperties</span></span> | <span data-ttu-id="c101d-159">Vlastnosti typu se liší pro jednotlivé typy (například: Azure Blob, tabulka Azure SQL).</span><span class="sxs-lookup"><span data-stu-id="c101d-159">The type properties are different for each type (for example: Azure Blob, Azure SQL table).</span></span> <span data-ttu-id="c101d-160">Podrobnosti o svých vlastnostech a podporované typy najdete v tématu [typ sady](#Type).</span><span class="sxs-lookup"><span data-stu-id="c101d-160">For details on the supported types and their properties, see [Dataset type](#Type).</span></span> |<span data-ttu-id="c101d-161">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-161">Yes</span></span> |<span data-ttu-id="c101d-162">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-162">NA</span></span> |
| <span data-ttu-id="c101d-163">external</span><span class="sxs-lookup"><span data-stu-id="c101d-163">external</span></span> | <span data-ttu-id="c101d-164">Logický příznak k určení, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="c101d-164">Boolean flag to specify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> <span data-ttu-id="c101d-165">Není-li vstupní datové sady pro aktivitu v kanálu aktuální, tento příznak nastavte na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="c101d-165">If the input dataset for an activity is not produced by the current pipeline, set this flag to true.</span></span> <span data-ttu-id="c101d-166">Tento příznak nastavte na hodnotu true pro vstupní datové sady první aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c101d-166">Set this flag to true for the input dataset of the first activity in the pipeline.</span></span>  |<span data-ttu-id="c101d-167">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-167">No</span></span> |<span data-ttu-id="c101d-168">False</span><span class="sxs-lookup"><span data-stu-id="c101d-168">false</span></span> |
| <span data-ttu-id="c101d-169">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c101d-169">availability</span></span> | <span data-ttu-id="c101d-170">Definuje okna pro zpracování (například hodinové nebo denní) nebo řezů model pro produkční datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-170">Defines the processing window (for example, hourly or daily) or the slicing model for the dataset production.</span></span> <span data-ttu-id="c101d-171">Jednotlivé jednotky data využívat a vyprodukované spuštění aktivity se nazývá datový řez.</span><span class="sxs-lookup"><span data-stu-id="c101d-171">Each unit of data consumed and produced by an activity run is called a data slice.</span></span> <span data-ttu-id="c101d-172">Pokud dostupnosti výstupní datové je nastavena na hodnotu denně (frekvenci - den, interval - 1), řez se vytvoří každý den.</span><span class="sxs-lookup"><span data-stu-id="c101d-172">If the availability of an output dataset is set to daily (frequency - Day, interval - 1), a slice is produced daily.</span></span> <br/><br/><span data-ttu-id="c101d-173">Podrobnosti najdete v tématu [datovou sadu dostupnosti](#Availability).</span><span class="sxs-lookup"><span data-stu-id="c101d-173">For details, see [Dataset availability](#Availability).</span></span> <br/><br/><span data-ttu-id="c101d-174">Podrobnosti na datovou sadu řezů modelu najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c101d-174">For details on the dataset slicing model, see the [Scheduling and execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="c101d-175">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-175">Yes</span></span> |<span data-ttu-id="c101d-176">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-176">NA</span></span> |
| <span data-ttu-id="c101d-177">Zásady</span><span class="sxs-lookup"><span data-stu-id="c101d-177">policy</span></span> |<span data-ttu-id="c101d-178">Definuje kritéria nebo podmínku, musíte splnit řezy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-178">Defines the criteria or the condition that the dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="c101d-179">Podrobnosti najdete v tématu [datovou sadu zásad](#Policy) části.</span><span class="sxs-lookup"><span data-stu-id="c101d-179">For details, see the [Dataset policy](#Policy) section.</span></span> |<span data-ttu-id="c101d-180">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-180">No</span></span> |<span data-ttu-id="c101d-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-181">NA</span></span> |

## <a name="dataset-example"></a><span data-ttu-id="c101d-182">Příklad datové sady</span><span class="sxs-lookup"><span data-stu-id="c101d-182">Dataset example</span></span>
<span data-ttu-id="c101d-183">V následujícím příkladu představuje datovou sadu tabulku s názvem **MyTable** v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c101d-183">In the following example, the dataset represents a table named **MyTable** in a SQL database.</span></span>

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="c101d-184">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="c101d-184">Note the following points:</span></span>

* <span data-ttu-id="c101d-185">**typ** je nastaven na AzureSqlTable.</span><span class="sxs-lookup"><span data-stu-id="c101d-185">**type** is set to AzureSqlTable.</span></span>
* <span data-ttu-id="c101d-186">**Název tabulky** typu (specifické pro typ AzureSqlTable) je nastavena na MyTable.</span><span class="sxs-lookup"><span data-stu-id="c101d-186">**tableName** type property (specific to AzureSqlTable type) is set to MyTable.</span></span>
* <span data-ttu-id="c101d-187">**linkedServiceName** odkazuje na propojené služby typu azuresqldatabase., která je definována v další fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="c101d-187">**linkedServiceName** refers to a linked service of type AzureSqlDatabase, which is defined in the next JSON snippet.</span></span> 
* <span data-ttu-id="c101d-188">**frekvence dostupnosti** je nastaven na den a **interval** je nastaven na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="c101d-188">**availability frequency** is set to Day, and **interval** is set to 1.</span></span> <span data-ttu-id="c101d-189">To znamená, že je řez datovou sadu vytváří denně.</span><span class="sxs-lookup"><span data-stu-id="c101d-189">This means that the dataset slice is produced daily.</span></span>  

<span data-ttu-id="c101d-190">**AzureSqlLinkedService** je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c101d-190">**AzureSqlLinkedService** is defined as follows:</span></span>

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

<span data-ttu-id="c101d-191">V předchozím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="c101d-191">In the preceding JSON snippet:</span></span>

* <span data-ttu-id="c101d-192">**typ** je nastaven na azuresqldatabase..</span><span class="sxs-lookup"><span data-stu-id="c101d-192">**type** is set to AzureSqlDatabase.</span></span>
* <span data-ttu-id="c101d-193">**connectionString** vlastnost typu Určuje informace pro připojení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c101d-193">**connectionString** type property specifies information to connect to a SQL database.</span></span>  

<span data-ttu-id="c101d-194">Jak vidíte, propojené služby definuje, jak se připojit k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="c101d-194">As you can see, the linked service defines how to connect to a SQL database.</span></span> <span data-ttu-id="c101d-195">Datová sada definuje, jaký tabulka je použít jako vstup a výstup aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c101d-195">The dataset defines what table is used as an input and output for the activity in a pipeline.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="c101d-196">Pokud se vytváří datové sady v kanálu, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="c101d-196">Unless a dataset is being produced by the pipeline, it should be marked as **external**.</span></span> <span data-ttu-id="c101d-197">Toto nastavení obecně platí pro vstupy první aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c101d-197">This setting generally applies to inputs of first activity in a pipeline.</span></span>   


## <span data-ttu-id="c101d-198"><a name="Type"></a>Typ sady</span><span class="sxs-lookup"><span data-stu-id="c101d-198"><a name="Type"></a> Dataset type</span></span>
<span data-ttu-id="c101d-199">Typ datové sady závisí na úložiště dat, které používáte.</span><span class="sxs-lookup"><span data-stu-id="c101d-199">The type of the dataset depends on the data store you use.</span></span> <span data-ttu-id="c101d-200">Najdete v následující tabulce najdete seznam úložišť dat podporovaných službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c101d-200">See the following table for a list of data stores supported by Data Factory.</span></span> <span data-ttu-id="c101d-201">Klikněte na úložiště dat pro informace o vytvoření propojené služby a sadu dat pro toto datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="c101d-201">Click a data store to learn how to create a linked service and a dataset for that data store.</span></span>

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> <span data-ttu-id="c101d-202">Úložiště dat, s * může být místní nebo v Azure infrastruktura jako služba (IaaS).</span><span class="sxs-lookup"><span data-stu-id="c101d-202">Data stores with * can be on-premises or on Azure infrastructure as a service (IaaS).</span></span> <span data-ttu-id="c101d-203">Tyto úložiště dat vyžaduje instalaci [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-203">These data stores require you to install [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="c101d-204">V příkladu v předchozí části, typ datové sady je nastavený na **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="c101d-204">In the example in the previous section, the type of the dataset is set to **AzureSqlTable**.</span></span> <span data-ttu-id="c101d-205">Pro datové sadě služby Azure Blob podobně typ datové sady je nastaven na **AzureBlob**, jak je znázorněno v následujícím kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="c101d-205">Similarly, for an Azure Blob dataset, the type of the dataset is set to **AzureBlob**, as shown in the following JSON:</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <span data-ttu-id="c101d-206"><a name="Structure"></a>Struktura datové sady</span><span class="sxs-lookup"><span data-stu-id="c101d-206"><a name="Structure"></a>Dataset structure</span></span>
<span data-ttu-id="c101d-207">**Struktura** část je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="c101d-207">The **structure** section is optional.</span></span> <span data-ttu-id="c101d-208">Definuje schéma datové sady ve obsahující kolekci názvů a typy dat sloupců.</span><span class="sxs-lookup"><span data-stu-id="c101d-208">It defines the schema of the dataset by containing a collection of names and data types of columns.</span></span> <span data-ttu-id="c101d-209">V části struktura použijete k poskytování informací o typu, který se používá k převést typy a mapování sloupců ze zdroje do cíle.</span><span class="sxs-lookup"><span data-stu-id="c101d-209">You use the structure section to provide type information that is used to convert types and map columns from the source to the destination.</span></span> <span data-ttu-id="c101d-210">V následujícím příkladu, datová sada má tři sloupce: `slicetimestamp`, `projectname`, a `pageviews`.</span><span class="sxs-lookup"><span data-stu-id="c101d-210">In the following example, the dataset has three columns: `slicetimestamp`, `projectname`, and `pageviews`.</span></span> <span data-ttu-id="c101d-211">Jsou typu řetězec, řetězec a Decimal, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="c101d-211">They are of type String, String, and Decimal, respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="c101d-212">Každý sloupec ve struktuře obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c101d-212">Each column in the structure contains the following properties:</span></span>

| <span data-ttu-id="c101d-213">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c101d-213">Property</span></span> | <span data-ttu-id="c101d-214">Popis</span><span class="sxs-lookup"><span data-stu-id="c101d-214">Description</span></span> | <span data-ttu-id="c101d-215">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c101d-215">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c101d-216">jméno</span><span class="sxs-lookup"><span data-stu-id="c101d-216">name</span></span> |<span data-ttu-id="c101d-217">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="c101d-217">Name of the column.</span></span> |<span data-ttu-id="c101d-218">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-218">Yes</span></span> |
| <span data-ttu-id="c101d-219">type</span><span class="sxs-lookup"><span data-stu-id="c101d-219">type</span></span> |<span data-ttu-id="c101d-220">Datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="c101d-220">Data type of the column.</span></span>  |<span data-ttu-id="c101d-221">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-221">No</span></span> |
| <span data-ttu-id="c101d-222">Jazyková verze</span><span class="sxs-lookup"><span data-stu-id="c101d-222">culture</span></span> |<span data-ttu-id="c101d-223">. Na základě NET jazykovou verzi, která se použije, když je typ typ formátu .NET: `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="c101d-223">.NET-based culture to be used when the type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="c101d-224">Výchozí hodnota je `en-us`.</span><span class="sxs-lookup"><span data-stu-id="c101d-224">The default is `en-us`.</span></span> |<span data-ttu-id="c101d-225">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-225">No</span></span> |
| <span data-ttu-id="c101d-226">Formát</span><span class="sxs-lookup"><span data-stu-id="c101d-226">format</span></span> |<span data-ttu-id="c101d-227">Řetězec, který se má použít, když je typ typ formátu .NET formátu: `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="c101d-227">Format string to be used when the type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="c101d-228">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-228">No</span></span> |

<span data-ttu-id="c101d-229">Následující pokyny vám pomohou určit, kdy se mají zahrnout informace o struktuře a co mají být zahrnuty **struktura** části.</span><span class="sxs-lookup"><span data-stu-id="c101d-229">The following guidelines help you determine when to include structure information, and what to include in the **structure** section.</span></span>

* <span data-ttu-id="c101d-230">**Pro strukturovaná data zdroje**, zadejte v části struktura pouze v případě, že chcete namapovat zdrojové sloupce na jímky sloupců a jejich názvy nejsou stejné.</span><span class="sxs-lookup"><span data-stu-id="c101d-230">**For structured data sources**, specify the structure section only if you want map source columns to sink columns, and their names are not the same.</span></span> <span data-ttu-id="c101d-231">Tento druh zdroj strukturovaných dat ukládá informace schématu a typu dat společně s samotná data.</span><span class="sxs-lookup"><span data-stu-id="c101d-231">This kind of structured data source stores data schema and type information along with the data itself.</span></span> <span data-ttu-id="c101d-232">Příklady strukturovaných dat zdroje: SQL Server, Oracle a tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="c101d-232">Examples of structured data sources include SQL Server, Oracle, and Azure table.</span></span> 
  
    <span data-ttu-id="c101d-233">Protože je již k dispozici pro strukturovaná data zdroje informací o typu, by neměla zahrnovat informace o typu, pokud obsahovat části struktura.</span><span class="sxs-lookup"><span data-stu-id="c101d-233">As type information is already available for structured data sources, you should not include type information when you do include the structure section.</span></span>
* <span data-ttu-id="c101d-234">**Pro schéma pro zdroje dat pro čtení (konkrétně úložiště objektů Blob)**, můžete k ukládání dat bez ukládání žádné schéma nebo typ informace s daty.</span><span class="sxs-lookup"><span data-stu-id="c101d-234">**For schema on read data sources (specifically Blob storage)**, you can choose to store data without storing any schema or type information with the data.</span></span> <span data-ttu-id="c101d-235">Pro tyto typy zdrojů dat zahrnovat chcete namapovat zdrojové sloupce na jímky sloupce struktury.</span><span class="sxs-lookup"><span data-stu-id="c101d-235">For these types of data sources, include structure when you want to map source columns to sink columns.</span></span> <span data-ttu-id="c101d-236">Také zahrnovat struktura, když je datová sada vstupem pro aktivitu kopírování a datové typy sady zdroje dat mají být převedeny na nativní typy pro jímky.</span><span class="sxs-lookup"><span data-stu-id="c101d-236">Also include structure when the dataset is an input for a copy activity, and data types of source dataset should be converted to native types for the sink.</span></span> 
    
    <span data-ttu-id="c101d-237">Objekt pro vytváření dat podporuje následující hodnoty pro poskytnutí informací o typu ve struktuře: **Int16, Int32, Int64, jedním, Double, Decimal, Byte [], logická hodnota, řetězec, Guid, Datetime, Datetimeoffset a časový interval**.</span><span class="sxs-lookup"><span data-stu-id="c101d-237">Data Factory supports the following values for providing type information in structure: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan**.</span></span> <span data-ttu-id="c101d-238">Tyto hodnoty jsou specifikace CLS (Common Language)-vyhovující,. Na základě NET typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c101d-238">These values are Common Language Specification (CLS)-compliant, .NET-based type values.</span></span>

<span data-ttu-id="c101d-239">Objekt pro vytváření dat automaticky provede převody typů, při přesouvání dat ze zdrojového úložiště dat do úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="c101d-239">Data Factory automatically performs type conversions when moving data from a source data store to a sink data store.</span></span> 
  

## <a name="dataset-availability"></a><span data-ttu-id="c101d-240">Datovou sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="c101d-240">Dataset availability</span></span>
<span data-ttu-id="c101d-241">**Dostupnosti** oddíl v datové sadě definuje okna zpracování (například hodinový, denní, nebo každý týden) pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-241">The **availability** section in a dataset defines the processing window (for example, hourly, daily, or weekly) for the dataset.</span></span> <span data-ttu-id="c101d-242">Další informace o aktivity windows najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-242">For more information about activity windows, see [Scheduling and execution](data-factory-scheduling-and-execution.md).</span></span>

<span data-ttu-id="c101d-243">V následující části dostupnosti Určuje, že výstupní datovou sadu se vytvářejí buď hodinu nebo vstupní datové sady každou hodinu je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="c101d-243">The following availability section specifies that the output dataset is either produced hourly, or the input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="c101d-244">Pokud kanálu má následující počáteční a koncový čas:</span><span class="sxs-lookup"><span data-stu-id="c101d-244">If the pipeline has the following start and end times:</span></span>  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

<span data-ttu-id="c101d-245">Výstupní sada vytváří každou hodinu v rámci kanálu spuštění a ukončení.</span><span class="sxs-lookup"><span data-stu-id="c101d-245">The output dataset is produced hourly within the pipeline start and end times.</span></span> <span data-ttu-id="c101d-246">Proto jsou pět řezy datové sady vyprodukované tímto kanálem, jeden pro každou aktivitu okno (12: 00 - 1 AM, 1: 00 - 2 AM, 2: 00 - 3 AM, 3: 00 - 4 AM, 4: 00 - 5: 00).</span><span class="sxs-lookup"><span data-stu-id="c101d-246">Therefore, there are five dataset slices produced by this pipeline, one for each activity window (12 AM - 1 AM, 1 AM - 2 AM, 2 AM - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM).</span></span> 

<span data-ttu-id="c101d-247">Následující tabulka popisuje vlastnosti, které můžete použít v části dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="c101d-247">The following table describes properties you can use in the availability section:</span></span>

| <span data-ttu-id="c101d-248">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c101d-248">Property</span></span> | <span data-ttu-id="c101d-249">Popis</span><span class="sxs-lookup"><span data-stu-id="c101d-249">Description</span></span> | <span data-ttu-id="c101d-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c101d-250">Required</span></span> | <span data-ttu-id="c101d-251">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c101d-251">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c101d-252">frekvence</span><span class="sxs-lookup"><span data-stu-id="c101d-252">frequency</span></span> |<span data-ttu-id="c101d-253">Určuje časovou jednotku pro produkční řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-253">Specifies the time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="c101d-254"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="c101d-254"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="c101d-255">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-255">Yes</span></span> |<span data-ttu-id="c101d-256">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-256">NA</span></span> |
| <span data-ttu-id="c101d-257">Interval</span><span class="sxs-lookup"><span data-stu-id="c101d-257">interval</span></span> |<span data-ttu-id="c101d-258">Určuje multiplikátor pro četnost.</span><span class="sxs-lookup"><span data-stu-id="c101d-258">Specifies a multiplier for frequency.</span></span><br/><br/><span data-ttu-id="c101d-259">"Frekvence x interval" Určuje, jak často se vytvářejí řez.</span><span class="sxs-lookup"><span data-stu-id="c101d-259">"Frequency x interval" determines how often the slice is produced.</span></span> <span data-ttu-id="c101d-260">Například pokud budete potřebovat datovou sadu, která se rozříznut hodinu, nastavíte <b>frekvence</b> k <b>hodinu</b>, a <b>interval</b> k <b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="c101d-260">For example, if you need the dataset to be sliced on an hourly basis, you set <b>frequency</b> to <b>Hour</b>, and <b>interval</b> to <b>1</b>.</span></span><br/><br/><span data-ttu-id="c101d-261">Všimněte si, že pokud zadáte **frekvence** jako **minutu**, měli byste nastavit interval hodnotu menší než 15.</span><span class="sxs-lookup"><span data-stu-id="c101d-261">Note that if you specify **frequency** as **Minute**, you should set the interval to no less than 15.</span></span> |<span data-ttu-id="c101d-262">Ano</span><span class="sxs-lookup"><span data-stu-id="c101d-262">Yes</span></span> |<span data-ttu-id="c101d-263">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-263">NA</span></span> |
| <span data-ttu-id="c101d-264">Styl</span><span class="sxs-lookup"><span data-stu-id="c101d-264">style</span></span> |<span data-ttu-id="c101d-265">Určuje, zda by měl být řez vytváří na začátku nebo na konci interval.</span><span class="sxs-lookup"><span data-stu-id="c101d-265">Specifies whether the slice should be produced at the start or end of the interval.</span></span><ul><li><span data-ttu-id="c101d-266">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="c101d-266">StartOfInterval</span></span></li><li><span data-ttu-id="c101d-267">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="c101d-267">EndOfInterval</span></span></li></ul><span data-ttu-id="c101d-268">Pokud **frekvence** je nastaven na **měsíc**, a **styl** je nastaven na **EndOfInterval**, řez se vytváří poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="c101d-268">If **frequency** is set to **Month**, and **style** is set to **EndOfInterval**, the slice is produced on the last day of month.</span></span> <span data-ttu-id="c101d-269">Pokud **styl** je nastaven na **StartOfInterval**, řez se vytváří první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="c101d-269">If **style** is set to **StartOfInterval**, the slice is produced on the first day of month.</span></span><br/><br/><span data-ttu-id="c101d-270">Pokud **frekvence** je nastaven na **den**, a **styl** je nastaven na **EndOfInterval**, řez se vytváří za poslední hodinu dne.</span><span class="sxs-lookup"><span data-stu-id="c101d-270">If **frequency** is set to **Day**, and **style** is set to **EndOfInterval**, the slice is produced in the last hour of the day.</span></span><br/><br/><span data-ttu-id="c101d-271">Pokud **frekvence** je nastaven na **hodinu**, a **styl** je nastaven na **EndOfInterval**, řez se vytvářejí na konci za hodinu.</span><span class="sxs-lookup"><span data-stu-id="c101d-271">If **frequency** is set to **Hour**, and **style** is set to **EndOfInterval**, the slice is produced at the end of the hour.</span></span> <span data-ttu-id="c101d-272">Například pro řez dobu 13: 00 – 14: 00, je řez vytvořeného ve 2.</span><span class="sxs-lookup"><span data-stu-id="c101d-272">For example, for a slice for the 1 PM - 2 PM period, the slice is produced at 2 PM.</span></span> |<span data-ttu-id="c101d-273">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-273">No</span></span> |<span data-ttu-id="c101d-274">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="c101d-274">EndOfInterval</span></span> |
| <span data-ttu-id="c101d-275">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="c101d-275">anchorDateTime</span></span> |<span data-ttu-id="c101d-276">Definuje absolutní pozici v čase plánovačem slouží k výpočtu hranice řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-276">Defines the absolute position in time used by the scheduler to compute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="c101d-277">Všimněte si, že pokud tento propoerty částí data, která jsou podrobnější než je zadaná četnost, se ignorují podrobnější částí.</span><span class="sxs-lookup"><span data-stu-id="c101d-277">Note that if this propoerty has date parts that are more granular than the specified frequency, the more granular parts are ignored.</span></span> <span data-ttu-id="c101d-278">Například pokud **interval** je **každou hodinu** (frekvence: hodin a interval: 1) a **anchorDateTime** obsahuje **minuty a sekundy**, pak části minuty a sekundy **anchorDateTime** jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="c101d-278">For example, if the **interval** is **hourly** (frequency: hour and interval: 1), and the **anchorDateTime** contains **minutes and seconds**, then the minutes and seconds parts of **anchorDateTime** are ignored.</span></span> |<span data-ttu-id="c101d-279">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-279">No</span></span> |<span data-ttu-id="c101d-280">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="c101d-280">01/01/0001</span></span> |
| <span data-ttu-id="c101d-281">Posun</span><span class="sxs-lookup"><span data-stu-id="c101d-281">offset</span></span> |<span data-ttu-id="c101d-282">Časový interval, ve kterém jsou zapuštěno počáteční a koncová všech řezech datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-282">Timespan by which the start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="c101d-283">Všimněte si, že pokud obě **anchorDateTime** a **posun** jsou nastaveny, výsledkem je kombinovaná shift.</span><span class="sxs-lookup"><span data-stu-id="c101d-283">Note that if both **anchorDateTime** and **offset** are specified, the result is the combined shift.</span></span> |<span data-ttu-id="c101d-284">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-284">No</span></span> |<span data-ttu-id="c101d-285">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-285">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="c101d-286">Příklad posunutí</span><span class="sxs-lookup"><span data-stu-id="c101d-286">offset example</span></span>
<span data-ttu-id="c101d-287">Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy začnou ve 12: 00 (půlnoc) koordinovaný světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="c101d-287">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM (midnight) Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="c101d-288">Pokud chcete, aby čas spuštění jako čas UTC 6: 00, nastavte posun, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="c101d-288">If you want the start time to be 6 AM UTC time instead, set the offset as shown in the following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="c101d-289">Příklad anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="c101d-289">anchorDateTime example</span></span>
<span data-ttu-id="c101d-290">V následujícím příkladu se sada vytváří jednou za 23 hodin.</span><span class="sxs-lookup"><span data-stu-id="c101d-290">In the following example, the dataset is produced once every 23 hours.</span></span> <span data-ttu-id="c101d-291">První řez spustí v době určeného **anchorDateTime**, který je nastaven na `2017-04-19T08:00:00` (UTC).</span><span class="sxs-lookup"><span data-stu-id="c101d-291">The first slice starts at the time specified by **anchorDateTime**, which is set to `2017-04-19T08:00:00` (UTC).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="c101d-292">Posun nebo styl příklad</span><span class="sxs-lookup"><span data-stu-id="c101d-292">offset/style example</span></span>
<span data-ttu-id="c101d-293">Tyto datové sady je každý měsíc a vytváří 3. v každém měsíci v 8:00 AM (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="c101d-293">The following dataset is monthly, and is produced on the 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <span data-ttu-id="c101d-294"><a name="Policy"></a>Datovou sadu zásad</span><span class="sxs-lookup"><span data-stu-id="c101d-294"><a name="Policy"></a>Dataset policy</span></span>
<span data-ttu-id="c101d-295">**Zásad** oddíl v definici datové sady definuje kritéria nebo podmínku, musíte splnit řezy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c101d-295">The **policy** section in the dataset definition defines the criteria or the condition that the dataset slices must fulfill.</span></span>

### <a name="validation-policies"></a><span data-ttu-id="c101d-296">Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="c101d-296">Validation policies</span></span>
| <span data-ttu-id="c101d-297">Název zásady</span><span class="sxs-lookup"><span data-stu-id="c101d-297">Policy name</span></span> | <span data-ttu-id="c101d-298">Popis</span><span class="sxs-lookup"><span data-stu-id="c101d-298">Description</span></span> | <span data-ttu-id="c101d-299">Použít</span><span class="sxs-lookup"><span data-stu-id="c101d-299">Applied to</span></span> | <span data-ttu-id="c101d-300">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c101d-300">Required</span></span> | <span data-ttu-id="c101d-301">Výchozí</span><span class="sxs-lookup"><span data-stu-id="c101d-301">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="c101d-302">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="c101d-302">minimumSizeMB</span></span> |<span data-ttu-id="c101d-303">Ověří, jestli data v **úložiště objektů Azure Blob** splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="c101d-303">Validates that the data in **Azure Blob storage** meets the minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="c101d-304">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="c101d-304">Azure Blob storage</span></span> |<span data-ttu-id="c101d-305">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-305">No</span></span> |<span data-ttu-id="c101d-306">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-306">NA</span></span> |
| <span data-ttu-id="c101d-307">minimumRows</span><span class="sxs-lookup"><span data-stu-id="c101d-307">minimumRows</span></span> |<span data-ttu-id="c101d-308">Ověří, jestli data v **Azure SQL database** nebo **tabulky Azure** obsahuje minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="c101d-308">Validates that the data in an **Azure SQL database** or an **Azure table** contains the minimum number of rows.</span></span> |<ul><li><span data-ttu-id="c101d-309">Databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="c101d-309">Azure SQL database</span></span></li><li><span data-ttu-id="c101d-310">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="c101d-310">Azure table</span></span></li></ul> |<span data-ttu-id="c101d-311">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-311">No</span></span> |<span data-ttu-id="c101d-312">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="c101d-312">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="c101d-313">Příklady</span><span class="sxs-lookup"><span data-stu-id="c101d-313">Examples</span></span>
<span data-ttu-id="c101d-314">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="c101d-314">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="c101d-315">**minimumRows:**</span><span class="sxs-lookup"><span data-stu-id="c101d-315">**minimumRows:**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a><span data-ttu-id="c101d-316">Externích datových sad</span><span class="sxs-lookup"><span data-stu-id="c101d-316">External datasets</span></span>
<span data-ttu-id="c101d-317">Externích datových sad, jsou ty, které nejsou spuštěné kanálu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="c101d-317">External datasets are the ones that are not produced by a running pipeline in the data factory.</span></span> <span data-ttu-id="c101d-318">Pokud se datová sada je označena jako **externí**, **ExternalData** zásad může být definována, jež ovlivňují chování řez dostupnost datové sady.</span><span class="sxs-lookup"><span data-stu-id="c101d-318">If the dataset is marked as **external**, the **ExternalData** policy may be defined to influence the behavior of the dataset slice availability.</span></span>

<span data-ttu-id="c101d-319">Pokud datové sady je vytvářen službou Data Factory, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="c101d-319">Unless a dataset is being produced by Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="c101d-320">Toto nastavení se obvykle platí pro vstupy první aktivitu v kanálu, pokud se používá aktivitu nebo řetězení kanálu.</span><span class="sxs-lookup"><span data-stu-id="c101d-320">This setting generally applies to the inputs of first activity in a pipeline, unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="c101d-321">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="c101d-321">Name</span></span> | <span data-ttu-id="c101d-322">Popis</span><span class="sxs-lookup"><span data-stu-id="c101d-322">Description</span></span> | <span data-ttu-id="c101d-323">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c101d-323">Required</span></span> | <span data-ttu-id="c101d-324">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="c101d-324">Default value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c101d-325">dataDelay</span><span class="sxs-lookup"><span data-stu-id="c101d-325">dataDelay</span></span> |<span data-ttu-id="c101d-326">Doba zpoždění kontroly na dostupnost externích dat pro danou řez.</span><span class="sxs-lookup"><span data-stu-id="c101d-326">The time to delay the check on the availability of the external data for the given slice.</span></span> <span data-ttu-id="c101d-327">Můžete například zpoždění hodinové kontroly pomocí tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="c101d-327">For example, you can delay an hourly check by using this setting.</span></span><br/><br/><span data-ttu-id="c101d-328">Toto nastavení platí jenom pro aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="c101d-328">The setting only applies to the present time.</span></span>  <span data-ttu-id="c101d-329">Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření se spustí: 10: 00.</span><span class="sxs-lookup"><span data-stu-id="c101d-329">For example, if it is 1:00 PM right now and this value is 10 minutes, the validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="c101d-330">Všimněte si, že toto nastavení neovlivňuje řezy v minulosti.</span><span class="sxs-lookup"><span data-stu-id="c101d-330">Note that this setting does not affect slices in the past.</span></span> <span data-ttu-id="c101d-331">Řezy s **řez koncový čas** + **dataDelay** < **nyní** jsou zpracovávány bez jakéhokoli zpoždění.</span><span class="sxs-lookup"><span data-stu-id="c101d-331">Slices with **Slice End Time** + **dataDelay** < **Now** are processed without any delay.</span></span><br/><br/><span data-ttu-id="c101d-332">Časy větší než 23:59 hodin by měl být určena pomocí `day.hours:minutes:seconds` formátu.</span><span class="sxs-lookup"><span data-stu-id="c101d-332">Times greater than 23:59 hours should be specified by using the `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="c101d-333">Například pokud chcete zadat 24 hodin, nepoužívejte 24:00:00.</span><span class="sxs-lookup"><span data-stu-id="c101d-333">For example, to specify 24 hours, don't use 24:00:00.</span></span> <span data-ttu-id="c101d-334">Místo toho použijte 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="c101d-334">Instead, use 1.00:00:00.</span></span> <span data-ttu-id="c101d-335">Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="c101d-335">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="c101d-336">1 den a 4 hodiny zadejte 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="c101d-336">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="c101d-337">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-337">No</span></span> |<span data-ttu-id="c101d-338">0</span><span class="sxs-lookup"><span data-stu-id="c101d-338">0</span></span> |
| <span data-ttu-id="c101d-339">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="c101d-339">retryInterval</span></span> |<span data-ttu-id="c101d-340">Doba čekání mezi selhání a další pokus.</span><span class="sxs-lookup"><span data-stu-id="c101d-340">The wait time between a failure and the next attempt.</span></span> <span data-ttu-id="c101d-341">Toto nastavení platí pro aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="c101d-341">This setting applies to present time.</span></span> <span data-ttu-id="c101d-342">Pokud předchozí zkuste se nezdařila, je dalším pokusu o po **retryInterval** období.</span><span class="sxs-lookup"><span data-stu-id="c101d-342">If the previous try failed, the next try is after the **retryInterval** period.</span></span> <br/><br/><span data-ttu-id="c101d-343">Pokud je 1:00 PM nyní, můžeme začít prvního pokusu.</span><span class="sxs-lookup"><span data-stu-id="c101d-343">If it is 1:00 PM right now, we begin the first try.</span></span> <span data-ttu-id="c101d-344">Pokud doba trvání dokončení první kontrola ověření je 1 minuta a operace se nezdařila, další pokus proběhne v 1:00 + 1 min (doba trvání) + 1min (interval opakování) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="c101d-344">If the duration to complete the first validation check is 1 minute and the operation failed, the next retry is at 1:00 + 1min (duration) + 1min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="c101d-345">Řezy v minulosti není k dispozici žádné zpoždění není.</span><span class="sxs-lookup"><span data-stu-id="c101d-345">For slices in the past, there is no delay.</span></span> <span data-ttu-id="c101d-346">Opakovaném dojde okamžitě.</span><span class="sxs-lookup"><span data-stu-id="c101d-346">The retry happens immediately.</span></span> |<span data-ttu-id="c101d-347">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-347">No</span></span> |<span data-ttu-id="c101d-348">00:01:00 (1 min)</span><span class="sxs-lookup"><span data-stu-id="c101d-348">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="c101d-349">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="c101d-349">retryTimeout</span></span> |<span data-ttu-id="c101d-350">Časový limit pro jednotlivé pokusy o opakování.</span><span class="sxs-lookup"><span data-stu-id="c101d-350">The timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="c101d-351">Pokud je tato vlastnost nastavená na 10 minut, by se během deseti minut dokončit ověření.</span><span class="sxs-lookup"><span data-stu-id="c101d-351">If this property is set to 10 minutes, the validation should be completed within 10 minutes.</span></span> <span data-ttu-id="c101d-352">Pokud trvá déle než 10 minut, aby k ověření, opakovaném časového limitu.</span><span class="sxs-lookup"><span data-stu-id="c101d-352">If it takes longer than 10 minutes to perform the validation, the retry times out.</span></span><br/><br/><span data-ttu-id="c101d-353">Pokud všechny pokusy o ověření časový limit řez je označen jako **TimedOut**.</span><span class="sxs-lookup"><span data-stu-id="c101d-353">If all attempts for the validation time out, the slice is marked as **TimedOut**.</span></span> |<span data-ttu-id="c101d-354">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-354">No</span></span> |<span data-ttu-id="c101d-355">00:10:00 (10 minut)</span><span class="sxs-lookup"><span data-stu-id="c101d-355">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="c101d-356">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="c101d-356">maximumRetry</span></span> |<span data-ttu-id="c101d-357">Stanovený počet zkontrolujte dostupnost externí data.</span><span class="sxs-lookup"><span data-stu-id="c101d-357">The number of times to check for the availability of the external data.</span></span> <span data-ttu-id="c101d-358">Maximální povolená hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="c101d-358">The maximum allowed value is 10.</span></span> |<span data-ttu-id="c101d-359">Ne</span><span class="sxs-lookup"><span data-stu-id="c101d-359">No</span></span> |<span data-ttu-id="c101d-360">3</span><span class="sxs-lookup"><span data-stu-id="c101d-360">3</span></span> |


## <a name="create-datasets"></a><span data-ttu-id="c101d-361">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="c101d-361">Create datasets</span></span>
<span data-ttu-id="c101d-362">Datové sady můžete vytvořit pomocí jedné z těchto nástrojů nebo sady SDK:</span><span class="sxs-lookup"><span data-stu-id="c101d-362">You can create datasets by using one of these tools or SDKs:</span></span> 

- <span data-ttu-id="c101d-363">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="c101d-363">Copy Wizard</span></span> 
- <span data-ttu-id="c101d-364">portál Azure</span><span class="sxs-lookup"><span data-stu-id="c101d-364">Azure portal</span></span>
- <span data-ttu-id="c101d-365">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c101d-365">Visual Studio</span></span>
- <span data-ttu-id="c101d-366">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c101d-366">PowerShell</span></span>
- <span data-ttu-id="c101d-367">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c101d-367">Azure Resource Manager template</span></span>
- <span data-ttu-id="c101d-368">REST API</span><span class="sxs-lookup"><span data-stu-id="c101d-368">REST API</span></span>
- <span data-ttu-id="c101d-369">.NET API</span><span class="sxs-lookup"><span data-stu-id="c101d-369">.NET API</span></span>

<span data-ttu-id="c101d-370">Najdete v následujících kurzech podrobné pokyny pro vytváření kanálů a datové sady pomocí jedné z těchto nástrojů nebo sady SDK:</span><span class="sxs-lookup"><span data-stu-id="c101d-370">See the following tutorials for step-by-step instructions for creating pipelines and datasets by using one of these tools or SDKs:</span></span>
 
- [<span data-ttu-id="c101d-371">Vytvoření kanálu s aktivitou transformace dat</span><span class="sxs-lookup"><span data-stu-id="c101d-371">Build a pipeline with a data transformation activity</span></span>](data-factory-build-your-first-pipeline.md)
- [<span data-ttu-id="c101d-372">Vytvoření kanálu s aktivitou přesun dat</span><span class="sxs-lookup"><span data-stu-id="c101d-372">Build a pipeline with a data movement activity</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

<span data-ttu-id="c101d-373">Po kanálu se vytvoří a nasadí, můžete spravovat a monitorovat kanály pomocí oken Azure portal nebo aplikace, monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="c101d-373">After a pipeline is created and deployed, you can manage and monitor your pipelines by using the Azure portal blades, or the Monitoring and Management app.</span></span> <span data-ttu-id="c101d-374">Najdete v následujících tématech podrobné pokyny:</span><span class="sxs-lookup"><span data-stu-id="c101d-374">See the following topics for step-by-step instructions:</span></span> 

- [<span data-ttu-id="c101d-375">Monitorování a Správa kanálů pomocí oken webu Azure portal</span><span class="sxs-lookup"><span data-stu-id="c101d-375">Monitor and manage pipelines by using Azure portal blades</span></span>](data-factory-monitor-manage-pipelines.md)
- [<span data-ttu-id="c101d-376">Monitorování a Správa kanálů pomocí monitorování a správy aplikace</span><span class="sxs-lookup"><span data-stu-id="c101d-376">Monitor and manage pipelines by using the Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a><span data-ttu-id="c101d-377">Oboru datové sady</span><span class="sxs-lookup"><span data-stu-id="c101d-377">Scoped datasets</span></span>
<span data-ttu-id="c101d-378">Můžete vytvořit datové sady, které jsou omezená na kanálu pomocí **datové sady** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c101d-378">You can create datasets that are scoped to a pipeline by using the **datasets** property.</span></span> <span data-ttu-id="c101d-379">Tyto datové sady lze použít pouze aktivity v rámci tohoto kanálu, nikoli aktivity v jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="c101d-379">These datasets can only be used by activities within this pipeline, not by activities in other pipelines.</span></span> <span data-ttu-id="c101d-380">V následujícím příkladu definuje kanál pomocí dvě datové sady (InputDataset rdc a OutputDataset rdc) má být použit v rámci kanálu.</span><span class="sxs-lookup"><span data-stu-id="c101d-380">The following example defines a pipeline with two datasets (InputDataset-rdc and OutputDataset-rdc) to be used within the pipeline.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="c101d-381">Oboru datové sady jsou podporovány pouze s jednorázové kanály (kde **pipelineMode** je nastaven na **OneTime**).</span><span class="sxs-lookup"><span data-stu-id="c101d-381">Scoped datasets are supported only with one-time pipelines (where **pipelineMode** is set to **OneTime**).</span></span> <span data-ttu-id="c101d-382">V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c101d-382">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details.</span></span>
>
>

```json
{
    "name": "CopyPipeline-rdc",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="c101d-383">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c101d-383">Next steps</span></span>
- <span data-ttu-id="c101d-384">Další informace o kanály najdete v tématu [vytvořit kanály](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-384">For more information about pipelines, see [Create pipelines](data-factory-create-pipelines.md).</span></span> 
- <span data-ttu-id="c101d-385">Další informace o tom, jak jsou naplánované kanálů a jsou prováděny najdete v tématu [plánování a provádění v Azure Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="c101d-385">For more information about how pipelines are scheduled and executed, see [Scheduling and execution in Azure Data Factory](data-factory-scheduling-and-execution.md).</span></span> 
