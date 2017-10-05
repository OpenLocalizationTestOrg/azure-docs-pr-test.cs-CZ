---
title: "Scénáře využití služby Microsoft Azure Media Services a dostupnost funkcí v datových centrech | Dokumentace Microsoftu"
description: "V tomto tématu najdete přehled scénářů využití služby Microsoft Azure Media Services a dostupnost funkcí a služeb v datových centrech."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: juliako;anilmur
ms.openlocfilehash: d9994dd7bfb6b6bf949a7708c07651d667929ae4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a><span data-ttu-id="f18ec-103">Scénáře a dostupnost funkcí služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="f18ec-103">Scenarios and availability of Media Services features across datacenters</span></span>

<span data-ttu-id="f18ec-104">Microsoft Azure Media Services (AMS) umožňuje bezpečně nahrávat, ukládat, kódovat a balit obsah (video nebo zvuk) doručovaný na vyžádání i živě streamovaný různým klientům (například do televizí, počítačů a mobilních zařízení).</span><span class="sxs-lookup"><span data-stu-id="f18ec-104">Microsoft Azure Media Services (AMS) enables you to securely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery to various clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="f18ec-105">AMS funguje v několika datových centrech po celém světě.</span><span class="sxs-lookup"><span data-stu-id="f18ec-105">AMS operates in multiple data centers around the world.</span></span> <span data-ttu-id="f18ec-106">Tato datová centra jsou seskupená do geografických oblastí. To vám poskytuje flexibilitu při výběru místa pro sestavení vašich aplikací.</span><span class="sxs-lookup"><span data-stu-id="f18ec-106">These data centers are grouped in to geographic regions, giving you flexibility in choosing where to build your applications.</span></span> <span data-ttu-id="f18ec-107">[Seznam oblastí a jejich umístění](https://azure.microsoft.com/regions/) si můžete prohlédnout.</span><span class="sxs-lookup"><span data-stu-id="f18ec-107">You can review the [list of regions and their locations](https://azure.microsoft.com/regions/).</span></span> 

<span data-ttu-id="f18ec-108">Toto téma představuje běžné scénáře doručování obsahu [živě](#live_scenarios) nebo [na vyžádání](#vod_scenarios).</span><span class="sxs-lookup"><span data-stu-id="f18ec-108">This topic shows common scenarios for delivering your content [live](#live_scenarios) or [on-demand](#vod_scenarios).</span></span> <span data-ttu-id="f18ec-109">V tématu najdete také podrobnosti o dostupnosti funkcí a služeb pro média v datových centrech.</span><span class="sxs-lookup"><span data-stu-id="f18ec-109">The topic also provides details about availability of media features and services across data centers.</span></span>

## <a name="overview"></a><span data-ttu-id="f18ec-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="f18ec-110">Overview</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f18ec-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f18ec-111">Prerequisites</span></span>

<span data-ttu-id="f18ec-112">Pokud chcete začít používat Azure Media Services, potřebujete následující:</span><span class="sxs-lookup"><span data-stu-id="f18ec-112">To start using Azure Media Services, you should have the following:</span></span>

* <span data-ttu-id="f18ec-113">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f18ec-113">An Azure account.</span></span> <span data-ttu-id="f18ec-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="f18ec-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f18ec-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f18ec-115">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="f18ec-116">Účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="f18ec-116">An Azure Media Services account.</span></span> <span data-ttu-id="f18ec-117">Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-117">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="f18ec-118">Koncový bod streamování, ze kterého chcete streamovat obsah, musí být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-118">The streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span>

    <span data-ttu-id="f18ec-119">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-119">When your AMS account is created, a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="f18ec-120">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-120">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint has to be in the **Running** state.</span></span>

### <a name="commonly-used-objects-when-developing-against-the-ams-odata-model"></a><span data-ttu-id="f18ec-121">Běžně používané objekty při vývoji na základě modelu AMS OData</span><span class="sxs-lookup"><span data-stu-id="f18ec-121">Commonly used objects when developing against the AMS OData model</span></span>

<span data-ttu-id="f18ec-122">Následující obrázek ukazuje některé z nejčastěji používaných objektů při vývoji na základě modelu Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="f18ec-122">The following image shows some of the most commonly used objects when developing against the Media Services OData model.</span></span>

<span data-ttu-id="f18ec-123">Kliknutím na obrázek zobrazíte jeho plnou velikost.</span><span class="sxs-lookup"><span data-stu-id="f18ec-123">Click the image to view it full size.</span></span>  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="f18ec-124">Celý model můžete zobrazit [zde](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="f18ec-124">You can view the whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a><span data-ttu-id="f18ec-125">Ochrana obsahu v úložišti a doručování streamovaných médií v nešifrované podobě</span><span class="sxs-lookup"><span data-stu-id="f18ec-125">Protect content in storage and deliver streaming media in the clear (non-encrypted)</span></span>

![Pracovní postup videa na vyžádání (VoD)](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. <span data-ttu-id="f18ec-127">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="f18ec-127">Upload a high-quality media file into an asset.</span></span>

    <span data-ttu-id="f18ec-128">Doporučujeme použít na asset možnost šifrování úložiště. Ochráníte tak svůj obsah během nahrávání a během jeho pobytu v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f18ec-128">It is recommended to apply storage encryption option to your asset in order to protect your content during upload and while at rest in storage.</span></span>
2. <span data-ttu-id="f18ec-129">Zakódujte ho do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="f18ec-129">Encode to a set of adaptive bitrate MP4 files.</span></span>

    <span data-ttu-id="f18ec-130">Na výstupní asset doporučujeme použít možnost šifrování úložiště. Ochráníte tak váš obsah v úložišti.</span><span class="sxs-lookup"><span data-stu-id="f18ec-130">It is recommended to apply storage encryption option to the output asset in order to protect your content at rest.</span></span>
3. <span data-ttu-id="f18ec-131">Nakonfigurujte zásady doručení assetu (používané dynamickým balením).</span><span class="sxs-lookup"><span data-stu-id="f18ec-131">Configure asset delivery policy (used by dynamic packaging).</span></span>

    <span data-ttu-id="f18ec-132">Pokud váš asset používá šifrování úložiště, **musíte** nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="f18ec-132">If your asset is storage encrypted, you **must** configure asset delivery policy.</span></span>
4. <span data-ttu-id="f18ec-133">Publikujte asset tím, že vytvoříte lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="f18ec-133">Publish the asset by creating an OnDemand locator.</span></span>
5. <span data-ttu-id="f18ec-134">Streamujte publikovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="f18ec-134">Stream published content.</span></span>

<span data-ttu-id="f18ec-135">Informace o dostupnosti v datových centrech najdete v části [Dostupnost](#availability).</span><span class="sxs-lookup"><span data-stu-id="f18ec-135">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a><span data-ttu-id="f18ec-136">Ochrana obsahu v úložišti, doručování dynamicky šifrovaných streamovaných médií</span><span class="sxs-lookup"><span data-stu-id="f18ec-136">Protect content in storage, deliver dynamically encrypted streaming media</span></span>

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. <span data-ttu-id="f18ec-138">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="f18ec-138">Upload a high-quality media file into an asset.</span></span> <span data-ttu-id="f18ec-139">Na asset použijte možnost šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="f18ec-139">Apply storage encryption option to the asset.</span></span>
2. <span data-ttu-id="f18ec-140">Zakódujte ho do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="f18ec-140">Encode to a set of adaptive bitrate MP4 files.</span></span> <span data-ttu-id="f18ec-141">Na výstupní asset použijte možnost šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="f18ec-141">Apply storage encryption option to the output asset.</span></span>
3. <span data-ttu-id="f18ec-142">Pokud chcete asset během přehrávání dynamicky šifrovat, vytvořte šifrovací klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="f18ec-142">Create encryption content key for the asset you want to be dynamically encrypted during playback.</span></span>
4. <span data-ttu-id="f18ec-143">Nakonfigurujte zásady autorizace klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="f18ec-143">Configure content key authorization policy.</span></span>
5. <span data-ttu-id="f18ec-144">Nakonfigurujte zásady doručení assetu (používané dynamickým balením a dynamickým šifrováním).</span><span class="sxs-lookup"><span data-stu-id="f18ec-144">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
6. <span data-ttu-id="f18ec-145">Publikujte asset tím, že vytvoříte lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="f18ec-145">Publish the asset by creating an OnDemand locator.</span></span>
7. <span data-ttu-id="f18ec-146">Streamujte publikovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="f18ec-146">Stream published content.</span></span>

<span data-ttu-id="f18ec-147">Informace o dostupnosti v datových centrech najdete v části [Dostupnost](#availability).</span><span class="sxs-lookup"><span data-stu-id="f18ec-147">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a><span data-ttu-id="f18ec-148">Použití Media Analytics k získání prakticky uplatnitelných informací z videí</span><span class="sxs-lookup"><span data-stu-id="f18ec-148">Use Media Analytics to derive actionable insights from your videos</span></span>

<span data-ttu-id="f18ec-149">Media Analytics je kolekce řečových a vizuálních komponent, které organizacím a podnikům umožňují, aby ze svých videosouborů odvodily prakticky využitelné informace.</span><span class="sxs-lookup"><span data-stu-id="f18ec-149">Media Analytics is a collection of speech and vision components that make it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="f18ec-150">Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-150">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

1. <span data-ttu-id="f18ec-151">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="f18ec-151">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="f18ec-152">Zpracovávejte videa pomocí některé ze služeb Analýzy mediálních služeb popsaných v části [Přehled Analýz mediálních služeb](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-152">Process your videos with one of the Media Analytics services described in the [Media Analytics overview](media-services-analytics-overview.md) section.</span></span>
3. <span data-ttu-id="f18ec-153">Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="f18ec-153">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="f18ec-154">Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat.</span><span class="sxs-lookup"><span data-stu-id="f18ec-154">If a media processor produced an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="f18ec-155">Pokud procesor médií vytvořil soubor JSON, můžete ho stáhnout z úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="f18ec-155">If a media processor produced a JSON file, you can download the file from the Azure blob storage.</span></span>

<span data-ttu-id="f18ec-156">Informace o dostupnosti v datových centrech najdete v části [Dostupnost](#availability).</span><span class="sxs-lookup"><span data-stu-id="f18ec-156">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="deliver-progressive-download"></a><span data-ttu-id="f18ec-157">Doručení progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="f18ec-157">Deliver progressive download</span></span>

1. <span data-ttu-id="f18ec-158">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="f18ec-158">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="f18ec-159">Zakódujte ho do jednoho souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="f18ec-159">Encode to a single MP4 file.</span></span>
3. <span data-ttu-id="f18ec-160">Publikujte asset tím, že vytvoříte lokátor OnDemand nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="f18ec-160">Publish the asset by creating an OnDemand or SAS locator.</span></span>

    <span data-ttu-id="f18ec-161">Pokud používáte lokátor SAS, bude se obsah stahovat z úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="f18ec-161">If using SAS locator, the content is downloaded from the Azure blob storage.</span></span> <span data-ttu-id="f18ec-162">V takovém případě nepotřebujete koncové body streamování ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="f18ec-162">In this case, you do not need to have streaming endpoints in started state.</span></span>
4. <span data-ttu-id="f18ec-163">Progresivně stáhněte obsah.</span><span class="sxs-lookup"><span data-stu-id="f18ec-163">Progressively download content.</span></span>

## <span data-ttu-id="f18ec-164"><a id="live_scenarios"></a>Doručování živě streamovaných událostí</span><span class="sxs-lookup"><span data-stu-id="f18ec-164"><a id="live_scenarios"></a>Delivering live-streaming events</span></span> 

1. <span data-ttu-id="f18ec-165">Ingestujte živý obsah pomocí různých protokolů pro živé streamování (například RTMP nebo technologie Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="f18ec-165">Ingest live content using various live streaming protocols (for example RTMP or Smooth Streaming).</span></span>
2. <span data-ttu-id="f18ec-166">(volitelně) Kódujte datový proud na datový proud s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="f18ec-166">(optionally) Encode your stream into adaptive bitrate stream.</span></span>
3. <span data-ttu-id="f18ec-167">Zobrazte náhled živého streamování.</span><span class="sxs-lookup"><span data-stu-id="f18ec-167">Preview your live stream.</span></span>
4. <span data-ttu-id="f18ec-168">Doručujte obsah prostřednictvím běžných streamovacích protokolů (například MPEG DASH, Smooth, HLS) přímo k vašim zákazníkům nebo do sítě pro doručování obsahu (CDN) pro další distribuci.</span><span class="sxs-lookup"><span data-stu-id="f18ec-168">Deliver the content through common streaming protocols (for example, MPEG DASH, Smooth, HLS) directly to your customers, or to a Content Delivery Network (CDN) for further distribution.</span></span>

    <span data-ttu-id="f18ec-169">-nebo-</span><span class="sxs-lookup"><span data-stu-id="f18ec-169">-or-</span></span>

    <span data-ttu-id="f18ec-170">Zaznamenávejte a ukládejte ingestovaný obsah, abyste ho mohli streamovat později (video na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="f18ec-170">Record and store the ingested content in order to be streamed later (Video-on-Demand).</span></span>

<span data-ttu-id="f18ec-171">Při živém streamování můžete zvolit jednu z následujících cest:</span><span class="sxs-lookup"><span data-stu-id="f18ec-171">When doing live streaming, you can choose one of the following routes:</span></span>

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a><span data-ttu-id="f18ec-172">Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů (průchozí)</span><span class="sxs-lookup"><span data-stu-id="f18ec-172">Working with channels that receive multi-bitrate live stream from on-premises encoders (pass-through)</span></span>

<span data-ttu-id="f18ec-173">Následující diagram znázorňuje hlavní části platformy AMS, které se podílejí na **průchozím** pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="f18ec-173">The following diagram shows the major parts of the AMS platform that are involved in the **pass-through** workflow.</span></span>

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-current.png)

<span data-ttu-id="f18ec-175">Další informace najdete v článku o [práci s kanály, které přijímají živé streamování s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-175">For more information, see [Working with Channels that Receive Multi-bitrate Live Stream from On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).</span></span>

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a><span data-ttu-id="f18ec-176">Práce s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="f18ec-176">Working with channels that are enabled to perform live encoding with Azure Media Services</span></span>

<span data-ttu-id="f18ec-177">Následující diagram znázorňuje hlavní část platformy AMS, které se podílejí na pracovním postupu živého streamování, ve kterém má kanál povolené kódování v reálném čase pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="f18ec-177">The following diagram shows the major parts of the AMS platform that are involved in Live Streaming workflow where a Channel is enabled to perform live encoding with Media Services.</span></span>

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-new.png)

<span data-ttu-id="f18ec-179">Další informace najdete v článku o [práci s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-179">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="f18ec-180">Informace o dostupnosti v datových centrech najdete v části [Dostupnost](#availability).</span><span class="sxs-lookup"><span data-stu-id="f18ec-180">For information about availability in datacenters, see the [Availability](#availability) section.</span></span>

## <a name="consuming-content"></a><span data-ttu-id="f18ec-181">Konzumace obsahu</span><span class="sxs-lookup"><span data-stu-id="f18ec-181">Consuming content</span></span>

<span data-ttu-id="f18ec-182">Služba Azure Media Services nabízí nástroje, které potřebujete k vytvoření dynamických aplikací pro klientské přehrávače pro většinu platforem včetně: zařízení iOS, zařízení Android, Windows, Windows Phone, Xbox a set top boxy.</span><span class="sxs-lookup"><span data-stu-id="f18ec-182">Azure Media Services provides the tools you need to create rich, dynamic client player applications for most platforms including: iOS Devices, Android Devices, Windows, Windows Phone, Xbox, and Set-top boxes.</span></span> <span data-ttu-id="f18ec-183">Následující téma obsahuje odkazy na sady SDK a architektury přehrávačů, které můžete použít pro vývoj vlastních klientských aplikací, které můžou využívat streamovaná média ze služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="f18ec-183">The following topic provides links to SDKs and Player Frameworks that you can use to develop your own client applications that can consume streaming media from Media Services.</span></span> <span data-ttu-id="f18ec-184">Další informace najdete v tématu [Vývoj aplikací pro přehrávání videa](media-services-develop-video-players.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-184">For more information, see [Developing video payer applications](media-services-develop-video-players.md)</span></span>

## <a name="enabling-azure-cdn"></a><span data-ttu-id="f18ec-185">Povolení Azure CDN</span><span class="sxs-lookup"><span data-stu-id="f18ec-185">Enabling Azure CDN</span></span>

<span data-ttu-id="f18ec-186">Služba Media Services podporuje integraci s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="f18ec-186">Media Services supports integration with Azure CDN.</span></span> <span data-ttu-id="f18ec-187">Informace o povolení Azure CDN najdete v článku o [správě koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-187">For information on how to enable Azure CDN, see [How to Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>

## <span data-ttu-id="f18ec-188"><a id="scaling"></a>Škálování účtu Media Services</span><span class="sxs-lookup"><span data-stu-id="f18ec-188"><a id="scaling"></a>Scaling a Media Services account</span></span>

<span data-ttu-id="f18ec-189">Zákazníci AMS můžou ve svých účtech AMS škálovat koncové body streamování, zpracování médií a úložiště.</span><span class="sxs-lookup"><span data-stu-id="f18ec-189">AMS customers can scale streaming endpoints, media processing, and storage in their AMS accounts.</span></span>

* <span data-ttu-id="f18ec-190">Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-190">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="f18ec-191">Koncový bod streamování **Standard** je vhodný pro většinu úloh streamování.</span><span class="sxs-lookup"><span data-stu-id="f18ec-191">A **Standard** streaming endpoint is suitable for most streaming workloads.</span></span> <span data-ttu-id="f18ec-192">Zahrnuje stejné funkce jako koncové body streamování **Premium** a automaticky škáluje šířku odchozího pásma.</span><span class="sxs-lookup"><span data-stu-id="f18ec-192">It includes the same features as a **Premium** streaming endpoints and scales outbound bandwidth automatically.</span></span> 

    <span data-ttu-id="f18ec-193">Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="f18ec-193">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="f18ec-194">Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU).</span><span class="sxs-lookup"><span data-stu-id="f18ec-194">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="f18ec-195">Koncový bod streamování je možné škálovat přidáním jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="f18ec-195">The streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="f18ec-196">Každá jednotka streamování poskytuje aplikaci další kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="f18ec-196">Each SU provides additional bandwidth capacity to the application.</span></span> <span data-ttu-id="f18ec-197">Další informace o škálování koncových bodů streamování **Premium** najdete v tématu [Škálování koncových bodů streamování](media-services-portal-scale-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-197">For more information about scaling **Premium** streaming endpoints, see the [Scaling streaming endpoints](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

* <span data-ttu-id="f18ec-198">Účet Media Services je přidružený k typu rezervované jednotky, který určuje rychlost zpracování vašich úloh zpracování médií.</span><span class="sxs-lookup"><span data-stu-id="f18ec-198">A Media Services account is associated with a Reserved Unit Type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="f18ec-199">Můžete si vybrat mezi následujícími typy rezervovaných jednotek: **S1**, **S2** nebo **S3**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-199">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="f18ec-200">Například stejná úloha kódování bude rychlejší, když použijete typ rezervované jednotky **S2**, než kdybyste použili typ **S1**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-200">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span>

    <span data-ttu-id="f18ec-201">Kromě určení typu rezervované jednotky můžete určit, že chcete účet zřídit s **rezervovanými jednotkami** (RU).</span><span class="sxs-lookup"><span data-stu-id="f18ec-201">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="f18ec-202">Počet zřízených RU určuje počet úloh médií, které je možné v daném účtu zpracovávat současně.</span><span class="sxs-lookup"><span data-stu-id="f18ec-202">The number of provisioned RUs determines the number of media tasks that can be processed concurrently in a given account.</span></span>

    >[!NOTE]
    ><span data-ttu-id="f18ec-203">RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru.</span><span class="sxs-lookup"><span data-stu-id="f18ec-203">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="f18ec-204">Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.</span><span class="sxs-lookup"><span data-stu-id="f18ec-204">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

    <span data-ttu-id="f18ec-205">Další informace najdete v tématu [Škálování zpracování médií](media-services-portal-scale-media-processing.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-205">For more information see, [Scale media processing](media-services-portal-scale-media-processing.md).</span></span>
* <span data-ttu-id="f18ec-206">Svůj účet Media Services můžete škálovat také tím, že k němu přidáte účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="f18ec-206">You can also scale your Media Services account by adding storage accounts to it.</span></span> <span data-ttu-id="f18ec-207">Každý účet úložiště je omezen na 500 TB.</span><span class="sxs-lookup"><span data-stu-id="f18ec-207">Each storage account is limited to 500 TB.</span></span> <span data-ttu-id="f18ec-208">Pokud chcete úložiště rozšířit nad jeho výchozí omezení, můžete k jednomu účtu Media Services připojit více účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="f18ec-208">To expand your storage beyond the default limitations, you can choose to attach multiple storage accounts to a single Media Services account.</span></span> <span data-ttu-id="f18ec-209">Další informace najdete v tématu [Správa účtů úložiště](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-209">For more information, see [Manage storage accounts](meda-services-managing-multiple-storage-accounts.md).</span></span>

##<span data-ttu-id="f18ec-210"><a id="availability"></a>Dostupnost funkcí služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="f18ec-210"><a id="availability"></a> Availability of Media Services features across datacenters</span></span>

<span data-ttu-id="f18ec-211">V této části najdete podrobnosti o dostupnosti funkcí služby Media Services v datových centrech.</span><span class="sxs-lookup"><span data-stu-id="f18ec-211">This section provides details about availability of Media Services features across datacenters.</span></span>

### <a name="ams-accounts"></a><span data-ttu-id="f18ec-212">Účty AMS</span><span class="sxs-lookup"><span data-stu-id="f18ec-212">AMS accounts</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-213">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-213">Availability</span></span>

<span data-ttu-id="f18ec-214">Účty Media Service můžete vytvářet v následujících oblastech: Severní Evropa, Západní Evropa, USA – západ, USA – východ, Jihovýchodní Asie, Východní Asie, Japonsko – západ, Japonsko – východ, Brazílie – jih, Indie – západ, Indie – jih a Indie – střed.</span><span class="sxs-lookup"><span data-stu-id="f18ec-214">You can create Media Services accounts in the following regions: North Europe, West Europe, West US, East US, Southeast Asia, East Asia, Japan West, Japan East, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="streaming-endpoints"></a><span data-ttu-id="f18ec-215">Koncové body streamování</span><span class="sxs-lookup"><span data-stu-id="f18ec-215">Streaming endpoints</span></span> 

<span data-ttu-id="f18ec-216">Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-216">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="f18ec-217">Další informace najdete v části popisující [škálování](#scaling).</span><span class="sxs-lookup"><span data-stu-id="f18ec-217">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-218">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-218">Availability</span></span>

|<span data-ttu-id="f18ec-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="f18ec-219">Name</span></span>|<span data-ttu-id="f18ec-220">Status</span><span class="sxs-lookup"><span data-stu-id="f18ec-220">Status</span></span>|<span data-ttu-id="f18ec-221">Datová centra</span><span class="sxs-lookup"><span data-stu-id="f18ec-221">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="f18ec-222">Standard</span><span class="sxs-lookup"><span data-stu-id="f18ec-222">Standard</span></span>|<span data-ttu-id="f18ec-223">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-223">GA</span></span>|<span data-ttu-id="f18ec-224">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-224">All</span></span>|
|<span data-ttu-id="f18ec-225">Premium</span><span class="sxs-lookup"><span data-stu-id="f18ec-225">Premium</span></span>|<span data-ttu-id="f18ec-226">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-226">GA</span></span>|<span data-ttu-id="f18ec-227">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-227">All</span></span>|

### <a name="live-encoding"></a><span data-ttu-id="f18ec-228">Kódování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="f18ec-228">Live encoding</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-229">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-229">Availability</span></span>

<span data-ttu-id="f18ec-230">K dispozici ve všech datových centrech kromě oblastí: Německo, Brazílie – jih, Indie – západ, Indie – jih a Indie – střed.</span><span class="sxs-lookup"><span data-stu-id="f18ec-230">Available in all datacenters except: Germany, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="encoding-media-processors"></a><span data-ttu-id="f18ec-231">Kódovací procesory médií</span><span class="sxs-lookup"><span data-stu-id="f18ec-231">Encoding media processors</span></span>

<span data-ttu-id="f18ec-232">AMS nabízí dva kodéry na vyžádání – **Media Encoder Standard** a **Pracovní postup kodéru Media Encoder Premium**.</span><span class="sxs-lookup"><span data-stu-id="f18ec-232">AMS offers two on-demand encoders **Media Encoder Standard** and **Media Encoder Premium Workflow**.</span></span> <span data-ttu-id="f18ec-233">Další informace najdete v tématu [Přehled a porovnání kodérů médií na vyžádání v Azure](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-233">For more information, see [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md).</span></span> 

#### <a name="availability"></a><span data-ttu-id="f18ec-234">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-234">Availability</span></span>

|<span data-ttu-id="f18ec-235">Název procesoru médií</span><span class="sxs-lookup"><span data-stu-id="f18ec-235">Media processor name</span></span>|<span data-ttu-id="f18ec-236">Status</span><span class="sxs-lookup"><span data-stu-id="f18ec-236">Status</span></span>|<span data-ttu-id="f18ec-237">Datová centra</span><span class="sxs-lookup"><span data-stu-id="f18ec-237">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="f18ec-238">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="f18ec-238">Media Encoder Standard</span></span>|<span data-ttu-id="f18ec-239">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-239">GA</span></span>|<span data-ttu-id="f18ec-240">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-240">All</span></span>|
|<span data-ttu-id="f18ec-241">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="f18ec-241">Media Encoder Premium Workflow</span></span>|<span data-ttu-id="f18ec-242">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-242">GA</span></span>|<span data-ttu-id="f18ec-243">Všechny s výjimkou Číny</span><span class="sxs-lookup"><span data-stu-id="f18ec-243">All except China</span></span>|

### <a name="analytics-media-processors"></a><span data-ttu-id="f18ec-244">Analytické procesory médií</span><span class="sxs-lookup"><span data-stu-id="f18ec-244">Analytics media processors</span></span>

<span data-ttu-id="f18ec-245">Media Analytics je kolekce řečových a vizuálních komponent, které organizacím a podnikům umožňují, aby ze svých videosouborů odvodily prakticky využitelné informace.</span><span class="sxs-lookup"><span data-stu-id="f18ec-245">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="f18ec-246">Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-246">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-247">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-247">Availability</span></span>

|<span data-ttu-id="f18ec-248">Název procesoru médií</span><span class="sxs-lookup"><span data-stu-id="f18ec-248">Media processor name</span></span>|<span data-ttu-id="f18ec-249">Status</span><span class="sxs-lookup"><span data-stu-id="f18ec-249">Status</span></span>|<span data-ttu-id="f18ec-250">Datová centra</span><span class="sxs-lookup"><span data-stu-id="f18ec-250">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="f18ec-251">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="f18ec-251">Azure Media Face Detector</span></span>|<span data-ttu-id="f18ec-252">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-252">Preview</span></span>|<span data-ttu-id="f18ec-253">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-253">All</span></span>|
|<span data-ttu-id="f18ec-254">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="f18ec-254">Azure Media Hyperlapse</span></span>|<span data-ttu-id="f18ec-255">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-255">Preview</span></span>|<span data-ttu-id="f18ec-256">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-256">All</span></span>|
|<span data-ttu-id="f18ec-257">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="f18ec-257">Azure Media Indexer</span></span>|<span data-ttu-id="f18ec-258">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-258">GA</span></span>|<span data-ttu-id="f18ec-259">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-259">All</span></span>|
|<span data-ttu-id="f18ec-260">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="f18ec-260">Azure Media Motion Detector</span></span>|<span data-ttu-id="f18ec-261">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-261">Preview</span></span>|<span data-ttu-id="f18ec-262">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-262">All</span></span>|
|<span data-ttu-id="f18ec-263">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="f18ec-263">Azure Media OCR</span></span>|<span data-ttu-id="f18ec-264">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-264">Preview</span></span>|<span data-ttu-id="f18ec-265">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-265">All</span></span>|
|<span data-ttu-id="f18ec-266">Azure Media Redactor</span><span class="sxs-lookup"><span data-stu-id="f18ec-266">Azure Media Redactor</span></span>|<span data-ttu-id="f18ec-267">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-267">Preview</span></span>|<span data-ttu-id="f18ec-268">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-268">All</span></span>|
|<span data-ttu-id="f18ec-269">Azure Media Stabilizer</span><span class="sxs-lookup"><span data-stu-id="f18ec-269">Azure Media Stabilizer</span></span>|<span data-ttu-id="f18ec-270">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-270">Preview</span></span>|<span data-ttu-id="f18ec-271">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-271">All</span></span>|
|<span data-ttu-id="f18ec-272">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="f18ec-272">Azure Media Video Thumbnails</span></span>|<span data-ttu-id="f18ec-273">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-273">Preview</span></span>|<span data-ttu-id="f18ec-274">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-274">All</span></span>|
|<span data-ttu-id="f18ec-275">Azure Media Indexer 2</span><span class="sxs-lookup"><span data-stu-id="f18ec-275">Azure Media Indexer 2</span></span>|<span data-ttu-id="f18ec-276">Preview</span><span class="sxs-lookup"><span data-stu-id="f18ec-276">Preview</span></span>|<span data-ttu-id="f18ec-277">Všechny s výjimkou Číny a oblasti federální vlády</span><span class="sxs-lookup"><span data-stu-id="f18ec-277">All except China and Federal Government region</span></span>|

### <a name="protection"></a><span data-ttu-id="f18ec-278">Ochrana</span><span class="sxs-lookup"><span data-stu-id="f18ec-278">Protection</span></span>

<span data-ttu-id="f18ec-279">Microsoft Azure Media Services umožňuje zabezpečení médií od okamžiku opuštění počítače přes uložení a zpracování až po doručení.</span><span class="sxs-lookup"><span data-stu-id="f18ec-279">Microsoft Azure Media Services enables you to secure your media from the time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="f18ec-280">Další informace najdete v tématu [Ochrana obsahu AMS](media-services-content-protection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f18ec-280">For more information, see [Protecting AMS content](media-services-content-protection-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-281">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-281">Availability</span></span>

|<span data-ttu-id="f18ec-282">Šifrování</span><span class="sxs-lookup"><span data-stu-id="f18ec-282">Encryption</span></span>|<span data-ttu-id="f18ec-283">Status</span><span class="sxs-lookup"><span data-stu-id="f18ec-283">Status</span></span>|<span data-ttu-id="f18ec-284">Datová centra</span><span class="sxs-lookup"><span data-stu-id="f18ec-284">Datacenters</span></span>|
|---|---|---| 
|<span data-ttu-id="f18ec-285">Úložiště</span><span class="sxs-lookup"><span data-stu-id="f18ec-285">Storage</span></span>|<span data-ttu-id="f18ec-286">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-286">GA</span></span>|<span data-ttu-id="f18ec-287">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-287">All</span></span>|
|<span data-ttu-id="f18ec-288">Klíče AES-128</span><span class="sxs-lookup"><span data-stu-id="f18ec-288">AES-128 keys</span></span>|<span data-ttu-id="f18ec-289">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-289">GA</span></span>|<span data-ttu-id="f18ec-290">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-290">All</span></span>|
|<span data-ttu-id="f18ec-291">FairPlay</span><span class="sxs-lookup"><span data-stu-id="f18ec-291">Fairplay</span></span>|<span data-ttu-id="f18ec-292">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-292">GA</span></span>|<span data-ttu-id="f18ec-293">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-293">All</span></span>|
|<span data-ttu-id="f18ec-294">PlayReady</span><span class="sxs-lookup"><span data-stu-id="f18ec-294">PlayReady</span></span>|<span data-ttu-id="f18ec-295">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-295">GA</span></span>|<span data-ttu-id="f18ec-296">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-296">All</span></span>|
|<span data-ttu-id="f18ec-297">Widevine</span><span class="sxs-lookup"><span data-stu-id="f18ec-297">Widevine</span></span>|<span data-ttu-id="f18ec-298">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-298">GA</span></span>|<span data-ttu-id="f18ec-299">Všechna kromě oblastí Německo, Federální vláda a Čína.</span><span class="sxs-lookup"><span data-stu-id="f18ec-299">All except Germany, Federal Government and China.</span></span>

### <a name="reserved-units-rus"></a><span data-ttu-id="f18ec-300">Rezervované jednotky (RU)</span><span class="sxs-lookup"><span data-stu-id="f18ec-300">Reserved units (RUs)</span></span>

<span data-ttu-id="f18ec-301">Počet zřízených rezervovaných jednotek určuje počet úloh médií, které je možné v daném účtu zpracovávat současně.</span><span class="sxs-lookup"><span data-stu-id="f18ec-301">The number of provisioned reserved units determines the number of media tasks that can be processed concurrently in a given account.</span></span> 

<span data-ttu-id="f18ec-302">Další informace najdete v části popisující [škálování](#scaling).</span><span class="sxs-lookup"><span data-stu-id="f18ec-302">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-303">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-303">Availability</span></span>

<span data-ttu-id="f18ec-304">K dispozici ve všech datových centrech.</span><span class="sxs-lookup"><span data-stu-id="f18ec-304">Available in all datacenters.</span></span>

### <a name="reserved-unit-ru-type"></a><span data-ttu-id="f18ec-305">Typ rezervované jednotky (RU)</span><span class="sxs-lookup"><span data-stu-id="f18ec-305">Reserved unit (RU) type</span></span>

<span data-ttu-id="f18ec-306">Účet Media Services je přidružený k typu rezervované jednotky, který určuje rychlost zpracování vašich úloh zpracování médií.</span><span class="sxs-lookup"><span data-stu-id="f18ec-306">A Media Services account is associated with a Reserved unit type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="f18ec-307">Můžete si vybrat mezi následujícími typy rezervovaných jednotek: S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="f18ec-307">You can pick between the following reserved unit types: S1, S2, or S3.</span></span>

<span data-ttu-id="f18ec-308">Další informace najdete v části popisující [škálování](#scaling).</span><span class="sxs-lookup"><span data-stu-id="f18ec-308">For more information, see the [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="f18ec-309">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="f18ec-309">Availability</span></span>

|<span data-ttu-id="f18ec-310">Název typu RU</span><span class="sxs-lookup"><span data-stu-id="f18ec-310">RU type name</span></span>|<span data-ttu-id="f18ec-311">Status</span><span class="sxs-lookup"><span data-stu-id="f18ec-311">Status</span></span>|<span data-ttu-id="f18ec-312">Datová centra</span><span class="sxs-lookup"><span data-stu-id="f18ec-312">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="f18ec-313">S1</span><span class="sxs-lookup"><span data-stu-id="f18ec-313">S1</span></span>|<span data-ttu-id="f18ec-314">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-314">GA</span></span>|<span data-ttu-id="f18ec-315">Všechny</span><span class="sxs-lookup"><span data-stu-id="f18ec-315">All</span></span>|
|<span data-ttu-id="f18ec-316">S2</span><span class="sxs-lookup"><span data-stu-id="f18ec-316">S2</span></span>|<span data-ttu-id="f18ec-317">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-317">GA</span></span>|<span data-ttu-id="f18ec-318">Všechna kromě oblastí Brazílie – jih a Indie – západ</span><span class="sxs-lookup"><span data-stu-id="f18ec-318">All except Brazil South, and India West</span></span>|
|<span data-ttu-id="f18ec-319">S3</span><span class="sxs-lookup"><span data-stu-id="f18ec-319">S3</span></span>|<span data-ttu-id="f18ec-320">GA</span><span class="sxs-lookup"><span data-stu-id="f18ec-320">GA</span></span>|<span data-ttu-id="f18ec-321">Všechna kromě oblasti Indie – západ</span><span class="sxs-lookup"><span data-stu-id="f18ec-321">All except India West</span></span>|

## <a name="next-steps"></a><span data-ttu-id="f18ec-322">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f18ec-322">Next steps</span></span>

<span data-ttu-id="f18ec-323">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="f18ec-323">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f18ec-324">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f18ec-324">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

