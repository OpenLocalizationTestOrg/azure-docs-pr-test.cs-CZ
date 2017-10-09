---
title: "aaaConfigure hello toosend kodér Telestream Wirecast živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello Wirecast live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase. "
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
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="fef38-103">Použít hello Wirecast kodér toosend živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="fef38-103">Use hello Wirecast encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fef38-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="fef38-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="fef38-105">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="fef38-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="fef38-106">Čase</span><span class="sxs-lookup"><span data-stu-id="fef38-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="fef38-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="fef38-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="fef38-108">Toto téma ukazuje, jak tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="fef38-108">This topic shows how tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="fef38-109">Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="fef38-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="fef38-110">Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="fef38-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="fef38-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="fef38-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="fef38-112">Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="fef38-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fef38-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fef38-113">Prerequisites</span></span>
* [<span data-ttu-id="fef38-114">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="fef38-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="fef38-115">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="fef38-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="fef38-116">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="fef38-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="fef38-117">Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="fef38-117">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="fef38-118">Spusťte nástroj hello a připojte se účet tooyour AMS.</span><span class="sxs-lookup"><span data-stu-id="fef38-118">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="fef38-119">Tipy</span><span class="sxs-lookup"><span data-stu-id="fef38-119">Tips</span></span>
* <span data-ttu-id="fef38-120">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="fef38-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="fef38-121">Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="fef38-121">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="fef38-122">Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="fef38-122">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="fef38-123">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="fef38-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="fef38-124">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="fef38-124">Create a channel</span></span>
1. <span data-ttu-id="fef38-125">Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-125">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="fef38-126">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="fef38-126">Select **Create channel…**</span></span> <span data-ttu-id="fef38-127">v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-127">from hello menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="fef38-129">Zadejte název kanálu, hello pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="fef38-129">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="fef38-130">V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="fef38-130">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="fef38-131">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="fef38-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="fef38-132">Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="fef38-132">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="fef38-133">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="fef38-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="fef38-135">Hello kanálu může trvat stejně dlouho jako toostart 20 minut.</span><span class="sxs-lookup"><span data-stu-id="fef38-135">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="fef38-136">Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="fef38-136">While hello channel is starting you can [configure hello encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fef38-137">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="fef38-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="fef38-138">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="fef38-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="fef38-139"><a id=configure_wirecast_rtmp></a>Konfigurace hello kodér Telestream wirecast</span><span class="sxs-lookup"><span data-stu-id="fef38-139"><a id=configure_wirecast_rtmp></a>Configure hello Telestream Wirecast encoder</span></span>
<span data-ttu-id="fef38-140">V tento kurz hello se používají následující výstup nastavení.</span><span class="sxs-lookup"><span data-stu-id="fef38-140">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="fef38-141">Hello zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="fef38-141">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="fef38-142">**Video**:</span><span class="sxs-lookup"><span data-stu-id="fef38-142">**Video**:</span></span>

* <span data-ttu-id="fef38-143">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="fef38-143">Codec: H.264</span></span>
* <span data-ttu-id="fef38-144">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="fef38-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="fef38-145">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="fef38-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="fef38-146">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="fef38-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="fef38-147">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="fef38-147">Frame Rate: 30</span></span>

<span data-ttu-id="fef38-148">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="fef38-148">**Audio**:</span></span>

* <span data-ttu-id="fef38-149">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="fef38-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="fef38-150">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="fef38-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="fef38-151">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="fef38-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="fef38-152">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="fef38-152">Configuration steps</span></span>
1. <span data-ttu-id="fef38-153">Otevřete hello Telestream Wirecast aplikace na hello počítač se používá a nastavte pro streamování RTMP.</span><span class="sxs-lookup"><span data-stu-id="fef38-153">Open hello Telestream Wirecast application on hello machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="fef38-154">Výstup hello nakonfigurovat tak, že přejdete toohello **výstup** kartě a výběrem **nastavení výstupní...** .</span><span class="sxs-lookup"><span data-stu-id="fef38-154">Configure hello output by navigating toohello **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="fef38-155">Ujistěte se, zda text hello **cíl výstupu** je nastaven příliš**RTMP serveru**.</span><span class="sxs-lookup"><span data-stu-id="fef38-155">Make sure hello **Output Destination** is set too**RTMP Server**.</span></span>
3. <span data-ttu-id="fef38-156">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fef38-156">Click **OK**.</span></span>
4. <span data-ttu-id="fef38-157">Na stránce nastavení hello nastavit hello **cílové** pole toobe **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="fef38-157">On hello settings page, set hello **Destination** field toobe **Azure Media Services**.</span></span>

    <span data-ttu-id="fef38-158">Hello kódování profilu je předem vybraná příliš**Azure H.264 720 p 16:9 (1280 × 720)**.</span><span class="sxs-lookup"><span data-stu-id="fef38-158">hello Encoding profile is pre-selected too**Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="fef38-159">toocustomize tato nastavení, vyberte hello ozubené kolečko ikonu toohello napravo od hello rozevírací nabídku a pak zvolte **nové přednastavení**.</span><span class="sxs-lookup"><span data-stu-id="fef38-159">toocustomize these settings, select hello gear icon toohello right of hello drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="fef38-161">Nakonfigurujte kodér přednastavení.</span><span class="sxs-lookup"><span data-stu-id="fef38-161">Configure encoder presets.</span></span>

    <span data-ttu-id="fef38-162">Název hello přednastavení a vyhledejte hello následující doporučené nastavení:</span><span class="sxs-lookup"><span data-stu-id="fef38-162">Name hello preset, and check for hello following recommended settings:</span></span>

    <span data-ttu-id="fef38-163">**Video**</span><span class="sxs-lookup"><span data-stu-id="fef38-163">**Video**</span></span>

   * <span data-ttu-id="fef38-164">Kodér: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="fef38-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="fef38-165">Počet snímků za sekundu: 30</span><span class="sxs-lookup"><span data-stu-id="fef38-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="fef38-166">Průměrná přenosová rychlost: 5000 kbits za sekundu (lze upravit podle omezení sítě)</span><span class="sxs-lookup"><span data-stu-id="fef38-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="fef38-167">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="fef38-167">Profile: Main</span></span>
   * <span data-ttu-id="fef38-168">Klíče rámce každých: 60 rámce</span><span class="sxs-lookup"><span data-stu-id="fef38-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="fef38-169">**Zvuk**</span><span class="sxs-lookup"><span data-stu-id="fef38-169">**Audio**</span></span>

   * <span data-ttu-id="fef38-170">Cílová přenosová rychlost: 192 kbits za sekundu</span><span class="sxs-lookup"><span data-stu-id="fef38-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="fef38-171">Vzorkovací frekvence: 44 100 kHz</span><span class="sxs-lookup"><span data-stu-id="fef38-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="fef38-173">Stiskněte klávesu **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fef38-173">Press **Save**.</span></span>

    <span data-ttu-id="fef38-174">pole Encoding Hello má nyní k dispozici pro výběr hello nově vytvořený profil.</span><span class="sxs-lookup"><span data-stu-id="fef38-174">hello Encoding field now has hello newly created profile available for selection.</span></span>

    <span data-ttu-id="fef38-175">Ujistěte se, že je vybraný nový profil hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-175">Make sure hello new profile is selected.</span></span>
7. <span data-ttu-id="fef38-176">Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello Wirecast **koncový bod RTMP**.</span><span class="sxs-lookup"><span data-stu-id="fef38-176">Get hello channel's input URL in order tooassign it toohello Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="fef38-177">Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-177">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="fef38-178">Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="fef38-178">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="fef38-179">Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="fef38-179">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="fef38-181">V hello Wirecast **nastavení výstupní** okno, vložte tyto informace hello **adresu** pole části výstup hello a přiřadit název datového proudu.</span><span class="sxs-lookup"><span data-stu-id="fef38-181">In hello Wirecast **Output Settings** window, paste this information in hello **Address** field of hello output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="fef38-183">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="fef38-183">Select **OK**.</span></span>
2. <span data-ttu-id="fef38-184">Na hlavní hello **Wirecast** zkontrolujte vstupní zdroje pro připravení videa a zvuku a pak klikněte na tlačítko **datového proudu** v levém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-184">On hello main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in hello top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="fef38-186">Před kliknutím na **datového proudu**, můžete **musí** zajistěte, aby byl kanál hello připraven.</span><span class="sxs-lookup"><span data-stu-id="fef38-186">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="fef38-187">Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.</span><span class="sxs-lookup"><span data-stu-id="fef38-187">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="fef38-188">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="fef38-188">Test playback</span></span>

<span data-ttu-id="fef38-189">Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována.</span><span class="sxs-lookup"><span data-stu-id="fef38-189">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="fef38-190">V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="fef38-190">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="fef38-191">Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="fef38-191">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="fef38-192">Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit.</span><span class="sxs-lookup"><span data-stu-id="fef38-192">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="fef38-193">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="fef38-193">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="fef38-194">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="fef38-194">Create a program</span></span>
1. <span data-ttu-id="fef38-195">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="fef38-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="fef38-196">V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="fef38-196">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="fef38-198">Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="fef38-198">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="fef38-199">Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="fef38-199">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="fef38-200">Zkontrolujte hello **počáteční hello teď Program** pole.</span><span class="sxs-lookup"><span data-stu-id="fef38-200">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="fef38-201">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="fef38-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="fef38-202">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="fef38-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="fef38-203">Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="fef38-203">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="fef38-204">Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).</span><span class="sxs-lookup"><span data-stu-id="fef38-204">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="fef38-205">datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.</span><span class="sxs-lookup"><span data-stu-id="fef38-205">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="fef38-206">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="fef38-206">Troubleshooting</span></span>
<span data-ttu-id="fef38-207">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="fef38-207">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="fef38-208">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="fef38-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fef38-209">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="fef38-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
