---
title: "datový proud aaaLive s místními kodéry pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello k vytvoření kanálu, který je nakonfigurován pro průchozí doručování."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a><span data-ttu-id="82f99-103">Jak tooperform živé streamování s místními kodéry pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82f99-103">How tooperform live streaming with on-premises encoders using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="82f99-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="82f99-104">Portal</span></span>](media-services-portal-live-passthrough-get-started.md)
> * [<span data-ttu-id="82f99-105">.NET</span><span class="sxs-lookup"><span data-stu-id="82f99-105">.NET</span></span>](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [<span data-ttu-id="82f99-106">REST</span><span class="sxs-lookup"><span data-stu-id="82f99-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

<span data-ttu-id="82f99-107">Tento kurz vás provede kroky hello hello Azure portálu toocreate **kanál** který je nakonfigurován pro průchozí doručování.</span><span class="sxs-lookup"><span data-stu-id="82f99-107">This tutorial walks you through hello steps of using hello Azure portal toocreate a **Channel** that is configured for a pass-through delivery.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="82f99-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82f99-108">Prerequisites</span></span>
<span data-ttu-id="82f99-109">Hello následují požadované toocomplete hello kurzu:</span><span class="sxs-lookup"><span data-stu-id="82f99-109">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="82f99-110">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="82f99-110">An Azure account.</span></span> <span data-ttu-id="82f99-111">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82f99-111">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="82f99-112">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="82f99-112">A Media Services account.</span></span> <span data-ttu-id="82f99-113">toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="82f99-113">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="82f99-114">Webová kamera.</span><span class="sxs-lookup"><span data-stu-id="82f99-114">A webcam.</span></span> <span data-ttu-id="82f99-115">Například [kodér Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm).</span><span class="sxs-lookup"><span data-stu-id="82f99-115">For example, [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).</span></span>

<span data-ttu-id="82f99-116">Důrazně doporučujeme tooreview hello následující články:</span><span class="sxs-lookup"><span data-stu-id="82f99-116">It is highly recommended tooreview hello following articles:</span></span>

* [<span data-ttu-id="82f99-117">Podpora RTMP ve službě Azure Media Services a kodéry služby Live Encoding</span><span class="sxs-lookup"><span data-stu-id="82f99-117">Azure Media Services RTMP Support and Live Encoders</span></span>](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [<span data-ttu-id="82f99-118">Přehled živého streamování pomocí služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="82f99-118">Overview of Live Steaming using Azure Media Services</span></span>](media-services-manage-channels-overview.md)
* [<span data-ttu-id="82f99-119">Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi</span><span class="sxs-lookup"><span data-stu-id="82f99-119">Live streaming with on-premises encoders that create multi-bitrate streams</span></span>](media-services-live-streaming-with-onprem-encoders.md)

## <span data-ttu-id="82f99-120"><a id="scenario"></a>Běžný scénář živého streamování</span><span class="sxs-lookup"><span data-stu-id="82f99-120"><a id="scenario"></a>Common live streaming scenario</span></span>
<span data-ttu-id="82f99-121">Hello následující kroky popisují úlohy týkající se vytváření běžné aplikací pro živé streamování využívající kanály, které jsou nakonfigurované pro průchozí doručování.</span><span class="sxs-lookup"><span data-stu-id="82f99-121">hello following steps describe tasks involved in creating common live streaming applications that use channels that are configured for pass-through delivery.</span></span> <span data-ttu-id="82f99-122">Tento kurz ukazuje, jak toocreate a spravovat průchozí kanál a živé události.</span><span class="sxs-lookup"><span data-stu-id="82f99-122">This tutorial shows how toocreate and manage a pass-through channel and live events.</span></span>

>[!NOTE]
><span data-ttu-id="82f99-123">Zkontrolujte, zda text hello, ze kterého chcete obsah toostream koncový bod streamování je v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="82f99-123">Make sure hello streaming endpoint from which you want toostream content is in hello **Running** state.</span></span> 
    
1. <span data-ttu-id="82f99-124">Připojení počítače tooa videokameru.</span><span class="sxs-lookup"><span data-stu-id="82f99-124">Connect a video camera tooa computer.</span></span> <span data-ttu-id="82f99-125">Spusťte a nakonfigurujte místní kodér pro kódování v reálném čase, který produkuje RTMP s více přenosovými rychlostmi nebo fragmentovaný proud MP4.</span><span class="sxs-lookup"><span data-stu-id="82f99-125">Launch and configure an on-premises live encoder that outputs a multi-bitrate RTMP or Fragmented MP4 stream.</span></span> <span data-ttu-id="82f99-126">Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).</span><span class="sxs-lookup"><span data-stu-id="82f99-126">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>
   
    <span data-ttu-id="82f99-127">Tento krok můžete provést i po vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="82f99-127">This step could also be performed after you create your Channel.</span></span>
2. <span data-ttu-id="82f99-128">Vytvořit a spustit průchozí kanál.</span><span class="sxs-lookup"><span data-stu-id="82f99-128">Create and start a pass-through Channel.</span></span>
3. <span data-ttu-id="82f99-129">Načtení hello kanál ingestovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82f99-129">Retrieve hello Channel ingest URL.</span></span> 
   
    <span data-ttu-id="82f99-130">Hello URL ingestování používá hello za provozu kodér toosend hello datového proudu toohello kanál.</span><span class="sxs-lookup"><span data-stu-id="82f99-130">hello ingest URL is used by hello live encoder toosend hello stream toohello Channel.</span></span>
4. <span data-ttu-id="82f99-131">Načíst URL náhledu kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-131">Retrieve hello Channel preview URL.</span></span> 
   
    <span data-ttu-id="82f99-132">Pomocí této adresy URL tooverify, jestli kanál správně přijímá živý datový proud hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-132">Use this URL tooverify that your channel is properly receiving hello live stream.</span></span>
5. <span data-ttu-id="82f99-133">Vytvořte živou událost nebo program.</span><span class="sxs-lookup"><span data-stu-id="82f99-133">Create a live event/program.</span></span> 
   
    <span data-ttu-id="82f99-134">Při použití hello portálu Azure, vytváření živé události vytvoří také asset.</span><span class="sxs-lookup"><span data-stu-id="82f99-134">When using hello Azure portal, creating a live event also creates an asset.</span></span> 

6. <span data-ttu-id="82f99-135">Jakmile jsou připravené toostart Streamovat a archivovat, spusťte hello událost nebo program.</span><span class="sxs-lookup"><span data-stu-id="82f99-135">Start hello event/program when you are ready toostart streaming and archiving.</span></span>
7. <span data-ttu-id="82f99-136">Volitelně lze za provozu kodér hello signalizovaného toostart oznámení o inzerovaném programu.</span><span class="sxs-lookup"><span data-stu-id="82f99-136">Optionally, hello live encoder can be signaled toostart an advertisement.</span></span> <span data-ttu-id="82f99-137">Hello reklama bude vložena do výstupního datového proudu hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-137">hello advertisement is inserted in hello output stream.</span></span>
8. <span data-ttu-id="82f99-138">Vždy, když chcete toostop streamování a archivaci hello události, zastavte hello událost nebo program.</span><span class="sxs-lookup"><span data-stu-id="82f99-138">Stop hello event/program whenever you want toostop streaming and archiving hello event.</span></span>
9. <span data-ttu-id="82f99-139">Odstraňte hello událost nebo program (a volitelně můžete odstranit hello asset).</span><span class="sxs-lookup"><span data-stu-id="82f99-139">Delete hello event/program (and optionally delete hello asset).</span></span>     

> [!IMPORTANT]
> <span data-ttu-id="82f99-140">Zkontrolujte [živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md) toolearn o konceptech a důležité informace související s toolive streamování s místními kodéry a průchozími kanály.</span><span class="sxs-lookup"><span data-stu-id="82f99-140">Please review [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) toolearn about concepts and considerations related toolive streaming with on-premises encoders and pass-through channels.</span></span>
> 
> 

## <a name="tooview-notifications-and-errors"></a><span data-ttu-id="82f99-141">tooview upozornění a chyb</span><span class="sxs-lookup"><span data-stu-id="82f99-141">tooview notifications and errors</span></span>
<span data-ttu-id="82f99-142">Pokud chcete, aby tooview oznámení a chyby vytvořené hello portálu Azure, klikněte na ikonu oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-142">If you want tooview notifications and errors produced by hello Azure portal, click on hello Notification icon.</span></span>

![Oznámení](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a><span data-ttu-id="82f99-144">Vytvoření a spuštění průchozího kanálu.</span><span class="sxs-lookup"><span data-stu-id="82f99-144">Create and start pass-through channels and events</span></span>
<span data-ttu-id="82f99-145">Kanál, který je přidružen k událostem a programům, které umožňují toocontrol hello publikování a ukládání segmentů v živém datovém proudu.</span><span class="sxs-lookup"><span data-stu-id="82f99-145">A channel is associated with events/programs that enable you toocontrol hello publishing and storage of segments in a live stream.</span></span> <span data-ttu-id="82f99-146">Kanály spravují události.</span><span class="sxs-lookup"><span data-stu-id="82f99-146">Channels manage events.</span></span> 

<span data-ttu-id="82f99-147">Můžete zadat hello počet hodin, které chcete obsah hello zaznamenávají tooretain programu hello podle nastavení hello **archivačního okna** délka.</span><span class="sxs-lookup"><span data-stu-id="82f99-147">You can specify hello number of hours you want tooretain hello recorded content for hello program by setting hello **Archive Window** length.</span></span> <span data-ttu-id="82f99-148">Tuto hodnotu můžete nastavit v rozmezí od 5 minut tooa maximálně 25 hodin.</span><span class="sxs-lookup"><span data-stu-id="82f99-148">This value can be set from a minimum of 5 minutes tooa maximum of 25 hours.</span></span> <span data-ttu-id="82f99-149">Délka archivačního okna také určuje maximální množství času, které klienty můžete hledat zpět v čase od aktuální živé pozice hello hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-149">Archive window length also dictates hello maximum amount of time clients can seek back in time from hello current live position.</span></span> <span data-ttu-id="82f99-150">Události můžete spustit přes hello určenou dobu a obsah, který hello délky okna nevejde je vždy zahozen.</span><span class="sxs-lookup"><span data-stu-id="82f99-150">Events can run over hello specified amount of time, but content that falls behind hello window length is continuously discarded.</span></span> <span data-ttu-id="82f99-151">Hodnota této vlastnosti také určuje, jak dlouho hello klienta můžou růst manifesty.</span><span class="sxs-lookup"><span data-stu-id="82f99-151">This value of this property also determines how long hello client manifests can grow.</span></span>

<span data-ttu-id="82f99-152">Každá událost je přidružena k assetu.</span><span class="sxs-lookup"><span data-stu-id="82f99-152">Each event is associated with an asset.</span></span> <span data-ttu-id="82f99-153">toopublish hello událostí, je nutné vytvořit lokátor OnDemand pro hello související prostředek.</span><span class="sxs-lookup"><span data-stu-id="82f99-153">toopublish hello event, you must create an OnDemand locator for hello associated asset.</span></span> <span data-ttu-id="82f99-154">Tento Lokátor umožňuje toobuild adresu URL pro streamování, kterou potom poskytnete tooyour klientů.</span><span class="sxs-lookup"><span data-stu-id="82f99-154">Having this locator enables you toobuild a streaming URL that you can provide tooyour clients.</span></span>

<span data-ttu-id="82f99-155">Kanál podporuje až toothree souběžně s události, takže si můžete vytvořit několik archivů hello stejného příchozího datového proudu.</span><span class="sxs-lookup"><span data-stu-id="82f99-155">A channel supports up toothree concurrently running events so you can create multiple archives of hello same incoming stream.</span></span> <span data-ttu-id="82f99-156">To vám umožní toopublish a archivovat různé části události podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="82f99-156">This allows you toopublish and archive different parts of an event as needed.</span></span> <span data-ttu-id="82f99-157">Například vaše firemní požadavky je tooarchive 6 hodin programu, ale toobroadcast pouze posledních 10 minut.</span><span class="sxs-lookup"><span data-stu-id="82f99-157">For example, your business requirement is tooarchive 6 hours of a program, but toobroadcast only last 10 minutes.</span></span> <span data-ttu-id="82f99-158">tooaccomplish, je nutné toocreate dva současně spuštěné programy.</span><span class="sxs-lookup"><span data-stu-id="82f99-158">tooaccomplish this, you need toocreate two concurrently running programs.</span></span> <span data-ttu-id="82f99-159">Jeden program nastaven tooarchive 6 hodin hello události ale programu hello není publikována.</span><span class="sxs-lookup"><span data-stu-id="82f99-159">One program is set tooarchive 6 hours of hello event but hello program is not published.</span></span> <span data-ttu-id="82f99-160">Hello jiný program je sada tooarchive 10 minut a tento program budete publikovat.</span><span class="sxs-lookup"><span data-stu-id="82f99-160">hello other program is set tooarchive for 10 minutes and this program is published.</span></span>

<span data-ttu-id="82f99-161">Neměli byste znovu používat existující živé události.</span><span class="sxs-lookup"><span data-stu-id="82f99-161">You should not reuse existing live events.</span></span> <span data-ttu-id="82f99-162">Místo toho vytvořte a spusťte novou událost pro každou jednotlivou událost.</span><span class="sxs-lookup"><span data-stu-id="82f99-162">Instead, create and start a new event for each event.</span></span>

<span data-ttu-id="82f99-163">Spusťte hello událost v případě, že jsou připravené toostart streamování a archivaci.</span><span class="sxs-lookup"><span data-stu-id="82f99-163">Start hello event when you are ready toostart streaming and archiving.</span></span> <span data-ttu-id="82f99-164">Zastavte programu hello vždy, když chcete toostop streamování a archivaci události hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-164">Stop hello program whenever you want toostop streaming and archiving hello event.</span></span> 

<span data-ttu-id="82f99-165">obsah toodelete archivovat, zastavte a odstranit hello událost a potom odstraňte přidružený asset hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-165">toodelete archived content, stop and delete hello event and then delete hello associated asset.</span></span> <span data-ttu-id="82f99-166">Asset nemůžete odstranit, pokud se používá událost; Nejprve je třeba odstranit Hello událostí.</span><span class="sxs-lookup"><span data-stu-id="82f99-166">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="82f99-167">I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset.</span><span class="sxs-lookup"><span data-stu-id="82f99-167">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span>

<span data-ttu-id="82f99-168">Pokud chcete archivovat hello tooretain obsahu, ale není ho mít dostupný pro streamování, odstraňte Lokátor streamování hello.</span><span class="sxs-lookup"><span data-stu-id="82f99-168">If you do want tooretain hello archived content, but not have it available for streaming, delete hello streaming locator.</span></span>

### <a name="toouse-hello-portal-toocreate-a-channel"></a><span data-ttu-id="82f99-169">toouse hello portálu toocreate kanál</span><span class="sxs-lookup"><span data-stu-id="82f99-169">toouse hello portal toocreate a channel</span></span>
<span data-ttu-id="82f99-170">Tato část uvádí, jak toouse hello **rychle vytvořit** možnost toocreate průchozí kanál.</span><span class="sxs-lookup"><span data-stu-id="82f99-170">This section shows how toouse hello **Quick Create** option toocreate a pass-through channel.</span></span>

<span data-ttu-id="82f99-171">Další podrobnosti o průchozích kanálech najdete v tématu [Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="82f99-171">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

1. <span data-ttu-id="82f99-172">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="82f99-172">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="82f99-173">V hello **nastavení** okně klikněte na tlačítko **živé streamování**.</span><span class="sxs-lookup"><span data-stu-id="82f99-173">In hello **Settings** window, click **Live streaming**.</span></span> 
   
    ![Začínáme](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    <span data-ttu-id="82f99-175">Hello **živé streamování** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="82f99-175">hello **Live streaming** window appears.</span></span>
3. <span data-ttu-id="82f99-176">Klikněte na tlačítko **rychle vytvořit** ingestování toocreate průchozí kanál s hello RTMP.</span><span class="sxs-lookup"><span data-stu-id="82f99-176">Click **Quick Create** toocreate a pass-through channel with hello RTMP ingest protocol.</span></span>
   
    <span data-ttu-id="82f99-177">Hello **vytvořit nový kanál** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="82f99-177">hello **CREATE A NEW CHANNEL** window appears.</span></span>
4. <span data-ttu-id="82f99-178">Zadejte název nového kanálu hello a klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="82f99-178">Give hello new channel a name and click **Create**.</span></span> 
   
    <span data-ttu-id="82f99-179">Tím vytvoříte průchozí kanál s hello protokolem ingestování RTMP.</span><span class="sxs-lookup"><span data-stu-id="82f99-179">This creates a pass-through channel with hello RTMP ingest protocol.</span></span>

## <a name="create-events"></a><span data-ttu-id="82f99-180">Vytvoření událostí</span><span class="sxs-lookup"><span data-stu-id="82f99-180">Create events</span></span>
1. <span data-ttu-id="82f99-181">Vyberte kanál toowhich, chcete-li tooadd událost.</span><span class="sxs-lookup"><span data-stu-id="82f99-181">Select a channel toowhich you want tooadd an event.</span></span>
2. <span data-ttu-id="82f99-182">Stiskněte tlačítko **Živá událost**.</span><span class="sxs-lookup"><span data-stu-id="82f99-182">Press **Live Event** button.</span></span>

![Událost](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a><span data-ttu-id="82f99-184">Získání ingestovaných adres URL</span><span class="sxs-lookup"><span data-stu-id="82f99-184">Get ingest URLs</span></span>
<span data-ttu-id="82f99-185">Po vytvoření kanálu hello lze získat ingestovaných adres URL, které poskytnete kodéru toohello za provozu.</span><span class="sxs-lookup"><span data-stu-id="82f99-185">Once hello channel is created, you can get ingest URLs that you will provide toohello live encoder.</span></span> <span data-ttu-id="82f99-186">Kodér Hello používá tyto adresy URL tooinput živý datový proud.</span><span class="sxs-lookup"><span data-stu-id="82f99-186">hello encoder uses these URLs tooinput a live stream.</span></span>

![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a><span data-ttu-id="82f99-188">Sledování událostí hello</span><span class="sxs-lookup"><span data-stu-id="82f99-188">Watch hello event</span></span>
<span data-ttu-id="82f99-189">toowatch hello událostí, klikněte na tlačítko **sledovat** v hello Azure portal nebo kopírování hello adresu URL streamování a použijte přehrávač dle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="82f99-189">toowatch hello event, click **Watch** in hello Azure portal or copy hello streaming URL and use a player of your choice.</span></span> 

![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

<span data-ttu-id="82f99-191">Živé události se automaticky převedený tooon vyžádání obsah při zastavení.</span><span class="sxs-lookup"><span data-stu-id="82f99-191">Live event automatically get converted tooon-demand content when stopped.</span></span>

## <a name="clean-up"></a><span data-ttu-id="82f99-192">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="82f99-192">Clean up</span></span>
<span data-ttu-id="82f99-193">Další podrobnosti o průchozích kanálech najdete v tématu [Živé streamování pomocí místních kodérů, které vytvářejí datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="82f99-193">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

* <span data-ttu-id="82f99-194">Kanál se dá zastavit jenom v případě, že byly zastaveny všechny události nebo programy na hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="82f99-194">A channel can be stopped only when all events/programs on hello channel have been stopped.</span></span>  <span data-ttu-id="82f99-195">Jakmile hello kanál zastaví nedojde žádné poplatky.</span><span class="sxs-lookup"><span data-stu-id="82f99-195">Once hello Channel is stopped, it does not incur any charges.</span></span> <span data-ttu-id="82f99-196">Když potřebujete toostart ho znovu, bude mít hello stejnou ingestovanou adresu URL, takže nebude nutné tooreconfigure kodér.</span><span class="sxs-lookup"><span data-stu-id="82f99-196">When you need toostart it again, it will have hello same ingest URL so you won't need tooreconfigure your encoder.</span></span>
* <span data-ttu-id="82f99-197">Kanál se dá odstranit jenom v případě, že byly odstraněny všechny jeho živé události ve hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="82f99-197">A channel can be deleted only when all live events on hello channel have been deleted.</span></span>

## <a name="view-archived-content"></a><span data-ttu-id="82f99-198">Zobrazení archivovaného obsahu</span><span class="sxs-lookup"><span data-stu-id="82f99-198">View archived content</span></span>
<span data-ttu-id="82f99-199">I po zastavení a odstranění události hello, hello uživatelé by byl schopný toostream archivovaný obsah jako video na vyžádání, tak dlouho, dokud neodstraníte hello asset.</span><span class="sxs-lookup"><span data-stu-id="82f99-199">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span> <span data-ttu-id="82f99-200">Asset nemůžete odstranit, pokud se používá událost; Nejprve je třeba odstranit Hello událostí.</span><span class="sxs-lookup"><span data-stu-id="82f99-200">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="82f99-201">Vyberte prostředky, toomanage **nastavení** a klikněte na tlačítko **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="82f99-201">toomanage your assets, select **Setting** and click **Assets**.</span></span>

![Prostředky](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a><span data-ttu-id="82f99-203">Další krok</span><span class="sxs-lookup"><span data-stu-id="82f99-203">Next step</span></span>
<span data-ttu-id="82f99-204">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="82f99-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="82f99-205">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="82f99-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

