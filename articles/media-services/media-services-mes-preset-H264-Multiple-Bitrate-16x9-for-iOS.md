---
title: H264 Multiple Bitrate 16 x 9 pro iOS | Microsoft Docs
description: "Téma nabízí přehled ** H264 Multiple Bitrate 16 x 9 pro iOS ** přednastavení úloh."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5473e93a-8c18-4fa4-a1b9-215eba325af7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 9b938a5396e9f29fc9f2222056b84fbf13179dff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="h264-multiple-bitrate-16x9-for-ios"></a><span data-ttu-id="00f20-103">H264 Multiple Bitrate 16 x 9 pro iOS</span><span class="sxs-lookup"><span data-stu-id="00f20-103">H264 Multiple Bitrate 16x9 for iOS</span></span>
<span data-ttu-id="00f20-104">`Media Encoder Standard`definuje sadu kódování přednastavení, které můžete použít při vytváření úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="00f20-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="00f20-105">Můžete použít `preset name` zadat do formátu, který chcete zakódovat váš soubor média.</span><span class="sxs-lookup"><span data-stu-id="00f20-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="00f20-106">Nebo můžete vytvořit vlastní JSON nebo na základě XML přednastavení (pomocí kódování UTF-8 nebo UTF-16.</span><span class="sxs-lookup"><span data-stu-id="00f20-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="00f20-107">By pak předejte vlastní přednastavení kodéru.</span><span class="sxs-lookup"><span data-stu-id="00f20-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="00f20-108">Seznam všech názvů přednastavené podporovaných touto `Media Encoder Standard` kodér, najdete v části [přednastavení úloh pro Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00f20-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="00f20-109">Toto téma ukazuje `H264 Multiple Bitrate 16x9 for iOS` přednastavení ve formátu XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="00f20-109">This topic shows the `H264 Multiple Bitrate 16x9 for iOS` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="00f20-110">Tato předvolba vytvoří sadu 8 soubory MP4 zarovnaný GOP od 8500 kb/s až 200 kb/s a stereofonní zvuk AAC.</span><span class="sxs-lookup"><span data-stu-id="00f20-110">This preset produces a set of 8 GOP-aligned MP4 files, ranging from 8500 kbps to 200 kbps, and stereo AAC audio.</span></span> <span data-ttu-id="00f20-111">Podrobné informace o profilu přenosovou rychlostí, vzorkování rychlost atd tohoto přednastavení, zkontrolovat, XML nebo JSON definovaná níže.</span><span class="sxs-lookup"><span data-stu-id="00f20-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="00f20-112">Vysvětlení co každý prvek v těchto přednastavení znamená a platné hodnoty pro každý element, najdete v článku [Media Encoder Standard schématu](media-services-mes-schema.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="00f20-112">For explanations of what each element in these presets means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md) topic.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="00f20-113">Při úpravě `Width` a `Height` hodnot v rámci vrstvy, ujistěte se, aby zůstala konzistentní poměr stran.</span><span class="sxs-lookup"><span data-stu-id="00f20-113">When modifying the `Width` and `Height` values across layers, make sure that the aspect ratio remains consistent.</span></span> <span data-ttu-id="00f20-114">Příklad: 1920 × 1080, 1280 × 720, 1080 x 576 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="00f20-114">For example: 1920x1080, 1280x720, 1080x576, 640x360.</span></span> <span data-ttu-id="00f20-115">Neměli byste používat směs poměrům stran, jako například: 1280 × 720, 720 x 480, 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="00f20-115">You should not use a mixture of aspect ratios, such as: 1280x720, 720x480, 640x360.</span></span>  
  
 <span data-ttu-id="00f20-116">XML</span><span class="sxs-lookup"><span data-stu-id="00f20-116">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:03</KeyFrameInterval>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>8500</Bitrate>  
          <Width>1920</Width>  
          <Height>1080</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>High</Profile>  
          <Level>4</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>8500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>6500</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>6500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>5000</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>5000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>3500</Bitrate>  
          <Width>960</Width>  
          <Height>540</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Main</Profile>  
          <Level>3.1</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>3500</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>1200</Bitrate>  
          <Width>640</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3.1</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1200</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>600</Bitrate>  
          <Width>640</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>600</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>400</Bitrate>  
          <Width>480</Width>  
          <Height>270</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>400</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>200</Bitrate>  
          <Width>416</Width>  
          <Height>214</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>200</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>64</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 <span data-ttu-id="00f20-117">JSON</span><span class="sxs-lookup"><span data-stu-id="00f20-117">JSON</span></span>  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:03",  
      "H264Layers": [  
        {  
          "Profile": "High",  
          "Level": "4",  
          "Bitrate": 8500,  
          "MaxBitrate": 8500,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
          "Height": 1080,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 6500,  
          "MaxBitrate": 6500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 5000,  
          "MaxBitrate": 5000,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Main",  
          "Level": "3.1",  
          "Bitrate": 3500,  
          "MaxBitrate": 3500,  
          "BufferWindow": "00:00:05",  
          "Width": 960,  
          "Height": 540,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3.1",  
          "Bitrate": 1200,  
          "MaxBitrate": 1200,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 600,  
          "MaxBitrate": 600,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 400,  
          "MaxBitrate": 400,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
          "Height": 270,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 200,  
          "MaxBitrate": 200,  
          "BufferWindow": "00:00:05",  
          "Width": 416,  
          "Height": 214,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 64,  
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
