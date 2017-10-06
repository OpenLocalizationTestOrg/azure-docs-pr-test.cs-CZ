---
title: "aaaRedact řezy s Azure Media Analytics | Microsoft Docs"
description: "Toto téma ukazuje, jak tooredact otočená s Azure media analytics."
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
ms.openlocfilehash: 1f5688a8c6374151c526a9c702b904d8c3e46164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a><span data-ttu-id="f8525-103">Redigovat řezy s Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="f8525-103">Redact faces with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="f8525-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f8525-104">Overview</span></span>
<span data-ttu-id="f8525-105">**Azure Media Redactor** je [Azure Media Analytics](media-services-analytics-overview.md) procesor médií (PP), která nabízí redigování škálovatelné řez v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="f8525-106">Vzhled redigování umožňuje vám toomodify videa v pořadí tooblur řezy vybrané osob.</span><span class="sxs-lookup"><span data-stu-id="f8525-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="f8525-107">Můžete chtít toouse hello vzhled redigování služby ve veřejné scénáře zabezpečení a média zprávy.</span><span class="sxs-lookup"><span data-stu-id="f8525-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="f8525-108">Pár minut záznamů, která obsahuje více řezy může trvat hodiny tooredact ručně, ale s této služby hello vzhled redigování proces bude vyžadovat několika jednoduchých kroků.</span><span class="sxs-lookup"><span data-stu-id="f8525-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="f8525-109">Další informace najdete v tématu [to](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.</span><span class="sxs-lookup"><span data-stu-id="f8525-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="f8525-110">Toto téma uvádí podrobnosti o **Azure Media Redactor** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="f8525-110">This topic gives details about **Azure Media Redactor** and shows how toouse it with Media Services SDK for .NET.</span></span>

<span data-ttu-id="f8525-111">Hello **Azure Media Redactor** MP je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="f8525-111">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="f8525-112">Je k dispozici ve všech veřejných oblastí Azure a také datových centrech US Government a Číně.</span><span class="sxs-lookup"><span data-stu-id="f8525-112">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="f8525-113">Tato verze preview je aktuálně zdarma.</span><span class="sxs-lookup"><span data-stu-id="f8525-113">This preview is currently free of charge.</span></span> 

## <a name="face-redaction-modes"></a><span data-ttu-id="f8525-114">Vzhled redigování režimy</span><span class="sxs-lookup"><span data-stu-id="f8525-114">Face redaction modes</span></span>
<span data-ttu-id="f8525-115">Funguje obličeje redigování zjišťuje tyto řezy v každém snímku videa a sledování hello vzhled objektu obě dopředný a zpětně v čase, tak, aby hello stejné individuální může být hranice z jiných úhly také.</span><span class="sxs-lookup"><span data-stu-id="f8525-115">Facial redaction works by detecting faces in every frame of video and tracking hello face object both forwards and backwards in time, so that hello same individual can be blurred from other angles as well.</span></span> <span data-ttu-id="f8525-116">Hello automatizované redigování proces je velmi složitý a vždy nevytváří 100 % požadované výstup z tohoto důvodu, které jste s několika způsoby Media Analytics poskytuje toomodify hello závěrečný výstup.</span><span class="sxs-lookup"><span data-stu-id="f8525-116">hello automated redaction process is very complex and does not always produce 100% of desired output, for this reason Media Analytics provides you with a couple of ways toomodify hello final output.</span></span>

<span data-ttu-id="f8525-117">V režimu plně automatické přidání tooa je dva průchodu pracovního postupu, které umožňuje hello výběr/deaktivuje-selection nalezen ploch prostřednictvím seznam identifikátorů.</span><span class="sxs-lookup"><span data-stu-id="f8525-117">In addition tooa fully automatic mode, there is a two-pass workflow which allows hello selection/de-selection of found faces via a list of IDs.</span></span> <span data-ttu-id="f8525-118">Toomake libovolný za rámce úpravy hello MP také používá soubor metadat ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f8525-118">Also, toomake arbitrary per frame adjustments hello MP uses a metadata file in JSON format.</span></span> <span data-ttu-id="f8525-119">Tento pracovní postup je rozdělená do **analyzovat** a **Redact** režimy.</span><span class="sxs-lookup"><span data-stu-id="f8525-119">This workflow is split into **Analyze** and **Redact** modes.</span></span> <span data-ttu-id="f8525-120">Zkombinováním hello dva režimy při jednom průchodu používající obě úlohy v úloh. Tento režim je volána **kombinované**.</span><span class="sxs-lookup"><span data-stu-id="f8525-120">You can combine hello two modes in a single pass that runs both tasks in one job; this mode is called **Combined**.</span></span>

