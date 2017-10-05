---
title: "Konfigurace místních kodérů při vytvářejí proudy s více přenosovými rychlostmi pomocí Azure Media Services | Microsoft Docs"
description: "Toto téma uvádí místními kodéry, které můžete použít k zachycení živé události a odesílat živý datový proud jednou přenosovou rychlostí do AMS kanálů, (které jsou kódování v reálném čase povolit) pro další zpracování. Téma obsahuje odkazy na návodů, které ukazují, jak nakonfigurovat uvedené kodérů."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0ec6f046-0841-4673-9057-883bdbc30d5c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 68aaf0fda2e60c5736ab020c15e516144e11d68e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a><span data-ttu-id="c350f-104">Postup konfigurace místních kodérů při vytvářejí proudy s více přenosovými rychlostmi pomocí Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="c350f-104">How to configure on-premises encoders when using Azure Media Services to create multi-bitrate streams</span></span>
<span data-ttu-id="c350f-105">Toto téma uvádí místními kodéry, které můžete použít k zachycení živé události a odesílat živý datový proud jednou přenosovou rychlostí do AMS kanálů, (které jsou kódování v reálném čase povolit) pro další zpracování.</span><span class="sxs-lookup"><span data-stu-id="c350f-105">This topic lists on-premises live encoders that you can use to capture your live events and send a single bitrate live stream to AMS channels (that are live encoding enabled) for further processing.</span></span> <span data-ttu-id="c350f-106">V tématu taky obsahuje odkazy na návodů, které ukazují, jak nakonfigurovat uvedené kodérů.</span><span class="sxs-lookup"><span data-stu-id="c350f-106">The topic also links to tutorials that show how to configure listed encoders.</span></span>

## <a name="elemental-live"></a><span data-ttu-id="c350f-107">Elemental za provozu</span><span class="sxs-lookup"><span data-stu-id="c350f-107">Elemental Live</span></span>
<span data-ttu-id="c350f-108">Informace o tom, jak nakonfigurovat [elementární Live](http://www.elementaltechnologies.com/products/elemental-live) najdete v části ke odesílat živý datový proud s jednou přenosovou rychlostí do kanálu AMS [konfigurace elementární Live](media-services-configure-elemental-live-encoder.md).</span><span class="sxs-lookup"><span data-stu-id="c350f-108">For information on how to configure the [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder to send a single bitrate live stream to an AMS Channel, see [Configuring Elemental Live](media-services-configure-elemental-live-encoder.md).</span></span>

## <a name="flash-media-live-encoder"></a><span data-ttu-id="c350f-109">Flash Media Encoder za provozu</span><span class="sxs-lookup"><span data-stu-id="c350f-109">Flash Media Live Encoder</span></span>
<span data-ttu-id="c350f-110">Informace o tom, jak nakonfigurovat [Flash Live kodér médií](http://www.adobe.com/products/flash-media-encoder.html) najdete v části encoder (FMLE) k odeslání živý datový proud s jednou přenosovou rychlostí do kanálu AMS [konfigurace FMLE](media-services-configure-fmle-live-encoder.md) .</span><span class="sxs-lookup"><span data-stu-id="c350f-110">For information on how to configure the [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder to send a single bitrate live stream to an AMS Channel, see [Configuring FMLE](media-services-configure-fmle-live-encoder.md) .</span></span>

## <a name="telestream-wirecast"></a><span data-ttu-id="c350f-111">Kodér Telestream Wirecast</span><span class="sxs-lookup"><span data-stu-id="c350f-111">Telestream Wirecast</span></span>
<span data-ttu-id="c350f-112">Informace o tom, jak nakonfigurovat [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) najdete v části ke odesílat živý datový proud s jednou přenosovou rychlostí do kanálu AMS [konfigurace Wirecast](media-services-configure-wirecast-live-encoder.md).</span><span class="sxs-lookup"><span data-stu-id="c350f-112">For information on how to configure the [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) encoder to send a single bitrate live stream to an AMS Channel, see [Configuring Wirecast](media-services-configure-wirecast-live-encoder.md).</span></span>

## <a name="newtek-tricaster"></a><span data-ttu-id="c350f-113">NewTek čase</span><span class="sxs-lookup"><span data-stu-id="c350f-113">NewTek TriCaster</span></span>
<span data-ttu-id="c350f-114">Informace o tom, jak nakonfigurovat [čase](http://newtek.com/products/tricaster-40.html) najdete v části ke odesílat živý datový proud s jednou přenosovou rychlostí do kanálu AMS [konfigurace čase](media-services-configure-tricaster-live-encoder.md).</span><span class="sxs-lookup"><span data-stu-id="c350f-114">For information on how to configure the [Tricaster](http://newtek.com/products/tricaster-40.html) encoder to send a single bitrate live stream to an AMS Channel, see [Configuring Tricaster](media-services-configure-tricaster-live-encoder.md).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c350f-115">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c350f-115">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c350f-116">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c350f-116">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="c350f-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="c350f-117">See also</span></span>
<span data-ttu-id="c350f-118">[Živé streamování využívající Azure Media Services k vytvářejí proudy s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="c350f-118">[Live streaming using Azure Media Services to create multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).</span></span>

