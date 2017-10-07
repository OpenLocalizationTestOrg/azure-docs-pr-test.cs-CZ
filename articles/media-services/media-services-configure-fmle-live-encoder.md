---
title: "aaaConfigure hello FMLE kodér toosend živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello toosend kodér rozhraní Flash média Live Encoder (FMLE) jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 780d911c5186ec09c784264f9a0d0c3f8059d305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-fmle-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="3e20f-103">Použít hello FMLE kodér toosend živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="3e20f-103">Use hello FMLE encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e20f-104">FMLE</span><span class="sxs-lookup"><span data-stu-id="3e20f-104">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
> * [<span data-ttu-id="3e20f-105">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="3e20f-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="3e20f-106">Čase</span><span class="sxs-lookup"><span data-stu-id="3e20f-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="3e20f-107">Wirecast</span><span class="sxs-lookup"><span data-stu-id="3e20f-107">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
>
>

<span data-ttu-id="3e20f-108">Toto téma ukazuje, jak tooconfigure hello [Flash Live kodér médií](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) kodér toosend tooAMS kanály, které jsou povolené kódování v reálném čase datového proudu s jednou přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="3e20f-108">This topic shows how tooconfigure hello [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="3e20f-109">Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="3e20f-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="3e20f-110">Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="3e20f-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="3e20f-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="3e20f-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="3e20f-112">Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="3e20f-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

<span data-ttu-id="3e20f-113">Všimněte si, že tento kurz popisuje použití AAC.</span><span class="sxs-lookup"><span data-stu-id="3e20f-113">Note that this tutorial describes using AAC.</span></span> <span data-ttu-id="3e20f-114">Však nepodporuje FMLE podporuje AAC ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e20f-114">However, FMLE doesn’t supports AAC by default.</span></span> <span data-ttu-id="3e20f-115">Potřebovali byste toopurchase modulu plug-in pro kódování AAC například kvůli MainConcept: [AAC modulu plug-in](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span><span class="sxs-lookup"><span data-stu-id="3e20f-115">You would need toopurchase a plugin for AAC encoding such as from MainConcept: [AAC plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e20f-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3e20f-116">Prerequisites</span></span>
* [<span data-ttu-id="3e20f-117">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="3e20f-117">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="3e20f-118">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3e20f-118">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="3e20f-119">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="3e20f-119">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="3e20f-120">Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="3e20f-120">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="3e20f-121">Spusťte nástroj hello a připojte se účet tooyour AMS.</span><span class="sxs-lookup"><span data-stu-id="3e20f-121">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="3e20f-122">Tipy</span><span class="sxs-lookup"><span data-stu-id="3e20f-122">Tips</span></span>
* <span data-ttu-id="3e20f-123">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="3e20f-123">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="3e20f-124">Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="3e20f-124">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="3e20f-125">Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="3e20f-125">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="3e20f-126">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="3e20f-126">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="3e20f-127">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="3e20f-127">Create a channel</span></span>
1. <span data-ttu-id="3e20f-128">Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello.</span><span class="sxs-lookup"><span data-stu-id="3e20f-128">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="3e20f-129">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="3e20f-129">Select **Create channel…**</span></span> <span data-ttu-id="3e20f-130">v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="3e20f-130">from hello menu.</span></span>

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. <span data-ttu-id="3e20f-132">Zadejte název kanálu, hello pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="3e20f-132">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="3e20f-133">V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-133">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="3e20f-134">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="3e20f-134">You can leave all other settings as is.</span></span>

    <span data-ttu-id="3e20f-135">Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="3e20f-135">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="3e20f-136">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-136">Click **Create Channel**.</span></span>

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> <span data-ttu-id="3e20f-138">Hello kanálu může trvat stejně dlouho jako toostart 20 minut.</span><span class="sxs-lookup"><span data-stu-id="3e20f-138">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="3e20f-139">Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span><span class="sxs-lookup"><span data-stu-id="3e20f-139">While hello channel is starting you can [configure hello encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e20f-140">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="3e20f-140">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="3e20f-141">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="3e20f-141">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="3e20f-142"><a id=configure_fmle_rtmp></a>Konfigurace kodér FMLE hello</span><span class="sxs-lookup"><span data-stu-id="3e20f-142"><a id=configure_fmle_rtmp></a>Configure hello FMLE encoder</span></span>
<span data-ttu-id="3e20f-143">V tento kurz hello se používají následující výstup nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e20f-143">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="3e20f-144">Hello zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="3e20f-144">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="3e20f-145">**Video**:</span><span class="sxs-lookup"><span data-stu-id="3e20f-145">**Video**:</span></span>

* <span data-ttu-id="3e20f-146">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="3e20f-146">Codec: H.264</span></span>
* <span data-ttu-id="3e20f-147">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="3e20f-147">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="3e20f-148">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="3e20f-148">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="3e20f-149">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="3e20f-149">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="3e20f-150">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="3e20f-150">Frame Rate: 30</span></span>

<span data-ttu-id="3e20f-151">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="3e20f-151">**Audio**:</span></span>

* <span data-ttu-id="3e20f-152">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="3e20f-152">Codec: AAC (LC)</span></span>
* <span data-ttu-id="3e20f-153">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="3e20f-153">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="3e20f-154">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="3e20f-154">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="3e20f-155">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="3e20f-155">Configuration steps</span></span>
1. <span data-ttu-id="3e20f-156">Přejděte toohello, které rozhraní Flash Live kodér médií na (FMLE) na počítači hello používá.</span><span class="sxs-lookup"><span data-stu-id="3e20f-156">Navigate toohello Flash Media Live Encoder’s (FMLE) interface on hello machine being used.</span></span>

    <span data-ttu-id="3e20f-157">rozhraní Hello je jeden hlavní stránce nastavení.</span><span class="sxs-lookup"><span data-stu-id="3e20f-157">hello interface is one main page of settings.</span></span> <span data-ttu-id="3e20f-158">Věnujte prosím na vědomí následující hello doporučené nastavení tooget začít s streamování využívající FMLE.</span><span class="sxs-lookup"><span data-stu-id="3e20f-158">Please take note of hello following recommended settings tooget started with streaming using FMLE.</span></span>

   * <span data-ttu-id="3e20f-159">Formát: H.264 obnovovací frekvence: 30,00</span><span class="sxs-lookup"><span data-stu-id="3e20f-159">Format: H.264 Frame Rate: 30.00</span></span>
   * <span data-ttu-id="3e20f-160">Vstupní velikost: 1280 × 720</span><span class="sxs-lookup"><span data-stu-id="3e20f-160">Input Size: 1280 x 720</span></span>
   * <span data-ttu-id="3e20f-161">Přenosová rychlost: 5000 kb/s (lze upravit podle omezení sítě)</span><span class="sxs-lookup"><span data-stu-id="3e20f-161">Bit Rate: 5000 Kbps (Can be adjusted based on network limitations)</span></span>  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     <span data-ttu-id="3e20f-163">Při použití prokládaných zdroje, prosím hello zaškrtnout možnost "Volby Odstranit prokládání"</span><span class="sxs-lookup"><span data-stu-id="3e20f-163">When using interlaced sources, please checkmark hello “Deinterlace” option</span></span>
2. <span data-ttu-id="3e20f-164">Vyberte hello klíče ikonu další tooFormat, tyto další nastavení by mělo být:</span><span class="sxs-lookup"><span data-stu-id="3e20f-164">Select hello wrench icon next tooFormat, these additional settings should be:</span></span>

   * <span data-ttu-id="3e20f-165">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="3e20f-165">Profile: Main</span></span>
   * <span data-ttu-id="3e20f-166">Úroveň: 4.0</span><span class="sxs-lookup"><span data-stu-id="3e20f-166">Level: 4.0</span></span>
   * <span data-ttu-id="3e20f-167">Frekvence @keyframe, které určuje: 2 sekund</span><span class="sxs-lookup"><span data-stu-id="3e20f-167">Keyframe Frequency: 2 seconds</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. <span data-ttu-id="3e20f-169">Nastavte hello následující důležité zvuk nastavení:</span><span class="sxs-lookup"><span data-stu-id="3e20f-169">Set hello following important audio setting:</span></span>

   * <span data-ttu-id="3e20f-170">Formát: AAC</span><span class="sxs-lookup"><span data-stu-id="3e20f-170">Format: AAC</span></span>
   * <span data-ttu-id="3e20f-171">Vzorkovací frekvence: 44100 Hz</span><span class="sxs-lookup"><span data-stu-id="3e20f-171">Sample Rate: 44100 Hz</span></span>
   * <span data-ttu-id="3e20f-172">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="3e20f-172">Bitrate: 192 Kbps</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. <span data-ttu-id="3e20f-174">Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello FMLE na **koncový bod RTMP**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-174">Get hello channel's input URL in order tooassign it toohello FMLE's **RTMP Endpoint**.</span></span>

    <span data-ttu-id="3e20f-175">Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="3e20f-175">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="3e20f-176">Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3e20f-176">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="3e20f-177">Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-177">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. <span data-ttu-id="3e20f-179">Tyto informace vložte hello **FMS URL** pole části výstup hello a přiřadit název datového proudu.</span><span class="sxs-lookup"><span data-stu-id="3e20f-179">Paste this information in hello **FMS URL** field of hello output section, and assign a stream name.</span></span>

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    <span data-ttu-id="3e20f-181">Pro další redundanci opakujte tyto kroky s hello sekundární adresa URL vstupu.</span><span class="sxs-lookup"><span data-stu-id="3e20f-181">For extra redundancy, repeat these steps with hello Secondary Input URL.</span></span>
6. <span data-ttu-id="3e20f-182">Vyberte **Connect** (Připojit).</span><span class="sxs-lookup"><span data-stu-id="3e20f-182">Select **Connect**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e20f-183">Před kliknutím na **připojit**, můžete **musí** zajistěte, aby byl kanál hello připraven.</span><span class="sxs-lookup"><span data-stu-id="3e20f-183">Before you click **Connect**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="3e20f-184">Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.</span><span class="sxs-lookup"><span data-stu-id="3e20f-184">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="3e20f-185">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="3e20f-185">Test playback</span></span>

<span data-ttu-id="3e20f-186">Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována.</span><span class="sxs-lookup"><span data-stu-id="3e20f-186">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="3e20f-187">V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-187">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

<span data-ttu-id="3e20f-188">Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="3e20f-188">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="3e20f-189">Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit.</span><span class="sxs-lookup"><span data-stu-id="3e20f-189">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="3e20f-190">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="3e20f-190">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="3e20f-191">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="3e20f-191">Create a program</span></span>
1. <span data-ttu-id="3e20f-192">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="3e20f-192">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="3e20f-193">V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-193">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. <span data-ttu-id="3e20f-195">Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="3e20f-195">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="3e20f-196">Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="3e20f-196">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="3e20f-197">Zkontrolujte hello **počáteční hello teď Program** pole.</span><span class="sxs-lookup"><span data-stu-id="3e20f-197">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="3e20f-198">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-198">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="3e20f-199">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="3e20f-199">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="3e20f-200">Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="3e20f-200">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="3e20f-201">Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).</span><span class="sxs-lookup"><span data-stu-id="3e20f-201">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="3e20f-202">datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3e20f-202">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="3e20f-203">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="3e20f-203">Troubleshooting</span></span>
<span data-ttu-id="3e20f-204">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="3e20f-204">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="3e20f-205">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="3e20f-205">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3e20f-206">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="3e20f-206">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
