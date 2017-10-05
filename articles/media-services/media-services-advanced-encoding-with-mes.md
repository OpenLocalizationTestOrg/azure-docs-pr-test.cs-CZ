---
title: "Pokročilé kódování přizpůsobením MES přednastavení | Microsoft Docs"
description: "Toto téma ukazuje, jak provádět pokročilé kódování přizpůsobením Media Encoder Standard přednastavení úloh."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 8de3bdd45261c84a0e1bb90f1c58863ad740dd5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a><span data-ttu-id="e7909-103">Pokročilé kódování přizpůsobením MES přednastavení</span><span class="sxs-lookup"><span data-stu-id="e7909-103">Perform advanced encoding by customizing MES presets</span></span> 

## <a name="overview"></a><span data-ttu-id="e7909-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e7909-104">Overview</span></span>

<span data-ttu-id="e7909-105">Toto téma ukazuje, jak přizpůsobit Media Encoder Standard přednastavení.</span><span class="sxs-lookup"><span data-stu-id="e7909-105">This topic shows how to customize Media Encoder Standard presets.</span></span> <span data-ttu-id="e7909-106">[Kódování s Media Encoder Standard pomocí vlastních předvoleb](media-services-custom-mes-presets-with-dotnet.md) téma ukazuje, jak vytvořit kódování úlohy a úlohy, která spustí tuto úlohu pomocí rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="e7909-106">The [Encoding with Media Encoder Standard using custom presets](media-services-custom-mes-presets-with-dotnet.md) topic shows how to use .NET to create an encoding task and a job that executes this task.</span></span> <span data-ttu-id="e7909-107">Jakmile upravíte přednastavení, zadejte vlastní přednastavení kódování úlohy.</span><span class="sxs-lookup"><span data-stu-id="e7909-107">Once you customize a preset, supply the custom presets to the encoding task.</span></span> 

>[!NOTE]
><span data-ttu-id="e7909-108">Pokud používáte přednastavení XML, ujistěte se, chcete-li zachovat pořadí prvků, jak je znázorněno v následující ukázky XML (například KeyFrameInterval by měl předcházet SceneChangeDetection).</span><span class="sxs-lookup"><span data-stu-id="e7909-108">If using an XML preset, make sure to preserve the order of elements, as shown in XML samples below (for example, KeyFrameInterval should precede SceneChangeDetection).</span></span>
>

<span data-ttu-id="e7909-109">V tomto tématu je ukázán vlastních předvoleb, které provádět následující úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="e7909-109">In this topic, the custom presets that perform the following encoding tasks are demonstrated.</span></span>

## <a name="support-for-relative-sizes"></a><span data-ttu-id="e7909-110">Podpora pro relativní velikosti</span><span class="sxs-lookup"><span data-stu-id="e7909-110">Support for relative sizes</span></span>

<span data-ttu-id="e7909-111">Při vytváření miniatur, není potřeba vždycky zadat výstupní šířky a výšky v pixelech.</span><span class="sxs-lookup"><span data-stu-id="e7909-111">When generating thumbnails, you do not need to always specify output width and height in pixels.</span></span> <span data-ttu-id="e7909-112">Je můžete zadat v procentech v rozsahu [1 %,..., 100 %].</span><span class="sxs-lookup"><span data-stu-id="e7909-112">You can specify them in percentages, in the range [1%, …, 100%].</span></span>

### <a name="json-preset"></a><span data-ttu-id="e7909-113">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-113">JSON preset</span></span>
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a><span data-ttu-id="e7909-114">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-114">XML preset</span></span>
    <Width>100%</Width>
    <Height>100%</Height>

## <span data-ttu-id="e7909-115"><a id="thumbnails"></a>Vytváření miniatur</span><span class="sxs-lookup"><span data-stu-id="e7909-115"><a id="thumbnails"></a>Generate thumbnails</span></span>

<span data-ttu-id="e7909-116">V této části ukazuje, jak přizpůsobit přednastavení, která generuje miniatur.</span><span class="sxs-lookup"><span data-stu-id="e7909-116">This section shows how to customize a preset that generates thumbnails.</span></span> <span data-ttu-id="e7909-117">Přednastavení definovaná níže obsahuje informace o tom, jak chcete zakódovat váš soubor, jakož i informace potřebné k vytváření miniatur.</span><span class="sxs-lookup"><span data-stu-id="e7909-117">The preset defined below contains information on how you want to encode your file as well as information needed to generate thumbnails.</span></span> <span data-ttu-id="e7909-118">Můžete využít některé z přednastavení MES zdokumentovaný [to](media-services-mes-presets-overview.md) a přidejte kód, který generuje miniatur.</span><span class="sxs-lookup"><span data-stu-id="e7909-118">You can take any of the MES presets documented [this](media-services-mes-presets-overview.md) section and add code that generates thumbnails.</span></span>  

