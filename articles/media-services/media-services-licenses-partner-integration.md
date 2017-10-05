---
title: "Pomocí partnery pro doručování licence na Widevine do služby Azure Media Services | Microsoft Docs"
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) k poskytování datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. Licence PlayReady pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5bcad5a4-c0bb-4871-9cce-808a913c53e6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 6867e4f910970121df3858516c6bab3114c3c6f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a><span data-ttu-id="cda98-104">Využití služeb partnerů k distribuci licencí Widevine pro Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="cda98-104">Using partners to deliver Widevine licenses to Azure Media Services</span></span>
## <a name="overview"></a><span data-ttu-id="cda98-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="cda98-105">Overview</span></span>
<span data-ttu-id="cda98-106">Microsoft Azure Media Services umožňuje doručovat MPEG-DASH, které jsou chráněné pomocí Widevine DRM, který se zašifrují podle specifikace Common Encryption (CENC).</span><span class="sxs-lookup"><span data-stu-id="cda98-106">Microsoft Azure Media Services enables you to deliver MPEG-DASH protected with Widevine DRM, which is encrypted per the Common Encryption (CENC) specification.</span></span>

<span data-ttu-id="cda98-107">Spuštění pomocí .NET SDK služby Media Services verze 3.5.2, Media Services umožňuje konfigurovat šablonu licence Widevine a získání licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="cda98-107">Starting with the Media Services .NET SDK version 3.5.2, Media Services enables you to configure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="cda98-108">Licence Widevine vám také mohou doručit následující partneři AMS : [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="cda98-108">You can also use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

## <a name="castlabs"></a><span data-ttu-id="cda98-109">castLabs</span><span class="sxs-lookup"><span data-stu-id="cda98-109">castLabs</span></span>
<span data-ttu-id="cda98-110">Můžete použít [castLabs](http://castlabs.com/company/partners/azure/) pro doručování licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="cda98-110">You can use [castLabs](http://castlabs.com/company/partners/azure/) to deliver Widevine licenses.</span></span> <span data-ttu-id="cda98-111">Další informace najdete v tématu [pomocí castLabs k poskytování DRM licence k Azure Media Services](media-services-castlabs-integration.md)</span><span class="sxs-lookup"><span data-stu-id="cda98-111">For more information, see [Using castLabs to deliver DRM licenses to Azure Media Services](media-services-castlabs-integration.md)</span></span>

## <a name="axinom"></a><span data-ttu-id="cda98-112">Axinom</span><span class="sxs-lookup"><span data-stu-id="cda98-112">Axinom</span></span>
<span data-ttu-id="cda98-113">Můžete použít [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) pro doručování licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="cda98-113">You can use [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) to deliver Widevine licenses.</span></span> <span data-ttu-id="cda98-114">Další informace najdete v tématu [pomocí Axinom k poskytování DRM licence k Azure Media Services](media-services-axinom-integration.md)</span><span class="sxs-lookup"><span data-stu-id="cda98-114">For more information, see [Using Axinom to deliver DRM licenses to Azure Media Services](media-services-axinom-integration.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="cda98-115">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="cda98-115">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cda98-116">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="cda98-116">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="cda98-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="cda98-117">See also</span></span>
[<span data-ttu-id="cda98-118">Použití běžného dynamického šifrování PlayReady nebo Widevine</span><span class="sxs-lookup"><span data-stu-id="cda98-118">Using PlayReady and/or Widevine dynamic common encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="cda98-119">Blog na Mingfei</span><span class="sxs-lookup"><span data-stu-id="cda98-119">Mingfei’s blog</span></span>](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

