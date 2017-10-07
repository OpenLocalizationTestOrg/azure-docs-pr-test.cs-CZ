---
title: aaaDetect vzhled a emoce s Azure Media Analytics | Microsoft Docs
description: "Toto téma ukazuje, jak toodetect otočená a emoce s Azure Media Analytics."
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
ms.openlocfilehash: f58d81d82dde08a694cdb4d92c6bab6a40a9c157
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a><span data-ttu-id="10f86-103">Zjistit vzhled a emoce s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="10f86-103">Detect Face and Emotion with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="10f86-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="10f86-104">Overview</span></span>
<span data-ttu-id="10f86-105">Hello **Azure Media vzhled detektor** procesor médií (PP) vám umožní toocount, sledovat pohybů a i zapojení cílové skupiny měřidla a reakce prostřednictvím obličeje výrazy.</span><span class="sxs-lookup"><span data-stu-id="10f86-105">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="10f86-106">Tato služba obsahuje dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="10f86-106">This service contains two features:</span></span> 

* <span data-ttu-id="10f86-107">**Vzhled detekce**</span><span class="sxs-lookup"><span data-stu-id="10f86-107">**Face detection**</span></span>
  
    <span data-ttu-id="10f86-108">Vzhled zjišťování vyhledá a sleduje lidského tyto řezy v rámci video.</span><span class="sxs-lookup"><span data-stu-id="10f86-108">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="10f86-109">Více řezy lze zjistit a následně je sledovat jako pohyb se s hello čas a umístění metadata vrácená v souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="10f86-109">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="10f86-110">Při sledování pokusí toogive konzistentní toohello ID stejné čelí, zatímco je osoba hello pohyb na obrazovce, přestože bráněno nebo stručně nechte hello rámce.</span><span class="sxs-lookup"><span data-stu-id="10f86-110">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="10f86-111">Tato služba neprovede rozpoznávání obličeje.</span><span class="sxs-lookup"><span data-stu-id="10f86-111">This service does not perform facial recognition.</span></span> <span data-ttu-id="10f86-112">Osoba, která zůstane hello rámce nebo se stane nelze blokovat. pro příliš dlouho přidělí nové ID když se vrátí.</span><span class="sxs-lookup"><span data-stu-id="10f86-112">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="10f86-113">**Emocí**</span><span class="sxs-lookup"><span data-stu-id="10f86-113">**Emotion detection**</span></span>
  
    <span data-ttu-id="10f86-114">Emocí je volitelná součást hello vzhled detekce média procesor, který vrátí analýzy na více duševní atributů z hello řezy detekuje, včetně štěstí, sadness, obavy, anger a další.</span><span class="sxs-lookup"><span data-stu-id="10f86-114">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

<span data-ttu-id="10f86-115">Hello **Azure Media vzhled detektor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="10f86-115">hello **Azure Media Face Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="10f86-116">Toto téma uvádí podrobnosti o **Azure Media vzhled detektor** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="10f86-116">This topic gives details about  **Azure Media Face Detector** and shows how toouse it with Media Services SDK for .NET.</span></span>

## <a name="face-detector-input-files"></a><span data-ttu-id="10f86-117">Čelí detektor vstupní soubory</span><span class="sxs-lookup"><span data-stu-id="10f86-117">Face Detector input files</span></span>
<span data-ttu-id="10f86-118">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="10f86-118">Video files.</span></span> <span data-ttu-id="10f86-119">V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="10f86-119">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="face-detector-output-files"></a><span data-ttu-id="10f86-120">Čelí detektor výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="10f86-120">Face Detector output files</span></span>
<span data-ttu-id="10f86-121">Hello vzhled detekce a sledování rozhraní API poskytuje vysokou přesnost vzhled umístění zjišťování a sledování, které může zjistit, až too64 lidské řezy v video.</span><span class="sxs-lookup"><span data-stu-id="10f86-121">hello face detection and tracking API provides high precision face location detection and tracking that can detect up too64 human faces in a video.</span></span> <span data-ttu-id="10f86-122">Čelní řezy zadejte hello nejlepších výsledků při straně řezy a malé řezy (menší než nebo roven hodnotě too24x24 pixelů) nemusí být jako přesné.</span><span class="sxs-lookup"><span data-stu-id="10f86-122">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) might not be as accurate.</span></span>

