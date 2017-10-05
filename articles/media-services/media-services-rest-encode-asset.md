---
title: "Postup kódování Azure assetu pomocí kodéru Media Encoder Standard | Microsoft Docs"
description: "Další informace o použití Media Encoder Standard ke kódování mediální obsah v Azure Media Services. Ukázky kódu pomocí rozhraní REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 796f3b5a4dd56a0160986600cbbcf38faf8add56
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a><span data-ttu-id="f161e-104">Postup kódování assetu pomocí kodéru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="f161e-104">How to encode an asset by using Media Encoder Standard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f161e-105">.NET</span><span class="sxs-lookup"><span data-stu-id="f161e-105">.NET</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [<span data-ttu-id="f161e-106">REST</span><span class="sxs-lookup"><span data-stu-id="f161e-106">REST</span></span>](media-services-rest-encode-asset.md)
> * [<span data-ttu-id="f161e-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f161e-107">Portal</span></span>](media-services-portal-encode.md)
>
>

## <a name="overview"></a><span data-ttu-id="f161e-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="f161e-108">Overview</span></span>
<span data-ttu-id="f161e-109">Chcete-li poskytovat digitální video přes Internet, je nutné médium komprimovat.</span><span class="sxs-lookup"><span data-stu-id="f161e-109">To deliver digital video over the Internet, you must compress the media.</span></span> <span data-ttu-id="f161e-110">Digitální video soubory jsou velké, může být příliš velký pro doručení přes Internet nebo pro zařízení vašich zákazníků a zobrazeny správně.</span><span class="sxs-lookup"><span data-stu-id="f161e-110">Digital video files are large and may be too big to deliver over the Internet, or for your customers’ devices to display properly.</span></span> <span data-ttu-id="f161e-111">Kódování je proces komprimace videa a zvuku, takže vaši zákazníci mohou zobrazit médiu.</span><span class="sxs-lookup"><span data-stu-id="f161e-111">Encoding is the process of compressing video and audio so your customers can view your media.</span></span>

<span data-ttu-id="f161e-112">Kódování úlohy jsou některé z nejběžnějších zpracování operací ve službě Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="f161e-112">Encoding jobs are one of the most common processing operations in Azure Media Services.</span></span> <span data-ttu-id="f161e-113">K převodu mediálních souborů z jednoho kódování do druhého se využívají kódovací úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-113">You create encoding jobs to convert media files from one encoding to another.</span></span> <span data-ttu-id="f161e-114">Při kódování, můžete použít předdefinované kodéru Media Services (Media Encoder Standard).</span><span class="sxs-lookup"><span data-stu-id="f161e-114">When you encode, you can use the Media Services built-in encoder (Media Encoder Standard).</span></span> <span data-ttu-id="f161e-115">Můžete také použít kodér poskytovanými partnerem Media Services.</span><span class="sxs-lookup"><span data-stu-id="f161e-115">You can also use an encoder provided by a Media Services partner.</span></span> <span data-ttu-id="f161e-116">Třetí strany kodéry jsou k dispozici prostřednictvím Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="f161e-116">Third-party encoders are available through the Azure Marketplace.</span></span> <span data-ttu-id="f161e-117">Můžete zadat podrobnosti úlohy kódování, pomocí přednastavení definované pro kodér nebo pomocí přednastavené konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="f161e-117">You can specify the details of encoding tasks by using preset strings defined for your encoder, or by using preset configuration files.</span></span> <span data-ttu-id="f161e-118">Typy přednastavení, které jsou k dispozici, najdete v sekci [přednastavení úloh pro Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="f161e-118">To see the types of presets that are available, see [Task Presets for Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="f161e-119">Každá úloha může mít jeden nebo více úloh, v závislosti na typu zpracování, který chcete provést.</span><span class="sxs-lookup"><span data-stu-id="f161e-119">Each job can have one or more tasks depending on the type of processing that you want to accomplish.</span></span> <span data-ttu-id="f161e-120">Přes rozhraní REST API můžete vytvořit úlohy a jejich související úlohy v jedné ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="f161e-120">Through the REST API, you can create jobs and their related tasks in one of two ways:</span></span>

