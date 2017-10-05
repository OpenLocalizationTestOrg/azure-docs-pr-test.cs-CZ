---
title: "Škálování streamování koncové body pomocí portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede jednotlivými kroky škálování koncových bodů streamování pomocí portálu Azure."
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
ms.openlocfilehash: 4bb891371e3fc802fa667688a88878db18e32422
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a><span data-ttu-id="d2b48-103">Škálování koncových bodů streamování pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2b48-103">Scale streaming endpoints with the Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="d2b48-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="d2b48-104">Overview</span></span>

> [!NOTE]
> <span data-ttu-id="d2b48-105">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d2b48-105">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="d2b48-106">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d2b48-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="d2b48-107">Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="d2b48-107">**Premium** streaming endpoints are suitable for advanced workloads, providing dedicated and scalable bandwidth capacity.</span></span> <span data-ttu-id="d2b48-108">Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU).</span><span class="sxs-lookup"><span data-stu-id="d2b48-108">Customers that have a **Premium** streaming endpoint, by default get one streaming unit (SU).</span></span> <span data-ttu-id="d2b48-109">Koncový bod streamování je možné škálovat přidáním jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="d2b48-109">The streaming endpoint can be scaled by adding SUs.</span></span> <span data-ttu-id="d2b48-110">Každá jednotka streamování poskytuje aplikaci další kapacitu šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="d2b48-110">Each SU provides additional bandwidth capacity to the application.</span></span> <span data-ttu-id="d2b48-111">Další informace o datových proudů typy koncových bodů a konfigurace CDN najdete v tématu [koncový bod streamování přehled](media-services-portal-manage-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="d2b48-111">For more information about streaming endpoint types and CDN configuration, see the [Streaming Endpoint overview](media-services-portal-manage-streaming-endpoints.md) topic.</span></span>
 
<span data-ttu-id="d2b48-112">Toto téma ukazuje, jak se škálovat koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="d2b48-112">This topic shows how to scale a streaming endpoint.</span></span>

<span data-ttu-id="d2b48-113">Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).</span><span class="sxs-lookup"><span data-stu-id="d2b48-113">For information about pricing details, see [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107).</span></span>

## <a name="scale-streaming-endpoints"></a><span data-ttu-id="d2b48-114">Škálování koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="d2b48-114">Scale streaming endpoints</span></span>

<span data-ttu-id="d2b48-115">Chcete-li změnit počet jednotek streamování, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="d2b48-115">To change the number of streaming units, do the following:</span></span>

1. <span data-ttu-id="d2b48-116">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="d2b48-116">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="d2b48-117">V **nastavení** vyberte **koncové body streamování**.</span><span class="sxs-lookup"><span data-stu-id="d2b48-117">In the **Settings** window, select **Streaming endpoints**.</span></span>
3. <span data-ttu-id="d2b48-118">Klikněte na koncový bod streamování, kterou chcete škálovat.</span><span class="sxs-lookup"><span data-stu-id="d2b48-118">Click on the streaming endpoint that you want to scale.</span></span> 

    [!NOTE] <span data-ttu-id="d2b48-119">Je možné škálovat jenom **Premium** koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="d2b48-119">You can only scale **Premium** streaming endpoints.</span></span>

4. <span data-ttu-id="d2b48-120">Nastavte posuvník zadat počet jednotek streamování.</span><span class="sxs-lookup"><span data-stu-id="d2b48-120">Move the slider to specify the number of streaming units.</span></span>

    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a><span data-ttu-id="d2b48-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2b48-122">Next steps</span></span>
<span data-ttu-id="d2b48-123">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="d2b48-123">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d2b48-124">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d2b48-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

