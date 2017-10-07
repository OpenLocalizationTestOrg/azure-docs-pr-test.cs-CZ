---
title: aaaDetect pohyby s Azure Media Analytics | Microsoft Docs
description: "Hello detektor pohybu médií Azure media procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video."
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
ms.openlocfilehash: cb431375c92222053ed2239dd4e45767524dab68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a><span data-ttu-id="b204c-103">Zjištění pohyby s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="b204c-103">Detect Motions with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="b204c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b204c-104">Overview</span></span>
<span data-ttu-id="b204c-105">Hello **detektor pohybu médií Azure** média procesoru (PP) umožňuje tooefficiently můžete identifikovat části týkající se v rámci souboru jinak dlouhé a bezproblémové video.</span><span class="sxs-lookup"><span data-stu-id="b204c-105">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="b204c-106">Detekce pohybu lze použít na statické fotoaparát záznamů tooidentify části hello videa kde dojde k pohybu.</span><span class="sxs-lookup"><span data-stu-id="b204c-106">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="b204c-107">Vygeneruje soubor JSON obsahující metadata s časová razítka a hello ohraničujícího oblasti, kde došlo k události hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-107">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="b204c-108">Cílem směrem zabezpečení video informační kanály, tato technologie je možné toocategorize pohybu do příslušné události a falešně pozitivních například stínů a osvětlení změny.</span><span class="sxs-lookup"><span data-stu-id="b204c-108">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="b204c-109">To umožňuje toogenerate výstrahy zabezpečení z fotoaparátu kanály bez nevyžádané pošty s nekonečná důležité události, aniž by byly situacích možné tooextract zájmu z extrémně dlouhé sledováním videa.</span><span class="sxs-lookup"><span data-stu-id="b204c-109">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

<span data-ttu-id="b204c-110">Hello **detektor pohybu médií Azure** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="b204c-110">hello **Azure Media Motion Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="b204c-111">Toto téma uvádí podrobnosti o **detektor pohybu médií Azure** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET</span><span class="sxs-lookup"><span data-stu-id="b204c-111">This topic gives details about  **Azure Media Motion Detector** and shows how toouse it with Media Services SDK for .NET</span></span>

## <a name="motion-detector-input-files"></a><span data-ttu-id="b204c-112">Vstupní soubory detekce pohybu</span><span class="sxs-lookup"><span data-stu-id="b204c-112">Motion Detector input files</span></span>
<span data-ttu-id="b204c-113">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="b204c-113">Video files.</span></span> <span data-ttu-id="b204c-114">V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="b204c-114">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration-preset"></a><span data-ttu-id="b204c-115">Konfigurace úlohy (přednastavených)</span><span class="sxs-lookup"><span data-stu-id="b204c-115">Task configuration (preset)</span></span>
<span data-ttu-id="b204c-116">Při vytváření úlohy s **detektor pohybu médií Azure**, je nutné zadat jedno z přednastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b204c-116">When creating a task with **Azure Media Motion Detector**, you must specify a configuration preset.</span></span> 

### <a name="parameters"></a><span data-ttu-id="b204c-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="b204c-117">Parameters</span></span>
<span data-ttu-id="b204c-118">Můžete použít hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="b204c-118">You can use hello following parameters:</span></span>

