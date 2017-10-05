---
title: "Konfigurace kodér Telestream wirecast odeslat živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak nakonfigurovat za provozu kodér Wirecast k odeslání datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: c4df14f24650ce431dfb31cc774cab6d3cf3aef0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="9832c-103">Pomocí kodéru Wirecast odesílat živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="9832c-103">Use the Wirecast encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9832c-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="9832c-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="9832c-105">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="9832c-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="9832c-106">Čase</span><span class="sxs-lookup"><span data-stu-id="9832c-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="9832c-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="9832c-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="9832c-108">Toto téma ukazuje, jak nakonfigurovat [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) za provozu kodér Odeslat datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="9832c-108">This topic shows how to configure the [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="9832c-109">Další informace najdete v článku o [práci s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="9832c-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="9832c-110">Tento kurz ukazuje, jak spravovat Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="9832c-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="9832c-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="9832c-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="9832c-112">Pokud jste na Mac nebo Linux, použijte portál Azure k vytvoření [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="9832c-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9832c-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9832c-113">Prerequisites</span></span>
* [<span data-ttu-id="9832c-114">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="9832c-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="9832c-115">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="9832c-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="9832c-116">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="9832c-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="9832c-117">Nainstalujte nejnovější verzi [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="9832c-117">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="9832c-118">Spusťte nástroj a připojte se ke svému účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="9832c-118">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="9832c-119">Tipy</span><span class="sxs-lookup"><span data-stu-id="9832c-119">Tips</span></span>
* <span data-ttu-id="9832c-120">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="9832c-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="9832c-121">Obvykle při určování nároky na šířku pásma je dvakrát streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="9832c-121">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="9832c-122">Přestože není povinný požadavek, pomůže omezit účinek zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="9832c-122">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="9832c-123">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="9832c-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="9832c-124">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="9832c-124">Create a channel</span></span>
1. <span data-ttu-id="9832c-125">V nástroj AMSE, přejděte na **živé** kartě a klikněte pravým tlačítkem v oblasti kanálu.</span><span class="sxs-lookup"><span data-stu-id="9832c-125">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="9832c-126">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="9832c-126">Select **Create channel…**</span></span> <span data-ttu-id="9832c-127">v nabídce.</span><span class="sxs-lookup"><span data-stu-id="9832c-127">from the menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="9832c-129">Zadejte název kanálu, pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="9832c-129">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="9832c-130">V části Nastavení kanál, vyberte **standardní** pro Live Encoding možnost s protokolem vstup nastavena na **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="9832c-130">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="9832c-131">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="9832c-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="9832c-132">Zajistěte, aby **nyní spustit nový kanál** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="9832c-132">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="9832c-133">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="9832c-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="9832c-135">Kanál může trvat až 20 minut před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="9832c-135">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="9832c-136">Při spouštění kanál můžete [nakonfigurovat kodér](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="9832c-136">While the channel is starting you can [configure the encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9832c-137">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="9832c-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="9832c-138">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="9832c-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="9832c-139"><a id=configure_wirecast_rtmp></a>Konfigurace kodér Telestream wirecast</span><span class="sxs-lookup"><span data-stu-id="9832c-139"><a id=configure_wirecast_rtmp></a>Configure the Telestream Wirecast encoder</span></span>
<span data-ttu-id="9832c-140">V tomto kurzu se používají následující nastavení výstup.</span><span class="sxs-lookup"><span data-stu-id="9832c-140">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="9832c-141">Zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="9832c-141">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="9832c-142">**Video**:</span><span class="sxs-lookup"><span data-stu-id="9832c-142">**Video**:</span></span>

* <span data-ttu-id="9832c-143">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="9832c-143">Codec: H.264</span></span>
* <span data-ttu-id="9832c-144">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="9832c-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="9832c-145">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="9832c-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="9832c-146">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="9832c-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="9832c-147">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="9832c-147">Frame Rate: 30</span></span>

<span data-ttu-id="9832c-148">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="9832c-148">**Audio**:</span></span>

* <span data-ttu-id="9832c-149">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="9832c-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="9832c-150">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="9832c-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="9832c-151">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="9832c-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="9832c-152">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="9832c-152">Configuration steps</span></span>
1. <span data-ttu-id="9832c-153">Otevřete aplikaci Telestream Wirecast na počítač se používá a nastavte pro streamování RTMP.</span><span class="sxs-lookup"><span data-stu-id="9832c-153">Open the Telestream Wirecast application on the machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="9832c-154">Konfigurace výstup přechodem na **výstup** kartě a výběrem **nastavení výstupní...** .</span><span class="sxs-lookup"><span data-stu-id="9832c-154">Configure the output by navigating to the **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="9832c-155">Zajistěte, aby **cíl výstupu** je nastaven na **RTMP serveru**.</span><span class="sxs-lookup"><span data-stu-id="9832c-155">Make sure the **Output Destination** is set to **RTMP Server**.</span></span>
3. <span data-ttu-id="9832c-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9832c-156">Click **OK**.</span></span>
4. <span data-ttu-id="9832c-157">Na stránce nastavení nastavit **cílové** pole, které chcete být **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="9832c-157">On the settings page, set the **Destination** field to be **Azure Media Services**.</span></span>

    <span data-ttu-id="9832c-158">Profil kódování je předem vybraný k **Azure H.264 720 p 16:9 (1280 × 720)**.</span><span class="sxs-lookup"><span data-stu-id="9832c-158">The Encoding profile is pre-selected to **Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="9832c-159">Pokud chcete toto nastavení upravit, vyberte ikonu ozubené kolečko napravo od rozevíracího dolů a poté zvolte **nové přednastavení**.</span><span class="sxs-lookup"><span data-stu-id="9832c-159">To customize these settings, select the gear icon to the right of the drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="9832c-161">Nakonfigurujte kodér přednastavení.</span><span class="sxs-lookup"><span data-stu-id="9832c-161">Configure encoder presets.</span></span>

    <span data-ttu-id="9832c-162">Název přednastavení a vyhledejte následující doporučené nastavení:</span><span class="sxs-lookup"><span data-stu-id="9832c-162">Name the preset, and check for the following recommended settings:</span></span>

    <span data-ttu-id="9832c-163">**Video**</span><span class="sxs-lookup"><span data-stu-id="9832c-163">**Video**</span></span>

   * <span data-ttu-id="9832c-164">Kodér: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="9832c-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="9832c-165">Počet snímků za sekundu: 30</span><span class="sxs-lookup"><span data-stu-id="9832c-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="9832c-166">Průměrná přenosová rychlost: 5000 kbits za sekundu (lze upravit podle omezení sítě)</span><span class="sxs-lookup"><span data-stu-id="9832c-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="9832c-167">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="9832c-167">Profile: Main</span></span>
   * <span data-ttu-id="9832c-168">Klíče rámce každých: 60 rámce</span><span class="sxs-lookup"><span data-stu-id="9832c-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="9832c-169">**Zvuk**</span><span class="sxs-lookup"><span data-stu-id="9832c-169">**Audio**</span></span>

   * <span data-ttu-id="9832c-170">Cílová přenosová rychlost: 192 kbits za sekundu</span><span class="sxs-lookup"><span data-stu-id="9832c-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="9832c-171">Vzorkovací frekvence: 44 100 kHz</span><span class="sxs-lookup"><span data-stu-id="9832c-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="9832c-173">Stiskněte klávesu **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9832c-173">Press **Save**.</span></span>

    <span data-ttu-id="9832c-174">Pole Encoding má nyní k dispozici pro výběr nově vytvořený profil.</span><span class="sxs-lookup"><span data-stu-id="9832c-174">The Encoding field now has the newly created profile available for selection.</span></span>

    <span data-ttu-id="9832c-175">Ujistěte se, že je vybraný nový profil.</span><span class="sxs-lookup"><span data-stu-id="9832c-175">Make sure the new profile is selected.</span></span>
7. <span data-ttu-id="9832c-176">Get kanál vstup URL aby bylo možné ho přiřadit Wirecast **koncový bod RTMP**.</span><span class="sxs-lookup"><span data-stu-id="9832c-176">Get the channel's input URL in order to assign it to the Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="9832c-177">Přejděte zpět na nástroj AMSE a zkontrolovat stav dokončení kanálu.</span><span class="sxs-lookup"><span data-stu-id="9832c-177">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="9832c-178">Jakmile se stav změnil ze **počáteční** k **systémem**, můžete získat vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="9832c-178">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="9832c-179">Při spuštění je kanál, klikněte pravým tlačítkem na název kanálu, přejděte dolů hover přes **adresa URL vstupu kopírování do schránky** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="9832c-179">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="9832c-181">V Wirecast **nastavení výstupní** okně Vložit tyto informace **adresu** pole výstup oddílu, a přiřadit název datového proudu.</span><span class="sxs-lookup"><span data-stu-id="9832c-181">In the Wirecast **Output Settings** window, paste this information in the **Address** field of the output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="9832c-183">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9832c-183">Select **OK**.</span></span>
2. <span data-ttu-id="9832c-184">V hlavním **Wirecast** zkontrolujte vstupní zdroje pro připravení videa a zvuku a pak klikněte na tlačítko **datového proudu** v levém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="9832c-184">On the main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in the top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="9832c-186">Před kliknutím na **datového proudu**, můžete **musí** Ujistěte se, že kanál je připravený.</span><span class="sxs-lookup"><span data-stu-id="9832c-186">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="9832c-187">Ujistěte se také, nechcete bez vstupní příspěvku kanálu po dobu delší než 15 minut > z kanál ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="9832c-187">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="9832c-188">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="9832c-188">Test playback</span></span>

<span data-ttu-id="9832c-189">Přejděte do nástroj AMSE, a klikněte pravým tlačítkem na kanál, který má být testována.</span><span class="sxs-lookup"><span data-stu-id="9832c-189">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="9832c-190">V nabídce pozastavte ukazatel myši nad **přehrávání ve verzi Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="9832c-190">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="9832c-191">Pokud datový proud se zobrazí v přehrávači, pak kodér správně nakonfigurovaný pro připojení k AMS.</span><span class="sxs-lookup"><span data-stu-id="9832c-191">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="9832c-192">Je-li k chybě, kanál bude nutné resetovat a upravit nastavení kodéru.</span><span class="sxs-lookup"><span data-stu-id="9832c-192">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="9832c-193">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="9832c-193">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="9832c-194">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="9832c-194">Create a program</span></span>
1. <span data-ttu-id="9832c-195">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="9832c-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="9832c-196">V části **živé** v nástroj AMSE, klikněte v oblasti program pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="9832c-196">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="9832c-198">Název programu a v případě potřeby upravit **délka archivačního okna** (výchozí 4 hodiny).</span><span class="sxs-lookup"><span data-stu-id="9832c-198">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="9832c-199">Můžete také určit umístění úložiště nebo ponechte jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="9832c-199">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="9832c-200">Zkontrolujte **nyní spustit Program** pole.</span><span class="sxs-lookup"><span data-stu-id="9832c-200">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="9832c-201">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="9832c-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="9832c-202">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="9832c-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="9832c-203">Jakmile program běží, potvrďte přehrávání tak, že kliknete program pravým tlačítkem a přejdete na **přehrávání programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="9832c-203">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="9832c-204">Po potvrzení, klikněte pravým tlačítkem na program znovu a vyberte **zkopírujte adresu URL výstup do schránky** (nebo načtení těchto informací z **programu informace a nastavení** možnost v nabídce).</span><span class="sxs-lookup"><span data-stu-id="9832c-204">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="9832c-205">Datový proud je nyní připravena vložených v přehrávač, nebo distribuovány do cílovou skupinu pro zobrazení za provozu.</span><span class="sxs-lookup"><span data-stu-id="9832c-205">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="9832c-206">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="9832c-206">Troubleshooting</span></span>
<span data-ttu-id="9832c-207">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="9832c-207">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="9832c-208">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="9832c-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9832c-209">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9832c-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
