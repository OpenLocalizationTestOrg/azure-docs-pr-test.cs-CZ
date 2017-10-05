---
title: "Konfigurace kodér elementární za provozu na odesílat živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak nakonfigurovat kodér elementární za provozu k odeslání datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase."
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
ms.openlocfilehash: 668a3ab46a70c0ee25fa87031d27c0f4333ec89c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="41035-103">Pomocí kodéru elementární Live odesílat živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="41035-103">Use the Elemental Live encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="41035-104">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="41035-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="41035-105">Čase</span><span class="sxs-lookup"><span data-stu-id="41035-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="41035-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="41035-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="41035-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="41035-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="41035-108">Toto téma ukazuje, jak nakonfigurovat [elementární Live](http://www.elementaltechnologies.com/products/elemental-live) ke odesílat datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="41035-108">This topic shows how to configure the [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="41035-109">Další informace najdete v článku o [práci s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="41035-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="41035-110">Tento kurz ukazuje, jak spravovat Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="41035-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="41035-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="41035-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="41035-112">Pokud jste na Mac nebo Linux, použijte portál Azure k vytvoření [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="41035-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41035-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="41035-113">Prerequisites</span></span>
* <span data-ttu-id="41035-114">Musí mít praktické znalosti použití elementární Live webové rozhraní pro vytváření živé události.</span><span class="sxs-lookup"><span data-stu-id="41035-114">Must have a working knowledge of using Elemental Live web interface to create live events.</span></span>
* [<span data-ttu-id="41035-115">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="41035-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="41035-116">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="41035-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="41035-117">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="41035-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="41035-118">Nainstalujte nejnovější verzi [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="41035-118">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="41035-119">Spusťte nástroj a připojte se ke svému účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="41035-119">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="41035-120">Tipy</span><span class="sxs-lookup"><span data-stu-id="41035-120">Tips</span></span>
* <span data-ttu-id="41035-121">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="41035-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="41035-122">Obvykle při určování nároky na šířku pásma je dvakrát streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="41035-122">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="41035-123">Přestože není povinný požadavek, pomůže omezit účinek zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="41035-123">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="41035-124">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="41035-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="41035-125">Ingestování elementární živé s RTP</span><span class="sxs-lookup"><span data-stu-id="41035-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="41035-126">V této části ukazuje, jak nakonfigurovat kodéru elementární za provozu, který odešle datový proud s jednou přenosovou rychlostí za provozu přes protokol RTP.</span><span class="sxs-lookup"><span data-stu-id="41035-126">This section shows how to configure the Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="41035-127">Další informace najdete v tématu [stream MPEG-TS využívající RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="41035-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="41035-128">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="41035-128">Create a channel</span></span>

1. <span data-ttu-id="41035-129">V nástroj AMSE, přejděte na **živé** kartě a klikněte pravým tlačítkem v oblasti kanálu.</span><span class="sxs-lookup"><span data-stu-id="41035-129">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="41035-130">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="41035-130">Select **Create channel…**</span></span> <span data-ttu-id="41035-131">v nabídce.</span><span class="sxs-lookup"><span data-stu-id="41035-131">from the menu.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="41035-133">Zadejte název kanálu, pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="41035-133">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="41035-134">V části Nastavení kanál, vyberte **standardní** pro Live Encoding možnost s protokolem vstup nastavena na **RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="41035-134">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTP (MPEG-TS)**.</span></span> <span data-ttu-id="41035-135">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="41035-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="41035-136">Zajistěte, aby **nyní spustit nový kanál** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="41035-136">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="41035-137">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="41035-137">Click **Create Channel**.</span></span>

   ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="41035-139">Kanál může trvat až 20 minut před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="41035-139">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="41035-140">Při spouštění kanál můžete [nakonfigurovat kodér](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="41035-140">While the channel is starting you can [configure the encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41035-141">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="41035-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="41035-142">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="41035-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="41035-143"><a id=configure_elemental_rtp></a>Konfigurace kodér elementární za provozu</span><span class="sxs-lookup"><span data-stu-id="41035-143"><a id=configure_elemental_rtp></a>Configure the Elemental Live encoder</span></span>
<span data-ttu-id="41035-144">V tomto kurzu se používají následující nastavení výstup.</span><span class="sxs-lookup"><span data-stu-id="41035-144">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="41035-145">Zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="41035-145">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="41035-146">**Video**:</span><span class="sxs-lookup"><span data-stu-id="41035-146">**Video**:</span></span>

* <span data-ttu-id="41035-147">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="41035-147">Codec: H.264</span></span>
* <span data-ttu-id="41035-148">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="41035-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="41035-149">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="41035-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="41035-150">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="41035-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="41035-151">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="41035-151">Frame Rate: 30</span></span>

<span data-ttu-id="41035-152">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="41035-152">**Audio**:</span></span>

* <span data-ttu-id="41035-153">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="41035-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="41035-154">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="41035-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="41035-155">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="41035-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="41035-156">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="41035-156">Configuration steps</span></span>
1. <span data-ttu-id="41035-157">Přejděte na **elementární Live** webové rozhraní a nastavení kodéru pro **UDP/TS** streamování.</span><span class="sxs-lookup"><span data-stu-id="41035-157">Navigate to the **Elemental Live** web interface, and set up the encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="41035-158">Jakmile dojde k vytvoření nové události, posuňte se dolů a výstup skupiny a přidat **UDP/TS** skupiny výstupu.</span><span class="sxs-lookup"><span data-stu-id="41035-158">Once a new event is created, scroll down to the output groups and add the **UDP/TS** output group.</span></span>
3. <span data-ttu-id="41035-159">Vytvořit nový výstupní výběrem **nového datového proudu** a pak levým na **přidat výstup**.</span><span class="sxs-lookup"><span data-stu-id="41035-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="41035-161">Doporučuje se, že elementární událost má záznamy typu časového kódu nastavit na "Systémové hodiny" pomohou kodér znovu připojit v případě selhání datového proudu.</span><span class="sxs-lookup"><span data-stu-id="41035-161">It is recommended that the Elemental event has the timecode set to "System Clock" to help the encoder reconnect in the case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="41035-162">Teď, když byla vytvořena výstup, klikněte na tlačítko **přidat datový proud**.</span><span class="sxs-lookup"><span data-stu-id="41035-162">Now that the Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="41035-163">Můžete teď konfigurovat nastavení výstup.</span><span class="sxs-lookup"><span data-stu-id="41035-163">The output settings can now be configured.</span></span>
5. <span data-ttu-id="41035-164">Posuňte se dolů "Datový proud 1" kterou jste právě vytvořili, klikněte **Video** na levé straně a rozbalte **Upřesnit** v oddílu nastavení.</span><span class="sxs-lookup"><span data-stu-id="41035-164">Scroll down to the "Stream 1" that was just created, click the **Video** tab on the left and expand the **Advanced** settings section.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="41035-166">Zatímco elementární Live obsahuje širokou škálu dostupné přizpůsobení, doporučujeme následující nastavení pro zahájení práce s streamování AMS.</span><span class="sxs-lookup"><span data-stu-id="41035-166">While Elemental Live has a wide range of available customizing, the following settings are recommended for getting started with streaming to AMS.</span></span>

   * <span data-ttu-id="41035-167">Řešení: 1280 × 720</span><span class="sxs-lookup"><span data-stu-id="41035-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="41035-168">Kmitočet snímků: 30</span><span class="sxs-lookup"><span data-stu-id="41035-168">Framerate: 30</span></span>
   * <span data-ttu-id="41035-169">GOP velikost: 60 rámce</span><span class="sxs-lookup"><span data-stu-id="41035-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="41035-170">Prokládání režim: progresivní</span><span class="sxs-lookup"><span data-stu-id="41035-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="41035-171">Přenosovou rychlostí: 5000000 bitů/s (to se dá upravit podle omezení sítě)</span><span class="sxs-lookup"><span data-stu-id="41035-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="41035-173">Získáte vstupní adresa URL kanálu.</span><span class="sxs-lookup"><span data-stu-id="41035-173">Get the channel's input URL.</span></span>

    <span data-ttu-id="41035-174">Přejděte zpět na nástroj AMSE a zkontrolovat stav dokončení kanálu.</span><span class="sxs-lookup"><span data-stu-id="41035-174">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="41035-175">Jakmile se stav změnil ze **počáteční** k **systémem**, můžete získat vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="41035-175">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="41035-176">Při spuštění je kanál, klikněte pravým tlačítkem na název kanálu, přejděte dolů hover přes **adresa URL vstupu kopírování do schránky** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="41035-176">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="41035-178">Tyto informace v vložit **primární cílové** pole z elementární.</span><span class="sxs-lookup"><span data-stu-id="41035-178">Paste this information in the **Primary Destination** field of the Elemental.</span></span> <span data-ttu-id="41035-179">Všechna ostatní nastavení může zůstat výchozí.</span><span class="sxs-lookup"><span data-stu-id="41035-179">All other settings can remain the default.</span></span>

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="41035-181">Pro další redundanci opakujte tyto kroky s adresou URL sekundární vstup tak, že vytvoříte samostatné kartě "Výstupní" pro streamování UDP/TS.</span><span class="sxs-lookup"><span data-stu-id="41035-181">For extra redundancy, repeat these steps with the Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="41035-182">Klikněte na tlačítko **vytvořit** (Pokud byla vytvořena novou událost) nebo **aktualizace** (Pokud úpravy existující událostí) a pak pokračujte spustit kodér.</span><span class="sxs-lookup"><span data-stu-id="41035-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed to start the encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="41035-183">Před kliknutím na **spustit** na webové rozhraní elementární za provozu je **musí** Ujistěte se, že kanál je připravený.</span><span class="sxs-lookup"><span data-stu-id="41035-183">Before you click **Start** on the Elemental Live web interface, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="41035-184">Ujistěte se také, nechcete ponechat kanál ve stavu Připraveno bez události po dobu delší než > 15 minut.</span><span class="sxs-lookup"><span data-stu-id="41035-184">Also, make sure not to leave the Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="41035-185">Po spuštění datového proudu pro 30 sekund, přejděte zpět na AMSE nástroj a testování přehrávání.</span><span class="sxs-lookup"><span data-stu-id="41035-185">After the stream has been running for 30 seconds, navigate back to the AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="41035-186">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="41035-186">Test playback</span></span>

<span data-ttu-id="41035-187">Přejděte do nástroj AMSE, a klikněte pravým tlačítkem na kanál, který má být testována.</span><span class="sxs-lookup"><span data-stu-id="41035-187">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="41035-188">V nabídce pozastavte ukazatel myši nad **přehrávání ve verzi Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="41035-188">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="41035-189">Pokud datový proud se zobrazí v přehrávači, pak kodér správně nakonfigurovaný pro připojení k AMS.</span><span class="sxs-lookup"><span data-stu-id="41035-189">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="41035-190">Je-li k chybě, kanál bude nutné resetovat a upravit nastavení kodéru.</span><span class="sxs-lookup"><span data-stu-id="41035-190">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="41035-191">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="41035-191">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="41035-192">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="41035-192">Create a program</span></span>
1. <span data-ttu-id="41035-193">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="41035-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="41035-194">V části **živé** v nástroj AMSE, klikněte v oblasti program pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="41035-194">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![Elementární](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="41035-196">Název programu a v případě potřeby upravit **délka archivačního okna** (výchozí 4 hodiny).</span><span class="sxs-lookup"><span data-stu-id="41035-196">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="41035-197">Můžete také určit umístění úložiště nebo ponechte jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="41035-197">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="41035-198">Zkontrolujte **nyní spustit Program** pole.</span><span class="sxs-lookup"><span data-stu-id="41035-198">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="41035-199">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="41035-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="41035-200">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="41035-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="41035-201">Jakmile program běží, potvrďte přehrávání tak, že kliknete program pravým tlačítkem a přejdete na **přehrávání programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="41035-201">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="41035-202">Po potvrzení, klikněte pravým tlačítkem na program znovu a vyberte **zkopírujte adresu URL výstup do schránky** (nebo načtení těchto informací z **programu informace a nastavení** možnost v nabídce).</span><span class="sxs-lookup"><span data-stu-id="41035-202">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="41035-203">Datový proud je nyní připravena vložených v přehrávač, nebo distribuovány do cílovou skupinu pro zobrazení za provozu.</span><span class="sxs-lookup"><span data-stu-id="41035-203">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="41035-204">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="41035-204">Troubleshooting</span></span>
<span data-ttu-id="41035-205">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="41035-205">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="41035-206">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="41035-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="41035-207">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="41035-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