| <span data-ttu-id="b204c-119">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="b204c-119">Name</span></span> | <span data-ttu-id="b204c-120">Možnosti</span><span class="sxs-lookup"><span data-stu-id="b204c-120">Options</span></span> | <span data-ttu-id="b204c-121">Popis</span><span class="sxs-lookup"><span data-stu-id="b204c-121">Description</span></span> | <span data-ttu-id="b204c-122">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b204c-122">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b204c-123">sensitivityLevel</span><span class="sxs-lookup"><span data-stu-id="b204c-123">sensitivityLevel</span></span> |<span data-ttu-id="b204c-124">Řetězce: "nízká", 'střední', 'vysoká.</span><span class="sxs-lookup"><span data-stu-id="b204c-124">String:'low', 'medium', 'high'</span></span> |<span data-ttu-id="b204c-125">Nastaví úroveň hello velkých a malých písmen na které pohyby se použije v hlášení.</span><span class="sxs-lookup"><span data-stu-id="b204c-125">Sets hello sensitivity level at which motions is reported.</span></span> <span data-ttu-id="b204c-126">Upravte tuto tooadjust množství falešně pozitivních zjištění.</span><span class="sxs-lookup"><span data-stu-id="b204c-126">Adjust this tooadjust amount of false positives.</span></span> |<span data-ttu-id="b204c-127">"střední"</span><span class="sxs-lookup"><span data-stu-id="b204c-127">'medium'</span></span> |
| <span data-ttu-id="b204c-128">frameSamplingValue</span><span class="sxs-lookup"><span data-stu-id="b204c-128">frameSamplingValue</span></span> |<span data-ttu-id="b204c-129">Kladné celé číslo</span><span class="sxs-lookup"><span data-stu-id="b204c-129">Positive integer</span></span> |<span data-ttu-id="b204c-130">Nastaví frekvenci hello na kterých běží algoritmus.</span><span class="sxs-lookup"><span data-stu-id="b204c-130">Sets hello frequency at which algorithm runs.</span></span> <span data-ttu-id="b204c-131">každý snímek se rovná 1, 2 znamená každé 2 rámce a tak dále.</span><span class="sxs-lookup"><span data-stu-id="b204c-131">1 equals every frame, 2 means every 2nd frame, and so on.</span></span> |<span data-ttu-id="b204c-132">1</span><span class="sxs-lookup"><span data-stu-id="b204c-132">1</span></span> |
| <span data-ttu-id="b204c-133">detectLightChange</span><span class="sxs-lookup"><span data-stu-id="b204c-133">detectLightChange</span></span> |<span data-ttu-id="b204c-134">Logická hodnota: "PRAVDA", "Nepravda"</span><span class="sxs-lookup"><span data-stu-id="b204c-134">Boolean:'true', 'false'</span></span> |<span data-ttu-id="b204c-135">Nastaví, zda světla změny jsou hlášeny ve výsledcích hello</span><span class="sxs-lookup"><span data-stu-id="b204c-135">Sets whether light changes are reported in hello results</span></span> |<span data-ttu-id="b204c-136">"Nepravda"</span><span class="sxs-lookup"><span data-stu-id="b204c-136">'False'</span></span> |
| <span data-ttu-id="b204c-137">mergeTimeThreshold</span><span class="sxs-lookup"><span data-stu-id="b204c-137">mergeTimeThreshold</span></span> |<span data-ttu-id="b204c-138">Čas xs: Hh: mm:</span><span class="sxs-lookup"><span data-stu-id="b204c-138">Xs-time: Hh:mm:ss</span></span><br/><span data-ttu-id="b204c-139">Příklad: 00:00:03</span><span class="sxs-lookup"><span data-stu-id="b204c-139">Example: 00:00:03</span></span> |<span data-ttu-id="b204c-140">Určuje časový interval hello mezi pohybu událostí, kde bude 2 události kombinaci a nahlášena jako 1.</span><span class="sxs-lookup"><span data-stu-id="b204c-140">Specifies hello time window between motion events where 2 events will be combined and reported as 1.</span></span> |<span data-ttu-id="b204c-141">00:00:00</span><span class="sxs-lookup"><span data-stu-id="b204c-141">00:00:00</span></span> |
| <span data-ttu-id="b204c-142">detectionZones</span><span class="sxs-lookup"><span data-stu-id="b204c-142">detectionZones</span></span> |<span data-ttu-id="b204c-143">Pole detekce zóny:</span><span class="sxs-lookup"><span data-stu-id="b204c-143">An array of detection zones:</span></span><br/><span data-ttu-id="b204c-144">-Detekce zóny je pole 3 nebo více bodů</span><span class="sxs-lookup"><span data-stu-id="b204c-144">- Detection Zone is an array of 3 or more points</span></span><br/><span data-ttu-id="b204c-145">– Bod je x a y souřadnice z 0 too1.</span><span class="sxs-lookup"><span data-stu-id="b204c-145">- Point is a x and y coordinate from 0 too1.</span></span> |<span data-ttu-id="b204c-146">Popisuje hello seznam detekce polygonálních zón toobe použít.</span><span class="sxs-lookup"><span data-stu-id="b204c-146">Describes hello list of polygonal detection zones toobe used.</span></span><br/><span data-ttu-id="b204c-147">Výsledky se ohlásí hello zón jako ID, s hello první z nich vrácení 'id': 0</span><span class="sxs-lookup"><span data-stu-id="b204c-147">Results will be reported with hello zones as an ID, with hello first one being 'id':0</span></span> |<span data-ttu-id="b204c-148">Jedné oblasti, které zahrnuje celou rámce hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-148">Single zone which covers hello entire frame.</span></span> |

