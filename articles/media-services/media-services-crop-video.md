---
title: "Oříznutí videa s Media Encoder Standard - Azure | Microsoft Docs"
description: "Tento článek ukazuje, jak oříznout videa pomocí procesoru Media Encoder Standard."
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
ms.openlocfilehash: 60d0ce14a271fcbe698559da95ca011cb888b221
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a><span data-ttu-id="d5282-103">Oříznutí videa pomocí kodéru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="d5282-103">Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="d5282-104">Chcete-li oříznout váš vstup videa můžete Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="d5282-104">You can use Media Encoder Standard (MES) to crop your input video.</span></span> <span data-ttu-id="d5282-105">Oříznutí je proces kódování pixelů v rámci dané okno a výběrem obdélníkový okno v rámci videa.</span><span class="sxs-lookup"><span data-stu-id="d5282-105">Cropping is the process of selecting a rectangular window within the video frame, and encoding just the pixels within that window.</span></span> <span data-ttu-id="d5282-106">Následující diagram pomáhá proces znázorněn.</span><span class="sxs-lookup"><span data-stu-id="d5282-106">The following diagram helps illustrate the process.</span></span>

![Oříznutí video](./media/media-services-crop-video/media-services-crop-video01.png)

<span data-ttu-id="d5282-108">Předpokládejme, že máte jako vstupní video, které má rozlišení 1920 × 1080 pixelů (poměr stran 16:9), ale má černé pruhy (pilíře polí) v levém dolním a pravém, tak, aby pouze okno 4:3 nebo 1440 × 1080 pixelů obsahuje aktivní video.</span><span class="sxs-lookup"><span data-stu-id="d5282-108">Suppose you have as input a video that has a resolution of 1920x1080 pixels (16:9 aspect ratio), but has black bars (pillar boxes) at the left and right, so that only a 4:3 window or 1440x1080 pixels contains active video.</span></span> <span data-ttu-id="d5282-109">Můžete pomocí MES oříznout nebo upravte si černé pruhy a kódování 1440 × 1080 oblast.</span><span class="sxs-lookup"><span data-stu-id="d5282-109">You can use MES to crop or edit out the black bars, and encode the 1440x1080 region.</span></span>

<span data-ttu-id="d5282-110">Oříznutí v MES je předem zpracování fázi, takže oříznutí parametrů v kódování přednastavených týkají původního vstupní video.</span><span class="sxs-lookup"><span data-stu-id="d5282-110">Cropping in MES is a pre-processing stage, so the cropping parameters in the encoding preset apply to the original input video.</span></span> <span data-ttu-id="d5282-111">Kódování je další fázi a použít nastavení šířky a výšky *předem zpracovaných* video a nikoli k původní video.</span><span class="sxs-lookup"><span data-stu-id="d5282-111">Encoding is a subsequent stage, and the width/height settings apply to the *pre-processed* video, and not to the original video.</span></span> <span data-ttu-id="d5282-112">Při navrhování vaší přednastavených musíte udělat následující: (a) vyberte ořezové parametry podle původní vstupní video a (b) vyberte vaše kódování nastavení podle oříznutý video.</span><span class="sxs-lookup"><span data-stu-id="d5282-112">When designing your preset you need to do the following: (a) select the crop parameters based on the original input video, and (b) select your encode settings based on the cropped video.</span></span> <span data-ttu-id="d5282-113">Pokud se neshodují vaše kódování nastavení oříznutý video, výstup nebudou podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="d5282-113">If you do not match your encode settings to the cropped video, the output will not be as you expect.</span></span>

<span data-ttu-id="d5282-114">[Následující](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) téma ukazuje, jak vytvořit úlohu kódování s MES a jak určit vlastní přednastavení kódování úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5282-114">The [following](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic shows how to create an encoding job with MES and how to specify a custom preset for the encoding task.</span></span> 

## <a name="creating-a-custom-preset"></a><span data-ttu-id="d5282-115">Vytváření vlastní předvolby</span><span class="sxs-lookup"><span data-stu-id="d5282-115">Creating a custom preset</span></span>
<span data-ttu-id="d5282-116">V příkladu v diagramu:</span><span class="sxs-lookup"><span data-stu-id="d5282-116">In the example shown in the diagram:</span></span>

