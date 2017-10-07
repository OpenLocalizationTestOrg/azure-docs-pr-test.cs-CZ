---
title: "aaaScale streamování koncové body pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello škálování koncových bodů streamování s hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: e466edf9232558b9e270f54ee2849cd9b22ad121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="1d63a-103">Škálování koncových bodů streamování s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1d63a-103">Scale streaming endpoints with hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="1d63a-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1d63a-104">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="1d63a-105">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1d63a-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="1d63a-106">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d63a-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="1d63a-107">Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="1d63a-107">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="1d63a-108">Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU).</span><span class="sxs-lookup"><span data-stu-id="1d63a-108">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="1d63a-109">koncový bod streamování Hello je možné rozšířit přidáním služby SUs.</span><span class="sxs-lookup"><span data-stu-id="1d63a-109">hello streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="1d63a-110">Každý SU poskytuje dodatečnou šířku pásma kapacity toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="1d63a-110">Each SU provides additional bandwidth capacity toohello application.</span></span> <span data-ttu-id="1d63a-111">Další informace o datových proudů typy koncových bodů a konfigurace CDN najdete v tématu hello [koncový bod streamování přehled](media-services-portal-manage-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="1d63a-111">For more information about streaming endpoint types and CDN configuration, see hello [Streaming Endpoint overview](media-services-portal-manage-streaming-endpoints.md) topic.</span></span>
 
<span data-ttu-id="1d63a-112">Toto téma ukazuje, jak tooscale koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="1d63a-112">This topic shows how tooscale a streaming endpoint.</span></span>

<span data-ttu-id="1d63a-113">Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).</span><span class="sxs-lookup"><span data-stu-id="1d63a-113">For information about pricing details, see [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107).</span></span>

## <a name="scale-streaming-endpoints"></a><span data-ttu-id="1d63a-114">Škálování koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="1d63a-114">Scale streaming endpoints</span></span>

<span data-ttu-id="1d63a-115">toochange hello počet jednotek streamování, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1d63a-115">toochange hello number of streaming units, do hello following:</span></span>

1. <span data-ttu-id="1d63a-116">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d63a-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="1d63a-117">V hello **nastavení** vyberte **koncové body streamování**.</span><span class="sxs-lookup"><span data-stu-id="1d63a-117">In hello **Settings** window, select **Streaming endpoints**.</span></span>
3. <span data-ttu-id="1d63a-118">Klikněte na koncový bod streamování hello, že chcete tooscale.</span><span class="sxs-lookup"><span data-stu-id="1d63a-118">Click on hello streaming endpoint that you want tooscale.</span></span> 

    [!NOTE] <span data-ttu-id="1d63a-119">Je možné škálovat jenom **Premium** koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="1d63a-119">You can only scale **Premium** streaming endpoints.</span></span>

4. <span data-ttu-id="1d63a-120">Přesuňte hello posuvníku toospecify hello počet jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="1d63a-120">Move hello slider toospecify hello number of streaming units.</span></span>

    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a><span data-ttu-id="1d63a-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d63a-122">Next steps</span></span>
<span data-ttu-id="1d63a-123">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="1d63a-123">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1d63a-124">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1d63a-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

