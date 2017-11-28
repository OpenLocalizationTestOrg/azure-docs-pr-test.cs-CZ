---
title: "hello aaaScale média zpracování pomocí portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello škálování média zpracování pomocí hello portálu Azure."
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
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a><span data-ttu-id="42071-103">Změna hello vyhrazený typ jednotky.</span><span class="sxs-lookup"><span data-stu-id="42071-103">Change hello reserved unit type</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="42071-104">.NET</span><span class="sxs-lookup"><span data-stu-id="42071-104">.NET</span></span>](media-services-dotnet-encoding-units.md)
> * [<span data-ttu-id="42071-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42071-105">Portal</span></span>](media-services-portal-scale-media-processing.md)
> * [<span data-ttu-id="42071-106">REST</span><span class="sxs-lookup"><span data-stu-id="42071-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [<span data-ttu-id="42071-107">Java</span><span class="sxs-lookup"><span data-stu-id="42071-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="42071-108">PHP</span><span class="sxs-lookup"><span data-stu-id="42071-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="42071-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="42071-109">Overview</span></span>

<span data-ttu-id="42071-110">Účet Media Services je přidružen vyhrazené jednotky typu, který určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="42071-110">A Media Services account is associated with a Reserved Unit Type, which determines hello speed with which your media processing tasks are processed.</span></span> <span data-ttu-id="42071-111">Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**.</span><span class="sxs-lookup"><span data-stu-id="42071-111">You can pick between hello following reserved unit types: **S1**, **S2**, or **S3**.</span></span> <span data-ttu-id="42071-112">Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu.</span><span class="sxs-lookup"><span data-stu-id="42071-112">For example, hello same encoding job runs faster when you use hello **S2** reserved unit type compare toohello **S1** type.</span></span>

<span data-ttu-id="42071-113">Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných** (ruština).</span><span class="sxs-lookup"><span data-stu-id="42071-113">In addition toospecifying hello reserved unit type, you can specify tooprovision your account with **Reserved Units** (RUs).</span></span> <span data-ttu-id="42071-114">Hello počet zřízené RUs určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.</span><span class="sxs-lookup"><span data-stu-id="42071-114">hello number of provisioned RUs determines hello number of media tasks that can be processed concurrently in a given account.</span></span>

>[!NOTE]
><span data-ttu-id="42071-115">RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru.</span><span class="sxs-lookup"><span data-stu-id="42071-115">RUs work for parallelizing all media processing, including indexing jobs using Azure Media Indexer.</span></span> <span data-ttu-id="42071-116">Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.</span><span class="sxs-lookup"><span data-stu-id="42071-116">However, unlike encoding, indexing jobs do not get processed faster with faster reserved units.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42071-117">Ujistěte se, zda text hello tooreview [přehled](media-services-scale-media-processing-overview.md) tématu tooget Další informace o škálování média zpracování tématu.</span><span class="sxs-lookup"><span data-stu-id="42071-117">Make sure tooreview hello [overview](media-services-scale-media-processing-overview.md) topic tooget more information about scaling media processing topic.</span></span>
> 
> 

## <a name="scale-media-processing"></a><span data-ttu-id="42071-118">Škálování zpracování médií</span><span class="sxs-lookup"><span data-stu-id="42071-118">Scale media processing</span></span>
<span data-ttu-id="42071-119">toochange hello vyhrazené jednotky typu a hello počet jednotek rezervovaných hello následující:</span><span class="sxs-lookup"><span data-stu-id="42071-119">toochange hello reserved unit type and hello number of reserved units, do hello following:</span></span>

1. <span data-ttu-id="42071-120">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="42071-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="42071-121">V hello **nastavení** vyberte **jednotky rezervované pro média**.</span><span class="sxs-lookup"><span data-stu-id="42071-121">In hello **Settings** window, select **Media reserved units**.</span></span>
   
    <span data-ttu-id="42071-122">toochange hello počet jednotek rezervovaných pro hello vybrali jednotku rezervovanou typ, použijte hello **obsluhovat jednotky média** posuvníku.</span><span class="sxs-lookup"><span data-stu-id="42071-122">toochange hello number of reserved units for hello selected reserved unit type, use hello **Media Served Units** slider.</span></span>
   
    <span data-ttu-id="42071-123">toochange hello **vyhrazený typ jednotky**, stiskněte klávesu S1, S2 nebo S3.</span><span class="sxs-lookup"><span data-stu-id="42071-123">toochange hello **RESERVED UNIT TYPE**, press S1, S2, or S3.</span></span>
   
    ![Stránka procesorů](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. <span data-ttu-id="42071-125">Změny ULOŽTE hello stiskněte tlačítko toosave.</span><span class="sxs-lookup"><span data-stu-id="42071-125">Press hello SAVE button toosave your changes.</span></span>
   
    <span data-ttu-id="42071-126">Hello nové vyhrazené jednotky jsou přiděleny po stisknutí klávesy uložit.</span><span class="sxs-lookup"><span data-stu-id="42071-126">hello new reserved units are allocated when you press SAVE.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42071-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42071-127">Next steps</span></span>
<span data-ttu-id="42071-128">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="42071-128">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="42071-129">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="42071-129">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

