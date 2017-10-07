---
title: "datové sady aaaCreate v Azure Data Factory | Microsoft Docs"
description: "Zjistěte, jak toocreate datové sady v Azure Data Factory s příklady, které používají vlastnosti, jako posun a anchorDateTime."
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
ms.openlocfilehash: 181859ed250595d756df73e9ebcac08d9e7184c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="datasets-in-azure-data-factory"></a><span data-ttu-id="6f0dc-104">Datové sady ve službě Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6f0dc-104">Datasets in Azure Data Factory</span></span>
<span data-ttu-id="6f0dc-105">Tento článek popisuje, jaké datové sady se, jak jsou definovány ve formátu JSON, a způsobu jejich použití v Azure Data Factory kanálů.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-105">This article describes what datasets are, how they are defined in JSON format, and how they are used in Azure Data Factory pipelines.</span></span> <span data-ttu-id="6f0dc-106">Poskytuje podrobnosti o jednotlivých částech (například struktura, dostupnost a zásad) v definici JSON hello datové sady.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-106">It provides details about each section (for example, structure, availability, and policy) in hello dataset JSON definition.</span></span> <span data-ttu-id="6f0dc-107">Hello článku také obsahuje příklady použití hello **posun**, **anchorDateTime**, a **styl** vlastnosti v definici JSON datové sady.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-107">hello article also provides examples for using hello **offset**, **anchorDateTime**, and **style** properties in a dataset JSON definition.</span></span>

