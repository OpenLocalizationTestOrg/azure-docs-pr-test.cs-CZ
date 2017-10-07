---
title: "aaaGet spuštění s doručováním VoD pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 5c1c1b1f74ec1f1301120fe8e5a5ae183fe0338f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-hello-azure-portal"></a><span data-ttu-id="29ac5-103">Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="29ac5-103">Get started with delivering content on demand using hello Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="29ac5-104">Tento kurz vás provede procesem hello kroky implementace základní služby doručování obsahu vyžádání pomocí Azure Media Services (AMS) aplikace pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="29ac5-104">This tutorial walks you through hello steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using hello Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29ac5-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="29ac5-105">Prerequisites</span></span>
<span data-ttu-id="29ac5-106">Hello následují požadované toocomplete hello kurzu:</span><span class="sxs-lookup"><span data-stu-id="29ac5-106">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="29ac5-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="29ac5-107">An Azure account.</span></span> <span data-ttu-id="29ac5-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="29ac5-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="29ac5-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="29ac5-109">A Media Services account.</span></span> <span data-ttu-id="29ac5-110">toocreate účet Media Services najdete v části [jak tooCreate účtu Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="29ac5-110">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="29ac5-111">Tento kurz zahrnuje hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="29ac5-111">This tutorial includes hello following tasks:</span></span>

1. <span data-ttu-id="29ac5-112">Spusťte koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="29ac5-113">Nahrání videosouboru</span><span class="sxs-lookup"><span data-stu-id="29ac5-113">Upload a video file.</span></span>
3. <span data-ttu-id="29ac5-114">Zakódujte hello zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="29ac5-114">Encode hello source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="29ac5-115">Publikujte hello asset a get streamování a progresivní stahování adresy URL.</span><span class="sxs-lookup"><span data-stu-id="29ac5-115">Publish hello asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="29ac5-116">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="29ac5-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="29ac5-117">Spusťte koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-117">Start streaming endpoints</span></span> 

<span data-ttu-id="29ac5-118">Při práci se službou Azure Media Services je jedním hello nejběžnějších scénářů doručování videa přes streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="29ac5-118">When working with Azure Media Services one of hello most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="29ac5-119">Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver kódováním MP4 obsah ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) v běhu, aniž byste museli toostore předem zabalené vaší s adaptivní přenosovou rychlostí verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-119">Media Services provides dynamic packaging, which allows you toodeliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having toostore pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="29ac5-120">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="29ac5-120">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="29ac5-121">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="29ac5-121">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

<span data-ttu-id="29ac5-122">toostart hello koncový bod streamování, hello následující:</span><span class="sxs-lookup"><span data-stu-id="29ac5-122">toostart hello streaming endpoint, do hello following:</span></span>

1. <span data-ttu-id="29ac5-123">Přihlaste se na hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29ac5-123">Log in at hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="29ac5-124">V okně Nastavení hello klikněte Streaming koncové body.</span><span class="sxs-lookup"><span data-stu-id="29ac5-124">In hello Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="29ac5-125">Klikněte na tlačítko hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-125">Click hello default streaming endpoint.</span></span> 

    <span data-ttu-id="29ac5-126">Zobrazí se okno Hello výchozí podrobnosti koncový bod STREAMOVÁNÍ.</span><span class="sxs-lookup"><span data-stu-id="29ac5-126">hello DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="29ac5-127">Klikněte na ikonu Start hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-127">Click hello Start icon.</span></span>
5. <span data-ttu-id="29ac5-128">Klikněte na tlačítko toosave tlačítko hello uložit provedené změny.</span><span class="sxs-lookup"><span data-stu-id="29ac5-128">Click hello Save button toosave your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="29ac5-129">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="29ac5-129">Upload files</span></span>
<span data-ttu-id="29ac5-130">toostream videa pomocí služby Azure Media Services, budete potřebovat tooupload hello zdrojová videa, zakódovat je do více přenosových rychlostí a publikujte hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="29ac5-130">toostream videos using Azure Media Services, you need tooupload hello source videos, encode them into multiple bitrates, and publish hello result.</span></span> <span data-ttu-id="29ac5-131">prvním krokem Hello je popsaná v této části.</span><span class="sxs-lookup"><span data-stu-id="29ac5-131">hello first step is covered in this section.</span></span> 

