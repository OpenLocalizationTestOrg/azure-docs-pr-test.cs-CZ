---
title: "aaaConfigure hello NewTek čase kodér toosend živý datový proud s jednou přenosovou rychlostí | Microsoft Docs"
description: "Toto téma ukazuje, jak tooconfigure hello čase live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase."
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
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="7c695-103">Použít hello NewTek čase kodér toosend živý datový proud s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="7c695-103">Use hello NewTek TriCaster encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c695-104">Čase</span><span class="sxs-lookup"><span data-stu-id="7c695-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="7c695-105">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="7c695-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="7c695-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="7c695-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="7c695-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="7c695-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="7c695-108">Toto téma ukazuje, jak tooconfigure hello [čase NewTek](http://newtek.com/products/tricaster-40.html) live kodér toosend jednou přenosovou rychlostí datového proudu tooAMS kanály, které jsou povolené pro kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="7c695-108">This topic shows how tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="7c695-109">Další informace najdete v tématu [práce s kanály, že jsou povolené tooPerform živé kódování službou Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="7c695-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="7c695-110">Tento kurz ukazuje, jak toomanage Azure Media Services (AMS) s nástrojem Azure Media Services Explorer (AMSE).</span><span class="sxs-lookup"><span data-stu-id="7c695-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="7c695-111">Tento nástroj lze spustit pouze na počítačích s Windows.</span><span class="sxs-lookup"><span data-stu-id="7c695-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="7c695-112">Pokud jste na Mac nebo Linux, použijte hello Azure portálu toocreate [kanály](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) a [programy](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="7c695-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7c695-113">Při použití čase pro odesílání v příspěvek kanálu tooAMS kanály, které jsou povolené kódování v reálném čase, může být video nebo zvuk chyb v živé události Pokud určité funkce čase, jako je rychlé vyjímání mezi informační kanály nebo přepnutí z slaty .</span><span class="sxs-lookup"><span data-stu-id="7c695-113">When using Tricaster for sending in a contribution feed tooAMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="7c695-114">Hello AMS tým pracuje na řešení těchto problémů do té doby, se nedoporučuje toouse tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="7c695-114">hello AMS team is working on fixing these issues, until then, it is not recommend toouse these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="7c695-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c695-115">Prerequisites</span></span>
* [<span data-ttu-id="7c695-116">Vytvoření účtu Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="7c695-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="7c695-117">Ujistěte se, je koncový bod streamování, spuštěná.</span><span class="sxs-lookup"><span data-stu-id="7c695-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="7c695-118">Další informace najdete v tématu [spravovat koncové body streamování v účtu Media Services](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="7c695-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="7c695-119">Nainstalujte nejnovější verzi hello hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) nástroj.</span><span class="sxs-lookup"><span data-stu-id="7c695-119">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="7c695-120">Spusťte nástroj hello a připojte se účet tooyour AMS.</span><span class="sxs-lookup"><span data-stu-id="7c695-120">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="7c695-121">Tipy</span><span class="sxs-lookup"><span data-stu-id="7c695-121">Tips</span></span>
* <span data-ttu-id="7c695-122">Pokud je to možné, použijte standardní kabelové internetové připojení.</span><span class="sxs-lookup"><span data-stu-id="7c695-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="7c695-123">Obvykle při určování nároky na šířku pásma je toodouble hello streamování přenosových rychlostí.</span><span class="sxs-lookup"><span data-stu-id="7c695-123">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="7c695-124">Přestože není povinný požadavek, pomůže zmírnit dopad hello zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="7c695-124">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="7c695-125">Při použití softwaru na základě kodéry, zavřete se všechny nepotřebné programy.</span><span class="sxs-lookup"><span data-stu-id="7c695-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="7c695-126">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="7c695-126">Create a channel</span></span>
1. <span data-ttu-id="7c695-127">Přejděte v hello nástroj AMSE, toohello **živé** kartě a klikněte pravým tlačítkem v rámci oblasti kanál hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-127">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="7c695-128">Vyberte **vytvořit kanál...**</span><span class="sxs-lookup"><span data-stu-id="7c695-128">Select **Create channel…**</span></span> <span data-ttu-id="7c695-129">v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-129">from hello menu.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="7c695-131">Zadejte název kanálu, hello pole popisu je volitelné.</span><span class="sxs-lookup"><span data-stu-id="7c695-131">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="7c695-132">V části Nastavení kanál, vyberte **standardní** pro hello Live Encoding možnost s hello vstupní protokol nastaven příliš**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="7c695-132">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="7c695-133">Všechna ostatní nastavení jako je můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="7c695-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="7c695-134">Ujistěte se, zda text hello **počáteční hello nový kanál teď** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="7c695-134">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="7c695-135">Klikněte na tlačítko **vytvořit kanál**.</span><span class="sxs-lookup"><span data-stu-id="7c695-135">Click **Create Channel**.</span></span>

   ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="7c695-137">Hello kanálu může trvat stejně dlouho jako toostart 20 minut.</span><span class="sxs-lookup"><span data-stu-id="7c695-137">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="7c695-138">Při spouštění hello kanál můžete [konfigurace hello kodér](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="7c695-138">While hello channel is starting you can [configure hello encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c695-139">Všimněte si, že fakturace začne hned, jak kanál přejde do stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="7c695-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="7c695-140">Další informace najdete v tématu [kanálu stavy](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="7c695-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="7c695-141"><a id=configure_tricaster_rtmp></a>Konfigurace hello kodér NewTek čase</span><span class="sxs-lookup"><span data-stu-id="7c695-141"><a id=configure_tricaster_rtmp></a>Configure hello NewTek TriCaster encoder</span></span>
<span data-ttu-id="7c695-142">V tento kurz hello se používají následující výstup nastavení.</span><span class="sxs-lookup"><span data-stu-id="7c695-142">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="7c695-143">Hello zbývající část tohoto oddílu popisuje kroky konfigurace podrobněji.</span><span class="sxs-lookup"><span data-stu-id="7c695-143">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="7c695-144">**Video**:</span><span class="sxs-lookup"><span data-stu-id="7c695-144">**Video**:</span></span>

* <span data-ttu-id="7c695-145">Kodeků: H.264</span><span class="sxs-lookup"><span data-stu-id="7c695-145">Codec: H.264</span></span>
* <span data-ttu-id="7c695-146">Profil: Vysoká (úroveň 4.0)</span><span class="sxs-lookup"><span data-stu-id="7c695-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="7c695-147">Přenosovou rychlostí: 5000 kb/s</span><span class="sxs-lookup"><span data-stu-id="7c695-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="7c695-148">Klíčový snímek: 2 sekund (60 sekund)</span><span class="sxs-lookup"><span data-stu-id="7c695-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="7c695-149">Míra s rámečkem: 30</span><span class="sxs-lookup"><span data-stu-id="7c695-149">Frame Rate: 30</span></span>

<span data-ttu-id="7c695-150">**Zvuk**:</span><span class="sxs-lookup"><span data-stu-id="7c695-150">**Audio**:</span></span>

* <span data-ttu-id="7c695-151">Kodeků: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="7c695-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="7c695-152">Přenosovou rychlostí: 192 kb /</span><span class="sxs-lookup"><span data-stu-id="7c695-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="7c695-153">Vzorkovací frekvence: 44,1 kHz</span><span class="sxs-lookup"><span data-stu-id="7c695-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="7c695-154">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="7c695-154">Configuration steps</span></span>
1. <span data-ttu-id="7c695-155">Vytvořte novou **čase NewTek** projektu v závislosti na tom, jaké vstupní zdroj videa se používá.</span><span class="sxs-lookup"><span data-stu-id="7c695-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="7c695-156">Jednou v rámci projektu, najde hello **datového proudu** tlačítko a klikněte na tlačítko hello ozubené kolečko ikonu další tooit tooaccess hello datového proudu konfigurace nabídky.</span><span class="sxs-lookup"><span data-stu-id="7c695-156">Once within that project, find hello **Stream** button, and click hello gear icon next tooit tooaccess hello stream configuration menu.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="7c695-158">Jakmile nabídce hello otevře, klikněte na tlačítko **nový** pod nadpisem hello připojení.</span><span class="sxs-lookup"><span data-stu-id="7c695-158">Once hello menu has opened, click **New** under hello Connection heading.</span></span> <span data-ttu-id="7c695-159">Po zobrazení výzvy pro typ připojení hello vyberte **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="7c695-159">When prompted for hello connection type, select **Adobe Flash**.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="7c695-161">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c695-161">Click **OK**.</span></span>
5. <span data-ttu-id="7c695-162">Profil aplikace FMLE lze importovat teď kliknutím hello šipkou rozevíracího seznamu v části **streamování profil** procházet příliš**Procházet**.</span><span class="sxs-lookup"><span data-stu-id="7c695-162">An FMLE profile can now be imported by clicking hello drop down arrow under **Streaming Profile** and navigating too**Browse**.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="7c695-164">Přejděte toowhere hello nakonfigurované FMLE profil byl uložen.</span><span class="sxs-lookup"><span data-stu-id="7c695-164">Navigate toowhere hello configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="7c695-165">Vyberte ji a stiskněte klávesu **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c695-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="7c695-166">Po nahrání profilu hello pokračujte dalším krokem toohello.</span><span class="sxs-lookup"><span data-stu-id="7c695-166">Once hello profile is uploaded, proceed toohello next step.</span></span>
8. <span data-ttu-id="7c695-167">Získání vstupní adresa URL kanálu hello v pořadí tooassign ho toohello čase **koncový bod RTMP**.</span><span class="sxs-lookup"><span data-stu-id="7c695-167">Get hello channel's input URL in order tooassign it toohello Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="7c695-168">Přejděte zpět toohello nástroj AMSE a zkontrolovat stav dokončení kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-168">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="7c695-169">Jakmile hello stav se změnil z **počáteční** příliš**systémem**, můžete získat hello vstupní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="7c695-169">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="7c695-170">Když běží hello kanál, klikněte pravým tlačítkem na název kanálu hello, přejděte dolů toohover přes **kopie vstupu URL tooclipboard** a pak vyberte **primární adresa URL vstupu**.</span><span class="sxs-lookup"><span data-stu-id="7c695-170">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="7c695-172">Tyto informace vložte hello **umístění** pole v části **serveru Flash** v rámci projektu čase hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-172">Paste this information in hello **Location** field under **Flash Server** within hello Tricaster project.</span></span> <span data-ttu-id="7c695-173">Přiřadit také název datového proudu v hello **ID datového proudu** pole.</span><span class="sxs-lookup"><span data-stu-id="7c695-173">Also assign a stream name in hello **Stream ID** field.</span></span>

    <span data-ttu-id="7c695-174">Pokud informace datový proud byl přidán toohello FMLE profil, se můžete také naimportovat toothis část kliknutím **importovat nastavení**, navigace profil FMLE toohello uložit a kliknutím na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c695-174">If stream information was added toohello FMLE profile, it can also be imported toothis section by clicking **Import Settings**, navigating toohello saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="7c695-175">Hello příslušná pole Flash serveru by měl naplnění hello informace z FMLE.</span><span class="sxs-lookup"><span data-stu-id="7c695-175">hello relevant Flash Server fields should populate with hello information from FMLE.</span></span>

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="7c695-177">Po dokončení klikněte na tlačítko **OK** v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-177">When finished, click **OK** at hello bottom of hello screen.</span></span> <span data-ttu-id="7c695-178">Jakmile jsou připravené videosoubory a zvukové vstupy do hello čase, začněte streamování tooAMS kliknutím hello **datového proudu** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c695-178">When video and audio inputs into hello Tricaster are ready, begin streaming tooAMS by clicking hello **Stream** button.</span></span>

     ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="7c695-180">Před kliknutím na **datového proudu**, můžete **musí** zajistěte, aby byl kanál hello připraven.</span><span class="sxs-lookup"><span data-stu-id="7c695-180">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="7c695-181">Ujistěte se také, není tooleave hello kanál ve stavu Připraveno bez vstupní příspěvku kanálu po dobu delší než > 15 minut.</span><span class="sxs-lookup"><span data-stu-id="7c695-181">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="7c695-182">Přehrávání testu</span><span class="sxs-lookup"><span data-stu-id="7c695-182">Test playback</span></span>
<span data-ttu-id="7c695-183">Nástroj AMSE toohello přejděte a klikněte pravým tlačítkem na toobe kanál hello testována.</span><span class="sxs-lookup"><span data-stu-id="7c695-183">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="7c695-184">V nabídce hello, najeďte myší na **přehrávání hello Preview** a vyberte **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="7c695-184">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="7c695-185">Pokud datový proud hello objeví v hello player, hello kodér bylo správně nakonfigurované tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="7c695-185">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="7c695-186">Je-li k chybě, bude nutné hello kanál toobe resetování a kodér nastavení upravit.</span><span class="sxs-lookup"><span data-stu-id="7c695-186">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="7c695-187">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="7c695-187">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="7c695-188">Vytvořit program</span><span class="sxs-lookup"><span data-stu-id="7c695-188">Create a program</span></span>
1. <span data-ttu-id="7c695-189">Po potvrzení kanálu přehrávání vytvořte program.</span><span class="sxs-lookup"><span data-stu-id="7c695-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="7c695-190">V části hello **živé** v nástroj AMSE hello, klikněte v oblasti programu hello pravým tlačítkem a vyberte **vytvořit nový Program**.</span><span class="sxs-lookup"><span data-stu-id="7c695-190">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![čase](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="7c695-192">Název programu hello a v případě potřeby upravit hello **délka archivačního okna** (které hodiny too4 výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="7c695-192">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="7c695-193">Můžete také určit umístění úložiště nebo ponechte jako výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="7c695-193">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="7c695-194">Zkontrolujte hello **počáteční hello teď Program** pole.</span><span class="sxs-lookup"><span data-stu-id="7c695-194">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="7c695-195">Klikněte na tlačítko **vytvořit Program**.</span><span class="sxs-lookup"><span data-stu-id="7c695-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="7c695-196">Vytváření programu trvá kratší dobu, než vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="7c695-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="7c695-197">Jakmile hello aplikaci, potvrďte přehrávání tak, že kliknete pravým tlačítkem programu hello a navigace příliš**přehrávání hello programech** a potom vyberete **s Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="7c695-197">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="7c695-198">Po potvrzení, klikněte pravým tlačítkem na programu hello znovu a vyberte **zkopírujte tooClipboard URL výstup hello** (nebo načtení těchto informací z hello **programu informace a nastavení** možnost nabídce hello).</span><span class="sxs-lookup"><span data-stu-id="7c695-198">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="7c695-199">datový proud Hello je nyní připraven toobe vložených v přehrávač nebo cílovou skupinu distribuované tooan live zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7c695-199">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="7c695-200">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7c695-200">Troubleshooting</span></span>
<span data-ttu-id="7c695-201">Najdete v tématu hello [řešení potíží s](media-services-troubleshooting-live-streaming.md) tématu pokyny.</span><span class="sxs-lookup"><span data-stu-id="7c695-201">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="7c695-202">Další krok</span><span class="sxs-lookup"><span data-stu-id="7c695-202">Next step</span></span>
<span data-ttu-id="7c695-203">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="7c695-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7c695-204">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7c695-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
