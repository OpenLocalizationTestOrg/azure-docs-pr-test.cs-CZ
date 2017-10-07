---
title: "aaaPerform advanced kódování přizpůsobením MES přednastavení | Microsoft Docs"
description: "Toto téma ukazuje, jak tooperform advanced kódování přizpůsobením Media Encoder Standard přednastavení úloh."
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
ms.openlocfilehash: 9caa68fafacaf51f91f0554c5bafe491928d8c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a><span data-ttu-id="10dc2-103">Pokročilé kódování přizpůsobením MES přednastavení</span><span class="sxs-lookup"><span data-stu-id="10dc2-103">Perform advanced encoding by customizing MES presets</span></span> 

## <a name="overview"></a><span data-ttu-id="10dc2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="10dc2-104">Overview</span></span>

<span data-ttu-id="10dc2-105">Toto téma ukazuje, jak přednastavení toocustomize Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="10dc2-105">This topic shows how toocustomize Media Encoder Standard presets.</span></span> <span data-ttu-id="10dc2-106">Hello [kódování s Media Encoder Standard pomocí vlastních předvoleb](media-services-custom-mes-presets-with-dotnet.md) téma ukazuje, jak toocreate .NET toouse kódování úlohy a úlohy, která spustí tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-106">hello [Encoding with Media Encoder Standard using custom presets](media-services-custom-mes-presets-with-dotnet.md) topic shows how toouse .NET toocreate an encoding task and a job that executes this task.</span></span> <span data-ttu-id="10dc2-107">Jakmile upravíte přednastavení, zadejte hello vlastní přednastavení toohello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="10dc2-107">Once you customize a preset, supply hello custom presets toohello encoding task.</span></span> 

>[!NOTE]
><span data-ttu-id="10dc2-108">Pokud používáte přednastavení XML, ujistěte se, že pořadí hello toopreserve prvků, jak je znázorněno v následující ukázky XML (například KeyFrameInterval by měl předcházet SceneChangeDetection).</span><span class="sxs-lookup"><span data-stu-id="10dc2-108">If using an XML preset, make sure toopreserve hello order of elements, as shown in XML samples below (for example, KeyFrameInterval should precede SceneChangeDetection).</span></span>
>

<span data-ttu-id="10dc2-109">V tomto tématu je ukázán hello vlastních předvoleb, které provádět následující úlohy kódování hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-109">In this topic, hello custom presets that perform hello following encoding tasks are demonstrated.</span></span>

## <a name="support-for-relative-sizes"></a><span data-ttu-id="10dc2-110">Podpora pro relativní velikosti</span><span class="sxs-lookup"><span data-stu-id="10dc2-110">Support for relative sizes</span></span>

<span data-ttu-id="10dc2-111">Při vytváření miniatur, není nutné tooalways zadejte výstupní šířky a výšky v pixelech.</span><span class="sxs-lookup"><span data-stu-id="10dc2-111">When generating thumbnails, you do not need tooalways specify output width and height in pixels.</span></span> <span data-ttu-id="10dc2-112">Je můžete zadat v procentech v rozsahu hello [1 %,..., 100 %].</span><span class="sxs-lookup"><span data-stu-id="10dc2-112">You can specify them in percentages, in hello range [1%, …, 100%].</span></span>

### <a name="json-preset"></a><span data-ttu-id="10dc2-113">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-113">JSON preset</span></span>
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a><span data-ttu-id="10dc2-114">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-114">XML preset</span></span>
    <Width>100%</Width>
    <Height>100%</Height>

## <span data-ttu-id="10dc2-115"><a id="thumbnails"></a>Vytváření miniatur</span><span class="sxs-lookup"><span data-stu-id="10dc2-115"><a id="thumbnails"></a>Generate thumbnails</span></span>

<span data-ttu-id="10dc2-116">Tato část uvádí, jak toocustomize přednastavení, která generuje miniatur.</span><span class="sxs-lookup"><span data-stu-id="10dc2-116">This section shows how toocustomize a preset that generates thumbnails.</span></span> <span data-ttu-id="10dc2-117">Hello přednastavení definovaná níže obsahuje informace o tom, jak chcete tooencode souboru stejně jako miniatury toogenerate potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="10dc2-117">hello preset defined below contains information on how you want tooencode your file as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="10dc2-118">Můžete využít některé z přednastavení MES hello zdokumentovaný [to](media-services-mes-presets-overview.md) a přidejte kód, který generuje miniatur.</span><span class="sxs-lookup"><span data-stu-id="10dc2-118">You can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and add code that generates thumbnails.</span></span>  

