---
title: "H264 Jednotné přenosovou rychlostí 16 x 9 SD zvuk 5.1 | Microsoft Docs"
description: "Téma nabízí přehled ** H264 jednou přenosovou rychlostí 16 x 9 SD zvuk 5.1* * úloh přednastavení."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d57cd39f-60e8-4248-a21d-c5b9f096b0ce
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 6e8b7201d1c37deb02803f2e5160c68c8fdf1ed8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="h264-single-bitrate-16x9-sd-audio-51"></a><span data-ttu-id="80d46-103">H264 Jednou přenosovou rychlostí 16 x 9 SD zvuk 5.1</span><span class="sxs-lookup"><span data-stu-id="80d46-103">H264 Single Bitrate 16x9 SD Audio 5.1</span></span>
<span data-ttu-id="80d46-104">`Media Encoder Standard`definuje sadu kódování přednastavení, které můžete použít při vytváření úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="80d46-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="80d46-105">Můžete použít `preset name` zadat do formátu, který chcete zakódovat váš soubor média.</span><span class="sxs-lookup"><span data-stu-id="80d46-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="80d46-106">Nebo můžete vytvořit vlastní JSON nebo na základě XML přednastavení (pomocí kódování UTF-8 nebo UTF-16.</span><span class="sxs-lookup"><span data-stu-id="80d46-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="80d46-107">By pak předejte vlastní přednastavení kodéru.</span><span class="sxs-lookup"><span data-stu-id="80d46-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="80d46-108">Seznam všech názvů přednastavené podporovaných touto `Media Encoder Standard` kodér, najdete v části [přednastavení úloh pro Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="80d46-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="80d46-109">Toto téma ukazuje `H264 Single Bitrate 16x9 SD Audio 5.1` přednastavení ve formátu XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="80d46-109">This topic shows the `H264 Single Bitrate 16x9 SD Audio 5.1` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="80d46-110">Tato předvolba vytváří jednoho souboru MP4 s přenosovou rychlostí 2200 kb/s a zvuku AAC 5.1.</span><span class="sxs-lookup"><span data-stu-id="80d46-110">This preset produces a single MP4 file with a bitrate of 2200 kbps, and AAC 5.1 audio.</span></span> <span data-ttu-id="80d46-111">Podrobné informace o profilu přenosovou rychlostí, vzorkování rychlost atd tohoto přednastavení, zkontrolovat, XML nebo JSON definovaná níže.</span><span class="sxs-lookup"><span data-stu-id="80d46-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="80d46-112">Vysvětlení znamená, co každý prvek a platné hodnoty pro každý element, najdete v článku [Media Encoder Standard schématu](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="80d46-112">For explanations of what each element means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>  
  
 <span data-ttu-id="80d46-113">XML</span><span class="sxs-lookup"><span data-stu-id="80d46-113">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>2200</Bitrate>  
          <Width>848</Width>  
          <Height>480</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>2200</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 <span data-ttu-id="80d46-114">JSON</span><span class="sxs-lookup"><span data-stu-id="80d46-114">JSON</span></span>  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 2200,  
          "MaxBitrate": 2200,  
          "BufferWindow": "00:00:05",  
          "Width": 848,  
          "Height": 480,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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
```
