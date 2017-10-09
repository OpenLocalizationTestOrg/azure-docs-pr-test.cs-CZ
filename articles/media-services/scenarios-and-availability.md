---
title: "aaaMicrosoft Azure Media Services scénáře a dostupnost funkcí přes datových center | Microsoft Docs"
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
ms.openlocfilehash: 3dbab6998ed5da738baf8f1e2fb096dfba336e19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a><span data-ttu-id="d5512-103">Scénáře a dostupnost funkcí služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="d5512-103">Scenarios and availability of Media Services features across datacenters</span></span>

<span data-ttu-id="d5512-104">Microsoft Azure Media Services (AMS) vám umožní toosecurely nahrávání, ukládání, kódovat a video nebo zvuk obsah balíčku pro vysílání na vyžádání i živé streamování doručení toovarious klienty (například TV, počítače a mobilní zařízení).</span><span class="sxs-lookup"><span data-stu-id="d5512-104">Microsoft Azure Media Services (AMS) enables you toosecurely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery toovarious clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="d5512-105">AMS funguje v několika datových centrech kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="d5512-105">AMS operates in multiple data centers around hello world.</span></span> <span data-ttu-id="d5512-106">Těchto datových centrech jsou seskupené v oblastech toogeographic, která poskytuje flexibilitu při výběru místa toobuild vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5512-106">These data centers are grouped in toogeographic regions, giving you flexibility in choosing where toobuild your applications.</span></span> <span data-ttu-id="d5512-107">Můžete zkontrolovat hello [seznam oblastí a jejich umístění](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="d5512-107">You can review hello [list of regions and their locations](https://azure.microsoft.com/regions/).</span></span> 

