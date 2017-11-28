---
title: "aaaScheduling a spuštění pomocí služby Data Factory | Microsoft Docs"
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
ms.openlocfilehash: 6114dd4896f5537c789c3b632fb90e501b694285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-factory-scheduling-and-execution"></a><span data-ttu-id="9211c-103">Data Factory plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="9211c-103">Data Factory scheduling and execution</span></span>
<span data-ttu-id="9211c-104">Tento článek vysvětluje aspekty plánování a provádění hello hello Azure Data Factory aplikačního modelu služby.</span><span class="sxs-lookup"><span data-stu-id="9211c-104">This article explains hello scheduling and execution aspects of hello Azure Data Factory application model.</span></span> <span data-ttu-id="9211c-105">Tento článek předpokládá, že chápete základní informace o objektu pro vytváření dat aplikací modelu koncepty, včetně aktivit, kanálů, propojené služby a datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-105">This article assumes that you understand basics of Data Factory application model concepts, including activity, pipelines, linked services, and datasets.</span></span> <span data-ttu-id="9211c-106">Základní koncepty objektu pro vytváření dat Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="9211c-106">For basic concepts of Azure Data Factory, see hello following articles:</span></span>

* [<span data-ttu-id="9211c-107">Úvod tooData objekt pro vytváření</span><span class="sxs-lookup"><span data-stu-id="9211c-107">Introduction tooData Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="9211c-108">Kanály</span><span class="sxs-lookup"><span data-stu-id="9211c-108">Pipelines</span></span>](data-factory-create-pipelines.md)
* [<span data-ttu-id="9211c-109">Datové sady</span><span class="sxs-lookup"><span data-stu-id="9211c-109">Datasets</span></span>](data-factory-create-datasets.md) 

## <a name="start-and-end-times-of-pipeline"></a><span data-ttu-id="9211c-110">Počáteční a koncový čas kanálu</span><span class="sxs-lookup"><span data-stu-id="9211c-110">Start and end times of pipeline</span></span>
<span data-ttu-id="9211c-111">Kanál je aktivní jenom mezi jeho **spustit** čas a **end** čas.</span><span class="sxs-lookup"><span data-stu-id="9211c-111">A pipeline is active only between its **start** time and **end** time.</span></span> <span data-ttu-id="9211c-112">Nebude provedena před časem zahájení hello nebo po hello koncový čas.</span><span class="sxs-lookup"><span data-stu-id="9211c-112">It is not executed before hello start time or after hello end time.</span></span> <span data-ttu-id="9211c-113">Pokud hello kanálu pozastaví, nebude provedena bez ohledu na jeho počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="9211c-113">If hello pipeline is paused, it is not executed irrespective of its start and end time.</span></span> <span data-ttu-id="9211c-114">Pro toorun kanálu by nemělo být pozastavena.</span><span class="sxs-lookup"><span data-stu-id="9211c-114">For a pipeline toorun, it should not be paused.</span></span> <span data-ttu-id="9211c-115">Zjistíte, tato nastavení (zahájení, ukončení, pozastavena) v definici kanálu hello:</span><span class="sxs-lookup"><span data-stu-id="9211c-115">You find these settings (start, end, paused) in hello pipeline definition:</span></span> 

```json
"start": "2017-04-01T08:00:00Z",
"end": "2017-04-01T11:00:00Z"
"isPaused": false
```

