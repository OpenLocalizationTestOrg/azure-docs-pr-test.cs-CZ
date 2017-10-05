---
title: "Zjištění pohyby s Azure Media Analytics | Microsoft Docs"
description: "Procesor médií detektor pohybu médií Azure (PP) umožňuje efektivně identifikaci části týkající se v rámci souboru jinak dlouhé a bezproblémové video."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 115ad9dfd88062f23d5d17eed8897ce5d2ca8484
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a><span data-ttu-id="96525-103">Zjištění pohyby s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="96525-103">Detect Motions with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="96525-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="96525-104">Overview</span></span>
<span data-ttu-id="96525-105">**Detektor pohybu médií Azure** procesor médií (PP) umožňuje efektivně identifikaci části týkající se v rámci souboru jinak dlouhé a bezproblémové video.</span><span class="sxs-lookup"><span data-stu-id="96525-105">The **Azure Media Motion Detector** media processor (MP) enables you to efficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="96525-106">Detekce pohybu dají použít na statické kamer k identifikaci části videa, kde dochází k pohybu.</span><span class="sxs-lookup"><span data-stu-id="96525-106">Motion detection can be used on static camera footage to identify sections of the video where motion occurs.</span></span> <span data-ttu-id="96525-107">Vygeneruje soubor JSON obsahující metadata s časová razítka a ohraničující oblasti, kde došlo k události.</span><span class="sxs-lookup"><span data-stu-id="96525-107">It generates a JSON file containing a metadata with timestamps and the bounding region where the event occurred.</span></span>

<span data-ttu-id="96525-108">Cílem směrem zabezpečení video informační kanály, tato technologie je možné zařadit do kategorií pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny.</span><span class="sxs-lookup"><span data-stu-id="96525-108">Targeted towards security video feeds, this technology is able to categorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="96525-109">To umožňuje generovat výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, při schopnost extrahovat z extrémně dlouhé sledováním videa situacích, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="96525-109">This allows you to generate security alerts from camera feeds without being spammed with endless irrelevant events, while being able to extract moments of interest from extremely long surveillance videos.</span></span>

<span data-ttu-id="96525-110">**Detektor pohybu médií Azure** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="96525-110">The **Azure Media Motion Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="96525-111">Toto téma uvádí podrobnosti o **detektor pohybu médií Azure** a ukazuje, jak pomocí sady Media Services SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="96525-111">This topic gives details about  **Azure Media Motion Detector** and shows how to use it with Media Services SDK for .NET</span></span>

## <a name="motion-detector-input-files"></a><span data-ttu-id="96525-112">Vstupní soubory detekce pohybu</span><span class="sxs-lookup"><span data-stu-id="96525-112">Motion Detector input files</span></span>
<span data-ttu-id="96525-113">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="96525-113">Video files.</span></span> <span data-ttu-id="96525-114">V současné době jsou podporovány následující formáty: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="96525-114">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration-preset"></a><span data-ttu-id="96525-115">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="96525-115">Task configuration (preset)</span></span>
<span data-ttu-id="96525-116">Při vytváření úlohy s **detektor pohybu médií Azure**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96525-116">When creating a task with **Azure Media Motion Detector**, you must specify a configuration preset.</span></span> 

### <a name="parameters"></a><span data-ttu-id="96525-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="96525-117">Parameters</span></span>
<span data-ttu-id="96525-118">Můžete použít následující parametry:</span><span class="sxs-lookup"><span data-stu-id="96525-118">You can use the following parameters:</span></span>

