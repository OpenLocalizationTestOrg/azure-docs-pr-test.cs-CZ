---
title: "Škálování média zpracování přidáním kódování jednotky – Azure |  Microsoft Docs"
description: "Další postup přidání kódování jednotky s rozhraním .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;milangada;
ms.openlocfilehash: 72a8729d22a9e76c8076d7a3347619a2163e4f09
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a><span data-ttu-id="fcac2-103">Jak škálovat kódování pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="fcac2-103">How to scale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fcac2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fcac2-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="fcac2-105">.NET</span><span class="sxs-lookup"><span data-stu-id="fcac2-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="fcac2-106">REST</span><span class="sxs-lookup"><span data-stu-id="fcac2-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="fcac2-107">Java</span><span class="sxs-lookup"><span data-stu-id="fcac2-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="fcac2-108">PHP</span><span class="sxs-lookup"><span data-stu-id="fcac2-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="fcac2-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="fcac2-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fcac2-110">Projděte si [přehled](media-services-scale-media-processing-overview.md) tématu, které chcete získat další informace o škálování média zpracování tématu.</span><span class="sxs-lookup"><span data-stu-id="fcac2-110">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="fcac2-111">Chcete-li změnit typ jednotku rezervovanou a počet jednotky rezervované pro kódování pomocí sady .NET SDK, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fcac2-111">To change the reserved unit type and the number of encoding reserved units using .NET SDK, do the following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="fcac2-112">Otevření lístku podpory</span><span class="sxs-lookup"><span data-stu-id="fcac2-112">Opening a Support Ticket</span></span>
<span data-ttu-id="fcac2-113">Ve výchozím nastavení každých účtu Media Services můžete škálovat až 25 kódování a 5 na vyžádání jednotek rezervovaných pro streamování.</span><span class="sxs-lookup"><span data-stu-id="fcac2-113">By default every Media Services account can scale to up to 25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="fcac2-114">Vyšší limit můžete požádat tak, že otevřete lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="fcac2-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="fcac2-115">Otevřete lístek podpory</span><span class="sxs-lookup"><span data-stu-id="fcac2-115">Open a support ticket</span></span>
<span data-ttu-id="fcac2-116">Chcete-li otevřít podporu lístku takto:</span><span class="sxs-lookup"><span data-stu-id="fcac2-116">To open a support ticket do the following:</span></span>

1. <span data-ttu-id="fcac2-117">Klikněte na tlačítko [získat podporu](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="fcac2-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="fcac2-118">Pokud nejste přihlášeni, budete vyzváni k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="fcac2-118">If you are not logged in, you will be prompted to enter your credentials.</span></span>
2. <span data-ttu-id="fcac2-119">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="fcac2-119">Select your subscription.</span></span>
3. <span data-ttu-id="fcac2-120">V rámci podpory typu vyberte "Technical".</span><span class="sxs-lookup"><span data-stu-id="fcac2-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="fcac2-121">Kliknutím na tlačítko "Vytvoření lístku".</span><span class="sxs-lookup"><span data-stu-id="fcac2-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="fcac2-122">Vyberte "Azure Media Services" v seznamu produktu uvedené na další stránku.</span><span class="sxs-lookup"><span data-stu-id="fcac2-122">Select "Azure Media Services" in the product list presented on the next page.</span></span>
6. <span data-ttu-id="fcac2-123">Vyberte typ problému"", je vhodné pro váš problém.</span><span class="sxs-lookup"><span data-stu-id="fcac2-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="fcac2-124">Klikněte na tlačítko Pokračovat.</span><span class="sxs-lookup"><span data-stu-id="fcac2-124">Click Continue.</span></span>
8. <span data-ttu-id="fcac2-125">Postupujte podle pokynů na další stránce a potom zadejte podrobnosti o problému.</span><span class="sxs-lookup"><span data-stu-id="fcac2-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="fcac2-126">Klikněte na tlačítko Odeslat otevřete lístek.</span><span class="sxs-lookup"><span data-stu-id="fcac2-126">Click submit to open the ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="fcac2-127">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="fcac2-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fcac2-128">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="fcac2-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

