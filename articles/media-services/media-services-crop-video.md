---
title: aaaHow toocrop videa s Media Encoder Standard - Azure | Microsoft Docs
description: "Tento článek ukazuje, jak toocrop videa pomocí procesoru Media Encoder Standard."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 2b4ac3d96228b93c890a38c57c4913988de1e8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a><span data-ttu-id="ac0b0-103">Oříznutí videa pomocí kodéru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="ac0b0-103">Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="ac0b0-104">Toocrop Media Encoder Standard (MES) můžete použít váš vstup videa.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-104">You can use Media Encoder Standard (MES) toocrop your input video.</span></span> <span data-ttu-id="ac0b0-105">Oříznutí je proces hello kódování právě hello pixelů v rámci dané okno a výběrem obdélníkový okno v rámci video hello.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-105">Cropping is hello process of selecting a rectangular window within hello video frame, and encoding just hello pixels within that window.</span></span> <span data-ttu-id="ac0b0-106">Následující diagram Hello pomáhá hello proces znázorněn.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-106">hello following diagram helps illustrate hello process.</span></span>

![Oříznutí video](./media/media-services-crop-video/media-services-crop-video01.png)

<span data-ttu-id="ac0b0-108">Předpokládejme, že máte jako vstupní video, které má rozlišení 1920 × 1080 pixelů (poměr stran 16:9), ale má černé pruhy (pilíře polí) na hello vlevo a vpravo, tak, aby pouze okno 4:3 nebo 1440 × 1080 pixelů obsahuje aktivní video.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-108">Suppose you have as input a video that has a resolution of 1920x1080 pixels (16:9 aspect ratio), but has black bars (pillar boxes) at hello left and right, so that only a 4:3 window or 1440x1080 pixels contains active video.</span></span> <span data-ttu-id="ac0b0-109">Můžete použít MES toocrop nebo upravte si hello černým řádky a kódování hello 1440 × 1080 oblast.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-109">You can use MES toocrop or edit out hello black bars, and encode hello 1440x1080 region.</span></span>

<span data-ttu-id="ac0b0-110">Oříznutí v MES je předběžného zpracování fáze, abyste hello oříznutí parametry v kódování přednastavených hello použít toohello původní vstupní video.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-110">Cropping in MES is a pre-processing stage, so hello cropping parameters in hello encoding preset apply toohello original input video.</span></span> <span data-ttu-id="ac0b0-111">Kódování je další fázi a použít nastavení šířky a výšky hello toohello *předem zpracovaných* video a není toohello původní video.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-111">Encoding is a subsequent stage, and hello width/height settings apply toohello *pre-processed* video, and not toohello original video.</span></span> <span data-ttu-id="ac0b0-112">Při navrhování vaší přednastavených budete potřebovat následující hello toodo: (a) vyberte hello ořezové parametry podle hello původní vstupní video a (b) vyberte vaše kódování nastavení podle hello oříznout video.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-112">When designing your preset you need toodo hello following: (a) select hello crop parameters based on hello original input video, and (b) select your encode settings based on hello cropped video.</span></span> <span data-ttu-id="ac0b0-113">Pokud se neshodují vaše kódování toohello nastavení oříznout video, výstup hello nebudou podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-113">If you do not match your encode settings toohello cropped video, hello output will not be as you expect.</span></span>

<span data-ttu-id="ac0b0-114">Hello [následující](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) téma ukazuje, jak toocreate úlohu kódování s MES a jak toospecify vlastní předvolby pro hello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-114">hello [following](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic shows how toocreate an encoding job with MES and how toospecify a custom preset for hello encoding task.</span></span> 

## <a name="creating-a-custom-preset"></a><span data-ttu-id="ac0b0-115">Vytváření vlastní předvolby</span><span class="sxs-lookup"><span data-stu-id="ac0b0-115">Creating a custom preset</span></span>
<span data-ttu-id="ac0b0-116">Hello příkladu v diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="ac0b0-116">In hello example shown in hello diagram:</span></span>