| <span data-ttu-id="96525-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="96525-119">Name</span></span> | <span data-ttu-id="96525-120">Možnosti</span><span class="sxs-lookup"><span data-stu-id="96525-120">Options</span></span> | <span data-ttu-id="96525-121">Popis</span><span class="sxs-lookup"><span data-stu-id="96525-121">Description</span></span> | <span data-ttu-id="96525-122">Výchozí</span><span class="sxs-lookup"><span data-stu-id="96525-122">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="96525-123">sensitivityLevel</span><span class="sxs-lookup"><span data-stu-id="96525-123">sensitivityLevel</span></span> |<span data-ttu-id="96525-124">Řetězce: "nízká", 'střední', 'vysoká.</span><span class="sxs-lookup"><span data-stu-id="96525-124">String:'low', 'medium', 'high'</span></span> |<span data-ttu-id="96525-125">Nastaví úroveň citlivosti, které pohyby se použije v hlášení.</span><span class="sxs-lookup"><span data-stu-id="96525-125">Sets the sensitivity level at which motions is reported.</span></span> <span data-ttu-id="96525-126">Upravte tak, aby upravit množství falešně pozitivních zjištění.</span><span class="sxs-lookup"><span data-stu-id="96525-126">Adjust this to adjust amount of false positives.</span></span> |<span data-ttu-id="96525-127">"střední"</span><span class="sxs-lookup"><span data-stu-id="96525-127">'medium'</span></span> |
| <span data-ttu-id="96525-128">frameSamplingValue</span><span class="sxs-lookup"><span data-stu-id="96525-128">frameSamplingValue</span></span> |<span data-ttu-id="96525-129">Kladné celé číslo</span><span class="sxs-lookup"><span data-stu-id="96525-129">Positive integer</span></span> |<span data-ttu-id="96525-130">Nastaví frekvenci, na kterých běží algoritmus.</span><span class="sxs-lookup"><span data-stu-id="96525-130">Sets the frequency at which algorithm runs.</span></span> <span data-ttu-id="96525-131">každý snímek se rovná 1, 2 znamená každé 2 rámce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="96525-131">1 equals every frame, 2 means every 2nd frame, and so on.</span></span> |<span data-ttu-id="96525-132">1</span><span class="sxs-lookup"><span data-stu-id="96525-132">1</span></span> |
| <span data-ttu-id="96525-133">detectLightChange</span><span class="sxs-lookup"><span data-stu-id="96525-133">detectLightChange</span></span> |<span data-ttu-id="96525-134">Logická hodnota: "PRAVDA", "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="96525-134">Boolean:'true', 'false'</span></span> |<span data-ttu-id="96525-135">Nastaví, zda se ve výsledcích hlásí světla změny</span><span class="sxs-lookup"><span data-stu-id="96525-135">Sets whether light changes are reported in the results</span></span> |<span data-ttu-id="96525-136">"Nepravda"</span><span class="sxs-lookup"><span data-stu-id="96525-136">'False'</span></span> |
| <span data-ttu-id="96525-137">mergeTimeThreshold</span><span class="sxs-lookup"><span data-stu-id="96525-137">mergeTimeThreshold</span></span> |<span data-ttu-id="96525-138">Čas xs: Hh: mm:</span><span class="sxs-lookup"><span data-stu-id="96525-138">Xs-time: Hh:mm:ss</span></span><br/><span data-ttu-id="96525-139">Příklad: 00:00:03</span><span class="sxs-lookup"><span data-stu-id="96525-139">Example: 00:00:03</span></span> |<span data-ttu-id="96525-140">Určuje časový interval mezi pohybu událostí, kde bude 2 události kombinaci a nahlášena jako 1.</span><span class="sxs-lookup"><span data-stu-id="96525-140">Specifies the time window between motion events where 2 events will be combined and reported as 1.</span></span> |<span data-ttu-id="96525-141">00:00:00</span><span class="sxs-lookup"><span data-stu-id="96525-141">00:00:00</span></span> |
| <span data-ttu-id="96525-142">detectionZones</span><span class="sxs-lookup"><span data-stu-id="96525-142">detectionZones</span></span> |<span data-ttu-id="96525-143">Pole detekce zóny:</span><span class="sxs-lookup"><span data-stu-id="96525-143">An array of detection zones:</span></span><br/><span data-ttu-id="96525-144">-Detekce zóny je pole 3 nebo více bodů</span><span class="sxs-lookup"><span data-stu-id="96525-144">- Detection Zone is an array of 3 or more points</span></span><br/><span data-ttu-id="96525-145">– Bod je x a y souřadnic od 0 do 1.</span><span class="sxs-lookup"><span data-stu-id="96525-145">- Point is a x and y coordinate from 0 to 1.</span></span> |<span data-ttu-id="96525-146">Popisuje seznamu detekce polygonálních zón, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="96525-146">Describes the list of polygonal detection zones to be used.</span></span><br/><span data-ttu-id="96525-147">Výsledky se ohlásí s zóny jako ID, se první z nich probíhá 'id': 0</span><span class="sxs-lookup"><span data-stu-id="96525-147">Results will be reported with the zones as an ID, with the first one being 'id':0</span></span> |<span data-ttu-id="96525-148">Jedné oblasti, které zahrnuje celou rámečku.</span><span class="sxs-lookup"><span data-stu-id="96525-148">Single zone which covers the entire frame.</span></span> |

