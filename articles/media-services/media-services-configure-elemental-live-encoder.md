---
title: "aaaConfigure hello elementární Live toosend kodér živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello elementární Live toosend kodér jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="afb2e-103">Použít hello elementární Live kodér toosend živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="afb2e-103">Use hello Elemental Live encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="afb2e-104">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="afb2e-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="afb2e-105">Čase</span><span class="sxs-lookup"><span data-stu-id="afb2e-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="afb2e-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="afb2e-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="afb2e-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="afb2e-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="afb2e-108">Toto téma ukazuje, jak tooconfigure hello [elementární Live](http://www.elementaltechnologies.com/products/elemental-live) kodér toosend tooAMS kanály, které jsou povolené kódování v reálném čase datového proudu s jednou přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="afb2e-108">This topic shows how tooconfigure hello [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="afb2e-109">Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="afb2e-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="afb2e-110">Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="afb2e-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="afb2e-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="afb2e-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="afb2e-112">Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="afb2e-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="afb2e-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="afb2e-113">Prerequisites</span></span>
* <span data-ttu-id="afb2e-114">Musí mít praktické znalosti pomocí elementární Live webové rozhraní toocreate živé události.</span><span class="sxs-lookup"><span data-stu-id="afb2e-114">Must have a working knowledge of using Elemental Live web interface toocreate live events.</span></span>
* [<span data-ttu-id="afb2e-115">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="afb2e-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="afb2e-116">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="afb2e-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="afb2e-117">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="afb2e-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="afb2e-118">Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="afb2e-118">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="afb2e-119">Spusťte nástroj hello a připojte se účet tooyour AMS.</span><span class="sxs-lookup"><span data-stu-id="afb2e-119">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="afb2e-120">Tipy</span><span class="sxs-lookup"><span data-stu-id="afb2e-120">Tips</span></span>
* <span data-ttu-id="afb2e-121">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="afb2e-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="afb2e-122">Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="afb2e-122">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="afb2e-123">Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="afb2e-123">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="afb2e-124">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="afb2e-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="afb2e-125">Ingestování elementární živé s RTP</span><span class="sxs-lookup"><span data-stu-id="afb2e-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="afb2e-126">Tato část uvádí, jak tooconfigure hello elementární Live kodér, který odešle s jednou přenosovou rychlostí živý datový proud využívající RTP.</span><span class="sxs-lookup"><span data-stu-id="afb2e-126">This section shows how tooconfigure hello Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="afb2e-127">Další informace najdete v tématu [stream MPEG-TS využívající RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="afb2e-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="afb2e-128">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="afb2e-128">Create a channel</span></span>

1. <span data-ttu-id="afb2e-129">Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-129">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="afb2e-130">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="afb2e-130">Select **Create channel…**</span></span> <span data-ttu-id="afb2e-131">v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-131">from hello menu.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="afb2e-133">Zadejte název kanálu, hello pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="afb2e-133">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="afb2e-134">V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-134">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTP (MPEG-TS)**.</span></span> <span data-ttu-id="afb2e-135">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="afb2e-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="afb2e-136">Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="afb2e-136">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="afb2e-137">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-137">Click **Create Channel**.</span></span>

   ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="afb2e-139">Hello kanálu může trvat stejně dlouho jako toostart 20 minut.</span><span class="sxs-lookup"><span data-stu-id="afb2e-139">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="afb2e-140">Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="afb2e-140">While hello channel is starting you can [configure hello encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="afb2e-141">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="afb2e-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="afb2e-142">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="afb2e-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="afb2e-143"><a id=configure_elemental_rtp></a>Konfigurace kodér hello elementární za provozu</span><span class="sxs-lookup"><span data-stu-id="afb2e-143"><a id=configure_elemental_rtp></a>Configure hello Elemental Live encoder</span></span>
<span data-ttu-id="afb2e-144">V tento kurz hello se používají následující výstup nastavení.</span><span class="sxs-lookup"><span data-stu-id="afb2e-144">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="afb2e-145">Hello zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="afb2e-145">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="afb2e-146">**Video**:</span><span class="sxs-lookup"><span data-stu-id="afb2e-146">**Video**:</span></span>

* <span data-ttu-id="afb2e-147">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="afb2e-147">Codec: H.264</span></span>
* <span data-ttu-id="afb2e-148">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="afb2e-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="afb2e-149">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="afb2e-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="afb2e-150">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="afb2e-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="afb2e-151">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="afb2e-151">Frame Rate: 30</span></span>

<span data-ttu-id="afb2e-152">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="afb2e-152">**Audio**:</span></span>

* <span data-ttu-id="afb2e-153">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="afb2e-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="afb2e-154">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="afb2e-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="afb2e-155">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="afb2e-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="afb2e-156">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="afb2e-156">Configuration steps</span></span>
1. <span data-ttu-id="afb2e-157">Přejděte toohello **elementární Live** webové rozhraní a nastavte hello kodéru pro **UDP/TS** streamování.</span><span class="sxs-lookup"><span data-stu-id="afb2e-157">Navigate toohello **Elemental Live** web interface, and set up hello encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="afb2e-158">Jakmile dojde k vytvoření nové události, posuňte se dolů toohello výstup skupiny a přidejte hello **UDP/TS** skupiny výstupu.</span><span class="sxs-lookup"><span data-stu-id="afb2e-158">Once a new event is created, scroll down toohello output groups and add hello **UDP/TS** output group.</span></span>
3. <span data-ttu-id="afb2e-159">Vytvořit nový výstupní výběrem **nového datového proudu** a pak levým na **přidat výstup**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="afb2e-161">Doporučujeme tuto událost elementární hello má časový kód hello nastavit také "Systémové hodiny" toohelp hello kodér znovu připojit v případě hello selhání datového proudu.</span><span class="sxs-lookup"><span data-stu-id="afb2e-161">It is recommended that hello Elemental event has hello timecode set too"System Clock" toohelp hello encoder reconnect in hello case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="afb2e-162">Teď, když hello výstup byl vytvořen, klikněte na tlačítko **přidat datový proud**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-162">Now that hello Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="afb2e-163">nastavení výstupní Hello se teď dá nakonfigurovat.</span><span class="sxs-lookup"><span data-stu-id="afb2e-163">hello output settings can now be configured.</span></span>
5. <span data-ttu-id="afb2e-164">Posuňte se dolů toohello "Stream 1", kterou jste právě vytvořili, klikněte na tlačítko hello **Video** na levé straně hello a rozbalte hello **Upřesnit** v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="afb2e-164">Scroll down toohello "Stream 1" that was just created, click hello **Video** tab on hello left and expand hello **Advanced** settings section.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="afb2e-166">Zatímco elementární Live obsahuje širokou škálu dostupné přizpůsobení, hello, doporučujeme následující nastavení pro zahájení práce s streamování tooAMS.</span><span class="sxs-lookup"><span data-stu-id="afb2e-166">While Elemental Live has a wide range of available customizing, hello following settings are recommended for getting started with streaming tooAMS.</span></span>

   * <span data-ttu-id="afb2e-167">Řešení: 1280 × 720</span><span class="sxs-lookup"><span data-stu-id="afb2e-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="afb2e-168">Kmitočet snímků: 30</span><span class="sxs-lookup"><span data-stu-id="afb2e-168">Framerate: 30</span></span>
   * <span data-ttu-id="afb2e-169">GOP velikost: 60 rámce</span><span class="sxs-lookup"><span data-stu-id="afb2e-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="afb2e-170">Prokládání režim: progresivní</span><span class="sxs-lookup"><span data-stu-id="afb2e-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="afb2e-171">Přenosovou rychlostí: 5000000 bitů/s (to se dá upravit podle omezení sítě)</span><span class="sxs-lookup"><span data-stu-id="afb2e-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="afb2e-173">Získáte vstupní adresa URL kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-173">Get hello channel's input URL.</span></span>

    <span data-ttu-id="afb2e-174">Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-174">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="afb2e-175">Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="afb2e-175">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="afb2e-176">Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-176">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="afb2e-178">Tyto informace vložte hello **primární cílové** pole z hello Elemental.</span><span class="sxs-lookup"><span data-stu-id="afb2e-178">Paste this information in hello **Primary Destination** field of hello Elemental.</span></span> <span data-ttu-id="afb2e-179">Všechna ostatní nastavení může zůstat výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-179">All other settings can remain hello default.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="afb2e-181">Pro další redundanci opakujte tyto kroky s hello sekundární adresa URL vstupu tak, že vytvoříte samostatné kartě "Výstupní" pro streamování UDP/TS.</span><span class="sxs-lookup"><span data-stu-id="afb2e-181">For extra redundancy, repeat these steps with hello Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="afb2e-182">Klikněte na tlačítko **vytvořit** (Pokud byla vytvořena novou událost) nebo **aktualizace** (Pokud úpravy existující událostí) a poté pokračujte toostart hello kodér.</span><span class="sxs-lookup"><span data-stu-id="afb2e-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed toostart hello encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="afb2e-183">Před kliknutím na **spustit** na hello elementární Live webové rozhraní, můžete **musí** zajistěte, aby byl kanál hello připraven.</span><span class="sxs-lookup"><span data-stu-id="afb2e-183">Before you click **Start** on hello Elemental Live web interface, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="afb2e-184">Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez události po dobu delší než > 15 minut.</span><span class="sxs-lookup"><span data-stu-id="afb2e-184">Also, make sure not tooleave hello Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="afb2e-185">Po spuštění hello datového proudu pro 30 sekund, přejděte zpět toohello AMSE nástroj a testování přehrávání.</span><span class="sxs-lookup"><span data-stu-id="afb2e-185">After hello stream has been running for 30 seconds, navigate back toohello AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="afb2e-186">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="afb2e-186">Test playback</span></span>

<span data-ttu-id="afb2e-187">Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována.</span><span class="sxs-lookup"><span data-stu-id="afb2e-187">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="afb2e-188">V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-188">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="afb2e-189">Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="afb2e-189">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="afb2e-190">Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit.</span><span class="sxs-lookup"><span data-stu-id="afb2e-190">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="afb2e-191">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="afb2e-191">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="afb2e-192">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="afb2e-192">Create a program</span></span>
1. <span data-ttu-id="afb2e-193">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="afb2e-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="afb2e-194">V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-194">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="afb2e-196">Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="afb2e-196">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="afb2e-197">Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="afb2e-197">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="afb2e-198">Zkontrolujte hello **počáteční hello teď Program** pole.</span><span class="sxs-lookup"><span data-stu-id="afb2e-198">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="afb2e-199">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="afb2e-200">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="afb2e-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="afb2e-201">Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="afb2e-201">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="afb2e-202">Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).</span><span class="sxs-lookup"><span data-stu-id="afb2e-202">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="afb2e-203">datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.</span><span class="sxs-lookup"><span data-stu-id="afb2e-203">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="afb2e-204">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="afb2e-204">Troubleshooting</span></span>
<span data-ttu-id="afb2e-205">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="afb2e-205">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="afb2e-206">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="afb2e-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="afb2e-207">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="afb2e-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
