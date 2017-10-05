---
title: "Redigovat řezy s Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje, jak redigovat řezy s Azure media analytics."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;
ms.openlocfilehash: 747f3ae1a7484515083c590942de3da22568cd39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a><span data-ttu-id="9f6dd-103">Redigovat řezy s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="9f6dd-103">Redact faces with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="9f6dd-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="9f6dd-104">Overview</span></span>
<span data-ttu-id="9f6dd-105">**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in the cloud.</span></span> <span data-ttu-id="9f6dd-106">Vzhled redigování umožní vám upravit videa k rozostření řezy vybrané jednotlivce.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-106">Face redaction enables you to modify your video in order to blur faces of selected individuals.</span></span> <span data-ttu-id="9f6dd-107">Můžete použít službu redigování řez ve veřejné scénáře zabezpečení a média zprávy.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-107">You may want to use the face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="9f6dd-108">Pár minut záznamů, která obsahuje více řezy může trvat hodiny redigovat ručně, ale s touto službou proces redigování řez bude vyžadovat několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-108">A few minutes of footage that contains multiple faces can take hours to redact manually, but with this service the face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="9f6dd-109">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="9f6dd-110">Toto téma uvádí podrobnosti o **Azure Media Redactor** a ukazuje, jak pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-110">This topic gives details about **Azure Media Redactor** and shows how to use it with Media Services SDK for .NET.</span></span>

<span data-ttu-id="9f6dd-111">**Azure Media Redactor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-111">The **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="9f6dd-112">Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-112">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="9f6dd-113">Tato verze preview je aktuálně zdarma.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-113">This preview is currently free of charge.</span></span> 

## <a name="face-redaction-modes"></a><span data-ttu-id="9f6dd-114">Vzhled redigování režimy</span><span class="sxs-lookup"><span data-stu-id="9f6dd-114">Face redaction modes</span></span>
<span data-ttu-id="9f6dd-115">Obličeje redigování funguje zjišťování tyto řezy v každém snímku videa a sledováním vzhled objekt obou dopředný a zpětně v průběhu času, aby stejné jednotlivých můžete hranice z jiných úhly také.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-115">Facial redaction works by detecting faces in every frame of video and tracking the face object both forwards and backwards in time, so that the same individual can be blurred from other angles as well.</span></span> <span data-ttu-id="9f6dd-116">Proces automatického redigování je velmi složitý a nemá vždy produktu 100 % požadované výstupu z tohoto důvodu Media Analytics nabízí několik způsobů, jak upravit finální výstup.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-116">The automated redaction process is very complex and does not always produce 100% of desired output, for this reason Media Analytics provides you with a couple of ways to modify the final output.</span></span>

<span data-ttu-id="9f6dd-117">Kromě plně automatickými režimu je dva průchodu pracovního postupu, které umožňuje výběr nebo deaktivuje-selection nalezen ploch prostřednictvím seznam identifikátorů.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-117">In addition to a fully automatic mode, there is a two-pass workflow which allows the selection/de-selection of found faces via a list of IDs.</span></span> <span data-ttu-id="9f6dd-118">Také aby libovolný za rámce úpravy MP používá soubor metadat ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-118">Also, to make arbitrary per frame adjustments the MP uses a metadata file in JSON format.</span></span> <span data-ttu-id="9f6dd-119">Tento pracovní postup je rozdělená do **analyzovat** a **Redact** režimy.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-119">This workflow is split into **Analyze** and **Redact** modes.</span></span> <span data-ttu-id="9f6dd-120">Zkombinováním dva režimy při jednom průchodu používající obě úlohy v úloh. Tento režim je volána **kombinované**.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-120">You can combine the two modes in a single pass that runs both tasks in one job; this mode is called **Combined**.</span></span>

### <a name="combined-mode"></a><span data-ttu-id="9f6dd-121">Kombinovaná režimu</span><span class="sxs-lookup"><span data-stu-id="9f6dd-121">Combined mode</span></span>
<span data-ttu-id="9f6dd-122">Vznikne tak zredigované mp4 automaticky bez jakékoli ruční vstup.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-122">This will produce a redacted mp4 automatically without any manual input.</span></span>

