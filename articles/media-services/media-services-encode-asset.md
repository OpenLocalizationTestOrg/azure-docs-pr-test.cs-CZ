---
title: "Přehled a porovnání Azure na vyžádání média kodéry | Microsoft Docs"
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
ms.openlocfilehash: 538a6ab60168735c2626a93cdeedd8d4999a6efc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a><span data-ttu-id="d1669-103">Přehled a porovnání Azure na vyžádání média kodéry</span><span class="sxs-lookup"><span data-stu-id="d1669-103">Overview and comparison of Azure on demand media encoders</span></span>
## <a name="encoding-overview"></a><span data-ttu-id="d1669-104">Kódování – přehled</span><span class="sxs-lookup"><span data-stu-id="d1669-104">Encoding overview</span></span>
<span data-ttu-id="d1669-105">Azure Media Services poskytuje několik možností kódování média v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d1669-105">Azure Media Services provides multiple options for the encoding of media in the cloud.</span></span>

<span data-ttu-id="d1669-106">Při spouštění pomocí služby Media Services, je důležité si uvědomit rozdíl mezi formáty kodeků a souboru.</span><span class="sxs-lookup"><span data-stu-id="d1669-106">When starting out with Media Services, it is important to understand the difference between codecs and file formats.</span></span>
<span data-ttu-id="d1669-107">Kodeky jsou software, který implementuje kompresi nebo dekompresi algoritmy, zatímco formáty souborů jsou kontejnery, které obsahují komprimované video.</span><span class="sxs-lookup"><span data-stu-id="d1669-107">Codecs are the software that implements the compression/decompression algorithms whereas file formats are containers that hold the compressed video.</span></span>

<span data-ttu-id="d1669-108">Služba Media Services poskytuje dynamické balení, což vám umožní dodávat váš obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming), aniž byste je museli znovu zabalit do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="d1669-108">Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) without you having to re-package into these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="d1669-109">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="d1669-109">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="d1669-110">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="d1669-110">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> <span data-ttu-id="d1669-111">Chcete využít výhod [dynamické balení](media-services-dynamic-packaging-overview.md), musíte udělat následující:</span><span class="sxs-lookup"><span data-stu-id="d1669-111">To take advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need to do the following:</span></span>
>
><span data-ttu-id="d1669-112">Navíc zakódujte váš zdrojový soubor do sady souborů MP4 s adaptivní přenosovou rychlostí nebo technologie Smooth Streaming soubory s adaptivní přenosovou rychlostí (postup kódování je ukázán později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="d1669-112">Also, encode your source file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).</span></span>

<span data-ttu-id="d1669-113">Služba Media Services podporuje následující na vyžádání kodéry, které jsou popsané v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="d1669-113">Media Services supports the following on demand encoders that are described in this article:</span></span>