### <a name="json-example"></a><span data-ttu-id="b204c-149">Příklad JSON</span><span class="sxs-lookup"><span data-stu-id="b204c-149">JSON example</span></span>
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


## <a name="motion-detector-output-files"></a><span data-ttu-id="b204c-150">Pohybu detektor výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="b204c-150">Motion Detector output files</span></span>
<span data-ttu-id="b204c-151">Úlohu detekce pohybu vrátí soubor JSON v hello výstupní asset, který popisuje hello pohybu výstrahy a jejich kategorie, v rámci hello video.</span><span class="sxs-lookup"><span data-stu-id="b204c-151">A motion detection job will return a JSON file in hello output asset which describes hello motion alerts, and their categories, within hello video.</span></span> <span data-ttu-id="b204c-152">Hello soubor bude obsahovat informace o hello čas a dobu trvání pohybu v hello video zjištěn.</span><span class="sxs-lookup"><span data-stu-id="b204c-152">hello file will contain information about hello time and duration of motion detected in hello video.</span></span>

<span data-ttu-id="b204c-153">Hello pohybu detektor API poskytuje indikátory po nejsou objekty v pohybu v pevné pozadí videa (například sledováním video).</span><span class="sxs-lookup"><span data-stu-id="b204c-153">hello Motion Detector API provides indicators once there are objects in motion in a fixed background video (e.g. a surveillance video).</span></span> <span data-ttu-id="b204c-154">Hello detektor pohybu je vyškolení tooreduce falešné výstrahy, jako je například osvětlení a stínové změny.</span><span class="sxs-lookup"><span data-stu-id="b204c-154">hello Motion Detector is trained tooreduce false alarms, such as lighting and shadow changes.</span></span> <span data-ttu-id="b204c-155">Aktuální omezení algoritmů hello zahrnují noci vize videa, poloprůhledné objekty a malé objekty.</span><span class="sxs-lookup"><span data-stu-id="b204c-155">Current limitations of hello algorithms include night vision videos, semi-transparent objects, and small objects.</span></span>

### <span data-ttu-id="b204c-156"><a id="output_elements"></a>Elementy výstupního souboru JSON, hello</span><span class="sxs-lookup"><span data-stu-id="b204c-156"><a id="output_elements"></a>Elements of hello output JSON file</span></span>
> [!NOTE]
> <span data-ttu-id="b204c-157">V nejnovější verzi hello formátu JSON výstup hello se změnil a pro někteří zákazníci mohou představovat narušující změně.</span><span class="sxs-lookup"><span data-stu-id="b204c-157">In hello latest release, hello Output JSON format has changed and may represent a breaking change for some customers.</span></span>
> 
> 

<span data-ttu-id="b204c-158">Hello následující tabulka popisuje prvky souboru JSON výstup hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-158">hello following table describes elements of hello output JSON file.</span></span>

