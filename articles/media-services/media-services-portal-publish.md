---
title: "AAA\"publikování obsahu pomocí hello portálu Azure | Microsoft Docs\""
description: "Tento kurz vás provede kroky hello publikování svůj obsah pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 92c364eb-5a5f-4f4e-8816-b162c031bb40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a7a3867a6939b4b9da883176c6cc20c99d6c54e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-content-with-hello-azure-portal"></a><span data-ttu-id="1d35b-103">Publikovat obsah s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1d35b-103">Publish content with hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d35b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1d35b-104">Portal</span></span>](media-services-portal-publish.md)
> * [<span data-ttu-id="1d35b-105">.NET</span><span class="sxs-lookup"><span data-stu-id="1d35b-105">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="1d35b-106">REST</span><span class="sxs-lookup"><span data-stu-id="1d35b-106">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="1d35b-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d35b-107">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="1d35b-108">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1d35b-108">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="1d35b-109">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d35b-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="1d35b-110">tooprovide uživatelů s adresou URL, která se dá použít toostream nebo stažení vašeho obsahu, je nejprve nutné příliš "publikovat" asset vytvořením lokátoru.</span><span class="sxs-lookup"><span data-stu-id="1d35b-110">tooprovide your user with a  URL that can be used toostream or download your content, you first need too"publish" your asset by creating a locator.</span></span> <span data-ttu-id="1d35b-111">Lokátory zajišťují přístup toofiles obsažené v hello asset.</span><span class="sxs-lookup"><span data-stu-id="1d35b-111">Locators provide access toofiles contained in hello asset.</span></span> <span data-ttu-id="1d35b-112">Služba Media Services podporuje dva typy lokátorů:</span><span class="sxs-lookup"><span data-stu-id="1d35b-112">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="1d35b-113">Streamování (OnDemandOrigin) lokátory, používají pro adaptivní streamování (například toostream MPEG DASH, HLS nebo technologie Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="1d35b-113">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, toostream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="1d35b-114">toocreate Lokátor streamování váš asset musí obsahovat soubor .ism.</span><span class="sxs-lookup"><span data-stu-id="1d35b-114">toocreate a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="1d35b-115">Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="1d35b-115">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="1d35b-116">Adresu URL streamování má následující formát hello a můžete ji použít tooplay assetů technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="1d35b-116">A streaming URL has hello following format and you can use it tooplay Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="1d35b-117">připojit toobuild na adresu URL, streamování HLS (format = m3u8-aapl) toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1d35b-117">toobuild an HLS streaming URL, append (format=m3u8-aapl) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="1d35b-118">připojit toobuild na adresu URL, streamování MPEG DASH (formát = mpd. čas csf) toohello adresy URL.</span><span class="sxs-lookup"><span data-stu-id="1d35b-118">toobuild an  MPEG DASH streaming URL, append (format=mpd-time-csf) toohello URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

<span data-ttu-id="1d35b-119">Adresa URL typu SAS má následující formát hello.</span><span class="sxs-lookup"><span data-stu-id="1d35b-119">A SAS URL has hello following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

<span data-ttu-id="1d35b-120">Další informace najdete v tématu [doručování obsahu přehled](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d35b-120">For more information, see [Delivering content overview](media-services-deliver-content-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1d35b-121">Pokud jste použili hello portálu toocreate lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.</span><span class="sxs-lookup"><span data-stu-id="1d35b-121">If you used hello portal toocreate locators before March 2015, locators with a two year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="1d35b-122">tooupdate na datum vypršení platnosti lokátoru, použijte [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1d35b-122">tooupdate an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="1d35b-123">Všimněte si, že při aktualizaci hello datum vypršení platnosti lokátoru SAS hello se změní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="1d35b-123">Note that when you update hello expiration date of a SAS locator, hello URL changes.</span></span>

### <a name="toouse-hello-portal-toopublish-an-asset"></a><span data-ttu-id="1d35b-124">toouse hello portálu toopublish prostředek</span><span class="sxs-lookup"><span data-stu-id="1d35b-124">toouse hello portal toopublish an asset</span></span>
<span data-ttu-id="1d35b-125">toouse hello portálu toopublish prostředek, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1d35b-125">toouse hello portal toopublish an asset, do hello following:</span></span>

1. <span data-ttu-id="1d35b-126">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d35b-126">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="1d35b-127">Vyberte **Nastavení** > **Assety**.</span><span class="sxs-lookup"><span data-stu-id="1d35b-127">Select **Settings** > **Assets**.</span></span>
3. <span data-ttu-id="1d35b-128">Vyberte, které chcete toopublish asset hello.</span><span class="sxs-lookup"><span data-stu-id="1d35b-128">Select hello asset that you want toopublish.</span></span>
4. <span data-ttu-id="1d35b-129">Klikněte na tlačítko hello **publikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d35b-129">Click hello **Publish** button.</span></span>
5. <span data-ttu-id="1d35b-130">Vyberte typ lokátoru hello.</span><span class="sxs-lookup"><span data-stu-id="1d35b-130">Select hello locator type.</span></span>
6. <span data-ttu-id="1d35b-131">Stiskněte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="1d35b-131">Press **Add**.</span></span>
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="1d35b-133">Adresa URL Hello přidá toohello seznam **publikovaných adres URL**.</span><span class="sxs-lookup"><span data-stu-id="1d35b-133">hello URL will be added toohello list of **Published URLs**.</span></span>

## <a name="play-content-from-hello-portal"></a><span data-ttu-id="1d35b-134">Přehrávání obsahu z portálu hello</span><span class="sxs-lookup"><span data-stu-id="1d35b-134">Play content from hello portal</span></span>
<span data-ttu-id="1d35b-135">Hello portál Azure nabízí přehrávač obsahu, které můžete použít tootest videa.</span><span class="sxs-lookup"><span data-stu-id="1d35b-135">hello Azure portal provides a content player that you can use tootest your video.</span></span>

<span data-ttu-id="1d35b-136">Klikněte na tlačítko hello požadovaného video a potom klikněte na hello **přehrání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1d35b-136">Click hello desired video and then click hello **Play** button.</span></span>

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="1d35b-138">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="1d35b-138">Some considerations apply:</span></span>

* <span data-ttu-id="1d35b-139">Zajistěte, aby byla publikována hello videa.</span><span class="sxs-lookup"><span data-stu-id="1d35b-139">Make sure hello video has been published.</span></span>
* <span data-ttu-id="1d35b-140">To **přehrávač médií** přehrává z výchozího hello koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="1d35b-140">This **Media player** plays from hello default streaming endpoint.</span></span> <span data-ttu-id="1d35b-141">Pokud chcete, aby tooplay z jiného než výchozího koncového bodu, streamování klikněte toocopy hello adresu URL a použijte jiný přehrávač.</span><span class="sxs-lookup"><span data-stu-id="1d35b-141">If you want tooplay from a non-default streaming endpoint, click toocopy hello URL and use another player.</span></span> <span data-ttu-id="1d35b-142">Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="1d35b-142">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
* <span data-ttu-id="1d35b-143">koncový bod, ze kterého jsou datové proudy streamování Hello musí být spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1d35b-143">hello streaming endpoint from which you are streaming must be running.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="1d35b-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d35b-144">Next steps</span></span>
<span data-ttu-id="1d35b-145">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d35b-145">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1d35b-146">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1d35b-146">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