> [!NOTE]
> <span data-ttu-id="e7909-119">**SceneChangeDetection** nastavení v následujících přednastavení lze nastavit pouze na hodnotu true, pokud jsou kódování s jednou přenosovou rychlostí videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-119">The **SceneChangeDetection** setting in the following preset can only be set to true if you are encoding to a single  bitrate video.</span></span> <span data-ttu-id="e7909-120">Pokud jsou kódování na video s více přenosovými rychlostmi a sadu **SceneChangeDetection** na hodnotu true, vrátí kodér k chybě.</span><span class="sxs-lookup"><span data-stu-id="e7909-120">If you are encoding to a multi-bitrate video and set **SceneChangeDetection** to true, the encoder returns an error.</span></span>  
>
>

<span data-ttu-id="e7909-121">Informace o schématu najdete v tématu [to](media-services-mes-schema.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="e7909-121">For information about schema, see [this](media-services-mes-schema.md) topic.</span></span>

<span data-ttu-id="e7909-122">Projděte si [aspekty](#considerations) části.</span><span class="sxs-lookup"><span data-stu-id="e7909-122">Make sure to review the [Considerations](#considerations) section.</span></span>

### <span data-ttu-id="e7909-123"><a id="json"></a>Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-123"><a id="json"></a>JSON preset</span></span>
    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"

            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <span data-ttu-id="e7909-124"><a id="xml"></a>Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-124"><a id="xml"></a>XML preset</span></span>
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a><span data-ttu-id="e7909-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e7909-125">Considerations</span></span>

<span data-ttu-id="e7909-126">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="e7909-126">The following considerations apply:</span></span>

* <span data-ttu-id="e7909-127">Použití explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že se vstupní zdroj alespoň 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="e7909-127">The use of explicit timestamps for Start/Step/Range assumes that the input source is at least 1 minute long.</span></span>
* <span data-ttu-id="e7909-128">JPG nebo Png nebo BmpImage prvky má spustit, krok a rozsah řetězec atributy – to jde interpretovat jako:</span><span class="sxs-lookup"><span data-stu-id="e7909-128">Jpg/Png/BmpImage elements have Start, Step, and Range string attributes – these can be interpreted as:</span></span>

  * <span data-ttu-id="e7909-129">Rámce číslo, pokud jsou nezáporná celá čísla, například "Start": "120",</span><span class="sxs-lookup"><span data-stu-id="e7909-129">Frame Number if they are non-negative integers, for example "Start": "120",</span></span>
  * <span data-ttu-id="e7909-130">Vzhledem ke zdrojové doba trvání, pokud vyjádřený jako % konci, například "Start": "15 %", nebo</span><span class="sxs-lookup"><span data-stu-id="e7909-130">Relative to source duration if expressed as %-suffixed, for example "Start": "15%", OR</span></span>
  * <span data-ttu-id="e7909-131">Časové razítko, pokud vyjádřený jako hh: mm:...</span><span class="sxs-lookup"><span data-stu-id="e7909-131">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="e7909-132">formátování, například "Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="e7909-132">format, for example "Start" : "00:01:00"</span></span>

    <span data-ttu-id="e7909-133">Můžete kombinovat a párovat zápisy, jako je prosím.</span><span class="sxs-lookup"><span data-stu-id="e7909-133">You can mix and match notations as you please.</span></span>

    <span data-ttu-id="e7909-134">Kromě toho spustit také podporuje speciální makra: {osvědčené}, která se pokusí určit první "zajímavé" snímek obsahu Poznámka: (krok a rozsah ignorují při spuštění je nastaven na {nejvhodnější})</span><span class="sxs-lookup"><span data-stu-id="e7909-134">Additionally, Start also supports a special Macro:{Best}, which attempts to determine the first “interesting” frame of the content NOTE: (Step and Range are ignored when Start is set to {Best})</span></span>
  * <span data-ttu-id="e7909-135">Výchozí nastavení: Spuštění: {nejlepší}</span><span class="sxs-lookup"><span data-stu-id="e7909-135">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="e7909-136">Výstupní formát je třeba explicitně zadat pro každý formát obrázku: Jpg nebo Png nebo BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="e7909-136">Output format needs to be explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="e7909-137">Pokud jsou k dispozici, odpovídá MES JpgVideo k JpgFormat a tak dále.</span><span class="sxs-lookup"><span data-stu-id="e7909-137">When present, MES matches JpgVideo to JpgFormat and so on.</span></span> <span data-ttu-id="e7909-138">OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, které musí být k dispozici (jednou a jen jednou) pro výstupní formáty bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e7909-138">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs to be present (once and only once) for image output formats.</span></span>

## <span data-ttu-id="e7909-139"><a id="trim_video"></a>Trim video (výstřižek.)</span><span class="sxs-lookup"><span data-stu-id="e7909-139"><a id="trim_video"></a>Trim a video (clipping)</span></span>
<span data-ttu-id="e7909-140">Tato část pojednává o Úprava přednastavení kodér oříznutí nebo trim vstupní video, kde je vstupní soubor mezzanine takzvané soubor nebo soubor na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="e7909-140">This section talks about modifying the encoder presets to clip or trim the input video where the input is a so-called mezzanine file or on-demand file.</span></span> <span data-ttu-id="e7909-141">Kodér lze také v případě potřeby upraví nebo trim prostředek, která je zachytit nebo archivovat z živý datový proud – podrobnosti o to jsou k dispozici v [tomto blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="e7909-141">The encoder can also be used to clip or trim an asset, which is captured or archived from a live stream – the details for this are available in [this blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span></span>

<span data-ttu-id="e7909-142">Oříznout videa, mohou mít libovolný přednastavení MES zdokumentovaný [to](media-services-mes-presets-overview.md) část a upravte **zdroje** – element (jak je znázorněno níže).</span><span class="sxs-lookup"><span data-stu-id="e7909-142">To trim your videos, you can take any of the MES presets documented [this](media-services-mes-presets-overview.md) section and modify the **Sources** element (as shown below).</span></span> <span data-ttu-id="e7909-143">Hodnota čas spuštění musí odpovídat absolutní časová razítka vstupní videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-143">The value of StartTime needs to match the absolute timestamps of the input video.</span></span> <span data-ttu-id="e7909-144">Například pokud první snímek vstupní video má časovým razítkem 12:00:10.000, pak čas spuštění by měl alespoň 12:00:10.000 a větší.</span><span class="sxs-lookup"><span data-stu-id="e7909-144">For example, if the first frame of the input video has a timestamp of 12:00:10.000, then StartTime should be at least 12:00:10.000 and greater.</span></span> <span data-ttu-id="e7909-145">V následujícím příkladu předpokládáme, že vstupní video má výchozí časové razítko nula.</span><span class="sxs-lookup"><span data-stu-id="e7909-145">In the example below, we assume that the input video has a starting timestamp of zero.</span></span> <span data-ttu-id="e7909-146">**Zdroje** musí být umístěny na začátku přednastavení.</span><span class="sxs-lookup"><span data-stu-id="e7909-146">**Sources** should be placed at the beginning of the preset.</span></span>

### <span data-ttu-id="e7909-147"><a id="json"></a>Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-147"><a id="json"></a>JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
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
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
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
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
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
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
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

### <a name="xml-preset"></a><span data-ttu-id="e7909-148">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-148">XML preset</span></span>
<span data-ttu-id="e7909-149">Oříznout videa, mohou mít libovolný přednastavení MES zdokumentovaný [sem](media-services-mes-presets-overview.md) a upravovat **zdroje** – element (jak je znázorněno níže).</span><span class="sxs-lookup"><span data-stu-id="e7909-149">To trim your videos, you can take any of the MES presets documented [here](media-services-mes-presets-overview.md) and modify the **Sources** element (as shown below).</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <span data-ttu-id="e7909-150"><a id="overlay"></a>Vytvoření překrytí</span><span class="sxs-lookup"><span data-stu-id="e7909-150"><a id="overlay"></a>Create an overlay</span></span>

<span data-ttu-id="e7909-151">Media Encoder Standard umožňuje překrytí bitovou kopii do existujícího videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-151">The Media Encoder Standard allows you to overlay an image onto an existing video.</span></span> <span data-ttu-id="e7909-152">V současné době jsou podporovány následující formáty: png, jpg, gif, bmp a.</span><span class="sxs-lookup"><span data-stu-id="e7909-152">Currently, the following formats are supported: png, jpg, gif, and bmp.</span></span> <span data-ttu-id="e7909-153">Předvolba definovaná níže je základní příklad překryvné video.</span><span class="sxs-lookup"><span data-stu-id="e7909-153">The preset defined below is a basic example  of a video overlay.</span></span>

<span data-ttu-id="e7909-154">Kromě definování soubor přednastavení, máte také umožní Media Services vědět, který soubor v prostředku je bitovou kopii překrytí a soubor, který je zdrojem videa na kterém chcete překrýt bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="e7909-154">In addition to defining a preset file, you also have to let Media Services know which file in the asset is the overlay image and which file is the source video onto which you want to overlay the image.</span></span> <span data-ttu-id="e7909-155">Video souboru musí být **primární** souboru.</span><span class="sxs-lookup"><span data-stu-id="e7909-155">The video file has to be the **primary** file.</span></span>

<span data-ttu-id="e7909-156">Pokud používáte rozhraní .NET, přidejte následující dvě funkce v rozhraní .NET příkladu definované v [to](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) tématu.</span><span class="sxs-lookup"><span data-stu-id="e7909-156">If you are using .NET, add the following two functions to the .NET example defined in [this](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic.</span></span> <span data-ttu-id="e7909-157">**UploadMediaFilesFromFolder** funkce odešle soubory ze složky (například BigBuckBunny.mp4 a Image001.png) a nastaví soubor mp4 jako primární soubor v prostředku.</span><span class="sxs-lookup"><span data-stu-id="e7909-157">The **UploadMediaFilesFromFolder** function uploads files from a folder (for example, BigBuckBunny.mp4 and Image001.png) and sets the mp4 file to be the primary file in the asset.</span></span> <span data-ttu-id="e7909-158">**EncodeWithOverlay** funkce používá vlastní přednastavené soubor, který byl předán do ní (například přednastavení, která odpovídá) k vytvoření úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="e7909-158">The **EncodeWithOverlay** function uses the custom preset file that was passed to it (for example, the preset that follows) to create the encoding task.</span></span>


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // The following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input assets to be encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset to contain the results of the job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> <span data-ttu-id="e7909-159">Aktuální omezení:</span><span class="sxs-lookup"><span data-stu-id="e7909-159">Current limitations:</span></span>
>
> <span data-ttu-id="e7909-160">Nastavení krytí překrytí není podporováno.</span><span class="sxs-lookup"><span data-stu-id="e7909-160">The overlay opacity setting is not supported.</span></span>
>
> <span data-ttu-id="e7909-161">Video váš zdrojový soubor a soubor bitové kopie překrytí musí být ve stejné asset a video soubor musí být nastavena jako primární soubor v tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="e7909-161">Your source video file and the overlay image file have to be in the same asset, and the video file needs to be set as the primary file in this asset.</span></span>
>
>

### <a name="json-preset"></a><span data-ttu-id="e7909-162">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-162">JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
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
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a><span data-ttu-id="e7909-163">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-163">XML preset</span></span>
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <span data-ttu-id="e7909-164"><a id="silent_audio"></a>Vložit tichou zvuk sledovat, pokud vstup neobsahuje žádný zvuk</span><span class="sxs-lookup"><span data-stu-id="e7909-164"><a id="silent_audio"></a>Insert a silent audio track when input has no audio</span></span>
<span data-ttu-id="e7909-165">Ve výchozím nastavení Pokud odesíláte vstup kodéru, který obsahuje pouze video a žádné zvuk, pak výstupní asset obsahuje soubory, které obsahují pouze video data.</span><span class="sxs-lookup"><span data-stu-id="e7909-165">By default, if you send an input to the encoder that contains only video, and no audio, then the output asset contains files that contain only video data.</span></span> <span data-ttu-id="e7909-166">Některé přehrávače nemusí být schopná zpracovat takové výstupní datové proudy.</span><span class="sxs-lookup"><span data-stu-id="e7909-166">Some players may not be able to handle such output streams.</span></span> <span data-ttu-id="e7909-167">Toto nastavení slouží k vynucení kodér přidat tichou zvuk sledovat ve výstupu v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="e7909-167">You can use this setting to force the encoder to add a silent audio track to the output in that scenario.</span></span>

<span data-ttu-id="e7909-168">Chcete-li vynutit kodér k vytvoření asset, který obsahuje tichou zvuk sledovat, kdy se vstup neobsahuje žádný zvuk, zadejte hodnotu "InsertSilenceIfNoAudio".</span><span class="sxs-lookup"><span data-stu-id="e7909-168">To force the encoder to produce an asset that contains a silent audio track when input has no audio, specify the "InsertSilenceIfNoAudio" value.</span></span>

<span data-ttu-id="e7909-169">Můžete využít některé z přednastavení MES zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="e7909-169">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

### <a name="json-preset"></a><span data-ttu-id="e7909-170">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-170">JSON preset</span></span>
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a><span data-ttu-id="e7909-171">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-171">XML preset</span></span>
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <span data-ttu-id="e7909-172"><a id="deinterlacing"></a>Zakázat automatické zrušte prokládání.</span><span class="sxs-lookup"><span data-stu-id="e7909-172"><a id="deinterlacing"></a>Disable auto de-interlacing</span></span>
<span data-ttu-id="e7909-173">Zákazníci, nemusíte nic dělat, když se jako obsah prokládání být automaticky zrušte prokládaných.</span><span class="sxs-lookup"><span data-stu-id="e7909-173">Customers don’t need to do anything if they like the interlace contents to be automatically de-interlaced.</span></span> <span data-ttu-id="e7909-174">Prokládání zrušte automaticky je na (výchozí) MES nepodporuje automatické zjišťování prokládaných snímků a pouze zrušte interlaces označena jako prokládaných rámce.</span><span class="sxs-lookup"><span data-stu-id="e7909-174">When the auto de-interlacing is on (default) the MES does the auto detection of interlaced frames and only de-interlaces frames marked as interlaced.</span></span>

<span data-ttu-id="e7909-175">Můžete vypnout prokládání zrušte automaticky.</span><span class="sxs-lookup"><span data-stu-id="e7909-175">You can turn the auto de-interlacing off.</span></span> <span data-ttu-id="e7909-176">Tato možnost se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="e7909-176">This option is not recommended.</span></span>

### <a name="json-preset"></a><span data-ttu-id="e7909-177">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-177">JSON preset</span></span>
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a><span data-ttu-id="e7909-178">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-178">XML preset</span></span>
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <span data-ttu-id="e7909-179"><a id="audio_only"></a>Pouze přednastavení</span><span class="sxs-lookup"><span data-stu-id="e7909-179"><a id="audio_only"></a>Audio-only presets</span></span>
<span data-ttu-id="e7909-180">V této části ukážeme dvou pouze MES přednastavení: AAC zvuk a AAC dobrý kvalitu zvuku.</span><span class="sxs-lookup"><span data-stu-id="e7909-180">This section demonstrates two audio-only MES presets: AAC Audio and AAC Good Quality Audio.</span></span>

### <a name="aac-audio"></a><span data-ttu-id="e7909-181">AAC zvuk</span><span class="sxs-lookup"><span data-stu-id="e7909-181">AAC Audio</span></span>
    {
      "Version": 1.0,
      "Codecs": [
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
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a><span data-ttu-id="e7909-182">AAC kvalitních zvuk</span><span class="sxs-lookup"><span data-stu-id="e7909-182">AAC Good Quality Audio</span></span>
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="e7909-183"><a id="concatenate"></a>Řetězení dvou nebo více souborů video</span><span class="sxs-lookup"><span data-stu-id="e7909-183"><a id="concatenate"></a>Concatenate two or more video files</span></span>

<span data-ttu-id="e7909-184">Následující příklad ilustruje, jak můžete vygenerovat přednastavení ke zřetězení dvou nebo více souborů videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-184">The following example illustrates how you can generate a preset to concatenate two or more video files.</span></span> <span data-ttu-id="e7909-185">Nejběžnější scénáře je, pokud chcete přidat hlavičku nebo vozidel hlavní video.</span><span class="sxs-lookup"><span data-stu-id="e7909-185">The most common scenario is when you want to add a header or a trailer to the main video.</span></span> <span data-ttu-id="e7909-186">Slouží při video soubory upravovaný společně sdílet vlastnosti (rozlišení videa, obnovovací frekvence, zvuk sledovat počet atd.).</span><span class="sxs-lookup"><span data-stu-id="e7909-186">The intended use is when the video files being edited together share  properties (video resolution, frame rate, audio track count, etc.).</span></span> <span data-ttu-id="e7909-187">By měl třeba dbát na to kombinovat videa různý kmitočet, nebo mají jiný počet zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="e7909-187">You should take care not to mix videos of different frame rates, or with different number of audio tracks.</span></span>

>[!NOTE]
><span data-ttu-id="e7909-188">Současný návrh zřetězení funkce očekává, že jsou vstupní videosoubory konzistentní z hlediska řešení, obnovovací frekvence atd.</span><span class="sxs-lookup"><span data-stu-id="e7909-188">The current design of the concatenation feature expects that the input video clips are consistent in terms of resolution, frame rate etc.</span></span> 

### <a name="requirements-and-considerations"></a><span data-ttu-id="e7909-189">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="e7909-189">Requirements and considerations</span></span>

* <span data-ttu-id="e7909-190">Vstupní videa by měla mít pouze jeden zvuk sledovat.</span><span class="sxs-lookup"><span data-stu-id="e7909-190">Input videos should only have one audio track.</span></span>
* <span data-ttu-id="e7909-191">Vstup videa, by měly mít stejnou snímků za sekundu.</span><span class="sxs-lookup"><span data-stu-id="e7909-191">Input videos should all have the same frame rate.</span></span>
* <span data-ttu-id="e7909-192">Musíte nahrát videa do samostatné prostředky a videím nastavit jako primární soubor v každé asset.</span><span class="sxs-lookup"><span data-stu-id="e7909-192">You must upload your videos into separate assets and set the videos as the primary file in each asset.</span></span>
* <span data-ttu-id="e7909-193">Je třeba vědět trvání videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-193">You need to know the duration of your videos.</span></span>
* <span data-ttu-id="e7909-194">Přednastavené následující příklady předpokládá, že všechny vstupní videa spustit s časovým razítkem nula.</span><span class="sxs-lookup"><span data-stu-id="e7909-194">The preset examples below assumes that all the input videos start with a timestamp of zero.</span></span> <span data-ttu-id="e7909-195">Budete muset upravit hodnoty StartTime, pokud videím mít různé výchozí časové razítko, což je obvykle případ s archivy za provozu.</span><span class="sxs-lookup"><span data-stu-id="e7909-195">You need to modify the StartTime values if the videos have different starting timestamp, as is typically the case with live archives.</span></span>
* <span data-ttu-id="e7909-196">Přednastavení JSON umožňuje explicitní odkazy na hodnoty ID vstupní prostředků.</span><span class="sxs-lookup"><span data-stu-id="e7909-196">The JSON preset makes explicit references to the AssetID values of the input assets.</span></span>
* <span data-ttu-id="e7909-197">Ukázkový kód předpokládá, že přednastavených JSON byla uložena do místního souboru, jako je například "C:\supportFiles\preset.json".</span><span class="sxs-lookup"><span data-stu-id="e7909-197">The sample code assumes that the JSON preset has been saved to a local file, such as "C:\supportFiles\preset.json".</span></span> <span data-ttu-id="e7909-198">Předpokládá také, zda byly vytvořeny dva prostředky tím, že nahrajete dva video soubory a že znáte výsledné hodnoty ID.</span><span class="sxs-lookup"><span data-stu-id="e7909-198">It also assumes that two assets have been created by uploading two video files, and that you know the resultant AssetID values.</span></span>
* <span data-ttu-id="e7909-199">Fragment kódu a JSON přednastavených ukazuje příklad zřetězení dva video soubory.</span><span class="sxs-lookup"><span data-stu-id="e7909-199">The code snippet and JSON preset shows an example of concatenating two video files.</span></span> <span data-ttu-id="e7909-200">Můžete ji rozšířit do více než dva videa pomocí:</span><span class="sxs-lookup"><span data-stu-id="e7909-200">You can extend it to more than two videos by:</span></span>

  1. <span data-ttu-id="e7909-201">Úloha volání. InputAssets.Add() opakovaně Chcete-li přidat další videa, v pořadí.</span><span class="sxs-lookup"><span data-stu-id="e7909-201">Calling task.InputAssets.Add() repeatedly to add more videos, in order.</span></span>
  2. <span data-ttu-id="e7909-202">Provedení odpovídající upraví "Zdroje" elementu v kódu JSON, můžete přidat další položky ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e7909-202">Making corresponding edits to the "Sources" element in the JSON, by adding more entries, in the same order.</span></span>

### <a name="net-code"></a><span data-ttu-id="e7909-203">Kód .NET</span><span class="sxs-lookup"><span data-stu-id="e7909-203">.NET code</span></span>

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job.
    // This output is specified as AssetCreationOptions.None, which
    // means the output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a><span data-ttu-id="e7909-204">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-204">JSON preset</span></span>

<span data-ttu-id="e7909-205">Aktualizujte vaše vlastní předvolby s ID prostředků, které chcete zřetězit a s příslušnou dobu segmentu pro každý video.</span><span class="sxs-lookup"><span data-stu-id="e7909-205">Update your custom preset with ids of the assets that you want to concatenate, and with the appropriate time segment for each video.</span></span>

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
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

## <span data-ttu-id="e7909-206"><a id="crop"></a>Oříznutí videa pomocí procesoru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="e7909-206"><a id="crop"></a>Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="e7909-207">Najdete v článku [oříznout videa pomocí procesoru Media Encoder Standard](media-services-crop-video.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="e7909-207">See the [Crop videos with Media Encoder Standard](media-services-crop-video.md) topic.</span></span>

## <span data-ttu-id="e7909-208"><a id="no_video"></a>Vložit video sledovat při vstupu je žádné video</span><span class="sxs-lookup"><span data-stu-id="e7909-208"><a id="no_video"></a>Insert a video track when input has no video</span></span>

<span data-ttu-id="e7909-209">Ve výchozím nastavení Pokud odesíláte vstup kodéru, který obsahuje pouze zvuk a obraz, pak výstupní asset obsahuje soubory, které obsahují pouze zvuková data.</span><span class="sxs-lookup"><span data-stu-id="e7909-209">By default, if you send an input to the encoder that contains only audio, and no video, then the output asset contains files that contain only audio data.</span></span> <span data-ttu-id="e7909-210">Některé přehrávače, včetně Azure Media Player (viz [to](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nemusí být schopná zpracovat takové datové proudy.</span><span class="sxs-lookup"><span data-stu-id="e7909-210">Some players, including Azure Media Player (see [this](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) may not be able to handle such streams.</span></span> <span data-ttu-id="e7909-211">Toto nastavení slouží k vynucení kodér přidat černobílý video sledovat ve výstupu v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="e7909-211">You can use this setting to force the encoder to add a monochrome video track to the output in that scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="e7909-212">Vynucení kodér vložit výstupu videa sledovat zvětšuje velikost výstupní Asset, a tím vám účtovány náklady pro úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="e7909-212">Forcing the encoder to insert an output video track increases the size of the output Asset, and thereby the cost incurred for the encoding Task.</span></span> <span data-ttu-id="e7909-213">Byste měli spustit testy ověření, že toto výsledné zvýšení má pouze mírné vliv na vaše měsíční poplatky.</span><span class="sxs-lookup"><span data-stu-id="e7909-213">You should run tests to verify that this resultant increase has only a modest impact on your monthly charges.</span></span>
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a><span data-ttu-id="e7909-214">Vložení videa na pouze nejnižší přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="e7909-214">Inserting video at only the lowest bitrate</span></span>

<span data-ttu-id="e7909-215">Předpokládejme, že jsou pomocí více kódování přenosovou rychlostí, jako přednastavení ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) ke kódování celý vstupní katalogu pro streamování, obsahující směs video soubory a soubory jen zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="e7909-215">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) to encode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="e7909-216">V tomto scénáři při ve vstupu je žádné video, můžete vynutit kodér vložte černobílý video sledovat na nejnižší přenosovou oproti vložení videa na každou výstupní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="e7909-216">In this scenario, when the input has no video, you may want to force the encoder to insert a monochrome video track at just the lowest bitrate, as opposed to inserting video at every output bitrate.</span></span> <span data-ttu-id="e7909-217">Jak toho docílit, budete muset použít **InsertBlackIfNoVideoBottomLayerOnly** příznak.</span><span class="sxs-lookup"><span data-stu-id="e7909-217">To achieve this, you need to use the **InsertBlackIfNoVideoBottomLayerOnly** flag.</span></span>

<span data-ttu-id="e7909-218">Můžete využít některé z přednastavení MES zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="e7909-218">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="e7909-219">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-219">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="e7909-220">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-220">XML preset</span></span>

<span data-ttu-id="e7909-221">Při použití XML, použít podmínku = "InsertBlackIfNoVideoBottomLayerOnly" jako atribut k **H264Video** elementu a podmínky = "InsertSilenceIfNoAudio" jako atribut k **AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="e7909-221">When using XML, use Condition="InsertBlackIfNoVideoBottomLayerOnly" as an attribute to the **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute to **AACAudio**.</span></span>

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a><span data-ttu-id="e7909-222">Vkládání video vůbec výstup přenosových rychlostí</span><span class="sxs-lookup"><span data-stu-id="e7909-222">Inserting video at all output bitrates</span></span>
<span data-ttu-id="e7909-223">Předpokládejme, že jsou pomocí více kódování přenosovou rychlostí, jako přednastavení ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) ke kódování celý vstupní katalogu pro streamování, obsahující směs video soubory a soubory jen zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="e7909-223">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) to encode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="e7909-224">V tomto scénáři při ve vstupu je žádné video, můžete vynutit kodér vložit černobílý video sledovat vůbec přenosových rychlostí výstup.</span><span class="sxs-lookup"><span data-stu-id="e7909-224">In this scenario, when the input has no video, you may want to force the encoder to insert a monochrome video track at all the output bitrates.</span></span> <span data-ttu-id="e7909-225">To zajistí, že výstupu prostředky jsou všechny informace, které bylo zajištěno homogenní, s ohledem na počet sleduje videa a audia sleduje.</span><span class="sxs-lookup"><span data-stu-id="e7909-225">This ensures that your output Assets are all homogenous with respect to number of video tracks and audio tracks.</span></span> <span data-ttu-id="e7909-226">Jak toho docílit, je třeba zadat příznak "InsertBlackIfNoVideo".</span><span class="sxs-lookup"><span data-stu-id="e7909-226">To achieve this, you need to specify the "InsertBlackIfNoVideo" flag.</span></span>

<span data-ttu-id="e7909-227">Můžete využít některé z přednastavení MES zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="e7909-227">You can take any of the MES presets documented in [this](media-services-mes-presets-overview.md) section, and make the following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="e7909-228">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-228">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="e7909-229">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-229">XML preset</span></span>

<span data-ttu-id="e7909-230">Při použití XML, použít podmínku = "InsertBlackIfNoVideo" jako atribut k **H264Video** elementu a podmínky = "InsertSilenceIfNoAudio" jako atribut k **AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="e7909-230">When using XML, use Condition="InsertBlackIfNoVideo" as an attribute to the **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute to **AACAudio**.</span></span>

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <span data-ttu-id="e7909-231"><a id="rotate_video"></a>Otočit video</span><span class="sxs-lookup"><span data-stu-id="e7909-231"><a id="rotate_video"></a>Rotate a video</span></span>
<span data-ttu-id="e7909-232">[Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje oběh podle úhly 0/90 nebo 180 nebo 270.</span><span class="sxs-lookup"><span data-stu-id="e7909-232">The [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 0/90/180/270.</span></span> <span data-ttu-id="e7909-233">Výchozí chování je "Auto", kde se pokusí zjistit metadata otočení příchozí videosoubor a kompenzovat ho.</span><span class="sxs-lookup"><span data-stu-id="e7909-233">The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming video file and compensate for it.</span></span> <span data-ttu-id="e7909-234">Patří **zdroje** element pro jedno z přednastavení definované v [to](media-services-mes-presets-overview.md) části:</span><span class="sxs-lookup"><span data-stu-id="e7909-234">Include the following **Sources** element to one of the presets defined in [this](media-services-mes-presets-overview.md) section:</span></span>

### <a name="json-preset"></a><span data-ttu-id="e7909-235">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="e7909-235">JSON preset</span></span>
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a><span data-ttu-id="e7909-236">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="e7909-236">XML preset</span></span>
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

<span data-ttu-id="e7909-237">Další informace naleznete v [to](media-services-mes-schema.md#PreserveResolutionAfterRotation) téma pro další informace o interpretace kodéru: nastavení šířky a výšky v přednastavení, při aktivaci otočení honoráře.</span><span class="sxs-lookup"><span data-stu-id="e7909-237">Also, see [this](media-services-mes-schema.md#PreserveResolutionAfterRotation) topic for more information on how the encoder interprets the Width and Height settings in the preset, when rotation compensation is triggered.</span></span>

<span data-ttu-id="e7909-238">Hodnota "0" můžete použít k označení kodéru ignorovat otočení metadata, pokud je k dispozici v vstup videa.</span><span class="sxs-lookup"><span data-stu-id="e7909-238">You can use the value "0" to indicate to the encoder to ignore rotation metadata, if present, in the input video.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="e7909-239">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="e7909-239">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e7909-240">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="e7909-240">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="e7909-241">Viz také</span><span class="sxs-lookup"><span data-stu-id="e7909-241">See Also</span></span>
[<span data-ttu-id="e7909-242">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="e7909-242">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)
