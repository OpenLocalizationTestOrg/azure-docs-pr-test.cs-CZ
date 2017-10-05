---
title: Zjistit vzhled a emoce s Azure Media Analytics | Microsoft Docs
description: "Toto téma ukazuje, jak zjistit, řezy a emoce s Azure Media Analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: d7f3bc6c0d21db7adbb0c16c752d4ce49e99da5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a><span data-ttu-id="7782b-103">Zjistit vzhled a emoce s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="7782b-103">Detect Face and Emotion with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="7782b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7782b-104">Overview</span></span>
<span data-ttu-id="7782b-105">**Azure Media vzhled detektor** procesor médií (PP) umožňuje count, sledovat pohybů a i vyhodnocení zapojení cílové skupiny a reakce prostřednictvím obličeje výrazy.</span><span class="sxs-lookup"><span data-stu-id="7782b-105">The **Azure Media Face Detector** media processor (MP) enables you to count, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="7782b-106">Tato služba obsahuje dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="7782b-106">This service contains two features:</span></span> 

* <span data-ttu-id="7782b-107">**Vzhled detekce**</span><span class="sxs-lookup"><span data-stu-id="7782b-107">**Face detection**</span></span>
  
    <span data-ttu-id="7782b-108">Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video.</span><span class="sxs-lookup"><span data-stu-id="7782b-108">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="7782b-109">Více řezy lze zjistit a následně je sledovat jako pohyb se s metadaty čas a umístění, vrátí se v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="7782b-109">Multiple faces can be detected and subsequently be tracked as they move around, with the time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="7782b-110">Při sledování, pokusí se dávat konzistentní ID stejné písmo při osoba, která je manipulaci se na obrazovce, přestože bráněno nebo stručně nechte rámečku.</span><span class="sxs-lookup"><span data-stu-id="7782b-110">During tracking, it will attempt to give a consistent ID to the same face while the person is moving around on screen, even if they are obstructed or briefly leave the frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="7782b-111">Tato služba neprovede rozpoznávání obličeje.</span><span class="sxs-lookup"><span data-stu-id="7782b-111">This service does not perform facial recognition.</span></span> <span data-ttu-id="7782b-112">Osoba, která zůstane rámečku nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.</span><span class="sxs-lookup"><span data-stu-id="7782b-112">An individual who leaves the frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="7782b-113">**Emocí**</span><span class="sxs-lookup"><span data-stu-id="7782b-113">**Emotion detection**</span></span>
  
    <span data-ttu-id="7782b-114">Emocí je volitelná součást procesor média detekce řez, který vrátí analýzy na více duševní atributů z řezy detekuje, včetně štěstí, sadness, obavy, anger a další.</span><span class="sxs-lookup"><span data-stu-id="7782b-114">Emotion Detection is an optional component of the Face Detection Media Processor that returns analysis on multiple emotional attributes from the faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

<span data-ttu-id="7782b-115">**Azure Media vzhled detektor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="7782b-115">The **Azure Media Face Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="7782b-116">Toto téma uvádí podrobnosti o **Azure Media vzhled detektor** a ukazuje, jak pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="7782b-116">This topic gives details about  **Azure Media Face Detector** and shows how to use it with Media Services SDK for .NET.</span></span>

## <a name="face-detector-input-files"></a><span data-ttu-id="7782b-117">Čelí detektor vstupní soubory</span><span class="sxs-lookup"><span data-stu-id="7782b-117">Face Detector input files</span></span>
<span data-ttu-id="7782b-118">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="7782b-118">Video files.</span></span> <span data-ttu-id="7782b-119">V současné době jsou podporovány následující formáty: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="7782b-119">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="face-detector-output-files"></a><span data-ttu-id="7782b-120">Čelí detektor výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="7782b-120">Face Detector output files</span></span>
<span data-ttu-id="7782b-121">Rozhraní API pro detekci a sledování vzhled poskytuje vysokou přesnost vzhled umístění zjišťování a sledování, které může zjistit až 64 lidského tyto řezy v videa.</span><span class="sxs-lookup"><span data-stu-id="7782b-121">The face detection and tracking API provides high precision face location detection and tracking that can detect up to 64 human faces in a video.</span></span> <span data-ttu-id="7782b-122">Čelní řezy zadejte nejlepších výsledků dosáhnete, při straně řezy a malé řezy (je menší než nebo rovno 24 x 24 pixelů), nemusí být jako přesné.</span><span class="sxs-lookup"><span data-stu-id="7782b-122">Frontal faces provide the best results, while side faces and small faces (less than or equal to 24x24 pixels) might not be as accurate.</span></span>