1. <span data-ttu-id="29ac5-132">V hello **nastavení** okně klikněte na tlačítko **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-132">In hello **Setting** window, click **Assets**.</span></span>
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="29ac5-134">Klikněte na tlačítko hello **nahrát** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29ac5-134">Click hello **Upload** button.</span></span>
   
    <span data-ttu-id="29ac5-135">Hello **nahrát asset videa** se zobrazí v okně.</span><span class="sxs-lookup"><span data-stu-id="29ac5-135">hello **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="29ac5-136">Velikost souboru není nijak omezená.</span><span class="sxs-lookup"><span data-stu-id="29ac5-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="29ac5-137">Procházet toohello požadovaného video ve vašem počítači, vyberte ho a klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="29ac5-137">Browse toohello desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="29ac5-138">Spustí nahrávání Hello a zobrazí se průběh hello pod názvem souboru hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-138">hello upload starts and you can see hello progress under hello file name.</span></span>  

<span data-ttu-id="29ac5-139">Po dokončení nahrávání hello by se zobrazit hello nový prostředek zobrazí v hello **prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="29ac5-139">Once hello upload completes, you see hello new asset listed in hello **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="29ac5-140">Kódování assetů</span><span class="sxs-lookup"><span data-stu-id="29ac5-140">Encode assets</span></span>

<span data-ttu-id="29ac5-141">Při práci se službou Azure Media Services je jedním nejběžnější scénářů hello doručování tooyour klienti streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="29ac5-141">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="29ac5-142">Služba Media Services podporuje následující adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="29ac5-142">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="29ac5-143">tooprepare videa pro streamování s adaptivní přenosovou rychlostí, je nutné tooencode svůj zdroj videa do souborů s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="29ac5-143">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="29ac5-144">Měli byste použít hello **Media Encoder Standard** tooencode kodér videa.</span><span class="sxs-lookup"><span data-stu-id="29ac5-144">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="29ac5-145">Služba Media Services také poskytuje dynamické balení, což vám umožní toodeliver vaše soubory MP4 více přenosovými rychlostmi v hello následující streamování formáty: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste museli toorepackage do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-145">Media Services also provides dynamic packaging, which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toorepackage into these streaming formats.</span></span> <span data-ttu-id="29ac5-146">Při dynamickém balení stačí pouze toostore a sestavení platím hello souborů v jednom úložném formátu a služba Media Services a slouží hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="29ac5-146">With dynamic packaging, you only need toostore and pay for hello files in single storage format and Media Services builds and serves hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="29ac5-147">tootake výhod dynamického balení, je nutné tooencode zdrojového souboru do sady souborů MP4 s více přenosovými rychlostmi (postup hello kódování je ukázán později v této části).</span><span class="sxs-lookup"><span data-stu-id="29ac5-147">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

