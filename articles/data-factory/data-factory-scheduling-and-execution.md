---
title: "Plánování a provádění pomocí služby Data Factory | Microsoft Docs"
description: "Další aspekty plánování a provádění aplikačního modelu služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 088a83df-4d1b-4ac1-afb3-0787a9bd1ca5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: e6fd92cde91ae5f171c855c07fa8974a19703b41
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="data-factory-scheduling-and-execution"></a><span data-ttu-id="838f4-103">Data Factory plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="838f4-103">Data Factory scheduling and execution</span></span>
<span data-ttu-id="838f4-104">Tento článek vysvětluje aspekty plánování a spouštění aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="838f4-104">This article explains the scheduling and execution aspects of the Azure Data Factory application model.</span></span> <span data-ttu-id="838f4-105">Tento článek předpokládá, že chápete základní informace o objektu pro vytváření dat aplikací modelu koncepty, včetně aktivit, kanálů, propojené služby a datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-105">This article assumes that you understand basics of Data Factory application model concepts, including activity, pipelines, linked services, and datasets.</span></span> <span data-ttu-id="838f4-106">Základní koncepty objektu pro vytváření dat Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="838f4-106">For basic concepts of Azure Data Factory, see the following articles:</span></span>

* [<span data-ttu-id="838f4-107">Úvod do služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="838f4-107">Introduction to Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="838f4-108">Kanály</span><span class="sxs-lookup"><span data-stu-id="838f4-108">Pipelines</span></span>](data-factory-create-pipelines.md)
* [<span data-ttu-id="838f4-109">Datové sady</span><span class="sxs-lookup"><span data-stu-id="838f4-109">Datasets</span></span>](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a><span data-ttu-id="838f4-110">Počáteční a koncový čas kanálu</span><span class="sxs-lookup"><span data-stu-id="838f4-110">Start and end times of pipeline</span></span>
<span data-ttu-id="838f4-111">Kanál je aktivní jenom mezi jeho **spustit** čas a **end** čas.</span><span class="sxs-lookup"><span data-stu-id="838f4-111">A pipeline is active only between its **start** time and **end** time.</span></span> <span data-ttu-id="838f4-112">Nebude provedena před časem zahájení nebo po koncovém čase.</span><span class="sxs-lookup"><span data-stu-id="838f4-112">It is not executed before the start time or after the end time.</span></span> <span data-ttu-id="838f4-113">Pokud kanálu pozastaví, nebude provedena bez ohledu na jeho počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="838f4-113">If the pipeline is paused, it is not executed irrespective of its start and end time.</span></span> <span data-ttu-id="838f4-114">Pro kanál ke spuštění by neměl být pozastavena.</span><span class="sxs-lookup"><span data-stu-id="838f4-114">For a pipeline to run, it should not be paused.</span></span> <span data-ttu-id="838f4-115">Zjistíte, tato nastavení (zahájení, ukončení, pozastavena) v definici kanál:</span><span class="sxs-lookup"><span data-stu-id="838f4-115">You find these settings (start, end, paused) in the pipeline definition:</span></span> 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

<span data-ttu-id="838f4-116">Další informace najdete v těchto vlastností [vytvořit kanály](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="838f4-116">For more information these properties, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> 


## <a name="specify-schedule-for-an-activity"></a><span data-ttu-id="838f4-117">Zadejte plán pro aktivitu</span><span class="sxs-lookup"><span data-stu-id="838f4-117">Specify schedule for an activity</span></span>
<span data-ttu-id="838f4-118">Není kanálu, který se spustí.</span><span class="sxs-lookup"><span data-stu-id="838f4-118">It is not the pipeline that is executed.</span></span> <span data-ttu-id="838f4-119">Je aktivity v kanálu, které se provádějí v kontextu kanálu.</span><span class="sxs-lookup"><span data-stu-id="838f4-119">It is the activities in the pipeline that are executed in the overall context of the pipeline.</span></span> <span data-ttu-id="838f4-120">Můžete zadat plán opakování pro aktivitu pomocí **Plánovač** část JSON aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-120">You can specify a recurring schedule for an activity by using the **scheduler** section of activity JSON.</span></span> <span data-ttu-id="838f4-121">Například můžete naplánovat aktivity ke spuštění každou hodinu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="838f4-121">For example, you can schedule an activity to run hourly as follows:</span></span>  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

<span data-ttu-id="838f4-122">Jak je znázorněno v následujícím diagramu, zadat že plán pro aktivitu vytvoří řadu přeskakujícího windows se v kanálu spuštění a ukončení.</span><span class="sxs-lookup"><span data-stu-id="838f4-122">As shown in the following diagram, specifying a schedule for an activity creates a series of tumbling windows with in the pipeline start and end times.</span></span> <span data-ttu-id="838f4-123">Přeskakující windows jsou řadu pevné velikosti nepřekrývají, souvislý časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="838f4-123">Tumbling windows are a series of fixed-size non-overlapping, contiguous time intervals.</span></span> <span data-ttu-id="838f4-124">Tyto logické přeskakující windows pro aktivitu se nazývají **aktivity windows**.</span><span class="sxs-lookup"><span data-stu-id="838f4-124">These logical tumbling windows for an activity are called **activity windows**.</span></span>

![Příklad aktivitu plánovače](media/data-factory-scheduling-and-execution/scheduler-example.png)

<span data-ttu-id="838f4-126">**Plánovač** vlastnost pro aktivitu je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="838f4-126">The **scheduler** property for an activity is optional.</span></span> <span data-ttu-id="838f4-127">Pokud určíte tuto vlastnost, musí se shodovat cadence, které zadáte v definici výstupní datovou sadu aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-127">If you do specify this property, it must match the cadence you specify in the definition of output dataset for the activity.</span></span> <span data-ttu-id="838f4-128">Výstupní datové sady v současné době řídí plán.</span><span class="sxs-lookup"><span data-stu-id="838f4-128">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="838f4-129">Proto je nutné vytvořit datovou sadu výstupů i v případě, že aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="838f4-129">Therefore, you must create an output dataset even if the activity does not produce any output.</span></span> 

## <a name="specify-schedule-for-a-dataset"></a><span data-ttu-id="838f4-130">Zadejte plán pro datové sady</span><span class="sxs-lookup"><span data-stu-id="838f4-130">Specify schedule for a dataset</span></span>
<span data-ttu-id="838f4-131">Aktivita v kanálu pro vytváření dat může trvat vstup nula nebo více **datové sady** a vytvoří výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-131">An activity in a Data Factory pipeline can take zero or more input **datasets** and produce one or more output datasets.</span></span> <span data-ttu-id="838f4-132">Pro aktivitu, můžete zadat cadence, kdy je k dispozici vstupních dat nebo výstupní data se vytvářejí pomocí **dostupnosti** část v definicích datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-132">For an activity, you can specify the cadence at which the input data is available or the output data is produced by using the **availability** section in the dataset definitions.</span></span> 

<span data-ttu-id="838f4-133">**Frekvence** v **dostupnosti** část Určuje časovou jednotku.</span><span class="sxs-lookup"><span data-stu-id="838f4-133">**Frequency** in the **availability** section specifies the time unit.</span></span> <span data-ttu-id="838f4-134">Frekvence povolené hodnoty jsou: minutu, hodinu, den, týden a měsíce.</span><span class="sxs-lookup"><span data-stu-id="838f4-134">The allowed values for frequency are: Minute, Hour, Day, Week, and Month.</span></span> <span data-ttu-id="838f4-135">**Interval** vlastnost v části Dostupnost určuje multiplikátor pro četnost.</span><span class="sxs-lookup"><span data-stu-id="838f4-135">The **interval** property in the availability section specifies a multiplier for frequency.</span></span> <span data-ttu-id="838f4-136">Například: Pokud frekvence je nastavená na den a interval je nastaven na hodnotu 1 pro datovou sadu výstupů, výstupní data se vytvoří každý den.</span><span class="sxs-lookup"><span data-stu-id="838f4-136">For example: if the frequency is set to Day and interval is set to 1 for an output dataset, the output data is produced daily.</span></span> <span data-ttu-id="838f4-137">Pokud zadáte četnost jako minutu, doporučujeme nastavit interval na menší než 15.</span><span class="sxs-lookup"><span data-stu-id="838f4-137">If you specify the frequency as minute, we recommend that you set the interval to no less than 15.</span></span> 

<span data-ttu-id="838f4-138">V následujícím příkladu vstupních dat je k dispozici každou hodinu a výstupní data se vytvářejí každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="838f4-138">In the following example, the input data is available hourly and the output data is produced hourly (`"frequency": "Hour", "interval": 1`).</span></span> 

<span data-ttu-id="838f4-139">**Vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="838f4-139">**Input dataset:**</span></span> 

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```


<span data-ttu-id="838f4-140">**Výstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="838f4-140">**Output dataset**</span></span>

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
            "format": {
                "type": "TextFormat"
            },
            "partitionedBy": [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" }}
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="838f4-141">V současné době **výstupní datovou sadu disky plán**.</span><span class="sxs-lookup"><span data-stu-id="838f4-141">Currently, **output dataset drives the schedule**.</span></span> <span data-ttu-id="838f4-142">Jinými slovy zadaný pro výstupní datovou sadu plán slouží ke spuštění aktivity za běhu.</span><span class="sxs-lookup"><span data-stu-id="838f4-142">In other words, the schedule specified for the output dataset is used to run an activity at runtime.</span></span> <span data-ttu-id="838f4-143">Proto je nutné vytvořit datovou sadu výstupů i v případě, že aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="838f4-143">Therefore, you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="838f4-144">Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte.</span><span class="sxs-lookup"><span data-stu-id="838f4-144">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> 

<span data-ttu-id="838f4-145">V následující definici kanál **Plánovač** vlastnost se používá k určení plán aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-145">In the following pipeline definition, the **scheduler** property is used to specify schedule for the activity.</span></span> <span data-ttu-id="838f4-146">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="838f4-146">This property is optional.</span></span> <span data-ttu-id="838f4-147">Plán pro aktivitu v současné době musí odpovídat plánu, zadaný pro výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-147">Currently, the schedule for the activity must match the schedule specified for the output dataset.</span></span>
 
```json
{
    "name": "SamplePipeline",
    "properties": {
        "description": "copy activity",
        "activities": [
            {
                "type": "Copy",
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 100000,
                        "writeBatchTimeout": "00:05:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureSQLInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        ],
        "start": "2017-04-01T08:00:00Z",
        "end": "2017-04-01T11:00:00Z"
    }
}
```

<span data-ttu-id="838f4-148">V tomto příkladu aktivity spouští každou hodinu mezi počáteční a koncový čas kanálu.</span><span class="sxs-lookup"><span data-stu-id="838f4-148">In this example, the activity runs hourly between the start and end times of the pipeline.</span></span> <span data-ttu-id="838f4-149">Výstupní data se vytvářejí každou hodinu pro windows 3 hodiny (8: 00 - 9 AM, 9: 00 – 10: 00 a 10 AM - 11 AM).</span><span class="sxs-lookup"><span data-stu-id="838f4-149">The output data is produced hourly for three-hour windows (8 AM - 9 AM, 9 AM - 10 AM, and 10 AM - 11 AM).</span></span> 

<span data-ttu-id="838f4-150">Je volána jednotlivých jednotek data využívat nebo vyprodukované aktivitu spustit **datový řez**.</span><span class="sxs-lookup"><span data-stu-id="838f4-150">Each unit of data consumed or produced by an activity run is called a **data slice**.</span></span> <span data-ttu-id="838f4-151">Následující diagram ukazuje příklad aktivitu jednu vstupní datovou sadu a jednu výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="838f4-151">The following diagram shows an example of an activity with one input dataset and one output dataset:</span></span> 

![Dostupnost plánovače](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

<span data-ttu-id="838f4-153">Diagram znázorňuje hodinové datové řezy vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-153">The diagram shows the hourly data slices for the input and output dataset.</span></span> <span data-ttu-id="838f4-154">Diagram zobrazuje tři vstupní řezy, které jsou připravené ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="838f4-154">The diagram shows three input slices that are ready for processing.</span></span> <span data-ttu-id="838f4-155">Aktivita 10 11 AM probíhá, vytváření výstupní řez AM 10 11.</span><span class="sxs-lookup"><span data-stu-id="838f4-155">The 10-11 AM activity is in progress, producing the 10-11 AM output slice.</span></span> 

<span data-ttu-id="838f4-156">Dostanete časový interval přidružené spuštění aktuálního řezu v datové sadě JSON pomocí proměnných: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) a [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="838f4-156">You can access the time interval associated with the current slice in the dataset JSON by using variables: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) and [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="838f4-157">Podobně můžete přistupovat časový interval, který je přidružený okně aktivita pomocí WindowStart a WindowEnd.</span><span class="sxs-lookup"><span data-stu-id="838f4-157">Similarly, you can access the time interval associated with an activity window by using the WindowStart and WindowEnd.</span></span> <span data-ttu-id="838f4-158">Plán aktivity musí odpovídat plán výstupní datovou sadu aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-158">The schedule of an activity must match the schedule of the output dataset for the activity.</span></span> <span data-ttu-id="838f4-159">Proto SliceStart a SliceEnd hodnoty jsou stejné jako hodnoty WindowStart a WindowEnd v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="838f4-159">Therefore, the SliceStart and SliceEnd values are the same as WindowStart and WindowEnd values respectively.</span></span> <span data-ttu-id="838f4-160">Další informace o těchto proměnných najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables) články.</span><span class="sxs-lookup"><span data-stu-id="838f4-160">For more information on these variables, see [Data Factory functions and system variables](data-factory-functions-variables.md#data-factory-system-variables) articles.</span></span>  

<span data-ttu-id="838f4-161">Tyto proměnné můžete použít pro jiné účely vaše aktivity JSON.</span><span class="sxs-lookup"><span data-stu-id="838f4-161">You can use these variables for different purposes in your activity JSON.</span></span> <span data-ttu-id="838f4-162">Například je můžete použít k výběru dat z vstupní a výstupní datové sady reprezentující data časové řady (například: 8: 00 do 9: 00).</span><span class="sxs-lookup"><span data-stu-id="838f4-162">For example, you can use them to select data from input and output datasets representing time series data (for example: 8 AM to 9 AM).</span></span> <span data-ttu-id="838f4-163">Tento příklad také používá **WindowStart** a **WindowEnd** vyberte relevantní data pro aktivitu spustit a zkopírujte jej do objektu blob s příslušnou **folderPath**.</span><span class="sxs-lookup"><span data-stu-id="838f4-163">This example also uses **WindowStart** and **WindowEnd** to select relevant data for an activity run and copy it to a blob with the appropriate **folderPath**.</span></span> <span data-ttu-id="838f4-164">**FolderPath** Parametrizovaná mít samostatnou složku pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="838f4-164">The **folderPath** is parameterized to have a separate folder for every hour.</span></span>  

<span data-ttu-id="838f4-165">V předchozím příkladu plán zadaný pro vstupní a výstupní datové sady je stejné (každou hodinu).</span><span class="sxs-lookup"><span data-stu-id="838f4-165">In the preceding example, the schedule specified for input and output datasets is the same (hourly).</span></span> <span data-ttu-id="838f4-166">Pokud vstupní datovou sadu aktivity je k dispozici na jinou frekvenci, například každých 15 minut, aktivity, která vytváří tento výstupní datovou sadu stále běží jednou za hodinu jako výstupní datovou sadu se řídí plán aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-166">If the input dataset for the activity is available at a different frequency, say every 15 minutes, the activity that produces this output dataset still runs once an hour as the output dataset is what drives the activity schedule.</span></span> <span data-ttu-id="838f4-167">Další informace najdete v tématu [modelu datové sady s jinou frekvencí](#model-datasets-with-different-frequencies).</span><span class="sxs-lookup"><span data-stu-id="838f4-167">For more information, see [Model datasets with different frequencies](#model-datasets-with-different-frequencies).</span></span>

## <a name="dataset-availability-and-policies"></a><span data-ttu-id="838f4-168">Dostupnost datové sady a zásady</span><span class="sxs-lookup"><span data-stu-id="838f4-168">Dataset availability and policies</span></span>
<span data-ttu-id="838f4-169">Jste viděli využití frekvence a intervalu vlastností v části dostupnosti definice datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-169">You have seen the usage of frequency and interval properties in the availability section of dataset definition.</span></span> <span data-ttu-id="838f4-170">Existuje několik dalších vlastností, které mají vliv na plánování a provádění aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-170">There are a few other properties that affect the scheduling and execution of an activity.</span></span> 

### <a name="dataset-availability"></a><span data-ttu-id="838f4-171">Datovou sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="838f4-171">Dataset availability</span></span> 
<span data-ttu-id="838f4-172">Následující tabulka popisuje vlastnosti, které můžete použít v **dostupnosti** části:</span><span class="sxs-lookup"><span data-stu-id="838f4-172">The following table describes properties you can use in the **availability** section:</span></span>

| <span data-ttu-id="838f4-173">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="838f4-173">Property</span></span> | <span data-ttu-id="838f4-174">Popis</span><span class="sxs-lookup"><span data-stu-id="838f4-174">Description</span></span> | <span data-ttu-id="838f4-175">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="838f4-175">Required</span></span> | <span data-ttu-id="838f4-176">Výchozí</span><span class="sxs-lookup"><span data-stu-id="838f4-176">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="838f4-177">frekvence</span><span class="sxs-lookup"><span data-stu-id="838f4-177">frequency</span></span> |<span data-ttu-id="838f4-178">Určuje časovou jednotku pro produkční řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-178">Specifies the time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="838f4-179"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="838f4-179"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="838f4-180">Ano</span><span class="sxs-lookup"><span data-stu-id="838f4-180">Yes</span></span> |<span data-ttu-id="838f4-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="838f4-181">NA</span></span> |
| <span data-ttu-id="838f4-182">Interval</span><span class="sxs-lookup"><span data-stu-id="838f4-182">interval</span></span> |<span data-ttu-id="838f4-183">Určuje multiplikátor pro četnost</span><span class="sxs-lookup"><span data-stu-id="838f4-183">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="838f4-184">"Frekvence x interval" Určuje, jak často se vytvářejí řez.</span><span class="sxs-lookup"><span data-stu-id="838f4-184">”Frequency x interval” determines how often the slice is produced.</span></span><br/><br/><span data-ttu-id="838f4-185">Pokud budete potřebovat datovou sadu, která se rozříznut hodinu, nastavíte <b>frekvence</b> k <b>hodinu</b>, a <b>interval</b> k <b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="838f4-185">If you need the dataset to be sliced on an hourly basis, you set <b>Frequency</b> to <b>Hour</b>, and <b>interval</b> to <b>1</b>.</span></span><br/><br/><span data-ttu-id="838f4-186"><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme nastavit interval na menší než 15</span><span class="sxs-lookup"><span data-stu-id="838f4-186"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set the interval to no less than 15</span></span> |<span data-ttu-id="838f4-187">Ano</span><span class="sxs-lookup"><span data-stu-id="838f4-187">Yes</span></span> |<span data-ttu-id="838f4-188">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="838f4-188">NA</span></span> |
| <span data-ttu-id="838f4-189">Styl</span><span class="sxs-lookup"><span data-stu-id="838f4-189">style</span></span> |<span data-ttu-id="838f4-190">Určuje, zda by měl být na zahájení a ukončení intervalu předložen řez.</span><span class="sxs-lookup"><span data-stu-id="838f4-190">Specifies whether the slice should be produced at the start/end of the interval.</span></span><ul><li><span data-ttu-id="838f4-191">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="838f4-191">StartOfInterval</span></span></li><li><span data-ttu-id="838f4-192">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="838f4-192">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="838f4-193">Pokud je nastavena frekvence měsíc a styl je nastaven na EndOfInterval, řez vytváří poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="838f4-193">If Frequency is set to Month and style is set to EndOfInterval, the slice is produced on the last day of month.</span></span> <span data-ttu-id="838f4-194">Pokud je styl nastavené na StartOfInterval, řez vytváří první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="838f4-194">If the style is set to StartOfInterval, the slice is produced on the first day of month.</span></span><br/><br/><span data-ttu-id="838f4-195">Pokud je nastavena frekvence den a styl je nastaven na EndOfInterval, řez se vytvářejí za poslední hodinu dne.</span><span class="sxs-lookup"><span data-stu-id="838f4-195">If Frequency is set to Day and style is set to EndOfInterval, the slice is produced in the last hour of the day.</span></span><br/><br/><span data-ttu-id="838f4-196">Pokud je nastavena frekvence hodinu a styl je nastaven na EndOfInterval, řez se vytvářejí na konci za hodinu.</span><span class="sxs-lookup"><span data-stu-id="838f4-196">If Frequency is set to Hour and style is set to EndOfInterval, the slice is produced at the end of the hour.</span></span> <span data-ttu-id="838f4-197">Například pro řez dobu 13: 00 – 14: 00, je řez vytvořeného ve 2.</span><span class="sxs-lookup"><span data-stu-id="838f4-197">For example, for a slice for 1 PM – 2 PM period, the slice is produced at 2 PM.</span></span> |<span data-ttu-id="838f4-198">Ne</span><span class="sxs-lookup"><span data-stu-id="838f4-198">No</span></span> |<span data-ttu-id="838f4-199">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="838f4-199">EndOfInterval</span></span> |
| <span data-ttu-id="838f4-200">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="838f4-200">anchorDateTime</span></span> |<span data-ttu-id="838f4-201">Definuje absolutní pozici v čase plánovačem slouží k výpočtu hranice řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-201">Defines the absolute position in time used by scheduler to compute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="838f4-202"><b>Poznámka:</b>: Pokud AnchorDateTime má částí data, která jsou podrobnější než je četnost pak podrobnější části jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="838f4-202"><b>Note</b>: If the AnchorDateTime has date parts that are more granular than the frequency then the more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="838f4-203">Například pokud <b>interval</b> je <b>každou hodinu</b> (frekvence: hodinu a intervalu: 1) a <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b>, pak se <b>minuty a sekundy</b> částí AnchorDateTime jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="838f4-203">For example, if the <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and the <b>AnchorDateTime</b> contains <b>minutes and seconds</b>, then the <b>minutes and seconds</b> parts of the AnchorDateTime are ignored.</span></span> |<span data-ttu-id="838f4-204">Ne</span><span class="sxs-lookup"><span data-stu-id="838f4-204">No</span></span> |<span data-ttu-id="838f4-205">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="838f4-205">01/01/0001</span></span> |
| <span data-ttu-id="838f4-206">Posun</span><span class="sxs-lookup"><span data-stu-id="838f4-206">offset</span></span> |<span data-ttu-id="838f4-207">Časový interval, ve kterém jsou zapuštěno počáteční a koncová všech řezech datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-207">Timespan by which the start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="838f4-208"><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem je kombinovaná shift.</span><span class="sxs-lookup"><span data-stu-id="838f4-208"><b>Note</b>: If both anchorDateTime and offset are specified, the result is the combined shift.</span></span> |<span data-ttu-id="838f4-209">Ne</span><span class="sxs-lookup"><span data-stu-id="838f4-209">No</span></span> |<span data-ttu-id="838f4-210">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="838f4-210">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="838f4-211">Příklad posunutí</span><span class="sxs-lookup"><span data-stu-id="838f4-211">offset example</span></span>
<span data-ttu-id="838f4-212">Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy spuštění na čas UTC 12: 00 (půlnoc).</span><span class="sxs-lookup"><span data-stu-id="838f4-212">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM UTC time (midnight).</span></span> <span data-ttu-id="838f4-213">Pokud chcete, aby čas spuštění jako čas UTC 6: 00, nastavte posun, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="838f4-213">If you want the start time to be 6 AM UTC time instead, set the offset as shown in the following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="838f4-214">Příklad anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="838f4-214">anchorDateTime example</span></span>
<span data-ttu-id="838f4-215">V následujícím příkladu se sada vytváří jednou za 23 hodin.</span><span class="sxs-lookup"><span data-stu-id="838f4-215">In the following example, the dataset is produced once every 23 hours.</span></span> <span data-ttu-id="838f4-216">První řez spustí v době určeného anchorDateTime, který je nastaven na `2017-04-19T08:00:00` (Světový čas UTC).</span><span class="sxs-lookup"><span data-stu-id="838f4-216">The first slice starts at the time specified by the anchorDateTime, which is set to `2017-04-19T08:00:00` (UTC time).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="838f4-217">Posun nebo styl příklad</span><span class="sxs-lookup"><span data-stu-id="838f4-217">offset/style Example</span></span>
<span data-ttu-id="838f4-218">Tyto datové sady je měsíční datová sada a vytváří 3rd v každém měsíci v 8:00 AM (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="838f4-218">The following dataset is a monthly dataset and is produced on 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a><span data-ttu-id="838f4-219">Datovou sadu zásad</span><span class="sxs-lookup"><span data-stu-id="838f4-219">Dataset policy</span></span>
<span data-ttu-id="838f4-220">Datovou sadu, může mít definované zásady ověřování, která určuje, jak může být ověřen data generována řez provádění předtím, než je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="838f4-220">A dataset can have a validation policy defined that specifies how the data generated by a slice execution can be validated before it is ready for consumption.</span></span> <span data-ttu-id="838f4-221">V takových případech po dokončení provádění řez výstupní stav řezu se změní na **čekání** s substatus z **ověření**.</span><span class="sxs-lookup"><span data-stu-id="838f4-221">In such cases, after the slice has finished execution, the output slice status is changed to **Waiting** with a substatus of **Validation**.</span></span> <span data-ttu-id="838f4-222">Po ověření řezy jsou stav řezu se změní na **připraven**.</span><span class="sxs-lookup"><span data-stu-id="838f4-222">After the slices are validated, the slice status changes to **Ready**.</span></span> <span data-ttu-id="838f4-223">Pokud datový řez pochází, ale nebyla úspěšná ověření, nebudou zpracovány spuštění aktivity pro příjem dat datové řezy, které závisí na tento řez.</span><span class="sxs-lookup"><span data-stu-id="838f4-223">If a data slice has been produced but did not pass the validation, activity runs for downstream slices that depend on this slice are not processed.</span></span> <span data-ttu-id="838f4-224">[Monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) popisuje různé stavy datové řezy ve službě Data Factory.</span><span class="sxs-lookup"><span data-stu-id="838f4-224">[Monitor and manage pipelines](data-factory-monitor-manage-pipelines.md) covers the various states of data slices in Data Factory.</span></span>

<span data-ttu-id="838f4-225">**Zásad** oddíl v definici datové sady definuje kritéria nebo podmínku, musíte splnit řezy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-225">The **policy** section in dataset definition defines the criteria or the condition that the dataset slices must fulfill.</span></span> <span data-ttu-id="838f4-226">Následující tabulka popisuje vlastnosti, které můžete použít v **zásad** části:</span><span class="sxs-lookup"><span data-stu-id="838f4-226">The following table describes properties you can use in the **policy** section:</span></span>

| <span data-ttu-id="838f4-227">Název zásady</span><span class="sxs-lookup"><span data-stu-id="838f4-227">Policy Name</span></span> | <span data-ttu-id="838f4-228">Popis</span><span class="sxs-lookup"><span data-stu-id="838f4-228">Description</span></span> | <span data-ttu-id="838f4-229">Použít</span><span class="sxs-lookup"><span data-stu-id="838f4-229">Applied To</span></span> | <span data-ttu-id="838f4-230">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="838f4-230">Required</span></span> | <span data-ttu-id="838f4-231">Výchozí</span><span class="sxs-lookup"><span data-stu-id="838f4-231">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="838f4-232">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="838f4-232">minimumSizeMB</span></span> | <span data-ttu-id="838f4-233">Ověří, jestli data v **objektů blob v Azure** splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="838f4-233">Validates that the data in an **Azure blob** meets the minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="838f4-234">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="838f4-234">Azure Blob</span></span> |<span data-ttu-id="838f4-235">Ne</span><span class="sxs-lookup"><span data-stu-id="838f4-235">No</span></span> |<span data-ttu-id="838f4-236">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="838f4-236">NA</span></span> |
| <span data-ttu-id="838f4-237">minimumRows</span><span class="sxs-lookup"><span data-stu-id="838f4-237">minimumRows</span></span> | <span data-ttu-id="838f4-238">Ověří, jestli data v **Azure SQL database** nebo **tabulky Azure** obsahuje minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="838f4-238">Validates that the data in an **Azure SQL database** or an **Azure table** contains the minimum number of rows.</span></span> |<ul><li><span data-ttu-id="838f4-239">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="838f4-239">Azure SQL Database</span></span></li><li><span data-ttu-id="838f4-240">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="838f4-240">Azure Table</span></span></li></ul> |<span data-ttu-id="838f4-241">Ne</span><span class="sxs-lookup"><span data-stu-id="838f4-241">No</span></span> |<span data-ttu-id="838f4-242">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="838f4-242">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="838f4-243">Příklady</span><span class="sxs-lookup"><span data-stu-id="838f4-243">Examples</span></span>
<span data-ttu-id="838f4-244">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="838f4-244">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="838f4-245">**minimumRows**</span><span class="sxs-lookup"><span data-stu-id="838f4-245">**minimumRows**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

<span data-ttu-id="838f4-246">Další informace o těchto vlastnostech a příklady naleznete v tématu [vytvoření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="838f4-246">For more information about these properties and examples, see [Create datasets](data-factory-create-datasets.md) article.</span></span> 

## <a name="activity-policies"></a><span data-ttu-id="838f4-247">Zásady aktivit</span><span class="sxs-lookup"><span data-stu-id="838f4-247">Activity policies</span></span>
<span data-ttu-id="838f4-248">Zásady ovlivňují chování běhu aktivity, konkrétně při zpracování řezu tabulky.</span><span class="sxs-lookup"><span data-stu-id="838f4-248">Policies affect the run-time behavior of an activity, specifically when the slice of a table is processed.</span></span> <span data-ttu-id="838f4-249">Následující tabulka obsahuje podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="838f4-249">The following table provides the details.</span></span>

| <span data-ttu-id="838f4-250">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="838f4-250">Property</span></span> | <span data-ttu-id="838f4-251">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="838f4-251">Permitted values</span></span> | <span data-ttu-id="838f4-252">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="838f4-252">Default Value</span></span> | <span data-ttu-id="838f4-253">Popis</span><span class="sxs-lookup"><span data-stu-id="838f4-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="838f4-254">Souběžnosti</span><span class="sxs-lookup"><span data-stu-id="838f4-254">concurrency</span></span> |<span data-ttu-id="838f4-255">Integer</span><span class="sxs-lookup"><span data-stu-id="838f4-255">Integer</span></span> <br/><br/><span data-ttu-id="838f4-256">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="838f4-256">Max value: 10</span></span> |<span data-ttu-id="838f4-257">1</span><span class="sxs-lookup"><span data-stu-id="838f4-257">1</span></span> |<span data-ttu-id="838f4-258">Počet souběžných spuštění aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-258">Number of concurrent executions of the activity.</span></span><br/><br/><span data-ttu-id="838f4-259">Určuje počet spuštění paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="838f4-259">It determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="838f4-260">Například pokud aktivitu musí projít, velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="838f4-260">For example, if an activity needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> |
| <span data-ttu-id="838f4-261">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="838f4-261">executionPriorityOrder</span></span> |<span data-ttu-id="838f4-262">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="838f4-262">NewestFirst</span></span><br/><br/><span data-ttu-id="838f4-263">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="838f4-263">OldestFirst</span></span> |<span data-ttu-id="838f4-264">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="838f4-264">OldestFirst</span></span> |<span data-ttu-id="838f4-265">Určuje pořadí datové řezy, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="838f4-265">Determines the ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="838f4-266">Pokud máte 2 řezy (jeden situaci ve 4 a další v 17: 00) a jsou obě čekající na zpracování.</span><span class="sxs-lookup"><span data-stu-id="838f4-266">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="838f4-267">Pokud jste nastavili executionPriorityOrder být NewestFirst, je nejprve zpracování řezu v 17: 00.</span><span class="sxs-lookup"><span data-stu-id="838f4-267">If you set the executionPriorityOrder to be NewestFirst, the slice at 5 PM is processed first.</span></span> <span data-ttu-id="838f4-268">Podobně pokud nastavíte executionPriorityORder být OldestFIrst, pak ve 4 zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="838f4-268">Similarly if you set the executionPriorityORder to be OldestFIrst, then the slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="838f4-269">Opakování</span><span class="sxs-lookup"><span data-stu-id="838f4-269">retry</span></span> |<span data-ttu-id="838f4-270">Integer</span><span class="sxs-lookup"><span data-stu-id="838f4-270">Integer</span></span><br/><br/><span data-ttu-id="838f4-271">Maximální hodnota může být 10</span><span class="sxs-lookup"><span data-stu-id="838f4-271">Max value can be 10</span></span> |<span data-ttu-id="838f4-272">0</span><span class="sxs-lookup"><span data-stu-id="838f4-272">0</span></span> |<span data-ttu-id="838f4-273">Počet opakování, než se zpracování dat pro řez je označen jako selhání.</span><span class="sxs-lookup"><span data-stu-id="838f4-273">Number of retries before the data processing for the slice is marked as Failure.</span></span> <span data-ttu-id="838f4-274">Provedení aktivity pro datový řez je opakovat až zadaný počet.</span><span class="sxs-lookup"><span data-stu-id="838f4-274">Activity execution for a data slice is retried up to the specified retry count.</span></span> <span data-ttu-id="838f4-275">Opakovaném provádí co nejdříve po selhání.</span><span class="sxs-lookup"><span data-stu-id="838f4-275">The retry is done as soon as possible after the failure.</span></span> |
| <span data-ttu-id="838f4-276">Časový limit</span><span class="sxs-lookup"><span data-stu-id="838f4-276">timeout</span></span> |<span data-ttu-id="838f4-277">Časový interval</span><span class="sxs-lookup"><span data-stu-id="838f4-277">TimeSpan</span></span> |<span data-ttu-id="838f4-278">00:00:00</span><span class="sxs-lookup"><span data-stu-id="838f4-278">00:00:00</span></span> |<span data-ttu-id="838f4-279">Časový limit aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-279">Timeout for the activity.</span></span> <span data-ttu-id="838f4-280">Příklad: 00:10:00 (znamená časový limit 10 minut)</span><span class="sxs-lookup"><span data-stu-id="838f4-280">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="838f4-281">Pokud hodnota není zadána nebo je 0, časový limit je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="838f4-281">If a value is not specified or is 0, the timeout is infinite.</span></span><br/><br/><span data-ttu-id="838f4-282">Pokud bude čas zpracování dat na řez překročí hodnota časového limitu, se zruší a systém se pokusí opakujte zpracování.</span><span class="sxs-lookup"><span data-stu-id="838f4-282">If the data processing time on a slice exceeds the timeout value, it is canceled, and the system attempts to retry the processing.</span></span> <span data-ttu-id="838f4-283">Počet pokusů, závisí na vlastnost opakování.</span><span class="sxs-lookup"><span data-stu-id="838f4-283">The number of retries depends on the retry property.</span></span> <span data-ttu-id="838f4-284">Když dojde k vypršení časového limitu, je stav nastaven na TimedOut.</span><span class="sxs-lookup"><span data-stu-id="838f4-284">When timeout occurs, the status is set to TimedOut.</span></span> |
| <span data-ttu-id="838f4-285">Zpoždění</span><span class="sxs-lookup"><span data-stu-id="838f4-285">delay</span></span> |<span data-ttu-id="838f4-286">Časový interval</span><span class="sxs-lookup"><span data-stu-id="838f4-286">TimeSpan</span></span> |<span data-ttu-id="838f4-287">00:00:00</span><span class="sxs-lookup"><span data-stu-id="838f4-287">00:00:00</span></span> |<span data-ttu-id="838f4-288">Zadejte zpoždění před zpracování dat řezu spustí.</span><span class="sxs-lookup"><span data-stu-id="838f4-288">Specify the delay before data processing of the slice starts.</span></span><br/><br/><span data-ttu-id="838f4-289">Provádění aktivity pro datový řez se spustí po zpoždění očekávaný čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="838f4-289">The execution of activity for a data slice is started after the Delay is past the expected execution time.</span></span><br/><br/><span data-ttu-id="838f4-290">Příklad: 00:10:00 (znamená zpoždění 10 minut)</span><span class="sxs-lookup"><span data-stu-id="838f4-290">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="838f4-291">opakování po delší době</span><span class="sxs-lookup"><span data-stu-id="838f4-291">longRetry</span></span> |<span data-ttu-id="838f4-292">Integer</span><span class="sxs-lookup"><span data-stu-id="838f4-292">Integer</span></span><br/><br/><span data-ttu-id="838f4-293">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="838f4-293">Max value: 10</span></span> |<span data-ttu-id="838f4-294">1</span><span class="sxs-lookup"><span data-stu-id="838f4-294">1</span></span> |<span data-ttu-id="838f4-295">Počet dlouho opakování pokusů, než řez spuštění se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="838f4-295">The number of long retry attempts before the slice execution is failed.</span></span><br/><br/><span data-ttu-id="838f4-296">pokusy o opakování po delší době jsou rozmístěny ve longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="838f4-296">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="838f4-297">Takže pokud je třeba zadat čas mezi pokusy o opakování, použijte opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="838f4-297">So if you need to specify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="838f4-298">Pokud jsou zadané opakování a opakování po delší době, jednotlivé pokusy o opakování po delší době zahrnuje opakovaných pokusů a je maximální počet pokusů o opakování * opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="838f4-298">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and the max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="838f4-299">Například, pokud bychom měli následující nastavení v zásadách aktivit:</span><span class="sxs-lookup"><span data-stu-id="838f4-299">For example, if we have the following settings in the activity policy:</span></span><br/><span data-ttu-id="838f4-300">Opakujte: 3</span><span class="sxs-lookup"><span data-stu-id="838f4-300">Retry: 3</span></span><br/><span data-ttu-id="838f4-301">opakování po delší době: 2</span><span class="sxs-lookup"><span data-stu-id="838f4-301">longRetry: 2</span></span><br/><span data-ttu-id="838f4-302">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="838f4-302">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="838f4-303">Předpokládá se jenom jeden řez provést (stav Čeká) a provedení aktivity pokaždé, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="838f4-303">Assume there is only one slice to execute (status is Waiting) and the activity execution fails every time.</span></span> <span data-ttu-id="838f4-304">Nejdřív by 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="838f4-304">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="838f4-305">Po každém pokusu o stav řezu bude opakovat.</span><span class="sxs-lookup"><span data-stu-id="838f4-305">After each attempt, the slice status would be Retry.</span></span> <span data-ttu-id="838f4-306">Po první 3 pokusy jsou přes, bude stav řezu opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="838f4-306">After first 3 attempts are over, the slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="838f4-307">Po hodině (který je na longRetryInteval hodnota) bude další sadu 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="838f4-307">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="838f4-308">Poté stav řezu by se nezdařilo a by se pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="838f4-308">After that, the slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="838f4-309">Proto celkové 6 pokusy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="838f4-309">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="838f4-310">Pokud žádné spuštění úspěšné, stav řezu by mít připravené a jsou pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="838f4-310">If any execution succeeds, the slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="838f4-311">opakování po delší době je možné použít situace, kdy závislé data dorazí na Nedeterministický časy nebo je v nestabilním stavu v rámci které zpracování dat dojde celém prostředí.</span><span class="sxs-lookup"><span data-stu-id="838f4-311">longRetry may be used in situations where dependent data arrives at non-deterministic times or the overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="838f4-312">V takových případech to, které opakování, jedna po druhé nemusí být úspěšná a díky tomu v intervalech čas má za následek požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="838f4-312">In such cases, doing retries one after another may not help and doing so after an interval of time results in the desired output.</span></span><br/><br/><span data-ttu-id="838f4-313">Word varování: nenastavujte vysoké hodnoty pro opakování po delší době nebo longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="838f4-313">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="838f4-314">Vyšší hodnoty obvykle implikují dalších systémových otázek.</span><span class="sxs-lookup"><span data-stu-id="838f4-314">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="838f4-315">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="838f4-315">longRetryInterval</span></span> |<span data-ttu-id="838f4-316">Časový interval</span><span class="sxs-lookup"><span data-stu-id="838f4-316">TimeSpan</span></span> |<span data-ttu-id="838f4-317">00:00:00</span><span class="sxs-lookup"><span data-stu-id="838f4-317">00:00:00</span></span> |<span data-ttu-id="838f4-318">Prodleva mezi pokusy o opakování dlouho</span><span class="sxs-lookup"><span data-stu-id="838f4-318">The delay between long retry attempts</span></span> |

<span data-ttu-id="838f4-319">Další informace najdete v tématu [kanály](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="838f4-319">For more information, see [Pipelines](data-factory-create-pipelines.md) article.</span></span> 

## <a name="parallel-processing-of-data-slices"></a><span data-ttu-id="838f4-320">Paralelní zpracování datové řezy</span><span class="sxs-lookup"><span data-stu-id="838f4-320">Parallel processing of data slices</span></span>
<span data-ttu-id="838f4-321">Počáteční datum pro kanál můžete nastavit v minulosti.</span><span class="sxs-lookup"><span data-stu-id="838f4-321">You can set the start date for the pipeline in the past.</span></span> <span data-ttu-id="838f4-322">Pokud tak učiníte, Data Factory automaticky vypočítá všechny datové řezy (back výplněmi) v minulosti a zahájí zpracování je.</span><span class="sxs-lookup"><span data-stu-id="838f4-322">When you do so, Data Factory automatically calculates (back fills) all data slices in the past and begins processing them.</span></span> <span data-ttu-id="838f4-323">Například: Pokud vytvoření kanálu s počátečním datem 2017-04-01 a aktuální datum následuje 2017-04-10.</span><span class="sxs-lookup"><span data-stu-id="838f4-323">For example: if you create a pipeline with start date 2017-04-01 and the current date is 2017-04-10.</span></span> <span data-ttu-id="838f4-324">Pokud cadence výstupní datové sady je denně, Data Factory spustí zpracování všech řezech z 2017-04-01 do 2017-04-09 okamžitě, protože je počáteční datum v minulosti.</span><span class="sxs-lookup"><span data-stu-id="838f4-324">If the cadence of the output dataset is daily, then Data Factory starts processing all the slices from 2017-04-01 to 2017-04-09 immediately because the start date is in the past.</span></span> <span data-ttu-id="838f4-325">Z 2017-04-10 není zpracování řezu ještě protože hodnota vlastnosti stylu v části dostupnosti je EndOfInterval ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="838f4-325">The slice from 2017-04-10 is not processed yet because the value of style property in the availability section is EndOfInterval by default.</span></span> <span data-ttu-id="838f4-326">Zpracování nejstarší řezu se nejprve jako výchozí hodnota executionPriorityOrder je OldestFirst.</span><span class="sxs-lookup"><span data-stu-id="838f4-326">The oldest slice is processed first as the default value of executionPriorityOrder is OldestFirst.</span></span> <span data-ttu-id="838f4-327">Popis vlastnost stylu naleznete v tématu [datovou sadu dostupnosti](#dataset-availability) části.</span><span class="sxs-lookup"><span data-stu-id="838f4-327">For a description of the style property, see [dataset availability](#dataset-availability) section.</span></span> <span data-ttu-id="838f4-328">Popis části executionPriorityOrder najdete v tématu [zásady aktivit](#activity-policies) části.</span><span class="sxs-lookup"><span data-stu-id="838f4-328">For a description of the executionPriorityOrder section, see the [activity policies](#activity-policies) section.</span></span> 

<span data-ttu-id="838f4-329">Můžete nakonfigurovat, zpět vyplněno datové řezy, které mají být zpracovány současně nastavením **souběžnosti** vlastnost v **zásad** části kódu JSON aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-329">You can configure back-filled data slices to be processed in parallel by setting the **concurrency** property in the **policy** section of the activity JSON.</span></span> <span data-ttu-id="838f4-330">Tato vlastnost určuje počet spuštění paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="838f4-330">This property determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="838f4-331">Výchozí hodnota pro vlastnost souběžnosti je 1.</span><span class="sxs-lookup"><span data-stu-id="838f4-331">The default value for the concurrency property is 1.</span></span> <span data-ttu-id="838f4-332">Proto jeden zpracování řezu se současně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="838f4-332">Therefore, one slice is processed at a time by default.</span></span> <span data-ttu-id="838f4-333">Maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="838f4-333">The maximum value is 10.</span></span> <span data-ttu-id="838f4-334">Když kanál musí projít velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="838f4-334">When a pipeline needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> 

## <a name="rerun-a-failed-data-slice"></a><span data-ttu-id="838f4-335">Opětovné spuštění neúspěšné datový řez</span><span class="sxs-lookup"><span data-stu-id="838f4-335">Rerun a failed data slice</span></span>
<span data-ttu-id="838f4-336">Když dojde k chybě při zpracování dat řezu, můžete zjistit proč zpracování řezu se nezdařilo pomocí oken webu Azure portal nebo monitorování a správě aplikace.</span><span class="sxs-lookup"><span data-stu-id="838f4-336">When an error occurs while processing a data slice, you can find out why the processing of a slice failed by using Azure portal blades or Monitor and Manage App.</span></span> <span data-ttu-id="838f4-337">V tématu [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="838f4-337">See [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md) for details.</span></span>

<span data-ttu-id="838f4-338">Prohlédněte si následující příklad ukazuje dvě aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-338">Consider the following example, which shows two activities.</span></span> <span data-ttu-id="838f4-339">Aktivity "activity1" a aktivitu 2.</span><span class="sxs-lookup"><span data-stu-id="838f4-339">Activity1 and Activity 2.</span></span> <span data-ttu-id="838f4-340">Aktivity "activity1" využívá řez Dataset1 a produkuje řez Dataset2, který využívá jako vstup "activity2" k vytvoření řez konečné datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-340">Activity1 consumes a slice of Dataset1 and produces a slice of Dataset2, which is consumed as an input by Activity2 to produce a slice of the Final Dataset.</span></span>

![Řez se nezdařilo](./media/data-factory-scheduling-and-execution/failed-slice.png)

<span data-ttu-id="838f4-342">Diagram ukazuje, že mimo tři poslední řezů, došlo k chybě, která generovala řez AM 9 10 pro Dataset2.</span><span class="sxs-lookup"><span data-stu-id="838f4-342">The diagram shows that out of three recent slices, there was a failure producing the 9-10 AM slice for Dataset2.</span></span> <span data-ttu-id="838f4-343">Objekt pro vytváření dat automaticky sleduje závislostí pro datovou sadu časové řady.</span><span class="sxs-lookup"><span data-stu-id="838f4-343">Data Factory automatically tracks dependency for the time series dataset.</span></span> <span data-ttu-id="838f4-344">V důsledku toho se nespustí aktivity při spuštění pro příjem dat řez 9 – 10: 00.</span><span class="sxs-lookup"><span data-stu-id="838f4-344">As a result, it does not start the activity run for the 9-10 AM downstream slice.</span></span>

<span data-ttu-id="838f4-345">Datový objekt pro vytváření monitorování a nástroje pro správu umožňují rozbalit diagnostické protokoly pro selhání řez snadno najít hlavní příčinu problému a opravte ho.</span><span class="sxs-lookup"><span data-stu-id="838f4-345">Data Factory monitoring and management tools allow you to drill into the diagnostic logs for the failed slice to easily find the root cause for the issue and fix it.</span></span> <span data-ttu-id="838f4-346">Po opravení problému můžete snadno začít aktivity při spuštění k vytvoření se nezdařilo řez.</span><span class="sxs-lookup"><span data-stu-id="838f4-346">After you have fixed the issue, you can easily start the activity run to produce the failed slice.</span></span> <span data-ttu-id="838f4-347">Další informace o tom, jak znovu spustit a pochopit přechodů mezi stavy pro datové řezy najdete v tématu [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="838f4-347">For more information on how to rerun and understand state transitions for data slices, see [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span>

<span data-ttu-id="838f4-348">Po spustit řez AM 9 10 pro znovu **Dataset2**, Data Factory spustí spustit pro řez závislé 9 10 AM na poslední datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="838f4-348">After you rerun the 9-10 AM slice for **Dataset2**, Data Factory starts the run for the 9-10 AM dependent slice on the final dataset.</span></span>

![Opětovné spuštění neúspěšné řez](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a><span data-ttu-id="838f4-350">Více aktivit v kanálu</span><span class="sxs-lookup"><span data-stu-id="838f4-350">Multiple activities in a pipeline</span></span>
<span data-ttu-id="838f4-351">Můžete mít více než jednu aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="838f4-351">You can have more than one activity in a pipeline.</span></span> <span data-ttu-id="838f4-352">Pokud máte více aktivit v kanálu a výstup aktivity není vstup jinou aktivitu, může paralelně spustit aktivity, pokud připravení řezy vstupní data pro aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-352">If you have multiple activities in a pipeline and the output of an activity is not an input of another activity, the activities may run in parallel if input data slices for the activities are ready.</span></span>

<span data-ttu-id="838f4-353">Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-353">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="838f4-354">Aktivity může být v kanálu stejné nebo jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="838f4-354">The activities can be in the same pipeline or in different pipelines.</span></span> <span data-ttu-id="838f4-355">Druhá aktivita se spustí, pouze pokud první skončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="838f4-355">The second activity executes only when the first one finishes successfully.</span></span>

<span data-ttu-id="838f4-356">Zvažte například následující případ, kdy kanálu má dvě aktivity:</span><span class="sxs-lookup"><span data-stu-id="838f4-356">For example, consider the following case where a pipeline has two activities:</span></span>

1. <span data-ttu-id="838f4-357">A1 aktivity, která vyžaduje externí vstupní datové sady D1 a vytvoří výstupní datovou sadu D2.</span><span class="sxs-lookup"><span data-stu-id="838f4-357">Activity A1 that requires external input dataset D1, and produces output dataset D2.</span></span>
2. <span data-ttu-id="838f4-358">A2 aktivity, který vyžaduje vstup z datové sady D2 a vytvoří výstupní datovou sadu D3.</span><span class="sxs-lookup"><span data-stu-id="838f4-358">Activity A2 that requires input from dataset D2, and produces output dataset D3.</span></span>

<span data-ttu-id="838f4-359">V tomto scénáři aktivity A1 a A2, byly ve stejné kanálu.</span><span class="sxs-lookup"><span data-stu-id="838f4-359">In this scenario, activities A1 and A2 are in the same pipeline.</span></span> <span data-ttu-id="838f4-360">Aktivity A1 spustí, když externích dat je k dispozici a je dosaženo frekvence naplánovaná dostupnost.</span><span class="sxs-lookup"><span data-stu-id="838f4-360">The activity A1 runs when the external data is available and the scheduled availability frequency is reached.</span></span> <span data-ttu-id="838f4-361">Aktivity A2 spustí naplánované řezy z D2 k dispozici a je dosaženo frekvence naplánovaná dostupnost.</span><span class="sxs-lookup"><span data-stu-id="838f4-361">The activity A2 runs when the scheduled slices from D2 become available and the scheduled availability frequency is reached.</span></span> <span data-ttu-id="838f4-362">Pokud dojde k chybě v jednom z řezy v datové sadě D2, nespustí se pro tento řez A2, dokud nebude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="838f4-362">If there is an error in one of the slices in dataset D2, A2 does not run for that slice until it becomes available.</span></span>

<span data-ttu-id="838f4-363">Zobrazení diagramu s obou aktivity v kanálu stejné by vypadat podobně jako v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="838f4-363">The Diagram view with both activities in the same pipeline would look like the following diagram:</span></span>

![Řetězení aktivity v kanálu stejné](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

<span data-ttu-id="838f4-365">Jak už bylo zmíněno dříve, aktivity může být v jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="838f4-365">As mentioned earlier, the activities could be in different pipelines.</span></span> <span data-ttu-id="838f4-366">V takové situaci zobrazení diagramu bude vypadat následující diagram:</span><span class="sxs-lookup"><span data-stu-id="838f4-366">In such a scenario, the diagram view would look like the following diagram:</span></span>

![Řetězení aktivity ve dvou kanálů](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

<span data-ttu-id="838f4-368">Najdete v článku [zkopírujte postupně](#copy-sequentially) část v příloze příklad.</span><span class="sxs-lookup"><span data-stu-id="838f4-368">See the [copy sequentially](#copy-sequentially) section in the appendix for an example.</span></span>

## <a name="model-datasets-with-different-frequencies"></a><span data-ttu-id="838f4-369">Datové sady modelu s jinou frekvence</span><span class="sxs-lookup"><span data-stu-id="838f4-369">Model datasets with different frequencies</span></span>
<span data-ttu-id="838f4-370">Frekvence pro vstupní a výstupní datové sady a okno plánu aktivity v ukázky, byly stejné.</span><span class="sxs-lookup"><span data-stu-id="838f4-370">In the samples, the frequencies for input and output datasets and the activity schedule window were the same.</span></span> <span data-ttu-id="838f4-371">Některé scénáře vyžadují možnost vytvoření výstupu frekvencí jiný než frekvence jeden nebo více vstupů.</span><span class="sxs-lookup"><span data-stu-id="838f4-371">Some scenarios require the ability to produce output at a frequency different than the frequencies of one or more inputs.</span></span> <span data-ttu-id="838f4-372">Objekt pro vytváření dat podporuje tyto scénáře modelování.</span><span class="sxs-lookup"><span data-stu-id="838f4-372">Data Factory supports modeling these scenarios.</span></span>

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a><span data-ttu-id="838f4-373">Příklad 1: Vytvoření sestavy denní výstup pro vstupní data, která je k dispozici každou hodinu</span><span class="sxs-lookup"><span data-stu-id="838f4-373">Sample 1: Produce a daily output report for input data that is available every hour</span></span>
<span data-ttu-id="838f4-374">Vezměte v úvahu scénář, ve kterém můžete mít vstupní měření data ze senzorů, které jsou k dispozici každou hodinu do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="838f4-374">Consider a scenario in which you have input measurement data from sensors available every hour in Azure Blob storage.</span></span> <span data-ttu-id="838f4-375">Chcete vytvořit sestavu denní agregační s statistické údaje, třeba střední, maximální a minimální den s [Data Factory hive aktivity](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="838f4-375">You want to produce a daily aggregate report with statistics such as mean, maximum, and minimum for the day with [Data Factory hive activity](data-factory-hive-activity.md).</span></span>

<span data-ttu-id="838f4-376">Zde je, jak můžete model tento scénář se objekt pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="838f4-376">Here is how you can model this scenario with Data Factory:</span></span>

<span data-ttu-id="838f4-377">**Vstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="838f4-377">**Input dataset**</span></span>

<span data-ttu-id="838f4-378">Hodinové vstupní soubory jsou vyřadit ve složce pro daný den.</span><span class="sxs-lookup"><span data-stu-id="838f4-378">The hourly input files are dropped in the folder for the given day.</span></span> <span data-ttu-id="838f4-379">Dostupnost pro vstup je nastavený na **hodinu** (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="838f4-379">Availability for input is set at **Hour** (frequency: Hour, interval: 1).</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="838f4-380">**Výstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="838f4-380">**Output dataset**</span></span>

<span data-ttu-id="838f4-381">Jedna výstupní soubor se vytvoří každý den ve složce den.</span><span class="sxs-lookup"><span data-stu-id="838f4-381">One output file is created every day in the day's folder.</span></span> <span data-ttu-id="838f4-382">Dostupnost výstupu je nastavený na **den** (frekvence: den a interval: 1).</span><span class="sxs-lookup"><span data-stu-id="838f4-382">Availability of output is set at **Day** (frequency: Day and interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="838f4-383">**Aktivity: aktivitu v kanálu hivu**</span><span class="sxs-lookup"><span data-stu-id="838f4-383">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="838f4-384">Skriptu hive obdrží odpovídající *data a času* informace jako parametry, které používají **WindowStart** proměnné, jak je znázorněno v následujícím fragmentu kódu.</span><span class="sxs-lookup"><span data-stu-id="838f4-384">The hive script receives the appropriate *DateTime* information as parameters that use the **WindowStart** variable as shown in the following snippet.</span></span> <span data-ttu-id="838f4-385">Skriptu hive používá tuto proměnnou k načítání dat z správnou složku dne a spusťte agregace generovat výstup.</span><span class="sxs-lookup"><span data-stu-id="838f4-385">The hive script uses this variable to load the data from the correct folder for the day and run the aggregation to generate the output.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
        {
            "name": "SampleHiveActivity",
            "inputs": [
                {
                    "name": "AzureBlobInput"
                }
            ],
            "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adftutorial\\hivequery.hql",
                "scriptLinkedService": "StorageLinkedService",
                "defines": {
                    "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                    "Month": "$$Text.Format('{0:MM}',WindowStart)",
                    "Day": "$$Text.Format('{0:dd}',WindowStart)"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },            
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 2,
                "timeout": "01:00:00"
            }
         }
     ]
   }
}
```

<span data-ttu-id="838f4-386">Následující diagram znázorňuje scénář z hlediska data závislostí.</span><span class="sxs-lookup"><span data-stu-id="838f4-386">The following diagram shows the scenario from a data-dependency point of view.</span></span>

![Data závislostí](./media/data-factory-scheduling-and-execution/data-dependency.png)

<span data-ttu-id="838f4-388">Výstupní řez pro každý den, závisí na 24 řezů hodinové ze vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-388">The output slice for every day depends on 24 hourly slices from an input dataset.</span></span> <span data-ttu-id="838f4-389">Tyto závislosti služby Data Factory automaticky vypočítá po zjištění vstupních dat datové řezy, které patří do stejné období jako výstupní řez se vytváří.</span><span class="sxs-lookup"><span data-stu-id="838f4-389">Data Factory computes these dependencies automatically by figuring out the input data slices that fall in the same time period as the output slice to be produced.</span></span> <span data-ttu-id="838f4-390">Pokud některé z 24 vstupní řezy není k dispozici, čeká na objekt pro vytváření dat vstupní řez bude připravená před zahájením denní aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="838f4-390">If any of the 24 input slices is not available, Data Factory waits for the input slice to be ready before starting the daily activity run.</span></span>

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a><span data-ttu-id="838f4-391">Příklad 2: Zadejte závislostí s výrazy a funkce pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="838f4-391">Sample 2: Specify dependency with expressions and Data Factory functions</span></span>
<span data-ttu-id="838f4-392">Pojďme se jiný scénář.</span><span class="sxs-lookup"><span data-stu-id="838f4-392">Let’s consider another scenario.</span></span> <span data-ttu-id="838f4-393">Předpokládejme, že máte aktivitu hive, který zpracovává dvě vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-393">Suppose you have a hive activity that processes two input datasets.</span></span> <span data-ttu-id="838f4-394">Jeden z nich má nová data denně, ale jeden z nich získá nová data každý týden.</span><span class="sxs-lookup"><span data-stu-id="838f4-394">One of them has new data daily, but one of them gets new data every week.</span></span> <span data-ttu-id="838f4-395">Předpokládejme, že chcete provést spojení mezi dvěma vstupy a vytváření výstupu každý den.</span><span class="sxs-lookup"><span data-stu-id="838f4-395">Suppose you wanted to do a join across the two inputs and produce an output every day.</span></span>

<span data-ttu-id="838f4-396">Jednoduchý přístup, ve které Data Factory automaticky obrázků na právo vstupní řezy ke zpracování zarovnání na výstup, který datový řez časové období není funkční.</span><span class="sxs-lookup"><span data-stu-id="838f4-396">The simple approach in which Data Factory automatically figures out the right input slices to process by aligning to the output data slice’s time period does not work.</span></span>

<span data-ttu-id="838f4-397">Je nutné zadat, že pro každé aktivity při spuštění, objektu pro vytváření dat mají používat minulého týdne datový řez pro týdenní vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-397">You must specify that for every activity run, the Data Factory should use last week’s data slice for the weekly input dataset.</span></span> <span data-ttu-id="838f4-398">Při použití Azure Data Factory funkcí jak je znázorněno v následujícím fragmentu kódu k implementaci tohoto chování.</span><span class="sxs-lookup"><span data-stu-id="838f4-398">You use Azure Data Factory functions as shown in the following snippet to implement this behavior.</span></span>

<span data-ttu-id="838f4-399">**Input1: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="838f4-399">**Input1: Azure blob**</span></span>

<span data-ttu-id="838f4-400">První vstup je Azure blob, denně aktualizují.</span><span class="sxs-lookup"><span data-stu-id="838f4-400">The first input is the Azure blob being updated daily.</span></span>

```json
{
  "name": "AzureBlobInputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="838f4-401">**Input2: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="838f4-401">**Input2: Azure blob**</span></span>

<span data-ttu-id="838f4-402">Input2 je objekt blob systému Azure se aktualizovaný týdně.</span><span class="sxs-lookup"><span data-stu-id="838f4-402">Input2 is the Azure blob being updated weekly.</span></span>

```json
{
  "name": "AzureBlobInputWeekly",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 7
    }
  }
}
```

<span data-ttu-id="838f4-403">**Výstup: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="838f4-403">**Output: Azure blob**</span></span>

<span data-ttu-id="838f4-404">Jedna výstupní soubor se vytvoří každý den ve složce dne.</span><span class="sxs-lookup"><span data-stu-id="838f4-404">One output file is created every day in the folder for the day.</span></span> <span data-ttu-id="838f4-405">Dostupnost výstupu nastavený na **den** (frekvence: den, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="838f4-405">Availability of output is set to **day** (frequency: Day, interval: 1).</span></span>

```json
{
  "name": "AzureBlobOutputDaily",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
      "partitionedBy": [
        { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
        { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "MM"}},
        { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "dd"}}
      ],
      "format": {
        "type": "TextFormat"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="838f4-406">**Aktivity: aktivitu v kanálu hivu**</span><span class="sxs-lookup"><span data-stu-id="838f4-406">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="838f4-407">Aktivita hive přebírá dva vstupy a produkuje řez výstupních každý den.</span><span class="sxs-lookup"><span data-stu-id="838f4-407">The hive activity takes the two inputs and produces an output slice every day.</span></span> <span data-ttu-id="838f4-408">Zadávat lze každý den výstupní řez závislý na předchozí týden vstupní řez pro týdenní vstup následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="838f4-408">You can specify every day’s output slice to depend on the previous week’s input slice for weekly input as follows.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-01-01T08:00:00",
    "end":"2015-01-01T11:00:00",
    "description":"hive activity",
    "activities": [
      {
        "name": "SampleHiveActivity",
        "inputs": [
          {
            "name": "AzureBlobInputDaily"
          },
          {
            "name": "AzureBlobInputWeekly",
            "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
            "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutputDaily"
          }
        ],
        "linkedServiceName": "HDInsightLinkedService",
        "type": "HDInsightHive",
        "typeProperties": {
          "scriptPath": "adftutorial\\hivequery.hql",
          "scriptLinkedService": "StorageLinkedService",
          "defines": {
            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
            "Month": "$$Text.Format('{0:MM}',WindowStart)",
            "Day": "$$Text.Format('{0:dd}',WindowStart)"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        },            
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 2,  
          "timeout": "01:00:00"
        }
       }
     ]
   }
}
```

<span data-ttu-id="838f4-409">V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam funkce a systémové proměnné, které podporuje služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="838f4-409">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of functions and system variables that Data Factory supports.</span></span>

## <a name="appendix"></a><span data-ttu-id="838f4-410">Příloha</span><span class="sxs-lookup"><span data-stu-id="838f4-410">Appendix</span></span>

### <a name="example-copy-sequentially"></a><span data-ttu-id="838f4-411">Příklad: kopírování postupně</span><span class="sxs-lookup"><span data-stu-id="838f4-411">Example: copy sequentially</span></span>
<span data-ttu-id="838f4-412">Je možné spustit více operací kopírování jedna po druhé způsobem sekvenční/řazení.</span><span class="sxs-lookup"><span data-stu-id="838f4-412">It is possible to run multiple copy operations one after another in a sequential/ordered manner.</span></span> <span data-ttu-id="838f4-413">Například můžete mít dvě kopie aktivity v kanálu (CopyActivity1 a CopyActivity2) s následující výstupní datové sady vstupních dat:</span><span class="sxs-lookup"><span data-stu-id="838f4-413">For example, you might have two copy activities in a pipeline (CopyActivity1 and CopyActivity2) with the following input data output datasets:</span></span>   

<span data-ttu-id="838f4-414">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="838f4-414">CopyActivity1</span></span>

<span data-ttu-id="838f4-415">Vstup: datové sady.</span><span class="sxs-lookup"><span data-stu-id="838f4-415">Input: Dataset.</span></span> <span data-ttu-id="838f4-416">Výstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="838f4-416">Output: Dataset2.</span></span>

<span data-ttu-id="838f4-417">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="838f4-417">CopyActivity2</span></span>

<span data-ttu-id="838f4-418">Vstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="838f4-418">Input: Dataset2.</span></span>  <span data-ttu-id="838f4-419">Výstup: Dataset3.</span><span class="sxs-lookup"><span data-stu-id="838f4-419">Output: Dataset3.</span></span>

<span data-ttu-id="838f4-420">CopyActivity2 spustí jenom v případě, že CopyActivity1 proběhla úspěšně a Dataset2 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="838f4-420">CopyActivity2 would run only if the CopyActivity1 has run successfully and Dataset2 is available.</span></span>

<span data-ttu-id="838f4-421">Tady je ukázkový kanál JSON:</span><span class="sxs-lookup"><span data-stu-id="838f4-421">Here is the sample pipeline JSON:</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob1ToBlob2",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset3"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob2ToBlob3",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="838f4-422">Všimněte si, že v příkladu je výstupní datovou sadu první aktivitu kopírování (Dataset2) zadané jako vstup pro druhý aktivity.</span><span class="sxs-lookup"><span data-stu-id="838f4-422">Notice that in the example, the output dataset of the first copy activity (Dataset2) is specified as input for the second activity.</span></span> <span data-ttu-id="838f4-423">Proto druhá aktivita spustí pouze v případě, že výstupní datové sady z první aktivitu je připraven.</span><span class="sxs-lookup"><span data-stu-id="838f4-423">Therefore, the second activity runs only when the output dataset from the first activity is ready.</span></span>  

<span data-ttu-id="838f4-424">V příkladu CopyActivity2 může mít různé vstupu, například Dataset3, ale zadat Dataset2 jako vstup CopyActivity2, takže aktivity nespustí, dokud nebude dokončeno CopyActivity1.</span><span class="sxs-lookup"><span data-stu-id="838f4-424">In the example, CopyActivity2 can have a different input, such as Dataset3, but you specify Dataset2 as an input to CopyActivity2, so the activity does not run until CopyActivity1 finishes.</span></span> <span data-ttu-id="838f4-425">Například:</span><span class="sxs-lookup"><span data-stu-id="838f4-425">For example:</span></span>

<span data-ttu-id="838f4-426">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="838f4-426">CopyActivity1</span></span>

<span data-ttu-id="838f4-427">Vstup: Dataset1.</span><span class="sxs-lookup"><span data-stu-id="838f4-427">Input: Dataset1.</span></span> <span data-ttu-id="838f4-428">Výstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="838f4-428">Output: Dataset2.</span></span>

<span data-ttu-id="838f4-429">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="838f4-429">CopyActivity2</span></span>

<span data-ttu-id="838f4-430">Vstupy: Dataset3, Dataset2.</span><span class="sxs-lookup"><span data-stu-id="838f4-430">Inputs: Dataset3, Dataset2.</span></span> <span data-ttu-id="838f4-431">Výstup: Dataset4.</span><span class="sxs-lookup"><span data-stu-id="838f4-431">Output: Dataset4.</span></span>

```json
{
    "name": "ChainActivities",
    "properties": {
        "description": "Run activities in sequence",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset1"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset2"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlobToBlob",
                "description": "Copy data from a blob to another"
            },
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "Dataset3"
                    },
                    {
                        "name": "Dataset2"
                    }
                ],
                "outputs": [
                    {
                        "name": "Dataset4"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "CopyFromBlob3ToBlob4",
                "description": "Copy data from a blob to another"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="838f4-432">Všimněte si, že v příkladu jsou dva vstupní datové sady zadané pro druhý aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="838f4-432">Notice that in the example, two input datasets are specified for the second copy activity.</span></span> <span data-ttu-id="838f4-433">Jsou-li více vstupů, pouze první vstupní datovou sadu se používá ke kopírování dat, ale jiné datové sady se používají jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="838f4-433">When multiple inputs are specified, only the first input dataset is used for copying data, but other datasets are used as dependencies.</span></span> <span data-ttu-id="838f4-434">CopyActivity2 by spustit až po splnění následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="838f4-434">CopyActivity2 would start only after the following conditions are met:</span></span>

* <span data-ttu-id="838f4-435">CopyActivity1 byla úspěšně dokončena a Dataset2 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="838f4-435">CopyActivity1 has successfully completed and Dataset2 is available.</span></span> <span data-ttu-id="838f4-436">Tato datová sada se nepoužívá při kopírování dat do Dataset4.</span><span class="sxs-lookup"><span data-stu-id="838f4-436">This dataset is not used when copying data to Dataset4.</span></span> <span data-ttu-id="838f4-437">Pouze funguje jako plánování závislost pro CopyActivity2.</span><span class="sxs-lookup"><span data-stu-id="838f4-437">It only acts as a scheduling dependency for CopyActivity2.</span></span>   
* <span data-ttu-id="838f4-438">Dataset3 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="838f4-438">Dataset3 is available.</span></span> <span data-ttu-id="838f4-439">Tuto datovou sadu představuje data, která se zkopírují do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="838f4-439">This dataset represents the data that is copied to the destination.</span></span> 