<span data-ttu-id="10f86-123">Hello zjištěné a sledovaných řezy jsou vráceny pomocí souřadnic (vlevo, top, šířku a výšku) určující hello umístění řezy hello obrázku v pixelech, jakož i vzhled ID číslo označující hello to jednotlivých sledování.</span><span class="sxs-lookup"><span data-stu-id="10f86-123">hello detected and tracked faces are returned with coordinates (left, top, width, and height) indicating hello location of faces in hello image in pixels, as well as a face ID number indicating hello tracking of that individual.</span></span> <span data-ttu-id="10f86-124">Čísla ID vzhled jsou náchylné k chybám tooreset okolností při čelní vzhled hello dojde ke ztrátě nebo překryté hello rámce, výsledkem některé jednotlivce získávání přiřazeno více ID.</span><span class="sxs-lookup"><span data-stu-id="10f86-124">Face ID numbers are prone tooreset under circumstances when hello frontal face is lost or overlapped in hello frame, resulting in some individuals getting assigned multiple IDs.</span></span>

## <span data-ttu-id="10f86-125"><a id="output_elements"></a>Elementy výstupního souboru JSON, hello</span><span class="sxs-lookup"><span data-stu-id="10f86-125"><a id="output_elements"></a>Elements of hello output JSON file</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

<span data-ttu-id="10f86-126">Detektor vzhled používá techniky fragmentace (kde hello metadata může dojít k rozdělení v založené na čase bloky a můžete si stáhnout pouze to, co potřebujete) a segmentace (kde hello události jsou k rozdělení v případě, že získají příliš velký).</span><span class="sxs-lookup"><span data-stu-id="10f86-126">Face Detector uses techniques of fragmentation (where hello metadata can be broken up in time-based chunks and you can download only what you need), and segmentation (where hello events are broken up in case they get too large).</span></span> <span data-ttu-id="10f86-127">Některé jednoduché výpočty můžete transformovat hello data.</span><span class="sxs-lookup"><span data-stu-id="10f86-127">Some simple calculations can help you transform hello data.</span></span> <span data-ttu-id="10f86-128">Například, pokud událost zahájená 6300 (rysky), s časovou 2997 (rysky za sekundu) a kmitočet snímků 29,97 (snímků za sekundu), pak:</span><span class="sxs-lookup"><span data-stu-id="10f86-128">For example, if an event started at 6300 (ticks), with a timescale of 2997 (ticks/sec) and framerate of 29.97 (frames/sec), then:</span></span>

* <span data-ttu-id="10f86-129">Spuštění nebo časový rámec = 2.1 sekund</span><span class="sxs-lookup"><span data-stu-id="10f86-129">Start/Timescale = 2.1 seconds</span></span>
* <span data-ttu-id="10f86-130">Sekund x Framerate = 63 rámce</span><span class="sxs-lookup"><span data-stu-id="10f86-130">Seconds x Framerate = 63 frames</span></span>

