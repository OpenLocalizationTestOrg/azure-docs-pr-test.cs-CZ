---
title: "aaaHow tooencode prostředek Azure pomocí kodéru Media Encoder Standard | Microsoft Docs"
description: "Zjistěte, jak toouse Media Encoder Standard tooencode média obsahu na službu Azure Media Services. Ukázky kódu pomocí rozhraní REST API."
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
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a><span data-ttu-id="897c0-104">Jak tooencode assetu pomocí kodéru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="897c0-104">How tooencode an asset by using Media Encoder Standard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="897c0-105">.NET</span><span class="sxs-lookup"><span data-stu-id="897c0-105">.NET</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [<span data-ttu-id="897c0-106">REST</span><span class="sxs-lookup"><span data-stu-id="897c0-106">REST</span></span>](media-services-rest-encode-asset.md)
> * [<span data-ttu-id="897c0-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="897c0-107">Portal</span></span>](media-services-portal-encode.md)
>
>

## <a name="overview"></a><span data-ttu-id="897c0-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="897c0-108">Overview</span></span>
<span data-ttu-id="897c0-109">toodeliver digitální video přes hello Internetu, musí komprese hello médií.</span><span class="sxs-lookup"><span data-stu-id="897c0-109">toodeliver digital video over hello Internet, you must compress hello media.</span></span> <span data-ttu-id="897c0-110">Digitální video soubory jsou velké, může být příliš velký toodeliver prostřednictvím hello Internet nebo pro vaše zákazníky zařízení toodisplay správně.</span><span class="sxs-lookup"><span data-stu-id="897c0-110">Digital video files are large and may be too big toodeliver over hello Internet, or for your customers’ devices toodisplay properly.</span></span> <span data-ttu-id="897c0-111">Kódování je proces hello komprimace videa a zvuku, takže vaši zákazníci mohou zobrazit médiu.</span><span class="sxs-lookup"><span data-stu-id="897c0-111">Encoding is hello process of compressing video and audio so your customers can view your media.</span></span>

<span data-ttu-id="897c0-112">Kódování úlohy jsou některé z nejběžnějších operací zpracování hello ve službě Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-112">Encoding jobs are one of hello most common processing operations in Azure Media Services.</span></span> <span data-ttu-id="897c0-113">Z jednoho kódování tooanother vytvoříte kódování úlohy tooconvert mediálních souborů.</span><span class="sxs-lookup"><span data-stu-id="897c0-113">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="897c0-114">Při kódování, můžete použít hello Media Services předdefinované kodér (Media Encoder Standard).</span><span class="sxs-lookup"><span data-stu-id="897c0-114">When you encode, you can use hello Media Services built-in encoder (Media Encoder Standard).</span></span> <span data-ttu-id="897c0-115">Můžete také použít kodér poskytovanými partnerem Media Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-115">You can also use an encoder provided by a Media Services partner.</span></span> <span data-ttu-id="897c0-116">Jsou k dispozici prostřednictvím Azure Marketplace hello kodéry třetích stran.</span><span class="sxs-lookup"><span data-stu-id="897c0-116">Third-party encoders are available through hello Azure Marketplace.</span></span> <span data-ttu-id="897c0-117">Můžete zadat podrobnosti hello úlohy kódování, pomocí přednastavení definované pro kodér nebo pomocí přednastavené konfigurační soubory.</span><span class="sxs-lookup"><span data-stu-id="897c0-117">You can specify hello details of encoding tasks by using preset strings defined for your encoder, or by using preset configuration files.</span></span> <span data-ttu-id="897c0-118">typy hello toosee přednastavení, které jsou k dispozici, najdete v části [přednastavení úloh pro Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="897c0-118">toosee hello types of presets that are available, see [Task Presets for Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="897c0-119">Každá úloha může mít jeden nebo více úloh v závislosti na typu hello zpracování, které chcete tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="897c0-119">Each job can have one or more tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="897c0-120">Prostřednictvím hello REST API můžete vytvořit úlohy a jejich související úlohy v jedné ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="897c0-120">Through hello REST API, you can create jobs and their related tasks in one of two ways:</span></span>

* <span data-ttu-id="897c0-121">Úlohy může být definována vložením prostřednictvím hello úlohy navigační vlastnost u entity úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-121">Tasks can be defined inline through hello Tasks navigation property on Job entities.</span></span>
* <span data-ttu-id="897c0-122">Použijte dávkovým zpracováním OData.</span><span class="sxs-lookup"><span data-stu-id="897c0-122">Use OData batch processing.</span></span>

<span data-ttu-id="897c0-123">Doporučujeme vždy zakódovat zdrojové soubory do sady souborů MP4 adaptivní přenosovou rychlostí a pak převést hello sadu toohello požadovaný formát pomocí [dynamické balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-123">We recommend that you always encode your source files into an adaptive bitrate MP4 set, and then convert hello set toohello desired format by using [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="897c0-124">Pokud výstupní asset používá šifrování úložiště, musíte nakonfigurovat zásady doručení assetu hello.</span><span class="sxs-lookup"><span data-stu-id="897c0-124">If your output asset is storage encrypted, you must configure hello asset delivery policy.</span></span> <span data-ttu-id="897c0-125">Další informace najdete v tématu [konfigurace zásad doručení assetu](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-125">For more information, see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="897c0-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="897c0-126">Considerations</span></span>

<span data-ttu-id="897c0-127">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="897c0-127">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="897c0-128">Další informace najdete v tématu [instalační program pro Media Services REST API vývoj](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-128">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

<span data-ttu-id="897c0-129">Než začnete, odkazující na procesory médií, ověřte, že mají hello ID správná média procesoru.</span><span class="sxs-lookup"><span data-stu-id="897c0-129">Before you start referencing media processors, verify that you have hello correct media processor ID.</span></span> <span data-ttu-id="897c0-130">Další informace najdete v tématu [získat procesory médií](media-services-rest-get-media-processor.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-130">For more information, see [Get media processors](media-services-rest-get-media-processor.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="897c0-131">Připojení služby tooMedia</span><span class="sxs-lookup"><span data-stu-id="897c0-131">Connect tooMedia Services</span></span>

<span data-ttu-id="897c0-132">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-132">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="897c0-133">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-133">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="897c0-134">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="897c0-134">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-a-job-with-a-single-encoding-task"></a><span data-ttu-id="897c0-135">Vytvoření úlohy pomocí jednoho úkolu kódování</span><span class="sxs-lookup"><span data-stu-id="897c0-135">Create a job with a single encoding task</span></span>
> [!NOTE]
> <span data-ttu-id="897c0-136">Při práci s hello Media Services REST API, hello platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="897c0-136">When you're working with hello Media Services REST API, hello following considerations apply:</span></span>
>
> <span data-ttu-id="897c0-137">Při přístupu k entity ve službě Media Services, musíte nastavit specifická pole hlaviček a hodnoty ve své žádosti HTTP.</span><span class="sxs-lookup"><span data-stu-id="897c0-137">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="897c0-138">Další informace najdete v tématu [nastavení pro vývoj pro Media Services REST API](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-138">For more information, see [Setup for Media Services REST API development](media-services-rest-how-to-use.md).</span></span>
>
> <span data-ttu-id="897c0-139">Po úspěšném připojení toohttps://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-139">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="897c0-140">Je nutné provést následující volání toohello nový identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="897c0-140">You must make subsequent calls toohello new URI.</span></span> <span data-ttu-id="897c0-141">Informace o tom, jak tooconnect toohello AMS rozhraní API, najdete v části [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-141">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
>
> <span data-ttu-id="897c0-142">Při použití formátu JSON a zadání toouse hello **__metadata** – klíčové slovo v požadavku hello (například tooreferences propojeného objektu), je nutné nastavit hello **přijmout** záhlaví příliš[JSON podrobný formát ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Přijměte: application/json; odata = verbose.</span><span class="sxs-lookup"><span data-stu-id="897c0-142">When using JSON and specifying toouse hello **__metadata** keyword in hello request (for example, tooreferences a linked object), you must set hello **Accept** header too[JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accept: application/json;odata=verbose.</span></span>
>
>

<span data-ttu-id="897c0-143">Hello následující ukázka ukazuje, jak toocreate a post úlohu s jeden úkol nastavit tooencode video na konkrétní řešení a kvality.</span><span class="sxs-lookup"><span data-stu-id="897c0-143">hello following example shows you how toocreate and post a job with one task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="897c0-144">Při kódování pomocí procesoru Media Encoder Standard, můžete použít přednastavení úloh konfigurace zadané [zde](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="897c0-144">When you encode with Media Encoder Standard, you can use task configuration presets specified [here](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="897c0-145">Žádost:</span><span class="sxs-lookup"><span data-stu-id="897c0-145">Request:</span></span>

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

<span data-ttu-id="897c0-146">Odpověď:</span><span class="sxs-lookup"><span data-stu-id="897c0-146">Response:</span></span>

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a><span data-ttu-id="897c0-147">Nastavte název prostředku výstup hello</span><span class="sxs-lookup"><span data-stu-id="897c0-147">Set hello output asset's name</span></span>
<span data-ttu-id="897c0-148">Hello následující příklad ukazuje, jak tooset hello assetName atribut:</span><span class="sxs-lookup"><span data-stu-id="897c0-148">hello following example shows how tooset hello assetName attribute:</span></span>

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a><span data-ttu-id="897c0-149">Požadavky</span><span class="sxs-lookup"><span data-stu-id="897c0-149">Considerations</span></span>
* <span data-ttu-id="897c0-150">Taskbody – vlastnosti musí používat literál XML toodefine hello počet vstup nebo výstup prostředky, které jsou používány hello úloh.</span><span class="sxs-lookup"><span data-stu-id="897c0-150">TaskBody properties must use literal XML toodefine hello number of input, or output assets that are used by hello task.</span></span> <span data-ttu-id="897c0-151">Hello úloh téma obsahuje hello definice schématu XML pro hello XML.</span><span class="sxs-lookup"><span data-stu-id="897c0-151">hello task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="897c0-152">V hello taskbody – definice, každý vnitřní hodnota <inputAsset> a <outputAsset> musí být nastavena jako JobInputAsset(value) nebo JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="897c0-152">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="897c0-153">Úloha může mít více prostředků výstup.</span><span class="sxs-lookup"><span data-stu-id="897c0-153">A task can have multiple output assets.</span></span> <span data-ttu-id="897c0-154">Jeden JobOutputAsset(x) lze použít jako výstup úlohy pro úlohu pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="897c0-154">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="897c0-155">Můžete zadat JobInputAsset nebo JobOutputAsset jako vstupní datový zdroj úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-155">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="897c0-156">Úlohy nesmí tvoří cyklus.</span><span class="sxs-lookup"><span data-stu-id="897c0-156">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="897c0-157">Parametr Hodnota Hello předáte tooJobInputAsset nebo JobOutputAsset představuje hodnotu hello index pro určitý prostředek.</span><span class="sxs-lookup"><span data-stu-id="897c0-157">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an asset.</span></span> <span data-ttu-id="897c0-158">Hello skutečné prostředky jsou definovány v hello InputMediaAssets a OutputMediaAssets navigačních vlastností v definici entity hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-158">hello actual assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello job entity definition.</span></span>
* <span data-ttu-id="897c0-159">Služba Media Services je integrovaná v OData v3, hello jednotlivé prostředky v hello InputMediaAssets a OutputMediaAssets navigační vlastnost kolekce se odkazuje prostřednictvím "__metadata: identifikátor uri" dvojice název hodnota.</span><span class="sxs-lookup"><span data-stu-id="897c0-159">Because Media Services is built on OData v3, hello individual assets in hello InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
* <span data-ttu-id="897c0-160">InputMediaAssets mapuje tooone nebo další prostředky, které jste vytvořili ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-160">InputMediaAssets maps tooone or more assets that you created in Media Services.</span></span> <span data-ttu-id="897c0-161">OutputMediaAssets jsou vytvořené pomocí systému hello.</span><span class="sxs-lookup"><span data-stu-id="897c0-161">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="897c0-162">Nemusíte se odkazovat na existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="897c0-162">They don't reference an existing asset.</span></span>
* <span data-ttu-id="897c0-163">OutputMediaAssets může mít název pomocí atributu assetName hello.</span><span class="sxs-lookup"><span data-stu-id="897c0-163">OutputMediaAssets can be named by using hello assetName attribute.</span></span> <span data-ttu-id="897c0-164">Pokud tento atribut není k dispozici, je název hello hello OutputMediaAsset libovolnou hodnotu vnitřní text hello hello <outputAsset> element je s příponou hello název úlohy hodnotu nebo hodnotu Id úlohy hello (v případě hello, kde není definován název vlastnosti hello).</span><span class="sxs-lookup"><span data-stu-id="897c0-164">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="897c0-165">Například pokud nastavíte hodnotu assetName příliš "Ukázkový", pak hello OutputMediaAsset název je nastavena příliš "ukázkové."</span><span class="sxs-lookup"><span data-stu-id="897c0-165">For example, if you set a value for assetName too"Sample," then hello OutputMediaAsset Name property is set too"Sample."</span></span> <span data-ttu-id="897c0-166">Pokud nebylo nastavit hodnotu assetName, ale nastavit název úlohy hello příliš "NewJob", pak hello název OutputMediaAsset by však bylo "_NewJob JobOutputAsset (hodnota)."</span><span class="sxs-lookup"><span data-stu-id="897c0-166">However, if you didn't set a value for assetName, but did set hello job name too"NewJob," then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob."</span></span>

## <a name="create-a-job-with-chained-tasks"></a><span data-ttu-id="897c0-167">Vytvořit úlohu zřetězené úlohy</span><span class="sxs-lookup"><span data-stu-id="897c0-167">Create a job with chained tasks</span></span>
<span data-ttu-id="897c0-168">V mnoha scénářích aplikace mají vývojáři toocreate řadu zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-168">In many application scenarios, developers want toocreate a series of processing tasks.</span></span> <span data-ttu-id="897c0-169">Ve službě Media Services můžete vytvořit řadu zřetězené úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-169">In Media Services, you can create a series of chained tasks.</span></span> <span data-ttu-id="897c0-170">Každý úkol provádí různé zpracování kroky a můžete použít různé média procesory.</span><span class="sxs-lookup"><span data-stu-id="897c0-170">Each task performs different processing steps and can use different media processors.</span></span> <span data-ttu-id="897c0-171">Hello zřetězené úlohy můžete z tooanother jeden úkol, provádění lineární pořadí úloh na hello asset přebírají prostředek.</span><span class="sxs-lookup"><span data-stu-id="897c0-171">hello chained tasks can hand off an asset from one task tooanother, performing a linear sequence of tasks on hello asset.</span></span> <span data-ttu-id="897c0-172">Hello úlohy prováděné v rámci úlohy však nejsou požadované toobe v pořadí.</span><span class="sxs-lookup"><span data-stu-id="897c0-172">However, hello tasks performed in a job are not required toobe in a sequence.</span></span> <span data-ttu-id="897c0-173">Když vytvoříte úlohu zřetězené, hello zřetězené **ITask** objekty jsou vytvořené v jedné **IJob** objektu.</span><span class="sxs-lookup"><span data-stu-id="897c0-173">When you create a chained task, hello chained **ITask** objects are created in a single **IJob** object.</span></span>

> [!NOTE]
> <span data-ttu-id="897c0-174">Je aktuálně omezeno na 30 úloh na úlohu.</span><span class="sxs-lookup"><span data-stu-id="897c0-174">There is currently a limit of 30 tasks per job.</span></span> <span data-ttu-id="897c0-175">Pokud potřebujete více než 30 úloh toochain, vytvořte více než jednu úlohu toocontain hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-175">If you need toochain more than 30 tasks, create more than one job toocontain hello tasks.</span></span>
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


### <a name="considerations"></a><span data-ttu-id="897c0-176">Požadavky</span><span class="sxs-lookup"><span data-stu-id="897c0-176">Considerations</span></span>
<span data-ttu-id="897c0-177">řetězení úloh tooenable:</span><span class="sxs-lookup"><span data-stu-id="897c0-177">tooenable task chaining:</span></span>

* <span data-ttu-id="897c0-178">Úloha musí mít alespoň dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-178">A job must have at least two tasks.</span></span>
* <span data-ttu-id="897c0-179">Musí existovat alespoň jeden úkol, jejichž vstup je výstup hello jiná úloha v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="897c0-179">There must be at least one task whose input is hello output of another task in hello job.</span></span>

## <a name="use-odata-batch-processing"></a><span data-ttu-id="897c0-180">Použít dávkovým zpracováním OData</span><span class="sxs-lookup"><span data-stu-id="897c0-180">Use OData batch processing</span></span>
<span data-ttu-id="897c0-181">Hello následující příklad ukazuje, jak toouse OData batch toocreate zpracování úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="897c0-181">hello following example shows how toouse OData batch processing toocreate a job and tasks.</span></span> <span data-ttu-id="897c0-182">Informace o zpracování dávky, najdete v části [Open Data Protocol (OData) dávkové zpracování](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="897c0-182">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

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



## <a name="create-a-job-by-using-a-jobtemplate"></a><span data-ttu-id="897c0-183">Vytvořit úlohu pomocí JobTemplate</span><span class="sxs-lookup"><span data-stu-id="897c0-183">Create a job by using a JobTemplate</span></span>
<span data-ttu-id="897c0-184">Když při zpracování více prostředků pomocí společnou sadu úloh, pomocí úlohu JobTemplate toospecify hello výchozí přednastavení nebo tooset hello pořadí úkolů.</span><span class="sxs-lookup"><span data-stu-id="897c0-184">When you process multiple assets by using a common set of tasks, use a JobTemplate toospecify hello default task presets, or tooset hello order of tasks.</span></span>

<span data-ttu-id="897c0-185">Hello následující příklad ukazuje, jak toocreate JobTemplate s TaskTemplate, který je definovanými v řádku.</span><span class="sxs-lookup"><span data-stu-id="897c0-185">hello following example shows how toocreate a JobTemplate with a TaskTemplate that is defined inline.</span></span> <span data-ttu-id="897c0-186">Hello TaskTemplate používá hello Media Encoder Standard jako hello MediaProcessor tooencode hello asset soubor.</span><span class="sxs-lookup"><span data-stu-id="897c0-186">hello TaskTemplate uses hello Media Encoder Standard as hello MediaProcessor tooencode hello asset file.</span></span> <span data-ttu-id="897c0-187">Další MediaProcessors však lze použít také.</span><span class="sxs-lookup"><span data-stu-id="897c0-187">However, other MediaProcessors can be used as well.</span></span>

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
> <span data-ttu-id="897c0-188">Na rozdíl od jinými entitami Media Services musí definovat nový identifikátor GUID pro každou TaskTemplate a umístěte jej v hello taskTemplateId a vlastnost Id v textu vaší žádosti.</span><span class="sxs-lookup"><span data-stu-id="897c0-188">Unlike other Media Services entities, you must define a new GUID identifier for each TaskTemplate and place it in hello taskTemplateId and Id property in your request body.</span></span> <span data-ttu-id="897c0-189">schéma obsahu identifikace Hello musí následovat hello schéma popsaných v identifikovat Azure Media Services entity.</span><span class="sxs-lookup"><span data-stu-id="897c0-189">hello content identification scheme must follow hello scheme described in Identify Azure Media Services Entities.</span></span> <span data-ttu-id="897c0-190">Navíc JobTemplates nelze aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="897c0-190">Also, JobTemplates cannot be updated.</span></span> <span data-ttu-id="897c0-191">Místo toho musíte vytvořit novou s aktualizované změny.</span><span class="sxs-lookup"><span data-stu-id="897c0-191">Instead, you must create a new one with your updated changes.</span></span>
>
>

<span data-ttu-id="897c0-192">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="897c0-192">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .


<span data-ttu-id="897c0-193">Následující příklad ukazuje, jak Hello toocreate úlohu, která odkazuje na JobTemplate Id:</span><span class="sxs-lookup"><span data-stu-id="897c0-193">hello following example shows how toocreate a job that references a JobTemplate Id:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


<span data-ttu-id="897c0-194">V případě úspěchu se vrátí hello následující odpověď:</span><span class="sxs-lookup"><span data-stu-id="897c0-194">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a><span data-ttu-id="897c0-195">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="897c0-195">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="897c0-196">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="897c0-196">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="897c0-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="897c0-197">Next steps</span></span>
<span data-ttu-id="897c0-198">Teď, když víte, jak toocreate tooencode úlohy na prostředek, najdete v části [jak toocheck úlohy průběh pomocí služby Media Services](media-services-rest-check-job-progress.md).</span><span class="sxs-lookup"><span data-stu-id="897c0-198">Now that you know how toocreate a job tooencode an asset, see [How toocheck job progress with Media Services](media-services-rest-check-job-progress.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="897c0-199">Viz také</span><span class="sxs-lookup"><span data-stu-id="897c0-199">See also</span></span>
[<span data-ttu-id="897c0-200">Získat procesory médií</span><span class="sxs-lookup"><span data-stu-id="897c0-200">Get Media Processors</span></span>](media-services-rest-get-media-processor.md)
