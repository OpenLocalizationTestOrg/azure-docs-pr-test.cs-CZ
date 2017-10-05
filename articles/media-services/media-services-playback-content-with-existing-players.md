---
title: "Použít existující přehrávače k přehrávání obsahu – Azure | Microsoft Docs"
description: "Toto téma obsahuje seznam existující přehrávače, můžete použít k přehrávání obsahu."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: juliako
ms.openlocfilehash: 48f373b013b1192c353352b801876d706d91dd28
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="playing-your-content-with-existing-players"></a><span data-ttu-id="97ccc-103">Přehrávání obsahu s existující hráči</span><span class="sxs-lookup"><span data-stu-id="97ccc-103">Playing your content with existing players</span></span>
<span data-ttu-id="97ccc-104">Azure Media Services podporuje mnoho oblíbených streamování formátů, jako je například technologie Smooth Streaming, HTTP Live Streaming a MPEG-Dash.</span><span class="sxs-lookup"><span data-stu-id="97ccc-104">Azure Media Services supports many popular streaming formats, such as Smooth Streaming, HTTP Live Streaming, and MPEG-Dash.</span></span> <span data-ttu-id="97ccc-105">Toto téma vám ukazuje na existující přehrávačů, které můžete použít k testování vaše datové proudy.</span><span class="sxs-lookup"><span data-stu-id="97ccc-105">This topic points you to existing players that you can use to test your streams.</span></span>

### <a name="the-azure-portal-media-services-content-player"></a><span data-ttu-id="97ccc-106">Azure portálu Media Services obsahu přehrávač.</span><span class="sxs-lookup"><span data-stu-id="97ccc-106">The Azure portal Media Services content player</span></span>
<span data-ttu-id="97ccc-107">**Azure** portál nabízí přehrávač obsahu, který můžete použít k testování videa.</span><span class="sxs-lookup"><span data-stu-id="97ccc-107">The **Azure** portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="97ccc-108">Klikněte na požadované video (zajistěte, aby byl [publikovaná](media-services-portal-publish.md)) a klikněte na tlačítko **přehrání** tlačítko v dolní části portálu.</span><span class="sxs-lookup"><span data-stu-id="97ccc-108">Click on the desired video (make sure it was [published](media-services-portal-publish.md)) and click the **Play** button at the bottom of the portal.</span></span>

<span data-ttu-id="97ccc-109">Musí být splněny určité předpoklady:</span><span class="sxs-lookup"><span data-stu-id="97ccc-109">Some considerations apply:</span></span>

* <span data-ttu-id="97ccc-110">Přehrávač **MEDIA SERVICES CONTENT PLAYER** přehrává z výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="97ccc-110">The **MEDIA SERVICES CONTENT PLAYER** plays from the default streaming endpoint.</span></span> <span data-ttu-id="97ccc-111">Pokud chcete přehrávat z jiného než výchozích koncového bodu streamování, použijte jiný přehrávač.</span><span class="sxs-lookup"><span data-stu-id="97ccc-111">If you want to play from a non-default streaming endpoint, use another player.</span></span> <span data-ttu-id="97ccc-112">Například [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="97ccc-112">For example, [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a><span data-ttu-id="97ccc-114">Přehrávač médií Azure</span><span class="sxs-lookup"><span data-stu-id="97ccc-114">Azure Media Player</span></span>
<span data-ttu-id="97ccc-115">Použití [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) k přehrávání obsahu (Vymazat nebo chráněného) v některém z následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="97ccc-115">Use [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to playback your content (clear or protected) in any of the following formats:</span></span>

* <span data-ttu-id="97ccc-116">Technologie Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="97ccc-116">Smooth Streaming</span></span>
* <span data-ttu-id="97ccc-117">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="97ccc-117">MPEG DASH</span></span>
* <span data-ttu-id="97ccc-118">HLS</span><span class="sxs-lookup"><span data-stu-id="97ccc-118">HLS</span></span>
* <span data-ttu-id="97ccc-119">Progresivní MP4</span><span class="sxs-lookup"><span data-stu-id="97ccc-119">Progressive MP4</span></span>

### <a name="flash-player"></a><span data-ttu-id="97ccc-120">Flash Player</span><span class="sxs-lookup"><span data-stu-id="97ccc-120">Flash Player</span></span>
#### <a name="aes-encrypted-with-token"></a><span data-ttu-id="97ccc-121">AES šifrované pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="97ccc-121">AES-encrypted with Token</span></span>
[<span data-ttu-id="97ccc-122">http://aestoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="97ccc-122">http://aestoken.azurewebsites.net</span></span>](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a><span data-ttu-id="97ccc-123">Vybavené přehrávače prostředí Silverlight</span><span class="sxs-lookup"><span data-stu-id="97ccc-123">Silverlight Players</span></span>
#### <a name="monitoring"></a><span data-ttu-id="97ccc-124">Monitorování</span><span class="sxs-lookup"><span data-stu-id="97ccc-124">Monitoring</span></span>
[<span data-ttu-id="97ccc-125">http://smf.cloudapp.NET/healthmonitor</span><span class="sxs-lookup"><span data-stu-id="97ccc-125">http://smf.cloudapp.net/healthmonitor</span></span>](http://smf.cloudapp.net/healthmonitor)

#### <a name="playready-with-token"></a><span data-ttu-id="97ccc-126">PlayReady pomocí tokenu</span><span class="sxs-lookup"><span data-stu-id="97ccc-126">PlayReady with Token</span></span>
[<span data-ttu-id="97ccc-127">http://sltoken.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="97ccc-127">http://sltoken.azurewebsites.net</span></span>](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a><span data-ttu-id="97ccc-128">Přehrávače čárka</span><span class="sxs-lookup"><span data-stu-id="97ccc-128">DASH Players</span></span>
[<span data-ttu-id="97ccc-129">http://dashplayer.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="97ccc-129">http://dashplayer.azurewebsites.net</span></span>](http://dashplayer.azurewebsites.net)

[<span data-ttu-id="97ccc-130">http://dashif.org</span><span class="sxs-lookup"><span data-stu-id="97ccc-130">http://dashif.org</span></span>](http://dashif.org)

### <a name="other"></a><span data-ttu-id="97ccc-131">Ostatní</span><span class="sxs-lookup"><span data-stu-id="97ccc-131">Other</span></span>
<span data-ttu-id="97ccc-132">K testování HLS adresy URL můžete použít také:</span><span class="sxs-lookup"><span data-stu-id="97ccc-132">To test HLS URLs you can also use:</span></span>

* <span data-ttu-id="97ccc-133">**Safari** na zařízení s iOS nebo</span><span class="sxs-lookup"><span data-stu-id="97ccc-133">**Safari** on an iOS device or</span></span>
* <span data-ttu-id="97ccc-134">**Přehrávač HLS 3ivx** v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="97ccc-134">**3ivx HLS Player** on Windows.</span></span>

## <a name="developing-video-players"></a><span data-ttu-id="97ccc-135">Vývoj přehrávačů videa</span><span class="sxs-lookup"><span data-stu-id="97ccc-135">Developing video players</span></span>
<span data-ttu-id="97ccc-136">Informace o tom, jak vyvíjet vlastní přehrávače najdete v tématu [vývoj přehrávačů videa](media-services-develop-video-players.md)</span><span class="sxs-lookup"><span data-stu-id="97ccc-136">For information about how to develop your own players, see [Developing video players](media-services-develop-video-players.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="97ccc-137">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="97ccc-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="97ccc-138">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="97ccc-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