| <span data-ttu-id="b204c-159">Element</span><span class="sxs-lookup"><span data-stu-id="b204c-159">Element</span></span> | <span data-ttu-id="b204c-160">Popis</span><span class="sxs-lookup"><span data-stu-id="b204c-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b204c-161">Verze</span><span class="sxs-lookup"><span data-stu-id="b204c-161">Version</span></span> |<span data-ttu-id="b204c-162">Vztahuje se toohello verzi hello Video rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b204c-162">This refers toohello version of hello Video API.</span></span> <span data-ttu-id="b204c-163">aktuální verze Hello je 2.</span><span class="sxs-lookup"><span data-stu-id="b204c-163">hello current version is 2.</span></span> |
| <span data-ttu-id="b204c-164">Časová osa</span><span class="sxs-lookup"><span data-stu-id="b204c-164">Timescale</span></span> |<span data-ttu-id="b204c-165">"Rysky" za sekundu hello videa.</span><span class="sxs-lookup"><span data-stu-id="b204c-165">"Ticks" per second of hello video.</span></span> |
| <span data-ttu-id="b204c-166">Posun</span><span class="sxs-lookup"><span data-stu-id="b204c-166">Offset</span></span> |<span data-ttu-id="b204c-167">Posun Hello čas pro časová razítka v "rysky".</span><span class="sxs-lookup"><span data-stu-id="b204c-167">hello time offset for timestamps in "ticks".</span></span> <span data-ttu-id="b204c-168">Ve verzi 1.0 rozhraní API, Video bude vždy 0.</span><span class="sxs-lookup"><span data-stu-id="b204c-168">In version 1.0 of Video APIs, this will always be 0.</span></span> <span data-ttu-id="b204c-169">V budoucích scénáře, které podporujeme, tato hodnota může změnit.</span><span class="sxs-lookup"><span data-stu-id="b204c-169">In future scenarios we support, this value may change.</span></span> |
| <span data-ttu-id="b204c-170">kmitočet snímků</span><span class="sxs-lookup"><span data-stu-id="b204c-170">Framerate</span></span> |<span data-ttu-id="b204c-171">Počet snímků za sekundu hello videa.</span><span class="sxs-lookup"><span data-stu-id="b204c-171">Frames per second of hello video.</span></span> |
| <span data-ttu-id="b204c-172">Šířka, Výška</span><span class="sxs-lookup"><span data-stu-id="b204c-172">Width, Height</span></span> |<span data-ttu-id="b204c-173">Odkazuje toohello šířka a výška hello videa v pixelech.</span><span class="sxs-lookup"><span data-stu-id="b204c-173">Refers toohello width and height of hello video in pixels.</span></span> |
| <span data-ttu-id="b204c-174">Start</span><span class="sxs-lookup"><span data-stu-id="b204c-174">Start</span></span> |<span data-ttu-id="b204c-175">Hello spustit časové razítko v "rysky".</span><span class="sxs-lookup"><span data-stu-id="b204c-175">hello start timestamp in "ticks".</span></span> |
| <span data-ttu-id="b204c-176">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="b204c-176">Duration</span></span> |<span data-ttu-id="b204c-177">Délka Hello hello událost v "rysky".</span><span class="sxs-lookup"><span data-stu-id="b204c-177">hello length of hello event, in "ticks".</span></span> |
| <span data-ttu-id="b204c-178">Interval</span><span class="sxs-lookup"><span data-stu-id="b204c-178">Interval</span></span> |<span data-ttu-id="b204c-179">interval Hello každou položku v hello událost v "rysky".</span><span class="sxs-lookup"><span data-stu-id="b204c-179">hello interval of each entry in hello event, in "ticks".</span></span> |
| <span data-ttu-id="b204c-180">Události</span><span class="sxs-lookup"><span data-stu-id="b204c-180">Events</span></span> |<span data-ttu-id="b204c-181">Každý fragment událostí obsahuje hello pohybu během této doby trvání.</span><span class="sxs-lookup"><span data-stu-id="b204c-181">Each event fragment contains hello motion detected within that time duration.</span></span> |
| <span data-ttu-id="b204c-182">Typ</span><span class="sxs-lookup"><span data-stu-id="b204c-182">Type</span></span> |<span data-ttu-id="b204c-183">V aktuální verzi hello je to vždy (2) pro obecné pohybu.</span><span class="sxs-lookup"><span data-stu-id="b204c-183">In hello current version, this is always ‘2’ for generic motion.</span></span> <span data-ttu-id="b204c-184">Tento popisek poskytuje rozhraní API Video hello flexibilitu toocategorize pohybu v budoucích verzích.</span><span class="sxs-lookup"><span data-stu-id="b204c-184">This label gives Video APIs hello flexibility toocategorize motion in future versions.</span></span> |
| <span data-ttu-id="b204c-185">RegionID</span><span class="sxs-lookup"><span data-stu-id="b204c-185">RegionID</span></span> |<span data-ttu-id="b204c-186">Jak je popsáno výše, bude vždy 0 v této verzi.</span><span class="sxs-lookup"><span data-stu-id="b204c-186">As explained above, this will always be 0 in this version.</span></span> <span data-ttu-id="b204c-187">Tento popisek poskytuje rozhraní API Video hello flexibilitu toofind pohybu v různých oblastech v budoucích verzích.</span><span class="sxs-lookup"><span data-stu-id="b204c-187">This label gives Video API hello flexibility toofind motion in various regions in future versions.</span></span> |
| <span data-ttu-id="b204c-188">Oblasti</span><span class="sxs-lookup"><span data-stu-id="b204c-188">Regions</span></span> |<span data-ttu-id="b204c-189">Odkazuje toohello oblasti videa, kde se zajímáte o pohybu.</span><span class="sxs-lookup"><span data-stu-id="b204c-189">Refers toohello area in your video where you care about motion.</span></span> <br/><br/><span data-ttu-id="b204c-190">-"id" představuje hello oblast oblasti – v této verzi je jen jeden, ID 0.</span><span class="sxs-lookup"><span data-stu-id="b204c-190">-"id" represents hello region area – in this version there is only one, ID 0.</span></span> <br/><span data-ttu-id="b204c-191">-"typ" představuje hello tvar hello oblast, která se zajímáte o pro pohybu.</span><span class="sxs-lookup"><span data-stu-id="b204c-191">-"type" represents hello shape of hello region you care about for motion.</span></span> <span data-ttu-id="b204c-192">V současné době jsou podporovány "Obdélník" a "mnohoúhelníku".</span><span class="sxs-lookup"><span data-stu-id="b204c-192">Currently, "rectangle" and "polygon" are supported.</span></span><br/> <span data-ttu-id="b204c-193">Pokud jste zadali "Obdélník", hello oblast má dimenzí v X, Y, šířka a výška.</span><span class="sxs-lookup"><span data-stu-id="b204c-193">If you specified "rectangle", hello region has dimensions in X, Y, Width, and Height.</span></span> <span data-ttu-id="b204c-194">Hello X a Y souřadnice představují hello levém horním XY souřadnice oblasti hello v normalizovaný měřítkem 0.0 too1.0.</span><span class="sxs-lookup"><span data-stu-id="b204c-194">hello X and Y coordinates represent hello upper left hand XY coordinates of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="b204c-195">Hello šířky a výšky představují hello velikost hello oblast v normalizovaný měřítkem 0.0 too1.0.</span><span class="sxs-lookup"><span data-stu-id="b204c-195">hello width and height represent hello size of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="b204c-196">V aktuální verzi hello X, Y, šířka a výška vždy stanoví na 0, 0 a 1, 1.</span><span class="sxs-lookup"><span data-stu-id="b204c-196">In hello current version, X, Y, Width, and Height are always fixed at 0, 0 and 1, 1.</span></span> <br/><span data-ttu-id="b204c-197">Pokud jste zadali "mnohoúhelníku", má oblast hello dimenzí v bodech.</span><span class="sxs-lookup"><span data-stu-id="b204c-197">If you specified "polygon", hello region has dimensions in points.</span></span> <br/> |
| <span data-ttu-id="b204c-198">fragmenty</span><span class="sxs-lookup"><span data-stu-id="b204c-198">Fragments</span></span> |<span data-ttu-id="b204c-199">Hello metadata je blokové až do různých segmentů názvem fragmenty.</span><span class="sxs-lookup"><span data-stu-id="b204c-199">hello metadata is chunked up into different segments called fragments.</span></span> <span data-ttu-id="b204c-200">Každý fragment obsahuje počáteční, doba trvání, číslo intervalu a událostí.</span><span class="sxs-lookup"><span data-stu-id="b204c-200">Each fragment contains a start, duration, interval number, and event(s).</span></span> <span data-ttu-id="b204c-201">Fragment s žádné události znamená, že žádné pohybu byl nalezen během této počáteční čas a dobu trvání.</span><span class="sxs-lookup"><span data-stu-id="b204c-201">A fragment with no events means that no motion was detected during that start time and duration.</span></span> |
| <span data-ttu-id="b204c-202">Hranaté závorky]</span><span class="sxs-lookup"><span data-stu-id="b204c-202">Brackets []</span></span> |<span data-ttu-id="b204c-203">Každá závorka představuje jeden interval hello události.</span><span class="sxs-lookup"><span data-stu-id="b204c-203">Each bracket represents one interval in hello event.</span></span> <span data-ttu-id="b204c-204">Byla zjištěna prázdný závorky pro tento interval znamená, že žádné pohybu.</span><span class="sxs-lookup"><span data-stu-id="b204c-204">Empty brackets for that interval means that no motion was detected.</span></span> |
| <span data-ttu-id="b204c-205">Umístění</span><span class="sxs-lookup"><span data-stu-id="b204c-205">locations</span></span> |<span data-ttu-id="b204c-206">Tento nový záznam v části události uvádí hello umístění, kde došlo k pohybu hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-206">This new entry under events lists hello location where hello motion occurred.</span></span> <span data-ttu-id="b204c-207">Toto je konkrétnější než hello detekce zóny.</span><span class="sxs-lookup"><span data-stu-id="b204c-207">This is more specific than hello detection zones.</span></span> |

