---
title: "porovnání Azure na a aaaOverview potřebují média kodéry | Microsoft Docs"
description: "Toto téma nabízí přehled a porovnání Azure na vyžádání kodéry média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a><span data-ttu-id="32873-103">Přehled a porovnání Azure na vyžádání média kodéry</span><span class="sxs-lookup"><span data-stu-id="32873-103">Overview and comparison of Azure on demand media encoders</span></span>
## <a name="encoding-overview"></a><span data-ttu-id="32873-104">Kódování – přehled</span><span class="sxs-lookup"><span data-stu-id="32873-104">Encoding overview</span></span>
<span data-ttu-id="32873-105">Azure Media Services poskytuje několik možností hello kódování médií v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="32873-105">Azure Media Services provides multiple options for hello encoding of media in hello cloud.</span></span>

<span data-ttu-id="32873-106">Při spouštění pomocí služby Media Services, je důležité toounderstand hello rozdíl mezi formáty kodeků a souboru.</span><span class="sxs-lookup"><span data-stu-id="32873-106">When starting out with Media Services, it is important toounderstand hello difference between codecs and file formats.</span></span>
<span data-ttu-id="32873-107">Kodeky jsou hello software, který implementuje hello kompresi nebo dekompresi algoritmy, zatímco formáty souborů jsou kontejnery, které obsahují hello komprimované video.</span><span class="sxs-lookup"><span data-stu-id="32873-107">Codecs are hello software that implements hello compression/decompression algorithms whereas file formats are containers that hold hello compressed video.</span></span>