* <span data-ttu-id="f161e-121">Úlohy může být definována vložením prostřednictvím úlohy navigační vlastnost u entity úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-121">Tasks can be defined inline through the Tasks navigation property on Job entities.</span></span>
* <span data-ttu-id="f161e-122">Použijte dávkovým zpracováním OData.</span><span class="sxs-lookup"><span data-stu-id="f161e-122">Use OData batch processing.</span></span>

<span data-ttu-id="f161e-123">Doporučujeme vždy zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak sadu převést na požadovaný formát pomocí [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-123">We recommend that you always encode your source files into an adaptive bitrate MP4 set, and then convert the set to the desired format by using [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="f161e-124">Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="f161e-124">If your output asset is storage encrypted, you must configure the asset delivery policy.</span></span> <span data-ttu-id="f161e-125">Další informace najdete v tématu [konfigurace zásad doručení assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-125">For more information, see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="f161e-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f161e-126">Considerations</span></span>

<span data-ttu-id="f161e-127">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="f161e-127">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="f161e-128">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-128">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

<span data-ttu-id="f161e-129">Než začnete, odkazující na procesory médií, ověřte, zda máte správná média ID procesoru.</span><span class="sxs-lookup"><span data-stu-id="f161e-129">Before you start referencing media processors, verify that you have the correct media processor ID.</span></span> <span data-ttu-id="f161e-130">Další informace najdete v tématu [získat procesory médií](media-services-rest-get-media-processor.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-130">For more information, see [Get media processors](media-services-rest-get-media-processor.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="f161e-131">Připojení ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f161e-131">Connect to Media Services</span></span>

<span data-ttu-id="f161e-132">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-132">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="f161e-133">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="f161e-133">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="f161e-134">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="f161e-134">You must make subsequent calls to the new URI.</span></span>

## <a name="create-a-job-with-a-single-encoding-task"></a><span data-ttu-id="f161e-135">Vytvoření úlohy pomocí jednoho úkolu kódování</span><span class="sxs-lookup"><span data-stu-id="f161e-135">Create a job with a single encoding task</span></span>
> [!NOTE]
> <span data-ttu-id="f161e-136">Při práci s Media Services REST API, platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="f161e-136">When you're working with the Media Services REST API, the following considerations apply:</span></span>
>
> <span data-ttu-id="f161e-137">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="f161e-137">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="f161e-138">Další informace najdete v tématu [nastavení pro vývoj pro Media Services REST API](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-138">For more information, see [Setup for Media Services REST API development](media-services-rest-how-to-use.md).</span></span>
>
> <span data-ttu-id="f161e-139">Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="f161e-139">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="f161e-140">Je nutné provést následující volání nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="f161e-140">You must make subsequent calls to the new URI.</span></span> <span data-ttu-id="f161e-141">Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-141">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
>
> <span data-ttu-id="f161e-142">Při použití formátu JSON a určení pro použití **__metadata** – klíčové slovo v žádosti o (například k odkazuje na objekt odkazovaný), musíte nastavit **přijmout** hlavičky k [JSON podrobný formát](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): přijmout: application/json; odata = podrobné.</span><span class="sxs-lookup"><span data-stu-id="f161e-142">When using JSON and specifying to use the **__metadata** keyword in the request (for example, to references a linked object), you must set the **Accept** header to [JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accept: application/json;odata=verbose.</span></span>
>
>

<span data-ttu-id="f161e-143">Následující příklad ukazuje, jak vytvořit a odeslat úlohu s jeden úkol nastavit ke kódování videa na konkrétní řešení a kvality.</span><span class="sxs-lookup"><span data-stu-id="f161e-143">The following example shows you how to create and post a job with one task set to encode a video at a specific resolution and quality.</span></span> <span data-ttu-id="f161e-144">Při kódování pomocí procesoru Media Encoder Standard, můžete použít přednastavení úloh konfigurace zadané [zde](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="f161e-144">When you encode with Media Encoder Standard, you can use task configuration presets specified [here](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="f161e-145">Žádost:</span><span class="sxs-lookup"><span data-stu-id="f161e-145">Request:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

<span data-ttu-id="f161e-146">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="f161e-146">Response:</span></span>

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a><span data-ttu-id="f161e-147">Nastavte název výstupního assetu</span><span class="sxs-lookup"><span data-stu-id="f161e-147">Set the output asset's name</span></span>
<span data-ttu-id="f161e-148">Následující příklad ukazuje, jak nastavit atribut assetName:</span><span class="sxs-lookup"><span data-stu-id="f161e-148">The following example shows how to set the assetName attribute:</span></span>

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a><span data-ttu-id="f161e-149">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f161e-149">Considerations</span></span>
* <span data-ttu-id="f161e-150">Taskbody – vlastnosti používaly literál XML Definujte počet vstup nebo výstup prostředky, které jsou používány úlohu.</span><span class="sxs-lookup"><span data-stu-id="f161e-150">TaskBody properties must use literal XML to define the number of input, or output assets that are used by the task.</span></span> <span data-ttu-id="f161e-151">Úloha téma obsahuje definici schématu XML pro soubor XML.</span><span class="sxs-lookup"><span data-stu-id="f161e-151">The task topic contains the XML Schema Definition for the XML.</span></span>
* <span data-ttu-id="f161e-152">V definici taskbody – každý vnitřní hodnota <inputAsset> a <outputAsset> musí být nastavena jako JobInputAsset(value) nebo JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="f161e-152">In the TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="f161e-153">Úloha může mít více prostředků výstup.</span><span class="sxs-lookup"><span data-stu-id="f161e-153">A task can have multiple output assets.</span></span> <span data-ttu-id="f161e-154">Jeden JobOutputAsset(x) lze použít jako výstup úlohy pro úlohu pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="f161e-154">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="f161e-155">Můžete zadat JobInputAsset nebo JobOutputAsset jako vstupní datový zdroj úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-155">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="f161e-156">Úlohy nesmí tvoří cyklus.</span><span class="sxs-lookup"><span data-stu-id="f161e-156">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="f161e-157">Hodnota parametru, který můžete předat JobInputAsset nebo JobOutputAsset představuje hodnotu indexu pro určitý prostředek.</span><span class="sxs-lookup"><span data-stu-id="f161e-157">The value parameter that you pass to JobInputAsset or JobOutputAsset represents the index value for an asset.</span></span> <span data-ttu-id="f161e-158">Skutečné prostředky jsou definovány v navigační vlastnosti InputMediaAssets a OutputMediaAssets v definici úlohy entity.</span><span class="sxs-lookup"><span data-stu-id="f161e-158">The actual assets are defined in the InputMediaAssets and OutputMediaAssets navigation properties on the job entity definition.</span></span>
* <span data-ttu-id="f161e-159">Služba Media Services je integrovaná v OData v3, jsou jednotlivé prostředky v kolekcích navigační vlastnost InputMediaAssets a OutputMediaAssets odkazuje prostřednictvím "__metadata: identifikátor uri" dvojice název hodnota.</span><span class="sxs-lookup"><span data-stu-id="f161e-159">Because Media Services is built on OData v3, the individual assets in the InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
* <span data-ttu-id="f161e-160">InputMediaAssets se mapuje na jeden nebo více prostředků, které jste vytvořili ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="f161e-160">InputMediaAssets maps to one or more assets that you created in Media Services.</span></span> <span data-ttu-id="f161e-161">OutputMediaAssets jsou vytvořené v systému.</span><span class="sxs-lookup"><span data-stu-id="f161e-161">OutputMediaAssets are created by the system.</span></span> <span data-ttu-id="f161e-162">Nemusíte se odkazovat na existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="f161e-162">They don't reference an existing asset.</span></span>
* <span data-ttu-id="f161e-163">Pomocí atributu assetName může mít název OutputMediaAssets.</span><span class="sxs-lookup"><span data-stu-id="f161e-163">OutputMediaAssets can be named by using the assetName attribute.</span></span> <span data-ttu-id="f161e-164">Pokud tento atribut není k dispozici, je název OutputMediaAsset bez ohledu na hodnotu vnitřní text z <outputAsset> element je s příponou název úlohy hodnota nebo hodnota Id úlohy (v případě, kde není definována vlastnost Name).</span><span class="sxs-lookup"><span data-stu-id="f161e-164">If this attribute is not present, then the name of the OutputMediaAsset is whatever the inner text value of the <outputAsset> element is with a suffix of either the Job Name value, or the Job Id value (in the case where the Name property isn't defined).</span></span> <span data-ttu-id="f161e-165">Například pokud nastavíte hodnotu assetName "Ukázce", pak název OutputMediaAsset je nastavena na "Ukázka."</span><span class="sxs-lookup"><span data-stu-id="f161e-165">For example, if you set a value for assetName to "Sample," then the OutputMediaAsset Name property is set to "Sample."</span></span> <span data-ttu-id="f161e-166">Ale pokud nebylo nastavit hodnotu assetName, ale nastavena název úlohy na "NewJob", pak OutputMediaAsset název by měl být "_NewJob JobOutputAsset (hodnota)."</span><span class="sxs-lookup"><span data-stu-id="f161e-166">However, if you didn't set a value for assetName, but did set the job name to "NewJob," then the OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob."</span></span>

## <a name="create-a-job-with-chained-tasks"></a><span data-ttu-id="f161e-167">Vytvořit úlohu zřetězené úlohy</span><span class="sxs-lookup"><span data-stu-id="f161e-167">Create a job with chained tasks</span></span>
<span data-ttu-id="f161e-168">V mnoha scénářích aplikace vývojáři chcete vytvořit řadu zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-168">In many application scenarios, developers want to create a series of processing tasks.</span></span> <span data-ttu-id="f161e-169">Ve službě Media Services můžete vytvořit řadu zřetězené úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-169">In Media Services, you can create a series of chained tasks.</span></span> <span data-ttu-id="f161e-170">Každý úkol provádí různé zpracování kroky a můžete použít různé média procesory.</span><span class="sxs-lookup"><span data-stu-id="f161e-170">Each task performs different processing steps and can use different media processors.</span></span> <span data-ttu-id="f161e-171">Zřetězené úlohy můžete předat prostředek od jednoho do jiného provádění lineární pořadí úloh na asset.</span><span class="sxs-lookup"><span data-stu-id="f161e-171">The chained tasks can hand off an asset from one task to another, performing a linear sequence of tasks on the asset.</span></span> <span data-ttu-id="f161e-172">Úlohy prováděné v rámci úlohy, ale nemusejí být v pořadí.</span><span class="sxs-lookup"><span data-stu-id="f161e-172">However, the tasks performed in a job are not required to be in a sequence.</span></span> <span data-ttu-id="f161e-173">Když vytvoříte úlohu zřetězené, zřetězené **ITask** objekty jsou vytvořené v jedné **IJob** objektu.</span><span class="sxs-lookup"><span data-stu-id="f161e-173">When you create a chained task, the chained **ITask** objects are created in a single **IJob** object.</span></span>

> [!NOTE]
> <span data-ttu-id="f161e-174">Je aktuálně omezeno na 30 úloh na úlohu.</span><span class="sxs-lookup"><span data-stu-id="f161e-174">There is currently a limit of 30 tasks per job.</span></span> <span data-ttu-id="f161e-175">Pokud potřebujete zřetězit více než 30 úloh, vytvořte více než jednu úlohu tak, aby obsahovala úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-175">If you need to chain more than 30 tasks, create more than one job to contain the tasks.</span></span>
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a><span data-ttu-id="f161e-176">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f161e-176">Considerations</span></span>
<span data-ttu-id="f161e-177">Chcete-li povolit řetězení úloh:</span><span class="sxs-lookup"><span data-stu-id="f161e-177">To enable task chaining:</span></span>

* <span data-ttu-id="f161e-178">Úloha musí mít alespoň dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-178">A job must have at least two tasks.</span></span>
* <span data-ttu-id="f161e-179">Musí existovat alespoň jeden úkol, jejichž vstup je výstup jiná úloha v úloze.</span><span class="sxs-lookup"><span data-stu-id="f161e-179">There must be at least one task whose input is the output of another task in the job.</span></span>

## <a name="use-odata-batch-processing"></a><span data-ttu-id="f161e-180">Použít dávkovým zpracováním OData</span><span class="sxs-lookup"><span data-stu-id="f161e-180">Use OData batch processing</span></span>
<span data-ttu-id="f161e-181">Následující příklad ukazuje, jak používat dávkovým zpracováním OData k vytvoření úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="f161e-181">The following example shows how to use OData batch processing to create a job and tasks.</span></span> <span data-ttu-id="f161e-182">Informace o zpracování dávky, najdete v části [Open Data Protocol (OData) dávkové zpracování](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="f161e-182">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a><span data-ttu-id="f161e-183">Vytvořit úlohu pomocí JobTemplate</span><span class="sxs-lookup"><span data-stu-id="f161e-183">Create a job by using a JobTemplate</span></span>
<span data-ttu-id="f161e-184">Když při zpracování více prostředků pomocí společnou sadu úloh, použijte JobTemplate zadat výchozí přednastavení úloh nebo nastavit pořadí úkolů.</span><span class="sxs-lookup"><span data-stu-id="f161e-184">When you process multiple assets by using a common set of tasks, use a JobTemplate to specify the default task presets, or to set the order of tasks.</span></span>

<span data-ttu-id="f161e-185">Následující příklad ukazuje, jak vytvořit JobTemplate s TaskTemplate, která je definována vložením.</span><span class="sxs-lookup"><span data-stu-id="f161e-185">The following example shows how to create a JobTemplate with a TaskTemplate that is defined inline.</span></span> <span data-ttu-id="f161e-186">TaskTemplate používá Media Encoder Standard jako MediaProcessor ke kódování souboru prostředku.</span><span class="sxs-lookup"><span data-stu-id="f161e-186">The TaskTemplate uses the Media Encoder Standard as the MediaProcessor to encode the asset file.</span></span> <span data-ttu-id="f161e-187">Další MediaProcessors však lze použít také.</span><span class="sxs-lookup"><span data-stu-id="f161e-187">However, other MediaProcessors can be used as well.</span></span>

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> <span data-ttu-id="f161e-188">Na rozdíl od jinými entitami Media Services musíte definovat nový identifikátor GUID pro každou TaskTemplate a umístěte jej v taskTemplateId a vlastnost Id v textu vaší žádosti.</span><span class="sxs-lookup"><span data-stu-id="f161e-188">Unlike other Media Services entities, you must define a new GUID identifier for each TaskTemplate and place it in the taskTemplateId and Id property in your request body.</span></span> <span data-ttu-id="f161e-189">Schéma obsahu identifikace musí následovat schéma popsaných v identifikovat Azure Media Services entity.</span><span class="sxs-lookup"><span data-stu-id="f161e-189">The content identification scheme must follow the scheme described in Identify Azure Media Services Entities.</span></span> <span data-ttu-id="f161e-190">Navíc JobTemplates nelze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f161e-190">Also, JobTemplates cannot be updated.</span></span> <span data-ttu-id="f161e-191">Místo toho musíte vytvořit novou s aktualizované změny.</span><span class="sxs-lookup"><span data-stu-id="f161e-191">Instead, you must create a new one with your updated changes.</span></span>
>
>

<span data-ttu-id="f161e-192">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="f161e-192">If successful, the following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .


<span data-ttu-id="f161e-193">Následující příklad ukazuje, jak vytvořit úlohu, která odkazuje na JobTemplate Id:</span><span class="sxs-lookup"><span data-stu-id="f161e-193">The following example shows how to create a job that references a JobTemplate Id:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


<span data-ttu-id="f161e-194">V případě úspěchu se vrátí následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="f161e-194">If successful, the following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a><span data-ttu-id="f161e-195">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f161e-195">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f161e-196">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f161e-196">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="f161e-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f161e-197">Next steps</span></span>
<span data-ttu-id="f161e-198">Teď, když víte, jak vytvořit úlohu kódování assetu, najdete v článku [jak zkontrolovat průběh úlohy pomocí služby Media Services](media-services-rest-check-job-progress.md).</span><span class="sxs-lookup"><span data-stu-id="f161e-198">Now that you know how to create a job to encode an asset, see [How to check job progress with Media Services](media-services-rest-check-job-progress.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f161e-199">Viz také</span><span class="sxs-lookup"><span data-stu-id="f161e-199">See also</span></span>
[<span data-ttu-id="f161e-200">Získat procesory médií</span><span class="sxs-lookup"><span data-stu-id="f161e-200">Get Media Processors</span></span>](media-services-rest-get-media-processor.md)