| <span data-ttu-id="9f6dd-123">Krok</span><span class="sxs-lookup"><span data-stu-id="9f6dd-123">Stage</span></span> | <span data-ttu-id="9f6dd-124">Název souboru</span><span class="sxs-lookup"><span data-stu-id="9f6dd-124">File Name</span></span> | <span data-ttu-id="9f6dd-125">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9f6dd-125">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f6dd-126">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="9f6dd-126">Input asset</span></span> |<span data-ttu-id="9f6dd-127">foo.bar</span><span class="sxs-lookup"><span data-stu-id="9f6dd-127">foo.bar</span></span> |<span data-ttu-id="9f6dd-128">Video ve formátu WMV, MOV nebo MP4</span><span class="sxs-lookup"><span data-stu-id="9f6dd-128">Video in WMV, MOV, or MP4 format</span></span> |
| <span data-ttu-id="9f6dd-129">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-129">Input config</span></span> |<span data-ttu-id="9f6dd-130">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-130">Job configuration preset</span></span> |<span data-ttu-id="9f6dd-131">{'version':'1.0 ', 'možnosti': {"režim": "kombinovanou"}}</span><span class="sxs-lookup"><span data-stu-id="9f6dd-131">{'version':'1.0', 'options': {'mode':'combined'}}</span></span> |
| <span data-ttu-id="9f6dd-132">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="9f6dd-132">Output asset</span></span> |<span data-ttu-id="9f6dd-133">foo_redacted.MP4</span><span class="sxs-lookup"><span data-stu-id="9f6dd-133">foo_redacted.mp4</span></span> |<span data-ttu-id="9f6dd-134">Video s stírá použít</span><span class="sxs-lookup"><span data-stu-id="9f6dd-134">Video with blurring applied</span></span> |