### <a name="combined-mode"></a><span data-ttu-id="f8525-121">Kombinovaná režimu</span><span class="sxs-lookup"><span data-stu-id="f8525-121">Combined mode</span></span>
<span data-ttu-id="f8525-122">Vznikne tak zredigované mp4 automaticky bez jakékoli ruční vstup.</span><span class="sxs-lookup"><span data-stu-id="f8525-122">This will produce a redacted mp4 automatically without any manual input.</span></span>

| <span data-ttu-id="f8525-123">Krok</span><span class="sxs-lookup"><span data-stu-id="f8525-123">Stage</span></span> | <span data-ttu-id="f8525-124">Název souboru</span><span class="sxs-lookup"><span data-stu-id="f8525-124">File Name</span></span> | <span data-ttu-id="f8525-125">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f8525-125">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f8525-126">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="f8525-126">Input asset</span></span> |<span data-ttu-id="f8525-127">foo.bar</span><span class="sxs-lookup"><span data-stu-id="f8525-127">foo.bar</span></span> |<span data-ttu-id="f8525-128">Video ve formátu WMV, MOV nebo MP4</span><span class="sxs-lookup"><span data-stu-id="f8525-128">Video in WMV, MOV, or MP4 format</span></span> |
| <span data-ttu-id="f8525-129">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-129">Input config</span></span> |<span data-ttu-id="f8525-130">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-130">Job configuration preset</span></span> |<span data-ttu-id="f8525-131">{'version':'1.0 ', 'možnosti': {"režim": "kombinovanou"}}</span><span class="sxs-lookup"><span data-stu-id="f8525-131">{'version':'1.0', 'options': {'mode':'combined'}}</span></span> |
| <span data-ttu-id="f8525-132">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="f8525-132">Output asset</span></span> |<span data-ttu-id="f8525-133">foo_redacted.MP4</span><span class="sxs-lookup"><span data-stu-id="f8525-133">foo_redacted.mp4</span></span> |<span data-ttu-id="f8525-134">Video s stírá použít</span><span class="sxs-lookup"><span data-stu-id="f8525-134">Video with blurring applied</span></span> |