1. <span data-ttu-id="d5282-117">Původní vstup je 1920 × 1080</span><span class="sxs-lookup"><span data-stu-id="d5282-117">Original input is 1920x1080</span></span>
2. <span data-ttu-id="d5282-118">Musí se výstup 1440 × 1080, který je umístěn na střed v rámci vstupní oříznut</span><span class="sxs-lookup"><span data-stu-id="d5282-118">It needs to be cropped to an output of 1440x1080, which is centered in the input frame</span></span>
3. <span data-ttu-id="d5282-119">To znamená posunem X (1920 – 1440) / 2 = 240 a Y posunutí nula</span><span class="sxs-lookup"><span data-stu-id="d5282-119">This means an X offset of (1920 – 1440)/2 = 240, and a Y offset of zero</span></span>
4. <span data-ttu-id="d5282-120">Šířka a výška rámečku ořezové jsou 1440 a 1080, v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="d5282-120">The Width and Height of the Crop rectangle are 1440 and 1080, respectively</span></span>
5. <span data-ttu-id="d5282-121">Ve fázi kódovat požádejte je vytvořit tři vrstvy, jsou rozlišení 1440 × 1080 960 × 720 a 480 x 360, v uvedeném pořadí</span><span class="sxs-lookup"><span data-stu-id="d5282-121">In the encode stage, the ask is to produce three layers, are resolutions 1440x1080, 960x720 and 480x360, respectively</span></span>

### <a name="json-preset"></a><span data-ttu-id="d5282-122">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="d5282-122">JSON preset</span></span>
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


## <a name="restrictions-on-cropping"></a><span data-ttu-id="d5282-123">Omezení oříznutí</span><span class="sxs-lookup"><span data-stu-id="d5282-123">Restrictions on cropping</span></span>
<span data-ttu-id="d5282-124">Oříznutí funkce měl by být ručně.</span><span class="sxs-lookup"><span data-stu-id="d5282-124">The cropping feature is meant to be manual.</span></span> <span data-ttu-id="d5282-125">Museli byste se načíst vaše vstupní video do vhodný úpravy nástroj, který umožňuje vybrat rámců, které vás zajímají, umístěte kurzor k určení posunutí pro rámečku oříznutí, určete kódování přednastavení, která je přizpůsobená pro tuto konkrétní video, atd. Tato funkce není určena k povolení věcmi, jako jsou: automatické zjišťování a odebrání ohraničení černým letterbox/pillarbox v váš vstup videa.</span><span class="sxs-lookup"><span data-stu-id="d5282-125">You would need to load your input video into a suitable editing tool that lets you select frames of interest, position the cursor to determine offsets for the cropping rectangle, to determine the encoding preset that is tuned for that particular video, etc. This feature is not meant to enable things like: automatic detection and removal of black letterbox/pillarbox borders in your input video.</span></span>

<span data-ttu-id="d5282-126">Následující omezení se vztahují na funkci oříznutí.</span><span class="sxs-lookup"><span data-stu-id="d5282-126">Following constraints apply to the cropping feature.</span></span> <span data-ttu-id="d5282-127">Pokud nejsou splněny tyto, kódovat úkolů selhat nebo vytvoření neočekávané výstupu.</span><span class="sxs-lookup"><span data-stu-id="d5282-127">If these are not met, the encode Task can fail, or produce an unexpected output.</span></span>

1. <span data-ttu-id="d5282-128">Souřadnic a velikosti obdélníku ořezové mít nevejde se do vstupní video</span><span class="sxs-lookup"><span data-stu-id="d5282-128">The co-ordinates and size of the Crop rectangle have to fit within the input video</span></span>
2. <span data-ttu-id="d5282-129">Jak je uvedeno nahoře, šířku a výšku v nastavení kódovat mít tak, aby odpovídaly oříznutý video</span><span class="sxs-lookup"><span data-stu-id="d5282-129">As mentioned above, the Width & Height in the encode settings have to correspond to the cropped video</span></span>
3. <span data-ttu-id="d5282-130">Oříznutí platí pro videa zaznamenat v režimu na šířku (tj. nevztahuje se na videa s smartphone zaznamenaná uchovávat svisle nebo v režimu na výšku)</span><span class="sxs-lookup"><span data-stu-id="d5282-130">Cropping applies to videos captured in landscape mode (i.e. not applicable to videos recorded with a smartphone held vertically or in portrait mode)</span></span>
4. <span data-ttu-id="d5282-131">Funguje nejlépe s progresivní video zachytit pomocí odmocnina pixelů</span><span class="sxs-lookup"><span data-stu-id="d5282-131">Works best with progressive video captured with square pixels</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="d5282-132">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="d5282-132">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="d5282-133">Další krok</span><span class="sxs-lookup"><span data-stu-id="d5282-133">Next step</span></span>
<span data-ttu-id="d5282-134">V tématu Azure Media Services kurzů můžete další informace o skvělé funkce, které nabízí AMS.</span><span class="sxs-lookup"><span data-stu-id="d5282-134">See Azure Media Services learning paths to help you learn about great features offered by AMS.</span></span>  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