> [!NOTE]
> <span data-ttu-id="10dc2-119">Hello **SceneChangeDetection** nastavení hello následující přednastavené lze nastavit pouze tootrue Pokud jsou kódování video tooa jednou přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="10dc2-119">hello **SceneChangeDetection** setting in hello following preset can only be set tootrue if you are encoding tooa single  bitrate video.</span></span> <span data-ttu-id="10dc2-120">Pokud jsou kódování více přenosovými rychlostmi tooa video a nastavte **SceneChangeDetection** tootrue, hello kodér vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-120">If you are encoding tooa multi-bitrate video and set **SceneChangeDetection** tootrue, hello encoder returns an error.</span></span>  
>
>

<span data-ttu-id="10dc2-121">Informace o schématu najdete v tématu [to](media-services-mes-schema.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-121">For information about schema, see [this](media-services-mes-schema.md) topic.</span></span>

<span data-ttu-id="10dc2-122">Ujistěte se, zda text hello tooreview [aspekty](#considerations) části.</span><span class="sxs-lookup"><span data-stu-id="10dc2-122">Make sure tooreview hello [Considerations](#considerations) section.</span></span>

### <span data-ttu-id="10dc2-123"><a id="json"></a>Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-123"><a id="json"></a>JSON preset</span></span>
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


### <span data-ttu-id="10dc2-124"><a id="xml"></a>Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-124"><a id="xml"></a>XML preset</span></span>
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

### <a name="considerations"></a><span data-ttu-id="10dc2-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10dc2-125">Considerations</span></span>

<span data-ttu-id="10dc2-126">použít Hello následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="10dc2-126">hello following considerations apply:</span></span>

* <span data-ttu-id="10dc2-127">použití Hello explicitní časová razítka pro spuštění nebo krok nebo rozsah předpokládá, že tento vstupní zdroj hello je dlouhé alespoň 1 minuta.</span><span class="sxs-lookup"><span data-stu-id="10dc2-127">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="10dc2-128">JPG nebo Png nebo BmpImage prvky má spustit, krok a rozsah řetězec atributy – to jde interpretovat jako:</span><span class="sxs-lookup"><span data-stu-id="10dc2-128">Jpg/Png/BmpImage elements have Start, Step, and Range string attributes – these can be interpreted as:</span></span>

  * <span data-ttu-id="10dc2-129">Rámce číslo, pokud jsou nezáporná celá čísla, například "Start": "120",</span><span class="sxs-lookup"><span data-stu-id="10dc2-129">Frame Number if they are non-negative integers, for example "Start": "120",</span></span>
  * <span data-ttu-id="10dc2-130">Doba trvání relativní toosource Pokud vyjádřený jako konci %, například "Start": "15 %", nebo</span><span class="sxs-lookup"><span data-stu-id="10dc2-130">Relative toosource duration if expressed as %-suffixed, for example "Start": "15%", OR</span></span>
  * <span data-ttu-id="10dc2-131">Časové razítko, pokud vyjádřený jako hh: mm:...</span><span class="sxs-lookup"><span data-stu-id="10dc2-131">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="10dc2-132">formátování, například "Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="10dc2-132">format, for example "Start" : "00:01:00"</span></span>

    <span data-ttu-id="10dc2-133">Můžete kombinovat a párovat zápisy, jako je prosím.</span><span class="sxs-lookup"><span data-stu-id="10dc2-133">You can mix and match notations as you please.</span></span>

    <span data-ttu-id="10dc2-134">Kromě toho spustit také podporuje speciální makra: {osvědčené}, který pokusí toodetermine hello první "zajímavé" snímek obsahu hello Poznámka: (krok a rozsah ignorují při spuštění je nastaven příliš {nejlepší})</span><span class="sxs-lookup"><span data-stu-id="10dc2-134">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="10dc2-135">Výchozí nastavení: Spuštění: {nejlepší}</span><span class="sxs-lookup"><span data-stu-id="10dc2-135">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="10dc2-136">Výstupní formát potřebuje toobe explicitně zadaná pro každý formát obrázku: Jpg nebo Png nebo BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="10dc2-136">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="10dc2-137">Pokud jsou k dispozici, odpovídá MES JpgVideo tooJpgFormat a tak dále.</span><span class="sxs-lookup"><span data-stu-id="10dc2-137">When present, MES matches JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="10dc2-138">OutputFormat zavádí nové makro konkrétní kodek obrázků: {Index}, který potřebuje toobe přítomen (jednou a jen jednou) pro výstupní formáty bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="10dc2-138">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <span data-ttu-id="10dc2-139"><a id="trim_video"></a>Trim video (výstřižek.)</span><span class="sxs-lookup"><span data-stu-id="10dc2-139"><a id="trim_video"></a>Trim a video (clipping)</span></span>
<span data-ttu-id="10dc2-140">Tato část rozhovory úprava hello kodér přednastavení tooclip nebo trim hello vstupní video, kde je hello vstup takzvané soubor mezzanine soubor nebo soubor na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="10dc2-140">This section talks about modifying hello encoder presets tooclip or trim hello input video where hello input is a so-called mezzanine file or on-demand file.</span></span> <span data-ttu-id="10dc2-141">Hello kodér lze také použít tooclip nebo trim prostředek, která je zachytit nebo archivovat z živý datový proud – podrobnosti hello jsou k dispozici v [tomto blogu](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="10dc2-141">hello encoder can also be used tooclip or trim an asset, which is captured or archived from a live stream – hello details for this are available in [this blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span></span>

<span data-ttu-id="10dc2-142">tootrim videa, můžete využít některé z přednastavení MES hello zdokumentovaný [to](media-services-mes-presets-overview.md) část a upravte hello **zdroje** – element (jak je znázorněno níže).</span><span class="sxs-lookup"><span data-stu-id="10dc2-142">tootrim your videos, you can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and modify hello **Sources** element (as shown below).</span></span> <span data-ttu-id="10dc2-143">Hodnota Hello StartTime musí vstupní videa hello toomatch časová razítka absolutní hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-143">hello value of StartTime needs toomatch hello absolute timestamps of hello input video.</span></span> <span data-ttu-id="10dc2-144">Například pokud hello první snímek hello vstupní video má časovým razítkem 12:00:10.000, pak čas spuštění by mělo obsahovat aspoň 12:00:10.000 a větší.</span><span class="sxs-lookup"><span data-stu-id="10dc2-144">For example, if hello first frame of hello input video has a timestamp of 12:00:10.000, then StartTime should be at least 12:00:10.000 and greater.</span></span> <span data-ttu-id="10dc2-145">V hello následujícím příkladu předpokládáme, že hello vstupní video má výchozí časové razítko nula.</span><span class="sxs-lookup"><span data-stu-id="10dc2-145">In hello example below, we assume that hello input video has a starting timestamp of zero.</span></span> <span data-ttu-id="10dc2-146">**Zdroje** musí být umístěny na začátku hello přednastavených hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-146">**Sources** should be placed at hello beginning of hello preset.</span></span>

### <span data-ttu-id="10dc2-147"><a id="json"></a>Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-147"><a id="json"></a>JSON preset</span></span>
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

### <a name="xml-preset"></a><span data-ttu-id="10dc2-148">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-148">XML preset</span></span>
<span data-ttu-id="10dc2-149">tootrim videa, můžete využít některé z přednastavení MES hello zdokumentovaný [sem](media-services-mes-presets-overview.md) a upravit hello **zdroje** – element (jak je znázorněno níže).</span><span class="sxs-lookup"><span data-stu-id="10dc2-149">tootrim your videos, you can take any of hello MES presets documented [here](media-services-mes-presets-overview.md) and modify hello **Sources** element (as shown below).</span></span>

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

## <span data-ttu-id="10dc2-150"><a id="overlay"></a>Vytvoření překrytí</span><span class="sxs-lookup"><span data-stu-id="10dc2-150"><a id="overlay"></a>Create an overlay</span></span>

<span data-ttu-id="10dc2-151">Hello Media Encoder Standard umožňuje toooverlay bitovou kopii do existujícího videa.</span><span class="sxs-lookup"><span data-stu-id="10dc2-151">hello Media Encoder Standard allows you toooverlay an image onto an existing video.</span></span> <span data-ttu-id="10dc2-152">V současné době jsou podporovány následující formáty hello: png, jpg, gif, bmp a.</span><span class="sxs-lookup"><span data-stu-id="10dc2-152">Currently, hello following formats are supported: png, jpg, gif, and bmp.</span></span> <span data-ttu-id="10dc2-153">Hello předvolba definovaná níže je základní příklad překryvné video.</span><span class="sxs-lookup"><span data-stu-id="10dc2-153">hello preset defined below is a basic example  of a video overlay.</span></span>

<span data-ttu-id="10dc2-154">Kromě toho toodefining soubor přednastavení, máte také toolet Media Services vědět, který soubor v hello asset je hello překrytí image a který soubor je hello zdroj video, na kterém chcete, aby bitová kopie toooverlay hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-154">In addition toodefining a preset file, you also have toolet Media Services know which file in hello asset is hello overlay image and which file is hello source video onto which you want toooverlay hello image.</span></span> <span data-ttu-id="10dc2-155">video soubor Hello má toobe hello **primární** souboru.</span><span class="sxs-lookup"><span data-stu-id="10dc2-155">hello video file has toobe hello **primary** file.</span></span>

<span data-ttu-id="10dc2-156">Pokud používáte rozhraní .NET, přidejte následující dvě funkce toohello .NET příklad definované v hello [to](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) tématu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-156">If you are using .NET, add hello following two functions toohello .NET example defined in [this](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic.</span></span> <span data-ttu-id="10dc2-157">Hello **UploadMediaFilesFromFolder** funkce odešle soubory ze složky (například BigBuckBunny.mp4 a Image001.png) a nastaví hello mp4 souboru toobe hello primární soubor v hello asset.</span><span class="sxs-lookup"><span data-stu-id="10dc2-157">hello **UploadMediaFilesFromFolder** function uploads files from a folder (for example, BigBuckBunny.mp4 and Image001.png) and sets hello mp4 file toobe hello primary file in hello asset.</span></span> <span data-ttu-id="10dc2-158">Hello **EncodeWithOverlay** funkce používá hello vlastní přednastavené soubor, který byl předán tooit (například hello přednastavení této způsobem) toocreate hello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="10dc2-158">hello **EncodeWithOverlay** function uses hello custom preset file that was passed tooit (for example, hello preset that follows) toocreate hello encoding task.</span></span>


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // hello following code assumes 
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
        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input assets toobe encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset toocontain hello results of hello job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> <span data-ttu-id="10dc2-159">Aktuální omezení:</span><span class="sxs-lookup"><span data-stu-id="10dc2-159">Current limitations:</span></span>
>
> <span data-ttu-id="10dc2-160">Hello překrytí krytí nastavení není podporováno.</span><span class="sxs-lookup"><span data-stu-id="10dc2-160">hello overlay opacity setting is not supported.</span></span>
>
> <span data-ttu-id="10dc2-161">Video zdrojový soubor a soubor bitové kopie překrytí hello mít toobe v hello stejné asset a hello videosoubor potřebám toobe sady jako primární soubor hello v tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="10dc2-161">Your source video file and hello overlay image file have toobe in hello same asset, and hello video file needs toobe set as hello primary file in this asset.</span></span>
>
>

### <a name="json-preset"></a><span data-ttu-id="10dc2-162">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-162">JSON preset</span></span>
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


### <a name="xml-preset"></a><span data-ttu-id="10dc2-163">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-163">XML preset</span></span>
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


## <span data-ttu-id="10dc2-164"><a id="silent_audio"></a>Vložit tichou zvuk sledovat, pokud vstup neobsahuje žádný zvuk</span><span class="sxs-lookup"><span data-stu-id="10dc2-164"><a id="silent_audio"></a>Insert a silent audio track when input has no audio</span></span>
<span data-ttu-id="10dc2-165">Ve výchozím nastavení Pokud odesíláte vstupní toohello kodér, který obsahuje pouze video a žádné zvuk, pak hello výstupní asset obsahuje soubory, které obsahují pouze video data.</span><span class="sxs-lookup"><span data-stu-id="10dc2-165">By default, if you send an input toohello encoder that contains only video, and no audio, then hello output asset contains files that contain only video data.</span></span> <span data-ttu-id="10dc2-166">Některé přehrávače nemusí být možné toohandle takové výstupní datové proudy.</span><span class="sxs-lookup"><span data-stu-id="10dc2-166">Some players may not be able toohandle such output streams.</span></span> <span data-ttu-id="10dc2-167">V tomto scénáři můžete použít tato nastavení tooforce hello kodér tooadd výstup toohello tichou zvuk sledovat.</span><span class="sxs-lookup"><span data-stu-id="10dc2-167">You can use this setting tooforce hello encoder tooadd a silent audio track toohello output in that scenario.</span></span>

<span data-ttu-id="10dc2-168">tooforce hello kodér tooproduce asset, který obsahuje tichou zvuk sledovat, kdy se vstup neobsahuje žádný zvuk, zadejte hodnotu "InsertSilenceIfNoAudio" hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-168">tooforce hello encoder tooproduce an asset that contains a silent audio track when input has no audio, specify hello "InsertSilenceIfNoAudio" value.</span></span>

<span data-ttu-id="10dc2-169">Mohou mít libovolný hello MES přednastavení zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny hello:</span><span class="sxs-lookup"><span data-stu-id="10dc2-169">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

### <a name="json-preset"></a><span data-ttu-id="10dc2-170">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-170">JSON preset</span></span>
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a><span data-ttu-id="10dc2-171">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-171">XML preset</span></span>
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <span data-ttu-id="10dc2-172"><a id="deinterlacing"></a>Zakázat automatické zrušte prokládání.</span><span class="sxs-lookup"><span data-stu-id="10dc2-172"><a id="deinterlacing"></a>Disable auto de-interlacing</span></span>
<span data-ttu-id="10dc2-173">Zákazníci nepotřebují toodo nic Pokud se jako hello prokládání toobe obsah automaticky prokládaných zrušte.</span><span class="sxs-lookup"><span data-stu-id="10dc2-173">Customers don’t need toodo anything if they like hello interlace contents toobe automatically de-interlaced.</span></span> <span data-ttu-id="10dc2-174">Když zrušíte-li automaticky hello prokládání v hello (výchozí), které hello MES automaticky detekce prokládaných snímků a pouze zrušte interlaces rámce označena jako prokládaných.</span><span class="sxs-lookup"><span data-stu-id="10dc2-174">When hello auto de-interlacing is on (default) hello MES does hello auto detection of interlaced frames and only de-interlaces frames marked as interlaced.</span></span>

<span data-ttu-id="10dc2-175">Můžete vypnout hello automaticky algoritmy pro odstranění prokládání.</span><span class="sxs-lookup"><span data-stu-id="10dc2-175">You can turn hello auto de-interlacing off.</span></span> <span data-ttu-id="10dc2-176">Tato možnost se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="10dc2-176">This option is not recommended.</span></span>

### <a name="json-preset"></a><span data-ttu-id="10dc2-177">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-177">JSON preset</span></span>
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a><span data-ttu-id="10dc2-178">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-178">XML preset</span></span>
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <span data-ttu-id="10dc2-179"><a id="audio_only"></a>Pouze přednastavení</span><span class="sxs-lookup"><span data-stu-id="10dc2-179"><a id="audio_only"></a>Audio-only presets</span></span>
<span data-ttu-id="10dc2-180">V této části ukážeme dvou pouze MES přednastavení: AAC zvuk a AAC dobrý kvalitu zvuku.</span><span class="sxs-lookup"><span data-stu-id="10dc2-180">This section demonstrates two audio-only MES presets: AAC Audio and AAC Good Quality Audio.</span></span>

### <a name="aac-audio"></a><span data-ttu-id="10dc2-181">AAC zvuk</span><span class="sxs-lookup"><span data-stu-id="10dc2-181">AAC Audio</span></span>
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

### <a name="aac-good-quality-audio"></a><span data-ttu-id="10dc2-182">AAC kvalitních zvuk</span><span class="sxs-lookup"><span data-stu-id="10dc2-182">AAC Good Quality Audio</span></span>
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

## <span data-ttu-id="10dc2-183"><a id="concatenate"></a>Řetězení dvou nebo více souborů video</span><span class="sxs-lookup"><span data-stu-id="10dc2-183"><a id="concatenate"></a>Concatenate two or more video files</span></span>

<span data-ttu-id="10dc2-184">Hello následující příklad ukazuje, jak můžete vygenerovat přednastavené tooconcatenate dva nebo více video soubory.</span><span class="sxs-lookup"><span data-stu-id="10dc2-184">hello following example illustrates how you can generate a preset tooconcatenate two or more video files.</span></span> <span data-ttu-id="10dc2-185">nejběžnější scénáře Hello je, když chcete tooadd, hlavičku nebo video hlavní toohello přípojného.</span><span class="sxs-lookup"><span data-stu-id="10dc2-185">hello most common scenario is when you want tooadd a header or a trailer toohello main video.</span></span> <span data-ttu-id="10dc2-186">Hello určené použití je v případě videosouborů hello upravovaný společně sdílet vlastnosti (rozlišení videa, obnovovací frekvence, zvuk sledovat počet atd.).</span><span class="sxs-lookup"><span data-stu-id="10dc2-186">hello intended use is when hello video files being edited together share  properties (video resolution, frame rate, audio track count, etc.).</span></span> <span data-ttu-id="10dc2-187">Měli postará není toomix videa různý kmitočet, nebo mají jiný počet zvukových stop.</span><span class="sxs-lookup"><span data-stu-id="10dc2-187">You should take care not toomix videos of different frame rates, or with different number of audio tracks.</span></span>

>[!NOTE]
><span data-ttu-id="10dc2-188">Hello aktuálního návrhu hello zřetězení funkce očekává, že hello vstupní videosoubory jsou konzistentní z hlediska řešení, obnovovací frekvence atd.</span><span class="sxs-lookup"><span data-stu-id="10dc2-188">hello current design of hello concatenation feature expects that hello input video clips are consistent in terms of resolution, frame rate etc.</span></span> 

### <a name="requirements-and-considerations"></a><span data-ttu-id="10dc2-189">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="10dc2-189">Requirements and considerations</span></span>

* <span data-ttu-id="10dc2-190">Vstupní videa by měla mít pouze jeden zvuk sledovat.</span><span class="sxs-lookup"><span data-stu-id="10dc2-190">Input videos should only have one audio track.</span></span>
* <span data-ttu-id="10dc2-191">Vstup videa, by měly mít hello stejné obnovovací frekvence.</span><span class="sxs-lookup"><span data-stu-id="10dc2-191">Input videos should all have hello same frame rate.</span></span>
* <span data-ttu-id="10dc2-192">Musíte nahrát videa do samostatné prostředky a nastavit jako primární soubor hello v každé asset videa hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-192">You must upload your videos into separate assets and set hello videos as hello primary file in each asset.</span></span>
* <span data-ttu-id="10dc2-193">Je nutné tooknow hello trvání videa.</span><span class="sxs-lookup"><span data-stu-id="10dc2-193">You need tooknow hello duration of your videos.</span></span>
* <span data-ttu-id="10dc2-194">Hello přednastavené níže uvedených příkladech se předpokládá, že všechny vstupní videa hello spustit s časovým razítkem nula.</span><span class="sxs-lookup"><span data-stu-id="10dc2-194">hello preset examples below assumes that all hello input videos start with a timestamp of zero.</span></span> <span data-ttu-id="10dc2-195">Je nutné hodnoty StartTime hello toomodify Pokud hello videa mít různé výchozí časové razítko, jako je obvykle hello případě s archivy za provozu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-195">You need toomodify hello StartTime values if hello videos have different starting timestamp, as is typically hello case with live archives.</span></span>
* <span data-ttu-id="10dc2-196">Hello přednastavených JSON umožňuje explicitní odkazy hodnoty ID toohello hello vstupní prostředků.</span><span class="sxs-lookup"><span data-stu-id="10dc2-196">hello JSON preset makes explicit references toohello AssetID values of hello input assets.</span></span>
* <span data-ttu-id="10dc2-197">Hello ukázkový kód předpokládá, že tento hello přednastavení JSON se uložila tooa místního souboru, jako je například "C:\supportFiles\preset.json".</span><span class="sxs-lookup"><span data-stu-id="10dc2-197">hello sample code assumes that hello JSON preset has been saved tooa local file, such as "C:\supportFiles\preset.json".</span></span> <span data-ttu-id="10dc2-198">Předpokládá také, zda byly vytvořeny dva prostředky tím, že nahrajete dva video soubory a že znáte hello výsledné hodnoty ID.</span><span class="sxs-lookup"><span data-stu-id="10dc2-198">It also assumes that two assets have been created by uploading two video files, and that you know hello resultant AssetID values.</span></span>
* <span data-ttu-id="10dc2-199">Hello fragmentu kódu a JSON přednastavených ukazuje příklad zřetězení dva video soubory.</span><span class="sxs-lookup"><span data-stu-id="10dc2-199">hello code snippet and JSON preset shows an example of concatenating two video files.</span></span> <span data-ttu-id="10dc2-200">Můžete ji rozšířit toomore než dva videa pomocí:</span><span class="sxs-lookup"><span data-stu-id="10dc2-200">You can extend it toomore than two videos by:</span></span>

  1. <span data-ttu-id="10dc2-201">Úloha volání. InputAssets.Add() opakovaně tooadd další videa, v pořadí.</span><span class="sxs-lookup"><span data-stu-id="10dc2-201">Calling task.InputAssets.Add() repeatedly tooadd more videos, in order.</span></span>
  2. <span data-ttu-id="10dc2-202">Provedení odpovídající upravuje element toohello "zdroje" v hello formát JSON, můžete přidat další položky v hello stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="10dc2-202">Making corresponding edits toohello "Sources" element in hello JSON, by adding more entries, in hello same order.</span></span>

### <a name="net-code"></a><span data-ttu-id="10dc2-203">Kód .NET</span><span class="sxs-lookup"><span data-stu-id="10dc2-203">.NET code</span></span>

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass tooit hello name of the
    // processor toouse for hello specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load hello XML (or JSON) from hello local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify hello input videos toobe concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset toocontain hello results of hello job.
    // This output is specified as AssetCreationOptions.None, which
    // means hello output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a><span data-ttu-id="10dc2-204">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-204">JSON preset</span></span>

<span data-ttu-id="10dc2-205">Aktualizujte vaše vlastní předvolby s ID hello prostředků, které chcete tooconcatenate a hello příslušnou dobu segmentu pro každý video.</span><span class="sxs-lookup"><span data-stu-id="10dc2-205">Update your custom preset with ids of hello assets that you want tooconcatenate, and with hello appropriate time segment for each video.</span></span>

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

## <span data-ttu-id="10dc2-206"><a id="crop"></a>Oříznutí videa pomocí procesoru Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="10dc2-206"><a id="crop"></a>Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="10dc2-207">V tématu hello [oříznout videa pomocí procesoru Media Encoder Standard](media-services-crop-video.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="10dc2-207">See hello [Crop videos with Media Encoder Standard](media-services-crop-video.md) topic.</span></span>

## <span data-ttu-id="10dc2-208"><a id="no_video"></a>Vložit video sledovat při vstupu je žádné video</span><span class="sxs-lookup"><span data-stu-id="10dc2-208"><a id="no_video"></a>Insert a video track when input has no video</span></span>

<span data-ttu-id="10dc2-209">Ve výchozím nastavení Pokud odesíláte vstupní toohello kodér, který obsahuje pouze zvuk a obraz, pak hello výstupní asset obsahuje soubory, které obsahují pouze zvuková data.</span><span class="sxs-lookup"><span data-stu-id="10dc2-209">By default, if you send an input toohello encoder that contains only audio, and no video, then hello output asset contains files that contain only audio data.</span></span> <span data-ttu-id="10dc2-210">Některé přehrávače, včetně přehrávač médií Azure (viz [to](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nemusí být možné toohandle takové datové proudy.</span><span class="sxs-lookup"><span data-stu-id="10dc2-210">Some players, including Azure Media Player (see [this](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) may not be able toohandle such streams.</span></span> <span data-ttu-id="10dc2-211">Tato nastavení tooforce hello kodér tooadd můžete použít v tomto scénáři výstup toohello černobílý sledovat videa.</span><span class="sxs-lookup"><span data-stu-id="10dc2-211">You can use this setting tooforce hello encoder tooadd a monochrome video track toohello output in that scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="10dc2-212">Vynucení tooinsert kodér hello výstupu videa sledovat zvyšuje hello velikost hello výstupní Asset a tím hello náklady na hello kódování úloh.</span><span class="sxs-lookup"><span data-stu-id="10dc2-212">Forcing hello encoder tooinsert an output video track increases hello size of hello output Asset, and thereby hello cost incurred for hello encoding Task.</span></span> <span data-ttu-id="10dc2-213">Byste měli spustit testy tooverify, který toto výsledné zvýšení má pouze mírné vliv na vaše měsíční poplatky.</span><span class="sxs-lookup"><span data-stu-id="10dc2-213">You should run tests tooverify that this resultant increase has only a modest impact on your monthly charges.</span></span>
>

### <a name="inserting-video-at-only-hello-lowest-bitrate"></a><span data-ttu-id="10dc2-214">Vložení videa na pouze nejnižší přenosovou rychlostí hello</span><span class="sxs-lookup"><span data-stu-id="10dc2-214">Inserting video at only hello lowest bitrate</span></span>

<span data-ttu-id="10dc2-215">Předpokládejme, že jsou pomocí více kódování přenosovou rychlostí, jako přednastavení ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode celého vstupní katalogu pro streamování, který obsahuje kombinaci pouze soubory a videosoubory.</span><span class="sxs-lookup"><span data-stu-id="10dc2-215">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="10dc2-216">V tomto scénáři při vstupu hello je žádné video, můžete chtít tooforce hello kodér tooinsert sledovat videa černobílý tisk na právě hello nejnižší přenosovou rychlostí, jako názvem na rozdíl od tooinserting videa na každou výstupní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="10dc2-216">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at just hello lowest bitrate, as opposed tooinserting video at every output bitrate.</span></span> <span data-ttu-id="10dc2-217">tooachieve, budete potřebovat toouse hello **InsertBlackIfNoVideoBottomLayerOnly** příznak.</span><span class="sxs-lookup"><span data-stu-id="10dc2-217">tooachieve this, you need toouse hello **InsertBlackIfNoVideoBottomLayerOnly** flag.</span></span>

<span data-ttu-id="10dc2-218">Mohou mít libovolný hello MES přednastavení zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny hello:</span><span class="sxs-lookup"><span data-stu-id="10dc2-218">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="10dc2-219">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-219">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="10dc2-220">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-220">XML preset</span></span>

<span data-ttu-id="10dc2-221">Pokud používáte XML, použít podmínku = "InsertBlackIfNoVideoBottomLayerOnly" jako atributu toohello **H264Video** elementu a podmínky = "InsertSilenceIfNoAudio" jako atribut příliš**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="10dc2-221">When using XML, use Condition="InsertBlackIfNoVideoBottomLayerOnly" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

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

### <a name="inserting-video-at-all-output-bitrates"></a><span data-ttu-id="10dc2-222">Vkládání video vůbec výstup přenosových rychlostí</span><span class="sxs-lookup"><span data-stu-id="10dc2-222">Inserting video at all output bitrates</span></span>
<span data-ttu-id="10dc2-223">Předpokládejme, že jsou pomocí více kódování přenosovou rychlostí, jako přednastavení ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode celého vstupní katalogu pro streamování, který obsahuje kombinaci pouze soubory a videosoubory.</span><span class="sxs-lookup"><span data-stu-id="10dc2-223">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="10dc2-224">V tomto scénáři při vstupu hello je žádné video, můžete chtít tooforce hello kodér tooinsert černobílý video sledovat na všechny přenosových rychlostí výstup hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-224">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at all hello output bitrates.</span></span> <span data-ttu-id="10dc2-225">To zajistí, že výstupu prostředky jsou všechny informace, které bylo zajištěno homogenní, s ohledem toonumber sleduje videa a audia sleduje.</span><span class="sxs-lookup"><span data-stu-id="10dc2-225">This ensures that your output Assets are all homogenous with respect toonumber of video tracks and audio tracks.</span></span> <span data-ttu-id="10dc2-226">tooachieve, je nutné toospecify hello příznak "InsertBlackIfNoVideo".</span><span class="sxs-lookup"><span data-stu-id="10dc2-226">tooachieve this, you need toospecify hello "InsertBlackIfNoVideo" flag.</span></span>

<span data-ttu-id="10dc2-227">Mohou mít libovolný hello MES přednastavení zdokumentována [to](media-services-mes-presets-overview.md) části a proveďte následující změny hello:</span><span class="sxs-lookup"><span data-stu-id="10dc2-227">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="10dc2-228">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-228">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="10dc2-229">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-229">XML preset</span></span>

<span data-ttu-id="10dc2-230">Pokud používáte XML, použít podmínku = "InsertBlackIfNoVideo" jako atributu toohello **H264Video** elementu a podmínky = "InsertSilenceIfNoAudio" jako atribut příliš**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="10dc2-230">When using XML, use Condition="InsertBlackIfNoVideo" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

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

## <span data-ttu-id="10dc2-231"><a id="rotate_video"></a>Otočit video</span><span class="sxs-lookup"><span data-stu-id="10dc2-231"><a id="rotate_video"></a>Rotate a video</span></span>
<span data-ttu-id="10dc2-232">Hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) podporuje oběh podle úhly 0/90 nebo 180 nebo 270.</span><span class="sxs-lookup"><span data-stu-id="10dc2-232">hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 0/90/180/270.</span></span> <span data-ttu-id="10dc2-233">Hello výchozí chování je "Auto", kde pokusí toodetect hello otočení metadata v hello příchozí videosoubor a kompenzovat ho.</span><span class="sxs-lookup"><span data-stu-id="10dc2-233">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming video file and compensate for it.</span></span> <span data-ttu-id="10dc2-234">Zahrnout hello následující **zdroje** element tooone hello přednastavení definované v [to](media-services-mes-presets-overview.md) části:</span><span class="sxs-lookup"><span data-stu-id="10dc2-234">Include hello following **Sources** element tooone of hello presets defined in [this](media-services-mes-presets-overview.md) section:</span></span>

### <a name="json-preset"></a><span data-ttu-id="10dc2-235">Přednastavení JSON</span><span class="sxs-lookup"><span data-stu-id="10dc2-235">JSON preset</span></span>
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
### <a name="xml-preset"></a><span data-ttu-id="10dc2-236">Přednastavení XML</span><span class="sxs-lookup"><span data-stu-id="10dc2-236">XML preset</span></span>
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

<span data-ttu-id="10dc2-237">Další informace naleznete v [to](media-services-mes-schema.md#PreserveResolutionAfterRotation) téma pro další informace o interpretace kodér hello: nastavení šířky a výšky hello v hello přednastavení, když se aktivuje otočení kompenzace.</span><span class="sxs-lookup"><span data-stu-id="10dc2-237">Also, see [this](media-services-mes-schema.md#PreserveResolutionAfterRotation) topic for more information on how hello encoder interprets hello Width and Height settings in hello preset, when rotation compensation is triggered.</span></span>

<span data-ttu-id="10dc2-238">Hello hodnotu "0" tooindicate toohello kodér tooignore otočení metadata, můžete použít, pokud existuje ve vstupní video hello.</span><span class="sxs-lookup"><span data-stu-id="10dc2-238">You can use hello value "0" tooindicate toohello encoder tooignore rotation metadata, if present, in hello input video.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="10dc2-239">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="10dc2-239">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="10dc2-240">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="10dc2-240">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="10dc2-241">Viz také</span><span class="sxs-lookup"><span data-stu-id="10dc2-241">See Also</span></span>
[<span data-ttu-id="10dc2-242">Kódování Přehled služby Media Services</span><span class="sxs-lookup"><span data-stu-id="10dc2-242">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)