* [<span data-ttu-id="d1669-114">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1669-114">Media Encoder Standard</span></span>](media-services-encode-asset.md#media-encoder-standard)
* [<span data-ttu-id="d1669-115">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="d1669-115">Media Encoder Premium Workflow</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

<span data-ttu-id="d1669-116">Tento článek poskytuje stručný přehled na vyžádání kodéry média a poskytuje odkazy na články, které poskytují podrobnější informace.</span><span class="sxs-lookup"><span data-stu-id="d1669-116">This article gives a brief overview of on demand media encoders and provides links to articles that give more detailed information.</span></span> <span data-ttu-id="d1669-117">V tématu taky najdete porovnání kodérů.</span><span class="sxs-lookup"><span data-stu-id="d1669-117">The topic also provides comparison of the encoders.</span></span>

>[!NOTE]
><span data-ttu-id="d1669-118">Ve výchozím nastavení může každý účet Media Services mít jeden aktivní kódování úkol najednou.</span><span class="sxs-lookup"><span data-stu-id="d1669-118">By default each Media Services account can have one active encoding task at a time.</span></span> <span data-ttu-id="d1669-119">Je možné rezervovat kódování jednotky, které vám umožní mít více kódování úloh spuštěných současně, jednu pro jednotlivé kódování vyhrazené jednotky, které jste si koupili.</span><span class="sxs-lookup"><span data-stu-id="d1669-119">You can reserve encoding units that allow you to have multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase.</span></span> <span data-ttu-id="d1669-120">Informace najdete v tématu [škálování kódování jednotky](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d1669-120">For information, see [Scaling encoding units](media-services-scale-media-processing-overview.md).</span></span>

## <a name="media-encoder-standard"></a><span data-ttu-id="d1669-121">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1669-121">Media Encoder Standard</span></span>
### <a name="how-to-use"></a><span data-ttu-id="d1669-122">Způsob použití</span><span class="sxs-lookup"><span data-stu-id="d1669-122">How to use</span></span>
[<span data-ttu-id="d1669-123">Postup Kódovat pomocí kodéru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d1669-123">How to encode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a><span data-ttu-id="d1669-124">Formáty</span><span class="sxs-lookup"><span data-stu-id="d1669-124">Formats</span></span>
[<span data-ttu-id="d1669-125">Formáty a kodeky</span><span class="sxs-lookup"><span data-stu-id="d1669-125">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a><span data-ttu-id="d1669-126">Přednastavení</span><span class="sxs-lookup"><span data-stu-id="d1669-126">Presets</span></span>
<span data-ttu-id="d1669-127">Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="d1669-127">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="d1669-128">Vstup a výstup metadat</span><span class="sxs-lookup"><span data-stu-id="d1669-128">Input and output metadata</span></span>
<span data-ttu-id="d1669-129">Vstupní metadata kodéry je popsán [zde](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="d1669-129">The encoders input metadata is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="d1669-130">Výstup metadat kodéry je popsán [zde](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="d1669-130">The encoders output metadata is described [here](media-services-output-metadata-schema.md).</span></span>

### <a name="generate-thumbnails"></a><span data-ttu-id="d1669-131">Vytváření miniatur</span><span class="sxs-lookup"><span data-stu-id="d1669-131">Generate thumbnails</span></span>
<span data-ttu-id="d1669-132">Informace najdete v tématu [jak vygenerovat miniatur pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span><span class="sxs-lookup"><span data-stu-id="d1669-132">For information, see [How to generate thumbnails using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span></span>

### <a name="trim-videos-clipping"></a><span data-ttu-id="d1669-133">Trim videa (výstřižek)</span><span class="sxs-lookup"><span data-stu-id="d1669-133">Trim videos (clipping)</span></span>
<span data-ttu-id="d1669-134">Informace najdete v tématu [postup trim videa pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span><span class="sxs-lookup"><span data-stu-id="d1669-134">For information, see [How to trim videos using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span></span>

### <a name="create-overlays"></a><span data-ttu-id="d1669-135">Vytvoření překryvy</span><span class="sxs-lookup"><span data-stu-id="d1669-135">Create overlays</span></span>
<span data-ttu-id="d1669-136">Informace najdete v tématu [vytvoření překryvy pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="d1669-136">For information, see [How to create overlays using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

### <a name="see-also"></a><span data-ttu-id="d1669-137">Viz také</span><span class="sxs-lookup"><span data-stu-id="d1669-137">See also</span></span>
[<span data-ttu-id="d1669-138">Blog služby Media Services</span><span class="sxs-lookup"><span data-stu-id="d1669-138">The Media Services blog</span></span>](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a><span data-ttu-id="d1669-139">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="d1669-139">Media Encoder Premium Workflow</span></span>
### <a name="overview"></a><span data-ttu-id="d1669-140">Přehled</span><span class="sxs-lookup"><span data-stu-id="d1669-140">Overview</span></span>
[<span data-ttu-id="d1669-141">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="d1669-141">Introducing Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a><span data-ttu-id="d1669-142">Způsob použití</span><span class="sxs-lookup"><span data-stu-id="d1669-142">How to use</span></span>
<span data-ttu-id="d1669-143">Media Encoder Premium pracovní postup je nakonfigurovat pomocí komplexní pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="d1669-143">Media Encoder Premium Workflow is configured using complex workflows.</span></span> <span data-ttu-id="d1669-144">Soubory pracovního postupu by bylo možné vytvořit a aktualizovat pomocí [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.</span><span class="sxs-lookup"><span data-stu-id="d1669-144">Workflow files could be created and updated using the [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

[<span data-ttu-id="d1669-145">Jak používat Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="d1669-145">How to Use Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a><span data-ttu-id="d1669-146">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="d1669-146">Known issues</span></span>
<span data-ttu-id="d1669-147">Pokud vaše vstupní video neobsahuje uzavřen, přidávání titulků, výstup Asset bude stále obsahovat prázdný soubor TTML.</span><span class="sxs-lookup"><span data-stu-id="d1669-147">If your input video does not contain closed captioning, the output Asset will still contain an empty TTML file.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="d1669-148">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="d1669-148">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d1669-149">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d1669-149">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="d1669-150">Související články</span><span class="sxs-lookup"><span data-stu-id="d1669-150">Related articles</span></span>
* [<span data-ttu-id="d1669-151">Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="d1669-151">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="d1669-152">Kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="d1669-152">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