<span data-ttu-id="7782b-123">Zjištěné a sledovaných řezy jsou vráceny pomocí souřadnic (vlevo, top, šířku a výšku) označující umístění tyto řezy v obrázku v pixelech, stejně jako číslo ID vzhled určující sledování to jednotlivých.</span><span class="sxs-lookup"><span data-stu-id="7782b-123">The detected and tracked faces are returned with coordinates (left, top, width, and height) indicating the location of faces in the image in pixels, as well as a face ID number indicating the tracking of that individual.</span></span> <span data-ttu-id="7782b-124">Čísla ID vzhled jsou náchylné k resetování okolností v případě, že dojde ke ztrátě nebo překryté v rámečku, čelní tučné výsledkem některé jednotlivce získávání přiřazeno více ID.</span><span class="sxs-lookup"><span data-stu-id="7782b-124">Face ID numbers are prone to reset under circumstances when the frontal face is lost or overlapped in the frame, resulting in some individuals getting assigned multiple IDs.</span></span>

## <span data-ttu-id="7782b-125"><a id="output_elements"></a>Elementy výstupního souboru JSON</span><span class="sxs-lookup"><span data-stu-id="7782b-125"><a id="output_elements"></a>Elements of the output JSON file</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

<span data-ttu-id="7782b-126">Čelí detektor používá techniky fragmentace (kde metadata může dojít k rozdělení v založené na čase bloky a můžete si stáhnout pouze to, co potřebujete) a segmentace (kde události jsou k rozdělení v případě, že získají příliš velký).</span><span class="sxs-lookup"><span data-stu-id="7782b-126">Face Detector uses techniques of fragmentation (where the metadata can be broken up in time-based chunks and you can download only what you need), and segmentation (where the events are broken up in case they get too large).</span></span> <span data-ttu-id="7782b-127">Některé jednoduché výpočty můžete transformovat data.</span><span class="sxs-lookup"><span data-stu-id="7782b-127">Some simple calculations can help you transform the data.</span></span> <span data-ttu-id="7782b-128">Například, pokud událost zahájená 6300 (rysky), s časovou 2997 (rysky za sekundu) a kmitočet snímků 29,97 (snímků za sekundu), pak:</span><span class="sxs-lookup"><span data-stu-id="7782b-128">For example, if an event started at 6300 (ticks), with a timescale of 2997 (ticks/sec) and framerate of 29.97 (frames/sec), then:</span></span>

* <span data-ttu-id="7782b-129">Spuštění nebo časový rámec = 2.1 sekund</span><span class="sxs-lookup"><span data-stu-id="7782b-129">Start/Timescale = 2.1 seconds</span></span>
* <span data-ttu-id="7782b-130">Sekund x Framerate = 63 rámce</span><span class="sxs-lookup"><span data-stu-id="7782b-130">Seconds x Framerate = 63 frames</span></span>