1. <span data-ttu-id="ac0b0-117">Původní vstup je 1920 × 1080</span><span class="sxs-lookup"><span data-stu-id="ac0b0-117">Original input is 1920x1080</span></span>
2. <span data-ttu-id="ac0b0-118">Je nutné toobe oříznout tooan výstup 1440 × 1080, který je umístěn na střed hello vstupní rámce</span><span class="sxs-lookup"><span data-stu-id="ac0b0-118">It needs toobe cropped tooan output of 1440x1080, which is centered in hello input frame</span></span>
3. <span data-ttu-id="ac0b0-119">To znamená posunem X (1920 – 1440) / 2 = 240 a Y posunutí nula</span><span class="sxs-lookup"><span data-stu-id="ac0b0-119">This means an X offset of (1920 – 1440)/2 = 240, and a Y offset of zero</span></span>
4. <span data-ttu-id="ac0b0-120">Hello šířky a výšky obdélníku ořezové hello jsou 1440 a 1080, v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="ac0b0-120">hello Width and Height of hello Crop rectangle are 1440 and 1080, respectively</span></span>
5. <span data-ttu-id="ac0b0-121">V hello kódování fázi hello požádat je tooproduce tři vrstvy, jsou rozlišení 1440 × 1080 960 × 720 a 480 x 360, v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="ac0b0-121">In hello encode stage, hello ask is tooproduce three layers, are resolutions 1440x1080, 960x720 and 480x360, respectively</span></span>

### <a name="json-preset"></a><span data-ttu-id="ac0b0-122">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="ac0b0-122">JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


## <a name="restrictions-on-cropping"></a><span data-ttu-id="ac0b0-123">Omezení oříznutí</span><span class="sxs-lookup"><span data-stu-id="ac0b0-123">Restrictions on cropping</span></span>
<span data-ttu-id="ac0b0-124">Hello oříznutí funkce je určená toobe ručně.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-124">hello cropping feature is meant toobe manual.</span></span> <span data-ttu-id="ac0b0-125">Potřebovali byste tooload váš vstup videa v vhodný úpravy nástroj, který umožňuje vybrat rámců, které vás zajímají, umístěte hello kurzor toodetermine posunutí pro hello oříznutí obdélníku, toodetermine hello kódování přednastavení, která je přizpůsobená pro tuto konkrétní video, atd. Tato funkce není určena tooenable věcmi, jako jsou: automatické zjišťování a odebrání ohraničení černým letterbox/pillarbox v váš vstup videa.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-125">You would need tooload your input video into a suitable editing tool that lets you select frames of interest, position hello cursor toodetermine offsets for hello cropping rectangle, toodetermine hello encoding preset that is tuned for that particular video, etc. This feature is not meant tooenable things like: automatic detection and removal of black letterbox/pillarbox borders in your input video.</span></span>

<span data-ttu-id="ac0b0-126">Následující omezení platí toohello oříznutí funkce.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-126">Following constraints apply toohello cropping feature.</span></span> <span data-ttu-id="ac0b0-127">Pokud nejsou splněny tyto, hello kódování úloh může selhat nebo vytvoření neočekávané výstupu.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-127">If these are not met, hello encode Task can fail, or produce an unexpected output.</span></span>

1. <span data-ttu-id="ac0b0-128">Hello souřadnic a velikost obdélníku ořezové hello mít toofit v rámci hello vstupní video</span><span class="sxs-lookup"><span data-stu-id="ac0b0-128">hello co-ordinates and size of hello Crop rectangle have toofit within hello input video</span></span>
2. <span data-ttu-id="ac0b0-129">Jak je uvedeno nahoře, hello šířka a výška v hello kódování nastavení mít toocorrespond toohello oříznout video</span><span class="sxs-lookup"><span data-stu-id="ac0b0-129">As mentioned above, hello Width & Height in hello encode settings have toocorrespond toohello cropped video</span></span>
3. <span data-ttu-id="ac0b0-130">Oříznutí platí toovideos zaznamenat v režimu na šířku (tj. není k dispozici toovideos zaznamenávají s smartphone uchovávat svisle nebo v režimu na výšku)</span><span class="sxs-lookup"><span data-stu-id="ac0b0-130">Cropping applies toovideos captured in landscape mode (i.e. not applicable toovideos recorded with a smartphone held vertically or in portrait mode)</span></span>
4. <span data-ttu-id="ac0b0-131">Funguje nejlépe s progresivní video zachytit pomocí odmocnina pixelů</span><span class="sxs-lookup"><span data-stu-id="ac0b0-131">Works best with progressive video captured with square pixels</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="ac0b0-132">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ac0b0-132">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="ac0b0-133">Další krok</span><span class="sxs-lookup"><span data-stu-id="ac0b0-133">Next step</span></span>
<span data-ttu-id="ac0b0-134">V tématu Azure Media Services učení toohelp cesty, které další informace o skvělé funkce, které nabízí AMS.</span><span class="sxs-lookup"><span data-stu-id="ac0b0-134">See Azure Media Services learning paths toohelp you learn about great features offered by AMS.</span></span>  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