### <a name="json-example"></a><span data-ttu-id="96525-149">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="96525-149">JSON example</span></span>
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a><span data-ttu-id="96525-150">Pohybu detektor výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="96525-150">Motion Detector output files</span></span>
<span data-ttu-id="96525-151">Úlohu detekce pohybu vrátí soubor JSON v výstupní asset, která popisuje výstrahy pohybu a jejich kategorie, v rámci videa.</span><span class="sxs-lookup"><span data-stu-id="96525-151">A motion detection job will return a JSON file in the output asset which describes the motion alerts, and their categories, within the video.</span></span> <span data-ttu-id="96525-152">Soubor bude obsahovat informace o čas a dobu trvání pohybu zjistil ve videu.</span><span class="sxs-lookup"><span data-stu-id="96525-152">The file will contain information about the time and duration of motion detected in the video.</span></span>

<span data-ttu-id="96525-153">Rozhraní API detektor pohybu poskytuje indikátory po nejsou objekty v pohybu v pevné pozadí videa (například sledováním video).</span><span class="sxs-lookup"><span data-stu-id="96525-153">The Motion Detector API provides indicators once there are objects in motion in a fixed background video (e.g. a surveillance video).</span></span> <span data-ttu-id="96525-154">Detektor pohybu je vycvičena ke snížení falešné výstrahy, jako je například osvětlení a stínové změny.</span><span class="sxs-lookup"><span data-stu-id="96525-154">The Motion Detector is trained to reduce false alarms, such as lighting and shadow changes.</span></span> <span data-ttu-id="96525-155">Aktuální omezení algoritmů zahrnují noci vize videa, poloprůhledné objekty a malé objekty.</span><span class="sxs-lookup"><span data-stu-id="96525-155">Current limitations of the algorithms include night vision videos, semi-transparent objects, and small objects.</span></span>

### <span data-ttu-id="96525-156"><a id="output_elements"></a>Elementy výstupního souboru JSON</span><span class="sxs-lookup"><span data-stu-id="96525-156"><a id="output_elements"></a>Elements of the output JSON file</span></span>
> [!NOTE]
> <span data-ttu-id="96525-157">Nejnovější verze formátu JSON výstupu se změnil a pro někteří zákazníci mohou představovat narušující změně.</span><span class="sxs-lookup"><span data-stu-id="96525-157">In the latest release, the Output JSON format has changed and may represent a breaking change for some customers.</span></span>
> 
> 

<span data-ttu-id="96525-158">Následující tabulka popisuje elementy výstupního souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="96525-158">The following table describes elements of the output JSON file.</span></span>