<span data-ttu-id="32873-108">Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver vaše s adaptivní přenosovou rychlostí MP4 nebo technologie Smooth Streaming kódování obsahu ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) bez nutnosti toore balíček do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="32873-108">Media Services provides dynamic packaging which allows you toodeliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) without you having toore-package into these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="32873-109">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="32873-109">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="32873-110">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="32873-110">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> <span data-ttu-id="32873-111">tootake výhod [dynamické balení](media-services-dynamic-packaging-overview.md), je nutné toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="32873-111">tootake advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need toodo hello following:</span></span>
>
><span data-ttu-id="32873-112">Navíc zakódujte váš zdrojový soubor do sady souborů MP4 nebo technologie Smooth Streaming soubory s adaptivní přenosovou rychlostí (postup hello kódování je ukázán později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="32873-112">Also, encode your source file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (hello encoding steps are demonstrated later in this tutorial).</span></span>

<span data-ttu-id="32873-113">Služba Media Services podporuje následující hello na vyžádání kodéry, které jsou popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="32873-113">Media Services supports hello following on demand encoders that are described in this article:</span></span>

* [<span data-ttu-id="32873-114">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="32873-114">Media Encoder Standard</span></span>](media-services-encode-asset.md#media-encoder-standard)
* [<span data-ttu-id="32873-115">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="32873-115">Media Encoder Premium Workflow</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

<span data-ttu-id="32873-116">Tento článek poskytuje stručný přehled na vyžádání kodéry média a poskytuje tooarticles odkazy, které poskytují podrobnější informace.</span><span class="sxs-lookup"><span data-stu-id="32873-116">This article gives a brief overview of on demand media encoders and provides links tooarticles that give more detailed information.</span></span> <span data-ttu-id="32873-117">Hello téma obsahuje také porovnání kodéry hello.</span><span class="sxs-lookup"><span data-stu-id="32873-117">hello topic also provides comparison of hello encoders.</span></span>

>[!NOTE]
><span data-ttu-id="32873-118">Ve výchozím nastavení může každý účet Media Services mít jeden aktivní kódování úkol najednou.</span><span class="sxs-lookup"><span data-stu-id="32873-118">By default each Media Services account can have one active encoding task at a time.</span></span> <span data-ttu-id="32873-119">Je možné rezervovat kódování jednotky, které vám umožňují toohave více kódování úloh spuštěných současně, jednu pro jednotlivé kódování vyhrazené jednotky, které jste si koupili.</span><span class="sxs-lookup"><span data-stu-id="32873-119">You can reserve encoding units that allow you toohave multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase.</span></span> <span data-ttu-id="32873-120">Informace najdete v tématu [škálování kódování jednotky](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32873-120">For information, see [Scaling encoding units](media-services-scale-media-processing-overview.md).</span></span>

## <a name="media-encoder-standard"></a><span data-ttu-id="32873-121">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="32873-121">Media Encoder Standard</span></span>
### <a name="how-toouse"></a><span data-ttu-id="32873-122">Jak toouse</span><span class="sxs-lookup"><span data-stu-id="32873-122">How toouse</span></span>
[<span data-ttu-id="32873-123">Jak tooencode pomocí procesoru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="32873-123">How tooencode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a><span data-ttu-id="32873-124">Formáty</span><span class="sxs-lookup"><span data-stu-id="32873-124">Formats</span></span>
[<span data-ttu-id="32873-125">Formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="32873-125">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a><span data-ttu-id="32873-126">Přednastavení</span><span class="sxs-lookup"><span data-stu-id="32873-126">Presets</span></span>
<span data-ttu-id="32873-127">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="32873-127">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="32873-128">Vstup a výstup metadat</span><span class="sxs-lookup"><span data-stu-id="32873-128">Input and output metadata</span></span>
<span data-ttu-id="32873-129">Hello kodéry vstupní metadat je popsán [zde](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="32873-129">hello encoders input metadata is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="32873-130">Hello kodéry výstup metadat je popsán [zde](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="32873-130">hello encoders output metadata is described [here](media-services-output-metadata-schema.md).</span></span>

### <a name="generate-thumbnails"></a><span data-ttu-id="32873-131">Vytváření miniatur</span><span class="sxs-lookup"><span data-stu-id="32873-131">Generate thumbnails</span></span>
<span data-ttu-id="32873-132">Informace najdete v tématu [jak toogenerate miniatur pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span><span class="sxs-lookup"><span data-stu-id="32873-132">For information, see [How toogenerate thumbnails using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span></span>

### <a name="trim-videos-clipping"></a><span data-ttu-id="32873-133">Trim videa (výstřižek)</span><span class="sxs-lookup"><span data-stu-id="32873-133">Trim videos (clipping)</span></span>
<span data-ttu-id="32873-134">Informace najdete v tématu [jak tootrim videa pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span><span class="sxs-lookup"><span data-stu-id="32873-134">For information, see [How tootrim videos using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span></span>

### <a name="create-overlays"></a><span data-ttu-id="32873-135">Vytvoření překryvy</span><span class="sxs-lookup"><span data-stu-id="32873-135">Create overlays</span></span>
<span data-ttu-id="32873-136">Informace najdete v tématu [jak toocreate překryvy pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="32873-136">For information, see [How toocreate overlays using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

### <a name="see-also"></a><span data-ttu-id="32873-137">Viz také</span><span class="sxs-lookup"><span data-stu-id="32873-137">See also</span></span>
[<span data-ttu-id="32873-138">blog Hello Media Services</span><span class="sxs-lookup"><span data-stu-id="32873-138">hello Media Services blog</span></span>](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a><span data-ttu-id="32873-139">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="32873-139">Media Encoder Premium Workflow</span></span>
### <a name="overview"></a><span data-ttu-id="32873-140">Přehled</span><span class="sxs-lookup"><span data-stu-id="32873-140">Overview</span></span>
[<span data-ttu-id="32873-141">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="32873-141">Introducing Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a><span data-ttu-id="32873-142">Jak toouse</span><span class="sxs-lookup"><span data-stu-id="32873-142">How toouse</span></span>
<span data-ttu-id="32873-143">Media Encoder Premium pracovní postup je nakonfigurovat pomocí komplexní pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="32873-143">Media Encoder Premium Workflow is configured using complex workflows.</span></span> <span data-ttu-id="32873-144">Soubory pracovního postupu by bylo možné vytvořit a aktualizovat pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.</span><span class="sxs-lookup"><span data-stu-id="32873-144">Workflow files could be created and updated using hello [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

[<span data-ttu-id="32873-145">Jak tooUse Premium kódování ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="32873-145">How tooUse Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a><span data-ttu-id="32873-146">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="32873-146">Known issues</span></span>
<span data-ttu-id="32873-147">Pokud vaše vstupní video neobsahuje titulků, hello výstupní že Asset bude stále obsahovat prázdný soubor TTML.</span><span class="sxs-lookup"><span data-stu-id="32873-147">If your input video does not contain closed captioning, hello output Asset will still contain an empty TTML file.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="32873-148">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="32873-148">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="32873-149">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="32873-149">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="32873-150">Související články</span><span class="sxs-lookup"><span data-stu-id="32873-150">Related articles</span></span>
* [<span data-ttu-id="32873-151">Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="32873-151">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="32873-152">Kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="32873-152">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
