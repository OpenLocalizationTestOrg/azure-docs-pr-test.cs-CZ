---
title: "aaaAzure objekt pro vytváření dat – referenční dokumentace skriptování JSON | Microsoft Docs"
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
ms.openlocfilehash: 813fd752bb0ecb1b513d022b9f302325105dac31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---json-scripting-reference"></a><span data-ttu-id="5e83d-103">Azure Data Factory - referenčních informacích o skriptování JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-103">Azure Data Factory - JSON Scripting Reference</span></span>
<span data-ttu-id="5e83d-104">Tento článek obsahuje schémata JSON a příklady pro definování entity Azure Data Factory (kanál, aktivity, datové sady a propojené služby).</span><span class="sxs-lookup"><span data-stu-id="5e83d-104">This article provides JSON schemas and examples for defining Azure Data Factory entities (pipeline, activity, dataset, and linked service).</span></span>  

## <a name="pipeline"></a><span data-ttu-id="5e83d-105">Kanál</span><span class="sxs-lookup"><span data-stu-id="5e83d-105">Pipeline</span></span> 
<span data-ttu-id="5e83d-106">Hello základní strukturu pro definici kanálu je následující:</span><span class="sxs-lookup"><span data-stu-id="5e83d-106">hello high-level structure for a pipeline definition is as follows:</span></span> 

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

<span data-ttu-id="5e83d-107">Následující tabulka popisuje vlastnosti hello v rámci kanálu hello definici JSON:</span><span class="sxs-lookup"><span data-stu-id="5e83d-107">Following table describes hello properties within hello pipeline JSON definition:</span></span>

| <span data-ttu-id="5e83d-108">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-108">Property</span></span> | <span data-ttu-id="5e83d-109">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-109">Description</span></span> | <span data-ttu-id="5e83d-110">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-110">Required</span></span>
-------- | ----------- | --------
| <span data-ttu-id="5e83d-111">jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-111">name</span></span> | <span data-ttu-id="5e83d-112">Název kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-112">Name of hello pipeline.</span></span> <span data-ttu-id="5e83d-113">Zadejte název, který představuje hello akci, která hello aktivity nebo kanál je nakonfigurované toodo</span><span class="sxs-lookup"><span data-stu-id="5e83d-113">Specify a name that represents hello action that hello activity or pipeline is configured toodo</span></span><br/><ul><li><span data-ttu-id="5e83d-114">Maximální počet znaků: 260.</span><span class="sxs-lookup"><span data-stu-id="5e83d-114">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="5e83d-115">Musí začínat písmenem, číslicí nebo podtržítkem (_).</span><span class="sxs-lookup"><span data-stu-id="5e83d-115">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="5e83d-116">Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="5e83d-116">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="5e83d-117">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-117">Yes</span></span> |
| <span data-ttu-id="5e83d-118">description</span><span class="sxs-lookup"><span data-stu-id="5e83d-118">description</span></span> |<span data-ttu-id="5e83d-119">Popisuje, jaké aktivity hello nebo kanálu se používá pro text</span><span class="sxs-lookup"><span data-stu-id="5e83d-119">Text describing what hello activity or pipeline is used for</span></span> | <span data-ttu-id="5e83d-120">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-120">No</span></span> |
| <span data-ttu-id="5e83d-121">activities</span><span class="sxs-lookup"><span data-stu-id="5e83d-121">activities</span></span> | <span data-ttu-id="5e83d-122">Obsahuje seznam aktivit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-122">Contains a list of activities.</span></span> | <span data-ttu-id="5e83d-123">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-123">Yes</span></span> |
| <span data-ttu-id="5e83d-124">start</span><span class="sxs-lookup"><span data-stu-id="5e83d-124">start</span></span> |<span data-ttu-id="5e83d-125">Počáteční datum a čas pro kanál hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-125">Start date-time for hello pipeline.</span></span> <span data-ttu-id="5e83d-126">Musí být v [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="5e83d-126">Must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="5e83d-127">Příklad: 2014-10-14T16:32:41.</span><span class="sxs-lookup"><span data-stu-id="5e83d-127">For example: 2014-10-14T16:32:41.</span></span> <br/><br/><span data-ttu-id="5e83d-128">Je možné toospecify místního času, například Odhadovaný čas.</span><span class="sxs-lookup"><span data-stu-id="5e83d-128">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="5e83d-129">Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6</span><span class="sxs-lookup"><span data-stu-id="5e83d-129">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="5e83d-130">Hello počáteční a koncové vlastnosti společně zadejte aktivní období pro kanál hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-130">hello start and end properties together specify active period for hello pipeline.</span></span> <span data-ttu-id="5e83d-131">Výstup řezy jenom vytváří se v tomto aktivní období.</span><span class="sxs-lookup"><span data-stu-id="5e83d-131">Output slices are only produced with in this active period.</span></span> |<span data-ttu-id="5e83d-132">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-132">No</span></span><br/><br/><span data-ttu-id="5e83d-133">Pokud zadáte hodnotu pro vlastnost end hello, musíte zadat hodnotu pro vlastnost začátku hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-133">If you specify a value for hello end property, you must specify value for hello start property.</span></span><br/><br/><span data-ttu-id="5e83d-134">Hello počáteční a koncový čas může být prázdný toocreate kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-134">hello start and end times can both be empty toocreate a pipeline.</span></span> <span data-ttu-id="5e83d-135">Je potřeba zadat obě hodnoty tooset na aktivní období kanálu toorun hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-135">You must specify both values tooset an active period for hello pipeline toorun.</span></span> <span data-ttu-id="5e83d-136">Pokud nezadáte počáteční a koncový čas při vytváření kanálu, můžete je nastavit pomocí rutiny Set-AzureRmDataFactoryPipelineActivePeriod hello později.</span><span class="sxs-lookup"><span data-stu-id="5e83d-136">If you do not specify start and end times when creating a pipeline, you can set them using hello Set-AzureRmDataFactoryPipelineActivePeriod cmdlet later.</span></span> |
| <span data-ttu-id="5e83d-137">End</span><span class="sxs-lookup"><span data-stu-id="5e83d-137">end</span></span> |<span data-ttu-id="5e83d-138">Koncové datum a čas pro kanál hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-138">End date-time for hello pipeline.</span></span> <span data-ttu-id="5e83d-139">Pokud zadaný, musí být ve formátu ISO.</span><span class="sxs-lookup"><span data-stu-id="5e83d-139">If specified must be in ISO format.</span></span> <span data-ttu-id="5e83d-140">Příklad: 2014-10-14T17:32:41</span><span class="sxs-lookup"><span data-stu-id="5e83d-140">For example: 2014-10-14T17:32:41</span></span> <br/><br/><span data-ttu-id="5e83d-141">Je možné toospecify místního času, například Odhadovaný čas.</span><span class="sxs-lookup"><span data-stu-id="5e83d-141">It is possible toospecify a local time, for example an EST time.</span></span> <span data-ttu-id="5e83d-142">Tady je příklad: `2016-02-27T06:00:00**-05:00`, což je odhadované AM 6</span><span class="sxs-lookup"><span data-stu-id="5e83d-142">Here is an example: `2016-02-27T06:00:00**-05:00`, which is 6 AM EST.</span></span><br/><br/><span data-ttu-id="5e83d-143">9999-09-09 toorun hello kanálu bez omezení, zadejte jako hello hodnotu pro vlastnost end hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-143">toorun hello pipeline indefinitely, specify 9999-09-09 as hello value for hello end property.</span></span> |<span data-ttu-id="5e83d-144">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-144">No</span></span> <br/><br/><span data-ttu-id="5e83d-145">Pokud zadáte hodnotu pro vlastnost začátku hello, musíte zadat hodnotu pro vlastnost end hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-145">If you specify a value for hello start property, you must specify value for hello end property.</span></span><br/><br/><span data-ttu-id="5e83d-146">Naleznete v poznámkách k hello **spustit** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-146">See notes for hello **start** property.</span></span> |
| <span data-ttu-id="5e83d-147">isPaused</span><span class="sxs-lookup"><span data-stu-id="5e83d-147">isPaused</span></span> |<span data-ttu-id="5e83d-148">Pokud sada tootrue hello kanálu nelze spustit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-148">If set tootrue hello pipeline does not run.</span></span> <span data-ttu-id="5e83d-149">Výchozí hodnota = false.</span><span class="sxs-lookup"><span data-stu-id="5e83d-149">Default value = false.</span></span> <span data-ttu-id="5e83d-150">Můžete použít tento tooenable vlastnosti nebo zakázat.</span><span class="sxs-lookup"><span data-stu-id="5e83d-150">You can use this property tooenable or disable.</span></span> |<span data-ttu-id="5e83d-151">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-151">No</span></span> |
| <span data-ttu-id="5e83d-152">pipelineMode</span><span class="sxs-lookup"><span data-stu-id="5e83d-152">pipelineMode</span></span> |<span data-ttu-id="5e83d-153">Metoda Hello plánování spuštění pro hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-153">hello method for scheduling runs for hello pipeline.</span></span> <span data-ttu-id="5e83d-154">Povolené hodnoty jsou: naplánované (výchozí), jednorázově.</span><span class="sxs-lookup"><span data-stu-id="5e83d-154">Allowed values are: scheduled (default), onetime.</span></span><br/><br/><span data-ttu-id="5e83d-155">"Pravidelnou" označuje, že kanál hello se spustí v zadaném časovém intervalu podle tooits aktivní období (počáteční a koncový čas).</span><span class="sxs-lookup"><span data-stu-id="5e83d-155">‘Scheduled’ indicates that hello pipeline runs at a specified time interval according tooits active period (start and end time).</span></span> <span data-ttu-id="5e83d-156">"Jednorázově" označuje, že kanál hello spustí jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="5e83d-156">‘Onetime’ indicates that hello pipeline runs only once.</span></span> <span data-ttu-id="5e83d-157">Po vytvoření jednorázově kanály nelze aktuálně upravit nebo aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="5e83d-157">Onetime pipelines once created cannot be modified/updated currently.</span></span> <span data-ttu-id="5e83d-158">V tématu [Onetime kanálu](data-factory-create-pipelines.md#onetime-pipeline) podrobnosti o jednorázově nastavení.</span><span class="sxs-lookup"><span data-stu-id="5e83d-158">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details about onetime setting.</span></span> |<span data-ttu-id="5e83d-159">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-159">No</span></span> |
| <span data-ttu-id="5e83d-160">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="5e83d-160">expirationTime</span></span> |<span data-ttu-id="5e83d-161">Doba, po vytvoření, pro které hello kanálu je platný a by měla zůstat zřízené.</span><span class="sxs-lookup"><span data-stu-id="5e83d-161">Duration of time after creation for which hello pipeline is valid and should remain provisioned.</span></span> <span data-ttu-id="5e83d-162">Pokud nemá žádné aktivní, se nezdařilo, nebo čekající spuštění kanálu hello automaticky odstraněna po nedosáhne hello čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-162">If it does not have any active, failed, or pending runs, hello pipeline is deleted automatically once it reaches hello expiration time.</span></span> |<span data-ttu-id="5e83d-163">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-163">No</span></span> |


## <a name="activity"></a><span data-ttu-id="5e83d-164">Aktivita</span><span class="sxs-lookup"><span data-stu-id="5e83d-164">Activity</span></span> 
<span data-ttu-id="5e83d-165">Základní struktura Hello pro aktivitu v rámci kanálu definice (aktivity element) je následující:</span><span class="sxs-lookup"><span data-stu-id="5e83d-165">hello high-level structure for an activity within a pipeline definition (activities element) is as follows:</span></span>

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

<span data-ttu-id="5e83d-166">Následující tabulky popisují hello vlastnosti v rámci aktivity hello definici JSON:</span><span class="sxs-lookup"><span data-stu-id="5e83d-166">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="5e83d-167">Značka</span><span class="sxs-lookup"><span data-stu-id="5e83d-167">Tag</span></span> | <span data-ttu-id="5e83d-168">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-168">Description</span></span> | <span data-ttu-id="5e83d-169">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-169">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-170">jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-170">name</span></span> |<span data-ttu-id="5e83d-171">Název aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-171">Name of hello activity.</span></span> <span data-ttu-id="5e83d-172">Zadejte název, který představuje hello akci, která aktivita hello nakonfigurovat toodo</span><span class="sxs-lookup"><span data-stu-id="5e83d-172">Specify a name that represents hello action that hello activity is configured toodo</span></span><br/><ul><li><span data-ttu-id="5e83d-173">Maximální počet znaků: 260.</span><span class="sxs-lookup"><span data-stu-id="5e83d-173">Maximum number of characters: 260</span></span></li><li><span data-ttu-id="5e83d-174">Musí začínat písmenem, číslicí nebo podtržítkem (_).</span><span class="sxs-lookup"><span data-stu-id="5e83d-174">Must start with a letter number, or an underscore (_)</span></span></li><li><span data-ttu-id="5e83d-175">Nejsou povolené tyto znaky: ".", "+","?", "/", "<",">", "*", "%", "&", ":","\\"</span><span class="sxs-lookup"><span data-stu-id="5e83d-175">Following characters are not allowed: “.”, “+”, “?”, “/”, “<”,”>”,”*”,”%”,”&”,”:”,”\\”</span></span></li></ul> |<span data-ttu-id="5e83d-176">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-176">Yes</span></span> |
| <span data-ttu-id="5e83d-177">description</span><span class="sxs-lookup"><span data-stu-id="5e83d-177">description</span></span> |<span data-ttu-id="5e83d-178">Popisuje, jaké aktivity hello se používá pro text.</span><span class="sxs-lookup"><span data-stu-id="5e83d-178">Text describing what hello activity is used for.</span></span> |<span data-ttu-id="5e83d-179">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-179">Yes</span></span> |
| <span data-ttu-id="5e83d-180">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-180">type</span></span> |<span data-ttu-id="5e83d-181">Určuje typ hello hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-181">Specifies hello type of hello activity.</span></span> <span data-ttu-id="5e83d-182">V tématu hello [ÚLOŽIŠŤ dat](#data-stores) a [aktivit TRANSFORMACE dat](#data-transformation-activities) oddíly pro různé typy aktivit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-182">See hello [DATA STORES](#data-stores) and [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) sections for different types of activities.</span></span> |<span data-ttu-id="5e83d-183">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-183">Yes</span></span> |
| <span data-ttu-id="5e83d-184">Vstupy</span><span class="sxs-lookup"><span data-stu-id="5e83d-184">inputs</span></span> |<span data-ttu-id="5e83d-185">Vstupní tabulky použité aktivitou hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-185">Input tables used by hello activity</span></span><br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |<span data-ttu-id="5e83d-186">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-186">Yes</span></span> |
| <span data-ttu-id="5e83d-187">Výstupy</span><span class="sxs-lookup"><span data-stu-id="5e83d-187">outputs</span></span> |<span data-ttu-id="5e83d-188">Výstupní tabulky použité aktivitou hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-188">Output tables used by hello activity.</span></span><br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |<span data-ttu-id="5e83d-189">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-189">Yes</span></span> |
| <span data-ttu-id="5e83d-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="5e83d-190">linkedServiceName</span></span> |<span data-ttu-id="5e83d-191">Název hello propojené služby používané hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-191">Name of hello linked service used by hello activity.</span></span> <br/><br/><span data-ttu-id="5e83d-192">Aktivita může vyžadovat, že zadáváte hello propojené služby, která propojí toohello požadované výpočetním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-192">An activity may require that you specify hello linked service that links toohello required compute environment.</span></span> |<span data-ttu-id="5e83d-193">Ano pro aktivity HDInsight, Azure Machine Learning aktivity a aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-193">Yes for HDInsight activities, Azure Machine Learning activities, and Stored Procedure Activity.</span></span> <br/><br/><span data-ttu-id="5e83d-194">Ne ve všech ostatních případech</span><span class="sxs-lookup"><span data-stu-id="5e83d-194">No for all others</span></span> |
| <span data-ttu-id="5e83d-195">typeProperties</span><span class="sxs-lookup"><span data-stu-id="5e83d-195">typeProperties</span></span> |<span data-ttu-id="5e83d-196">Vlastnosti v rámci typeProperties části hello závisí na typu aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-196">Properties in hello typeProperties section depend on type of hello activity.</span></span> |<span data-ttu-id="5e83d-197">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-197">No</span></span> |
| <span data-ttu-id="5e83d-198">policy</span><span class="sxs-lookup"><span data-stu-id="5e83d-198">policy</span></span> |<span data-ttu-id="5e83d-199">Zásady, které ovlivňují chování běhové hello hello aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-199">Policies that affect hello run-time behavior of hello activity.</span></span> <span data-ttu-id="5e83d-200">Pokud není zadaný, použijí se výchozí zásady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-200">If it is not specified, default policies are used.</span></span> |<span data-ttu-id="5e83d-201">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-201">No</span></span> |
| <span data-ttu-id="5e83d-202">Scheduler</span><span class="sxs-lookup"><span data-stu-id="5e83d-202">scheduler</span></span> |<span data-ttu-id="5e83d-203">Vlastnost "scheduler" je použité toodefine potřeby plánování aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-203">“scheduler” property is used toodefine desired scheduling for hello activity.</span></span> <span data-ttu-id="5e83d-204">Jeho podvlastnosti jsou hello stejné jako ty, které v hello hello [vlastnost availability v datové sadě](data-factory-create-datasets.md#dataset-availability).</span><span class="sxs-lookup"><span data-stu-id="5e83d-204">Its subproperties are hello same as hello ones in hello [availability property in a dataset](data-factory-create-datasets.md#dataset-availability).</span></span> |<span data-ttu-id="5e83d-205">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-205">No</span></span> |

### <a name="policies"></a><span data-ttu-id="5e83d-206">Zásady</span><span class="sxs-lookup"><span data-stu-id="5e83d-206">Policies</span></span>
<span data-ttu-id="5e83d-207">Zásady ovlivňují chování hello běhu aktivity, konkrétně v případě, že je zpracování řezu hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-207">Policies affect hello run-time behavior of an activity, specifically when hello slice of a table is processed.</span></span> <span data-ttu-id="5e83d-208">Hello následující tabulka poskytuje podrobnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-208">hello following table provides hello details.</span></span>

| <span data-ttu-id="5e83d-209">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-209">Property</span></span> | <span data-ttu-id="5e83d-210">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-210">Permitted values</span></span> | <span data-ttu-id="5e83d-211">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="5e83d-211">Default Value</span></span> | <span data-ttu-id="5e83d-212">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-212">Description</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-213">Souběžnosti</span><span class="sxs-lookup"><span data-stu-id="5e83d-213">concurrency</span></span> |<span data-ttu-id="5e83d-214">Integer</span><span class="sxs-lookup"><span data-stu-id="5e83d-214">Integer</span></span> <br/><br/><span data-ttu-id="5e83d-215">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="5e83d-215">Max value: 10</span></span> |<span data-ttu-id="5e83d-216">1</span><span class="sxs-lookup"><span data-stu-id="5e83d-216">1</span></span> |<span data-ttu-id="5e83d-217">Počet souběžných spuštění aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-217">Number of concurrent executions of hello activity.</span></span><br/><br/><span data-ttu-id="5e83d-218">Určuje hello počet spuštěních paralelní aktivity, které se může stát při jiné řezy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-218">It determines hello number of parallel activity executions that can happen on different slices.</span></span> <span data-ttu-id="5e83d-219">Například pokud aktivita vyžaduje toogo prostřednictvím velké sady dostupných dat, mají větší hodnotu souběžnosti urychluje zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-219">For example, if an activity needs toogo through a large set of available data, having a larger concurrency value speeds up hello data processing.</span></span> |
| <span data-ttu-id="5e83d-220">executionPriorityOrder</span><span class="sxs-lookup"><span data-stu-id="5e83d-220">executionPriorityOrder</span></span> |<span data-ttu-id="5e83d-221">NewestFirst</span><span class="sxs-lookup"><span data-stu-id="5e83d-221">NewestFirst</span></span><br/><br/><span data-ttu-id="5e83d-222">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="5e83d-222">OldestFirst</span></span> |<span data-ttu-id="5e83d-223">OldestFirst</span><span class="sxs-lookup"><span data-stu-id="5e83d-223">OldestFirst</span></span> |<span data-ttu-id="5e83d-224">Určuje pořadí hello datové řezy, které jsou zpracovávány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-224">Determines hello ordering of data slices that are being processed.</span></span><br/><br/><span data-ttu-id="5e83d-225">Pokud máte 2 řezy (jeden situaci ve 4 a další v 17: 00) a jsou obě čekající na zpracování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-225">For example, if you have 2 slices (one happening at 4pm, and another one at 5pm), and both are pending execution.</span></span> <span data-ttu-id="5e83d-226">Pokud jste nastavili hello executionPriorityOrder toobe NewestFirst, hello řez v 17: 00, je zpracován jako první.</span><span class="sxs-lookup"><span data-stu-id="5e83d-226">If you set hello executionPriorityOrder toobe NewestFirst, hello slice at 5 PM is processed first.</span></span> <span data-ttu-id="5e83d-227">Podobně pokud nastavíte hello executionPriorityORder toobe OldestFIrst, pak hello ve 4 zpracování řezu se.</span><span class="sxs-lookup"><span data-stu-id="5e83d-227">Similarly if you set hello executionPriorityORder toobe OldestFIrst, then hello slice at 4 PM is processed.</span></span> |
| <span data-ttu-id="5e83d-228">retry</span><span class="sxs-lookup"><span data-stu-id="5e83d-228">retry</span></span> |<span data-ttu-id="5e83d-229">Integer</span><span class="sxs-lookup"><span data-stu-id="5e83d-229">Integer</span></span><br/><br/><span data-ttu-id="5e83d-230">Maximální hodnota může být 10</span><span class="sxs-lookup"><span data-stu-id="5e83d-230">Max value can be 10</span></span> |<span data-ttu-id="5e83d-231">0</span><span class="sxs-lookup"><span data-stu-id="5e83d-231">0</span></span> |<span data-ttu-id="5e83d-232">Počet opakovaných pokusů před hello zpracování dat pro hello řez je označena jako selhání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-232">Number of retries before hello data processing for hello slice is marked as Failure.</span></span> <span data-ttu-id="5e83d-233">Provedení aktivity pro datový řez je opakovat až toohello zadaný počet opakování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-233">Activity execution for a data slice is retried up toohello specified retry count.</span></span> <span data-ttu-id="5e83d-234">co nejdříve po selhání hello se provádí Hello opakování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-234">hello retry is done as soon as possible after hello failure.</span></span> |
| <span data-ttu-id="5e83d-235">timeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-235">timeout</span></span> |<span data-ttu-id="5e83d-236">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-236">TimeSpan</span></span> |<span data-ttu-id="5e83d-237">00:00:00</span><span class="sxs-lookup"><span data-stu-id="5e83d-237">00:00:00</span></span> |<span data-ttu-id="5e83d-238">Časový limit aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-238">Timeout for hello activity.</span></span> <span data-ttu-id="5e83d-239">Příklad: 00:10:00 (znamená časový limit 10 minut)</span><span class="sxs-lookup"><span data-stu-id="5e83d-239">Example: 00:10:00 (implies timeout 10 mins)</span></span><br/><br/><span data-ttu-id="5e83d-240">Pokud hodnota není zadána nebo je 0, vypršení časového limitu hello je nekonečno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-240">If a value is not specified or is 0, hello timeout is infinite.</span></span><br/><br/><span data-ttu-id="5e83d-241">Pokud doba zpracování dat hello na řez překročí hodnota časového limitu hello, se zruší a hello systém pokusí tooretry hello zpracování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-241">If hello data processing time on a slice exceeds hello timeout value, it is canceled, and hello system attempts tooretry hello processing.</span></span> <span data-ttu-id="5e83d-242">Hello počet opakovaných pokusů závisí na vlastnosti opakování hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-242">hello number of retries depends on hello retry property.</span></span> <span data-ttu-id="5e83d-243">Když dojde k vypršení časového limitu, je nastaven stav hello tooTimedOut.</span><span class="sxs-lookup"><span data-stu-id="5e83d-243">When timeout occurs, hello status is set tooTimedOut.</span></span> |
| <span data-ttu-id="5e83d-244">Zpoždění</span><span class="sxs-lookup"><span data-stu-id="5e83d-244">delay</span></span> |<span data-ttu-id="5e83d-245">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-245">TimeSpan</span></span> |<span data-ttu-id="5e83d-246">00:00:00</span><span class="sxs-lookup"><span data-stu-id="5e83d-246">00:00:00</span></span> |<span data-ttu-id="5e83d-247">Zadejte zpoždění hello před spuštěním zpracování dat hello řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-247">Specify hello delay before data processing of hello slice starts.</span></span><br/><br/><span data-ttu-id="5e83d-248">Hello provádění aktivity pro datový řez je spuštěn v minulosti hello očekávaný čas provádění po hello zpoždění.</span><span class="sxs-lookup"><span data-stu-id="5e83d-248">hello execution of activity for a data slice is started after hello Delay is past hello expected execution time.</span></span><br/><br/><span data-ttu-id="5e83d-249">Příklad: 00:10:00 (znamená zpoždění 10 minut)</span><span class="sxs-lookup"><span data-stu-id="5e83d-249">Example: 00:10:00 (implies delay of 10 mins)</span></span> |
| <span data-ttu-id="5e83d-250">opakování po delší době</span><span class="sxs-lookup"><span data-stu-id="5e83d-250">longRetry</span></span> |<span data-ttu-id="5e83d-251">Integer</span><span class="sxs-lookup"><span data-stu-id="5e83d-251">Integer</span></span><br/><br/><span data-ttu-id="5e83d-252">Maximální hodnota: 10</span><span class="sxs-lookup"><span data-stu-id="5e83d-252">Max value: 10</span></span> |<span data-ttu-id="5e83d-253">1</span><span class="sxs-lookup"><span data-stu-id="5e83d-253">1</span></span> |<span data-ttu-id="5e83d-254">Hello počet dlouho opakování pokusů, než hello řez spuštění se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="5e83d-254">hello number of long retry attempts before hello slice execution is failed.</span></span><br/><br/><span data-ttu-id="5e83d-255">pokusy o opakování po delší době jsou rozmístěny ve longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="5e83d-255">longRetry attempts are spaced by longRetryInterval.</span></span> <span data-ttu-id="5e83d-256">Takže pokud budete potřebovat toospecify doba mezi pokusy o opakování, použijte opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="5e83d-256">So if you need toospecify a time between retry attempts, use longRetry.</span></span> <span data-ttu-id="5e83d-257">Pokud jsou zadané opakování a opakování po delší době, jednotlivé pokusy o opakování po delší době zahrnuje opakovaných pokusů a je hello maximální počet pokusů o opakování * opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="5e83d-257">If both Retry and longRetry are specified, each longRetry attempt includes Retry attempts and hello max number of attempts is Retry * longRetry.</span></span><br/><br/><span data-ttu-id="5e83d-258">Například, pokud bychom měli hello následující nastavení v zásadě hello aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-258">For example, if we have hello following settings in hello activity policy:</span></span><br/><span data-ttu-id="5e83d-259">Opakujte: 3</span><span class="sxs-lookup"><span data-stu-id="5e83d-259">Retry: 3</span></span><br/><span data-ttu-id="5e83d-260">opakování po delší době: 2</span><span class="sxs-lookup"><span data-stu-id="5e83d-260">longRetry: 2</span></span><br/><span data-ttu-id="5e83d-261">longRetryInterval: 01:00:00</span><span class="sxs-lookup"><span data-stu-id="5e83d-261">longRetryInterval: 01:00:00</span></span><br/><br/><span data-ttu-id="5e83d-262">Předpokládá se jenom jeden řez tooexecute (stav Čeká) a provedení aktivity hello pokaždé, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="5e83d-262">Assume there is only one slice tooexecute (status is Waiting) and hello activity execution fails every time.</span></span> <span data-ttu-id="5e83d-263">Nejdřív by 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-263">Initially there would be 3 consecutive execution attempts.</span></span> <span data-ttu-id="5e83d-264">Po každém pokusu o stav řezu hello bude opakovat.</span><span class="sxs-lookup"><span data-stu-id="5e83d-264">After each attempt, hello slice status would be Retry.</span></span> <span data-ttu-id="5e83d-265">Po první 3 pokusy jsou přes, bude stav řezu hello opakování po delší době.</span><span class="sxs-lookup"><span data-stu-id="5e83d-265">After first 3 attempts are over, hello slice status would be LongRetry.</span></span><br/><br/><span data-ttu-id="5e83d-266">Po hodině (který je na longRetryInteval hodnota) bude další sadu 3 provádění po sobě jdoucích pokusů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-266">After an hour (that is, longRetryInteval’s value), there would be another set of 3 consecutive execution attempts.</span></span> <span data-ttu-id="5e83d-267">Od tohoto stavu řezu hello by se nezdařilo a by se pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-267">After that, hello slice status would be Failed and no more retries would be attempted.</span></span> <span data-ttu-id="5e83d-268">Proto celkové 6 pokusy byly provedeny.</span><span class="sxs-lookup"><span data-stu-id="5e83d-268">Hence overall 6 attempts were made.</span></span><br/><br/><span data-ttu-id="5e83d-269">Pokud žádné spuštění úspěšné, stav řezu hello by připravené a jsou pokus o žádné další opakování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-269">If any execution succeeds, hello slice status would be Ready and no more retries are attempted.</span></span><br/><br/><span data-ttu-id="5e83d-270">opakování po delší době je možné použít situace, kdy závislé data dorazí v časech Nedeterministický nebo hello celém prostředí je v nestabilním stavu v rámci které zpracování dat dojde.</span><span class="sxs-lookup"><span data-stu-id="5e83d-270">longRetry may be used in situations where dependent data arrives at non-deterministic times or hello overall environment is flaky under which data processing occurs.</span></span> <span data-ttu-id="5e83d-271">V takových případech nemusí být úspěšná při provádění opakování, jedna po druhé a tak v intervalech čas má za následek hello potřeby výstup.</span><span class="sxs-lookup"><span data-stu-id="5e83d-271">In such cases, doing retries one after another may not help and doing so after an interval of time results in hello desired output.</span></span><br/><br/><span data-ttu-id="5e83d-272">Word varování: nenastavujte vysoké hodnoty pro opakování po delší době nebo longRetryInterval.</span><span class="sxs-lookup"><span data-stu-id="5e83d-272">Word of caution: do not set high values for longRetry or longRetryInterval.</span></span> <span data-ttu-id="5e83d-273">Vyšší hodnoty obvykle implikují dalších systémových otázek.</span><span class="sxs-lookup"><span data-stu-id="5e83d-273">Typically, higher values imply other systemic issues.</span></span> |
| <span data-ttu-id="5e83d-274">longRetryInterval</span><span class="sxs-lookup"><span data-stu-id="5e83d-274">longRetryInterval</span></span> |<span data-ttu-id="5e83d-275">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-275">TimeSpan</span></span> |<span data-ttu-id="5e83d-276">00:00:00</span><span class="sxs-lookup"><span data-stu-id="5e83d-276">00:00:00</span></span> |<span data-ttu-id="5e83d-277">Hello prodleva mezi pokusy o opakování dlouho</span><span class="sxs-lookup"><span data-stu-id="5e83d-277">hello delay between long retry attempts</span></span> |

### <a name="typeproperties-section"></a><span data-ttu-id="5e83d-278">části v rámci typeProperties</span><span class="sxs-lookup"><span data-stu-id="5e83d-278">typeProperties section</span></span>
<span data-ttu-id="5e83d-279">část rámci typeProperties Hello se liší pro každou aktivitu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-279">hello typeProperties section is different for each activity.</span></span> <span data-ttu-id="5e83d-280">Transformace aktivity mají pouze vlastnosti typu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-280">Transformation activities have just hello type properties.</span></span> <span data-ttu-id="5e83d-281">V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-281">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span> 

<span data-ttu-id="5e83d-282">**Aktivita kopírování** má dvě témata v rámci typeProperties části hello: **zdroj** a **podřízený**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-282">**Copy activity** has two subsections in hello typeProperties section: **source** and **sink**.</span></span> <span data-ttu-id="5e83d-283">V tématu [ÚLOŽIŠŤ dat](#data-stores) pro JSON ukázky, zobrazující jak jako zdroj a jímka úložiště toouse datové části v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-283">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span> 

### <a name="sample-copy-pipeline"></a><span data-ttu-id="5e83d-284">Ukázkový kanál kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-284">Sample copy pipeline</span></span>
<span data-ttu-id="5e83d-285">V hello následující ukázkový kanál služby, je jedna aktivita typu **kopie** v hello **aktivity** části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-285">In hello following sample pipeline, there is one activity of type **Copy** in hello **activities** section.</span></span> <span data-ttu-id="5e83d-286">V této ukázce hello [aktivity kopírování](data-factory-data-movement-activities.md) zkopíruje data z Azure Blob storage tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="5e83d-286">In this sample, hello [Copy activity](data-factory-data-movement-activities.md) copies data from an Azure Blob storage tooan Azure SQL database.</span></span> 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
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

<span data-ttu-id="5e83d-287">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="5e83d-287">Note hello following points:</span></span>

* <span data-ttu-id="5e83d-288">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span>
* <span data-ttu-id="5e83d-289">Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-289">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span>
* <span data-ttu-id="5e83d-290">V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-290">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span>

<span data-ttu-id="5e83d-291">V tématu [ÚLOŽIŠŤ dat](#data-stores) pro JSON ukázky, zobrazující jak jako zdroj a jímka úložiště toouse datové části v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-291">See [DATA STORES](#data-stores) section in this article for JSON samples that show how toouse a data store as a source and/or sink.</span></span>    

<span data-ttu-id="5e83d-292">Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="5e83d-292">For a complete walkthrough of creating this pipeline, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="sample-transformation-pipeline"></a><span data-ttu-id="5e83d-293">Ukázkový kanál transformace</span><span class="sxs-lookup"><span data-stu-id="5e83d-293">Sample transformation pipeline</span></span>
<span data-ttu-id="5e83d-294">V hello následující ukázkový kanál služby, je jedna aktivita typu **HDInsightHive** v hello **aktivity** části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-294">In hello following sample pipeline, there is one activity of type **HDInsightHive** in hello **activities** section.</span></span> <span data-ttu-id="5e83d-295">V této ukázce hello [aktivitu HDInsight Hive](data-factory-hive-activity.md) transformuje data z úložiště objektů Blob v Azure tak, že spustíte soubor skriptu Hive v clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="5e83d-295">In this sample, hello [HDInsight Hive activity](data-factory-hive-activity.md) transforms data from an Azure Blob storage by running a Hive script file on an Azure HDInsight Hadoop cluster.</span></span> 

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

<span data-ttu-id="5e83d-296">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="5e83d-296">Note hello following points:</span></span> 

* <span data-ttu-id="5e83d-297">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-297">In hello activities section, there is only one activity whose **type** is set too**HDInsightHive**.</span></span>
* <span data-ttu-id="5e83d-298">soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **AzureStorageLinkedService**) a v  **skript** složky v kontejneru hello **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-298">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>
* <span data-ttu-id="5e83d-299">Hello **definuje** část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span><span class="sxs-lookup"><span data-stu-id="5e83d-299">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).</span></span>

<span data-ttu-id="5e83d-300">V tématu [aktivit TRANSFORMACE dat](#data-transformation-activities) v tomto článku pro ukázky JSON, které definují aktivit transformace v datovém kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-300">See [DATA TRANSFORMATION ACTIVITIES](#data-transformation-activities) section in this article for JSON samples that define transformation activities in a pipeline.</span></span>

<span data-ttu-id="5e83d-301">Kompletní a podrobný postup vytváření tohoto kanálu, najdete v části [kurz: vytvoření vaší první dat tooprocess kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="5e83d-301">For a complete walkthrough of creating this pipeline, see [Tutorial: Build your first pipeline tooprocess data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="linked-service"></a><span data-ttu-id="5e83d-302">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-302">Linked service</span></span>
<span data-ttu-id="5e83d-303">Hello základní strukturu pro definici propojené služby je následující:</span><span class="sxs-lookup"><span data-stu-id="5e83d-303">hello high-level structure for a linked service definition is as follows:</span></span>

```json
{
    "name": "<name of hello linked service>",
    "properties": {
        "type": "<type of hello linked service>",
        "typeProperties": {
        }
    }
}
```

<span data-ttu-id="5e83d-304">Následující tabulky popisují hello vlastnosti v rámci aktivity hello definici JSON:</span><span class="sxs-lookup"><span data-stu-id="5e83d-304">Following table describe hello properties within hello activity JSON definition:</span></span>

| <span data-ttu-id="5e83d-305">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-305">Property</span></span> | <span data-ttu-id="5e83d-306">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-306">Description</span></span> | <span data-ttu-id="5e83d-307">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-307">Required</span></span> |
| -------- | ----------- | -------- | 
| <span data-ttu-id="5e83d-308">jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-308">name</span></span> | <span data-ttu-id="5e83d-309">Název hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-309">Name of hello linked service.</span></span> | <span data-ttu-id="5e83d-310">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-310">Yes</span></span> | 
| <span data-ttu-id="5e83d-311">vlastnosti – typ</span><span class="sxs-lookup"><span data-stu-id="5e83d-311">properties - type</span></span> | <span data-ttu-id="5e83d-312">Typ hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-312">Type of hello linked service.</span></span> <span data-ttu-id="5e83d-313">Příklad: úložiště Azure, Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5e83d-313">For example: Azure Storage, Azure SQL Database.</span></span> |
| <span data-ttu-id="5e83d-314">typeProperties</span><span class="sxs-lookup"><span data-stu-id="5e83d-314">typeProperties</span></span> | <span data-ttu-id="5e83d-315">Hello rámci typeProperties oddíl obsahuje prvky, které se liší u každé úložiště dat nebo výpočetní prostředí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-315">hello typeProperties section has elements that are different for each data store or compute environment.</span></span> <span data-ttu-id="5e83d-316">V tématu [úložišť dat](#datastores) části pro všechna data hello ukládání propojené služby a [výpočetní prostředí](#compute-environments) pro všechny hello výpočetní propojené služby</span><span class="sxs-lookup"><span data-stu-id="5e83d-316">See [data stores](#datastores) section for all hello data store linked services and [compute environments](#compute-environments) for all hello compute linked services</span></span> |   

## <a name="dataset"></a><span data-ttu-id="5e83d-317">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-317">Dataset</span></span> 
<span data-ttu-id="5e83d-318">Datové sady v Azure Data Factory je definován následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5e83d-318">A dataset in Azure Data Factory is defined as follows:</span></span>

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

<span data-ttu-id="5e83d-319">Hello následující tabulka popisuje vlastnosti v hello výše JSON:</span><span class="sxs-lookup"><span data-stu-id="5e83d-319">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="5e83d-320">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-320">Property</span></span> | <span data-ttu-id="5e83d-321">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-321">Description</span></span> | <span data-ttu-id="5e83d-322">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-322">Required</span></span> | <span data-ttu-id="5e83d-323">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5e83d-323">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-324">jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-324">name</span></span> | <span data-ttu-id="5e83d-325">Název datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-325">Name of hello dataset.</span></span> <span data-ttu-id="5e83d-326">V tématu [Azure Data Factory - pravidla po pojmenování](data-factory-naming-rules.md) pravidla pojmenování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-326">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="5e83d-327">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-327">Yes</span></span> |<span data-ttu-id="5e83d-328">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-328">NA</span></span> |
| <span data-ttu-id="5e83d-329">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-329">type</span></span> | <span data-ttu-id="5e83d-330">Typ hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-330">Type of hello dataset.</span></span> <span data-ttu-id="5e83d-331">Zadejte jeden z typů hello podporovaných službou Azure Data Factory (například: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="5e83d-331">Specify one of hello types supported by Azure Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <span data-ttu-id="5e83d-332">V tématu [ÚLOŽIŠŤ dat](#data-stores) části pro všechny hello datová úložiště a datové sady typy podporované službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-332">See [DATA STORES](#data-stores) section for all hello data stores and dataset types supported by Data Factory.</span></span> | 
| <span data-ttu-id="5e83d-333">Struktura</span><span class="sxs-lookup"><span data-stu-id="5e83d-333">structure</span></span> | <span data-ttu-id="5e83d-334">Schéma hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-334">Schema of hello dataset.</span></span> <span data-ttu-id="5e83d-335">Obsahuje sloupce, jejich typy, atd.</span><span class="sxs-lookup"><span data-stu-id="5e83d-335">It contains columns, their types, etc.</span></span> | <span data-ttu-id="5e83d-336">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-336">No</span></span> |<span data-ttu-id="5e83d-337">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-337">NA</span></span> |
| <span data-ttu-id="5e83d-338">typeProperties</span><span class="sxs-lookup"><span data-stu-id="5e83d-338">typeProperties</span></span> | <span data-ttu-id="5e83d-339">Vlastnosti odpovídající toohello vybraný typ.</span><span class="sxs-lookup"><span data-stu-id="5e83d-339">Properties corresponding toohello selected type.</span></span> <span data-ttu-id="5e83d-340">V tématu [ÚLOŽIŠŤ dat](#data-stores) části Podporované typy a jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-340">See [DATA STORES](#data-stores) section for supported types and their properties.</span></span> |<span data-ttu-id="5e83d-341">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-341">Yes</span></span> |<span data-ttu-id="5e83d-342">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-342">NA</span></span> |
| <span data-ttu-id="5e83d-343">external</span><span class="sxs-lookup"><span data-stu-id="5e83d-343">external</span></span> | <span data-ttu-id="5e83d-344">Logická hodnota příznak toospecify, zda datové sady je explicitně produkovaný kanálu objekt pro vytváření dat nebo ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-344">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> |<span data-ttu-id="5e83d-345">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-345">No</span></span> |<span data-ttu-id="5e83d-346">False</span><span class="sxs-lookup"><span data-stu-id="5e83d-346">false</span></span> |
| <span data-ttu-id="5e83d-347">dostupnosti</span><span class="sxs-lookup"><span data-stu-id="5e83d-347">availability</span></span> | <span data-ttu-id="5e83d-348">Definuje hello zpracování okno nebo hello řezů model pro produkční hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-348">Defines hello processing window or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="5e83d-349">Podrobnosti pro datovou sadu hello řezů modelu najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-349">For details on hello dataset slicing model, see [Scheduling and Execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="5e83d-350">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-350">Yes</span></span> |<span data-ttu-id="5e83d-351">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-351">NA</span></span> |
| <span data-ttu-id="5e83d-352">policy</span><span class="sxs-lookup"><span data-stu-id="5e83d-352">policy</span></span> |<span data-ttu-id="5e83d-353">Definuje kritéria hello nebo hello podmínku, která musíte splnit řezy hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-353">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="5e83d-354">Podrobnosti najdete v tématu [datovou sadu zásad](#Policy) části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-354">For details, see [Dataset Policy](#Policy) section.</span></span> |<span data-ttu-id="5e83d-355">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-355">No</span></span> |<span data-ttu-id="5e83d-356">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-356">NA</span></span> |

<span data-ttu-id="5e83d-357">Každý sloupec v hello **struktura** část obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5e83d-357">Each column in hello **structure** section contains hello following properties:</span></span>

| <span data-ttu-id="5e83d-358">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-358">Property</span></span> | <span data-ttu-id="5e83d-359">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-359">Description</span></span> | <span data-ttu-id="5e83d-360">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-360">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-361">jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-361">name</span></span> |<span data-ttu-id="5e83d-362">Název sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-362">Name of hello column.</span></span> |<span data-ttu-id="5e83d-363">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-363">Yes</span></span> |
| <span data-ttu-id="5e83d-364">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-364">type</span></span> |<span data-ttu-id="5e83d-365">Datový typ sloupce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-365">Data type of hello column.</span></span>  |<span data-ttu-id="5e83d-366">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-366">No</span></span> |
| <span data-ttu-id="5e83d-367">Jazyková verze</span><span class="sxs-lookup"><span data-stu-id="5e83d-367">culture</span></span> |<span data-ttu-id="5e83d-368">.NET na základě jazykovou verzi toobe používají, pokud je zadaný typ a je typ formátu .NET `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-368">.NET based culture toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="5e83d-369">Výchozí hodnota je `en-us`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-369">Default is `en-us`.</span></span> |<span data-ttu-id="5e83d-370">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-370">No</span></span> |
| <span data-ttu-id="5e83d-371">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-371">format</span></span> |<span data-ttu-id="5e83d-372">Formátování řetězce toobe používají, pokud je zadaný typ a je typ formátu .NET `Datetime` nebo `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-372">Format string toobe used when type is specified and is .NET type `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="5e83d-373">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-373">No</span></span> |

<span data-ttu-id="5e83d-374">V následující ukázka hello, hello datová sada má tři sloupce `slicetimestamp`, `projectname`, a `pageviews` a jsou typu: řetězec, řetězec a desetinných v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-374">In hello following example, hello dataset has three columns `slicetimestamp`, `projectname`, and `pageviews` and they are of type: String, String, and Decimal respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="5e83d-375">Hello následující tabulka popisuje vlastnosti, které můžete použít v hello **dostupnosti** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-375">hello following table describes properties you can use in hello **availability** section:</span></span>

| <span data-ttu-id="5e83d-376">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-376">Property</span></span> | <span data-ttu-id="5e83d-377">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-377">Description</span></span> | <span data-ttu-id="5e83d-378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-378">Required</span></span> | <span data-ttu-id="5e83d-379">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5e83d-379">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-380">frequency</span><span class="sxs-lookup"><span data-stu-id="5e83d-380">frequency</span></span> |<span data-ttu-id="5e83d-381">Určuje časovou jednotku hello k produkci řez datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-381">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="5e83d-382"><b>Podporované frekvence</b>: minutu, hodinu, den, týden, měsíc</span><span class="sxs-lookup"><span data-stu-id="5e83d-382"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="5e83d-383">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-383">Yes</span></span> |<span data-ttu-id="5e83d-384">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-384">NA</span></span> |
| <span data-ttu-id="5e83d-385">interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-385">interval</span></span> |<span data-ttu-id="5e83d-386">Určuje multiplikátor pro četnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-386">Specifies a multiplier for frequency</span></span><br/><br/><span data-ttu-id="5e83d-387">"Frekvence x interval" Určuje, jak často hello se vytvářejí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-387">”Frequency x interval” determines how often hello slice is produced.</span></span><br/><br/><span data-ttu-id="5e83d-388">Pokud třeba hello datovou sadu toobe rozříznut hodinu, nastavíte <b>frekvence</b> příliš<b>hodinu</b>, a <b>interval</b> příliš<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="5e83d-388">If you need hello dataset toobe sliced on an hourly basis, you set <b>Frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="5e83d-389"><b>Poznámka:</b>: Pokud zadáte četnost jako minutu, doporučujeme, abyste nastavili hello interval toono méně než 15</span><span class="sxs-lookup"><span data-stu-id="5e83d-389"><b>Note</b>: If you specify Frequency as Minute, we recommend that you set hello interval toono less than 15</span></span> |<span data-ttu-id="5e83d-390">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-390">Yes</span></span> |<span data-ttu-id="5e83d-391">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-391">NA</span></span> |
| <span data-ttu-id="5e83d-392">Styl</span><span class="sxs-lookup"><span data-stu-id="5e83d-392">style</span></span> |<span data-ttu-id="5e83d-393">Určuje, zda by měl být na hello počáteční nebo koncové intervalu hello předložen hello řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-393">Specifies whether hello slice should be produced at hello start/end of hello interval.</span></span><ul><li><span data-ttu-id="5e83d-394">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="5e83d-394">StartOfInterval</span></span></li><li><span data-ttu-id="5e83d-395">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="5e83d-395">EndOfInterval</span></span></li></ul><br/><br/><span data-ttu-id="5e83d-396">Pokud je nastavena frekvence tooMonth a je nastaven styl tooEndOfInterval, hello se vytvářejí na hello poslední den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="5e83d-396">If Frequency is set tooMonth and style is set tooEndOfInterval, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="5e83d-397">Pokud je styl hello nastavená tooStartOfInterval, hello se vytvářejí na hello první den v měsíci.</span><span class="sxs-lookup"><span data-stu-id="5e83d-397">If hello style is set tooStartOfInterval, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="5e83d-398">Pokud je nastavena frekvence tooDay a je nastaven styl tooEndOfInterval, hello se vytvářejí v hello poslední hodiny dne hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-398">If Frequency is set tooDay and style is set tooEndOfInterval, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="5e83d-399">Pokud je nastavena frekvence tooHour a je nastaven styl tooEndOfInterval, hello se vytvářejí na konci hello hello hodina.</span><span class="sxs-lookup"><span data-stu-id="5e83d-399">If Frequency is set tooHour and style is set tooEndOfInterval, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="5e83d-400">Například pro řez dobu 13: 00 – 14: 00, hello se vytvářejí na 14: 00.</span><span class="sxs-lookup"><span data-stu-id="5e83d-400">For example, for a slice for 1 PM – 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="5e83d-401">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-401">No</span></span> |<span data-ttu-id="5e83d-402">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="5e83d-402">EndOfInterval</span></span> |
| <span data-ttu-id="5e83d-403">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="5e83d-403">anchorDateTime</span></span> |<span data-ttu-id="5e83d-404">Definuje hello absolutní pozici v čase, které používají scheduler toocompute datovou sadu řez hranic.</span><span class="sxs-lookup"><span data-stu-id="5e83d-404">Defines hello absolute position in time used by scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="5e83d-405"><b>Poznámka:</b>: Pokud má hello AnchorDateTime částí data, která jsou podrobnější než frekvence hello pak hello podrobnější části jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-405"><b>Note</b>: If hello AnchorDateTime has date parts that are more granular than hello frequency then hello more granular parts are ignored.</span></span> <br/><br/><span data-ttu-id="5e83d-406">Například, pokud hello <b>interval</b> je <b>každou hodinu</b> (frekvence: hodin a interval: 1) a hello <b>AnchorDateTime</b> obsahuje <b>minuty a sekundy</b>pak hello <b>minuty a sekundy</b> částí hello AnchorDateTime jsou ignorovány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-406">For example, if hello <b>interval</b> is <b>hourly</b> (frequency: hour and interval: 1) and hello <b>AnchorDateTime</b> contains <b>minutes and seconds</b> then hello <b>minutes and seconds</b> parts of hello AnchorDateTime are ignored.</span></span> |<span data-ttu-id="5e83d-407">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-407">No</span></span> |<span data-ttu-id="5e83d-408">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="5e83d-408">01/01/0001</span></span> |
| <span data-ttu-id="5e83d-409">Posun</span><span class="sxs-lookup"><span data-stu-id="5e83d-409">offset</span></span> |<span data-ttu-id="5e83d-410">Časový interval, ve které hello začátku a konci všech řezech datovou sadu posunuty.</span><span class="sxs-lookup"><span data-stu-id="5e83d-410">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="5e83d-411"><b>Poznámka:</b>: Pokud jsou zadané anchorDateTime i posun, výsledkem hello je hello kombinaci shift.</span><span class="sxs-lookup"><span data-stu-id="5e83d-411"><b>Note</b>: If both anchorDateTime and offset are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="5e83d-412">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-412">No</span></span> |<span data-ttu-id="5e83d-413">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-413">NA</span></span> |

<span data-ttu-id="5e83d-414">Hello následující části dostupnosti Určuje, že hello výstupní datové sady je buď vytvořené každou hodinu (nebo) vstupní datovou sadu každou hodinu je k dispozici:</span><span class="sxs-lookup"><span data-stu-id="5e83d-414">hello following availability section specifies that hello output dataset is either produced hourly (or) input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="5e83d-415">Hello **zásad** oddíl v definici datové sady definuje kritéria hello nebo hello podmínku, která hello řezy datovou sadu musí splnit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-415">hello **policy** section in dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

| <span data-ttu-id="5e83d-416">Název zásady</span><span class="sxs-lookup"><span data-stu-id="5e83d-416">Policy Name</span></span> | <span data-ttu-id="5e83d-417">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-417">Description</span></span> | <span data-ttu-id="5e83d-418">Použít příliš</span><span class="sxs-lookup"><span data-stu-id="5e83d-418">Applied too</span></span>| <span data-ttu-id="5e83d-419">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-419">Required</span></span> | <span data-ttu-id="5e83d-420">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5e83d-420">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="5e83d-421">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="5e83d-421">minimumSizeMB</span></span> |<span data-ttu-id="5e83d-422">Ověří, zda hello data ve **objektů blob v Azure** hello splňuje požadavky na minimální velikost (v megabajtech).</span><span class="sxs-lookup"><span data-stu-id="5e83d-422">Validates that hello data in an **Azure blob** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="5e83d-423">Azure Blob</span><span class="sxs-lookup"><span data-stu-id="5e83d-423">Azure Blob</span></span> |<span data-ttu-id="5e83d-424">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-424">No</span></span> |<span data-ttu-id="5e83d-425">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-425">NA</span></span> |
| <span data-ttu-id="5e83d-426">minimumRows</span><span class="sxs-lookup"><span data-stu-id="5e83d-426">minimumRows</span></span> |<span data-ttu-id="5e83d-427">Ověří, zda hello data ve **Azure SQL database** nebo **tabulky Azure** obsahuje hello minimální počet řádků.</span><span class="sxs-lookup"><span data-stu-id="5e83d-427">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="5e83d-428">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5e83d-428">Azure SQL Database</span></span></li><li><span data-ttu-id="5e83d-429">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="5e83d-429">Azure Table</span></span></li></ul> |<span data-ttu-id="5e83d-430">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-430">No</span></span> |<span data-ttu-id="5e83d-431">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="5e83d-431">NA</span></span> |

<span data-ttu-id="5e83d-432">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="5e83d-432">**Example:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="5e83d-433">Není-li datovou sadu se vytváří pomocí Azure Data Factory, by měl být označen jako **externí**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-433">Unless a dataset is being produced by Azure Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="5e83d-434">Toto nastavení obecně platí toohello vstupy první aktivitu v kanálu, pokud aktivita nebo řetězení kanálu je používán.</span><span class="sxs-lookup"><span data-stu-id="5e83d-434">This setting generally applies toohello inputs of first activity in a pipeline unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="5e83d-435">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="5e83d-435">Name</span></span> | <span data-ttu-id="5e83d-436">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-436">Description</span></span> | <span data-ttu-id="5e83d-437">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-437">Required</span></span> | <span data-ttu-id="5e83d-438">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="5e83d-438">Default Value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-439">dataDelay</span><span class="sxs-lookup"><span data-stu-id="5e83d-439">dataDelay</span></span> |<span data-ttu-id="5e83d-440">Zkontrolujte hello čas toodelay na dostupnost hello hello externích dat pro danou řez hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-440">Time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="5e83d-441">Například pokud hello data nejsou k dispozici každou hodinu, hello kontrola toosee hello externích dat je k dispozici a hello odpovídající řez je připravený lze zpozdit pomocí dataDelay.</span><span class="sxs-lookup"><span data-stu-id="5e83d-441">For example, if hello data is available hourly, hello check toosee hello external data is available and hello corresponding slice is Ready can be delayed by using dataDelay.</span></span><br/><br/><span data-ttu-id="5e83d-442">Toohello platí pouze aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="5e83d-442">Only applies toohello present time.</span></span>  <span data-ttu-id="5e83d-443">Například pokud je 1:00 PM hned teď a tato hodnota je 10 minut, ověření hello se spustí: 10: 00.</span><span class="sxs-lookup"><span data-stu-id="5e83d-443">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="5e83d-444">Toto nastavení nemá vliv řezy v posledních hello (řezy s řez koncový čas + dataDelay < teď) jsou zpracovávány bez jakéhokoli zpoždění.</span><span class="sxs-lookup"><span data-stu-id="5e83d-444">This setting does not affect slices in hello past (slices with Slice End Time + dataDelay < Now) are processed without any delay.</span></span><br/><br/><span data-ttu-id="5e83d-445">Čas větší než 23:59 toospecified pomocí hello je nutné dobu `day.hours:minutes:seconds` formátu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-445">Time greater than 23:59 hours need toospecified using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="5e83d-446">Například toospecify 24 hodin, nepoužívejte 24:00:00; Místo toho použijte 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="5e83d-446">For example, toospecify 24 hours, don't use 24:00:00; instead, use 1.00:00:00.</span></span> <span data-ttu-id="5e83d-447">Pokud používáte 24:00:00, bude považován za 24 dní (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="5e83d-447">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="5e83d-448">1 den a 4 hodiny zadejte 1:04:00:00.</span><span class="sxs-lookup"><span data-stu-id="5e83d-448">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="5e83d-449">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-449">No</span></span> |<span data-ttu-id="5e83d-450">0</span><span class="sxs-lookup"><span data-stu-id="5e83d-450">0</span></span> |
| <span data-ttu-id="5e83d-451">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="5e83d-451">retryInterval</span></span> |<span data-ttu-id="5e83d-452">Doba čekání Hello mezi selhání a hello další opakujte pokus.</span><span class="sxs-lookup"><span data-stu-id="5e83d-452">hello wait time between a failure and hello next retry attempt.</span></span> <span data-ttu-id="5e83d-453">Pokud zkuste to nezdaří, zkuste další hello je po retryInterval.</span><span class="sxs-lookup"><span data-stu-id="5e83d-453">If a try fails, hello next try is after retryInterval.</span></span> <br/><br/><span data-ttu-id="5e83d-454">Pokud je 1:00 PM nyní, můžeme začít hello prvního pokusu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-454">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="5e83d-455">Pokud hello trvání toocomplete hello první ověření kontrola je 1 minuta a hello operace se nezdařila, hello další pokus proběhne v 1:00 + 1 min (doba trvání) + 1 min (interval opakování) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="5e83d-455">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1 min (duration) + 1 min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="5e83d-456">Řezy v posledních hello neexistuje žádné zpoždění není.</span><span class="sxs-lookup"><span data-stu-id="5e83d-456">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="5e83d-457">Hello opakování dojde okamžitě.</span><span class="sxs-lookup"><span data-stu-id="5e83d-457">hello retry happens immediately.</span></span> |<span data-ttu-id="5e83d-458">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-458">No</span></span> |<span data-ttu-id="5e83d-459">00:01:00 (1 min)</span><span class="sxs-lookup"><span data-stu-id="5e83d-459">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="5e83d-460">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-460">retryTimeout</span></span> |<span data-ttu-id="5e83d-461">Hello časový limit pro jednotlivé pokusy o opakování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-461">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="5e83d-462">Pokud je tato vlastnost nastavená too10 minut hello toobe potřeby ověření dokončeny v rámci 10 minut.</span><span class="sxs-lookup"><span data-stu-id="5e83d-462">If this property is set too10 minutes, hello validation needs toobe completed within 10 minutes.</span></span> <span data-ttu-id="5e83d-463">Pokud trvá déle než 10 minut tooperform hello ověření, opakujte hello časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-463">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="5e83d-464">Pokud všechny pokusy o ověření hello časového limitu, hello řez je označena jako TimedOut.</span><span class="sxs-lookup"><span data-stu-id="5e83d-464">If all attempts for hello validation times out, hello slice is marked as TimedOut.</span></span> |<span data-ttu-id="5e83d-465">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-465">No</span></span> |<span data-ttu-id="5e83d-466">00:10:00 (10 minut)</span><span class="sxs-lookup"><span data-stu-id="5e83d-466">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="5e83d-467">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="5e83d-467">maximumRetry</span></span> |<span data-ttu-id="5e83d-468">Počet opakování toocheck hello dostupnost externích dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-468">Number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="5e83d-469">Hello povolená, maximální hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="5e83d-469">hello allowed maximum value is 10.</span></span> |<span data-ttu-id="5e83d-470">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-470">No</span></span> |<span data-ttu-id="5e83d-471">3</span><span class="sxs-lookup"><span data-stu-id="5e83d-471">3</span></span> |


## <a name="data-stores"></a><span data-ttu-id="5e83d-472">DATOVÁ ÚLOŽIŠTĚ</span><span class="sxs-lookup"><span data-stu-id="5e83d-472">DATA STORES</span></span>
<span data-ttu-id="5e83d-473">Hello [propojená služba](#linked-service) části zadat popis pro elementy JSON, které jsou uvedeny běžné tooall typy propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-473">hello [Linked service](#linked-service) section provided descriptions for JSON elements that are common tooall types of linked services.</span></span> <span data-ttu-id="5e83d-474">Tato část obsahuje podrobnosti o JSON prvky, které jsou specifické tooeach úložišti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-474">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="5e83d-475">Hello [datovou sadu](#dataset) části zadat popis pro elementy JSON, které jsou uvedeny běžné typy tooall datových sad.</span><span class="sxs-lookup"><span data-stu-id="5e83d-475">hello [Dataset](#dataset) section provided descriptions for JSON elements that are common tooall types of datasets.</span></span> <span data-ttu-id="5e83d-476">Tato část obsahuje podrobnosti o JSON prvky, které jsou specifické tooeach úložišti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-476">This section provides details about JSON elements that are specific tooeach data store.</span></span>

<span data-ttu-id="5e83d-477">Hello [aktivity](#activity) části zadat popis pro elementy JSON, které jsou uvedeny běžné typy tooall aktivit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-477">hello [Activity](#activity) section provided descriptions for JSON elements that are common tooall types of activities.</span></span> <span data-ttu-id="5e83d-478">Tato část obsahuje podrobnosti o JSON prvky, které jsou konkrétní tooeach úložiště dat, pokud se používá jako zdroj/jímka v aktivitě kopírování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-478">This section provides details about JSON elements that are specific tooeach data store when it is used as a source/sink in a copy activity.</span></span>  

<span data-ttu-id="5e83d-479">Klikněte na odkaz hello hello úložiště mají zájem o toosee hello JSON schémata pro propojenou službu, datové sady a hello zdroj/jímka pro aktivitu kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-479">Click hello link for hello store you are interested in toosee hello JSON schemas for linked service, dataset, and hello source/sink for hello copy activity.</span></span>

| <span data-ttu-id="5e83d-480">Kategorie</span><span class="sxs-lookup"><span data-stu-id="5e83d-480">Category</span></span> | <span data-ttu-id="5e83d-481">Úložiště dat</span><span class="sxs-lookup"><span data-stu-id="5e83d-481">Data store</span></span> 
|:--- |:--- |
| <span data-ttu-id="5e83d-482">**Azure**</span><span class="sxs-lookup"><span data-stu-id="5e83d-482">**Azure**</span></span> |[<span data-ttu-id="5e83d-483">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-483">Azure Blob storage</span></span>](#azure-blob-storage) |
| &nbsp; |[<span data-ttu-id="5e83d-484">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e83d-484">Azure Data Lake Store</span></span>](#azure-datalake-store) |
| &nbsp; |[<span data-ttu-id="5e83d-485">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5e83d-485">Azure Cosmos DB</span></span>](#azure-cosmos-db) |
| &nbsp; |[<span data-ttu-id="5e83d-486">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5e83d-486">Azure SQL Database</span></span>](#azure-sql-database) |
| &nbsp; |[<span data-ttu-id="5e83d-487">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-487">Azure SQL Data Warehouse</span></span>](#azure-sql-data-warehouse) |
| &nbsp; |[<span data-ttu-id="5e83d-488">Azure Search</span><span class="sxs-lookup"><span data-stu-id="5e83d-488">Azure Search</span></span>](#azure-search) |
| &nbsp; |[<span data-ttu-id="5e83d-489">Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-489">Azure Table storage</span></span>](#azure-table-storage) |
| <span data-ttu-id="5e83d-490">**Databáze**</span><span class="sxs-lookup"><span data-stu-id="5e83d-490">**Databases**</span></span> |[<span data-ttu-id="5e83d-491">Amazon Redshift</span><span class="sxs-lookup"><span data-stu-id="5e83d-491">Amazon Redshift</span></span>](#amazon-redshift) |
| &nbsp; |[<span data-ttu-id="5e83d-492">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="5e83d-492">IBM DB2</span></span>](#ibm-db2) |
| &nbsp; |[<span data-ttu-id="5e83d-493">MySQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-493">MySQL</span></span>](#mysql) |
| &nbsp; |[<span data-ttu-id="5e83d-494">Oracle</span><span class="sxs-lookup"><span data-stu-id="5e83d-494">Oracle</span></span>](#oracle) |
| &nbsp; |[<span data-ttu-id="5e83d-495">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-495">PostgreSQL</span></span>](#postgresql) |
| &nbsp; |[<span data-ttu-id="5e83d-496">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-496">SAP Business Warehouse</span></span>](#sap-business-warehouse) |
| &nbsp; |[<span data-ttu-id="5e83d-497">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="5e83d-497">SAP HANA</span></span>](#sap-hana) |
| &nbsp; |[<span data-ttu-id="5e83d-498">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e83d-498">SQL Server</span></span>](#sql-server) |
| &nbsp; |[<span data-ttu-id="5e83d-499">Sybase</span><span class="sxs-lookup"><span data-stu-id="5e83d-499">Sybase</span></span>](#sybase) |
| &nbsp; |[<span data-ttu-id="5e83d-500">Teradata</span><span class="sxs-lookup"><span data-stu-id="5e83d-500">Teradata</span></span>](#teradata) |
| <span data-ttu-id="5e83d-501">**NoSQL**</span><span class="sxs-lookup"><span data-stu-id="5e83d-501">**NoSQL**</span></span> |[<span data-ttu-id="5e83d-502">Cassandra</span><span class="sxs-lookup"><span data-stu-id="5e83d-502">Cassandra</span></span>](#cassandra) |
| &nbsp; |[<span data-ttu-id="5e83d-503">MongoDB</span><span class="sxs-lookup"><span data-stu-id="5e83d-503">MongoDB</span></span>](#mongodb) |
| <span data-ttu-id="5e83d-504">**File**</span><span class="sxs-lookup"><span data-stu-id="5e83d-504">**File**</span></span> |[<span data-ttu-id="5e83d-505">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="5e83d-505">Amazon S3</span></span>](#amazon-s3) |
| &nbsp; |[<span data-ttu-id="5e83d-506">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="5e83d-506">File System</span></span>](#file-system) |
| &nbsp; |[<span data-ttu-id="5e83d-507">FTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-507">FTP</span></span>](#ftp) |
| &nbsp; |[<span data-ttu-id="5e83d-508">HDFS</span><span class="sxs-lookup"><span data-stu-id="5e83d-508">HDFS</span></span>](#hdfs) |
| &nbsp; |[<span data-ttu-id="5e83d-509">SFTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-509">SFTP</span></span>](#sftp) |
| <span data-ttu-id="5e83d-510">**Ostatní**</span><span class="sxs-lookup"><span data-stu-id="5e83d-510">**Others**</span></span> |[<span data-ttu-id="5e83d-511">HTTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-511">HTTP</span></span>](#http) |
| &nbsp; |[<span data-ttu-id="5e83d-512">OData</span><span class="sxs-lookup"><span data-stu-id="5e83d-512">OData</span></span>](#odata) |
| &nbsp; |[<span data-ttu-id="5e83d-513">ODBC</span><span class="sxs-lookup"><span data-stu-id="5e83d-513">ODBC</span></span>](#odbc) |
| &nbsp; |[<span data-ttu-id="5e83d-514">Salesforce</span><span class="sxs-lookup"><span data-stu-id="5e83d-514">Salesforce</span></span>](#salesforce) |
| &nbsp; |[<span data-ttu-id="5e83d-515">Webové tabulky</span><span class="sxs-lookup"><span data-stu-id="5e83d-515">Web Table</span></span>](#web-table) |

## <a name="azure-blob-storage"></a><span data-ttu-id="5e83d-516">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-516">Azure Blob Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-517">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-517">Linked service</span></span>
<span data-ttu-id="5e83d-518">Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-518">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="5e83d-519">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-519">Azure Storage Linked Service</span></span>
<span data-ttu-id="5e83d-520">toolink tooa účet úložiště Azure datovou továrnu pomocí hello **klíč účtu**, vytvoření služby Azure Storage, propojené.</span><span class="sxs-lookup"><span data-stu-id="5e83d-520">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="5e83d-521">toodefine Azure Storage, propojené služby, sada hello **typ** hello propojené služby příliš**azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-521">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="5e83d-522">Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-522">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-523">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-523">Property</span></span> | <span data-ttu-id="5e83d-524">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-524">Description</span></span> | <span data-ttu-id="5e83d-525">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-525">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-526">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-526">connectionString</span></span> |<span data-ttu-id="5e83d-527">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e83d-527">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-528">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-528">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="5e83d-529">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-529">Example</span></span>  

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

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="5e83d-530">Propojená služba Azure Storage SAS</span><span class="sxs-lookup"><span data-stu-id="5e83d-530">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="5e83d-531">Hello SAS úložiště Azure, propojené služby umožňuje toolink služby Azure data factory tooan účet úložiště Azure pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="5e83d-531">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="5e83d-532">Poskytuje objekt pro vytváření dat hello přístup omezený nebo časově vázaných tooall nebo konkrétní prostředky (kontejner nebo objektů blob) v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-532">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="5e83d-533">toolink tooa účet úložiště Azure datovou továrnu pomocí sdíleného přístupového podpisu vytvoření služby Azure úložiště SAS propojený.</span><span class="sxs-lookup"><span data-stu-id="5e83d-533">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="5e83d-534">toodefine Azure úložiště SAS propojená služba, sada hello **typ** hello propojené služby příliš**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-534">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="5e83d-535">Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-535">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="5e83d-536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-536">Property</span></span> | <span data-ttu-id="5e83d-537">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-537">Description</span></span> | <span data-ttu-id="5e83d-538">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-538">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-539">sasUri</span><span class="sxs-lookup"><span data-stu-id="5e83d-539">sasUri</span></span> |<span data-ttu-id="5e83d-540">Zadejte identifikátor URI podpis sdíleného přístupu toohello Azure Storage prostředky jako objekt blob, kontejneru nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-540">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="5e83d-541">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-541">Yes</span></span> |

##### <a name="example"></a><span data-ttu-id="5e83d-542">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-542">Example</span></span>

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

<span data-ttu-id="5e83d-543">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-543">For more information about these linked services, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-544">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-544">Dataset</span></span>
<span data-ttu-id="5e83d-545">toodefine datové sadě služby Azure Blob sadu hello **typ** sady dat hello příliš**AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-545">toodefine an Azure Blob dataset, set hello **type** of hello dataset too**AzureBlob**.</span></span> <span data-ttu-id="5e83d-546">Potom můžete určit následující konkrétní vlastnosti objektů Blob v Azure v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-546">Then, specify hello following Azure Blob specific properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-547">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-547">Property</span></span> | <span data-ttu-id="5e83d-548">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-548">Description</span></span> | <span data-ttu-id="5e83d-549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-549">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-550">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-550">folderPath</span></span> |<span data-ttu-id="5e83d-551">Cesta toohello kontejneru a složce v úložišti objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-551">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="5e83d-552">Příklad: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="5e83d-552">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="5e83d-553">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-553">Yes</span></span> |
| <span data-ttu-id="5e83d-554">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-554">fileName</span></span> |<span data-ttu-id="5e83d-555">Název objektu hello blob.</span><span class="sxs-lookup"><span data-stu-id="5e83d-555">Name of hello blob.</span></span> <span data-ttu-id="5e83d-556">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-556">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="5e83d-557">Pokud určíte název souboru, hello aktivitu (včetně kopie) funguje na hello konkrétní objekt Blob.</span><span class="sxs-lookup"><span data-stu-id="5e83d-557">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="5e83d-558">Pokud není zadán název souboru, zahrnuje kopírování všech objektů BLOB v hello folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-558">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="5e83d-559">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="5e83d-559">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="5e83d-560">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-560">No</span></span> |
| <span data-ttu-id="5e83d-561">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-561">partitionedBy</span></span> |<span data-ttu-id="5e83d-562">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="5e83d-562">partitionedBy is an optional property.</span></span> <span data-ttu-id="5e83d-563">Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-563">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="5e83d-564">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-564">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-565">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-565">No</span></span> |
| <span data-ttu-id="5e83d-566">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-566">format</span></span> | <span data-ttu-id="5e83d-567">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-567">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-568">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-568">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-569">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-569">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-570">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-570">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-571">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-571">No</span></span> |
| <span data-ttu-id="5e83d-572">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-572">compression</span></span> | <span data-ttu-id="5e83d-573">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-573">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-574">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-574">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-575">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-575">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-576">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-576">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-577">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-577">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-578">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-578">Example</span></span>

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


<span data-ttu-id="5e83d-579">Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-579">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#dataset-properties) article.</span></span>

### <a name="blobsource-in-copy-activity"></a><span data-ttu-id="5e83d-580">BlobSource v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-580">BlobSource in Copy Activity</span></span>
<span data-ttu-id="5e83d-581">Pokud jsou kopírování dat z Azure Blob Storage, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**BlobSource**a zadejte následující vlastnosti v hello ** zdroj ** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-581">If you are copying data from an Azure Blob Storage, set hello **source type** of hello copy activity too**BlobSource**, and specify following properties in hello **source **section:</span></span>

| <span data-ttu-id="5e83d-582">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-582">Property</span></span> | <span data-ttu-id="5e83d-583">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-583">Description</span></span> | <span data-ttu-id="5e83d-584">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-584">Allowed values</span></span> | <span data-ttu-id="5e83d-585">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-585">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-586">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-586">recursive</span></span> |<span data-ttu-id="5e83d-587">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-587">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-588">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="5e83d-588">True (default value), False</span></span> |<span data-ttu-id="5e83d-589">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-589">No</span></span> |

#### <a name="example-blobsource"></a><span data-ttu-id="5e83d-590">Příklad: BlobSource **</span><span class="sxs-lookup"><span data-stu-id="5e83d-590">Example: BlobSource**</span></span>
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
### <a name="blobsink-in-copy-activity"></a><span data-ttu-id="5e83d-591">BlobSink v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-591">BlobSink in Copy Activity</span></span>
<span data-ttu-id="5e83d-592">Pokud kopírujete data tooan Azure Blob Storage, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**BlobSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-592">If you are copying data tooan Azure Blob Storage, set hello **sink type** of hello copy activity too**BlobSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-593">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-593">Property</span></span> | <span data-ttu-id="5e83d-594">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-594">Description</span></span> | <span data-ttu-id="5e83d-595">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-595">Allowed values</span></span> | <span data-ttu-id="5e83d-596">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-596">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-597">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="5e83d-597">copyBehavior</span></span> |<span data-ttu-id="5e83d-598">Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-598">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="5e83d-599"><b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-599"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="5e83d-600">relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-600">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="5e83d-601"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello jsou v hello první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-601"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="5e83d-602">Hello zaměřením mít název automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-602">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="5e83d-603"><b>MergeFiles (výchozí):</b> slučuje všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-603"><b>MergeFiles (default):</b> merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="5e83d-604">Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="5e83d-604">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="5e83d-605">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-605">No</span></span> |

#### <a name="example-blobsink"></a><span data-ttu-id="5e83d-606">Příklad: BlobSink</span><span class="sxs-lookup"><span data-stu-id="5e83d-606">Example: BlobSink</span></span>

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

<span data-ttu-id="5e83d-607">Další informace najdete v tématu [konektor Azure Blob](data-factory-azure-blob-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-607">For more information, see [Azure Blob connector](data-factory-azure-blob-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-data-lake-store"></a><span data-ttu-id="5e83d-608">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="5e83d-608">Azure Data Lake Store</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-609">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-609">Linked service</span></span>
<span data-ttu-id="5e83d-610">toodefine služby Azure Data Lake Store propojené sady hello typ hello propojená služba příliš**AzureDataLakeStore**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-610">toodefine an Azure Data Lake Store linked service, set hello type of hello linked service too**AzureDataLakeStore**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-611">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-611">Property</span></span> | <span data-ttu-id="5e83d-612">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-612">Description</span></span> | <span data-ttu-id="5e83d-613">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-613">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-614">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-614">type</span></span> | <span data-ttu-id="5e83d-615">vlastnost typu Hello musí být nastavena na: **AzureDataLakeStore**</span><span class="sxs-lookup"><span data-stu-id="5e83d-615">hello type property must be set to: **AzureDataLakeStore**</span></span> | <span data-ttu-id="5e83d-616">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-616">Yes</span></span> |
| <span data-ttu-id="5e83d-617">dataLakeStoreUri</span><span class="sxs-lookup"><span data-stu-id="5e83d-617">dataLakeStoreUri</span></span> | <span data-ttu-id="5e83d-618">Zadejte informace o hello účtu Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e83d-618">Specify information about hello Azure Data Lake Store account.</span></span> <span data-ttu-id="5e83d-619">Je ve formátu hello: `https://[accountname].azuredatalakestore.net/webhdfs/v1` nebo `adl://[accountname].azuredatalakestore.net/`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-619">It is in hello following format: `https://[accountname].azuredatalakestore.net/webhdfs/v1` or `adl://[accountname].azuredatalakestore.net/`.</span></span> | <span data-ttu-id="5e83d-620">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-620">Yes</span></span> |
| <span data-ttu-id="5e83d-621">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="5e83d-621">subscriptionId</span></span> | <span data-ttu-id="5e83d-622">Předplatné Azure Id toowhich Data Lake Store patří.</span><span class="sxs-lookup"><span data-stu-id="5e83d-622">Azure subscription Id toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="5e83d-623">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="5e83d-623">Required for sink</span></span> |
| <span data-ttu-id="5e83d-624">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5e83d-624">resourceGroupName</span></span> | <span data-ttu-id="5e83d-625">Patří toowhich název skupiny prostředků Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e83d-625">Azure resource group name toowhich Data Lake Store belongs.</span></span> | <span data-ttu-id="5e83d-626">Vyžaduje se pro sink</span><span class="sxs-lookup"><span data-stu-id="5e83d-626">Required for sink</span></span> |
| <span data-ttu-id="5e83d-627">servicePrincipalId</span><span class="sxs-lookup"><span data-stu-id="5e83d-627">servicePrincipalId</span></span> | <span data-ttu-id="5e83d-628">Zadejte ID aplikace hello klienta.</span><span class="sxs-lookup"><span data-stu-id="5e83d-628">Specify hello application's client ID.</span></span> | <span data-ttu-id="5e83d-629">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="5e83d-629">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="5e83d-630">servicePrincipalKey</span><span class="sxs-lookup"><span data-stu-id="5e83d-630">servicePrincipalKey</span></span> | <span data-ttu-id="5e83d-631">Zadejte klíč aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-631">Specify hello application's key.</span></span> | <span data-ttu-id="5e83d-632">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="5e83d-632">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="5e83d-633">Klienta</span><span class="sxs-lookup"><span data-stu-id="5e83d-633">tenant</span></span> | <span data-ttu-id="5e83d-634">Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace.</span><span class="sxs-lookup"><span data-stu-id="5e83d-634">Specify hello tenant information (domain name or tenant ID) under which your application resides.</span></span> <span data-ttu-id="5e83d-635">Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-635">You can retrieve it by hovering hello mouse in hello top-right corner of hello Azure portal.</span></span> | <span data-ttu-id="5e83d-636">Ano (pro objekt zabezpečení ověřování služby)</span><span class="sxs-lookup"><span data-stu-id="5e83d-636">Yes (for service principal authentication)</span></span> |
| <span data-ttu-id="5e83d-637">Autorizace</span><span class="sxs-lookup"><span data-stu-id="5e83d-637">authorization</span></span> | <span data-ttu-id="5e83d-638">Klikněte na tlačítko **Authorize** tlačítka na hello **editoru služby Data Factory** a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky generovaný autorizace URL toothis vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-638">Click **Authorize** button in hello **Data Factory Editor** and enter your credential that assigns hello auto-generated authorization URL toothis property.</span></span> | <span data-ttu-id="5e83d-639">Ano (pro ověření přihlašovacích údajů uživatele)</span><span class="sxs-lookup"><span data-stu-id="5e83d-639">Yes (for user credential authentication)</span></span>|
| <span data-ttu-id="5e83d-640">ID relace</span><span class="sxs-lookup"><span data-stu-id="5e83d-640">sessionId</span></span> | <span data-ttu-id="5e83d-641">Id relace OAuth z autorizační relace, hello OAuth.</span><span class="sxs-lookup"><span data-stu-id="5e83d-641">OAuth session id from hello OAuth authorization session.</span></span> <span data-ttu-id="5e83d-642">Každé id relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="5e83d-642">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="5e83d-643">Toto nastavení se automaticky generuje při pomocí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-643">This setting is automatically generated when you use Data Factory Editor.</span></span> | <span data-ttu-id="5e83d-644">Ano (pro ověření přihlašovacích údajů uživatele)</span><span class="sxs-lookup"><span data-stu-id="5e83d-644">Yes (for user credential authentication)</span></span> |

#### <a name="example-using-service-principal-authentication"></a><span data-ttu-id="5e83d-645">Příklad: použití ověřování hlavní služby</span><span class="sxs-lookup"><span data-stu-id="5e83d-645">Example: using service principal authentication</span></span>
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

#### <a name="example-using-user-credential-authentication"></a><span data-ttu-id="5e83d-646">Příklad: použití ověřování pověření uživatele</span><span class="sxs-lookup"><span data-stu-id="5e83d-646">Example: using user credential authentication</span></span>
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

<span data-ttu-id="5e83d-647">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-647">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-648">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-648">Dataset</span></span>
<span data-ttu-id="5e83d-649">toodefine datové sadě služby Azure Data Lake Store sadu hello **typ** sady dat hello příliš**AzureDataLakeStore**a zadejte následující vlastnosti v hello hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-649">toodefine an Azure Data Lake Store dataset, set hello **type** of hello dataset too**AzureDataLakeStore**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-650">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-650">Property</span></span> | <span data-ttu-id="5e83d-651">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-651">Description</span></span> | <span data-ttu-id="5e83d-652">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-652">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-653">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-653">folderPath</span></span> |<span data-ttu-id="5e83d-654">Uložit cestu toohello kontejneru a složce v hello Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="5e83d-654">Path toohello container and folder in hello Azure Data Lake store.</span></span> |<span data-ttu-id="5e83d-655">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-655">Yes</span></span> |
| <span data-ttu-id="5e83d-656">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-656">fileName</span></span> |<span data-ttu-id="5e83d-657">Název souboru hello v hello Azure Data Lake store.</span><span class="sxs-lookup"><span data-stu-id="5e83d-657">Name of hello file in hello Azure Data Lake store.</span></span> <span data-ttu-id="5e83d-658">Název souboru je volitelné a velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-658">fileName is optional and case-sensitive.</span></span> <br/><br/><span data-ttu-id="5e83d-659">Pokud zadáte název souboru, na konkrétní soubor hello funguje hello aktivitu (včetně kopie).</span><span class="sxs-lookup"><span data-stu-id="5e83d-659">If you specify a filename, hello activity (including Copy) works on hello specific file.</span></span><br/><br/><span data-ttu-id="5e83d-660">Pokud není zadán název souboru, kopie zahrnuje všechny soubory v hello folderPath pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-660">When fileName is not specified, Copy includes all files in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="5e83d-661">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát: Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="5e83d-661">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="5e83d-662">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-662">No</span></span> |
| <span data-ttu-id="5e83d-663">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-663">partitionedBy</span></span> |<span data-ttu-id="5e83d-664">partitionedBy vlastnost je volitelná.</span><span class="sxs-lookup"><span data-stu-id="5e83d-664">partitionedBy is an optional property.</span></span> <span data-ttu-id="5e83d-665">Můžete ho toospecify dynamické folderPath a název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-665">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="5e83d-666">Například folderPath lze nastavit parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-666">For example, folderPath can be parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-667">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-667">No</span></span> |
| <span data-ttu-id="5e83d-668">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-668">format</span></span> | <span data-ttu-id="5e83d-669">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-669">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-670">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-670">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-671">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-671">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-672">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-672">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-673">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-673">No</span></span> |
| <span data-ttu-id="5e83d-674">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-674">compression</span></span> | <span data-ttu-id="5e83d-675">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-675">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-676">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-676">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-677">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-677">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-678">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-678">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-679">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-679">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-680">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-680">Example</span></span>
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

<span data-ttu-id="5e83d-681">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-681">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-data-lake-store-source-in-copy-activity"></a><span data-ttu-id="5e83d-682">Azure Data Lake Store zdroj v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-682">Azure Data Lake Store Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-683">Pokud jsou kopírování dat z Azure Data Lake Store, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**AzureDataLakeStoreSource**a zadejte následující vlastnosti v hello **zdroje**  části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-683">If you are copying data from an Azure Data Lake Store, set hello **source type** of hello copy activity too**AzureDataLakeStoreSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="5e83d-684">**AzureDataLakeStoreSource** podporuje následující vlastnosti hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-684">**AzureDataLakeStoreSource** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="5e83d-685">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-685">Property</span></span> | <span data-ttu-id="5e83d-686">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-686">Description</span></span> | <span data-ttu-id="5e83d-687">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-687">Allowed values</span></span> | <span data-ttu-id="5e83d-688">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-688">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-689">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-689">recursive</span></span> |<span data-ttu-id="5e83d-690">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-690">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-691">True (výchozí hodnota), False.</span><span class="sxs-lookup"><span data-stu-id="5e83d-691">True (default value), False</span></span> |<span data-ttu-id="5e83d-692">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-692">No</span></span> |

#### <a name="example-azuredatalakestoresource"></a><span data-ttu-id="5e83d-693">Příklad: AzureDataLakeStoreSource</span><span class="sxs-lookup"><span data-stu-id="5e83d-693">Example: AzureDataLakeStoreSource</span></span>

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

<span data-ttu-id="5e83d-694">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-694">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span>

### <a name="azure-data-lake-store-sink-in-copy-activity"></a><span data-ttu-id="5e83d-695">Podřízený Azure Data Lake Store v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-695">Azure Data Lake Store Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-696">Pokud kopírujete data tooan Azure Data Lake Store, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureDataLakeStoreSink**a zadejte následující vlastnosti v hello **podřízený**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-696">If you are copying data tooan Azure Data Lake Store, set hello **sink type** of hello copy activity too**AzureDataLakeStoreSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-697">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-697">Property</span></span> | <span data-ttu-id="5e83d-698">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-698">Description</span></span> | <span data-ttu-id="5e83d-699">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-699">Allowed values</span></span> | <span data-ttu-id="5e83d-700">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-700">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-701">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="5e83d-701">copyBehavior</span></span> |<span data-ttu-id="5e83d-702">Určuje chování kopie hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-702">Specifies hello copy behavior.</span></span> |<span data-ttu-id="5e83d-703"><b>PreserveHierarchy</b>: uchovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-703"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="5e83d-704">relativní cesta Hello zdrojové složky toosource souboru je identické toohello relativní cestu složky tootarget cílového souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-704">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="5e83d-705"><b>FlattenHierarchy</b>: všechny soubory ze zdrojové složky hello se vytvoří v hello první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-705"><b>FlattenHierarchy</b>: all files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="5e83d-706">Hello zaměřením jsou vytvořen s názvem automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-706">hello target files are created with auto generated name.</span></span><br/><br/><span data-ttu-id="5e83d-707"><b>MergeFiles</b>: sloučí všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-707"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="5e83d-708">Pokud je zadán hello název souboru nebo objekt Blob, název sloučené souboru hello by být zadaný název hello; jinak by automaticky generovaný soubor název.</span><span class="sxs-lookup"><span data-stu-id="5e83d-708">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="5e83d-709">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-709">No</span></span> |

#### <a name="example-azuredatalakestoresink"></a><span data-ttu-id="5e83d-710">Příklad: AzureDataLakeStoreSink</span><span class="sxs-lookup"><span data-stu-id="5e83d-710">Example: AzureDataLakeStoreSink</span></span>
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

<span data-ttu-id="5e83d-711">Další informace najdete v tématu [konektor Azure Data Lake Store](data-factory-azure-datalake-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-711">For more information, see [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-cosmos-db"></a><span data-ttu-id="5e83d-712">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5e83d-712">Azure Cosmos DB</span></span>  

### <a name="linked-service"></a><span data-ttu-id="5e83d-713">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-713">Linked service</span></span>
<span data-ttu-id="5e83d-714">toodefine Azure DB Cosmos propojená služba, sada hello **typ** hello propojené služby příliš**DocumentDb**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-714">toodefine an Azure Cosmos DB linked service, set hello **type** of hello linked service too**DocumentDb**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-715">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="5e83d-715">**Property**</span></span> | <span data-ttu-id="5e83d-716">**Popis**</span><span class="sxs-lookup"><span data-stu-id="5e83d-716">**Description**</span></span> | <span data-ttu-id="5e83d-717">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="5e83d-717">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-718">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-718">connectionString</span></span> |<span data-ttu-id="5e83d-719">Zadejte informace potřebné tooconnect tooAzure Cosmos DB databáze.</span><span class="sxs-lookup"><span data-stu-id="5e83d-719">Specify information needed tooconnect tooAzure Cosmos DB database.</span></span> |<span data-ttu-id="5e83d-720">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-720">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-721">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-721">Example</span></span>

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
<span data-ttu-id="5e83d-722">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-722">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-723">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-723">Dataset</span></span>
<span data-ttu-id="5e83d-724">toodefine datové sadě služby Azure Cosmos DB sady hello **typ** sady dat hello příliš**DocumentDbCollection**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-724">toodefine an Azure Cosmos DB dataset, set hello **type** of hello dataset too**DocumentDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-725">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="5e83d-725">**Property**</span></span> | <span data-ttu-id="5e83d-726">**Popis**</span><span class="sxs-lookup"><span data-stu-id="5e83d-726">**Description**</span></span> | <span data-ttu-id="5e83d-727">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="5e83d-727">**Required**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-728">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="5e83d-728">collectionName</span></span> |<span data-ttu-id="5e83d-729">Název hello kolekce Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e83d-729">Name of hello Azure Cosmos DB collection.</span></span> |<span data-ttu-id="5e83d-730">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-730">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-731">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-731">Example</span></span>

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
<span data-ttu-id="5e83d-732">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-732">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) article.</span></span>

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a><span data-ttu-id="5e83d-733">Zdroj kolekce Azure Cosmos DB v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-733">Azure Cosmos DB Collection Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-734">Pokud kopírujete data z databáze Cosmos Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**DocumentDbCollectionSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-734">If you are copying data from an Azure Cosmos DB, set hello **source type** of hello copy activity too**DocumentDbCollectionSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-735">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="5e83d-735">**Property**</span></span> | <span data-ttu-id="5e83d-736">**Popis**</span><span class="sxs-lookup"><span data-stu-id="5e83d-736">**Description**</span></span> | <span data-ttu-id="5e83d-737">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="5e83d-737">**Allowed values**</span></span> | <span data-ttu-id="5e83d-738">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="5e83d-738">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-739">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-739">query</span></span> |<span data-ttu-id="5e83d-740">Zadejte hello dotazu tooread data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-740">Specify hello query tooread data.</span></span> |<span data-ttu-id="5e83d-741">Řetězec nepodporuje Azure Cosmos DB dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-741">Query string supported by Azure Cosmos DB.</span></span> <br/><br/><span data-ttu-id="5e83d-742">Příklad:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span><span class="sxs-lookup"><span data-stu-id="5e83d-742">Example: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"`</span></span> |<span data-ttu-id="5e83d-743">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-743">No</span></span> <br/><br/><span data-ttu-id="5e83d-744">Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select <columns defined in structure> from mycollection`</span><span class="sxs-lookup"><span data-stu-id="5e83d-744">If not specified, hello SQL statement that is executed: `select <columns defined in structure> from mycollection`</span></span> |
| <span data-ttu-id="5e83d-745">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="5e83d-745">nestingSeparator</span></span> |<span data-ttu-id="5e83d-746">Je vnořený tooindicate speciální znak, který hello dokumentu</span><span class="sxs-lookup"><span data-stu-id="5e83d-746">Special character tooindicate that hello document is nested</span></span> |<span data-ttu-id="5e83d-747">Libovolný znak.</span><span class="sxs-lookup"><span data-stu-id="5e83d-747">Any character.</span></span> <br/><br/><span data-ttu-id="5e83d-748">Azure Cosmos DB je úložiště typu NoSQL pro dokumenty JSON, kde jsou povoleny vnořené struktury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-748">Azure Cosmos DB is a NoSQL store for JSON documents, where nested structures are allowed.</span></span> <span data-ttu-id="5e83d-749">Azure Data Factory umožňuje hierarchie toodenote uživatele prostřednictvím nestingSeparator, což je "."</span><span class="sxs-lookup"><span data-stu-id="5e83d-749">Azure Data Factory enables user toodenote hierarchy via nestingSeparator, which is “.”</span></span> <span data-ttu-id="5e83d-750">v hello výše příklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-750">in hello above examples.</span></span> <span data-ttu-id="5e83d-751">S oddělovačem hello aktivity kopírování hello vygeneruje hello "Name" objekt s tří podřízených elementů první, střední a poslední, podle too"Name.First", "Name.Middle" a "Name.Last" v hello Definice tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-751">With hello separator, hello copy activity will generate hello “Name” object with three children elements First, Middle and Last, according too“Name.First”, “Name.Middle” and “Name.Last” in hello table definition.</span></span> |<span data-ttu-id="5e83d-752">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-752">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-753">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-753">Example</span></span>

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

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a><span data-ttu-id="5e83d-754">Azure Cosmos DB kolekce podřízený v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-754">Azure Cosmos DB Collection Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-755">Pokud kopírujete data tooAzure Cosmos DB, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**DocumentDbCollectionSink**a zadejte následující vlastnosti v hello **podřízený**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-755">If you are copying data tooAzure Cosmos DB, set hello **sink type** of hello copy activity too**DocumentDbCollectionSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-756">**Vlastnost**</span><span class="sxs-lookup"><span data-stu-id="5e83d-756">**Property**</span></span> | <span data-ttu-id="5e83d-757">**Popis**</span><span class="sxs-lookup"><span data-stu-id="5e83d-757">**Description**</span></span> | <span data-ttu-id="5e83d-758">**Povolené hodnoty**</span><span class="sxs-lookup"><span data-stu-id="5e83d-758">**Allowed values**</span></span> | <span data-ttu-id="5e83d-759">**Požadované**</span><span class="sxs-lookup"><span data-stu-id="5e83d-759">**Required**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-760">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="5e83d-760">nestingSeparator</span></span> |<span data-ttu-id="5e83d-761">Je potřeba speciálního znaku v hello zdrojový sloupec název tooindicate, který vnořených dokumentů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-761">A special character in hello source column name tooindicate that nested document is needed.</span></span> <br/><br/><span data-ttu-id="5e83d-762">Například výše: `Name.First` ve výstupu hello tabulky vytváří hello strukturu JSON v dokumentu Cosmos DB hello:</span><span class="sxs-lookup"><span data-stu-id="5e83d-762">For example above: `Name.First` in hello output table produces hello following JSON structure in hello Cosmos DB document:</span></span><br/><br/><span data-ttu-id="5e83d-763">"Název": {</span><span class="sxs-lookup"><span data-stu-id="5e83d-763">"Name": {</span></span><br/>    <span data-ttu-id="5e83d-764">"První": "Jan"</span><span class="sxs-lookup"><span data-stu-id="5e83d-764">"First": "John"</span></span><br/><span data-ttu-id="5e83d-765">},</span><span class="sxs-lookup"><span data-stu-id="5e83d-765">},</span></span> |<span data-ttu-id="5e83d-766">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="5e83d-766">Character that is used tooseparate nesting levels.</span></span><br/><br/><span data-ttu-id="5e83d-767">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="5e83d-767">Default value is `.` (dot).</span></span> |<span data-ttu-id="5e83d-768">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="5e83d-768">Character that is used tooseparate nesting levels.</span></span> <br/><br/><span data-ttu-id="5e83d-769">Výchozí hodnota je `.` (tečka).</span><span class="sxs-lookup"><span data-stu-id="5e83d-769">Default value is `.` (dot).</span></span> |
| <span data-ttu-id="5e83d-770">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-770">writeBatchSize</span></span> |<span data-ttu-id="5e83d-771">Počet paralelní požadavků tooAzure Cosmos DB služby toocreate dokumenty.</span><span class="sxs-lookup"><span data-stu-id="5e83d-771">Number of parallel requests tooAzure Cosmos DB service toocreate documents.</span></span><br/><br/><span data-ttu-id="5e83d-772">Při kopírování dat z Azure Cosmos DB pomocí této vlastnosti lze optimalizovat výkon hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-772">You can fine-tune hello performance when copying data to/from Azure Cosmos DB by using this property.</span></span> <span data-ttu-id="5e83d-773">Lepšího výkonu můžete očekávat, když zvýšíte writeBatchSize, protože se odesílají další paralelní požadavky tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5e83d-773">You can expect a better performance when you increase writeBatchSize because more parallel requests tooAzure Cosmos DB are sent.</span></span> <span data-ttu-id="5e83d-774">Ale budete potřebovat tooavoid omezení, který lze vyvolat hello chybová zpráva: "Požadavků je velká".</span><span class="sxs-lookup"><span data-stu-id="5e83d-774">However you’ll need tooavoid throttling that can throw hello error message: "Request rate is large".</span></span><br/><br/><span data-ttu-id="5e83d-775">Omezení je určeno podle počtu faktorů, včetně velikosti dokumentů, počet podmínky v dokumentech, indexování zásad cílovou kolekci, atd. Pro operace kopírování, můžete použít lepší hello toohave kolekce (například S3) většina propustnost, které jsou k dispozici (2 500 žádostí jednotek za sekundu).</span><span class="sxs-lookup"><span data-stu-id="5e83d-775">Throttling is decided by a number of factors, including size of documents, number of terms in documents, indexing policy of target collection, etc. For copy operations, you can use a better collection (for example, S3) toohave hello most throughput available (2,500 request units/second).</span></span> |<span data-ttu-id="5e83d-776">Integer</span><span class="sxs-lookup"><span data-stu-id="5e83d-776">Integer</span></span> |<span data-ttu-id="5e83d-777">Ne (výchozí: 5)</span><span class="sxs-lookup"><span data-stu-id="5e83d-777">No (default: 5)</span></span> |
| <span data-ttu-id="5e83d-778">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-778">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-779">Doba pro operaci toocomplete hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-779">Wait time for hello operation toocomplete before it times out.</span></span> |<span data-ttu-id="5e83d-780">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-780">timespan</span></span><br/><br/> <span data-ttu-id="5e83d-781">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="5e83d-781">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5e83d-782">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-782">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-783">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-783">Example</span></span>

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
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, title: aaaTitle, Suffix: Suffix"
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

<span data-ttu-id="5e83d-784">Další informace najdete v tématu [konektor Azure Cosmos DB](data-factory-azure-documentdb-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-784">For more information, see [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-sql-database"></a><span data-ttu-id="5e83d-785">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5e83d-785">Azure SQL Database</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-786">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-786">Linked service</span></span>
<span data-ttu-id="5e83d-787">propojená služba, sada hello toodefine Azure SQL Database **typ** hello propojené služby příliš**azuresqldatabase**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-787">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-788">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-788">Property</span></span> | <span data-ttu-id="5e83d-789">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-789">Description</span></span> | <span data-ttu-id="5e83d-790">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-790">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-791">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-791">connectionString</span></span> |<span data-ttu-id="5e83d-792">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-792">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-793">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-793">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-794">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-794">Example</span></span>
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

<span data-ttu-id="5e83d-795">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-795">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-796">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-796">Dataset</span></span>
<span data-ttu-id="5e83d-797">toodefine datové sadě služby Azure SQL Database set hello **typ** sady dat hello příliš**AzureSqlTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-797">toodefine an Azure SQL Database dataset, set hello **type** of hello dataset too**AzureSqlTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-798">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-798">Property</span></span> | <span data-ttu-id="5e83d-799">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-799">Description</span></span> | <span data-ttu-id="5e83d-800">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-800">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-801">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-801">tableName</span></span> |<span data-ttu-id="5e83d-802">Název hello tabulku nebo zobrazení hello Azure SQL Database instance, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-802">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="5e83d-803">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-803">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-804">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-804">Example</span></span>

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
<span data-ttu-id="5e83d-805">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-805">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="5e83d-806">Zdroje SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-806">SQL Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-807">Pokud kopírujete data z databáze SQL Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-807">If you are copying data from an Azure SQL Database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-808">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-808">Property</span></span> | <span data-ttu-id="5e83d-809">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-809">Description</span></span> | <span data-ttu-id="5e83d-810">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-810">Allowed values</span></span> | <span data-ttu-id="5e83d-811">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-811">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-812">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5e83d-812">sqlReaderQuery</span></span> |<span data-ttu-id="5e83d-813">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-813">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-814">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-814">SQL query string.</span></span> <span data-ttu-id="5e83d-815">Příklad: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-815">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-816">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-816">No</span></span> |
| <span data-ttu-id="5e83d-817">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-817">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="5e83d-818">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-818">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="5e83d-819">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-819">Name of hello stored procedure.</span></span> |<span data-ttu-id="5e83d-820">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-820">No</span></span> |
| <span data-ttu-id="5e83d-821">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-821">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-822">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-822">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5e83d-823">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-823">Name/value pairs.</span></span> <span data-ttu-id="5e83d-824">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-824">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5e83d-825">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-825">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-826">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-826">Example</span></span>

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
<span data-ttu-id="5e83d-827">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-827">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="5e83d-828">Jímku SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-828">SQL Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-829">Pokud kopírujete tooAzure dat SQL Database, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-829">If you are copying data tooAzure SQL Database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-830">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-830">Property</span></span> | <span data-ttu-id="5e83d-831">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-831">Description</span></span> | <span data-ttu-id="5e83d-832">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-832">Allowed values</span></span> | <span data-ttu-id="5e83d-833">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-833">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-834">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-834">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-835">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-835">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="5e83d-836">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-836">timespan</span></span><br/><br/> <span data-ttu-id="5e83d-837">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="5e83d-837">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5e83d-838">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-838">No</span></span> |
| <span data-ttu-id="5e83d-839">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-839">writeBatchSize</span></span> |<span data-ttu-id="5e83d-840">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-840">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="5e83d-841">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="5e83d-841">Integer (number of rows)</span></span> |<span data-ttu-id="5e83d-842">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="5e83d-842">No (default: 10000)</span></span> |
| <span data-ttu-id="5e83d-843">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5e83d-843">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5e83d-844">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-844">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="5e83d-845">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-845">A query statement.</span></span> |<span data-ttu-id="5e83d-846">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-846">No</span></span> |
| <span data-ttu-id="5e83d-847">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="5e83d-847">sliceIdentifierColumnName</span></span> |<span data-ttu-id="5e83d-848">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-848">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="5e83d-849">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="5e83d-849">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="5e83d-850">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-850">No</span></span> |
| <span data-ttu-id="5e83d-851">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-851">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="5e83d-852">Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-852">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="5e83d-853">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-853">Name of hello stored procedure.</span></span> |<span data-ttu-id="5e83d-854">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-854">No</span></span> |
| <span data-ttu-id="5e83d-855">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-855">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-856">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-856">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5e83d-857">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-857">Name/value pairs.</span></span> <span data-ttu-id="5e83d-858">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-858">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5e83d-859">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-859">No</span></span> |
| <span data-ttu-id="5e83d-860">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="5e83d-860">sqlWriterTableType</span></span> |<span data-ttu-id="5e83d-861">Zadejte toobe tabulku typu název používá v hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-861">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="5e83d-862">Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-862">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="5e83d-863">Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="5e83d-863">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="5e83d-864">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-864">A table type name.</span></span> |<span data-ttu-id="5e83d-865">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-865">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-866">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-866">Example</span></span>

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

<span data-ttu-id="5e83d-867">Další informace najdete v tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-867">For more information, see [Azure SQL connector](data-factory-azure-sql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="5e83d-868">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-868">Azure SQL Data Warehouse</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-869">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-869">Linked service</span></span>
<span data-ttu-id="5e83d-870">propojená služba, sada hello toodefine Azure SQL Data Warehouse **typ** hello propojené služby příliš**AzureSqlDW**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-870">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-871">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-871">Property</span></span> | <span data-ttu-id="5e83d-872">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-872">Description</span></span> | <span data-ttu-id="5e83d-873">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-873">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-874">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-874">connectionString</span></span> |<span data-ttu-id="5e83d-875">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Data Warehouse instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-875">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-876">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-876">Yes</span></span> |



#### <a name="example"></a><span data-ttu-id="5e83d-877">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-877">Example</span></span>

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

<span data-ttu-id="5e83d-878">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-878">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-879">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-879">Dataset</span></span>
<span data-ttu-id="5e83d-880">toodefine datové sadě služby Azure SQL Data Warehouse sadu hello **typ** sady dat hello příliš**AzureSqlDWTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-880">toodefine an Azure SQL Data Warehouse dataset, set hello **type** of hello dataset too**AzureSqlDWTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-881">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-881">Property</span></span> | <span data-ttu-id="5e83d-882">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-882">Description</span></span> | <span data-ttu-id="5e83d-883">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-883">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-884">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-884">tableName</span></span> |<span data-ttu-id="5e83d-885">Název hello tabulku nebo zobrazení v databázi Azure SQL Data Warehouse hello, která hello propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-885">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="5e83d-886">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-886">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-887">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-887">Example</span></span>

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

<span data-ttu-id="5e83d-888">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-888">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-dw-source-in-copy-activity"></a><span data-ttu-id="5e83d-889">Zdroj datového skladu SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-889">SQL DW Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-890">Pokud jsou kopírování dat z Azure SQL Data Warehouse, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlDWSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-890">If you are copying data from Azure SQL Data Warehouse, set hello **source type** of hello copy activity too**SqlDWSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-891">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-891">Property</span></span> | <span data-ttu-id="5e83d-892">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-892">Description</span></span> | <span data-ttu-id="5e83d-893">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-893">Allowed values</span></span> | <span data-ttu-id="5e83d-894">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-894">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-895">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5e83d-895">sqlReaderQuery</span></span> |<span data-ttu-id="5e83d-896">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-896">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-897">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-897">SQL query string.</span></span> <span data-ttu-id="5e83d-898">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-898">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-899">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-899">No</span></span> |
| <span data-ttu-id="5e83d-900">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-900">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="5e83d-901">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-901">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="5e83d-902">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-902">Name of hello stored procedure.</span></span> |<span data-ttu-id="5e83d-903">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-903">No</span></span> |
| <span data-ttu-id="5e83d-904">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-904">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-905">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-905">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5e83d-906">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-906">Name/value pairs.</span></span> <span data-ttu-id="5e83d-907">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-907">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5e83d-908">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-908">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-909">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-909">Example</span></span>

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

<span data-ttu-id="5e83d-910">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-910">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-dw-sink-in-copy-activity"></a><span data-ttu-id="5e83d-911">Podřízený datového skladu SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-911">SQL DW Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-912">Pokud kopírujete tooAzure dat SQL Data Warehouse, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlDWSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-912">If you are copying data tooAzure SQL Data Warehouse, set hello **sink type** of hello copy activity too**SqlDWSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-913">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-913">Property</span></span> | <span data-ttu-id="5e83d-914">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-914">Description</span></span> | <span data-ttu-id="5e83d-915">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-915">Allowed values</span></span> | <span data-ttu-id="5e83d-916">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-916">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-917">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5e83d-917">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5e83d-918">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-918">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="5e83d-919">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-919">A query statement.</span></span> |<span data-ttu-id="5e83d-920">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-920">No</span></span> |
| <span data-ttu-id="5e83d-921">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="5e83d-921">allowPolyBase</span></span> |<span data-ttu-id="5e83d-922">Určuje, zda toouse PolyBase (v případě potřeby) namísto mechanismus hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="5e83d-922">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="5e83d-923">**Hello doporučená způsob tooload dat do SQL Data Warehouse pomocí PolyBase je.**</span><span class="sxs-lookup"><span data-stu-id="5e83d-923">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> |<span data-ttu-id="5e83d-924">True</span><span class="sxs-lookup"><span data-stu-id="5e83d-924">True</span></span> <br/><span data-ttu-id="5e83d-925">NEPRAVDA (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-925">False (default)</span></span> |<span data-ttu-id="5e83d-926">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-926">No</span></span> |
| <span data-ttu-id="5e83d-927">polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="5e83d-927">polyBaseSettings</span></span> |<span data-ttu-id="5e83d-928">Skupinu vlastností, které je možné zadat při hello **allowPolybase** vlastnost je nastavena příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-928">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="5e83d-929">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-929">No</span></span> |
| <span data-ttu-id="5e83d-930">rejectValue</span><span class="sxs-lookup"><span data-stu-id="5e83d-930">rejectValue</span></span> |<span data-ttu-id="5e83d-931">Určuje číslo hello nebo podíl řádků, které může být odmítnutá před hello se dotaz nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5e83d-931">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="5e83d-932">Další informace o hello PolyBase odmítnout možnosti v hello **argumenty** části [vytvořit EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-932">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="5e83d-933">0 (výchozí), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="5e83d-933">0 (default), 1, 2, …</span></span> |<span data-ttu-id="5e83d-934">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-934">No</span></span> |
| <span data-ttu-id="5e83d-935">rejectType</span><span class="sxs-lookup"><span data-stu-id="5e83d-935">rejectType</span></span> |<span data-ttu-id="5e83d-936">Určuje, zda je zadána možnost rejectValue hello literálovou hodnotou nebo jako procento.</span><span class="sxs-lookup"><span data-stu-id="5e83d-936">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="5e83d-937">Hodnota (výchozí), procento</span><span class="sxs-lookup"><span data-stu-id="5e83d-937">Value (default), Percentage</span></span> |<span data-ttu-id="5e83d-938">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-938">No</span></span> |
| <span data-ttu-id="5e83d-939">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="5e83d-939">rejectSampleValue</span></span> |<span data-ttu-id="5e83d-940">Určuje hello počet řádků tooretrieve před hello PolyBase přepočítá hello procento odmítnutých řádků.</span><span class="sxs-lookup"><span data-stu-id="5e83d-940">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="5e83d-941">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="5e83d-941">1, 2, …</span></span> |<span data-ttu-id="5e83d-942">Ano, pokud **rejectType** je **procento**</span><span class="sxs-lookup"><span data-stu-id="5e83d-942">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="5e83d-943">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="5e83d-943">useTypeDefault</span></span> |<span data-ttu-id="5e83d-944">Určuje, jak toohandle chybějící hodnoty v oddělené textové soubory, když PolyBase načítá data z textového souboru hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-944">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="5e83d-945">Další informace o této vlastnosti z oddílu argumenty hello v [vytvořit EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e83d-945">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="5e83d-946">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-946">True, False (default)</span></span> |<span data-ttu-id="5e83d-947">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-947">No</span></span> |
| <span data-ttu-id="5e83d-948">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-948">writeBatchSize</span></span> |<span data-ttu-id="5e83d-949">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-949">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="5e83d-950">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="5e83d-950">Integer (number of rows)</span></span> |<span data-ttu-id="5e83d-951">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="5e83d-951">No (default: 10000)</span></span> |
| <span data-ttu-id="5e83d-952">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-952">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-953">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-953">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="5e83d-954">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-954">timespan</span></span><br/><br/> <span data-ttu-id="5e83d-955">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="5e83d-955">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5e83d-956">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-956">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-957">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-957">Example</span></span>

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

<span data-ttu-id="5e83d-958">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-958">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="azure-search"></a><span data-ttu-id="5e83d-959">Azure Search</span><span class="sxs-lookup"><span data-stu-id="5e83d-959">Azure Search</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-960">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-960">Linked service</span></span>
<span data-ttu-id="5e83d-961">propojená služba, sada hello toodefine Azure Search **typ** hello propojené služby příliš**AzureSearch**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-961">toodefine an Azure Search linked service, set hello **type** of hello linked service too**AzureSearch**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-962">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-962">Property</span></span> | <span data-ttu-id="5e83d-963">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-963">Description</span></span> | <span data-ttu-id="5e83d-964">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-964">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="5e83d-965">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="5e83d-965">url</span></span> | <span data-ttu-id="5e83d-966">Adresa URL pro hello služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5e83d-966">URL for hello Azure Search service.</span></span> | <span data-ttu-id="5e83d-967">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-967">Yes</span></span> |
| <span data-ttu-id="5e83d-968">key</span><span class="sxs-lookup"><span data-stu-id="5e83d-968">key</span></span> | <span data-ttu-id="5e83d-969">Klíč správce pro hello služby Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5e83d-969">Admin key for hello Azure Search service.</span></span> | <span data-ttu-id="5e83d-970">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-970">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-971">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-971">Example</span></span>

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

<span data-ttu-id="5e83d-972">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-972">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-973">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-973">Dataset</span></span>
<span data-ttu-id="5e83d-974">toodefine datové sadě služby Azure Search sadu hello **typ** sady dat hello příliš**AzureSearchIndex**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části :</span><span class="sxs-lookup"><span data-stu-id="5e83d-974">toodefine an Azure Search dataset, set hello **type** of hello dataset too**AzureSearchIndex**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-975">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-975">Property</span></span> | <span data-ttu-id="5e83d-976">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-976">Description</span></span> | <span data-ttu-id="5e83d-977">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-977">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="5e83d-978">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-978">type</span></span> | <span data-ttu-id="5e83d-979">musí být nastavena vlastnost typu Hello příliš**AzureSearchIndex**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-979">hello type property must be set too**AzureSearchIndex**.</span></span>| <span data-ttu-id="5e83d-980">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-980">Yes</span></span> |
| <span data-ttu-id="5e83d-981">indexName</span><span class="sxs-lookup"><span data-stu-id="5e83d-981">indexName</span></span> | <span data-ttu-id="5e83d-982">Název indexu Azure Search hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-982">Name of hello Azure Search index.</span></span> <span data-ttu-id="5e83d-983">Objekt pro vytváření dat nevytváří hello index.</span><span class="sxs-lookup"><span data-stu-id="5e83d-983">Data Factory does not create hello index.</span></span> <span data-ttu-id="5e83d-984">Hello index musí existovat ve službě Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5e83d-984">hello index must exist in Azure Search.</span></span> | <span data-ttu-id="5e83d-985">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-985">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-986">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-986">Example</span></span>

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

<span data-ttu-id="5e83d-987">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-987">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) article.</span></span>

### <a name="azure-search-index-sink-in-copy-activity"></a><span data-ttu-id="5e83d-988">Podřízený Index Azure Search v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-988">Azure Search Index Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-989">Pokud kopírujete indexu Azure Search tooan dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureSearchIndexSink**a zadejte následující vlastnosti v hello **podřízený**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-989">If you are copying data tooan Azure Search index, set hello **sink type** of hello copy activity too**AzureSearchIndexSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-990">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-990">Property</span></span> | <span data-ttu-id="5e83d-991">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-991">Description</span></span> | <span data-ttu-id="5e83d-992">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-992">Allowed values</span></span> | <span data-ttu-id="5e83d-993">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-993">Required</span></span> |
| -------- | ----------- | -------------- | -------- |
| <span data-ttu-id="5e83d-994">WriteBehavior</span><span class="sxs-lookup"><span data-stu-id="5e83d-994">WriteBehavior</span></span> | <span data-ttu-id="5e83d-995">Určuje, zda toomerge nebo nahradit, když dokumentu již existuje v indexu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-995">Specifies whether toomerge or replace when a document already exists in hello index.</span></span> | <span data-ttu-id="5e83d-996">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-996">Merge (default)</span></span><br/><span data-ttu-id="5e83d-997">Odeslat</span><span class="sxs-lookup"><span data-stu-id="5e83d-997">Upload</span></span>| <span data-ttu-id="5e83d-998">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-998">No</span></span> |
| <span data-ttu-id="5e83d-999">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-999">WriteBatchSize</span></span> | <span data-ttu-id="5e83d-1000">Ukládání dat do indexu Azure Search hello, když velikost vyrovnávací paměti hello dosáhne writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1000">Uploads data into hello Azure Search index when hello buffer size reaches writeBatchSize.</span></span> | <span data-ttu-id="5e83d-1001">1 too1 000.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1001">1 too1,000.</span></span> <span data-ttu-id="5e83d-1002">Výchozí hodnota je 1 000.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1002">Default value is 1000.</span></span> | <span data-ttu-id="5e83d-1003">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1003">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1004">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1004">Example</span></span>

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

<span data-ttu-id="5e83d-1005">Další informace najdete v tématu [konektor Azure Search](data-factory-azure-search-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1005">For more information, see [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) article.</span></span>

## <a name="azure-table-storage"></a><span data-ttu-id="5e83d-1006">Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-1006">Azure Table Storage</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1007">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1007">Linked service</span></span>
<span data-ttu-id="5e83d-1008">Existují dva typy propojené služby: propojená služba Azure Storage a propojená služba Azure Storage SAS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1008">There are two types of linked services: Azure Storage linked service and Azure Storage SAS linked service.</span></span>

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="5e83d-1009">Propojená služba Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5e83d-1009">Azure Storage Linked Service</span></span>
<span data-ttu-id="5e83d-1010">toolink tooa účet úložiště Azure datovou továrnu pomocí hello **klíč účtu**, vytvoření služby Azure Storage, propojené.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1010">toolink your Azure storage account tooa data factory by using hello **account key**, create an Azure Storage linked service.</span></span> <span data-ttu-id="5e83d-1011">toodefine Azure Storage, propojené služby, sada hello **typ** hello propojené služby příliš**azurestorage**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1011">toodefine an Azure Storage linked service, set hello **type** of hello linked service too**AzureStorage**.</span></span> <span data-ttu-id="5e83d-1012">Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1012">Then, you can specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1013">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1013">Property</span></span> | <span data-ttu-id="5e83d-1014">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1014">Description</span></span> | <span data-ttu-id="5e83d-1015">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1015">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-1016">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-1016">type</span></span> |<span data-ttu-id="5e83d-1017">vlastnost typu Hello musí být nastavena na: **azurestorage.**</span><span class="sxs-lookup"><span data-stu-id="5e83d-1017">hello type property must be set to: **AzureStorage**</span></span> |<span data-ttu-id="5e83d-1018">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1018">Yes</span></span> |
| <span data-ttu-id="5e83d-1019">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1019">connectionString</span></span> |<span data-ttu-id="5e83d-1020">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1020">Specify information needed tooconnect tooAzure storage for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-1021">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1021">Yes</span></span> |

<span data-ttu-id="5e83d-1022">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="5e83d-1022">**Example:**</span></span>  

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

#### <a name="azure-storage-sas-linked-service"></a><span data-ttu-id="5e83d-1023">Propojená služba Azure Storage SAS</span><span class="sxs-lookup"><span data-stu-id="5e83d-1023">Azure Storage SAS Linked Service</span></span>
<span data-ttu-id="5e83d-1024">Hello SAS úložiště Azure, propojené služby umožňuje toolink služby Azure data factory tooan účet úložiště Azure pomocí sdíleného přístupového podpisu (SAS).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1024">hello Azure Storage SAS linked service allows you toolink an Azure Storage Account tooan Azure data factory by using a Shared Access Signature (SAS).</span></span> <span data-ttu-id="5e83d-1025">Poskytuje objekt pro vytváření dat hello přístup omezený nebo časově vázaných tooall nebo konkrétní prostředky (kontejner nebo objektů blob) v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1025">It provides hello data factory with restricted/time-bound access tooall/specific resources (blob/container) in hello storage.</span></span> <span data-ttu-id="5e83d-1026">toolink tooa účet úložiště Azure datovou továrnu pomocí sdíleného přístupového podpisu vytvoření služby Azure úložiště SAS propojený.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1026">toolink your Azure storage account tooa data factory by using Shared Access Signature, create an Azure Storage SAS linked service.</span></span> <span data-ttu-id="5e83d-1027">toodefine Azure úložiště SAS propojená služba, sada hello **typ** hello propojené služby příliš**AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1027">toodefine an Azure Storage SAS linked service, set hello **type** of hello linked service too**AzureStorageSas**.</span></span> <span data-ttu-id="5e83d-1028">Potom můžete zadat následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1028">Then, you can specify following properties in hello **typeProperties** section:</span></span>   

| <span data-ttu-id="5e83d-1029">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1029">Property</span></span> | <span data-ttu-id="5e83d-1030">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1030">Description</span></span> | <span data-ttu-id="5e83d-1031">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1031">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-1032">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-1032">type</span></span> |<span data-ttu-id="5e83d-1033">vlastnost typu Hello musí být nastavena na: **AzureStorageSas**</span><span class="sxs-lookup"><span data-stu-id="5e83d-1033">hello type property must be set to: **AzureStorageSas**</span></span> |<span data-ttu-id="5e83d-1034">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1034">Yes</span></span> |
| <span data-ttu-id="5e83d-1035">sasUri</span><span class="sxs-lookup"><span data-stu-id="5e83d-1035">sasUri</span></span> |<span data-ttu-id="5e83d-1036">Zadejte identifikátor URI podpis sdíleného přístupu toohello Azure Storage prostředky jako objekt blob, kontejneru nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1036">Specify Shared Access Signature URI toohello Azure Storage resources such as blob, container, or table.</span></span> |<span data-ttu-id="5e83d-1037">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1037">Yes</span></span> |

<span data-ttu-id="5e83d-1038">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="5e83d-1038">**Example:**</span></span>

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

<span data-ttu-id="5e83d-1039">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1039">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1040">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1040">Dataset</span></span>
<span data-ttu-id="5e83d-1041">toodefine datové sadě služby Azure Table sadu hello **typ** sady dat hello příliš**AzureTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1041">toodefine an Azure Table dataset, set hello **type** of hello dataset too**AzureTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1042">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1042">Property</span></span> | <span data-ttu-id="5e83d-1043">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1043">Description</span></span> | <span data-ttu-id="5e83d-1044">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1044">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1045">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1045">tableName</span></span> |<span data-ttu-id="5e83d-1046">Název tabulky hello hello instance Azure tabulku databáze, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1046">Name of hello table in hello Azure Table Database instance that linked service refers to.</span></span> |<span data-ttu-id="5e83d-1047">Ano.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1047">Yes.</span></span> <span data-ttu-id="5e83d-1048">Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1048">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="5e83d-1049">Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1049">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1050">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1050">Example</span></span>

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

<span data-ttu-id="5e83d-1051">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1051">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) article.</span></span> 

### <a name="azure-table-source-in-copy-activity"></a><span data-ttu-id="5e83d-1052">Zdrojové tabulky Azure v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1052">Azure Table Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1053">Pokud jsou kopírování dat z úložiště tabulek Azure, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**AzureTableSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1053">If you are copying data from Azure Table Storage, set hello **source type** of hello copy activity too**AzureTableSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1054">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1054">Property</span></span> | <span data-ttu-id="5e83d-1055">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1055">Description</span></span> | <span data-ttu-id="5e83d-1056">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1056">Allowed values</span></span> | <span data-ttu-id="5e83d-1057">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1057">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1058">azureTableSourceQuery</span><span class="sxs-lookup"><span data-stu-id="5e83d-1058">azureTableSourceQuery</span></span> |<span data-ttu-id="5e83d-1059">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1059">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1060">Řetězec dotazu tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1060">Azure table query string.</span></span> <span data-ttu-id="5e83d-1061">Příklady v další části hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1061">See examples in hello next section.</span></span> |<span data-ttu-id="5e83d-1062">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1062">No.</span></span> <span data-ttu-id="5e83d-1063">Když název tabulky je zadán bez azureTableSourceQuery, jsou všechny záznamy z tabulky hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1063">When a tableName is specified without an azureTableSourceQuery, all records from hello table are copied toohello destination.</span></span> <span data-ttu-id="5e83d-1064">Pokud je zadána také azureTableSourceQuery, záznamy z hello tabulky, který splňuje hello dotazu jsou zkopírované toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1064">If an azureTableSourceQuery is also specified, records from hello table that satisfies hello query are copied toohello destination.</span></span> |
| <span data-ttu-id="5e83d-1065">azureTableSourceIgnoreTableNotFound</span><span class="sxs-lookup"><span data-stu-id="5e83d-1065">azureTableSourceIgnoreTableNotFound</span></span> |<span data-ttu-id="5e83d-1066">Označuje, zda swallow hello výjimky tabulky neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1066">Indicate whether swallow hello exception of table not exist.</span></span> |<span data-ttu-id="5e83d-1067">HODNOTA TRUE</span><span class="sxs-lookup"><span data-stu-id="5e83d-1067">TRUE</span></span><br/><span data-ttu-id="5e83d-1068">FALSE</span><span class="sxs-lookup"><span data-stu-id="5e83d-1068">FALSE</span></span> |<span data-ttu-id="5e83d-1069">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1069">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1070">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1070">Example</span></span>

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

<span data-ttu-id="5e83d-1071">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1071">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

### <a name="azure-table-sink-in-copy-activity"></a><span data-ttu-id="5e83d-1072">Podřízený tabulky Azure v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1072">Azure Table Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-1073">Pokud kopírujete data tooAzure Table Storage, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**AzureTableSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1073">If you are copying data tooAzure Table Storage, set hello **sink type** of hello copy activity too**AzureTableSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-1074">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1074">Property</span></span> | <span data-ttu-id="5e83d-1075">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1075">Description</span></span> | <span data-ttu-id="5e83d-1076">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1076">Allowed values</span></span> | <span data-ttu-id="5e83d-1077">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1077">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1078">azureTableDefaultPartitionKeyValue</span><span class="sxs-lookup"><span data-stu-id="5e83d-1078">azureTableDefaultPartitionKeyValue</span></span> |<span data-ttu-id="5e83d-1079">Výchozí hodnotu klíče oddílu, mohou být využívána hello jímky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1079">Default partition key value that can be used by hello sink.</span></span> |<span data-ttu-id="5e83d-1080">Hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1080">A string value.</span></span> |<span data-ttu-id="5e83d-1081">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1081">No</span></span> |
| <span data-ttu-id="5e83d-1082">azureTablePartitionKeyName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1082">azureTablePartitionKeyName</span></span> |<span data-ttu-id="5e83d-1083">Zadejte název hello sloupce, jejichž hodnoty se používají jako klíče oddílů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1083">Specify name of hello column whose values are used as partition keys.</span></span> <span data-ttu-id="5e83d-1084">Pokud není zadaný, použije se jako klíč oddílu hello AzureTableDefaultPartitionKeyValue.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1084">If not specified, AzureTableDefaultPartitionKeyValue is used as hello partition key.</span></span> |<span data-ttu-id="5e83d-1085">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1085">A column name.</span></span> |<span data-ttu-id="5e83d-1086">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1086">No</span></span> |
| <span data-ttu-id="5e83d-1087">azureTableRowKeyName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1087">azureTableRowKeyName</span></span> |<span data-ttu-id="5e83d-1088">Zadejte název hello sloupce, jejichž hodnoty sloupce jsou použity jako klíč řádku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1088">Specify name of hello column whose column values are used as row key.</span></span> <span data-ttu-id="5e83d-1089">Pokud není zadaný, použijte identifikátor GUID pro každý řádek.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1089">If not specified, use a GUID for each row.</span></span> |<span data-ttu-id="5e83d-1090">Název sloupce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1090">A column name.</span></span> |<span data-ttu-id="5e83d-1091">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1091">No</span></span> |
| <span data-ttu-id="5e83d-1092">azureTableInsertType</span><span class="sxs-lookup"><span data-stu-id="5e83d-1092">azureTableInsertType</span></span> |<span data-ttu-id="5e83d-1093">Hello režimu tooinsert data do tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1093">hello mode tooinsert data into Azure table.</span></span><br/><br/><span data-ttu-id="5e83d-1094">Tato vlastnost určuje, jestli mají existující řádky v tabulce výstup hello s odpovídajícím klíče oddílu a řádku jejich hodnoty nahradit nebo sloučit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1094">This property controls whether existing rows in hello output table with matching partition and row keys have their values replaced or merged.</span></span> <br/><br/><span data-ttu-id="5e83d-1095">toolearn o tom, jak tato nastavení (sloučení a nahraďte) fungují, najdete v části [vložení nebo sloučení Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) a [vložení nebo nahrazení Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) témata.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1095">toolearn about how these settings (merge and replace) work, see [Insert or Merge Entity](https://msdn.microsoft.com/library/azure/hh452241.aspx) and [Insert or Replace Entity](https://msdn.microsoft.com/library/azure/hh452242.aspx) topics.</span></span> <br/><br> <span data-ttu-id="5e83d-1096">Toto nastavení se vztahuje na úrovni řádku hello, není hello na úrovni tabulky, a ani možnost odstraní řádky v tabulce výstup hello, které nejsou k dispozici ve vstupu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1096">This setting applies at hello row level, not hello table level, and neither option deletes rows in hello output table that do not exist in hello input.</span></span> |<span data-ttu-id="5e83d-1097">Merge (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1097">merge (default)</span></span><br/><span data-ttu-id="5e83d-1098">Nahradit</span><span class="sxs-lookup"><span data-stu-id="5e83d-1098">replace</span></span> |<span data-ttu-id="5e83d-1099">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1099">No</span></span> |
| <span data-ttu-id="5e83d-1100">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-1100">writeBatchSize</span></span> |<span data-ttu-id="5e83d-1101">Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1101">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit.</span></span> |<span data-ttu-id="5e83d-1102">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1102">Integer (number of rows)</span></span> |<span data-ttu-id="5e83d-1103">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1103">No (default: 10000)</span></span> |
| <span data-ttu-id="5e83d-1104">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-1104">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-1105">Když je dosaženo hello writeBatchSize nebo writeBatchTimeout vkládá data do hello tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="5e83d-1105">Inserts data into hello Azure table when hello writeBatchSize or writeBatchTimeout is hit</span></span> |<span data-ttu-id="5e83d-1106">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-1106">timespan</span></span><br/><br/><span data-ttu-id="5e83d-1107">Příklad: "00:20:00" (20 minut)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1107">Example: “00:20:00” (20 minutes)</span></span> |<span data-ttu-id="5e83d-1108">Ne (výchozí hodnota časového limitu pro klienta toostorage výchozí hodnotu 90 sekundu)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1108">No (Default toostorage client default timeout value 90 sec)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1109">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1109">Example</span></span>

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
<span data-ttu-id="5e83d-1110">Další informace o těchto propojených služeb najdete v tématu [konektor Azure Table Storage](data-factory-azure-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1110">For more information about these linked services, see [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="amazon-redshift"></a><span data-ttu-id="5e83d-1111">Amazon RedShift</span><span class="sxs-lookup"><span data-stu-id="5e83d-1111">Amazon RedShift</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1112">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1112">Linked service</span></span>
<span data-ttu-id="5e83d-1113">toodefine Redshift Amazon propojená služba, sada hello **typ** hello propojené služby příliš**AmazonRedshift**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1113">toodefine an Amazon Redshift linked service, set hello **type** of hello linked service too**AmazonRedshift**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1114">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1114">Property</span></span> | <span data-ttu-id="5e83d-1115">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1115">Description</span></span> | <span data-ttu-id="5e83d-1116">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1116">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1117">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1117">server</span></span> |<span data-ttu-id="5e83d-1118">IP adresa nebo název hostitele serveru Amazon Redshift hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1118">IP address or host name of hello Amazon Redshift server.</span></span> |<span data-ttu-id="5e83d-1119">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1119">Yes</span></span> |
| <span data-ttu-id="5e83d-1120">port</span><span class="sxs-lookup"><span data-stu-id="5e83d-1120">port</span></span> |<span data-ttu-id="5e83d-1121">Hello počet hello port TCP, který hello Amazon Redshift server používá toolisten pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1121">hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.</span></span> |<span data-ttu-id="5e83d-1122">Ne, výchozí hodnota: 5439</span><span class="sxs-lookup"><span data-stu-id="5e83d-1122">No, default value: 5439</span></span> |
| <span data-ttu-id="5e83d-1123">Databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1123">database</span></span> |<span data-ttu-id="5e83d-1124">Název databáze Amazon Redshift hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1124">Name of hello Amazon Redshift database.</span></span> |<span data-ttu-id="5e83d-1125">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1125">Yes</span></span> |
| <span data-ttu-id="5e83d-1126">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1126">username</span></span> |<span data-ttu-id="5e83d-1127">Jméno uživatele, který má přístup toohello databáze.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1127">Name of user who has access toohello database.</span></span> |<span data-ttu-id="5e83d-1128">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1128">Yes</span></span> |
| <span data-ttu-id="5e83d-1129">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1129">password</span></span> |<span data-ttu-id="5e83d-1130">Heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1130">Password for hello user account.</span></span> |<span data-ttu-id="5e83d-1131">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1131">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1132">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1132">Example</span></span>

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

<span data-ttu-id="5e83d-1133">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1133">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1134">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1134">Dataset</span></span>
<span data-ttu-id="5e83d-1135">toodefine datové sadě služby Amazon Redshift sadu hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1135">toodefine an Amazon Redshift dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1136">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1136">Property</span></span> | <span data-ttu-id="5e83d-1137">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1137">Description</span></span> | <span data-ttu-id="5e83d-1138">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1138">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1139">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1139">tableName</span></span> |<span data-ttu-id="5e83d-1140">Název hello tabulky v databázi hello Amazon Redshift, která propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1140">Name of hello table in hello Amazon Redshift database that linked service refers to.</span></span> |<span data-ttu-id="5e83d-1141">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1141">No (if **query** of **RelationalSource** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-1142">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1142">Example</span></span>

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
<span data-ttu-id="5e83d-1143">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1143">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1144">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1144">Relational Source in Copy Activity</span></span> 
<span data-ttu-id="5e83d-1145">Pokud jsou kopírování dat z Amazon Redshift, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1145">If you are copying data from Amazon Redshift, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1146">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1146">Property</span></span> | <span data-ttu-id="5e83d-1147">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1147">Description</span></span> | <span data-ttu-id="5e83d-1148">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1148">Allowed values</span></span> | <span data-ttu-id="5e83d-1149">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1149">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1150">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1150">query</span></span> |<span data-ttu-id="5e83d-1151">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1151">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1152">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1152">SQL query string.</span></span> <span data-ttu-id="5e83d-1153">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1153">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-1154">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1154">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1155">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1155">Example</span></span>

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
<span data-ttu-id="5e83d-1156">Další informace najdete v tématu [Amazon Redshift konektor](#data-factory-amazon-redshift-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1156">For more information, see [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) article.</span></span>

## <a name="ibm-db2"></a><span data-ttu-id="5e83d-1157">IBM DB2</span><span class="sxs-lookup"><span data-stu-id="5e83d-1157">IBM DB2</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1158">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1158">Linked service</span></span>
<span data-ttu-id="5e83d-1159">propojená služba, sada hello toodefine IBM DB2 **typ** hello propojené služby příliš**OnPremisesDB2**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1159">toodefine an IBM DB2 linked service, set hello **type** of hello linked service too**OnPremisesDB2**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1160">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1160">Property</span></span> | <span data-ttu-id="5e83d-1161">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1161">Description</span></span> | <span data-ttu-id="5e83d-1162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1163">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1163">server</span></span> |<span data-ttu-id="5e83d-1164">Název serveru hello DB2.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1164">Name of hello DB2 server.</span></span> |<span data-ttu-id="5e83d-1165">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1165">Yes</span></span> |
| <span data-ttu-id="5e83d-1166">Databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1166">database</span></span> |<span data-ttu-id="5e83d-1167">Název databáze hello DB2.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1167">Name of hello DB2 database.</span></span> |<span data-ttu-id="5e83d-1168">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1168">Yes</span></span> |
| <span data-ttu-id="5e83d-1169">Schéma</span><span class="sxs-lookup"><span data-stu-id="5e83d-1169">schema</span></span> |<span data-ttu-id="5e83d-1170">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1170">Name of hello schema in hello database.</span></span> <span data-ttu-id="5e83d-1171">název schématu Hello rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1171">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="5e83d-1172">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1172">No</span></span> |
| <span data-ttu-id="5e83d-1173">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1173">authenticationType</span></span> |<span data-ttu-id="5e83d-1174">Typ ověřování používá databázi DB2 toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1174">Type of authentication used tooconnect toohello DB2 database.</span></span> <span data-ttu-id="5e83d-1175">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1175">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="5e83d-1176">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1176">Yes</span></span> |
| <span data-ttu-id="5e83d-1177">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1177">username</span></span> |<span data-ttu-id="5e83d-1178">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1178">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="5e83d-1179">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1179">No</span></span> |
| <span data-ttu-id="5e83d-1180">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1180">password</span></span> |<span data-ttu-id="5e83d-1181">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1181">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-1182">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1182">No</span></span> |
| <span data-ttu-id="5e83d-1183">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1183">gatewayName</span></span> |<span data-ttu-id="5e83d-1184">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi DB2.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1184">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises DB2 database.</span></span> |<span data-ttu-id="5e83d-1185">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1185">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1186">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1186">Example</span></span>
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
<span data-ttu-id="5e83d-1187">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1187">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1188">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1188">Dataset</span></span>
<span data-ttu-id="5e83d-1189">Datová sada toodefine DB2, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1189">toodefine a DB2 dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="5e83d-1190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1190">Property</span></span> | <span data-ttu-id="5e83d-1191">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1191">Description</span></span> | <span data-ttu-id="5e83d-1192">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1192">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1193">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1193">tableName</span></span> |<span data-ttu-id="5e83d-1194">Název tabulky hello instance databáze DB2 hello, kterou propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1194">Name of hello table in hello DB2 Database instance that linked service refers to.</span></span> <span data-ttu-id="5e83d-1195">Hello tableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1195">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="5e83d-1196">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1196">No (if **query** of **RelationalSource** is specified)</span></span> 

#### <a name="example"></a><span data-ttu-id="5e83d-1197">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1197">Example</span></span>
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

<span data-ttu-id="5e83d-1198">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1198">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1199">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1199">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1200">Pokud jsou kopírování dat z IBM DB2, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1200">If you are copying data from IBM DB2, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1201">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1201">Property</span></span> | <span data-ttu-id="5e83d-1202">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1202">Description</span></span> | <span data-ttu-id="5e83d-1203">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1203">Allowed values</span></span> | <span data-ttu-id="5e83d-1204">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1204">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1205">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1205">query</span></span> |<span data-ttu-id="5e83d-1206">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1206">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1207">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1207">SQL query string.</span></span> <span data-ttu-id="5e83d-1208">Například: `"query": "select * from "MySchema"."MyTable""`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1208">For example: `"query": "select * from "MySchema"."MyTable""`.</span></span> |<span data-ttu-id="5e83d-1209">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1209">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1210">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1210">Example</span></span>
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
<span data-ttu-id="5e83d-1211">Další informace najdete v tématu [IBM DB2 konektor](#data-factory-onprem-db2-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1211">For more information, see [IBM DB2 connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) article.</span></span>

## <a name="mysql"></a><span data-ttu-id="5e83d-1212">MySQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-1212">MySQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1213">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1213">Linked service</span></span>
<span data-ttu-id="5e83d-1214">toodefine MySQL propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesMySql**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1214">toodefine a MySQL linked service, set hello **type** of hello linked service too**OnPremisesMySql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1215">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1215">Property</span></span> | <span data-ttu-id="5e83d-1216">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1216">Description</span></span> | <span data-ttu-id="5e83d-1217">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1217">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1218">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1218">server</span></span> |<span data-ttu-id="5e83d-1219">Název serveru MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1219">Name of hello MySQL server.</span></span> |<span data-ttu-id="5e83d-1220">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1220">Yes</span></span> |
| <span data-ttu-id="5e83d-1221">Databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1221">database</span></span> |<span data-ttu-id="5e83d-1222">Název databáze MySQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1222">Name of hello MySQL database.</span></span> |<span data-ttu-id="5e83d-1223">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1223">Yes</span></span> |
| <span data-ttu-id="5e83d-1224">Schéma</span><span class="sxs-lookup"><span data-stu-id="5e83d-1224">schema</span></span> |<span data-ttu-id="5e83d-1225">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1225">Name of hello schema in hello database.</span></span> |<span data-ttu-id="5e83d-1226">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1226">No</span></span> |
| <span data-ttu-id="5e83d-1227">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1227">authenticationType</span></span> |<span data-ttu-id="5e83d-1228">Typ ověřování používá databázi MySQL toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1228">Type of authentication used tooconnect toohello MySQL database.</span></span> <span data-ttu-id="5e83d-1229">Možné hodnoty jsou: `Basic`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1229">Possible values are: `Basic`.</span></span> |<span data-ttu-id="5e83d-1230">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1230">Yes</span></span> |
| <span data-ttu-id="5e83d-1231">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1231">username</span></span> |<span data-ttu-id="5e83d-1232">Zadejte uživatelské jméno tooconnect toohello MySQL databázi.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1232">Specify user name tooconnect toohello MySQL database.</span></span> |<span data-ttu-id="5e83d-1233">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1233">Yes</span></span> |
| <span data-ttu-id="5e83d-1234">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1234">password</span></span> |<span data-ttu-id="5e83d-1235">Zadejte heslo pro hello uživatelského účtu, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1235">Specify password for hello user account you specified.</span></span> |<span data-ttu-id="5e83d-1236">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1236">Yes</span></span> |
| <span data-ttu-id="5e83d-1237">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1237">gatewayName</span></span> |<span data-ttu-id="5e83d-1238">Název hello brány, kterou hello služba Data Factory by měl použít toohello tooconnect, místní databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1238">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises MySQL database.</span></span> |<span data-ttu-id="5e83d-1239">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1239">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1240">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1240">Example</span></span>

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

<span data-ttu-id="5e83d-1241">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1241">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1242">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1242">Dataset</span></span>
<span data-ttu-id="5e83d-1243">toodefine MySQL datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1243">toodefine a MySQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1244">Property</span></span> | <span data-ttu-id="5e83d-1245">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1245">Description</span></span> | <span data-ttu-id="5e83d-1246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1247">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1247">tableName</span></span> |<span data-ttu-id="5e83d-1248">Název tabulky hello v hello instance databáze MySQL, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1248">Name of hello table in hello MySQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="5e83d-1249">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1249">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1250">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1250">Example</span></span>

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
<span data-ttu-id="5e83d-1251">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1251">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1252">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1252">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1253">Pokud kopírujete data z databáze MySQL, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1253">If you are copying data from a MySQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1254">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1254">Property</span></span> | <span data-ttu-id="5e83d-1255">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1255">Description</span></span> | <span data-ttu-id="5e83d-1256">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1256">Allowed values</span></span> | <span data-ttu-id="5e83d-1257">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1257">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1258">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1258">query</span></span> |<span data-ttu-id="5e83d-1259">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1259">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1260">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1260">SQL query string.</span></span> <span data-ttu-id="5e83d-1261">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1261">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-1262">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1262">No (if **tableName** of **dataset** is specified)</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-1263">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1263">Example</span></span>
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

<span data-ttu-id="5e83d-1264">Další informace najdete v tématu [MySQL konektor](data-factory-onprem-mysql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1264">For more information, see [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) article.</span></span> 

## <a name="oracle"></a><span data-ttu-id="5e83d-1265">Oracle</span><span class="sxs-lookup"><span data-stu-id="5e83d-1265">Oracle</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-1266">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1266">Linked service</span></span>
<span data-ttu-id="5e83d-1267">propojená služba, sada hello toodefine Oracle **typ** hello propojené služby příliš**OnPremisesOracle**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1267">toodefine an Oracle linked service, set hello **type** of hello linked service too**OnPremisesOracle**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1268">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1268">Property</span></span> | <span data-ttu-id="5e83d-1269">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1269">Description</span></span> | <span data-ttu-id="5e83d-1270">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1270">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1271">driverType</span><span class="sxs-lookup"><span data-stu-id="5e83d-1271">driverType</span></span> | <span data-ttu-id="5e83d-1272">Určete, jaká data toocopy toouse ovladače z / tooOracle databáze.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1272">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="5e83d-1273">Povolené hodnoty jsou **Microsoft** nebo **ODP** (výchozí).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1273">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="5e83d-1274">V tématu [podporované verze a instalace](#supported-versions-and-installation) části na podrobnosti o ovladači.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1274">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="5e83d-1275">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1275">No</span></span> |
| <span data-ttu-id="5e83d-1276">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1276">connectionString</span></span> | <span data-ttu-id="5e83d-1277">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello databáze Oracle instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1277">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="5e83d-1278">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1278">Yes</span></span> |
| <span data-ttu-id="5e83d-1279">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1279">gatewayName</span></span> | <span data-ttu-id="5e83d-1280">Název hello brány, který je použité tooconnect toohello místního serveru Oracle</span><span class="sxs-lookup"><span data-stu-id="5e83d-1280">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="5e83d-1281">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1281">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1282">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1282">Example</span></span>
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

<span data-ttu-id="5e83d-1283">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1283">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1284">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1284">Dataset</span></span>
<span data-ttu-id="5e83d-1285">toodefine datové sadě služby Oracle sadu hello **typ** sady dat hello příliš**OracleTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1285">toodefine an Oracle dataset, set hello **type** of hello dataset too**OracleTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1286">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1286">Property</span></span> | <span data-ttu-id="5e83d-1287">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1287">Description</span></span> | <span data-ttu-id="5e83d-1288">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1288">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1289">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1289">tableName</span></span> |<span data-ttu-id="5e83d-1290">Název tabulky hello v hello databáze Oracle, který hello propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1290">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="5e83d-1291">Ne (Pokud **oracleReaderQuery** z **OracleSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1291">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1292">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1292">Example</span></span>

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
<span data-ttu-id="5e83d-1293">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1293">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) article.</span></span>

### <a name="oracle-source-in-copy-activity"></a><span data-ttu-id="5e83d-1294">Zdroj Oracle v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1294">Oracle Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1295">Pokud kopírujete data z databáze Oracle, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**OracleSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1295">If you are copying data from an Oracle database, set hello **source type** of hello copy activity too**OracleSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1296">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1296">Property</span></span> | <span data-ttu-id="5e83d-1297">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1297">Description</span></span> | <span data-ttu-id="5e83d-1298">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1298">Allowed values</span></span> | <span data-ttu-id="5e83d-1299">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1299">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1300">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5e83d-1300">oracleReaderQuery</span></span> |<span data-ttu-id="5e83d-1301">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1301">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1302">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1302">SQL query string.</span></span> <span data-ttu-id="5e83d-1303">Příklad: `select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="5e83d-1303">For example: `select * from MyTable`</span></span> <br/><br/><span data-ttu-id="5e83d-1304">Pokud není zadaný, hello příkaz jazyka SQL, který se spustí:`select * from MyTable`</span><span class="sxs-lookup"><span data-stu-id="5e83d-1304">If not specified, hello SQL statement that is executed: `select * from MyTable`</span></span> |<span data-ttu-id="5e83d-1305">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1305">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1306">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1306">Example</span></span>

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

<span data-ttu-id="5e83d-1307">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1307">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

### <a name="oracle-sink-in-copy-activity"></a><span data-ttu-id="5e83d-1308">Podřízený Oracle v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1308">Oracle Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-1309">Pokud kopírujete databáze Oracle tooam dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**OracleSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1309">If you are copying data tooam Oracle database, set hello **sink type** of hello copy activity too**OracleSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-1310">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1310">Property</span></span> | <span data-ttu-id="5e83d-1311">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1311">Description</span></span> | <span data-ttu-id="5e83d-1312">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1312">Allowed values</span></span> | <span data-ttu-id="5e83d-1313">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1313">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1314">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-1314">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-1315">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1315">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="5e83d-1316">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-1316">timespan</span></span><br/><br/> <span data-ttu-id="5e83d-1317">Příklad: 00:30:00 (30 minut).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1317">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="5e83d-1318">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1318">No</span></span> |
| <span data-ttu-id="5e83d-1319">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-1319">writeBatchSize</span></span> |<span data-ttu-id="5e83d-1320">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1320">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="5e83d-1321">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1321">Integer (number of rows)</span></span> |<span data-ttu-id="5e83d-1322">Ne (výchozí: 100)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1322">No (default: 100)</span></span> |
| <span data-ttu-id="5e83d-1323">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5e83d-1323">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5e83d-1324">Zadejte dotaz aktivity kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1324">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="5e83d-1325">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1325">A query statement.</span></span> |<span data-ttu-id="5e83d-1326">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1326">No</span></span> |
| <span data-ttu-id="5e83d-1327">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1327">sliceIdentifierColumnName</span></span> |<span data-ttu-id="5e83d-1328">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1328">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="5e83d-1329">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1329">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="5e83d-1330">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1330">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1331">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1331">Example</span></span>
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
<span data-ttu-id="5e83d-1332">Další informace najdete v tématu [Oracle konektor](data-factory-onprem-oracle-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1332">For more information, see [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) article.</span></span>

## <a name="postgresql"></a><span data-ttu-id="5e83d-1333">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-1333">PostgreSQL</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1334">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1334">Linked service</span></span>
<span data-ttu-id="5e83d-1335">toodefine PostgreSQL propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesPostgreSql**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1335">toodefine a PostgreSQL linked service, set hello **type** of hello linked service too**OnPremisesPostgreSql**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1336">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1336">Property</span></span> | <span data-ttu-id="5e83d-1337">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1337">Description</span></span> | <span data-ttu-id="5e83d-1338">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1338">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1339">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1339">server</span></span> |<span data-ttu-id="5e83d-1340">Název serveru PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1340">Name of hello PostgreSQL server.</span></span> |<span data-ttu-id="5e83d-1341">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1341">Yes</span></span> |
| <span data-ttu-id="5e83d-1342">Databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1342">database</span></span> |<span data-ttu-id="5e83d-1343">Název databáze PostgreSQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1343">Name of hello PostgreSQL database.</span></span> |<span data-ttu-id="5e83d-1344">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1344">Yes</span></span> |
| <span data-ttu-id="5e83d-1345">Schéma</span><span class="sxs-lookup"><span data-stu-id="5e83d-1345">schema</span></span> |<span data-ttu-id="5e83d-1346">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1346">Name of hello schema in hello database.</span></span> <span data-ttu-id="5e83d-1347">název schématu Hello rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1347">hello schema name is case-sensitive.</span></span> |<span data-ttu-id="5e83d-1348">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1348">No</span></span> |
| <span data-ttu-id="5e83d-1349">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1349">authenticationType</span></span> |<span data-ttu-id="5e83d-1350">Typ ověřování používá databázi PostgreSQL toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1350">Type of authentication used tooconnect toohello PostgreSQL database.</span></span> <span data-ttu-id="5e83d-1351">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1351">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="5e83d-1352">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1352">Yes</span></span> |
| <span data-ttu-id="5e83d-1353">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1353">username</span></span> |<span data-ttu-id="5e83d-1354">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1354">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="5e83d-1355">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1355">No</span></span> |
| <span data-ttu-id="5e83d-1356">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1356">password</span></span> |<span data-ttu-id="5e83d-1357">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1357">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-1358">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1358">No</span></span> |
| <span data-ttu-id="5e83d-1359">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1359">gatewayName</span></span> |<span data-ttu-id="5e83d-1360">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1360">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises PostgreSQL database.</span></span> |<span data-ttu-id="5e83d-1361">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1361">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1362">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1362">Example</span></span>

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
<span data-ttu-id="5e83d-1363">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1363">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1364">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1364">Dataset</span></span>
<span data-ttu-id="5e83d-1365">toodefine PostgreSQL datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1365">toodefine a PostgreSQL dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1366">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1366">Property</span></span> | <span data-ttu-id="5e83d-1367">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1367">Description</span></span> | <span data-ttu-id="5e83d-1368">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1368">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1369">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1369">tableName</span></span> |<span data-ttu-id="5e83d-1370">Název tabulky hello v hello instance databáze PostgreSQL, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1370">Name of hello table in hello PostgreSQL Database instance that linked service refers to.</span></span> <span data-ttu-id="5e83d-1371">Hello tableName rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1371">hello tableName is case-sensitive.</span></span> |<span data-ttu-id="5e83d-1372">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1372">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1373">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1373">Example</span></span>
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
<span data-ttu-id="5e83d-1374">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1374">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1375">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1375">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1376">Pokud kopírujete data z databáze PostgreSQL, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1376">If you are copying data from a PostgreSQL database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1377">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1377">Property</span></span> | <span data-ttu-id="5e83d-1378">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1378">Description</span></span> | <span data-ttu-id="5e83d-1379">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1379">Allowed values</span></span> | <span data-ttu-id="5e83d-1380">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1380">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1381">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1381">query</span></span> |<span data-ttu-id="5e83d-1382">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1382">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1383">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1383">SQL query string.</span></span> <span data-ttu-id="5e83d-1384">Například: "dotaz": "vybrat * z \"MySchema\".\" MyTable\"".</span><span class="sxs-lookup"><span data-stu-id="5e83d-1384">For example: "query": "select * from \"MySchema\".\"MyTable\"".</span></span> |<span data-ttu-id="5e83d-1385">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1385">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1386">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1386">Example</span></span>

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

<span data-ttu-id="5e83d-1387">Další informace najdete v tématu [PostgreSQL konektor](data-factory-onprem-postgresql-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1387">For more information, see [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) article.</span></span>

## <a name="sap-business-warehouse"></a><span data-ttu-id="5e83d-1388">SAP Business Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-1388">SAP Business Warehouse</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-1389">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1389">Linked service</span></span>
<span data-ttu-id="5e83d-1390">propojená služba, sada hello toodefine SAP Business Warehouse (BW) **typ** hello propojené služby příliš**SapBw**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1390">toodefine a SAP Business Warehouse (BW) linked service, set hello **type** of hello linked service too**SapBw**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="5e83d-1391">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1391">Property</span></span> | <span data-ttu-id="5e83d-1392">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1392">Description</span></span> | <span data-ttu-id="5e83d-1393">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1393">Allowed values</span></span> | <span data-ttu-id="5e83d-1394">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1394">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="5e83d-1395">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1395">server</span></span> | <span data-ttu-id="5e83d-1396">Název hello serveru, na které hello SAP BW nachází instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1396">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="5e83d-1397">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1397">string</span></span> | <span data-ttu-id="5e83d-1398">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1398">Yes</span></span>
<span data-ttu-id="5e83d-1399">systemNumber</span><span class="sxs-lookup"><span data-stu-id="5e83d-1399">systemNumber</span></span> | <span data-ttu-id="5e83d-1400">Systém počet hello systému SAP BW.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1400">System number of hello SAP BW system.</span></span> | <span data-ttu-id="5e83d-1401">Desetinné číslo letopočty řetězec.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1401">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="5e83d-1402">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1402">Yes</span></span>
<span data-ttu-id="5e83d-1403">clientId</span><span class="sxs-lookup"><span data-stu-id="5e83d-1403">clientId</span></span> | <span data-ttu-id="5e83d-1404">ID klienta hello klienta v hello SAP W systému.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1404">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="5e83d-1405">Tři číslice desítkové číslo řetězec.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1405">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="5e83d-1406">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1406">Yes</span></span>
<span data-ttu-id="5e83d-1407">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1407">username</span></span> | <span data-ttu-id="5e83d-1408">Název hello uživatele, který má přístup k serveru SAP toohello</span><span class="sxs-lookup"><span data-stu-id="5e83d-1408">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="5e83d-1409">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1409">string</span></span> | <span data-ttu-id="5e83d-1410">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1410">Yes</span></span>
<span data-ttu-id="5e83d-1411">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1411">password</span></span> | <span data-ttu-id="5e83d-1412">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1412">Password for hello user.</span></span> | <span data-ttu-id="5e83d-1413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1413">string</span></span> | <span data-ttu-id="5e83d-1414">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1414">Yes</span></span>
<span data-ttu-id="5e83d-1415">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1415">gatewayName</span></span> | <span data-ttu-id="5e83d-1416">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP BW instancí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1416">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="5e83d-1417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1417">string</span></span> | <span data-ttu-id="5e83d-1418">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1418">Yes</span></span>
<span data-ttu-id="5e83d-1419">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1419">encryptedCredential</span></span> | <span data-ttu-id="5e83d-1420">Hello šifrovaný řetězec přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1420">hello encrypted credential string.</span></span> | <span data-ttu-id="5e83d-1421">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1421">string</span></span> | <span data-ttu-id="5e83d-1422">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1422">No</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-1423">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1423">Example</span></span>

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

<span data-ttu-id="5e83d-1424">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1424">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1425">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1425">Dataset</span></span>
<span data-ttu-id="5e83d-1426">toodefine SAP BW datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1426">toodefine a SAP BW dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="5e83d-1427">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP BW hello typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1427">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span>  

#### <a name="example"></a><span data-ttu-id="5e83d-1428">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1428">Example</span></span>

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
<span data-ttu-id="5e83d-1429">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1429">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1430">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1430">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1431">Pokud jsou kopírování dat z SAP Business Warehouse, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1431">If you are copying data from SAP Business Warehouse, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1432">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1432">Property</span></span> | <span data-ttu-id="5e83d-1433">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1433">Description</span></span> | <span data-ttu-id="5e83d-1434">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1434">Allowed values</span></span> | <span data-ttu-id="5e83d-1435">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1435">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1436">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1436">query</span></span> | <span data-ttu-id="5e83d-1437">Určuje hello MDX dotazu tooread data z instance SAP BW hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1437">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="5e83d-1438">Dotaz MDX.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1438">MDX query.</span></span> | <span data-ttu-id="5e83d-1439">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1439">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1440">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1440">Example</span></span>

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

<span data-ttu-id="5e83d-1441">Další informace najdete v tématu [SAP Business Warehouse konektor](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1441">For more information, see [SAP Business Warehouse connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sap-hana"></a><span data-ttu-id="5e83d-1442">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="5e83d-1442">SAP HANA</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1443">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1443">Linked service</span></span>
<span data-ttu-id="5e83d-1444">propojená služba, sada hello toodefine SAP HANA **typ** hello propojené služby příliš**SapHana**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1444">toodefine a SAP HANA linked service, set hello **type** of hello linked service too**SapHana**, and specify following properties in hello **typeProperties** section:</span></span>  

<span data-ttu-id="5e83d-1445">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1445">Property</span></span> | <span data-ttu-id="5e83d-1446">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1446">Description</span></span> | <span data-ttu-id="5e83d-1447">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1447">Allowed values</span></span> | <span data-ttu-id="5e83d-1448">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1448">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="5e83d-1449">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1449">server</span></span> | <span data-ttu-id="5e83d-1450">Název hello serveru, na které hello SAP HANA nachází instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1450">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="5e83d-1451">Pokud váš server používá vlastní port, zadejte `server:port`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1451">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="5e83d-1452">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1452">string</span></span> | <span data-ttu-id="5e83d-1453">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1453">Yes</span></span>
<span data-ttu-id="5e83d-1454">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1454">authenticationType</span></span> | <span data-ttu-id="5e83d-1455">Typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1455">Type of authentication.</span></span> | <span data-ttu-id="5e83d-1456">Řetězec.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1456">string.</span></span> <span data-ttu-id="5e83d-1457">"Základní" nebo "Systém Windows"</span><span class="sxs-lookup"><span data-stu-id="5e83d-1457">"Basic" or "Windows"</span></span> | <span data-ttu-id="5e83d-1458">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1458">Yes</span></span> 
<span data-ttu-id="5e83d-1459">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1459">username</span></span> | <span data-ttu-id="5e83d-1460">Název hello uživatele, který má přístup k serveru SAP toohello</span><span class="sxs-lookup"><span data-stu-id="5e83d-1460">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="5e83d-1461">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1461">string</span></span> | <span data-ttu-id="5e83d-1462">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1462">Yes</span></span>
<span data-ttu-id="5e83d-1463">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1463">password</span></span> | <span data-ttu-id="5e83d-1464">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1464">Password for hello user.</span></span> | <span data-ttu-id="5e83d-1465">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1465">string</span></span> | <span data-ttu-id="5e83d-1466">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1466">Yes</span></span>
<span data-ttu-id="5e83d-1467">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1467">gatewayName</span></span> | <span data-ttu-id="5e83d-1468">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní SAP HANA instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1468">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="5e83d-1469">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1469">string</span></span> | <span data-ttu-id="5e83d-1470">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1470">Yes</span></span>
<span data-ttu-id="5e83d-1471">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1471">encryptedCredential</span></span> | <span data-ttu-id="5e83d-1472">Hello šifrovaný řetězec přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1472">hello encrypted credential string.</span></span> | <span data-ttu-id="5e83d-1473">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1473">string</span></span> | <span data-ttu-id="5e83d-1474">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1474">No</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-1475">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1475">Example</span></span>

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
<span data-ttu-id="5e83d-1476">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1476">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#linked-service-properties) article.</span></span>
 
### <a name="dataset"></a><span data-ttu-id="5e83d-1477">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1477">Dataset</span></span>
<span data-ttu-id="5e83d-1478">toodefine SAP HANA datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1478">toodefine a SAP HANA dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="5e83d-1479">Nejsou k dispozici žádné vlastnosti specifické pro typ podporované pro datovou sadu SAP HANA hello typu **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1479">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 

#### <a name="example"></a><span data-ttu-id="5e83d-1480">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1480">Example</span></span>

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
<span data-ttu-id="5e83d-1481">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1481">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1482">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1482">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1483">Pokud jsou kopírování dat z jiného úložiště dat SAP HANA, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1483">If you are copying data from a SAP HANA data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1484">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1484">Property</span></span> | <span data-ttu-id="5e83d-1485">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1485">Description</span></span> | <span data-ttu-id="5e83d-1486">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1486">Allowed values</span></span> | <span data-ttu-id="5e83d-1487">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1487">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1488">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1488">query</span></span> | <span data-ttu-id="5e83d-1489">Určuje hello SQL dotaz tooread data z instance SAP HANA hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1489">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="5e83d-1490">Dotaz SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1490">SQL query.</span></span> | <span data-ttu-id="5e83d-1491">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1491">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-1492">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1492">Example</span></span>


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

<span data-ttu-id="5e83d-1493">Další informace najdete v tématu [SAP HANA konektor](data-factory-sap-hana-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1493">For more information, see [SAP HANA connector](data-factory-sap-hana-connector.md#copy-activity-properties) article.</span></span>


## <a name="sql-server"></a><span data-ttu-id="5e83d-1494">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1494">SQL Server</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1495">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1495">Linked service</span></span>
<span data-ttu-id="5e83d-1496">Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1496">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="5e83d-1497">Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1497">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="5e83d-1498">Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1498">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="5e83d-1499">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1499">Property</span></span> | <span data-ttu-id="5e83d-1500">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1500">Description</span></span> | <span data-ttu-id="5e83d-1501">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1501">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1502">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-1502">type</span></span> |<span data-ttu-id="5e83d-1503">vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1503">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="5e83d-1504">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1504">Yes</span></span> |
| <span data-ttu-id="5e83d-1505">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1505">connectionString</span></span> |<span data-ttu-id="5e83d-1506">Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1506">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="5e83d-1507">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1507">Yes</span></span> |
| <span data-ttu-id="5e83d-1508">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1508">gatewayName</span></span> |<span data-ttu-id="5e83d-1509">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1509">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="5e83d-1510">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1510">Yes</span></span> |
| <span data-ttu-id="5e83d-1511">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1511">username</span></span> |<span data-ttu-id="5e83d-1512">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1512">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="5e83d-1513">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1513">Example: **domainname\\username**.</span></span> |<span data-ttu-id="5e83d-1514">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1514">No</span></span> |
| <span data-ttu-id="5e83d-1515">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1515">password</span></span> |<span data-ttu-id="5e83d-1516">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1516">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-1517">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1517">No</span></span> |

<span data-ttu-id="5e83d-1518">Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="5e83d-1518">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="5e83d-1519">Příklad: JSON pro pomocí ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1519">Example: JSON for using SQL Authentication</span></span>

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
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="5e83d-1520">Příklad: JSON pro použití ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="5e83d-1520">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="5e83d-1521">Pokud jsou zadané uživatelské jméno a heslo, brána používá, je tooimpersonate hello zadaný uživatelský účet tooconnect toohello místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1521">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="5e83d-1522">Jinak brána připojí toohello systému SQL Server přímo kontext zabezpečení hello brány (jeho účet spuštění).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1522">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

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

<span data-ttu-id="5e83d-1523">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1523">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1524">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1524">Dataset</span></span>
<span data-ttu-id="5e83d-1525">toodefine datové sady SQL Server, sada hello **typ** sady dat hello příliš**SqlServerTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1525">toodefine a SQL Server dataset, set hello **type** of hello dataset too**SqlServerTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1526">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1526">Property</span></span> | <span data-ttu-id="5e83d-1527">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1527">Description</span></span> | <span data-ttu-id="5e83d-1528">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1528">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1529">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1529">tableName</span></span> |<span data-ttu-id="5e83d-1530">Název hello tabulku nebo zobrazení v hello instance databáze SQL serveru, který propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1530">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="5e83d-1531">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1531">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1532">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1532">Example</span></span>
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

<span data-ttu-id="5e83d-1533">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1533">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#dataset-properties) article.</span></span> 

### <a name="sql-source-in-copy-activity"></a><span data-ttu-id="5e83d-1534">Zdroje SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1534">Sql Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1535">Pokud kopírujete data z databáze systému SQL Server, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**SqlSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1535">If you are copying data from a SQL Server database, set hello **source type** of hello copy activity too**SqlSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1536">Property</span></span> | <span data-ttu-id="5e83d-1537">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1537">Description</span></span> | <span data-ttu-id="5e83d-1538">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1538">Allowed values</span></span> | <span data-ttu-id="5e83d-1539">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1539">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1540">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5e83d-1540">sqlReaderQuery</span></span> |<span data-ttu-id="5e83d-1541">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1541">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1542">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1542">SQL query string.</span></span> <span data-ttu-id="5e83d-1543">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1543">For example: `select * from MyTable`.</span></span> <span data-ttu-id="5e83d-1544">Může odkazovat více tabulek z databáze hello odkazuje hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1544">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="5e83d-1545">Pokud není zadaný, hello příkaz jazyka SQL, která se provedla: Vyberte možnost z MyTable.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1545">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="5e83d-1546">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1546">No</span></span> |
| <span data-ttu-id="5e83d-1547">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1547">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="5e83d-1548">Název hello uložené procedury, která čte data z hello zdrojové tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1548">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="5e83d-1549">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1549">Name of hello stored procedure.</span></span> |<span data-ttu-id="5e83d-1550">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1550">No</span></span> |
| <span data-ttu-id="5e83d-1551">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-1551">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-1552">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1552">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5e83d-1553">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1553">Name/value pairs.</span></span> <span data-ttu-id="5e83d-1554">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1554">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5e83d-1555">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1555">No</span></span> |

<span data-ttu-id="5e83d-1556">Pokud hello **sqlReaderQuery** je zadán pro hello SqlSource, hello aktivity kopírování spouští tento dotaz hello databáze systému SQL Server zdrojová tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1556">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="5e83d-1557">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1557">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="5e83d-1558">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1558">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="5e83d-1559">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1559">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="5e83d-1560">Při použití **sqlReaderStoredProcedureName**, stále potřebujete toospecify hodnotu pro hello **tableName** vlastnost v datové sadě hello JSON.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1560">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="5e83d-1561">Neexistují žádné ověření, ale adresovat této tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1561">There are no validations performed against this table though.</span></span>


#### <a name="example"></a><span data-ttu-id="5e83d-1562">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1562">Example</span></span>
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

<span data-ttu-id="5e83d-1563">V tomto příkladu **sqlReaderQuery** pro hello SqlSource je zadána.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1563">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="5e83d-1564">Hello aktivity kopírování spouští tento dotaz hello data hello tooget zdrojové databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1564">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="5e83d-1565">Alternativně můžete zadat uložené procedury zadáním hello **sqlReaderStoredProcedureName** a **storedProcedureParameters** (Pokud hello uložená procedura používá parametry).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1565">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="5e83d-1566">Hello sqlReaderQuery může odkazovat více tabulek v rámci hello databáze odkazuje hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1566">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="5e83d-1567">Není omezený tooonly hello tabulky nastavena jako hello typeProperty tableName datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1567">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="5e83d-1568">Pokud nezadáte sqlReaderQuery nebo sqlReaderStoredProcedureName, hello sloupce definovaný v oddílu Struktura hello jsou použité toobuild dotaz select toorun proti hello databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1568">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="5e83d-1569">Pokud definice datové sady hello nemá hello struktura, vyberou se všechny sloupce z tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1569">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="5e83d-1570">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1570">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

### <a name="sql-sink-in-copy-activity"></a><span data-ttu-id="5e83d-1571">Jímku SQL v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1571">Sql Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-1572">Pokud kopírujete databáze systému SQL Server tooa dat, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**SqlSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1572">If you are copying data tooa SQL Server database, set hello **sink type** of hello copy activity too**SqlSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-1573">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1573">Property</span></span> | <span data-ttu-id="5e83d-1574">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1574">Description</span></span> | <span data-ttu-id="5e83d-1575">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1575">Allowed values</span></span> | <span data-ttu-id="5e83d-1576">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1576">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1577">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-1577">writeBatchTimeout</span></span> |<span data-ttu-id="5e83d-1578">Doba pro toocomplete operaci vložení dávky hello Počkejte, než vyprší časový limit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1578">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="5e83d-1579">Časový interval</span><span class="sxs-lookup"><span data-stu-id="5e83d-1579">timespan</span></span><br/><br/> <span data-ttu-id="5e83d-1580">Příklad: "00: 30:00" (30 minut).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1580">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5e83d-1581">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1581">No</span></span> |
| <span data-ttu-id="5e83d-1582">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-1582">writeBatchSize</span></span> |<span data-ttu-id="5e83d-1583">Pokud velikost vyrovnávací paměti hello dosáhne writeBatchSize vkládá data do tabulky SQL hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1583">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="5e83d-1584">Celé číslo (počet řádků)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1584">Integer (number of rows)</span></span> |<span data-ttu-id="5e83d-1585">Ne (výchozí: 10000)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1585">No (default: 10000)</span></span> |
| <span data-ttu-id="5e83d-1586">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5e83d-1586">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5e83d-1587">Zadejte dotaz pro aktivitu kopírování tooexecute tak, aby se vyčistit data určitý řez.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1587">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="5e83d-1588">Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1588">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="5e83d-1589">Příkaz dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1589">A query statement.</span></span> |<span data-ttu-id="5e83d-1590">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1590">No</span></span> |
| <span data-ttu-id="5e83d-1591">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1591">sliceIdentifierColumnName</span></span> |<span data-ttu-id="5e83d-1592">Zadejte název sloupce pro aktivitu kopírování toofill s identifikátorem automaticky generovány řez, což je použité tooclean data určitý řez, pokud znovu spustit.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1592">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="5e83d-1593">Další informace najdete v tématu [opakovatelnosti](#repeatability-during-copy) části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1593">For more information, see [repeatability](#repeatability-during-copy) section.</span></span> |<span data-ttu-id="5e83d-1594">Název sloupce sloupce s datovým typem binary(32).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1594">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="5e83d-1595">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1595">No</span></span> |
| <span data-ttu-id="5e83d-1596">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1596">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="5e83d-1597">Název hello uložené procedury upserts (aktualizace nebo vložení) dat do cílové tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1597">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="5e83d-1598">Název hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1598">Name of hello stored procedure.</span></span> |<span data-ttu-id="5e83d-1599">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1599">No</span></span> |
| <span data-ttu-id="5e83d-1600">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-1600">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-1601">Parametry pro hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1601">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="5e83d-1602">Páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1602">Name/value pairs.</span></span> <span data-ttu-id="5e83d-1603">Názvy a malá a velká písmena parametry musí odpovídat názvům hello a malá a velká písmena parametry hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1603">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="5e83d-1604">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1604">No</span></span> |
| <span data-ttu-id="5e83d-1605">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="5e83d-1605">sqlWriterTableType</span></span> |<span data-ttu-id="5e83d-1606">Zadejte toobe název typu tabulky používán hello uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1606">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="5e83d-1607">Aktivita kopírování zpřístupní přesouvání dat hello v dočasné tabulce s tímto typem tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1607">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="5e83d-1608">Uložená procedura kódu můžete pak sloučit data hello kopírovány s existujícími daty.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1608">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="5e83d-1609">Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1609">A table type name.</span></span> |<span data-ttu-id="5e83d-1610">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1610">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1611">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1611">Example</span></span>
<span data-ttu-id="5e83d-1612">Hello kanál obsahuje aktivitu kopírování, je nakonfigurovaná toouse tyto vstupní a výstupní datové sady a je naplánované toorun každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1612">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="5e83d-1613">V kanálu hello definici JSON, hello **zdroj** je typ nastaven příliš**BlobSource** a **podřízený** je typ nastaven příliš**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1613">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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

<span data-ttu-id="5e83d-1614">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1614">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#copy-activity-properties) article.</span></span> 

## <a name="sybase"></a><span data-ttu-id="5e83d-1615">Sybase</span><span class="sxs-lookup"><span data-stu-id="5e83d-1615">Sybase</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1616">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1616">Linked service</span></span>
<span data-ttu-id="5e83d-1617">toodefine Sybase propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesSybase**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1617">toodefine a Sybase linked service, set hello **type** of hello linked service too**OnPremisesSybase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1618">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1618">Property</span></span> | <span data-ttu-id="5e83d-1619">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1619">Description</span></span> | <span data-ttu-id="5e83d-1620">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1620">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1621">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1621">server</span></span> |<span data-ttu-id="5e83d-1622">Název serveru Sybase hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1622">Name of hello Sybase server.</span></span> |<span data-ttu-id="5e83d-1623">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1623">Yes</span></span> |
| <span data-ttu-id="5e83d-1624">Databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1624">database</span></span> |<span data-ttu-id="5e83d-1625">Název databáze Sybase hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1625">Name of hello Sybase database.</span></span> |<span data-ttu-id="5e83d-1626">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1626">Yes</span></span> |
| <span data-ttu-id="5e83d-1627">Schéma</span><span class="sxs-lookup"><span data-stu-id="5e83d-1627">schema</span></span> |<span data-ttu-id="5e83d-1628">Název schématu hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1628">Name of hello schema in hello database.</span></span> |<span data-ttu-id="5e83d-1629">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1629">No</span></span> |
| <span data-ttu-id="5e83d-1630">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1630">authenticationType</span></span> |<span data-ttu-id="5e83d-1631">Typ ověřování používá databázi Sybase toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1631">Type of authentication used tooconnect toohello Sybase database.</span></span> <span data-ttu-id="5e83d-1632">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1632">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="5e83d-1633">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1633">Yes</span></span> |
| <span data-ttu-id="5e83d-1634">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1634">username</span></span> |<span data-ttu-id="5e83d-1635">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1635">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="5e83d-1636">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1636">No</span></span> |
| <span data-ttu-id="5e83d-1637">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1637">password</span></span> |<span data-ttu-id="5e83d-1638">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1638">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-1639">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1639">No</span></span> |
| <span data-ttu-id="5e83d-1640">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1640">gatewayName</span></span> |<span data-ttu-id="5e83d-1641">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi Sybase.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1641">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Sybase database.</span></span> |<span data-ttu-id="5e83d-1642">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1642">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1643">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1643">Example</span></span>
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

<span data-ttu-id="5e83d-1644">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1644">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1645">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1645">Dataset</span></span>
<span data-ttu-id="5e83d-1646">toodefine Sybase datovou sadu, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1646">toodefine a Sybase dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1647">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1647">Property</span></span> | <span data-ttu-id="5e83d-1648">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1648">Description</span></span> | <span data-ttu-id="5e83d-1649">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1649">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1650">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1650">tableName</span></span> |<span data-ttu-id="5e83d-1651">Název tabulky hello v hello instance databáze Sybase, která je propojená služba odkazuje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1651">Name of hello table in hello Sybase Database instance that linked service refers to.</span></span> |<span data-ttu-id="5e83d-1652">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1652">No (if **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1653">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1653">Example</span></span>

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

<span data-ttu-id="5e83d-1654">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1654">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1655">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1655">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1656">Pokud kopírujete data z databáze Sybase, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1656">If you are copying data from a Sybase database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1657">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1657">Property</span></span> | <span data-ttu-id="5e83d-1658">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1658">Description</span></span> | <span data-ttu-id="5e83d-1659">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1659">Allowed values</span></span> | <span data-ttu-id="5e83d-1660">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1660">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1661">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1661">query</span></span> |<span data-ttu-id="5e83d-1662">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1662">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1663">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1663">SQL query string.</span></span> <span data-ttu-id="5e83d-1664">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1664">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-1665">Ne (Pokud **tableName** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1665">No (if **tableName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1666">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1666">Example</span></span>

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

<span data-ttu-id="5e83d-1667">Další informace najdete v tématu [Sybase konektor](data-factory-onprem-sybase-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1667">For more information, see [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) article.</span></span>

## <a name="teradata"></a><span data-ttu-id="5e83d-1668">Teradata</span><span class="sxs-lookup"><span data-stu-id="5e83d-1668">Teradata</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1669">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1669">Linked service</span></span>
<span data-ttu-id="5e83d-1670">propojená služba, sada hello toodefine Teradata **typ** hello propojené služby příliš**OnPremisesTeradata**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1670">toodefine a Teradata linked service, set hello **type** of hello linked service too**OnPremisesTeradata**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1671">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1671">Property</span></span> | <span data-ttu-id="5e83d-1672">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1672">Description</span></span> | <span data-ttu-id="5e83d-1673">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1673">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1674">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1674">server</span></span> |<span data-ttu-id="5e83d-1675">Název serveru Teradata hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1675">Name of hello Teradata server.</span></span> |<span data-ttu-id="5e83d-1676">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1676">Yes</span></span> |
| <span data-ttu-id="5e83d-1677">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1677">authenticationType</span></span> |<span data-ttu-id="5e83d-1678">Typ ověřování používá tooconnect toohello Teradata databáze.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1678">Type of authentication used tooconnect toohello Teradata database.</span></span> <span data-ttu-id="5e83d-1679">Možné hodnoty jsou: anonymní, základní a systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1679">Possible values are: Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="5e83d-1680">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1680">Yes</span></span> |
| <span data-ttu-id="5e83d-1681">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1681">username</span></span> |<span data-ttu-id="5e83d-1682">Pokud používáte ověřování Basic nebo Windows, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1682">Specify user name if you are using Basic or Windows authentication.</span></span> |<span data-ttu-id="5e83d-1683">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1683">No</span></span> |
| <span data-ttu-id="5e83d-1684">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1684">password</span></span> |<span data-ttu-id="5e83d-1685">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1685">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-1686">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1686">No</span></span> |
| <span data-ttu-id="5e83d-1687">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1687">gatewayName</span></span> |<span data-ttu-id="5e83d-1688">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello místní Teradata databázi.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1688">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises Teradata database.</span></span> |<span data-ttu-id="5e83d-1689">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1689">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1690">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1690">Example</span></span>
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

<span data-ttu-id="5e83d-1691">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1691">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1692">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1692">Dataset</span></span>
<span data-ttu-id="5e83d-1693">toodefine datovou sadu objektu Teradata Blob, sada hello **typ** sady dat hello příliš**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1693">toodefine a Teradata Blob dataset, set hello **type** of hello dataset too**RelationalTable**.</span></span> <span data-ttu-id="5e83d-1694">Momentálně nejsou k dispozici žádné vlastnosti typu podporované pro datovou sadu hello Teradata.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1694">Currently, there are no type properties supported for hello Teradata dataset.</span></span> 

#### <a name="example"></a><span data-ttu-id="5e83d-1695">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1695">Example</span></span>
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

<span data-ttu-id="5e83d-1696">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1696">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-1697">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1697">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1698">Pokud kopírujete data z databáze Teradata, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1698">If you are copying data from a Teradata database, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1699">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1699">Property</span></span> | <span data-ttu-id="5e83d-1700">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1700">Description</span></span> | <span data-ttu-id="5e83d-1701">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1701">Allowed values</span></span> | <span data-ttu-id="5e83d-1702">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1702">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1703">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1703">query</span></span> |<span data-ttu-id="5e83d-1704">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1704">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1705">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1705">SQL query string.</span></span> <span data-ttu-id="5e83d-1706">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1706">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-1707">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1707">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1708">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1708">Example</span></span>

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

<span data-ttu-id="5e83d-1709">Další informace najdete v tématu [Teradata konektor](data-factory-onprem-teradata-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1709">For more information, see [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) article.</span></span>

## <a name="cassandra"></a><span data-ttu-id="5e83d-1710">Cassandra</span><span class="sxs-lookup"><span data-stu-id="5e83d-1710">Cassandra</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-1711">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1711">Linked service</span></span>
<span data-ttu-id="5e83d-1712">toodefine Cassandra propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesCassandra**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1712">toodefine a Cassandra linked service, set hello **type** of hello linked service too**OnPremisesCassandra**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1713">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1713">Property</span></span> | <span data-ttu-id="5e83d-1714">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1714">Description</span></span> | <span data-ttu-id="5e83d-1715">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1715">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1716">hostitele</span><span class="sxs-lookup"><span data-stu-id="5e83d-1716">host</span></span> |<span data-ttu-id="5e83d-1717">Jeden nebo více IP adres nebo názvů hostitelů Cassandra serverů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1717">One or more IP addresses or host names of Cassandra servers.</span></span><br/><br/><span data-ttu-id="5e83d-1718">Zadejte seznam IP adres nebo serverům tooall tooconnect názvy hostitele současně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1718">Specify a comma-separated list of IP addresses or host names tooconnect tooall servers concurrently.</span></span> |<span data-ttu-id="5e83d-1719">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1719">Yes</span></span> |
| <span data-ttu-id="5e83d-1720">port</span><span class="sxs-lookup"><span data-stu-id="5e83d-1720">port</span></span> |<span data-ttu-id="5e83d-1721">port TCP, který hello Cassandra server Hello používá toolisten pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1721">hello TCP port that hello Cassandra server uses toolisten for client connections.</span></span> |<span data-ttu-id="5e83d-1722">Ne, výchozí hodnota: 9042</span><span class="sxs-lookup"><span data-stu-id="5e83d-1722">No, default value: 9042</span></span> |
| <span data-ttu-id="5e83d-1723">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1723">authenticationType</span></span> |<span data-ttu-id="5e83d-1724">Basic nebo Anonymous</span><span class="sxs-lookup"><span data-stu-id="5e83d-1724">Basic, or Anonymous</span></span> |<span data-ttu-id="5e83d-1725">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1725">Yes</span></span> |
| <span data-ttu-id="5e83d-1726">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1726">username</span></span> |<span data-ttu-id="5e83d-1727">Zadejte uživatelské jméno pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1727">Specify user name for hello user account.</span></span> |<span data-ttu-id="5e83d-1728">Ano, pokud je nastavená authenticationType tooBasic.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1728">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="5e83d-1729">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1729">password</span></span> |<span data-ttu-id="5e83d-1730">Zadejte heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1730">Specify password for hello user account.</span></span> |<span data-ttu-id="5e83d-1731">Ano, pokud je nastavená authenticationType tooBasic.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1731">Yes, if authenticationType is set tooBasic.</span></span> |
| <span data-ttu-id="5e83d-1732">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1732">gatewayName</span></span> |<span data-ttu-id="5e83d-1733">Název Hello hello bránu, která je použité tooconnect toohello místní Cassandra databázi.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1733">hello name of hello gateway that is used tooconnect toohello on-premises Cassandra database.</span></span> |<span data-ttu-id="5e83d-1734">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1734">Yes</span></span> |
| <span data-ttu-id="5e83d-1735">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1735">encryptedCredential</span></span> |<span data-ttu-id="5e83d-1736">Přihlašovací údaje šifrované bránou hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1736">Credential encrypted by hello gateway.</span></span> |<span data-ttu-id="5e83d-1737">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1737">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1738">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1738">Example</span></span>

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

<span data-ttu-id="5e83d-1739">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1739">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-1740">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1740">Dataset</span></span>
<span data-ttu-id="5e83d-1741">toodefine Cassandra datovou sadu, sada hello **typ** sady dat hello příliš**CassandraTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1741">toodefine a Cassandra dataset, set hello **type** of hello dataset too**CassandraTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1742">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1742">Property</span></span> | <span data-ttu-id="5e83d-1743">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1743">Description</span></span> | <span data-ttu-id="5e83d-1744">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1744">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1745">keyspace</span><span class="sxs-lookup"><span data-stu-id="5e83d-1745">keyspace</span></span> |<span data-ttu-id="5e83d-1746">Název schématu v databázi Cassandra nebo hello keyspace.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1746">Name of hello keyspace or schema in Cassandra database.</span></span> |<span data-ttu-id="5e83d-1747">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1747">Yes (If **query** for **CassandraSource** is not defined).</span></span> |
| <span data-ttu-id="5e83d-1748">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1748">tableName</span></span> |<span data-ttu-id="5e83d-1749">Název hello tabulky v databázi Cassandra.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1749">Name of hello table in Cassandra database.</span></span> |<span data-ttu-id="5e83d-1750">Ano (Pokud **dotazu** pro **CassandraSource** není definován).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1750">Yes (If **query** for **CassandraSource** is not defined).</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1751">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1751">Example</span></span>

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

<span data-ttu-id="5e83d-1752">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1752">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) article.</span></span> 

### <a name="cassandra-source-in-copy-activity"></a><span data-ttu-id="5e83d-1753">Zdroj Cassandra v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1753">Cassandra Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1754">Pokud jsou kopírování dat z Cassandra, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**CassandraSource**a zadejte následující vlastnosti v hello **zdroj** části :</span><span class="sxs-lookup"><span data-stu-id="5e83d-1754">If you are copying data from Cassandra, set hello **source type** of hello copy activity too**CassandraSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1755">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1755">Property</span></span> | <span data-ttu-id="5e83d-1756">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1756">Description</span></span> | <span data-ttu-id="5e83d-1757">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1757">Allowed values</span></span> | <span data-ttu-id="5e83d-1758">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1758">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1759">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1759">query</span></span> |<span data-ttu-id="5e83d-1760">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1760">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1761">Dotaz SQL 92 nebo CQL dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1761">SQL-92 query or CQL query.</span></span> <span data-ttu-id="5e83d-1762">V tématu [CQL odkaz](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1762">See [CQL reference](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html).</span></span> <br/><br/><span data-ttu-id="5e83d-1763">Při použití příkazu jazyka SQL, zadejte **keyspace name.table název** toorepresent hello tabulky, které mají být tooquery.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1763">When using SQL query, specify **keyspace name.table name** toorepresent hello table you want tooquery.</span></span> |<span data-ttu-id="5e83d-1764">Ne (pokud jsou definovány tableName a keyspace v sadě dat).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1764">No (if tableName and keyspace on dataset are defined).</span></span> |
| <span data-ttu-id="5e83d-1765">consistencyLevel</span><span class="sxs-lookup"><span data-stu-id="5e83d-1765">consistencyLevel</span></span> |<span data-ttu-id="5e83d-1766">úroveň konzistence Hello Určuje, kolik repliky musí odpovídat požadavků na čtení tooa před vrácením dat toohello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1766">hello consistency level specifies how many replicas must respond tooa read request before returning data toohello client application.</span></span> <span data-ttu-id="5e83d-1767">Kontroly Cassandra hello zadaný počet replik pro data toosatisfy hello čtení požadavku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1767">Cassandra checks hello specified number of replicas for data toosatisfy hello read request.</span></span> |<span data-ttu-id="5e83d-1768">JEDEN, DVA, TŘI, KVORA, VŠE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1768">ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE.</span></span> <span data-ttu-id="5e83d-1769">V tématu [konfigurace konzistenci dat](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1769">See [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) for details.</span></span> |<span data-ttu-id="5e83d-1770">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1770">No.</span></span> <span data-ttu-id="5e83d-1771">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1771">Default value is ONE.</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1772">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1772">Example</span></span>
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra tooan Azure blob",
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

<span data-ttu-id="5e83d-1773">Další informace najdete v tématu [Cassandra konektor](data-factory-onprem-cassandra-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1773">For more information, see [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) article.</span></span>

## <a name="mongodb"></a><span data-ttu-id="5e83d-1774">MongoDB</span><span class="sxs-lookup"><span data-stu-id="5e83d-1774">MongoDB</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-1775">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1775">Linked service</span></span>
<span data-ttu-id="5e83d-1776">toodefine MongoDB propojená služba, sada hello **typ** hello propojené služby příliš**OnPremisesMongoDB**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1776">toodefine a MongoDB linked service, set hello **type** of hello linked service too**OnPremisesMongoDB**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1777">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1777">Property</span></span> | <span data-ttu-id="5e83d-1778">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1778">Description</span></span> | <span data-ttu-id="5e83d-1779">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1779">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1780">server</span><span class="sxs-lookup"><span data-stu-id="5e83d-1780">server</span></span> |<span data-ttu-id="5e83d-1781">IP adresa nebo název hostitele serveru MongoDB hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1781">IP address or host name of hello MongoDB server.</span></span> |<span data-ttu-id="5e83d-1782">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1782">Yes</span></span> |
| <span data-ttu-id="5e83d-1783">port</span><span class="sxs-lookup"><span data-stu-id="5e83d-1783">port</span></span> |<span data-ttu-id="5e83d-1784">Port TCP, který hello serveru MongoDB toolisten používá pro připojení klientů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1784">TCP port that hello MongoDB server uses toolisten for client connections.</span></span> |<span data-ttu-id="5e83d-1785">Volitelné, výchozí hodnota: 27017</span><span class="sxs-lookup"><span data-stu-id="5e83d-1785">Optional, default value: 27017</span></span> |
| <span data-ttu-id="5e83d-1786">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1786">authenticationType</span></span> |<span data-ttu-id="5e83d-1787">Základní, nebo anonymní.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1787">Basic, or Anonymous.</span></span> |<span data-ttu-id="5e83d-1788">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1788">Yes</span></span> |
| <span data-ttu-id="5e83d-1789">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-1789">username</span></span> |<span data-ttu-id="5e83d-1790">Uživatel účet tooaccess MongoDB.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1790">User account tooaccess MongoDB.</span></span> |<span data-ttu-id="5e83d-1791">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1791">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="5e83d-1792">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1792">password</span></span> |<span data-ttu-id="5e83d-1793">Heslo pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1793">Password for hello user.</span></span> |<span data-ttu-id="5e83d-1794">Ano (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1794">Yes (if basic authentication is used).</span></span> |
| <span data-ttu-id="5e83d-1795">authSource</span><span class="sxs-lookup"><span data-stu-id="5e83d-1795">authSource</span></span> |<span data-ttu-id="5e83d-1796">Název databáze hello MongoDB, že chcete toouse toocheck přihlašovacích údajů pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1796">Name of hello MongoDB database that you want toouse toocheck your credentials for authentication.</span></span> |<span data-ttu-id="5e83d-1797">Volitelný parametr (Pokud se používá základní ověřování).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1797">Optional (if basic authentication is used).</span></span> <span data-ttu-id="5e83d-1798">Výchozí: používá účet správce hello a hello databáze zadat pomocí vlastnost databaseName.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1798">default: uses hello admin account and hello database specified using databaseName property.</span></span> |
| <span data-ttu-id="5e83d-1799">Název databáze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1799">databaseName</span></span> |<span data-ttu-id="5e83d-1800">Název databáze hello MongoDB, které chcete tooaccess.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1800">Name of hello MongoDB database that you want tooaccess.</span></span> |<span data-ttu-id="5e83d-1801">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1801">Yes</span></span> |
| <span data-ttu-id="5e83d-1802">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1802">gatewayName</span></span> |<span data-ttu-id="5e83d-1803">Název brány hello, který přistupuje k úložišti dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1803">Name of hello gateway that accesses hello data store.</span></span> |<span data-ttu-id="5e83d-1804">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1804">Yes</span></span> |
| <span data-ttu-id="5e83d-1805">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1805">encryptedCredential</span></span> |<span data-ttu-id="5e83d-1806">Přihlašovací údaje zašifrovaná pomocí brány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1806">Credential encrypted by gateway.</span></span> |<span data-ttu-id="5e83d-1807">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="5e83d-1807">Optional</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1808">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1808">Example</span></span>

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< hello IP address or host name of hello MongoDB server >",
            "port": "<hello number of hello TCP port that hello MongoDB server uses toolisten for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< hello database that you want toouse toocheck your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

<span data-ttu-id="5e83d-1809">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1809">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#linked-service-properties)</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1810">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1810">Dataset</span></span>
<span data-ttu-id="5e83d-1811">toodefine MongoDB datovou sadu, sada hello **typ** sady dat hello příliš**MongoDbCollection**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1811">toodefine a MongoDB dataset, set hello **type** of hello dataset too**MongoDbCollection**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1812">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1812">Property</span></span> | <span data-ttu-id="5e83d-1813">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1813">Description</span></span> | <span data-ttu-id="5e83d-1814">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1814">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1815">Název_kolekce</span><span class="sxs-lookup"><span data-stu-id="5e83d-1815">collectionName</span></span> |<span data-ttu-id="5e83d-1816">Název kolekce hello v databázi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1816">Name of hello collection in MongoDB database.</span></span> |<span data-ttu-id="5e83d-1817">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1817">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1818">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1818">Example</span></span>

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

<span data-ttu-id="5e83d-1819">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1819">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#dataset-properties)</span></span>

#### <a name="mongodb-source-in-copy-activity"></a><span data-ttu-id="5e83d-1820">Zdroj MongoDB v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1820">MongoDB Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1821">Pokud jsou kopírování dat z MongoDB, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**MongoDbSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1821">If you are copying data from MongoDB, set hello **source type** of hello copy activity too**MongoDbSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1822">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1822">Property</span></span> | <span data-ttu-id="5e83d-1823">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1823">Description</span></span> | <span data-ttu-id="5e83d-1824">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1824">Allowed values</span></span> | <span data-ttu-id="5e83d-1825">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1825">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1826">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-1826">query</span></span> |<span data-ttu-id="5e83d-1827">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1827">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-1828">Řetězec dotazu SQL 92.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1828">SQL-92 query string.</span></span> <span data-ttu-id="5e83d-1829">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1829">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-1830">Ne (Pokud **Název_kolekce** z **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1830">No (if **collectionName** of **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1831">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1831">Example</span></span>

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

<span data-ttu-id="5e83d-1832">Další informace najdete v tématu [článku konektor MongoDB](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1832">For more information, see [MongoDB connector article](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)</span></span>

## <a name="amazon-s3"></a><span data-ttu-id="5e83d-1833">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="5e83d-1833">Amazon S3</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-1834">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1834">Linked service</span></span>
<span data-ttu-id="5e83d-1835">propojená služba, sada hello toodefine Amazon S3 **typ** hello propojené služby příliš**AwsAccessKey**a zadejte následující vlastnosti v hello **rámci typeProperties** části :</span><span class="sxs-lookup"><span data-stu-id="5e83d-1835">toodefine an Amazon S3 linked service, set hello **type** of hello linked service too**AwsAccessKey**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-1836">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1836">Property</span></span> | <span data-ttu-id="5e83d-1837">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1837">Description</span></span> | <span data-ttu-id="5e83d-1838">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1838">Allowed values</span></span> | <span data-ttu-id="5e83d-1839">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1839">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1840">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="5e83d-1840">accessKeyID</span></span> |<span data-ttu-id="5e83d-1841">ID hello tajný přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1841">ID of hello secret access key.</span></span> |<span data-ttu-id="5e83d-1842">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1842">string</span></span> |<span data-ttu-id="5e83d-1843">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1843">Yes</span></span> |
| <span data-ttu-id="5e83d-1844">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="5e83d-1844">secretAccessKey</span></span> |<span data-ttu-id="5e83d-1845">Hello tajný přístupový klíč, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1845">hello secret access key itself.</span></span> |<span data-ttu-id="5e83d-1846">Šifrované tajné řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1846">Encrypted secret string</span></span> |<span data-ttu-id="5e83d-1847">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1847">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-1848">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1848">Example</span></span>
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

<span data-ttu-id="5e83d-1849">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1849">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1850">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1850">Dataset</span></span>
<span data-ttu-id="5e83d-1851">Datová sada toodefine Amazon S3, sada hello **typ** sady dat hello příliš**AmazonS3**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1851">toodefine an Amazon S3 dataset, set hello **type** of hello dataset too**AmazonS3**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1852">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1852">Property</span></span> | <span data-ttu-id="5e83d-1853">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1853">Description</span></span> | <span data-ttu-id="5e83d-1854">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1854">Allowed values</span></span> | <span data-ttu-id="5e83d-1855">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1855">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1856">bucketName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1856">bucketName</span></span> |<span data-ttu-id="5e83d-1857">Název sady Hello S3.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1857">hello S3 bucket name.</span></span> |<span data-ttu-id="5e83d-1858">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1858">String</span></span> |<span data-ttu-id="5e83d-1859">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1859">Yes</span></span> |
| <span data-ttu-id="5e83d-1860">key</span><span class="sxs-lookup"><span data-stu-id="5e83d-1860">key</span></span> |<span data-ttu-id="5e83d-1861">klíč objektu Hello S3.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1861">hello S3 object key.</span></span> |<span data-ttu-id="5e83d-1862">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1862">String</span></span> |<span data-ttu-id="5e83d-1863">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1863">No</span></span> |
| <span data-ttu-id="5e83d-1864">Předpona</span><span class="sxs-lookup"><span data-stu-id="5e83d-1864">prefix</span></span> |<span data-ttu-id="5e83d-1865">Předpona pro klíč objektu S3 hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1865">Prefix for hello S3 object key.</span></span> <span data-ttu-id="5e83d-1866">Jsou vybrané objekty, jejichž klíče začít s touto předponou.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1866">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="5e83d-1867">Platí pouze v případě, klíč je prázdný.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1867">Applies only when key is empty.</span></span> |<span data-ttu-id="5e83d-1868">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1868">String</span></span> |<span data-ttu-id="5e83d-1869">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1869">No</span></span> |
| <span data-ttu-id="5e83d-1870">Verze</span><span class="sxs-lookup"><span data-stu-id="5e83d-1870">version</span></span> |<span data-ttu-id="5e83d-1871">Hello verze objektu S3, pokud je povolena Správa verzí S3.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1871">hello version of S3 object if S3 versioning is enabled.</span></span> |<span data-ttu-id="5e83d-1872">Řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-1872">String</span></span> |<span data-ttu-id="5e83d-1873">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1873">No</span></span> |
| <span data-ttu-id="5e83d-1874">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-1874">format</span></span> | <span data-ttu-id="5e83d-1875">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1875">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-1876">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1876">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-1877">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1877">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-1878">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1878">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-1879">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1879">No</span></span> | |
| <span data-ttu-id="5e83d-1880">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-1880">compression</span></span> | <span data-ttu-id="5e83d-1881">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1881">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-1882">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1882">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-1883">jsou Hello podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1883">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-1884">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1884">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-1885">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1885">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="5e83d-1886">bucketName + klíče určuje umístění hello hello S3 objektu, kde blok je hello Kořenový kontejner pro objekty S3 a klíč je objekt tooS3 hello úplnou cestu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1886">bucketName + key specifies hello location of hello S3 object where bucket is hello root container for S3 objects and key is hello full path tooS3 object.</span></span>

#### <a name="example-sample-dataset-with-prefix"></a><span data-ttu-id="5e83d-1887">Příklad: Ukázkovou datovou sadu s předponou</span><span class="sxs-lookup"><span data-stu-id="5e83d-1887">Example: Sample dataset with prefix</span></span>

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
#### <a name="example-sample-data-set-with-version"></a><span data-ttu-id="5e83d-1888">Příklad: Ukázka datové sady (verze)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1888">Example: Sample data set (with version)</span></span>

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

#### <a name="example-dynamic-paths-for-s3"></a><span data-ttu-id="5e83d-1889">Příklad: Dynamické cesty pro S3</span><span class="sxs-lookup"><span data-stu-id="5e83d-1889">Example: Dynamic paths for S3</span></span>
<span data-ttu-id="5e83d-1890">V ukázce hello používáme pevné hodnoty pro klíče a bucketName vlastnosti v datové sadě hello Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1890">In hello sample, we use fixed values for key and bucketName properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

<span data-ttu-id="5e83d-1891">Můžete mít vypočítat hello klíč a bucketName dynamicky za běhu pomocí systémové proměnné, jako je například SliceStart služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1891">You can have Data Factory calculate hello key and bucketName dynamically at runtime by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="5e83d-1892">Můžete provést stejný hello hello předpona vlastnosti datové sadě služby Amazon S3.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1892">You can do hello same for hello prefix property of an Amazon S3 dataset.</span></span> <span data-ttu-id="5e83d-1893">V tématu [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md) seznam podporované funkce a proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1893">See [Data Factory functions and system variables](data-factory-functions-variables.md) for a list of supported functions and variables.</span></span>

<span data-ttu-id="5e83d-1894">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1894">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="5e83d-1895">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1895">File System Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1896">Pokud kopírujete data z Amazonu S3, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části :</span><span class="sxs-lookup"><span data-stu-id="5e83d-1896">If you are copying data from Amazon S3, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>


| <span data-ttu-id="5e83d-1897">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1897">Property</span></span> | <span data-ttu-id="5e83d-1898">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1898">Description</span></span> | <span data-ttu-id="5e83d-1899">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1899">Allowed values</span></span> | <span data-ttu-id="5e83d-1900">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1900">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1901">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-1901">recursive</span></span> |<span data-ttu-id="5e83d-1902">Určuje, zda seznam toorecursively S3 objekty v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1902">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="5e83d-1903">hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="5e83d-1903">true/false</span></span> |<span data-ttu-id="5e83d-1904">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1904">No</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-1905">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1905">Example</span></span>


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

<span data-ttu-id="5e83d-1906">Další informace najdete v tématu [Amazon S3 konektor článku](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1906">For more information, see [Amazon S3 connector article](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).</span></span>

## <a name="file-system"></a><span data-ttu-id="5e83d-1907">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="5e83d-1907">File System</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-1908">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-1908">Linked service</span></span>
<span data-ttu-id="5e83d-1909">Můžete se propojit místní soubor systému tooan pro vytváření dat Azure s hello **místní souborový Server** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1909">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="5e83d-1910">Hello následující tabulka obsahuje popis JSON prvky, které jsou specifické toohello místní souborový Server propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1910">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="5e83d-1911">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1911">Property</span></span> | <span data-ttu-id="5e83d-1912">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1912">Description</span></span> | <span data-ttu-id="5e83d-1913">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1913">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1914">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-1914">type</span></span> |<span data-ttu-id="5e83d-1915">Ujistěte se, že hello vlastnost Typ nastavena příliš**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1915">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="5e83d-1916">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1916">Yes</span></span> |
| <span data-ttu-id="5e83d-1917">hostitele</span><span class="sxs-lookup"><span data-stu-id="5e83d-1917">host</span></span> |<span data-ttu-id="5e83d-1918">Určuje hello kořenovou cestu hello složky, které chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1918">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="5e83d-1919">Použít hello řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1919">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="5e83d-1920">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1920">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="5e83d-1921">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1921">Yes</span></span> |
| <span data-ttu-id="5e83d-1922">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="5e83d-1922">userid</span></span> |<span data-ttu-id="5e83d-1923">Zadejte ID hello hello uživatele, který má přístup toohello serveru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1923">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="5e83d-1924">Ne (když zvolíte encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1924">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="5e83d-1925">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-1925">password</span></span> |<span data-ttu-id="5e83d-1926">Zadejte hello heslo pro uživatele hello (ID uživatele).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1926">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="5e83d-1927">Ne (když zvolíte encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1927">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="5e83d-1928">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1928">encryptedCredential</span></span> |<span data-ttu-id="5e83d-1929">Zadejte hello šifrovat přihlašovací údaje, které můžete získat spuštěním rutiny New-AzureRmDataFactoryEncryptValue hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1929">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="5e83d-1930">Ne (když zvolíte toospecify ID uživatele a heslo ve formátu prostého textu)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1930">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="5e83d-1931">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1931">gatewayName</span></span> |<span data-ttu-id="5e83d-1932">Určuje název hello hello brány, že objekt pro vytváření dat mají používat tooconnect toohello místní souborový server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1932">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="5e83d-1933">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1933">Yes</span></span> |

#### <a name="sample-folder-path-definitions"></a><span data-ttu-id="5e83d-1934">Ukázka složky cesta definice</span><span class="sxs-lookup"><span data-stu-id="5e83d-1934">Sample folder path definitions</span></span> 
| <span data-ttu-id="5e83d-1935">Scénář</span><span class="sxs-lookup"><span data-stu-id="5e83d-1935">Scenario</span></span> | <span data-ttu-id="5e83d-1936">Hostování v definici propojené služby</span><span class="sxs-lookup"><span data-stu-id="5e83d-1936">Host in linked service definition</span></span> | <span data-ttu-id="5e83d-1937">folderPath v definici datové sady</span><span class="sxs-lookup"><span data-stu-id="5e83d-1937">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1938">Místní složky v počítači brány pro správu dat:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1938">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="5e83d-1939">Příklady: D:\\ \* nebo D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="5e83d-1939">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="5e83d-1940">D:\\ \\ (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1940">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="5e83d-1941">localhost (pro starší verze než Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1941">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="5e83d-1942">. \\ \\ nebo složky\\\\podsložky (pro Data Management Gateway 2.0 nebo novější)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1942">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="5e83d-1943">D:\\ \\ nebo D:\\\\složky\\\\podsložky (pro brány verzi nižší než 2.0)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1943">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="5e83d-1944">Vzdálené sdílené složce:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1944">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="5e83d-1945">Příklady: \\ \\myserver\\sdílet\\ \* nebo \\ \\myserver\\sdílet\\složky\\podsložky\\*</span><span class="sxs-lookup"><span data-stu-id="5e83d-1945">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="5e83d-1946">\\\\\\\\myserver\\\\sdílet</span><span class="sxs-lookup"><span data-stu-id="5e83d-1946">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="5e83d-1947">. \\ \\ nebo složky\\\\podsložky</span><span class="sxs-lookup"><span data-stu-id="5e83d-1947">.\\\\ or folder\\\\subfolder</span></span> |


#### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="5e83d-1948">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu</span><span class="sxs-lookup"><span data-stu-id="5e83d-1948">Example: Using username and password in plain text</span></span>

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

#### <a name="example-using-encryptedcredential"></a><span data-ttu-id="5e83d-1949">Příklad: Pomocí encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-1949">Example: Using encryptedcredential</span></span>

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

<span data-ttu-id="5e83d-1950">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1950">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#linked-service-properties).</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-1951">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-1951">Dataset</span></span>
<span data-ttu-id="5e83d-1952">toodefine datovou sadu systému souborů, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1952">toodefine a File System dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-1953">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1953">Property</span></span> | <span data-ttu-id="5e83d-1954">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1954">Description</span></span> | <span data-ttu-id="5e83d-1955">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1955">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-1956">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-1956">folderPath</span></span> |<span data-ttu-id="5e83d-1957">Určuje složku toohello cestou hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1957">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="5e83d-1958">Použít hello řídicí znak ' \' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1958">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="5e83d-1959">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1959">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="5e83d-1960">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1960">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="5e83d-1961">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-1961">Yes</span></span> |
| <span data-ttu-id="5e83d-1962">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-1962">fileName</span></span> |<span data-ttu-id="5e83d-1963">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1963">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="5e83d-1964">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1964">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="5e83d-1965">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor se hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1965">When fileName is not specified for an output dataset, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="5e83d-1966">`Data.<Guid>.txt`(Příklad: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="5e83d-1966">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="5e83d-1967">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1967">No</span></span> |
| <span data-ttu-id="5e83d-1968">fileFilter</span><span class="sxs-lookup"><span data-stu-id="5e83d-1968">fileFilter</span></span> |<span data-ttu-id="5e83d-1969">Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1969">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="5e83d-1970">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1970">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="5e83d-1971">Příklad 1: "fileFilter": "* .log"</span><span class="sxs-lookup"><span data-stu-id="5e83d-1971">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="5e83d-1972">Příklad 2: "fileFilter": 2016 - 1-?. TXT"</span><span class="sxs-lookup"><span data-stu-id="5e83d-1972">Example 2: "fileFilter": 2016-1-?.txt"</span></span><br/><br/><span data-ttu-id="5e83d-1973">Všimněte si, že fileFilter je použít pro datové sadě služby vstupní sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1973">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="5e83d-1974">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1974">No</span></span> |
| <span data-ttu-id="5e83d-1975">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-1975">partitionedBy</span></span> |<span data-ttu-id="5e83d-1976">Můžete vytvořit partitionedBy toospecify dynamické folderPath nebo název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1976">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="5e83d-1977">Příkladem je folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1977">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-1978">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1978">No</span></span> |
| <span data-ttu-id="5e83d-1979">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-1979">format</span></span> | <span data-ttu-id="5e83d-1980">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1980">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-1981">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1981">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-1982">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1982">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-1983">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1983">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-1984">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1984">No</span></span> |
| <span data-ttu-id="5e83d-1985">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-1985">compression</span></span> | <span data-ttu-id="5e83d-1986">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1986">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-1987">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1987">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-1988">v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1988">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-1989">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-1989">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5e83d-1990">Název souboru a fileFilter nelze současně použít.</span><span class="sxs-lookup"><span data-stu-id="5e83d-1990">You cannot use fileName and fileFilter simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-1991">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-1991">Example</span></span>

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

<span data-ttu-id="5e83d-1992">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-1992">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#dataset-properties).</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="5e83d-1993">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-1993">File System Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-1994">Pokud jsou kopírování dat systému souborů, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-1994">If you are copying data from File System, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-1995">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-1995">Property</span></span> | <span data-ttu-id="5e83d-1996">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-1996">Description</span></span> | <span data-ttu-id="5e83d-1997">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-1997">Allowed values</span></span> | <span data-ttu-id="5e83d-1998">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-1998">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-1999">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-1999">recursive</span></span> |<span data-ttu-id="5e83d-2000">Určuje, zda text hello je číst data rekurzivně z podsložky hello nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2000">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-2001">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2001">True, False (default)</span></span> |<span data-ttu-id="5e83d-2002">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2002">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2003">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2003">Example</span></span>

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
<span data-ttu-id="5e83d-2004">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2004">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

### <a name="file-system-sink-in-copy-activity"></a><span data-ttu-id="5e83d-2005">Systém souborů jímky v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2005">File System Sink in Copy Activity</span></span>
<span data-ttu-id="5e83d-2006">Pokud kopírujete data tooFile systému, nastavte hello **typ jímky** hello zkopírovat aktivity příliš**FileSystemSink**a zadejte následující vlastnosti v hello **podřízený** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2006">If you are copying data tooFile System, set hello **sink type** of hello copy activity too**FileSystemSink**, and specify following properties in hello **sink** section:</span></span>

| <span data-ttu-id="5e83d-2007">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2007">Property</span></span> | <span data-ttu-id="5e83d-2008">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2008">Description</span></span> | <span data-ttu-id="5e83d-2009">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2009">Allowed values</span></span> | <span data-ttu-id="5e83d-2010">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2010">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2011">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="5e83d-2011">copyBehavior</span></span> |<span data-ttu-id="5e83d-2012">Definuje chování kopie hello, pokud je zdroj hello BlobSource nebo systému souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2012">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="5e83d-2013">**PreserveHierarchy:** zachovává hello hierarchií souborů v cílové složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2013">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="5e83d-2014">Relativní cesta hello hello zdrojového souboru toohello zdrojové složky tedy je hello stejná jako relativní cesta hello hello cílový soubor toohello cílové složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2014">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="5e83d-2015">**FlattenHierarchy:** všechny soubory ze zdrojové složky hello se vytvoří v hello první úroveň cílové složce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2015">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="5e83d-2016">Hello zaměřením jsou vytvořen s názvem generován automaticky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2016">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="5e83d-2017">**MergeFiles:** slučuje všechny soubory ze hello zdrojové složky tooone souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2017">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="5e83d-2018">Pokud je zadán název název nebo objekt blob souboru hello, hello sloučené soubor je zadaný název hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2018">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="5e83d-2019">Jinak je název automaticky generovaný soubor.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2019">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="5e83d-2020">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2020">No</span></span> |
<span data-ttu-id="5e83d-2021">Auto-</span><span class="sxs-lookup"><span data-stu-id="5e83d-2021">auto-</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-2022">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2022">Example</span></span>

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

<span data-ttu-id="5e83d-2023">Další informace najdete v tématu [článku konektoru systému souborů](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2023">For more information, see [File System connector article](data-factory-onprem-file-system-connector.md#copy-activity-properties).</span></span>

## <a name="ftp"></a><span data-ttu-id="5e83d-2024">FTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-2024">FTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-2025">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2025">Linked service</span></span>
<span data-ttu-id="5e83d-2026">propojená služba, sada hello toodefine k serveru FTP **typ** hello propojené služby příliš**Server_ftp**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2026">toodefine an FTP linked service, set hello **type** of hello linked service too**FtpServer**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2027">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2027">Property</span></span> | <span data-ttu-id="5e83d-2028">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2028">Description</span></span> | <span data-ttu-id="5e83d-2029">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2029">Required</span></span> | <span data-ttu-id="5e83d-2030">Výchozí</span><span class="sxs-lookup"><span data-stu-id="5e83d-2030">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2031">hostitele</span><span class="sxs-lookup"><span data-stu-id="5e83d-2031">host</span></span> |<span data-ttu-id="5e83d-2032">Název nebo IP adresa hello serveru FTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-2032">Name or IP address of hello FTP Server</span></span> |<span data-ttu-id="5e83d-2033">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2033">Yes</span></span> |&nbsp; |
| <span data-ttu-id="5e83d-2034">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2034">authenticationType</span></span> |<span data-ttu-id="5e83d-2035">Zadejte typ ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2035">Specify authentication type</span></span> |<span data-ttu-id="5e83d-2036">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2036">Yes</span></span> |<span data-ttu-id="5e83d-2037">Anonymní, základní</span><span class="sxs-lookup"><span data-stu-id="5e83d-2037">Basic, Anonymous</span></span> |
| <span data-ttu-id="5e83d-2038">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2038">username</span></span> |<span data-ttu-id="5e83d-2039">Uživatel, který má přístup k serveru toohello FTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-2039">User who has access toohello FTP server</span></span> |<span data-ttu-id="5e83d-2040">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2040">No</span></span> |&nbsp; |
| <span data-ttu-id="5e83d-2041">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2041">password</span></span> |<span data-ttu-id="5e83d-2042">Heslo pro uživatele hello (username)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2042">Password for hello user (username)</span></span> |<span data-ttu-id="5e83d-2043">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2043">No</span></span> |&nbsp; |
| <span data-ttu-id="5e83d-2044">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-2044">encryptedCredential</span></span> |<span data-ttu-id="5e83d-2045">Server FTP hello tooaccess šifrovaném přihlašovacím údaji</span><span class="sxs-lookup"><span data-stu-id="5e83d-2045">Encrypted credential tooaccess hello FTP server</span></span> |<span data-ttu-id="5e83d-2046">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2046">No</span></span> |&nbsp; |
| <span data-ttu-id="5e83d-2047">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2047">gatewayName</span></span> |<span data-ttu-id="5e83d-2048">Název hello Brána pro správu dat brány tooconnect tooan místní FTP server</span><span class="sxs-lookup"><span data-stu-id="5e83d-2048">Name of hello Data Management Gateway gateway tooconnect tooan on-premises FTP server</span></span> |<span data-ttu-id="5e83d-2049">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2049">No</span></span> |&nbsp; |
| <span data-ttu-id="5e83d-2050">port</span><span class="sxs-lookup"><span data-stu-id="5e83d-2050">port</span></span> |<span data-ttu-id="5e83d-2051">Port, na které hello FTP server naslouchá</span><span class="sxs-lookup"><span data-stu-id="5e83d-2051">Port on which hello FTP server is listening</span></span> |<span data-ttu-id="5e83d-2052">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2052">No</span></span> |<span data-ttu-id="5e83d-2053">21</span><span class="sxs-lookup"><span data-stu-id="5e83d-2053">21</span></span> |
| <span data-ttu-id="5e83d-2054">enableSsl</span><span class="sxs-lookup"><span data-stu-id="5e83d-2054">enableSsl</span></span> |<span data-ttu-id="5e83d-2055">Zadejte, zda toouse FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2055">Specify whether toouse FTP over SSL/TLS channel</span></span> |<span data-ttu-id="5e83d-2056">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2056">No</span></span> |<span data-ttu-id="5e83d-2057">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="5e83d-2057">true</span></span> |
| <span data-ttu-id="5e83d-2058">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="5e83d-2058">enableServerCertificateValidation</span></span> |<span data-ttu-id="5e83d-2059">Zadejte, zda tooenable serveru SSL certifikát ověření, pokud pomocí FTP přes kanál SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2059">Specify whether tooenable server SSL certificate validation when using FTP over SSL/TLS channel</span></span> |<span data-ttu-id="5e83d-2060">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2060">No</span></span> |<span data-ttu-id="5e83d-2061">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="5e83d-2061">true</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="5e83d-2062">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2062">Example: Using Anonymous authentication</span></span>

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

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a><span data-ttu-id="5e83d-2063">Příklad: Pomocí uživatelského jména a hesla ve formátu prostého textu pro základní ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2063">Example: Using username and password in plain text for basic authentication</span></span>

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

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a><span data-ttu-id="5e83d-2064">Příklad: Pomocí portu, enableSsl, enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="5e83d-2064">Example: Using port, enableSsl, enableServerCertificateValidation</span></span>

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

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a><span data-ttu-id="5e83d-2065">Příklad: Pomocí encryptedCredential pro ověřování a brány</span><span class="sxs-lookup"><span data-stu-id="5e83d-2065">Example: Using encryptedCredential for authentication and gateway</span></span>

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

<span data-ttu-id="5e83d-2066">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2066">For more information, see [FTP connector](data-factory-ftp-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-2067">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2067">Dataset</span></span>
<span data-ttu-id="5e83d-2068">Datová sada toodefine k serveru FTP, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2068">toodefine an FTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2069">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2069">Property</span></span> | <span data-ttu-id="5e83d-2070">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2070">Description</span></span> | <span data-ttu-id="5e83d-2071">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2071">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2072">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-2072">folderPath</span></span> |<span data-ttu-id="5e83d-2073">Dílčí cesta toohello složka.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2073">Sub path toohello folder.</span></span> <span data-ttu-id="5e83d-2074">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2074">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="5e83d-2075">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2075">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="5e83d-2076">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2076">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="5e83d-2077">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2077">Yes</span></span> 
| <span data-ttu-id="5e83d-2078">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2078">fileName</span></span> |<span data-ttu-id="5e83d-2079">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2079">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="5e83d-2080">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2080">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="5e83d-2081">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2081">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="5e83d-2082">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="5e83d-2082">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="5e83d-2083">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2083">No</span></span> |
| <span data-ttu-id="5e83d-2084">fileFilter</span><span class="sxs-lookup"><span data-stu-id="5e83d-2084">fileFilter</span></span> |<span data-ttu-id="5e83d-2085">Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2085">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="5e83d-2086">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2086">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="5e83d-2087">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="5e83d-2087">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="5e83d-2088">Příklad 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="5e83d-2088">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="5e83d-2089">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2089">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="5e83d-2090">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2090">This property is not supported with HDFS.</span></span> |<span data-ttu-id="5e83d-2091">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2091">No</span></span> |
| <span data-ttu-id="5e83d-2092">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-2092">partitionedBy</span></span> |<span data-ttu-id="5e83d-2093">partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2093">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="5e83d-2094">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2094">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-2095">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2095">No</span></span> |
| <span data-ttu-id="5e83d-2096">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-2096">format</span></span> | <span data-ttu-id="5e83d-2097">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2097">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-2098">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2098">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-2099">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2099">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-2100">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2100">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-2101">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2101">No</span></span> |
| <span data-ttu-id="5e83d-2102">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-2102">compression</span></span> | <span data-ttu-id="5e83d-2103">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2103">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-2104">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**; a jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2104">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**; and supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-2105">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2105">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-2106">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2106">No</span></span> |
| <span data-ttu-id="5e83d-2107">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="5e83d-2107">useBinaryTransfer</span></span> |<span data-ttu-id="5e83d-2108">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2108">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="5e83d-2109">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2109">True for binary mode and false ASCII.</span></span> <span data-ttu-id="5e83d-2110">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2110">Default value: True.</span></span> <span data-ttu-id="5e83d-2111">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2111">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="5e83d-2112">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2112">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5e83d-2113">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2113">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-2114">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2114">Example</span></span>

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

<span data-ttu-id="5e83d-2115">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2115">For more information, see [FTP connector](data-factory-ftp-connector.md#dataset-properties) article.</span></span>

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="5e83d-2116">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2116">File System Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2117">Pokud kopírujete data ze serveru FTP, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2117">If you are copying data from an FTP server, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2118">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2118">Property</span></span> | <span data-ttu-id="5e83d-2119">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2119">Description</span></span> | <span data-ttu-id="5e83d-2120">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2120">Allowed values</span></span> | <span data-ttu-id="5e83d-2121">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2121">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2122">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-2122">recursive</span></span> |<span data-ttu-id="5e83d-2123">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2123">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-2124">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2124">True, False (default)</span></span> |<span data-ttu-id="5e83d-2125">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2125">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2126">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2126">Example</span></span>

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

<span data-ttu-id="5e83d-2127">Další informace najdete v tématu [konektor FTP](data-factory-ftp-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2127">For more information, see [FTP connector](data-factory-ftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="hdfs"></a><span data-ttu-id="5e83d-2128">HDFS</span><span class="sxs-lookup"><span data-stu-id="5e83d-2128">HDFS</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-2129">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2129">Linked service</span></span>
<span data-ttu-id="5e83d-2130">toodefine HDFS propojená služba, sada hello **typ** hello propojené služby příliš**Hdfs**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2130">toodefine a HDFS linked service, set hello **type** of hello linked service too**Hdfs**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2131">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2131">Property</span></span> | <span data-ttu-id="5e83d-2132">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2132">Description</span></span> | <span data-ttu-id="5e83d-2133">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2133">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2134">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2134">type</span></span> |<span data-ttu-id="5e83d-2135">vlastnost typu Hello musí být nastavena na: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="5e83d-2135">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="5e83d-2136">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2136">Yes</span></span> |
| <span data-ttu-id="5e83d-2137">URL</span><span class="sxs-lookup"><span data-stu-id="5e83d-2137">Url</span></span> |<span data-ttu-id="5e83d-2138">Adresa URL toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="5e83d-2138">URL toohello HDFS</span></span> |<span data-ttu-id="5e83d-2139">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2139">Yes</span></span> |
| <span data-ttu-id="5e83d-2140">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2140">authenticationType</span></span> |<span data-ttu-id="5e83d-2141">Anonymní, nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2141">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="5e83d-2142">toouse **ověřování protokolem Kerberos** HDFS konektor, najdete v části příliš[v této části](#use-kerberos-authentication-for-hdfs-connector) tooset prostředí místní odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2142">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="5e83d-2143">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2143">Yes</span></span> |
| <span data-ttu-id="5e83d-2144">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2144">userName</span></span> |<span data-ttu-id="5e83d-2145">Ověřování uživatelského jména pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2145">Username for Windows authentication.</span></span> |<span data-ttu-id="5e83d-2146">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2146">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="5e83d-2147">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2147">password</span></span> |<span data-ttu-id="5e83d-2148">Heslo pro ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2148">Password for Windows authentication.</span></span> |<span data-ttu-id="5e83d-2149">Ano (pro ověřování systému Windows)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2149">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="5e83d-2150">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2150">gatewayName</span></span> |<span data-ttu-id="5e83d-2151">Název hello brány, kterou služba Data Factory hello měli používat tooconnect toohello HDFS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2151">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="5e83d-2152">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2152">Yes</span></span> |
| <span data-ttu-id="5e83d-2153">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-2153">encryptedCredential</span></span> |<span data-ttu-id="5e83d-2154">[Nové AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) výstup hello přístup pověření.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2154">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="5e83d-2155">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2155">No</span></span> |

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="5e83d-2156">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2156">Example: Using Anonymous authentication</span></span>

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

#### <a name="example-using-windows-authentication"></a><span data-ttu-id="5e83d-2157">Příklad: Pomocí ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="5e83d-2157">Example: Using Windows authentication</span></span>

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

<span data-ttu-id="5e83d-2158">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2158">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-2159">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2159">Dataset</span></span>
<span data-ttu-id="5e83d-2160">toodefine HDFS datovou sadu, sada hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2160">toodefine a HDFS dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2161">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2161">Property</span></span> | <span data-ttu-id="5e83d-2162">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2162">Description</span></span> | <span data-ttu-id="5e83d-2163">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2163">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2164">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-2164">folderPath</span></span> |<span data-ttu-id="5e83d-2165">Složka toohello cesta.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2165">Path toohello folder.</span></span> <span data-ttu-id="5e83d-2166">Příklad:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="5e83d-2166">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="5e83d-2167">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2167">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="5e83d-2168">Příklad: pro folder\subfolder, určete složku\\\\podsložky a pro d:\samplefolder, zadejte d:\\\\ukázková_složka.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2168">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="5e83d-2169">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2169">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="5e83d-2170">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2170">Yes</span></span> |
| <span data-ttu-id="5e83d-2171">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2171">fileName</span></span> |<span data-ttu-id="5e83d-2172">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2172">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="5e83d-2173">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2173">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="5e83d-2174">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2174">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="5e83d-2175">Data. <Guid>.txt (například:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="5e83d-2175">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="5e83d-2176">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2176">No</span></span> |
| <span data-ttu-id="5e83d-2177">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-2177">partitionedBy</span></span> |<span data-ttu-id="5e83d-2178">partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2178">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="5e83d-2179">Příklad: folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2179">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-2180">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2180">No</span></span> |
| <span data-ttu-id="5e83d-2181">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-2181">format</span></span> | <span data-ttu-id="5e83d-2182">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2182">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-2183">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2183">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-2184">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2184">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-2185">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2185">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-2186">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2186">No</span></span> |
| <span data-ttu-id="5e83d-2187">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-2187">compression</span></span> | <span data-ttu-id="5e83d-2188">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2188">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-2189">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2189">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-2190">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2190">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-2191">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2191">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-2192">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2192">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5e83d-2193">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2193">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-2194">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2194">Example</span></span>

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

<span data-ttu-id="5e83d-2195">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2195">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="5e83d-2196">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2196">File System Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2197">Pokud kopírujete data z HDFS, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2197">If you are copying data from HDFS, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

<span data-ttu-id="5e83d-2198">**FileSystemSource** podporuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2198">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="5e83d-2199">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2199">Property</span></span> | <span data-ttu-id="5e83d-2200">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2200">Description</span></span> | <span data-ttu-id="5e83d-2201">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2201">Allowed values</span></span> | <span data-ttu-id="5e83d-2202">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2202">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2203">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-2203">recursive</span></span> |<span data-ttu-id="5e83d-2204">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2204">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-2205">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2205">True, False (default)</span></span> |<span data-ttu-id="5e83d-2206">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2206">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2207">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2207">Example</span></span>

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

<span data-ttu-id="5e83d-2208">Další informace najdete v tématu [HDFS konektor](#data-factory-hdfs-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2208">For more information, see [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) article.</span></span>

## <a name="sftp"></a><span data-ttu-id="5e83d-2209">SFTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-2209">SFTP</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-2210">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2210">Linked service</span></span>
<span data-ttu-id="5e83d-2211">propojená služba, sada hello toodefine protokolu SFTP **typ** hello propojené služby příliš**Sftp**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2211">toodefine an SFTP linked service, set hello **type** of hello linked service too**Sftp**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2212">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2212">Property</span></span> | <span data-ttu-id="5e83d-2213">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2213">Description</span></span> | <span data-ttu-id="5e83d-2214">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2214">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2215">hostitele</span><span class="sxs-lookup"><span data-stu-id="5e83d-2215">host</span></span> | <span data-ttu-id="5e83d-2216">Název nebo IP adresa serveru pomocí protokolu SFTP hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2216">Name or IP address of hello SFTP server.</span></span> |<span data-ttu-id="5e83d-2217">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2217">Yes</span></span> |
| <span data-ttu-id="5e83d-2218">port</span><span class="sxs-lookup"><span data-stu-id="5e83d-2218">port</span></span> |<span data-ttu-id="5e83d-2219">Port, na které hello SFTP server naslouchá.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2219">Port on which hello SFTP server is listening.</span></span> <span data-ttu-id="5e83d-2220">Hello výchozí hodnota je: 21</span><span class="sxs-lookup"><span data-stu-id="5e83d-2220">hello default value is: 21</span></span> |<span data-ttu-id="5e83d-2221">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2221">No</span></span> |
| <span data-ttu-id="5e83d-2222">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2222">authenticationType</span></span> |<span data-ttu-id="5e83d-2223">Zadejte typ ověřování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2223">Specify authentication type.</span></span> <span data-ttu-id="5e83d-2224">Povolené hodnoty: **základní**, **parametru SshPublicKey**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2224">Allowed values: **Basic**, **SshPublicKey**.</span></span> <br><br> <span data-ttu-id="5e83d-2225">Odkazovat příliš[základní ověřování pomocí](#using-basic-authentication) a [pomocí SSH ověření veřejného klíče](#using-ssh-public-key-authentication) částech na další vlastnosti a ukázky JSON v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2225">Refer too[Using basic authentication](#using-basic-authentication) and [Using SSH public key authentication](#using-ssh-public-key-authentication) sections on more properties and JSON samples respectively.</span></span> |<span data-ttu-id="5e83d-2226">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2226">Yes</span></span> |
| <span data-ttu-id="5e83d-2227">skipHostKeyValidation</span><span class="sxs-lookup"><span data-stu-id="5e83d-2227">skipHostKeyValidation</span></span> | <span data-ttu-id="5e83d-2228">Zadejte, zda tooskip hostitele klíče ověřování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2228">Specify whether tooskip host key validation.</span></span> | <span data-ttu-id="5e83d-2229">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2229">No.</span></span> <span data-ttu-id="5e83d-2230">Hello výchozí hodnota: false</span><span class="sxs-lookup"><span data-stu-id="5e83d-2230">hello default value: false</span></span> |
| <span data-ttu-id="5e83d-2231">hostKeyFingerprint</span><span class="sxs-lookup"><span data-stu-id="5e83d-2231">hostKeyFingerprint</span></span> | <span data-ttu-id="5e83d-2232">Zadejte hello prstu hello hostitele klíče.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2232">Specify hello finger print of hello host key.</span></span> | <span data-ttu-id="5e83d-2233">Ano, pokud hello `skipHostKeyValidation` nastavena toofalse.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2233">Yes if hello `skipHostKeyValidation` is set toofalse.</span></span>  |
| <span data-ttu-id="5e83d-2234">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2234">gatewayName</span></span> |<span data-ttu-id="5e83d-2235">Název hello Brána pro správu dat tooconnect tooan místní server pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2235">Name of hello Data Management Gateway tooconnect tooan on-premises SFTP server.</span></span> | <span data-ttu-id="5e83d-2236">Ano, pokud kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2236">Yes if copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="5e83d-2237">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-2237">encryptedCredential</span></span> | <span data-ttu-id="5e83d-2238">Šifrovaný přihlašovací údaj tooaccess hello SFTP server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2238">Encrypted credential tooaccess hello SFTP server.</span></span> <span data-ttu-id="5e83d-2239">Automaticky generovaný když zadáte základní ověřování (uživatelské jméno a heslo) nebo ověřování parametru SshPublicKey (uživatelské jméno + cesta privátního klíče nebo obsahu) v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2239">Auto-generated when you specify basic authentication (username + password) or SshPublicKey authentication (username + private key path or content) in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="5e83d-2240">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2240">No.</span></span> <span data-ttu-id="5e83d-2241">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2241">Apply only when copying data from an on-premises SFTP server.</span></span> |

#### <a name="example-using-basic-authentication"></a><span data-ttu-id="5e83d-2242">Příklad: Použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2242">Example: Using basic authentication</span></span>

<span data-ttu-id="5e83d-2243">toouse základní ověřování, nastavit `authenticationType` jako `Basic`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2243">toouse basic authentication, set `authenticationType` as `Basic`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="5e83d-2244">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2244">Property</span></span> | <span data-ttu-id="5e83d-2245">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2245">Description</span></span> | <span data-ttu-id="5e83d-2246">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2246">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2247">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2247">username</span></span> | <span data-ttu-id="5e83d-2248">Uživatel, který má přístup toohello SFTP server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2248">User who has access toohello SFTP server.</span></span> |<span data-ttu-id="5e83d-2249">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2249">Yes</span></span> |
| <span data-ttu-id="5e83d-2250">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2250">password</span></span> | <span data-ttu-id="5e83d-2251">Heslo pro uživatele hello (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2251">Password for hello user (username).</span></span> | <span data-ttu-id="5e83d-2252">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2252">Yes</span></span> |

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

#### <a name="example-basic-authentication-with-encrypted-credential"></a><span data-ttu-id="5e83d-2253">Příklad: Základní ověřování s šifrované pověření **</span><span class="sxs-lookup"><span data-stu-id="5e83d-2253">Example: Basic authentication with encrypted credential**</span></span>

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

#### <a name="using-ssh-public-key-authentication"></a><span data-ttu-id="5e83d-2254">Pomocí ověření veřejného klíče SSH: **</span><span class="sxs-lookup"><span data-stu-id="5e83d-2254">Using SSH public key authentication:**</span></span>

<span data-ttu-id="5e83d-2255">toouse základní ověřování, nastavit `authenticationType` jako `SshPublicKey`a zadejte následující vlastnosti kromě hello SFTP konektor obecné ty, které jsou zavedené v poslední části hello hello:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2255">toouse basic authentication, set `authenticationType` as `SshPublicKey`, and specify hello following properties besides hello SFTP connector generic ones introduced in hello last section:</span></span>

| <span data-ttu-id="5e83d-2256">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2256">Property</span></span> | <span data-ttu-id="5e83d-2257">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2257">Description</span></span> | <span data-ttu-id="5e83d-2258">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2258">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2259">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2259">username</span></span> |<span data-ttu-id="5e83d-2260">Uživatel, který má přístup toohello SFTP serveru</span><span class="sxs-lookup"><span data-stu-id="5e83d-2260">User who has access toohello SFTP server</span></span> |<span data-ttu-id="5e83d-2261">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2261">Yes</span></span> |
| <span data-ttu-id="5e83d-2262">privateKeyPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-2262">privateKeyPath</span></span> | <span data-ttu-id="5e83d-2263">Zadejte absolutní cestu toohello soubor privátního klíče můžete přístup k této brány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2263">Specify absolute path toohello private key file that gateway can access.</span></span> | <span data-ttu-id="5e83d-2264">Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2264">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> <br><br> <span data-ttu-id="5e83d-2265">Platí jenom v případě, že kopírování dat z místního serveru pomocí protokolu SFTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2265">Apply only when copying data from an on-premises SFTP server.</span></span> |
| <span data-ttu-id="5e83d-2266">privateKeyContent</span><span class="sxs-lookup"><span data-stu-id="5e83d-2266">privateKeyContent</span></span> | <span data-ttu-id="5e83d-2267">Serializovaná řetězec hello privátní klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2267">A serialized string of hello private key content.</span></span> <span data-ttu-id="5e83d-2268">Průvodce kopírováním Hello můžete číst soubor privátního klíče hello a extrahování hello privátní klíče obsahu automaticky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2268">hello Copy Wizard can read hello private key file and extract hello private key content automatically.</span></span> <span data-ttu-id="5e83d-2269">Pokud používáte jakékoli jiné nástroje nebo SDK, použijte vlastnost privateKeyPath hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2269">If you are using any other tool/SDK, use hello privateKeyPath property instead.</span></span> | <span data-ttu-id="5e83d-2270">Zadejte buď hello `privateKeyPath` nebo `privateKeyContent`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2270">Specify either hello `privateKeyPath` or `privateKeyContent`.</span></span> |
| <span data-ttu-id="5e83d-2271">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2271">passPhrase</span></span> | <span data-ttu-id="5e83d-2272">Pokud soubor klíče hello je chráněn heslo, zadejte hello průchodu fráze nebo hesla toodecrypt hello privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2272">Specify hello pass phrase/password toodecrypt hello private key if hello key file is protected by a pass phrase.</span></span> | <span data-ttu-id="5e83d-2273">Ano, pokud je chráněný soubor privátního klíče hello heslo.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2273">Yes if hello private key file is protected by a pass phrase.</span></span> |

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a><span data-ttu-id="5e83d-2274">Příklad: Parametru SshPublicKey ověřování pomocí privátního klíče obsahu **</span><span class="sxs-lookup"><span data-stu-id="5e83d-2274">Example: SshPublicKey authentication using private key content**</span></span>

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
            "privateKeyContent": "<base64 string of hello private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

<span data-ttu-id="5e83d-2275">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2275">For more information, see [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-2276">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2276">Dataset</span></span>
<span data-ttu-id="5e83d-2277">toodefine datové sadě služby SFTP sadu hello **typ** sady dat hello příliš**sdílení souborů**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2277">toodefine an SFTP dataset, set hello **type** of hello dataset too**FileShare**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2278">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2278">Property</span></span> | <span data-ttu-id="5e83d-2279">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2279">Description</span></span> | <span data-ttu-id="5e83d-2280">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2280">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2281">folderPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-2281">folderPath</span></span> |<span data-ttu-id="5e83d-2282">Dílčí cesta toohello složka.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2282">Sub path toohello folder.</span></span> <span data-ttu-id="5e83d-2283">Použít řídicí znak ' \ ' pro speciální znaky v řetězci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2283">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="5e83d-2284">V tématu [ukázka propojené definice služby a datovou sadu](#sample-linked-service-and-dataset-definitions) příklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2284">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="5e83d-2285">Tato vlastnost se můžete kombinovat **partitionBy** toohave složky cesty založené na řez počáteční nebo koncové hodnoty data a času.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2285">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="5e83d-2286">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2286">Yes</span></span> |
| <span data-ttu-id="5e83d-2287">fileName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2287">fileName</span></span> |<span data-ttu-id="5e83d-2288">Zadejte název hello hello souboru v hello **folderPath** Pokud chcete, aby hello tabulky toorefer tooa konkrétní soubor ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2288">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="5e83d-2289">Pokud nezadáte žádnou hodnotu pro tuto vlastnost, hello tabulka ukazuje tooall souborů ve složce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2289">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="5e83d-2290">Pokud není zadán název souboru pro datovou sadu výstupů, hello název hello vygeneruje soubor bude v hello následující tento formát:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2290">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="5e83d-2291">Data. <Guid>.txt (například: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="5e83d-2291">Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="5e83d-2292">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2292">No</span></span> |
| <span data-ttu-id="5e83d-2293">fileFilter</span><span class="sxs-lookup"><span data-stu-id="5e83d-2293">fileFilter</span></span> |<span data-ttu-id="5e83d-2294">Zadejte že filtr toobe používá tooselect podmnožinu souborů v hello folderPath, nikoli všech souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2294">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span><br/><br/><span data-ttu-id="5e83d-2295">Povolené hodnoty jsou: `*` (více znaků) a `?` (jeden znak).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2295">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="5e83d-2296">Příklady 1:`"fileFilter": "*.log"`</span><span class="sxs-lookup"><span data-stu-id="5e83d-2296">Examples 1: `"fileFilter": "*.log"`</span></span><br/><span data-ttu-id="5e83d-2297">Příklad 2:`"fileFilter": 2016-1-?.txt"`</span><span class="sxs-lookup"><span data-stu-id="5e83d-2297">Example 2: `"fileFilter": 2016-1-?.txt"`</span></span><br/><br/> <span data-ttu-id="5e83d-2298">fileFilter se vztahuje vstupní datové sady sdílení souborů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2298">fileFilter is applicable for an input FileShare dataset.</span></span> <span data-ttu-id="5e83d-2299">Tato vlastnost není podporována s HDFS.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2299">This property is not supported with HDFS.</span></span> |<span data-ttu-id="5e83d-2300">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2300">No</span></span> |
| <span data-ttu-id="5e83d-2301">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5e83d-2301">partitionedBy</span></span> |<span data-ttu-id="5e83d-2302">partitionedBy se dá použít toospecify dynamické folderPath, název souboru pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2302">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="5e83d-2303">Například folderPath parametry pro každou hodinu data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2303">For example, folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="5e83d-2304">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2304">No</span></span> |
| <span data-ttu-id="5e83d-2305">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-2305">format</span></span> | <span data-ttu-id="5e83d-2306">jsou podporovány následující typy formátu Hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2306">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-2307">Sada hello **typ** vlastnost pod formátu tooone z těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2307">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5e83d-2308">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2308">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5e83d-2309">Pokud chcete příliš**zkopírujte soubory jako-je** mezi souborové úložiště (binární kopie), přeskočte část formátu hello v obou definice vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2309">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5e83d-2310">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2310">No</span></span> |
| <span data-ttu-id="5e83d-2311">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-2311">compression</span></span> | <span data-ttu-id="5e83d-2312">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2312">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-2313">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2313">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-2314">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2314">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-2315">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2315">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-2316">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2316">No</span></span> |
| <span data-ttu-id="5e83d-2317">useBinaryTransfer</span><span class="sxs-lookup"><span data-stu-id="5e83d-2317">useBinaryTransfer</span></span> |<span data-ttu-id="5e83d-2318">Určit, jestli použít režim binární přenosu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2318">Specify whether use Binary transfer mode.</span></span> <span data-ttu-id="5e83d-2319">Platí pro binárního režimu a false ASCII.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2319">True for binary mode and false ASCII.</span></span> <span data-ttu-id="5e83d-2320">Výchozí hodnota: True.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2320">Default value: True.</span></span> <span data-ttu-id="5e83d-2321">Tuto vlastnost lze použít pouze v případě typu přidružené propojené služby typu: Server_ftp.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2321">This property can only be used when associated linked service type is of type: FtpServer.</span></span> |<span data-ttu-id="5e83d-2322">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2322">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5e83d-2323">Název souboru a fileFilter nelze použít současně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2323">filename and fileFilter cannot be used simultaneously.</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-2324">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2324">Example</span></span>

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path tooshared folder>",
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

<span data-ttu-id="5e83d-2325">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2325">For more information, see [SFTP connector](data-factory-sftp-connector.md#dataset-properties) article.</span></span> 

### <a name="file-system-source-in-copy-activity"></a><span data-ttu-id="5e83d-2326">Zdroj systému souborů v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2326">File System Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2327">Pokud kopírujete z protokolu SFTP zdroje dat, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**FileSystemSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2327">If you are copying data from an SFTP source, set hello **source type** of hello copy activity too**FileSystemSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2328">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2328">Property</span></span> | <span data-ttu-id="5e83d-2329">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2329">Description</span></span> | <span data-ttu-id="5e83d-2330">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2330">Allowed values</span></span> | <span data-ttu-id="5e83d-2331">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2331">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2332">Rekurzivní</span><span class="sxs-lookup"><span data-stu-id="5e83d-2332">recursive</span></span> |<span data-ttu-id="5e83d-2333">Určuje, zda text hello je číst data rekurzivně z hello podsložek nebo pouze z hello zadané složky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2333">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="5e83d-2334">Hodnota TRUE, False (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2334">True, False (default)</span></span> |<span data-ttu-id="5e83d-2335">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2335">No</span></span> |



#### <a name="example"></a><span data-ttu-id="5e83d-2336">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2336">Example</span></span>

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

<span data-ttu-id="5e83d-2337">Další informace najdete v tématu [SFTP konektor](data-factory-sftp-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2337">For more information, see [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) article.</span></span>


## <a name="http"></a><span data-ttu-id="5e83d-2338">HTTP</span><span class="sxs-lookup"><span data-stu-id="5e83d-2338">HTTP</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-2339">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2339">Linked service</span></span>
<span data-ttu-id="5e83d-2340">propojená služba, sada hello toodefine HTTP **typ** hello propojené služby příliš**Http**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2340">toodefine a HTTP linked service, set hello **type** of hello linked service too**Http**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2341">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2341">Property</span></span> | <span data-ttu-id="5e83d-2342">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2342">Description</span></span> | <span data-ttu-id="5e83d-2343">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2343">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2344">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="5e83d-2344">url</span></span> | <span data-ttu-id="5e83d-2345">Základní adresa URL toohello webového serveru</span><span class="sxs-lookup"><span data-stu-id="5e83d-2345">Base URL toohello Web Server</span></span> | <span data-ttu-id="5e83d-2346">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2346">Yes</span></span> |
| <span data-ttu-id="5e83d-2347">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2347">authenticationType</span></span> | <span data-ttu-id="5e83d-2348">Určuje typ ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2348">Specifies hello authentication type.</span></span> <span data-ttu-id="5e83d-2349">Povolené hodnoty jsou: **anonymní**, **základní**, **Digest**, **Windows**, **ClientCertificate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2349">Allowed values are: **Anonymous**, **Basic**, **Digest**, **Windows**, **ClientCertificate**.</span></span> <br><br> <span data-ttu-id="5e83d-2350">V uvedeném pořadí odkazovat toosections dál v této tabulce na další vlastnosti a ukázky JSON pro tyto typy ověřování.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2350">Refer toosections below this table on more properties and JSON samples for those authentication types respectively.</span></span> | <span data-ttu-id="5e83d-2351">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2351">Yes</span></span> |
| <span data-ttu-id="5e83d-2352">enableServerCertificateValidation</span><span class="sxs-lookup"><span data-stu-id="5e83d-2352">enableServerCertificateValidation</span></span> | <span data-ttu-id="5e83d-2353">Zadejte, zda, tooenable serveru SSL ověření certifikátu, pokud je zdroj HTTPS webového serveru</span><span class="sxs-lookup"><span data-stu-id="5e83d-2353">Specify whether tooenable server SSL certificate validation if source is HTTPS Web Server</span></span> | <span data-ttu-id="5e83d-2354">Ne, výchozí hodnota je true</span><span class="sxs-lookup"><span data-stu-id="5e83d-2354">No, default is true</span></span> |
| <span data-ttu-id="5e83d-2355">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2355">gatewayName</span></span> | <span data-ttu-id="5e83d-2356">Název hello Brána pro správu dat tooconnect tooan místní zdroje pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2356">Name of hello Data Management Gateway tooconnect tooan on-premises HTTP source.</span></span> | <span data-ttu-id="5e83d-2357">Ano, pokud kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2357">Yes if copying data from an on-premises HTTP source.</span></span> |
| <span data-ttu-id="5e83d-2358">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-2358">encryptedCredential</span></span> | <span data-ttu-id="5e83d-2359">Šifrovaný přihlašovací údaj tooaccess hello koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2359">Encrypted credential tooaccess hello HTTP endpoint.</span></span> <span data-ttu-id="5e83d-2360">Automaticky vygenerované při konfiguraci hello ověřovací informace v kopie Průvodce nebo hello ClickOnce automaticky otevřeném okně. dialog.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2360">Auto-generated when you configure hello authentication information in copy wizard or hello ClickOnce popup dialog.</span></span> | <span data-ttu-id="5e83d-2361">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2361">No.</span></span> <span data-ttu-id="5e83d-2362">Platí jenom v případě, že kopírování dat z místního serveru HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2362">Apply only when copying data from an on-premises HTTP server.</span></span> |

#### <a name="example-using-basic-digest-or-windows-authentication"></a><span data-ttu-id="5e83d-2363">Příklad: Použití ověřování Basic, ověřování algoritmem Digest nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="5e83d-2363">Example: Using Basic, Digest, or Windows authentication</span></span>
<span data-ttu-id="5e83d-2364">Nastavit `authenticationType` jako `Basic`, `Digest`, nebo `Windows`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2364">Set `authenticationType` as `Basic`, `Digest`, or `Windows`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="5e83d-2365">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2365">Property</span></span> | <span data-ttu-id="5e83d-2366">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2366">Description</span></span> | <span data-ttu-id="5e83d-2367">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2367">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2368">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2368">username</span></span> | <span data-ttu-id="5e83d-2369">Uživatelské jméno tooaccess hello koncový bod HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2369">Username tooaccess hello HTTP endpoint.</span></span> | <span data-ttu-id="5e83d-2370">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2370">Yes</span></span> |
| <span data-ttu-id="5e83d-2371">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2371">password</span></span> | <span data-ttu-id="5e83d-2372">Heslo pro uživatele hello (uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2372">Password for hello user (username).</span></span> | <span data-ttu-id="5e83d-2373">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2373">Yes</span></span> |

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

#### <a name="example-using-clientcertificate-authentication"></a><span data-ttu-id="5e83d-2374">Příklad: Použití ověřování ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="5e83d-2374">Example: Using ClientCertificate authentication</span></span>

<span data-ttu-id="5e83d-2375">toouse základní ověřování, nastavit `authenticationType` jako `ClientCertificate`a zadejte následující vlastnosti kromě hello HTTP konektor obecné těch, které jsou zavedené výše hello:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2375">toouse basic authentication, set `authenticationType` as `ClientCertificate`, and specify hello following properties besides hello HTTP connector generic ones introduced above:</span></span>

| <span data-ttu-id="5e83d-2376">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2376">Property</span></span> | <span data-ttu-id="5e83d-2377">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2377">Description</span></span> | <span data-ttu-id="5e83d-2378">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2378">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2379">embeddedCertData</span><span class="sxs-lookup"><span data-stu-id="5e83d-2379">embeddedCertData</span></span> | <span data-ttu-id="5e83d-2380">Hello obsah kódováním Base64 binárních dat souboru hello Personal Information Exchange (PFX).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2380">hello Base64-encoded contents of binary data of hello Personal Information Exchange (PFX) file.</span></span> | <span data-ttu-id="5e83d-2381">Zadejte buď hello `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2381">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="5e83d-2382">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="5e83d-2382">certThumbprint</span></span> | <span data-ttu-id="5e83d-2383">Hello kryptografický otisk certifikátu hello, který byl nainstalován v úložišti certifikátů počítače brány.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2383">hello thumbprint of hello certificate that was installed on your gateway machine’s cert store.</span></span> <span data-ttu-id="5e83d-2384">Platí jenom v případě, že kopírování dat z místního zdroje HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2384">Apply only when copying data from an on-premises HTTP source.</span></span> | <span data-ttu-id="5e83d-2385">Zadejte buď hello `embeddedCertData` nebo `certThumbprint`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2385">Specify either hello `embeddedCertData` or `certThumbprint`.</span></span> |
| <span data-ttu-id="5e83d-2386">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2386">password</span></span> | <span data-ttu-id="5e83d-2387">Heslo přidružené k hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2387">Password associated with hello certificate.</span></span> | <span data-ttu-id="5e83d-2388">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2388">No</span></span> |

<span data-ttu-id="5e83d-2389">Pokud používáte `certThumbprint` pro ověřování a hello certifikát nainstalován v osobním úložišti hello hello místního počítače, je třeba služba brány pro toohello toogrant hello oprávnění ke čtení:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2389">If you use `certThumbprint` for authentication and hello certificate is installed in hello personal store of hello local computer, you need toogrant hello read permission toohello gateway service:</span></span>

1. <span data-ttu-id="5e83d-2390">Spusťte konzolu Microsoft Management Console (MMC).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2390">Launch Microsoft Management Console (MMC).</span></span> <span data-ttu-id="5e83d-2391">Přidat hello **certifikáty** modul snap-in tohoto cíle hello **místního počítače**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2391">Add hello **Certificates** snap-in that targets hello **Local Computer**.</span></span>
2. <span data-ttu-id="5e83d-2392">Rozbalte položku **certifikáty**, **osobní**a klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2392">Expand **Certificates**, **Personal**, and click **Certificates**.</span></span>
3. <span data-ttu-id="5e83d-2393">Klikněte pravým tlačítkem na certifikát hello z osobního úložiště hello a vyberte **všechny úlohy**->**spravovat privátní klíče...**</span><span class="sxs-lookup"><span data-stu-id="5e83d-2393">Right-click hello certificate from hello personal store, and select **All Tasks**->**Manage Private Keys...**</span></span>
3. <span data-ttu-id="5e83d-2394">Na hello **zabezpečení** přidejte hello uživatelský účet, pod kterým běží hostitelská služba brány správy dat s certifikátem toohello hello přístup pro čtení.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2394">On hello **Security** tab, add hello user account under which Data Management Gateway Host Service is running with hello read access toohello certificate.</span></span>  

<span data-ttu-id="5e83d-2395">**Příklad: pomocí klientského certifikátu:** Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2395">**Example: using client certificate:** This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="5e83d-2396">Používá klientský certifikát, který je nainstalován na počítači hello s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2396">It uses a client certificate that is installed on hello machine with Data Management Gateway installed.</span></span>

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

#### <a name="example-using-client-certificate-in-a-file"></a><span data-ttu-id="5e83d-2397">Příklad: pomocí klientského certifikátu do souboru</span><span class="sxs-lookup"><span data-stu-id="5e83d-2397">Example: using client certificate in a file</span></span>
<span data-ttu-id="5e83d-2398">Tato propojená služba propojuje vaše data factory tooan místní HTTP webový server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2398">This linked service links your data factory tooan on-premises HTTP web server.</span></span> <span data-ttu-id="5e83d-2399">Soubor certifikátu klienta na počítači hello používá s brána správy dat nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2399">It uses a client certificate file on hello machine with Data Management Gateway installed.</span></span>

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

<span data-ttu-id="5e83d-2400">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2400">For more information, see [HTTP connector](data-factory-http-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-2401">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2401">Dataset</span></span>
<span data-ttu-id="5e83d-2402">Datová sada toodefine HTTP, sada hello **typ** sady dat hello příliš**Http**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2402">toodefine a HTTP dataset, set hello **type** of hello dataset too**Http**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2403">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2403">Property</span></span> | <span data-ttu-id="5e83d-2404">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2404">Description</span></span> | <span data-ttu-id="5e83d-2405">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2405">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-2406">relativeUrl</span><span class="sxs-lookup"><span data-stu-id="5e83d-2406">relativeUrl</span></span> | <span data-ttu-id="5e83d-2407">Relativní adresa URL toohello prostředek obsahující hello data.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2407">A relative URL toohello resource that contains hello data.</span></span> <span data-ttu-id="5e83d-2408">Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2408">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> <br><br> <span data-ttu-id="5e83d-2409">tooconstruct dynamické adresy URL, můžete použít [funkce pro vytváření dat a systémové proměnné](data-factory-functions-variables.md), například: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2409">tooconstruct dynamic URL, you can use [Data Factory functions and system variables](data-factory-functions-variables.md), Example: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`.</span></span> | <span data-ttu-id="5e83d-2410">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2410">No</span></span> |
| <span data-ttu-id="5e83d-2411">requestMethod</span><span class="sxs-lookup"><span data-stu-id="5e83d-2411">requestMethod</span></span> | <span data-ttu-id="5e83d-2412">Metoda HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2412">Http method.</span></span> <span data-ttu-id="5e83d-2413">Povolené hodnoty jsou **získat** nebo **POST**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2413">Allowed values are **GET** or **POST**.</span></span> | <span data-ttu-id="5e83d-2414">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2414">No.</span></span> <span data-ttu-id="5e83d-2415">Výchozí hodnota je `GET`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2415">Default is `GET`.</span></span> |
| <span data-ttu-id="5e83d-2416">additionalHeaders</span><span class="sxs-lookup"><span data-stu-id="5e83d-2416">additionalHeaders</span></span> | <span data-ttu-id="5e83d-2417">Další hlavičky žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2417">Additional HTTP request headers.</span></span> | <span data-ttu-id="5e83d-2418">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2418">No</span></span> |
| <span data-ttu-id="5e83d-2419">RequestBody</span><span class="sxs-lookup"><span data-stu-id="5e83d-2419">requestBody</span></span> | <span data-ttu-id="5e83d-2420">Text pro požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2420">Body for HTTP request.</span></span> | <span data-ttu-id="5e83d-2421">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2421">No</span></span> |
| <span data-ttu-id="5e83d-2422">Formát</span><span class="sxs-lookup"><span data-stu-id="5e83d-2422">format</span></span> | <span data-ttu-id="5e83d-2423">Pokud chcete, aby toosimply **načtení dat hello z koncový bod HTTP jako-je** bez analýza ho, přeskočte tento formát nastavení.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2423">If you want toosimply **retrieve hello data from HTTP endpoint as-is** without parsing it, skip this format settings.</span></span> <br><br> <span data-ttu-id="5e83d-2424">Pokud chcete tooparse hello HTTP odpovědi obsahu během kopírování, jsou podporovány následující typy formátu hello: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2424">If you want tooparse hello HTTP response content during copy, hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5e83d-2425">Další informace najdete v tématu [textovém formátu](data-factory-supported-file-and-compression-formats.md#text-format), [formátu Json](data-factory-supported-file-and-compression-formats.md#json-format), [Avro formát](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátu](data-factory-supported-file-and-compression-formats.md#orc-format), a [Parquet formát](data-factory-supported-file-and-compression-formats.md#parquet-format) oddíly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2425">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> |<span data-ttu-id="5e83d-2426">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2426">No</span></span> |
| <span data-ttu-id="5e83d-2427">Komprese</span><span class="sxs-lookup"><span data-stu-id="5e83d-2427">compression</span></span> | <span data-ttu-id="5e83d-2428">Zadejte typ hello a úroveň komprese dat hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2428">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5e83d-2429">Podporované typy jsou: **GZip**, **Deflate**, **BZip2**, a **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2429">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5e83d-2430">Jsou podporované úrovně: **Optimal** a **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2430">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5e83d-2431">Další informace najdete v tématu [formáty souborů a komprese v Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2431">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5e83d-2432">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2432">No</span></span> |

#### <a name="example-using-hello-get-default-method"></a><span data-ttu-id="5e83d-2433">Příklad: pomocí hello metodu GET (výchozí)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2433">Example: using hello GET (default) method</span></span>

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

#### <a name="example-using-hello-post-method"></a><span data-ttu-id="5e83d-2434">Příklad: pomocí metody POST hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-2434">Example: using hello POST method</span></span>

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
<span data-ttu-id="5e83d-2435">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2435">For more information, see [HTTP connector](data-factory-http-connector.md#dataset-properties) article.</span></span>

### <a name="http-source-in-copy-activity"></a><span data-ttu-id="5e83d-2436">Zdroj protokolu HTTP v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2436">HTTP Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2437">Pokud kopírujete data ze zdroje HTTP, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**HttpSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2437">If you are copying data from a HTTP source, set hello **source type** of hello copy activity too**HttpSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2438">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2438">Property</span></span> | <span data-ttu-id="5e83d-2439">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2439">Description</span></span> | <span data-ttu-id="5e83d-2440">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2440">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="5e83d-2441">httpRequestTimeout</span><span class="sxs-lookup"><span data-stu-id="5e83d-2441">httpRequestTimeout</span></span> | <span data-ttu-id="5e83d-2442">Hello vypršení časového limitu (časový interval) pro tooget požadavku HTTP hello odpověď.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2442">hello timeout (TimeSpan) for hello HTTP request tooget a response.</span></span> <span data-ttu-id="5e83d-2443">Je hello časový limit tooget odpověď, nebyla hello časový limit tooread data odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2443">It is hello timeout tooget a response, not hello timeout tooread response data.</span></span> | <span data-ttu-id="5e83d-2444">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2444">No.</span></span> <span data-ttu-id="5e83d-2445">Výchozí hodnota: 00:01:40</span><span class="sxs-lookup"><span data-stu-id="5e83d-2445">Default value: 00:01:40</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-2446">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2446">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source tooan Azure blob",
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

<span data-ttu-id="5e83d-2447">Další informace najdete v tématu [HTTP konektor](data-factory-http-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2447">For more information, see [HTTP connector](data-factory-http-connector.md#copy-activity-properties) article.</span></span>

## <a name="odata"></a><span data-ttu-id="5e83d-2448">OData</span><span class="sxs-lookup"><span data-stu-id="5e83d-2448">OData</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-2449">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2449">Linked service</span></span>
<span data-ttu-id="5e83d-2450">propojená služba, sada hello toodefine OData **typ** hello propojené služby příliš**OData**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2450">toodefine an OData linked service, set hello **type** of hello linked service too**OData**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2451">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2451">Property</span></span> | <span data-ttu-id="5e83d-2452">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2452">Description</span></span> | <span data-ttu-id="5e83d-2453">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2453">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2454">Adresa URL</span><span class="sxs-lookup"><span data-stu-id="5e83d-2454">url</span></span> |<span data-ttu-id="5e83d-2455">Adresa URL služby OData hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2455">Url of hello OData service.</span></span> |<span data-ttu-id="5e83d-2456">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2456">Yes</span></span> |
| <span data-ttu-id="5e83d-2457">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2457">authenticationType</span></span> |<span data-ttu-id="5e83d-2458">Typ ověřování používá tooconnect toohello OData zdroje.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2458">Type of authentication used tooconnect toohello OData source.</span></span> <br/><br/> <span data-ttu-id="5e83d-2459">Možné hodnoty pro cloudové prostředí OData, jsou anonymní, základní a OAuth (Upozorňujeme, že Azure Active Directory na základě OAuth aktuálně jedinou podpory Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2459">For cloud OData, possible values are Anonymous, Basic, and OAuth (note Azure Data Factory currently only support Azure Active Directory based OAuth).</span></span> <br/><br/> <span data-ttu-id="5e83d-2460">Pro místní OData možné hodnoty jsou anonymní, Basic a Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2460">For on-premises OData, possible values are Anonymous, Basic, and Windows.</span></span> |<span data-ttu-id="5e83d-2461">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2461">Yes</span></span> |
| <span data-ttu-id="5e83d-2462">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2462">username</span></span> |<span data-ttu-id="5e83d-2463">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2463">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="5e83d-2464">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2464">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="5e83d-2465">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2465">password</span></span> |<span data-ttu-id="5e83d-2466">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2466">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-2467">Ano (jenom Pokud používáte základní ověřování)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2467">Yes (only if you are using Basic authentication)</span></span> |
| <span data-ttu-id="5e83d-2468">authorizedCredential</span><span class="sxs-lookup"><span data-stu-id="5e83d-2468">authorizedCredential</span></span> |<span data-ttu-id="5e83d-2469">Pokud používáte OAuth, klikněte na tlačítko **Autorizovat** tlačítko v hello Průvodce kopírováním služby Data Factory editoru nebo editoru a zadejte svoje přihlašovací údaje, pak bude automaticky generovaný hello hodnota této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2469">If you are using OAuth, click **Authorize** button in hello Data Factory Copy Wizard or Editor and enter your credential, then hello value of this property will be auto-generated.</span></span> |<span data-ttu-id="5e83d-2470">Ano (jenom Pokud používáte ověřování OAuth)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2470">Yes (only if you are using OAuth authentication)</span></span> |
| <span data-ttu-id="5e83d-2471">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2471">gatewayName</span></span> |<span data-ttu-id="5e83d-2472">Název hello brány, kterou hello služba Data Factory měli používat službu tooconnect toohello místní OData.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2472">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises OData service.</span></span> <span data-ttu-id="5e83d-2473">Zadejte, pokud jsou kopírování dat z místní zdroj OData.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2473">Specify only if you are copying data from on-prem OData source.</span></span> |<span data-ttu-id="5e83d-2474">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2474">No</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="5e83d-2475">Příklad: použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2475">Example - Using Basic authentication</span></span>
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

#### <a name="example---using-anonymous-authentication"></a><span data-ttu-id="5e83d-2476">Příklad: pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2476">Example - Using Anonymous authentication</span></span>

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

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a><span data-ttu-id="5e83d-2477">Příklad: přístup k ověřování pomocí Windows místní zdroj OData</span><span class="sxs-lookup"><span data-stu-id="5e83d-2477">Example - Using Windows authentication accessing on-premises OData source</span></span>

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

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a><span data-ttu-id="5e83d-2478">Příklad - použití ověřování OAuth přístup ke cloudu zdroj OData</span><span class="sxs-lookup"><span data-stu-id="5e83d-2478">Example - Using OAuth authentication accessing cloud OData source</span></span>
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
            "authorizedCredential": "<auto generated by clicking hello Authorize button on UI>"
        }
    }
}
```

<span data-ttu-id="5e83d-2479">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2479">For more information, see [OData connector](data-factory-odata-connector.md#linked-service-properties) article.</span></span>

### <a name="dataset"></a><span data-ttu-id="5e83d-2480">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2480">Dataset</span></span>
<span data-ttu-id="5e83d-2481">toodefine datové sadě služby OData sadu hello **typ** sady dat hello příliš**ODataResource**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2481">toodefine an OData dataset, set hello **type** of hello dataset too**ODataResource**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2482">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2482">Property</span></span> | <span data-ttu-id="5e83d-2483">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2483">Description</span></span> | <span data-ttu-id="5e83d-2484">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2484">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2485">Cesta</span><span class="sxs-lookup"><span data-stu-id="5e83d-2485">path</span></span> |<span data-ttu-id="5e83d-2486">Toohello cestu OData prostředků</span><span class="sxs-lookup"><span data-stu-id="5e83d-2486">Path toohello OData resource</span></span> |<span data-ttu-id="5e83d-2487">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2487">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2488">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2488">Example</span></span>

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

<span data-ttu-id="5e83d-2489">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2489">For more information, see [OData connector](data-factory-odata-connector.md#dataset-properties) article.</span></span>

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-2490">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2490">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2491">Pokud kopírujete z OData zdroje dat, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2491">If you are copying data from an OData source, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2492">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2492">Property</span></span> | <span data-ttu-id="5e83d-2493">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2493">Description</span></span> | <span data-ttu-id="5e83d-2494">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2494">Example</span></span> | <span data-ttu-id="5e83d-2495">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2495">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2496">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-2496">query</span></span> |<span data-ttu-id="5e83d-2497">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2497">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-2498">"? $select = název, popis a $top = 5"</span><span class="sxs-lookup"><span data-stu-id="5e83d-2498">"?$select=Name, Description&$top=5"</span></span> |<span data-ttu-id="5e83d-2499">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2499">No</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2500">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2500">Example</span></span>

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

<span data-ttu-id="5e83d-2501">Další informace najdete v tématu [OData konektor](data-factory-odata-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2501">For more information, see [OData connector](data-factory-odata-connector.md#copy-activity-properties) article.</span></span>


## <a name="odbc"></a><span data-ttu-id="5e83d-2502">ODBC</span><span class="sxs-lookup"><span data-stu-id="5e83d-2502">ODBC</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-2503">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2503">Linked service</span></span>
<span data-ttu-id="5e83d-2504">propojená služba, sada hello toodefine ODBC **typ** hello propojené služby příliš**OnPremisesOdbc**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2504">toodefine an ODBC linked service, set hello **type** of hello linked service too**OnPremisesOdbc**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2505">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2505">Property</span></span> | <span data-ttu-id="5e83d-2506">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2506">Description</span></span> | <span data-ttu-id="5e83d-2507">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2507">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2508">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-2508">connectionString</span></span> |<span data-ttu-id="5e83d-2509">šifrovat přihlašovací údaje bez přístupu část Hello hello připojovací řetězec a volitelné přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2509">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="5e83d-2510">Příklady v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2510">See examples in hello following sections.</span></span> |<span data-ttu-id="5e83d-2511">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2511">Yes</span></span> |
| <span data-ttu-id="5e83d-2512">přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="5e83d-2512">credential</span></span> |<span data-ttu-id="5e83d-2513">Hello přístup pověření část hello připojovacího řetězce zadaného ve formátu ovladačem vlastnost hodnota.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2513">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="5e83d-2514">Příklad: "Uid =<user ID>; PWD =<password>; RefreshToken =<secret refresh token>; ".</span><span class="sxs-lookup"><span data-stu-id="5e83d-2514">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="5e83d-2515">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2515">No</span></span> |
| <span data-ttu-id="5e83d-2516">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2516">authenticationType</span></span> |<span data-ttu-id="5e83d-2517">Typ ověřování používá úložiště dat rozhraní ODBC toohello tooconnect.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2517">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="5e83d-2518">Možné hodnoty jsou: anonymní a Basic.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2518">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="5e83d-2519">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2519">Yes</span></span> |
| <span data-ttu-id="5e83d-2520">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2520">username</span></span> |<span data-ttu-id="5e83d-2521">Pokud používáte základní ověřování, zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2521">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="5e83d-2522">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2522">No</span></span> |
| <span data-ttu-id="5e83d-2523">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2523">password</span></span> |<span data-ttu-id="5e83d-2524">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2524">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-2525">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2525">No</span></span> |
| <span data-ttu-id="5e83d-2526">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2526">gatewayName</span></span> |<span data-ttu-id="5e83d-2527">Úložiště dat rozhraní ODBC toohello tooconnect měli použít název hello brány, kterou hello služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2527">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="5e83d-2528">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2528">Yes</span></span> |

#### <a name="example---using-basic-authentication"></a><span data-ttu-id="5e83d-2529">Příklad: použití základního ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2529">Example - Using Basic authentication</span></span>

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
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="5e83d-2530">Příklad: základní ověřování pomocí zašifrované přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="5e83d-2530">Example - Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="5e83d-2531">Můžete šifrovat přihlašovací údaje hello pomocí hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny (1.0 verzi prostředí Azure PowerShell) nebo [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 nebo starší verzi hello Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2531">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

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

#### <a name="example-using-anonymous-authentication"></a><span data-ttu-id="5e83d-2532">Příklad: Pomocí anonymní ověřování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2532">Example: Using Anonymous authentication</span></span>

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

<span data-ttu-id="5e83d-2533">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2533">For more information, see [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-2534">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2534">Dataset</span></span>
<span data-ttu-id="5e83d-2535">toodefine datovou sadu ODBC sadu hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2535">toodefine an ODBC dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2536">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2536">Property</span></span> | <span data-ttu-id="5e83d-2537">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2537">Description</span></span> | <span data-ttu-id="5e83d-2538">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2538">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2539">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2539">tableName</span></span> |<span data-ttu-id="5e83d-2540">Název tabulky hello v úložišti dat rozhraní ODBC hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2540">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="5e83d-2541">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2541">Yes</span></span> |


#### <a name="example"></a><span data-ttu-id="5e83d-2542">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2542">Example</span></span>

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

<span data-ttu-id="5e83d-2543">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2543">For more information, see [ODBC connector](data-factory-odbc-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-2544">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2544">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2545">Pokud jsou kopírování dat z úložiště dat rozhraní ODBC, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2545">If you are copying data from an ODBC data store, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2546">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2546">Property</span></span> | <span data-ttu-id="5e83d-2547">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2547">Description</span></span> | <span data-ttu-id="5e83d-2548">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2548">Allowed values</span></span> | <span data-ttu-id="5e83d-2549">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2549">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2550">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-2550">query</span></span> |<span data-ttu-id="5e83d-2551">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2551">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-2552">Řetězec dotazu SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2552">SQL query string.</span></span> <span data-ttu-id="5e83d-2553">Například: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2553">For example: `select * from MyTable`.</span></span> |<span data-ttu-id="5e83d-2554">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2554">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2555">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2555">Example</span></span>

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

<span data-ttu-id="5e83d-2556">Další informace najdete v tématu [ODBC konektor](data-factory-odbc-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2556">For more information, see [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) article.</span></span>

## <a name="salesforce"></a><span data-ttu-id="5e83d-2557">Salesforce</span><span class="sxs-lookup"><span data-stu-id="5e83d-2557">Salesforce</span></span>


### <a name="linked-service"></a><span data-ttu-id="5e83d-2558">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2558">Linked service</span></span>
<span data-ttu-id="5e83d-2559">toodefine Salesforce propojená služba, sada hello **typ** hello propojené služby příliš**Salesforce**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2559">toodefine a Salesforce linked service, set hello **type** of hello linked service too**Salesforce**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2560">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2560">Property</span></span> | <span data-ttu-id="5e83d-2561">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2561">Description</span></span> | <span data-ttu-id="5e83d-2562">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2562">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2563">environmentUrl</span><span class="sxs-lookup"><span data-stu-id="5e83d-2563">environmentUrl</span></span> | <span data-ttu-id="5e83d-2564">Zadejte adresu URL služby Salesforce instanci hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2564">Specify hello URL of Salesforce instance.</span></span> <br><br> <span data-ttu-id="5e83d-2565">-Výchozí hodnota je "https://login.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="5e83d-2565">- Default is "https://login.salesforce.com".</span></span> <br> <span data-ttu-id="5e83d-2566">-toocopy data z izolovaného prostoru, zadejte "https://test.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="5e83d-2566">- toocopy data from sandbox, specify "https://test.salesforce.com".</span></span> <br> <span data-ttu-id="5e83d-2567">-toocopy data z vlastní doménu, zadejte například "https://[domain].my.salesforce.com".</span><span class="sxs-lookup"><span data-stu-id="5e83d-2567">- toocopy data from custom domain, specify, for example, "https://[domain].my.salesforce.com".</span></span> |<span data-ttu-id="5e83d-2568">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2568">No</span></span> |
| <span data-ttu-id="5e83d-2569">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2569">username</span></span> |<span data-ttu-id="5e83d-2570">Zadejte uživatelské jméno pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2570">Specify a user name for hello user account.</span></span> |<span data-ttu-id="5e83d-2571">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2571">Yes</span></span> |
| <span data-ttu-id="5e83d-2572">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2572">password</span></span> |<span data-ttu-id="5e83d-2573">Zadejte heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2573">Specify a password for hello user account.</span></span> |<span data-ttu-id="5e83d-2574">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2574">Yes</span></span> |
| <span data-ttu-id="5e83d-2575">securityToken</span><span class="sxs-lookup"><span data-stu-id="5e83d-2575">securityToken</span></span> |<span data-ttu-id="5e83d-2576">Zadejte token zabezpečení pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2576">Specify a security token for hello user account.</span></span> <span data-ttu-id="5e83d-2577">V tématu [získal token zabezpečení](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) pokyny tooreset nebo získat token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2577">See [Get security token](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) for instructions on how tooreset/get a security token.</span></span> <span data-ttu-id="5e83d-2578">Obecně platí, najdete v části toolearn o tokeny zabezpečení [zabezpečení a hello rozhraní API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2578">toolearn about security tokens in general, see [Security and hello API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).</span></span> |<span data-ttu-id="5e83d-2579">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2579">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2580">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2580">Example</span></span>

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

<span data-ttu-id="5e83d-2581">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2581">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-2582">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2582">Dataset</span></span>
<span data-ttu-id="5e83d-2583">toodefine datovou sadu služby Salesforce, sada hello **typ** sady dat hello příliš**RelationalTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2583">toodefine a Salesforce dataset, set hello **type** of hello dataset too**RelationalTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2584">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2584">Property</span></span> | <span data-ttu-id="5e83d-2585">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2585">Description</span></span> | <span data-ttu-id="5e83d-2586">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2586">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2587">tableName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2587">tableName</span></span> |<span data-ttu-id="5e83d-2588">Název tabulky hello v Salesforce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2588">Name of hello table in Salesforce.</span></span> |<span data-ttu-id="5e83d-2589">Ne (Pokud **dotazu** z **RelationalSource** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2589">No (if a **query** of **RelationalSource** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2590">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2590">Example</span></span>

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

<span data-ttu-id="5e83d-2591">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2591">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#dataset-properties) article.</span></span> 

### <a name="relational-source-in-copy-activity"></a><span data-ttu-id="5e83d-2592">Relačního zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2592">Relational Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2593">Pokud kopírujete data ze služby Salesforce, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**RelationalSource**a zadejte následující vlastnosti v hello **zdroj** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2593">If you are copying data from Salesforce, set hello **source type** of hello copy activity too**RelationalSource**, and specify following properties in hello **source** section:</span></span>

| <span data-ttu-id="5e83d-2594">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2594">Property</span></span> | <span data-ttu-id="5e83d-2595">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2595">Description</span></span> | <span data-ttu-id="5e83d-2596">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2596">Allowed values</span></span> | <span data-ttu-id="5e83d-2597">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2597">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e83d-2598">query</span><span class="sxs-lookup"><span data-stu-id="5e83d-2598">query</span></span> |<span data-ttu-id="5e83d-2599">Použijte data tooread hello vlastního dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2599">Use hello custom query tooread data.</span></span> |<span data-ttu-id="5e83d-2600">Dotaz SQL 92 nebo [Salesforce objektu dotazu jazyka (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) dotazu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2600">A SQL-92 query or [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query.</span></span> <span data-ttu-id="5e83d-2601">Například `select * from MyTable__c`.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2601">For example:  `select * from MyTable__c`.</span></span> |<span data-ttu-id="5e83d-2602">Ne (Pokud hello **tableName** z hello **datovou sadu** je zadána)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2602">No (if hello **tableName** of hello **dataset** is specified)</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2603">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2603">Example</span></span>  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce tooan Azure blob",
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
> <span data-ttu-id="5e83d-2604">Hello "__c" součástí hello název rozhraní API je potřeba pro všechny vlastní objekt.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2604">hello "__c" part of hello API Name is needed for any custom object.</span></span>

<span data-ttu-id="5e83d-2605">Další informace najdete v tématu [konektor služby Salesforce](data-factory-salesforce-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2605">For more information, see [Salesforce connector](data-factory-salesforce-connector.md#copy-activity-properties) article.</span></span> 

## <a name="web-data"></a><span data-ttu-id="5e83d-2606">Data webové</span><span class="sxs-lookup"><span data-stu-id="5e83d-2606">Web Data</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2607">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2607">Linked service</span></span>
<span data-ttu-id="5e83d-2608">propojená služba, sada hello toodefine webové **typ** hello propojené služby příliš**webové**a zadejte následující vlastnosti v hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2608">toodefine a Web linked service, set hello **type** of hello linked service too**Web**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2609">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2609">Property</span></span> | <span data-ttu-id="5e83d-2610">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2610">Description</span></span> | <span data-ttu-id="5e83d-2611">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2611">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2612">URL</span><span class="sxs-lookup"><span data-stu-id="5e83d-2612">Url</span></span> |<span data-ttu-id="5e83d-2613">Adresa URL toohello webové zdroje</span><span class="sxs-lookup"><span data-stu-id="5e83d-2613">URL toohello Web source</span></span> |<span data-ttu-id="5e83d-2614">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2614">Yes</span></span> |
| <span data-ttu-id="5e83d-2615">authenticationType.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2615">authenticationType</span></span> |<span data-ttu-id="5e83d-2616">Anonymní.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2616">Anonymous.</span></span> |<span data-ttu-id="5e83d-2617">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2617">Yes</span></span> |
 

#### <a name="example"></a><span data-ttu-id="5e83d-2618">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2618">Example</span></span>


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

<span data-ttu-id="5e83d-2619">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2619">For more information, see [Web Table connector](data-factory-web-table-connector.md#linked-service-properties) article.</span></span> 

### <a name="dataset"></a><span data-ttu-id="5e83d-2620">Datová sada</span><span class="sxs-lookup"><span data-stu-id="5e83d-2620">Dataset</span></span>
<span data-ttu-id="5e83d-2621">toodefine webové datové sady, sada hello **typ** sady dat hello příliš**WebTable**a zadejte následující vlastnosti v hello hello **rámci typeProperties** části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2621">toodefine a Web dataset, set hello **type** of hello dataset too**WebTable**, and specify hello following properties in hello **typeProperties** section:</span></span> 

| <span data-ttu-id="5e83d-2622">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2622">Property</span></span> | <span data-ttu-id="5e83d-2623">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2623">Description</span></span> | <span data-ttu-id="5e83d-2624">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2624">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-2625">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2625">type</span></span> |<span data-ttu-id="5e83d-2626">Typ hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2626">type of hello dataset.</span></span> <span data-ttu-id="5e83d-2627">musí být nastaven příliš**WebTable**</span><span class="sxs-lookup"><span data-stu-id="5e83d-2627">must be set too**WebTable**</span></span> |<span data-ttu-id="5e83d-2628">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2628">Yes</span></span> |
| <span data-ttu-id="5e83d-2629">Cesta</span><span class="sxs-lookup"><span data-stu-id="5e83d-2629">path</span></span> |<span data-ttu-id="5e83d-2630">Na relativní adresa URL toohello prostředek, který obsahuje tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2630">A relative URL toohello resource that contains hello table.</span></span> |<span data-ttu-id="5e83d-2631">Ne.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2631">No.</span></span> <span data-ttu-id="5e83d-2632">Pokud cesta není zadána, je použít jenom hello adrese URL zadané v definici hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2632">When path is not specified, only hello URL specified in hello linked service definition is used.</span></span> |
| <span data-ttu-id="5e83d-2633">Index</span><span class="sxs-lookup"><span data-stu-id="5e83d-2633">index</span></span> |<span data-ttu-id="5e83d-2634">index Hello hello tabulky v hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2634">hello index of hello table in hello resource.</span></span> <span data-ttu-id="5e83d-2635">V tématu [Get index tabulky v stránku HTML](#get-index-of-a-table-in-an-html-page) části kroky toogetting index tabulky v stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2635">See [Get index of a table in an HTML page](#get-index-of-a-table-in-an-html-page) section for steps toogetting index of a table in an HTML page.</span></span> |<span data-ttu-id="5e83d-2636">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2636">Yes</span></span> |

#### <a name="example"></a><span data-ttu-id="5e83d-2637">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2637">Example</span></span>

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

<span data-ttu-id="5e83d-2638">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#dataset-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2638">For more information, see [Web Table connector](data-factory-web-table-connector.md#dataset-properties) article.</span></span> 

### <a name="web-source-in-copy-activity"></a><span data-ttu-id="5e83d-2639">Webové zdroje v aktivitě kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-2639">Web Source in Copy Activity</span></span>
<span data-ttu-id="5e83d-2640">Pokud jsou kopírování dat z tabulky webové, nastavte hello **typ zdroje** hello zkopírovat aktivity příliš**WebSource**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2640">If you are copying data from a web table, set hello **source type** of hello copy activity too**WebSource**.</span></span> <span data-ttu-id="5e83d-2641">V současné době po hello zdroj v aktivitě kopírování typu **WebSource**, jsou podporovány žádné další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2641">Currently, when hello source in copy activity is of type **WebSource**, no additional properties are supported.</span></span>

#### <a name="example"></a><span data-ttu-id="5e83d-2642">Příklad</span><span class="sxs-lookup"><span data-stu-id="5e83d-2642">Example</span></span>

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table tooan Azure blob",
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

<span data-ttu-id="5e83d-2643">Další informace najdete v tématu [webové tabulce konektor](data-factory-web-table-connector.md#copy-activity-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2643">For more information, see [Web Table connector](data-factory-web-table-connector.md#copy-activity-properties) article.</span></span> 

## <a name="compute-environments"></a><span data-ttu-id="5e83d-2644">VÝPOČETNÍ PROSTŘEDÍ</span><span class="sxs-lookup"><span data-stu-id="5e83d-2644">COMPUTE ENVIRONMENTS</span></span>
<span data-ttu-id="5e83d-2645">Hello následující tabulka uvádí hello výpočetních prostředích nepodporuje objekt pro vytváření dat a hello aktivit transformace, které můžou běžet na ně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2645">hello following table lists hello compute environments supported by Data Factory and hello transformation activities that can run on them.</span></span> <span data-ttu-id="5e83d-2646">Klikněte na odkaz hello výpočetní hello se zajímá toosee hello JSON schémata pro propojenou službu toolink ho tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2646">Click hello link for hello compute you are interested in toosee hello JSON schemas for linked service toolink it tooa data factory.</span></span> 

| <span data-ttu-id="5e83d-2647">Výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="5e83d-2647">Compute environment</span></span> | <span data-ttu-id="5e83d-2648">Aktivity</span><span class="sxs-lookup"><span data-stu-id="5e83d-2648">Activities</span></span> |
| --- | --- |
| <span data-ttu-id="5e83d-2649">[Cluster HDInsight na vyžádání](#on-demand-azure-hdinsight-cluster) nebo [vlastní cluster HDInsight](#existing-azure-hdinsight-cluster)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2649">[On-demand HDInsight cluster](#on-demand-azure-hdinsight-cluster) or [your own HDInsight cluster](#existing-azure-hdinsight-cluster)</span></span> |<span data-ttu-id="5e83d-2650">[Vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2650">[.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity)</span></span> |
| [<span data-ttu-id="5e83d-2651">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5e83d-2651">Azure Batch</span></span>](#azure-batch) |[<span data-ttu-id="5e83d-2652">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="5e83d-2652">.NET custom activity</span></span>](#net-custom-activity) |
| [<span data-ttu-id="5e83d-2653">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-2653">Azure Machine Learning</span></span>](#azure-machine-learning) | <span data-ttu-id="5e83d-2654">[Strojového učení aktivita provedení dávky](#machine-learning-batch-execution-activity), [strojového učení aktivita prostředku aktualizace](#machine-learning-update-resource-activity)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2654">[Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity)</span></span> |
| [<span data-ttu-id="5e83d-2655">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="5e83d-2655">Azure Data Lake Analytics</span></span>](#azure-data-lake-analytics) |[<span data-ttu-id="5e83d-2656">U-SQL Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="5e83d-2656">Data Lake Analytics U-SQL</span></span>](#data-lake-analytics-u-sql-activity) |
| <span data-ttu-id="5e83d-2657">[Databáze Azure SQL](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [systému SQL Server](#sql-server-1)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2657">[Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1)</span></span> |[<span data-ttu-id="5e83d-2658">Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="5e83d-2658">Stored Procedure</span></span>](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a><span data-ttu-id="5e83d-2659">Cluster Azure HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="5e83d-2659">On-demand Azure HDInsight cluster</span></span>
<span data-ttu-id="5e83d-2660">Hello služby Azure Data Factory můžete automaticky vytvoří systému Windows nebo Linux na vyžádání HDInsight tooprocess data clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2660">hello Azure Data Factory service can automatically create a Windows/Linux-based on-demand HDInsight cluster tooprocess data.</span></span> <span data-ttu-id="5e83d-2661">Hello clusteru je vytvořen ve stejné oblasti jako účet úložiště hello (vlastnost linkedServiceName v hello JSON) přidruženého k hello clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2661">hello cluster is created in hello same region as hello storage account (linkedServiceName property in hello JSON) associated with hello cluster.</span></span> <span data-ttu-id="5e83d-2662">Můžete spustit následující aktivit transformace na tato propojená služba hello: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [činnost MapReduce ](#hdinsight-mapreduce-activity), [Streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2662">You can run hello following transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2663">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2663">Linked service</span></span> 
<span data-ttu-id="5e83d-2664">Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello na vyžádání propojené služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2664">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an on-demand HDInsight linked service.</span></span>

| <span data-ttu-id="5e83d-2665">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2665">Property</span></span> | <span data-ttu-id="5e83d-2666">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2666">Description</span></span> | <span data-ttu-id="5e83d-2667">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2667">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2668">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2668">type</span></span> |<span data-ttu-id="5e83d-2669">vlastnost typu Hello musí být nastavená příliš**HDInsightOnDemand**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2669">hello type property should be set too**HDInsightOnDemand**.</span></span> |<span data-ttu-id="5e83d-2670">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2670">Yes</span></span> |
| <span data-ttu-id="5e83d-2671">Parametr ClusterSize</span><span class="sxs-lookup"><span data-stu-id="5e83d-2671">clusterSize</span></span> |<span data-ttu-id="5e83d-2672">Počet uzlů pracovního procesu nebo data v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2672">Number of worker/data nodes in hello cluster.</span></span> <span data-ttu-id="5e83d-2673">Vytvoření clusteru HDInsight Hello s 2 hlavních uzlech společně s hello počet uzlů pracovního procesu, který jste zadali pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2673">hello HDInsight cluster is created with 2 head nodes along with hello number of worker nodes you specify for this property.</span></span> <span data-ttu-id="5e83d-2674">Hello uzly jsou velikosti Standard_D3, který má 4 jádra, 4 pracovní uzly clusteru trvá 24 jader (4\*4 = 16 jader pro uzly pracovního procesu, plus 2\*4 = 8 jader pro head uzly).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2674">hello nodes are of size Standard_D3 that has 4 cores, so a 4 worker node cluster takes 24 cores (4\*4 = 16 cores for worker nodes, plus 2\*4 = 8 cores for head nodes).</span></span> <span data-ttu-id="5e83d-2675">V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) podrobnosti o hello Standard_D3 vrstvy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2675">See [Create Linux-based Hadoop clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) for details about hello Standard_D3 tier.</span></span> |<span data-ttu-id="5e83d-2676">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2676">Yes</span></span> |
| <span data-ttu-id="5e83d-2677">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="5e83d-2677">timetolive</span></span> |<span data-ttu-id="5e83d-2678">Hello povolená doba nečinnosti hello clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2678">hello allowed idle time for hello on-demand HDInsight cluster.</span></span> <span data-ttu-id="5e83d-2679">Určuje, jak dlouho clusteru HDInsight na vyžádání hello zůstane aktivní po dokončení činnosti spustit, pokud v clusteru hello nejsou žádné aktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2679">Specifies how long hello on-demand HDInsight cluster stays alive after completion of an activity run if there are no other active jobs in hello cluster.</span></span><br/><br/><span data-ttu-id="5e83d-2680">Pokud aktivita spuštění trvá 6 minut a timetolive je třeba nastavit too5 minut, hello clusteru zůstane aktivní po dobu 5 minut po hello 6 minut zpracování hello aktivity při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2680">For example, if an activity run takes 6 minutes and timetolive is set too5 minutes, hello cluster stays alive for 5 minutes after hello 6 minutes of processing hello activity run.</span></span> <span data-ttu-id="5e83d-2681">Pokud jiné aktivity při spuštění je proveden s hello 6 minut okna, je zpracován hello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2681">If another activity run is executed with hello 6 minutes window, it is processed by hello same cluster.</span></span><br/><br/><span data-ttu-id="5e83d-2682">Vytvoření clusteru HDInsight na vyžádání je náročná operace (může trvat), takže toto nastavení použijte jako potřebné tooimprove výkonu objektu pro vytváření dat pomocí opakovaného použití clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2682">Creating an on-demand HDInsight cluster is an expensive operation (could take a while), so use this setting as needed tooimprove performance of a data factory by reusing an on-demand HDInsight cluster.</span></span><br/><br/><span data-ttu-id="5e83d-2683">Pokud jste nastavili too0 hodnota timetolive, odstranění clusteru hello co nejrychleji hello aktivita běžet v zpracovaná.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2683">If you set timetolive value too0, hello cluster is deleted as soon as hello activity run in processed.</span></span> <span data-ttu-id="5e83d-2684">Na hello druhé straně, pokud jste nastavili na vysokou hodnotu, hello clusteru může zůstat nečinnosti zbytečně výsledkem vysoké náklady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2684">On hello other hand, if you set a high value, hello cluster may stay idle unnecessarily resulting in high costs.</span></span> <span data-ttu-id="5e83d-2685">Proto je důležité nastavit hello odpovídající hodnotu na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2685">Therefore, it is important that you set hello appropriate value based on your needs.</span></span><br/><br/><span data-ttu-id="5e83d-2686">Více kanálů můžete sdílet stejnou instanci clusteru HDInsight na vyžádání hello text hello, pokud je správně nastavena hodnota vlastnosti timetolive hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-2686">Multiple pipelines can share hello same instance of hello on-demand HDInsight cluster if hello timetolive property value is appropriately set</span></span> |<span data-ttu-id="5e83d-2687">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2687">Yes</span></span> |
| <span data-ttu-id="5e83d-2688">Verze</span><span class="sxs-lookup"><span data-stu-id="5e83d-2688">version</span></span> |<span data-ttu-id="5e83d-2689">Verze clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2689">Version of hello HDInsight cluster.</span></span> <span data-ttu-id="5e83d-2690">Podrobnosti najdete v tématu [podporované verze HDInsight v Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2690">For details, see [supported HDInsight versions in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> |<span data-ttu-id="5e83d-2691">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2691">No</span></span> |
| <span data-ttu-id="5e83d-2692">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2692">linkedServiceName</span></span> |<span data-ttu-id="5e83d-2693">Propojená služba toobe používá hello cluster na vyžádání pro ukládání a zpracování dat Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2693">Azure Storage linked service toobe used by hello on-demand cluster for storing and processing data.</span></span> <p><span data-ttu-id="5e83d-2694">V současné době nelze vytvořit cluster HDInsight na vyžádání, která používá Azure Data Lake Store jako hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2694">Currently, you cannot create an on-demand HDInsight cluster that uses an Azure Data Lake Store as hello storage.</span></span> <span data-ttu-id="5e83d-2695">Pokud chcete toostore hello Výsledná data z prostředí HDInsight zpracování v Azure Data Lake Store, pomocí aktivity kopírování toocopy hello dat z Azure Blob Storage toohello hello Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2695">If you want toostore hello result data from HDInsight processing in an Azure Data Lake Store, use a Copy Activity toocopy hello data from hello Azure Blob Storage toohello Azure Data Lake Store.</span></span></p>  | <span data-ttu-id="5e83d-2696">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2696">Yes</span></span> |
| <span data-ttu-id="5e83d-2697">additionalLinkedServiceNames</span><span class="sxs-lookup"><span data-stu-id="5e83d-2697">additionalLinkedServiceNames</span></span> |<span data-ttu-id="5e83d-2698">Určuje, že další účty úložiště pro hello HDInsight propojená služba tak, aby služba Data Factory hello můžete zaregistrovat vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2698">Specifies additional storage accounts for hello HDInsight linked service so that hello Data Factory service can register them on your behalf.</span></span> |<span data-ttu-id="5e83d-2699">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2699">No</span></span> |
| <span data-ttu-id="5e83d-2700">osType</span><span class="sxs-lookup"><span data-stu-id="5e83d-2700">osType</span></span> |<span data-ttu-id="5e83d-2701">Typ operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2701">Type of operating system.</span></span> <span data-ttu-id="5e83d-2702">Povolené hodnoty jsou: systému Windows (výchozí) a Linux</span><span class="sxs-lookup"><span data-stu-id="5e83d-2702">Allowed values are: Windows (default) and Linux</span></span> |<span data-ttu-id="5e83d-2703">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2703">No</span></span> |
| <span data-ttu-id="5e83d-2704">hcatalogLinkedServiceName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2704">hcatalogLinkedServiceName</span></span> |<span data-ttu-id="5e83d-2705">Název Hello SQL Azure, propojené služby databázi HCatalog toohello bodu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2705">hello name of Azure SQL linked service that point toohello HCatalog database.</span></span> <span data-ttu-id="5e83d-2706">cluster HDInsight na vyžádání Hello je vytvořená pomocí hello Azure SQL database jako hello metaúložiště.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2706">hello on-demand HDInsight cluster is created by using hello Azure SQL database as hello metastore.</span></span> |<span data-ttu-id="5e83d-2707">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2707">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="5e83d-2708">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2708">JSON example</span></span>
<span data-ttu-id="5e83d-2709">Hello následující JSON definuje základě Linux na vyžádání propojené služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2709">hello following JSON defines a Linux-based on-demand HDInsight linked service.</span></span> <span data-ttu-id="5e83d-2710">Hello služba Data Factory automaticky vytvoří **systémem Linux** clusteru HDInsight se při zpracování dat řezu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2710">hello Data Factory service automatically creates a **Linux-based** HDInsight cluster when processing a data slice.</span></span> 

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

<span data-ttu-id="5e83d-2711">Další informace najdete v tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2711">For more information, see [Compute linked services](data-factory-compute-linked-services.md) article.</span></span> 

## <a name="existing-azure-hdinsight-cluster"></a><span data-ttu-id="5e83d-2712">Existující cluster Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2712">Existing Azure HDInsight cluster</span></span>
<span data-ttu-id="5e83d-2713">Tooregister Azure HDInsight propojené služby můžete vytvořit vlastní cluster HDInsight s Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2713">You can create an Azure HDInsight linked service tooregister your own HDInsight cluster with Data Factory.</span></span> <span data-ttu-id="5e83d-2714">Můžete spustit následující aktivit transformace dat na tato propojená služba hello: [vlastní aktivity .NET](#net-custom-activity), [aktivitu Hivu](#hdinsight-hive-activity), [vepřových aktivity] (#-pig – aktivita hdinsight, [MapReduce aktivita](#hdinsight-mapreduce-activity), [streamování aktivity Hadoop](#hdinsight-streaming-activityd), [Spark aktivity](#hdinsight-spark-activity).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2714">You can run hello following data transformation activities on this linked service: [.NET custom activity](#net-custom-activity), [Hive activity](#hdinsight-hive-activity), [Pig activity](#hdinsight-pig-activity, [MapReduce activity](#hdinsight-mapreduce-activity), [Hadoop streaming activity](#hdinsight-streaming-activityd), [Spark activity](#hdinsight-spark-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2715">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2715">Linked service</span></span>
<span data-ttu-id="5e83d-2716">Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello propojené služby Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2716">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure HDInsight linked service.</span></span>

| <span data-ttu-id="5e83d-2717">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2717">Property</span></span> | <span data-ttu-id="5e83d-2718">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2718">Description</span></span> | <span data-ttu-id="5e83d-2719">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2719">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2720">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2720">type</span></span> |<span data-ttu-id="5e83d-2721">vlastnost typu Hello musí být nastavená příliš**HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2721">hello type property should be set too**HDInsight**.</span></span> |<span data-ttu-id="5e83d-2722">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2722">Yes</span></span> |
| <span data-ttu-id="5e83d-2723">clusterUri</span><span class="sxs-lookup"><span data-stu-id="5e83d-2723">clusterUri</span></span> |<span data-ttu-id="5e83d-2724">identifikátor URI clusteru HDInsight hello Hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2724">hello URI of hello HDInsight cluster.</span></span> |<span data-ttu-id="5e83d-2725">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2725">Yes</span></span> |
| <span data-ttu-id="5e83d-2726">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2726">username</span></span> |<span data-ttu-id="5e83d-2727">Zadejte název hello toobe uživatele hello používá tooconnect tooan stávajícího clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2727">Specify hello name of hello user toobe used tooconnect tooan existing HDInsight cluster.</span></span> |<span data-ttu-id="5e83d-2728">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2728">Yes</span></span> |
| <span data-ttu-id="5e83d-2729">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2729">password</span></span> |<span data-ttu-id="5e83d-2730">Zadejte heslo pro uživatelský účet hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2730">Specify password for hello user account.</span></span> |<span data-ttu-id="5e83d-2731">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2731">Yes</span></span> |
| <span data-ttu-id="5e83d-2732">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2732">linkedServiceName</span></span> | <span data-ttu-id="5e83d-2733">Název hello Azure Storage, propojené služby, která odkazuje úložiště objektů blob Azure toohello používá hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2733">Name of hello Azure Storage linked service that refers toohello Azure blob storage used by hello HDInsight cluster.</span></span> <p><span data-ttu-id="5e83d-2734">V současné době nelze zadat, že Azure Data Lake Store propojené služby pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2734">Currently, you cannot specify an Azure Data Lake Store linked service for this property.</span></span> <span data-ttu-id="5e83d-2735">Může přistupovat k datům v Azure Data Lake Store hello z skriptů Hive nebo Pig Pokud hello HDInsight cluster má přístup toohello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2735">You may access data in hello Azure Data Lake Store from Hive/Pig scripts if hello HDInsight cluster has access toohello Data Lake Store.</span></span> </p>  |<span data-ttu-id="5e83d-2736">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2736">Yes</span></span> |

<span data-ttu-id="5e83d-2737">Verzích clusterů HDInsight, které jsou podporovány, naleznete v části [podporované HDInsight verze](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2737">For versions of HDInsight clusters supported, see [supported HDInsight versions](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory).</span></span> 

#### <a name="json-example"></a><span data-ttu-id="5e83d-2738">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2738">JSON example</span></span>

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

## <a name="azure-batch"></a><span data-ttu-id="5e83d-2739">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5e83d-2739">Azure Batch</span></span>
<span data-ttu-id="5e83d-2740">Tooregister Azure Batch propojené služby můžete vytvořit pomocí služby data factory fondu služby Batch virtuálních počítačů (VM).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2740">You can create an Azure Batch linked service tooregister a Batch pool of virtual machines (VMs) with a data factory.</span></span> <span data-ttu-id="5e83d-2741">Můžete spustit pomocí Azure Batch nebo Azure HDInsight vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2741">You can run .NET custom activities using either Azure Batch or Azure HDInsight.</span></span> <span data-ttu-id="5e83d-2742">Můžete spustit [vlastní aktivity .NET](#net-custom-activity) na toto propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2742">You can run a [.NET custom activity](#net-custom-activity) on this linked service.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2743">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2743">Linked service</span></span>
<span data-ttu-id="5e83d-2744">Hello následující tabulka obsahuje popis vlastností hello použitými v definici Azure JSON hello Azure Batch propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2744">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Batch linked service.</span></span>

| <span data-ttu-id="5e83d-2745">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2745">Property</span></span> | <span data-ttu-id="5e83d-2746">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2746">Description</span></span> | <span data-ttu-id="5e83d-2747">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2747">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2748">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2748">type</span></span> |<span data-ttu-id="5e83d-2749">vlastnost typu Hello musí být nastavená příliš**AzureBatch**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2749">hello type property should be set too**AzureBatch**.</span></span> |<span data-ttu-id="5e83d-2750">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2750">Yes</span></span> |
| <span data-ttu-id="5e83d-2751">název účtu</span><span class="sxs-lookup"><span data-stu-id="5e83d-2751">accountName</span></span> |<span data-ttu-id="5e83d-2752">Název hello účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2752">Name of hello Azure Batch account.</span></span> |<span data-ttu-id="5e83d-2753">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2753">Yes</span></span> |
| <span data-ttu-id="5e83d-2754">accessKey</span><span class="sxs-lookup"><span data-stu-id="5e83d-2754">accessKey</span></span> |<span data-ttu-id="5e83d-2755">Přístupový klíč pro účet Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2755">Access key for hello Azure Batch account.</span></span> |<span data-ttu-id="5e83d-2756">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2756">Yes</span></span> |
| <span data-ttu-id="5e83d-2757">poolName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2757">poolName</span></span> |<span data-ttu-id="5e83d-2758">Název fondu hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2758">Name of hello pool of virtual machines.</span></span> |<span data-ttu-id="5e83d-2759">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2759">Yes</span></span> |
| <span data-ttu-id="5e83d-2760">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2760">linkedServiceName</span></span> |<span data-ttu-id="5e83d-2761">Název hello propojenou službu úložiště Azure přidruženého k této službě Azure Batch propojený.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2761">Name of hello Azure Storage linked service associated with this Azure Batch linked service.</span></span> <span data-ttu-id="5e83d-2762">Tato propojená služba se používá pro pracovní soubory požadované toorun hello aktivity a ukládání hello protokoly provádění aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2762">This linked service is used for staging files required toorun hello activity and storing hello activity execution logs.</span></span> |<span data-ttu-id="5e83d-2763">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2763">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="5e83d-2764">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2764">JSON example</span></span>

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

## <a name="azure-machine-learning"></a><span data-ttu-id="5e83d-2765">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-2765">Azure Machine Learning</span></span>
<span data-ttu-id="5e83d-2766">Vytvoření Azure Machine Learning propojené služby tooregister Machine Learning listu vyhodnocovací koncový bod pomocí služby data factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2766">You create an Azure Machine Learning linked service tooregister a Machine Learning batch scoring endpoint with a data factory.</span></span> <span data-ttu-id="5e83d-2767">Aktivity transformace dat dvě, které můžou běžet v této propojené službě: [aktivita provedení dávky Machine Learning](#machine-learning-batch-execution-activity), [aktivita prostředku aktualizace Machine Learning](#machine-learning-update-resource-activity).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2767">Two data transformation activities that can run on this linked service: [Machine Learning Batch Execution Activity](#machine-learning-batch-execution-activity), [Machine Learning Update Resource Activity](#machine-learning-update-resource-activity).</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2768">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2768">Linked service</span></span>
<span data-ttu-id="5e83d-2769">Hello následující tabulka obsahuje popis vlastností hello použitých v definici Azure JSON hello služby Azure Machine Learning propojený.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2769">hello following table provides descriptions for hello properties used in hello Azure JSON definition of an Azure Machine Learning linked service.</span></span>

| <span data-ttu-id="5e83d-2770">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2770">Property</span></span> | <span data-ttu-id="5e83d-2771">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2771">Description</span></span> | <span data-ttu-id="5e83d-2772">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2772">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2773">Typ</span><span class="sxs-lookup"><span data-stu-id="5e83d-2773">Type</span></span> |<span data-ttu-id="5e83d-2774">vlastnost typu Hello by měla být nastavena na: **AzureML**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2774">hello type property should be set to: **AzureML**.</span></span> |<span data-ttu-id="5e83d-2775">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2775">Yes</span></span> |
| <span data-ttu-id="5e83d-2776">mlEndpoint</span><span class="sxs-lookup"><span data-stu-id="5e83d-2776">mlEndpoint</span></span> |<span data-ttu-id="5e83d-2777">Hello dávkového vyhodnocování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2777">hello batch scoring URL.</span></span> |<span data-ttu-id="5e83d-2778">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2778">Yes</span></span> |
| <span data-ttu-id="5e83d-2779">apiKey</span><span class="sxs-lookup"><span data-stu-id="5e83d-2779">apiKey</span></span> |<span data-ttu-id="5e83d-2780">Hello publikovat rozhraní API modelu pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2780">hello published workspace model’s API.</span></span> |<span data-ttu-id="5e83d-2781">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2781">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="5e83d-2782">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2782">JSON example</span></span>

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

## <a name="azure-data-lake-analytics"></a><span data-ttu-id="5e83d-2783">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="5e83d-2783">Azure Data Lake Analytics</span></span>
<span data-ttu-id="5e83d-2784">Vytvoříte **Azure Data Lake Analytics** propojené služby toolink služby Azure Data Lake Analytics výpočetní služby tooan Azure data factory před použitím hello [aktivita Data Lake Analytics U-SQL](data-factory-usql-activity.md) v kanálu .</span><span class="sxs-lookup"><span data-stu-id="5e83d-2784">You create an **Azure Data Lake Analytics** linked service toolink an Azure Data Lake Analytics compute service tooan Azure data factory before using hello [Data Lake Analytics U-SQL activity](data-factory-usql-activity.md) in a pipeline.</span></span>

### <a name="linked-service"></a><span data-ttu-id="5e83d-2785">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2785">Linked service</span></span>

<span data-ttu-id="5e83d-2786">Hello následující tabulka obsahuje popis vlastností hello použitými v definici JSON hello Azure Data Lake Analytics propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2786">hello following table provides descriptions for hello properties used in hello JSON definition of an Azure Data Lake Analytics linked service.</span></span> 

| <span data-ttu-id="5e83d-2787">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2787">Property</span></span> | <span data-ttu-id="5e83d-2788">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2788">Description</span></span> | <span data-ttu-id="5e83d-2789">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2789">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2790">Typ</span><span class="sxs-lookup"><span data-stu-id="5e83d-2790">Type</span></span> |<span data-ttu-id="5e83d-2791">vlastnost typu Hello by měla být nastavena na: **AzureDataLakeAnalytics**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2791">hello type property should be set to: **AzureDataLakeAnalytics**.</span></span> |<span data-ttu-id="5e83d-2792">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2792">Yes</span></span> |
| <span data-ttu-id="5e83d-2793">název účtu</span><span class="sxs-lookup"><span data-stu-id="5e83d-2793">accountName</span></span> |<span data-ttu-id="5e83d-2794">Název účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2794">Azure Data Lake Analytics Account Name.</span></span> |<span data-ttu-id="5e83d-2795">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2795">Yes</span></span> |
| <span data-ttu-id="5e83d-2796">dataLakeAnalyticsUri</span><span class="sxs-lookup"><span data-stu-id="5e83d-2796">dataLakeAnalyticsUri</span></span> |<span data-ttu-id="5e83d-2797">Identifikátor URI služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2797">Azure Data Lake Analytics URI.</span></span> |<span data-ttu-id="5e83d-2798">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2798">No</span></span> |
| <span data-ttu-id="5e83d-2799">Autorizace</span><span class="sxs-lookup"><span data-stu-id="5e83d-2799">authorization</span></span> |<span data-ttu-id="5e83d-2800">Autorizační kód se načte automaticky po kliknutí na **Autorizovat** tlačítka na hello editoru služby Data Factory a dokončuje přihlášení OAuth hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2800">Authorization code is automatically retrieved after clicking **Authorize** button in hello Data Factory Editor and completing hello OAuth login.</span></span> |<span data-ttu-id="5e83d-2801">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2801">Yes</span></span> |
| <span data-ttu-id="5e83d-2802">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="5e83d-2802">subscriptionId</span></span> |<span data-ttu-id="5e83d-2803">Id předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="5e83d-2803">Azure subscription id</span></span> |<span data-ttu-id="5e83d-2804">Ne (když není určeno předplatné hello se používá pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2804">No (If not specified, subscription of hello data factory is used).</span></span> |
| <span data-ttu-id="5e83d-2805">Název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="5e83d-2805">resourceGroupName</span></span> |<span data-ttu-id="5e83d-2806">Název skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2806">Azure resource group name</span></span> |<span data-ttu-id="5e83d-2807">Ne (když není určeno skupiny prostředků hello se používá pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2807">No (If not specified, resource group of hello data factory is used).</span></span> |
| <span data-ttu-id="5e83d-2808">ID relace</span><span class="sxs-lookup"><span data-stu-id="5e83d-2808">sessionId</span></span> |<span data-ttu-id="5e83d-2809">id relace z relace autorizace OAuth hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2809">session id from hello OAuth authorization session.</span></span> <span data-ttu-id="5e83d-2810">Každé id relace je jedinečné a může být použit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2810">Each session id is unique and may only be used once.</span></span> <span data-ttu-id="5e83d-2811">Pokud používáte hello editoru služby Data Factory, toto ID se generuje automaticky.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2811">When you use hello Data Factory Editor, this ID is auto-generated.</span></span> |<span data-ttu-id="5e83d-2812">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2812">Yes</span></span> |


#### <a name="json-example"></a><span data-ttu-id="5e83d-2813">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2813">JSON example</span></span>
<span data-ttu-id="5e83d-2814">Následující ukázka Hello poskytuje definici JSON pro služby Azure Data Lake Analytics propojený.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2814">hello following example provides JSON definition for an Azure Data Lake Analytics linked service.</span></span>

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

## <a name="azure-sql-database"></a><span data-ttu-id="5e83d-2815">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5e83d-2815">Azure SQL Database</span></span>
<span data-ttu-id="5e83d-2816">Vytvoření služby Azure SQL propojené a jeho použití s hello [aktivity uložené procedury](#stored-procedure-activity) tooinvoke uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2816">You create an Azure SQL linked service and use it with hello [Stored Procedure Activity](#stored-procedure-activity) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2817">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2817">Linked service</span></span>
<span data-ttu-id="5e83d-2818">propojená služba, sada hello toodefine Azure SQL Database **typ** hello propojené služby příliš**azuresqldatabase**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2818">toodefine an Azure SQL Database linked service, set hello **type** of hello linked service too**AzureSqlDatabase**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2819">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2819">Property</span></span> | <span data-ttu-id="5e83d-2820">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2820">Description</span></span> | <span data-ttu-id="5e83d-2821">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2821">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2822">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-2822">connectionString</span></span> |<span data-ttu-id="5e83d-2823">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Database instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2823">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-2824">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2824">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="5e83d-2825">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2825">JSON example</span></span>

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

<span data-ttu-id="5e83d-2826">V tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2826">See [Azure SQL Connector](data-factory-azure-sql-connector.md#linked-service-properties) article for details about this linked service.</span></span>

## <a name="azure-sql-data-warehouse"></a><span data-ttu-id="5e83d-2827">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-2827">Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5e83d-2828">Vytvoření služby Azure SQL Data Warehouse propojené a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2828">You create an Azure SQL Data Warehouse linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2829">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2829">Linked service</span></span>
<span data-ttu-id="5e83d-2830">propojená služba, sada hello toodefine Azure SQL Data Warehouse **typ** hello propojené služby příliš**AzureSqlDW**a zadejte následující vlastnosti v hello **rámci typeProperties**části:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2830">toodefine an Azure SQL Data Warehouse linked service, set hello **type** of hello linked service too**AzureSqlDW**, and specify following properties in hello **typeProperties** section:</span></span>  

| <span data-ttu-id="5e83d-2831">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2831">Property</span></span> | <span data-ttu-id="5e83d-2832">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2832">Description</span></span> | <span data-ttu-id="5e83d-2833">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2833">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2834">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-2834">connectionString</span></span> |<span data-ttu-id="5e83d-2835">Zadejte informace potřebné pro vlastnost connectionString hello tooconnect toohello Azure SQL Data Warehouse instance.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2835">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> |<span data-ttu-id="5e83d-2836">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2836">Yes</span></span> |

#### <a name="json-example"></a><span data-ttu-id="5e83d-2837">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2837">JSON example</span></span>

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

<span data-ttu-id="5e83d-2838">Další informace najdete v tématu [konektor Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2838">For more information, see [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) article.</span></span> 

## <a name="sql-server"></a><span data-ttu-id="5e83d-2839">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e83d-2839">SQL Server</span></span> 
<span data-ttu-id="5e83d-2840">Vytvoření služby SQL serveru propojená a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2840">You create a SQL Server linked service and use it with hello [Stored Procedure Activity](data-factory-stored-proc-activity.md) tooinvoke a stored procedure from a Data Factory pipeline.</span></span> 

### <a name="linked-service"></a><span data-ttu-id="5e83d-2841">Propojená služba</span><span class="sxs-lookup"><span data-stu-id="5e83d-2841">Linked service</span></span>
<span data-ttu-id="5e83d-2842">Vytvoření propojené služby typu **onpremisessqlserver** toolink služby místní systém SQL Server databáze tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2842">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="5e83d-2843">Hello následující tabulka obsahuje popis služby SQL serveru propojená konkrétní tooon místní elementy JSON.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2843">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="5e83d-2844">Hello následující tabulka obsahuje popis pro konkrétní tooSQL elementy JSON serveru propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2844">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="5e83d-2845">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2845">Property</span></span> | <span data-ttu-id="5e83d-2846">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2846">Description</span></span> | <span data-ttu-id="5e83d-2847">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2847">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2848">type</span><span class="sxs-lookup"><span data-stu-id="5e83d-2848">type</span></span> |<span data-ttu-id="5e83d-2849">vlastnost typu Hello by měla být nastavena na: **onpremisessqlserver**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2849">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="5e83d-2850">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2850">Yes</span></span> |
| <span data-ttu-id="5e83d-2851">připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="5e83d-2851">connectionString</span></span> |<span data-ttu-id="5e83d-2852">Zadejte požadované informace connectionString tooconnect toohello místní databáze SQL serveru pomocí ověřování SQL nebo ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2852">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="5e83d-2853">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2853">Yes</span></span> |
| <span data-ttu-id="5e83d-2854">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5e83d-2854">gatewayName</span></span> |<span data-ttu-id="5e83d-2855">Název hello brány, kterou služba Data Factory hello měli používat toohello tooconnect, místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2855">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="5e83d-2856">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2856">Yes</span></span> |
| <span data-ttu-id="5e83d-2857">uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="5e83d-2857">username</span></span> |<span data-ttu-id="5e83d-2858">Zadejte uživatelské jméno, pokud používáte ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2858">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="5e83d-2859">Příklad: **domainname\\uživatelské jméno**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2859">Example: **domainname\\username**.</span></span> |<span data-ttu-id="5e83d-2860">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2860">No</span></span> |
| <span data-ttu-id="5e83d-2861">heslo</span><span class="sxs-lookup"><span data-stu-id="5e83d-2861">password</span></span> |<span data-ttu-id="5e83d-2862">Zadejte heslo pro hello uživatelského účtu, který jste zadali pro uživatelské jméno hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2862">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="5e83d-2863">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2863">No</span></span> |

<span data-ttu-id="5e83d-2864">Můžete šifrovat přihlašovací údaje pomocí hello **New-AzureRmDataFactoryEncryptValue** rutiny a použijte je v hello připojovací řetězec, jak ukazuje následující příklad hello (**EncryptedCredential** vlastnost):</span><span class="sxs-lookup"><span data-stu-id="5e83d-2864">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a><span data-ttu-id="5e83d-2865">Příklad: JSON pro pomocí ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2865">Example: JSON for using SQL Authentication</span></span>

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
#### <a name="example-json-for-using-windows-authentication"></a><span data-ttu-id="5e83d-2866">Příklad: JSON pro použití ověřování systému Windows</span><span class="sxs-lookup"><span data-stu-id="5e83d-2866">Example: JSON for using Windows Authentication</span></span>

<span data-ttu-id="5e83d-2867">Pokud jsou zadané uživatelské jméno a heslo, brána používá, je tooimpersonate hello zadaný uživatelský účet tooconnect toohello místní databázi systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2867">If username and password are specified, gateway uses them tooimpersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> <span data-ttu-id="5e83d-2868">Jinak brána připojí toohello systému SQL Server přímo kontext zabezpečení hello brány (jeho účet spuštění).</span><span class="sxs-lookup"><span data-stu-id="5e83d-2868">Otherwise, gateway connects toohello SQL Server directly with hello security context of Gateway (its startup account).</span></span>

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

<span data-ttu-id="5e83d-2869">Další informace najdete v tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2869">For more information, see [SQL Server connector](data-factory-sqlserver-connector.md#linked-service-properties) article.</span></span>

## <a name="data-transformation-activities"></a><span data-ttu-id="5e83d-2870">AKTIVITY TRANSFORMACE DAT</span><span class="sxs-lookup"><span data-stu-id="5e83d-2870">DATA TRANSFORMATION ACTIVITIES</span></span>

<span data-ttu-id="5e83d-2871">Aktivita</span><span class="sxs-lookup"><span data-stu-id="5e83d-2871">Activity</span></span> | <span data-ttu-id="5e83d-2872">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2872">Description</span></span>
-------- | -----------
[<span data-ttu-id="5e83d-2873">Aktivitu HDInsight Hive</span><span class="sxs-lookup"><span data-stu-id="5e83d-2873">HDInsight Hive activity</span></span>](#hdinsight-hive-activity) | <span data-ttu-id="5e83d-2874">Hello aktivitu HDInsight Hive v kanálu pro vytváření dat provede dotazů Hive sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2874">hello HDInsight Hive activity in a Data Factory pipeline executes Hive queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span> 
[<span data-ttu-id="5e83d-2875">Aktivita HDInsight Pig</span><span class="sxs-lookup"><span data-stu-id="5e83d-2875">HDInsight Pig activity</span></span>](#hdinsight-pig-activity) | <span data-ttu-id="5e83d-2876">Hello HDInsight Pig aktivity v kanálu pro vytváření dat provede Pig dotazy na vlastní nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2876">hello HDInsight Pig activity in a Data Factory pipeline executes Pig queries on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="5e83d-2877">Aktivita MapReduce služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2877">HDInsight MapReduce Activity</span></span>](#hdinsight-mapreduce-activity) | <span data-ttu-id="5e83d-2878">Hello činnost HDInsight MapReduce v objektu pro vytváření dat kanál provede MapReduce programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2878">hello HDInsight MapReduce activity in a Data Factory pipeline executes MapReduce programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="5e83d-2879">Aktivita Streamování služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2879">HDInsight Streaming Activity</span></span>](#hdinsight-streaming-activity) | <span data-ttu-id="5e83d-2880">Hello HDInsight streamované aktivitě v objektu pro vytváření dat kanál provede streamování Hadoop programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2880">hello HDInsight Streaming Activity in a Data Factory pipeline executes Hadoop Streaming programs on your own or on-demand Windows/Linux-based HDInsight cluster.</span></span>
[<span data-ttu-id="5e83d-2881">Aktivita Sparku služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2881">HDInsight Spark Activity</span></span>](#hdinsight-spark-activity) | <span data-ttu-id="5e83d-2882">Hello aktivity HDInsight Spark v objektu pro vytváření dat kanál provede na clusteru HDInsight Spark programy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2882">hello HDInsight Spark activity in a Data Factory pipeline executes Spark programs on your own HDInsight cluster.</span></span> 
[<span data-ttu-id="5e83d-2883">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-2883">Machine Learning Batch Execution Activity</span></span>](#machine-learning-batch-execution-activity) | <span data-ttu-id="5e83d-2884">Azure Data Factory umožňuje tooeasily můžete vytvořit kanály, které používají publikované Azure Machine Learning webová služba pro prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2884">Azure Data Factory enables you tooeasily create pipelines that use a published Azure Machine Learning web service for predictive analytics.</span></span> <span data-ttu-id="5e83d-2885">Pomocí hello aktivita provedení dávky v kanál služby Azure Data Factory, můžete vyvolat Machine Learning webové služby toomake předpovědi hello data v dávce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2885">Using hello Batch Execution Activity in an Azure Data Factory pipeline, you can invoke a Machine Learning web service toomake predictions on hello data in batch.</span></span> 
[<span data-ttu-id="5e83d-2886">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-2886">Machine Learning Update Resource Activity</span></span>](#machine-learning-update-resource-activity) | <span data-ttu-id="5e83d-2887">V průběhu času hello prediktivní modely v hello Machine Learning vyhodnocování experimenty potřebovat toobe retrained pomocí nové vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2887">Over time, hello predictive models in hello Machine Learning scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="5e83d-2888">Po dokončení práce s retraining, budete chtít tooupdate hello vyhodnocování webové služby s hello retrained model Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2888">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained Machine Learning model.</span></span> <span data-ttu-id="5e83d-2889">Hello aktivita prostředku aktualizace tooupdate hello webové služby můžete použít s hello nově cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2889">You can use hello Update Resource Activity tooupdate hello web service with hello newly trained model.</span></span>
[<span data-ttu-id="5e83d-2890">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="5e83d-2890">Stored Procedure Activity</span></span>](#stored-procedure-activity) | <span data-ttu-id="5e83d-2891">Aktivity uložené procedury hello můžete použít v objektu pro vytváření dat kanál tooinvoke uložené procedury v jednom z hello následující úložišť dat: databáze SQL Azure, Azure SQL Data Warehouse, databáze SQL serveru ve vašem podniku nebo virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2891">You can use hello Stored Procedure activity in a Data Factory pipeline tooinvoke a stored procedure in one of hello following data stores: Azure SQL Database, Azure SQL Data Warehouse, SQL Server Database in your enterprise or an Azure VM.</span></span> 
[<span data-ttu-id="5e83d-2892">Aktivita data Lake Analytics U-SQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-2892">Data Lake Analytics U-SQL activity</span></span>](#data-lake-analytics-u-sql-activity) | <span data-ttu-id="5e83d-2893">Data Lake Analytics U-SQL aktivity spouští skript U-SQL na clusteru služby Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2893">Data Lake Analytics U-SQL Activity runs a U-SQL script on an Azure Data Lake Analytics cluster.</span></span>  
[<span data-ttu-id="5e83d-2894">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="5e83d-2894">.NET custom activity</span></span>](#net-custom-activity) | <span data-ttu-id="5e83d-2895">Pokud potřebujete tootransform data způsobem, který není podporován službou Data Factory, můžete vytvořit vlastní aktivity s vlastní logikou zpracování dat a používat hello aktivity v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2895">If you need tootransform data in a way that is not supported by Data Factory, you can create a custom activity with your own data processing logic and use hello activity in hello pipeline.</span></span> <span data-ttu-id="5e83d-2896">Můžete nakonfigurovat hello vlastní .NET aktivity toorun pomocí služby Azure Batch nebo clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2896">You can configure hello custom .NET activity toorun using either an Azure Batch service or an Azure HDInsight cluster.</span></span> 

     
## <a name="hdinsight-hive-activity"></a><span data-ttu-id="5e83d-2897">Aktivita Hivu služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2897">HDInsight Hive Activity</span></span>
<span data-ttu-id="5e83d-2898">Můžete zadat následující vlastnosti v definici JSON aktivity Hive hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2898">You can specify hello following properties in a Hive Activity JSON definition.</span></span> <span data-ttu-id="5e83d-2899">musí být vlastnost typu Hello aktivity hello: **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2899">hello type property for hello activity must be: **HDInsightHive**.</span></span> <span data-ttu-id="5e83d-2900">Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2900">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-2901">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightHive aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2901">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightHive:</span></span>

| <span data-ttu-id="5e83d-2902">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2902">Property</span></span> | <span data-ttu-id="5e83d-2903">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2903">Description</span></span> | <span data-ttu-id="5e83d-2904">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2904">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2905">Skript</span><span class="sxs-lookup"><span data-stu-id="5e83d-2905">script</span></span> |<span data-ttu-id="5e83d-2906">Zadejte vloženého skriptu Hive hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-2906">Specify hello Hive script inline</span></span> |<span data-ttu-id="5e83d-2907">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2907">No</span></span> |
| <span data-ttu-id="5e83d-2908">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="5e83d-2908">script path</span></span> |<span data-ttu-id="5e83d-2909">Úložiště hello Hive skript v Azure blob storage a zadejte toohello hello cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2909">Store hello Hive script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="5e83d-2910">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2910">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="5e83d-2911">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2911">Both cannot be used together.</span></span> <span data-ttu-id="5e83d-2912">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2912">hello file name is case-sensitive.</span></span> |<span data-ttu-id="5e83d-2913">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2913">No</span></span> |
| <span data-ttu-id="5e83d-2914">definuje</span><span class="sxs-lookup"><span data-stu-id="5e83d-2914">defines</span></span> |<span data-ttu-id="5e83d-2915">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci skriptu Hive hello pomocí 'hiveconf.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2915">Specify parameters as key/value pairs for referencing within hello Hive script using 'hiveconf'</span></span> |<span data-ttu-id="5e83d-2916">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2916">No</span></span> |

<span data-ttu-id="5e83d-2917">Tyto vlastnosti typu se konkrétní toohello Hive aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2917">These type properties are specific toohello Hive Activity.</span></span> <span data-ttu-id="5e83d-2918">Ostatní vlastnosti (mimo oddíl rámci typeProperties hello) jsou podporovány pro všechny aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2918">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="5e83d-2919">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2919">JSON example</span></span>
<span data-ttu-id="5e83d-2920">Hello následující JSON definuje aktivitu HDInsight Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2920">hello following JSON defines a HDInsight Hive activity in a pipeline.</span></span>  

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

<span data-ttu-id="5e83d-2921">Další informace najdete v tématu [Hive aktivity](data-factory-hive-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2921">For more information, see [Hive Activity](data-factory-hive-activity.md) article.</span></span> 

## <a name="hdinsight-pig-activity"></a><span data-ttu-id="5e83d-2922">Aktivita Pig služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2922">HDInsight Pig Activity</span></span>
<span data-ttu-id="5e83d-2923">Můžete zadat následující vlastnosti v definici JSON aktivity Pig hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2923">You can specify hello following properties in a Pig Activity JSON definition.</span></span> <span data-ttu-id="5e83d-2924">musí být vlastnost typu Hello aktivity hello: **HDInsightPig**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2924">hello type property for hello activity must be: **HDInsightPig**.</span></span> <span data-ttu-id="5e83d-2925">Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2925">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-2926">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightPig aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2926">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightPig:</span></span> 

| <span data-ttu-id="5e83d-2927">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2927">Property</span></span> | <span data-ttu-id="5e83d-2928">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2928">Description</span></span> | <span data-ttu-id="5e83d-2929">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2929">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2930">Skript</span><span class="sxs-lookup"><span data-stu-id="5e83d-2930">script</span></span> |<span data-ttu-id="5e83d-2931">Zadejte vloženého skriptu Pig hello</span><span class="sxs-lookup"><span data-stu-id="5e83d-2931">Specify hello Pig script inline</span></span> |<span data-ttu-id="5e83d-2932">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2932">No</span></span> |
| <span data-ttu-id="5e83d-2933">cestu ke skriptu</span><span class="sxs-lookup"><span data-stu-id="5e83d-2933">script path</span></span> |<span data-ttu-id="5e83d-2934">Uložte skript Pig hello v Azure blob storage a zadejte toohello hello cestě k souboru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2934">Store hello Pig script in an Azure blob storage and provide hello path toohello file.</span></span> <span data-ttu-id="5e83d-2935">Pomocí vlastnosti 'skript' nebo 'scriptPath'.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2935">Use 'script' or 'scriptPath' property.</span></span> <span data-ttu-id="5e83d-2936">Obě nelze použít společně.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2936">Both cannot be used together.</span></span> <span data-ttu-id="5e83d-2937">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2937">hello file name is case-sensitive.</span></span> |<span data-ttu-id="5e83d-2938">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2938">No</span></span> |
| <span data-ttu-id="5e83d-2939">definuje</span><span class="sxs-lookup"><span data-stu-id="5e83d-2939">defines</span></span> |<span data-ttu-id="5e83d-2940">Zadejte parametry dvojic klíč/hodnota pro odkazování v rámci hello skript Pig</span><span class="sxs-lookup"><span data-stu-id="5e83d-2940">Specify parameters as key/value pairs for referencing within hello Pig script</span></span> |<span data-ttu-id="5e83d-2941">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2941">No</span></span> |

<span data-ttu-id="5e83d-2942">Tyto vlastnosti typu se konkrétní toohello Pig aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2942">These type properties are specific toohello Pig Activity.</span></span> <span data-ttu-id="5e83d-2943">Ostatní vlastnosti (mimo oddíl rámci typeProperties hello) jsou podporovány pro všechny aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2943">Other properties (outside hello typeProperties section) are supported for all activities.</span></span>   

### <a name="json-example"></a><span data-ttu-id="5e83d-2944">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2944">JSON example</span></span>

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

<span data-ttu-id="5e83d-2945">Další informace najdete v tématu [Pig aktivity](#data-factory-pig-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2945">For more information, see [Pig Activity](#data-factory-pig-activity.md) article.</span></span> 

## <a name="hdinsight-mapreduce-activity"></a><span data-ttu-id="5e83d-2946">Aktivita MapReduce služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2946">HDInsight MapReduce Activity</span></span>
<span data-ttu-id="5e83d-2947">Můžete zadat následující vlastnosti v definici JSON aktivity MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2947">You can specify hello following properties in a MapReduce Activity JSON definition.</span></span> <span data-ttu-id="5e83d-2948">musí být vlastnost typu Hello aktivity hello: **HDInsightMapReduce**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2948">hello type property for hello activity must be: **HDInsightMapReduce**.</span></span> <span data-ttu-id="5e83d-2949">Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2949">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-2950">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightMapReduce aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2950">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightMapReduce:</span></span> 

| <span data-ttu-id="5e83d-2951">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2951">Property</span></span> | <span data-ttu-id="5e83d-2952">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2952">Description</span></span> | <span data-ttu-id="5e83d-2953">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-2953">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-2954">jarLinkedService</span><span class="sxs-lookup"><span data-stu-id="5e83d-2954">jarLinkedService</span></span> | <span data-ttu-id="5e83d-2955">Název hello propojené služby pro Azure Storage, který obsahuje soubor JAR hello hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2955">Name of hello linked service for hello Azure Storage that contains hello JAR file.</span></span> | <span data-ttu-id="5e83d-2956">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2956">Yes</span></span> |
| <span data-ttu-id="5e83d-2957">jarFilePath</span><span class="sxs-lookup"><span data-stu-id="5e83d-2957">jarFilePath</span></span> | <span data-ttu-id="5e83d-2958">Cesta soubor JAR toohello v hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2958">Path toohello JAR file in hello Azure Storage.</span></span> | <span data-ttu-id="5e83d-2959">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2959">Yes</span></span> | 
| <span data-ttu-id="5e83d-2960">Název třídy</span><span class="sxs-lookup"><span data-stu-id="5e83d-2960">className</span></span> | <span data-ttu-id="5e83d-2961">Název hlavní třídy hello v soubor JAR hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2961">Name of hello main class in hello JAR file.</span></span> | <span data-ttu-id="5e83d-2962">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-2962">Yes</span></span> | 
| <span data-ttu-id="5e83d-2963">Argumenty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2963">arguments</span></span> | <span data-ttu-id="5e83d-2964">Seznam argumentů oddělených čárkami programu hello MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2964">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="5e83d-2965">V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2965">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="5e83d-2966">pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2966">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | <span data-ttu-id="5e83d-2967">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-2967">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="5e83d-2968">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-2968">JSON example</span></span>

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline tooRun a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix toodetermine hello similarity between two items",
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
                "description": "Custom Map Reduce toogenerate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

<span data-ttu-id="5e83d-2969">Další informace najdete v tématu [činnost MapReduce](data-factory-map-reduce.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2969">For more information, see [MapReduce Activity](data-factory-map-reduce.md) article.</span></span> 

## <a name="hdinsight-streaming-activity"></a><span data-ttu-id="5e83d-2970">Aktivita Streamování služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-2970">HDInsight Streaming Activity</span></span>
<span data-ttu-id="5e83d-2971">Můžete zadat následující vlastnosti v definici JSON aktivity streamování Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2971">You can specify hello following properties in a Hadoop Streaming Activity JSON definition.</span></span> <span data-ttu-id="5e83d-2972">musí být vlastnost typu Hello aktivity hello: **HDInsightStreaming**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2972">hello type property for hello activity must be: **HDInsightStreaming**.</span></span> <span data-ttu-id="5e83d-2973">Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2973">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-2974">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightStreaming aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-2974">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightStreaming:</span></span> 

| <span data-ttu-id="5e83d-2975">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-2975">Property</span></span> | <span data-ttu-id="5e83d-2976">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-2976">Description</span></span> | 
| --- | --- |
| <span data-ttu-id="5e83d-2977">Mapper</span><span class="sxs-lookup"><span data-stu-id="5e83d-2977">mapper</span></span> | <span data-ttu-id="5e83d-2978">Název spustitelné hello mapper.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2978">Name of hello mapper executable.</span></span> <span data-ttu-id="5e83d-2979">V příkladu hello je cat.exe hello mapper spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2979">In hello example, cat.exe is hello mapper executable.</span></span>| 
| <span data-ttu-id="5e83d-2980">reduktorem</span><span class="sxs-lookup"><span data-stu-id="5e83d-2980">reducer</span></span> | <span data-ttu-id="5e83d-2981">Název spustitelného souboru hello reduktorem.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2981">Name of hello reducer executable.</span></span> <span data-ttu-id="5e83d-2982">V příkladu hello je wc.exe hello reduktorem spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2982">In hello example, wc.exe is hello reducer executable.</span></span> | 
| <span data-ttu-id="5e83d-2983">Vstup</span><span class="sxs-lookup"><span data-stu-id="5e83d-2983">input</span></span> | <span data-ttu-id="5e83d-2984">Vstupní soubor (včetně umístění) pro hello mapper.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2984">Input file (including location) for hello mapper.</span></span> <span data-ttu-id="5e83d-2985">V příkladu hello: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample je kontejner objektů blob hello, například/data/Gutenberg je složka hello a davinci.txt je hello blob.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2985">In hello example: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is hello blob container, example/data/Gutenberg is hello folder, and davinci.txt is hello blob.</span></span> |
| <span data-ttu-id="5e83d-2986">Výstup</span><span class="sxs-lookup"><span data-stu-id="5e83d-2986">output</span></span> | <span data-ttu-id="5e83d-2987">Ve výstupním souboru (včetně umístění) reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2987">Output file (including location) for hello reducer.</span></span> <span data-ttu-id="5e83d-2988">výstup Hello úlohy streamování Hadoop hello je zapsán toohello umístění zadané pro tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2988">hello output of hello Hadoop Streaming job is written toohello location specified for this property.</span></span> |
| <span data-ttu-id="5e83d-2989">filePaths</span><span class="sxs-lookup"><span data-stu-id="5e83d-2989">filePaths</span></span> | <span data-ttu-id="5e83d-2990">Cesty pro spustitelné soubory mapper a reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2990">Paths for hello mapper and reducer executables.</span></span> <span data-ttu-id="5e83d-2991">V příkladu hello: "adfsample/example/apps/wc.exe" adfsample je kontejner objektů blob hello, příklad nebo aplikací je složka hello a wc.exe je hello spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2991">In hello example: "adfsample/example/apps/wc.exe", adfsample is hello blob container, example/apps is hello folder, and wc.exe is hello executable.</span></span> | 
| <span data-ttu-id="5e83d-2992">fileLinkedService</span><span class="sxs-lookup"><span data-stu-id="5e83d-2992">fileLinkedService</span></span> | <span data-ttu-id="5e83d-2993">Propojená služba, která představuje hello úložiště Azure, který obsahuje soubory hello zadané v části filePaths hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2993">Azure Storage linked service that represents hello Azure storage that contains hello files specified in hello filePaths section.</span></span> | 
| <span data-ttu-id="5e83d-2994">Argumenty</span><span class="sxs-lookup"><span data-stu-id="5e83d-2994">arguments</span></span> | <span data-ttu-id="5e83d-2995">Seznam argumentů oddělených čárkami programu hello MapReduce.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2995">A list of comma-separated arguments for hello MapReduce program.</span></span> <span data-ttu-id="5e83d-2996">V době běhu zobrazí několik další argumenty (například: mapreduce.job.tags) z rozhraní MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2996">At runtime, you see a few extra arguments (for example: mapreduce.job.tags) from hello MapReduce framework.</span></span> <span data-ttu-id="5e83d-2997">pomocí možnosti a hodnoty jako argumenty, jak ukazuje následující příklad hello toodifferentiate vaše argumenty s argumenty hello MapReduce, zvažte (- s, – vstup, – výstupní atd., jsou možnosti bezprostředně následované jejich hodnoty)</span><span class="sxs-lookup"><span data-stu-id="5e83d-2997">toodifferentiate your arguments with hello MapReduce arguments, consider using both option and value as arguments as shown in hello following example (-s, --input, --output etc., are options immediately followed by their values)</span></span> | 
| <span data-ttu-id="5e83d-2998">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="5e83d-2998">getDebugInfo</span></span> | <span data-ttu-id="5e83d-2999">Element volitelné.</span><span class="sxs-lookup"><span data-stu-id="5e83d-2999">An optional element.</span></span> <span data-ttu-id="5e83d-3000">Pokud je nastavené tooFailure, protokoly hello se stáhnou pouze při selhání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3000">When it is set tooFailure, hello logs are downloaded only on failure.</span></span> <span data-ttu-id="5e83d-3001">Pokud je nastavené tooAll, protokoly budou staženy vždy bez ohledu na stav spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3001">When it is set tooAll, logs are always downloaded irrespective of hello execution status.</span></span> | 

> [!NOTE]
> <span data-ttu-id="5e83d-3002">Je nutné zadat výstupní datovou sadu pro hello streamované aktivitě Hadoop pro hello **výstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3002">You must specify an output dataset for hello Hadoop Streaming Activity for hello **outputs** property.</span></span> <span data-ttu-id="5e83d-3003">Tato datová sada může být právě fiktivní datovou sadu, která je požadovaná toodrive hello kanálu plán (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3003">This dataset can be just a dummy dataset that is required toodrive hello pipeline schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="5e83d-3004">Pokud aktivita hello neberou vstup, můžete přeskočit zadávání vstupní datové sady pro hello aktivitu pro hello **vstupy** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3004">If hello activity doesn't take an input, you can skip specifying an input dataset for hello activity for hello **inputs** property.</span></span>  

## <a name="json-example"></a><span data-ttu-id="5e83d-3005">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3005">JSON example</span></span>

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

<span data-ttu-id="5e83d-3006">Další informace najdete v tématu [streamované aktivitě Hadoop](data-factory-hadoop-streaming-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3006">For more information, see [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md) article.</span></span> 

## <a name="hdinsight-spark-activity"></a><span data-ttu-id="5e83d-3007">Aktivita Spark služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="5e83d-3007">HDInsight Spark Activity</span></span>
<span data-ttu-id="5e83d-3008">Můžete zadat následující vlastnosti v definici JSON aktivity Spark hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3008">You can specify hello following properties in a Spark Activity JSON definition.</span></span> <span data-ttu-id="5e83d-3009">musí být vlastnost typu Hello aktivity hello: **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3009">hello type property for hello activity must be: **HDInsightSpark**.</span></span> <span data-ttu-id="5e83d-3010">Musíte nejprve vytvořit propojené služby HDInsight a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3010">You must create a HDInsight linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-3011">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooHDInsightSpark aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3011">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooHDInsightSpark:</span></span> 

| <span data-ttu-id="5e83d-3012">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3012">Property</span></span> | <span data-ttu-id="5e83d-3013">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3013">Description</span></span> | <span data-ttu-id="5e83d-3014">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3014">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="5e83d-3015">rootPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-3015">rootPath</span></span> | <span data-ttu-id="5e83d-3016">kontejner objektů Blob Azure Hello a složky, která obsahuje soubor Spark hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3016">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="5e83d-3017">Název souboru Hello je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3017">hello file name is case-sensitive.</span></span> | <span data-ttu-id="5e83d-3018">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3018">Yes</span></span> |
| <span data-ttu-id="5e83d-3019">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="5e83d-3019">entryFilePath</span></span> | <span data-ttu-id="5e83d-3020">Relativní cesta toohello kořenové složky hello Spark nebo balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3020">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="5e83d-3021">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3021">Yes</span></span> |
| <span data-ttu-id="5e83d-3022">Název třídy</span><span class="sxs-lookup"><span data-stu-id="5e83d-3022">className</span></span> | <span data-ttu-id="5e83d-3023">Hlavní třídy aplikace Java/Spark</span><span class="sxs-lookup"><span data-stu-id="5e83d-3023">Application's Java/Spark main class</span></span> | <span data-ttu-id="5e83d-3024">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3024">No</span></span> | 
| <span data-ttu-id="5e83d-3025">Argumenty</span><span class="sxs-lookup"><span data-stu-id="5e83d-3025">arguments</span></span> | <span data-ttu-id="5e83d-3026">Seznam argumentů příkazového řádku toohello Spark programu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3026">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="5e83d-3027">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3027">No</span></span> | 
| <span data-ttu-id="5e83d-3028">proxyUser</span><span class="sxs-lookup"><span data-stu-id="5e83d-3028">proxyUser</span></span> | <span data-ttu-id="5e83d-3029">Spark program hello tooimpersonate tooexecute Hello uživatelského účtu</span><span class="sxs-lookup"><span data-stu-id="5e83d-3029">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="5e83d-3030">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3030">No</span></span> | 
| <span data-ttu-id="5e83d-3031">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="5e83d-3031">sparkConfig</span></span> | <span data-ttu-id="5e83d-3032">Vlastnosti konfigurace Spark.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3032">Spark configuration properties.</span></span> | <span data-ttu-id="5e83d-3033">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3033">No</span></span> | 
| <span data-ttu-id="5e83d-3034">getdebuginfo –</span><span class="sxs-lookup"><span data-stu-id="5e83d-3034">getDebugInfo</span></span> | <span data-ttu-id="5e83d-3035">Určuje, když jsou soubory protokolu Spark hello zkopírovaný toohello úložiště Azure používá HDInsight cluster (nebo) zadaný ve sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3035">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="5e83d-3036">Povolené hodnoty: None, vždy nebo selhání.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3036">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="5e83d-3037">Výchozí hodnota: žádné.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3037">Default value: None.</span></span> | <span data-ttu-id="5e83d-3038">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3038">No</span></span> | 
| <span data-ttu-id="5e83d-3039">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="5e83d-3039">sparkJobLinkedService</span></span> | <span data-ttu-id="5e83d-3040">Hello Azure Storage propojená služba, která obsahuje soubor úlohy Spark hello, závislosti a protokoly.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3040">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="5e83d-3041">Pokud hodnotu pro tuto vlastnost nezadáte, použije se hello úložiště přidruženého k clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3041">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="5e83d-3042">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3042">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="5e83d-3043">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3043">JSON example</span></span>

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
<span data-ttu-id="5e83d-3044">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3044">Note hello following points:</span></span> 

- <span data-ttu-id="5e83d-3045">Hello **typ** vlastnost je nastavena příliš**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3045">hello **type** property is set too**HDInsightSpark**.</span></span>
- <span data-ttu-id="5e83d-3046">Hello **rootPath** je nastaven příliš**adfspark\\pyFiles** kde adfspark je kontejner objektů Blob Azure hello a pyFiles je dobře složka v tomto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3046">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="5e83d-3047">V tomto příkladu je hello Azure Blob Storage hello ten, který je přidružen hello Spark cluster.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3047">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="5e83d-3048">Můžete nahrát soubor tooa hello jiného úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3048">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="5e83d-3049">Pokud tak učiníte, vytváření Azure Storage, propojené služby toolink tohoto úložiště účet toohello dat.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3049">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="5e83d-3050">Potom zadejte název hello hello propojené služby jako hodnotu pro hello **sparkJobLinkedService** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3050">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="5e83d-3051">V tématu [vlastnosti aktivity Spark](#spark-activity-properties) podrobnosti o této vlastnosti a dalších vlastností nepodporuje hello Spark aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3051">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>
- <span data-ttu-id="5e83d-3052">Hello **entryFilePath** nastavena toohello **test.py**, což je soubor python hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3052">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span> 
- <span data-ttu-id="5e83d-3053">Hello **getdebuginfo –** vlastnost je nastavena příliš**vždy**, což znamená, že soubory protokolu hello jsou vždy vygeneruje (úspěch nebo neúspěch).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3053">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>  

    > [!IMPORTANT]
    > <span data-ttu-id="5e83d-3054">Doporučujeme vám, nenastavujte tato vlastnost tooAlways v produkčním prostředí, pokud jsou řešení problémů.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3054">We recommend that you do not set this property tooAlways in a production environment unless you are troubleshooting an issue.</span></span> 
- <span data-ttu-id="5e83d-3055">Hello **výstupy** obsahuje jednu výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3055">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="5e83d-3056">Je třeba zadat datovou sadu výstupů i v případě, že hello spark program nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3056">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="5e83d-3057">plán hello Hello výstupní datovou sadu jednotky pro kanál hello (hodinový, denní, atd.).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3057">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>

<span data-ttu-id="5e83d-3058">Další informace o aktivitě hello najdete v tématu [Spark aktivity](data-factory-spark.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3058">For more information about hello activity, see [Spark Activity](data-factory-spark.md) article.</span></span>  

## <a name="machine-learning-batch-execution-activity"></a><span data-ttu-id="5e83d-3059">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-3059">Machine Learning Batch Execution Activity</span></span>
<span data-ttu-id="5e83d-3060">Můžete zadat následující vlastnosti v definici Azure ML dávky spuštění aktivity JSON hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3060">You can specify hello following properties in a Azure ML Batch Execution Activity JSON definition.</span></span> <span data-ttu-id="5e83d-3061">musí být vlastnost typu Hello aktivity hello: **AzureMLBatchExecution**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3061">hello type property for hello activity must be: **AzureMLBatchExecution**.</span></span> <span data-ttu-id="5e83d-3062">Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3062">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-3063">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooAzureMLBatchExecution aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3063">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLBatchExecution:</span></span>

<span data-ttu-id="5e83d-3064">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3064">Property</span></span> | <span data-ttu-id="5e83d-3065">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3065">Description</span></span> | <span data-ttu-id="5e83d-3066">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3066">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="5e83d-3067">webServiceInput</span><span class="sxs-lookup"><span data-stu-id="5e83d-3067">webServiceInput</span></span> | <span data-ttu-id="5e83d-3068">datovou sadu toobe Hello předat jako vstup pro hello Azure ML webové služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3068">hello dataset toobe passed as an input for hello Azure ML web service.</span></span> <span data-ttu-id="5e83d-3069">Tato datová sada musí být součástí hello vstupy aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3069">This dataset must also be included in hello inputs for hello activity.</span></span> |<span data-ttu-id="5e83d-3070">Použijte webServiceInput nebo webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3070">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="5e83d-3071">webServiceInputs</span><span class="sxs-lookup"><span data-stu-id="5e83d-3071">webServiceInputs</span></span> | <span data-ttu-id="5e83d-3072">Zadejte, datové sady toobe předány jako vstupy pro hello Azure ML webové služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3072">Specify datasets toobe passed as inputs for hello Azure ML web service.</span></span> <span data-ttu-id="5e83d-3073">Pokud hello webová služba přijímá více vstupů, použijte vlastnost webServiceInputs hello místo použití hello webServiceInput vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3073">If hello web service takes multiple inputs, use hello webServiceInputs property instead of using hello webServiceInput property.</span></span> <span data-ttu-id="5e83d-3074">Datové sady, které odkazují hello **webServiceInputs** musí také obsahovat hello aktivity **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3074">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span> | <span data-ttu-id="5e83d-3075">Použijte webServiceInput nebo webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3075">Use either webServiceInput or webServiceInputs.</span></span> | 
<span data-ttu-id="5e83d-3076">webServiceOutputs</span><span class="sxs-lookup"><span data-stu-id="5e83d-3076">webServiceOutputs</span></span> | <span data-ttu-id="5e83d-3077">Hello datové sady, který je přiřazen jako výstup pro webovou službu Azure ML hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3077">hello datasets that are assigned as outputs for hello Azure ML web service.</span></span> <span data-ttu-id="5e83d-3078">Webová služba Hello vrátí výstupní data v této datové sadě.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3078">hello web service returns output data in this dataset.</span></span> | <span data-ttu-id="5e83d-3079">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3079">Yes</span></span> | 
<span data-ttu-id="5e83d-3080">globalParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-3080">globalParameters</span></span> | <span data-ttu-id="5e83d-3081">Zadejte hodnoty pro parametry hello webové služby v této části.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3081">Specify values for hello web service parameters in this section.</span></span> | <span data-ttu-id="5e83d-3082">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3082">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="5e83d-3083">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3083">JSON example</span></span>
<span data-ttu-id="5e83d-3084">V tomto příkladu hello aktivita má datovou sadu hello **MLSqlInput** jako vstup a **MLSqlOutput** jako výstup hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3084">In this example, hello activity has hello dataset **MLSqlInput** as input and **MLSqlOutput** as hello output.</span></span> <span data-ttu-id="5e83d-3085">Hello **MLSqlInput** se předá jako webové služby vstupní toohello podle pomocí hello **webServiceInput** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3085">hello **MLSqlInput** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="5e83d-3086">Hello **MLSqlOutput** se předá jako výstup toohello webové služby s použitím hello **webServiceOutputs** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3086">hello **MLSqlOutput** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span> 

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

<span data-ttu-id="5e83d-3087">V příkladu JSON hello hello nasadili Azure Machine Learning webové služby používá čtečka čipových karet a zapisovače modulu tooread a zápis dat z / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3087">In hello JSON example, hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="5e83d-3088">Tato webová služba zpřístupní hello následující čtyři parametry: název serveru, název databáze, název serveru uživatelského účtu a heslo uživatelského účtu serveru databáze.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3088">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>

> [!NOTE]
> <span data-ttu-id="5e83d-3089">Pouze vstupy a výstupy hello AzureMLBatchExecution aktivity mohou být předána jako toohello parametry webové služby.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3089">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="5e83d-3090">Například v hello výše fragmentu kódu JSON, je MLSqlInput vstupní toohello AzureMLBatchExecution aktivity, které se předá jako vstupní toohello webové služby pomocí parametru webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3090">For example, in hello above JSON snippet, MLSqlInput is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>

## <a name="machine-learning-update-resource-activity"></a><span data-ttu-id="5e83d-3091">Aktivita aktualizace prostředku služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e83d-3091">Machine Learning Update Resource Activity</span></span>
<span data-ttu-id="5e83d-3092">Můžete zadat následující vlastnosti v definici Azure ML aktualizace prostředků aktivity JSON hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3092">You can specify hello following properties in a Azure ML Update Resource Activity JSON definition.</span></span> <span data-ttu-id="5e83d-3093">musí být vlastnost typu Hello aktivity hello: **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3093">hello type property for hello activity must be: **AzureMLUpdateResource**.</span></span> <span data-ttu-id="5e83d-3094">Musíte vytvořit Azure Machine Learning nejprve propojené služby a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3094">You must create a Azure Machine Learning linked service first and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-3095">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooAzureMLUpdateResource aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3095">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooAzureMLUpdateResource:</span></span>

<span data-ttu-id="5e83d-3096">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3096">Property</span></span> | <span data-ttu-id="5e83d-3097">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3097">Description</span></span> | <span data-ttu-id="5e83d-3098">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3098">Required</span></span> 
-------- | ----------- | --------
<span data-ttu-id="5e83d-3099">trainedModelName</span><span class="sxs-lookup"><span data-stu-id="5e83d-3099">trainedModelName</span></span> | <span data-ttu-id="5e83d-3100">Název hello retrained modelu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3100">Name of hello retrained model.</span></span> | <span data-ttu-id="5e83d-3101">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3101">Yes</span></span> |  
<span data-ttu-id="5e83d-3102">trainedModelDatasetName</span><span class="sxs-lookup"><span data-stu-id="5e83d-3102">trainedModelDatasetName</span></span> | <span data-ttu-id="5e83d-3103">Datová sada polohovací toohello reprezentuje soubor iLearner vrácený hello retraining operaci.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3103">Dataset pointing toohello iLearner file returned by hello retraining operation.</span></span> | <span data-ttu-id="5e83d-3104">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3104">Yes</span></span> | 

### <a name="json-example"></a><span data-ttu-id="5e83d-3105">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3105">JSON example</span></span>
<span data-ttu-id="5e83d-3106">Hello kanálu má dvě aktivity: **AzureMLBatchExecution** a **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3106">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="5e83d-3107">Hello aktivita provedení dávky Azure ML trvá hello Cvičná data jako vstup a vytvoří soubor iLearner jako výstup.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3107">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="5e83d-3108">Aktivita Hello vyvolá hello školení webové služby (výukový experiment zveřejněné jako webovou službu) se vstupem hello trénovací data a obdrží od hello webservice soubor ilearner hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3108">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="5e83d-3109">Hello placeholderBlob je právě fiktivní výstupní datovou sadu, která vyžadují hello Azure Data Factory služby toorun hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3109">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>


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

## <a name="data-lake-analytics-u-sql-activity"></a><span data-ttu-id="5e83d-3110">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="5e83d-3110">Data Lake Analytics U-SQL Activity</span></span>
<span data-ttu-id="5e83d-3111">Můžete zadat hello následující vlastnosti v definici JSON aktivity U-SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3111">You can specify hello following properties in a U-SQL Activity JSON definition.</span></span> <span data-ttu-id="5e83d-3112">musí být vlastnost typu Hello aktivity hello: **DataLakeAnalyticsU SQL**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3112">hello type property for hello activity must be: **DataLakeAnalyticsU-SQL**.</span></span> <span data-ttu-id="5e83d-3113">Musíte vytvořit služby Azure Data Lake Analytics propojené a zadejte název hello ji jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3113">You must create an Azure Data Lake Analytics linked service and specify hello name of it as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-3114">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ aktivity tooDataLakeAnalyticsU-SQL:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3114">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDataLakeAnalyticsU-SQL:</span></span> 

| <span data-ttu-id="5e83d-3115">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3115">Property</span></span> | <span data-ttu-id="5e83d-3116">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3116">Description</span></span> | <span data-ttu-id="5e83d-3117">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3117">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-3118">scriptPath</span><span class="sxs-lookup"><span data-stu-id="5e83d-3118">scriptPath</span></span> |<span data-ttu-id="5e83d-3119">Cesta toofolder, který obsahuje skript hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3119">Path toofolder that contains hello U-SQL script.</span></span> <span data-ttu-id="5e83d-3120">Název souboru hello rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3120">Name of hello file is case-sensitive.</span></span> |<span data-ttu-id="5e83d-3121">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="5e83d-3121">No (if you use script)</span></span> |
| <span data-ttu-id="5e83d-3122">scriptLinkedService</span><span class="sxs-lookup"><span data-stu-id="5e83d-3122">scriptLinkedService</span></span> |<span data-ttu-id="5e83d-3123">Propojené služby, který odkazuje hello úložiště, který obsahuje toohello hello skriptu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="5e83d-3123">Linked service that links hello storage that contains hello script toohello data factory</span></span> |<span data-ttu-id="5e83d-3124">Ne (když používáte skript)</span><span class="sxs-lookup"><span data-stu-id="5e83d-3124">No (if you use script)</span></span> |
| <span data-ttu-id="5e83d-3125">Skript</span><span class="sxs-lookup"><span data-stu-id="5e83d-3125">script</span></span> |<span data-ttu-id="5e83d-3126">Zadejte místo zadání scriptPath a scriptLinkedService zpracování vloženého skriptu.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3126">Specify inline script instead of specifying scriptPath and scriptLinkedService.</span></span> <span data-ttu-id="5e83d-3127">Například: "skript": "Vytvořit databázi test".</span><span class="sxs-lookup"><span data-stu-id="5e83d-3127">For example: "script": "CREATE DATABASE test".</span></span> |<span data-ttu-id="5e83d-3128">Ne (když používáte scriptPath a scriptLinkedService)</span><span class="sxs-lookup"><span data-stu-id="5e83d-3128">No (if you use scriptPath and scriptLinkedService)</span></span> |
| <span data-ttu-id="5e83d-3129">degreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="5e83d-3129">degreeOfParallelism</span></span> |<span data-ttu-id="5e83d-3130">maximální počet uzlů Hello současně použít toorun hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3130">hello maximum number of nodes simultaneously used toorun hello job.</span></span> |<span data-ttu-id="5e83d-3131">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3131">No</span></span> |
| <span data-ttu-id="5e83d-3132">Priorita</span><span class="sxs-lookup"><span data-stu-id="5e83d-3132">priority</span></span> |<span data-ttu-id="5e83d-3133">Určuje, které z uložených ve frontě úloh by měl být vybrané toorun nejdřív.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3133">Determines which jobs out of all that are queued should be selected toorun first.</span></span> <span data-ttu-id="5e83d-3134">Hello nižší hello číslo, vyšší prioritu hello hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3134">hello lower hello number, hello higher hello priority.</span></span> |<span data-ttu-id="5e83d-3135">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3135">No</span></span> |
| <span data-ttu-id="5e83d-3136">parameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-3136">parameters</span></span> |<span data-ttu-id="5e83d-3137">Parametry pro skript hello U-SQL</span><span class="sxs-lookup"><span data-stu-id="5e83d-3137">Parameters for hello U-SQL script</span></span> |<span data-ttu-id="5e83d-3138">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3138">No</span></span> |

### <a name="json-example"></a><span data-ttu-id="5e83d-3139">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3139">JSON example</span></span>

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

<span data-ttu-id="5e83d-3140">Další informace najdete v tématu [Data Lake Analytics U-SQL aktivity](data-factory-usql-activity.md).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3140">For more information, see [Data Lake Analytics U-SQL Activity](data-factory-usql-activity.md).</span></span> 

## <a name="stored-procedure-activity"></a><span data-ttu-id="5e83d-3141">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="5e83d-3141">Stored Procedure Activity</span></span>
<span data-ttu-id="5e83d-3142">Můžete zadat následující vlastnosti v definici uložené procedury aktivity JSON hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3142">You can specify hello following properties in a Stored Procedure Activity JSON definition.</span></span> <span data-ttu-id="5e83d-3143">musí být vlastnost typu Hello aktivity hello: **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3143">hello type property for hello activity must be: **SqlServerStoredProcedure**.</span></span> <span data-ttu-id="5e83d-3144">Musíte vytvořit jednu z hello následující propojené služby a zadejte název hello hello propojené služby jako hodnotu pro hello **linkedServiceName** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3144">You must create an one of hello following linked services and specify hello name of hello linked service as a value for hello **linkedServiceName** property:</span></span>

- <span data-ttu-id="5e83d-3145">SQL Server</span><span class="sxs-lookup"><span data-stu-id="5e83d-3145">SQL Server</span></span> 
- <span data-ttu-id="5e83d-3146">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5e83d-3146">Azure SQL Database</span></span>
- <span data-ttu-id="5e83d-3147">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e83d-3147">Azure SQL Data Warehouse</span></span>

<span data-ttu-id="5e83d-3148">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooSqlServerStoredProcedure aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3148">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooSqlServerStoredProcedure:</span></span>

| <span data-ttu-id="5e83d-3149">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3149">Property</span></span> | <span data-ttu-id="5e83d-3150">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3150">Description</span></span> | <span data-ttu-id="5e83d-3151">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3151">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e83d-3152">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e83d-3152">storedProcedureName</span></span> |<span data-ttu-id="5e83d-3153">Zadejte název hello hello uložené procedury v hello Azure SQL database nebo Azure SQL Data Warehouse, která je reprezentována hello propojené služby, která hello používá výstupní tabulka.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3153">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="5e83d-3154">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3154">Yes</span></span> |
| <span data-ttu-id="5e83d-3155">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e83d-3155">storedProcedureParameters</span></span> |<span data-ttu-id="5e83d-3156">Zadejte hodnoty pro parametry uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3156">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="5e83d-3157">Pokud potřebujete toopass hodnotu null pro parametr, použijte syntaxi hello: "param1": null (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3157">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="5e83d-3158">Viz následující ukázka toolearn o používání této vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3158">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="5e83d-3159">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3159">No</span></span> |

<span data-ttu-id="5e83d-3160">Pokud zadáte vstupní datové sady, musí být k dispozici (v 'Připravený' stav) pro hello uložené procedury aktivity toorun.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3160">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="5e83d-3161">vstupní datové sady Hello nelze zpracovat v hello uložené procedury jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3161">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="5e83d-3162">Je pouze použité toocheck hello závislostí před počáteční hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3162">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> <span data-ttu-id="5e83d-3163">Je nutné zadat výstupní datovou sadu aktivity uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3163">You must specify an output dataset for a stored procedure activity.</span></span> 

<span data-ttu-id="5e83d-3164">Výstupní datové sady určuje hello **plán** pro hello uložené procedury aktivity (každou hodinu, týdně, měsíčně, atd.).</span><span class="sxs-lookup"><span data-stu-id="5e83d-3164">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <span data-ttu-id="5e83d-3165">Hello výstupní datovou sadu musí používat **propojená služba** který odkazuje tooan databáze SQL Azure nebo Azure SQL Data Warehouse nebo databázi SQL Server, ve kterém chcete hello toorun uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3165">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="5e83d-3166">Hello výstupní datovou sadu může sloužit jako výsledek hello toopass způsob hello uložené procedury pro následné zpracování pomocí jiné aktivity ([řetězení aktivity](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3166">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in hello pipeline.</span></span> <span data-ttu-id="5e83d-3167">Ale objekt pro vytváření dat nelze zapsat automaticky hello výstupní datové sady toothis uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3167">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="5e83d-3168">Je, že hello této tabulky SQL tooa zápisů, které hello výstupní datová sada odkazuje na uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3168">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="5e83d-3169">V některých případech může být hello výstupní datovou sadu **fiktivní datovou sadu**, který se používá jen toospecify hello plánu pro spuštěnou hello uložené procedury aktivity.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3169">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span>  

### <a name="json-example"></a><span data-ttu-id="5e83d-3170">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3170">JSON example</span></span>

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

<span data-ttu-id="5e83d-3171">Další informace najdete v tématu [aktivity uložené procedury](data-factory-stored-proc-activity.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3171">For more information, see [Stored Procedure Activity](data-factory-stored-proc-activity.md) article.</span></span> 

## <a name="net-custom-activity"></a><span data-ttu-id="5e83d-3172">Vlastní aktivita .NET</span><span class="sxs-lookup"><span data-stu-id="5e83d-3172">.NET custom activity</span></span>
<span data-ttu-id="5e83d-3173">Můžete zadat následující vlastnosti v rozhraní .NET vlastní aktivity definici JSON hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3173">You can specify hello following properties in a .NET custom activity JSON definition.</span></span> <span data-ttu-id="5e83d-3174">musí být vlastnost typu Hello aktivity hello: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3174">hello type property for hello activity must be: **DotNetActivity**.</span></span> <span data-ttu-id="5e83d-3175">Musíte vytvořit propojené služby Azure HDInsight nebo Azure Batch propojené služby a zadejte název hello hello propojené služby jako hodnotu pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3175">You must create an Azure HDInsight linked service or an Azure Batch linked service, and specify hello name of hello linked service as a value for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5e83d-3176">Hello následující vlastnosti jsou podporovány v hello **rámci typeProperties** části při nastavení hello typ tooDotNetActivity aktivity:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3176">hello following properties are supported in hello **typeProperties** section when you set hello type of activity tooDotNetActivity:</span></span>
 
| <span data-ttu-id="5e83d-3177">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="5e83d-3177">Property</span></span> | <span data-ttu-id="5e83d-3178">Popis</span><span class="sxs-lookup"><span data-stu-id="5e83d-3178">Description</span></span> | <span data-ttu-id="5e83d-3179">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="5e83d-3179">Required</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="5e83d-3180">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="5e83d-3180">AssemblyName</span></span> | <span data-ttu-id="5e83d-3181">Název sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3181">Name of hello assembly.</span></span> <span data-ttu-id="5e83d-3182">V příkladu hello je: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3182">In hello example, it is: **MyDotnetActivity.dll**.</span></span> | <span data-ttu-id="5e83d-3183">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3183">Yes</span></span> |
| <span data-ttu-id="5e83d-3184">Vstupní bod</span><span class="sxs-lookup"><span data-stu-id="5e83d-3184">EntryPoint</span></span> |<span data-ttu-id="5e83d-3185">Název třídy hello, který implementuje rozhraní IDotNetActivity hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3185">Name of hello class that implements hello IDotNetActivity interface.</span></span> <span data-ttu-id="5e83d-3186">V příkladu hello je: **MyDotNetActivityNS.MyDotNetActivity** kde MyDotNetActivityNS je obor názvů hello a MyDotNetActivity je třída hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3186">In hello example, it is: **MyDotNetActivityNS.MyDotNetActivity** where MyDotNetActivityNS is hello namespace and MyDotNetActivity is hello class.</span></span>  | <span data-ttu-id="5e83d-3187">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3187">Yes</span></span> | 
| <span data-ttu-id="5e83d-3188">PackageLinkedService</span><span class="sxs-lookup"><span data-stu-id="5e83d-3188">PackageLinkedService</span></span> | <span data-ttu-id="5e83d-3189">Název propojené služby Azure Storage ukazující toohello úložiště objektů blob, který obsahuje soubor zip vlastní aktivity hello hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3189">Name of hello Azure Storage linked service that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="5e83d-3190">V příkladu hello je: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3190">In hello example, it is: **AzureStorageLinkedService**.</span></span>| <span data-ttu-id="5e83d-3191">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3191">Yes</span></span> |
| <span data-ttu-id="5e83d-3192">PackageFile</span><span class="sxs-lookup"><span data-stu-id="5e83d-3192">PackageFile</span></span> | <span data-ttu-id="5e83d-3193">Název souboru zip hello.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3193">Name of hello zip file.</span></span> <span data-ttu-id="5e83d-3194">V příkladu hello je: **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3194">In hello example, it is: **customactivitycontainer/MyDotNetActivity.zip**.</span></span> | <span data-ttu-id="5e83d-3195">Ano</span><span class="sxs-lookup"><span data-stu-id="5e83d-3195">Yes</span></span> |
| <span data-ttu-id="5e83d-3196">ExtendedProperties</span><span class="sxs-lookup"><span data-stu-id="5e83d-3196">extendedProperties</span></span> | <span data-ttu-id="5e83d-3197">Rozšířené vlastnosti, které můžete definovat a předat na kód toohello .NET.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3197">Extended properties that you can define and pass on toohello .NET code.</span></span> <span data-ttu-id="5e83d-3198">V tomto příkladu hello **SliceStart** proměnná je nastavená hodnota tooa podle hello SliceStart systémové proměnné.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3198">In this example, hello **SliceStart** variable is set tooa value based on hello SliceStart system variable.</span></span> | <span data-ttu-id="5e83d-3199">Ne</span><span class="sxs-lookup"><span data-stu-id="5e83d-3199">No</span></span> | 

### <a name="json-example"></a><span data-ttu-id="5e83d-3200">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="5e83d-3200">JSON example</span></span>

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

<span data-ttu-id="5e83d-3201">Podrobné informace najdete v tématu [použít vlastní aktivity v datové továrně](data-factory-use-custom-activities.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e83d-3201">For detailed information, see [Use custom activities in Data Factory](data-factory-use-custom-activities.md) article.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5e83d-3202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5e83d-3202">Next Steps</span></span>
<span data-ttu-id="5e83d-3203">V tématu hello následující kurzy:</span><span class="sxs-lookup"><span data-stu-id="5e83d-3203">See hello following tutorials:</span></span> 

- [<span data-ttu-id="5e83d-3204">Kurz: vytvoření kanálu s aktivitou kopírování</span><span class="sxs-lookup"><span data-stu-id="5e83d-3204">Tutorial: create a pipeline with a copy activity</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [<span data-ttu-id="5e83d-3205">Kurz: vytvoření kanálu s aktivitou hive</span><span class="sxs-lookup"><span data-stu-id="5e83d-3205">Tutorial: create a pipeline with a hive activity</span></span>](data-factory-build-your-first-pipeline-using-editor.md)