> [!NOTE]
> <span data-ttu-id="6f0dc-108">Pokud jste tooData nový objekt pro vytváření, najdete v části [Úvod tooAzure Data Factory](data-factory-introduction.md) Přehled.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-108">If you are new tooData Factory, see [Introduction tooAzure Data Factory](data-factory-introduction.md) for an overview.</span></span> <span data-ttu-id="6f0dc-109">Pokud nemáte praktických zkušeností s vytvářením objektů pro vytváření dat, vám může pomoci lépe porozumět ve čtení hello [kurzu transformaci dat](data-factory-build-your-first-pipeline.md) a hello [kurzu přesun dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-109">If you do not have hands-on experience with creating data factories, you can gain a better understanding by reading hello [data transformation tutorial](data-factory-build-your-first-pipeline.md) and hello [data movement tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="6f0dc-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="6f0dc-110">Overview</span></span>
<span data-ttu-id="6f0dc-111">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-111">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="6f0dc-112">A **kanálu** je logické seskupení **aktivity** společně provádějí úlohy.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-112">A **pipeline** is a logical grouping of **activities** that together perform a task.</span></span> <span data-ttu-id="6f0dc-113">Hello aktivity v kanálu definují akce tooperform na vaše data.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-113">hello activities in a pipeline define actions tooperform on your data.</span></span> <span data-ttu-id="6f0dc-114">Můžete například použít kopie aktivity toocopy data z tooAzure systému SQL Server místní úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-114">For example, you might use a copy activity toocopy data from an on-premises SQL Server tooAzure Blob storage.</span></span> <span data-ttu-id="6f0dc-115">Pak můžete použít aktivitu Hive, která se spouští skript Hive na model data tooprocess clusteru Azure HDInsight z objektu Blob úložiště tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-115">Then, you might use a Hive activity that runs a Hive script on an Azure HDInsight cluster tooprocess data from Blob storage tooproduce output data.</span></span> <span data-ttu-id="6f0dc-116">Nakonec můžete použít druhé kopie aktivity toocopy hello výstupní data tooAzure SQL Data Warehouse, nad kterou business intelligence (BI), vytváření sestav jsou integrované řešení.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-116">Finally, you might use a second copy activity toocopy hello output data tooAzure SQL Data Warehouse, on top of which business intelligence (BI) reporting solutions are built.</span></span> <span data-ttu-id="6f0dc-117">Další informace o kanály a aktivity, najdete v části [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-117">For more information about pipelines and activities, see [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md).</span></span>

<span data-ttu-id="6f0dc-118">Aktivita může trvat vstup nula nebo více **datové sady**a vytvoří výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-118">An activity can take zero or more input **datasets**, and produce one or more output datasets.</span></span> <span data-ttu-id="6f0dc-119">Vstupní datové sady reprezentuje hello vstup pro aktivitu v kanálu hello a výstupní datové reprezentuje hello výstup aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-119">An input dataset represents hello input for an activity in hello pipeline, and an output dataset represents hello output for hello activity.</span></span> <span data-ttu-id="6f0dc-120">Datové sady identifikují data v rámci různých úložišť dat, jako jsou tabulky, soubory, složky a dokumenty.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-120">Datasets identify data within different data stores, such as tables, files, folders, and documents.</span></span> <span data-ttu-id="6f0dc-121">Například datové sadě služby Azure Blob určuje hello kontejner objektů blob a složky v úložišti objektů Blob, ze které hello kanálu by měl číst hello data.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-121">For example, an Azure Blob dataset specifies hello blob container and folder in Blob storage from which hello pipeline should read hello data.</span></span> 

<span data-ttu-id="6f0dc-122">Než vytvoříte datové sady, vytvořit **propojená služba** toolink data ukládat toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-122">Before you create a dataset, create a **linked service** toolink your data store toohello data factory.</span></span> <span data-ttu-id="6f0dc-123">Propojené služby jsou mnohem jako připojovací řetězce, které definují hello připojení informace potřebné pro vytváření dat tooconnect tooexternal prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-123">Linked services are much like connection strings, which define hello connection information needed for Data Factory tooconnect tooexternal resources.</span></span> <span data-ttu-id="6f0dc-124">Datové sady identifikují dat v rámci hello propojená datová úložiště, jako jsou tabulky SQL, souborů, složek a dokumentů.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-124">Datasets identify data within hello linked data stores, such as SQL tables, files, folders, and documents.</span></span> <span data-ttu-id="6f0dc-125">Například Azure Storage propojená služba propojuje vytváření dat toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-125">For example, an Azure Storage linked service links a storage account toohello data factory.</span></span> <span data-ttu-id="6f0dc-126">Datové sadě služby Azure Blob představuje kontejner objektů blob hello a hello složku, která obsahuje toobe vstupní objekty BLOB hello zpracovat.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-126">An Azure Blob dataset represents hello blob container and hello folder that contains hello input blobs toobe processed.</span></span> 

<span data-ttu-id="6f0dc-127">Tady je ukázkový scénář.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-127">Here is a sample scenario.</span></span> <span data-ttu-id="6f0dc-128">toocopy data z databáze SQL tooa úložiště objektů Blob, vytvoříte dvě propojené služby: úložiště Azure a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-128">toocopy data from Blob storage tooa SQL database, you create two linked services: Azure Storage and Azure SQL Database.</span></span> <span data-ttu-id="6f0dc-129">Pak vytvořte dvě datové sady: datovou sadu objektu Blob Azure, (který se týká toohello propojené služby Azure Storage) a tabulky SQL Azure datovou sadu (odkazuje toohello Azure SQL Database propojené služby).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-129">Then, create two datasets: Azure Blob dataset (which refers toohello Azure Storage linked service) and Azure SQL Table dataset (which refers toohello Azure SQL Database linked service).</span></span> <span data-ttu-id="6f0dc-130">Hello Azure Storage a Azure SQL Database propojené služby obsahovat připojovací řetězce, které objekt pro vytváření dat používá v modulu runtime tooconnect tooyour Azure Storage a Azure SQL Database, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-130">hello Azure Storage and Azure SQL Database linked services contain connection strings that Data Factory uses at runtime tooconnect tooyour Azure Storage and Azure SQL Database, respectively.</span></span> <span data-ttu-id="6f0dc-131">datovou sadu objektu Blob Azure Hello určuje hello kontejner objektů blob a objektů blob složku, která obsahuje hello vstupní objekty BLOB ve službě Blob storage.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-131">hello Azure Blob dataset specifies hello blob container and blob folder that contains hello input blobs in your Blob storage.</span></span> <span data-ttu-id="6f0dc-132">Datová sada tabulky SQL Azure Hello Určuje, že hello tabulky SQL ve vašich datech hello toowhich databáze SQL je toobe zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-132">hello Azure SQL Table dataset specifies hello SQL table in your SQL database toowhich hello data is toobe copied.</span></span>

<span data-ttu-id="6f0dc-133">Hello následující obrázek znázorňuje vztahy hello mezi kanálu, aktivity, datové sady a propojené služby objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-133">hello following diagram shows hello relationships among pipeline, activity, dataset, and linked service in Data Factory:</span></span> 

![Vztah mezi kanálu, aktivity, datové sady, propojených služeb](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a><span data-ttu-id="6f0dc-135">JSON datové sady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-135">Dataset JSON</span></span>
<span data-ttu-id="6f0dc-136">Datové sady ve službě Data Factory je definováno ve formátu JSON následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-136">A dataset in Data Factory is defined in JSON format as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="6f0dc-137">Hello následující tabulka popisuje vlastnosti v hello výše JSON:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-137">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="6f0dc-138">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6f0dc-138">Property</span></span> | <span data-ttu-id="6f0dc-139">Popis</span><span class="sxs-lookup"><span data-stu-id="6f0dc-139">Description</span></span> | <span data-ttu-id="6f0dc-140">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6f0dc-140">Required</span></span> | <span data-ttu-id="6f0dc-141">Výchozí</span><span class="sxs-lookup"><span data-stu-id="6f0dc-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6f0dc-142">jméno</span><span class="sxs-lookup"><span data-stu-id="6f0dc-142">name</span></span> |<span data-ttu-id="6f0dc-143">Název datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-143">Name of hello dataset.</span></span> <span data-ttu-id="6f0dc-144">V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-144">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="6f0dc-145">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-145">Yes</span></span> |<span data-ttu-id="6f0dc-146">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-146">NA</span></span> |
| <span data-ttu-id="6f0dc-147">type</span><span class="sxs-lookup"><span data-stu-id="6f0dc-147">type</span></span> |<span data-ttu-id="6f0dc-148">Typ hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-148">Type of hello dataset.</span></span> <span data-ttu-id="6f0dc-149">Zadejte jeden z typů hello podporovaných službou Data Factory (například: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-149">Specify one of hello types supported by Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <br/><br/><span data-ttu-id="6f0dc-150">Podrobnosti najdete v tématu [typ sady](#Type).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-150">For details, see [Dataset type](#Type).</span></span> |<span data-ttu-id="6f0dc-151">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-151">Yes</span></span> |<span data-ttu-id="6f0dc-152">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-152">NA</span></span> |
| <span data-ttu-id="6f0dc-153">Struktura</span><span class="sxs-lookup"><span data-stu-id="6f0dc-153">structure</span></span> |<span data-ttu-id="6f0dc-154">Schéma hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-154">Schema of hello dataset.</span></span><br/><br/><span data-ttu-id="6f0dc-155">Podrobnosti najdete v tématu [strukturu datové sady](#Structure).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-155">For details, see [Dataset structure](#Structure).</span></span> |<span data-ttu-id="6f0dc-156">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-156">No</span></span> |<span data-ttu-id="6f0dc-157">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-157">NA</span></span> |
| <span data-ttu-id="6f0dc-158">typeProperties</span><span class="sxs-lookup"><span data-stu-id="6f0dc-158">typeProperties</span></span> | <span data-ttu-id="6f0dc-159">vlastnosti typu Hello se liší pro jednotlivé typy (například: Azure Blob, tabulka Azure SQL).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-159">hello type properties are different for each type (for example: Azure Blob, Azure SQL table).</span></span> <span data-ttu-id="6f0dc-160">Podrobnosti o svých vlastnostech a hello podporované typy najdete v tématu [typ sady](#Type).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-160">For details on hello supported types and their properties, see [Dataset type](#Type).</span></span> |<span data-ttu-id="6f0dc-161">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-161">Yes</span></span> |<span data-ttu-id="6f0dc-162">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-162">NA</span></span> |
| <span data-ttu-id="6f0dc-163">external</span><span class="sxs-lookup"><span data-stu-id="6f0dc-163">external</span></span> | <span data-ttu-id="6f0dc-164">Logická hodnota příznak toospecify, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-164">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> <span data-ttu-id="6f0dc-165">Pokud není hello vstupní datové sady pro aktivitu vytvořil hello aktuální kanálu, nastavte tento příznak tootrue.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-165">If hello input dataset for an activity is not produced by hello current pipeline, set this flag tootrue.</span></span> <span data-ttu-id="6f0dc-166">Nastavte tento příznak tootrue pro hello vstupní datové sady hello první aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-166">Set this flag tootrue for hello input dataset of hello first activity in hello pipeline.</span></span>  |<span data-ttu-id="6f0dc-167">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-167">No</span></span> |<span data-ttu-id="6f0dc-168">False</span><span class="sxs-lookup"><span data-stu-id="6f0dc-168">false</span></span> |
| <span data-ttu-id="6f0dc-169">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="6f0dc-169">availability</span></span> | <span data-ttu-id="6f0dc-170">Definuje hello okno zpracování (například hodinové nebo denní) nebo hello řezů model pro produkční hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-170">Defines hello processing window (for example, hourly or daily) or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="6f0dc-171">Jednotlivé jednotky data využívat a vyprodukované spuštění aktivity se nazývá datový řez.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-171">Each unit of data consumed and produced by an activity run is called a data slice.</span></span> <span data-ttu-id="6f0dc-172">Pokud se hello dostupnost výstupní datové sady toodaily (frekvenci - den, interval - 1), řez vytváří denně.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-172">If hello availability of an output dataset is set toodaily (frequency - Day, interval - 1), a slice is produced daily.</span></span> <br/><br/><span data-ttu-id="6f0dc-173">Podrobnosti najdete v tématu [datovou sadu dostupnosti](#Availability).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-173">For details, see [Dataset availability](#Availability).</span></span> <br/><br/><span data-ttu-id="6f0dc-174">Podrobnosti o datovou sadu hello řezů modelu, najdete v části hello [plánování a provádění](data-factory-scheduling-and-execution.md) článku.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-174">For details on hello dataset slicing model, see hello [Scheduling and execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="6f0dc-175">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-175">Yes</span></span> |<span data-ttu-id="6f0dc-176">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-176">NA</span></span> |
| <span data-ttu-id="6f0dc-177">policy</span><span class="sxs-lookup"><span data-stu-id="6f0dc-177">policy</span></span> |<span data-ttu-id="6f0dc-178">Definuje kritéria hello nebo hello podmínku, která musíte splnit řezy hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-178">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="6f0dc-179">Podrobnosti najdete v tématu hello [datovou sadu zásad](#Policy) části.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-179">For details, see hello [Dataset policy](#Policy) section.</span></span> |<span data-ttu-id="6f0dc-180">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-180">No</span></span> |<span data-ttu-id="6f0dc-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-181">NA</span></span> |

## <a name="dataset-example"></a><span data-ttu-id="6f0dc-182">Příklad datové sady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-182">Dataset example</span></span>
<span data-ttu-id="6f0dc-183">V následující ukázka hello, datová sada hello představuje tabulku s názvem **MyTable** v databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-183">In hello following example, hello dataset represents a table named **MyTable** in a SQL database.</span></span>

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

<span data-ttu-id="6f0dc-184">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-184">Note hello following points:</span></span>

* <span data-ttu-id="6f0dc-185">**typ** nastavena tooAzureSqlTable.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-185">**type** is set tooAzureSqlTable.</span></span>
* <span data-ttu-id="6f0dc-186">**Název tabulky** vlastnost type (typ konkrétní tooAzureSqlTable) je nastavená tooMyTable.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-186">**tableName** type property (specific tooAzureSqlTable type) is set tooMyTable.</span></span>
* <span data-ttu-id="6f0dc-187">**linkedServiceName** odkazuje tooa propojené služby typu azuresqldatabase., která je definována v hello další fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-187">**linkedServiceName** refers tooa linked service of type AzureSqlDatabase, which is defined in hello next JSON snippet.</span></span> 
* <span data-ttu-id="6f0dc-188">**frekvence dostupnosti** nastavena tooDay, a **interval** nastavena too1.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-188">**availability frequency** is set tooDay, and **interval** is set too1.</span></span> <span data-ttu-id="6f0dc-189">To znamená, že hello datovou sadu se vytvářejí denně.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-189">This means that hello dataset slice is produced daily.</span></span>  

<span data-ttu-id="6f0dc-190">**AzureSqlLinkedService** je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-190">**AzureSqlLinkedService** is defined as follows:</span></span>

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

<span data-ttu-id="6f0dc-191">V předchozím fragmentu kódu JSON hello:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-191">In hello preceding JSON snippet:</span></span>

* <span data-ttu-id="6f0dc-192">**typ** nastavena tooAzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-192">**type** is set tooAzureSqlDatabase.</span></span>
* <span data-ttu-id="6f0dc-193">**connectionString** vlastnost typu Určuje informace tooconnect tooa SQL database.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-193">**connectionString** type property specifies information tooconnect tooa SQL database.</span></span>  

<span data-ttu-id="6f0dc-194">Jak vidíte, hello propojené služby definuje jak tooconnect tooa SQL database.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-194">As you can see, hello linked service defines how tooconnect tooa SQL database.</span></span> <span data-ttu-id="6f0dc-195">Datová sada Hello definuje, jaký tabulka je použít jako vstup a výstup hello aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-195">hello dataset defines what table is used as an input and output for hello activity in a pipeline.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="6f0dc-196">Pokud datové sady je vytvářen hello kanálu, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-196">Unless a dataset is being produced by hello pipeline, it should be marked as **external**.</span></span> <span data-ttu-id="6f0dc-197">Toto nastavení obecně platí tooinputs první aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-197">This setting generally applies tooinputs of first activity in a pipeline.</span></span>   


## <span data-ttu-id="6f0dc-198"><a name="Type"></a>Typ sady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-198"><a name="Type"></a> Dataset type</span></span>
<span data-ttu-id="6f0dc-199">Typ Hello hello sady dat závisí na hello úložiště dat, které používáte.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-199">hello type of hello dataset depends on hello data store you use.</span></span> <span data-ttu-id="6f0dc-200">Viz následující tabulka obsahuje seznam podporovaných službou Data Factory úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-200">See hello following table for a list of data stores supported by Data Factory.</span></span> <span data-ttu-id="6f0dc-201">Klikněte na tlačítko data store toolearn jak toocreate propojené služby a datové sady pro tato data uložit.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-201">Click a data store toolearn how toocreate a linked service and a dataset for that data store.</span></span>

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> <span data-ttu-id="6f0dc-202">Úložiště dat, s * může být místní nebo v Azure infrastruktura jako služba (IaaS).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-202">Data stores with * can be on-premises or on Azure infrastructure as a service (IaaS).</span></span> <span data-ttu-id="6f0dc-203">Tyto úložiště dat vyžadují tooinstall [Brána pro správu dat](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-203">These data stores require you tooinstall [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="6f0dc-204">V příkladu hello v předchozí části hello hello typ DataSet hello je nastaven příliš**AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-204">In hello example in hello previous section, hello type of hello dataset is set too**AzureSqlTable**.</span></span> <span data-ttu-id="6f0dc-205">Podobně pro datové sadě služby Azure Blob hello hello sady dat je typ nastaven příliš**AzureBlob**, jak je znázorněno v následujícím JSON hello:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-205">Similarly, for an Azure Blob dataset, hello type of hello dataset is set too**AzureBlob**, as shown in hello following JSON:</span></span>

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

## <span data-ttu-id="6f0dc-206"><a name="Structure"></a>Struktura datové sady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-206"><a name="Structure"></a>Dataset structure</span></span>
<span data-ttu-id="6f0dc-207">Hello **struktura** část je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-207">hello **structure** section is optional.</span></span> <span data-ttu-id="6f0dc-208">Definuje schéma hello datovou sadu hello podle obsahující kolekci názvů a typy dat sloupců.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-208">It defines hello schema of hello dataset by containing a collection of names and data types of columns.</span></span> <span data-ttu-id="6f0dc-209">Použijete hello struktura části tooprovide typ informace, které jsou používané tooconvert typy a mapování sloupců z hello zdroj toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-209">You use hello structure section tooprovide type information that is used tooconvert types and map columns from hello source toohello destination.</span></span> <span data-ttu-id="6f0dc-210">V následující ukázka hello, hello datová sada má tři sloupce: `slicetimestamp`, `projectname`, a `pageviews`.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-210">In hello following example, hello dataset has three columns: `slicetimestamp`, `projectname`, and `pageviews`.</span></span> <span data-ttu-id="6f0dc-211">Jsou typu řetězec, řetězec a Decimal, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-211">They are of type String, String, and Decimal, respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="6f0dc-212">Každý sloupec struktury hello obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-212">Each column in hello structure contains hello following properties:</span></span>

| <span data-ttu-id="6f0dc-213">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6f0dc-213">Property</span></span> | <span data-ttu-id="6f0dc-214">Popis</span><span class="sxs-lookup"><span data-stu-id="6f0dc-214">Description</span></span> | <span data-ttu-id="6f0dc-215">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6f0dc-215">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6f0dc-216">jméno</span><span class="sxs-lookup"><span data-stu-id="6f0dc-216">name</span></span> |<span data-ttu-id="6f0dc-217">Název sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-217">Name of hello column.</span></span> |<span data-ttu-id="6f0dc-218">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-218">Yes</span></span> |
| <span data-ttu-id="6f0dc-219">type</span><span class="sxs-lookup"><span data-stu-id="6f0dc-219">type</span></span> |<span data-ttu-id="6f0dc-220">Datový typ sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-220">Data type of hello column.</span></span>  |<span data-ttu-id="6f0dc-221">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-221">No</span></span> |
| <span data-ttu-id="6f0dc-222">Jazyková verze</span><span class="sxs-lookup"><span data-stu-id="6f0dc-222">culture</span></span> |<span data-ttu-id="6f0dc-223">. Jazykovou verzi na základě NET toobe používá při hello typ je typ formátu .NET: `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-223">.NET-based culture toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="6f0dc-224">Výchozí hodnota Hello je `en-us`.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-224">hello default is `en-us`.</span></span> |<span data-ttu-id="6f0dc-225">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-225">No</span></span> |
| <span data-ttu-id="6f0dc-226">Formát</span><span class="sxs-lookup"><span data-stu-id="6f0dc-226">format</span></span> |<span data-ttu-id="6f0dc-227">Formátování toobe řetězec se používá při hello typ je typ formátu .NET: `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-227">Format string toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="6f0dc-228">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-228">No</span></span> |

<span data-ttu-id="6f0dc-229">Hello následující pokyny vám pomohou určit, kdy tooinclude struktury informace a jaké tooinclude v hello **struktura** části.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-229">hello following guidelines help you determine when tooinclude structure information, and what tooinclude in hello **structure** section.</span></span>

* <span data-ttu-id="6f0dc-230">**Pro strukturovaná data zdroje**, zadejte část struktura hello pouze v případě, že chcete, aby mapování zdrojových sloupců toosink sloupců a jejich názvy nejsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-230">**For structured data sources**, specify hello structure section only if you want map source columns toosink columns, and their names are not hello same.</span></span> <span data-ttu-id="6f0dc-231">Tento druh zdroj strukturovaných dat ukládá informace schématu a typu dat společně s vlastními daty hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-231">This kind of structured data source stores data schema and type information along with hello data itself.</span></span> <span data-ttu-id="6f0dc-232">Příklady strukturovaných dat zdroje: SQL Server, Oracle a tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-232">Examples of structured data sources include SQL Server, Oracle, and Azure table.</span></span> 
  
    <span data-ttu-id="6f0dc-233">Protože je již k dispozici pro strukturovaná data zdroje informací o typu, by neměla zahrnovat informace o typu, obsahují části struktura hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-233">As type information is already available for structured data sources, you should not include type information when you do include hello structure section.</span></span>
* <span data-ttu-id="6f0dc-234">**Pro schéma pro zdroje dat pro čtení (konkrétně úložiště objektů Blob)**, můžete toostore data bez ukládání žádné schéma nebo typ informace s daty hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-234">**For schema on read data sources (specifically Blob storage)**, you can choose toostore data without storing any schema or type information with hello data.</span></span> <span data-ttu-id="6f0dc-235">Pro tyto typy zdrojů dat zahrnují struktura, když chcete, aby toomap zdrojové sloupce toosink sloupce.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-235">For these types of data sources, include structure when you want toomap source columns toosink columns.</span></span> <span data-ttu-id="6f0dc-236">Také zahrnovat struktura hello datová sada vstupem pro aktivitu kopírování a datové typy datovou sadu zdroj by měl být převedená toonative typy pro hello sink.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-236">Also include structure when hello dataset is an input for a copy activity, and data types of source dataset should be converted toonative types for hello sink.</span></span> 
    
    <span data-ttu-id="6f0dc-237">Objekt pro vytváření dat podporuje následující hodnoty pro poskytnutí informací o typu ve struktuře hello: **Int16, Int32, Int64, jedním, Double, Decimal, Byte [], logická hodnota, řetězec, Guid, Datetime, Datetimeoffset a časový interval**.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-237">Data Factory supports hello following values for providing type information in structure: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan**.</span></span> <span data-ttu-id="6f0dc-238">Tyto hodnoty jsou specifikace CLS (Common Language)-vyhovující,. Na základě NET typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-238">These values are Common Language Specification (CLS)-compliant, .NET-based type values.</span></span>

<span data-ttu-id="6f0dc-239">Objekt pro vytváření dat převody typů automaticky provede při přesunu, že data ze zdrojových dat úložiště tooa podřízený data.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-239">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span> 
  

## <a name="dataset-availability"></a><span data-ttu-id="6f0dc-240">Datovou sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="6f0dc-240">Dataset availability</span></span>
<span data-ttu-id="6f0dc-241">Hello **dostupnosti** oddíl v datové sadě definuje hello okna pro zpracování (například hodinový, denní, nebo každý týden) pro datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-241">hello **availability** section in a dataset defines hello processing window (for example, hourly, daily, or weekly) for hello dataset.</span></span> <span data-ttu-id="6f0dc-242">Další informace o aktivity windows najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-242">For more information about activity windows, see [Scheduling and execution](data-factory-scheduling-and-execution.md).</span></span>

<span data-ttu-id="6f0dc-243">Následující části dostupnosti Hello Určuje, že hello výstupní datovou sadu se buď vytvářejí hodinu nebo vstupní datové sady hello každou hodinu je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-243">hello following availability section specifies that hello output dataset is either produced hourly, or hello input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="6f0dc-244">Když hello kanálu hello následující počáteční a koncový čas:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-244">If hello pipeline has hello following start and end times:</span></span>  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

<span data-ttu-id="6f0dc-245">Hello výstupní datovou sadu se vytvářejí každou hodinu v rámci kanálu hello spuštění a ukončení.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-245">hello output dataset is produced hourly within hello pipeline start and end times.</span></span> <span data-ttu-id="6f0dc-246">Proto jsou pět řezy datové sady vyprodukované tímto kanálem, jeden pro každou aktivitu okno (12: 00 - 1 AM, 1: 00 - 2 AM, 2: 00 - 3 AM, 3: 00 - 4 AM, 4: 00 - 5: 00).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-246">Therefore, there are five dataset slices produced by this pipeline, one for each activity window (12 AM - 1 AM, 1 AM - 2 AM, 2 AM - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM).</span></span> 

<span data-ttu-id="6f0dc-247">Hello následující tabulka popisuje vlastnosti, které můžete použít v části hello dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-247">hello following table describes properties you can use in hello availability section:</span></span>

| <span data-ttu-id="6f0dc-248">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="6f0dc-248">Property</span></span> | <span data-ttu-id="6f0dc-249">Popis</span><span class="sxs-lookup"><span data-stu-id="6f0dc-249">Description</span></span> | <span data-ttu-id="6f0dc-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6f0dc-250">Required</span></span> | <span data-ttu-id="6f0dc-251">Výchozí</span><span class="sxs-lookup"><span data-stu-id="6f0dc-251">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6f0dc-252">frequency</span><span class="sxs-lookup"><span data-stu-id="6f0dc-252">frequency</span></span> |<span data-ttu-id="6f0dc-253">Určuje časovou jednotku hello k produkci řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-253">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="6f0dc-254"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="6f0dc-254"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="6f0dc-255">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-255">Yes</span></span> |<span data-ttu-id="6f0dc-256">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-256">NA</span></span> |
| <span data-ttu-id="6f0dc-257">interval</span><span class="sxs-lookup"><span data-stu-id="6f0dc-257">interval</span></span> |<span data-ttu-id="6f0dc-258">Určuje multiplikátor pro četnost.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-258">Specifies a multiplier for frequency.</span></span><br/><br/><span data-ttu-id="6f0dc-259">"Frekvence x interval" Určuje, jak často hello se vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-259">"Frequency x interval" determines how often hello slice is produced.</span></span> <span data-ttu-id="6f0dc-260">Například pokud potřebují hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-260">For example, if you need hello dataset toobe sliced on an hourly basis, you set <b>frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="6f0dc-261">Všimněte si, že pokud zadáte **frekvence** jako **minutu**, byste měli nastavit hello interval toono méně než 15.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-261">Note that if you specify **frequency** as **Minute**, you should set hello interval toono less than 15.</span></span> |<span data-ttu-id="6f0dc-262">Ano</span><span class="sxs-lookup"><span data-stu-id="6f0dc-262">Yes</span></span> |<span data-ttu-id="6f0dc-263">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-263">NA</span></span> |
| <span data-ttu-id="6f0dc-264">Styl</span><span class="sxs-lookup"><span data-stu-id="6f0dc-264">style</span></span> |<span data-ttu-id="6f0dc-265">Určuje, zda by měl být na hello počáteční nebo koncový intervalu hello předložen hello řez.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-265">Specifies whether hello slice should be produced at hello start or end of hello interval.</span></span><ul><li><span data-ttu-id="6f0dc-266">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="6f0dc-266">StartOfInterval</span></span></li><li><span data-ttu-id="6f0dc-267">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="6f0dc-267">EndOfInterval</span></span></li></ul><span data-ttu-id="6f0dc-268">Pokud **frekvence** je nastaven příliš**měsíc**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí na hello poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-268">If **frequency** is set too**Month**, and **style** is set too**EndOfInterval**, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="6f0dc-269">Pokud **styl** je nastaven příliš**StartOfInterval**, hello se vytvářejí na hello první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-269">If **style** is set too**StartOfInterval**, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="6f0dc-270">Pokud **frekvence** je nastaven příliš**den**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí v hello poslední hodiny dne hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-270">If **frequency** is set too**Day**, and **style** is set too**EndOfInterval**, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="6f0dc-271">Pokud **frekvence** je nastaven příliš**hodinu**, a **styl** je nastaven příliš**EndOfInterval**, hello se vytvářejí na konci hello hello hodina.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-271">If **frequency** is set too**Hour**, and **style** is set too**EndOfInterval**, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="6f0dc-272">Například pro řez pro hello období 13: 00 – 14: 00, hello se vytvářejí na 14: 00.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-272">For example, for a slice for hello 1 PM - 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="6f0dc-273">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-273">No</span></span> |<span data-ttu-id="6f0dc-274">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="6f0dc-274">EndOfInterval</span></span> |
| <span data-ttu-id="6f0dc-275">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="6f0dc-275">anchorDateTime</span></span> |<span data-ttu-id="6f0dc-276">Definuje hello absolutní pozici v čase, které používá hello Plánovač toocompute datovou sadu řez hranice.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-276">Defines hello absolute position in time used by hello scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="6f0dc-277">Všimněte si, že pokud má tento propoerty částí data, která jsou podrobnější než hello zadané frekvence hello podrobnější částí se ignorují.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-277">Note that if this propoerty has date parts that are more granular than hello specified frequency, hello more granular parts are ignored.</span></span> <span data-ttu-id="6f0dc-278">Například, pokud hello **interval** je **každou hodinu** (frekvence: hodin a interval: 1) a hello **anchorDateTime** obsahuje **minuty a sekundy**, pak hello minut a sekund **anchorDateTime** jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-278">For example, if hello **interval** is **hourly** (frequency: hour and interval: 1), and hello **anchorDateTime** contains **minutes and seconds**, then hello minutes and seconds parts of **anchorDateTime** are ignored.</span></span> |<span data-ttu-id="6f0dc-279">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-279">No</span></span> |<span data-ttu-id="6f0dc-280">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="6f0dc-280">01/01/0001</span></span> |
| <span data-ttu-id="6f0dc-281">Posun</span><span class="sxs-lookup"><span data-stu-id="6f0dc-281">offset</span></span> |<span data-ttu-id="6f0dc-282">Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-282">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="6f0dc-283">Všimněte si, že pokud obě **anchorDateTime** a **posun** jsou nastaveny, výsledkem hello je hello kombinaci shift.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-283">Note that if both **anchorDateTime** and **offset** are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="6f0dc-284">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-284">No</span></span> |<span data-ttu-id="6f0dc-285">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-285">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="6f0dc-286">Příklad posunutí</span><span class="sxs-lookup"><span data-stu-id="6f0dc-286">offset example</span></span>
<span data-ttu-id="6f0dc-287">Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy začnou ve 12: 00 (půlnoc) koordinovaný světový čas (UTC).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-287">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM (midnight) Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="6f0dc-288">Pokud chcete místo toho hello počáteční čas čas UTC toobe 6: 00, nastavte hello posun, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-288">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="6f0dc-289">Příklad anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="6f0dc-289">anchorDateTime example</span></span>
<span data-ttu-id="6f0dc-290">V následujícím příkladu hello hello sada vytváří jednou za 23 hodin.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-290">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="6f0dc-291">Hello první řez spustí v době hello určeného **anchorDateTime**, který je nastaven příliš`2017-04-19T08:00:00` (UTC).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-291">hello first slice starts at hello time specified by **anchorDateTime**, which is set too`2017-04-19T08:00:00` (UTC).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="6f0dc-292">Posun nebo styl příklad</span><span class="sxs-lookup"><span data-stu-id="6f0dc-292">offset/style example</span></span>
<span data-ttu-id="6f0dc-293">Hello následující datová sada každý měsíc a vytváří hello 3rd v každém měsíci v 8:00 AM (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="6f0dc-293">hello following dataset is monthly, and is produced on hello 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <span data-ttu-id="6f0dc-294"><a name="Policy"></a>Datovou sadu zásad</span><span class="sxs-lookup"><span data-stu-id="6f0dc-294"><a name="Policy"></a>Dataset policy</span></span>
<span data-ttu-id="6f0dc-295">Hello **zásad** oddíl v definici datové sady hello definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-295">hello **policy** section in hello dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

### <a name="validation-policies"></a><span data-ttu-id="6f0dc-296">Zásady ověřování</span><span class="sxs-lookup"><span data-stu-id="6f0dc-296">Validation policies</span></span>
| <span data-ttu-id="6f0dc-297">Název zásady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-297">Policy name</span></span> | <span data-ttu-id="6f0dc-298">Popis</span><span class="sxs-lookup"><span data-stu-id="6f0dc-298">Description</span></span> | <span data-ttu-id="6f0dc-299">Použít příliš</span><span class="sxs-lookup"><span data-stu-id="6f0dc-299">Applied too</span></span>| <span data-ttu-id="6f0dc-300">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6f0dc-300">Required</span></span> | <span data-ttu-id="6f0dc-301">Výchozí</span><span class="sxs-lookup"><span data-stu-id="6f0dc-301">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="6f0dc-302">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="6f0dc-302">minimumSizeMB</span></span> |<span data-ttu-id="6f0dc-303">Ověří, že hello data v **úložiště objektů Azure Blob** hello splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-303">Validates that hello data in **Azure Blob storage** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="6f0dc-304">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="6f0dc-304">Azure Blob storage</span></span> |<span data-ttu-id="6f0dc-305">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-305">No</span></span> |<span data-ttu-id="6f0dc-306">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-306">NA</span></span> |
| <span data-ttu-id="6f0dc-307">minimumRows</span><span class="sxs-lookup"><span data-stu-id="6f0dc-307">minimumRows</span></span> |<span data-ttu-id="6f0dc-308">Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-308">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="6f0dc-309">Databáze SQL Azure</span><span class="sxs-lookup"><span data-stu-id="6f0dc-309">Azure SQL database</span></span></li><li><span data-ttu-id="6f0dc-310">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="6f0dc-310">Azure table</span></span></li></ul> |<span data-ttu-id="6f0dc-311">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-311">No</span></span> |<span data-ttu-id="6f0dc-312">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="6f0dc-312">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="6f0dc-313">Příklady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-313">Examples</span></span>
<span data-ttu-id="6f0dc-314">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="6f0dc-314">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="6f0dc-315">**minimumRows:**</span><span class="sxs-lookup"><span data-stu-id="6f0dc-315">**minimumRows:**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a><span data-ttu-id="6f0dc-316">Externích datových sad</span><span class="sxs-lookup"><span data-stu-id="6f0dc-316">External datasets</span></span>
<span data-ttu-id="6f0dc-317">Externích datových sad jsou ty, které nejsou od spuštěné kanál v objektu pro vytváření dat hello hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-317">External datasets are hello ones that are not produced by a running pipeline in hello data factory.</span></span> <span data-ttu-id="6f0dc-318">Pokud hello datové sady je označena jako **externí**, hello **ExternalData** zásad může být definovaná tooinfluence hello chování hello datovou sadu řez dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-318">If hello dataset is marked as **external**, hello **ExternalData** policy may be defined tooinfluence hello behavior of hello dataset slice availability.</span></span>

<span data-ttu-id="6f0dc-319">Pokud datové sady je vytvářen službou Data Factory, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-319">Unless a dataset is being produced by Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="6f0dc-320">Toto nastavení obecně platí toohello vstupy první aktivitu v kanálu, pokud se používá aktivitu nebo řetězení kanálu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-320">This setting generally applies toohello inputs of first activity in a pipeline, unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="6f0dc-321">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6f0dc-321">Name</span></span> | <span data-ttu-id="6f0dc-322">Popis</span><span class="sxs-lookup"><span data-stu-id="6f0dc-322">Description</span></span> | <span data-ttu-id="6f0dc-323">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6f0dc-323">Required</span></span> | <span data-ttu-id="6f0dc-324">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="6f0dc-324">Default value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6f0dc-325">dataDelay</span><span class="sxs-lookup"><span data-stu-id="6f0dc-325">dataDelay</span></span> |<span data-ttu-id="6f0dc-326">čas Hello toodelay hello zkontrolovat dostupnost hello hello externích dat pro hello daného řez.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-326">hello time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="6f0dc-327">Můžete například zpoždění hodinové kontroly pomocí tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-327">For example, you can delay an hourly check by using this setting.</span></span><br/><br/><span data-ttu-id="6f0dc-328">Hello nastavení pouze platí toohello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-328">hello setting only applies toohello present time.</span></span>  <span data-ttu-id="6f0dc-329">Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření hello se spustí: 10: 00.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-329">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="6f0dc-330">Všimněte si, že toto nastavení neovlivňuje řezy v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-330">Note that this setting does not affect slices in hello past.</span></span> <span data-ttu-id="6f0dc-331">Řezy s **řez koncový čas** + **dataDelay** < **nyní** jsou zpracovávány bez jakéhokoli zpoždění.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-331">Slices with **Slice End Time** + **dataDelay** < **Now** are processed without any delay.</span></span><br/><br/><span data-ttu-id="6f0dc-332">Časy větší než 23:59 hodin zadat pomocí hello `day.hours:minutes:seconds` formátu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-332">Times greater than 23:59 hours should be specified by using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="6f0dc-333">Například toospecify 24 hodin, nepoužívejte 24:00:00.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-333">For example, toospecify 24 hours, don't use 24:00:00.</span></span> <span data-ttu-id="6f0dc-334">Místo toho použijte 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-334">Instead, use 1.00:00:00.</span></span> <span data-ttu-id="6f0dc-335">Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-335">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="6f0dc-336">1 den a 4 hodiny zadejte 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-336">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="6f0dc-337">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-337">No</span></span> |<span data-ttu-id="6f0dc-338">0</span><span class="sxs-lookup"><span data-stu-id="6f0dc-338">0</span></span> |
| <span data-ttu-id="6f0dc-339">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="6f0dc-339">retryInterval</span></span> |<span data-ttu-id="6f0dc-340">Doba čekání Hello mezi selhání i hello další pokusy.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-340">hello wait time between a failure and hello next attempt.</span></span> <span data-ttu-id="6f0dc-341">Toto nastavení platí toopresent čas.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-341">This setting applies toopresent time.</span></span> <span data-ttu-id="6f0dc-342">Pokud se nezdařila předchozí zkuste hello, zkuste další hello po hello **retryInterval** období.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-342">If hello previous try failed, hello next try is after hello **retryInterval** period.</span></span> <br/><br/><span data-ttu-id="6f0dc-343">Pokud je 1:00 PM nyní, můžeme začít hello prvního pokusu.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-343">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="6f0dc-344">Pokud hello trvání toocomplete hello první ověření kontrola je 1 minuta a hello operace se nezdařila, hello další pokus proběhne v 1:00 + 1 min (doba trvání) + 1min (interval opakování) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-344">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1min (duration) + 1min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="6f0dc-345">Řezy v posledních hello neexistuje žádné zpoždění není.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-345">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="6f0dc-346">Hello opakování dojde okamžitě.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-346">hello retry happens immediately.</span></span> |<span data-ttu-id="6f0dc-347">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-347">No</span></span> |<span data-ttu-id="6f0dc-348">00:01:00 (1 min)</span><span class="sxs-lookup"><span data-stu-id="6f0dc-348">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="6f0dc-349">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="6f0dc-349">retryTimeout</span></span> |<span data-ttu-id="6f0dc-350">Hello časový limit pro jednotlivé pokusy o opakování.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-350">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="6f0dc-351">Pokud je tato vlastnost nastavená too10 minut hello ověření musí být dokončeny v rámci 10 minut.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-351">If this property is set too10 minutes, hello validation should be completed within 10 minutes.</span></span> <span data-ttu-id="6f0dc-352">Pokud trvá déle než 10 minut tooperform hello ověření, opakujte hello časový limit.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-352">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="6f0dc-353">Pokud všechny pokusy o hello ověření časového limitu hello řez je označen jako **TimedOut**.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-353">If all attempts for hello validation time out, hello slice is marked as **TimedOut**.</span></span> |<span data-ttu-id="6f0dc-354">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-354">No</span></span> |<span data-ttu-id="6f0dc-355">00:10:00 (10 minut)</span><span class="sxs-lookup"><span data-stu-id="6f0dc-355">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="6f0dc-356">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="6f0dc-356">maximumRetry</span></span> |<span data-ttu-id="6f0dc-357">Hello kolikrát toocheck hello dostupnost externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-357">hello number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="6f0dc-358">Hello maximální povolená hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-358">hello maximum allowed value is 10.</span></span> |<span data-ttu-id="6f0dc-359">Ne</span><span class="sxs-lookup"><span data-stu-id="6f0dc-359">No</span></span> |<span data-ttu-id="6f0dc-360">3</span><span class="sxs-lookup"><span data-stu-id="6f0dc-360">3</span></span> |


## <a name="create-datasets"></a><span data-ttu-id="6f0dc-361">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="6f0dc-361">Create datasets</span></span>
<span data-ttu-id="6f0dc-362">Datové sady můžete vytvořit pomocí jedné z těchto nástrojů nebo sady SDK:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-362">You can create datasets by using one of these tools or SDKs:</span></span> 

- <span data-ttu-id="6f0dc-363">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="6f0dc-363">Copy Wizard</span></span> 
- <span data-ttu-id="6f0dc-364">portál Azure</span><span class="sxs-lookup"><span data-stu-id="6f0dc-364">Azure portal</span></span>
- <span data-ttu-id="6f0dc-365">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f0dc-365">Visual Studio</span></span>
- <span data-ttu-id="6f0dc-366">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f0dc-366">PowerShell</span></span>
- <span data-ttu-id="6f0dc-367">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="6f0dc-367">Azure Resource Manager template</span></span>
- <span data-ttu-id="6f0dc-368">REST API</span><span class="sxs-lookup"><span data-stu-id="6f0dc-368">REST API</span></span>
- <span data-ttu-id="6f0dc-369">.NET API</span><span class="sxs-lookup"><span data-stu-id="6f0dc-369">.NET API</span></span>

<span data-ttu-id="6f0dc-370">V tématu hello následující kurzy pro podrobné pokyny pro vytváření kanálů a datové sady pomocí jedné z těchto nástrojů nebo sady SDK:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-370">See hello following tutorials for step-by-step instructions for creating pipelines and datasets by using one of these tools or SDKs:</span></span>
 
- [<span data-ttu-id="6f0dc-371">Vytvoření kanálu s aktivitou transformace dat</span><span class="sxs-lookup"><span data-stu-id="6f0dc-371">Build a pipeline with a data transformation activity</span></span>](data-factory-build-your-first-pipeline.md)
- [<span data-ttu-id="6f0dc-372">Vytvoření kanálu s aktivitou přesun dat</span><span class="sxs-lookup"><span data-stu-id="6f0dc-372">Build a pipeline with a data movement activity</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

<span data-ttu-id="6f0dc-373">Po kanálu se vytvoří a nasadí, můžete spravovat a monitorovat hello kanály pomocí oken webu Azure portal nebo hello monitorování a správu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-373">After a pipeline is created and deployed, you can manage and monitor your pipelines by using hello Azure portal blades, or hello Monitoring and Management app.</span></span> <span data-ttu-id="6f0dc-374">V tématu hello následující témata podrobné pokyny:</span><span class="sxs-lookup"><span data-stu-id="6f0dc-374">See hello following topics for step-by-step instructions:</span></span> 

- [<span data-ttu-id="6f0dc-375">Monitorování a Správa kanálů pomocí oken webu Azure portal</span><span class="sxs-lookup"><span data-stu-id="6f0dc-375">Monitor and manage pipelines by using Azure portal blades</span></span>](data-factory-monitor-manage-pipelines.md)
- [<span data-ttu-id="6f0dc-376">Monitorování a Správa kanálů pomocí monitorování a správu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6f0dc-376">Monitor and manage pipelines by using hello Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a><span data-ttu-id="6f0dc-377">Oboru datové sady</span><span class="sxs-lookup"><span data-stu-id="6f0dc-377">Scoped datasets</span></span>
<span data-ttu-id="6f0dc-378">Můžete vytvořit datové sady, které jsou vymezená tooa kanálu pomocí hello **datové sady** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-378">You can create datasets that are scoped tooa pipeline by using hello **datasets** property.</span></span> <span data-ttu-id="6f0dc-379">Tyto datové sady lze použít pouze aktivity v rámci tohoto kanálu, nikoli aktivity v jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-379">These datasets can only be used by activities within this pipeline, not by activities in other pipelines.</span></span> <span data-ttu-id="6f0dc-380">Následující ukázka Hello definuje kanál s dvě datové sady (InputDataset rdc a OutputDataset rdc) toobe používaných v rámci kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-380">hello following example defines a pipeline with two datasets (InputDataset-rdc and OutputDataset-rdc) toobe used within hello pipeline.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="6f0dc-381">Oboru datové sady jsou podporovány pouze s jednorázové kanály (kde **pipelineMode** je nastaven příliš**OneTime**).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-381">Scoped datasets are supported only with one-time pipelines (where **pipelineMode** is set too**OneTime**).</span></span> <span data-ttu-id="6f0dc-382">V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6f0dc-382">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="6f0dc-383">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f0dc-383">Next steps</span></span>
- <span data-ttu-id="6f0dc-384">Další informace o kanály najdete v tématu [vytvořit kanály](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-384">For more information about pipelines, see [Create pipelines](data-factory-create-pipelines.md).</span></span> 
- <span data-ttu-id="6f0dc-385">Další informace o tom, jak jsou naplánované kanálů a jsou prováděny najdete v tématu [plánování a provádění v Azure Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="6f0dc-385">For more information about how pipelines are scheduled and executed, see [Scheduling and execution in Azure Data Factory](data-factory-scheduling-and-execution.md).</span></span> 
