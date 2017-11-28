---
title: aaaUsing partnery toodeliver Widevine licence tooAzure Media Services | Microsoft Docs
description: "Tento článek popisuje, jak můžete použít Azure Media Services (AMS) toodeliver datový proud, který je dynamicky šifrovat pomocí PlayReady a Widevine technologiemi DRM AMS. licence PlayReady Hello pochází z Media Services PlayReady licenčního serveru a licence Widevine doručuje castLabs licenční server."
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
ms.openlocfilehash: 3c18a8a22ced239931dea5385020194bd6d83f28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-partners-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="667ad-104">Pomocí partnery toodeliver Widevine licence tooAzure Media Services</span><span class="sxs-lookup"><span data-stu-id="667ad-104">Using partners toodeliver Widevine licenses tooAzure Media Services</span></span>
## <a name="overview"></a><span data-ttu-id="667ad-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="667ad-105">Overview</span></span>
<span data-ttu-id="667ad-106">Microsoft Azure Media Services umožňuje vám toodeliver MPEG-DASH chránit pomocí Widevine DRM, který se zašifrují podle hello specifikace Common Encryption (CENC).</span><span class="sxs-lookup"><span data-stu-id="667ad-106">Microsoft Azure Media Services enables you toodeliver MPEG-DASH protected with Widevine DRM, which is encrypted per hello Common Encryption (CENC) specification.</span></span>

<span data-ttu-id="667ad-107">Počínaje hello sady Media Services .NET SDK verze 3.5.2, Media Services umožňuje vám tooconfigure Widevine šablona licence a získání licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="667ad-107">Starting with hello Media Services .NET SDK version 3.5.2, Media Services enables you tooconfigure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="667ad-108">Můžete také použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="667ad-108">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

## <a name="castlabs"></a><span data-ttu-id="667ad-109">castLabs</span><span class="sxs-lookup"><span data-stu-id="667ad-109">castLabs</span></span>
<span data-ttu-id="667ad-110">Můžete použít [castLabs](http://castlabs.com/company/partners/azure/) toodeliver licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="667ad-110">You can use [castLabs](http://castlabs.com/company/partners/azure/) toodeliver Widevine licenses.</span></span> <span data-ttu-id="667ad-111">Další informace najdete v tématu [pomocí castLabs toodeliver DRM licence tooAzure Media Services](media-services-castlabs-integration.md)</span><span class="sxs-lookup"><span data-stu-id="667ad-111">For more information, see [Using castLabs toodeliver DRM licenses tooAzure Media Services](media-services-castlabs-integration.md)</span></span>

## <a name="axinom"></a><span data-ttu-id="667ad-112">Axinom</span><span class="sxs-lookup"><span data-stu-id="667ad-112">Axinom</span></span>
<span data-ttu-id="667ad-113">Můžete použít [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) toodeliver licence na Widevine.</span><span class="sxs-lookup"><span data-stu-id="667ad-113">You can use [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) toodeliver Widevine licenses.</span></span> <span data-ttu-id="667ad-114">Další informace najdete v tématu [toodeliver Axinom pomocí DRM licence tooAzure Media Services](media-services-axinom-integration.md)</span><span class="sxs-lookup"><span data-stu-id="667ad-114">For more information, see [Using Axinom toodeliver DRM licenses tooAzure Media Services](media-services-axinom-integration.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="667ad-115">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="667ad-115">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="667ad-116">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="667ad-116">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="667ad-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="667ad-117">See also</span></span>
[<span data-ttu-id="667ad-118">Použití běžného dynamického šifrování PlayReady nebo Widevine</span><span class="sxs-lookup"><span data-stu-id="667ad-118">Using PlayReady and/or Widevine dynamic common encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="667ad-119">Blog na Mingfei</span><span class="sxs-lookup"><span data-stu-id="667ad-119">Mingfei’s blog</span></span>](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

