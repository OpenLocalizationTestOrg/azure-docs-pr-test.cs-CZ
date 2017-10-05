---
title: "Začínáme s doručováním videa na vyžádání (VoD) pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a webu Azure Portal."
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
ms.openlocfilehash: a8eeeeff412837acba14b441a3c590edf7e3597a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a><span data-ttu-id="4dc1c-103">Začínáme s doručováním obsahu na vyžádání pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4dc1c-103">Get started with delivering content on demand using the Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="4dc1c-104">V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a webu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4dc1c-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4dc1c-105">Prerequisites</span></span>
<span data-ttu-id="4dc1c-106">K dokončení kurzu potřebujete následující:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="4dc1c-107">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-107">An Azure account.</span></span> <span data-ttu-id="4dc1c-108">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="4dc1c-109">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-109">A Media Services account.</span></span> <span data-ttu-id="4dc1c-110">Pokud chcete vytvořit účet Media Services, přečtěte si článek [Jak vytvořit účet Media Services](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="4dc1c-111">Tento kurz sestává z následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-111">This tutorial includes the following tasks:</span></span>

1. <span data-ttu-id="4dc1c-112">Spusťte koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="4dc1c-113">Nahrání videosouboru</span><span class="sxs-lookup"><span data-stu-id="4dc1c-113">Upload a video file.</span></span>
3. <span data-ttu-id="4dc1c-114">Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="4dc1c-114">Encode the source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="4dc1c-115">Publikování assetu a získání adres URL streamování a progresivního stahování</span><span class="sxs-lookup"><span data-stu-id="4dc1c-115">Publish the asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="4dc1c-116">Přehrání obsahu</span><span class="sxs-lookup"><span data-stu-id="4dc1c-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="4dc1c-117">Spusťte koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-117">Start streaming endpoints</span></span> 

<span data-ttu-id="4dc1c-118">Při práci se službou Azure Media Services je jedním z nejběžnější scénářů doručování videa prostřednictvím streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-118">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="4dc1c-119">Služba Media Services poskytuje dynamické balení, které umožňuje doručovat obsah s adaptivní přenosovou rychlostí s kódováním MP4 ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming). není přitom potřeba ukládat předem zabalené verze pro každý z těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-119">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="4dc1c-120">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-120">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="4dc1c-121">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-121">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

<span data-ttu-id="4dc1c-122">Pokud chcete spustit koncový bod streamování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-122">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="4dc1c-123">Přihlaste se na [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-123">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4dc1c-124">V okně Nastavení klikněte na Koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-124">In the Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="4dc1c-125">Klikněte na výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-125">Click the default streaming endpoint.</span></span> 

    <span data-ttu-id="4dc1c-126">Zobrazí se okno VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-126">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="4dc1c-127">Klikněte na ikonu Spustit.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-127">Click the Start icon.</span></span>
5. <span data-ttu-id="4dc1c-128">Kliknutím na tlačítko Uložit uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-128">Click the Save button to save your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="4dc1c-129">Nahrání souborů</span><span class="sxs-lookup"><span data-stu-id="4dc1c-129">Upload files</span></span>
<span data-ttu-id="4dc1c-130">Pokud chcete streamovat videa pomocí služby Azure Media Services, musíte nahrát zdrojová videa, zakódovat je do více přenosových rychlostí a výsledek publikovat.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-130">To stream videos using Azure Media Services, you need to upload the source videos, encode them into multiple bitrates, and publish the result.</span></span> <span data-ttu-id="4dc1c-131">První krok pokrývá tato část.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-131">The first step is covered in this section.</span></span> 

1. <span data-ttu-id="4dc1c-132">V okně **Nastavení** klikněte na **Assety**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-132">In the **Setting** window, click **Assets**.</span></span>
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="4dc1c-134">Klikněte na tlačítko **Nahrát**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-134">Click the **Upload** button.</span></span>
   
    <span data-ttu-id="4dc1c-135">Zobrazí se okno **Nahrát asset videa**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-135">The **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4dc1c-136">Velikost souboru není nijak omezená.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="4dc1c-137">Přejděte v počítači na požadovaného video, vyberte ho a klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-137">Browse to the desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="4dc1c-138">Spustí se nahrávání. Jeho průběh můžete sledovat pod názvem souboru.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-138">The upload starts and you can see the progress under the file name.</span></span>  

<span data-ttu-id="4dc1c-139">Po dokončení nahrávání se nový prostředek zobrazí v okně **Assety**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-139">Once the upload completes, you see the new asset listed in the **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="4dc1c-140">Kódování assetů</span><span class="sxs-lookup"><span data-stu-id="4dc1c-140">Encode assets</span></span>

<span data-ttu-id="4dc1c-141">Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-141">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="4dc1c-142">Služba Media Services podporuje následující technologie streamování s adaptivní přenosovou rychlostí: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-142">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="4dc1c-143">Příprava vašich videí pro streamování s adaptivní přenosovou rychlostí spočívá v zakódování zdrojového videa zdroje do souborů s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-143">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="4dc1c-144">Ke kódování vašich videí byste měli použít kodér **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-144">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="4dc1c-145">Služba Media Services také poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 s více přenosovými rychlostmi ve formátech streamování MPEG DASH, HLS nebo technologie Smooth Streaming, aniž byste je museli znovu zabalit do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-145">Media Services also provides dynamic packaging, which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to repackage into these streaming formats.</span></span> <span data-ttu-id="4dc1c-146">Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services sestaví a dodá vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-146">With dynamic packaging, you only need to store and pay for the files in single storage format and Media Services builds and serves the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="4dc1c-147">Pokud chcete využít výhod dynamického balení, musíte zdrojový soubor zakódovat do sady souborů MP4 s více přenosovými rychlostmi (postup kódování je uvedený dále v této části).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-147">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

### <a name="to-use-the-portal-to-encode"></a><span data-ttu-id="4dc1c-148">Použití portálu ke kódování</span><span class="sxs-lookup"><span data-stu-id="4dc1c-148">To use the portal to encode</span></span>
<span data-ttu-id="4dc1c-149">Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-149">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="4dc1c-150">V okně **Nastavení** vyberte **Assety**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-150">In the **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="4dc1c-151">V okně **Assety** vyberte asset, který chcete zakódovat.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-151">In the **Assets** window, select the asset that you would like to encode.</span></span>
3. <span data-ttu-id="4dc1c-152">Stiskněte tlačítko **Kódovat**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-152">Press the **Encode** button.</span></span>
4. <span data-ttu-id="4dc1c-153">V okně **Kódovat asset** vyberte procesor „Media Encoder Standard“ a jedno z přednastavení.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-153">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="4dc1c-154">Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="4dc1c-155">Pokud se chystáte řídit, která předvolba kódování se použije, pamatujte, že je důležité vybrat předvolbu, která je nejvhodnější pro vaše vstupní video.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-155">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="4dc1c-156">Pokud například více, že vaše vstupní video má rozlišení 1920 × 1080 pixelů, můžete použít přednastavení „H264 Multiple Bitrate 1080p“.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="4dc1c-157">Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="4dc1c-158">Pro snadnější správu máte možnost upravit název výstupního assetu a název úlohy.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-158">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="4dc1c-160">Stiskněte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="4dc1c-161">Monitorování průběhu úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="4dc1c-161">Monitor encoding job progress</span></span>
<span data-ttu-id="4dc1c-162">Pokud chcete monitorovat průběh úlohy kódování, klikněte na **Nastavení** (v horní části stránky) a pak vyberte **Úlohy**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-162">To monitor the progress of the encoding job, click **Settings** (at the top of the page) and then select **Jobs**.</span></span>

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="4dc1c-164">Publikování obsahu</span><span class="sxs-lookup"><span data-stu-id="4dc1c-164">Publish content</span></span>
<span data-ttu-id="4dc1c-165">Pokud chcete uživateli poskytnout adresu URL, kterou lze použít ke streamování nebo stažení vašeho obsahu, musíte asset nejprve „publikovat“ vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-165">To provide your user with a  URL that can be used to stream or download your content, you first need to "publish" your asset by creating a locator.</span></span> <span data-ttu-id="4dc1c-166">Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-166">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="4dc1c-167">Služba Media Services podporuje dva typy lokátorů:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="4dc1c-168">Lokátory streamování (OnDemandOrigin), které se používají pro adaptivní streamování (například pro streamování MPEG, HLS nebo technologie Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, to stream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="4dc1c-169">Pokud chcete vytvořit lokátor streamování, váš asset musí obsahovat soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-169">To create a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="4dc1c-170">Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="4dc1c-171">Adresa URL streamování má následující formát a můžete ji použít k přehrávání mediálních assetů technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-171">A streaming URL has the following format and you can use it to play Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="4dc1c-172">Pokud chcete vytvořit adresu URL streamování HLS, připojte na konec adresy (format=m3u8-aapl).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-172">To build an HLS streaming URL, append (format=m3u8-aapl) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="4dc1c-173">Pokud chcete vytvořit adresu URL streamování MPEG DASH, připojte na konec adresy (format=mpd-time-csf).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-173">To build an  MPEG DASH streaming URL, append (format=mpd-time-csf) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="4dc1c-174">Adresa URL typu SAS má následující formát.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-174">A SAS URL has the following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="4dc1c-175">Pokud jste použili portál k vytvoření lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-175">If you used the portal to create locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="4dc1c-176">Pokud chcete aktualizovat datum vypršení platnosti lokátoru, použijte rozhraní [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) API nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-176">To update an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="4dc1c-177">Při aktualizaci data vypršení platnosti lokátoru SAS se změní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-177">When you update the expiration date of a SAS locator, the URL changes.</span></span>

### <a name="to-use-the-portal-to-publish-an-asset"></a><span data-ttu-id="4dc1c-178">Postup publikování assetu pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="4dc1c-178">To use the portal to publish an asset</span></span>
<span data-ttu-id="4dc1c-179">Postup publikování assetu pomocí portálu:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-179">To use the portal to publish an asset, do the following:</span></span>

1. <span data-ttu-id="4dc1c-180">Vyberte **Nastavení** > **Assety**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="4dc1c-181">Vyberte asset, který chcete publikovat.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-181">Select the asset that you want to publish.</span></span>
3. <span data-ttu-id="4dc1c-182">Potom klikněte na tlačítko **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-182">Click the **Publish** button.</span></span>
4. <span data-ttu-id="4dc1c-183">Vyberte typ lokátoru.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-183">Select the locator type.</span></span>
5. <span data-ttu-id="4dc1c-184">Stiskněte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-184">Press **Add**.</span></span>
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="4dc1c-186">Adresa URL se přidá do seznamu **publikovaných adres URL**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-186">The URL is added to the list of **Published URLs**.</span></span>

## <a name="play-content-from-the-portal"></a><span data-ttu-id="4dc1c-187">Přehrávání obsahu z portálu</span><span class="sxs-lookup"><span data-stu-id="4dc1c-187">Play content from the portal</span></span>
<span data-ttu-id="4dc1c-188">Azure Portal nabízí přehrávač obsahu, který můžete použít k testování videa.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-188">The Azure portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="4dc1c-189">Klikněte na požadované video a potom klikněte na tlačítko **Přehrát**.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-189">Click the desired video and then click the **Play** button.</span></span>

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="4dc1c-191">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="4dc1c-191">Some considerations apply:</span></span>

* <span data-ttu-id="4dc1c-192">Streamování začnete tak, že spustíte **výchozí** koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-192">To begin streaming, start running the **default** streaming endpoint.</span></span>
* <span data-ttu-id="4dc1c-193">Zkontrolujte, že bylo video publikováno.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-193">Make sure the video has been published.</span></span>
* <span data-ttu-id="4dc1c-194">Tento **Přehrávač médií** přehrává z výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-194">This **Media player** plays from the default streaming endpoint.</span></span> <span data-ttu-id="4dc1c-195">Pokud chcete přehrávat z jiného než výchozího koncového bodu streamování, klikněte na zkopírování adresy URL a použijte jiný přehrávač.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-195">If you want to play from a non-default streaming endpoint, click to copy the URL and use another player.</span></span> <span data-ttu-id="4dc1c-196">Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="4dc1c-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4dc1c-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4dc1c-197">Next steps</span></span>
<span data-ttu-id="4dc1c-198">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="4dc1c-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4dc1c-199">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="4dc1c-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