### <a name="toouse-hello-portal-tooencode"></a><span data-ttu-id="29ac5-148">portál tooencode toouse hello</span><span class="sxs-lookup"><span data-stu-id="29ac5-148">toouse hello portal tooencode</span></span>
<span data-ttu-id="29ac5-149">Tato část popisuje kroky hello tooencode může trvat svůj obsah pomocí procesoru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="29ac5-149">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="29ac5-150">V hello **nastavení** vyberte **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-150">In hello **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="29ac5-151">V hello **prostředky** okno, vyberte hello asset, které chcete tooencode.</span><span class="sxs-lookup"><span data-stu-id="29ac5-151">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
3. <span data-ttu-id="29ac5-152">Stiskněte klávesu hello **kódovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29ac5-152">Press hello **Encode** button.</span></span>
4. <span data-ttu-id="29ac5-153">V hello **kódovat asset** oken, vyberte hello procesor "Media Encoder Standard" a jedno z přednastavení.</span><span class="sxs-lookup"><span data-stu-id="29ac5-153">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="29ac5-154">Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="29ac5-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="29ac5-155">Pokud máte v plánu toocontrol které předvolby kódování se používá, mějte na paměti: je důležité tooselect hello přednastavení, která je nejvhodnější pro vaše vstupní video.</span><span class="sxs-lookup"><span data-stu-id="29ac5-155">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="29ac5-156">Například pokud znáte vaše vstupní video má rozlišení 1920 × 1080 pixelů, pak můžete použít hello "H264 Multiple Bitrate 1080p" přednastavené.</span><span class="sxs-lookup"><span data-stu-id="29ac5-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="29ac5-157">Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.</span><span class="sxs-lookup"><span data-stu-id="29ac5-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="29ac5-158">Pro snadnější správu máte možnost úpravy hello název hello výstupní asset a název hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="29ac5-158">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="29ac5-160">Stiskněte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="29ac5-161">Monitorování průběhu úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="29ac5-161">Monitor encoding job progress</span></span>
<span data-ttu-id="29ac5-162">Klikněte na tlačítko toomonitor hello průběh úlohy kódování hello **nastavení** (v hello horní části stránky hello) a potom vyberte **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-162">toomonitor hello progress of hello encoding job, click **Settings** (at hello top of hello page) and then select **Jobs**.</span></span>

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="29ac5-164">Publikování obsahu</span><span class="sxs-lookup"><span data-stu-id="29ac5-164">Publish content</span></span>
<span data-ttu-id="29ac5-165">tooprovide uživatelů s adresou URL, která se dá použít toostream nebo stažení vašeho obsahu, je nejprve nutné příliš "publikovat" asset vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="29ac5-165">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="29ac5-166">Lokátory zajišťují přístup toofiles obsažené v hello asset.</span><span class="sxs-lookup"><span data-stu-id="29ac5-166">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="29ac5-167">Služba Media Services podporuje dva typy lokátorů:</span><span class="sxs-lookup"><span data-stu-id="29ac5-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="29ac5-168">Streamování (OnDemandOrigin) lokátory, používají pro adaptivní streamování (například toostream MPEG DASH, HLS nebo technologie Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="29ac5-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="29ac5-169">toocreate Lokátor streamování váš asset musí obsahovat soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="29ac5-169">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="29ac5-170">Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="29ac5-171">Adresu URL streamování má následující formát hello a můžete ji použít tooplay assetů technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="29ac5-171">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="29ac5-172">připojit toobuild na adresu URL, streamování HLS (format = m3u8-aapl) toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="29ac5-172">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="29ac5-173">připojit toobuild na adresu URL, streamování MPEG DASH (formát = mpd. čas csf) toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="29ac5-173">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="29ac5-174">Adresa URL typu SAS má následující formát hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-174">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="29ac5-175">Pokud jste použili hello portálu toocreate lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.</span><span class="sxs-lookup"><span data-stu-id="29ac5-175">If you used hello portal toocreate locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="29ac5-176">tooupdate na datum vypršení platnosti lokátoru, použijte [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="29ac5-176">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="29ac5-177">Při aktualizaci hello datum vypršení platnosti lokátoru SAS se změní adresa URL hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-177">When you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="29ac5-178">toouse hello portálu toopublish prostředek</span><span class="sxs-lookup"><span data-stu-id="29ac5-178">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="29ac5-179">toouse hello portálu toopublish prostředek, hello následující:</span><span class="sxs-lookup"><span data-stu-id="29ac5-179">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="29ac5-180">Vyberte **Nastavení** > **Assety**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="29ac5-181">Vyberte, které chcete toopublish asset hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-181">Select hello asset that you want toopublish.</span></span>
3. <span data-ttu-id="29ac5-182">Klikněte na tlačítko hello **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29ac5-182">Click hello **Publish** button.</span></span>
4. <span data-ttu-id="29ac5-183">Vyberte typ lokátoru hello.</span><span class="sxs-lookup"><span data-stu-id="29ac5-183">Select hello locator type.</span></span>
5. <span data-ttu-id="29ac5-184">Stiskněte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-184">Press **Add**.</span></span>
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="29ac5-186">Adresa URL Hello je přidána toohello seznam **publikovaných adres URL**.</span><span class="sxs-lookup"><span data-stu-id="29ac5-186">hello URL is added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="29ac5-187">Přehrávání obsahu z portálu hello</span><span class="sxs-lookup"><span data-stu-id="29ac5-187">Play content from hello portal</span></span>
<span data-ttu-id="29ac5-188">Hello portál Azure nabízí přehrávač obsahu, které můžete použít tootest videa.</span><span class="sxs-lookup"><span data-stu-id="29ac5-188">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="29ac5-189">Klikněte na tlačítko hello požadovaného video a potom klikněte na hello **přehrání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="29ac5-189">Click hello desired video and then click hello **Play** button.</span></span>

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="29ac5-191">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="29ac5-191">Some considerations apply:</span></span>

* <span data-ttu-id="29ac5-192">toobegin streamování, spusťte spuštěné hello **výchozí** koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-192">toobegin streaming, start running hello **default** streaming endpoint.</span></span>
* <span data-ttu-id="29ac5-193">Zajistěte, aby byla publikována hello videa.</span><span class="sxs-lookup"><span data-stu-id="29ac5-193">Make sure hello video has been published.</span></span>
* <span data-ttu-id="29ac5-194">To **přehrávač médií** přehrává z výchozího hello koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="29ac5-194">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="29ac5-195">Pokud chcete, aby tooplay z jiného než výchozího koncového bodu, streamování klikněte toocopy hello adresu URL a použijte jiný přehrávač.</span><span class="sxs-lookup"><span data-stu-id="29ac5-195">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="29ac5-196">Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="29ac5-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="29ac5-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29ac5-197">Next steps</span></span>
<span data-ttu-id="29ac5-198">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="29ac5-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="29ac5-199">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="29ac5-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

