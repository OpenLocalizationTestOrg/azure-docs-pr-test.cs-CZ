---
title: "Škálování zpracování média pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu vás provede jednotlivými kroky škálování média zpracování pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 46ca29d3e66701f2abcb185791089e94761984e8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="change-the-reserved-unit-type"></a><span data-ttu-id="9af6f-103">Změna typu rezervované jednotky</span><span class="sxs-lookup"><span data-stu-id="9af6f-103">Change the reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9af6f-104">.NET</span><span class="sxs-lookup"><span data-stu-id="9af6f-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="9af6f-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9af6f-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="9af6f-106">REST</span><span class="sxs-lookup"><span data-stu-id="9af6f-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="9af6f-107">Java</span><span class="sxs-lookup"><span data-stu-id="9af6f-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="9af6f-108">PHP</span><span class="sxs-lookup"><span data-stu-id="9af6f-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="9af6f-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="9af6f-109">Overview</span></span>

<span data-ttu-id="9af6f-110">Účet Media Services je přidružený k typu rezervované jednotky, který určuje rychlost zpracování vašich úloh zpracování médií.</span><span class="sxs-lookup"><span data-stu-id="9af6f-110">A Media Services account is associated with a Reserved Unit Type, which determines the speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="9af6f-111">Můžete si vybrat mezi následujícími typy rezervovaných jednotek: **S1**, **S2** nebo **S3**.</span><span class="sxs-lookup"><span data-stu-id="9af6f-111">You can pick between the following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="9af6f-112">Například stejná úloha kódování bude rychlejší, když použijete typ rezervované jednotky **S2**, než kdybyste použili typ **S1**.</span><span class="sxs-lookup"><span data-stu-id="9af6f-112">For example, the same encoding job runs faster when you use the **S2** reserved unit type compare to the **S1** type.</span></span>

<span data-ttu-id="9af6f-113">Kromě určení typu rezervované jednotky můžete určit, že chcete účet zřídit s **rezervovanými jednotkami** (RU).</span><span class="sxs-lookup"><span data-stu-id="9af6f-113">In addition to specifying the reserved unit type, you can specify to provision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="9af6f-114">Počet zřízených RU určuje počet úloh médií, které je možné v daném účtu zpracovávat současně.</span><span class="sxs-lookup"><span data-stu-id="9af6f-114">The number of provisioned RUs determines the number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="9af6f-115">RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru.</span><span class="sxs-lookup"><span data-stu-id="9af6f-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="9af6f-116">Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.</span><span class="sxs-lookup"><span data-stu-id="9af6f-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9af6f-117">Projděte si [přehled](media-services-scale-media-processing-overview.md) tématu, které chcete získat další informace o škálování média zpracování tématu.</span><span class="sxs-lookup"><span data-stu-id="9af6f-117">Make sure to review the [overview](media-services-scale-media-processing-overview.md) topic to get more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="9af6f-118">Škálování zpracování médií</span><span class="sxs-lookup"><span data-stu-id="9af6f-118">Scale media processing</span></span>
<span data-ttu-id="9af6f-119">Chcete-li změnit typ jednotku rezervovanou a počet jednotek rezervovaných, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="9af6f-119">To change the reserved unit type and the number of reserved units, do the following:</span></span>

1. <span data-ttu-id="9af6f-120">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="9af6f-120">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="9af6f-121">V **nastavení** vyberte **jednotky rezervované pro média**.</span><span class="sxs-lookup"><span data-stu-id="9af6f-121">In the **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="9af6f-122">Chcete-li změnit počet jednotek rezervovaných pro vybranou jednotku rezervovanou typ, použijte **obsluhovat jednotky média** posuvníku.</span><span class="sxs-lookup"><span data-stu-id="9af6f-122">To change the number of reserved units for the selected reserved unit type, use the **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="9af6f-123">Chcete-li změnit **vyhrazený typ jednotky**, stiskněte klávesu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="9af6f-123">To change the **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Stránka procesorů](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="9af6f-125">Stisknutím tlačítka ULOŽIT uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="9af6f-125">Press the SAVE button to save your changes.</span></span>
   
    <span data-ttu-id="9af6f-126">Nové vyhrazené jednotky jsou přiděleny po stisknutí klávesy uložit.</span><span class="sxs-lookup"><span data-stu-id="9af6f-126">The new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9af6f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9af6f-127">Next steps</span></span>
<span data-ttu-id="9af6f-128">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="9af6f-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9af6f-129">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9af6f-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

