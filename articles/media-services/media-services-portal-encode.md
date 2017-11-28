---
title: "aaaEncode assetu pomocí kodéru Media Encoder Standard s hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello kódování assetu pomocí kodéru Media Encoder Standard s hello portálu Azure."
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
ms.openlocfilehash: 0d118bbbe1fa9f4ba0bfa3ea3b10fb541d1d6379
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-hello-azure-portal"></a><span data-ttu-id="5e786-103">Kódování assetu pomocí kodéru Media Encoder Standard s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5e786-103">Encode an asset using Media Encoder Standard with hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="5e786-104">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5e786-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="5e786-105">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e786-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

<span data-ttu-id="5e786-106">Při práci se službou Azure Media Services je jedním nejběžnější scénářů hello doručování tooyour klienti streamování s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="5e786-106">When working with Azure Media Services one of hello most common scenarios is delivering adaptive bitrate streaming tooyour clients.</span></span> <span data-ttu-id="5e786-107">Služba Media Services podporuje následující adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="5e786-107">Media Services supports hello following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="5e786-108">tooprepare videa pro streamování s adaptivní přenosovou rychlostí, je nutné tooencode svůj zdroj videa do souborů s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="5e786-108">tooprepare your videos for adaptive bitrate streaming, you need tooencode your source video into multi-bitrate files.</span></span> <span data-ttu-id="5e786-109">Měli byste použít hello **Media Encoder Standard** tooencode kodér videa.</span><span class="sxs-lookup"><span data-stu-id="5e786-109">You should use hello **Media Encoder Standard** encoder tooencode your videos.</span></span>  

<span data-ttu-id="5e786-110">Služba Media Services také poskytuje dynamické balení, což vám umožní toodeliver vaše soubory MP4 více přenosovými rychlostmi v hello následující streamování formáty: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste museli toore balíček do těchto formátů streamování.</span><span class="sxs-lookup"><span data-stu-id="5e786-110">Media Services also provides dynamic packaging which allows you toodeliver your multi-bitrate MP4s in hello following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having toore-package into these streaming formats.</span></span> <span data-ttu-id="5e786-111">Při dynamickém balení stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="5e786-111">With dynamic packaging you only need toostore and pay for hello files in single storage format and Media Services will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="5e786-112">tootake výhod dynamického balení, je nutné tooencode zdrojového souboru do sady souborů MP4 s více přenosovými rychlostmi (postup hello kódování je ukázán později v této části).</span><span class="sxs-lookup"><span data-stu-id="5e786-112">tootake advantage of dynamic packaging, you need tooencode your source file into a set of multi-bitrate MP4 files (hello encoding steps are demonstrated later in this section).</span></span>

<span data-ttu-id="5e786-113">média tooscale zpracování, najdete v části [to](media-services-portal-scale-media-processing.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="5e786-113">tooscale media processing, see [this](media-services-portal-scale-media-processing.md) topic.</span></span>

## <a name="encode-with-hello-azure-portal"></a><span data-ttu-id="5e786-114">Kódování s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5e786-114">Encode with hello Azure portal</span></span>
<span data-ttu-id="5e786-115">Tato část popisuje kroky hello tooencode může trvat svůj obsah pomocí procesoru Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="5e786-115">This section describes hello steps you can take tooencode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="5e786-116">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="5e786-116">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="5e786-117">V hello **nastavení** vyberte **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="5e786-117">In hello **Settings** window, select **Assets**.</span></span>  
3. <span data-ttu-id="5e786-118">V hello **prostředky** okno, vyberte hello asset, které chcete tooencode.</span><span class="sxs-lookup"><span data-stu-id="5e786-118">In hello **Assets** window, select hello asset that you would like tooencode.</span></span>
4. <span data-ttu-id="5e786-119">Stiskněte klávesu hello **kódovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5e786-119">Press hello **Encode** button.</span></span>
5. <span data-ttu-id="5e786-120">V hello **kódovat asset** oken, vyberte hello procesor "Media Encoder Standard" a jedno z přednastavení.</span><span class="sxs-lookup"><span data-stu-id="5e786-120">In hello **Encode an asset** window, select hello "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="5e786-121">Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e786-121">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="5e786-122">Pokud máte v plánu toocontrol které předvolby kódování se používá, mějte na paměti: je důležité tooselect hello přednastavení, která je nejvhodnější pro vaše vstupní video.</span><span class="sxs-lookup"><span data-stu-id="5e786-122">If you plan toocontrol which encoding preset is used, keep this in mind: it is important tooselect hello preset that is most appropriate for your input video.</span></span> <span data-ttu-id="5e786-123">Například pokud znáte vaše vstupní video má rozlišení 1920 × 1080 pixelů, pak můžete použít hello "H264 Multiple Bitrate 1080p" přednastavené.</span><span class="sxs-lookup"><span data-stu-id="5e786-123">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use hello "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="5e786-124">Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.</span><span class="sxs-lookup"><span data-stu-id="5e786-124">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="5e786-125">Pro snadnější správu máte možnost úpravy hello název hello výstupní asset a název hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="5e786-125">For easier management, you have an option of editing hello name of hello output asset, and hello name of hello job.</span></span>
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. <span data-ttu-id="5e786-127">Stiskněte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5e786-127">Press **Create**.</span></span>

## <a name="next-step"></a><span data-ttu-id="5e786-128">Další krok</span><span class="sxs-lookup"><span data-stu-id="5e786-128">Next step</span></span>
<span data-ttu-id="5e786-129">Průběhu úlohy kódování s hello portál Azure, můžete sledovat, jak je popsáno v [to](media-services-portal-check-job-progress.md) článku.</span><span class="sxs-lookup"><span data-stu-id="5e786-129">You can monitor encoding job progress with hello Azure portal, as described in [this](media-services-portal-check-job-progress.md) article.</span></span>  

## <a name="media-services-learning-paths"></a><span data-ttu-id="5e786-130">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="5e786-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5e786-131">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="5e786-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

