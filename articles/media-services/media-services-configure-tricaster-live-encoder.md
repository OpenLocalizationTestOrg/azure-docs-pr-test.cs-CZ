---
title: "Konfigurace kodér NewTek čase k odesílání živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak nakonfigurovat za provozu kodér čase k odesílání datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 42b012fb98bd0504c931ce391d63aecca8c3d311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="2a4fd-103">Pomocí kodéru NewTek čase odesílat živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="2a4fd-103">Use the NewTek TriCaster encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a4fd-104">Čase</span><span class="sxs-lookup"><span data-stu-id="2a4fd-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="2a4fd-105">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="2a4fd-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="2a4fd-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="2a4fd-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="2a4fd-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="2a4fd-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="2a4fd-108">Toto téma ukazuje, jak nakonfigurovat [čase NewTek](http://newtek.com/products/tricaster-40.html) za provozu kodér Odeslat datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-108">This topic shows how to configure the [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="2a4fd-109">Další informace najdete v článku o [práci s kanály, které mají povolené kódování v reálném čase pomocí služby Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="2a4fd-110">Tento kurz ukazuje, jak spravovat Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="2a4fd-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="2a4fd-112">Pokud jste na Mac nebo Linux, použijte portál Azure k vytvoření [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2a4fd-113">Při použití čase pro odesílání v příspěvek informační kanál AMS kanály, které jsou povolené kódování v reálném čase, může být video nebo zvuk chyb v živé události Pokud určité funkce čase, jako je rychlé vyjímání mezi informační kanály nebo přepnutí z slaty.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-113">When using Tricaster for sending in a contribution feed to AMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="2a4fd-114">AMS tým pracuje na řešení těchto problémů do té doby, ho není doporučujeme používat tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-114">The AMS team is working on fixing these issues, until then, it is not recommend to use these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="2a4fd-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a4fd-115">Prerequisites</span></span>
* [<span data-ttu-id="2a4fd-116">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="2a4fd-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="2a4fd-117">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="2a4fd-118">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="2a4fd-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="2a4fd-119">Nainstalujte nejnovější verzi [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-119">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="2a4fd-120">Spusťte nástroj a připojte se ke svému účtu AMS.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-120">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="2a4fd-121">Tipy</span><span class="sxs-lookup"><span data-stu-id="2a4fd-121">Tips</span></span>
* <span data-ttu-id="2a4fd-122">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="2a4fd-123">Obvykle při určování nároky na šířku pásma je dvakrát streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-123">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="2a4fd-124">Přestože není povinný požadavek, pomůže omezit účinek zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-124">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="2a4fd-125">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="2a4fd-126">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="2a4fd-126">Create a channel</span></span>
1. <span data-ttu-id="2a4fd-127">V nástroj AMSE, přejděte na **živé** kartě a klikněte pravým tlačítkem v oblasti kanálu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-127">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="2a4fd-128">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="2a4fd-128">Select **Create channel…**</span></span> <span data-ttu-id="2a4fd-129">v nabídce.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-129">from the menu.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="2a4fd-131">Zadejte název kanálu, pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-131">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="2a4fd-132">V části Nastavení kanál, vyberte **standardní** pro Live Encoding možnost s protokolem vstup nastavena na **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-132">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="2a4fd-133">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="2a4fd-134">Zajistěte, aby **nyní spustit nový kanál** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-134">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="2a4fd-135">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-135">Click **Create Channel**.</span></span>

   ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="2a4fd-137">Kanál může trvat až 20 minut před spuštěním.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-137">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="2a4fd-138">Při spouštění kanál můžete [nakonfigurovat kodér](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-138">While the channel is starting you can [configure the encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2a4fd-139">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="2a4fd-140">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="2a4fd-141"><a id=configure_tricaster_rtmp></a>Konfigurace kodéru NewTek čase</span><span class="sxs-lookup"><span data-stu-id="2a4fd-141"><a id=configure_tricaster_rtmp></a>Configure the NewTek TriCaster encoder</span></span>
<span data-ttu-id="2a4fd-142">V tomto kurzu se používají následující nastavení výstup.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-142">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="2a4fd-143">Zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-143">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="2a4fd-144">**Video**:</span><span class="sxs-lookup"><span data-stu-id="2a4fd-144">**Video**:</span></span>

* <span data-ttu-id="2a4fd-145">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="2a4fd-145">Codec: H.264</span></span>
* <span data-ttu-id="2a4fd-146">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="2a4fd-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="2a4fd-147">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="2a4fd-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="2a4fd-148">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="2a4fd-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="2a4fd-149">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="2a4fd-149">Frame Rate: 30</span></span>

<span data-ttu-id="2a4fd-150">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="2a4fd-150">**Audio**:</span></span>

* <span data-ttu-id="2a4fd-151">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="2a4fd-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="2a4fd-152">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="2a4fd-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="2a4fd-153">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="2a4fd-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="2a4fd-154">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="2a4fd-154">Configuration steps</span></span>
1. <span data-ttu-id="2a4fd-155">Vytvořte novou **čase NewTek** projektu v závislosti na tom, jaké vstupní zdroj videa se používá.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="2a4fd-156">Jednou v rámci projektu, Najít **datového proudu** tlačítko a klikněte na ikonu ozubené kolečko vedle sebe pro přístup k nabídce konfiguraci datového proudu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-156">Once within that project, find the **Stream** button, and click the gear icon next to it to access the stream configuration menu.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="2a4fd-158">Jakmile nabídce otevře, klikněte na tlačítko **nový** v části připojení.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-158">Once the menu has opened, click **New** under the Connection heading.</span></span> <span data-ttu-id="2a4fd-159">Po zobrazení výzvy pro typ připojení, vyberte **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-159">When prompted for the connection type, select **Adobe Flash**.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="2a4fd-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-161">Click **OK**.</span></span>
5. <span data-ttu-id="2a4fd-162">Kliknutím šipku rozevíracího seznamu v části lze nyní importovat profilem FMLE **streamování profil** a přejdete na **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-162">An FMLE profile can now be imported by clicking the drop down arrow under **Streaming Profile** and navigating to **Browse**.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="2a4fd-164">Přejděte k uložení nakonfigurované FMLE profilu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-164">Navigate to where the configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="2a4fd-165">Vyberte ji a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="2a4fd-166">Po nahrání profilu pokračujte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-166">Once the profile is uploaded, proceed to the next step.</span></span>
8. <span data-ttu-id="2a4fd-167">Get kanál vstup URL aby bylo možné ho přiřadit čase **koncový bod RTMP**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-167">Get the channel's input URL in order to assign it to the Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="2a4fd-168">Přejděte zpět na nástroj AMSE a zkontrolovat stav dokončení kanálu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-168">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="2a4fd-169">Jakmile se stav změnil ze **počáteční** k **systémem**, můžete získat vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-169">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="2a4fd-170">Při spuštění je kanál, klikněte pravým tlačítkem na název kanálu, přejděte dolů hover přes **adresa URL vstupu kopírování do schránky** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-170">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="2a4fd-172">Tyto informace v vložit **umístění** pole v části **serveru Flash** v čase projektu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-172">Paste this information in the **Location** field under **Flash Server** within the Tricaster project.</span></span> <span data-ttu-id="2a4fd-173">Název datového proudu v přiřadit také **ID datového proudu** pole.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-173">Also assign a stream name in the **Stream ID** field.</span></span>

    <span data-ttu-id="2a4fd-174">Pokud informace datový proud byl přidán do profilu FMLE, se můžete také naimportovat do této části kliknutím **importovat nastavení**, přejdete na uloženého profilu FMLE a kliknutím na **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-174">If stream information was added to the FMLE profile, it can also be imported to this section by clicking **Import Settings**, navigating to the saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="2a4fd-175">Příslušná pole Flash serveru by měl naplnění informací z FMLE.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-175">The relevant Flash Server fields should populate with the information from FMLE.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="2a4fd-177">Po dokončení klikněte na tlačítko **OK** v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-177">When finished, click **OK** at the bottom of the screen.</span></span> <span data-ttu-id="2a4fd-178">Jakmile jsou připravené videosoubory a zvukové vstupy do čase, začněte streamování AMS kliknutím **datového proudu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-178">When video and audio inputs into the Tricaster are ready, begin streaming to AMS by clicking the **Stream** button.</span></span>

     ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="2a4fd-180">Před kliknutím na **datového proudu**, můžete **musí** Ujistěte se, že kanál je připravený.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-180">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="2a4fd-181">Ujistěte se také, nechcete bez vstupní příspěvku kanálu po dobu delší než 15 minut > z kanál ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-181">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="2a4fd-182">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="2a4fd-182">Test playback</span></span>
<span data-ttu-id="2a4fd-183">Přejděte do nástroj AMSE, a klikněte pravým tlačítkem na kanál, který má být testována.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-183">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="2a4fd-184">V nabídce pozastavte ukazatel myši nad **přehrávání ve verzi Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-184">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="2a4fd-185">Pokud datový proud se zobrazí v přehrávači, pak kodér správně nakonfigurovaný pro připojení k AMS.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-185">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="2a4fd-186">Je-li k chybě, kanál bude nutné resetovat a upravit nastavení kodéru.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-186">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="2a4fd-187">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-187">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="2a4fd-188">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="2a4fd-188">Create a program</span></span>
1. <span data-ttu-id="2a4fd-189">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="2a4fd-190">V části **živé** v nástroj AMSE, klikněte v oblasti program pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-190">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="2a4fd-192">Název programu a v případě potřeby upravit **délka archivačního okna** (výchozí 4 hodiny).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-192">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="2a4fd-193">Můžete také určit umístění úložiště nebo ponechte jako výchozí.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-193">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="2a4fd-194">Zkontrolujte **nyní spustit Program** pole.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-194">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="2a4fd-195">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="2a4fd-196">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="2a4fd-197">Jakmile program běží, potvrďte přehrávání tak, že kliknete program pravým tlačítkem a přejdete na **přehrávání programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-197">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="2a4fd-198">Po potvrzení, klikněte pravým tlačítkem na program znovu a vyberte **zkopírujte adresu URL výstup do schránky** (nebo načtení těchto informací z **programu informace a nastavení** možnost v nabídce).</span><span class="sxs-lookup"><span data-stu-id="2a4fd-198">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="2a4fd-199">Datový proud je nyní připravena vložených v přehrávač, nebo distribuovány do cílovou skupinu pro zobrazení za provozu.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-199">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="2a4fd-200">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="2a4fd-200">Troubleshooting</span></span>
<span data-ttu-id="2a4fd-201">Podrobnosti najdete [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-201">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="2a4fd-202">Další krok</span><span class="sxs-lookup"><span data-stu-id="2a4fd-202">Next step</span></span>
<span data-ttu-id="2a4fd-203">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="2a4fd-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2a4fd-204">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="2a4fd-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