#### <a name="input-example"></a><span data-ttu-id="9f6dd-135">Vstupní příklad:</span><span class="sxs-lookup"><span data-stu-id="9f6dd-135">Input example:</span></span>
[<span data-ttu-id="9f6dd-136">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="9f6dd-136">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a><span data-ttu-id="9f6dd-137">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="9f6dd-137">Output example:</span></span>
[<span data-ttu-id="9f6dd-138">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="9f6dd-138">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a><span data-ttu-id="9f6dd-139">Analýza režimu</span><span class="sxs-lookup"><span data-stu-id="9f6dd-139">Analyze mode</span></span>
<span data-ttu-id="9f6dd-140">**Analyzovat** průchodu dva průchodu pracovního postupu přebírá vstup videa a vytvoří soubor JSON místa vzhled a obrázků jpg jednotlivých zjistila řez.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-140">The **analyze** pass of the two-pass workflow takes a video input and produces a JSON file of face locations, and jpg images of each detected face.</span></span>

| <span data-ttu-id="9f6dd-141">Krok</span><span class="sxs-lookup"><span data-stu-id="9f6dd-141">Stage</span></span> | <span data-ttu-id="9f6dd-142">Název souboru</span><span class="sxs-lookup"><span data-stu-id="9f6dd-142">File Name</span></span> | <span data-ttu-id="9f6dd-143">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9f6dd-143">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f6dd-144">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="9f6dd-144">Input asset</span></span> |<span data-ttu-id="9f6dd-145">foo.bar</span><span class="sxs-lookup"><span data-stu-id="9f6dd-145">foo.bar</span></span> |<span data-ttu-id="9f6dd-146">Video ve formátu WMV, MPV nebo MP4</span><span class="sxs-lookup"><span data-stu-id="9f6dd-146">Video in WMV, MPV, or MP4 format</span></span> |
| <span data-ttu-id="9f6dd-147">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-147">Input config</span></span> |<span data-ttu-id="9f6dd-148">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-148">Job configuration preset</span></span> |<span data-ttu-id="9f6dd-149">{'version':'1.0 ', 'možnosti': {'režimu': 'analyzovat.}}</span><span class="sxs-lookup"><span data-stu-id="9f6dd-149">{'version':'1.0', 'options': {'mode':'analyze'}}</span></span> |
| <span data-ttu-id="9f6dd-150">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="9f6dd-150">Output asset</span></span> |<span data-ttu-id="9f6dd-151">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="9f6dd-151">foo_annotations.json</span></span> |<span data-ttu-id="9f6dd-152">Poznámky data vzhled umístění ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-152">Annotation data of face locations in JSON format.</span></span> <span data-ttu-id="9f6dd-153">To se dá upravit uživatel k úpravě stírá ohraničujícího polí.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-153">This can be edited by the user to modify the blurring bounding boxes.</span></span> <span data-ttu-id="9f6dd-154">Viz následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-154">See sample below.</span></span> |
| <span data-ttu-id="9f6dd-155">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="9f6dd-155">Output asset</span></span> |<span data-ttu-id="9f6dd-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span><span class="sxs-lookup"><span data-stu-id="9f6dd-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span></span> |<span data-ttu-id="9f6dd-157">Oříznutý jpg jednotlivých zjistil vzhled, kde číslo udává labelId písmo</span><span class="sxs-lookup"><span data-stu-id="9f6dd-157">A cropped jpg of each detected face, where the number indicates the labelId of the face</span></span> |

#### <a name="output-example"></a><span data-ttu-id="9f6dd-158">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="9f6dd-158">Output example:</span></span>

    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated

### <a name="redact-mode"></a><span data-ttu-id="9f6dd-159">Redigovat režimu</span><span class="sxs-lookup"><span data-stu-id="9f6dd-159">Redact mode</span></span>
<span data-ttu-id="9f6dd-160">Druhé fázi pracovního postupu přebírá větší počet vstupních hodnot, které musí zkombinovat do jednoho datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-160">The second pass of the workflow takes a larger number of inputs that must be combined into a single asset.</span></span>

<span data-ttu-id="9f6dd-161">To zahrnuje seznam ID k rozostření, původní video a poznámky JSON.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-161">This includes a list of IDs to blur, the original video, and the annotations JSON.</span></span> <span data-ttu-id="9f6dd-162">Tento režim pomocí poznámky použije stírá na vstup videa.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-162">This mode uses the annotations to apply blurring on the input video.</span></span>

<span data-ttu-id="9f6dd-163">Výstup z průchodu analyzovat nezahrnuje původní video.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-163">The output from the Analyze pass does not include the original video.</span></span> <span data-ttu-id="9f6dd-164">Video je třeba nahraje do vstupní asset pro úlohu režimu Redact a vybrat jako primární soubor.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-164">The video needs to be uploaded into the input asset for the Redact mode task and selected as the primary file.</span></span>

| <span data-ttu-id="9f6dd-165">Krok</span><span class="sxs-lookup"><span data-stu-id="9f6dd-165">Stage</span></span> | <span data-ttu-id="9f6dd-166">Název souboru</span><span class="sxs-lookup"><span data-stu-id="9f6dd-166">File Name</span></span> | <span data-ttu-id="9f6dd-167">Poznámky</span><span class="sxs-lookup"><span data-stu-id="9f6dd-167">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9f6dd-168">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="9f6dd-168">Input asset</span></span> |<span data-ttu-id="9f6dd-169">foo.bar</span><span class="sxs-lookup"><span data-stu-id="9f6dd-169">foo.bar</span></span> |<span data-ttu-id="9f6dd-170">Video ve formátu WMV, MPV nebo MP4.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-170">Video in WMV, MPV, or MP4 format.</span></span> <span data-ttu-id="9f6dd-171">Stejné jako v kroku 1 videa.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-171">Same video as in step 1.</span></span> |
| <span data-ttu-id="9f6dd-172">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="9f6dd-172">Input asset</span></span> |<span data-ttu-id="9f6dd-173">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="9f6dd-173">foo_annotations.json</span></span> |<span data-ttu-id="9f6dd-174">Soubor metadat poznámky z první fázi, pomocí volitelné úpravy.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-174">annotations metadata file from phase one, with optional modifications.</span></span> |
| <span data-ttu-id="9f6dd-175">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="9f6dd-175">Input asset</span></span> |<span data-ttu-id="9f6dd-176">foo_IDList.txt (volitelné)</span><span class="sxs-lookup"><span data-stu-id="9f6dd-176">foo_IDList.txt (Optional)</span></span> |<span data-ttu-id="9f6dd-177">Volitelné nový řádek oddělený seznam vzhled ID chcete redigovat.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-177">Optional new line separated list of face IDs to redact.</span></span> <span data-ttu-id="9f6dd-178">Pokud necháte prázdnou, tato rozostří všechny řezy.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-178">If left blank, this blurs all faces.</span></span> |
| <span data-ttu-id="9f6dd-179">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-179">Input config</span></span> |<span data-ttu-id="9f6dd-180">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="9f6dd-180">Job configuration preset</span></span> |<span data-ttu-id="9f6dd-181">{'version':'1.0 ', 'možnosti': {'režimu': 'redigovat.}}</span><span class="sxs-lookup"><span data-stu-id="9f6dd-181">{'version':'1.0', 'options': {'mode':'redact'}}</span></span> |
| <span data-ttu-id="9f6dd-182">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="9f6dd-182">Output asset</span></span> |<span data-ttu-id="9f6dd-183">foo_redacted.MP4</span><span class="sxs-lookup"><span data-stu-id="9f6dd-183">foo_redacted.mp4</span></span> |<span data-ttu-id="9f6dd-184">Video s stírá použít podle poznámky</span><span class="sxs-lookup"><span data-stu-id="9f6dd-184">Video with blurring applied based on annotations</span></span> |

#### <a name="example-output"></a><span data-ttu-id="9f6dd-185">Příklad výstupu</span><span class="sxs-lookup"><span data-stu-id="9f6dd-185">Example output</span></span>
<span data-ttu-id="9f6dd-186">Toto je výstup IDList s jedno ID vybrané.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-186">This is the output from an IDList with one ID selected.</span></span>

[<span data-ttu-id="9f6dd-187">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="9f6dd-187">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

<span data-ttu-id="9f6dd-188">Příklad foo_IDList.txt</span><span class="sxs-lookup"><span data-stu-id="9f6dd-188">Example foo_IDList.txt</span></span>
 
     1
     2
     3

## <a name="blur-types"></a><span data-ttu-id="9f6dd-189">Rozostření typy</span><span class="sxs-lookup"><span data-stu-id="9f6dd-189">Blur types</span></span>

<span data-ttu-id="9f6dd-190">V **kombinované** nebo **Redact** režimu, existují 5 rozostření různé režimy můžete vybrat z prostřednictvím konfigurace vstupu JSON: **nízká**, **Med**, **vysokou**, **ladění**, a **černé**.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-190">In the **Combined** or **Redact** mode, there are 5 different blur modes you can choose from via the JSON input configuration: **Low**, **Med**, **High**, **Debug**, and **Black**.</span></span> <span data-ttu-id="9f6dd-191">Ve výchozím nastavení **Med** se používá.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-191">By default **Med** is used.</span></span>

<span data-ttu-id="9f6dd-192">Můžete najít ukázky níže rozostření typů.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-192">You can find samples of the blur types below.</span></span>

### <a name="example-json"></a><span data-ttu-id="9f6dd-193">Příklad JSON:</span><span class="sxs-lookup"><span data-stu-id="9f6dd-193">Example JSON:</span></span>

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a><span data-ttu-id="9f6dd-194">Nízký</span><span class="sxs-lookup"><span data-stu-id="9f6dd-194">Low</span></span>

![Nízký](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a><span data-ttu-id="9f6dd-196">Med</span><span class="sxs-lookup"><span data-stu-id="9f6dd-196">Med</span></span>

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a><span data-ttu-id="9f6dd-198">Vysoký</span><span class="sxs-lookup"><span data-stu-id="9f6dd-198">High</span></span>

![Vysoký](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a><span data-ttu-id="9f6dd-200">Ladění</span><span class="sxs-lookup"><span data-stu-id="9f6dd-200">Debug</span></span>

![Ladění](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a><span data-ttu-id="9f6dd-202">Černá</span><span class="sxs-lookup"><span data-stu-id="9f6dd-202">Black</span></span>

![Černá](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a><span data-ttu-id="9f6dd-204">Elementy výstupního souboru JSON</span><span class="sxs-lookup"><span data-stu-id="9f6dd-204">Elements of the output JSON file</span></span>

<span data-ttu-id="9f6dd-205">MP redigování poskytuje vysokou přesnost vzhled umístění detekce a sledování, může zjistit až 64 lidského řezy snímek videa.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-205">The Redaction MP provides high precision face location detection and tracking that can detect up to 64 human faces in a video frame.</span></span> <span data-ttu-id="9f6dd-206">Čelní řezy zadejte nejlepších výsledků dosáhnete, při straně řezy a malé řezy (je menší než nebo rovno 24 x 24 pixelů) jsou dost náročné.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-206">Frontal faces provide the best results, while side faces and small faces (less than or equal to 24x24 pixels) are challenging.</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a><span data-ttu-id="9f6dd-207">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9f6dd-207">.NET sample code</span></span>

<span data-ttu-id="9f6dd-208">Program zobrazí následující postup:</span><span class="sxs-lookup"><span data-stu-id="9f6dd-208">The following program shows how to:</span></span>

1. <span data-ttu-id="9f6dd-209">Vytvořte asset a nahrajte soubor média do assetu.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-209">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="9f6dd-210">Vytvořte úlohu s úkolem redigování vzhled podle konfigurační soubor, který obsahuje následující přednastavení json.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-210">Create a job with a face redaction task based on a configuration file that contains the following json preset.</span></span> 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. <span data-ttu-id="9f6dd-211">Stáhněte soubory JSON výstupu.</span><span class="sxs-lookup"><span data-stu-id="9f6dd-211">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="9f6dd-212">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="9f6dd-212">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="9f6dd-213">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="9f6dd-213">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="9f6dd-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="9f6dd-214">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceRedaction
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

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a><span data-ttu-id="9f6dd-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f6dd-215">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="9f6dd-216">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="9f6dd-216">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="9f6dd-217">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="9f6dd-217">Related links</span></span>
[<span data-ttu-id="9f6dd-218">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="9f6dd-218">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="9f6dd-219">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="9f6dd-219">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