## <a name="face-detection-input-and-output-example"></a><span data-ttu-id="7782b-131">Čelí detekce vstup a výstup příklad</span><span class="sxs-lookup"><span data-stu-id="7782b-131">Face detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="7782b-132">Vstupní video</span><span class="sxs-lookup"><span data-stu-id="7782b-132">Input video</span></span>
[<span data-ttu-id="7782b-133">Vstupní Video</span><span class="sxs-lookup"><span data-stu-id="7782b-133">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="7782b-134">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="7782b-134">Task configuration (preset)</span></span>
<span data-ttu-id="7782b-135">Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7782b-135">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="7782b-136">Následující předvolba konfigurace je jen pro zjišťování řez.</span><span class="sxs-lookup"><span data-stu-id="7782b-136">The following configuration preset is just for face detection.</span></span>

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a><span data-ttu-id="7782b-137">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="7782b-137">Attribute descriptions</span></span>
| <span data-ttu-id="7782b-138">Název atributu</span><span class="sxs-lookup"><span data-stu-id="7782b-138">Attribute name</span></span> | <span data-ttu-id="7782b-139">Popis</span><span class="sxs-lookup"><span data-stu-id="7782b-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7782b-140">Režim</span><span class="sxs-lookup"><span data-stu-id="7782b-140">Mode</span></span> |<span data-ttu-id="7782b-141">Rychlé - se rychlé zpracování rychlostí, ale méně přesný (výchozí).</span><span class="sxs-lookup"><span data-stu-id="7782b-141">Fast - fast processing speed, but less accurate (default).</span></span>|

### <a name="json-output"></a><span data-ttu-id="7782b-142">Výstup JSON</span><span class="sxs-lookup"><span data-stu-id="7782b-142">JSON output</span></span>
<span data-ttu-id="7782b-143">Následující příklad výstupu JSON byl zkrácen.</span><span class="sxs-lookup"><span data-stu-id="7782b-143">The following example of JSON output was truncated.</span></span>

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a><span data-ttu-id="7782b-144">Emocí vstup a výstup příklad</span><span class="sxs-lookup"><span data-stu-id="7782b-144">Emotion detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="7782b-145">Vstupní video</span><span class="sxs-lookup"><span data-stu-id="7782b-145">Input video</span></span>
[<span data-ttu-id="7782b-146">Vstupní Video</span><span class="sxs-lookup"><span data-stu-id="7782b-146">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="7782b-147">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="7782b-147">Task configuration (preset)</span></span>
<span data-ttu-id="7782b-148">Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7782b-148">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="7782b-149">Následující konfigurace přednastavení určuje vytvořit JSON podle detekce rozpoznávání emocí úrovně.</span><span class="sxs-lookup"><span data-stu-id="7782b-149">The following configuration preset specifies to create JSON based on the emotion detection.</span></span>

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a><span data-ttu-id="7782b-150">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="7782b-150">Attribute descriptions</span></span>
| <span data-ttu-id="7782b-151">Název atributu</span><span class="sxs-lookup"><span data-stu-id="7782b-151">Attribute name</span></span> | <span data-ttu-id="7782b-152">Popis</span><span class="sxs-lookup"><span data-stu-id="7782b-152">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7782b-153">Režim</span><span class="sxs-lookup"><span data-stu-id="7782b-153">Mode</span></span> |<span data-ttu-id="7782b-154">Řezy: Pouze čelí detekce.</span><span class="sxs-lookup"><span data-stu-id="7782b-154">Faces: Only face detection.</span></span><br/><span data-ttu-id="7782b-155">PerFaceEmotion: Vrátí rozpoznávání emocí úrovně nezávisle pro každý řez zjišťování.</span><span class="sxs-lookup"><span data-stu-id="7782b-155">PerFaceEmotion: Return emotion independently for each face detection.</span></span><br/><span data-ttu-id="7782b-156">AggregateEmotion: Návratový průměrná rozpoznávání emocí úrovně hodnoty pro všechny tyto řezy v rámečku.</span><span class="sxs-lookup"><span data-stu-id="7782b-156">AggregateEmotion: Return average emotion values for all faces in frame.</span></span> |
| <span data-ttu-id="7782b-157">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="7782b-157">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="7782b-158">Použijte, pokud je vybrána AggregateEmotion režimu.</span><span class="sxs-lookup"><span data-stu-id="7782b-158">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="7782b-159">Určuje délku video, na které se používají k vytvoření každý agregační výsledek v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="7782b-159">Specifies the length of video used to produce each aggregate result, in milliseconds.</span></span> |
| <span data-ttu-id="7782b-160">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="7782b-160">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="7782b-161">Použijte, pokud je vybrána AggregateEmotion režimu.</span><span class="sxs-lookup"><span data-stu-id="7782b-161">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="7782b-162">Určuje, jak často k vytvoření agregačních výsledků.</span><span class="sxs-lookup"><span data-stu-id="7782b-162">Specifies with what frequency to produce aggregate results.</span></span> |

#### <a name="aggregate-defaults"></a><span data-ttu-id="7782b-163">Agregační výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="7782b-163">Aggregate defaults</span></span>
<span data-ttu-id="7782b-164">Níže jsou doporučené hodnoty pro nastavení agregační okno a intervalu.</span><span class="sxs-lookup"><span data-stu-id="7782b-164">Below are recommended values for the aggregate window and interval settings.</span></span> <span data-ttu-id="7782b-165">AggregateEmotionWindowMs by měl být delší než AggregateEmotionIntervalMs.</span><span class="sxs-lookup"><span data-stu-id="7782b-165">AggregateEmotionWindowMs should be longer than AggregateEmotionIntervalMs.</span></span>

|| <span data-ttu-id="7782b-166">Výchozí hodnoty (s)</span><span class="sxs-lookup"><span data-stu-id="7782b-166">Defaults(s)</span></span> | <span data-ttu-id="7782b-167">Min(s)</span><span class="sxs-lookup"><span data-stu-id="7782b-167">Min(s)</span></span> | <span data-ttu-id="7782b-168">Max(s)</span><span class="sxs-lookup"><span data-stu-id="7782b-168">Max(s)</span></span> |
|--- | --- | --- | --- |
| <span data-ttu-id="7782b-169">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="7782b-169">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="7782b-170">0.5</span><span class="sxs-lookup"><span data-stu-id="7782b-170">0.5</span></span> |<span data-ttu-id="7782b-171">2</span><span class="sxs-lookup"><span data-stu-id="7782b-171">2</span></span> |<span data-ttu-id="7782b-172">0.25</span><span class="sxs-lookup"><span data-stu-id="7782b-172">0.25</span></span>|
| <span data-ttu-id="7782b-173">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="7782b-173">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="7782b-174">0.5</span><span class="sxs-lookup"><span data-stu-id="7782b-174">0.5</span></span> |<span data-ttu-id="7782b-175">1</span><span class="sxs-lookup"><span data-stu-id="7782b-175">1</span></span> |<span data-ttu-id="7782b-176">0.25</span><span class="sxs-lookup"><span data-stu-id="7782b-176">0.25</span></span>|

### <a name="json-output"></a><span data-ttu-id="7782b-177">Výstup JSON</span><span class="sxs-lookup"><span data-stu-id="7782b-177">JSON output</span></span>
<span data-ttu-id="7782b-178">Výstup pro agregační rozpoznávání emocí úrovně (zkrácený) ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="7782b-178">JSON output for aggregate emotion (truncated):</span></span>

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,

## <a name="limitations"></a><span data-ttu-id="7782b-179">Omezení</span><span class="sxs-lookup"><span data-stu-id="7782b-179">Limitations</span></span>
* <span data-ttu-id="7782b-180">Podporované formáty vstupní video zahrnují MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="7782b-180">The supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="7782b-181">Rozsah velikost rozpoznat řez je 24 x 24 k 2048 x 2048 bodů.</span><span class="sxs-lookup"><span data-stu-id="7782b-181">The detectable face size range is 24x24 to 2048x2048 pixels.</span></span> <span data-ttu-id="7782b-182">Řezy mimo tento rozsah nebudou zjištěna.</span><span class="sxs-lookup"><span data-stu-id="7782b-182">The faces out of this range will not be detected.</span></span>
* <span data-ttu-id="7782b-183">Pro každý video je maximální počet řezy vrátil 64.</span><span class="sxs-lookup"><span data-stu-id="7782b-183">For each video, the maximum number of faces returned is 64.</span></span>
* <span data-ttu-id="7782b-184">Některé řezy nemusí být detekována z důvodu technické problémy; například velké vzhled úhly (head pozice) a velké NF pásmová.</span><span class="sxs-lookup"><span data-stu-id="7782b-184">Some faces may not be detected due to technical challenges; e.g. very large face angles (head-pose), and large occlusion.</span></span> <span data-ttu-id="7782b-185">Čelní a téměř čelní strany mít nejlepších výsledků.</span><span class="sxs-lookup"><span data-stu-id="7782b-185">Frontal and near-frontal faces have the best results.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="7782b-186">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="7782b-186">.NET sample code</span></span>

<span data-ttu-id="7782b-187">Program zobrazí následující postup:</span><span class="sxs-lookup"><span data-stu-id="7782b-187">The following program shows how to:</span></span>

1. <span data-ttu-id="7782b-188">Vytvořte asset a nahrajte soubor média do assetu.</span><span class="sxs-lookup"><span data-stu-id="7782b-188">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="7782b-189">Vytvořte úlohu s úkolem detekce vzhled podle konfigurační soubor, který obsahuje následující přednastavení json.</span><span class="sxs-lookup"><span data-stu-id="7782b-189">Create a job with a face detection task based on a configuration file that contains the following json preset.</span></span> 
   
        {
            "version": "1.0"
        }
3. <span data-ttu-id="7782b-190">Stáhněte soubory JSON výstupu.</span><span class="sxs-lookup"><span data-stu-id="7782b-190">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="7782b-191">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="7782b-191">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="7782b-192">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="7782b-192">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="7782b-193">Příklad</span><span class="sxs-lookup"><span data-stu-id="7782b-193">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceDetection
    {
        class Program
        {
            private static readonly string _AADTenantDomain =
                      ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                      ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                // Run the FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference to Azure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
        }
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="7782b-194">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="7782b-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7782b-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7782b-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="7782b-196">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="7782b-196">Related links</span></span>
[<span data-ttu-id="7782b-197">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="7782b-197">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="7782b-198">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="7782b-198">Azure Media Analytics demos</span></span>](http://amslabs.azurewebsites.net/demos/Analytics.html)