#### <a name="input-example"></a><span data-ttu-id="f8525-135">Vstupní příklad:</span><span class="sxs-lookup"><span data-stu-id="f8525-135">Input example:</span></span>
[<span data-ttu-id="f8525-136">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="f8525-136">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a><span data-ttu-id="f8525-137">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="f8525-137">Output example:</span></span>
[<span data-ttu-id="f8525-138">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="f8525-138">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a><span data-ttu-id="f8525-139">Analýza režimu</span><span class="sxs-lookup"><span data-stu-id="f8525-139">Analyze mode</span></span>
<span data-ttu-id="f8525-140">Hello **analyzovat** průchodu hello dva průchodu pracovního postupu přebírá vstup videa a vytvoří soubor JSON místa vzhled a obrázků jpg jednotlivých zjistila řez.</span><span class="sxs-lookup"><span data-stu-id="f8525-140">hello **analyze** pass of hello two-pass workflow takes a video input and produces a JSON file of face locations, and jpg images of each detected face.</span></span>

| <span data-ttu-id="f8525-141">Krok</span><span class="sxs-lookup"><span data-stu-id="f8525-141">Stage</span></span> | <span data-ttu-id="f8525-142">Název souboru</span><span class="sxs-lookup"><span data-stu-id="f8525-142">File Name</span></span> | <span data-ttu-id="f8525-143">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f8525-143">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f8525-144">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="f8525-144">Input asset</span></span> |<span data-ttu-id="f8525-145">foo.bar</span><span class="sxs-lookup"><span data-stu-id="f8525-145">foo.bar</span></span> |<span data-ttu-id="f8525-146">Video ve formátu WMV, MPV nebo MP4</span><span class="sxs-lookup"><span data-stu-id="f8525-146">Video in WMV, MPV, or MP4 format</span></span> |
| <span data-ttu-id="f8525-147">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-147">Input config</span></span> |<span data-ttu-id="f8525-148">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-148">Job configuration preset</span></span> |<span data-ttu-id="f8525-149">{'version':'1.0 ', 'možnosti': {'režimu': 'analyzovat.}}</span><span class="sxs-lookup"><span data-stu-id="f8525-149">{'version':'1.0', 'options': {'mode':'analyze'}}</span></span> |
| <span data-ttu-id="f8525-150">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="f8525-150">Output asset</span></span> |<span data-ttu-id="f8525-151">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="f8525-151">foo_annotations.json</span></span> |<span data-ttu-id="f8525-152">Poznámky data vzhled umístění ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="f8525-152">Annotation data of face locations in JSON format.</span></span> <span data-ttu-id="f8525-153">To se dá upravit pomocí hello uživatele toomodify hello stírá ohraničujícího polí.</span><span class="sxs-lookup"><span data-stu-id="f8525-153">This can be edited by hello user toomodify hello blurring bounding boxes.</span></span> <span data-ttu-id="f8525-154">Viz následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="f8525-154">See sample below.</span></span> |
| <span data-ttu-id="f8525-155">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="f8525-155">Output asset</span></span> |<span data-ttu-id="f8525-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span><span class="sxs-lookup"><span data-stu-id="f8525-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span></span> |<span data-ttu-id="f8525-157">Oříznutý jpg jednotlivých zjistil vzhled, kde hello číslo označuje hello labelId řezu hello</span><span class="sxs-lookup"><span data-stu-id="f8525-157">A cropped jpg of each detected face, where hello number indicates hello labelId of hello face</span></span> |

#### <a name="output-example"></a><span data-ttu-id="f8525-158">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="f8525-158">Output example:</span></span>

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

### <a name="redact-mode"></a><span data-ttu-id="f8525-159">Redigovat režimu</span><span class="sxs-lookup"><span data-stu-id="f8525-159">Redact mode</span></span>
<span data-ttu-id="f8525-160">Hello druhého průchodu hello pracovního postupu přebírá větší počet vstupních hodnot, které musí zkombinovat do jednoho datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="f8525-160">hello second pass of hello workflow takes a larger number of inputs that must be combined into a single asset.</span></span>

<span data-ttu-id="f8525-161">To zahrnuje seznam ID tooblur hello původní video a poznámky hello JSON.</span><span class="sxs-lookup"><span data-stu-id="f8525-161">This includes a list of IDs tooblur, hello original video, and hello annotations JSON.</span></span> <span data-ttu-id="f8525-162">Tento režim používá hello poznámky tooapply stírá na vstupní video hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-162">This mode uses hello annotations tooapply blurring on hello input video.</span></span>

<span data-ttu-id="f8525-163">výstup hello analyzovat průchodu Hello nezahrnuje původní video hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-163">hello output from hello Analyze pass does not include hello original video.</span></span> <span data-ttu-id="f8525-164">Hello video musí toobe nahraje do vstupní asset hello hello Redact režimu úlohy a vybrat jako primární soubor hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-164">hello video needs toobe uploaded into hello input asset for hello Redact mode task and selected as hello primary file.</span></span>

| <span data-ttu-id="f8525-165">Krok</span><span class="sxs-lookup"><span data-stu-id="f8525-165">Stage</span></span> | <span data-ttu-id="f8525-166">Název souboru</span><span class="sxs-lookup"><span data-stu-id="f8525-166">File Name</span></span> | <span data-ttu-id="f8525-167">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f8525-167">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f8525-168">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="f8525-168">Input asset</span></span> |<span data-ttu-id="f8525-169">foo.bar</span><span class="sxs-lookup"><span data-stu-id="f8525-169">foo.bar</span></span> |<span data-ttu-id="f8525-170">Video ve formátu WMV, MPV nebo MP4.</span><span class="sxs-lookup"><span data-stu-id="f8525-170">Video in WMV, MPV, or MP4 format.</span></span> <span data-ttu-id="f8525-171">Stejné jako v kroku 1 videa.</span><span class="sxs-lookup"><span data-stu-id="f8525-171">Same video as in step 1.</span></span> |
| <span data-ttu-id="f8525-172">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="f8525-172">Input asset</span></span> |<span data-ttu-id="f8525-173">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="f8525-173">foo_annotations.json</span></span> |<span data-ttu-id="f8525-174">Soubor metadat poznámky z první fázi, pomocí volitelné úpravy.</span><span class="sxs-lookup"><span data-stu-id="f8525-174">annotations metadata file from phase one, with optional modifications.</span></span> |
| <span data-ttu-id="f8525-175">Vstupní prostředek</span><span class="sxs-lookup"><span data-stu-id="f8525-175">Input asset</span></span> |<span data-ttu-id="f8525-176">foo_IDList.txt (volitelné)</span><span class="sxs-lookup"><span data-stu-id="f8525-176">foo_IDList.txt (Optional)</span></span> |<span data-ttu-id="f8525-177">Volitelné nový řádek oddělený seznam ID tooredact řez.</span><span class="sxs-lookup"><span data-stu-id="f8525-177">Optional new line separated list of face IDs tooredact.</span></span> <span data-ttu-id="f8525-178">Pokud necháte prázdnou, tato rozostří všechny řezy.</span><span class="sxs-lookup"><span data-stu-id="f8525-178">If left blank, this blurs all faces.</span></span> |
| <span data-ttu-id="f8525-179">Vstupní konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-179">Input config</span></span> |<span data-ttu-id="f8525-180">Předvolby úlohy konfigurace</span><span class="sxs-lookup"><span data-stu-id="f8525-180">Job configuration preset</span></span> |<span data-ttu-id="f8525-181">{'version':'1.0 ', 'možnosti': {'režimu': 'redigovat.}}</span><span class="sxs-lookup"><span data-stu-id="f8525-181">{'version':'1.0', 'options': {'mode':'redact'}}</span></span> |
| <span data-ttu-id="f8525-182">Výstupní asset</span><span class="sxs-lookup"><span data-stu-id="f8525-182">Output asset</span></span> |<span data-ttu-id="f8525-183">foo_redacted.MP4</span><span class="sxs-lookup"><span data-stu-id="f8525-183">foo_redacted.mp4</span></span> |<span data-ttu-id="f8525-184">Video s stírá použít podle poznámky</span><span class="sxs-lookup"><span data-stu-id="f8525-184">Video with blurring applied based on annotations</span></span> |

#### <a name="example-output"></a><span data-ttu-id="f8525-185">Příklad výstupu</span><span class="sxs-lookup"><span data-stu-id="f8525-185">Example output</span></span>
<span data-ttu-id="f8525-186">Toto je výstup hello IDList s jedno ID vybrané.</span><span class="sxs-lookup"><span data-stu-id="f8525-186">This is hello output from an IDList with one ID selected.</span></span>

[<span data-ttu-id="f8525-187">přehrát toto video</span><span class="sxs-lookup"><span data-stu-id="f8525-187">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

<span data-ttu-id="f8525-188">Příklad foo_IDList.txt</span><span class="sxs-lookup"><span data-stu-id="f8525-188">Example foo_IDList.txt</span></span>
 
     1
     2
     3

## <a name="blur-types"></a><span data-ttu-id="f8525-189">Rozostření typy</span><span class="sxs-lookup"><span data-stu-id="f8525-189">Blur types</span></span>

<span data-ttu-id="f8525-190">V hello **kombinované** nebo **Redact** režimu, existují 5 rozostření různé režimy můžete vybrat z prostřednictvím hello JSON vstupu konfigurace: **nízká**, **Med**, **Vysokou**, **ladění**, a **černé**.</span><span class="sxs-lookup"><span data-stu-id="f8525-190">In hello **Combined** or **Redact** mode, there are 5 different blur modes you can choose from via hello JSON input configuration: **Low**, **Med**, **High**, **Debug**, and **Black**.</span></span> <span data-ttu-id="f8525-191">Ve výchozím nastavení **Med** se používá.</span><span class="sxs-lookup"><span data-stu-id="f8525-191">By default **Med** is used.</span></span>

<span data-ttu-id="f8525-192">Můžete najít, že ukázky hello rozostření typy níže.</span><span class="sxs-lookup"><span data-stu-id="f8525-192">You can find samples of hello blur types below.</span></span>

### <a name="example-json"></a><span data-ttu-id="f8525-193">Příklad JSON:</span><span class="sxs-lookup"><span data-stu-id="f8525-193">Example JSON:</span></span>

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a><span data-ttu-id="f8525-194">Nízký</span><span class="sxs-lookup"><span data-stu-id="f8525-194">Low</span></span>

![Nízký](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a><span data-ttu-id="f8525-196">Med</span><span class="sxs-lookup"><span data-stu-id="f8525-196">Med</span></span>

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a><span data-ttu-id="f8525-198">Vysoký</span><span class="sxs-lookup"><span data-stu-id="f8525-198">High</span></span>

![Vysoký](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a><span data-ttu-id="f8525-200">Ladění</span><span class="sxs-lookup"><span data-stu-id="f8525-200">Debug</span></span>

![Ladění](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a><span data-ttu-id="f8525-202">Černá</span><span class="sxs-lookup"><span data-stu-id="f8525-202">Black</span></span>

![Černá](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="f8525-204">Elementy výstupního souboru JSON, hello</span><span class="sxs-lookup"><span data-stu-id="f8525-204">Elements of hello output JSON file</span></span>

<span data-ttu-id="f8525-205">Hello redigování MP poskytuje vysokou přesnost vzhled umístění zjišťování a sledování, které může zjistit, až too64 lidské řezy v snímek videa.</span><span class="sxs-lookup"><span data-stu-id="f8525-205">hello Redaction MP provides high precision face location detection and tracking that can detect up too64 human faces in a video frame.</span></span> <span data-ttu-id="f8525-206">Čelní řezy zadejte hello nejlepších výsledků při straně řezy a malé řezy (menší než nebo roven hodnotě too24x24 pixelů) jsou náročné.</span><span class="sxs-lookup"><span data-stu-id="f8525-206">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) are challenging.</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a><span data-ttu-id="f8525-207">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="f8525-207">.NET sample code</span></span>

<span data-ttu-id="f8525-208">ukazuje programu Hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="f8525-208">hello following program shows how to:</span></span>

1. <span data-ttu-id="f8525-209">Vytvořte asset a nahrajte soubor média do hello asset.</span><span class="sxs-lookup"><span data-stu-id="f8525-209">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="f8525-210">Vytvořte úlohu s úkolem redigování vzhled podle konfigurační soubor, který obsahuje následující json přednastavených hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-210">Create a job with a face redaction task based on a configuration file that contains hello following json preset.</span></span> 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. <span data-ttu-id="f8525-211">Stáhněte soubory JSON výstup hello.</span><span class="sxs-lookup"><span data-stu-id="f8525-211">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="f8525-212">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="f8525-212">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="f8525-213">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f8525-213">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="f8525-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="f8525-214">Example</span></span>

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

            // Run hello FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download hello job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload hello input media file toostorage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference tooAzure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from hello specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with hello encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a><span data-ttu-id="f8525-215">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8525-215">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f8525-216">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f8525-216">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="f8525-217">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="f8525-217">Related links</span></span>
[<span data-ttu-id="f8525-218">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="f8525-218">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="f8525-219">Ukázky služby Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="f8525-219">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