<span data-ttu-id="b204c-208">Následující Hello je příklad výstupu JSON</span><span class="sxs-lookup"><span data-stu-id="b204c-208">hello following is a JSON output example</span></span>

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
## <a name="limitations"></a><span data-ttu-id="b204c-209">Omezení</span><span class="sxs-lookup"><span data-stu-id="b204c-209">Limitations</span></span>
* <span data-ttu-id="b204c-210">vstupní video formáty Hello podporována jsou MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="b204c-210">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="b204c-211">Detekce pohybu je optimalizovaná pro stojící pozadí videa.</span><span class="sxs-lookup"><span data-stu-id="b204c-211">Motion Detection is optimized for stationary background videos.</span></span> <span data-ttu-id="b204c-212">algoritmus Hello se zaměřuje na snížení falešné výstrahy, jako je například změny osvětlení a stínů.</span><span class="sxs-lookup"><span data-stu-id="b204c-212">hello algorithm focuses on reducing false alarms, such as lighting changes, and shadows.</span></span>
* <span data-ttu-id="b204c-213">Některé pohybu nemusí být detekována z důvodu problémů tootechnical; například noci vize videa, poloprůhledné objekty a malé objekty.</span><span class="sxs-lookup"><span data-stu-id="b204c-213">Some motion may not be detected due tootechnical challenges; e.g. night vision videos, semi-transparent objects, and small objects.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="b204c-214">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="b204c-214">.NET sample code</span></span>