| <span data-ttu-id="96525-159">Element</span><span class="sxs-lookup"><span data-stu-id="96525-159">Element</span></span> | <span data-ttu-id="96525-160">Popis</span><span class="sxs-lookup"><span data-stu-id="96525-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="96525-161">Verze</span><span class="sxs-lookup"><span data-stu-id="96525-161">Version</span></span> |<span data-ttu-id="96525-162">Vztahuje se na verzi rozhraní API Video.</span><span class="sxs-lookup"><span data-stu-id="96525-162">This refers to the version of the Video API.</span></span> <span data-ttu-id="96525-163">Aktuální verze je 2.</span><span class="sxs-lookup"><span data-stu-id="96525-163">The current version is 2.</span></span> |
| <span data-ttu-id="96525-164">Časová osa</span><span class="sxs-lookup"><span data-stu-id="96525-164">Timescale</span></span> |<span data-ttu-id="96525-165">"Rysky" za sekundu videa.</span><span class="sxs-lookup"><span data-stu-id="96525-165">"Ticks" per second of the video.</span></span> |
| <span data-ttu-id="96525-166">Posun</span><span class="sxs-lookup"><span data-stu-id="96525-166">Offset</span></span> |<span data-ttu-id="96525-167">Časový posun pro časová razítka v "rysky".</span><span class="sxs-lookup"><span data-stu-id="96525-167">The time offset for timestamps in "ticks".</span></span> <span data-ttu-id="96525-168">Ve verzi 1.0 rozhraní API, Video bude vždy 0.</span><span class="sxs-lookup"><span data-stu-id="96525-168">In version 1.0 of Video APIs, this will always be 0.</span></span> <span data-ttu-id="96525-169">V budoucích scénáře, které podporujeme, tato hodnota může změnit.</span><span class="sxs-lookup"><span data-stu-id="96525-169">In future scenarios we support, this value may change.</span></span> |
| <span data-ttu-id="96525-170">kmitočet snímků</span><span class="sxs-lookup"><span data-stu-id="96525-170">Framerate</span></span> |<span data-ttu-id="96525-171">Počet snímků za sekundu videa.</span><span class="sxs-lookup"><span data-stu-id="96525-171">Frames per second of the video.</span></span> |
| <span data-ttu-id="96525-172">Šířka, Výška</span><span class="sxs-lookup"><span data-stu-id="96525-172">Width, Height</span></span> |<span data-ttu-id="96525-173">Odkazuje na šířku a výšku videa v pixelech.</span><span class="sxs-lookup"><span data-stu-id="96525-173">Refers to the width and height of the video in pixels.</span></span> |
| <span data-ttu-id="96525-174">Start</span><span class="sxs-lookup"><span data-stu-id="96525-174">Start</span></span> |<span data-ttu-id="96525-175">Razítka start "rysky".</span><span class="sxs-lookup"><span data-stu-id="96525-175">The start timestamp in "ticks".</span></span> |
| <span data-ttu-id="96525-176">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="96525-176">Duration</span></span> |<span data-ttu-id="96525-177">Délka události v "rysky".</span><span class="sxs-lookup"><span data-stu-id="96525-177">The length of the event, in "ticks".</span></span> |
| <span data-ttu-id="96525-178">Interval</span><span class="sxs-lookup"><span data-stu-id="96525-178">Interval</span></span> |<span data-ttu-id="96525-179">Interval každou položku v události v "rysky".</span><span class="sxs-lookup"><span data-stu-id="96525-179">The interval of each entry in the event, in "ticks".</span></span> |
| <span data-ttu-id="96525-180">Události</span><span class="sxs-lookup"><span data-stu-id="96525-180">Events</span></span> |<span data-ttu-id="96525-181">Každý fragment událostí obsahuje pohybu během této doby trvání.</span><span class="sxs-lookup"><span data-stu-id="96525-181">Each event fragment contains the motion detected within that time duration.</span></span> |
| <span data-ttu-id="96525-182">Typ</span><span class="sxs-lookup"><span data-stu-id="96525-182">Type</span></span> |<span data-ttu-id="96525-183">V aktuální verzi je to vždy (2) pro obecné pohybu.</span><span class="sxs-lookup"><span data-stu-id="96525-183">In the current version, this is always ‘2’ for generic motion.</span></span> <span data-ttu-id="96525-184">Tento popisek poskytuje rozhraní API Video flexibilitu zařadit do kategorií pohybu v budoucích verzích.</span><span class="sxs-lookup"><span data-stu-id="96525-184">This label gives Video APIs the flexibility to categorize motion in future versions.</span></span> |
| <span data-ttu-id="96525-185">RegionID</span><span class="sxs-lookup"><span data-stu-id="96525-185">RegionID</span></span> |<span data-ttu-id="96525-186">Jak je popsáno výše, bude vždy 0 v této verzi.</span><span class="sxs-lookup"><span data-stu-id="96525-186">As explained above, this will always be 0 in this version.</span></span> <span data-ttu-id="96525-187">Tento popisek poskytuje rozhraní API Video flexibilitu najít pohybu v různých oblastech v budoucích verzích.</span><span class="sxs-lookup"><span data-stu-id="96525-187">This label gives Video API the flexibility to find motion in various regions in future versions.</span></span> |
| <span data-ttu-id="96525-188">Oblasti</span><span class="sxs-lookup"><span data-stu-id="96525-188">Regions</span></span> |<span data-ttu-id="96525-189">Odkazuje na oblasti v videa, kde se zajímáte o pohybu.</span><span class="sxs-lookup"><span data-stu-id="96525-189">Refers to the area in your video where you care about motion.</span></span> <br/><br/><span data-ttu-id="96525-190">-"id" představuje oblasti oblast – v této verzi je jen jeden, ID 0.</span><span class="sxs-lookup"><span data-stu-id="96525-190">-"id" represents the region area – in this version there is only one, ID 0.</span></span> <br/><span data-ttu-id="96525-191">-"typ" představuje obrazec oblasti, která se zajímáte o pro pohybu.</span><span class="sxs-lookup"><span data-stu-id="96525-191">-"type" represents the shape of the region you care about for motion.</span></span> <span data-ttu-id="96525-192">V současné době jsou podporovány "Obdélník" a "mnohoúhelníku".</span><span class="sxs-lookup"><span data-stu-id="96525-192">Currently, "rectangle" and "polygon" are supported.</span></span><br/> <span data-ttu-id="96525-193">Pokud jste zadali "Obdélník", oblast má dimenzí v X, Y, šířka a výška.</span><span class="sxs-lookup"><span data-stu-id="96525-193">If you specified "rectangle", the region has dimensions in X, Y, Width, and Height.</span></span> <span data-ttu-id="96525-194">Souřadnice X a Y představují levém horním XY souřadnice oblasti v normalizovaný škále od 0,0 do 1,0.</span><span class="sxs-lookup"><span data-stu-id="96525-194">The X and Y coordinates represent the upper left hand XY coordinates of the region in a normalized scale of 0.0 to 1.0.</span></span> <span data-ttu-id="96525-195">Šířka a výška představují velikost oblasti v normalizovaný škále od 0,0 do 1,0.</span><span class="sxs-lookup"><span data-stu-id="96525-195">The width and height represent the size of the region in a normalized scale of 0.0 to 1.0.</span></span> <span data-ttu-id="96525-196">V aktuální verzi X, Y, šířka a výška vždy stanoví na 0, 0 a 1, 1.</span><span class="sxs-lookup"><span data-stu-id="96525-196">In the current version, X, Y, Width, and Height are always fixed at 0, 0 and 1, 1.</span></span> <br/><span data-ttu-id="96525-197">Pokud jste zadali "mnohoúhelníku", má oblast dimenzí v bodech.</span><span class="sxs-lookup"><span data-stu-id="96525-197">If you specified "polygon", the region has dimensions in points.</span></span> <br/> |
| <span data-ttu-id="96525-198">fragmenty</span><span class="sxs-lookup"><span data-stu-id="96525-198">Fragments</span></span> |<span data-ttu-id="96525-199">Metadata se blokové až do různých segmentů názvem fragmenty.</span><span class="sxs-lookup"><span data-stu-id="96525-199">The metadata is chunked up into different segments called fragments.</span></span> <span data-ttu-id="96525-200">Každý fragment obsahuje počáteční, doba trvání, číslo intervalu a událostí.</span><span class="sxs-lookup"><span data-stu-id="96525-200">Each fragment contains a start, duration, interval number, and event(s).</span></span> <span data-ttu-id="96525-201">Fragment s žádné události znamená, že žádné pohybu byl nalezen během této počáteční čas a dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="96525-201">A fragment with no events means that no motion was detected during that start time and duration.</span></span> |
| <span data-ttu-id="96525-202">Hranaté závorky]</span><span class="sxs-lookup"><span data-stu-id="96525-202">Brackets []</span></span> |<span data-ttu-id="96525-203">Každá závorka představuje jeden interval v události.</span><span class="sxs-lookup"><span data-stu-id="96525-203">Each bracket represents one interval in the event.</span></span> <span data-ttu-id="96525-204">Byla zjištěna prázdný závorky pro tento interval znamená, že žádné pohybu.</span><span class="sxs-lookup"><span data-stu-id="96525-204">Empty brackets for that interval means that no motion was detected.</span></span> |
| <span data-ttu-id="96525-205">Umístění</span><span class="sxs-lookup"><span data-stu-id="96525-205">locations</span></span> |<span data-ttu-id="96525-206">Tento nový záznam v části události uvádí umístění, kde došlo k chybě provozu.</span><span class="sxs-lookup"><span data-stu-id="96525-206">This new entry under events lists the location where the motion occurred.</span></span> <span data-ttu-id="96525-207">Toto je konkrétnější než detekce zóny.</span><span class="sxs-lookup"><span data-stu-id="96525-207">This is more specific than the detection zones.</span></span> |