## <a name="face-detection-input-and-output-example"></a><span data-ttu-id="10f86-131">Čelí detekce vstup a výstup příklad</span><span class="sxs-lookup"><span data-stu-id="10f86-131">Face detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="10f86-132">Vstupní video</span><span class="sxs-lookup"><span data-stu-id="10f86-132">Input video</span></span>
[<span data-ttu-id="10f86-133">Vstupní Video</span><span class="sxs-lookup"><span data-stu-id="10f86-133">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="10f86-134">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="10f86-134">Task configuration (preset)</span></span>
<span data-ttu-id="10f86-135">Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="10f86-135">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="10f86-136">Hello následující přednastavených konfigurace platí jenom pro zjišťování řez.</span><span class="sxs-lookup"><span data-stu-id="10f86-136">hello following configuration preset is just for face detection.</span></span>

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a><span data-ttu-id="10f86-137">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="10f86-137">Attribute descriptions</span></span>
| <span data-ttu-id="10f86-138">Název atributu</span><span class="sxs-lookup"><span data-stu-id="10f86-138">Attribute name</span></span> | <span data-ttu-id="10f86-139">Popis</span><span class="sxs-lookup"><span data-stu-id="10f86-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="10f86-140">Režim</span><span class="sxs-lookup"><span data-stu-id="10f86-140">Mode</span></span> |<span data-ttu-id="10f86-141">Rychlé - se rychlé zpracování rychlostí, ale méně přesný (výchozí).</span><span class="sxs-lookup"><span data-stu-id="10f86-141">Fast - fast processing speed, but less accurate (default).</span></span>|

### <a name="json-output"></a><span data-ttu-id="10f86-142">Výstup JSON</span><span class="sxs-lookup"><span data-stu-id="10f86-142">JSON output</span></span>
<span data-ttu-id="10f86-143">Následující příklad výstupu JSON Hello byl zkrácen.</span><span class="sxs-lookup"><span data-stu-id="10f86-143">hello following example of JSON output was truncated.</span></span>

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

## <a name="emotion-detection-input-and-output-example"></a><span data-ttu-id="10f86-144">Emocí vstup a výstup příklad</span><span class="sxs-lookup"><span data-stu-id="10f86-144">Emotion detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="10f86-145">Vstupní video</span><span class="sxs-lookup"><span data-stu-id="10f86-145">Input video</span></span>
[<span data-ttu-id="10f86-146">Vstupní Video</span><span class="sxs-lookup"><span data-stu-id="10f86-146">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="10f86-147">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="10f86-147">Task configuration (preset)</span></span>
<span data-ttu-id="10f86-148">Při vytváření úlohy s **Azure Media vzhled detektor**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="10f86-148">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="10f86-149">Následující konfigurace přednastavených Hello určuje toocreate JSON v závislosti na emocí hello.</span><span class="sxs-lookup"><span data-stu-id="10f86-149">hello following configuration preset specifies toocreate JSON based on hello emotion detection.</span></span>

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a><span data-ttu-id="10f86-150">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="10f86-150">Attribute descriptions</span></span>
| <span data-ttu-id="10f86-151">Název atributu</span><span class="sxs-lookup"><span data-stu-id="10f86-151">Attribute name</span></span> | <span data-ttu-id="10f86-152">Popis</span><span class="sxs-lookup"><span data-stu-id="10f86-152">Description</span></span> |
| --- | --- |
| <span data-ttu-id="10f86-153">Režim</span><span class="sxs-lookup"><span data-stu-id="10f86-153">Mode</span></span> |<span data-ttu-id="10f86-154">Řezy: Pouze čelí detekce.</span><span class="sxs-lookup"><span data-stu-id="10f86-154">Faces: Only face detection.</span></span><br/><span data-ttu-id="10f86-155">PerFaceEmotion: Vrátí rozpoznávání emocí úrovně nezávisle pro každý řez zjišťování.</span><span class="sxs-lookup"><span data-stu-id="10f86-155">PerFaceEmotion: Return emotion independently for each face detection.</span></span><br/><span data-ttu-id="10f86-156">AggregateEmotion: Návratový průměrná rozpoznávání emocí úrovně hodnoty pro všechny tyto řezy v rámečku.</span><span class="sxs-lookup"><span data-stu-id="10f86-156">AggregateEmotion: Return average emotion values for all faces in frame.</span></span> |
| <span data-ttu-id="10f86-157">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="10f86-157">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="10f86-158">Použijte, pokud je vybrána AggregateEmotion režimu.</span><span class="sxs-lookup"><span data-stu-id="10f86-158">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="10f86-159">Určuje délku hello video použité tooproduce každý výsledek agregační v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="10f86-159">Specifies hello length of video used tooproduce each aggregate result, in milliseconds.</span></span> |
| <span data-ttu-id="10f86-160">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="10f86-160">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="10f86-161">Použijte, pokud je vybrána AggregateEmotion režimu.</span><span class="sxs-lookup"><span data-stu-id="10f86-161">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="10f86-162">Určuje s výsledky jaká frekvence tooproduce agregace.</span><span class="sxs-lookup"><span data-stu-id="10f86-162">Specifies with what frequency tooproduce aggregate results.</span></span> |

#### <a name="aggregate-defaults"></a><span data-ttu-id="10f86-163">Agregační výchozí hodnoty</span><span class="sxs-lookup"><span data-stu-id="10f86-163">Aggregate defaults</span></span>
<span data-ttu-id="10f86-164">Níže jsou doporučené hodnoty pro agregační oddílové hello a nastavení intervalu.</span><span class="sxs-lookup"><span data-stu-id="10f86-164">Below are recommended values for hello aggregate window and interval settings.</span></span> <span data-ttu-id="10f86-165">AggregateEmotionWindowMs by měl být delší než AggregateEmotionIntervalMs.</span><span class="sxs-lookup"><span data-stu-id="10f86-165">AggregateEmotionWindowMs should be longer than AggregateEmotionIntervalMs.</span></span>

|| <span data-ttu-id="10f86-166">Výchozí hodnoty (s)</span><span class="sxs-lookup"><span data-stu-id="10f86-166">Defaults(s)</span></span> | <span data-ttu-id="10f86-167">Min(s)</span><span class="sxs-lookup"><span data-stu-id="10f86-167">Min(s)</span></span> | <span data-ttu-id="10f86-168">Max(s)</span><span class="sxs-lookup"><span data-stu-id="10f86-168">Max(s)</span></span> |
|--- | --- | --- | --- |
| <span data-ttu-id="10f86-169">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="10f86-169">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="10f86-170">0.5</span><span class="sxs-lookup"><span data-stu-id="10f86-170">0.5</span></span> |<span data-ttu-id="10f86-171">2</span><span class="sxs-lookup"><span data-stu-id="10f86-171">2</span></span> |<span data-ttu-id="10f86-172">0.25</span><span class="sxs-lookup"><span data-stu-id="10f86-172">0.25</span></span>|
| <span data-ttu-id="10f86-173">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="10f86-173">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="10f86-174">0.5</span><span class="sxs-lookup"><span data-stu-id="10f86-174">0.5</span></span> |<span data-ttu-id="10f86-175">1</span><span class="sxs-lookup"><span data-stu-id="10f86-175">1</span></span> |<span data-ttu-id="10f86-176">0.25</span><span class="sxs-lookup"><span data-stu-id="10f86-176">0.25</span></span>|

### <a name="json-output"></a><span data-ttu-id="10f86-177">Výstup JSON</span><span class="sxs-lookup"><span data-stu-id="10f86-177">JSON output</span></span>
<span data-ttu-id="10f86-178">Výstup pro agregační rozpoznávání emocí úrovně (zkrácený) ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="10f86-178">JSON output for aggregate emotion (truncated):</span></span>

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

## <a name="limitations"></a><span data-ttu-id="10f86-179">Omezení</span><span class="sxs-lookup"><span data-stu-id="10f86-179">Limitations</span></span>
* <span data-ttu-id="10f86-180">vstupní video formáty Hello podporována jsou MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="10f86-180">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="10f86-181">rozsah velikost Hello rozpoznat řez je 24 x 24 too2048x2048 pixelů.</span><span class="sxs-lookup"><span data-stu-id="10f86-181">hello detectable face size range is 24x24 too2048x2048 pixels.</span></span> <span data-ttu-id="10f86-182">Hello řezy mimo tento rozsah nebudou zjištěna.</span><span class="sxs-lookup"><span data-stu-id="10f86-182">hello faces out of this range will not be detected.</span></span>
* <span data-ttu-id="10f86-183">Pro každý video hello maximální počet řezy vrátil je 64.</span><span class="sxs-lookup"><span data-stu-id="10f86-183">For each video, hello maximum number of faces returned is 64.</span></span>
* <span data-ttu-id="10f86-184">Některé řezy nemusí být detekována z důvodu problémů tootechnical; například velké vzhled úhly (head pozice) a velké NF pásmová.</span><span class="sxs-lookup"><span data-stu-id="10f86-184">Some faces may not be detected due tootechnical challenges; e.g. very large face angles (head-pose), and large occlusion.</span></span> <span data-ttu-id="10f86-185">Čelní a téměř čelní strany mít nejlepších výsledků dosáhnete hello.</span><span class="sxs-lookup"><span data-stu-id="10f86-185">Frontal and near-frontal faces have hello best results.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="10f86-186">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="10f86-186">.NET sample code</span></span>

<span data-ttu-id="10f86-187">ukazuje programu Hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="10f86-187">hello following program shows how to:</span></span>

1. <span data-ttu-id="10f86-188">Vytvořte asset a nahrajte soubor média do hello asset.</span><span class="sxs-lookup"><span data-stu-id="10f86-188">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="10f86-189">Vytvořte úlohu s úkolem detekce vzhled podle konfigurační soubor, který obsahuje následující json přednastavených hello.</span><span class="sxs-lookup"><span data-stu-id="10f86-189">Create a job with a face detection task based on a configuration file that contains hello following json preset.</span></span> 
   
        {
            "version": "1.0"
        }
3. <span data-ttu-id="10f86-190">Stáhněte soubory JSON výstup hello.</span><span class="sxs-lookup"><span data-stu-id="10f86-190">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="10f86-191">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="10f86-191">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="10f86-192">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="10f86-192">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="10f86-193">Příklad</span><span class="sxs-lookup"><span data-stu-id="10f86-193">Example</span></span>

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

                // Run hello FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference tooAzure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

                // Use hello following event handler toocheck job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch hello job.
                job.Submit();

                // Check job execution and wait for job toofinish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, hello event handling
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="10f86-194">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="10f86-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="10f86-195">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="10f86-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="10f86-196">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="10f86-196">Related links</span></span>
[<span data-ttu-id="10f86-197">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="10f86-197">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="10f86-198">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="10f86-198">Azure Media Analytics demos</span></span>](http://amslabs.azurewebsites.net/demos/Analytics.html)