<span data-ttu-id="d5512-108">Toto téma představuje běžné scénáře doručování obsahu [živě](#live_scenarios) nebo [na vyžádání](#vod_scenarios).</span><span class="sxs-lookup"><span data-stu-id="d5512-108">This topic shows common scenarios for delivering your content [live](#live_scenarios) or [on-demand](#vod_scenarios).</span></span> <span data-ttu-id="d5512-109">Hello téma obsahuje také podrobnosti o dostupnosti funkce média a služby napříč datovými centry.</span><span class="sxs-lookup"><span data-stu-id="d5512-109">hello topic also provides details about availability of media features and services across data centers.</span></span>

## <a name="overview"></a><span data-ttu-id="d5512-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="d5512-110">Overview</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d5512-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5512-111">Prerequisites</span></span>

<span data-ttu-id="d5512-112">toostart využívající Azure Media Services, měli byste mít hello následující:</span><span class="sxs-lookup"><span data-stu-id="d5512-112">toostart using Azure Media Services, you should have hello following:</span></span>

* <span data-ttu-id="d5512-113">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d5512-113">An Azure account.</span></span> <span data-ttu-id="d5512-114">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d5512-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d5512-115">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d5512-115">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="d5512-116">Účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5512-116">An Azure Media Services account.</span></span> <span data-ttu-id="d5512-117">Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-117">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="d5512-118">Hello koncový bod, ze kterého chcete obsah toostream streamování má toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="d5512-118">hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span>

    <span data-ttu-id="d5512-119">Při vytváření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="d5512-119">When your AMS account is created, a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="d5512-120">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello koncový bod streamování má toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="d5512-120">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint has toobe in hello **Running** state.</span></span>

### <a name="commonly-used-objects-when-developing-against-hello-ams-odata-model"></a><span data-ttu-id="d5512-121">Běžně používané objekty při vývoji proti hello AMS OData modelu</span><span class="sxs-lookup"><span data-stu-id="d5512-121">Commonly used objects when developing against hello AMS OData model</span></span>

<span data-ttu-id="d5512-122">Hello následující obrázek ukazuje některé objekty hello nejčastěji používaná při vývoji vůči modelu hello Media Services OData.</span><span class="sxs-lookup"><span data-stu-id="d5512-122">hello following image shows some of hello most commonly used objects when developing against hello Media Services OData model.</span></span>

<span data-ttu-id="d5512-123">Klikněte na tlačítko tooview hello bitové kopie je plná velikost.</span><span class="sxs-lookup"><span data-stu-id="d5512-123">Click hello image tooview it full size.</span></span>  

<a href="./media/media-services-overview/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-overview/media-services-overview-object-model-small.png"></a> 

<span data-ttu-id="d5512-124">Můžete zobrazit celý modelu hello [zde](https://media.windows.net/API/$metadata?api-version=2.15).</span><span class="sxs-lookup"><span data-stu-id="d5512-124">You can view hello whole model [here](https://media.windows.net/API/$metadata?api-version=2.15).</span></span>  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-hello-clear-non-encrypted"></a><span data-ttu-id="d5512-125">Ochrana obsahu v úložišti a doručování streamovaných médií v hello clear (bez šifrování)</span><span class="sxs-lookup"><span data-stu-id="d5512-125">Protect content in storage and deliver streaming media in hello clear (non-encrypted)</span></span>

![Pracovní postup videa na vyžádání (VoD)](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. <span data-ttu-id="d5512-127">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="d5512-127">Upload a high-quality media file into an asset.</span></span>

    <span data-ttu-id="d5512-128">Doporučujeme majetku, tooyour možnost šifrování úložiště tooapply v pořadí tooprotect svůj obsah během nahrávání a při umístěná v úložišti.</span><span class="sxs-lookup"><span data-stu-id="d5512-128">It is recommended tooapply storage encryption option tooyour asset in order tooprotect your content during upload and while at rest in storage.</span></span>
2. <span data-ttu-id="d5512-129">Zakódujte tooa sadu souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="d5512-129">Encode tooa set of adaptive bitrate MP4 files.</span></span>

    <span data-ttu-id="d5512-130">Doporučuje se, že toohello možnost šifrování úložiště tooapply výstupní asset v pořadí tooprotect váš obsah v úložišti.</span><span class="sxs-lookup"><span data-stu-id="d5512-130">It is recommended tooapply storage encryption option toohello output asset in order tooprotect your content at rest.</span></span>
3. <span data-ttu-id="d5512-131">Nakonfigurujte zásady doručení assetu (používané dynamickým balením).</span><span class="sxs-lookup"><span data-stu-id="d5512-131">Configure asset delivery policy (used by dynamic packaging).</span></span>

    <span data-ttu-id="d5512-132">Pokud váš asset používá šifrování úložiště, **musíte** nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="d5512-132">If your asset is storage encrypted, you **must** configure asset delivery policy.</span></span>
4. <span data-ttu-id="d5512-133">Publikujte hello asset tak, že vytvoříte Lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="d5512-133">Publish hello asset by creating an OnDemand locator.</span></span>
5. <span data-ttu-id="d5512-134">Streamujte publikovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="d5512-134">Stream published content.</span></span>

<span data-ttu-id="d5512-135">Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-135">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a><span data-ttu-id="d5512-136">Ochrana obsahu v úložišti, doručování dynamicky šifrovaných streamovaných médií</span><span class="sxs-lookup"><span data-stu-id="d5512-136">Protect content in storage, deliver dynamically encrypted streaming media</span></span>

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. <span data-ttu-id="d5512-138">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="d5512-138">Upload a high-quality media file into an asset.</span></span> <span data-ttu-id="d5512-139">Použijte asset toohello možnost šifrování úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5512-139">Apply storage encryption option toohello asset.</span></span>
2. <span data-ttu-id="d5512-140">Zakódujte tooa sadu souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="d5512-140">Encode tooa set of adaptive bitrate MP4 files.</span></span> <span data-ttu-id="d5512-141">Použijte úložiště šifrování možnost toohello výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="d5512-141">Apply storage encryption option toohello output asset.</span></span>
3. <span data-ttu-id="d5512-142">Vytvořte šifrovací klíč obsahu pro prostředek hello chcete toobe během přehrávání dynamicky šifrovat.</span><span class="sxs-lookup"><span data-stu-id="d5512-142">Create encryption content key for hello asset you want toobe dynamically encrypted during playback.</span></span>
4. <span data-ttu-id="d5512-143">Nakonfigurujte zásady autorizace klíče obsahu.</span><span class="sxs-lookup"><span data-stu-id="d5512-143">Configure content key authorization policy.</span></span>
5. <span data-ttu-id="d5512-144">Nakonfigurujte zásady doručení assetu (používané dynamickým balením a dynamickým šifrováním).</span><span class="sxs-lookup"><span data-stu-id="d5512-144">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
6. <span data-ttu-id="d5512-145">Publikujte hello asset tak, že vytvoříte Lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="d5512-145">Publish hello asset by creating an OnDemand locator.</span></span>
7. <span data-ttu-id="d5512-146">Streamujte publikovaný obsah.</span><span class="sxs-lookup"><span data-stu-id="d5512-146">Stream published content.</span></span>

<span data-ttu-id="d5512-147">Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-147">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="use-media-analytics-tooderive-actionable-insights-from-your-videos"></a><span data-ttu-id="d5512-148">Použití Media Analytics tooderive prakticky uplatnitelných informací z videí</span><span class="sxs-lookup"><span data-stu-id="d5512-148">Use Media Analytics tooderive actionable insights from your videos</span></span>

<span data-ttu-id="d5512-149">Media Analytics je kolekce řečových a vizuálních komponent, které usnadňují organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů.</span><span class="sxs-lookup"><span data-stu-id="d5512-149">Media Analytics is a collection of speech and vision components that make it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="d5512-150">Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-150">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

1. <span data-ttu-id="d5512-151">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="d5512-151">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="d5512-152">Zpracujte videa s jedním z služeb Media Analytics hello popsaných v hello [Media Analytics přehled](media-services-analytics-overview.md) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-152">Process your videos with one of hello Media Analytics services described in hello [Media Analytics overview](media-services-analytics-overview.md) section.</span></span>
3. <span data-ttu-id="d5512-153">Procesory médií z Media Analytics vytvářejí soubory MP4 nebo soubory JSON.</span><span class="sxs-lookup"><span data-stu-id="d5512-153">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="d5512-154">Pokud procesor médií vytvořil soubor MP4, můžete ho progresivně stahovat hello souboru.</span><span class="sxs-lookup"><span data-stu-id="d5512-154">If a media processor produced an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="d5512-155">Pokud procesor médií vytvořil soubor JSON, můžete stáhnout soubor hello z hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d5512-155">If a media processor produced a JSON file, you can download hello file from hello Azure blob storage.</span></span>

<span data-ttu-id="d5512-156">Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-156">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="deliver-progressive-download"></a><span data-ttu-id="d5512-157">Doručení progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="d5512-157">Deliver progressive download</span></span>

1. <span data-ttu-id="d5512-158">Nahrajte do prostředku vysoce kvalitní mediální soubor.</span><span class="sxs-lookup"><span data-stu-id="d5512-158">Upload a high-quality media file into an asset.</span></span>
2. <span data-ttu-id="d5512-159">Zakódujte tooa jednoho souboru MP4.</span><span class="sxs-lookup"><span data-stu-id="d5512-159">Encode tooa single MP4 file.</span></span>
3. <span data-ttu-id="d5512-160">Publikujte hello asset tak, že vytvoříte Lokátor OnDemand nebo SAS.</span><span class="sxs-lookup"><span data-stu-id="d5512-160">Publish hello asset by creating an OnDemand or SAS locator.</span></span>

    <span data-ttu-id="d5512-161">Pokud používáte Lokátor SAS, hello obsah se stáhne ze hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d5512-161">If using SAS locator, hello content is downloaded from hello Azure blob storage.</span></span> <span data-ttu-id="d5512-162">V takovém případě nepotřebujete toohave koncové body v Začínáme stavu streamování.</span><span class="sxs-lookup"><span data-stu-id="d5512-162">In this case, you do not need toohave streaming endpoints in started state.</span></span>
4. <span data-ttu-id="d5512-163">Progresivně stáhněte obsah.</span><span class="sxs-lookup"><span data-stu-id="d5512-163">Progressively download content.</span></span>

## <span data-ttu-id="d5512-164"><a id="live_scenarios"></a>Doručování živě streamovaných událostí</span><span class="sxs-lookup"><span data-stu-id="d5512-164"><a id="live_scenarios"></a>Delivering live-streaming events</span></span> 

1. <span data-ttu-id="d5512-165">Ingestujte živý obsah pomocí různých protokolů pro živé streamování (například RTMP nebo technologie Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="d5512-165">Ingest live content using various live streaming protocols (for example RTMP or Smooth Streaming).</span></span>
2. <span data-ttu-id="d5512-166">(volitelně) Kódujte datový proud na datový proud s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="d5512-166">(optionally) Encode your stream into adaptive bitrate stream.</span></span>
3. <span data-ttu-id="d5512-167">Zobrazte náhled živého streamování.</span><span class="sxs-lookup"><span data-stu-id="d5512-167">Preview your live stream.</span></span>
4. <span data-ttu-id="d5512-168">doručování obsahu hello prostřednictvím běžných streamovacích protokolů (například MPEG DASH, Smooth, HLS) přímo tooyour zákazníky nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci.</span><span class="sxs-lookup"><span data-stu-id="d5512-168">Deliver hello content through common streaming protocols (for example, MPEG DASH, Smooth, HLS) directly tooyour customers, or tooa Content Delivery Network (CDN) for further distribution.</span></span>

    <span data-ttu-id="d5512-169">-nebo-</span><span class="sxs-lookup"><span data-stu-id="d5512-169">-or-</span></span>

    <span data-ttu-id="d5512-170">Záznam a úložiště hello požity obsah v pořadí toobe Streamovat později (video na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="d5512-170">Record and store hello ingested content in order toobe streamed later (Video-on-Demand).</span></span>

<span data-ttu-id="d5512-171">Při provádění živé vysílání datového proudu, je možné, že jedna z následujících hello tras:</span><span class="sxs-lookup"><span data-stu-id="d5512-171">When doing live streaming, you can choose one of hello following routes:</span></span>

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a><span data-ttu-id="d5512-172">Práce s kanály, které přijímají živé datové proudy s více přenosovými rychlostmi z místních kodérů (průchozí)</span><span class="sxs-lookup"><span data-stu-id="d5512-172">Working with channels that receive multi-bitrate live stream from on-premises encoders (pass-through)</span></span>

<span data-ttu-id="d5512-173">Hello následující diagram znázorňuje hello hlavní části platformy hello AMS, které se podílejí na hello **průchozí** pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="d5512-173">hello following diagram shows hello major parts of hello AMS platform that are involved in hello **pass-through** workflow.</span></span>

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-current.png)

<span data-ttu-id="d5512-175">Další informace najdete v článku o [práci s kanály, které přijímají živé streamování s více přenosovými rychlostmi z místních kodérů](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-175">For more information, see [Working with Channels that Receive Multi-bitrate Live Stream from On-premises Encoders](media-services-live-streaming-with-onprem-encoders.md).</span></span>

### <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a><span data-ttu-id="d5512-176">Práce s kanály, které jsou povolené tooperform za provozu, kódování službou Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="d5512-176">Working with channels that are enabled tooperform live encoding with Azure Media Services</span></span>

<span data-ttu-id="d5512-177">Hello následující diagram znázorňuje hello hlavní části platformy AMS hello souvisejících s pracovním postupu živého streamování kde kanál je s povoleným tooperform za provozu, kódování pomocí služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5512-177">hello following diagram shows hello major parts of hello AMS platform that are involved in Live Streaming workflow where a Channel is enabled tooperform live encoding with Media Services.</span></span>

![Živý pracovní postup](./media/scenarios-and-availability/media-services-live-streaming-new.png)

<span data-ttu-id="d5512-179">Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-179">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="d5512-180">Informace o dostupnosti v datových centrech najdete v tématu hello [dostupnosti](#availability) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-180">For information about availability in datacenters, see hello [Availability](#availability) section.</span></span>

## <a name="consuming-content"></a><span data-ttu-id="d5512-181">Konzumace obsahu</span><span class="sxs-lookup"><span data-stu-id="d5512-181">Consuming content</span></span>

<span data-ttu-id="d5512-182">Azure Media Services poskytuje hello nástroje budete potřebovat toocreate bohaté aplikací pro dynamické klientské přehrávače pro většinu platforem včetně: zařízení iOS, zařízení se systémem Android, Windows, Windows Phone, Xbox a Set top polí.</span><span class="sxs-lookup"><span data-stu-id="d5512-182">Azure Media Services provides hello tools you need toocreate rich, dynamic client player applications for most platforms including: iOS Devices, Android Devices, Windows, Windows Phone, Xbox, and Set-top boxes.</span></span> <span data-ttu-id="d5512-183">Hello následující téma obsahuje odkazy tooSDKs a architektury přehrávačů, které můžete použít toodevelop vlastních klientských aplikací, které můžou využívat streamovaná média ze služby Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5512-183">hello following topic provides links tooSDKs and Player Frameworks that you can use toodevelop your own client applications that can consume streaming media from Media Services.</span></span> <span data-ttu-id="d5512-184">Další informace najdete v tématu [Vývoj aplikací pro přehrávání videa](media-services-develop-video-players.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-184">For more information, see [Developing video payer applications](media-services-develop-video-players.md)</span></span>

## <a name="enabling-azure-cdn"></a><span data-ttu-id="d5512-185">Povolení Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d5512-185">Enabling Azure CDN</span></span>

<span data-ttu-id="d5512-186">Služba Media Services podporuje integraci s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="d5512-186">Media Services supports integration with Azure CDN.</span></span> <span data-ttu-id="d5512-187">Informace o tom najdete v části tooenable Azure CDN [jak tooManage koncových bodů streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-187">For information on how tooenable Azure CDN, see [How tooManage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>

## <span data-ttu-id="d5512-188"><a id="scaling"></a>Škálování účtu Media Services</span><span class="sxs-lookup"><span data-stu-id="d5512-188"><a id="scaling"></a>Scaling a Media Services account</span></span>

<span data-ttu-id="d5512-189">Zákazníci AMS můžou ve svých účtech AMS škálovat koncové body streamování, zpracování médií a úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5512-189">AMS customers can scale streaming endpoints, media processing, and storage in their AMS accounts.</span></span>

* <span data-ttu-id="d5512-190">Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**.</span><span class="sxs-lookup"><span data-stu-id="d5512-190">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="d5512-191">Koncový bod streamování **Standard** je vhodný pro většinu úloh streamování.</span><span class="sxs-lookup"><span data-stu-id="d5512-191">A **Standard** streaming endpoint is suitable for most streaming workloads.</span></span> <span data-ttu-id="d5512-192">Obsahuje hello stejné funkce jako **Premium** streamování koncových bodů a škálují šířky odchozího pásma automaticky.</span><span class="sxs-lookup"><span data-stu-id="d5512-192">It includes hello same features as a **Premium** streaming endpoints and scales outbound bandwidth automatically.</span></span> 

    <span data-ttu-id="d5512-193">Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="d5512-193">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="d5512-194">Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU).</span><span class="sxs-lookup"><span data-stu-id="d5512-194">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="d5512-195">koncový bod streamování Hello je možné rozšířit přidáním služby SUs.</span><span class="sxs-lookup"><span data-stu-id="d5512-195">hello streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="d5512-196">Každý SU poskytuje dodatečnou šířku pásma kapacity toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5512-196">Each SU provides additional bandwidth capacity toohello application.</span></span> <span data-ttu-id="d5512-197">Další informace o škálování **Premium** koncové body streamování, najdete v části hello [škálování koncových bodů streamování](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="d5512-197">For more information about scaling **Premium** streaming endpoints, see hello [Scaling streaming endpoints](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

* <span data-ttu-id="d5512-198">Účet Media Services je přidružen vyhrazené jednotky typu, který určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5512-198">A Media Services account is associated with a Reserved Unit Type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="d5512-199">Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**.</span><span class="sxs-lookup"><span data-stu-id="d5512-199">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="d5512-200">Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu.</span><span class="sxs-lookup"><span data-stu-id="d5512-200">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span>

    <span data-ttu-id="d5512-201">Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných** (ruština).</span><span class="sxs-lookup"><span data-stu-id="d5512-201">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="d5512-202">Hello počet zřízené RUs určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="d5512-202">hello number of provisioned RUs determines hello number of media tasks that can be processed concurrently in a given account.</span></span>

    >[!NOTE]
    ><span data-ttu-id="d5512-203">RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru.</span><span class="sxs-lookup"><span data-stu-id="d5512-203">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="d5512-204">Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.</span><span class="sxs-lookup"><span data-stu-id="d5512-204">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

    <span data-ttu-id="d5512-205">Další informace najdete v tématu [Škálování zpracování médií](media-services-portal-scale-media-processing.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-205">For more information see, [Scale media processing](media-services-portal-scale-media-processing.md).</span></span>
* <span data-ttu-id="d5512-206">Váš účet Media Services můžete škálovat také přidáním tooit účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5512-206">You can also scale your Media Services account by adding storage accounts tooit.</span></span> <span data-ttu-id="d5512-207">Každý účet úložiště je omezená too500 TB.</span><span class="sxs-lookup"><span data-stu-id="d5512-207">Each storage account is limited too500 TB.</span></span> <span data-ttu-id="d5512-208">tooexpand úložiště nad rámec hello výchozí omezení, můžete zvolit tooattach více úložiště účtů tooa jednomu účtu Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5512-208">tooexpand your storage beyond hello default limitations, you can choose tooattach multiple storage accounts tooa single Media Services account.</span></span> <span data-ttu-id="d5512-209">Další informace najdete v tématu [Správa účtů úložiště](meda-services-managing-multiple-storage-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-209">For more information, see [Manage storage accounts](meda-services-managing-multiple-storage-accounts.md).</span></span>

##<span data-ttu-id="d5512-210"><a id="availability"></a>Dostupnost funkcí služby Media Services v datových centrech</span><span class="sxs-lookup"><span data-stu-id="d5512-210"><a id="availability"></a> Availability of Media Services features across datacenters</span></span>

<span data-ttu-id="d5512-211">V této části najdete podrobnosti o dostupnosti funkcí služby Media Services v datových centrech.</span><span class="sxs-lookup"><span data-stu-id="d5512-211">This section provides details about availability of Media Services features across datacenters.</span></span>

### <a name="ams-accounts"></a><span data-ttu-id="d5512-212">Účty AMS</span><span class="sxs-lookup"><span data-stu-id="d5512-212">AMS accounts</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-213">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-213">Availability</span></span>

<span data-ttu-id="d5512-214">Můžete vytvořit účty služby Media Services v hello následující oblasti: Severní Evropa, západní Evropa, západní USA, Východ USA, jihovýchodní Asie, východní Asie, Japonsko – Západ, Japonsko – východ, Brazílie – Jih, Indie – Západ, Indie – jih a Indie – střed.</span><span class="sxs-lookup"><span data-stu-id="d5512-214">You can create Media Services accounts in hello following regions: North Europe, West Europe, West US, East US, Southeast Asia, East Asia, Japan West, Japan East, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="streaming-endpoints"></a><span data-ttu-id="d5512-215">Koncové body streamování</span><span class="sxs-lookup"><span data-stu-id="d5512-215">Streaming endpoints</span></span> 

<span data-ttu-id="d5512-216">Zákazníci Media Services si můžou zvolit koncový bod streamování **Standard**, nebo koncový bod streamování **Premium**.</span><span class="sxs-lookup"><span data-stu-id="d5512-216">Media Services customers can choose either a **Standard** streaming endpoint or a **Premium** streaming endpoint.</span></span> <span data-ttu-id="d5512-217">Další informace najdete v tématu hello [škálování](#scaling) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-217">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-218">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-218">Availability</span></span>

|<span data-ttu-id="d5512-219">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="d5512-219">Name</span></span>|<span data-ttu-id="d5512-220">Status</span><span class="sxs-lookup"><span data-stu-id="d5512-220">Status</span></span>|<span data-ttu-id="d5512-221">Datová centra</span><span class="sxs-lookup"><span data-stu-id="d5512-221">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="d5512-222">Standard</span><span class="sxs-lookup"><span data-stu-id="d5512-222">Standard</span></span>|<span data-ttu-id="d5512-223">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-223">GA</span></span>|<span data-ttu-id="d5512-224">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-224">All</span></span>|
|<span data-ttu-id="d5512-225">Premium</span><span class="sxs-lookup"><span data-stu-id="d5512-225">Premium</span></span>|<span data-ttu-id="d5512-226">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-226">GA</span></span>|<span data-ttu-id="d5512-227">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-227">All</span></span>|

### <a name="live-encoding"></a><span data-ttu-id="d5512-228">Kódování v reálném čase</span><span class="sxs-lookup"><span data-stu-id="d5512-228">Live encoding</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-229">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-229">Availability</span></span>

<span data-ttu-id="d5512-230">K dispozici ve všech datových centrech kromě oblastí: Německo, Brazílie – jih, Indie – západ, Indie – jih a Indie – střed.</span><span class="sxs-lookup"><span data-stu-id="d5512-230">Available in all datacenters except: Germany, Brazil South, India West, India South, and India Central.</span></span> 

### <a name="encoding-media-processors"></a><span data-ttu-id="d5512-231">Kódovací procesory médií</span><span class="sxs-lookup"><span data-stu-id="d5512-231">Encoding media processors</span></span>

<span data-ttu-id="d5512-232">AMS nabízí dva kodéry na vyžádání – **Media Encoder Standard** a **Pracovní postup kodéru Media Encoder Premium**.</span><span class="sxs-lookup"><span data-stu-id="d5512-232">AMS offers two on-demand encoders **Media Encoder Standard** and **Media Encoder Premium Workflow**.</span></span> <span data-ttu-id="d5512-233">Další informace najdete v tématu [Přehled a porovnání kodérů médií na vyžádání v Azure](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-233">For more information, see [Overview and comparison of Azure on-demand media encoders](media-services-encode-asset.md).</span></span> 

#### <a name="availability"></a><span data-ttu-id="d5512-234">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-234">Availability</span></span>

|<span data-ttu-id="d5512-235">Název procesoru médií</span><span class="sxs-lookup"><span data-stu-id="d5512-235">Media processor name</span></span>|<span data-ttu-id="d5512-236">Status</span><span class="sxs-lookup"><span data-stu-id="d5512-236">Status</span></span>|<span data-ttu-id="d5512-237">Datová centra</span><span class="sxs-lookup"><span data-stu-id="d5512-237">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="d5512-238">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d5512-238">Media Encoder Standard</span></span>|<span data-ttu-id="d5512-239">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-239">GA</span></span>|<span data-ttu-id="d5512-240">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-240">All</span></span>|
|<span data-ttu-id="d5512-241">Pracovní postup kodéru Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="d5512-241">Media Encoder Premium Workflow</span></span>|<span data-ttu-id="d5512-242">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-242">GA</span></span>|<span data-ttu-id="d5512-243">Všechny s výjimkou Číny</span><span class="sxs-lookup"><span data-stu-id="d5512-243">All except China</span></span>|

### <a name="analytics-media-processors"></a><span data-ttu-id="d5512-244">Analytické procesory médií</span><span class="sxs-lookup"><span data-stu-id="d5512-244">Analytics media processors</span></span>

<span data-ttu-id="d5512-245">Media Analytics je kolekce řečových a vizuálních komponent, které usnadňuje organizacím a podnikům umožňují tooderive prakticky uplatnitelných informací ze svých videosouborů.</span><span class="sxs-lookup"><span data-stu-id="d5512-245">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="d5512-246">Další informace najdete v článku o [přehledu Azure Media Services Analytics](media-services-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-246">For more information, see [Azure Media Services Analytics Overview](media-services-analytics-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-247">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-247">Availability</span></span>

|<span data-ttu-id="d5512-248">Název procesoru médií</span><span class="sxs-lookup"><span data-stu-id="d5512-248">Media processor name</span></span>|<span data-ttu-id="d5512-249">Status</span><span class="sxs-lookup"><span data-stu-id="d5512-249">Status</span></span>|<span data-ttu-id="d5512-250">Datová centra</span><span class="sxs-lookup"><span data-stu-id="d5512-250">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="d5512-251">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="d5512-251">Azure Media Face Detector</span></span>|<span data-ttu-id="d5512-252">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-252">Preview</span></span>|<span data-ttu-id="d5512-253">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-253">All</span></span>|
|<span data-ttu-id="d5512-254">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="d5512-254">Azure Media Hyperlapse</span></span>|<span data-ttu-id="d5512-255">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-255">Preview</span></span>|<span data-ttu-id="d5512-256">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-256">All</span></span>|
|<span data-ttu-id="d5512-257">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="d5512-257">Azure Media Indexer</span></span>|<span data-ttu-id="d5512-258">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-258">GA</span></span>|<span data-ttu-id="d5512-259">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-259">All</span></span>|
|<span data-ttu-id="d5512-260">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="d5512-260">Azure Media Motion Detector</span></span>|<span data-ttu-id="d5512-261">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-261">Preview</span></span>|<span data-ttu-id="d5512-262">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-262">All</span></span>|
|<span data-ttu-id="d5512-263">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="d5512-263">Azure Media OCR</span></span>|<span data-ttu-id="d5512-264">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-264">Preview</span></span>|<span data-ttu-id="d5512-265">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-265">All</span></span>|
|<span data-ttu-id="d5512-266">Azure Media Redactor</span><span class="sxs-lookup"><span data-stu-id="d5512-266">Azure Media Redactor</span></span>|<span data-ttu-id="d5512-267">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-267">Preview</span></span>|<span data-ttu-id="d5512-268">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-268">All</span></span>|
|<span data-ttu-id="d5512-269">Azure Media Stabilizer</span><span class="sxs-lookup"><span data-stu-id="d5512-269">Azure Media Stabilizer</span></span>|<span data-ttu-id="d5512-270">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-270">Preview</span></span>|<span data-ttu-id="d5512-271">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-271">All</span></span>|
|<span data-ttu-id="d5512-272">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="d5512-272">Azure Media Video Thumbnails</span></span>|<span data-ttu-id="d5512-273">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-273">Preview</span></span>|<span data-ttu-id="d5512-274">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-274">All</span></span>|
|<span data-ttu-id="d5512-275">Azure Media Indexer 2</span><span class="sxs-lookup"><span data-stu-id="d5512-275">Azure Media Indexer 2</span></span>|<span data-ttu-id="d5512-276">Preview</span><span class="sxs-lookup"><span data-stu-id="d5512-276">Preview</span></span>|<span data-ttu-id="d5512-277">Všechny s výjimkou Číny a oblasti federální vlády</span><span class="sxs-lookup"><span data-stu-id="d5512-277">All except China and Federal Government region</span></span>|

### <a name="protection"></a><span data-ttu-id="d5512-278">Ochrana</span><span class="sxs-lookup"><span data-stu-id="d5512-278">Protection</span></span>

<span data-ttu-id="d5512-279">Microsoft Azure Media Services umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení.</span><span class="sxs-lookup"><span data-stu-id="d5512-279">Microsoft Azure Media Services enables you toosecure your media from hello time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="d5512-280">Další informace najdete v tématu [Ochrana obsahu AMS](media-services-content-protection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5512-280">For more information, see [Protecting AMS content](media-services-content-protection-overview.md).</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-281">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-281">Availability</span></span>

|<span data-ttu-id="d5512-282">Šifrování</span><span class="sxs-lookup"><span data-stu-id="d5512-282">Encryption</span></span>|<span data-ttu-id="d5512-283">Status</span><span class="sxs-lookup"><span data-stu-id="d5512-283">Status</span></span>|<span data-ttu-id="d5512-284">Datová centra</span><span class="sxs-lookup"><span data-stu-id="d5512-284">Datacenters</span></span>|
|---|---|---| 
|<span data-ttu-id="d5512-285">Úložiště</span><span class="sxs-lookup"><span data-stu-id="d5512-285">Storage</span></span>|<span data-ttu-id="d5512-286">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-286">GA</span></span>|<span data-ttu-id="d5512-287">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-287">All</span></span>|
|<span data-ttu-id="d5512-288">Klíče AES-128</span><span class="sxs-lookup"><span data-stu-id="d5512-288">AES-128 keys</span></span>|<span data-ttu-id="d5512-289">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-289">GA</span></span>|<span data-ttu-id="d5512-290">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-290">All</span></span>|
|<span data-ttu-id="d5512-291">FairPlay</span><span class="sxs-lookup"><span data-stu-id="d5512-291">Fairplay</span></span>|<span data-ttu-id="d5512-292">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-292">GA</span></span>|<span data-ttu-id="d5512-293">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-293">All</span></span>|
|<span data-ttu-id="d5512-294">PlayReady</span><span class="sxs-lookup"><span data-stu-id="d5512-294">PlayReady</span></span>|<span data-ttu-id="d5512-295">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-295">GA</span></span>|<span data-ttu-id="d5512-296">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-296">All</span></span>|
|<span data-ttu-id="d5512-297">Widevine</span><span class="sxs-lookup"><span data-stu-id="d5512-297">Widevine</span></span>|<span data-ttu-id="d5512-298">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-298">GA</span></span>|<span data-ttu-id="d5512-299">Všechna kromě oblastí Německo, Federální vláda a Čína.</span><span class="sxs-lookup"><span data-stu-id="d5512-299">All except Germany, Federal Government and China.</span></span>

### <a name="reserved-units-rus"></a><span data-ttu-id="d5512-300">Rezervované jednotky (RU)</span><span class="sxs-lookup"><span data-stu-id="d5512-300">Reserved units (RUs)</span></span>

<span data-ttu-id="d5512-301">Hello počet jednotek rezervovaných pro zřízené určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="d5512-301">hello number of provisioned reserved units determines hello number of media tasks that can be processed concurrently in a given account.</span></span> 

<span data-ttu-id="d5512-302">Další informace najdete v tématu hello [škálování](#scaling) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-302">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-303">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-303">Availability</span></span>

<span data-ttu-id="d5512-304">K dispozici ve všech datových centrech.</span><span class="sxs-lookup"><span data-stu-id="d5512-304">Available in all datacenters.</span></span>

### <a name="reserved-unit-ru-type"></a><span data-ttu-id="d5512-305">Typ rezervované jednotky (RU)</span><span class="sxs-lookup"><span data-stu-id="d5512-305">Reserved unit (RU) type</span></span>

<span data-ttu-id="d5512-306">Účet Media Services je přidružený k typu jednotku rezervovanou, která určuje, ke které se zpracovávají médiu zpracování úlohy rychlost hello.</span><span class="sxs-lookup"><span data-stu-id="d5512-306">A Media Services account is associated with a Reserved unit type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="d5512-307">Můžete si vybrat mezi hello následující vyhrazené typy jednotka: S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="d5512-307">You can pick between hello following reserved unit types: S1, S2, or S3.</span></span>

<span data-ttu-id="d5512-308">Další informace najdete v tématu hello [škálování](#scaling) části.</span><span class="sxs-lookup"><span data-stu-id="d5512-308">For more information, see hello [scaling](#scaling) section.</span></span>

#### <a name="availability"></a><span data-ttu-id="d5512-309">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="d5512-309">Availability</span></span>

|<span data-ttu-id="d5512-310">Název typu RU</span><span class="sxs-lookup"><span data-stu-id="d5512-310">RU type name</span></span>|<span data-ttu-id="d5512-311">Status</span><span class="sxs-lookup"><span data-stu-id="d5512-311">Status</span></span>|<span data-ttu-id="d5512-312">Datová centra</span><span class="sxs-lookup"><span data-stu-id="d5512-312">Datacenters</span></span>
|---|---|---|
|<span data-ttu-id="d5512-313">S1</span><span class="sxs-lookup"><span data-stu-id="d5512-313">S1</span></span>|<span data-ttu-id="d5512-314">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-314">GA</span></span>|<span data-ttu-id="d5512-315">Všechny</span><span class="sxs-lookup"><span data-stu-id="d5512-315">All</span></span>|
|<span data-ttu-id="d5512-316">S2</span><span class="sxs-lookup"><span data-stu-id="d5512-316">S2</span></span>|<span data-ttu-id="d5512-317">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-317">GA</span></span>|<span data-ttu-id="d5512-318">Všechna kromě oblastí Brazílie – jih a Indie – západ</span><span class="sxs-lookup"><span data-stu-id="d5512-318">All except Brazil South, and India West</span></span>|
|<span data-ttu-id="d5512-319">S3</span><span class="sxs-lookup"><span data-stu-id="d5512-319">S3</span></span>|<span data-ttu-id="d5512-320">GA</span><span class="sxs-lookup"><span data-stu-id="d5512-320">GA</span></span>|<span data-ttu-id="d5512-321">Všechna kromě oblasti Indie – západ</span><span class="sxs-lookup"><span data-stu-id="d5512-321">All except India West</span></span>|

## <a name="next-steps"></a><span data-ttu-id="d5512-322">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5512-322">Next steps</span></span>

<span data-ttu-id="d5512-323">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="d5512-323">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d5512-324">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d5512-324">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

