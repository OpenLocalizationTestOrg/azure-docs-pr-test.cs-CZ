---
title: "Kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede jednotlivými kroky kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a><span data-ttu-id="5244a-103">Kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5244a-103">Encode an asset using Media Encoder Standard with the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="5244a-104">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5244a-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="5244a-105">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5244a-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="5244a-106">Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům.</span><span class="sxs-lookup"><span data-stu-id="5244a-106">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="5244a-107">Služba Media Services podporuje následující technologie streamování s adaptivní přenosovou rychlostí: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="5244a-107">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="5244a-108">Příprava vašich videí pro streamování s adaptivní přenosovou rychlostí spočívá v zakódování zdrojového videa zdroje do souborů s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="5244a-108">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="5244a-109">Ke kódování vašich videí byste měli použít kodér **Media Encoder Standard**.</span><span class="sxs-lookup"><span data-stu-id="5244a-109">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="5244a-110">Služba Media Services také poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 více přenosovými rychlostmi ve formátech streamování: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste je museli znovu zabalit do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="5244a-110">Media Services also provides dynamic packaging which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to re-package into these streaming formats.</span></span> <span data-ttu-id="5244a-111">Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="5244a-111">With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="5244a-112">Pokud chcete využít výhod dynamického balení, musíte zdrojový soubor zakódovat do sady souborů MP4 s více přenosovými rychlostmi (postup kódování je uvedený dále v této části).</span><span class="sxs-lookup"><span data-stu-id="5244a-112">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="5244a-113">Škálování zpracování média, najdete v části [to](media-services-portal-scale-media-processing.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="5244a-113">To scale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-the-azure-portal"></a><span data-ttu-id="5244a-114">Zakódovat pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5244a-114">Encode with the Azure portal</span></span>
<span data-ttu-id="5244a-115">Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="5244a-115">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="5244a-116">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="5244a-116">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="5244a-117">V okně **Nastavení** vyberte **Assety**.</span><span class="sxs-lookup"><span data-stu-id="5244a-117">In the **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="5244a-118">V okně **Assety** vyberte asset, který chcete zakódovat.</span><span class="sxs-lookup"><span data-stu-id="5244a-118">In the **Assets** window, select the asset that you would like to encode.</span></span>
4. <span data-ttu-id="5244a-119">Stiskněte tlačítko **Kódovat**.</span><span class="sxs-lookup"><span data-stu-id="5244a-119">Press the **Encode** button.</span></span>
5. <span data-ttu-id="5244a-120">V okně **Kódovat asset** vyberte procesor „Media Encoder Standard“ a jedno z přednastavení.</span><span class="sxs-lookup"><span data-stu-id="5244a-120">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="5244a-121">Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5244a-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="5244a-122">Pokud se chystáte řídit, která předvolba kódování se použije, pamatujte, že je důležité vybrat předvolbu, která je nejvhodnější pro vaše vstupní video.</span><span class="sxs-lookup"><span data-stu-id="5244a-122">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="5244a-123">Pokud například více, že vaše vstupní video má rozlišení 1920 × 1080 pixelů, můžete použít přednastavení „H264 Multiple Bitrate 1080p“.</span><span class="sxs-lookup"><span data-stu-id="5244a-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="5244a-124">Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.</span><span class="sxs-lookup"><span data-stu-id="5244a-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="5244a-125">Pro snadnější správu máte možnost upravit název výstupního assetu a název úlohy.</span><span class="sxs-lookup"><span data-stu-id="5244a-125">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="5244a-127">Stiskněte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5244a-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="5244a-128">Další krok</span><span class="sxs-lookup"><span data-stu-id="5244a-128">Next step</span></span>
<span data-ttu-id="5244a-129">Kódování průběh úlohy pomocí portálu Azure, můžete sledovat, jak je popsáno v [to](media-services-portal-check-job-progress.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5244a-129">You can monitor encoding job progress with the Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="5244a-130">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="5244a-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5244a-131">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="5244a-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