<span data-ttu-id="b204c-215">ukazuje programu Hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="b204c-215">hello following program shows how to:</span></span>

1. <span data-ttu-id="b204c-216">Vytvořte asset a nahrajte soubor média do hello asset.</span><span class="sxs-lookup"><span data-stu-id="b204c-216">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="b204c-217">Vytvořte úlohu s úkolem detekce pohybu na videu podle konfigurační soubor, který obsahuje následující json přednastavených hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-217">Create a job with a video motion detection task based on a configuration file that contains hello following json preset.</span></span> 
   
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
3. <span data-ttu-id="b204c-218">Stáhněte soubory JSON výstup hello.</span><span class="sxs-lookup"><span data-stu-id="b204c-218">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="b204c-219">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="b204c-219">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="b204c-220">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="b204c-220">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="b204c-221">Příklad</span><span class="sxs-lookup"><span data-stu-id="b204c-221">Example</span></span>


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
            // Read values from hello App.config file.
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

                // Run hello VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference tooAzure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="b204c-222">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="b204c-222">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b204c-223">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="b204c-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="b204c-224">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="b204c-224">Related links</span></span>
[<span data-ttu-id="b204c-225">Blog Azure detektor pohybu Media Services</span><span class="sxs-lookup"><span data-stu-id="b204c-225">Azure Media Services Motion Detector blog</span></span>](https://azure.microsoft.com/blog/motion-detector-update/)

[<span data-ttu-id="b204c-226">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="b204c-226">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="b204c-227">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="b204c-227">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

