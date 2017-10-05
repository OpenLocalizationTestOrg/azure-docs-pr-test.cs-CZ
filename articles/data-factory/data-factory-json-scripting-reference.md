---
title: "Azure Data Factory - referenčních informacích o skriptování JSON | Microsoft Docs"
description: "Poskytuje schémata JSON entit služby Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 805106c0a5cdbff1f143f22a2ae59f6d2a0bf126
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a><span data-ttu-id="3e9dc-103">Azure Data Factory - referenčních informacích o skriptování JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-103">Azure Data Factory - JSON Scripting Reference</span></span>
<span data-ttu-id="3e9dc-104">Tento článek obsahuje schémata JSON a příklady pro definování entity Azure Data Factory (kanál, aktivity, datové sady a propojené služby).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-104">This article provides JSON schemas and examples for defining Azure Data Factory entities (pipeline, activity, dataset, and linked service).</span></span>  

## <a name="pipeline"></a><span data-ttu-id="3e9dc-105">Kanál</span><span class="sxs-lookup"><span data-stu-id="3e9dc-105">Pipeline</span></span> 
<span data-ttu-id="3e9dc-106">Základní strukturu pro definici kanálu je následující:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-106">The high-level structure for a pipeline definition is as follows:</span></span> 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="3e9dc-107">Následující tabulka popisuje vlastnosti v rámci kanálu definici JSON:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-107">Following table describes the properties within the pipeline JSON definition:</span></span>

| <span data-ttu-id="3e9dc-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-108">Property</span></span> | <span data-ttu-id="3e9dc-109">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-109">Description</span></span> | <span data-ttu-id="3e9dc-110">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-110">Required</span></span>
-------- | ----------- | --------
| <span data-ttu-id="3e9dc-111">jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-111">name</span></span> | <span data-ttu-id="3e9dc-112">Název kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-112">Name of the pipeline.</span></span> <span data-ttu-id="3e9dc-113">Zadejte název, který představuje akci, aktivity nebo kanálu je nakonfigurovaný</span><span class="sxs-lookup"><span data-stu-id="3e9dc-113">Specify a name that represents the action that the activity or pipeline is configured to do</span></span><br/><ul><li><span data-ttu-id="3e9dc-114">Maximální počet znaků: 260</span><span class="sxs-lookup"><span data-stu-id="3e9dc-114">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="3e9dc-115">Musí začínat číslem písmenem nebo podtržítkem (_)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-115">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="3e9dc-116">Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-116">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="3e9dc-117">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-117">Yes</span></span> |
| <span data-ttu-id="3e9dc-118">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-118">description</span></span> |<span data-ttu-id="3e9dc-119">Text popisující, co aktivity nebo kanálu se používá pro</span><span class="sxs-lookup"><span data-stu-id="3e9dc-119">Text describing what the activity or pipeline is used for</span></span> | <span data-ttu-id="3e9dc-120">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-120">No</span></span> |
| <span data-ttu-id="3e9dc-121">aktivity</span><span class="sxs-lookup"><span data-stu-id="3e9dc-121">activities</span></span> | <span data-ttu-id="3e9dc-122">Obsahuje seznam aktivit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-122">Contains a list of activities.</span></span> | <span data-ttu-id="3e9dc-123">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-123">Yes</span></span> |
| <span data-ttu-id="3e9dc-124">start</span><span class="sxs-lookup"><span data-stu-id="3e9dc-124">start</span></span> |<span data-ttu-id="3e9dc-125">Počáteční datum a čas pro kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-125">Start date-time for the pipeline.</span></span> <span data-ttu-id="3e9dc-126">Musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-126">Must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="3e9dc-127">Příklad: 2014-10-14T16:32:41.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-127">For example: 2014-10-14T16:32:41.</span></span> <br/><br/><span data-ttu-id="3e9dc-128">Je možné zadat místní čas, například Odhadovaný čas.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-128">It is possible to specify a local time, for example an EST time.</span></span> <span data-ttu-id="3e9dc-129">Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6</span><span class="sxs-lookup"><span data-stu-id="3e9dc-129">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="3e9dc-130">Počáteční a koncové vlastnosti společně zadejte aktivní období kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-130">The start and end properties together specify active period for the pipeline.</span></span> <span data-ttu-id="3e9dc-131">Výstup řezy jenom vytváří se v tomto aktivní období.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-131">Output slices are only produced with in this active period.</span></span> |<span data-ttu-id="3e9dc-132">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-132">No</span></span><br/><br/><span data-ttu-id="3e9dc-133">Pokud zadáte hodnotu pro vlastnost end, zadejte hodnotu pro vlastnost start.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-133">If you specify a value for the end property, you must specify value for the start property.</span></span><br/><br/><span data-ttu-id="3e9dc-134">Počáteční a koncový čas i lze vytvořit kanál prázdný.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-134">The start and end times can both be empty to create a pipeline.</span></span> <span data-ttu-id="3e9dc-135">Musíte zadat obě hodnoty se nastavit aktivní období pro kanál ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-135">You must specify both values to set an active period for the pipeline to run.</span></span> <span data-ttu-id="3e9dc-136">Pokud nezadáte počáteční a koncový čas při vytváření kanálu, můžete nastavit pomocí rutiny Set-AzureRmDataFactoryPipelineActivePeriod později.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-136">If you do not specify start and end times when creating a pipeline, you can set them using the Set-AzureRmDataFactoryPipelineActivePeriod cmdlet later.</span></span> |
| <span data-ttu-id="3e9dc-137">End</span><span class="sxs-lookup"><span data-stu-id="3e9dc-137">end</span></span> |<span data-ttu-id="3e9dc-138">Koncové datum a čas pro kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-138">End date-time for the pipeline.</span></span> <span data-ttu-id="3e9dc-139">Pokud zadaný, musí být ve formátu ISO.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-139">If specified must be in ISO format.</span></span> <span data-ttu-id="3e9dc-140">Příklad: 2014-10-14T17:32:41</span><span class="sxs-lookup"><span data-stu-id="3e9dc-140">For example: 2014-10-14T17:32:41</span></span> <br/><br/><span data-ttu-id="3e9dc-141">Je možné zadat místní čas, například Odhadovaný čas.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-141">It is possible to specify a local time, for example an EST time.</span></span> <span data-ttu-id="3e9dc-142">Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6</span><span class="sxs-lookup"><span data-stu-id="3e9dc-142">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="3e9dc-143">Chcete-li kanál spouštět bez omezení, zadejte jako hodnotu pro vlastnost end 9999-09-09.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-143">To run the pipeline indefinitely, specify 9999-09-09 as the value for the end property.</span></span> |<span data-ttu-id="3e9dc-144">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-144">No</span></span> <br/><br/><span data-ttu-id="3e9dc-145">Pokud zadáte hodnotu pro vlastnost spustit, musíte zadat hodnotu pro vlastnost end.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-145">If you specify a value for the start property, you must specify value for the end property.</span></span><br/><br/><span data-ttu-id="3e9dc-146">Naleznete v poznámkách k **spustit** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-146">See notes for the **start** property.</span></span> |
| <span data-ttu-id="3e9dc-147">isPaused</span><span class="sxs-lookup"><span data-stu-id="3e9dc-147">isPaused</span></span> |<span data-ttu-id="3e9dc-148">Pokud je nastaven na hodnotu true kanálu nelze spustit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-148">If set to true the pipeline does not run.</span></span> <span data-ttu-id="3e9dc-149">Výchozí hodnota = false.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-149">Default value = false.</span></span> <span data-ttu-id="3e9dc-150">Tato vlastnost slouží k povolení nebo zakázání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-150">You can use this property to enable or disable.</span></span> |<span data-ttu-id="3e9dc-151">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-151">No</span></span> |
| <span data-ttu-id="3e9dc-152">pipelineMode</span><span class="sxs-lookup"><span data-stu-id="3e9dc-152">pipelineMode</span></span> |<span data-ttu-id="3e9dc-153">Metoda pro naplánování spuštění pro kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-153">The method for scheduling runs for the pipeline.</span></span> <span data-ttu-id="3e9dc-154">Povolené hodnoty jsou: naplánované (výchozí), jednorázově.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-154">Allowed values are: scheduled (default), onetime.</span></span><br/><br/><span data-ttu-id="3e9dc-155">"Pravidelnou" udává, že kanál spouští v zadaném časovém intervalu podle jeho aktivní období (počáteční a koncový čas).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-155">‘Scheduled’ indicates that the pipeline runs at a specified time interval according to its active period (start and end time).</span></span> <span data-ttu-id="3e9dc-156">'Jednorázově' udává, že kanál spouští jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-156">‘Onetime’ indicates that the pipeline runs only once.</span></span> <span data-ttu-id="3e9dc-157">Po vytvoření jednorázově kanály nelze aktuálně upravit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-157">Onetime pipelines once created cannot be modified/updated currently.</span></span> <span data-ttu-id="3e9dc-158">V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti o jednorázově nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-158">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details about onetime setting.</span></span> |<span data-ttu-id="3e9dc-159">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-159">No</span></span> |
| <span data-ttu-id="3e9dc-160">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="3e9dc-160">expirationTime</span></span> |<span data-ttu-id="3e9dc-161">Doba, po vytvoření, pro který kanálu je platný a by měla zůstat zřízené.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-161">Duration of time after creation for which the pipeline is valid and should remain provisioned.</span></span> <span data-ttu-id="3e9dc-162">Pokud nemá žádné aktivní, se nezdařilo, nebo čekající spuštění kanálu automaticky odstraněna po dosažení času vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-162">If it does not have any active, failed, or pending runs, the pipeline is deleted automatically once it reaches the expiration time.</span></span> |<span data-ttu-id="3e9dc-163">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-163">No</span></span> |


## <a name="activity"></a><span data-ttu-id="3e9dc-164">Aktivita</span><span class="sxs-lookup"><span data-stu-id="3e9dc-164">Activity</span></span> 
<span data-ttu-id="3e9dc-165">Základní strukturu pro aktivitu v rámci kanálu definice (aktivity element) je následující:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-165">The high-level structure for an activity within a pipeline definition (activities element) is as follows:</span></span>

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

<span data-ttu-id="3e9dc-166">Následující tabulka popisuje vlastnosti v rámci aktivity definici JSON:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-166">Following table describe the properties within the activity JSON definition:</span></span>

| <span data-ttu-id="3e9dc-167">Značka</span><span class="sxs-lookup"><span data-stu-id="3e9dc-167">Tag</span></span> | <span data-ttu-id="3e9dc-168">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-168">Description</span></span> | <span data-ttu-id="3e9dc-169">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-170">jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-170">name</span></span> |<span data-ttu-id="3e9dc-171">Název aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-171">Name of the activity.</span></span> <span data-ttu-id="3e9dc-172">Zadejte název, který představuje akci nakonfigurovaný tak, aby se aktivity</span><span class="sxs-lookup"><span data-stu-id="3e9dc-172">Specify a name that represents the action that the activity is configured to do</span></span><br/><ul><li><span data-ttu-id="3e9dc-173">Maximální počet znaků: 260</span><span class="sxs-lookup"><span data-stu-id="3e9dc-173">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="3e9dc-174">Musí začínat číslem písmenem nebo podtržítkem (_)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-174">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="3e9dc-175">Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-175">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="3e9dc-176">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-176">Yes</span></span> |
| <span data-ttu-id="3e9dc-177">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-177">description</span></span> |<span data-ttu-id="3e9dc-178">Text popisující, co se používá aktivitu pro.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-178">Text describing what the activity is used for.</span></span> |<span data-ttu-id="3e9dc-179">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-179">Yes</span></span> |
| <span data-ttu-id="3e9dc-180">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-180">type</span></span> |<span data-ttu-id="3e9dc-181">Určuje typ aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-181">Specifies the type of the activity.</span></span> <span data-ttu-id="3e9dc-182">Najdete v článku [ÚLOŽIŠŤ dat](#data-stores) a [aktivit TRANSFORMACE dat](#data-transformation-activities) oddíly pro různé typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-182">See the [DATA STORES](#data-stores) and [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) sections for different types of activities.</span></span> |<span data-ttu-id="3e9dc-183">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-183">Yes</span></span> |
| <span data-ttu-id="3e9dc-184">Vstupy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-184">inputs</span></span> |<span data-ttu-id="3e9dc-185">Vstupní tabulky použité aktivitou</span><span class="sxs-lookup"><span data-stu-id="3e9dc-185">Input tables used by the activity</span></span><br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |<span data-ttu-id="3e9dc-186">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-186">Yes</span></span> |
| <span data-ttu-id="3e9dc-187">Výstupy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-187">outputs</span></span> |<span data-ttu-id="3e9dc-188">Výstupní tabulky použité aktivitou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-188">Output tables used by the activity.</span></span><br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |<span data-ttu-id="3e9dc-189">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-189">Yes</span></span> |
| <span data-ttu-id="3e9dc-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-190">linkedServiceName</span></span> |<span data-ttu-id="3e9dc-191">Název propojené služby použité aktivitou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-191">Name of the linked service used by the activity.</span></span> <br/><br/><span data-ttu-id="3e9dc-192">Aktivita může vyžadovat, že zadáváte propojené služby, která odkazuje na požadované výpočetním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-192">An activity may require that you specify the linked service that links to the required compute environment.</span></span> |<span data-ttu-id="3e9dc-193">Ano pro aktivity HDInsight, Azure Machine Learning aktivity a aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-193">Yes for HDInsight activities, Azure Machine Learning activities, and Stored Procedure Activity.</span></span> <br/><br/><span data-ttu-id="3e9dc-194">Ne všechny ostatní uživatelé</span><span class="sxs-lookup"><span data-stu-id="3e9dc-194">No for all others</span></span> |
| <span data-ttu-id="3e9dc-195">rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="3e9dc-195">typeProperties</span></span> |<span data-ttu-id="3e9dc-196">Vlastnosti v rámci typeProperties části závisí na typu aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-196">Properties in the typeProperties section depend on type of the activity.</span></span> |<span data-ttu-id="3e9dc-197">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-197">No</span></span> |
| <span data-ttu-id="3e9dc-198">Zásady</span><span class="sxs-lookup"><span data-stu-id="3e9dc-198">policy</span></span> |<span data-ttu-id="3e9dc-199">Zásady, které ovlivňují chování běhu aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-199">Policies that affect the run-time behavior of the activity.</span></span> <span data-ttu-id="3e9dc-200">Pokud není zadaný, použijí se výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-200">If it is not specified, default policies are used.</span></span> |<span data-ttu-id="3e9dc-201">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-201">No</span></span> |
| <span data-ttu-id="3e9dc-202">Scheduler</span><span class="sxs-lookup"><span data-stu-id="3e9dc-202">scheduler</span></span> |<span data-ttu-id="3e9dc-203">Vlastnost "scheduler" se používá k definování požadované plánování pro aktivitu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-203">“scheduler” property is used to define desired scheduling for the activity.</span></span> <span data-ttu-id="3e9dc-204">Jeho podvlastnosti jsou stejné jako ty, které jsou v [vlastnost availability v datové sadě](data-factory-create-datasets.md#dataset-availability).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-204">Its subproperties are the same as the ones in the [availability property in a dataset](data-factory-create-datasets.md#dataset-availability).</span></span> |<span data-ttu-id="3e9dc-205">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-205">No</span></span> |

### <a name="policies"></a><span data-ttu-id="3e9dc-206">Zásady</span><span class="sxs-lookup"><span data-stu-id="3e9dc-206">Policies</span></span>
<span data-ttu-id="3e9dc-207">Zásady ovlivňují chování běhu aktivity, konkrétně při zpracování řezu tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-207">Policies affect the run-time behavior of an activity, specifically when the slice of a table is processed.</span></span> <span data-ttu-id="3e9dc-208">Následující tabulka obsahuje podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-208">The following table provides the details.</span></span>

| <span data-ttu-id="3e9dc-209">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-209">Property</span></span> | <span data-ttu-id="3e9dc-210">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-210">Permitted values</span></span> | <span data-ttu-id="3e9dc-211">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3e9dc-211">Default Value</span></span> | <span data-ttu-id="3e9dc-212">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-212">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-213">Souběžnosti</span><span class="sxs-lookup"><span data-stu-id="3e9dc-213">concurrency</span></span> |<span data-ttu-id="3e9dc-214">Integer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-214">Integer</span></span> <br/><br/><span data-ttu-id="3e9dc-215">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="3e9dc-215">Max value: 10</span></span> |<span data-ttu-id="3e9dc-216">1</span><span class="sxs-lookup"><span data-stu-id="3e9dc-216">1</span></span> |<span data-ttu-id="3e9dc-217">Počet souběžných spuštění aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-217">Number of concurrent executions of the activity.</span></span><br/><br/><span data-ttu-id="3e9dc-218">Určuje počet spuštění paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-218">It determines the number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="3e9dc-219">Například pokud aktivitu musí projít, velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-219">For example, if an activity needs to go through a large set of available data, having a larger concurrency value speeds up the data processing.</span></span> |
| <span data-ttu-id="3e9dc-220">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="3e9dc-220">executionPriorityOrder</span></span> |<span data-ttu-id="3e9dc-221">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="3e9dc-221">NewestFirst</span></span><br/><br/><span data-ttu-id="3e9dc-222">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="3e9dc-222">OldestFirst</span></span> |<span data-ttu-id="3e9dc-223">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="3e9dc-223">OldestFirst</span></span> |<span data-ttu-id="3e9dc-224">Určuje pořadí datové řezy, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-224">Determines the ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="3e9dc-225">Pokud máte 2 řezy (jeden situaci ve 4 a další v 17: 00) a jsou obě čekající na zpracování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-225">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="3e9dc-226">Pokud jste nastavili executionPriorityOrder být NewestFirst, je nejprve zpracování řezu v 17: 00.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-226">If you set the executionPriorityOrder to be NewestFirst, the slice at 5 PM is processed first.</span></span> <span data-ttu-id="3e9dc-227">Podobně pokud nastavíte executionPriorityORder být OldestFIrst, pak ve 4 zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-227">Similarly if you set the executionPriorityORder to be OldestFIrst, then the slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="3e9dc-228">Opakování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-228">retry</span></span> |<span data-ttu-id="3e9dc-229">Integer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-229">Integer</span></span><br/><br/><span data-ttu-id="3e9dc-230">Maximální hodnota může být 10</span><span class="sxs-lookup"><span data-stu-id="3e9dc-230">Max value can be 10</span></span> |<span data-ttu-id="3e9dc-231">0</span><span class="sxs-lookup"><span data-stu-id="3e9dc-231">0</span></span> |<span data-ttu-id="3e9dc-232">Počet opakování, než se zpracování dat pro řez je označen jako selhání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-232">Number of retries before the data processing for the slice is marked as Failure.</span></span> <span data-ttu-id="3e9dc-233">Provedení aktivity pro datový řez je opakovat až zadaný počet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-233">Activity execution for a data slice is retried up to the specified retry count.</span></span> <span data-ttu-id="3e9dc-234">Opakovaném provádí co nejdříve po selhání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-234">The retry is done as soon as possible after the failure.</span></span> |
| <span data-ttu-id="3e9dc-235">Časový limit</span><span class="sxs-lookup"><span data-stu-id="3e9dc-235">timeout</span></span> |<span data-ttu-id="3e9dc-236">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-236">TimeSpan</span></span> |<span data-ttu-id="3e9dc-237">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3e9dc-237">00:00:00</span></span> |<span data-ttu-id="3e9dc-238">Časový limit aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-238">Timeout for the activity.</span></span> <span data-ttu-id="3e9dc-239">Příklad: 00:10:00 (znamená časový limit 10 minut)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-239">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="3e9dc-240">Pokud hodnota není zadána nebo je 0, časový limit je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-240">If a value is not specified or is 0, the timeout is infinite.</span></span><br/><br/><span data-ttu-id="3e9dc-241">Pokud bude čas zpracování dat na řez překročí hodnota časového limitu, se zruší a systém se pokusí opakujte zpracování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-241">If the data processing time on a slice exceeds the timeout value, it is canceled, and the system attempts to retry the processing.</span></span> <span data-ttu-id="3e9dc-242">Počet pokusů, závisí na vlastnost opakování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-242">The number of retries depends on the retry property.</span></span> <span data-ttu-id="3e9dc-243">Když dojde k vypršení časového limitu, je stav nastaven na TimedOut.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-243">When timeout occurs, the status is set to TimedOut.</span></span> |
| <span data-ttu-id="3e9dc-244">Zpoždění</span><span class="sxs-lookup"><span data-stu-id="3e9dc-244">delay</span></span> |<span data-ttu-id="3e9dc-245">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-245">TimeSpan</span></span> |<span data-ttu-id="3e9dc-246">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3e9dc-246">00:00:00</span></span> |<span data-ttu-id="3e9dc-247">Zadejte zpoždění před zpracování dat řezu spustí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-247">Specify the delay before data processing of the slice starts.</span></span><br/><br/><span data-ttu-id="3e9dc-248">Provádění aktivity pro datový řez se spustí po zpoždění očekávaný čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-248">The execution of activity for a data slice is started after the Delay is past the expected execution time.</span></span><br/><br/><span data-ttu-id="3e9dc-249">Příklad: 00:10:00 (znamená zpoždění 10 minut)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-249">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="3e9dc-250">opakování po delší době</span><span class="sxs-lookup"><span data-stu-id="3e9dc-250">longRetry</span></span> |<span data-ttu-id="3e9dc-251">Integer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-251">Integer</span></span><br/><br/><span data-ttu-id="3e9dc-252">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="3e9dc-252">Max value: 10</span></span> |<span data-ttu-id="3e9dc-253">1</span><span class="sxs-lookup"><span data-stu-id="3e9dc-253">1</span></span> |<span data-ttu-id="3e9dc-254">Počet dlouho opakování pokusů, než řez spuštění se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-254">The number of long retry attempts before the slice execution is failed.</span></span><br/><br/><span data-ttu-id="3e9dc-255">pokusy o opakování po delší době jsou rozmístěny ve longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-255">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="3e9dc-256">Takže pokud je třeba zadat čas mezi pokusy o opakování, použijte opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-256">So if you need to specify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="3e9dc-257">Pokud jsou zadané opakování a opakování po delší době, jednotlivé pokusy o opakování po delší době zahrnuje opakovaných pokusů a je maximální počet pokusů o opakování * opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-257">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and the max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="3e9dc-258">Například, pokud bychom měli následující nastavení v zásadách aktivit:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-258">For example, if we have the following settings in the activity policy:</span></span><br/><span data-ttu-id="3e9dc-259">Opakujte: 3</span><span class="sxs-lookup"><span data-stu-id="3e9dc-259">Retry: 3</span></span><br/><span data-ttu-id="3e9dc-260">opakování po delší době: 2</span><span class="sxs-lookup"><span data-stu-id="3e9dc-260">longRetry: 2</span></span><br/><span data-ttu-id="3e9dc-261">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="3e9dc-261">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="3e9dc-262">Předpokládá se jenom jeden řez provést (stav Čeká) a provedení aktivity pokaždé, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-262">Assume there is only one slice to execute (status is Waiting) and the activity execution fails every time.</span></span> <span data-ttu-id="3e9dc-263">Nejdřív by 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-263">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="3e9dc-264">Po každém pokusu o stav řezu bude opakovat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-264">After each attempt, the slice status would be Retry.</span></span> <span data-ttu-id="3e9dc-265">Po první 3 pokusy jsou přes, bude stav řezu opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-265">After first 3 attempts are over, the slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="3e9dc-266">Po hodině (který je na longRetryInteval hodnota) bude další sadu 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-266">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="3e9dc-267">Poté stav řezu by se nezdařilo a by se pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-267">After that, the slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="3e9dc-268">Proto celkové 6 pokusy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-268">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="3e9dc-269">Pokud žádné spuštění úspěšné, stav řezu by mít připravené a jsou pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-269">If any execution succeeds, the slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="3e9dc-270">opakování po delší době je možné použít situace, kdy závislé data dorazí na Nedeterministický časy nebo je v nestabilním stavu v rámci které zpracování dat dojde celém prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-270">longRetry may be used in situations where dependent data arrives at non-deterministic times or the overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="3e9dc-271">V takových případech to, které opakování, jedna po druhé nemusí být úspěšná a díky tomu v intervalech čas má za následek požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-271">In such cases, doing retries one after another may not help and doing so after an interval of time results in the desired output.</span></span><br/><br/><span data-ttu-id="3e9dc-272">Word varování: nenastavujte vysoké hodnoty pro opakování po delší době nebo longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-272">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="3e9dc-273">Vyšší hodnoty obvykle implikují dalších systémových otázek.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-273">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="3e9dc-274">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-274">longRetryInterval</span></span> |<span data-ttu-id="3e9dc-275">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-275">TimeSpan</span></span> |<span data-ttu-id="3e9dc-276">00:00:00</span><span class="sxs-lookup"><span data-stu-id="3e9dc-276">00:00:00</span></span> |<span data-ttu-id="3e9dc-277">Prodleva mezi pokusy o opakování dlouho</span><span class="sxs-lookup"><span data-stu-id="3e9dc-277">The delay between long retry attempts</span></span> |

### <a name="typeproperties-section"></a><span data-ttu-id="3e9dc-278">části v rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="3e9dc-278">typeProperties section</span></span>
<span data-ttu-id="3e9dc-279">V rámci typeProperties části se liší pro každou aktivitu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-279">The typeProperties section is different for each activity.</span></span> <span data-ttu-id="3e9dc-280">Transformace aktivity mají vlastnosti typu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-280">Transformation activities have just the type properties.</span></span> <span data-ttu-id="3e9dc-281">V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-281">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span> 

<span data-ttu-id="3e9dc-282">**Aktivita kopírování** má dvě témata v rámci typeProperties oddílu: **zdroj** a **podřízený**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-282">**Copy activity** has two subsections in the typeProperties section: **source** and **sink**.</span></span> <span data-ttu-id="3e9dc-283">V tématu [ÚLOŽIŠŤ dat](#data-stores) části v tomto článku pro JSON vzorků, které ukazují, jak použít data jako zdroj a jímka úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-283">See [DATA STORES](#data-stores) section in this article for JSON samples that show how to use a data store as a source and/or sink.</span></span> 

### <a name="sample-copy-pipeline"></a><span data-ttu-id="3e9dc-284">Ukázkový kanál kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-284">Sample copy pipeline</span></span>
<span data-ttu-id="3e9dc-285">V následující ukázkový kanál, je jedna aktivita typu **kopie** v **aktivity** části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-285">In the following sample pipeline, there is one activity of type **Copy** in the **activities** section.</span></span> <span data-ttu-id="3e9dc-286">V této ukázce [aktivity kopírování](data-factory-data-movement-activities.md) kopíruje data z Azure Blob storage do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-286">In this sample, the [Copy activity](data-factory-data-movement-activities.md) copies data from an Azure Blob storage to an Azure SQL database.</span></span> 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

<span data-ttu-id="3e9dc-287">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-287">Note the following points:</span></span>

* <span data-ttu-id="3e9dc-288">V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span>
* <span data-ttu-id="3e9dc-289">Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-289">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span>
* <span data-ttu-id="3e9dc-290">V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-290">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span>

<span data-ttu-id="3e9dc-291">V tématu [ÚLOŽIŠŤ dat](#data-stores) části v tomto článku pro JSON vzorků, které ukazují, jak použít data jako zdroj a jímka úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-291">See [DATA STORES](#data-stores) section in this article for JSON samples that show how to use a data store as a source and/or sink.</span></span>    

<span data-ttu-id="3e9dc-292">Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: kopírování dat z úložiště objektů Blob do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-292">For a complete walkthrough of creating this pipeline, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="sample-transformation-pipeline"></a><span data-ttu-id="3e9dc-293">Ukázkový kanál transformace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-293">Sample transformation pipeline</span></span>
<span data-ttu-id="3e9dc-294">V následující ukázkový kanál, je jedna aktivita typu **HDInsightHive** v **aktivity** části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-294">In the following sample pipeline, there is one activity of type **HDInsightHive** in the **activities** section.</span></span> <span data-ttu-id="3e9dc-295">V této ukázce [aktivitu HDInsight Hive](data-factory-hive-activity.md) transformuje data z úložiště objektů Blob v Azure tak, že spustíte soubor skriptu Hive v clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-295">In this sample, the [HDInsight Hive activity](data-factory-hive-activity.md) transforms data from an Azure Blob storage by running a Hive script file on an Azure HDInsight Hadoop cluster.</span></span> 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
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
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3e9dc-296">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-296">Note the following points:</span></span> 

* <span data-ttu-id="3e9dc-297">V části aktivit je jenom jedna aktivita jejichž **typ** je nastaven na **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-297">In the activities section, there is only one activity whose **type** is set to **HDInsightHive**.</span></span>
* <span data-ttu-id="3e9dc-298">Soubor skriptu Hive **partitionweblogs.hql** je uložený v účtu služby Azure Storage (který určuje služba scriptLinkedService s názvem **AzureStorageLinkedService**) a ve složce **script** v kontejneru **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-298">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in the container **adfgetstarted**.</span></span>
* <span data-ttu-id="3e9dc-299">**Definuje** části slouží k určení nastavení běhového prostředí, které se předávají skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-299">The **defines** section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span></span>

<span data-ttu-id="3e9dc-300">V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-300">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span>

<span data-ttu-id="3e9dc-301">Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: sestavit svůj první kanál pro zpracování dat pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-301">For a complete walkthrough of creating this pipeline, see [Tutorial: Build your first pipeline to process data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="linked-service"></a><span data-ttu-id="3e9dc-302">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-302">Linked service</span></span>
<span data-ttu-id="3e9dc-303">Základní strukturu pro definici propojené služby je následující:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-303">The high-level structure for a linked service definition is as follows:</span></span>

```json
{
    "name": "<name of the linked service>",
    "properties": {
        "type": "<type of the linked service>",
        "typeProperties": {
        }
    }
}
```

<span data-ttu-id="3e9dc-304">Následující tabulka popisuje vlastnosti v rámci aktivity definici JSON:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-304">Following table describe the properties within the activity JSON definition:</span></span>

| <span data-ttu-id="3e9dc-305">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-305">Property</span></span> | <span data-ttu-id="3e9dc-306">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-306">Description</span></span> | <span data-ttu-id="3e9dc-307">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-307">Required</span></span> |
| -------- | ----------- | -------- | 
| <span data-ttu-id="3e9dc-308">jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-308">name</span></span> | <span data-ttu-id="3e9dc-309">Název propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-309">Name of the linked service.</span></span> | <span data-ttu-id="3e9dc-310">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-310">Yes</span></span> | 
| <span data-ttu-id="3e9dc-311">vlastnosti – typ</span><span class="sxs-lookup"><span data-stu-id="3e9dc-311">properties - type</span></span> | <span data-ttu-id="3e9dc-312">Typ propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-312">Type of the linked service.</span></span> <span data-ttu-id="3e9dc-313">Příklad: úložiště Azure, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-313">For example: Azure Storage, Azure SQL Database.</span></span> |
| <span data-ttu-id="3e9dc-314">rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="3e9dc-314">typeProperties</span></span> | <span data-ttu-id="3e9dc-315">V rámci typeProperties části obsahuje prvky, které se liší u každé úložiště dat nebo výpočetní prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-315">The typeProperties section has elements that are different for each data store or compute environment.</span></span> <span data-ttu-id="3e9dc-316">V tématu [úložišť dat](#datastores) části pro všechna data ukládat propojené služby a [výpočetní prostředí](#compute-environments) pro všechny výpočetní propojené služby</span><span class="sxs-lookup"><span data-stu-id="3e9dc-316">See [data stores](#datastores) section for all the data store linked services and [compute environments](#compute-environments) for all the compute linked services</span></span> |   

## <a name="dataset"></a><span data-ttu-id="3e9dc-317">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-317">Dataset</span></span> 
<span data-ttu-id="3e9dc-318">Datové sady v Azure Data Factory je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-318">A dataset in Azure Data Factory is defined as follows:</span></span>

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

<span data-ttu-id="3e9dc-319">Následující tabulka popisuje vlastnosti v výše uvedený kód JSON:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-319">The following table describes properties in the above JSON:</span></span>   

| <span data-ttu-id="3e9dc-320">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-320">Property</span></span> | <span data-ttu-id="3e9dc-321">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-321">Description</span></span> | <span data-ttu-id="3e9dc-322">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-322">Required</span></span> | <span data-ttu-id="3e9dc-323">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e9dc-323">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-324">jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-324">name</span></span> | <span data-ttu-id="3e9dc-325">Název datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-325">Name of the dataset.</span></span> <span data-ttu-id="3e9dc-326">V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-326">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="3e9dc-327">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-327">Yes</span></span> |<span data-ttu-id="3e9dc-328">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-328">NA</span></span> |
| <span data-ttu-id="3e9dc-329">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-329">type</span></span> | <span data-ttu-id="3e9dc-330">Typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-330">Type of the dataset.</span></span> <span data-ttu-id="3e9dc-331">Zadejte jeden z typů podporovaných službou Azure Data Factory (například: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-331">Specify one of the types supported by Azure Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <span data-ttu-id="3e9dc-332">V tématu [ÚLOŽIŠŤ dat](#data-stores) části pro všechny datové úložiště a datové sady typy podporované službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-332">See [DATA STORES](#data-stores) section for all the data stores and dataset types supported by Data Factory.</span></span> | 
| <span data-ttu-id="3e9dc-333">Struktura</span><span class="sxs-lookup"><span data-stu-id="3e9dc-333">structure</span></span> | <span data-ttu-id="3e9dc-334">Schéma datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-334">Schema of the dataset.</span></span> <span data-ttu-id="3e9dc-335">Obsahuje sloupce, jejich typy, atd.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-335">It contains columns, their types, etc.</span></span> | <span data-ttu-id="3e9dc-336">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-336">No</span></span> |<span data-ttu-id="3e9dc-337">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-337">NA</span></span> |
| <span data-ttu-id="3e9dc-338">rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="3e9dc-338">typeProperties</span></span> | <span data-ttu-id="3e9dc-339">Vlastnosti odpovídající vybranému typu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-339">Properties corresponding to the selected type.</span></span> <span data-ttu-id="3e9dc-340">V tématu [ÚLOŽIŠŤ dat](#data-stores) části Podporované typy a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-340">See [DATA STORES](#data-stores) section for supported types and their properties.</span></span> |<span data-ttu-id="3e9dc-341">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-341">Yes</span></span> |<span data-ttu-id="3e9dc-342">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-342">NA</span></span> |
| <span data-ttu-id="3e9dc-343">external</span><span class="sxs-lookup"><span data-stu-id="3e9dc-343">external</span></span> | <span data-ttu-id="3e9dc-344">Logický příznak k určení, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-344">Boolean flag to specify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> |<span data-ttu-id="3e9dc-345">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-345">No</span></span> |<span data-ttu-id="3e9dc-346">False</span><span class="sxs-lookup"><span data-stu-id="3e9dc-346">false</span></span> |
| <span data-ttu-id="3e9dc-347">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="3e9dc-347">availability</span></span> | <span data-ttu-id="3e9dc-348">Definuje okna pro zpracování nebo řezů model pro produkční datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-348">Defines the processing window or the slicing model for the dataset production.</span></span> <span data-ttu-id="3e9dc-349">Podrobnosti na datovou sadu řezů modelu najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-349">For details on the dataset slicing model, see [Scheduling and Execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="3e9dc-350">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-350">Yes</span></span> |<span data-ttu-id="3e9dc-351">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-351">NA</span></span> |
| <span data-ttu-id="3e9dc-352">Zásady</span><span class="sxs-lookup"><span data-stu-id="3e9dc-352">policy</span></span> |<span data-ttu-id="3e9dc-353">Definuje kritéria nebo podmínku, musíte splnit řezy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-353">Defines the criteria or the condition that the dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="3e9dc-354">Podrobnosti najdete v tématu [datovou sadu zásad](#Policy) části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-354">For details, see [Dataset Policy](#Policy) section.</span></span> |<span data-ttu-id="3e9dc-355">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-355">No</span></span> |<span data-ttu-id="3e9dc-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-356">NA</span></span> |

<span data-ttu-id="3e9dc-357">Každý sloupec v **struktura** část obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-357">Each column in the **structure** section contains the following properties:</span></span>

| <span data-ttu-id="3e9dc-358">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-358">Property</span></span> | <span data-ttu-id="3e9dc-359">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-359">Description</span></span> | <span data-ttu-id="3e9dc-360">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-360">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-361">jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-361">name</span></span> |<span data-ttu-id="3e9dc-362">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-362">Name of the column.</span></span> |<span data-ttu-id="3e9dc-363">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-363">Yes</span></span> |
| <span data-ttu-id="3e9dc-364">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-364">type</span></span> |<span data-ttu-id="3e9dc-365">Datový typ sloupce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-365">Data type of the column.</span></span>  |<span data-ttu-id="3e9dc-366">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-366">No</span></span> |
| <span data-ttu-id="3e9dc-367">Jazyková verze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-367">culture</span></span> |<span data-ttu-id="3e9dc-368">.NET na základě jazykovou verzi, která se použije, když je zadaný typ a typ formátu .NET `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-368">.NET based culture to be used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="3e9dc-369">Výchozí hodnota je `en-us`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-369">Default is `en-us`.</span></span> |<span data-ttu-id="3e9dc-370">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-370">No</span></span> |
| <span data-ttu-id="3e9dc-371">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-371">format</span></span> |<span data-ttu-id="3e9dc-372">Řetězec, který se má použít, když je zadaný typ a typ formátu .NET formátu `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-372">Format string to be used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="3e9dc-373">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-373">No</span></span> |

<span data-ttu-id="3e9dc-374">V následujícím příkladu, datová sada má tři sloupce `slicetimestamp`, `projectname`, a `pageviews` a jsou typu: řetězec, řetězec a desetinných v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-374">In the following example, the dataset has three columns `slicetimestamp`, `projectname`, and `pageviews` and they are of type: String, String, and Decimal respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="3e9dc-375">Následující tabulka popisuje vlastnosti, které můžete použít v **dostupnosti** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-375">The following table describes properties you can use in the **availability** section:</span></span>

| <span data-ttu-id="3e9dc-376">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-376">Property</span></span> | <span data-ttu-id="3e9dc-377">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-377">Description</span></span> | <span data-ttu-id="3e9dc-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-378">Required</span></span> | <span data-ttu-id="3e9dc-379">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e9dc-379">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-380">frekvence</span><span class="sxs-lookup"><span data-stu-id="3e9dc-380">frequency</span></span> |<span data-ttu-id="3e9dc-381">Určuje časovou jednotku pro produkční řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-381">Specifies the time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="3e9dc-382"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="3e9dc-382"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="3e9dc-383">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-383">Yes</span></span> |<span data-ttu-id="3e9dc-384">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-384">NA</span></span> |
| <span data-ttu-id="3e9dc-385">Interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-385">interval</span></span> |<span data-ttu-id="3e9dc-386">Určuje multiplikátor pro četnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-386">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="3e9dc-387">"Frekvence x interval" Určuje, jak často se vytvářejí řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-387">”Frequency x interval” determines how often the slice is produced.</span></span><br/><br/><span data-ttu-id="3e9dc-388">Pokud budete potřebovat datovou sadu, která se rozříznut hodinu, nastavíte <b>frekvence</b> k <b>hodinu</b>, a <b>interval</b> k <b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-388">If you need the dataset to be sliced on an hourly basis, you set <b>Frequency</b> to <b>Hour</b>, and <b>interval</b> to <b>1</b>.</span></span><br/><br/><span data-ttu-id="3e9dc-389"><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme nastavit interval na menší než 15</span><span class="sxs-lookup"><span data-stu-id="3e9dc-389"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set the interval to no less than 15</span></span> |<span data-ttu-id="3e9dc-390">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-390">Yes</span></span> |<span data-ttu-id="3e9dc-391">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-391">NA</span></span> |
| <span data-ttu-id="3e9dc-392">Styl</span><span class="sxs-lookup"><span data-stu-id="3e9dc-392">style</span></span> |<span data-ttu-id="3e9dc-393">Určuje, zda by měl být na zahájení a ukončení intervalu předložen řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-393">Specifies whether the slice should be produced at the start/end of the interval.</span></span><ul><li><span data-ttu-id="3e9dc-394">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-394">StartOfInterval</span></span></li><li><span data-ttu-id="3e9dc-395">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-395">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="3e9dc-396">Pokud je nastavena frekvence měsíc a styl je nastaven na EndOfInterval, řez vytváří poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-396">If Frequency is set to Month and style is set to EndOfInterval, the slice is produced on the last day of month.</span></span> <span data-ttu-id="3e9dc-397">Pokud je styl nastavené na StartOfInterval, řez vytváří první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-397">If the style is set to StartOfInterval, the slice is produced on the first day of month.</span></span><br/><br/><span data-ttu-id="3e9dc-398">Pokud je nastavena frekvence den a styl je nastaven na EndOfInterval, řez se vytvářejí za poslední hodinu dne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-398">If Frequency is set to Day and style is set to EndOfInterval, the slice is produced in the last hour of the day.</span></span><br/><br/><span data-ttu-id="3e9dc-399">Pokud je nastavena frekvence hodinu a styl je nastaven na EndOfInterval, řez se vytvářejí na konci za hodinu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-399">If Frequency is set to Hour and style is set to EndOfInterval, the slice is produced at the end of the hour.</span></span> <span data-ttu-id="3e9dc-400">Například pro řez dobu 13: 00 – 14: 00, je řez vytvořeného ve 2.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-400">For example, for a slice for 1 PM – 2 PM period, the slice is produced at 2 PM.</span></span> |<span data-ttu-id="3e9dc-401">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-401">No</span></span> |<span data-ttu-id="3e9dc-402">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-402">EndOfInterval</span></span> |
| <span data-ttu-id="3e9dc-403">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="3e9dc-403">anchorDateTime</span></span> |<span data-ttu-id="3e9dc-404">Definuje absolutní pozici v čase plánovačem slouží k výpočtu hranice řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-404">Defines the absolute position in time used by scheduler to compute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="3e9dc-405"><b>Poznámka:</b>: Pokud AnchorDateTime má částí data, která jsou podrobnější než je četnost pak podrobnější části jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-405"><b>Note</b>: If the AnchorDateTime has date parts that are more granular than the frequency then the more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="3e9dc-406">Například pokud <b>interval</b> je <b>každou hodinu</b> (frekvence: hodinu a intervalu: 1) a <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b> potom <b>minuty a sekundy</b> částí AnchorDateTime jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-406">For example, if the <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and the <b>AnchorDateTime</b> contains <b>minutes and seconds</b> then the <b>minutes and seconds</b> parts of the AnchorDateTime are ignored.</span></span> |<span data-ttu-id="3e9dc-407">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-407">No</span></span> |<span data-ttu-id="3e9dc-408">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="3e9dc-408">01/01/0001</span></span> |
| <span data-ttu-id="3e9dc-409">Posun</span><span class="sxs-lookup"><span data-stu-id="3e9dc-409">offset</span></span> |<span data-ttu-id="3e9dc-410">Časový interval, ve kterém jsou zapuštěno počáteční a koncová všech řezech datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-410">Timespan by which the start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="3e9dc-411"><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem je kombinovaná shift.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-411"><b>Note</b>: If both anchorDateTime and offset are specified, the result is the combined shift.</span></span> |<span data-ttu-id="3e9dc-412">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-412">No</span></span> |<span data-ttu-id="3e9dc-413">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-413">NA</span></span> |

<span data-ttu-id="3e9dc-414">V následující části dostupnosti Určuje, že výstupní datovou sadu je buď vytvořené každou hodinu (nebo) vstupní datovou sadu každou hodinu je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-414">The following availability section specifies that the output dataset is either produced hourly (or) input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="3e9dc-415">**Zásad** oddíl v definici datové sady definuje kritéria nebo podmínku, musíte splnit řezy datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-415">The **policy** section in dataset definition defines the criteria or the condition that the dataset slices must fulfill.</span></span>

| <span data-ttu-id="3e9dc-416">Název zásady</span><span class="sxs-lookup"><span data-stu-id="3e9dc-416">Policy Name</span></span> | <span data-ttu-id="3e9dc-417">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-417">Description</span></span> | <span data-ttu-id="3e9dc-418">Použít</span><span class="sxs-lookup"><span data-stu-id="3e9dc-418">Applied To</span></span> | <span data-ttu-id="3e9dc-419">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-419">Required</span></span> | <span data-ttu-id="3e9dc-420">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e9dc-420">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-421">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="3e9dc-421">minimumSizeMB</span></span> |<span data-ttu-id="3e9dc-422">Ověří, jestli data v **objektů blob v Azure** splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-422">Validates that the data in an **Azure blob** meets the minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="3e9dc-423">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="3e9dc-423">Azure Blob</span></span> |<span data-ttu-id="3e9dc-424">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-424">No</span></span> |<span data-ttu-id="3e9dc-425">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-425">NA</span></span> |
| <span data-ttu-id="3e9dc-426">minimumRows</span><span class="sxs-lookup"><span data-stu-id="3e9dc-426">minimumRows</span></span> |<span data-ttu-id="3e9dc-427">Ověří, jestli data v **Azure SQL database** nebo **tabulky Azure** obsahuje minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-427">Validates that the data in an **Azure SQL database** or an **Azure table** contains the minimum number of rows.</span></span> |<ul><li><span data-ttu-id="3e9dc-428">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3e9dc-428">Azure SQL Database</span></span></li><li><span data-ttu-id="3e9dc-429">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="3e9dc-429">Azure Table</span></span></li></ul> |<span data-ttu-id="3e9dc-430">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-430">No</span></span> |<span data-ttu-id="3e9dc-431">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="3e9dc-431">NA</span></span> |

<span data-ttu-id="3e9dc-432">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-432">**Example:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="3e9dc-433">Není-li datovou sadu se vytváří pomocí Azure Data Factory, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-433">Unless a dataset is being produced by Azure Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="3e9dc-434">Toto nastavení se obvykle platí pro vstupy první aktivitu v kanálu, pokud aktivita nebo řetězení kanálu se používá.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-434">This setting generally applies to the inputs of first activity in a pipeline unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="3e9dc-435">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-435">Name</span></span> | <span data-ttu-id="3e9dc-436">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-436">Description</span></span> | <span data-ttu-id="3e9dc-437">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-437">Required</span></span> | <span data-ttu-id="3e9dc-438">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="3e9dc-438">Default Value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-439">dataDelay</span><span class="sxs-lookup"><span data-stu-id="3e9dc-439">dataDelay</span></span> |<span data-ttu-id="3e9dc-440">Doba zpoždění před kontroly na dostupnost externích dat pro danou řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-440">Time to delay the check on the availability of the external data for the given slice.</span></span> <span data-ttu-id="3e9dc-441">Například data každou hodinu je k dispozici, může být zkontrolujte externích dat je k dispozici a odpovídající řez je připravený zpožděn pomocí dataDelay.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-441">For example, if the data is available hourly, the check to see the external data is available and the corresponding slice is Ready can be delayed by using dataDelay.</span></span><br/><br/><span data-ttu-id="3e9dc-442">Platí jenom pro aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-442">Only applies to the present time.</span></span>  <span data-ttu-id="3e9dc-443">Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření se spustí: 10: 00.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-443">For example, if it is 1:00 PM right now and this value is 10 minutes, the validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="3e9dc-444">Toto nastavení nemá vliv řezy v minulosti (řezy s řez koncový čas + dataDelay < teď) jsou zpracovávány bez jakéhokoli zpoždění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-444">This setting does not affect slices in the past (slices with Slice End Time + dataDelay < Now) are processed without any delay.</span></span><br/><br/><span data-ttu-id="3e9dc-445">Čas větší než 23:59 hodin muset zadat pomocí `day.hours:minutes:seconds` formátu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-445">Time greater than 23:59 hours need to specified using the `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="3e9dc-446">Například pokud chcete zadat 24 hodin, nepoužívejte 24:00:00; Místo toho použijte 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-446">For example, to specify 24 hours, don't use 24:00:00; instead, use 1.00:00:00.</span></span> <span data-ttu-id="3e9dc-447">Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-447">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="3e9dc-448">1 den a 4 hodiny zadejte 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-448">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="3e9dc-449">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-449">No</span></span> |<span data-ttu-id="3e9dc-450">0</span><span class="sxs-lookup"><span data-stu-id="3e9dc-450">0</span></span> |
| <span data-ttu-id="3e9dc-451">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-451">retryInterval</span></span> |<span data-ttu-id="3e9dc-452">Doba čekání mezi selhání a další opakujte pokus.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-452">The wait time between a failure and the next retry attempt.</span></span> <span data-ttu-id="3e9dc-453">Pokud se nezdaří zkuste to, je dalším pokusu o po retryInterval.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-453">If a try fails, the next try is after retryInterval.</span></span> <br/><br/><span data-ttu-id="3e9dc-454">Pokud je 1:00 PM nyní, můžeme začít prvního pokusu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-454">If it is 1:00 PM right now, we begin the first try.</span></span> <span data-ttu-id="3e9dc-455">Pokud doba trvání dokončení první kontrola ověření je 1 minuta a operace se nezdařila, další pokus proběhne v 1:00 + 1 min (doba trvání) + 1 min (interval opakování) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-455">If the duration to complete the first validation check is 1 minute and the operation failed, the next retry is at 1:00 + 1 min (duration) + 1 min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="3e9dc-456">Řezy v minulosti není k dispozici žádné zpoždění není.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-456">For slices in the past, there is no delay.</span></span> <span data-ttu-id="3e9dc-457">Opakovaném dojde okamžitě.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-457">The retry happens immediately.</span></span> |<span data-ttu-id="3e9dc-458">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-458">No</span></span> |<span data-ttu-id="3e9dc-459">00:01:00 (1 min)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-459">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="3e9dc-460">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-460">retryTimeout</span></span> |<span data-ttu-id="3e9dc-461">Časový limit pro jednotlivé pokusy o opakování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-461">The timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="3e9dc-462">Pokud je tato vlastnost nastavena na 10 minut, ověření musí být dokončeny v rámci 10 minut.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-462">If this property is set to 10 minutes, the validation needs to be completed within 10 minutes.</span></span> <span data-ttu-id="3e9dc-463">Pokud trvá déle než 10 minut, aby k ověření, opakovaném časového limitu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-463">If it takes longer than 10 minutes to perform the validation, the retry times out.</span></span><br/><br/><span data-ttu-id="3e9dc-464">Pokud vyprší všechny pokusy o ověření řezu se označí jako TimedOut.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-464">If all attempts for the validation times out, the slice is marked as TimedOut.</span></span> |<span data-ttu-id="3e9dc-465">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-465">No</span></span> |<span data-ttu-id="3e9dc-466">00:10:00 (10 minut)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-466">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="3e9dc-467">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="3e9dc-467">maximumRetry</span></span> |<span data-ttu-id="3e9dc-468">Počet přístupů k zkontrolujte dostupnost externí data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-468">Number of times to check for the availability of the external data.</span></span> <span data-ttu-id="3e9dc-469">Maximální povolená hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-469">The allowed maximum value is 10.</span></span> |<span data-ttu-id="3e9dc-470">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-470">No</span></span> |<span data-ttu-id="3e9dc-471">3</span><span class="sxs-lookup"><span data-stu-id="3e9dc-471">3</span></span> |


## <a name="data-stores"></a><span data-ttu-id="3e9dc-472">DATOVÁ ÚLOŽIŠTĚ</span><span class="sxs-lookup"><span data-stu-id="3e9dc-472">DATA STORES</span></span>
<span data-ttu-id="3e9dc-473">[Propojená služba](#linked-service) části zadat popis pro elementy JSON, které jsou společné pro všechny typy propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-473">The [Linked service](#linked-service) section provided descriptions for JSON elements that are common to all types of linked services.</span></span> <span data-ttu-id="3e9dc-474">Tato část obsahuje podrobnosti o elementy JSON, které jsou specifické pro každé datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-474">This section provides details about JSON elements that are specific to each data store.</span></span>

<span data-ttu-id="3e9dc-475">[Datovou sadu](#dataset) části zadat popis pro elementy JSON, které jsou společné pro všechny typy datových sad.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-475">The [Dataset](#dataset) section provided descriptions for JSON elements that are common to all types of datasets.</span></span> <span data-ttu-id="3e9dc-476">Tato část obsahuje podrobnosti o elementy JSON, které jsou specifické pro každé datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-476">This section provides details about JSON elements that are specific to each data store.</span></span>

<span data-ttu-id="3e9dc-477">[Aktivity](#activity) části zadat popis pro elementy JSON, které jsou společné pro všechny typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-477">The [Activity](#activity) section provided descriptions for JSON elements that are common to all types of activities.</span></span> <span data-ttu-id="3e9dc-478">Tato část obsahuje podrobnosti o elementy JSON, které jsou specifické pro každý úložiště dat, pokud se používá jako zdroj/jímka v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-478">This section provides details about JSON elements that are specific to each data store when it is used as a source/sink in a copy activity.</span></span>  

<span data-ttu-id="3e9dc-479">Kliknutím na odkaz pro úložiště, které vás zajímají zobrazíte schémata JSON propojené služby, datové sady a zdroj/jímka pro aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-479">Click the link for the store you are interested in to see the JSON schemas for linked service, dataset, and the source/sink for the copy activity.</span></span>

| <span data-ttu-id="3e9dc-480">Kategorie</span><span class="sxs-lookup"><span data-stu-id="3e9dc-480">Category</span></span> | <span data-ttu-id="3e9dc-481">Úložiště dat</span><span class="sxs-lookup"><span data-stu-id="3e9dc-481">Data store</span></span> 
|:--- |:--- |
| <span data-ttu-id="3e9dc-482">**Azure**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-482">**Azure**</span></span> |[<span data-ttu-id="3e9dc-483">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-483">Azure Blob storage</span></span>](#azure-blob-storage) |
| &nbsp; |[<span data-ttu-id="3e9dc-484">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3e9dc-484">Azure Data Lake Store</span></span>](#azure-datalake-store) |
| &nbsp; |[<span data-ttu-id="3e9dc-485">Databáze Azure Cosmos</span><span class="sxs-lookup"><span data-stu-id="3e9dc-485">Azure Cosmos DB</span></span>](#azure-cosmos-db) |
| &nbsp; |[<span data-ttu-id="3e9dc-486">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3e9dc-486">Azure SQL Database</span></span>](#azure-sql-database) |
| &nbsp; |[<span data-ttu-id="3e9dc-487">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-487">Azure SQL Data Warehouse</span></span>](#azure-sql-data-warehouse) |
| &nbsp; |[<span data-ttu-id="3e9dc-488">Azure Search</span><span class="sxs-lookup"><span data-stu-id="3e9dc-488">Azure Search</span></span>](#azure-search) |
| &nbsp; |[<span data-ttu-id="3e9dc-489">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-489">Azure Table storage</span></span>](#azure-table-storage) |
| <span data-ttu-id="3e9dc-490">**Databáze**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-490">**Databases**</span></span> |[<span data-ttu-id="3e9dc-491">Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="3e9dc-491">Amazon Redshift</span></span>](#amazon-redshift) |
| &nbsp; |[<span data-ttu-id="3e9dc-492">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="3e9dc-492">IBM DB2</span></span>](#ibm-db2) |
| &nbsp; |[<span data-ttu-id="3e9dc-493">MySQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-493">MySQL</span></span>](#mysql) |
| &nbsp; |[<span data-ttu-id="3e9dc-494">Oracle</span><span class="sxs-lookup"><span data-stu-id="3e9dc-494">Oracle</span></span>](#oracle) |
| &nbsp; |[<span data-ttu-id="3e9dc-495">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-495">PostgreSQL</span></span>](#postgresql) |
| &nbsp; |[<span data-ttu-id="3e9dc-496">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-496">SAP Business Warehouse</span></span>](#sap-business-warehouse) |
| &nbsp; |[<span data-ttu-id="3e9dc-497">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3e9dc-497">SAP HANA</span></span>](#sap-hana) |
| &nbsp; |[<span data-ttu-id="3e9dc-498">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-498">SQL Server</span></span>](#sql-server) |
| &nbsp; |[<span data-ttu-id="3e9dc-499">Sybase</span><span class="sxs-lookup"><span data-stu-id="3e9dc-499">Sybase</span></span>](#sybase) |
| &nbsp; |[<span data-ttu-id="3e9dc-500">Teradata</span><span class="sxs-lookup"><span data-stu-id="3e9dc-500">Teradata</span></span>](#teradata) |
| <span data-ttu-id="3e9dc-501">**NoSQL**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-501">**NoSQL**</span></span> |[<span data-ttu-id="3e9dc-502">Cassandra</span><span class="sxs-lookup"><span data-stu-id="3e9dc-502">Cassandra</span></span>](#cassandra) |
| &nbsp; |[<span data-ttu-id="3e9dc-503">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3e9dc-503">MongoDB</span></span>](#mongodb) |
| <span data-ttu-id="3e9dc-504">**File**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-504">**File**</span></span> |[<span data-ttu-id="3e9dc-505">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="3e9dc-505">Amazon S3</span></span>](#amazon-s3) |
| &nbsp; |[<span data-ttu-id="3e9dc-506">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="3e9dc-506">File System</span></span>](#file-system) |
| &nbsp; |[<span data-ttu-id="3e9dc-507">FTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-507">FTP</span></span>](#ftp) |
| &nbsp; |[<span data-ttu-id="3e9dc-508">HDFS</span><span class="sxs-lookup"><span data-stu-id="3e9dc-508">HDFS</span></span>](#hdfs) |
| &nbsp; |[<span data-ttu-id="3e9dc-509">SFTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-509">SFTP</span></span>](#sftp) |
| <span data-ttu-id="3e9dc-510">**Ostatní**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-510">**Others**</span></span> |[<span data-ttu-id="3e9dc-511">HTTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-511">HTTP</span></span>](#http) |
| &nbsp; |[<span data-ttu-id="3e9dc-512">OData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-512">OData</span></span>](#odata) |
| &nbsp; |[<span data-ttu-id="3e9dc-513">ODBC</span><span class="sxs-lookup"><span data-stu-id="3e9dc-513">ODBC</span></span>](#odbc) |
| &nbsp; |[<span data-ttu-id="3e9dc-514">Salesforce</span><span class="sxs-lookup"><span data-stu-id="3e9dc-514">Salesforce</span></span>](#salesforce) |
| &nbsp; |[<span data-ttu-id="3e9dc-515">Webové tabulky</span><span class="sxs-lookup"><span data-stu-id="3e9dc-515">Web Table</span></span>](#web-table) |

## <a name="azure-blob-storage"></a><span data-ttu-id="3e9dc-516">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-516">Azure Blob Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-517">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-517">Linked service</span></span>
<span data-ttu-id="3e9dc-518">Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-518">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="3e9dc-519">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-519">Azure Storage Linked Service</span></span>
<span data-ttu-id="3e9dc-520">Propojení účtu úložiště Azure data factory pomocí **klíč účtu**, vytvoření služby Azure Storage, propojené.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-520">To link your Azure storage account to a data factory by using the **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="3e9dc-521">K definování Azure Storage propojené služby, nastavte **typ** propojené služby pro **azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-521">To define an Azure Storage linked service, set the **type** of the linked service to **AzureStorage**.</span></span> <span data-ttu-id="3e9dc-522">Potom můžete zadat následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-522">Then, you can specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-523">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-523">Property</span></span> | <span data-ttu-id="3e9dc-524">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-524">Description</span></span> | <span data-ttu-id="3e9dc-525">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-525">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-526">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-526">connectionString</span></span> |<span data-ttu-id="3e9dc-527">Zadejte informace potřebné pro připojení k úložišti Azure pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-527">Specify information needed to connect to Azure storage for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-528">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-528">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="3e9dc-529">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-529">Example</span></span>  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="3e9dc-530">Propojená služba Azure Storage SAS</span><span class="sxs-lookup"><span data-stu-id="3e9dc-530">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="3e9dc-531">Služba Azure úložiště SAS propojené umožňuje propojení účet úložiště Azure do Azure data factory pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-531">The Azure Storage SAS linked service allows you to link an Azure Storage Account to an Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="3e9dc-532">Poskytuje objekt pro vytváření dat omezený nebo časově vázaných přístup k prostředkům všechna nebo konkrétní (kontejner nebo objektů blob) v úložišti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-532">It provides the data factory with restricted/time-bound access to all/specific resources (blob/container) in the storage.</span></span> <span data-ttu-id="3e9dc-533">Propojte si účet úložiště Azure se objekt pro vytváření dat pomocí sdíleného přístupového podpisu, vytvořte SAS služby Azure Storage propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-533">To link your Azure storage account to a data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="3e9dc-534">K definování úložiště SAS Azure propojené služby, nastavte **typ** propojené služby pro **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-534">To define an Azure Storage SAS linked service, set the **type** of the linked service to **AzureStorageSas**.</span></span> <span data-ttu-id="3e9dc-535">Potom můžete zadat následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-535">Then, you can specify following properties in the **typeProperties** section:</span></span>   

| <span data-ttu-id="3e9dc-536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-536">Property</span></span> | <span data-ttu-id="3e9dc-537">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-537">Description</span></span> | <span data-ttu-id="3e9dc-538">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-538">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-539">sasUri</span><span class="sxs-lookup"><span data-stu-id="3e9dc-539">sasUri</span></span> |<span data-ttu-id="3e9dc-540">Zadejte identifikátor URI podpis sdíleného přístupu k prostředkům Azure Storage jako objekt blob, kontejneru nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-540">Specify Shared Access Signature URI to the Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="3e9dc-541">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-541">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="3e9dc-542">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-542">Example</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="3e9dc-543">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-543">For more information about these linked services, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-544">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-544">Dataset</span></span>
<span data-ttu-id="3e9dc-545">Chcete-li definovat datové sadě služby Azure Blob, nastavte **typ** datové sady, která **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-545">To define an Azure Blob dataset, set the **type** of the dataset to **AzureBlob**.</span></span> <span data-ttu-id="3e9dc-546">Potom zadejte následující vlastnosti objektů Blob v Azure konkrétní v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-546">Then, specify the following Azure Blob specific properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-547">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-547">Property</span></span> | <span data-ttu-id="3e9dc-548">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-548">Description</span></span> | <span data-ttu-id="3e9dc-549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-549">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-550">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-550">folderPath</span></span> |<span data-ttu-id="3e9dc-551">Cesta ke kontejneru a složce v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-551">Path to the container and folder in the blob storage.</span></span> <span data-ttu-id="3e9dc-552">Příklad: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="3e9dc-552">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="3e9dc-553">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-553">Yes</span></span> |
| <span data-ttu-id="3e9dc-554">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-554">fileName</span></span> |<span data-ttu-id="3e9dc-555">Název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-555">Name of the blob.</span></span> <span data-ttu-id="3e9dc-556">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-556">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="3e9dc-557">Pokud zadáte název souboru, na konkrétní objekt Blob funguje aktivitu (včetně kopie).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-557">If you specify a filename, the activity (including Copy) works on the specific Blob.</span></span><br/><br/><span data-ttu-id="3e9dc-558">Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-558">When fileName is not specified, Copy includes all Blobs in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="3e9dc-559">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3e9dc-559">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3e9dc-560">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-560">No</span></span> |
| <span data-ttu-id="3e9dc-561">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-561">partitionedBy</span></span> |<span data-ttu-id="3e9dc-562">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-562">partitionedBy is an optional property.</span></span> <span data-ttu-id="3e9dc-563">Můžete ji k určení dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-563">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="3e9dc-564">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-564">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-565">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-565">No</span></span> |
| <span data-ttu-id="3e9dc-566">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-566">format</span></span> | <span data-ttu-id="3e9dc-567">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-567">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-568">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-568">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-569">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-569">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-570">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-570">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-571">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-571">No</span></span> |
| <span data-ttu-id="3e9dc-572">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-572">compression</span></span> | <span data-ttu-id="3e9dc-573">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-573">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-574">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-574">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-575">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-575">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-576">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-576">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-577">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-577">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-578">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-578">Example</span></span>

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


<span data-ttu-id="3e9dc-579">Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-579">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#dataset-properties) article.</span></span>

### <a name="blobsource-in-copy-activity"></a><span data-ttu-id="3e9dc-580">BlobSource v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-580">BlobSource in Copy Activity</span></span>
<span data-ttu-id="3e9dc-581">Pokud jsou kopírování dat z Azure Blob Storage, nastavte **typ zdroje** kopie aktivity na **BlobSource**a zadejte následující vlastnosti v ** zdroj ** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-581">If you are copying data from an Azure Blob Storage, set the **source type** of the copy activity to **BlobSource**, and specify following properties in the **source **section:</span></span>

| <span data-ttu-id="3e9dc-582">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-582">Property</span></span> | <span data-ttu-id="3e9dc-583">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-583">Description</span></span> | <span data-ttu-id="3e9dc-584">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-584">Allowed values</span></span> | <span data-ttu-id="3e9dc-585">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-585">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-586">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-586">recursive</span></span> |<span data-ttu-id="3e9dc-587">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-587">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-588">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-588">True (default value), False</span></span> |<span data-ttu-id="3e9dc-589">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-589">No</span></span> |

#### <a name="example-blobsource"></a><span data-ttu-id="3e9dc-590">Příklad: BlobSource **</span><span class="sxs-lookup"><span data-stu-id="3e9dc-590">Example: BlobSource**</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a><span data-ttu-id="3e9dc-591">BlobSink v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-591">BlobSink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-592">Pokud jsou kopírování dat do Azure Blob Storage, nastavte **typ jímky** kopie aktivity na **BlobSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-592">If you are copying data to an Azure Blob Storage, set the **sink type** of the copy activity to **BlobSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-593">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-593">Property</span></span> | <span data-ttu-id="3e9dc-594">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-594">Description</span></span> | <span data-ttu-id="3e9dc-595">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-595">Allowed values</span></span> | <span data-ttu-id="3e9dc-596">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-596">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-597">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3e9dc-597">copyBehavior</span></span> |<span data-ttu-id="3e9dc-598">Definuje chování kopie, pokud je zdroj BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-598">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="3e9dc-599"><b>PreserveHierarchy</b>: zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-599"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="3e9dc-600">Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-600">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="3e9dc-601"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou v první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-601"><b>FlattenHierarchy</b>: all files from the source folder are in the first level of target folder.</span></span> <span data-ttu-id="3e9dc-602">Cílové soubory mít název automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-602">The target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="3e9dc-603"><b>MergeFiles (výchozí):</b> slučuje všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-603"><b>MergeFiles (default):</b> merges all files from the source folder to one file.</span></span> <span data-ttu-id="3e9dc-604">Pokud je zadán název souboru nebo objekt Blob, název souboru sloučené by být zadaný název; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-604">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="3e9dc-605">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-605">No</span></span> |

#### <a name="example-blobsink"></a><span data-ttu-id="3e9dc-606">Příklad: BlobSink</span><span class="sxs-lookup"><span data-stu-id="3e9dc-606">Example: BlobSink</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-607">Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-607">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-data-lake-store"></a><span data-ttu-id="3e9dc-608">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3e9dc-608">Azure Data Lake Store</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-609">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-609">Linked service</span></span>
<span data-ttu-id="3e9dc-610">K definování Azure Data Lake Store propojené služby, nastavte typ propojené služby pro **AzureDataLakeStore**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-610">To define an Azure Data Lake Store linked service, set the type of the linked service to **AzureDataLakeStore**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-611">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-611">Property</span></span> | <span data-ttu-id="3e9dc-612">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-612">Description</span></span> | <span data-ttu-id="3e9dc-613">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-613">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-614">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-614">type</span></span> | <span data-ttu-id="3e9dc-615">Vlastnost typu musí být nastavena na: **AzureDataLakeStore**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-615">The type property must be set to: **AzureDataLakeStore**</span></span> | <span data-ttu-id="3e9dc-616">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-616">Yes</span></span> |
| <span data-ttu-id="3e9dc-617">dataLakeStoreUri</span><span class="sxs-lookup"><span data-stu-id="3e9dc-617">dataLakeStoreUri</span></span> | <span data-ttu-id="3e9dc-618">Zadejte informace o účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-618">Specify information about the Azure Data Lake Store account.</span></span> <span data-ttu-id="3e9dc-619">Je v následujícím formátu: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-619">It is in the following format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="3e9dc-620">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-620">Yes</span></span> |
| <span data-ttu-id="3e9dc-621">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="3e9dc-621">subscriptionId</span></span> | <span data-ttu-id="3e9dc-622">Id předplatného Azure, ke kterému patří Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-622">Azure subscription Id to which Data Lake Store belongs.</span></span> | <span data-ttu-id="3e9dc-623">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="3e9dc-623">Required for sink</span></span> |
| <span data-ttu-id="3e9dc-624">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3e9dc-624">resourceGroupName</span></span> | <span data-ttu-id="3e9dc-625">Název skupiny prostředků Azure, ke kterému patří Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-625">Azure resource group name to which Data Lake Store belongs.</span></span> | <span data-ttu-id="3e9dc-626">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="3e9dc-626">Required for sink</span></span> |
| <span data-ttu-id="3e9dc-627">servicePrincipalId</span><span class="sxs-lookup"><span data-stu-id="3e9dc-627">servicePrincipalId</span></span> | <span data-ttu-id="3e9dc-628">Zadejte ID aplikace klienta.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-628">Specify the application's client ID.</span></span> | <span data-ttu-id="3e9dc-629">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-629">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3e9dc-630">servicePrincipalKey</span><span class="sxs-lookup"><span data-stu-id="3e9dc-630">servicePrincipalKey</span></span> | <span data-ttu-id="3e9dc-631">Zadejte klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-631">Specify the application's key.</span></span> | <span data-ttu-id="3e9dc-632">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-632">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3e9dc-633">Klienta</span><span class="sxs-lookup"><span data-stu-id="3e9dc-633">tenant</span></span> | <span data-ttu-id="3e9dc-634">Zadejte informace o klienta (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-634">Specify the tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="3e9dc-635">Můžete ji načíst podržením ukazatele myši v pravém horním rohu portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-635">You can retrieve it by hovering the mouse in the top-right corner of the Azure portal.</span></span> | <span data-ttu-id="3e9dc-636">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-636">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="3e9dc-637">Autorizace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-637">authorization</span></span> | <span data-ttu-id="3e9dc-638">Klikněte na tlačítko **Authorize** v tlačítko **editoru služby Data Factory** a zadejte svoje přihlašovací údaje, které přiřadí automaticky generovaný autorizace adresu URL pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-638">Click **Authorize** button in the **Data Factory Editor** and enter your credential that assigns the auto-generated authorization URL to this property.</span></span> | <span data-ttu-id="3e9dc-639">Ano (pro ověření přihlašovacích údajů uživatele)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-639">Yes (for user credential authentication)</span></span>|
| <span data-ttu-id="3e9dc-640">ID relace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-640">sessionId</span></span> | <span data-ttu-id="3e9dc-641">Id relace OAuth z autorizační relace OAuth.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-641">OAuth session id from the OAuth authorization session.</span></span> <span data-ttu-id="3e9dc-642">Každé id relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-642">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="3e9dc-643">Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-643">This setting is automatically generated when you use Data Factory Editor.</span></span> | <span data-ttu-id="3e9dc-644">Ano (pro ověření přihlašovacích údajů uživatele)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-644">Yes (for user credential authentication)</span></span> |

#### <a name="example-using-service-principal-authentication"></a><span data-ttu-id="3e9dc-645">Příklad: použití ověřování hlavní služby</span><span class="sxs-lookup"><span data-stu-id="3e9dc-645">Example: using service principal authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a><span data-ttu-id="3e9dc-646">Příklad: použití ověřování pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-646">Example: using user credential authentication</span></span>
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

<span data-ttu-id="3e9dc-647">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-647">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-648">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-648">Dataset</span></span>
<span data-ttu-id="3e9dc-649">Chcete-li definovat datové sadě služby Azure Data Lake Store, nastavte **typ** datové sady, která **AzureDataLakeStore**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-649">To define an Azure Data Lake Store dataset, set the **type** of the dataset to **AzureDataLakeStore**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-650">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-650">Property</span></span> | <span data-ttu-id="3e9dc-651">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-651">Description</span></span> | <span data-ttu-id="3e9dc-652">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-652">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-653">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-653">folderPath</span></span> |<span data-ttu-id="3e9dc-654">Cesta ke kontejneru a složce v Azure Data Lake úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-654">Path to the container and folder in the Azure Data Lake store.</span></span> |<span data-ttu-id="3e9dc-655">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-655">Yes</span></span> |
| <span data-ttu-id="3e9dc-656">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-656">fileName</span></span> |<span data-ttu-id="3e9dc-657">Název souboru v úložišti Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-657">Name of the file in the Azure Data Lake store.</span></span> <span data-ttu-id="3e9dc-658">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-658">fileName is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="3e9dc-659">Pokud zadáte název souboru, aktivitu (včetně kopie) funguje na konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-659">If you specify a filename, the activity (including Copy) works on the specific file.</span></span><br/><br/><span data-ttu-id="3e9dc-660">Pokud není zadán název souboru, kopie zahrnuje všechny soubory v folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-660">When fileName is not specified, Copy includes all files in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="3e9dc-661">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3e9dc-661">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3e9dc-662">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-662">No</span></span> |
| <span data-ttu-id="3e9dc-663">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-663">partitionedBy</span></span> |<span data-ttu-id="3e9dc-664">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-664">partitionedBy is an optional property.</span></span> <span data-ttu-id="3e9dc-665">Můžete ji k určení dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-665">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="3e9dc-666">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-666">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-667">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-667">No</span></span> |
| <span data-ttu-id="3e9dc-668">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-668">format</span></span> | <span data-ttu-id="3e9dc-669">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-669">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-670">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-670">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-671">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-671">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-672">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-672">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-673">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-673">No</span></span> |
| <span data-ttu-id="3e9dc-674">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-674">compression</span></span> | <span data-ttu-id="3e9dc-675">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-675">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-676">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-676">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-677">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-677">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-678">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-678">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-679">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-679">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-680">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-680">Example</span></span>
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-681">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-681">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-data-lake-store-source-in-copy-activity"></a><span data-ttu-id="3e9dc-682">Azure Data Lake Store zdroj v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-682">Azure Data Lake Store Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-683">Pokud jsou kopírování dat z Azure Data Lake Store, nastavte **typ zdroje** kopie aktivity na **AzureDataLakeStoreSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-683">If you are copying data from an Azure Data Lake Store, set the **source type** of the copy activity to **AzureDataLakeStoreSource**, and specify following properties in the **source** section:</span></span>

<span data-ttu-id="3e9dc-684">**AzureDataLakeStoreSource** podporuje následující vlastnosti **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-684">**AzureDataLakeStoreSource** supports the following properties **typeProperties** section:</span></span>

| <span data-ttu-id="3e9dc-685">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-685">Property</span></span> | <span data-ttu-id="3e9dc-686">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-686">Description</span></span> | <span data-ttu-id="3e9dc-687">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-687">Allowed values</span></span> | <span data-ttu-id="3e9dc-688">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-688">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-689">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-689">recursive</span></span> |<span data-ttu-id="3e9dc-690">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-690">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-691">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-691">True (default value), False</span></span> |<span data-ttu-id="3e9dc-692">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-692">No</span></span> |

#### <a name="example-azuredatalakestoresource"></a><span data-ttu-id="3e9dc-693">Příklad: AzureDataLakeStoreSource</span><span class="sxs-lookup"><span data-stu-id="3e9dc-693">Example: AzureDataLakeStoreSource</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-694">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-694">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span>

### <a name="azure-data-lake-store-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-695">Podřízený Azure Data Lake Store v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-695">Azure Data Lake Store Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-696">Pokud jsou kopírování dat do Azure Data Lake Store, nastavte **typ jímky** kopie aktivity na **AzureDataLakeStoreSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-696">If you are copying data to an Azure Data Lake Store, set the **sink type** of the copy activity to **AzureDataLakeStoreSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-697">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-697">Property</span></span> | <span data-ttu-id="3e9dc-698">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-698">Description</span></span> | <span data-ttu-id="3e9dc-699">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-699">Allowed values</span></span> | <span data-ttu-id="3e9dc-700">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-700">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-701">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3e9dc-701">copyBehavior</span></span> |<span data-ttu-id="3e9dc-702">Určuje chování kopírování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-702">Specifies the copy behavior.</span></span> |<span data-ttu-id="3e9dc-703"><b>PreserveHierarchy</b>: zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-703"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="3e9dc-704">Relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-704">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="3e9dc-705"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky jsou vytvořené v první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-705"><b>FlattenHierarchy</b>: all files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="3e9dc-706">Cílové soubory jsou vytvořeny s názvem automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-706">The target files are created with auto generated name.</span></span><br/><br/><span data-ttu-id="3e9dc-707"><b>MergeFiles</b>: sloučí všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-707"><b>MergeFiles</b>: merges all files from the source folder to one file.</span></span> <span data-ttu-id="3e9dc-708">Pokud je zadán název souboru nebo objekt Blob, název souboru sloučené by být zadaný název; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-708">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="3e9dc-709">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-709">No</span></span> |

#### <a name="example-azuredatalakestoresink"></a><span data-ttu-id="3e9dc-710">Příklad: AzureDataLakeStoreSink</span><span class="sxs-lookup"><span data-stu-id="3e9dc-710">Example: AzureDataLakeStoreSink</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-711">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-711">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-cosmos-db"></a><span data-ttu-id="3e9dc-712">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3e9dc-712">Azure Cosmos DB</span></span>  

### <a name="linked-service"></a><span data-ttu-id="3e9dc-713">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-713">Linked service</span></span>
<span data-ttu-id="3e9dc-714">K definování Azure DB Cosmos propojené služby, nastavte **typ** propojené služby pro **DocumentDb**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-714">To define an Azure Cosmos DB linked service, set the **type** of the linked service to **DocumentDb**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-715">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-715">**Property**</span></span> | <span data-ttu-id="3e9dc-716">**Popis**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-716">**Description**</span></span> | <span data-ttu-id="3e9dc-717">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-717">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-718">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-718">connectionString</span></span> |<span data-ttu-id="3e9dc-719">Zadejte informace potřebné pro připojení k databázi Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-719">Specify information needed to connect to Azure Cosmos DB database.</span></span> |<span data-ttu-id="3e9dc-720">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-720">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-721">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-721">Example</span></span>

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
<span data-ttu-id="3e9dc-722">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-722">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-723">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-723">Dataset</span></span>
<span data-ttu-id="3e9dc-724">Chcete-li definovat datové sadě služby Azure Cosmos DB, nastavte **typ** datové sady, která **DocumentDbCollection**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-724">To define an Azure Cosmos DB dataset, set the **type** of the dataset to **DocumentDbCollection**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-725">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-725">**Property**</span></span> | <span data-ttu-id="3e9dc-726">**Popis**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-726">**Description**</span></span> | <span data-ttu-id="3e9dc-727">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-727">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-728">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="3e9dc-728">collectionName</span></span> |<span data-ttu-id="3e9dc-729">Název kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-729">Name of the Azure Cosmos DB collection.</span></span> |<span data-ttu-id="3e9dc-730">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-730">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-731">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-731">Example</span></span>

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="3e9dc-732">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-732">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) article.</span></span>

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a><span data-ttu-id="3e9dc-733">Zdroj kolekce Azure Cosmos DB v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-733">Azure Cosmos DB Collection Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-734">Pokud kopírujete data z databáze Cosmos Azure, nastavte **typ zdroje** kopie aktivity na **DocumentDbCollectionSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-734">If you are copying data from an Azure Cosmos DB, set the **source type** of the copy activity to **DocumentDbCollectionSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-735">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-735">**Property**</span></span> | <span data-ttu-id="3e9dc-736">**Popis**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-736">**Description**</span></span> | <span data-ttu-id="3e9dc-737">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-737">**Allowed values**</span></span> | <span data-ttu-id="3e9dc-738">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-738">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-739">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-739">query</span></span> |<span data-ttu-id="3e9dc-740">Zadejte dotaz číst data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-740">Specify the query to read data.</span></span> |<span data-ttu-id="3e9dc-741">Řetězec nepodporuje Azure Cosmos DB dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-741">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="3e9dc-742">Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-742">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="3e9dc-743">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-743">No</span></span> <br/><br/><span data-ttu-id="3e9dc-744">Pokud není zadaný příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-744">If not specified, the SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="3e9dc-745">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="3e9dc-745">nestingSeparator</span></span> |<span data-ttu-id="3e9dc-746">Speciální znak indikující, že dokument je vnořený</span><span class="sxs-lookup"><span data-stu-id="3e9dc-746">Special character to indicate that the document is nested</span></span> |<span data-ttu-id="3e9dc-747">Libovolný znak.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-747">Any character.</span></span> <br/><br/><span data-ttu-id="3e9dc-748">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-748">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="3e9dc-749">Azure Data Factory umožňuje uživateli označují hierarchie prostřednictvím nestingSeparator, což je "."</span><span class="sxs-lookup"><span data-stu-id="3e9dc-749">Azure Data Factory enables user to denote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="3e9dc-750">v předchozích příkladech.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-750">in the above examples.</span></span> <span data-ttu-id="3e9dc-751">S oddělovačem, aktivitě kopírování bude generovat objekt "Name" tři podřízené elementy nejprve, střední a příjmení podle "Name.First", "Name.Middle" a "Name.Last" v definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-751">With the separator, the copy activity will generate the “Name” object with three children elements First, Middle and Last, according to “Name.First”, “Name.Middle” and “Name.Last” in the table definition.</span></span> |<span data-ttu-id="3e9dc-752">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-752">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-753">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-753">Example</span></span>

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-754">Azure Cosmos DB kolekce podřízený v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-754">Azure Cosmos DB Collection Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-755">Pokud jsou kopírování dat do Azure Cosmos DB, nastavte **typ jímky** kopie aktivity na **DocumentDbCollectionSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-755">If you are copying data to Azure Cosmos DB, set the **sink type** of the copy activity to **DocumentDbCollectionSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-756">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-756">**Property**</span></span> | <span data-ttu-id="3e9dc-757">**Popis**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-757">**Description**</span></span> | <span data-ttu-id="3e9dc-758">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-758">**Allowed values**</span></span> | <span data-ttu-id="3e9dc-759">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-759">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-760">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="3e9dc-760">nestingSeparator</span></span> |<span data-ttu-id="3e9dc-761">Budete potřebovat speciální znak v názvu sloupce zdroj označíte, že vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-761">A special character in the source column name to indicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="3e9dc-762">Například výše: `Name.First` ve výstupu tabulky vytvoří následující strukturu JSON v dokumentu Cosmos DB:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-762">For example above: `Name.First` in the output table produces the following JSON structure in the Cosmos DB document:</span></span><br/><br/><span data-ttu-id="3e9dc-763">"Název": {</span><span class="sxs-lookup"><span data-stu-id="3e9dc-763">"Name": {</span></span><br/>    <span data-ttu-id="3e9dc-764">"První": "Jan"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-764">"First": "John"</span></span><br/><span data-ttu-id="3e9dc-765">},</span><span class="sxs-lookup"><span data-stu-id="3e9dc-765">},</span></span> |<span data-ttu-id="3e9dc-766">Znak, který se používá k oddělení úrovní vnoření.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-766">Character that is used to separate nesting levels.</span></span><br/><br/><span data-ttu-id="3e9dc-767">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-767">Default value is `.` (dot).</span></span> |<span data-ttu-id="3e9dc-768">Znak, který se používá k oddělení úrovní vnoření.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-768">Character that is used to separate nesting levels.</span></span> <br/><br/><span data-ttu-id="3e9dc-769">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-769">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="3e9dc-770">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-770">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-771">Počet paralelní požadavků do služby Azure Cosmos DB vytvářet dokumenty.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-771">Number of parallel requests to Azure Cosmos DB service to create documents.</span></span><br/><br/><span data-ttu-id="3e9dc-772">Při kopírování dat z Azure Cosmos DB pomocí této vlastnosti lze optimalizovat výkon.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-772">You can fine-tune the performance when copying data to/from Azure Cosmos DB by using this property.</span></span> <span data-ttu-id="3e9dc-773">Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní požadavky pro Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-773">You can expect a better performance when you increase writeBatchSize because more parallel requests to Azure Cosmos DB are sent.</span></span> <span data-ttu-id="3e9dc-774">Ale budete muset vyhnout, omezení šířky pásma, který lze vyvolat chybovou zprávu: "Požadavků je velká".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-774">However you’ll need to avoid throttling that can throw the error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="3e9dc-775">Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít kolekci lepší (například S3) tak, aby měl nejvíce propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-775">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (for example, S3) to have the most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="3e9dc-776">Integer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-776">Integer</span></span> |<span data-ttu-id="3e9dc-777">Ne (výchozí: 5)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-777">No (default: 5)</span></span> |
| <span data-ttu-id="3e9dc-778">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-778">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-779">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-779">Wait time for the operation to complete before it times out.</span></span> |<span data-ttu-id="3e9dc-780">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-780">timespan</span></span><br/><br/> <span data-ttu-id="3e9dc-781">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-781">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3e9dc-782">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-782">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-783">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-783">Example</span></span>

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

<span data-ttu-id="3e9dc-784">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-784">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="3e9dc-785">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3e9dc-785">Azure SQL Database</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-786">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-786">Linked service</span></span>
<span data-ttu-id="3e9dc-787">K definování Azure SQL Database propojené služby, nastavte **typ** propojené služby pro **azuresqldatabase**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-787">To define an Azure SQL Database linked service, set the **type** of the linked service to **AzureSqlDatabase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-788">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-788">Property</span></span> | <span data-ttu-id="3e9dc-789">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-789">Description</span></span> | <span data-ttu-id="3e9dc-790">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-790">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-791">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-791">connectionString</span></span> |<span data-ttu-id="3e9dc-792">Zadejte informace potřebné pro připojení k instanci databáze SQL Azure pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-792">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-793">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-793">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-794">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-794">Example</span></span>
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3e9dc-795">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-795">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-796">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-796">Dataset</span></span>
<span data-ttu-id="3e9dc-797">Chcete-li definovat datové sadě služby Azure SQL Database, nastavte **typ** datové sady, která **AzureSqlTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-797">To define an Azure SQL Database dataset, set the **type** of the dataset to **AzureSqlTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-798">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-798">Property</span></span> | <span data-ttu-id="3e9dc-799">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-799">Description</span></span> | <span data-ttu-id="3e9dc-800">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-800">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-801">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-801">tableName</span></span> |<span data-ttu-id="3e9dc-802">Název tabulky nebo zobrazení instance databáze SQL Azure, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-802">Name of the table or view in the Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-803">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-803">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-804">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-804">Example</span></span>

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3e9dc-805">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-805">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="3e9dc-806">Zdroje SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-806">SQL Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-807">Pokud kopírujete data z databáze SQL Azure, nastavte **typ zdroje** kopie aktivity na **SqlSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-807">If you are copying data from an Azure SQL Database, set the **source type** of the copy activity to **SqlSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-808">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-808">Property</span></span> | <span data-ttu-id="3e9dc-809">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-809">Description</span></span> | <span data-ttu-id="3e9dc-810">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-810">Allowed values</span></span> | <span data-ttu-id="3e9dc-811">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-811">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-812">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3e9dc-812">sqlReaderQuery</span></span> |<span data-ttu-id="3e9dc-813">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-813">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-814">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-814">SQL query string.</span></span> <span data-ttu-id="3e9dc-815">Příklad: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-815">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-816">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-816">No</span></span> |
| <span data-ttu-id="3e9dc-817">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-817">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3e9dc-818">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-818">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="3e9dc-819">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-819">Name of the stored procedure.</span></span> |<span data-ttu-id="3e9dc-820">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-820">No</span></span> |
| <span data-ttu-id="3e9dc-821">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-821">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-822">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-822">Parameters for the stored procedure.</span></span> |<span data-ttu-id="3e9dc-823">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-823">Name/value pairs.</span></span> <span data-ttu-id="3e9dc-824">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-824">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="3e9dc-825">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-825">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-826">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-826">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3e9dc-827">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-827">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-828">Jímku SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-828">SQL Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-829">Pokud data kopírujete do Azure SQL Database, nastavte **typ jímky** kopie aktivity na **SqlSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-829">If you are copying data to Azure SQL Database, set the **sink type** of the copy activity to **SqlSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-830">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-830">Property</span></span> | <span data-ttu-id="3e9dc-831">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-831">Description</span></span> | <span data-ttu-id="3e9dc-832">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-832">Allowed values</span></span> | <span data-ttu-id="3e9dc-833">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-833">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-834">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-834">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-835">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-835">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="3e9dc-836">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-836">timespan</span></span><br/><br/> <span data-ttu-id="3e9dc-837">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-837">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3e9dc-838">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-838">No</span></span> |
| <span data-ttu-id="3e9dc-839">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-839">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-840">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-840">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3e9dc-841">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-841">Integer (number of rows)</span></span> |<span data-ttu-id="3e9dc-842">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-842">No (default: 10000)</span></span> |
| <span data-ttu-id="3e9dc-843">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-843">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3e9dc-844">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-844">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3e9dc-845">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-845">A query statement.</span></span> |<span data-ttu-id="3e9dc-846">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-846">No</span></span> |
| <span data-ttu-id="3e9dc-847">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-847">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3e9dc-848">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-848">Specify a column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="3e9dc-849">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-849">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3e9dc-850">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-850">No</span></span> |
| <span data-ttu-id="3e9dc-851">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-851">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="3e9dc-852">Název uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-852">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="3e9dc-853">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-853">Name of the stored procedure.</span></span> |<span data-ttu-id="3e9dc-854">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-854">No</span></span> |
| <span data-ttu-id="3e9dc-855">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-855">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-856">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-856">Parameters for the stored procedure.</span></span> |<span data-ttu-id="3e9dc-857">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-857">Name/value pairs.</span></span> <span data-ttu-id="3e9dc-858">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-858">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="3e9dc-859">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-859">No</span></span> |
| <span data-ttu-id="3e9dc-860">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-860">sqlWriterTableType</span></span> |<span data-ttu-id="3e9dc-861">Zadejte název typu tabulky má být použit v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-861">Specify a table type name to be used in the stored procedure.</span></span> <span data-ttu-id="3e9dc-862">Aktivita kopírování zpřístupní přesouvání dat v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-862">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="3e9dc-863">Uložená procedura kód pak sloučit data kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-863">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="3e9dc-864">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-864">A table type name.</span></span> |<span data-ttu-id="3e9dc-865">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-865">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-866">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-866">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-867">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-867">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="3e9dc-868">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-868">Azure SQL Data Warehouse</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-869">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-869">Linked service</span></span>
<span data-ttu-id="3e9dc-870">K definování Azure SQL Data Warehouse propojené služby, nastavte **typ** propojené služby pro **AzureSqlDW**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-870">To define an Azure SQL Data Warehouse linked service, set the **type** of the linked service to **AzureSqlDW**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-871">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-871">Property</span></span> | <span data-ttu-id="3e9dc-872">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-872">Description</span></span> | <span data-ttu-id="3e9dc-873">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-873">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-874">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-874">connectionString</span></span> |<span data-ttu-id="3e9dc-875">Zadejte informace potřebné pro připojení k Azure SQL Data Warehouse instance pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-875">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-876">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-876">Yes</span></span> |



#### <a name="example"></a><span data-ttu-id="3e9dc-877">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-877">Example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3e9dc-878">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-878">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-879">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-879">Dataset</span></span>
<span data-ttu-id="3e9dc-880">Chcete-li definovat datové sadě služby Azure SQL Data Warehouse, nastavte **typ** datové sady, která **AzureSqlDWTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-880">To define an Azure SQL Data Warehouse dataset, set the **type** of the dataset to **AzureSqlDWTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-881">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-881">Property</span></span> | <span data-ttu-id="3e9dc-882">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-882">Description</span></span> | <span data-ttu-id="3e9dc-883">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-883">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-884">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-884">tableName</span></span> |<span data-ttu-id="3e9dc-885">Název tabulky nebo zobrazení v databázi Azure SQL Data Warehouse, která propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-885">Name of the table or view in the Azure SQL Data Warehouse database that the linked service refers to.</span></span> |<span data-ttu-id="3e9dc-886">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-886">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-887">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-887">Example</span></span>

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-888">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-888">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-dw-source-in-copy-activity"></a><span data-ttu-id="3e9dc-889">Zdroj datového skladu SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-889">SQL DW Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-890">Pokud jsou kopírování dat z Azure SQL Data Warehouse, nastavte **typ zdroje** kopie aktivity na **SqlDWSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-890">If you are copying data from Azure SQL Data Warehouse, set the **source type** of the copy activity to **SqlDWSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-891">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-891">Property</span></span> | <span data-ttu-id="3e9dc-892">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-892">Description</span></span> | <span data-ttu-id="3e9dc-893">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-893">Allowed values</span></span> | <span data-ttu-id="3e9dc-894">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-894">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-895">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3e9dc-895">sqlReaderQuery</span></span> |<span data-ttu-id="3e9dc-896">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-896">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-897">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-897">SQL query string.</span></span> <span data-ttu-id="3e9dc-898">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-898">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-899">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-899">No</span></span> |
| <span data-ttu-id="3e9dc-900">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-900">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3e9dc-901">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-901">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="3e9dc-902">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-902">Name of the stored procedure.</span></span> |<span data-ttu-id="3e9dc-903">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-903">No</span></span> |
| <span data-ttu-id="3e9dc-904">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-904">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-905">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-905">Parameters for the stored procedure.</span></span> |<span data-ttu-id="3e9dc-906">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-906">Name/value pairs.</span></span> <span data-ttu-id="3e9dc-907">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-907">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="3e9dc-908">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-908">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-909">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-909">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-910">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-910">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-dw-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-911">Podřízený datového skladu SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-911">SQL DW Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-912">Pokud jsou kopírování dat do Azure SQL Data Warehouse, nastavte **typ jímky** kopie aktivity na **SqlDWSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-912">If you are copying data to Azure SQL Data Warehouse, set the **sink type** of the copy activity to **SqlDWSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-913">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-913">Property</span></span> | <span data-ttu-id="3e9dc-914">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-914">Description</span></span> | <span data-ttu-id="3e9dc-915">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-915">Allowed values</span></span> | <span data-ttu-id="3e9dc-916">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-916">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-917">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-917">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3e9dc-918">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-918">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3e9dc-919">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-919">A query statement.</span></span> |<span data-ttu-id="3e9dc-920">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-920">No</span></span> |
| <span data-ttu-id="3e9dc-921">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="3e9dc-921">allowPolyBase</span></span> |<span data-ttu-id="3e9dc-922">Označuje, zda místo hromadné vložení mechanismus použít PolyBase (v případě potřeby).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-922">Indicates whether to use PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="3e9dc-923">**Pomocí PolyBase je doporučeným způsobem, jak načíst data do SQL Data Warehouse.**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-923">**Using PolyBase is the recommended way to load data into SQL Data Warehouse.**</span></span> |<span data-ttu-id="3e9dc-924">True</span><span class="sxs-lookup"><span data-stu-id="3e9dc-924">True</span></span> <br/><span data-ttu-id="3e9dc-925">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-925">False (default)</span></span> |<span data-ttu-id="3e9dc-926">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-926">No</span></span> |
| <span data-ttu-id="3e9dc-927">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="3e9dc-927">polyBaseSettings</span></span> |<span data-ttu-id="3e9dc-928">Skupinu vlastností, které se dají zadat při **allowPolybase** je nastavena na **true**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-928">A group of properties that can be specified when the **allowPolybase** property is set to **true**.</span></span> |&nbsp; |<span data-ttu-id="3e9dc-929">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-929">No</span></span> |
| <span data-ttu-id="3e9dc-930">rejectValue</span><span class="sxs-lookup"><span data-stu-id="3e9dc-930">rejectValue</span></span> |<span data-ttu-id="3e9dc-931">Určuje číslo nebo podíl řádků, které může být odmítnutá předtím, než se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-931">Specifies the number or percentage of rows that can be rejected before the query fails.</span></span> <br/><br/><span data-ttu-id="3e9dc-932">Další informace o možnostech odmítněte používání funkce PolyBase v **argumenty** části [vytvořit EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-932">Learn more about the PolyBase’s reject options in the **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="3e9dc-933">0 (výchozí), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="3e9dc-933">0 (default), 1, 2, …</span></span> |<span data-ttu-id="3e9dc-934">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-934">No</span></span> |
| <span data-ttu-id="3e9dc-935">rejectType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-935">rejectType</span></span> |<span data-ttu-id="3e9dc-936">Určuje, zda je hodnota literálu nebo jako procento zadána možnost rejectValue.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-936">Specifies whether the rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="3e9dc-937">Hodnota (výchozí), procento</span><span class="sxs-lookup"><span data-stu-id="3e9dc-937">Value (default), Percentage</span></span> |<span data-ttu-id="3e9dc-938">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-938">No</span></span> |
| <span data-ttu-id="3e9dc-939">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="3e9dc-939">rejectSampleValue</span></span> |<span data-ttu-id="3e9dc-940">Určuje počet řádků k načtení předtím, než PolyBase přepočítá procento odmítnutých řádků.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-940">Determines the number of rows to retrieve before the PolyBase recalculates the percentage of rejected rows.</span></span> |<span data-ttu-id="3e9dc-941">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="3e9dc-941">1, 2, …</span></span> |<span data-ttu-id="3e9dc-942">Ano, pokud **rejectType** je **procento**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-942">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="3e9dc-943">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="3e9dc-943">useTypeDefault</span></span> |<span data-ttu-id="3e9dc-944">Určuje způsob zpracování chybějící hodnoty v textových souborů s oddělovači, když PolyBase načítá data z textového souboru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-944">Specifies how to handle missing values in delimited text files when PolyBase retrieves data from the text file.</span></span><br/><br/><span data-ttu-id="3e9dc-945">Další informace o této vlastnosti v části argumenty [vytvořit EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-945">Learn more about this property from the Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="3e9dc-946">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-946">True, False (default)</span></span> |<span data-ttu-id="3e9dc-947">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-947">No</span></span> |
| <span data-ttu-id="3e9dc-948">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-948">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-949">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-949">Inserts data into the SQL table when the buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="3e9dc-950">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-950">Integer (number of rows)</span></span> |<span data-ttu-id="3e9dc-951">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-951">No (default: 10000)</span></span> |
| <span data-ttu-id="3e9dc-952">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-952">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-953">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-953">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="3e9dc-954">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-954">timespan</span></span><br/><br/> <span data-ttu-id="3e9dc-955">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-955">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3e9dc-956">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-956">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-957">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-957">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-958">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-958">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-search"></a><span data-ttu-id="3e9dc-959">Azure Search</span><span class="sxs-lookup"><span data-stu-id="3e9dc-959">Azure Search</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-960">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-960">Linked service</span></span>
<span data-ttu-id="3e9dc-961">K definování Azure Search propojené služby, nastavte **typ** propojené služby pro **AzureSearch**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-961">To define an Azure Search linked service, set the **type** of the linked service to **AzureSearch**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-962">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-962">Property</span></span> | <span data-ttu-id="3e9dc-963">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-963">Description</span></span> | <span data-ttu-id="3e9dc-964">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-964">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e9dc-965">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-965">url</span></span> | <span data-ttu-id="3e9dc-966">Adresa URL pro službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-966">URL for the Azure Search service.</span></span> | <span data-ttu-id="3e9dc-967">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-967">Yes</span></span> |
| <span data-ttu-id="3e9dc-968">key</span><span class="sxs-lookup"><span data-stu-id="3e9dc-968">key</span></span> | <span data-ttu-id="3e9dc-969">Klíč správce pro službu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-969">Admin key for the Azure Search service.</span></span> | <span data-ttu-id="3e9dc-970">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-970">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-971">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-971">Example</span></span>

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

<span data-ttu-id="3e9dc-972">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-972">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-973">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-973">Dataset</span></span>
<span data-ttu-id="3e9dc-974">Chcete-li definovat datové sadě služby Azure Search, nastavte **typ** datové sady, která **AzureSearchIndex**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-974">To define an Azure Search dataset, set the **type** of the dataset to **AzureSearchIndex**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-975">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-975">Property</span></span> | <span data-ttu-id="3e9dc-976">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-976">Description</span></span> | <span data-ttu-id="3e9dc-977">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-977">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e9dc-978">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-978">type</span></span> | <span data-ttu-id="3e9dc-979">Vlastnost typu musí být nastavená na **AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-979">The type property must be set to **AzureSearchIndex**.</span></span>| <span data-ttu-id="3e9dc-980">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-980">Yes</span></span> |
| <span data-ttu-id="3e9dc-981">indexName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-981">indexName</span></span> | <span data-ttu-id="3e9dc-982">Název indexu Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-982">Name of the Azure Search index.</span></span> <span data-ttu-id="3e9dc-983">Objekt pro vytváření dat vytvořit index.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-983">Data Factory does not create the index.</span></span> <span data-ttu-id="3e9dc-984">Index musí existovat ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-984">The index must exist in Azure Search.</span></span> | <span data-ttu-id="3e9dc-985">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-985">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-986">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-986">Example</span></span>

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

<span data-ttu-id="3e9dc-987">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-987">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) article.</span></span>

### <a name="azure-search-index-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-988">Podřízený Index Azure Search v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-988">Azure Search Index Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-989">Pokud jsou kopírování dat do indexu Azure Search, nastavte **typ jímky** kopie aktivity na **AzureSearchIndexSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-989">If you are copying data to an Azure Search index, set the **sink type** of the copy activity to **AzureSearchIndexSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-990">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-990">Property</span></span> | <span data-ttu-id="3e9dc-991">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-991">Description</span></span> | <span data-ttu-id="3e9dc-992">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-992">Allowed values</span></span> | <span data-ttu-id="3e9dc-993">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-993">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="3e9dc-994">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="3e9dc-994">WriteBehavior</span></span> | <span data-ttu-id="3e9dc-995">Určuje, jestli se má sloučit nebo nahradit, pokud již dokument v indexu existuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-995">Specifies whether to merge or replace when a document already exists in the index.</span></span> | <span data-ttu-id="3e9dc-996">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-996">Merge (default)</span></span><br/><span data-ttu-id="3e9dc-997">Odeslat</span><span class="sxs-lookup"><span data-stu-id="3e9dc-997">Upload</span></span>| <span data-ttu-id="3e9dc-998">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-998">No</span></span> |
| <span data-ttu-id="3e9dc-999">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-999">WriteBatchSize</span></span> | <span data-ttu-id="3e9dc-1000">Ukládání dat do indexu Azure Search, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1000">Uploads data into the Azure Search index when the buffer size reaches writeBatchSize.</span></span> | <span data-ttu-id="3e9dc-1001">1 do 1000.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1001">1 to 1,000.</span></span> <span data-ttu-id="3e9dc-1002">Výchozí hodnota je 1 000.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1002">Default value is 1000.</span></span> | <span data-ttu-id="3e9dc-1003">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1003">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1004">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1004">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1005">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1005">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-table-storage"></a><span data-ttu-id="3e9dc-1006">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1006">Azure Table Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1007">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1007">Linked service</span></span>
<span data-ttu-id="3e9dc-1008">Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1008">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="3e9dc-1009">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1009">Azure Storage Linked Service</span></span>
<span data-ttu-id="3e9dc-1010">Propojení účtu úložiště Azure data factory pomocí **klíč účtu**, vytvoření služby Azure Storage, propojené.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1010">To link your Azure storage account to a data factory by using the **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="3e9dc-1011">K definování Azure Storage propojené služby, nastavte **typ** propojené služby pro **azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1011">To define an Azure Storage linked service, set the **type** of the linked service to **AzureStorage**.</span></span> <span data-ttu-id="3e9dc-1012">Potom můžete zadat následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1012">Then, you can specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1013">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1013">Property</span></span> | <span data-ttu-id="3e9dc-1014">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1014">Description</span></span> | <span data-ttu-id="3e9dc-1015">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1015">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-1016">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1016">type</span></span> |<span data-ttu-id="3e9dc-1017">Vlastnost typu musí být nastavena na: **azurestorage.**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1017">The type property must be set to: **AzureStorage**</span></span> |<span data-ttu-id="3e9dc-1018">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1018">Yes</span></span> |
| <span data-ttu-id="3e9dc-1019">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1019">connectionString</span></span> |<span data-ttu-id="3e9dc-1020">Zadejte informace potřebné pro připojení k úložišti Azure pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1020">Specify information needed to connect to Azure storage for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-1021">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1021">Yes</span></span> |

<span data-ttu-id="3e9dc-1022">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1022">**Example:**</span></span>  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="3e9dc-1023">Propojená služba Azure Storage SAS</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1023">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="3e9dc-1024">Služba Azure úložiště SAS propojené umožňuje propojení účet úložiště Azure do Azure data factory pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1024">The Azure Storage SAS linked service allows you to link an Azure Storage Account to an Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="3e9dc-1025">Poskytuje objekt pro vytváření dat omezený nebo časově vázaných přístup k prostředkům všechna nebo konkrétní (kontejner nebo objektů blob) v úložišti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1025">It provides the data factory with restricted/time-bound access to all/specific resources (blob/container) in the storage.</span></span> <span data-ttu-id="3e9dc-1026">Propojte si účet úložiště Azure se objekt pro vytváření dat pomocí sdíleného přístupového podpisu, vytvořte SAS služby Azure Storage propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1026">To link your Azure storage account to a data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="3e9dc-1027">K definování úložiště SAS Azure propojené služby, nastavte **typ** propojené služby pro **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1027">To define an Azure Storage SAS linked service, set the **type** of the linked service to **AzureStorageSas**.</span></span> <span data-ttu-id="3e9dc-1028">Potom můžete zadat následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1028">Then, you can specify following properties in the **typeProperties** section:</span></span>   

| <span data-ttu-id="3e9dc-1029">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1029">Property</span></span> | <span data-ttu-id="3e9dc-1030">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1030">Description</span></span> | <span data-ttu-id="3e9dc-1031">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1031">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-1032">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1032">type</span></span> |<span data-ttu-id="3e9dc-1033">Vlastnost typu musí být nastavena na: **AzureStorageSas**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1033">The type property must be set to: **AzureStorageSas**</span></span> |<span data-ttu-id="3e9dc-1034">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1034">Yes</span></span> |
| <span data-ttu-id="3e9dc-1035">sasUri</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1035">sasUri</span></span> |<span data-ttu-id="3e9dc-1036">Zadejte identifikátor URI podpis sdíleného přístupu k prostředkům Azure Storage jako objekt blob, kontejneru nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1036">Specify Shared Access Signature URI to the Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="3e9dc-1037">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1037">Yes</span></span> |

<span data-ttu-id="3e9dc-1038">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1038">**Example:**</span></span>

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

<span data-ttu-id="3e9dc-1039">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1039">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1040">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1040">Dataset</span></span>
<span data-ttu-id="3e9dc-1041">Chcete-li definovat datové sadě služby Azure Table, nastavte **typ** datové sady, která **AzureTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1041">To define an Azure Table dataset, set the **type** of the dataset to **AzureTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1042">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1042">Property</span></span> | <span data-ttu-id="3e9dc-1043">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1043">Description</span></span> | <span data-ttu-id="3e9dc-1044">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1044">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1045">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1045">tableName</span></span> |<span data-ttu-id="3e9dc-1046">Název tabulky instance Azure tabulku databáze, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1046">Name of the table in the Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1047">Ano.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1047">Yes.</span></span> <span data-ttu-id="3e9dc-1048">Pokud název tabulky je zadán bez azureTableSourceQuery, všechny záznamy z tabulky se zkopírují do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1048">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="3e9dc-1049">Pokud je zadána také azureTableSourceQuery, záznamy z tabulky, která splňuje dotaz zkopírují do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1049">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1050">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1050">Example</span></span>

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1051">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1051">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-table-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1052">Zdrojové tabulky Azure v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1052">Azure Table Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1053">Pokud jsou kopírování dat z úložiště tabulek Azure, nastavte **typ zdroje** kopie aktivity na **AzureTableSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1053">If you are copying data from Azure Table Storage, set the **source type** of the copy activity to **AzureTableSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1054">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1054">Property</span></span> | <span data-ttu-id="3e9dc-1055">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1055">Description</span></span> | <span data-ttu-id="3e9dc-1056">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1056">Allowed values</span></span> | <span data-ttu-id="3e9dc-1057">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1057">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1058">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1058">azureTableSourceQuery</span></span> |<span data-ttu-id="3e9dc-1059">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1059">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1060">Řetězec dotazu tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1060">Azure table query string.</span></span> <span data-ttu-id="3e9dc-1061">Příklady v další části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1061">See examples in the next section.</span></span> |<span data-ttu-id="3e9dc-1062">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1062">No.</span></span> <span data-ttu-id="3e9dc-1063">Pokud název tabulky je zadán bez azureTableSourceQuery, všechny záznamy z tabulky se zkopírují do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1063">When a tableName is specified without an azureTableSourceQuery, all records from the table are copied to the destination.</span></span> <span data-ttu-id="3e9dc-1064">Pokud je zadána také azureTableSourceQuery, záznamy z tabulky, která splňuje dotaz zkopírují do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1064">If an azureTableSourceQuery is also specified, records from the table that satisfies the query are copied to the destination.</span></span> |
| <span data-ttu-id="3e9dc-1065">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1065">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="3e9dc-1066">Označuje, zda swallow výjimky tabulky neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1066">Indicate whether swallow the exception of table not exist.</span></span> |<span data-ttu-id="3e9dc-1067">HODNOTA TRUE</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1067">TRUE</span></span><br/><span data-ttu-id="3e9dc-1068">FALSE</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1068">FALSE</span></span> |<span data-ttu-id="3e9dc-1069">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1069">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1070">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1070">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1071">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1071">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

### <a name="azure-table-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-1072">Podřízený tabulky Azure v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1072">Azure Table Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1073">Pokud jsou kopírování dat do Azure Table Storage, nastavte **typ jímky** kopie aktivity na **AzureTableSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1073">If you are copying data to Azure Table Storage, set the **sink type** of the copy activity to **AzureTableSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-1074">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1074">Property</span></span> | <span data-ttu-id="3e9dc-1075">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1075">Description</span></span> | <span data-ttu-id="3e9dc-1076">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1076">Allowed values</span></span> | <span data-ttu-id="3e9dc-1077">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1077">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1078">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1078">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="3e9dc-1079">Výchozí hodnotu klíče oddílu, mohou být využívána jímky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1079">Default partition key value that can be used by the sink.</span></span> |<span data-ttu-id="3e9dc-1080">Hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1080">A string value.</span></span> |<span data-ttu-id="3e9dc-1081">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1081">No</span></span> |
| <span data-ttu-id="3e9dc-1082">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1082">azureTablePartitionKeyName</span></span> |<span data-ttu-id="3e9dc-1083">Zadejte název sloupce, jejichž hodnoty se používají jako klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1083">Specify name of the column whose values are used as partition keys.</span></span> <span data-ttu-id="3e9dc-1084">Pokud není zadaný, použije se AzureTableDefaultPartitionKeyValue jako klíč oddílu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1084">If not specified, AzureTableDefaultPartitionKeyValue is used as the partition key.</span></span> |<span data-ttu-id="3e9dc-1085">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1085">A column name.</span></span> |<span data-ttu-id="3e9dc-1086">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1086">No</span></span> |
| <span data-ttu-id="3e9dc-1087">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1087">azureTableRowKeyName</span></span> |<span data-ttu-id="3e9dc-1088">Zadejte název sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1088">Specify name of the column whose column values are used as row key.</span></span> <span data-ttu-id="3e9dc-1089">Pokud není zadaný, použijte identifikátor GUID pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1089">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="3e9dc-1090">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1090">A column name.</span></span> |<span data-ttu-id="3e9dc-1091">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1091">No</span></span> |
| <span data-ttu-id="3e9dc-1092">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1092">azureTableInsertType</span></span> |<span data-ttu-id="3e9dc-1093">Režim vložení dat do tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1093">The mode to insert data into Azure table.</span></span><br/><br/><span data-ttu-id="3e9dc-1094">Tato vlastnost určuje, jestli mají existující řádky v tabulce output s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1094">This property controls whether existing rows in the output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="3e9dc-1095">Další informace o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1095">To learn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="3e9dc-1096">Toto nastavení se vztahuje na úrovni řádků, není úrovni tabulky a ani možnost odstraní řádků do výstupní tabulky, které nejsou k dispozici ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1096">This setting applies at the row level, not the table level, and neither option deletes rows in the output table that do not exist in the input.</span></span> |<span data-ttu-id="3e9dc-1097">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1097">merge (default)</span></span><br/><span data-ttu-id="3e9dc-1098">Nahradit</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1098">replace</span></span> |<span data-ttu-id="3e9dc-1099">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1099">No</span></span> |
| <span data-ttu-id="3e9dc-1100">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1100">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-1101">Když je dosaženo writeBatchSize nebo writeBatchTimeout vkládá data do tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1101">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="3e9dc-1102">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1102">Integer (number of rows)</span></span> |<span data-ttu-id="3e9dc-1103">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1103">No (default: 10000)</span></span> |
| <span data-ttu-id="3e9dc-1104">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1104">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-1105">Když je dosaženo writeBatchSize nebo writeBatchTimeout vkládá data do tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1105">Inserts data into the Azure table when the writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="3e9dc-1106">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1106">timespan</span></span><br/><br/><span data-ttu-id="3e9dc-1107">Příklad: "00:20:00" (20 minut)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1107">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="3e9dc-1108">Ne (výchozí nastavení časového limitu výchozí úložiště klienta hodnotu 90 sekundu)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1108">No (Default to storage client default timeout value 90 sec)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1109">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1109">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3e9dc-1110">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1110">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="amazon-redshift"></a><span data-ttu-id="3e9dc-1111">Amazon RedShift</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1111">Amazon RedShift</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1112">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1112">Linked service</span></span>
<span data-ttu-id="3e9dc-1113">K definování Amazon Redshift propojené služby, nastavte **typ** propojené služby pro **AmazonRedshift**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1113">To define an Amazon Redshift linked service, set the **type** of the linked service to **AmazonRedshift**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1114">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1114">Property</span></span> | <span data-ttu-id="3e9dc-1115">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1115">Description</span></span> | <span data-ttu-id="3e9dc-1116">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1116">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1117">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1117">server</span></span> |<span data-ttu-id="3e9dc-1118">IP adresa nebo název hostitele serveru Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1118">IP address or host name of the Amazon Redshift server.</span></span> |<span data-ttu-id="3e9dc-1119">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1119">Yes</span></span> |
| <span data-ttu-id="3e9dc-1120">port</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1120">port</span></span> |<span data-ttu-id="3e9dc-1121">Číslo portu TCP, který používá server Amazon Redshift naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1121">The number of the TCP port that the Amazon Redshift server uses to listen for client connections.</span></span> |<span data-ttu-id="3e9dc-1122">Ne, výchozí hodnota: 5439</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1122">No, default value: 5439</span></span> |
| <span data-ttu-id="3e9dc-1123">Databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1123">database</span></span> |<span data-ttu-id="3e9dc-1124">Název databáze Amazon Redshift.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1124">Name of the Amazon Redshift database.</span></span> |<span data-ttu-id="3e9dc-1125">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1125">Yes</span></span> |
| <span data-ttu-id="3e9dc-1126">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1126">username</span></span> |<span data-ttu-id="3e9dc-1127">Jméno uživatele, který má přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1127">Name of user who has access to the database.</span></span> |<span data-ttu-id="3e9dc-1128">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1128">Yes</span></span> |
| <span data-ttu-id="3e9dc-1129">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1129">password</span></span> |<span data-ttu-id="3e9dc-1130">Heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1130">Password for the user account.</span></span> |<span data-ttu-id="3e9dc-1131">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1131">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1132">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1132">Example</span></span>

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

<span data-ttu-id="3e9dc-1133">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1133">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1134">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1134">Dataset</span></span>
<span data-ttu-id="3e9dc-1135">Chcete-li definovat datové sadě služby Amazon Redshift, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1135">To define an Amazon Redshift dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1136">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1136">Property</span></span> | <span data-ttu-id="3e9dc-1137">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1137">Description</span></span> | <span data-ttu-id="3e9dc-1138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1138">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1139">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1139">tableName</span></span> |<span data-ttu-id="3e9dc-1140">Název tabulky v databázi Amazon Redshift, propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1140">Name of the table in the Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1141">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1141">No (if **query** of **RelationalSource** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-1142">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1142">Example</span></span>

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3e9dc-1143">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1143">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1144">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1144">Relational Source in Copy Activity</span></span> 
<span data-ttu-id="3e9dc-1145">Pokud jsou kopírování dat z Amazon Redshift, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1145">If you are copying data from Amazon Redshift, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1146">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1146">Property</span></span> | <span data-ttu-id="3e9dc-1147">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1147">Description</span></span> | <span data-ttu-id="3e9dc-1148">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1148">Allowed values</span></span> | <span data-ttu-id="3e9dc-1149">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1149">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1150">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1150">query</span></span> |<span data-ttu-id="3e9dc-1151">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1151">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1152">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1152">SQL query string.</span></span> <span data-ttu-id="3e9dc-1153">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1153">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-1154">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1154">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1155">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1155">Example</span></span>

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="3e9dc-1156">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1156">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) article.</span></span>

## <a name="ibm-db2"></a><span data-ttu-id="3e9dc-1157">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1157">IBM DB2</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1158">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1158">Linked service</span></span>
<span data-ttu-id="3e9dc-1159">K definování IBM DB2 propojené služby, nastavte **typ** propojené služby pro **OnPremisesDB2**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1159">To define an IBM DB2 linked service, set the **type** of the linked service to **OnPremisesDB2**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1160">Property</span></span> | <span data-ttu-id="3e9dc-1161">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1161">Description</span></span> | <span data-ttu-id="3e9dc-1162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1163">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1163">server</span></span> |<span data-ttu-id="3e9dc-1164">Název serveru DB2.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1164">Name of the DB2 server.</span></span> |<span data-ttu-id="3e9dc-1165">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1165">Yes</span></span> |
| <span data-ttu-id="3e9dc-1166">Databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1166">database</span></span> |<span data-ttu-id="3e9dc-1167">Název databáze DB2.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1167">Name of the DB2 database.</span></span> |<span data-ttu-id="3e9dc-1168">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1168">Yes</span></span> |
| <span data-ttu-id="3e9dc-1169">Schéma</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1169">schema</span></span> |<span data-ttu-id="3e9dc-1170">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1170">Name of the schema in the database.</span></span> <span data-ttu-id="3e9dc-1171">Název schématu rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1171">The schema name is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-1172">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1172">No</span></span> |
| <span data-ttu-id="3e9dc-1173">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1173">authenticationType</span></span> |<span data-ttu-id="3e9dc-1174">Typ ověřování používaný pro připojení k databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1174">Type of authentication used to connect to the DB2 database.</span></span> <span data-ttu-id="3e9dc-1175">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1175">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3e9dc-1176">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1176">Yes</span></span> |
| <span data-ttu-id="3e9dc-1177">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1177">username</span></span> |<span data-ttu-id="3e9dc-1178">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1178">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-1179">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1179">No</span></span> |
| <span data-ttu-id="3e9dc-1180">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1180">password</span></span> |<span data-ttu-id="3e9dc-1181">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1181">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-1182">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1182">No</span></span> |
| <span data-ttu-id="3e9dc-1183">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1183">gatewayName</span></span> |<span data-ttu-id="3e9dc-1184">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1184">Name of the gateway that the Data Factory service should use to connect to the on-premises DB2 database.</span></span> |<span data-ttu-id="3e9dc-1185">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1185">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1186">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1186">Example</span></span>
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="3e9dc-1187">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1187">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1188">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1188">Dataset</span></span>
<span data-ttu-id="3e9dc-1189">Chcete-li definovat DB2 datovou sadu, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1189">To define a DB2 dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="3e9dc-1190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1190">Property</span></span> | <span data-ttu-id="3e9dc-1191">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1191">Description</span></span> | <span data-ttu-id="3e9dc-1192">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1192">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1193">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1193">tableName</span></span> |<span data-ttu-id="3e9dc-1194">Název tabulky instance databáze DB2 na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1194">Name of the table in the DB2 Database instance that linked service refers to.</span></span> <span data-ttu-id="3e9dc-1195">TableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1195">The tableName is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-1196">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1196">No (if **query** of **RelationalSource** is specified)</span></span> 

#### <a name="example"></a><span data-ttu-id="3e9dc-1197">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1197">Example</span></span>
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1198">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1198">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1199">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1199">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1200">Pokud jsou kopírování dat z IBM DB2, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1200">If you are copying data from IBM DB2, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1201">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1201">Property</span></span> | <span data-ttu-id="3e9dc-1202">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1202">Description</span></span> | <span data-ttu-id="3e9dc-1203">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1203">Allowed values</span></span> | <span data-ttu-id="3e9dc-1204">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1204">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1205">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1205">query</span></span> |<span data-ttu-id="3e9dc-1206">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1206">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1207">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1207">SQL query string.</span></span> <span data-ttu-id="3e9dc-1208">Například: `"query": "select * from "MySchema"."MyTable""`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1208">For example: `"query": "select * from "MySchema"."MyTable""`.</span></span> |<span data-ttu-id="3e9dc-1209">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1209">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1210">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1210">Example</span></span>
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
<span data-ttu-id="3e9dc-1211">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1211">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) article.</span></span>

## <a name="mysql"></a><span data-ttu-id="3e9dc-1212">MySQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1212">MySQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1213">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1213">Linked service</span></span>
<span data-ttu-id="3e9dc-1214">K definování MySQL propojené služby, nastavte **typ** propojené služby pro **OnPremisesMySql**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1214">To define a MySQL linked service, set the **type** of the linked service to **OnPremisesMySql**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1215">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1215">Property</span></span> | <span data-ttu-id="3e9dc-1216">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1216">Description</span></span> | <span data-ttu-id="3e9dc-1217">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1217">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1218">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1218">server</span></span> |<span data-ttu-id="3e9dc-1219">Název serveru databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1219">Name of the MySQL server.</span></span> |<span data-ttu-id="3e9dc-1220">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1220">Yes</span></span> |
| <span data-ttu-id="3e9dc-1221">Databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1221">database</span></span> |<span data-ttu-id="3e9dc-1222">Název databáze MySQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1222">Name of the MySQL database.</span></span> |<span data-ttu-id="3e9dc-1223">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1223">Yes</span></span> |
| <span data-ttu-id="3e9dc-1224">Schéma</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1224">schema</span></span> |<span data-ttu-id="3e9dc-1225">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1225">Name of the schema in the database.</span></span> |<span data-ttu-id="3e9dc-1226">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1226">No</span></span> |
| <span data-ttu-id="3e9dc-1227">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1227">authenticationType</span></span> |<span data-ttu-id="3e9dc-1228">Typ ověřování používaný pro připojení k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1228">Type of authentication used to connect to the MySQL database.</span></span> <span data-ttu-id="3e9dc-1229">Možné hodnoty jsou: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1229">Possible values are: `Basic`.</span></span> |<span data-ttu-id="3e9dc-1230">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1230">Yes</span></span> |
| <span data-ttu-id="3e9dc-1231">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1231">username</span></span> |<span data-ttu-id="3e9dc-1232">Zadejte uživatelské jméno pro připojení k databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1232">Specify user name to connect to the MySQL database.</span></span> |<span data-ttu-id="3e9dc-1233">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1233">Yes</span></span> |
| <span data-ttu-id="3e9dc-1234">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1234">password</span></span> |<span data-ttu-id="3e9dc-1235">Zadejte heslo pro uživatelský účet, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1235">Specify password for the user account you specified.</span></span> |<span data-ttu-id="3e9dc-1236">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1236">Yes</span></span> |
| <span data-ttu-id="3e9dc-1237">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1237">gatewayName</span></span> |<span data-ttu-id="3e9dc-1238">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1238">Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database.</span></span> |<span data-ttu-id="3e9dc-1239">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1239">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1240">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1240">Example</span></span>

```json
{
    "name": "OnPremMySqlLinkedService",
    "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
            "server": "<server name>",
            "database": "<database name>",
            "schema": "<schema name>",
            "authenticationType": "<authentication type>",
            "userName": "<user name>",
            "password": "<password>",
            "gatewayName": "<gateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1241">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1241">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1242">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1242">Dataset</span></span>
<span data-ttu-id="3e9dc-1243">Chcete-li definovat datovou sadu MySQL, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1243">To define a MySQL dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1244">Property</span></span> | <span data-ttu-id="3e9dc-1245">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1245">Description</span></span> | <span data-ttu-id="3e9dc-1246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1247">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1247">tableName</span></span> |<span data-ttu-id="3e9dc-1248">Název tabulky instance databáze MySQL na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1248">Name of the table in the MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1249">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1249">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1250">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1250">Example</span></span>

```json
{
    "name": "MySqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremMySqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3e9dc-1251">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1251">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1252">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1252">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1253">Pokud kopírujete data z databáze MySQL, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1253">If you are copying data from a MySQL database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1254">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1254">Property</span></span> | <span data-ttu-id="3e9dc-1255">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1255">Description</span></span> | <span data-ttu-id="3e9dc-1256">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1256">Allowed values</span></span> | <span data-ttu-id="3e9dc-1257">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1257">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1258">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1258">query</span></span> |<span data-ttu-id="3e9dc-1259">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1259">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1260">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1260">SQL query string.</span></span> <span data-ttu-id="3e9dc-1261">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1261">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-1262">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1262">No (if **tableName** of **dataset** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-1263">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1263">Example</span></span>
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1264">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1264">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="oracle"></a><span data-ttu-id="3e9dc-1265">Oracle</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1265">Oracle</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1266">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1266">Linked service</span></span>
<span data-ttu-id="3e9dc-1267">K definování Oracle propojené služby, nastavte **typ** propojené služby pro **OnPremisesOracle**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1267">To define an Oracle linked service, set the **type** of the linked service to **OnPremisesOracle**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1268">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1268">Property</span></span> | <span data-ttu-id="3e9dc-1269">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1269">Description</span></span> | <span data-ttu-id="3e9dc-1270">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1270">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1271">driverType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1271">driverType</span></span> | <span data-ttu-id="3e9dc-1272">Určete, který ovladač použít ke zkopírování dat z/do databáze Oracle.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1272">Specify which driver to use to copy data from/to Oracle Database.</span></span> <span data-ttu-id="3e9dc-1273">Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1273">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="3e9dc-1274">V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1274">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="3e9dc-1275">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1275">No</span></span> |
| <span data-ttu-id="3e9dc-1276">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1276">connectionString</span></span> | <span data-ttu-id="3e9dc-1277">Zadejte informace potřebné pro připojení k instanci databáze Oracle pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1277">Specify information needed to connect to the Oracle Database instance for the connectionString property.</span></span> | <span data-ttu-id="3e9dc-1278">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1278">Yes</span></span> |
| <span data-ttu-id="3e9dc-1279">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1279">gatewayName</span></span> | <span data-ttu-id="3e9dc-1280">Název brány, aby se používá pro připojení k místnímu serveru Oracle</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1280">Name of the gateway that that is used to connect to the on-premises Oracle server</span></span> |<span data-ttu-id="3e9dc-1281">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1281">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1282">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1282">Example</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1283">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1283">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1284">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1284">Dataset</span></span>
<span data-ttu-id="3e9dc-1285">Chcete-li definovat datové sadě služby Oracle, nastavte **typ** datové sady, která **OracleTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1285">To define an Oracle dataset, set the **type** of the dataset to **OracleTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1286">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1286">Property</span></span> | <span data-ttu-id="3e9dc-1287">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1287">Description</span></span> | <span data-ttu-id="3e9dc-1288">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1288">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1289">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1289">tableName</span></span> |<span data-ttu-id="3e9dc-1290">Název tabulky v databázi Oracle, který propojená služba odkazuje na.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1290">Name of the table in the Oracle Database that the linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1291">Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1291">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1292">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1292">Example</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3e9dc-1293">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1293">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) article.</span></span>

### <a name="oracle-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1294">Zdroj Oracle v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1294">Oracle Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1295">Pokud kopírujete data z databáze Oracle, nastavte **typ zdroje** kopie aktivity na **OracleSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1295">If you are copying data from an Oracle database, set the **source type** of the copy activity to **OracleSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1296">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1296">Property</span></span> | <span data-ttu-id="3e9dc-1297">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1297">Description</span></span> | <span data-ttu-id="3e9dc-1298">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1298">Allowed values</span></span> | <span data-ttu-id="3e9dc-1299">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1299">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1300">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1300">oracleReaderQuery</span></span> |<span data-ttu-id="3e9dc-1301">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1301">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1302">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1302">SQL query string.</span></span> <span data-ttu-id="3e9dc-1303">Příklad: `select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1303">For example: `select * from MyTable`</span></span> <br/><br/><span data-ttu-id="3e9dc-1304">Pokud není zadaný příkaz jazyka SQL, který se spustí:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1304">If not specified, the SQL statement that is executed: `select * from MyTable`</span></span> |<span data-ttu-id="3e9dc-1305">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1305">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1306">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1306">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1307">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1307">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

### <a name="oracle-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-1308">Podřízený Oracle v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1308">Oracle Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1309">Pokud jsou kopírování dat do databáze Oracle am, nastavte **typ jímky** kopie aktivity na **OracleSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1309">If you are copying data to am Oracle database, set the **sink type** of the copy activity to **OracleSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-1310">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1310">Property</span></span> | <span data-ttu-id="3e9dc-1311">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1311">Description</span></span> | <span data-ttu-id="3e9dc-1312">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1312">Allowed values</span></span> | <span data-ttu-id="3e9dc-1313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1313">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1314">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1314">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-1315">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1315">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="3e9dc-1316">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1316">timespan</span></span><br/><br/> <span data-ttu-id="3e9dc-1317">Příklad: 00:30:00 (30 minut).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1317">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="3e9dc-1318">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1318">No</span></span> |
| <span data-ttu-id="3e9dc-1319">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1319">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-1320">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1320">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3e9dc-1321">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1321">Integer (number of rows)</span></span> |<span data-ttu-id="3e9dc-1322">Ne (výchozí: 100)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1322">No (default: 100)</span></span> |
| <span data-ttu-id="3e9dc-1323">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1323">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3e9dc-1324">Zadejte dotaz pro aktivitu kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1324">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="3e9dc-1325">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1325">A query statement.</span></span> |<span data-ttu-id="3e9dc-1326">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1326">No</span></span> |
| <span data-ttu-id="3e9dc-1327">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1327">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3e9dc-1328">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1328">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="3e9dc-1329">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1329">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3e9dc-1330">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1330">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1331">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1331">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3e9dc-1332">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1332">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

## <a name="postgresql"></a><span data-ttu-id="3e9dc-1333">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1333">PostgreSQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1334">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1334">Linked service</span></span>
<span data-ttu-id="3e9dc-1335">K definování PostgreSQL propojené služby, nastavte **typ** propojené služby pro **OnPremisesPostgreSql**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1335">To define a PostgreSQL linked service, set the **type** of the linked service to **OnPremisesPostgreSql**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1336">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1336">Property</span></span> | <span data-ttu-id="3e9dc-1337">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1337">Description</span></span> | <span data-ttu-id="3e9dc-1338">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1338">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1339">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1339">server</span></span> |<span data-ttu-id="3e9dc-1340">Název serveru PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1340">Name of the PostgreSQL server.</span></span> |<span data-ttu-id="3e9dc-1341">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1341">Yes</span></span> |
| <span data-ttu-id="3e9dc-1342">Databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1342">database</span></span> |<span data-ttu-id="3e9dc-1343">Název databáze PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1343">Name of the PostgreSQL database.</span></span> |<span data-ttu-id="3e9dc-1344">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1344">Yes</span></span> |
| <span data-ttu-id="3e9dc-1345">Schéma</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1345">schema</span></span> |<span data-ttu-id="3e9dc-1346">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1346">Name of the schema in the database.</span></span> <span data-ttu-id="3e9dc-1347">Název schématu rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1347">The schema name is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-1348">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1348">No</span></span> |
| <span data-ttu-id="3e9dc-1349">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1349">authenticationType</span></span> |<span data-ttu-id="3e9dc-1350">Typ ověřování používaný pro připojení k databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1350">Type of authentication used to connect to the PostgreSQL database.</span></span> <span data-ttu-id="3e9dc-1351">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1351">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3e9dc-1352">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1352">Yes</span></span> |
| <span data-ttu-id="3e9dc-1353">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1353">username</span></span> |<span data-ttu-id="3e9dc-1354">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1354">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-1355">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1355">No</span></span> |
| <span data-ttu-id="3e9dc-1356">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1356">password</span></span> |<span data-ttu-id="3e9dc-1357">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1357">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-1358">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1358">No</span></span> |
| <span data-ttu-id="3e9dc-1359">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1359">gatewayName</span></span> |<span data-ttu-id="3e9dc-1360">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1360">Name of the gateway that the Data Factory service should use to connect to the on-premises PostgreSQL database.</span></span> |<span data-ttu-id="3e9dc-1361">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1361">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1362">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1362">Example</span></span>

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
<span data-ttu-id="3e9dc-1363">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1363">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1364">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1364">Dataset</span></span>
<span data-ttu-id="3e9dc-1365">Chcete-li definovat PostgreSQL datovou sadu, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1365">To define a PostgreSQL dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1366">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1366">Property</span></span> | <span data-ttu-id="3e9dc-1367">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1367">Description</span></span> | <span data-ttu-id="3e9dc-1368">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1368">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1369">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1369">tableName</span></span> |<span data-ttu-id="3e9dc-1370">Název tabulky instance databáze PostgreSQL, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1370">Name of the table in the PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="3e9dc-1371">TableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1371">The tableName is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-1372">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1372">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1373">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1373">Example</span></span>
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```
<span data-ttu-id="3e9dc-1374">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1374">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1375">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1375">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1376">Pokud kopírujete data z databáze PostgreSQL, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1376">If you are copying data from a PostgreSQL database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1377">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1377">Property</span></span> | <span data-ttu-id="3e9dc-1378">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1378">Description</span></span> | <span data-ttu-id="3e9dc-1379">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1379">Allowed values</span></span> | <span data-ttu-id="3e9dc-1380">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1380">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1381">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1381">query</span></span> |<span data-ttu-id="3e9dc-1382">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1382">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1383">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1383">SQL query string.</span></span> <span data-ttu-id="3e9dc-1384">Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1384">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="3e9dc-1385">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1385">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1386">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1386">Example</span></span>

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1387">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1387">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) article.</span></span>

## <a name="sap-business-warehouse"></a><span data-ttu-id="3e9dc-1388">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1388">SAP Business Warehouse</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-1389">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1389">Linked service</span></span>
<span data-ttu-id="3e9dc-1390">K definování SAP Business Warehouse (BW) propojené služby, nastavte **typ** propojené služby pro **SapBw**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1390">To define a SAP Business Warehouse (BW) linked service, set the **type** of the linked service to **SapBw**, and specify following properties in the **typeProperties** section:</span></span>  

<span data-ttu-id="3e9dc-1391">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1391">Property</span></span> | <span data-ttu-id="3e9dc-1392">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1392">Description</span></span> | <span data-ttu-id="3e9dc-1393">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1393">Allowed values</span></span> | <span data-ttu-id="3e9dc-1394">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1394">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="3e9dc-1395">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1395">server</span></span> | <span data-ttu-id="3e9dc-1396">Název serveru, na kterém se nachází instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1396">Name of the server on which the SAP BW instance resides.</span></span> | <span data-ttu-id="3e9dc-1397">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1397">string</span></span> | <span data-ttu-id="3e9dc-1398">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1398">Yes</span></span>
<span data-ttu-id="3e9dc-1399">systemNumber</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1399">systemNumber</span></span> | <span data-ttu-id="3e9dc-1400">Číslo systému SAP BW systému.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1400">System number of the SAP BW system.</span></span> | <span data-ttu-id="3e9dc-1401">Desetinné číslo letopočty řetězec.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1401">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="3e9dc-1402">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1402">Yes</span></span>
<span data-ttu-id="3e9dc-1403">clientId</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1403">clientId</span></span> | <span data-ttu-id="3e9dc-1404">ID klienta v systému SAP W klienta.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1404">Client ID of the client in the SAP W system.</span></span> | <span data-ttu-id="3e9dc-1405">Tři číslice desítkové číslo řetězec.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1405">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="3e9dc-1406">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1406">Yes</span></span>
<span data-ttu-id="3e9dc-1407">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1407">username</span></span> | <span data-ttu-id="3e9dc-1408">Jméno uživatele, který má přístup k serveru SAP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1408">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="3e9dc-1409">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1409">string</span></span> | <span data-ttu-id="3e9dc-1410">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1410">Yes</span></span>
<span data-ttu-id="3e9dc-1411">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1411">password</span></span> | <span data-ttu-id="3e9dc-1412">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1412">Password for the user.</span></span> | <span data-ttu-id="3e9dc-1413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1413">string</span></span> | <span data-ttu-id="3e9dc-1414">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1414">Yes</span></span>
<span data-ttu-id="3e9dc-1415">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1415">gatewayName</span></span> | <span data-ttu-id="3e9dc-1416">Název brány, kterou služba Data Factory měla použít pro připojení k místní instanci SAP BW.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1416">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP BW instance.</span></span> | <span data-ttu-id="3e9dc-1417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1417">string</span></span> | <span data-ttu-id="3e9dc-1418">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1418">Yes</span></span>
<span data-ttu-id="3e9dc-1419">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1419">encryptedCredential</span></span> | <span data-ttu-id="3e9dc-1420">Řetězec šifrovaný přihlašovací údaj.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1420">The encrypted credential string.</span></span> | <span data-ttu-id="3e9dc-1421">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1421">string</span></span> | <span data-ttu-id="3e9dc-1422">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1422">No</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-1423">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1423">Example</span></span>

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1424">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1424">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1425">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1425">Dataset</span></span>
<span data-ttu-id="3e9dc-1426">Chcete-li definovat SAP BW datovou sadu, nastavte **typ** datové sady, která **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1426">To define a SAP BW dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="3e9dc-1427">Nejsou žádné vlastnosti specifické pro typ podporované pro SAP BW datovou sadu typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1427">There are no type-specific properties supported for the SAP BW dataset of type **RelationalTable**.</span></span>  

#### <a name="example"></a><span data-ttu-id="3e9dc-1428">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1428">Example</span></span>

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3e9dc-1429">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1429">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1430">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1430">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1431">Pokud jsou kopírování dat z SAP Business Warehouse, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1431">If you are copying data from SAP Business Warehouse, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1432">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1432">Property</span></span> | <span data-ttu-id="3e9dc-1433">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1433">Description</span></span> | <span data-ttu-id="3e9dc-1434">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1434">Allowed values</span></span> | <span data-ttu-id="3e9dc-1435">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1435">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1436">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1436">query</span></span> | <span data-ttu-id="3e9dc-1437">Určuje dotaz MDX číst data z instance SAP BW.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1437">Specifies the MDX query to read data from the SAP BW instance.</span></span> | <span data-ttu-id="3e9dc-1438">Dotaz MDX.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1438">MDX query.</span></span> | <span data-ttu-id="3e9dc-1439">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1439">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1440">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1440">Example</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<MDX query for SAP BW>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1441">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1441">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sap-hana"></a><span data-ttu-id="3e9dc-1442">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1442">SAP HANA</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1443">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1443">Linked service</span></span>
<span data-ttu-id="3e9dc-1444">K definování SAP HANA propojené služby, nastavte **typ** propojené služby pro **SapHana**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1444">To define a SAP HANA linked service, set the **type** of the linked service to **SapHana**, and specify following properties in the **typeProperties** section:</span></span>  

<span data-ttu-id="3e9dc-1445">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1445">Property</span></span> | <span data-ttu-id="3e9dc-1446">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1446">Description</span></span> | <span data-ttu-id="3e9dc-1447">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1447">Allowed values</span></span> | <span data-ttu-id="3e9dc-1448">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1448">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="3e9dc-1449">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1449">server</span></span> | <span data-ttu-id="3e9dc-1450">Název serveru, na kterém se nachází instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1450">Name of the server on which the SAP HANA instance resides.</span></span> <span data-ttu-id="3e9dc-1451">Pokud váš server používá vlastní port, zadejte `server:port`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1451">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="3e9dc-1452">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1452">string</span></span> | <span data-ttu-id="3e9dc-1453">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1453">Yes</span></span>
<span data-ttu-id="3e9dc-1454">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1454">authenticationType</span></span> | <span data-ttu-id="3e9dc-1455">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1455">Type of authentication.</span></span> | <span data-ttu-id="3e9dc-1456">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1456">string.</span></span> <span data-ttu-id="3e9dc-1457">"Základní" nebo "Systém Windows"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1457">"Basic" or "Windows"</span></span> | <span data-ttu-id="3e9dc-1458">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1458">Yes</span></span> 
<span data-ttu-id="3e9dc-1459">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1459">username</span></span> | <span data-ttu-id="3e9dc-1460">Jméno uživatele, který má přístup k serveru SAP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1460">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="3e9dc-1461">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1461">string</span></span> | <span data-ttu-id="3e9dc-1462">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1462">Yes</span></span>
<span data-ttu-id="3e9dc-1463">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1463">password</span></span> | <span data-ttu-id="3e9dc-1464">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1464">Password for the user.</span></span> | <span data-ttu-id="3e9dc-1465">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1465">string</span></span> | <span data-ttu-id="3e9dc-1466">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1466">Yes</span></span>
<span data-ttu-id="3e9dc-1467">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1467">gatewayName</span></span> | <span data-ttu-id="3e9dc-1468">Název brány, kterou služba Data Factory měla použít pro připojení k místní instanci SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1468">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP HANA instance.</span></span> | <span data-ttu-id="3e9dc-1469">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1469">string</span></span> | <span data-ttu-id="3e9dc-1470">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1470">Yes</span></span>
<span data-ttu-id="3e9dc-1471">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1471">encryptedCredential</span></span> | <span data-ttu-id="3e9dc-1472">Řetězec šifrovaný přihlašovací údaj.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1472">The encrypted credential string.</span></span> | <span data-ttu-id="3e9dc-1473">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1473">string</span></span> | <span data-ttu-id="3e9dc-1474">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1474">No</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-1475">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1475">Example</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
<span data-ttu-id="3e9dc-1476">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1476">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) article.</span></span>
 
### <a name="dataset"></a><span data-ttu-id="3e9dc-1477">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1477">Dataset</span></span>
<span data-ttu-id="3e9dc-1478">Chcete-li definovat SAP HANA datovou sadu, nastavte **typ** datové sady, která **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1478">To define a SAP HANA dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="3e9dc-1479">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP HANA typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1479">There are no type-specific properties supported for the SAP HANA dataset of type **RelationalTable**.</span></span> 

#### <a name="example"></a><span data-ttu-id="3e9dc-1480">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1480">Example</span></span>

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="3e9dc-1481">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1481">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1482">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1482">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1483">Pokud jsou kopírování dat z jiného úložiště dat SAP HANA, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1483">If you are copying data from a SAP HANA data store, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1484">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1484">Property</span></span> | <span data-ttu-id="3e9dc-1485">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1485">Description</span></span> | <span data-ttu-id="3e9dc-1486">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1486">Allowed values</span></span> | <span data-ttu-id="3e9dc-1487">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1487">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1488">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1488">query</span></span> | <span data-ttu-id="3e9dc-1489">Určuje příkaz jazyka SQL pro čtení dat z instance SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1489">Specifies the SQL query to read data from the SAP HANA instance.</span></span> | <span data-ttu-id="3e9dc-1490">Dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1490">SQL query.</span></span> | <span data-ttu-id="3e9dc-1491">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1491">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-1492">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1492">Example</span></span>


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1493">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1493">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) article.</span></span>


## <a name="sql-server"></a><span data-ttu-id="3e9dc-1494">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1494">SQL Server</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1495">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1495">Linked service</span></span>
<span data-ttu-id="3e9dc-1496">Vytvoření propojené služby typu **onpremisessqlserver** propojit místní databázi systému SQL Server do služby data factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1496">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="3e9dc-1497">Následující tabulka obsahuje popis specifické pro službu SQL serveru propojená místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1497">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="3e9dc-1498">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro SQL Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1498">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="3e9dc-1499">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1499">Property</span></span> | <span data-ttu-id="3e9dc-1500">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1500">Description</span></span> | <span data-ttu-id="3e9dc-1501">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1501">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1502">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1502">type</span></span> |<span data-ttu-id="3e9dc-1503">Vlastnost typu musí být nastavená na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1503">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="3e9dc-1504">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1504">Yes</span></span> |
| <span data-ttu-id="3e9dc-1505">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1505">connectionString</span></span> |<span data-ttu-id="3e9dc-1506">Zadejte připojovací řetězec informace potřebné pro připojení k místní databázi systému SQL Server pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1506">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-1507">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1507">Yes</span></span> |
| <span data-ttu-id="3e9dc-1508">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1508">gatewayName</span></span> |<span data-ttu-id="3e9dc-1509">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1509">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="3e9dc-1510">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1510">Yes</span></span> |
| <span data-ttu-id="3e9dc-1511">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1511">username</span></span> |<span data-ttu-id="3e9dc-1512">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1512">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="3e9dc-1513">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1513">Example: **domainname\\username**.</span></span> |<span data-ttu-id="3e9dc-1514">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1514">No</span></span> |
| <span data-ttu-id="3e9dc-1515">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1515">password</span></span> |<span data-ttu-id="3e9dc-1516">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1516">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-1517">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1517">No</span></span> |

<span data-ttu-id="3e9dc-1518">Můžete šifrovat přihlašovací údaje pomocí **New-AzureRmDataFactoryEncryptValue** rutiny a jak je znázorněno v následujícím příkladu je využít v připojovacím řetězci (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1518">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="3e9dc-1519">Příklad: JSON pro pomocí ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1519">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="3e9dc-1520">Příklad: JSON pro použití ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1520">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="3e9dc-1521">Pokud jsou zadané uživatelské jméno a heslo, brána je používá k zosobnění zadaný uživatelský účet pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1521">If username and password are specified, gateway uses them to impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> <span data-ttu-id="3e9dc-1522">Jinak brána připojí k systému SQL Server přímo s kontextem zabezpečení brány (jeho účet spuštění).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1522">Otherwise, gateway connects to the SQL Server directly with the security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1523">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1523">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1524">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1524">Dataset</span></span>
<span data-ttu-id="3e9dc-1525">Chcete-li definovat datové sady SQL Server, nastavte **typ** datové sady, která **SqlServerTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1525">To define a SQL Server dataset, set the **type** of the dataset to **SqlServerTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1526">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1526">Property</span></span> | <span data-ttu-id="3e9dc-1527">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1527">Description</span></span> | <span data-ttu-id="3e9dc-1528">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1528">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1529">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1529">tableName</span></span> |<span data-ttu-id="3e9dc-1530">Název tabulky nebo zobrazení instance databáze serveru SQL, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1530">Name of the table or view in the SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1531">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1531">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1532">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1532">Example</span></span>
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1533">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1533">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1534">Zdroje SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1534">Sql Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1535">Pokud kopírujete data z databáze systému SQL Server, nastavte **typ zdroje** kopie aktivity na **SqlSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1535">If you are copying data from a SQL Server database, set the **source type** of the copy activity to **SqlSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1536">Property</span></span> | <span data-ttu-id="3e9dc-1537">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1537">Description</span></span> | <span data-ttu-id="3e9dc-1538">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1538">Allowed values</span></span> | <span data-ttu-id="3e9dc-1539">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1539">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1540">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1540">sqlReaderQuery</span></span> |<span data-ttu-id="3e9dc-1541">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1541">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1542">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1542">SQL query string.</span></span> <span data-ttu-id="3e9dc-1543">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1543">For example: `select * from MyTable`.</span></span> <span data-ttu-id="3e9dc-1544">Může odkazovat více tabulek z databáze odkazuje vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1544">May reference multiple tables from the database referenced by the input dataset.</span></span> <span data-ttu-id="3e9dc-1545">Pokud není zadaný příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1545">If not specified, the SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="3e9dc-1546">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1546">No</span></span> |
| <span data-ttu-id="3e9dc-1547">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1547">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="3e9dc-1548">Název uložené procedury, který čte data ze zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1548">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="3e9dc-1549">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1549">Name of the stored procedure.</span></span> |<span data-ttu-id="3e9dc-1550">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1550">No</span></span> |
| <span data-ttu-id="3e9dc-1551">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1551">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-1552">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1552">Parameters for the stored procedure.</span></span> |<span data-ttu-id="3e9dc-1553">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1553">Name/value pairs.</span></span> <span data-ttu-id="3e9dc-1554">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1554">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="3e9dc-1555">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1555">No</span></span> |

<span data-ttu-id="3e9dc-1556">Pokud **sqlReaderQuery** je zadán pro SqlSource, aktivitě kopírování spustí tento dotaz na zdroji databáze systému SQL Server získat data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1556">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the SQL Server Database source to get the data.</span></span>

<span data-ttu-id="3e9dc-1557">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1557">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="3e9dc-1558">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části struktura slouží k vytvoření dotazu vyberte možnost spustit v databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1558">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="3e9dc-1559">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1559">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="3e9dc-1560">Při použití **sqlReaderStoredProcedureName**, stále je třeba zadat hodnotu pro **tableName** vlastnost v datové sadě JSON.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1560">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="3e9dc-1561">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1561">There are no validations performed against this table though.</span></span>


#### <a name="example"></a><span data-ttu-id="3e9dc-1562">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1562">Example</span></span>
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1563">V tomto příkladu **sqlReaderQuery** je zadán pro SqlSource.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1563">In this example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="3e9dc-1564">Aktivita kopírování spustí tento dotaz na zdroji databáze systému SQL Server získat data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1564">The Copy Activity runs this query against the SQL Server Database source to get the data.</span></span> <span data-ttu-id="3e9dc-1565">Alternativně můžete zadat uložené procedury zadáním **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud uložená procedura přebírá parametry).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1565">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span> <span data-ttu-id="3e9dc-1566">SqlReaderQuery může odkazovat více tabulek v databázi odkazuje vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1566">The sqlReaderQuery can reference multiple tables within the database referenced by the input dataset.</span></span> <span data-ttu-id="3e9dc-1567">Se neomezuje jenom do tabulky, nastavte jako typeProperty tableName datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1567">It is not limited to only the table set as the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="3e9dc-1568">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, sloupce definované v části struktura slouží k vytvoření dotazu vyberte možnost spustit v databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1568">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="3e9dc-1569">Pokud definice datové sady nemá strukturu, jsou vybrány všechny sloupce z tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1569">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="3e9dc-1570">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1570">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-1571">Jímku SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1571">Sql Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1572">Pokud data kopírujete do databáze systému SQL Server, nastavte **typ jímky** kopie aktivity na **SqlSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1572">If you are copying data to a SQL Server database, set the **sink type** of the copy activity to **SqlSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-1573">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1573">Property</span></span> | <span data-ttu-id="3e9dc-1574">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1574">Description</span></span> | <span data-ttu-id="3e9dc-1575">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1575">Allowed values</span></span> | <span data-ttu-id="3e9dc-1576">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1576">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1577">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1577">writeBatchTimeout</span></span> |<span data-ttu-id="3e9dc-1578">Počkejte, než čas na dokončení předtím, než vyprší časový limit operace dávkové vložení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1578">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="3e9dc-1579">Časový interval</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1579">timespan</span></span><br/><br/> <span data-ttu-id="3e9dc-1580">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1580">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="3e9dc-1581">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1581">No</span></span> |
| <span data-ttu-id="3e9dc-1582">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1582">writeBatchSize</span></span> |<span data-ttu-id="3e9dc-1583">Vloží data do tabulky SQL, když velikost vyrovnávací paměti dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1583">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="3e9dc-1584">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1584">Integer (number of rows)</span></span> |<span data-ttu-id="3e9dc-1585">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1585">No (default: 10000)</span></span> |
| <span data-ttu-id="3e9dc-1586">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1586">sqlWriterCleanupScript</span></span> |<span data-ttu-id="3e9dc-1587">Zadejte dotaz aktivity kopírování provést tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1587">Specify query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="3e9dc-1588">Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1588">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="3e9dc-1589">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1589">A query statement.</span></span> |<span data-ttu-id="3e9dc-1590">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1590">No</span></span> |
| <span data-ttu-id="3e9dc-1591">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1591">sliceIdentifierColumnName</span></span> |<span data-ttu-id="3e9dc-1592">Zadejte název sloupce pro aktivitu kopírování vyplníte identifikátor automaticky generovány řez, který se používá k vyčištění dat určitý řez při spusťte znovu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1592">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="3e9dc-1593">Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1593">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="3e9dc-1594">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1594">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="3e9dc-1595">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1595">No</span></span> |
| <span data-ttu-id="3e9dc-1596">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1596">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="3e9dc-1597">Název uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1597">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="3e9dc-1598">Název uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1598">Name of the stored procedure.</span></span> |<span data-ttu-id="3e9dc-1599">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1599">No</span></span> |
| <span data-ttu-id="3e9dc-1600">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1600">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-1601">Parametry pro uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1601">Parameters for the stored procedure.</span></span> |<span data-ttu-id="3e9dc-1602">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1602">Name/value pairs.</span></span> <span data-ttu-id="3e9dc-1603">Názvy a malá a velká písmena parametry musí odpovídat názvům a malá a velká písmena parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1603">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="3e9dc-1604">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1604">No</span></span> |
| <span data-ttu-id="3e9dc-1605">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1605">sqlWriterTableType</span></span> |<span data-ttu-id="3e9dc-1606">Zadejte název typu tabulky má být použit v uložené proceduře.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1606">Specify table type name to be used in the stored procedure.</span></span> <span data-ttu-id="3e9dc-1607">Aktivita kopírování zpřístupní přesouvání dat v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1607">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="3e9dc-1608">Uložená procedura kód pak sloučit data kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1608">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="3e9dc-1609">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1609">A table type name.</span></span> |<span data-ttu-id="3e9dc-1610">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1610">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1611">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1611">Example</span></span>
<span data-ttu-id="3e9dc-1612">Kanál obsahuje aktivitu kopírování, která je konfigurovaná pro používání těchto vstupní a výstupní datové sady a je naplánováno spuštění každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1612">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="3e9dc-1613">V definici JSON kanálu **zdroj** je typ nastaven na **BlobSource** a **podřízený** je typ nastaven na **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1613">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1614">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1614">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sybase"></a><span data-ttu-id="3e9dc-1615">Sybase</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1615">Sybase</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1616">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1616">Linked service</span></span>
<span data-ttu-id="3e9dc-1617">K definování Sybase propojené služby, nastavte **typ** propojené služby pro **OnPremisesSybase**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1617">To define a Sybase linked service, set the **type** of the linked service to **OnPremisesSybase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1618">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1618">Property</span></span> | <span data-ttu-id="3e9dc-1619">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1619">Description</span></span> | <span data-ttu-id="3e9dc-1620">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1620">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1621">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1621">server</span></span> |<span data-ttu-id="3e9dc-1622">Název serveru databáze Sybase.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1622">Name of the Sybase server.</span></span> |<span data-ttu-id="3e9dc-1623">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1623">Yes</span></span> |
| <span data-ttu-id="3e9dc-1624">Databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1624">database</span></span> |<span data-ttu-id="3e9dc-1625">Název databáze Sybase.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1625">Name of the Sybase database.</span></span> |<span data-ttu-id="3e9dc-1626">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1626">Yes</span></span> |
| <span data-ttu-id="3e9dc-1627">Schéma</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1627">schema</span></span> |<span data-ttu-id="3e9dc-1628">Název schématu v databázi.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1628">Name of the schema in the database.</span></span> |<span data-ttu-id="3e9dc-1629">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1629">No</span></span> |
| <span data-ttu-id="3e9dc-1630">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1630">authenticationType</span></span> |<span data-ttu-id="3e9dc-1631">Typ ověřování používaný pro připojení k databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1631">Type of authentication used to connect to the Sybase database.</span></span> <span data-ttu-id="3e9dc-1632">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1632">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3e9dc-1633">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1633">Yes</span></span> |
| <span data-ttu-id="3e9dc-1634">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1634">username</span></span> |<span data-ttu-id="3e9dc-1635">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1635">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-1636">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1636">No</span></span> |
| <span data-ttu-id="3e9dc-1637">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1637">password</span></span> |<span data-ttu-id="3e9dc-1638">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1638">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-1639">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1639">No</span></span> |
| <span data-ttu-id="3e9dc-1640">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1640">gatewayName</span></span> |<span data-ttu-id="3e9dc-1641">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1641">Name of the gateway that the Data Factory service should use to connect to the on-premises Sybase database.</span></span> |<span data-ttu-id="3e9dc-1642">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1642">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1643">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1643">Example</span></span>
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1644">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1644">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1645">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1645">Dataset</span></span>
<span data-ttu-id="3e9dc-1646">Chcete-li definovat Sybase datovou sadu, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1646">To define a Sybase dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1647">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1647">Property</span></span> | <span data-ttu-id="3e9dc-1648">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1648">Description</span></span> | <span data-ttu-id="3e9dc-1649">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1649">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1650">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1650">tableName</span></span> |<span data-ttu-id="3e9dc-1651">Název tabulky instance databáze Sybase na kterou odkazuje propojená služba.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1651">Name of the table in the Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="3e9dc-1652">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1652">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1653">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1653">Example</span></span>

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1654">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1654">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1655">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1655">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1656">Pokud kopírujete data z databáze Sybase, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1656">If you are copying data from a Sybase database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1657">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1657">Property</span></span> | <span data-ttu-id="3e9dc-1658">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1658">Description</span></span> | <span data-ttu-id="3e9dc-1659">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1659">Allowed values</span></span> | <span data-ttu-id="3e9dc-1660">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1660">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1661">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1661">query</span></span> |<span data-ttu-id="3e9dc-1662">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1662">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1663">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1663">SQL query string.</span></span> <span data-ttu-id="3e9dc-1664">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1664">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-1665">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1665">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1666">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1666">Example</span></span>

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1667">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1667">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) article.</span></span>

## <a name="teradata"></a><span data-ttu-id="3e9dc-1668">Teradata</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1668">Teradata</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1669">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1669">Linked service</span></span>
<span data-ttu-id="3e9dc-1670">K definování Teradata propojené služby, nastavte **typ** propojené služby pro **OnPremisesTeradata**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1670">To define a Teradata linked service, set the **type** of the linked service to **OnPremisesTeradata**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1671">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1671">Property</span></span> | <span data-ttu-id="3e9dc-1672">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1672">Description</span></span> | <span data-ttu-id="3e9dc-1673">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1673">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1674">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1674">server</span></span> |<span data-ttu-id="3e9dc-1675">Název serveru Teradata.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1675">Name of the Teradata server.</span></span> |<span data-ttu-id="3e9dc-1676">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1676">Yes</span></span> |
| <span data-ttu-id="3e9dc-1677">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1677">authenticationType</span></span> |<span data-ttu-id="3e9dc-1678">Typ ověřování používaný pro připojení k databázi Teradata.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1678">Type of authentication used to connect to the Teradata database.</span></span> <span data-ttu-id="3e9dc-1679">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1679">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3e9dc-1680">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1680">Yes</span></span> |
| <span data-ttu-id="3e9dc-1681">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1681">username</span></span> |<span data-ttu-id="3e9dc-1682">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1682">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-1683">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1683">No</span></span> |
| <span data-ttu-id="3e9dc-1684">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1684">password</span></span> |<span data-ttu-id="3e9dc-1685">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1685">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-1686">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1686">No</span></span> |
| <span data-ttu-id="3e9dc-1687">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1687">gatewayName</span></span> |<span data-ttu-id="3e9dc-1688">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi Teradata.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1688">Name of the gateway that the Data Factory service should use to connect to the on-premises Teradata database.</span></span> |<span data-ttu-id="3e9dc-1689">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1689">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1690">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1690">Example</span></span>
```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1691">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1691">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1692">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1692">Dataset</span></span>
<span data-ttu-id="3e9dc-1693">Chcete-li definovat datovou sadu objektu Teradata Blob, nastavte **typ** datové sady, která **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1693">To define a Teradata Blob dataset, set the **type** of the dataset to **RelationalTable**.</span></span> <span data-ttu-id="3e9dc-1694">Momentálně nejsou k dispozici žádné vlastnosti typu podporované pro datovou sadu Teradata.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1694">Currently, there are no type properties supported for the Teradata dataset.</span></span> 

#### <a name="example"></a><span data-ttu-id="3e9dc-1695">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1695">Example</span></span>
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1696">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1696">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1697">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1697">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1698">Pokud kopírujete data z databáze Teradata, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1698">If you are copying data from a Teradata database, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1699">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1699">Property</span></span> | <span data-ttu-id="3e9dc-1700">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1700">Description</span></span> | <span data-ttu-id="3e9dc-1701">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1701">Allowed values</span></span> | <span data-ttu-id="3e9dc-1702">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1702">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1703">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1703">query</span></span> |<span data-ttu-id="3e9dc-1704">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1704">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1705">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1705">SQL query string.</span></span> <span data-ttu-id="3e9dc-1706">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1706">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-1707">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1707">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1708">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1708">Example</span></span>

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3e9dc-1709">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1709">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) article.</span></span>

## <a name="cassandra"></a><span data-ttu-id="3e9dc-1710">Cassandra</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1710">Cassandra</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-1711">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1711">Linked service</span></span>
<span data-ttu-id="3e9dc-1712">Pokud chcete definovat Cassandra propojené služby, nastavte **typ** propojené služby pro **OnPremisesCassandra**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1712">To define a Cassandra linked service, set the **type** of the linked service to **OnPremisesCassandra**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1713">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1713">Property</span></span> | <span data-ttu-id="3e9dc-1714">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1714">Description</span></span> | <span data-ttu-id="3e9dc-1715">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1715">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1716">hostitele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1716">host</span></span> |<span data-ttu-id="3e9dc-1717">Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1717">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="3e9dc-1718">Zadejte seznam IP adres nebo názvů hostitelů se připojit na všechny servery současně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1718">Specify a comma-separated list of IP addresses or host names to connect to all servers concurrently.</span></span> |<span data-ttu-id="3e9dc-1719">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1719">Yes</span></span> |
| <span data-ttu-id="3e9dc-1720">port</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1720">port</span></span> |<span data-ttu-id="3e9dc-1721">Port TCP, který používá Cassandra server naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1721">The TCP port that the Cassandra server uses to listen for client connections.</span></span> |<span data-ttu-id="3e9dc-1722">Ne, výchozí hodnota: 9042</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1722">No, default value: 9042</span></span> |
| <span data-ttu-id="3e9dc-1723">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1723">authenticationType</span></span> |<span data-ttu-id="3e9dc-1724">Basic nebo Anonymous</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1724">Basic, or Anonymous</span></span> |<span data-ttu-id="3e9dc-1725">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1725">Yes</span></span> |
| <span data-ttu-id="3e9dc-1726">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1726">username</span></span> |<span data-ttu-id="3e9dc-1727">Zadejte uživatelské jméno pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1727">Specify user name for the user account.</span></span> |<span data-ttu-id="3e9dc-1728">Ano, pokud authenticationType je nastaven na Basic.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1728">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="3e9dc-1729">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1729">password</span></span> |<span data-ttu-id="3e9dc-1730">Zadejte heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1730">Specify password for the user account.</span></span> |<span data-ttu-id="3e9dc-1731">Ano, pokud authenticationType je nastaven na Basic.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1731">Yes, if authenticationType is set to Basic.</span></span> |
| <span data-ttu-id="3e9dc-1732">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1732">gatewayName</span></span> |<span data-ttu-id="3e9dc-1733">Název brány, která se používá pro připojení k místní databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1733">The name of the gateway that is used to connect to the on-premises Cassandra database.</span></span> |<span data-ttu-id="3e9dc-1734">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1734">Yes</span></span> |
| <span data-ttu-id="3e9dc-1735">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1735">encryptedCredential</span></span> |<span data-ttu-id="3e9dc-1736">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1736">Credential encrypted by the gateway.</span></span> |<span data-ttu-id="3e9dc-1737">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1737">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1738">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1738">Example</span></span>

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1739">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1739">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-1740">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1740">Dataset</span></span>
<span data-ttu-id="3e9dc-1741">Chcete-li definovat Cassandra datovou sadu, nastavte **typ** datové sady, která **CassandraTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1741">To define a Cassandra dataset, set the **type** of the dataset to **CassandraTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1742">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1742">Property</span></span> | <span data-ttu-id="3e9dc-1743">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1743">Description</span></span> | <span data-ttu-id="3e9dc-1744">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1744">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1745">keyspace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1745">keyspace</span></span> |<span data-ttu-id="3e9dc-1746">Název keyspace nebo schéma v Cassandra databáze.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1746">Name of the keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="3e9dc-1747">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1747">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="3e9dc-1748">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1748">tableName</span></span> |<span data-ttu-id="3e9dc-1749">Název tabulky v databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1749">Name of the table in Cassandra database.</span></span> |<span data-ttu-id="3e9dc-1750">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1750">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1751">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1751">Example</span></span>

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1752">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1752">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) article.</span></span> 

### <a name="cassandra-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1753">Zdroj Cassandra v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1753">Cassandra Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1754">Pokud jsou kopírování dat z Cassandra, nastavte **typ zdroje** kopie aktivity na **CassandraSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1754">If you are copying data from Cassandra, set the **source type** of the copy activity to **CassandraSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1755">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1755">Property</span></span> | <span data-ttu-id="3e9dc-1756">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1756">Description</span></span> | <span data-ttu-id="3e9dc-1757">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1757">Allowed values</span></span> | <span data-ttu-id="3e9dc-1758">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1758">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1759">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1759">query</span></span> |<span data-ttu-id="3e9dc-1760">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1760">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1761">Dotaz SQL 92 nebo CQL dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1761">SQL-92 query or CQL query.</span></span> <span data-ttu-id="3e9dc-1762">V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1762">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="3e9dc-1763">Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** představují tabulky, které mají být zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1763">When using SQL query, specify **keyspace name.table name** to represent the table you want to query.</span></span> |<span data-ttu-id="3e9dc-1764">Ne (pokud jsou definovány tableName a keyspace v sadě dat).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1764">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="3e9dc-1765">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1765">consistencyLevel</span></span> |<span data-ttu-id="3e9dc-1766">Úroveň konzistence Určuje, kolik repliky musí odpovědět na požadavek čtení před vrácením dat do klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1766">The consistency level specifies how many replicas must respond to a read request before returning data to the client application.</span></span> <span data-ttu-id="3e9dc-1767">Cassandra ověří zadaný počet replik pro data, aby pokryl požadavek na čtení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1767">Cassandra checks the specified number of replicas for data to satisfy the read request.</span></span> |<span data-ttu-id="3e9dc-1768">JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1768">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="3e9dc-1769">V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1769">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="3e9dc-1770">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1770">No.</span></span> <span data-ttu-id="3e9dc-1771">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1771">Default value is ONE.</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1772">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1772">Example</span></span>
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-1773">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1773">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) article.</span></span>

## <a name="mongodb"></a><span data-ttu-id="3e9dc-1774">MongoDB</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1774">MongoDB</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-1775">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1775">Linked service</span></span>
<span data-ttu-id="3e9dc-1776">K definování MongoDB propojené služby, nastavte **typ** propojené služby pro **OnPremisesMongoDB**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1776">To define a MongoDB linked service, set the **type** of the linked service to **OnPremisesMongoDB**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1777">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1777">Property</span></span> | <span data-ttu-id="3e9dc-1778">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1778">Description</span></span> | <span data-ttu-id="3e9dc-1779">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1779">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1780">server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1780">server</span></span> |<span data-ttu-id="3e9dc-1781">IP adresa nebo název hostitele serveru MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1781">IP address or host name of the MongoDB server.</span></span> |<span data-ttu-id="3e9dc-1782">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1782">Yes</span></span> |
| <span data-ttu-id="3e9dc-1783">port</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1783">port</span></span> |<span data-ttu-id="3e9dc-1784">Port TCP, který používá MongoDB server naslouchat pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1784">TCP port that the MongoDB server uses to listen for client connections.</span></span> |<span data-ttu-id="3e9dc-1785">Volitelné, výchozí hodnota: 27017</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1785">Optional, default value: 27017</span></span> |
| <span data-ttu-id="3e9dc-1786">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1786">authenticationType</span></span> |<span data-ttu-id="3e9dc-1787">Základní, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1787">Basic, or Anonymous.</span></span> |<span data-ttu-id="3e9dc-1788">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1788">Yes</span></span> |
| <span data-ttu-id="3e9dc-1789">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1789">username</span></span> |<span data-ttu-id="3e9dc-1790">Uživatelský účet pro přístup k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1790">User account to access MongoDB.</span></span> |<span data-ttu-id="3e9dc-1791">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1791">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="3e9dc-1792">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1792">password</span></span> |<span data-ttu-id="3e9dc-1793">Heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1793">Password for the user.</span></span> |<span data-ttu-id="3e9dc-1794">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1794">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="3e9dc-1795">authSource</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1795">authSource</span></span> |<span data-ttu-id="3e9dc-1796">Název databáze MongoDB, kterou chcete použít ke kontrole přihlašovacích údajů pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1796">Name of the MongoDB database that you want to use to check your credentials for authentication.</span></span> |<span data-ttu-id="3e9dc-1797">Volitelný parametr (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1797">Optional (if basic authentication is used).</span></span> <span data-ttu-id="3e9dc-1798">Výchozí: používá účet správce a do databáze určené pomocí vlastnost databaseName.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1798">default: uses the admin account and the database specified using databaseName property.</span></span> |
| <span data-ttu-id="3e9dc-1799">Název databáze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1799">databaseName</span></span> |<span data-ttu-id="3e9dc-1800">Název databáze MongoDB, kterou chcete získat přístup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1800">Name of the MongoDB database that you want to access.</span></span> |<span data-ttu-id="3e9dc-1801">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1801">Yes</span></span> |
| <span data-ttu-id="3e9dc-1802">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1802">gatewayName</span></span> |<span data-ttu-id="3e9dc-1803">Název brány, který přistupuje k úložišti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1803">Name of the gateway that accesses the data store.</span></span> |<span data-ttu-id="3e9dc-1804">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1804">Yes</span></span> |
| <span data-ttu-id="3e9dc-1805">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1805">encryptedCredential</span></span> |<span data-ttu-id="3e9dc-1806">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1806">Credential encrypted by gateway.</span></span> |<span data-ttu-id="3e9dc-1807">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1807">Optional</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1808">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1808">Example</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1809">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1809">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1810">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1810">Dataset</span></span>
<span data-ttu-id="3e9dc-1811">Chcete-li definovat datovou sadu s MongoDB, nastavte **typ** datové sady, která **MongoDbCollection**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1811">To define a MongoDB dataset, set the **type** of the dataset to **MongoDbCollection**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1812">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1812">Property</span></span> | <span data-ttu-id="3e9dc-1813">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1813">Description</span></span> | <span data-ttu-id="3e9dc-1814">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1814">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1815">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1815">collectionName</span></span> |<span data-ttu-id="3e9dc-1816">Název kolekce v databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1816">Name of the collection in MongoDB database.</span></span> |<span data-ttu-id="3e9dc-1817">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1817">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1818">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1818">Example</span></span>

```json
{
    "name": "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

<span data-ttu-id="3e9dc-1819">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1819">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span></span>

#### <a name="mongodb-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1820">Zdroj MongoDB v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1820">MongoDB Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1821">Pokud jsou kopírování dat z MongoDB, nastavte **typ zdroje** kopie aktivity na **MongoDbSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1821">If you are copying data from MongoDB, set the **source type** of the copy activity to **MongoDbSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1822">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1822">Property</span></span> | <span data-ttu-id="3e9dc-1823">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1823">Description</span></span> | <span data-ttu-id="3e9dc-1824">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1824">Allowed values</span></span> | <span data-ttu-id="3e9dc-1825">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1825">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1826">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1826">query</span></span> |<span data-ttu-id="3e9dc-1827">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1827">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-1828">Řetězec dotazu SQL 92.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1828">SQL-92 query string.</span></span> <span data-ttu-id="3e9dc-1829">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1829">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-1830">Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1830">No (if **collectionName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1831">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1831">Example</span></span>

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1832">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1832">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span></span>

## <a name="amazon-s3"></a><span data-ttu-id="3e9dc-1833">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1833">Amazon S3</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-1834">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1834">Linked service</span></span>
<span data-ttu-id="3e9dc-1835">K definování Amazon S3 propojené služby, nastavte **typ** propojené služby pro **AwsAccessKey**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1835">To define an Amazon S3 linked service, set the **type** of the linked service to **AwsAccessKey**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-1836">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1836">Property</span></span> | <span data-ttu-id="3e9dc-1837">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1837">Description</span></span> | <span data-ttu-id="3e9dc-1838">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1838">Allowed values</span></span> | <span data-ttu-id="3e9dc-1839">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1839">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1840">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1840">accessKeyID</span></span> |<span data-ttu-id="3e9dc-1841">ID tajný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1841">ID of the secret access key.</span></span> |<span data-ttu-id="3e9dc-1842">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1842">string</span></span> |<span data-ttu-id="3e9dc-1843">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1843">Yes</span></span> |
| <span data-ttu-id="3e9dc-1844">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1844">secretAccessKey</span></span> |<span data-ttu-id="3e9dc-1845">Tajný přístupový klíč sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1845">The secret access key itself.</span></span> |<span data-ttu-id="3e9dc-1846">Šifrované tajné řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1846">Encrypted secret string</span></span> |<span data-ttu-id="3e9dc-1847">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1847">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-1848">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1848">Example</span></span>
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1849">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1849">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1850">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1850">Dataset</span></span>
<span data-ttu-id="3e9dc-1851">Chcete-li definovat datové sadě služby Amazon S3, nastavte **typ** datové sady, která **AmazonS3**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1851">To define an Amazon S3 dataset, set the **type** of the dataset to **AmazonS3**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1852">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1852">Property</span></span> | <span data-ttu-id="3e9dc-1853">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1853">Description</span></span> | <span data-ttu-id="3e9dc-1854">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1854">Allowed values</span></span> | <span data-ttu-id="3e9dc-1855">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1855">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1856">bucketName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1856">bucketName</span></span> |<span data-ttu-id="3e9dc-1857">Název sady S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1857">The S3 bucket name.</span></span> |<span data-ttu-id="3e9dc-1858">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1858">String</span></span> |<span data-ttu-id="3e9dc-1859">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1859">Yes</span></span> |
| <span data-ttu-id="3e9dc-1860">key</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1860">key</span></span> |<span data-ttu-id="3e9dc-1861">Klíč objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1861">The S3 object key.</span></span> |<span data-ttu-id="3e9dc-1862">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1862">String</span></span> |<span data-ttu-id="3e9dc-1863">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1863">No</span></span> |
| <span data-ttu-id="3e9dc-1864">Předpona</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1864">prefix</span></span> |<span data-ttu-id="3e9dc-1865">Předpona pro klíč objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1865">Prefix for the S3 object key.</span></span> <span data-ttu-id="3e9dc-1866">Jsou vybrané objekty, jejichž klíče začít s touto předponou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1866">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="3e9dc-1867">Platí pouze v případě, klíč je prázdný.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1867">Applies only when key is empty.</span></span> |<span data-ttu-id="3e9dc-1868">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1868">String</span></span> |<span data-ttu-id="3e9dc-1869">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1869">No</span></span> |
| <span data-ttu-id="3e9dc-1870">Verze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1870">version</span></span> |<span data-ttu-id="3e9dc-1871">Verze objektu S3, pokud je povolena Správa verzí S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1871">The version of S3 object if S3 versioning is enabled.</span></span> |<span data-ttu-id="3e9dc-1872">Řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1872">String</span></span> |<span data-ttu-id="3e9dc-1873">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1873">No</span></span> |
| <span data-ttu-id="3e9dc-1874">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1874">format</span></span> | <span data-ttu-id="3e9dc-1875">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1875">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-1876">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1876">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-1877">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1877">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-1878">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1878">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-1879">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1879">No</span></span> | |
| <span data-ttu-id="3e9dc-1880">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1880">compression</span></span> | <span data-ttu-id="3e9dc-1881">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1881">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-1882">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1882">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-1883">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1883">The supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-1884">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1884">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-1885">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1885">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="3e9dc-1886">bucketName + klíče určuje umístění objektu S3, kde blok je kořenový kontejner pro objekty S3 a klíč je úplná cesta k objektu S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1886">bucketName + key specifies the location of the S3 object where bucket is the root container for S3 objects and key is the full path to S3 object.</span></span>

#### <a name="example-sample-dataset-with-prefix"></a><span data-ttu-id="3e9dc-1887">Příklad: Ukázkovou datovou sadu s předponou</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1887">Example: Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a><span data-ttu-id="3e9dc-1888">Příklad: Ukázka datové sady (verze)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1888">Example: Sample data set (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a><span data-ttu-id="3e9dc-1889">Příklad: Dynamické cesty pro S3</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1889">Example: Dynamic paths for S3</span></span>
<span data-ttu-id="3e9dc-1890">V ukázce používáme pevné hodnoty pro klíče a bucketName vlastnosti v datové sadě Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1890">In the sample, we use fixed values for key and bucketName properties in the Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

<span data-ttu-id="3e9dc-1891">Můžete mít vypočítat klíč a bucketName dynamicky za běhu pomocí systémové proměnné, jako je například SliceStart služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1891">You can have Data Factory calculate the key and bucketName dynamically at runtime by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="3e9dc-1892">Můžete provést stejný pro vlastnost předponu datové sadě služby Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1892">You can do the same for the prefix property of an Amazon S3 dataset.</span></span> <span data-ttu-id="3e9dc-1893">V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam podporované funkce a proměnné.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1893">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of supported functions and variables.</span></span>

<span data-ttu-id="3e9dc-1894">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1894">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1895">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1895">File System Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1896">Pokud kopírujete data z Amazonu S3, nastavte **typ zdroje** kopie aktivity na **FileSystemSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1896">If you are copying data from Amazon S3, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>


| <span data-ttu-id="3e9dc-1897">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1897">Property</span></span> | <span data-ttu-id="3e9dc-1898">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1898">Description</span></span> | <span data-ttu-id="3e9dc-1899">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1899">Allowed values</span></span> | <span data-ttu-id="3e9dc-1900">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1900">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1901">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1901">recursive</span></span> |<span data-ttu-id="3e9dc-1902">Určuje, jestli k rekurzivnímu seznamu S3 objekty v adresáři.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1902">Specifies whether to recursively list S3 objects under the directory.</span></span> |<span data-ttu-id="3e9dc-1903">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1903">true/false</span></span> |<span data-ttu-id="3e9dc-1904">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1904">No</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-1905">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1905">Example</span></span>


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-1906">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1906">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span></span>

## <a name="file-system"></a><span data-ttu-id="3e9dc-1907">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1907">File System</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-1908">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1908">Linked service</span></span>
<span data-ttu-id="3e9dc-1909">Systém souborů na místě můžete propojit s objektem pro vytváření dat Azure s **místní souborový Server** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1909">You can link an on-premises file system to an Azure data factory with the **On-Premises File Server** linked service.</span></span> <span data-ttu-id="3e9dc-1910">Následující tabulka obsahuje popis elementy JSON, které jsou specifické pro službu propojené místní souborový Server.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1910">The following table provides descriptions for JSON elements that are specific to the On-Premises File Server linked service.</span></span>

| <span data-ttu-id="3e9dc-1911">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1911">Property</span></span> | <span data-ttu-id="3e9dc-1912">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1912">Description</span></span> | <span data-ttu-id="3e9dc-1913">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1913">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1914">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1914">type</span></span> |<span data-ttu-id="3e9dc-1915">Ujistěte se, že je vlastnost Typ nastavena **OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1915">Ensure that the type property is set to **OnPremisesFileServer**.</span></span> |<span data-ttu-id="3e9dc-1916">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1916">Yes</span></span> |
| <span data-ttu-id="3e9dc-1917">hostitele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1917">host</span></span> |<span data-ttu-id="3e9dc-1918">Určuje cestu kořenové složky, kterou chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1918">Specifies the root path of the folder that you want to copy.</span></span> <span data-ttu-id="3e9dc-1919">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1919">Use the escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="3e9dc-1920">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1920">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="3e9dc-1921">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1921">Yes</span></span> |
| <span data-ttu-id="3e9dc-1922">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1922">userid</span></span> |<span data-ttu-id="3e9dc-1923">Zadejte ID uživatele, který má přístup k serveru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1923">Specify the ID of the user who has access to the server.</span></span> |<span data-ttu-id="3e9dc-1924">Ne (když zvolíte encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1924">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="3e9dc-1925">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1925">password</span></span> |<span data-ttu-id="3e9dc-1926">Zadejte heslo pro uživatele (ID uživatele).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1926">Specify the password for the user (userid).</span></span> |<span data-ttu-id="3e9dc-1927">Ne (když zvolíte encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1927">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="3e9dc-1928">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1928">encryptedCredential</span></span> |<span data-ttu-id="3e9dc-1929">Zadejte zašifrované přihlašovací údaje, které můžete získat spuštěním rutiny New-AzureRmDataFactoryEncryptValue.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1929">Specify the encrypted credentials that you can get by running the New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="3e9dc-1930">Ne (když zvolíte možnost zadat ID uživatele a heslo ve formátu prostého textu)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1930">No (if you choose to specify userid and password in plain text)</span></span> |
| <span data-ttu-id="3e9dc-1931">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1931">gatewayName</span></span> |<span data-ttu-id="3e9dc-1932">Určuje název brány, kterou Data Factory měla použít pro připojení k souborovému serveru místně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1932">Specifies the name of the gateway that Data Factory should use to connect to the on-premises file server.</span></span> |<span data-ttu-id="3e9dc-1933">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1933">Yes</span></span> |

#### <a name="sample-folder-path-definitions"></a><span data-ttu-id="3e9dc-1934">Ukázka složky cesta definice</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1934">Sample folder path definitions</span></span> 
| <span data-ttu-id="3e9dc-1935">Scénář</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1935">Scenario</span></span> | <span data-ttu-id="3e9dc-1936">Hostování v definici propojené služby</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1936">Host in linked service definition</span></span> | <span data-ttu-id="3e9dc-1937">folderPath v definici datové sady</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1937">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1938">Místní složky v počítači brány pro správu dat:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1938">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="3e9dc-1939">Příklady: D:\\ \* nebo D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1939">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="3e9dc-1940">D:\\ \\ (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1940">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="3e9dc-1941">localhost (pro starší verze než Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1941">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="3e9dc-1942">. \\ \\ nebo složky\\\\podsložky (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1942">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="3e9dc-1943">D:\\ \\ nebo D:\\\\složky\\\\podsložky (pro brány verzi nižší než 2.0)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1943">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="3e9dc-1944">Vzdálené sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1944">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="3e9dc-1945">Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\*</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1945">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="3e9dc-1946">\\\\\\\\myserver\\\\sdílet</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1946">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="3e9dc-1947">. \\ \\ nebo složky\\\\podsložky</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1947">.\\\\ or folder\\\\subfolder</span></span> |


#### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="3e9dc-1948">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1948">Example: Using username and password in plain text</span></span>

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a><span data-ttu-id="3e9dc-1949">Příklad: Pomocí encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1949">Example: Using encryptedcredential</span></span>

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-1950">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1950">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-1951">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1951">Dataset</span></span>
<span data-ttu-id="3e9dc-1952">Chcete-li definovat datovou sadu systému souborů, nastavte **typ** datové sady, která **sdílení souborů**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1952">To define a File System dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-1953">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1953">Property</span></span> | <span data-ttu-id="3e9dc-1954">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1954">Description</span></span> | <span data-ttu-id="3e9dc-1955">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1955">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-1956">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1956">folderPath</span></span> |<span data-ttu-id="3e9dc-1957">Určuje dílčí cestou ke složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1957">Specifies the subpath to the folder.</span></span> <span data-ttu-id="3e9dc-1958">Použít řídicí znak ' \' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1958">Use the escape character ‘\’ for special characters in the string.</span></span> <span data-ttu-id="3e9dc-1959">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1959">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3e9dc-1960">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1960">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3e9dc-1961">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1961">Yes</span></span> |
| <span data-ttu-id="3e9dc-1962">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1962">fileName</span></span> |<span data-ttu-id="3e9dc-1963">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1963">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="3e9dc-1964">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1964">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="3e9dc-1965">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1965">When fileName is not specified for an output dataset, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="3e9dc-1966">`Data.<Guid>.txt`(Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1966">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="3e9dc-1967">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1967">No</span></span> |
| <span data-ttu-id="3e9dc-1968">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1968">fileFilter</span></span> |<span data-ttu-id="3e9dc-1969">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1969">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="3e9dc-1970">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1970">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3e9dc-1971">Příklad 1: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1971">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="3e9dc-1972">Příklad 2: "fileFilter": 2016 - 1-?. TXT"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1972">Example 2: "fileFilter": 2016-1-?.txt"</span></span><br/><br/><span data-ttu-id="3e9dc-1973">Všimněte si, že fileFilter je použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1973">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="3e9dc-1974">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1974">No</span></span> |
| <span data-ttu-id="3e9dc-1975">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1975">partitionedBy</span></span> |<span data-ttu-id="3e9dc-1976">PartitionedBy můžete použít k určení dynamické folderPath nebo název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1976">You can use partitionedBy to specify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="3e9dc-1977">Příkladem je folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1977">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-1978">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1978">No</span></span> |
| <span data-ttu-id="3e9dc-1979">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1979">format</span></span> | <span data-ttu-id="3e9dc-1980">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1980">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-1981">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1981">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-1982">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1982">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-1983">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1983">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-1984">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1984">No</span></span> |
| <span data-ttu-id="3e9dc-1985">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1985">compression</span></span> | <span data-ttu-id="3e9dc-1986">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1986">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-1987">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1987">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-1988">v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1988">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-1989">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1989">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3e9dc-1990">Název souboru a fileFilter nelze současně použít.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1990">You cannot use fileName and fileFilter simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-1991">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1991">Example</span></span>

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-1992">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1992">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3e9dc-1993">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1993">File System Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-1994">Pokud jsou kopírování dat systému souborů, nastavte **typ zdroje** kopie aktivity na **FileSystemSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1994">If you are copying data from File System, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-1995">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1995">Property</span></span> | <span data-ttu-id="3e9dc-1996">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1996">Description</span></span> | <span data-ttu-id="3e9dc-1997">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1997">Allowed values</span></span> | <span data-ttu-id="3e9dc-1998">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1998">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-1999">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-1999">recursive</span></span> |<span data-ttu-id="3e9dc-2000">Označuje, zda je data načíst rekurzivně z podsložky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2000">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-2001">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2001">True, False (default)</span></span> |<span data-ttu-id="3e9dc-2002">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2002">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2003">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2003">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
<span data-ttu-id="3e9dc-2004">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2004">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

### <a name="file-system-sink-in-copy-activity"></a><span data-ttu-id="3e9dc-2005">Systém souborů jímky v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2005">File System Sink in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2006">Pokud data kopírujete do systému souborů, nastavte **typ jímky** kopie aktivity na **FileSystemSink**a zadejte následující vlastnosti v **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2006">If you are copying data to File System, set the **sink type** of the copy activity to **FileSystemSink**, and specify following properties in the **sink** section:</span></span>

| <span data-ttu-id="3e9dc-2007">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2007">Property</span></span> | <span data-ttu-id="3e9dc-2008">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2008">Description</span></span> | <span data-ttu-id="3e9dc-2009">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2009">Allowed values</span></span> | <span data-ttu-id="3e9dc-2010">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2010">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2011">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2011">copyBehavior</span></span> |<span data-ttu-id="3e9dc-2012">Definuje chování kopie, pokud je zdroj BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2012">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="3e9dc-2013">**PreserveHierarchy:** zachovává hierarchii souborů v cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2013">**PreserveHierarchy:** Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="3e9dc-2014">To znamená relativní cesta zdrojového souboru do zdrojové složky je stejný jako relativní cestu k souboru cíl k cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2014">That is, the relative path of the source file to the source folder is the same as the relative path of the target file to the target folder.</span></span><br/><br/><span data-ttu-id="3e9dc-2015">**FlattenHierarchy:** všechny soubory ze zdrojové složky jsou vytvořené v první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2015">**FlattenHierarchy:** All files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="3e9dc-2016">Cílové soubory jsou vytvořeny pomocí názvu objektu generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2016">The target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="3e9dc-2017">**MergeFiles:** slučuje všechny soubory ze zdrojové složky pro jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2017">**MergeFiles:** Merges all files from the source folder to one file.</span></span> <span data-ttu-id="3e9dc-2018">Pokud je zadán název nebo objekt blob název souboru, název souboru sloučené je zadaný název.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2018">If the file name/blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="3e9dc-2019">Jinak je název automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2019">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="3e9dc-2020">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2020">No</span></span> |
<span data-ttu-id="3e9dc-2021">Auto-</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2021">auto-</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-2022">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2022">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-2023">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2023">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

## <a name="ftp"></a><span data-ttu-id="3e9dc-2024">FTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2024">FTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2025">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2025">Linked service</span></span>
<span data-ttu-id="3e9dc-2026">K definování k serveru FTP propojené služby, nastavte **typ** propojené služby pro **Server_ftp**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2026">To define an FTP linked service, set the **type** of the linked service to **FtpServer**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2027">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2027">Property</span></span> | <span data-ttu-id="3e9dc-2028">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2028">Description</span></span> | <span data-ttu-id="3e9dc-2029">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2029">Required</span></span> | <span data-ttu-id="3e9dc-2030">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2030">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2031">hostitele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2031">host</span></span> |<span data-ttu-id="3e9dc-2032">Název nebo IP adresa serveru FTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2032">Name or IP address of the FTP Server</span></span> |<span data-ttu-id="3e9dc-2033">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2033">Yes</span></span> |&nbsp; |
| <span data-ttu-id="3e9dc-2034">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2034">authenticationType</span></span> |<span data-ttu-id="3e9dc-2035">Zadejte typ ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2035">Specify authentication type</span></span> |<span data-ttu-id="3e9dc-2036">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2036">Yes</span></span> |<span data-ttu-id="3e9dc-2037">Anonymní, základní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2037">Basic, Anonymous</span></span> |
| <span data-ttu-id="3e9dc-2038">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2038">username</span></span> |<span data-ttu-id="3e9dc-2039">Uživatel, který má přístup k serveru FTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2039">User who has access to the FTP server</span></span> |<span data-ttu-id="3e9dc-2040">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2040">No</span></span> |&nbsp; |
| <span data-ttu-id="3e9dc-2041">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2041">password</span></span> |<span data-ttu-id="3e9dc-2042">Heslo pro uživatele (username)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2042">Password for the user (username)</span></span> |<span data-ttu-id="3e9dc-2043">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2043">No</span></span> |&nbsp; |
| <span data-ttu-id="3e9dc-2044">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2044">encryptedCredential</span></span> |<span data-ttu-id="3e9dc-2045">Šifrovaný přihlašovací údaje pro přístup k serveru FTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2045">Encrypted credential to access the FTP server</span></span> |<span data-ttu-id="3e9dc-2046">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2046">No</span></span> |&nbsp; |
| <span data-ttu-id="3e9dc-2047">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2047">gatewayName</span></span> |<span data-ttu-id="3e9dc-2048">Název brány, brána pro správu dat pro připojení k serveru FTP na místě</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2048">Name of the Data Management Gateway gateway to connect to an on-premises FTP server</span></span> |<span data-ttu-id="3e9dc-2049">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2049">No</span></span> |&nbsp; |
| <span data-ttu-id="3e9dc-2050">port</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2050">port</span></span> |<span data-ttu-id="3e9dc-2051">Port, na kterém naslouchá FTP server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2051">Port on which the FTP server is listening</span></span> |<span data-ttu-id="3e9dc-2052">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2052">No</span></span> |<span data-ttu-id="3e9dc-2053">21</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2053">21</span></span> |
| <span data-ttu-id="3e9dc-2054">enableSsl</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2054">enableSsl</span></span> |<span data-ttu-id="3e9dc-2055">Určete, zda chcete pomocí funkce FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2055">Specify whether to use FTP over SSL/TLS channel</span></span> |<span data-ttu-id="3e9dc-2056">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2056">No</span></span> |<span data-ttu-id="3e9dc-2057">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2057">true</span></span> |
| <span data-ttu-id="3e9dc-2058">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2058">enableServerCertificateValidation</span></span> |<span data-ttu-id="3e9dc-2059">Určete, zda chcete povolit ověřování certifikátu protokolu SSL serveru, pokud používáte FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2059">Specify whether to enable server SSL certificate validation when using FTP over SSL/TLS channel</span></span> |<span data-ttu-id="3e9dc-2060">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2060">No</span></span> |<span data-ttu-id="3e9dc-2061">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2061">true</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3e9dc-2062">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2062">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="3e9dc-2063">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2063">Example: Using username and password in plain text for basic authentication</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="3e9dc-2064">Příklad: Pomocí portu, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2064">Example: Using port, enableSsl, enableServerCertificateValidation</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="3e9dc-2065">Příklad: Pomocí encryptedCredential pro ověřování a brány</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2065">Example: Using encryptedCredential for authentication and gateway</span></span>

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

<span data-ttu-id="3e9dc-2066">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2066">For more information, see [FTP connector](data-factory-ftp-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-2067">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2067">Dataset</span></span>
<span data-ttu-id="3e9dc-2068">Chcete-li definovat datové sadě služby FTP, nastavte **typ** datové sady, která **sdílení souborů**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2068">To define an FTP dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2069">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2069">Property</span></span> | <span data-ttu-id="3e9dc-2070">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2070">Description</span></span> | <span data-ttu-id="3e9dc-2071">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2071">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2072">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2072">folderPath</span></span> |<span data-ttu-id="3e9dc-2073">Sub – cesta ke složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2073">Sub path to the folder.</span></span> <span data-ttu-id="3e9dc-2074">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2074">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="3e9dc-2075">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2075">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3e9dc-2076">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2076">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3e9dc-2077">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2077">Yes</span></span> 
| <span data-ttu-id="3e9dc-2078">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2078">fileName</span></span> |<span data-ttu-id="3e9dc-2079">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2079">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="3e9dc-2080">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2080">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="3e9dc-2081">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2081">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="3e9dc-2082">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2082">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3e9dc-2083">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2083">No</span></span> |
| <span data-ttu-id="3e9dc-2084">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2084">fileFilter</span></span> |<span data-ttu-id="3e9dc-2085">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2085">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="3e9dc-2086">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2086">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3e9dc-2087">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2087">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="3e9dc-2088">Příklad 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2088">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="3e9dc-2089">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2089">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="3e9dc-2090">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2090">This property is not supported with HDFS.</span></span> |<span data-ttu-id="3e9dc-2091">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2091">No</span></span> |
| <span data-ttu-id="3e9dc-2092">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2092">partitionedBy</span></span> |<span data-ttu-id="3e9dc-2093">partitionedBy slouží k určení dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2093">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3e9dc-2094">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2094">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-2095">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2095">No</span></span> |
| <span data-ttu-id="3e9dc-2096">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2096">format</span></span> | <span data-ttu-id="3e9dc-2097">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2097">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-2098">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2098">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-2099">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2099">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-2100">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2100">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-2101">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2101">No</span></span> |
| <span data-ttu-id="3e9dc-2102">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2102">compression</span></span> | <span data-ttu-id="3e9dc-2103">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2103">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-2104">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2104">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-2105">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2105">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-2106">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2106">No</span></span> |
| <span data-ttu-id="3e9dc-2107">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2107">useBinaryTransfer</span></span> |<span data-ttu-id="3e9dc-2108">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2108">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="3e9dc-2109">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2109">True for binary mode and false ASCII.</span></span> <span data-ttu-id="3e9dc-2110">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2110">Default value: True.</span></span> <span data-ttu-id="3e9dc-2111">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2111">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="3e9dc-2112">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2112">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3e9dc-2113">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2113">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-2114">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2114">Example</span></span>

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3e9dc-2115">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2115">For more information, see [FTP connector](data-factory-ftp-connector.md#dataset-properties) article.</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2116">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2116">File System Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2117">Pokud kopírujete data ze serveru FTP, nastavte **typ zdroje** kopie aktivity na **FileSystemSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2117">If you are copying data from an FTP server, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2118">Property</span></span> | <span data-ttu-id="3e9dc-2119">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2119">Description</span></span> | <span data-ttu-id="3e9dc-2120">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2120">Allowed values</span></span> | <span data-ttu-id="3e9dc-2121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2121">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2122">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2122">recursive</span></span> |<span data-ttu-id="3e9dc-2123">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2123">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-2124">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2124">True, False (default)</span></span> |<span data-ttu-id="3e9dc-2125">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2125">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2126">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2126">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-2127">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2127">For more information, see [FTP connector](data-factory-ftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="hdfs"></a><span data-ttu-id="3e9dc-2128">HDFS</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2128">HDFS</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2129">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2129">Linked service</span></span>
<span data-ttu-id="3e9dc-2130">K definování HDFS propojené služby, nastavte **typ** propojené služby pro **Hdfs**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2130">To define a HDFS linked service, set the **type** of the linked service to **Hdfs**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2131">Property</span></span> | <span data-ttu-id="3e9dc-2132">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2132">Description</span></span> | <span data-ttu-id="3e9dc-2133">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2133">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2134">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2134">type</span></span> |<span data-ttu-id="3e9dc-2135">Vlastnost typu musí být nastavena na: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2135">The type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="3e9dc-2136">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2136">Yes</span></span> |
| <span data-ttu-id="3e9dc-2137">URL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2137">Url</span></span> |<span data-ttu-id="3e9dc-2138">Adresa URL HDFS</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2138">URL to the HDFS</span></span> |<span data-ttu-id="3e9dc-2139">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2139">Yes</span></span> |
| <span data-ttu-id="3e9dc-2140">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2140">authenticationType</span></span> |<span data-ttu-id="3e9dc-2141">Anonymní, nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2141">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="3e9dc-2142">Použít **ověřování protokolem Kerberos** HDFS konektor, najdete v části [v této části](#use-kerberos-authentication-for-hdfs-connector) odpovídajícím způsobem nastavit v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2142">To use **Kerberos authentication** for HDFS connector, refer to [this section](#use-kerberos-authentication-for-hdfs-connector) to set up your on-premises environment accordingly.</span></span> |<span data-ttu-id="3e9dc-2143">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2143">Yes</span></span> |
| <span data-ttu-id="3e9dc-2144">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2144">userName</span></span> |<span data-ttu-id="3e9dc-2145">Ověřování uživatelského jména pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2145">Username for Windows authentication.</span></span> |<span data-ttu-id="3e9dc-2146">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2146">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="3e9dc-2147">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2147">password</span></span> |<span data-ttu-id="3e9dc-2148">Heslo pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2148">Password for Windows authentication.</span></span> |<span data-ttu-id="3e9dc-2149">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2149">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="3e9dc-2150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2150">gatewayName</span></span> |<span data-ttu-id="3e9dc-2151">Název brány, kterou služba Data Factory měla použít pro připojení k HDFS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2151">Name of the gateway that the Data Factory service should use to connect to the HDFS.</span></span> |<span data-ttu-id="3e9dc-2152">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2152">Yes</span></span> |
| <span data-ttu-id="3e9dc-2153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2153">encryptedCredential</span></span> |<span data-ttu-id="3e9dc-2154">[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup pověření přístup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2154">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential.</span></span> |<span data-ttu-id="3e9dc-2155">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2155">No</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3e9dc-2156">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2156">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a><span data-ttu-id="3e9dc-2157">Příklad: Pomocí ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2157">Example: Using Windows authentication</span></span>

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-2158">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2158">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-2159">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2159">Dataset</span></span>
<span data-ttu-id="3e9dc-2160">Chcete-li definovat HDFS datovou sadu, nastavte **typ** datové sady, která **sdílení souborů**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2160">To define a HDFS dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2161">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2161">Property</span></span> | <span data-ttu-id="3e9dc-2162">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2162">Description</span></span> | <span data-ttu-id="3e9dc-2163">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2163">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2164">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2164">folderPath</span></span> |<span data-ttu-id="3e9dc-2165">Cesta ke složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2165">Path to the folder.</span></span> <span data-ttu-id="3e9dc-2166">Příklad:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2166">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="3e9dc-2167">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2167">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="3e9dc-2168">Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2168">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="3e9dc-2169">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2169">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3e9dc-2170">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2170">Yes</span></span> |
| <span data-ttu-id="3e9dc-2171">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2171">fileName</span></span> |<span data-ttu-id="3e9dc-2172">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2172">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="3e9dc-2173">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2173">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="3e9dc-2174">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2174">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="3e9dc-2175">Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2175">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3e9dc-2176">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2176">No</span></span> |
| <span data-ttu-id="3e9dc-2177">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2177">partitionedBy</span></span> |<span data-ttu-id="3e9dc-2178">partitionedBy slouží k určení dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2178">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3e9dc-2179">Příklad: folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2179">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-2180">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2180">No</span></span> |
| <span data-ttu-id="3e9dc-2181">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2181">format</span></span> | <span data-ttu-id="3e9dc-2182">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2182">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-2183">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2183">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-2184">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2184">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-2185">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2185">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-2186">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2186">No</span></span> |
| <span data-ttu-id="3e9dc-2187">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2187">compression</span></span> | <span data-ttu-id="3e9dc-2188">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2188">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-2189">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2189">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-2190">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2190">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-2191">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2191">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-2192">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2192">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3e9dc-2193">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2193">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-2194">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2194">Example</span></span>

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3e9dc-2195">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2195">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2196">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2196">File System Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2197">Pokud kopírujete data z HDFS, nastavte **typ zdroje** kopie aktivity na **FileSystemSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2197">If you are copying data from HDFS, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

<span data-ttu-id="3e9dc-2198">**FileSystemSource** podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2198">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="3e9dc-2199">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2199">Property</span></span> | <span data-ttu-id="3e9dc-2200">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2200">Description</span></span> | <span data-ttu-id="3e9dc-2201">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2201">Allowed values</span></span> | <span data-ttu-id="3e9dc-2202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2202">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2203">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2203">recursive</span></span> |<span data-ttu-id="3e9dc-2204">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2204">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-2205">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2205">True, False (default)</span></span> |<span data-ttu-id="3e9dc-2206">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2206">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2207">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2207">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-2208">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2208">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) article.</span></span>

## <a name="sftp"></a><span data-ttu-id="3e9dc-2209">SFTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2209">SFTP</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-2210">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2210">Linked service</span></span>
<span data-ttu-id="3e9dc-2211">K definování protokolu SFTP propojené služby, nastavte **typ** propojené služby pro **Sftp**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2211">To define an SFTP linked service, set the **type** of the linked service to **Sftp**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2212">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2212">Property</span></span> | <span data-ttu-id="3e9dc-2213">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2213">Description</span></span> | <span data-ttu-id="3e9dc-2214">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2214">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2215">hostitele</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2215">host</span></span> | <span data-ttu-id="3e9dc-2216">Název nebo IP adresa serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2216">Name or IP address of the SFTP server.</span></span> |<span data-ttu-id="3e9dc-2217">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2217">Yes</span></span> |
| <span data-ttu-id="3e9dc-2218">port</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2218">port</span></span> |<span data-ttu-id="3e9dc-2219">Port, na kterém naslouchá server pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2219">Port on which the SFTP server is listening.</span></span> <span data-ttu-id="3e9dc-2220">Výchozí hodnota je: 21</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2220">The default value is: 21</span></span> |<span data-ttu-id="3e9dc-2221">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2221">No</span></span> |
| <span data-ttu-id="3e9dc-2222">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2222">authenticationType</span></span> |<span data-ttu-id="3e9dc-2223">Zadejte typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2223">Specify authentication type.</span></span> <span data-ttu-id="3e9dc-2224">Povolené hodnoty: **základní**, **parametru SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2224">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="3e9dc-2225">Odkazovat na [základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2225">Refer to [Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="3e9dc-2226">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2226">Yes</span></span> |
| <span data-ttu-id="3e9dc-2227">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2227">skipHostKeyValidation</span></span> | <span data-ttu-id="3e9dc-2228">Určete, zda chcete přeskočit ověření klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2228">Specify whether to skip host key validation.</span></span> | <span data-ttu-id="3e9dc-2229">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2229">No.</span></span> <span data-ttu-id="3e9dc-2230">Výchozí hodnota: false</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2230">The default value: false</span></span> |
| <span data-ttu-id="3e9dc-2231">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2231">hostKeyFingerprint</span></span> | <span data-ttu-id="3e9dc-2232">Zadejte prstu klíče hostitele.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2232">Specify the finger print of the host key.</span></span> | <span data-ttu-id="3e9dc-2233">Ano, pokud `skipHostKeyValidation` nastaven na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2233">Yes if the `skipHostKeyValidation` is set to false.</span></span>  |
| <span data-ttu-id="3e9dc-2234">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2234">gatewayName</span></span> |<span data-ttu-id="3e9dc-2235">Název brány pro správu dat pro připojení k serveru pomocí protokolu SFTP místně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2235">Name of the Data Management Gateway to connect to an on-premises SFTP server.</span></span> | <span data-ttu-id="3e9dc-2236">Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2236">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="3e9dc-2237">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2237">encryptedCredential</span></span> | <span data-ttu-id="3e9dc-2238">Šifrovaný přihlašovací údaje pro přístup k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2238">Encrypted credential to access the SFTP server.</span></span> <span data-ttu-id="3e9dc-2239">Automaticky generovaný když zadáte v Průvodci kopírovat nebo dialogové okno místní ClickOnce základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsah).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2239">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="3e9dc-2240">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2240">No.</span></span> <span data-ttu-id="3e9dc-2241">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2241">Apply only when copying data from an on-premises SFTP server.</span></span> |

#### <a name="example-using-basic-authentication"></a><span data-ttu-id="3e9dc-2242">Příklad: Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2242">Example: Using basic authentication</span></span>

<span data-ttu-id="3e9dc-2243">Chcete-li základní ověřování použijte, nastavte `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě konektor SFTP obecné ty, které jsou zavedené v poslední části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2243">To use basic authentication, set `authenticationType` as `Basic`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="3e9dc-2244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2244">Property</span></span> | <span data-ttu-id="3e9dc-2245">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2245">Description</span></span> | <span data-ttu-id="3e9dc-2246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2246">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2247">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2247">username</span></span> | <span data-ttu-id="3e9dc-2248">Uživatel, který má přístup k serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2248">User who has access to the SFTP server.</span></span> |<span data-ttu-id="3e9dc-2249">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2249">Yes</span></span> |
| <span data-ttu-id="3e9dc-2250">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2250">password</span></span> | <span data-ttu-id="3e9dc-2251">Heslo pro uživatele (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2251">Password for the user (username).</span></span> | <span data-ttu-id="3e9dc-2252">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2252">Yes</span></span> |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="3e9dc-2253">Příklad: Základní ověřování s šifrované pověření **</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2253">Example: Basic authentication with encrypted credential**</span></span>

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="3e9dc-2254">Pomocí ověření veřejného klíče SSH: **</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2254">Using SSH public key authentication:**</span></span>

<span data-ttu-id="3e9dc-2255">Chcete-li základní ověřování použijte, nastavte `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě konektor SFTP obecné ty, které jsou zavedené v poslední části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2255">To use basic authentication, set `authenticationType` as `SshPublicKey`, and specify the following properties besides the SFTP connector generic ones introduced in the last section:</span></span>

| <span data-ttu-id="3e9dc-2256">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2256">Property</span></span> | <span data-ttu-id="3e9dc-2257">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2257">Description</span></span> | <span data-ttu-id="3e9dc-2258">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2258">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2259">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2259">username</span></span> |<span data-ttu-id="3e9dc-2260">Uživatel, který má přístup k serveru pomocí protokolu SFTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2260">User who has access to the SFTP server</span></span> |<span data-ttu-id="3e9dc-2261">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2261">Yes</span></span> |
| <span data-ttu-id="3e9dc-2262">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2262">privateKeyPath</span></span> | <span data-ttu-id="3e9dc-2263">Zadejte absolutní cestu k souboru privátního klíče můžete přístup k této brány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2263">Specify absolute path to the private key file that gateway can access.</span></span> | <span data-ttu-id="3e9dc-2264">Zadejte buď `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2264">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="3e9dc-2265">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2265">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="3e9dc-2266">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2266">privateKeyContent</span></span> | <span data-ttu-id="3e9dc-2267">Serializovaná řetězec privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2267">A serialized string of the private key content.</span></span> <span data-ttu-id="3e9dc-2268">Průvodce kopírováním můžete číst soubor privátního klíče a automaticky extrahování privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2268">The Copy Wizard can read the private key file and extract the private key content automatically.</span></span> <span data-ttu-id="3e9dc-2269">Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2269">If you are using any other tool/SDK, use the privateKeyPath property instead.</span></span> | <span data-ttu-id="3e9dc-2270">Zadejte buď `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2270">Specify either the `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="3e9dc-2271">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2271">passPhrase</span></span> | <span data-ttu-id="3e9dc-2272">Zadejte průchodu fráze nebo hesla k dešifrování privátního klíče, pokud soubor klíče je chráněn heslo.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2272">Specify the pass phrase/password to decrypt the private key if the key file is protected by a pass phrase.</span></span> | <span data-ttu-id="3e9dc-2273">Ano, pokud heslo je chráněný soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2273">Yes if the private key file is protected by a pass phrase.</span></span> |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="3e9dc-2274">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu **</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2274">Example: SshPublicKey authentication using private key content**</span></span>

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

<span data-ttu-id="3e9dc-2275">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2275">For more information, see [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-2276">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2276">Dataset</span></span>
<span data-ttu-id="3e9dc-2277">Chcete-li definovat datové sadě služby pomocí protokolu SFTP, nastavte **typ** datové sady, která **sdílení souborů**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2277">To define an SFTP dataset, set the **type** of the dataset to **FileShare**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2278">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2278">Property</span></span> | <span data-ttu-id="3e9dc-2279">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2279">Description</span></span> | <span data-ttu-id="3e9dc-2280">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2280">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2281">folderPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2281">folderPath</span></span> |<span data-ttu-id="3e9dc-2282">Sub – cesta ke složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2282">Sub path to the folder.</span></span> <span data-ttu-id="3e9dc-2283">Použít řídicí znak ' \ ' pro speciální znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2283">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="3e9dc-2284">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2284">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="3e9dc-2285">Tato vlastnost se můžete kombinovat **partitionBy** tak, aby měl složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2285">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="3e9dc-2286">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2286">Yes</span></span> |
| <span data-ttu-id="3e9dc-2287">fileName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2287">fileName</span></span> |<span data-ttu-id="3e9dc-2288">Zadejte název souboru do **folderPath** Pokud chcete, aby v tabulce odkazovat na konkrétní soubor ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2288">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="3e9dc-2289">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, tabulka odkazuje na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2289">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="3e9dc-2290">Pokud není zadán název souboru pro datovou sadu výstupů, název vygenerovaný soubor bude v následujícím tento formát:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2290">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="3e9dc-2291">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2291">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="3e9dc-2292">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2292">No</span></span> |
| <span data-ttu-id="3e9dc-2293">fileFilter</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2293">fileFilter</span></span> |<span data-ttu-id="3e9dc-2294">Zadejte filtr pro umožňuje vybrat podmnožinu souborů v folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2294">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="3e9dc-2295">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2295">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="3e9dc-2296">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2296">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="3e9dc-2297">Příklad 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2297">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="3e9dc-2298">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2298">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="3e9dc-2299">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2299">This property is not supported with HDFS.</span></span> |<span data-ttu-id="3e9dc-2300">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2300">No</span></span> |
| <span data-ttu-id="3e9dc-2301">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2301">partitionedBy</span></span> |<span data-ttu-id="3e9dc-2302">partitionedBy slouží k určení dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2302">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="3e9dc-2303">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2303">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="3e9dc-2304">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2304">No</span></span> |
| <span data-ttu-id="3e9dc-2305">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2305">format</span></span> | <span data-ttu-id="3e9dc-2306">Jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2306">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-2307">Nastavte **typ** vlastnost pod formát na jednu z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2307">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="3e9dc-2308">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2308">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="3e9dc-2309">Pokud chcete **zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2309">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="3e9dc-2310">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2310">No</span></span> |
| <span data-ttu-id="3e9dc-2311">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2311">compression</span></span> | <span data-ttu-id="3e9dc-2312">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2312">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-2313">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2313">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-2314">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2314">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-2315">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2315">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-2316">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2316">No</span></span> |
| <span data-ttu-id="3e9dc-2317">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2317">useBinaryTransfer</span></span> |<span data-ttu-id="3e9dc-2318">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2318">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="3e9dc-2319">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2319">True for binary mode and false ASCII.</span></span> <span data-ttu-id="3e9dc-2320">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2320">Default value: True.</span></span> <span data-ttu-id="3e9dc-2321">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2321">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="3e9dc-2322">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2322">No</span></span> |

> [!NOTE]
> <span data-ttu-id="3e9dc-2323">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2323">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-2324">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2324">Example</span></span>

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3e9dc-2325">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2325">For more information, see [SFTP connector](data-factory-sftp-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2326">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2326">File System Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2327">Pokud kopírujete z protokolu SFTP zdroje dat, nastavte **typ zdroje** kopie aktivity na **FileSystemSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2327">If you are copying data from an SFTP source, set the **source type** of the copy activity to **FileSystemSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2328">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2328">Property</span></span> | <span data-ttu-id="3e9dc-2329">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2329">Description</span></span> | <span data-ttu-id="3e9dc-2330">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2330">Allowed values</span></span> | <span data-ttu-id="3e9dc-2331">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2331">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2332">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2332">recursive</span></span> |<span data-ttu-id="3e9dc-2333">Označuje, zda je data načíst rekurzivně z dílčí složky nebo pouze do zadané složky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2333">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="3e9dc-2334">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2334">True, False (default)</span></span> |<span data-ttu-id="3e9dc-2335">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2335">No</span></span> |



#### <a name="example"></a><span data-ttu-id="3e9dc-2336">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2336">Example</span></span>

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-2337">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2337">For more information, see [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="http"></a><span data-ttu-id="3e9dc-2338">HTTP</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2338">HTTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2339">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2339">Linked service</span></span>
<span data-ttu-id="3e9dc-2340">K definování HTTP propojené služby, nastavte **typ** propojené služby pro **Http**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2340">To define a HTTP linked service, set the **type** of the linked service to **Http**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2341">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2341">Property</span></span> | <span data-ttu-id="3e9dc-2342">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2342">Description</span></span> | <span data-ttu-id="3e9dc-2343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2343">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2344">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2344">url</span></span> | <span data-ttu-id="3e9dc-2345">Základní adresu URL na webový server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2345">Base URL to the Web Server</span></span> | <span data-ttu-id="3e9dc-2346">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2346">Yes</span></span> |
| <span data-ttu-id="3e9dc-2347">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2347">authenticationType</span></span> | <span data-ttu-id="3e9dc-2348">Určuje typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2348">Specifies the authentication type.</span></span> <span data-ttu-id="3e9dc-2349">Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2349">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="3e9dc-2350">Naleznete v části dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2350">Refer to sections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="3e9dc-2351">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2351">Yes</span></span> |
| <span data-ttu-id="3e9dc-2352">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2352">enableServerCertificateValidation</span></span> | <span data-ttu-id="3e9dc-2353">Určete, zda chcete povolit ověřování certifikátu protokolu SSL serveru, pokud je zdroj HTTPS webového serveru</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2353">Specify whether to enable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="3e9dc-2354">Ne, výchozí hodnota je true</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2354">No, default is true</span></span> |
| <span data-ttu-id="3e9dc-2355">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2355">gatewayName</span></span> | <span data-ttu-id="3e9dc-2356">Název brány pro správu dat pro připojení k místnímu zdroji HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2356">Name of the Data Management Gateway to connect to an on-premises HTTP source.</span></span> | <span data-ttu-id="3e9dc-2357">Ano, pokud kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2357">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="3e9dc-2358">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2358">encryptedCredential</span></span> | <span data-ttu-id="3e9dc-2359">Šifrovaný přihlašovací údaje pro přístup k koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2359">Encrypted credential to access the HTTP endpoint.</span></span> <span data-ttu-id="3e9dc-2360">Automaticky vygenerované při konfiguraci informace o ověřování v Průvodce kopírováním nebo dialogové okno místní ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2360">Auto-generated when you configure the authentication information in copy wizard or the ClickOnce popup dialog.</span></span> | <span data-ttu-id="3e9dc-2361">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2361">No.</span></span> <span data-ttu-id="3e9dc-2362">Platí jenom v případě, že kopírování dat z místního serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2362">Apply only when copying data from an on-premises HTTP server.</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="3e9dc-2363">Příklad: Použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2363">Example: Using Basic, Digest, or Windows authentication</span></span>
<span data-ttu-id="3e9dc-2364">Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě konektor HTTP obecné ty, které jsou zavedené výše:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2364">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="3e9dc-2365">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2365">Property</span></span> | <span data-ttu-id="3e9dc-2366">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2366">Description</span></span> | <span data-ttu-id="3e9dc-2367">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2367">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2368">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2368">username</span></span> | <span data-ttu-id="3e9dc-2369">Uživatelské jméno pro přístup k koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2369">Username to access the HTTP endpoint.</span></span> | <span data-ttu-id="3e9dc-2370">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2370">Yes</span></span> |
| <span data-ttu-id="3e9dc-2371">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2371">password</span></span> | <span data-ttu-id="3e9dc-2372">Heslo pro uživatele (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2372">Password for the user (username).</span></span> | <span data-ttu-id="3e9dc-2373">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2373">Yes</span></span> |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a><span data-ttu-id="3e9dc-2374">Příklad: Použití ověřování ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2374">Example: Using ClientCertificate authentication</span></span>

<span data-ttu-id="3e9dc-2375">Chcete-li základní ověřování použijte, nastavte `authenticationType` jako `ClientCertificate`a zadejte následující vlastnosti kromě konektor HTTP obecné ty, které jsou zavedené výše:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2375">To use basic authentication, set `authenticationType` as `ClientCertificate`, and specify the following properties besides the HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="3e9dc-2376">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2376">Property</span></span> | <span data-ttu-id="3e9dc-2377">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2377">Description</span></span> | <span data-ttu-id="3e9dc-2378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2378">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2379">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2379">embeddedCertData</span></span> | <span data-ttu-id="3e9dc-2380">Obsah s kódováním base64, pomocí binárních dat soubor Personal Information Exchange (PFX).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2380">The Base64-encoded contents of binary data of the Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="3e9dc-2381">Zadejte buď `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2381">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="3e9dc-2382">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2382">certThumbprint</span></span> | <span data-ttu-id="3e9dc-2383">Kryptografický otisk certifikátu, který byl nainstalován v úložišti certifikátů počítače brány.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2383">The thumbprint of the certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="3e9dc-2384">Platí jenom v případě, že kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2384">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="3e9dc-2385">Zadejte buď `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2385">Specify either the `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="3e9dc-2386">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2386">password</span></span> | <span data-ttu-id="3e9dc-2387">Heslo přidružené k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2387">Password associated with the certificate.</span></span> | <span data-ttu-id="3e9dc-2388">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2388">No</span></span> |

<span data-ttu-id="3e9dc-2389">Pokud používáte `certThumbprint` pro ověřování a certifikát nainstalovaný v osobním úložišti místního počítače, musí udělit oprávnění ke čtení ke službě brány:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2389">If you use `certThumbprint` for authentication and the certificate is installed in the personal store of the local computer, you need to grant the read permission to the gateway service:</span></span>

1. <span data-ttu-id="3e9dc-2390">Spusťte konzolu Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2390">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="3e9dc-2391">Přidat **certifikáty** modul snap-in zacílený **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2391">Add the **Certificates** snap-in that targets the **Local Computer**.</span></span>
2. <span data-ttu-id="3e9dc-2392">Rozbalte položku **certifikáty**, **osobní**a klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2392">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="3e9dc-2393">Klikněte pravým tlačítkem na certifikát z osobního úložiště a vyberte **všechny úlohy**->**spravovat privátní klíče...**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2393">Right-click the certificate from the personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="3e9dc-2394">Na **zabezpečení** přidejte uživatelský účet, pod kterou je spuštěna hostitelská služba brány správy dat s přístupem pro čtení k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2394">On the **Security** tab, add the user account under which Data Management Gateway Host Service is running with the read access to the certificate.</span></span>  

<span data-ttu-id="3e9dc-2395">**Příklad: pomocí klientského certifikátu:** Tato propojená služba propojuje datovou továrnu místní webový server HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2395">**Example: using client certificate:** This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="3e9dc-2396">Používá klientský certifikát, který je nainstalován na počítači s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2396">It uses a client certificate that is installed on the machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="3e9dc-2397">Příklad: pomocí klientského certifikátu do souboru</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2397">Example: using client certificate in a file</span></span>
<span data-ttu-id="3e9dc-2398">Tato propojená služba propojuje datovou továrnu místní webový server HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2398">This linked service links your data factory to an on-premises HTTP web server.</span></span> <span data-ttu-id="3e9dc-2399">Používá soubor certifikátu klienta na počítači s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2399">It uses a client certificate file on the machine with Data Management Gateway installed.</span></span>

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

<span data-ttu-id="3e9dc-2400">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2400">For more information, see [HTTP connector](data-factory-http-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-2401">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2401">Dataset</span></span>
<span data-ttu-id="3e9dc-2402">Chcete-li definovat datovou sadu protokolu HTTP, nastavte **typ** datové sady, která **Http**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2402">To define a HTTP dataset, set the **type** of the dataset to **Http**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2403">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2403">Property</span></span> | <span data-ttu-id="3e9dc-2404">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2404">Description</span></span> | <span data-ttu-id="3e9dc-2405">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2405">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-2406">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2406">relativeUrl</span></span> | <span data-ttu-id="3e9dc-2407">Relativní adresa URL k prostředku, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2407">A relative URL to the resource that contains the data.</span></span> <span data-ttu-id="3e9dc-2408">Pokud cesta není zadána, je použít jenom adresu URL, zadaný v definici propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2408">When path is not specified, only the URL specified in the linked service definition is used.</span></span> <br><br> <span data-ttu-id="3e9dc-2409">Chcete-li vytvořit dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2409">To construct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), Example: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span></span> | <span data-ttu-id="3e9dc-2410">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2410">No</span></span> |
| <span data-ttu-id="3e9dc-2411">requestMethod</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2411">requestMethod</span></span> | <span data-ttu-id="3e9dc-2412">Metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2412">Http method.</span></span> <span data-ttu-id="3e9dc-2413">Povolené hodnoty jsou **získat** nebo **POST**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2413">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="3e9dc-2414">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2414">No.</span></span> <span data-ttu-id="3e9dc-2415">Výchozí hodnota je `GET`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2415">Default is `GET`.</span></span> |
| <span data-ttu-id="3e9dc-2416">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2416">additionalHeaders</span></span> | <span data-ttu-id="3e9dc-2417">Další hlavičky žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2417">Additional HTTP request headers.</span></span> | <span data-ttu-id="3e9dc-2418">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2418">No</span></span> |
| <span data-ttu-id="3e9dc-2419">RequestBody</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2419">requestBody</span></span> | <span data-ttu-id="3e9dc-2420">Text pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2420">Body for HTTP request.</span></span> | <span data-ttu-id="3e9dc-2421">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2421">No</span></span> |
| <span data-ttu-id="3e9dc-2422">Formát</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2422">format</span></span> | <span data-ttu-id="3e9dc-2423">Pokud chcete jednoduše **načtou data z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2423">If you want to simply **retrieve the data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="3e9dc-2424">Pokud chcete analyzovat během kopírování obsahu odpovědi HTTP, jsou podporovány následující typy formátu: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2424">If you want to parse the HTTP response content during copy, the following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="3e9dc-2425">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2425">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="3e9dc-2426">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2426">No</span></span> |
| <span data-ttu-id="3e9dc-2427">Komprese</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2427">compression</span></span> | <span data-ttu-id="3e9dc-2428">Zadejte typ a úroveň komprese pro data.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2428">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="3e9dc-2429">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2429">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="3e9dc-2430">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2430">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="3e9dc-2431">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2431">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="3e9dc-2432">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2432">No</span></span> |

#### <a name="example-using-the-get-default-method"></a><span data-ttu-id="3e9dc-2433">Příklad: použití metody GET (výchozí)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2433">Example: using the GET (default) method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-the-post-method"></a><span data-ttu-id="3e9dc-2434">Příklad: pomocí metody POST</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2434">Example: using the POST method</span></span>

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
<span data-ttu-id="3e9dc-2435">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2435">For more information, see [HTTP connector](data-factory-http-connector.md#dataset-properties) article.</span></span>

### <a name="http-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2436">Zdroj protokolu HTTP v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2436">HTTP Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2437">Pokud kopírujete data ze zdroje HTTP, nastavte **typ zdroje** kopie aktivity na **HttpSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2437">If you are copying data from a HTTP source, set the **source type** of the copy activity to **HttpSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2438">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2438">Property</span></span> | <span data-ttu-id="3e9dc-2439">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2439">Description</span></span> | <span data-ttu-id="3e9dc-2440">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2440">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e9dc-2441">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2441">httpRequestTimeout</span></span> | <span data-ttu-id="3e9dc-2442">Časový limit (TimeSpan) pro získání odezvy požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2442">The timeout (TimeSpan) for the HTTP request to get a response.</span></span> <span data-ttu-id="3e9dc-2443">Získání odezvy, není časový limit číst data odpovědi je časový limit.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2443">It is the timeout to get a response, not the timeout to read response data.</span></span> | <span data-ttu-id="3e9dc-2444">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2444">No.</span></span> <span data-ttu-id="3e9dc-2445">Výchozí hodnota: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2445">Default value: 00:01:40</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-2446">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2446">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-2447">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2447">For more information, see [HTTP connector](data-factory-http-connector.md#copy-activity-properties) article.</span></span>

## <a name="odata"></a><span data-ttu-id="3e9dc-2448">OData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2448">OData</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2449">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2449">Linked service</span></span>
<span data-ttu-id="3e9dc-2450">K definování OData propojené služby, nastavte **typ** propojené služby pro **OData**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2450">To define an OData linked service, set the **type** of the linked service to **OData**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2451">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2451">Property</span></span> | <span data-ttu-id="3e9dc-2452">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2452">Description</span></span> | <span data-ttu-id="3e9dc-2453">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2453">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2454">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2454">url</span></span> |<span data-ttu-id="3e9dc-2455">Adresa URL služby OData.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2455">Url of the OData service.</span></span> |<span data-ttu-id="3e9dc-2456">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2456">Yes</span></span> |
| <span data-ttu-id="3e9dc-2457">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2457">authenticationType</span></span> |<span data-ttu-id="3e9dc-2458">Typ ověřování používaný pro připojení ke zdroji OData.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2458">Type of authentication used to connect to the OData source.</span></span> <br/><br/> <span data-ttu-id="3e9dc-2459">Možné hodnoty pro cloudové prostředí OData, jsou anonymní, základní a OAuth (Upozorňujeme, že Azure Active Directory na základě OAuth aktuálně jedinou podpory Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2459">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="3e9dc-2460">Pro místní OData možné hodnoty jsou anonymní, Basic a Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2460">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="3e9dc-2461">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2461">Yes</span></span> |
| <span data-ttu-id="3e9dc-2462">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2462">username</span></span> |<span data-ttu-id="3e9dc-2463">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2463">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="3e9dc-2464">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2464">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="3e9dc-2465">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2465">password</span></span> |<span data-ttu-id="3e9dc-2466">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2466">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-2467">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2467">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="3e9dc-2468">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2468">authorizedCredential</span></span> |<span data-ttu-id="3e9dc-2469">Pokud používáte OAuth, klikněte na tlačítko **Autorizovat** tlačítko Průvodce kopírováním služby Data Factory nebo editoru a zadejte svoje přihlašovací údaje, pak bude automaticky generovaný hodnota této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2469">If you are using OAuth, click **Authorize** button in the Data Factory Copy Wizard or Editor and enter your credential, then the value of this property will be auto-generated.</span></span> |<span data-ttu-id="3e9dc-2470">Ano (jenom Pokud používáte ověřování OAuth)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2470">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="3e9dc-2471">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2471">gatewayName</span></span> |<span data-ttu-id="3e9dc-2472">Název brány, kterou služba Data Factory měla použít pro připojení ke službě OData na místě.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2472">Name of the gateway that the Data Factory service should use to connect to the on-premises OData service.</span></span> <span data-ttu-id="3e9dc-2473">Zadejte, pokud jsou kopírování dat z místní zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2473">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="3e9dc-2474">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2474">No</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="3e9dc-2475">Příklad: použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2475">Example - Using Basic authentication</span></span>
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a><span data-ttu-id="3e9dc-2476">Příklad: pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2476">Example - Using Anonymous authentication</span></span>

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="3e9dc-2477">Příklad: přístup k ověřování pomocí Windows místní zdroj OData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2477">Example - Using Windows authentication accessing on-premises OData source</span></span>

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="3e9dc-2478">Příklad - použití ověřování OAuth přístup ke cloudu zdroj OData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2478">Example - Using OAuth authentication accessing cloud OData source</span></span>
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

<span data-ttu-id="3e9dc-2479">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2479">For more information, see [OData connector](data-factory-odata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="3e9dc-2480">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2480">Dataset</span></span>
<span data-ttu-id="3e9dc-2481">Chcete-li definovat datové sadě služby OData, nastavte **typ** datové sady, která **ODataResource**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2481">To define an OData dataset, set the **type** of the dataset to **ODataResource**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2482">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2482">Property</span></span> | <span data-ttu-id="3e9dc-2483">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2483">Description</span></span> | <span data-ttu-id="3e9dc-2484">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2484">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2485">Cesta</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2485">path</span></span> |<span data-ttu-id="3e9dc-2486">Cesta k prostředku OData</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2486">Path to the OData resource</span></span> |<span data-ttu-id="3e9dc-2487">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2487">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2488">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2488">Example</span></span>

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

<span data-ttu-id="3e9dc-2489">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2489">For more information, see [OData connector](data-factory-odata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2490">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2490">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2491">Pokud kopírujete z OData zdroje dat, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2491">If you are copying data from an OData source, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2492">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2492">Property</span></span> | <span data-ttu-id="3e9dc-2493">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2493">Description</span></span> | <span data-ttu-id="3e9dc-2494">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2494">Example</span></span> | <span data-ttu-id="3e9dc-2495">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2495">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2496">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2496">query</span></span> |<span data-ttu-id="3e9dc-2497">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2497">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-2498">"? $select = název, popis a $top = 5"</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2498">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="3e9dc-2499">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2499">No</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2500">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2500">Example</span></span>

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

<span data-ttu-id="3e9dc-2501">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2501">For more information, see [OData connector](data-factory-odata-connector.md#copy-activity-properties) article.</span></span>


## <a name="odbc"></a><span data-ttu-id="3e9dc-2502">ODBC</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2502">ODBC</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-2503">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2503">Linked service</span></span>
<span data-ttu-id="3e9dc-2504">K definování ODBC propojené služby, nastavte **typ** propojené služby pro **OnPremisesOdbc**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2504">To define an ODBC linked service, set the **type** of the linked service to **OnPremisesOdbc**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2505">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2505">Property</span></span> | <span data-ttu-id="3e9dc-2506">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2506">Description</span></span> | <span data-ttu-id="3e9dc-2507">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2507">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2508">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2508">connectionString</span></span> |<span data-ttu-id="3e9dc-2509">Přístup k pověření část připojovací řetězec a volitelné šifrovat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2509">The non-access credential portion of the connection string and an optional encrypted credential.</span></span> <span data-ttu-id="3e9dc-2510">Příklady v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2510">See examples in the following sections.</span></span> |<span data-ttu-id="3e9dc-2511">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2511">Yes</span></span> |
| <span data-ttu-id="3e9dc-2512">přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2512">credential</span></span> |<span data-ttu-id="3e9dc-2513">Část přístup přihlašovacích údajů z připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2513">The access credential portion of the connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="3e9dc-2514">Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2514">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="3e9dc-2515">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2515">No</span></span> |
| <span data-ttu-id="3e9dc-2516">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2516">authenticationType</span></span> |<span data-ttu-id="3e9dc-2517">Typ ověřování používaný pro připojení k úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2517">Type of authentication used to connect to the ODBC data store.</span></span> <span data-ttu-id="3e9dc-2518">Možné hodnoty jsou: anonymní a Basic.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2518">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="3e9dc-2519">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2519">Yes</span></span> |
| <span data-ttu-id="3e9dc-2520">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2520">username</span></span> |<span data-ttu-id="3e9dc-2521">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2521">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="3e9dc-2522">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2522">No</span></span> |
| <span data-ttu-id="3e9dc-2523">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2523">password</span></span> |<span data-ttu-id="3e9dc-2524">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2524">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-2525">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2525">No</span></span> |
| <span data-ttu-id="3e9dc-2526">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2526">gatewayName</span></span> |<span data-ttu-id="3e9dc-2527">Název brány, kterou služba Data Factory měla použít pro připojení k úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2527">Name of the gateway that the Data Factory service should use to connect to the ODBC data store.</span></span> |<span data-ttu-id="3e9dc-2528">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2528">Yes</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="3e9dc-2529">Příklad: použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2529">Example - Using Basic authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="3e9dc-2530">Příklad: základní ověřování pomocí zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2530">Example - Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="3e9dc-2531">Můžete šifrovat přihlašovací údaje pomocí [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi prostředí Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2531">You can encrypt the credentials using the [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of the Azure PowerShell).</span></span>  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="3e9dc-2532">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2532">Example: Using Anonymous authentication</span></span>

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="3e9dc-2533">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2533">For more information, see [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-2534">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2534">Dataset</span></span>
<span data-ttu-id="3e9dc-2535">Chcete-li definovat datové sadě služby ODBC, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2535">To define an ODBC dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2536">Property</span></span> | <span data-ttu-id="3e9dc-2537">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2537">Description</span></span> | <span data-ttu-id="3e9dc-2538">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2538">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2539">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2539">tableName</span></span> |<span data-ttu-id="3e9dc-2540">Název tabulky v úložišti dat ODBC.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2540">Name of the table in the ODBC data store.</span></span> |<span data-ttu-id="3e9dc-2541">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2541">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="3e9dc-2542">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2542">Example</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-2543">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2543">For more information, see [ODBC connector](data-factory-odbc-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2544">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2544">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2545">Pokud jsou kopírování dat z úložiště dat rozhraní ODBC, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2545">If you are copying data from an ODBC data store, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2546">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2546">Property</span></span> | <span data-ttu-id="3e9dc-2547">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2547">Description</span></span> | <span data-ttu-id="3e9dc-2548">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2548">Allowed values</span></span> | <span data-ttu-id="3e9dc-2549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2549">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2550">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2550">query</span></span> |<span data-ttu-id="3e9dc-2551">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2551">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-2552">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2552">SQL query string.</span></span> <span data-ttu-id="3e9dc-2553">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2553">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="3e9dc-2554">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2554">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2555">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2555">Example</span></span>

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

<span data-ttu-id="3e9dc-2556">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2556">For more information, see [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) article.</span></span>

## <a name="salesforce"></a><span data-ttu-id="3e9dc-2557">Salesforce</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2557">Salesforce</span></span>


### <a name="linked-service"></a><span data-ttu-id="3e9dc-2558">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2558">Linked service</span></span>
<span data-ttu-id="3e9dc-2559">K definování Salesforce propojené služby, nastavte **typ** propojené služby pro **Salesforce**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2559">To define a Salesforce linked service, set the **type** of the linked service to **Salesforce**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2560">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2560">Property</span></span> | <span data-ttu-id="3e9dc-2561">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2561">Description</span></span> | <span data-ttu-id="3e9dc-2562">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2562">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2563">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2563">environmentUrl</span></span> | <span data-ttu-id="3e9dc-2564">Zadejte adresu URL služby Salesforce instanci.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2564">Specify the URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="3e9dc-2565">-Výchozí hodnota je "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2565">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="3e9dc-2566">-Ke zkopírování dat z izolovaného prostoru, zadejte "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2566">- To copy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="3e9dc-2567">-Ke zkopírování dat z vlastní domény, zadejte, například "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2567">- To copy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="3e9dc-2568">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2568">No</span></span> |
| <span data-ttu-id="3e9dc-2569">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2569">username</span></span> |<span data-ttu-id="3e9dc-2570">Zadejte uživatelské jméno pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2570">Specify a user name for the user account.</span></span> |<span data-ttu-id="3e9dc-2571">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2571">Yes</span></span> |
| <span data-ttu-id="3e9dc-2572">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2572">password</span></span> |<span data-ttu-id="3e9dc-2573">Zadejte heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2573">Specify a password for the user account.</span></span> |<span data-ttu-id="3e9dc-2574">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2574">Yes</span></span> |
| <span data-ttu-id="3e9dc-2575">securityToken</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2575">securityToken</span></span> |<span data-ttu-id="3e9dc-2576">Zadejte token zabezpečení pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2576">Specify a security token for the user account.</span></span> <span data-ttu-id="3e9dc-2577">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny o tom, jak resetování nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2577">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how to reset/get a security token.</span></span> <span data-ttu-id="3e9dc-2578">Obecné informace o tokeny zabezpečení najdete v tématu [zabezpečení a rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2578">To learn about security tokens in general, see [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="3e9dc-2579">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2579">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2580">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2580">Example</span></span>

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

<span data-ttu-id="3e9dc-2581">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2581">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-2582">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2582">Dataset</span></span>
<span data-ttu-id="3e9dc-2583">Chcete-li definovat datová sada služby Salesforce, nastavte **typ** datové sady, která **RelationalTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2583">To define a Salesforce dataset, set the **type** of the dataset to **RelationalTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2584">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2584">Property</span></span> | <span data-ttu-id="3e9dc-2585">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2585">Description</span></span> | <span data-ttu-id="3e9dc-2586">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2586">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2587">tableName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2587">tableName</span></span> |<span data-ttu-id="3e9dc-2588">Název tabulky v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2588">Name of the table in Salesforce.</span></span> |<span data-ttu-id="3e9dc-2589">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2589">No (if a **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2590">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2590">Example</span></span>

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

<span data-ttu-id="3e9dc-2591">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2591">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2592">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2592">Relational Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2593">Pokud kopírujete data ze služby Salesforce, nastavte **typ zdroje** kopie aktivity na **RelationalSource**a zadejte následující vlastnosti v **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2593">If you are copying data from Salesforce, set the **source type** of the copy activity to **RelationalSource**, and specify following properties in the **source** section:</span></span>

| <span data-ttu-id="3e9dc-2594">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2594">Property</span></span> | <span data-ttu-id="3e9dc-2595">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2595">Description</span></span> | <span data-ttu-id="3e9dc-2596">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2596">Allowed values</span></span> | <span data-ttu-id="3e9dc-2597">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2597">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="3e9dc-2598">query</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2598">query</span></span> |<span data-ttu-id="3e9dc-2599">Čtení dat pomocí vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2599">Use the custom query to read data.</span></span> |<span data-ttu-id="3e9dc-2600">Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2600">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="3e9dc-2601">Například `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2601">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="3e9dc-2602">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2602">No (if the **tableName** of the **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2603">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2603">Example</span></span>  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="3e9dc-2604">Část "__c" název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2604">The "__c" part of the API Name is needed for any custom object.</span></span>

<span data-ttu-id="3e9dc-2605">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2605">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#copy-activity-properties) article.</span></span> 

## <a name="web-data"></a><span data-ttu-id="3e9dc-2606">Data webové</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2606">Web Data</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2607">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2607">Linked service</span></span>
<span data-ttu-id="3e9dc-2608">K definování webové propojené služby, nastavte **typ** propojené služby pro **webové**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2608">To define a Web linked service, set the **type** of the linked service to **Web**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2609">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2609">Property</span></span> | <span data-ttu-id="3e9dc-2610">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2610">Description</span></span> | <span data-ttu-id="3e9dc-2611">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2611">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2612">URL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2612">Url</span></span> |<span data-ttu-id="3e9dc-2613">Adresa URL pro webové zdroje</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2613">URL to the Web source</span></span> |<span data-ttu-id="3e9dc-2614">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2614">Yes</span></span> |
| <span data-ttu-id="3e9dc-2615">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2615">authenticationType</span></span> |<span data-ttu-id="3e9dc-2616">Anonymní.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2616">Anonymous.</span></span> |<span data-ttu-id="3e9dc-2617">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2617">Yes</span></span> |
 

#### <a name="example"></a><span data-ttu-id="3e9dc-2618">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2618">Example</span></span>


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

<span data-ttu-id="3e9dc-2619">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2619">For more information, see [Web Table connector](data-factory-web-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="3e9dc-2620">Datová sada</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2620">Dataset</span></span>
<span data-ttu-id="3e9dc-2621">Chcete-li definovat webové datovou sadu, nastavte **typ** datové sady, která **WebTable**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2621">To define a Web dataset, set the **type** of the dataset to **WebTable**, and specify the following properties in the **typeProperties** section:</span></span> 

| <span data-ttu-id="3e9dc-2622">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2622">Property</span></span> | <span data-ttu-id="3e9dc-2623">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2623">Description</span></span> | <span data-ttu-id="3e9dc-2624">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2624">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-2625">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2625">type</span></span> |<span data-ttu-id="3e9dc-2626">Typ datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2626">type of the dataset.</span></span> <span data-ttu-id="3e9dc-2627">musí být nastavena na **WebTable**</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2627">must be set to **WebTable**</span></span> |<span data-ttu-id="3e9dc-2628">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2628">Yes</span></span> |
| <span data-ttu-id="3e9dc-2629">Cesta</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2629">path</span></span> |<span data-ttu-id="3e9dc-2630">Relativní adresa URL prostředek, který obsahuje tabulku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2630">A relative URL to the resource that contains the table.</span></span> |<span data-ttu-id="3e9dc-2631">Ne.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2631">No.</span></span> <span data-ttu-id="3e9dc-2632">Pokud cesta není zadána, je použít jenom adresu URL, zadaný v definici propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2632">When path is not specified, only the URL specified in the linked service definition is used.</span></span> |
| <span data-ttu-id="3e9dc-2633">Index</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2633">index</span></span> |<span data-ttu-id="3e9dc-2634">Index tabulky v prostředku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2634">The index of the table in the resource.</span></span> <span data-ttu-id="3e9dc-2635">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části Postup získání index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2635">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps to getting index of a table in an HTML page.</span></span> |<span data-ttu-id="3e9dc-2636">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2636">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="3e9dc-2637">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2637">Example</span></span>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="3e9dc-2638">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2638">For more information, see [Web Table connector](data-factory-web-table-connector.md#dataset-properties) article.</span></span> 

### <a name="web-source-in-copy-activity"></a><span data-ttu-id="3e9dc-2639">Webové zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2639">Web Source in Copy Activity</span></span>
<span data-ttu-id="3e9dc-2640">Pokud jsou kopírování dat z tabulky webové, nastavte **typ zdroje** kopie aktivity na **WebSource**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2640">If you are copying data from a web table, set the **source type** of the copy activity to **WebSource**.</span></span> <span data-ttu-id="3e9dc-2641">V současné době po zdroji v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2641">Currently, when the source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>

#### <a name="example"></a><span data-ttu-id="3e9dc-2642">Příklad</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2642">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

<span data-ttu-id="3e9dc-2643">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2643">For more information, see [Web Table connector](data-factory-web-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="compute-environments"></a><span data-ttu-id="3e9dc-2644">VÝPOČETNÍ PROSTŘEDÍ</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2644">COMPUTE ENVIRONMENTS</span></span>
<span data-ttu-id="3e9dc-2645">Následující tabulka uvádí výpočetních prostředích nepodporuje objekt pro vytváření dat a aktivit transformace, které můžou běžet na ně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2645">The following table lists the compute environments supported by Data Factory and the transformation activities that can run on them.</span></span> <span data-ttu-id="3e9dc-2646">Kliknutím na odkaz pro výpočty, které vás zajímají zobrazíte schémata JSON pro propojenou službu propojení s služby data factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2646">Click the link for the compute you are interested in to see the JSON schemas for linked service to link it to a data factory.</span></span> 

| <span data-ttu-id="3e9dc-2647">Výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2647">Compute environment</span></span> | <span data-ttu-id="3e9dc-2648">Aktivity</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2648">Activities</span></span> |
| --- | --- |
| <span data-ttu-id="3e9dc-2649">[Cluster HDInsight na vyžádání](#on-demand-azure-hdinsight-cluster) nebo [vlastní cluster HDInsight](#existing-azure-hdinsight-cluster)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2649">[On-demand HDInsight cluster](#on-demand-azure-hdinsight-cluster) or [your own HDInsight cluster](#existing-azure-hdinsight-cluster)</span></span> |<span data-ttu-id="3e9dc-2650">[Vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2650">[.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity)</span></span> |
| [<span data-ttu-id="3e9dc-2651">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2651">Azure Batch</span></span>](#azure-batch) |[<span data-ttu-id="3e9dc-2652">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2652">.NET custom activity</span></span>](#net-custom-activity) |
| [<span data-ttu-id="3e9dc-2653">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2653">Azure Machine Learning</span></span>](#azure-machine-learning) | <span data-ttu-id="3e9dc-2654">[Strojového učení aktivita provedení dávky](#machine-learning-batch-execution-activity), [strojového učení aktivita prostředku aktualizace](#machine-learning-update-resource-activity)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2654">[Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity)</span></span> |
| [<span data-ttu-id="3e9dc-2655">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2655">Azure Data Lake Analytics</span></span>](#azure-data-lake-analytics) |[<span data-ttu-id="3e9dc-2656">U-SQL Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2656">Data Lake Analytics U-SQL</span></span>](#data-lake-analytics-u-sql-activity) |
| <span data-ttu-id="3e9dc-2657">[Databáze Azure SQL](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [systému SQL Server](#sql-server-1)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span></span> |[<span data-ttu-id="3e9dc-2658">Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2658">Stored Procedure</span></span>](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a><span data-ttu-id="3e9dc-2659">Cluster Azure HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2659">On-demand Azure HDInsight cluster</span></span>
<span data-ttu-id="3e9dc-2660">Služba Azure Data Factory můžete automaticky vytvoření clusteru HDInsight se systémem Windows nebo Linux na vyžádání pro zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2660">The Azure Data Factory service can automatically create a Windows/Linux-based on-demand HDInsight cluster to process data.</span></span> <span data-ttu-id="3e9dc-2661">Cluster se vytvoří ve stejné oblasti jako účet úložiště (vlastnost linkedServiceName v kódu JSON) přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2661">The cluster is created in the same region as the storage account (linkedServiceName property in the JSON) associated with the cluster.</span></span> <span data-ttu-id="3e9dc-2662">Můžete spustit následující aktivit transformace na tato propojená služba: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2662">You can run the following transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2663">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2663">Linked service</span></span> 
<span data-ttu-id="3e9dc-2664">Následující tabulka obsahuje popis vlastností použitých v definici Azure JSON HDInsight propojené služby na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2664">The following table provides descriptions for the properties used in the Azure JSON definition of an on-demand HDInsight linked service.</span></span>

| <span data-ttu-id="3e9dc-2665">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2665">Property</span></span> | <span data-ttu-id="3e9dc-2666">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2666">Description</span></span> | <span data-ttu-id="3e9dc-2667">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2667">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2668">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2668">type</span></span> |<span data-ttu-id="3e9dc-2669">Vlastnost typu musí být nastavená na **HDInsightOnDemand**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2669">The type property should be set to **HDInsightOnDemand**.</span></span> |<span data-ttu-id="3e9dc-2670">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2670">Yes</span></span> |
| <span data-ttu-id="3e9dc-2671">Parametr ClusterSize</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2671">clusterSize</span></span> |<span data-ttu-id="3e9dc-2672">Počet uzlů pracovního procesu nebo data v clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2672">Number of worker/data nodes in the cluster.</span></span> <span data-ttu-id="3e9dc-2673">Vytvoření clusteru HDInsight s 2 hlavních uzlech spolu s počtem uzlů pracovního procesu, který jste zadali pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2673">The HDInsight cluster is created with 2 head nodes along with the number of worker nodes you specify for this property.</span></span> <span data-ttu-id="3e9dc-2674">Uzly jsou velikosti Standard_D3, který má 4 jádra, 4 pracovní uzly clusteru trvá 24 jader (4\*4 = 16 jader pro uzly pracovního procesu, plus 2\*4 = 8 jader pro head uzly).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2674">The nodes are of size Standard_D3 that has 4 cores, so a 4 worker node cluster takes 24 cores (4\*4 = 16 cores for worker nodes, plus 2\*4 = 8 cores for head nodes).</span></span> <span data-ttu-id="3e9dc-2675">V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) podrobnosti o Standard_D3 vrstvy.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2675">See [Create Linux-based Hadoop clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) for details about the Standard_D3 tier.</span></span> |<span data-ttu-id="3e9dc-2676">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2676">Yes</span></span> |
| <span data-ttu-id="3e9dc-2677">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2677">timetolive</span></span> |<span data-ttu-id="3e9dc-2678">Povolené doby nečinnosti pro cluster HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2678">The allowed idle time for the on-demand HDInsight cluster.</span></span> <span data-ttu-id="3e9dc-2679">Určuje, jak dlouho clusteru HDInsight na vyžádání zůstane aktivní po dokončení činnosti spustit, pokud nejsou žádné aktivní úlohy v clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2679">Specifies how long the on-demand HDInsight cluster stays alive after completion of an activity run if there are no other active jobs in the cluster.</span></span><br/><br/><span data-ttu-id="3e9dc-2680">Například pokud spuštění aktivity trvá 6 minut a timetolive nastavena na 5 minut, clusteru zůstává aktivní po dobu 5 minut po spuštění 6 minut zpracování aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2680">For example, if an activity run takes 6 minutes and timetolive is set to 5 minutes, the cluster stays alive for 5 minutes after the 6 minutes of processing the activity run.</span></span> <span data-ttu-id="3e9dc-2681">Pokud se okno 6 minut proveden jiné aktivity při spuštění, je zpracován stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2681">If another activity run is executed with the 6 minutes window, it is processed by the same cluster.</span></span><br/><br/><span data-ttu-id="3e9dc-2682">Vytvoření clusteru HDInsight na vyžádání je náročná operace (může trvat), takže použití tohoto nastavení podle potřeby ke zlepšení výkonu služby data factory pomocí opakovaného použití clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2682">Creating an on-demand HDInsight cluster is an expensive operation (could take a while), so use this setting as needed to improve performance of a data factory by reusing an on-demand HDInsight cluster.</span></span><br/><br/><span data-ttu-id="3e9dc-2683">Pokud hodnota timetolive nastavíte na 0, odstranění clusteru hned, jak aktivita běžet v zpracovaná.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2683">If you set timetolive value to 0, the cluster is deleted as soon as the activity run in processed.</span></span> <span data-ttu-id="3e9dc-2684">Na druhé straně, pokud jste nastavili na vysokou hodnotu, může přerušit clusteru nečinnosti zbytečně výsledkem vysoké náklady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2684">On the other hand, if you set a high value, the cluster may stay idle unnecessarily resulting in high costs.</span></span> <span data-ttu-id="3e9dc-2685">Proto je důležité nastavit odpovídající hodnotu na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2685">Therefore, it is important that you set the appropriate value based on your needs.</span></span><br/><br/><span data-ttu-id="3e9dc-2686">Více kanálů můžete sdílet stejnou instanci clusteru HDInsight na vyžádání, pokud je hodnota vlastnosti timetolive správně nastavena</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2686">Multiple pipelines can share the same instance of the on-demand HDInsight cluster if the timetolive property value is appropriately set</span></span> |<span data-ttu-id="3e9dc-2687">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2687">Yes</span></span> |
| <span data-ttu-id="3e9dc-2688">Verze</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2688">version</span></span> |<span data-ttu-id="3e9dc-2689">Verze clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2689">Version of the HDInsight cluster.</span></span> <span data-ttu-id="3e9dc-2690">Podrobnosti najdete v tématu [podporované verze HDInsight v Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2690">For details, see [supported HDInsight versions in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> |<span data-ttu-id="3e9dc-2691">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2691">No</span></span> |
| <span data-ttu-id="3e9dc-2692">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2692">linkedServiceName</span></span> |<span data-ttu-id="3e9dc-2693">Propojená služba má být používána clusteru na vyžádání pro ukládání a zpracování dat Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2693">Azure Storage linked service to be used by the on-demand cluster for storing and processing data.</span></span> <p><span data-ttu-id="3e9dc-2694">V současné době nelze vytvořit cluster HDInsight na vyžádání, který používá jako úložiště Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2694">Currently, you cannot create an on-demand HDInsight cluster that uses an Azure Data Lake Store as the storage.</span></span> <span data-ttu-id="3e9dc-2695">Pokud chcete uložit výsledek data z HDInsight zpracování v Azure Data Lake Store, pomocí aktivity kopírování zkopírovat data z Azure Blob Storage do Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2695">If you want to store the result data from HDInsight processing in an Azure Data Lake Store, use a Copy Activity to copy the data from the Azure Blob Storage to the Azure Data Lake Store.</span></span></p>  | <span data-ttu-id="3e9dc-2696">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2696">Yes</span></span> |
| <span data-ttu-id="3e9dc-2697">additionalLinkedServiceNames</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2697">additionalLinkedServiceNames</span></span> |<span data-ttu-id="3e9dc-2698">Určuje, že další účty úložiště pro HDInsight propojená služba tak, aby služba Data Factory můžete zaregistrovat vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2698">Specifies additional storage accounts for the HDInsight linked service so that the Data Factory service can register them on your behalf.</span></span> |<span data-ttu-id="3e9dc-2699">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2699">No</span></span> |
| <span data-ttu-id="3e9dc-2700">osType</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2700">osType</span></span> |<span data-ttu-id="3e9dc-2701">Typ operačního systému.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2701">Type of operating system.</span></span> <span data-ttu-id="3e9dc-2702">Povolené hodnoty jsou: systému Windows (výchozí) a Linux</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2702">Allowed values are: Windows (default) and Linux</span></span> |<span data-ttu-id="3e9dc-2703">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2703">No</span></span> |
| <span data-ttu-id="3e9dc-2704">hcatalogLinkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2704">hcatalogLinkedServiceName</span></span> |<span data-ttu-id="3e9dc-2705">Název Azure SQL propojené služby, které odkazují na databázi HCatalog.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2705">The name of Azure SQL linked service that point to the HCatalog database.</span></span> <span data-ttu-id="3e9dc-2706">Cluster HDInsight na vyžádání je vytvořená pomocí Azure SQL database jako metaúložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2706">The on-demand HDInsight cluster is created by using the Azure SQL database as the metastore.</span></span> |<span data-ttu-id="3e9dc-2707">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2707">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3e9dc-2708">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2708">JSON example</span></span>
<span data-ttu-id="3e9dc-2709">Následující kód JSON určuje základě Linux na vyžádání propojené služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2709">The following JSON defines a Linux-based on-demand HDInsight linked service.</span></span> <span data-ttu-id="3e9dc-2710">Služba Data Factory automaticky vytvoří **systémem Linux** clusteru HDInsight se při zpracování dat řezu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2710">The Data Factory service automatically creates a **Linux-based** HDInsight cluster when processing a data slice.</span></span> 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

<span data-ttu-id="3e9dc-2711">Další informace najdete v tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2711">For more information, see [Compute linked services](data-factory-compute-linked-services.md) article.</span></span> 

## <a name="existing-azure-hdinsight-cluster"></a><span data-ttu-id="3e9dc-2712">Existující cluster Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2712">Existing Azure HDInsight cluster</span></span>
<span data-ttu-id="3e9dc-2713">Můžete vytvořit propojené služby Azure HDInsight k registraci vlastní cluster HDInsight s Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2713">You can create an Azure HDInsight linked service to register your own HDInsight cluster with Data Factory.</span></span> <span data-ttu-id="3e9dc-2714">Můžete spustit následující aktivit transformace dat na tato propojená služba: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2714">You can run the following data transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2715">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2715">Linked service</span></span>
<span data-ttu-id="3e9dc-2716">Následující tabulka obsahuje popis vlastností použitých v definici Azure JSON propojené služby Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2716">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure HDInsight linked service.</span></span>

| <span data-ttu-id="3e9dc-2717">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2717">Property</span></span> | <span data-ttu-id="3e9dc-2718">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2718">Description</span></span> | <span data-ttu-id="3e9dc-2719">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2719">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2720">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2720">type</span></span> |<span data-ttu-id="3e9dc-2721">Vlastnost typu musí být nastavená na **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2721">The type property should be set to **HDInsight**.</span></span> |<span data-ttu-id="3e9dc-2722">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2722">Yes</span></span> |
| <span data-ttu-id="3e9dc-2723">clusterUri</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2723">clusterUri</span></span> |<span data-ttu-id="3e9dc-2724">Identifikátor URI clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2724">The URI of the HDInsight cluster.</span></span> |<span data-ttu-id="3e9dc-2725">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2725">Yes</span></span> |
| <span data-ttu-id="3e9dc-2726">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2726">username</span></span> |<span data-ttu-id="3e9dc-2727">Zadejte jméno uživatele, který se má použít pro připojení k existujícímu clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2727">Specify the name of the user to be used to connect to an existing HDInsight cluster.</span></span> |<span data-ttu-id="3e9dc-2728">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2728">Yes</span></span> |
| <span data-ttu-id="3e9dc-2729">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2729">password</span></span> |<span data-ttu-id="3e9dc-2730">Zadejte heslo pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2730">Specify password for the user account.</span></span> |<span data-ttu-id="3e9dc-2731">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2731">Yes</span></span> |
| <span data-ttu-id="3e9dc-2732">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2732">linkedServiceName</span></span> | <span data-ttu-id="3e9dc-2733">Název úložiště Azure, propojené služby, která odkazuje na Azure blob storage používaný v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2733">Name of the Azure Storage linked service that refers to the Azure blob storage used by the HDInsight cluster.</span></span> <p><span data-ttu-id="3e9dc-2734">V současné době nelze zadat, že Azure Data Lake Store propojené služby pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2734">Currently, you cannot specify an Azure Data Lake Store linked service for this property.</span></span> <span data-ttu-id="3e9dc-2735">Může přistupovat k datům v Azure Data Lake Store z skriptů Hive nebo Pig Pokud HDInsight cluster má přístup do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2735">You may access data in the Azure Data Lake Store from Hive/Pig scripts if the HDInsight cluster has access to the Data Lake Store.</span></span> </p>  |<span data-ttu-id="3e9dc-2736">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2736">Yes</span></span> |

<span data-ttu-id="3e9dc-2737">Verzích clusterů HDInsight, které jsou podporovány, naleznete v části [podporované HDInsight verze](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2737">For versions of HDInsight clusters supported, see [supported HDInsight versions](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> 

#### <a name="json-example"></a><span data-ttu-id="3e9dc-2738">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2738">JSON example</span></span>

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a><span data-ttu-id="3e9dc-2739">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2739">Azure Batch</span></span>
<span data-ttu-id="3e9dc-2740">Služby Azure Batch propojené můžete zaregistrovat fondu služby Batch virtuálních počítačů (VM) můžete vytvořit pomocí služby data factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2740">You can create an Azure Batch linked service to register a Batch pool of virtual machines (VMs) with a data factory.</span></span> <span data-ttu-id="3e9dc-2741">Můžete spustit pomocí Azure Batch nebo Azure HDInsight vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2741">You can run .NET custom activities using either Azure Batch or Azure HDInsight.</span></span> <span data-ttu-id="3e9dc-2742">Můžete spustit [vlastní aktivity .NET](#net-custom-activity) na toto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2742">You can run a [.NET custom activity](#net-custom-activity) on this linked service.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2743">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2743">Linked service</span></span>
<span data-ttu-id="3e9dc-2744">Následující tabulka obsahuje popis vlastností použitých v definici Azure JSON služby Azure Batch propojený.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2744">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure Batch linked service.</span></span>

| <span data-ttu-id="3e9dc-2745">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2745">Property</span></span> | <span data-ttu-id="3e9dc-2746">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2746">Description</span></span> | <span data-ttu-id="3e9dc-2747">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2747">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2748">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2748">type</span></span> |<span data-ttu-id="3e9dc-2749">Vlastnost typu musí být nastavená na **AzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2749">The type property should be set to **AzureBatch**.</span></span> |<span data-ttu-id="3e9dc-2750">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2750">Yes</span></span> |
| <span data-ttu-id="3e9dc-2751">název účtu</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2751">accountName</span></span> |<span data-ttu-id="3e9dc-2752">Název účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2752">Name of the Azure Batch account.</span></span> |<span data-ttu-id="3e9dc-2753">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2753">Yes</span></span> |
| <span data-ttu-id="3e9dc-2754">accessKey</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2754">accessKey</span></span> |<span data-ttu-id="3e9dc-2755">Přístupový klíč pro účet Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2755">Access key for the Azure Batch account.</span></span> |<span data-ttu-id="3e9dc-2756">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2756">Yes</span></span> |
| <span data-ttu-id="3e9dc-2757">poolName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2757">poolName</span></span> |<span data-ttu-id="3e9dc-2758">Název fondu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2758">Name of the pool of virtual machines.</span></span> |<span data-ttu-id="3e9dc-2759">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2759">Yes</span></span> |
| <span data-ttu-id="3e9dc-2760">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2760">linkedServiceName</span></span> |<span data-ttu-id="3e9dc-2761">Název úložiště Azure propojená služba přidruženého k této službě Azure Batch propojený.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2761">Name of the Azure Storage linked service associated with this Azure Batch linked service.</span></span> <span data-ttu-id="3e9dc-2762">Tato propojená služba se používá pro pracovní soubory potřebné ke spuštění aktivity a ukládání protokoly spuštění aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2762">This linked service is used for staging files required to run the activity and storing the activity execution logs.</span></span> |<span data-ttu-id="3e9dc-2763">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2763">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="3e9dc-2764">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2764">JSON example</span></span>

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a><span data-ttu-id="3e9dc-2765">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2765">Azure Machine Learning</span></span>
<span data-ttu-id="3e9dc-2766">Vytváření služby Azure Machine Learning propojené k registraci Machine Learning dávkového vyhodnocování koncový bod s služby data factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2766">You create an Azure Machine Learning linked service to register a Machine Learning batch scoring endpoint with a data factory.</span></span> <span data-ttu-id="3e9dc-2767">Aktivity transformace dat dvě, které můžou běžet v této propojené službě: [aktivita provedení dávky Machine Learning](#machine-learning-batch-execution-activity), [aktivita prostředku aktualizace Machine Learning](#machine-learning-update-resource-activity).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2767">Two data transformation activities that can run on this linked service: [Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2768">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2768">Linked service</span></span>
<span data-ttu-id="3e9dc-2769">Následující tabulka obsahuje popis vlastností použitých v definici Azure JSON služby Azure Machine Learning propojený.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2769">The following table provides descriptions for the properties used in the Azure JSON definition of an Azure Machine Learning linked service.</span></span>

| <span data-ttu-id="3e9dc-2770">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2770">Property</span></span> | <span data-ttu-id="3e9dc-2771">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2771">Description</span></span> | <span data-ttu-id="3e9dc-2772">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2772">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2773">Typ</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2773">Type</span></span> |<span data-ttu-id="3e9dc-2774">Vlastnost typu musí být nastavená na: **AzureML**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2774">The type property should be set to: **AzureML**.</span></span> |<span data-ttu-id="3e9dc-2775">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2775">Yes</span></span> |
| <span data-ttu-id="3e9dc-2776">mlEndpoint</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2776">mlEndpoint</span></span> |<span data-ttu-id="3e9dc-2777">Adresu URL dávkového vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2777">The batch scoring URL.</span></span> |<span data-ttu-id="3e9dc-2778">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2778">Yes</span></span> |
| <span data-ttu-id="3e9dc-2779">apiKey</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2779">apiKey</span></span> |<span data-ttu-id="3e9dc-2780">Rozhraní API modelu publikované prostoru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2780">The published workspace model’s API.</span></span> |<span data-ttu-id="3e9dc-2781">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2781">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3e9dc-2782">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2782">JSON example</span></span>

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a><span data-ttu-id="3e9dc-2783">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2783">Azure Data Lake Analytics</span></span>
<span data-ttu-id="3e9dc-2784">Vytvoříte **Azure Data Lake Analytics** propojená služba Azure Data Lake Analytics výpočetní služby s objektem pro vytváření dat Azure před použitím [aktivita Data Lake Analytics U-SQL](data-factory-usql-activity.md) v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2784">You create an **Azure Data Lake Analytics** linked service to link an Azure Data Lake Analytics compute service to an Azure data factory before using the [Data Lake Analytics U-SQL activity](data-factory-usql-activity.md) in a pipeline.</span></span>

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2785">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2785">Linked service</span></span>

<span data-ttu-id="3e9dc-2786">Následující tabulka obsahuje popis vlastností použitých v definici JSON služby Azure Data Lake Analytics propojený.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2786">The following table provides descriptions for the properties used in the JSON definition of an Azure Data Lake Analytics linked service.</span></span> 

| <span data-ttu-id="3e9dc-2787">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2787">Property</span></span> | <span data-ttu-id="3e9dc-2788">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2788">Description</span></span> | <span data-ttu-id="3e9dc-2789">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2789">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2790">Typ</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2790">Type</span></span> |<span data-ttu-id="3e9dc-2791">Vlastnost typu musí být nastavená na: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2791">The type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="3e9dc-2792">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2792">Yes</span></span> |
| <span data-ttu-id="3e9dc-2793">název účtu</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2793">accountName</span></span> |<span data-ttu-id="3e9dc-2794">Název účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2794">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="3e9dc-2795">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2795">Yes</span></span> |
| <span data-ttu-id="3e9dc-2796">dataLakeAnalyticsUri</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2796">dataLakeAnalyticsUri</span></span> |<span data-ttu-id="3e9dc-2797">Identifikátor URI služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2797">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="3e9dc-2798">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2798">No</span></span> |
| <span data-ttu-id="3e9dc-2799">Autorizace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2799">authorization</span></span> |<span data-ttu-id="3e9dc-2800">Autorizační kód se načte automaticky po kliknutí na **Autorizovat** tlačítko v editoru služby Data Factory a dokončí se přihlášení OAuth.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2800">Authorization code is automatically retrieved after clicking **Authorize** button in the Data Factory Editor and completing the OAuth login.</span></span> |<span data-ttu-id="3e9dc-2801">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2801">Yes</span></span> |
| <span data-ttu-id="3e9dc-2802">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2802">subscriptionId</span></span> |<span data-ttu-id="3e9dc-2803">Id předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2803">Azure subscription id</span></span> |<span data-ttu-id="3e9dc-2804">Ne (když není určeno, předplatné objektu pro vytváření dat se používá).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2804">No (If not specified, subscription of the data factory is used).</span></span> |
| <span data-ttu-id="3e9dc-2805">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2805">resourceGroupName</span></span> |<span data-ttu-id="3e9dc-2806">Název skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2806">Azure resource group name</span></span> |<span data-ttu-id="3e9dc-2807">Ne (když není určeno, skupinu prostředků objektu pro vytváření dat se používá).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2807">No (If not specified, resource group of the data factory is used).</span></span> |
| <span data-ttu-id="3e9dc-2808">ID relace</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2808">sessionId</span></span> |<span data-ttu-id="3e9dc-2809">id relace z autorizační relace OAuth.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2809">session id from the OAuth authorization session.</span></span> <span data-ttu-id="3e9dc-2810">Každé id relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2810">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="3e9dc-2811">Při použití editoru služby Data Factory toto ID se generuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2811">When you use the Data Factory Editor, this ID is auto-generated.</span></span> |<span data-ttu-id="3e9dc-2812">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2812">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="3e9dc-2813">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2813">JSON example</span></span>
<span data-ttu-id="3e9dc-2814">Následující příklad uvádí definici JSON pro služby Azure Data Lake Analytics propojený.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2814">The following example provides JSON definition for an Azure Data Lake Analytics linked service.</span></span>

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a><span data-ttu-id="3e9dc-2815">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2815">Azure SQL Database</span></span>
<span data-ttu-id="3e9dc-2816">Vytvoření služby Azure SQL propojené a použít je s [aktivity uložené procedury](#stored-procedure-activity) k vyvolání uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2816">You create an Azure SQL linked service and use it with the [Stored Procedure Activity](#stored-procedure-activity) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2817">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2817">Linked service</span></span>
<span data-ttu-id="3e9dc-2818">K definování Azure SQL Database propojené služby, nastavte **typ** propojené služby pro **azuresqldatabase**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2818">To define an Azure SQL Database linked service, set the **type** of the linked service to **AzureSqlDatabase**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2819">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2819">Property</span></span> | <span data-ttu-id="3e9dc-2820">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2820">Description</span></span> | <span data-ttu-id="3e9dc-2821">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2821">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2822">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2822">connectionString</span></span> |<span data-ttu-id="3e9dc-2823">Zadejte informace potřebné pro připojení k instanci databáze SQL Azure pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2823">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-2824">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2824">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3e9dc-2825">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2825">JSON example</span></span>

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3e9dc-2826">V tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2826">See [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) article for details about this linked service.</span></span>

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="3e9dc-2827">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2827">Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3e9dc-2828">Vytvoření služby Azure SQL Data Warehouse propojené a použít je s [aktivity uložené procedury](data-factory-stored-proc-activity.md) k vyvolání uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2828">You create an Azure SQL Data Warehouse linked service and use it with the [Stored Procedure Activity](data-factory-stored-proc-activity.md) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2829">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2829">Linked service</span></span>
<span data-ttu-id="3e9dc-2830">K definování Azure SQL Data Warehouse propojené služby, nastavte **typ** propojené služby pro **AzureSqlDW**a zadejte následující vlastnosti v **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2830">To define an Azure SQL Data Warehouse linked service, set the **type** of the linked service to **AzureSqlDW**, and specify following properties in the **typeProperties** section:</span></span>  

| <span data-ttu-id="3e9dc-2831">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2831">Property</span></span> | <span data-ttu-id="3e9dc-2832">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2832">Description</span></span> | <span data-ttu-id="3e9dc-2833">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2833">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2834">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2834">connectionString</span></span> |<span data-ttu-id="3e9dc-2835">Zadejte informace potřebné pro připojení k Azure SQL Data Warehouse instance pro vlastnost connectionString.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2835">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> |<span data-ttu-id="3e9dc-2836">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2836">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="3e9dc-2837">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2837">JSON example</span></span>

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

<span data-ttu-id="3e9dc-2838">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2838">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

## <a name="sql-server"></a><span data-ttu-id="3e9dc-2839">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2839">SQL Server</span></span> 
<span data-ttu-id="3e9dc-2840">Vytvoření služby SQL serveru propojená a použít je s [aktivity uložené procedury](data-factory-stored-proc-activity.md) k vyvolání uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2840">You create a SQL Server linked service and use it with the [Stored Procedure Activity](data-factory-stored-proc-activity.md) to invoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="3e9dc-2841">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2841">Linked service</span></span>
<span data-ttu-id="3e9dc-2842">Vytvoření propojené služby typu **onpremisessqlserver** propojit místní databázi systému SQL Server do služby data factory.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2842">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="3e9dc-2843">Následující tabulka obsahuje popis specifické pro službu SQL serveru propojená místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2843">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="3e9dc-2844">Následující tabulka obsahuje popis JSON elementy, které jsou specifické pro SQL Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2844">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="3e9dc-2845">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2845">Property</span></span> | <span data-ttu-id="3e9dc-2846">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2846">Description</span></span> | <span data-ttu-id="3e9dc-2847">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2847">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2848">type</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2848">type</span></span> |<span data-ttu-id="3e9dc-2849">Vlastnost typu musí být nastavená na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2849">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="3e9dc-2850">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2850">Yes</span></span> |
| <span data-ttu-id="3e9dc-2851">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2851">connectionString</span></span> |<span data-ttu-id="3e9dc-2852">Zadejte připojovací řetězec informace potřebné pro připojení k místní databázi systému SQL Server pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2852">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="3e9dc-2853">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2853">Yes</span></span> |
| <span data-ttu-id="3e9dc-2854">gatewayName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2854">gatewayName</span></span> |<span data-ttu-id="3e9dc-2855">Název brány, kterou služba Data Factory měla použít pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2855">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="3e9dc-2856">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2856">Yes</span></span> |
| <span data-ttu-id="3e9dc-2857">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2857">username</span></span> |<span data-ttu-id="3e9dc-2858">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2858">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="3e9dc-2859">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2859">Example: **domainname\\username**.</span></span> |<span data-ttu-id="3e9dc-2860">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2860">No</span></span> |
| <span data-ttu-id="3e9dc-2861">heslo</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2861">password</span></span> |<span data-ttu-id="3e9dc-2862">Zadejte heslo pro uživatelský účet, který jste zadali pro uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2862">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="3e9dc-2863">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2863">No</span></span> |

<span data-ttu-id="3e9dc-2864">Můžete šifrovat přihlašovací údaje pomocí **New-AzureRmDataFactoryEncryptValue** rutiny a jak je znázorněno v následujícím příkladu je využít v připojovacím řetězci (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2864">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="3e9dc-2865">Příklad: JSON pro pomocí ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2865">Example: JSON for using SQL Authentication</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="3e9dc-2866">Příklad: JSON pro použití ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2866">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="3e9dc-2867">Pokud jsou zadané uživatelské jméno a heslo, brána je používá k zosobnění zadaný uživatelský účet pro připojení k místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2867">If username and password are specified, gateway uses them to impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> <span data-ttu-id="3e9dc-2868">Jinak brána připojí k systému SQL Server přímo s kontextem zabezpečení brány (jeho účet spuštění).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2868">Otherwise, gateway connects to the SQL Server directly with the security context of Gateway (its startup account).</span></span>

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="3e9dc-2869">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2869">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span>

## <a name="data-transformation-activities"></a><span data-ttu-id="3e9dc-2870">AKTIVITY TRANSFORMACE DAT</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2870">DATA TRANSFORMATION ACTIVITIES</span></span>

<span data-ttu-id="3e9dc-2871">Aktivita</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2871">Activity</span></span> | <span data-ttu-id="3e9dc-2872">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2872">Description</span></span>
-------- | -----------
[<span data-ttu-id="3e9dc-2873">Aktivitu HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2873">HDInsight Hive activity</span></span>](#hdinsight-hive-activity) | <span data-ttu-id="3e9dc-2874">Aktivity HDInsight Hive v kanálu pro vytváření dat provede dotazů Hive sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2874">The HDInsight Hive activity in a Data Factory pipeline executes Hive queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span> 
[<span data-ttu-id="3e9dc-2875">Aktivita HDInsight Pig</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2875">HDInsight Pig activity</span></span>](#hdinsight-pig-activity) | <span data-ttu-id="3e9dc-2876">HDInsight Pig aktivity v kanálu pro vytváření dat provede Pig dotazy na vlastní nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2876">The HDInsight Pig activity in a Data Factory pipeline executes Pig queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3e9dc-2877">Aktivita MapReduce služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2877">HDInsight MapReduce Activity</span></span>](#hdinsight-mapreduce-activity) | <span data-ttu-id="3e9dc-2878">Činnost HDInsight MapReduce v objektu pro vytváření dat kanál provede MapReduce programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2878">The HDInsight MapReduce activity in a Data Factory pipeline executes MapReduce programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3e9dc-2879">Aktivita Streamování služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2879">HDInsight Streaming Activity</span></span>](#hdinsight-streaming-activity) | <span data-ttu-id="3e9dc-2880">HDInsight streamované aktivitě v objektu pro vytváření dat kanál provede streamování Hadoop programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2880">The HDInsight Streaming Activity in a Data Factory pipeline executes Hadoop Streaming programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="3e9dc-2881">Aktivita Sparku služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2881">HDInsight Spark Activity</span></span>](#hdinsight-spark-activity) | <span data-ttu-id="3e9dc-2882">Aktivity HDInsight Spark v objektu pro vytváření dat kanál provede na clusteru HDInsight Spark programy.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2882">The HDInsight Spark activity in a Data Factory pipeline executes Spark programs on your own HDInsight cluster.</span></span> 
[<span data-ttu-id="3e9dc-2883">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2883">Machine Learning Batch Execution Activity</span></span>](#machine-learning-batch-execution-activity) | <span data-ttu-id="3e9dc-2884">Azure Data Factory můžete snadno vytvořit kanály, které používají publikované webové služby Azure Machine Learning pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2884">Azure Data Factory enables you to easily create pipelines that use a published Azure Machine Learning web service for predictive analytics.</span></span> <span data-ttu-id="3e9dc-2885">Aktivita provedení dávky v kanál služby Azure Data Factory, můžete vyvolat webové služby Machine Learning k provádět předpovědi na datech v dávce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2885">Using the Batch Execution Activity in an Azure Data Factory pipeline, you can invoke a Machine Learning web service to make predictions on the data in batch.</span></span> 
[<span data-ttu-id="3e9dc-2886">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2886">Machine Learning Update Resource Activity</span></span>](#machine-learning-update-resource-activity) | <span data-ttu-id="3e9dc-2887">V průběhu času prediktivní modely v Machine Learning vyhodnocování experimentů muset být retrained pomocí nové vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2887">Over time, the predictive models in the Machine Learning scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="3e9dc-2888">Jakmile jste hotovi s přeučení chcete aktualizovat webovou službu vyhodnocování s modelem retrained Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2888">After you are done with retraining, you want to update the scoring web service with the retrained Machine Learning model.</span></span> <span data-ttu-id="3e9dc-2889">Aktivita prostředku aktualizace můžete aktualizovat webovou službu s nově naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2889">You can use the Update Resource Activity to update the web service with the newly trained model.</span></span>
[<span data-ttu-id="3e9dc-2890">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2890">Stored Procedure Activity</span></span>](#stored-procedure-activity) | <span data-ttu-id="3e9dc-2891">Aktivity uložené procedury v kanálu pro vytváření dat můžete použít k vyvolání uložené procedury v jednom z následujících úložišť dat: databáze SQL Azure, Azure SQL Data Warehouse, databáze SQL serveru ve vašem podniku nebo virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2891">You can use the Stored Procedure activity in a Data Factory pipeline to invoke a stored procedure in one of the following data stores: Azure SQL Database, Azure SQL Data Warehouse, SQL Server Database in your enterprise or an Azure VM.</span></span> 
[<span data-ttu-id="3e9dc-2892">Aktivita data Lake Analytics U-SQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2892">Data Lake Analytics U-SQL activity</span></span>](#data-lake-analytics-u-sql-activity) | <span data-ttu-id="3e9dc-2893">Data Lake Analytics U-SQL aktivity spouští skript U-SQL na clusteru služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2893">Data Lake Analytics U-SQL Activity runs a U-SQL script on an Azure Data Lake Analytics cluster.</span></span>  
[<span data-ttu-id="3e9dc-2894">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2894">.NET custom activity</span></span>](#net-custom-activity) | <span data-ttu-id="3e9dc-2895">Pokud potřebujete transformovat data způsobem, který není podporován službou Data Factory, můžete vytvořit vlastní aktivity s logika zpracování dat a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2895">If you need to transform data in a way that is not supported by Data Factory, you can create a custom activity with your own data processing logic and use the activity in the pipeline.</span></span> <span data-ttu-id="3e9dc-2896">Můžete nakonfigurovat vlastní .NET aktivity ke spuštění pomocí služby Azure Batch nebo clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2896">You can configure the custom .NET activity to run using either an Azure Batch service or an Azure HDInsight cluster.</span></span> 

     
## <a name="hdinsight-hive-activity"></a><span data-ttu-id="3e9dc-2897">Aktivita Hivu služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2897">HDInsight Hive Activity</span></span>
<span data-ttu-id="3e9dc-2898">V definici JSON aktivity Hive můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2898">You can specify the following properties in a Hive Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-2899">Musí být vlastnost typu aktivity: **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2899">The type property for the activity must be: **HDInsightHive**.</span></span> <span data-ttu-id="3e9dc-2900">Musíte nejprve vytvořit propojené služby HDInsight a zadejte jako hodnotu pro název je **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2900">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-2901">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na HDInsightHive:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2901">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightHive:</span></span>

| <span data-ttu-id="3e9dc-2902">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2902">Property</span></span> | <span data-ttu-id="3e9dc-2903">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2903">Description</span></span> | <span data-ttu-id="3e9dc-2904">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2904">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2905">Skript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2905">script</span></span> |<span data-ttu-id="3e9dc-2906">Zadejte vloženého skriptu Hive</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2906">Specify the Hive script inline</span></span> |<span data-ttu-id="3e9dc-2907">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2907">No</span></span> |
| <span data-ttu-id="3e9dc-2908">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2908">script path</span></span> |<span data-ttu-id="3e9dc-2909">Uložení skriptu Hive v Azure blob storage a zadejte cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2909">Store the Hive script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="3e9dc-2910">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2910">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="3e9dc-2911">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2911">Both cannot be used together.</span></span> <span data-ttu-id="3e9dc-2912">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2912">The file name is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-2913">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2913">No</span></span> |
| <span data-ttu-id="3e9dc-2914">definuje</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2914">defines</span></span> |<span data-ttu-id="3e9dc-2915">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive pomocí 'hiveconf.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2915">Specify parameters as key/value pairs for referencing within the Hive script using 'hiveconf'</span></span> |<span data-ttu-id="3e9dc-2916">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2916">No</span></span> |

<span data-ttu-id="3e9dc-2917">Tyto vlastnosti typu jsou specifická pro aktivitu Hive.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2917">These type properties are specific to the Hive Activity.</span></span> <span data-ttu-id="3e9dc-2918">Ostatní vlastnosti (mimo v rámci typeProperties části) jsou podporovány pro všechny aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2918">Other properties (outside the typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="3e9dc-2919">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2919">JSON example</span></span>
<span data-ttu-id="3e9dc-2920">Následující kód JSON určuje aktivitu HDInsight Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2920">The following JSON defines a HDInsight Hive activity in a pipeline.</span></span>  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

<span data-ttu-id="3e9dc-2921">Další informace najdete v tématu [Hive aktivity](data-factory-hive-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2921">For more information, see [Hive Activity](data-factory-hive-activity.md) article.</span></span> 

## <a name="hdinsight-pig-activity"></a><span data-ttu-id="3e9dc-2922">Aktivita Pig služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2922">HDInsight Pig Activity</span></span>
<span data-ttu-id="3e9dc-2923">V definici JSON aktivity Pig můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2923">You can specify the following properties in a Pig Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-2924">Musí být vlastnost typu aktivity: **HDInsightPig**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2924">The type property for the activity must be: **HDInsightPig**.</span></span> <span data-ttu-id="3e9dc-2925">Musíte nejprve vytvořit propojené služby HDInsight a zadejte jako hodnotu pro název je **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2925">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-2926">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na HDInsightPig:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2926">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightPig:</span></span> 

| <span data-ttu-id="3e9dc-2927">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2927">Property</span></span> | <span data-ttu-id="3e9dc-2928">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2928">Description</span></span> | <span data-ttu-id="3e9dc-2929">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2929">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2930">Skript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2930">script</span></span> |<span data-ttu-id="3e9dc-2931">Zadejte vložený skript Pig</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2931">Specify the Pig script inline</span></span> |<span data-ttu-id="3e9dc-2932">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2932">No</span></span> |
| <span data-ttu-id="3e9dc-2933">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2933">script path</span></span> |<span data-ttu-id="3e9dc-2934">Uložte skript Pig v Azure blob storage a zadejte cestu k souboru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2934">Store the Pig script in an Azure blob storage and provide the path to the file.</span></span> <span data-ttu-id="3e9dc-2935">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2935">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="3e9dc-2936">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2936">Both cannot be used together.</span></span> <span data-ttu-id="3e9dc-2937">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2937">The file name is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-2938">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2938">No</span></span> |
| <span data-ttu-id="3e9dc-2939">definuje</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2939">defines</span></span> |<span data-ttu-id="3e9dc-2940">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skript Pig</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2940">Specify parameters as key/value pairs for referencing within the Pig script</span></span> |<span data-ttu-id="3e9dc-2941">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2941">No</span></span> |

<span data-ttu-id="3e9dc-2942">Tyto vlastnosti typu jsou specifická pro aktivitu Pig.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2942">These type properties are specific to the Pig Activity.</span></span> <span data-ttu-id="3e9dc-2943">Ostatní vlastnosti (mimo v rámci typeProperties části) jsou podporovány pro všechny aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2943">Other properties (outside the typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="3e9dc-2944">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2944">JSON example</span></span>

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

<span data-ttu-id="3e9dc-2945">Další informace najdete v tématu [Pig aktivity](#data-factory-pig-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2945">For more information, see [Pig Activity](#data-factory-pig-activity.md) article.</span></span> 

## <a name="hdinsight-mapreduce-activity"></a><span data-ttu-id="3e9dc-2946">Aktivita MapReduce služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2946">HDInsight MapReduce Activity</span></span>
<span data-ttu-id="3e9dc-2947">V definici JSON aktivity MapReduce, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2947">You can specify the following properties in a MapReduce Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-2948">Musí být vlastnost typu aktivity: **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2948">The type property for the activity must be: **HDInsightMapReduce**.</span></span> <span data-ttu-id="3e9dc-2949">Musíte nejprve vytvořit propojené služby HDInsight a zadejte jako hodnotu pro název je **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2949">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-2950">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na HDInsightMapReduce:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2950">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightMapReduce:</span></span> 

| <span data-ttu-id="3e9dc-2951">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2951">Property</span></span> | <span data-ttu-id="3e9dc-2952">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2952">Description</span></span> | <span data-ttu-id="3e9dc-2953">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2953">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-2954">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2954">jarLinkedService</span></span> | <span data-ttu-id="3e9dc-2955">Název propojené služby pro Azure Storage, který obsahuje soubor JAR.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2955">Name of the linked service for the Azure Storage that contains the JAR file.</span></span> | <span data-ttu-id="3e9dc-2956">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2956">Yes</span></span> |
| <span data-ttu-id="3e9dc-2957">jarFilePath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2957">jarFilePath</span></span> | <span data-ttu-id="3e9dc-2958">Cesta k souboru JAR ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2958">Path to the JAR file in the Azure Storage.</span></span> | <span data-ttu-id="3e9dc-2959">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2959">Yes</span></span> | 
| <span data-ttu-id="3e9dc-2960">Název třídy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2960">className</span></span> | <span data-ttu-id="3e9dc-2961">Název hlavní třídy v souboru JAR.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2961">Name of the main class in the JAR file.</span></span> | <span data-ttu-id="3e9dc-2962">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2962">Yes</span></span> | 
| <span data-ttu-id="3e9dc-2963">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2963">arguments</span></span> | <span data-ttu-id="3e9dc-2964">Seznam argumentů programu MapReduce, oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2964">A list of comma-separated arguments for the MapReduce program.</span></span> <span data-ttu-id="3e9dc-2965">V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2965">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from the MapReduce framework.</span></span> <span data-ttu-id="3e9dc-2966">Chcete-li rozlišit vaší argumenty s argumenty MapReduce, zvažte, pomocí možnosti a hodnoty jako argumenty, jak je znázorněno v následujícím příkladu (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2966">To differentiate your arguments with the MapReduce arguments, consider using both option and value as arguments as shown in the following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | <span data-ttu-id="3e9dc-2967">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2967">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3e9dc-2968">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2968">JSON example</span></span>

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix to determine the similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

<span data-ttu-id="3e9dc-2969">Další informace najdete v tématu [činnost MapReduce](data-factory-map-reduce.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2969">For more information, see [MapReduce Activity](data-factory-map-reduce.md) article.</span></span> 

## <a name="hdinsight-streaming-activity"></a><span data-ttu-id="3e9dc-2970">Aktivita Streamování služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2970">HDInsight Streaming Activity</span></span>
<span data-ttu-id="3e9dc-2971">V definici JSON aktivity streamování Hadoop, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2971">You can specify the following properties in a Hadoop Streaming Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-2972">Musí být vlastnost typu aktivity: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2972">The type property for the activity must be: **HDInsightStreaming**.</span></span> <span data-ttu-id="3e9dc-2973">Musíte nejprve vytvořit propojené služby HDInsight a zadejte jako hodnotu pro název je **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2973">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-2974">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na HDInsightStreaming:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2974">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightStreaming:</span></span> 

| <span data-ttu-id="3e9dc-2975">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2975">Property</span></span> | <span data-ttu-id="3e9dc-2976">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2976">Description</span></span> | 
| --- | --- |
| <span data-ttu-id="3e9dc-2977">Mapper</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2977">mapper</span></span> | <span data-ttu-id="3e9dc-2978">Název spustitelného souboru mapper.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2978">Name of the mapper executable.</span></span> <span data-ttu-id="3e9dc-2979">V příkladu je cat.exe mapper spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2979">In the example, cat.exe is the mapper executable.</span></span>| 
| <span data-ttu-id="3e9dc-2980">reduktorem</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2980">reducer</span></span> | <span data-ttu-id="3e9dc-2981">Název spustitelného souboru reduktorem.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2981">Name of the reducer executable.</span></span> <span data-ttu-id="3e9dc-2982">V příkladu je wc.exe reduktorem spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2982">In the example, wc.exe is the reducer executable.</span></span> | 
| <span data-ttu-id="3e9dc-2983">Vstup</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2983">input</span></span> | <span data-ttu-id="3e9dc-2984">Vstupní soubor (včetně umístění) pro mapper.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2984">Input file (including location) for the mapper.</span></span> <span data-ttu-id="3e9dc-2985">Příklad: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob, například/data/Gutenberg je složka, a davinci.txt je objekt blob.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2985">In the example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is the blob container, example/data/Gutenberg is the folder, and davinci.txt is the blob.</span></span> |
| <span data-ttu-id="3e9dc-2986">Výstup</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2986">output</span></span> | <span data-ttu-id="3e9dc-2987">Ve výstupním souboru (včetně umístění) reduktorem.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2987">Output file (including location) for the reducer.</span></span> <span data-ttu-id="3e9dc-2988">Výstup úlohy streamování Hadoop je zapsán do umístění zadané pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2988">The output of the Hadoop Streaming job is written to the location specified for this property.</span></span> |
| <span data-ttu-id="3e9dc-2989">filePaths</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2989">filePaths</span></span> | <span data-ttu-id="3e9dc-2990">Cesty pro spustitelné soubory mapper a reduktorem.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2990">Paths for the mapper and reducer executables.</span></span> <span data-ttu-id="3e9dc-2991">Příklad: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob, příklad nebo aplikací je složka a wc.exe je spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2991">In the example: "adfsample/example/apps/wc.exe", adfsample is the blob container, example/apps is the folder, and wc.exe is the executable.</span></span> | 
| <span data-ttu-id="3e9dc-2992">fileLinkedService</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2992">fileLinkedService</span></span> | <span data-ttu-id="3e9dc-2993">Propojená služba, která představuje úložiště Azure, který obsahuje soubory zadané v části filePaths Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2993">Azure Storage linked service that represents the Azure storage that contains the files specified in the filePaths section.</span></span> | 
| <span data-ttu-id="3e9dc-2994">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2994">arguments</span></span> | <span data-ttu-id="3e9dc-2995">Seznam argumentů programu MapReduce, oddělených čárkami.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2995">A list of comma-separated arguments for the MapReduce program.</span></span> <span data-ttu-id="3e9dc-2996">V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2996">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from the MapReduce framework.</span></span> <span data-ttu-id="3e9dc-2997">Chcete-li rozlišit vaší argumenty s argumenty MapReduce, zvažte, pomocí možnosti a hodnoty jako argumenty, jak je znázorněno v následujícím příkladu (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2997">To differentiate your arguments with the MapReduce arguments, consider using both option and value as arguments as shown in the following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | 
| <span data-ttu-id="3e9dc-2998">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2998">getDebugInfo</span></span> | <span data-ttu-id="3e9dc-2999">Element volitelné.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-2999">An optional element.</span></span> <span data-ttu-id="3e9dc-3000">Pokud je nastavena k chybě, protokoly se stáhnou pouze při selhání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3000">When it is set to Failure, the logs are downloaded only on failure.</span></span> <span data-ttu-id="3e9dc-3001">Pokud je nastavena pro všechny, protokoly budou staženy vždy bez ohledu na stav spuštění.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3001">When it is set to All, logs are always downloaded irrespective of the execution status.</span></span> | 

> [!NOTE]
> <span data-ttu-id="3e9dc-3002">Je nutno zadat pro streamované aktivitě Hadoop pro datovou sadu výstupů **výstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3002">You must specify an output dataset for the Hadoop Streaming Activity for the **outputs** property.</span></span> <span data-ttu-id="3e9dc-3003">Tato datová sada může být právě fiktivní datovou sadu, která je potřeba jednotka plán kanálu (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3003">This dataset can be just a dummy dataset that is required to drive the pipeline schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="3e9dc-3004">Pokud aktivita neberou vstup, můžete přeskočit vstupní datové sady pro aktivitu pro určení **vstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3004">If the activity doesn't take an input, you can skip specifying an input dataset for the activity for the **inputs** property.</span></span>  

## <a name="json-example"></a><span data-ttu-id="3e9dc-3005">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3005">JSON example</span></span>

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

<span data-ttu-id="3e9dc-3006">Další informace najdete v tématu [streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3006">For more information, see [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) article.</span></span> 

## <a name="hdinsight-spark-activity"></a><span data-ttu-id="3e9dc-3007">Aktivita Spark služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3007">HDInsight Spark Activity</span></span>
<span data-ttu-id="3e9dc-3008">V definici JSON aktivity Spark můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3008">You can specify the following properties in a Spark Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3009">Musí být vlastnost typu aktivity: **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3009">The type property for the activity must be: **HDInsightSpark**.</span></span> <span data-ttu-id="3e9dc-3010">Musíte nejprve vytvořit propojené služby HDInsight a zadejte jako hodnotu pro název je **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3010">You must create a HDInsight linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-3011">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na HDInsightSpark:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3011">The following properties are supported in the **typeProperties** section when you set the type of activity to HDInsightSpark:</span></span> 

| <span data-ttu-id="3e9dc-3012">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3012">Property</span></span> | <span data-ttu-id="3e9dc-3013">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3013">Description</span></span> | <span data-ttu-id="3e9dc-3014">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3014">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="3e9dc-3015">rootPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3015">rootPath</span></span> | <span data-ttu-id="3e9dc-3016">Kontejner objektů Blob v Azure a složky, která obsahuje soubor Spark.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3016">The Azure Blob container and folder that contains the Spark file.</span></span> <span data-ttu-id="3e9dc-3017">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3017">The file name is case-sensitive.</span></span> | <span data-ttu-id="3e9dc-3018">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3018">Yes</span></span> |
| <span data-ttu-id="3e9dc-3019">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3019">entryFilePath</span></span> | <span data-ttu-id="3e9dc-3020">Relativní cesta ke kořenové složce Spark kódu nebo balíčku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3020">Relative path to the root folder of the Spark code/package.</span></span> | <span data-ttu-id="3e9dc-3021">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3021">Yes</span></span> |
| <span data-ttu-id="3e9dc-3022">Název třídy</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3022">className</span></span> | <span data-ttu-id="3e9dc-3023">Hlavní třídy aplikace Java/Spark</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3023">Application's Java/Spark main class</span></span> | <span data-ttu-id="3e9dc-3024">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3024">No</span></span> | 
| <span data-ttu-id="3e9dc-3025">Argumenty</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3025">arguments</span></span> | <span data-ttu-id="3e9dc-3026">Seznam argumentů příkazového řádku pro Spark program.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3026">A list of command-line arguments to the Spark program.</span></span> | <span data-ttu-id="3e9dc-3027">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3027">No</span></span> | 
| <span data-ttu-id="3e9dc-3028">proxyUser</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3028">proxyUser</span></span> | <span data-ttu-id="3e9dc-3029">Uživatelský účet zosobnění spuštění programu Spark</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3029">The user account to impersonate to execute the Spark program</span></span> | <span data-ttu-id="3e9dc-3030">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3030">No</span></span> | 
| <span data-ttu-id="3e9dc-3031">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3031">sparkConfig</span></span> | <span data-ttu-id="3e9dc-3032">Vlastnosti konfigurace Spark.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3032">Spark configuration properties.</span></span> | <span data-ttu-id="3e9dc-3033">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3033">No</span></span> | 
| <span data-ttu-id="3e9dc-3034">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3034">getDebugInfo</span></span> | <span data-ttu-id="3e9dc-3035">Určuje, kdy soubory protokolu Spark se zkopírují do úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3035">Specifies when the Spark log files are copied to the Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="3e9dc-3036">Povolené hodnoty: None, vždy nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3036">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="3e9dc-3037">Výchozí hodnota: žádné.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3037">Default value: None.</span></span> | <span data-ttu-id="3e9dc-3038">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3038">No</span></span> | 
| <span data-ttu-id="3e9dc-3039">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3039">sparkJobLinkedService</span></span> | <span data-ttu-id="3e9dc-3040">Azure Storage propojená služba, která obsahuje Spark soubor úlohy, závislosti a protokoly.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3040">The Azure Storage linked service that holds the Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="3e9dc-3041">Pokud hodnotu pro tuto vlastnost nezadáte, použije se úložiště přidružený k clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3041">If you do not specify a value for this property, the storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="3e9dc-3042">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3042">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3e9dc-3043">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3043">JSON example</span></span>

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
<span data-ttu-id="3e9dc-3044">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3044">Note the following points:</span></span> 

- <span data-ttu-id="3e9dc-3045">**Typ** je nastavena na **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3045">The **type** property is set to **HDInsightSpark**.</span></span>
- <span data-ttu-id="3e9dc-3046">**RootPath** je nastaven na **adfspark\\pyFiles** kde adfspark je kontejner objektů Blob v Azure a pyFiles je dobře složku v daném kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3046">The **rootPath** is set to **adfspark\\pyFiles** where adfspark is the Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="3e9dc-3047">V tomto příkladu úložiště objektů Blob Azure je ten, který je přidružen Spark cluster.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3047">In this example, the Azure Blob Storage is the one that is associated with the Spark cluster.</span></span> <span data-ttu-id="3e9dc-3048">Můžete nahrát soubor do jiného úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3048">You can upload the file to a different Azure Storage.</span></span> <span data-ttu-id="3e9dc-3049">Pokud tak učiníte, vytvoření služby Azure Storage, propojené propojení objektu pro vytváření dat. Tento účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3049">If you do so, create an Azure Storage linked service to link that storage account to the data factory.</span></span> <span data-ttu-id="3e9dc-3050">Potom zadejte název propojené služby, jako hodnotu **sparkJobLinkedService** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3050">Then, specify the name of the linked service as a value for the **sparkJobLinkedService** property.</span></span> <span data-ttu-id="3e9dc-3051">V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností podporované aktivitou Spark.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3051">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by the Spark Activity.</span></span>
- <span data-ttu-id="3e9dc-3052">**EntryFilePath** je nastaven na **test.py**, což je soubor python.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3052">The **entryFilePath** is set to the **test.py**, which is the python file.</span></span> 
- <span data-ttu-id="3e9dc-3053">**Getdebuginfo –** je nastavena na **vždy**, tzn., soubory protokolů jsou vždy vygeneruje (úspěch nebo neúspěch).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3053">The **getDebugInfo** property is set to **Always**, which means the log files are always generated (success or failure).</span></span>  

    > [!IMPORTANT]
    > <span data-ttu-id="3e9dc-3054">Doporučujeme vám, že nenastavíte tuto vlastnost na vždy v produkčním prostředí Pokud řešíte problém.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3054">We recommend that you do not set this property to Always in a production environment unless you are troubleshooting an issue.</span></span> 
- <span data-ttu-id="3e9dc-3055">**Výstupy** obsahuje jednu výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3055">The **outputs** section has one output dataset.</span></span> <span data-ttu-id="3e9dc-3056">Je třeba zadat datovou sadu výstupů i v případě, že spark program nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3056">You must specify an output dataset even if the spark program does not produce any output.</span></span> <span data-ttu-id="3e9dc-3057">Výstupní datovou sadu jednotky plán pro kanál (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3057">The output dataset drives the schedule for the pipeline (hourly, daily, etc.).</span></span>

<span data-ttu-id="3e9dc-3058">Další informace o aktivitě najdete v tématu [Spark aktivity](data-factory-spark.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3058">For more information about the activity, see [Spark Activity](data-factory-spark.md) article.</span></span>  

## <a name="machine-learning-batch-execution-activity"></a><span data-ttu-id="3e9dc-3059">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3059">Machine Learning Batch Execution Activity</span></span>
<span data-ttu-id="3e9dc-3060">V definici Azure ML dávky spuštění aktivity JSON, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3060">You can specify the following properties in a Azure ML Batch Execution Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3061">Musí být vlastnost typu aktivity: **AzureMLBatchExecution**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3061">The type property for the activity must be: **AzureMLBatchExecution**.</span></span> <span data-ttu-id="3e9dc-3062">Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název ji jako hodnotu **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3062">You must create a Azure Machine Learning linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-3063">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na AzureMLBatchExecution:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3063">The following properties are supported in the **typeProperties** section when you set the type of activity to AzureMLBatchExecution:</span></span>

<span data-ttu-id="3e9dc-3064">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3064">Property</span></span> | <span data-ttu-id="3e9dc-3065">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3065">Description</span></span> | <span data-ttu-id="3e9dc-3066">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3066">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="3e9dc-3067">webServiceInput</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3067">webServiceInput</span></span> | <span data-ttu-id="3e9dc-3068">Datové sady mají být předány jako vstup pro webovou službu Azure ML.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3068">The dataset to be passed as an input for the Azure ML web service.</span></span> <span data-ttu-id="3e9dc-3069">Tato datová sada musí být součástí vstupy pro aktivitu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3069">This dataset must also be included in the inputs for the activity.</span></span> |<span data-ttu-id="3e9dc-3070">Použijte webServiceInput nebo webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3070">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="3e9dc-3071">webServiceInputs</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3071">webServiceInputs</span></span> | <span data-ttu-id="3e9dc-3072">Zadejte datové sady, které mají být předány jako vstupy pro webovou službu Azure ML.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3072">Specify datasets to be passed as inputs for the Azure ML web service.</span></span> <span data-ttu-id="3e9dc-3073">Pokud webová služba přijímá více vstupů, použijte vlastnost webServiceInputs místo pomocí vlastnosti webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3073">If the web service takes multiple inputs, use the webServiceInputs property instead of using the webServiceInput property.</span></span> <span data-ttu-id="3e9dc-3074">Datové sady, které odkazují **webServiceInputs** musí také obsahovat aktivity **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3074">Datasets that are referenced by the **webServiceInputs** must also be included in the Activity **inputs**.</span></span> | <span data-ttu-id="3e9dc-3075">Použijte webServiceInput nebo webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3075">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="3e9dc-3076">webServiceOutputs</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3076">webServiceOutputs</span></span> | <span data-ttu-id="3e9dc-3077">Datové sady, které jsou přiřazeny jako výstup pro webovou službu Azure ML.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3077">The datasets that are assigned as outputs for the Azure ML web service.</span></span> <span data-ttu-id="3e9dc-3078">Webová služba vrátí výstupní data v této datové sadě.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3078">The web service returns output data in this dataset.</span></span> | <span data-ttu-id="3e9dc-3079">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3079">Yes</span></span> | 
<span data-ttu-id="3e9dc-3080">globalParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3080">globalParameters</span></span> | <span data-ttu-id="3e9dc-3081">Zadejte hodnoty pro parametry webové služby v této části.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3081">Specify values for the web service parameters in this section.</span></span> | <span data-ttu-id="3e9dc-3082">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3082">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3e9dc-3083">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3083">JSON example</span></span>
<span data-ttu-id="3e9dc-3084">V tomto příkladu aktivity obsahuje datovou sadu **MLSqlInput** jako vstup a **MLSqlOutput** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3084">In this example, the activity has the dataset **MLSqlInput** as input and **MLSqlOutput** as the output.</span></span> <span data-ttu-id="3e9dc-3085">**MLSqlInput** předaným jako vstup k webové službě pomocí pomocí **webServiceInput** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3085">The **MLSqlInput** is passed as an input to the web service by using the **webServiceInput** JSON property.</span></span> <span data-ttu-id="3e9dc-3086">**MLSqlOutput** předaný jako výstup webové služby s použitím **webServiceOutputs** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3086">The **MLSqlOutput** is passed as an output to the Web service by using the **webServiceOutputs** JSON property.</span></span> 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

<span data-ttu-id="3e9dc-3087">V příkladu JSON nasazené služby Azure Machine Learning Web používá čtečka čipových karet a modul zapisovače pro čtení a zápis dat z/do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3087">In the JSON example, the deployed Azure Machine Learning Web service uses a reader and a writer module to read/write data from/to an Azure SQL Database.</span></span> <span data-ttu-id="3e9dc-3088">Tato webová služba zveřejňuje následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a heslo uživatelského účtu serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3088">This Web service exposes the following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>

> [!NOTE]
> <span data-ttu-id="3e9dc-3089">Pouze vstupy a výstupy aktivity AzureMLBatchExecution lze předat jako parametry webové služby.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3089">Only inputs and outputs of the AzureMLBatchExecution activity can be passed as parameters to the Web service.</span></span> <span data-ttu-id="3e9dc-3090">Například ve výše uvedeném fragmentu JSON je MLSqlInput vstup AzureMLBatchExecution aktivity, která se předá jako vstup k webové službě pomocí parametru webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3090">For example, in the above JSON snippet, MLSqlInput is an input to the AzureMLBatchExecution activity, which is passed as an input to the Web service via webServiceInput parameter.</span></span>

## <a name="machine-learning-update-resource-activity"></a><span data-ttu-id="3e9dc-3091">Aktivita aktualizace prostředku služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3091">Machine Learning Update Resource Activity</span></span>
<span data-ttu-id="3e9dc-3092">V definici Azure ML aktualizace prostředků aktivity JSON, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3092">You can specify the following properties in a Azure ML Update Resource Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3093">Musí být vlastnost typu aktivity: **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3093">The type property for the activity must be: **AzureMLUpdateResource**.</span></span> <span data-ttu-id="3e9dc-3094">Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název ji jako hodnotu **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3094">You must create a Azure Machine Learning linked service first and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-3095">Následující vlastnosti jsou podporovány v **rámci typeProperties** části Pokud nastavíte typ aktivity AzureMLUpdateResource:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3095">The following properties are supported in the **typeProperties** section when you set the type of activity to AzureMLUpdateResource:</span></span>

<span data-ttu-id="3e9dc-3096">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3096">Property</span></span> | <span data-ttu-id="3e9dc-3097">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3097">Description</span></span> | <span data-ttu-id="3e9dc-3098">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3098">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="3e9dc-3099">trainedModelName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3099">trainedModelName</span></span> | <span data-ttu-id="3e9dc-3100">Název retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3100">Name of the retrained model.</span></span> | <span data-ttu-id="3e9dc-3101">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3101">Yes</span></span> |  
<span data-ttu-id="3e9dc-3102">trainedModelDatasetName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3102">trainedModelDatasetName</span></span> | <span data-ttu-id="3e9dc-3103">Datová sada odkazuje na soubor iLearner vrácené retraining operací.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3103">Dataset pointing to the iLearner file returned by the retraining operation.</span></span> | <span data-ttu-id="3e9dc-3104">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3104">Yes</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3e9dc-3105">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3105">JSON example</span></span>
<span data-ttu-id="3e9dc-3106">Kanál má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3106">The pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="3e9dc-3107">Aktivita provedení dávky Azure ML trvá Cvičná data jako vstup a vytvoří soubor iLearner jako výstup.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3107">The Azure ML Batch Execution activity takes the training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="3e9dc-3108">Aktivity vyvolá webovou službu školení (výukový experiment zveřejněné jako webovou službu) se vstupní Cvičná data a přijímá reprezentuje soubor ilearner z webovou službu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3108">The activity invokes the training web service (training experiment exposed as a web service) with the input training data and receives the ilearner file from the webservice.</span></span> <span data-ttu-id="3e9dc-3109">PlaceholderBlob je právě fiktivní výstupní datovou sadu, která je vyžadovaná službou Azure Data Factory ke spuštění kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3109">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="3e9dc-3110">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3110">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="3e9dc-3111">V definici JSON aktivity U-SQL můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3111">You can specify the following properties in a U-SQL Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3112">Musí být vlastnost typu aktivity: **DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3112">The type property for the activity must be: **DataLakeAnalyticsU-SQL**.</span></span> <span data-ttu-id="3e9dc-3113">Musíte vytvořit služby Azure Data Lake Analytics propojené a zadejte název ji jako hodnotu **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3113">You must create an Azure Data Lake Analytics linked service and specify the name of it as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-3114">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity DataLakeAnalyticsU-SQL:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3114">The following properties are supported in the **typeProperties** section when you set the type of activity to DataLakeAnalyticsU-SQL:</span></span> 

| <span data-ttu-id="3e9dc-3115">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3115">Property</span></span> | <span data-ttu-id="3e9dc-3116">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3116">Description</span></span> | <span data-ttu-id="3e9dc-3117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3117">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-3118">scriptPath</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3118">scriptPath</span></span> |<span data-ttu-id="3e9dc-3119">Cesta ke složce, který obsahuje skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3119">Path to folder that contains the U-SQL script.</span></span> <span data-ttu-id="3e9dc-3120">Název souboru je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3120">Name of the file is case-sensitive.</span></span> |<span data-ttu-id="3e9dc-3121">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3121">No (if you use script)</span></span> |
| <span data-ttu-id="3e9dc-3122">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3122">scriptLinkedService</span></span> |<span data-ttu-id="3e9dc-3123">Propojené služby, který odkazuje úložiště, který obsahuje skript pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3123">Linked service that links the storage that contains the script to the data factory</span></span> |<span data-ttu-id="3e9dc-3124">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3124">No (if you use script)</span></span> |
| <span data-ttu-id="3e9dc-3125">Skript</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3125">script</span></span> |<span data-ttu-id="3e9dc-3126">Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3126">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="3e9dc-3127">Například: "skript": "Vytvořit databázi test".</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3127">For example: "script": "CREATE DATABASE test".</span></span> |<span data-ttu-id="3e9dc-3128">Ne (když používáte scriptPath a scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3128">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="3e9dc-3129">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3129">degreeOfParallelism</span></span> |<span data-ttu-id="3e9dc-3130">Maximální počet uzlů současně slouží ke spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3130">The maximum number of nodes simultaneously used to run the job.</span></span> |<span data-ttu-id="3e9dc-3131">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3131">No</span></span> |
| <span data-ttu-id="3e9dc-3132">Priorita</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3132">priority</span></span> |<span data-ttu-id="3e9dc-3133">Určuje, jaké úlohy mimo všechny, které jsou zařazeny do fronty, měla by být vybrána má spustit jako první.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3133">Determines which jobs out of all that are queued should be selected to run first.</span></span> <span data-ttu-id="3e9dc-3134">Čím nižší je číslo, tím vyšší je priorita.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3134">The lower the number, the higher the priority.</span></span> |<span data-ttu-id="3e9dc-3135">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3135">No</span></span> |
| <span data-ttu-id="3e9dc-3136">Parametry</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3136">parameters</span></span> |<span data-ttu-id="3e9dc-3137">Parametry pro skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3137">Parameters for the U-SQL script</span></span> |<span data-ttu-id="3e9dc-3138">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3138">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="3e9dc-3139">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3139">JSON example</span></span>

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3e9dc-3140">Další informace najdete v tématu [Data Lake Analytics U-SQL aktivity](data-factory-usql-activity.md).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3140">For more information, see [Data Lake Analytics U-SQL Activity](data-factory-usql-activity.md).</span></span> 

## <a name="stored-procedure-activity"></a><span data-ttu-id="3e9dc-3141">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3141">Stored Procedure Activity</span></span>
<span data-ttu-id="3e9dc-3142">V definici uložené procedury aktivity JSON, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3142">You can specify the following properties in a Stored Procedure Activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3143">Musí být vlastnost typu aktivity: **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3143">The type property for the activity must be: **SqlServerStoredProcedure**.</span></span> <span data-ttu-id="3e9dc-3144">Musíte vytvořit některého z následujících propojených služeb a zadejte jako hodnotu pro název propojené služby **linkedServiceName** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3144">You must create an one of the following linked services and specify the name of the linked service as a value for the **linkedServiceName** property:</span></span>

- <span data-ttu-id="3e9dc-3145">SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3145">SQL Server</span></span> 
- <span data-ttu-id="3e9dc-3146">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3146">Azure SQL Database</span></span>
- <span data-ttu-id="3e9dc-3147">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3147">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="3e9dc-3148">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na SqlServerStoredProcedure:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3148">The following properties are supported in the **typeProperties** section when you set the type of activity to SqlServerStoredProcedure:</span></span>

| <span data-ttu-id="3e9dc-3149">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3149">Property</span></span> | <span data-ttu-id="3e9dc-3150">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3150">Description</span></span> | <span data-ttu-id="3e9dc-3151">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3151">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e9dc-3152">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3152">storedProcedureName</span></span> |<span data-ttu-id="3e9dc-3153">Zadejte název uložené procedury v databázi Azure SQL nebo Azure SQL Data Warehouse, která je reprezentována propojené služby, která používá výstupní tabulka.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3153">Specify the name of the stored procedure in the Azure SQL database or Azure SQL Data Warehouse that is represented by the linked service that the output table uses.</span></span> |<span data-ttu-id="3e9dc-3154">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3154">Yes</span></span> |
| <span data-ttu-id="3e9dc-3155">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3155">storedProcedureParameters</span></span> |<span data-ttu-id="3e9dc-3156">Zadejte hodnoty pro parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3156">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="3e9dc-3157">Pokud potřebujete předat hodnotu null pro parametr, použijte syntaxi: "param1": null (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3157">If you need to pass null for a parameter, use the syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="3e9dc-3158">Viz následující ukázka Další informace o používání této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3158">See the following sample to learn about using this property.</span></span> |<span data-ttu-id="3e9dc-3159">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3159">No</span></span> |

<span data-ttu-id="3e9dc-3160">Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) se spouští aktivita uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3160">If you do specify an input dataset, it must be available (in ‘Ready’ status) for the stored procedure activity to run.</span></span> <span data-ttu-id="3e9dc-3161">Vstupní datové sady nelze zpracovat v uložené proceduře jako parametr.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3161">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="3e9dc-3162">Používá se pouze ke kontrole závislost před zahájením aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3162">It is only used to check the dependency before starting the stored procedure activity.</span></span> <span data-ttu-id="3e9dc-3163">Je nutné zadat výstupní datovou sadu aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3163">You must specify an output dataset for a stored procedure activity.</span></span> 

<span data-ttu-id="3e9dc-3164">Určuje výstupní datovou sadu **plán** aktivity uložené procedury (každou hodinu, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3164">Output dataset specifies the **schedule** for the stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <span data-ttu-id="3e9dc-3165">Musíte použít výstupní datovou sadu **propojená služba** který odkazuje na databázi SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, který chcete spustit uloženou proceduru.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3165">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <span data-ttu-id="3e9dc-3166">Výstupní datovou sadu může sloužit jako způsob, jak předat výsledek uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) v kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3166">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in the pipeline.</span></span> <span data-ttu-id="3e9dc-3167">Ale objekt pro vytváření dat nelze zapsat automaticky výstup uložené procedury pro tuto datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3167">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="3e9dc-3168">Je uložené procedury, která zapisuje do tabulky SQL, odkazující na výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3168">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <span data-ttu-id="3e9dc-3169">V některých případech může být výstupní datovou sadu **fiktivní datovou sadu**, který slouží pouze k určení plánu pro spuštěnou aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3169">In some cases, the output dataset can be a **dummy dataset**, which is used only to specify the schedule for running the stored procedure activity.</span></span>  

### <a name="json-example"></a><span data-ttu-id="3e9dc-3170">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3170">JSON example</span></span>

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

<span data-ttu-id="3e9dc-3171">Další informace najdete v tématu [aktivity uložené procedury](data-factory-stored-proc-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3171">For more information, see [Stored Procedure Activity](data-factory-stored-proc-activity.md) article.</span></span> 

## <a name="net-custom-activity"></a><span data-ttu-id="3e9dc-3172">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3172">.NET custom activity</span></span>
<span data-ttu-id="3e9dc-3173">V rozhraní .NET vlastní aktivity definici JSON, můžete zadat následující vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3173">You can specify the following properties in a .NET custom activity JSON definition.</span></span> <span data-ttu-id="3e9dc-3174">Musí být vlastnost typu aktivity: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3174">The type property for the activity must be: **DotNetActivity**.</span></span> <span data-ttu-id="3e9dc-3175">Musíte vytvořit propojené služby Azure HDInsight nebo Azure Batch propojené služby a zadejte jako hodnotu pro název propojené služby **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3175">You must create an Azure HDInsight linked service or an Azure Batch linked service, and specify the name of the linked service as a value for the **linkedServiceName** property.</span></span> <span data-ttu-id="3e9dc-3176">Následující vlastnosti jsou podporovány v **rámci typeProperties** části při nastavení typu aktivity na DotNetActivity:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3176">The following properties are supported in the **typeProperties** section when you set the type of activity to DotNetActivity:</span></span>
 
| <span data-ttu-id="3e9dc-3177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3177">Property</span></span> | <span data-ttu-id="3e9dc-3178">Popis</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3178">Description</span></span> | <span data-ttu-id="3e9dc-3179">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3179">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="3e9dc-3180">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3180">AssemblyName</span></span> | <span data-ttu-id="3e9dc-3181">Název sestavení.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3181">Name of the assembly.</span></span> <span data-ttu-id="3e9dc-3182">V příkladu je: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3182">In the example, it is: **MyDotnetActivity.dll**.</span></span> | <span data-ttu-id="3e9dc-3183">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3183">Yes</span></span> |
| <span data-ttu-id="3e9dc-3184">Vstupní bod</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3184">EntryPoint</span></span> |<span data-ttu-id="3e9dc-3185">Název třídy, která implementuje rozhraní IDotNetActivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3185">Name of the class that implements the IDotNetActivity interface.</span></span> <span data-ttu-id="3e9dc-3186">V příkladu je: **MyDotNetActivityNS.MyDotNetActivity** kde MyDotNetActivityNS je obor názvů a MyDotNetActivity je třída.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3186">In the example, it is: **MyDotNetActivityNS.MyDotNetActivity** where MyDotNetActivityNS is the namespace and MyDotNetActivity is the class.</span></span>  | <span data-ttu-id="3e9dc-3187">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3187">Yes</span></span> | 
| <span data-ttu-id="3e9dc-3188">PackageLinkedService</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3188">PackageLinkedService</span></span> | <span data-ttu-id="3e9dc-3189">Název úložiště Azure, propojené služby, která odkazuje na úložiště objektů blob, který obsahuje soubor zip vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3189">Name of the Azure Storage linked service that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="3e9dc-3190">V příkladu je: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3190">In the example, it is: **AzureStorageLinkedService**.</span></span>| <span data-ttu-id="3e9dc-3191">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3191">Yes</span></span> |
| <span data-ttu-id="3e9dc-3192">PackageFile</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3192">PackageFile</span></span> | <span data-ttu-id="3e9dc-3193">Název souboru zip.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3193">Name of the zip file.</span></span> <span data-ttu-id="3e9dc-3194">V příkladu je: **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3194">In the example, it is: **customactivitycontainer/MyDotNetActivity.zip**.</span></span> | <span data-ttu-id="3e9dc-3195">Ano</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3195">Yes</span></span> |
| <span data-ttu-id="3e9dc-3196">ExtendedProperties</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3196">extendedProperties</span></span> | <span data-ttu-id="3e9dc-3197">Rozšířené vlastnosti, které můžete definovat a předat kód .NET.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3197">Extended properties that you can define and pass on to the .NET code.</span></span> <span data-ttu-id="3e9dc-3198">V tomto příkladu **SliceStart** proměnná je nastavená na hodnotu podle proměnnou SliceStart systému.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3198">In this example, the **SliceStart** variable is set to a value based on the SliceStart system variable.</span></span> | <span data-ttu-id="3e9dc-3199">Ne</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3199">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="3e9dc-3200">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3200">JSON example</span></span>

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

<span data-ttu-id="3e9dc-3201">Podrobné informace najdete v tématu [použít vlastní aktivity v datové továrně](data-factory-use-custom-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3201">For detailed information, see [Use custom activities in Data Factory](data-factory-use-custom-activities.md) article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="3e9dc-3202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3202">Next Steps</span></span>
<span data-ttu-id="3e9dc-3203">Viz následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3203">See the following tutorials:</span></span> 

- [<span data-ttu-id="3e9dc-3204">Kurz: vytvoření kanálu s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3204">Tutorial: create a pipeline with a copy activity</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [<span data-ttu-id="3e9dc-3205">Kurz: vytvoření kanálu s aktivitou hive</span><span class="sxs-lookup"><span data-stu-id="3e9dc-3205">Tutorial: create a pipeline with a hive activity</span></span>](data-factory-build-your-first-pipeline-using-editor.md)