<span data-ttu-id="96525-208">Toto je příklad výstupu JSON</span><span class="sxs-lookup"><span data-stu-id="96525-208">The following is a JSON output example</span></span>

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a><span data-ttu-id="96525-209">Omezení</span><span class="sxs-lookup"><span data-stu-id="96525-209">Limitations</span></span>
* <span data-ttu-id="96525-210">Podporované formáty vstupní video zahrnují MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="96525-210">The supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="96525-211">Detekce pohybu je optimalizovaná pro stojící pozadí videa.</span><span class="sxs-lookup"><span data-stu-id="96525-211">Motion Detection is optimized for stationary background videos.</span></span> <span data-ttu-id="96525-212">Algoritmus se zaměřuje na snížení falešné výstrahy, jako je například změny osvětlení a stínů.</span><span class="sxs-lookup"><span data-stu-id="96525-212">The algorithm focuses on reducing false alarms, such as lighting changes, and shadows.</span></span>
* <span data-ttu-id="96525-213">Některé pohybu nemusí být detekována z důvodu technické problémy; například noci vize videa, poloprůhledné objekty a malé objekty.</span><span class="sxs-lookup"><span data-stu-id="96525-213">Some motion may not be detected due to technical challenges; e.g. night vision videos, semi-transparent objects, and small objects.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="96525-214">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="96525-214">.NET sample code</span></span>