<span data-ttu-id="9211c-116">Další informace najdete v těchto vlastností [vytvořit kanály](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9211c-116">For more information these properties, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> 


## <a name="specify-schedule-for-an-activity"></a><span data-ttu-id="9211c-117">Zadejte plán pro aktivitu</span><span class="sxs-lookup"><span data-stu-id="9211c-117">Specify schedule for an activity</span></span>
<span data-ttu-id="9211c-118">Není hello kanál, který se spustí.</span><span class="sxs-lookup"><span data-stu-id="9211c-118">It is not hello pipeline that is executed.</span></span> <span data-ttu-id="9211c-119">Je hello aktivity v kanálu hello, které jsou spouštěny v hello celkové kontextu hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="9211c-119">It is hello activities in hello pipeline that are executed in hello overall context of hello pipeline.</span></span> <span data-ttu-id="9211c-120">Můžete zadat plán opakování pro aktivitu pomocí hello **Plánovač** část JSON aktivity.</span><span class="sxs-lookup"><span data-stu-id="9211c-120">You can specify a recurring schedule for an activity by using hello **scheduler** section of activity JSON.</span></span> <span data-ttu-id="9211c-121">Například můžete naplánovat aktivitu toorun každou hodinu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9211c-121">For example, you can schedule an activity toorun hourly as follows:</span></span>  

```json
"scheduler": {
    "frequency": "Hour",
    "interval": 1
},
```

<span data-ttu-id="9211c-122">Jak ukazuje následující diagram hello, zadat že plán pro aktivitu vytvoří řadu přeskakující windows se v hello kanálu spuštění a ukončení.</span><span class="sxs-lookup"><span data-stu-id="9211c-122">As shown in hello following diagram, specifying a schedule for an activity creates a series of tumbling windows with in hello pipeline start and end times.</span></span> <span data-ttu-id="9211c-123">Přeskakující windows jsou řadu pevné velikosti nepřekrývají, souvislý časové intervaly.</span><span class="sxs-lookup"><span data-stu-id="9211c-123">Tumbling windows are a series of fixed-size non-overlapping, contiguous time intervals.</span></span> <span data-ttu-id="9211c-124">Tyto logické přeskakující windows pro aktivitu se nazývají **aktivity windows**.</span><span class="sxs-lookup"><span data-stu-id="9211c-124">These logical tumbling windows for an activity are called **activity windows**.</span></span>

![Příklad aktivitu plánovače](media/data-factory-scheduling-and-execution/scheduler-example.png)

<span data-ttu-id="9211c-126">Hello **Plánovač** vlastnost pro aktivitu je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="9211c-126">hello **scheduler** property for an activity is optional.</span></span> <span data-ttu-id="9211c-127">Pokud určíte tuto vlastnost, musí se shodovat hello cadence, který určíte v definici hello výstupní datovou sadu aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-127">If you do specify this property, it must match hello cadence you specify in hello definition of output dataset for hello activity.</span></span> <span data-ttu-id="9211c-128">Výstupní datové sady v současné době je, jaké jednotky hello plán.</span><span class="sxs-lookup"><span data-stu-id="9211c-128">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="9211c-129">Proto je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="9211c-129">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> 

## <a name="specify-schedule-for-a-dataset"></a><span data-ttu-id="9211c-130">Zadejte plán pro datové sady</span><span class="sxs-lookup"><span data-stu-id="9211c-130">Specify schedule for a dataset</span></span>
<span data-ttu-id="9211c-131">Aktivita v kanálu pro vytváření dat může trvat vstup nula nebo více **datové sady** a vytvoří výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-131">An activity in a Data Factory pipeline can take zero or more input **datasets** and produce one or more output datasets.</span></span> <span data-ttu-id="9211c-132">Pro aktivitu, můžete zadat hello cadence, na které hello je k dispozici vstupní data nebo data výstup hello je vytvořená pomocí hello **dostupnosti** část v definicích hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="9211c-132">For an activity, you can specify hello cadence at which hello input data is available or hello output data is produced by using hello **availability** section in hello dataset definitions.</span></span> 

<span data-ttu-id="9211c-133">**Frekvence** v hello **dostupnosti** část určuje hello časovou jednotku.</span><span class="sxs-lookup"><span data-stu-id="9211c-133">**Frequency** in hello **availability** section specifies hello time unit.</span></span> <span data-ttu-id="9211c-134">Hello povolené jsou hodnoty frekvence: minutu, hodinu, den, týden a měsíce.</span><span class="sxs-lookup"><span data-stu-id="9211c-134">hello allowed values for frequency are: Minute, Hour, Day, Week, and Month.</span></span> <span data-ttu-id="9211c-135">Hello **interval** vlastnost v části dostupnosti hello určuje multiplikátor pro četnost.</span><span class="sxs-lookup"><span data-stu-id="9211c-135">hello **interval** property in hello availability section specifies a multiplier for frequency.</span></span> <span data-ttu-id="9211c-136">Například: Pokud je nastavena hello frekvence tooDay a interval se nastavuje too1 pro datovou sadu výstupů, hello výstupní data se vytvoří každý den.</span><span class="sxs-lookup"><span data-stu-id="9211c-136">For example: if hello frequency is set tooDay and interval is set too1 for an output dataset, hello output data is produced daily.</span></span> <span data-ttu-id="9211c-137">Pokud zadáte četnost hello jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15.</span><span class="sxs-lookup"><span data-stu-id="9211c-137">If you specify hello frequency as minute, we recommend that you set hello interval toono less than 15.</span></span> 

<span data-ttu-id="9211c-138">V následujícím příkladu hello, hello vstupních dat je k dispozici každou hodinu a výstupní data hello se vytvářejí každou hodinu (`"frequency": "Hour", "interval": 1`).</span><span class="sxs-lookup"><span data-stu-id="9211c-138">In hello following example, hello input data is available hourly and hello output data is produced hourly (`"frequency": "Hour", "interval": 1`).</span></span> 

<span data-ttu-id="9211c-139">**Vstupní datové sady:**</span><span class="sxs-lookup"><span data-stu-id="9211c-139">**Input dataset:**</span></span> 

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


<span data-ttu-id="9211c-140">**Výstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="9211c-140">**Output dataset**</span></span>

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

<span data-ttu-id="9211c-141">V současné době **výstupní datovou sadu jednotky hello plán**.</span><span class="sxs-lookup"><span data-stu-id="9211c-141">Currently, **output dataset drives hello schedule**.</span></span> <span data-ttu-id="9211c-142">Jinými slovy zadaný pro výstupní datovou sadu hello plán hello je použité toorun aktivitu za běhu.</span><span class="sxs-lookup"><span data-stu-id="9211c-142">In other words, hello schedule specified for hello output dataset is used toorun an activity at runtime.</span></span> <span data-ttu-id="9211c-143">Proto je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="9211c-143">Therefore, you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="9211c-144">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-144">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 

<span data-ttu-id="9211c-145">V následující hello kanálu definice hello **Plánovač** vlastnost je použité toospecify plán aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-145">In hello following pipeline definition, hello **scheduler** property is used toospecify schedule for hello activity.</span></span> <span data-ttu-id="9211c-146">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="9211c-146">This property is optional.</span></span> <span data-ttu-id="9211c-147">V současné době hello plán aktivity hello musí odpovídat hello plán zadaný pro hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="9211c-147">Currently, hello schedule for hello activity must match hello schedule specified for hello output dataset.</span></span>
 
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

<span data-ttu-id="9211c-148">V tomto příkladu hello běh aktivit každou hodinu mezi hello spuštění a ukončení hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="9211c-148">In this example, hello activity runs hourly between hello start and end times of hello pipeline.</span></span> <span data-ttu-id="9211c-149">Hello výstupních dat každou hodinu vytváří pro windows 3 hodiny (8: 00 - 9 AM, 9: 00 – 10: 00 a 10 AM - 11 AM).</span><span class="sxs-lookup"><span data-stu-id="9211c-149">hello output data is produced hourly for three-hour windows (8 AM - 9 AM, 9 AM - 10 AM, and 10 AM - 11 AM).</span></span> 

<span data-ttu-id="9211c-150">Je volána jednotlivých jednotek data využívat nebo vyprodukované aktivitu spustit **datový řez**.</span><span class="sxs-lookup"><span data-stu-id="9211c-150">Each unit of data consumed or produced by an activity run is called a **data slice**.</span></span> <span data-ttu-id="9211c-151">Hello následující diagram ukazuje příklad aktivitu jednu vstupní datovou sadu a jednu výstupní datovou sadu:</span><span class="sxs-lookup"><span data-stu-id="9211c-151">hello following diagram shows an example of an activity with one input dataset and one output dataset:</span></span> 

![Dostupnost plánovače](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

<span data-ttu-id="9211c-153">Hello diagram znázorňuje hello hodinové datové řezy pro hello vstupní a výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="9211c-153">hello diagram shows hello hourly data slices for hello input and output dataset.</span></span> <span data-ttu-id="9211c-154">Hello diagram znázorňuje tři vstupní řezy, které jsou připravené ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="9211c-154">hello diagram shows three input slices that are ready for processing.</span></span> <span data-ttu-id="9211c-155">Aktivita 10 11 AM Hello je v průběhu vytváření hello 10 11 AM výstupní řez.</span><span class="sxs-lookup"><span data-stu-id="9211c-155">hello 10-11 AM activity is in progress, producing hello 10-11 AM output slice.</span></span> 

<span data-ttu-id="9211c-156">Dostanete hello časový interval přidružené hello aktuálního řezu v datové sadě hello JSON pomocí proměnných: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) a [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="9211c-156">You can access hello time interval associated with hello current slice in hello dataset JSON by using variables: [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) and [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="9211c-157">Podobně můžete přistupovat přidružené okno s aktivity pomocí hello WindowStart a WindowEnd hello časový interval.</span><span class="sxs-lookup"><span data-stu-id="9211c-157">Similarly, you can access hello time interval associated with an activity window by using hello WindowStart and WindowEnd.</span></span> <span data-ttu-id="9211c-158">plán Hello aktivity musí odpovídat hello plán hello výstupní datovou sadu aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-158">hello schedule of an activity must match hello schedule of hello output dataset for hello activity.</span></span> <span data-ttu-id="9211c-159">Proto hello SliceStart a SliceEnd hodnoty jsou hello stejné jako WindowStart a WindowEnd hodnoty v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="9211c-159">Therefore, hello SliceStart and SliceEnd values are hello same as WindowStart and WindowEnd values respectively.</span></span> <span data-ttu-id="9211c-160">Další informace o těchto proměnných najdete v tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables) články.</span><span class="sxs-lookup"><span data-stu-id="9211c-160">For more information on these variables, see [Data Factory functions and system variables](data-factory-functions-variables.md#data-factory-system-variables) articles.</span></span>  

<span data-ttu-id="9211c-161">Tyto proměnné můžete použít pro jiné účely vaše aktivity JSON.</span><span class="sxs-lookup"><span data-stu-id="9211c-161">You can use these variables for different purposes in your activity JSON.</span></span> <span data-ttu-id="9211c-162">Například můžete jejich data tooselect ze vstupní a výstupní datové sady reprezentující data časové řady (například: 8 AM too9 m).</span><span class="sxs-lookup"><span data-stu-id="9211c-162">For example, you can use them tooselect data from input and output datasets representing time series data (for example: 8 AM too9 AM).</span></span> <span data-ttu-id="9211c-163">Tento příklad také používá **WindowStart** a **WindowEnd** tooselect relevantní data pro aktivitu spustit a zkopírujte ho tooa objektů blob s hello odpovídající **folderPath**.</span><span class="sxs-lookup"><span data-stu-id="9211c-163">This example also uses **WindowStart** and **WindowEnd** tooselect relevant data for an activity run and copy it tooa blob with hello appropriate **folderPath**.</span></span> <span data-ttu-id="9211c-164">Hello **folderPath** je parametrizované toohave samostatnou složku pro každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="9211c-164">hello **folderPath** is parameterized toohave a separate folder for every hour.</span></span>  

<span data-ttu-id="9211c-165">V předchozím příkladu hello je zadaný pro vstupní a výstupní datové sady plán hello hello stejné (každou hodinu).</span><span class="sxs-lookup"><span data-stu-id="9211c-165">In hello preceding example, hello schedule specified for input and output datasets is hello same (hourly).</span></span> <span data-ttu-id="9211c-166">Pokud vstupní datové sady hello aktivity hello je k dispozici na jinou frekvenci, například každých 15 minut, hello aktivity, která vytváří tento výstupní datovou sadu stále spouští jednou za hodinu hello výstupní datová sada, jaké jednotky hello plán aktivity.</span><span class="sxs-lookup"><span data-stu-id="9211c-166">If hello input dataset for hello activity is available at a different frequency, say every 15 minutes, hello activity that produces this output dataset still runs once an hour as hello output dataset is what drives hello activity schedule.</span></span> <span data-ttu-id="9211c-167">Další informace najdete v tématu [modelu datové sady s jinou frekvencí](#model-datasets-with-different-frequencies).</span><span class="sxs-lookup"><span data-stu-id="9211c-167">For more information, see [Model datasets with different frequencies](#model-datasets-with-different-frequencies).</span></span>

## <a name="dataset-availability-and-policies"></a><span data-ttu-id="9211c-168">Dostupnost datové sady a zásady</span><span class="sxs-lookup"><span data-stu-id="9211c-168">Dataset availability and policies</span></span>
<span data-ttu-id="9211c-169">Jste viděli hello využití frekvence a intervalu vlastností v části dostupnosti hello definice datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-169">You have seen hello usage of frequency and interval properties in hello availability section of dataset definition.</span></span> <span data-ttu-id="9211c-170">Existuje několik vlastností, které ovlivňují hello plánování a provádění aktivity.</span><span class="sxs-lookup"><span data-stu-id="9211c-170">There are a few other properties that affect hello scheduling and execution of an activity.</span></span> 

### <a name="dataset-availability"></a><span data-ttu-id="9211c-171">Datovou sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="9211c-171">Dataset availability</span></span> 
<span data-ttu-id="9211c-172">Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **dostupnosti** části:</span><span class="sxs-lookup"><span data-stu-id="9211c-172">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="9211c-173">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9211c-173">Property</span></span> | <span data-ttu-id="9211c-174">Popis</span><span class="sxs-lookup"><span data-stu-id="9211c-174">Description</span></span> | <span data-ttu-id="9211c-175">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9211c-175">Required</span></span> | <span data-ttu-id="9211c-176">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9211c-176">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9211c-177">frequency</span><span class="sxs-lookup"><span data-stu-id="9211c-177">frequency</span></span> |<span data-ttu-id="9211c-178">Určuje časovou jednotku hello k produkci řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="9211c-178">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="9211c-179"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="9211c-179"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="9211c-180">Ano</span><span class="sxs-lookup"><span data-stu-id="9211c-180">Yes</span></span> |<span data-ttu-id="9211c-181">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9211c-181">NA</span></span> |
| <span data-ttu-id="9211c-182">interval</span><span class="sxs-lookup"><span data-stu-id="9211c-182">interval</span></span> |<span data-ttu-id="9211c-183">Určuje multiplikátor pro četnost</span><span class="sxs-lookup"><span data-stu-id="9211c-183">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="9211c-184">"Frekvence x interval" Určuje, jak často hello se vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="9211c-184">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="9211c-185">Pokud třeba hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="9211c-185">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="9211c-186"><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15</span><span class="sxs-lookup"><span data-stu-id="9211c-186"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="9211c-187">Ano</span><span class="sxs-lookup"><span data-stu-id="9211c-187">Yes</span></span> |<span data-ttu-id="9211c-188">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9211c-188">NA</span></span> |
| <span data-ttu-id="9211c-189">Styl</span><span class="sxs-lookup"><span data-stu-id="9211c-189">style</span></span> |<span data-ttu-id="9211c-190">Určuje, zda by měl být na hello počáteční nebo koncové intervalu hello předložen hello řez.</span><span class="sxs-lookup"><span data-stu-id="9211c-190">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="9211c-191">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="9211c-191">StartOfInterval</span></span></li><li><span data-ttu-id="9211c-192">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="9211c-192">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="9211c-193">Pokud je nastavena frekvence tooMonth a je nastaven styl tooEndOfInterval, hello se vytvářejí na hello poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="9211c-193">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="9211c-194">Pokud je styl hello nastavená tooStartOfInterval, hello se vytvářejí na hello první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="9211c-194">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="9211c-195">Pokud je nastavena frekvence tooDay a je nastaven styl tooEndOfInterval, hello se vytvářejí v hello poslední hodiny dne hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-195">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="9211c-196">Pokud je nastavena frekvence tooHour a je nastaven styl tooEndOfInterval, hello se vytvářejí na konci hello hello hodina.</span><span class="sxs-lookup"><span data-stu-id="9211c-196">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="9211c-197">Například pro řez dobu 13: 00 – 14: 00, hello se vytvářejí na 14: 00.</span><span class="sxs-lookup"><span data-stu-id="9211c-197">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="9211c-198">Ne</span><span class="sxs-lookup"><span data-stu-id="9211c-198">No</span></span> |<span data-ttu-id="9211c-199">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="9211c-199">EndOfInterval</span></span> |
| <span data-ttu-id="9211c-200">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="9211c-200">anchorDateTime</span></span> |<span data-ttu-id="9211c-201">Definuje hello absolutní pozici v čase, které používají scheduler toocompute datovou sadu řez hranic.</span><span class="sxs-lookup"><span data-stu-id="9211c-201">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="9211c-202"><b>Poznámka:</b>: Pokud má hello AnchorDateTime částí data, která jsou podrobnější než frekvence hello pak hello podrobnější části jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="9211c-202"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="9211c-203">Například, pokud hello <b>interval</b> je <b>každou hodinu</b> (frekvence: hodin a interval: 1) a hello <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b>, pak hello <b>minuty a sekundy</b> částí hello AnchorDateTime jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="9211c-203">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b>, then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="9211c-204">Ne</span><span class="sxs-lookup"><span data-stu-id="9211c-204">No</span></span> |<span data-ttu-id="9211c-205">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="9211c-205">01/01/0001</span></span> |
| <span data-ttu-id="9211c-206">Posun</span><span class="sxs-lookup"><span data-stu-id="9211c-206">offset</span></span> |<span data-ttu-id="9211c-207">Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty.</span><span class="sxs-lookup"><span data-stu-id="9211c-207">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="9211c-208"><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem hello je hello kombinaci shift.</span><span class="sxs-lookup"><span data-stu-id="9211c-208"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="9211c-209">Ne</span><span class="sxs-lookup"><span data-stu-id="9211c-209">No</span></span> |<span data-ttu-id="9211c-210">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9211c-210">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="9211c-211">Příklad posunutí</span><span class="sxs-lookup"><span data-stu-id="9211c-211">offset example</span></span>
<span data-ttu-id="9211c-212">Ve výchozím nastavení, každý den (`"frequency": "Day", "interval": 1`) řezy spuštění na čas UTC 12: 00 (půlnoc).</span><span class="sxs-lookup"><span data-stu-id="9211c-212">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM UTC time (midnight).</span></span> <span data-ttu-id="9211c-213">Pokud chcete místo toho hello počáteční čas čas UTC toobe 6: 00, nastavte hello posun, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="9211c-213">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="9211c-214">Příklad anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="9211c-214">anchorDateTime example</span></span>
<span data-ttu-id="9211c-215">V následujícím příkladu hello hello sada vytváří jednou za 23 hodin.</span><span class="sxs-lookup"><span data-stu-id="9211c-215">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="9211c-216">Hello první řez spustí v době hello určeného hello anchorDateTime, který je nastaven příliš`2017-04-19T08:00:00` (Světový čas UTC).</span><span class="sxs-lookup"><span data-stu-id="9211c-216">hello first slice starts at hello time specified by hello anchorDateTime, which is set too`2017-04-19T08:00:00` (UTC time).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="9211c-217">Posun nebo styl příklad</span><span class="sxs-lookup"><span data-stu-id="9211c-217">offset/style Example</span></span>
<span data-ttu-id="9211c-218">Hello následující datové sady je měsíční datová sada a vytváří 3rd v každém měsíci v 8:00 AM (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="9211c-218">hello following dataset is a monthly dataset and is produced on 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

### <a name="dataset-policy"></a><span data-ttu-id="9211c-219">Datovou sadu zásad</span><span class="sxs-lookup"><span data-stu-id="9211c-219">Dataset policy</span></span>
<span data-ttu-id="9211c-220">Datovou sadu, může mít definované zásady ověřování, která určuje, jak může být ověřen hello data generována řez provádění předtím, než je připraven ke spotřebování.</span><span class="sxs-lookup"><span data-stu-id="9211c-220">A dataset can have a validation policy defined that specifies how hello data generated by a slice execution can be validated before it is ready for consumption.</span></span> <span data-ttu-id="9211c-221">V takových případech po dokončení provádění hello řez hello výstupní řez je změnit stav příliš**čekání** s substatus z **ověření**.</span><span class="sxs-lookup"><span data-stu-id="9211c-221">In such cases, after hello slice has finished execution, hello output slice status is changed too**Waiting** with a substatus of **Validation**.</span></span> <span data-ttu-id="9211c-222">Po ověření hello řezy jsou stav řezu hello změní příliš**připraven**.</span><span class="sxs-lookup"><span data-stu-id="9211c-222">After hello slices are validated, hello slice status changes too**Ready**.</span></span> <span data-ttu-id="9211c-223">Pokud datový řez pochází, ale neprošel ověřením platnosti hello, nebudou zpracovány spuštění aktivity pro příjem dat datové řezy, které závisí na tento řez.</span><span class="sxs-lookup"><span data-stu-id="9211c-223">If a data slice has been produced but did not pass hello validation, activity runs for downstream slices that depend on this slice are not processed.</span></span> <span data-ttu-id="9211c-224">[Monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) zahrnuje hello různé stavy datové řezy ve službě Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9211c-224">[Monitor and manage pipelines](data-factory-monitor-manage-pipelines.md) covers hello various states of data slices in Data Factory.</span></span>

<span data-ttu-id="9211c-225">Hello **zásad** oddíl v definici datové sady definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit.</span><span class="sxs-lookup"><span data-stu-id="9211c-225">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <span data-ttu-id="9211c-226">Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **zásad** části:</span><span class="sxs-lookup"><span data-stu-id="9211c-226">hello following table describes properties you can use in hello **policy** section:</span></span>

| <span data-ttu-id="9211c-227">Název zásady</span><span class="sxs-lookup"><span data-stu-id="9211c-227">Policy Name</span></span> | <span data-ttu-id="9211c-228">Popis</span><span class="sxs-lookup"><span data-stu-id="9211c-228">Description</span></span> | <span data-ttu-id="9211c-229">Použít příliš</span><span class="sxs-lookup"><span data-stu-id="9211c-229">Applied too</span></span>| <span data-ttu-id="9211c-230">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="9211c-230">Required</span></span> | <span data-ttu-id="9211c-231">Výchozí</span><span class="sxs-lookup"><span data-stu-id="9211c-231">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="9211c-232">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="9211c-232">minimumSizeMB</span></span> | <span data-ttu-id="9211c-233">Ověří, zda hello data ve **objektů blob v Azure** hello splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="9211c-233">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="9211c-234">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="9211c-234">Azure Blob</span></span> |<span data-ttu-id="9211c-235">Ne</span><span class="sxs-lookup"><span data-stu-id="9211c-235">No</span></span> |<span data-ttu-id="9211c-236">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9211c-236">NA</span></span> |
| <span data-ttu-id="9211c-237">minimumRows</span><span class="sxs-lookup"><span data-stu-id="9211c-237">minimumRows</span></span> | <span data-ttu-id="9211c-238">Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="9211c-238">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="9211c-239">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="9211c-239">Azure SQL Database</span></span></li><li><span data-ttu-id="9211c-240">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="9211c-240">Azure Table</span></span></li></ul> |<span data-ttu-id="9211c-241">Ne</span><span class="sxs-lookup"><span data-stu-id="9211c-241">No</span></span> |<span data-ttu-id="9211c-242">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="9211c-242">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="9211c-243">Příklady</span><span class="sxs-lookup"><span data-stu-id="9211c-243">Examples</span></span>
<span data-ttu-id="9211c-244">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="9211c-244">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="9211c-245">**minimumRows**</span><span class="sxs-lookup"><span data-stu-id="9211c-245">**minimumRows**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

<span data-ttu-id="9211c-246">Další informace o těchto vlastnostech a příklady naleznete v tématu [vytvoření datových sad](data-factory-create-datasets.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9211c-246">For more information about these properties and examples, see [Create datasets](data-factory-create-datasets.md) article.</span></span> 

## <a name="activity-policies"></a><span data-ttu-id="9211c-247">Zásady aktivit</span><span class="sxs-lookup"><span data-stu-id="9211c-247">Activity policies</span></span>
<span data-ttu-id="9211c-248">Zásady ovlivňují chování hello běhu aktivity, konkrétně v případě, že je zpracování řezu hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="9211c-248">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="9211c-249">Hello následující tabulka poskytuje podrobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-249">hello following table provides hello details.</span></span>

| <span data-ttu-id="9211c-250">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="9211c-250">Property</span></span> | <span data-ttu-id="9211c-251">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="9211c-251">Permitted values</span></span> | <span data-ttu-id="9211c-252">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="9211c-252">Default Value</span></span> | <span data-ttu-id="9211c-253">Popis</span><span class="sxs-lookup"><span data-stu-id="9211c-253">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9211c-254">Souběžnosti</span><span class="sxs-lookup"><span data-stu-id="9211c-254">concurrency</span></span> |<span data-ttu-id="9211c-255">Integer</span><span class="sxs-lookup"><span data-stu-id="9211c-255">Integer</span></span> <br/><br/><span data-ttu-id="9211c-256">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="9211c-256">Max value: 10</span></span> |<span data-ttu-id="9211c-257">1</span><span class="sxs-lookup"><span data-stu-id="9211c-257">1</span></span> |<span data-ttu-id="9211c-258">Počet souběžných spuštění aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-258">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="9211c-259">Určuje hello počet spuštěních paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="9211c-259">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="9211c-260">Například pokud aktivita vyžaduje toogo prostřednictvím velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-260">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="9211c-261">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="9211c-261">executionPriorityOrder</span></span> |<span data-ttu-id="9211c-262">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="9211c-262">NewestFirst</span></span><br/><br/><span data-ttu-id="9211c-263">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="9211c-263">OldestFirst</span></span> |<span data-ttu-id="9211c-264">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="9211c-264">OldestFirst</span></span> |<span data-ttu-id="9211c-265">Určuje pořadí hello datové řezy, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="9211c-265">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="9211c-266">Pokud máte 2 řezy (jeden situaci ve 4 a další v 17: 00) a jsou obě čekající na zpracování.</span><span class="sxs-lookup"><span data-stu-id="9211c-266">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="9211c-267">Pokud jste nastavili hello executionPriorityOrder toobe NewestFirst, hello řez v 17: 00, je zpracován jako první.</span><span class="sxs-lookup"><span data-stu-id="9211c-267">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="9211c-268">Podobně pokud nastavíte hello executionPriorityORder toobe OldestFIrst, pak hello ve 4 zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="9211c-268">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="9211c-269">retry</span><span class="sxs-lookup"><span data-stu-id="9211c-269">retry</span></span> |<span data-ttu-id="9211c-270">Integer</span><span class="sxs-lookup"><span data-stu-id="9211c-270">Integer</span></span><br/><br/><span data-ttu-id="9211c-271">Maximální hodnota může být 10</span><span class="sxs-lookup"><span data-stu-id="9211c-271">Max value can be 10</span></span> |<span data-ttu-id="9211c-272">0</span><span class="sxs-lookup"><span data-stu-id="9211c-272">0</span></span> |<span data-ttu-id="9211c-273">Počet opakovaných pokusů před hello zpracování dat pro hello řez je označena jako selhání.</span><span class="sxs-lookup"><span data-stu-id="9211c-273">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="9211c-274">Provedení aktivity pro datový řez je opakovat až toohello zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="9211c-274">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="9211c-275">co nejdříve po selhání hello se provádí Hello opakování.</span><span class="sxs-lookup"><span data-stu-id="9211c-275">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="9211c-276">timeout</span><span class="sxs-lookup"><span data-stu-id="9211c-276">timeout</span></span> |<span data-ttu-id="9211c-277">Časový interval</span><span class="sxs-lookup"><span data-stu-id="9211c-277">TimeSpan</span></span> |<span data-ttu-id="9211c-278">00:00:00</span><span class="sxs-lookup"><span data-stu-id="9211c-278">00:00:00</span></span> |<span data-ttu-id="9211c-279">Časový limit aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-279">Timeout for hello activity.</span></span> <span data-ttu-id="9211c-280">Příklad: 00:10:00 (znamená časový limit 10 minut)</span><span class="sxs-lookup"><span data-stu-id="9211c-280">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="9211c-281">Pokud hodnota není zadána nebo je 0, vypršení časového limitu hello je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="9211c-281">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="9211c-282">Pokud doba zpracování dat hello na řez překročí hodnota časového limitu hello, se zruší a hello systém pokusí tooretry hello zpracování.</span><span class="sxs-lookup"><span data-stu-id="9211c-282">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="9211c-283">Hello počet opakovaných pokusů závisí na vlastnosti opakování hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-283">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="9211c-284">Když dojde k vypršení časového limitu, je nastaven stav hello tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="9211c-284">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="9211c-285">Zpoždění</span><span class="sxs-lookup"><span data-stu-id="9211c-285">delay</span></span> |<span data-ttu-id="9211c-286">Časový interval</span><span class="sxs-lookup"><span data-stu-id="9211c-286">TimeSpan</span></span> |<span data-ttu-id="9211c-287">00:00:00</span><span class="sxs-lookup"><span data-stu-id="9211c-287">00:00:00</span></span> |<span data-ttu-id="9211c-288">Zadejte zpoždění hello před spuštěním zpracování dat hello řez.</span><span class="sxs-lookup"><span data-stu-id="9211c-288">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="9211c-289">Hello provádění aktivity pro datový řez je spuštěn v minulosti hello očekávaný čas provádění po hello zpoždění.</span><span class="sxs-lookup"><span data-stu-id="9211c-289">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="9211c-290">Příklad: 00:10:00 (znamená zpoždění 10 minut)</span><span class="sxs-lookup"><span data-stu-id="9211c-290">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="9211c-291">opakování po delší době</span><span class="sxs-lookup"><span data-stu-id="9211c-291">longRetry</span></span> |<span data-ttu-id="9211c-292">Integer</span><span class="sxs-lookup"><span data-stu-id="9211c-292">Integer</span></span><br/><br/><span data-ttu-id="9211c-293">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="9211c-293">Max value: 10</span></span> |<span data-ttu-id="9211c-294">1</span><span class="sxs-lookup"><span data-stu-id="9211c-294">1</span></span> |<span data-ttu-id="9211c-295">Hello počet dlouho opakování pokusů, než hello řez spuštění se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9211c-295">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="9211c-296">pokusy o opakování po delší době jsou rozmístěny ve longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="9211c-296">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="9211c-297">Takže pokud budete potřebovat toospecify doba mezi pokusy o opakování, použijte opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="9211c-297">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="9211c-298">Pokud jsou zadané opakování a opakování po delší době, jednotlivé pokusy o opakování po delší době zahrnuje opakovaných pokusů a je hello maximální počet pokusů o opakování * opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="9211c-298">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="9211c-299">Například, pokud bychom měli hello následující nastavení v zásadě hello aktivity:</span><span class="sxs-lookup"><span data-stu-id="9211c-299">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="9211c-300">Opakujte: 3</span><span class="sxs-lookup"><span data-stu-id="9211c-300">Retry: 3</span></span><br/><span data-ttu-id="9211c-301">opakování po delší době: 2</span><span class="sxs-lookup"><span data-stu-id="9211c-301">longRetry: 2</span></span><br/><span data-ttu-id="9211c-302">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="9211c-302">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="9211c-303">Předpokládá se jenom jeden řez tooexecute (stav Čeká) a provedení aktivity hello pokaždé, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="9211c-303">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="9211c-304">Nejdřív by 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="9211c-304">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="9211c-305">Po každém pokusu o stav řezu hello bude opakovat.</span><span class="sxs-lookup"><span data-stu-id="9211c-305">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="9211c-306">Po první 3 pokusy jsou přes, bude stav řezu hello opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="9211c-306">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="9211c-307">Po hodině (který je na longRetryInteval hodnota) bude další sadu 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="9211c-307">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="9211c-308">Od tohoto stavu řezu hello by se nezdařilo a by se pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="9211c-308">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="9211c-309">Proto celkové 6 pokusy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="9211c-309">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="9211c-310">Pokud žádné spuštění úspěšné, stav řezu hello by připravené a jsou pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="9211c-310">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="9211c-311">opakování po delší době je možné použít situace, kdy závislé data dorazí v časech Nedeterministický nebo hello celém prostředí je v nestabilním stavu v rámci které zpracování dat dojde.</span><span class="sxs-lookup"><span data-stu-id="9211c-311">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="9211c-312">V takových případech nemusí být úspěšná při provádění opakování, jedna po druhé a tak v intervalech čas má za následek hello potřeby výstup.</span><span class="sxs-lookup"><span data-stu-id="9211c-312">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="9211c-313">Word varování: nenastavujte vysoké hodnoty pro opakování po delší době nebo longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="9211c-313">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="9211c-314">Vyšší hodnoty obvykle implikují dalších systémových otázek.</span><span class="sxs-lookup"><span data-stu-id="9211c-314">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="9211c-315">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="9211c-315">longRetryInterval</span></span> |<span data-ttu-id="9211c-316">Časový interval</span><span class="sxs-lookup"><span data-stu-id="9211c-316">TimeSpan</span></span> |<span data-ttu-id="9211c-317">00:00:00</span><span class="sxs-lookup"><span data-stu-id="9211c-317">00:00:00</span></span> |<span data-ttu-id="9211c-318">Hello prodleva mezi pokusy o opakování dlouho</span><span class="sxs-lookup"><span data-stu-id="9211c-318">hello delay between long retry attempts</span></span> |

<span data-ttu-id="9211c-319">Další informace najdete v tématu [kanály](data-factory-create-pipelines.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9211c-319">For more information, see [Pipelines](data-factory-create-pipelines.md) article.</span></span> 

## <a name="parallel-processing-of-data-slices"></a><span data-ttu-id="9211c-320">Paralelní zpracování datové řezy</span><span class="sxs-lookup"><span data-stu-id="9211c-320">Parallel processing of data slices</span></span>
<span data-ttu-id="9211c-321">Hello počáteční datum pro kanál hello můžete nastavit v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-321">You can set hello start date for hello pipeline in hello past.</span></span> <span data-ttu-id="9211c-322">Pokud tak učiníte, Data Factory automaticky vypočítá všechny datové řezy v posledních hello (back výplněmi) a zahájí zpracování je.</span><span class="sxs-lookup"><span data-stu-id="9211c-322">When you do so, Data Factory automatically calculates (back fills) all data slices in hello past and begins processing them.</span></span> <span data-ttu-id="9211c-323">Například: Pokud vytvoření kanálu s počátečním datem 2017-04-01 a hello aktuální datum následuje 2017-04-10.</span><span class="sxs-lookup"><span data-stu-id="9211c-323">For example: if you create a pipeline with start date 2017-04-01 and hello current date is 2017-04-10.</span></span> <span data-ttu-id="9211c-324">Pokud výstup hello cadence Dobrý den, že se datová sada denně a potom spustí služby Data Factory zpracování všech řezech hello z 2017-04-01 too2017-04-09 okamžitě protože hello počáteční datum je v minulosti hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-324">If hello cadence of hello output dataset is daily, then Data Factory starts processing all hello slices from 2017-04-01 too2017-04-09 immediately because hello start date is in hello past.</span></span> <span data-ttu-id="9211c-325">Hello z 2017-04-10 není zpracování řezu ještě protože hello vlastnost stylu v části dostupnosti hello hodnotu EndOfInterval ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9211c-325">hello slice from 2017-04-10 is not processed yet because hello value of style property in hello availability section is EndOfInterval by default.</span></span> <span data-ttu-id="9211c-326">Hello nejstarší zpracování řezu se nejprve jako výchozí hello executionPriorityOrder hodnotu OldestFirst.</span><span class="sxs-lookup"><span data-stu-id="9211c-326">hello oldest slice is processed first as hello default value of executionPriorityOrder is OldestFirst.</span></span> <span data-ttu-id="9211c-327">Popis vlastnost stylu hello najdete v tématu [datovou sadu dostupnosti](#dataset-availability) části.</span><span class="sxs-lookup"><span data-stu-id="9211c-327">For a description of hello style property, see [dataset availability](#dataset-availability) section.</span></span> <span data-ttu-id="9211c-328">Popis části executionPriorityOrder hello najdete v tématu hello [zásady aktivit](#activity-policies) části.</span><span class="sxs-lookup"><span data-stu-id="9211c-328">For a description of hello executionPriorityOrder section, see hello [activity policies](#activity-policies) section.</span></span> 

<span data-ttu-id="9211c-329">Můžete nakonfigurovat toobe řezy vyplněno zpět data souběžně zpracovávaných nastavení hello **souběžnosti** vlastnost hello **zásad** část JSON hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="9211c-329">You can configure back-filled data slices toobe processed in parallel by setting hello **concurrency** property in hello **policy** section of hello activity JSON.</span></span> <span data-ttu-id="9211c-330">Tato vlastnost určuje hello počet spuštěních paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="9211c-330">This property determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="9211c-331">Hello výchozí hodnota pro vlastnost souběžnosti hello je 1.</span><span class="sxs-lookup"><span data-stu-id="9211c-331">hello default value for hello concurrency property is 1.</span></span> <span data-ttu-id="9211c-332">Proto jeden zpracování řezu se současně ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="9211c-332">Therefore, one slice is processed at a time by default.</span></span> <span data-ttu-id="9211c-333">Hello maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="9211c-333">hello maximum value is 10.</span></span> <span data-ttu-id="9211c-334">Když kanál potřebuje toogo prostřednictvím velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-334">When a pipeline needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> 

## <a name="rerun-a-failed-data-slice"></a><span data-ttu-id="9211c-335">Opětovné spuštění neúspěšné datový řez</span><span class="sxs-lookup"><span data-stu-id="9211c-335">Rerun a failed data slice</span></span>
<span data-ttu-id="9211c-336">Když dojde k chybě při zpracování dat řezu, můžete zjistit proč pomocí oken webu Azure portal nebo monitorování a správě aplikace hello zpracování řezu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9211c-336">When an error occurs while processing a data slice, you can find out why hello processing of a slice failed by using Azure portal blades or Monitor and Manage App.</span></span> <span data-ttu-id="9211c-337">V tématu [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9211c-337">See [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md) for details.</span></span>

<span data-ttu-id="9211c-338">Vezměte v úvahu následující příklad, který ukazuje dvě aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-338">Consider hello following example, which shows two activities.</span></span> <span data-ttu-id="9211c-339">Aktivity "activity1" a aktivitu 2.</span><span class="sxs-lookup"><span data-stu-id="9211c-339">Activity1 and Activity 2.</span></span> <span data-ttu-id="9211c-340">Aktivity "activity1" využívá řez Dataset1 a produkuje řez Dataset2, které se spotřebovávají "activity2" tooproduce řez hello poslední datovou sadu jako vstup.</span><span class="sxs-lookup"><span data-stu-id="9211c-340">Activity1 consumes a slice of Dataset1 and produces a slice of Dataset2, which is consumed as an input by Activity2 tooproduce a slice of hello Final Dataset.</span></span>

![Řez se nezdařilo](./media/data-factory-scheduling-and-execution/failed-slice.png)

<span data-ttu-id="9211c-342">Hello diagram ukazuje, že mimo tři poslední řezů, že došlo 9 10 AM řez vytváření hello selhání pro Dataset2.</span><span class="sxs-lookup"><span data-stu-id="9211c-342">hello diagram shows that out of three recent slices, there was a failure producing hello 9-10 AM slice for Dataset2.</span></span> <span data-ttu-id="9211c-343">Objekt pro vytváření dat automaticky sleduje závislostí pro datovou sadu řady čas hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-343">Data Factory automatically tracks dependency for hello time series dataset.</span></span> <span data-ttu-id="9211c-344">V důsledku toho se nespustí hello aktivity při spuštění pro příjem dat řez hello AM 9-10.</span><span class="sxs-lookup"><span data-stu-id="9211c-344">As a result, it does not start hello activity run for hello 9-10 AM downstream slice.</span></span>

<span data-ttu-id="9211c-345">Datový objekt pro vytváření monitorování a nástroje pro správu umožní toodrill do diagnostickým protokolům hello hello selhání řez tooeasily najít hello kořenové způsobit hello problému a opravte ho.</span><span class="sxs-lookup"><span data-stu-id="9211c-345">Data Factory monitoring and management tools allow you toodrill into hello diagnostic logs for hello failed slice tooeasily find hello root cause for hello issue and fix it.</span></span> <span data-ttu-id="9211c-346">Po opravení problému hello můžete snadno začít hello aktivity při spuštění se nezdařilo řez tooproduce hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-346">After you have fixed hello issue, you can easily start hello activity run tooproduce hello failed slice.</span></span> <span data-ttu-id="9211c-347">Další informace o tom, toorerun a pochopit přechodů mezi stavy pro datové řezy, najdete v části [monitorování a Správa kanálů pomocí oken webu Azure portal](data-factory-monitor-manage-pipelines.md) nebo [monitorování a správu aplikace](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="9211c-347">For more information on how toorerun and understand state transitions for data slices, see [Monitoring and managing pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) or [Monitoring and Management app](data-factory-monitor-manage-app.md).</span></span>

<span data-ttu-id="9211c-348">Po znovu hello 9 10 AM slice pro **Dataset2**, spustí hello spustit pro hello 9 10 AM závislé řez na hello poslední datovou sadu služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9211c-348">After you rerun hello 9-10 AM slice for **Dataset2**, Data Factory starts hello run for hello 9-10 AM dependent slice on hello final dataset.</span></span>

![Opětovné spuštění neúspěšné řez](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="multiple-activities-in-a-pipeline"></a><span data-ttu-id="9211c-350">Více aktivit v kanálu</span><span class="sxs-lookup"><span data-stu-id="9211c-350">Multiple activities in a pipeline</span></span>
<span data-ttu-id="9211c-351">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="9211c-351">You can have more than one activity in a pipeline.</span></span> <span data-ttu-id="9211c-352">Pokud máte více aktivit v kanálu a hello výstup aktivity není vstup jinou aktivitu, může paralelně spustit hello aktivity, pokud připravení řezy vstupní data aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-352">If you have multiple activities in a pipeline and hello output of an activity is not an input of another activity, hello activities may run in parallel if input data slices for hello activities are ready.</span></span>

<span data-ttu-id="9211c-353">Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="9211c-353">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="9211c-354">Hello aktivity mohou být v hello stejné kanálu nebo v jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="9211c-354">hello activities can be in hello same pipeline or in different pipelines.</span></span> <span data-ttu-id="9211c-355">Druhá aktivita Hello spustí jenom v případě, že hello nejdřív jednu příště úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="9211c-355">hello second activity executes only when hello first one finishes successfully.</span></span>

<span data-ttu-id="9211c-356">Představte si třeba hello následující případ, kdy kanálu má dvě aktivity:</span><span class="sxs-lookup"><span data-stu-id="9211c-356">For example, consider hello following case where a pipeline has two activities:</span></span>

1. <span data-ttu-id="9211c-357">A1 aktivity, která vyžaduje externí vstupní datové sady D1 a vytvoří výstupní datovou sadu D2.</span><span class="sxs-lookup"><span data-stu-id="9211c-357">Activity A1 that requires external input dataset D1, and produces output dataset D2.</span></span>
2. <span data-ttu-id="9211c-358">A2 aktivity, který vyžaduje vstup z datové sady D2 a vytvoří výstupní datovou sadu D3.</span><span class="sxs-lookup"><span data-stu-id="9211c-358">Activity A2 that requires input from dataset D2, and produces output dataset D3.</span></span>

<span data-ttu-id="9211c-359">Tento scénář, aktivity A1 a A2 jsou v hello stejné kanálu.</span><span class="sxs-lookup"><span data-stu-id="9211c-359">In this scenario, activities A1 and A2 are in hello same pipeline.</span></span> <span data-ttu-id="9211c-360">Hello aktivity A1 spustí hello externích dat je k dispozici a je dosaženo hello Naplánované dostupnosti frekvence.</span><span class="sxs-lookup"><span data-stu-id="9211c-360">hello activity A1 runs when hello external data is available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="9211c-361">Hello aktivity A2 spustí hello naplánované řezy z D2 k dispozici a hello naplánovaná dostupnost frekvence je dosaženo.</span><span class="sxs-lookup"><span data-stu-id="9211c-361">hello activity A2 runs when hello scheduled slices from D2 become available and hello scheduled availability frequency is reached.</span></span> <span data-ttu-id="9211c-362">Pokud dojde k chybě v jednom z hello řezy v datové sadě D2, nespustí se pro tento řez A2, dokud nebude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9211c-362">If there is an error in one of hello slices in dataset D2, A2 does not run for that slice until it becomes available.</span></span>

<span data-ttu-id="9211c-363">Hello zobrazení diagramu s obou aktivitami v hello, že stejné kanálu bude vypadat hello následující diagram:</span><span class="sxs-lookup"><span data-stu-id="9211c-363">hello Diagram view with both activities in hello same pipeline would look like hello following diagram:</span></span>

![Řetězení aktivity v hello stejné kanálu](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

<span data-ttu-id="9211c-365">Jak už bylo zmíněno dříve, hello aktivity může být v jiné kanály.</span><span class="sxs-lookup"><span data-stu-id="9211c-365">As mentioned earlier, hello activities could be in different pipelines.</span></span> <span data-ttu-id="9211c-366">V takové situaci zobrazení diagramu hello bude vypadat hello následující diagram:</span><span class="sxs-lookup"><span data-stu-id="9211c-366">In such a scenario, hello diagram view would look like hello following diagram:</span></span>

![Řetězení aktivity ve dvou kanálů](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

<span data-ttu-id="9211c-368">V tématu hello [zkopírujte postupně](#copy-sequentially) část v příloze hello příklad.</span><span class="sxs-lookup"><span data-stu-id="9211c-368">See hello [copy sequentially](#copy-sequentially) section in hello appendix for an example.</span></span>

## <a name="model-datasets-with-different-frequencies"></a><span data-ttu-id="9211c-369">Datové sady modelu s jinou frekvence</span><span class="sxs-lookup"><span data-stu-id="9211c-369">Model datasets with different frequencies</span></span>
<span data-ttu-id="9211c-370">V hello ukázky byly hello stejné hello frekvencí pro vstupní a výstupní datové sady a hello aktivity okno plánu.</span><span class="sxs-lookup"><span data-stu-id="9211c-370">In hello samples, hello frequencies for input and output datasets and hello activity schedule window were hello same.</span></span> <span data-ttu-id="9211c-371">Některé scénáře vyžadují hello možnost tooproduce výstup frekvencí, který se liší od hello frekvence jeden nebo více vstupů.</span><span class="sxs-lookup"><span data-stu-id="9211c-371">Some scenarios require hello ability tooproduce output at a frequency different than hello frequencies of one or more inputs.</span></span> <span data-ttu-id="9211c-372">Objekt pro vytváření dat podporuje tyto scénáře modelování.</span><span class="sxs-lookup"><span data-stu-id="9211c-372">Data Factory supports modeling these scenarios.</span></span>

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a><span data-ttu-id="9211c-373">Příklad 1: Vytvoření sestavy denní výstup pro vstupní data, která je k dispozici každou hodinu</span><span class="sxs-lookup"><span data-stu-id="9211c-373">Sample 1: Produce a daily output report for input data that is available every hour</span></span>
<span data-ttu-id="9211c-374">Vezměte v úvahu scénář, ve kterém můžete mít vstupní měření data ze senzorů, které jsou k dispozici každou hodinu do úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="9211c-374">Consider a scenario in which you have input measurement data from sensors available every hour in Azure Blob storage.</span></span> <span data-ttu-id="9211c-375">Chcete, aby tooproduce denní agregační sestava s statistické údaje, třeba střední, maximální a minimální hello den s [Data Factory hive aktivity](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="9211c-375">You want tooproduce a daily aggregate report with statistics such as mean, maximum, and minimum for hello day with [Data Factory hive activity](data-factory-hive-activity.md).</span></span>

<span data-ttu-id="9211c-376">Zde je, jak můžete model tento scénář se objekt pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="9211c-376">Here is how you can model this scenario with Data Factory:</span></span>

<span data-ttu-id="9211c-377">**Vstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="9211c-377">**Input dataset**</span></span>

<span data-ttu-id="9211c-378">Hello vstupní každou hodinu zahozených soubory ve složce hello pro hello zadaný den.</span><span class="sxs-lookup"><span data-stu-id="9211c-378">hello hourly input files are dropped in hello folder for hello given day.</span></span> <span data-ttu-id="9211c-379">Dostupnost pro vstup je nastavený na **hodinu** (frekvence: hodiny, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="9211c-379">Availability for input is set at **Hour** (frequency: Hour, interval: 1).</span></span>

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
<span data-ttu-id="9211c-380">**Výstupní datové sady**</span><span class="sxs-lookup"><span data-stu-id="9211c-380">**Output dataset**</span></span>

<span data-ttu-id="9211c-381">Jedna výstupní soubor se vytvoří každý den ve složce hello den.</span><span class="sxs-lookup"><span data-stu-id="9211c-381">One output file is created every day in hello day's folder.</span></span> <span data-ttu-id="9211c-382">Dostupnost výstupu je nastavený na **den** (frekvence: den a interval: 1).</span><span class="sxs-lookup"><span data-stu-id="9211c-382">Availability of output is set at **Day** (frequency: Day and interval: 1).</span></span>

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

<span data-ttu-id="9211c-383">**Aktivity: aktivitu v kanálu hivu**</span><span class="sxs-lookup"><span data-stu-id="9211c-383">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="9211c-384">skript hive Hello obdrží hello odpovídající *data a času* informace jako parametry, které používají hello **WindowStart** proměnné, jak je znázorněno v následujícím fragmentu kódu hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-384">hello hive script receives hello appropriate *DateTime* information as parameters that use hello **WindowStart** variable as shown in hello following snippet.</span></span> <span data-ttu-id="9211c-385">skript hive Hello používá tato data hello proměnné tooload z správnou složku hello hello den a spusťte hello agregace toogenerate hello výstup.</span><span class="sxs-lookup"><span data-stu-id="9211c-385">hello hive script uses this variable tooload hello data from hello correct folder for hello day and run hello aggregation toogenerate hello output.</span></span>

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

<span data-ttu-id="9211c-386">Hello následující diagram znázorňuje hello scénář z hlediska data závislostí.</span><span class="sxs-lookup"><span data-stu-id="9211c-386">hello following diagram shows hello scenario from a data-dependency point of view.</span></span>

![Data závislostí](./media/data-factory-scheduling-and-execution/data-dependency.png)

<span data-ttu-id="9211c-388">Hello výstupní řez pro každý den, závisí na 24 řezů hodinové ze vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-388">hello output slice for every day depends on 24 hourly slices from an input dataset.</span></span> <span data-ttu-id="9211c-389">Data Factory výpočtů tyto závislosti automaticky po zjištění hello vstupní datové řezy, které se nacházejí v hello stejné jako hello výstupní řez toobe vytvořeného časové období.</span><span class="sxs-lookup"><span data-stu-id="9211c-389">Data Factory computes these dependencies automatically by figuring out hello input data slices that fall in hello same time period as hello output slice toobe produced.</span></span> <span data-ttu-id="9211c-390">Pokud žádné vstupní řezy hello 24 není k dispozici, čeká na objekt pro vytváření dat toobe vstupní řez hello připravené před spuštěním hello denní aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="9211c-390">If any of hello 24 input slices is not available, Data Factory waits for hello input slice toobe ready before starting hello daily activity run.</span></span>

### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a><span data-ttu-id="9211c-391">Příklad 2: Zadejte závislostí s výrazy a funkce pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="9211c-391">Sample 2: Specify dependency with expressions and Data Factory functions</span></span>
<span data-ttu-id="9211c-392">Pojďme se jiný scénář.</span><span class="sxs-lookup"><span data-stu-id="9211c-392">Let’s consider another scenario.</span></span> <span data-ttu-id="9211c-393">Předpokládejme, že máte aktivitu hive, který zpracovává dvě vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-393">Suppose you have a hive activity that processes two input datasets.</span></span> <span data-ttu-id="9211c-394">Jeden z nich má nová data denně, ale jeden z nich získá nová data každý týden.</span><span class="sxs-lookup"><span data-stu-id="9211c-394">One of them has new data daily, but one of them gets new data every week.</span></span> <span data-ttu-id="9211c-395">Předpokládejme, že jste chtěli toodo spojení mezi dvěma vstupy hello a vytvořit výstup každý den.</span><span class="sxs-lookup"><span data-stu-id="9211c-395">Suppose you wanted toodo a join across hello two inputs and produce an output every day.</span></span>

<span data-ttu-id="9211c-396">Hello jednoduchý přístup, ve kterém Data Factory automaticky hodnoty na pravém vstup hello řezy tooprocess slaďte toohello výstup, který datový řez časové období není funkční.</span><span class="sxs-lookup"><span data-stu-id="9211c-396">hello simple approach in which Data Factory automatically figures out hello right input slices tooprocess by aligning toohello output data slice’s time period does not work.</span></span>

<span data-ttu-id="9211c-397">Je nutné zadat, že pro každé aktivity při spuštění, hello objekt pro vytváření dat mají používat minulého týdne datový řez hello týdenní vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-397">You must specify that for every activity run, hello Data Factory should use last week’s data slice for hello weekly input dataset.</span></span> <span data-ttu-id="9211c-398">Můžete použít funkce Azure Data Factory, jak je znázorněno v následujícím fragmentu kódu tooimplement hello toto chování.</span><span class="sxs-lookup"><span data-stu-id="9211c-398">You use Azure Data Factory functions as shown in hello following snippet tooimplement this behavior.</span></span>

<span data-ttu-id="9211c-399">**Input1: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="9211c-399">**Input1: Azure blob**</span></span>

<span data-ttu-id="9211c-400">Hello první vstup, se právě hello objektů blob v Azure aktualizuje denně.</span><span class="sxs-lookup"><span data-stu-id="9211c-400">hello first input is hello Azure blob being updated daily.</span></span>

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

<span data-ttu-id="9211c-401">**Input2: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="9211c-401">**Input2: Azure blob**</span></span>

<span data-ttu-id="9211c-402">Input2 je hello objektů blob v Azure aktualizují každý týden.</span><span class="sxs-lookup"><span data-stu-id="9211c-402">Input2 is hello Azure blob being updated weekly.</span></span>

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

<span data-ttu-id="9211c-403">**Výstup: Azure blob**</span><span class="sxs-lookup"><span data-stu-id="9211c-403">**Output: Azure blob**</span></span>

<span data-ttu-id="9211c-404">Jedna výstupní soubor se vytvoří každý den ve složce hello hello den.</span><span class="sxs-lookup"><span data-stu-id="9211c-404">One output file is created every day in hello folder for hello day.</span></span> <span data-ttu-id="9211c-405">Dostupnost výstupu je nastaven příliš**den** (frekvence: den, interval: 1).</span><span class="sxs-lookup"><span data-stu-id="9211c-405">Availability of output is set too**day** (frequency: Day, interval: 1).</span></span>

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

<span data-ttu-id="9211c-406">**Aktivity: aktivitu v kanálu hivu**</span><span class="sxs-lookup"><span data-stu-id="9211c-406">**Activity: hive activity in a pipeline**</span></span>

<span data-ttu-id="9211c-407">Aktivita hive Hello trvá hello dva vstupy a produkuje řez výstupních každý den.</span><span class="sxs-lookup"><span data-stu-id="9211c-407">hello hive activity takes hello two inputs and produces an output slice every day.</span></span> <span data-ttu-id="9211c-408">Můžete zadat každý den výstupní řez toodepend na dobrý den předchozího týdne vstupní řez pro týdenní vstup následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="9211c-408">You can specify every day’s output slice toodepend on hello previous week’s input slice for weekly input as follows.</span></span>

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

<span data-ttu-id="9211c-409">V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam funkce a systémové proměnné, které podporuje služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="9211c-409">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of functions and system variables that Data Factory supports.</span></span>

## <a name="appendix"></a><span data-ttu-id="9211c-410">Příloha</span><span class="sxs-lookup"><span data-stu-id="9211c-410">Appendix</span></span>

### <a name="example-copy-sequentially"></a><span data-ttu-id="9211c-411">Příklad: kopírování postupně</span><span class="sxs-lookup"><span data-stu-id="9211c-411">Example: copy sequentially</span></span>
<span data-ttu-id="9211c-412">Ho je možné toorun více operací kopírování jedna po druhé způsobem sekvenční/řazení.</span><span class="sxs-lookup"><span data-stu-id="9211c-412">It is possible toorun multiple copy operations one after another in a sequential/ordered manner.</span></span> <span data-ttu-id="9211c-413">Například můžete mít dvě kopie aktivity v kanálu (CopyActivity1 a CopyActivity2) s hello následující vstupní data výstupní datové sady:</span><span class="sxs-lookup"><span data-stu-id="9211c-413">For example, you might have two copy activities in a pipeline (CopyActivity1 and CopyActivity2) with hello following input data output datasets:</span></span>   

<span data-ttu-id="9211c-414">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="9211c-414">CopyActivity1</span></span>

<span data-ttu-id="9211c-415">Vstup: datové sady.</span><span class="sxs-lookup"><span data-stu-id="9211c-415">Input: Dataset.</span></span> <span data-ttu-id="9211c-416">Výstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="9211c-416">Output: Dataset2.</span></span>

<span data-ttu-id="9211c-417">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="9211c-417">CopyActivity2</span></span>

<span data-ttu-id="9211c-418">Vstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="9211c-418">Input: Dataset2.</span></span>  <span data-ttu-id="9211c-419">Výstup: Dataset3.</span><span class="sxs-lookup"><span data-stu-id="9211c-419">Output: Dataset3.</span></span>

<span data-ttu-id="9211c-420">CopyActivity2 spustí jenom v případě hello CopyActivity1 proběhla úspěšně a Dataset2 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9211c-420">CopyActivity2 would run only if hello CopyActivity1 has run successfully and Dataset2 is available.</span></span>

<span data-ttu-id="9211c-421">Tady je ukázkový kanál služby hello JSON:</span><span class="sxs-lookup"><span data-stu-id="9211c-421">Here is hello sample pipeline JSON:</span></span>

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
                "description": "Copy data from a blob tooanother"
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
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2016-08-25T01:00:00Z",
        "end": "2016-08-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="9211c-422">Všimněte si, že v příkladu hello hello výstupní datovou sadu hello první aktivitu kopírování (Dataset2) zadané jako vstup pro druhá aktivita hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-422">Notice that in hello example, hello output dataset of hello first copy activity (Dataset2) is specified as input for hello second activity.</span></span> <span data-ttu-id="9211c-423">Proto hello druhá aktivita spustí pouze v případě, že hello výstupní datové sady z první aktivitu hello je připraven.</span><span class="sxs-lookup"><span data-stu-id="9211c-423">Therefore, hello second activity runs only when hello output dataset from hello first activity is ready.</span></span>  

<span data-ttu-id="9211c-424">V příkladu hello CopyActivity2 může mít různé vstupu, například Dataset3, ale zadat Dataset2 jako vstupní tooCopyActivity2, takže hello aktivity nespustí, dokud nebude dokončeno CopyActivity1.</span><span class="sxs-lookup"><span data-stu-id="9211c-424">In hello example, CopyActivity2 can have a different input, such as Dataset3, but you specify Dataset2 as an input tooCopyActivity2, so hello activity does not run until CopyActivity1 finishes.</span></span> <span data-ttu-id="9211c-425">Například:</span><span class="sxs-lookup"><span data-stu-id="9211c-425">For example:</span></span>

<span data-ttu-id="9211c-426">CopyActivity1</span><span class="sxs-lookup"><span data-stu-id="9211c-426">CopyActivity1</span></span>

<span data-ttu-id="9211c-427">Vstup: Dataset1.</span><span class="sxs-lookup"><span data-stu-id="9211c-427">Input: Dataset1.</span></span> <span data-ttu-id="9211c-428">Výstup: Dataset2.</span><span class="sxs-lookup"><span data-stu-id="9211c-428">Output: Dataset2.</span></span>

<span data-ttu-id="9211c-429">CopyActivity2</span><span class="sxs-lookup"><span data-stu-id="9211c-429">CopyActivity2</span></span>

<span data-ttu-id="9211c-430">Vstupy: Dataset3, Dataset2.</span><span class="sxs-lookup"><span data-stu-id="9211c-430">Inputs: Dataset3, Dataset2.</span></span> <span data-ttu-id="9211c-431">Výstup: Dataset4.</span><span class="sxs-lookup"><span data-stu-id="9211c-431">Output: Dataset4.</span></span>

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
                "description": "Copy data from a blob tooanother"
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
                "description": "Copy data from a blob tooanother"
            }
        ],
        "start": "2017-04-25T01:00:00Z",
        "end": "2017-04-25T01:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="9211c-432">Všimněte si, že v příkladu hello jsou dva vstupní datové sady zadané pro aktivitu kopírování druhý hello.</span><span class="sxs-lookup"><span data-stu-id="9211c-432">Notice that in hello example, two input datasets are specified for hello second copy activity.</span></span> <span data-ttu-id="9211c-433">Jsou-li více vstupů, pouze hello první vstupní datové sady se používá ke kopírování dat, ale jiné datové sady se používají jako závislosti.</span><span class="sxs-lookup"><span data-stu-id="9211c-433">When multiple inputs are specified, only hello first input dataset is used for copying data, but other datasets are used as dependencies.</span></span> <span data-ttu-id="9211c-434">CopyActivity2 by spustit až po hello jsou splněny následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="9211c-434">CopyActivity2 would start only after hello following conditions are met:</span></span>

* <span data-ttu-id="9211c-435">CopyActivity1 byla úspěšně dokončena a Dataset2 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9211c-435">CopyActivity1 has successfully completed and Dataset2 is available.</span></span> <span data-ttu-id="9211c-436">Tato datová sada se nepoužívá při kopírování dat tooDataset4.</span><span class="sxs-lookup"><span data-stu-id="9211c-436">This dataset is not used when copying data tooDataset4.</span></span> <span data-ttu-id="9211c-437">Pouze funguje jako plánování závislost pro CopyActivity2.</span><span class="sxs-lookup"><span data-stu-id="9211c-437">It only acts as a scheduling dependency for CopyActivity2.</span></span>   
* <span data-ttu-id="9211c-438">Dataset3 je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9211c-438">Dataset3 is available.</span></span> <span data-ttu-id="9211c-439">Tato datová sada představuje hello data, která je zkopírovaný toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="9211c-439">This dataset represents hello data that is copied toohello destination.</span></span> 
