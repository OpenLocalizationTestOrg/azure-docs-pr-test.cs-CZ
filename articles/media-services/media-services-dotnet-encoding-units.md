---
title: "média aaaScale zpracování přidáním kódování jednotky – Azure |  Microsoft Docs"
description: "Zjistěte, jak toohow tooadd kódování jednotky s rozhraním .NET"
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
ms.openlocfilehash: b9f71a6487c5d136319a38a1598d60edfaa81b9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-encoding-with-net-sdk"></a><span data-ttu-id="e5df2-103">Jak tooscale kódování pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e5df2-103">How tooscale encoding with .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5df2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e5df2-104">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="e5df2-105">.NET</span><span class="sxs-lookup"><span data-stu-id="e5df2-105">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="e5df2-106">REST</span><span class="sxs-lookup"><span data-stu-id="e5df2-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="e5df2-107">Java</span><span class="sxs-lookup"><span data-stu-id="e5df2-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="e5df2-108">PHP</span><span class="sxs-lookup"><span data-stu-id="e5df2-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="e5df2-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="e5df2-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e5df2-110">Ujistěte se, zda text hello tooreview [přehled](media-services-scale-media-processing-overview.md) tématu tooget Další informace o škálování média zpracování tématu.</span><span class="sxs-lookup"><span data-stu-id="e5df2-110">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

<span data-ttu-id="e5df2-111">toochange hello vyhrazené jednotky typu a hello číslo jednotky rezervované pro kódování pomocí sady .NET SDK hello následující:</span><span class="sxs-lookup"><span data-stu-id="e5df2-111">toochange hello reserved unit type and hello number of encoding reserved units using .NET SDK, do hello following:</span></span>

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds tooS1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a><span data-ttu-id="e5df2-112">Otevření lístku podpory</span><span class="sxs-lookup"><span data-stu-id="e5df2-112">Opening a Support Ticket</span></span>
<span data-ttu-id="e5df2-113">Ve výchozím nastavení každých účtu Media Services můžete škálovat tooup too25 kódování a 5 na vyžádání jednotek rezervovaných pro streamování.</span><span class="sxs-lookup"><span data-stu-id="e5df2-113">By default every Media Services account can scale tooup too25 Encoding and 5 On-Demand Streaming Reserved Units.</span></span> <span data-ttu-id="e5df2-114">Vyšší limit můžete požádat tak, že otevřete lístek podpory.</span><span class="sxs-lookup"><span data-stu-id="e5df2-114">You can request a higher limit by opening a support ticket.</span></span>

### <a name="open-a-support-ticket"></a><span data-ttu-id="e5df2-115">Otevřete lístek podpory</span><span class="sxs-lookup"><span data-stu-id="e5df2-115">Open a support ticket</span></span>
<span data-ttu-id="e5df2-116">tooopen lístek podpory hello následující:</span><span class="sxs-lookup"><span data-stu-id="e5df2-116">tooopen a support ticket do hello following:</span></span>

1. <span data-ttu-id="e5df2-117">Klikněte na tlačítko [získat podporu](https://manage.windowsazure.com/?getsupport=true).</span><span class="sxs-lookup"><span data-stu-id="e5df2-117">Click [Get Support](https://manage.windowsazure.com/?getsupport=true).</span></span> <span data-ttu-id="e5df2-118">Pokud nejste přihlášeni, budete mít výzvami tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e5df2-118">If you are not logged in, you will be prompted tooenter your credentials.</span></span>
2. <span data-ttu-id="e5df2-119">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="e5df2-119">Select your subscription.</span></span>
3. <span data-ttu-id="e5df2-120">V rámci podpory typu vyberte "Technical".</span><span class="sxs-lookup"><span data-stu-id="e5df2-120">Under support type, select "Technical".</span></span>
4. <span data-ttu-id="e5df2-121">Kliknutím na tlačítko "Vytvoření lístku".</span><span class="sxs-lookup"><span data-stu-id="e5df2-121">Click on "Create Ticket".</span></span>
5. <span data-ttu-id="e5df2-122">Vyberte "Azure Media Services" v seznamu produktů hello uvedené na další stránku hello.</span><span class="sxs-lookup"><span data-stu-id="e5df2-122">Select "Azure Media Services" in hello product list presented on hello next page.</span></span>
6. <span data-ttu-id="e5df2-123">Vyberte typ problému"", je vhodné pro váš problém.</span><span class="sxs-lookup"><span data-stu-id="e5df2-123">Select a "Problem type" that is appropriate for your issue.</span></span>
7. <span data-ttu-id="e5df2-124">Klikněte na tlačítko Pokračovat.</span><span class="sxs-lookup"><span data-stu-id="e5df2-124">Click Continue.</span></span>
8. <span data-ttu-id="e5df2-125">Postupujte podle pokynů na další stránce a potom zadejte podrobnosti o problému.</span><span class="sxs-lookup"><span data-stu-id="e5df2-125">Follow instructions on next page and then enter details about your issue.</span></span>
9. <span data-ttu-id="e5df2-126">Klikněte na tlačítko pro odeslání tooopen hello lístku.</span><span class="sxs-lookup"><span data-stu-id="e5df2-126">Click submit tooopen hello ticket.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="e5df2-127">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e5df2-127">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e5df2-128">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e5df2-128">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