<span data-ttu-id="96525-215">Program zobrazí následující postup:</span><span class="sxs-lookup"><span data-stu-id="96525-215">The following program shows how to:</span></span>

1. <span data-ttu-id="96525-216">Vytvořte asset a nahrajte soubor média do assetu.</span><span class="sxs-lookup"><span data-stu-id="96525-216">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="96525-217">Vytvořte úlohu s úkolem detekce pohybu na videu podle konfigurační soubor, který obsahuje následující přednastavení json.</span><span class="sxs-lookup"><span data-stu-id="96525-217">Create a job with a video motion detection task based on a configuration file that contains the following json preset.</span></span> 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
3. <span data-ttu-id="96525-218">Stáhněte soubory JSON výstupu.</span><span class="sxs-lookup"><span data-stu-id="96525-218">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="96525-219">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="96525-219">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="96525-220">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="96525-220">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="96525-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="96525-221">Example</span></span>


    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoMotionDetection
    {
        class Program
        {
            // Read values from the App.config file.
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

                // Run the VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference to Azure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="96525-222">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="96525-222">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="96525-223">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="96525-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="96525-224">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="96525-224">Related links</span></span>
[<span data-ttu-id="96525-225">Blog Azure detektor pohybu Media Services</span><span class="sxs-lookup"><span data-stu-id="96525-225">Azure Media Services Motion Detector blog</span></span>](https://azure.microsoft.com/blog/motion-detector-update/)

[<span data-ttu-id="96525-226">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="96525-226">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="96525-227">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="96525-227">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

