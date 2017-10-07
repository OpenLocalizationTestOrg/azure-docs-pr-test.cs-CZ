---
title: "kurzy aaaAvanced Media Encoder Premium pracovního postupu"
description: "Tento dokument obsahuje postupů, které ukazují, jak tooperform pokročilé úlohy s Media Encoder Premium pracovního postupu a také jak toocreate komplexní pracovní postupy pomocí Návrháře pracovního postupu."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a><span data-ttu-id="424e9-103">Pokročilé kurzy Media Encoder Premium pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-103">Advanced Media Encoder Premium Workflow tutorials</span></span>
## <a name="overview"></a><span data-ttu-id="424e9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="424e9-104">Overview</span></span>
<span data-ttu-id="424e9-105">Tento dokument obsahuje postupů, které ukazují jak pracovních toocustomize s **Návrhář postupu provádění**.</span><span class="sxs-lookup"><span data-stu-id="424e9-105">This document contains walkthroughs that show how toocustomize workflows with  **Workflow Designer**.</span></span> <span data-ttu-id="424e9-106">Můžete najít hello soubory skutečné pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span><span class="sxs-lookup"><span data-stu-id="424e9-106">You can find hello actual workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span></span>  

## <a name="toc"></a><span data-ttu-id="424e9-107">TOC</span><span class="sxs-lookup"><span data-stu-id="424e9-107">TOC</span></span>
<span data-ttu-id="424e9-108">jsou popsané Hello následující témata:</span><span class="sxs-lookup"><span data-stu-id="424e9-108">hello following topics are covered:</span></span>

* [<span data-ttu-id="424e9-109">Kódování MXF do MP4 s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="424e9-109">Encoding MXF into a single bitrate MP4</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [<span data-ttu-id="424e9-110">Spuštění nového pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-110">Starting a new workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [<span data-ttu-id="424e9-111">Pomocí hello vstupní soubor média</span><span class="sxs-lookup"><span data-stu-id="424e9-111">Using hello Media File Input</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [<span data-ttu-id="424e9-112">Probíhá kontrola mediální datové proudy</span><span class="sxs-lookup"><span data-stu-id="424e9-112">Inspecting media streams</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [<span data-ttu-id="424e9-113">Přidání video kodéru pro. Generování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="424e9-113">Adding a video encoder for .MP4 file generation</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [<span data-ttu-id="424e9-114">Kódování zvukový datový proud hello</span><span class="sxs-lookup"><span data-stu-id="424e9-114">Encoding hello audio stream</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [<span data-ttu-id="424e9-115">Multiplexní Audio a Video datových proudů do kontejner MP4</span><span class="sxs-lookup"><span data-stu-id="424e9-115">Multiplexing Audio and Video streams into an MP4 container</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [<span data-ttu-id="424e9-116">Zapisování souboru MP4 hello</span><span class="sxs-lookup"><span data-stu-id="424e9-116">Writing hello MP4 file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [<span data-ttu-id="424e9-117">Vytváření Asset Media Services z hello výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="424e9-117">Creating a Media Services Asset from hello output file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [<span data-ttu-id="424e9-118">Test hello dokončení pracovního postupu místně</span><span class="sxs-lookup"><span data-stu-id="424e9-118">Test hello finished workflow locally</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [<span data-ttu-id="424e9-119">Kódování MXF do multibitrate soubory MP4 s rychlostmi – dynamické balení povoleno</span><span class="sxs-lookup"><span data-stu-id="424e9-119">Encoding MXF into multibitrate MP4s - dynamic packaging enabled</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [<span data-ttu-id="424e9-120">Přidání další jeden nebo více MP4 výstupy</span><span class="sxs-lookup"><span data-stu-id="424e9-120">Adding one or more additional MP4 outputs</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [<span data-ttu-id="424e9-121">Konfigurace názvy výstupního souboru hello</span><span class="sxs-lookup"><span data-stu-id="424e9-121">Configuring hello file output names</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [<span data-ttu-id="424e9-122">Přidání samostatných sledovat zvuk</span><span class="sxs-lookup"><span data-stu-id="424e9-122">Adding a separate Audio Track</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [<span data-ttu-id="424e9-123">Přidání hello. Soubor SMIL ISM</span><span class="sxs-lookup"><span data-stu-id="424e9-123">Adding hello .ISM SMIL File</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [<span data-ttu-id="424e9-124">Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého</span><span class="sxs-lookup"><span data-stu-id="424e9-124">Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [<span data-ttu-id="424e9-125">Přehled tooenhance pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-125">Workflow overview tooenhance</span></span>](#workflow-overview-to-enhance)
  * [<span data-ttu-id="424e9-126">Konvence pro pojmenování souborů</span><span class="sxs-lookup"><span data-stu-id="424e9-126">File Naming Conventions</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [<span data-ttu-id="424e9-127">Publikování vlastnosti komponenty do kořenové hello pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-127">Publishing component properties onto hello workflow root</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [<span data-ttu-id="424e9-128">Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="424e9-128">Have generated output file names rely on published property values</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [<span data-ttu-id="424e9-129">Přidává se výstup MP4 toomultibitrate miniatur</span><span class="sxs-lookup"><span data-stu-id="424e9-129">Adding thumbnails toomultibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [<span data-ttu-id="424e9-130">Miniatury tooadd přehled pracovního postupu pro</span><span class="sxs-lookup"><span data-stu-id="424e9-130">Workflow overview tooadd thumbnails to</span></span>](#workflow-overview-to-add-thumbnails-to)
  * [<span data-ttu-id="424e9-131">Přidání JPG kódování</span><span class="sxs-lookup"><span data-stu-id="424e9-131">Adding JPG Encoding</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [<span data-ttu-id="424e9-132">Práci s barevný prostor převod</span><span class="sxs-lookup"><span data-stu-id="424e9-132">Dealing with Color Space conversion</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [<span data-ttu-id="424e9-133">Zápis hello miniatur</span><span class="sxs-lookup"><span data-stu-id="424e9-133">Writing hello thumbnails</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [<span data-ttu-id="424e9-134">Zjištění chyb v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-134">Detecting errors in a workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [<span data-ttu-id="424e9-135">Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-135">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [<span data-ttu-id="424e9-136">Oříznutí založené na čase multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="424e9-136">Time-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [<span data-ttu-id="424e9-137">Pracovní postup přehled toostart oříznutí k přidání</span><span class="sxs-lookup"><span data-stu-id="424e9-137">Workflow overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [<span data-ttu-id="424e9-138">Pomocí hello oříznutí datového proudu</span><span class="sxs-lookup"><span data-stu-id="424e9-138">Using hello Stream Trimmer</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [<span data-ttu-id="424e9-139">Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-139">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [<span data-ttu-id="424e9-140">Představení hello skriptování součásti</span><span class="sxs-lookup"><span data-stu-id="424e9-140">Introducing hello Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [<span data-ttu-id="424e9-141">Skriptování v rámci pracovního postupu: hello, world</span><span class="sxs-lookup"><span data-stu-id="424e9-141">Scripting within a workflow: hello world</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [<span data-ttu-id="424e9-142">Na základě rámce oříznutí multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="424e9-142">Frame-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [<span data-ttu-id="424e9-143">Plán, podle kterého přehled toostart oříznutí k přidání</span><span class="sxs-lookup"><span data-stu-id="424e9-143">Blueprint overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [<span data-ttu-id="424e9-144">Pomocí hello klip seznamu XML</span><span class="sxs-lookup"><span data-stu-id="424e9-144">Using hello Clip List XML</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [<span data-ttu-id="424e9-145">Úprava seznamu klip hello z součásti skriptování</span><span class="sxs-lookup"><span data-stu-id="424e9-145">Modifying hello clip list from a Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [<span data-ttu-id="424e9-146">Přidání vlastnosti ClippingEnabled usnadnění práce</span><span class="sxs-lookup"><span data-stu-id="424e9-146">Adding a ClippingEnabled convenience property</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <span data-ttu-id="424e9-147"><a id="MXF_to_MP4"></a>Kódování MXF do MP4 s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="424e9-147"><a id="MXF_to_MP4"></a>Encoding MXF into a single bitrate MP4</span></span>
<span data-ttu-id="424e9-148">V tomto návodu vytvoříme s jednou přenosovou rychlostí. Soubor MP4 s AAC-HE kódovaný zvuk ze. MXF vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="424e9-148">In this walkthrough we'll create a single bitrate .MP4 file with AAC-HE encoded audio from an .MXF input file.</span></span>

### <span data-ttu-id="424e9-149"><a id="MXF_to_MP4_start_new"></a>Spuštění nového pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-149"><a id="MXF_to_MP4_start_new"></a>Starting a new workflow</span></span>
<span data-ttu-id="424e9-150">Otevřete návrháře pracovních postupů a vyberte "Soubor"-"nový pracovní prostor"-"převod plán, podle kterého"</span><span class="sxs-lookup"><span data-stu-id="424e9-150">Open Workflow Designer and select "File"-"New Workspace"-"Transcode Blueprint"</span></span>

<span data-ttu-id="424e9-151">nový pracovní postup Hello zobrazí 3 prvky:</span><span class="sxs-lookup"><span data-stu-id="424e9-151">hello new workflow will show 3 elements:</span></span>

* <span data-ttu-id="424e9-152">Primární zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="424e9-152">Primary Source File</span></span>
* <span data-ttu-id="424e9-153">Seznam klip ve formátu XML</span><span class="sxs-lookup"><span data-stu-id="424e9-153">Clip List XML</span></span>
* <span data-ttu-id="424e9-154">Výstupní soubor nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="424e9-154">Output File/Asset</span></span>  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

<span data-ttu-id="424e9-156">*Nový pracovní postup kódování*</span><span class="sxs-lookup"><span data-stu-id="424e9-156">*New Encoding Workflow*</span></span>

### <span data-ttu-id="424e9-157"><a id="MXF_to_MP4_with_file_input"></a>Pomocí hello vstupní soubor média</span><span class="sxs-lookup"><span data-stu-id="424e9-157"><a id="MXF_to_MP4_with_file_input"></a>Using hello Media File Input</span></span>
<span data-ttu-id="424e9-158">V pořadí tooaccept naše vstupními médii souborů, jeden začíná přidání komponentu vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="424e9-158">In order tooaccept our input media file, one starts with adding a Media File Input component.</span></span> <span data-ttu-id="424e9-159">tooadd toohello součást pracovního postupu, podívejte se do vyhledávacího pole úložiště hello a přetáhněte hello potřeby položku na návrháře podokně hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-159">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span> <span data-ttu-id="424e9-160">To udělat pro vstupní soubor média hello a připojte hello primární zdrojový soubor součást toohello Filename vstupní PIN kódu z hello vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="424e9-160">Do this for hello Media File Input and connect hello Primary Source File component toohello Filename input pin from hello Media File Input.</span></span>

![Připojených mediálních souborů vstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

<span data-ttu-id="424e9-162">*Připojených mediálních souborů vstup*</span><span class="sxs-lookup"><span data-stu-id="424e9-162">*Connected Media File Input*</span></span>

<span data-ttu-id="424e9-163">Před jsme můžete provést, nejdřív potřebujeme návrháře pracovních postupů toohello tooindicate jaké ukázkový soubor rádi bychom znali toouse toodesign naše pracovního postupu se.</span><span class="sxs-lookup"><span data-stu-id="424e9-163">Before we can do much else, we'll first need tooindicate toohello workflow designer what sample file we'd like toouse toodesign our workflow with.</span></span> <span data-ttu-id="424e9-164">toodo Ano, klikněte na pozadí návrháře podokně hello a vyhledejte hello primární zdrojový soubor vlastnosti v podokně pravém vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-164">toodo so, click hello designer pane background and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="424e9-165">Klikněte na ikonu hello složky a vyberte hello požadovaného souboru tootest pracovního postupu hello se.</span><span class="sxs-lookup"><span data-stu-id="424e9-165">Click hello folder icon and select hello desired file tootest hello workflow with.</span></span> <span data-ttu-id="424e9-166">Jakmile to uděláte, bude součást vstupní soubor média hello zkontrolujte soubor hello a naplnit její výstup PIN tooreflect hello soubor, který ho prověřovány.</span><span class="sxs-lookup"><span data-stu-id="424e9-166">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file it inspected.</span></span>

![Vstupní soubor vyplněná média](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

<span data-ttu-id="424e9-168">*Vstupní soubor vyplněná média*</span><span class="sxs-lookup"><span data-stu-id="424e9-168">*Populated Media File Input*</span></span>

<span data-ttu-id="424e9-169">Při určuje s co zadané že rádi bychom znali toowork s, neříká ještě kde výstup hello kódovaný moct.</span><span class="sxs-lookup"><span data-stu-id="424e9-169">While this specifies with what input we'd like toowork with, it doesn't tell yet where hello encoded output should go to.</span></span> <span data-ttu-id="424e9-170">Podobně jako hello toohow byla nakonfigurována primární zdrojový soubor, teď nakonfigurovat hello výstupní složky proměnné vlastnost pod ním.</span><span class="sxs-lookup"><span data-stu-id="424e9-170">Similar toohow hello Primary Source File was configured, now configure hello Output Folder Variable property, just below it.</span></span>

![Nakonfigurovaný vstupní a výstupní vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

<span data-ttu-id="424e9-172">*Nakonfigurovaný vstupní a výstupní vlastnosti*</span><span class="sxs-lookup"><span data-stu-id="424e9-172">*Configured Input and Output properties*</span></span>

### <span data-ttu-id="424e9-173"><a id="MXF_to_MP4_streams"></a>Probíhá kontrola mediální datové proudy</span><span class="sxs-lookup"><span data-stu-id="424e9-173"><a id="MXF_to_MP4_streams"></a>Inspecting media streams</span></span>
<span data-ttu-id="424e9-174">Je často žádoucí, že tooknow jak hello datového proudu vypadá jako je například toků hello pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-174">Often it's desired tooknow how hello stream looks like that flows through hello workflow.</span></span> <span data-ttu-id="424e9-175">tooinspect datový proud v libovolném bodě v pracovním postupu hello, stačí kliknout na výstupu nebo vstupní PIN kód na všech součástí hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-175">tooinspect a stream at any point in hello workflow, just click an output or input pin on any of hello components.</span></span> <span data-ttu-id="424e9-176">V takovém případě zkuste klepnout na pin výstup hello nekomprimované Video z naší vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="424e9-176">In this case, try clicking on hello Uncompressed Video output pin from our Media File Input.</span></span> <span data-ttu-id="424e9-177">Zobrazí se dialogové okno otevřete umožňuje odchozí video tooinspect hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-177">A dialog will open up that allows tooinspect hello outbound video.</span></span>

![Probíhá kontrola hello nekomprimované Video výstupní kód pin](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

<span data-ttu-id="424e9-179">*Probíhá kontrola hello nekomprimované Video výstupní kód pin*</span><span class="sxs-lookup"><span data-stu-id="424e9-179">*Inspecting hello Uncompressed Video output pin*</span></span>

<span data-ttu-id="424e9-180">V našem případě sdělí nám například jsme se zabývají 1920 × 1080 vstupu v 24 snímků za sekundu v 4:2:2 vzorkování video o téměř 2 minut.</span><span class="sxs-lookup"><span data-stu-id="424e9-180">In our case, it tells us for example that we're dealing with a 1920x1080 input at 24 frames per second in 4:2:2 sampling for a video of almost 2 minutes.</span></span>

### <span data-ttu-id="424e9-181"><a id="MXF_to_MP4_file_generation"></a>Přidání video kodéru pro. Generování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="424e9-181"><a id="MXF_to_MP4_file_generation"></a>Adding a video encoder for .MP4 file generation</span></span>
<span data-ttu-id="424e9-182">Všimněte si, že nyní, nekomprimované Video a více nekomprimované zvuk výstup kódy PIN nejsou k dispozici pro použití na našich vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="424e9-182">Note that now, an Uncompressed Video and multiple Uncompressed Audio output pins are available for use on our Media File Input.</span></span> <span data-ttu-id="424e9-183">V pořadí tooencode hello příchozí video, potřebujeme komponentu kódování – v takovém případě pro generování. Soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="424e9-183">In order tooencode hello inbound video, we need an encoding component - in this case for generating .MP4 files.</span></span>

<span data-ttu-id="424e9-184">tooencode hello tooH.264 datový proud videa, přidejte plochu návrháře součást toohello AVC kodér videa pro hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-184">tooencode hello video stream tooH.264, add hello AVC Video Encoder component toohello designer surface.</span></span> <span data-ttu-id="424e9-185">Tato součást využívá uncompress datový proud videa jako vstup a poskytuje AVC komprimovaný datový proud videa na jeho výstupní kód pin.</span><span class="sxs-lookup"><span data-stu-id="424e9-185">This component takes an uncompress video stream as input and delivers an AVC compressed video stream on its output pin.</span></span>

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

<span data-ttu-id="424e9-187">*Bez připojení kodér AVC*</span><span class="sxs-lookup"><span data-stu-id="424e9-187">*Unconnected AVC Encoder*</span></span>

<span data-ttu-id="424e9-188">Jeho vlastnosti určují, jak hello kódování přesně se stane.</span><span class="sxs-lookup"><span data-stu-id="424e9-188">Its properties determine how hello encoding exactly happens.</span></span> <span data-ttu-id="424e9-189">Pojďme Podíváme se na některé z hello důležitější nastavení:</span><span class="sxs-lookup"><span data-stu-id="424e9-189">Let's have a look at some of hello more important settings:</span></span>

* <span data-ttu-id="424e9-190">Výstupní šířky a výšky výstup: Tyto určují hello řešení hello kódovaný videa.</span><span class="sxs-lookup"><span data-stu-id="424e9-190">Output width and Output height: these determine hello resolution of hello encoded video.</span></span> <span data-ttu-id="424e9-191">V našem případě přejdeme s 640 x 360</span><span class="sxs-lookup"><span data-stu-id="424e9-191">In our case let's go with 640x360</span></span>
* <span data-ttu-id="424e9-192">Obnovovací frekvence: toopassthrough sady, které se právě přijaté hello zdroj obnovovací frekvence, je možné toooverride tomto ale.</span><span class="sxs-lookup"><span data-stu-id="424e9-192">Frame Rate: when set toopassthrough it will just adopt hello source frame rate, it's possible toooverride this though.</span></span> <span data-ttu-id="424e9-193">Všimněte si, že takový převod kmitočet snímků není pohybu vyrovnanou.</span><span class="sxs-lookup"><span data-stu-id="424e9-193">Note that such framerate conversion is not motion-compensated.</span></span>
* <span data-ttu-id="424e9-194">Profil a úroveň: Tyto určují profil hello AVC a úroveň.</span><span class="sxs-lookup"><span data-stu-id="424e9-194">Profile and Level: these determine hello AVC profile and level.</span></span> <span data-ttu-id="424e9-195">tooconveniently získat další informace o různých úrovních hello a profily, klikněte na ikonu otazníku hello v součásti kodér videa AVC hello a stránku nápovědy hello se zobrazit více podrobností o jednotlivých úrovní hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-195">tooconveniently get more information about hello different levels and profiles, click hello question mark icon on hello AVC Video Encoder component and hello help page will show more detail about each of hello levels.</span></span> <span data-ttu-id="424e9-196">Pro naše ukázka přejděme s profilem hlavní na úrovni 3.2 (hello výchozí nastavení).</span><span class="sxs-lookup"><span data-stu-id="424e9-196">For our sample, let's go with Main Profile at level 3.2 (hello default).</span></span>
* <span data-ttu-id="424e9-197">Míra režimu řízení a přenosovou rychlostí (kb/s): v tomto scénáři jsme zvolit konstantní přenosovou (CBR) výstup rychlostí 1 200 kb/s</span><span class="sxs-lookup"><span data-stu-id="424e9-197">Rate Control Mode and Bitrate (kbps): in our scenario we opt for a constant bitrate (CBR) output at 1200 kbps</span></span>
* <span data-ttu-id="424e9-198">Video formát: jde o hello VUI (Video použitelnost informace), který získá zápisu do datového proudu hello H.264 (straně informace, které by mohly používat dekodér tooenhance hello zobrazení, ale není nezbytné toocorrectly dekódovat):</span><span class="sxs-lookup"><span data-stu-id="424e9-198">Video Format: this is about hello VUI (Video Usability Information) that gets written into hello H.264 stream (side information that might be used by a decoder tooenhance hello display but not essential toocorrectly decode):</span></span>
* <span data-ttu-id="424e9-199">NTSC (typická pro USA nebo Japonsko, pomocí 30 fps)</span><span class="sxs-lookup"><span data-stu-id="424e9-199">NTSC (typical for US or Japan, using 30 fps)</span></span>
* <span data-ttu-id="424e9-200">PAL (typická pro Evropu, pomocí 25 fps)</span><span class="sxs-lookup"><span data-stu-id="424e9-200">PAL (typical for Europe, using 25 fps)</span></span>
* <span data-ttu-id="424e9-201">Režim velikost GOP: nakonfigurujeme pevné velikosti GOP pro naše účely s intervalu klíč 2 sekund s GOPs uzavřen.</span><span class="sxs-lookup"><span data-stu-id="424e9-201">GOP Size Mode: we'll configure Fixed GOP Size for our purposes with a Key Interval of 2 seconds with Closed GOPs.</span></span> <span data-ttu-id="424e9-202">Tím se zajistí kompatibilitu s hello, které poskytuje dynamické balení Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="424e9-202">This ensures compatibility with hello dynamic packaging Azure Media Services provides.</span></span>

<span data-ttu-id="424e9-203">toofeed naše kodér AVC připojit pin výstup hello nekomprimované Video z hello vstupní soubor média součást toohello nekomprimované Video vstupní PIN kódu z hello AVC kodér.</span><span class="sxs-lookup"><span data-stu-id="424e9-203">toofeed our AVC encoder, connect hello Uncompressed Video output pin from hello Media File Input component toohello Uncompressed Video input pin from hello AVC encoder.</span></span>

![Kodér připojené AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

<span data-ttu-id="424e9-205">*Kodér připojené hlavní AVC*</span><span class="sxs-lookup"><span data-stu-id="424e9-205">*Connected AVC Main encoder*</span></span>

### <span data-ttu-id="424e9-206"><a id="MXF_to_MP4_audio"></a>Kódování zvukový datový proud hello</span><span class="sxs-lookup"><span data-stu-id="424e9-206"><a id="MXF_to_MP4_audio"></a>Encoding hello audio stream</span></span>
<span data-ttu-id="424e9-207">V tomto okamžiku budeme mít kódování video ale hello původní nekomprimované zvukový stream dál potřebuje toobe komprimované.</span><span class="sxs-lookup"><span data-stu-id="424e9-207">At this point, we have encoded video but hello original uncompressed audio stream still needs toobe compressed.</span></span> <span data-ttu-id="424e9-208">Pro tento budeme věnovat s AAC kódování tak, že hello součást kodér AAC (Dolby).</span><span class="sxs-lookup"><span data-stu-id="424e9-208">For this we'll go with AAC encoding by hello AAC Encoder (Dolby) component.</span></span> <span data-ttu-id="424e9-209">Přidejte ji toohello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-209">Add it toohello workflow.</span></span>

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

<span data-ttu-id="424e9-211">*Bez připojení AAC kodér*</span><span class="sxs-lookup"><span data-stu-id="424e9-211">*Unconnected AAC encoder*</span></span>

<span data-ttu-id="424e9-212">Teď je nekompatibility: při více než pravděpodobně hello vstupní soubor média bude mít dva různé nekomprimovaným zvuk datového proudu k dispozici je pouze jeden nekomprimované zvuk vstupní pin z hello AAC kodér: jeden pro hello zbývajících zvuk kanál a jeden pro hello práva.</span><span class="sxs-lookup"><span data-stu-id="424e9-212">Now there's an incompatibility: there's only a single uncompressed audio input pin from hello AAC Encoder while more than likely hello Media File Input will have two different uncompressed audio stream available: one for hello left audio channel and one for hello right.</span></span> <span data-ttu-id="424e9-213">(V případě, že pracujete s prostorový zvuk, který je 6 kanálů.) Proto není možné připojit toodirectly hello zvuk z hello vstupní soubor média zdroj do hello AAC zvuk kodér.</span><span class="sxs-lookup"><span data-stu-id="424e9-213">(If you're dealing with surround sound, that's 6 channels.) So it's not possible toodirectly connect hello audio from hello Media File Input source into hello AAC audio encoder.</span></span> <span data-ttu-id="424e9-214">Hello AAC součást očekává takzvané "prokládaná" zvuk datový proud: jeden datový proud, který má oba hello v levém horním a hello správné kanály prokládaný mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="424e9-214">hello AAC component expects a so-called "interleaved" audio stream: a single stream that has both hello left and hello right channels interleaved with each other.</span></span> <span data-ttu-id="424e9-215">Jakmile víme z našich zdrojový soubor média audio stopy na jaké pozice ve zdroji hello, můžete se vygeneruje taková prokládaná zvuk datový proud s hello správně přiřazené mluvčího pozice pro doleva a doprava.</span><span class="sxs-lookup"><span data-stu-id="424e9-215">Once we know from our source media file which audio tracks are on what position in hello source, we can generate such interleaved audio stream with hello correctly assigned speaker positions for left and right.</span></span>

<span data-ttu-id="424e9-216">Nejdřív jednu bude chtít toogenerated prokládaná datový proud z hello požadované zdroj zvukové kanály.</span><span class="sxs-lookup"><span data-stu-id="424e9-216">First one will want toogenerated an interleaved stream from hello required source audio channels.</span></span> <span data-ttu-id="424e9-217">Abychom to bude zpracovávat Hello Interleaver zvuk datového proudu součást.</span><span class="sxs-lookup"><span data-stu-id="424e9-217">hello Audio Stream Interleaver component will handle this for us.</span></span> <span data-ttu-id="424e9-218">Přidejte ji toohello pracovního postupu a připojte hello zvuk výstupy z hello vstupní soubor média do ní.</span><span class="sxs-lookup"><span data-stu-id="424e9-218">Add it toohello workflow and connect hello audio outputs from hello Media File Input into it.</span></span>

![Připojení Interleaver zvukový datový proud](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

<span data-ttu-id="424e9-220">*Připojení Interleaver zvukový datový proud*</span><span class="sxs-lookup"><span data-stu-id="424e9-220">*Connected Audio Stream Interleaver*</span></span>

<span data-ttu-id="424e9-221">Teď, když máme prokládaná zvukový datový proud, jsme ještě neurčili kde tooassign hello doleva nebo doprava mluvčího umisťuje k.</span><span class="sxs-lookup"><span data-stu-id="424e9-221">Now that we have an interleaved audio stream, we still didn't specify where tooassign hello left or right speaker positions to.</span></span> <span data-ttu-id="424e9-222">V pořadí toospecify to, jsme můžete využít hello přidělující mluvčího pozice uživatel.</span><span class="sxs-lookup"><span data-stu-id="424e9-222">In order toospecify this, we can leverage hello Speaker Position Assigner.</span></span>

![Přidání přidělující uživatel mluvčího pozice](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

<span data-ttu-id="424e9-224">*Přidání přidělující uživatel mluvčího pozice*</span><span class="sxs-lookup"><span data-stu-id="424e9-224">*Adding a Speaker Position Assigner*</span></span>

<span data-ttu-id="424e9-225">Nakonfigurujte hello přidělující mluvčího pozice uživatel pro použití s stereo vstupního datového proudu prostřednictvím filtr přednastavení kodér z "Vlastní" a hello kanál přednastavení názvem "2.0 (L, R)".</span><span class="sxs-lookup"><span data-stu-id="424e9-225">Configure hello Speaker Position Assigner for use with a stereo input stream through an Encoder Preset Filter of "Custom" and hello Channel Preset called "2.0 (L,R)".</span></span> <span data-ttu-id="424e9-226">(To přiřadí hello levém mluvčího pozice toochannel 1 a hello správné mluvčího pozice toochannel 2.)</span><span class="sxs-lookup"><span data-stu-id="424e9-226">(This will assign hello left speaker position toochannel 1 and hello right speaker position toochannel 2.)</span></span>

<span data-ttu-id="424e9-227">Připojte hello výstup hello přidělující mluvčího pozice uživatel toohello vstup z hello AAC kodér.</span><span class="sxs-lookup"><span data-stu-id="424e9-227">Connect hello output of hello Speaker Position Assigner toohello input of hello AAC Encoder.</span></span> <span data-ttu-id="424e9-228">Potom je nutné určit hello toowork kodér AAC s "2.0 (L, R)" kanál předvolby, aby věděl, že může toodeal s stereofonní zvuk jako vstup.</span><span class="sxs-lookup"><span data-stu-id="424e9-228">Then, tell hello AAC Encoder toowork with a "2.0 (L,R)" Channel Preset, so it knows toodeal with stereo audio as input.</span></span>

### <span data-ttu-id="424e9-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexní Audio a Video datových proudů do kontejner MP4</span><span class="sxs-lookup"><span data-stu-id="424e9-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexing Audio and Video streams into an MP4 container</span></span>
<span data-ttu-id="424e9-230">Zadaný naše AVC kódovaného datový proud videa a naše AAC kódovaný zvukový datový proud, jsme můžete zaznamenat do. Kontejner MP4.</span><span class="sxs-lookup"><span data-stu-id="424e9-230">Given our AVC encoded video stream and our AAC encoded audio stream, we can capture both into an .MP4 container.</span></span> <span data-ttu-id="424e9-231">kombinování různých datových proudů v jeden Hello proces se nazývá "multiplexní" (nebo "muxing").</span><span class="sxs-lookup"><span data-stu-id="424e9-231">hello process of mixing different streams into a single one is called "multiplexing" (or "muxing").</span></span> <span data-ttu-id="424e9-232">V takovém případě jsme se prokládání hello zvuk a video datové proudy hello v jediném souvislé. Balíček MP4.</span><span class="sxs-lookup"><span data-stu-id="424e9-232">In this case we're interleaving hello audio and hello video streams in a single coherent .MP4 package.</span></span> <span data-ttu-id="424e9-233">Hello komponenty, která koordinuje to pro. Kontejner MP4 se nazývá hello multiplexor ISO MPEG-4.</span><span class="sxs-lookup"><span data-stu-id="424e9-233">hello component that coordinates this for an .MP4 container is called hello ISO MPEG-4 Multiplexer.</span></span> <span data-ttu-id="424e9-234">Přidejte jeden plochu návrháře toohello a připojit hello AVC kodér videa a hello AAC kodér tooits vstupy.</span><span class="sxs-lookup"><span data-stu-id="424e9-234">Add one toohello designer surface and connect both hello AVC Video Encoder and hello AAC Encoder tooits inputs.</span></span>

![Připojené MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

<span data-ttu-id="424e9-236">*Připojené MPEG4 multiplexor*</span><span class="sxs-lookup"><span data-stu-id="424e9-236">*Connected MPEG4 Multiplexer*</span></span>

### <span data-ttu-id="424e9-237"><a id="MXF_to_MP4_writing_mp4"></a>Zapisování souboru MP4 hello</span><span class="sxs-lookup"><span data-stu-id="424e9-237"><a id="MXF_to_MP4_writing_mp4"></a>Writing hello MP4 file</span></span>
<span data-ttu-id="424e9-238">Při zápisu výstupního souboru, se používá hello výstup souboru součásti.</span><span class="sxs-lookup"><span data-stu-id="424e9-238">When writing an output file, hello File Output component is used.</span></span> <span data-ttu-id="424e9-239">Jsme se můžete připojit tento toohello výstup hello ISO MPEG-4 multiplexor tak, aby získá jeho výstup zapsán toodisk.</span><span class="sxs-lookup"><span data-stu-id="424e9-239">We can connect this toohello output of hello ISO MPEG-4 Multiplexer so that its output gets written toodisk.</span></span> <span data-ttu-id="424e9-240">toodo tohoto připojení hello kontejneru (MPEG-4) výstupní kód pin toohello zápisu vstupní pin hello výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-240">toodo this, connect hello Container (MPEG-4) output pin toohello Write input pin of hello File Output.</span></span>

![Připojení výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

<span data-ttu-id="424e9-242">*Připojení výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="424e9-242">*Connected File Output*</span></span>

<span data-ttu-id="424e9-243">Hello název souboru, který bude použit je určen podle hello vlastnosti souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-243">hello filename that will be used is determined by hello File property.</span></span> <span data-ttu-id="424e9-244">Tuto vlastnost mohou být pevně zakódované tooa zadána hodnota, bude pravděpodobně jeden má tooset přes výraz místo.</span><span class="sxs-lookup"><span data-stu-id="424e9-244">While that property can be hardcoded tooa given value, most likely one will want tooset it through an expression instead.</span></span>

<span data-ttu-id="424e9-245">pracovní postup hello toohave automaticky určit výstup hello souboru název vlastnosti z výrazu, klikněte na název další toohello souboru hello tlačítka (ikona další toohello složku).</span><span class="sxs-lookup"><span data-stu-id="424e9-245">toohave hello workflow automatically determine hello output File name property from an expression, click hello buton next toohello File name (next toohello folder icon).</span></span> <span data-ttu-id="424e9-246">Z hello rozevírací nabídce a vyberte položku "Výraz".</span><span class="sxs-lookup"><span data-stu-id="424e9-246">From hello drop down menu then select "Expression".</span></span> <span data-ttu-id="424e9-247">Tím se otevře editor výrazů hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-247">This will bring up hello expression editor.</span></span> <span data-ttu-id="424e9-248">Nejprve vymažte obsah hello hello editoru.</span><span class="sxs-lookup"><span data-stu-id="424e9-248">Clear hello contents of hello editor first.</span></span>

![Prázdný výraz editoru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

<span data-ttu-id="424e9-250">*Prázdný výraz editoru*</span><span class="sxs-lookup"><span data-stu-id="424e9-250">*Empty Expression Editor*</span></span>

<span data-ttu-id="424e9-251">Hello výraz editor umožňuje tooenter žádné literálovou hodnotou, smíšený s minimálně jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="424e9-251">hello expression editor allows tooenter any literal value, mixed with one or more variables.</span></span> <span data-ttu-id="424e9-252">Proměnné začínat znak dolaru.</span><span class="sxs-lookup"><span data-stu-id="424e9-252">Variables start with a dollar sign.</span></span> <span data-ttu-id="424e9-253">Jako zásahu hello $ klíč hello editor zobrazí rozevírací pole s volbou dostupným proměnným.</span><span class="sxs-lookup"><span data-stu-id="424e9-253">As you hit hello $ key, hello editor will show a dropdown box with a choice of available variables.</span></span> <span data-ttu-id="424e9-254">V našem případě použijeme kombinaci hello výstupní adresář proměnnou a hello vstupní soubor základní název proměnné:</span><span class="sxs-lookup"><span data-stu-id="424e9-254">In our case we'll use a combination of hello output directory variable and hello base input file name variable:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Naplní se Editor výrazů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

<span data-ttu-id="424e9-256">*Naplní se Editor výrazů*</span><span class="sxs-lookup"><span data-stu-id="424e9-256">*Filled out Expression Editor*</span></span>

> [!NOTE]
> <span data-ttu-id="424e9-257">V pořadí toosee najdete výstupního souboru kódování úlohy v Azure, je nutné zadat hodnotu v editoru výraz hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-257">In order toosee see an output file of your encoding job in Azure, you must provide a value in hello expression editor.</span></span>
>
>

<span data-ttu-id="424e9-258">Jakmile potvrdíte hello výraz zasažení ok, bude okně Vlastnosti hello v tuto chvíli náhled toowhat hodnota hello souboru vlastnost vyřeší.</span><span class="sxs-lookup"><span data-stu-id="424e9-258">When you confirm hello expression by hitting ok, hello property window will preview toowhat value hello File property resolves at this point in time.</span></span>

![Výraz souboru přeloží dir výstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

<span data-ttu-id="424e9-260">*Výraz souboru přeloží dir výstup*</span><span class="sxs-lookup"><span data-stu-id="424e9-260">*File Expression resolves output dir*</span></span>

### <span data-ttu-id="424e9-261"><a id="MXF_to_MP4_asset_from_output"></a>Vytváření Asset Media Services z hello výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="424e9-261"><a id="MXF_to_MP4_asset_from_output"></a>Creating a Media Services Asset from hello output file</span></span>
<span data-ttu-id="424e9-262">Když jsme napsali výstupní soubor MP4, potřebujeme ještě tooindicate, že tento soubor patří toohello výstupní asset, který služba media services bude generovat v důsledku spuštění tento pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="424e9-262">While we have written an MP4 output file, we still need tooindicate that this file belongs toohello output asset which media services will generate as a result of executing this workflow.</span></span> <span data-ttu-id="424e9-263">toothis koncové hello výstupní soubor nebo Asset uzlu na plátně hello pracovní postup se používá.</span><span class="sxs-lookup"><span data-stu-id="424e9-263">toothis end, hello Output File/Asset node on hello workflow canvas is used.</span></span> <span data-ttu-id="424e9-264">Všechny příchozí soubory do tohoto uzlu bude součástí Azure Media Services asset výsledné hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-264">All incoming files into this node will make part of hello resulting Azure Media Services asset.</span></span>

<span data-ttu-id="424e9-265">Připojte hello výstup souboru součást toohello výstupní soubor nebo Asset součást toofinish pracovního postupu hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-265">Connect hello File Output component toohello Output File/Asset component toofinish hello workflow.</span></span>

![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

<span data-ttu-id="424e9-267">*Dokončení pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-267">*Finished Workflow*</span></span>

### <span data-ttu-id="424e9-268"><a id="MXF_to_MP4_test"></a>Test hello dokončení pracovního postupu místně</span><span class="sxs-lookup"><span data-stu-id="424e9-268"><a id="MXF_to_MP4_test"></a>Test hello finished workflow locally</span></span>
<span data-ttu-id="424e9-269">pracovní postup hello tootest místně, stiskněte tlačítko play hello v hello nástrojů v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-269">tootest hello workflow locally, hit hello play button in hello toolbar at hello top.</span></span> <span data-ttu-id="424e9-270">Po dokončení provádění hello pracovního postupu zkontrolujte výstup hello generovaný v hello nakonfigurované výstupní složce.</span><span class="sxs-lookup"><span data-stu-id="424e9-270">When hello workflow finished executing, inspect hello output generated in hello configured output folder.</span></span> <span data-ttu-id="424e9-271">Uvidíte, že hello dokončení MP4 výstupního souboru, který byl zakódován z hello MXF vstupní zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="424e9-271">You'll see hello finished MP4 output file that was encoded from hello MXF input source file.</span></span>

## <span data-ttu-id="424e9-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Kódování MXF do MP4 - multibitrate povolené dynamické balení</span><span class="sxs-lookup"><span data-stu-id="424e9-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Encoding MXF into MP4 - multibitrate dynamic packaging enabled</span></span>
<span data-ttu-id="424e9-273">V tomto návodu vytvoříme sadu souborů MP4 s více přenosovou rychlostí s kódováním AAC zvuk z jedné. MXF vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="424e9-273">In this walkthrough we'll create a set of multiple bitrate MP4 files with AAC encoded audio from a single .MXF input file.</span></span>

<span data-ttu-id="424e9-274">Když výstupní asset více přenosovými rychlostmi požadované pro použití v kombinaci s funkce dynamické balení hello služeb Azure Media Services, více souborů MP4 zarovnaný GOP jednotlivých různých přenosovou rychlostí a řešení bude nutné toobe vygenerovat.</span><span class="sxs-lookup"><span data-stu-id="424e9-274">When a multi-bitrate asset output is desired for use in combination with hello Dynamic Packaging features offered by Azure Media Services, multiple GOP-aligned MP4 files of each a different bitrate and resolution will need toobe generated.</span></span> <span data-ttu-id="424e9-275">Ano, hello toodo [kódování MXF do MP4 s jednou přenosovou rychlostí](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) návod poskytuje nám to dobrý výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="424e9-275">toodo so, hello [Encoding MXF into a single bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) walkthrough provides us with a good starting point.</span></span>

![Spuštění pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

<span data-ttu-id="424e9-277">*Spuštění pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-277">*Starting Workflow*</span></span>

### <span data-ttu-id="424e9-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Přidání další jeden nebo více MP4 výstupy</span><span class="sxs-lookup"><span data-stu-id="424e9-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Adding one or more additional MP4 outputs</span></span>
<span data-ttu-id="424e9-279">Každý soubor MP4 v našem výsledné asset Azure Media Services bude podporovat různé přenosovou rychlostí a řešení.</span><span class="sxs-lookup"><span data-stu-id="424e9-279">Every MP4 file in our resulting Azure Media Services asset will support a different bitrate and resolution.</span></span> <span data-ttu-id="424e9-280">Umožňuje přidat jeden nebo více MP4 výstupní soubory toohello pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="424e9-280">Let's add one or more MP4 output files toohello workflow.</span></span>

<span data-ttu-id="424e9-281">toomake, že máme všechny naše video kodéry vytvořené pomocí hello stejné nastavení, nejpohodlnější tooduplicate hello stávající kodér videa AVC a nakonfigurovat jiné kombinace překlad IP adres a přenosovou rychlostí (přidejme mezi 960 x 540 na 25 snímků za sekundu na 2,5 MB/s).</span><span class="sxs-lookup"><span data-stu-id="424e9-281">toomake sure we have all our video encoders created with hello same settings, it's most convenient tooduplicate hello already existing AVC Video Encoder and configure another combination of resolution and bitrate (let's add one of 960 x 540 at 25 frames per second at 2,5 Mbps).</span></span> <span data-ttu-id="424e9-282">existující encoder hello tooduplicate kopírování vložte jej na plochu návrháře hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-282">tooduplicate hello existing encoder, copy paste it on hello designer surface.</span></span>

<span data-ttu-id="424e9-283">Připojte hello nekomprimované Video výstup hello vstupní soubor média do naší nové komponenty AVC pin.</span><span class="sxs-lookup"><span data-stu-id="424e9-283">Connect hello Uncompressed Video output pin of hello Media File Input into our new AVC component.</span></span>

![Druhý AVC kodér připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

<span data-ttu-id="424e9-285">*Druhý AVC kodér připojení*</span><span class="sxs-lookup"><span data-stu-id="424e9-285">*Second AVC encoder connected*</span></span>

<span data-ttu-id="424e9-286">Nyní přizpůsobíte hello konfigurace pro naše nové AVC kodér toooutput 960 x 540 2,5 MB/s.</span><span class="sxs-lookup"><span data-stu-id="424e9-286">Now adapt hello configuration for our new AVC encoder toooutput 960x540 at 2,5 Mbps.</span></span> <span data-ttu-id="424e9-287">(Použijte jeho vlastnosti "výstup šířka", "Výstup výšky" a "Přenosovou rychlostí (kbps)" pro tuto.)</span><span class="sxs-lookup"><span data-stu-id="424e9-287">(Use its properties "Output width", "Output height", and "Bitrate (kbps)" for this.)</span></span>

<span data-ttu-id="424e9-288">Zadané chceme toouse hello výsledné asset společně s Azure Media Services dynamické balení, hello koncový bod streamování musí toobe schopný vytvářet z těchto souborů MP4 s fragmenty HLS nebo fragmentovaný soubor MP4/DASH, které jsou právě zarovnaný tooeach/jiných způsobem že klienti, kteří jsou přepínání mezi různých přenosových rychlostí získat jediného smooth průběžné videa a zvuku rozhraní.</span><span class="sxs-lookup"><span data-stu-id="424e9-288">Given we want toouse hello resulting asset together with Azure Media Services' dynamic packaging, hello streaming endpoint needs toobe capable of generating from these MP4 files HLS/Fragmented MP4/DASH fragments that are exactly aligned tooeach other in a way that clients that are switching between different bitrates get a single smooth continuous video and audio experience.</span></span> <span data-ttu-id="424e9-289">toomake toho docílit, potřebujeme tooensure, že v hello vlastnosti obou AVC kodéry hello velikost GOP ("skupina obrázky") pro oba soubory MP4 je nastavený too2 sekund, což lze provést:</span><span class="sxs-lookup"><span data-stu-id="424e9-289">toomake that happen, we need tooensure that, in hello properties of both AVC encoders hello GOP ("group of pictures") size for both MP4 files is set too2 seconds, which can be done by:</span></span>

* <span data-ttu-id="424e9-290">nastavení velikosti GOP tooFixed GOP velikost režimu hello a</span><span class="sxs-lookup"><span data-stu-id="424e9-290">setting hello GOP Size Mode tooFixed GOP size and</span></span>
* <span data-ttu-id="424e9-291">Hello klíč rámce Interval tootwo (sekundy).</span><span class="sxs-lookup"><span data-stu-id="424e9-291">hello Key Frame Interval tootwo seconds.</span></span>
* <span data-ttu-id="424e9-292">také nastavte hello řízení IDR GOP tooClosed GOP tooensure, které jsou všechny GOP stálého na své vlastní bez závislosti</span><span class="sxs-lookup"><span data-stu-id="424e9-292">also set hello GOP IDR Control tooClosed GOP tooensure all GOP's are standing on their own without dependencies</span></span>

<span data-ttu-id="424e9-293">toomake naše pohodlný toounderstand pracovního postupu, přejmenujte hello první AVC kodér příliš "kodér videa AVC 640 x 360 1200 kb/s" a hello druhý kodér AVC "kodér videa AVC 960 x 540 2 500 kb/s".</span><span class="sxs-lookup"><span data-stu-id="424e9-293">toomake our workflow convenient toounderstand, rename hello first AVC encoder too"AVC Video Encoder 640x360 1200kbps" and hello second AVC encoder "AVC Video Encoder 960x540 2500 kbps".</span></span>

<span data-ttu-id="424e9-294">Přidáte druhý multiplexor ISO MPEG-4 a druhý výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-294">Now add a second ISO MPEG-4 Multiplexer and a second File Output.</span></span> <span data-ttu-id="424e9-295">Připojte hello multiplexor toohello nové AVC kodér a ujistěte se, že její výstup se přesměruje do hello výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-295">Connect hello multiplexer toohello new AVC encoder and make sure its output is directed into hello File Output.</span></span> <span data-ttu-id="424e9-296">Také připojit hello AAC zvuk kodér výstup toohello nové multiplexor na vstup.</span><span class="sxs-lookup"><span data-stu-id="424e9-296">Then also connect hello AAC audio encoder output toohello new multiplexer's input.</span></span> <span data-ttu-id="424e9-297">Hello výstup souboru zase pak může být připojené toohello výstupní soubor nebo Asset uzlu tooadd ho toohello Media Services Asset, který bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="424e9-297">hello File Output in turn can then be connected toohello Output File/Asset node tooadd it toohello Media Services Asset that will be created.</span></span>

![Druhý multiplexor a výstup souboru připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

<span data-ttu-id="424e9-299">*Druhý multiplexor a výstup souboru připojení*</span><span class="sxs-lookup"><span data-stu-id="424e9-299">*Second Muxer and File Output connected*</span></span>

<span data-ttu-id="424e9-300">Zajištění kompatibility se službou Azure Media Services dynamické balení nakonfigurujte počet tooGOP hello multiplexor na režimu bloku nebo doba trvání a nastavte hello GOPs za too1 bloku.</span><span class="sxs-lookup"><span data-stu-id="424e9-300">For compatibility with Azure Media Services dynamic packaging, configure hello multiplexer's Chunk Mode tooGOP count or duration and set hello GOPs per chunk too1.</span></span> <span data-ttu-id="424e9-301">(To by měl být výchozí hello.)</span><span class="sxs-lookup"><span data-stu-id="424e9-301">(This should be hello default.)</span></span>

![Režimy multiplexor bloku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

<span data-ttu-id="424e9-303">*Režimy multiplexor bloku*</span><span class="sxs-lookup"><span data-stu-id="424e9-303">*Muxer Chunk Modes*</span></span>

<span data-ttu-id="424e9-304">Poznámka: může být vhodné toorepeat tento proces pro další přenosovou rychlostí a řešení, že chcete toohave kombinace přidal výstup toohello asset.</span><span class="sxs-lookup"><span data-stu-id="424e9-304">Note: you may want toorepeat this process for any other bitrate and resolution combinations you want toohave added toohello asset output.</span></span>

### <span data-ttu-id="424e9-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurace názvy výstupního souboru hello</span><span class="sxs-lookup"><span data-stu-id="424e9-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Configuring hello file output names</span></span>
<span data-ttu-id="424e9-306">Máme více než jeden jediný soubor přidané toohello výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="424e9-306">We have more than one single file added toohello output asset.</span></span> <span data-ttu-id="424e9-307">To poskytuje jistotu hello nutné toomake, které názvy souborů pro každou hello výstupní soubory se liší od sebe navzájem a možná i použít zadávání názvů tak, že se z názvu souboru hello co že pracujete s.</span><span class="sxs-lookup"><span data-stu-id="424e9-307">This provides a need toomake sure hello filenames for each of hello output files are different from each other and maybe even apply a file-naming convention so it becomes clear from hello file name what you're dealing with.</span></span>

<span data-ttu-id="424e9-308">Názvy výstupního souboru se dá řídit přes výrazy v Návrháři hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-308">File output naming can be controlled through expressions in hello designer.</span></span> <span data-ttu-id="424e9-309">Otevřete podokno hello vlastnost pro jednu ze součástí výstup souboru hello a otevřete editor hello výraz pro hello vlastnost souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-309">Open hello property pane for one of hello File Output components and open hello expression editor for hello File property.</span></span> <span data-ttu-id="424e9-310">Naše první výstupní soubor byl nakonfigurovaný pomocí hello následující výraz (Zobrazit hello kurz pro přechod z [MXF tooa jednou přenosovou rychlostí MP4 výstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span><span class="sxs-lookup"><span data-stu-id="424e9-310">Our first output file was configured through hello following expression (see hello tutorial for going from [MXF tooa single bitrate MP4 output](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

<span data-ttu-id="424e9-311">To znamená, že naše filename je dáno dvou proměnných: hello výstupní adresář toowrite v a hello základní název zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-311">This means that our filename is determined by two variables: hello output directory toowrite in and hello source file base name.</span></span> <span data-ttu-id="424e9-312">dřívějším Hello je k dispozici jako vlastnost v kořenovém adresáři hello pracovního postupu a pozdější hello je dáno hello příchozího souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-312">hello former is exposed as a property on hello workflow root and hello latter is determined by hello incoming file.</span></span> <span data-ttu-id="424e9-313">Všimněte si, že hello výstupního adresáře je použít pro místní testování; Tato vlastnost bude přepsáno modul pracovních postupů hello při spuštění pracovního postupu hello procesorem hello médium založené na cloudu ve službě Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="424e9-313">Note that hello output directory is what you use for local testing; this property will be overridden by hello workflow engine when hello workflow is executed by hello cloud-based media processor in Azure Media Services.</span></span>
<span data-ttu-id="424e9-314">toogive i naše výstupní soubory konzistentní výstup pojmenování, změna hello pojmenování výraz, který se nejprve souboru:</span><span class="sxs-lookup"><span data-stu-id="424e9-314">toogive both our output files a consistent output naming, change hello first file naming expression to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="424e9-315">a druhý hello na:</span><span class="sxs-lookup"><span data-stu-id="424e9-315">and hello second to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="424e9-316">Spusťte test zprostředkující spustit toomake, že oba výstupní soubory MP4 správně generují.</span><span class="sxs-lookup"><span data-stu-id="424e9-316">Execute an intermediate test run toomake sure both MP4 output files are properly generated.</span></span>

### <span data-ttu-id="424e9-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Přidání samostatných sledovat zvuk</span><span class="sxs-lookup"><span data-stu-id="424e9-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Adding a separate Audio Track</span></span>
<span data-ttu-id="424e9-318">Jako ukážeme později při jsme generovat toogo soubor .ism s naše výstupní soubory MP4, bude také vyžadujeme pouze zvukový soubor MP4 jako hello zvuk sledování pro naše adaptivní streamování.</span><span class="sxs-lookup"><span data-stu-id="424e9-318">As we'll see later when we generate an .ism file toogo with our MP4 output files, we will also require a audio-only MP4 file as hello audio track for our adaptive streaming.</span></span> <span data-ttu-id="424e9-319">toocreate to souboru, přidání pracovního postupu další multiplexor toohello (multiplexor ISO-MPEG-4) a připojte pin výstup hello AAC kodér s jeho vstupní PIN kódu pro sledování 1.</span><span class="sxs-lookup"><span data-stu-id="424e9-319">toocreate this file, add an additional muxer toohello workflow (ISO-MPEG-4 Multiplexer) and connect hello AAC encoder's output pin with its input pin for Track 1.</span></span>

![Zvuk multiplexor přidán](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

<span data-ttu-id="424e9-321">*Zvuk multiplexor přidán*</span><span class="sxs-lookup"><span data-stu-id="424e9-321">*Audio Muxer Added*</span></span>

<span data-ttu-id="424e9-322">Vytvoření výstupního souboru součást toooutput hello odchozí proudu třetí z hello multiplexor a nakonfigurujte výraz pojmenování souboru hello:</span><span class="sxs-lookup"><span data-stu-id="424e9-322">Create a third File Output component toooutput hello outbound stream from hello muxer and configure hello file naming expression as:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Zvuk multiplexor vytvoření výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

<span data-ttu-id="424e9-324">*Zvuk multiplexor vytvoření výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="424e9-324">*Audio Muxer creating File Output*</span></span>

### <span data-ttu-id="424e9-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Přidání hello. Soubor SMIL ISM</span><span class="sxs-lookup"><span data-stu-id="424e9-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Adding hello .ISM SMIL File</span></span>
<span data-ttu-id="424e9-326">Hello dynamické balení toowork v kombinaci s obou MP4 soubory (a hello pouze MP4) v našem asset Media Services je také třeba soubor manifestu (také označovaný jako soubor "SMIL": synchronizované multimédia integrace jazyka).</span><span class="sxs-lookup"><span data-stu-id="424e9-326">For hello dynamic packaging toowork in combination with both MP4 files (and hello audio-only MP4) in our Media Services asset, we also need a manifest file (also called a "SMIL" file: Synchronized Multimedia Integration Language).</span></span> <span data-ttu-id="424e9-327">Tento soubor označuje tooAzure Media Services, jaké soubory MP4 jsou dostupné pro dynamické balení a které z těchto tooconsider pro hello zvuk streamování.</span><span class="sxs-lookup"><span data-stu-id="424e9-327">This file indicates tooAzure Media Services what MP4 files are available for dynamic packaging and which of those tooconsider for hello audio streaming.</span></span> <span data-ttu-id="424e9-328">Typické souboru manifestu sady na MP4 s jeden datový proud zvuku vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="424e9-328">A typical manifest file for a set of MP4's with a single audio stream looks like this:</span></span>

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

<span data-ttu-id="424e9-329">soubor .ism Hello obsahuje v rámci příkazu switch, odkaz tooeach hello jednotlivých MP4 videosouborů a kromě toho zvukový soubor toothose také jeden (nebo více) odkazuje na tooan MP4, obsahující pouze zvuk hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-329">hello .ism file contains within a switch statement, a reference tooeach of hello individual MP4 video files and in addition toothose also one (or more) audio file references tooan MP4 that only contains hello audio.</span></span>

<span data-ttu-id="424e9-330">Generování souboru manifestu hello pro naše sadu na MP4 lze provést prostřednictvím komponenty s názvem hello "AMS Manifest zapisovače".</span><span class="sxs-lookup"><span data-stu-id="424e9-330">Generating hello manifest file for our set of MP4's can be done through a component called hello "AMS Manifest Writer".</span></span> <span data-ttu-id="424e9-331">toouse, přetáhněte ji na hello prostor a připojte PIN výstup "Zápis úplná" hello z hello tři výstup souboru součásti toohello AMS Manifest zapisovače vstup.</span><span class="sxs-lookup"><span data-stu-id="424e9-331">toouse it, drag it onto hello surface and connect hello "Write Complete" output pins from hello three File Output components toohello AMS Manifest Writer input.</span></span> <span data-ttu-id="424e9-332">Potom zkontrolujte, zda výstup hello tooconnect hello AMS Manifest zapisovače toohello výstupní soubor nebo Asset.</span><span class="sxs-lookup"><span data-stu-id="424e9-332">Then make sure tooconnect hello output of hello AMS Manifest Writer toohello Output File/Asset.</span></span>

<span data-ttu-id="424e9-333">Stejně jako u našich souboru výstup součásti, nakonfigurujte název výstupního souboru hello .ism s výrazem:</span><span class="sxs-lookup"><span data-stu-id="424e9-333">As with our other file output components, configure hello .ism file output name with an expression:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

<span data-ttu-id="424e9-334">Naše dokončení pracovního postupu vypadá hello níže:</span><span class="sxs-lookup"><span data-stu-id="424e9-334">Our finished workflow looks like hello below:</span></span>

![Dokončení MXF toomultibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

<span data-ttu-id="424e9-336">*Dokončení MXF toomultibitrate MP4 pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-336">*Finished MXF toomultibitrate MP4 workflow*</span></span>

## <span data-ttu-id="424e9-337"><a id="MXF_to__multibitrate_MP4"></a>Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého</span><span class="sxs-lookup"><span data-stu-id="424e9-337"><a id="MXF_to__multibitrate_MP4"></a>Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>
<span data-ttu-id="424e9-338">V hello [předchozího návodu pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) jste viděli, jak jeden vstupní asset MXF, mohou být převedeny na výstupní asset s souborů MP4 s více přenosovými rychlostmi, pouze zvukový soubor MP4 a soubor manifestu pro použití ve spojení s Azure média Dynamické balení služby.</span><span class="sxs-lookup"><span data-stu-id="424e9-338">In hello [previous workflow walkthrough](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we've seen how a single MXF input asset can be converted into an output asset with multi-bitrate MP4 files, an audio-only MP4 file and a manifest file for use in conjunction with Azure Media Services dynamic packaging.</span></span>

<span data-ttu-id="424e9-339">Tento názorný postup se zobrazí, jak některé aspekty hello jdou vylepšit a provedené pohodlnější.</span><span class="sxs-lookup"><span data-stu-id="424e9-339">This walkthrough will show how some of hello aspects can be enhanced and made more convenient.</span></span>

### <span data-ttu-id="424e9-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Přehled tooenhance pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Workflow overview tooenhance</span></span>
![Tooenhance Multibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

<span data-ttu-id="424e9-342">*Tooenhance Multibitrate MP4 pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-342">*Multibitrate MP4 workflow tooenhance*</span></span>

### <span data-ttu-id="424e9-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>Konvence pro pojmenování souborů</span><span class="sxs-lookup"><span data-stu-id="424e9-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>File Naming Conventions</span></span>
<span data-ttu-id="424e9-344">V předchozím postupu hello jsme jednoduchý výraz zadaný jako hello základ pro generování názvy výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="424e9-344">In hello previous workflow we specified a simple expression as hello basis for generating output file names.</span></span> <span data-ttu-id="424e9-345">Když máme některé duplikace: všech součástí souboru jednotlivých výstup hello hello zadaný takové výraz.</span><span class="sxs-lookup"><span data-stu-id="424e9-345">We have some duplication though: all of hello hello individual output file components specified such expression.</span></span>

<span data-ttu-id="424e9-346">Například naše součást výstupní soubor pro první videosoubor hello nakonfigurovaná s tímto výrazem:</span><span class="sxs-lookup"><span data-stu-id="424e9-346">For example, our file output component for hello first video file is configured with this expression:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="424e9-347">Když pro hello druhý výstup video, máme výraz jako:</span><span class="sxs-lookup"><span data-stu-id="424e9-347">While for hello second output video, we have an expression like:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="424e9-348">Nebylo by čisticí méně chyba náchylné k chybám a pohodlnější, pokud jsme může odeberte některé z této duplikace a ujistěte se, co konfigurovatelnější místo?</span><span class="sxs-lookup"><span data-stu-id="424e9-348">Wouldn't it be cleaner, less error prone and more convenient if we could remove some of this duplication and make things more configurable instead?</span></span> <span data-ttu-id="424e9-349">Naštěstí můžeme: možnosti výraz hello návrháře v kombinaci s hello možnost toocreate vlastní vlastnosti v kořenovém adresáři naše pracovního postupu nám umožní úroveň pohodlí.</span><span class="sxs-lookup"><span data-stu-id="424e9-349">Luckily we can: hello designer's expression capabilities in combination with hello ability toocreate custom properties on our workflow root will give us an added layer of convenience.</span></span>

<span data-ttu-id="424e9-350">Předpokládejme, že jsme budete jednotka filename konfigurace z přenosových rychlostí hello hello jednotlivých souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="424e9-350">Let's assume we'll drive filename configuration from hello bitrates of hello individual MP4 files.</span></span> <span data-ttu-id="424e9-351">Tyto přenosových rychlostí usilujeme budete tooconfigure v jednoho centrálního umístit (na hello root naše grafu), z kterých byly používaná tooconfigure a jednotku generování názvů souborů.</span><span class="sxs-lookup"><span data-stu-id="424e9-351">These bitrates we'll aim tooconfigure in one central place (on hello root of our graph), from where they'll be accessed tooconfigure and drive file name generation.</span></span> <span data-ttu-id="424e9-352">toodo toho jsme spustit tím, že publikujete hello přenosovou rychlostí vlastnost z obou AVC kodéry toohello kořenové naše pracovního postupu, takže bude přístupný z obou hello kořenového také kvůli hello AVC kodérů.</span><span class="sxs-lookup"><span data-stu-id="424e9-352">toodo this, we start by publishing hello bitrate property from both AVC encoders toohello root of our workflow, so that it becomes accessible from both hello root as well as from hello AVC encoders.</span></span> <span data-ttu-id="424e9-353">(I když se zobrazí v dva různé body, se jenom jedna nadřazená hodnota.)</span><span class="sxs-lookup"><span data-stu-id="424e9-353">(Even if displayed in two different spots, there's only one underlying value.)</span></span>

### <span data-ttu-id="424e9-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publikování vlastnosti komponenty do kořenové hello pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publishing component properties onto hello workflow root</span></span>
<span data-ttu-id="424e9-355">Otevřete hello první AVC kodér, přejděte vlastnost toohello přenosovou rychlostí (kbps) a z rozevíracího seznamu hello Volba možnosti publikovat.</span><span class="sxs-lookup"><span data-stu-id="424e9-355">Open hello first AVC encoder, go toohello Bitrate (kbps) property and from hello dropdown choose Publish.</span></span>

![Publikování vlastnost hello přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

<span data-ttu-id="424e9-357">*Publikování vlastnost hello přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="424e9-357">*Publishing hello bitrate property*</span></span>

<span data-ttu-id="424e9-358">Konfigurace hello publikování dialogové okno kořenové toopublish toohello naše pracovního postupu grafu s názvem "video1bitrate" a publikovaná a čitelné zobrazovaný název "1 Video přenosovou rychlostí".</span><span class="sxs-lookup"><span data-stu-id="424e9-358">Configure hello publish dialog toopublish toohello root of our workflow graph, with a published name of "video1bitrate" and a readable display name of "Video 1 Bitrate".</span></span> <span data-ttu-id="424e9-359">Nakonfigurovat vlastní název skupiny označuje "Streamování přenosových rychlostí" a stiskněte tlačítko Publikovat.</span><span class="sxs-lookup"><span data-stu-id="424e9-359">Configure a custom group name called "Streaming Bitrates" and hit Publish.</span></span>

![Publikování vlastnost hello přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

<span data-ttu-id="424e9-361">*Dialogové okno Vlastnosti přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="424e9-361">*Publishing dialog for bitrate property*</span></span>

<span data-ttu-id="424e9-362">Opakování hello stejné pro vlastnost přenosovou rychlostí hello hello druhé AVC kodér a pojmenujte ji "video2bitrate" s názvem "2 Video přenosovou rychlostí" a zobrazení, v hello stejné vlastní skupiny "Streamování přenosových rychlostí".</span><span class="sxs-lookup"><span data-stu-id="424e9-362">Repeat hello same for hello bitrate property of hello second AVC encoder and name it "video2bitrate" with a display name of "Video 2 Bitrate", in hello same custom group "Streaming Bitrates".</span></span>

<span data-ttu-id="424e9-363">Pokud jsme teď prověřit vlastnosti kořenové hello pracovního postupu, uvidíme náš vlastní skupiny s hello, které se zobrazují dvě vlastnosti publikovaných.</span><span class="sxs-lookup"><span data-stu-id="424e9-363">If we now inspect hello workflow root properties, we'll see our custom group with hello two published properties show up.</span></span> <span data-ttu-id="424e9-364">Obě jsou odrážející hello hodnotu jejich odpovídajících přenosovou rychlostí AVC kodér.</span><span class="sxs-lookup"><span data-stu-id="424e9-364">Both are reflecting hello value of their respective AVC encoder bitrate.</span></span>

![Props publikované přenosovou rychlostí v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

<span data-ttu-id="424e9-366">Vždy, když chceme tooaccess tyto vlastnosti z kódu nebo z výrazu, jsme tak postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="424e9-366">Whenever we want tooaccess these properties from code or from an expression, we can do so like this:</span></span>

* <span data-ttu-id="424e9-367">z vloženého kódu z komponenty vpravo pod kořenovou hello: node.getPropertyAsString('.. / video1bitrate ", null)</span><span class="sxs-lookup"><span data-stu-id="424e9-367">from inline code from a component right below hello root: node.getPropertyAsString('../video1bitrate',null)</span></span>
* <span data-ttu-id="424e9-368">ve výrazu: ${ROOT_video1bitrate}</span><span class="sxs-lookup"><span data-stu-id="424e9-368">within an expression: ${ROOT_video1bitrate}</span></span>

<span data-ttu-id="424e9-369">Umožňuje dokončit skupiny "Streamování přenosových rychlostí" hello pomocí publikování naše zvuk sledovat přenosovou rychlostí na něm také.</span><span class="sxs-lookup"><span data-stu-id="424e9-369">Let's complete hello "Streaming Bitrates" group by publishing our audio track bitrate on it as well.</span></span> <span data-ttu-id="424e9-370">V rámci hello vlastnosti hello AAC kodér vyhledejte hello přenosovou rychlostí nastavení a vyberte možnost publikovat další tooit hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="424e9-370">Within hello properties of hello AAC Encoder, search for hello Bitrate setting and select Publish from hello dropdown next tooit.</span></span> <span data-ttu-id="424e9-371">Publikování toohello kořenové hello grafu s názvem "audio1bitrate" a zobrazované jméno "1 zvuk přenosovou rychlostí" v rámci naší vlastní skupiny "Streamování přenosových rychlostí".</span><span class="sxs-lookup"><span data-stu-id="424e9-371">Publish toohello root of hello graph with name "audio1bitrate" and display name "Audio 1 Bitrate" within our custom group "Streaming Bitrates".</span></span>

![Dialogové okno pro zvuk přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

<span data-ttu-id="424e9-373">*Dialogové okno pro zvuk přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="424e9-373">*Publishing dialog for audio bitrate*</span></span>

![Výsledný videa a zvuku props v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

<span data-ttu-id="424e9-375">*Výsledný videa a zvuku props v kořenovém adresáři*</span><span class="sxs-lookup"><span data-stu-id="424e9-375">*Resulting video and audio props on root*</span></span>

<span data-ttu-id="424e9-376">Upozorňujeme, že některé z těchto tří změna hodnoty také znovu nakonfiguruje a hodnoty na hello příslušné součásti jsou propojeny s hello změny (a případně pomocí publikovat).</span><span class="sxs-lookup"><span data-stu-id="424e9-376">Note that changing any of those three values also re-configures and changes hello values on hello respective components they are linked with (and where published from).</span></span>

### <span data-ttu-id="424e9-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="424e9-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Have generated output file names rely on published property values</span></span>
<span data-ttu-id="424e9-378">Místo hardcoding naše názvy generovaný soubor jsme nyní můžete měnit naše filename výraz na všech hello výstup souboru součásti toorely na hello přenosovou rychlostí vlastnosti, které jsme právě publikovaný v kořenovém adresáři hello grafu.</span><span class="sxs-lookup"><span data-stu-id="424e9-378">Instead of hardcoding our generated file names, we can now change our filename expression on each of hello File Output components toorely on hello bitrate properties we just published on hello graph root.</span></span> <span data-ttu-id="424e9-379">Počínaje naše první výstupního souboru, najít vlastnost souboru hello a upravit výraz hello takto:</span><span class="sxs-lookup"><span data-stu-id="424e9-379">Starting with our first file output, find hello File property and edit hello expression like this:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

<span data-ttu-id="424e9-380">Hello různé parametry v tomto výrazu můžete získat přístup a zadá stiskne hello dolaru na klávesnici hello při v okně výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-380">hello different parameters in this expression can be accessed and entered by hitting hello dollar sign on hello keyboard while in hello expression window.</span></span> <span data-ttu-id="424e9-381">Jeden z dostupných parametrů hello je naše video1bitrate vlastnosti, které jsme dříve publikované.</span><span class="sxs-lookup"><span data-stu-id="424e9-381">One of hello available parameters is our video1bitrate property which we published earlier.</span></span>

![Přístup k parametry v rámci výrazu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

<span data-ttu-id="424e9-383">*Přístup k parametry v rámci výrazu*</span><span class="sxs-lookup"><span data-stu-id="424e9-383">*Accessing parameters within an expression*</span></span>

<span data-ttu-id="424e9-384">Hello stejné pro výstup hello souboru pro naše druhý video:</span><span class="sxs-lookup"><span data-stu-id="424e9-384">Do hello same for hello file output for our second video:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

<span data-ttu-id="424e9-385">a pro výstup hello pouze zvukový soubor:</span><span class="sxs-lookup"><span data-stu-id="424e9-385">and for hello audio-only file output:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

<span data-ttu-id="424e9-386">Pokud nyní Změníme hello přenosovou rychlostí v žádném z hello video nebo zvuk soubory, bude třeba překonfigurovat příslušných kodér hello a konvence název souboru na základě přenosovou rychlostí hello bude dodržení všechny automatické.</span><span class="sxs-lookup"><span data-stu-id="424e9-386">If we now change hello bitrate for any of hello video or audio files, hello respective encoder will be reconfigured and hello bitrate-based file name convention will be honored all automatic.</span></span>

## <span data-ttu-id="424e9-387"><a id="thumbnails_to__multibitrate_MP4"></a>Přidává se výstup MP4 toomultibitrate miniatur</span><span class="sxs-lookup"><span data-stu-id="424e9-387"><a id="thumbnails_to__multibitrate_MP4"></a>Adding thumbnails toomultibitrate MP4 output</span></span>
<span data-ttu-id="424e9-388">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do přidává se výstup toohello miniatur.</span><span class="sxs-lookup"><span data-stu-id="424e9-388">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into adding thumbnails toohello output.</span></span>

### <span data-ttu-id="424e9-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Miniatury tooadd přehled pracovního postupu pro</span><span class="sxs-lookup"><span data-stu-id="424e9-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Workflow overview tooadd thumbnails to</span></span>
![Pracovní postup toostart Multibitrate MP4 z](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

<span data-ttu-id="424e9-391">*Pracovní postup toostart Multibitrate MP4 z*</span><span class="sxs-lookup"><span data-stu-id="424e9-391">*Multibitrate MP4 workflow toostart from*</span></span>

### <span data-ttu-id="424e9-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Přidání JPG kódování</span><span class="sxs-lookup"><span data-stu-id="424e9-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Adding JPG Encoding</span></span>
<span data-ttu-id="424e9-393">Hello srdcem naše miniatur generování bude hello JPG kodér součást možné toooutput JPG soubory.</span><span class="sxs-lookup"><span data-stu-id="424e9-393">hello heart of our thumbnail generation will be hello JPG Encoder component, able toooutput JPG files.</span></span>

![Kodér JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

<span data-ttu-id="424e9-395">*Kodér JPG*</span><span class="sxs-lookup"><span data-stu-id="424e9-395">*JPG Encoder*</span></span>

<span data-ttu-id="424e9-396">Naše nekomprimované Video datového proudu nelze připojit ale přímo z hello vstupní soubor média do kodér JPG hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-396">We cannot however directly connect our Uncompressed Video stream from hello Media File Input into hello JPG encoder.</span></span> <span data-ttu-id="424e9-397">Místo toho očekává, že toobe předat jednotlivé snímky.</span><span class="sxs-lookup"><span data-stu-id="424e9-397">Instead, it expects toobe handed individual frames.</span></span> <span data-ttu-id="424e9-398">To, provedeme prostřednictvím brány rámce Video komponent hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-398">This, we can do through hello Video Frame Gate component.</span></span>

![Připojení kodér JPG toohello brány rámečkem](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

<span data-ttu-id="424e9-400">*Připojení kodér JPG toohello brány rámečkem*</span><span class="sxs-lookup"><span data-stu-id="424e9-400">*Connecting a frame gate toohello JPG encoder*</span></span>

<span data-ttu-id="424e9-401">Hello rámce brány po každé mnoho sekund nebo rámce umožňuje toopass snímku videa.</span><span class="sxs-lookup"><span data-stu-id="424e9-401">hello frame gate once every so many seconds or frames allows a video frame toopass.</span></span> <span data-ttu-id="424e9-402">Hello intervalu a hello časového posunu s který tomu je možné konfigurovat ve vlastnostech hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-402">hello interval and hello time offset with which this happens is configurable in hello properties.</span></span>

![Vlastnosti videa rámce brány](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

<span data-ttu-id="424e9-404">*Vlastnosti videa rámce brány*</span><span class="sxs-lookup"><span data-stu-id="424e9-404">*Video Frame Gate properties*</span></span>

<span data-ttu-id="424e9-405">Umožňuje vytvořit miniaturu každou minutu nastavením tooTime hello režimu (v sekundách) a hello too60 intervalu.</span><span class="sxs-lookup"><span data-stu-id="424e9-405">Let's create a thumbnail every minute by setting hello mode tooTime (seconds) and hello Interval too60.</span></span>

### <span data-ttu-id="424e9-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Práci s barevný prostor převod</span><span class="sxs-lookup"><span data-stu-id="424e9-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Dealing with Color Space conversion</span></span>
<span data-ttu-id="424e9-407">Při jeví jako logické jak nekomprimované Video PIN hello rámce brány a hello vstupní soubor média lze nyní připojit, jsme byste získali upozornění, když jsme by učinit.</span><span class="sxs-lookup"><span data-stu-id="424e9-407">While it would seem logical both Uncompressed Video pins of hello frame gate and hello Media File Input can now be connected, we would get a warning if we would do so.</span></span>

![Vstupní barva místo chyby](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

<span data-ttu-id="424e9-409">*Vstupní barva místo chyby*</span><span class="sxs-lookup"><span data-stu-id="424e9-409">*Input color space error*</span></span>

<span data-ttu-id="424e9-410">Je to proto hello způsob barvy, která je reprezentována informace v našem původní nezpracovaná nekomprimované datový proud videa, pocházejících z našich MXF se liší od jaké hello očekává kodér JPG.</span><span class="sxs-lookup"><span data-stu-id="424e9-410">This is because hello way in which colour information is represented in our original raw uncompressed video stream, coming from our MXF, is different from what hello JPG Encoder is expecting.</span></span> <span data-ttu-id="424e9-411">Více konkrétně takzvané "barvu místo" "RGB" nebo "Ve stupních šedi" je očekáván tooflow v.</span><span class="sxs-lookup"><span data-stu-id="424e9-411">More specifically, a so-called "color space" of "RGB" or "Grayscale" is expected tooflow in.</span></span> <span data-ttu-id="424e9-412">To znamená, že hello Video rámce brány na datový proud videa příchozí potřebovat toohave převodu z týkající se jeho barevný prostor použije první.</span><span class="sxs-lookup"><span data-stu-id="424e9-412">This means that hello Video Frame Gate's inbound video stream will need toohave a conversion applied regarding its color space first.</span></span>

<span data-ttu-id="424e9-413">Přetáhněte na hello pracovního postupu hello barva místo převaděč - Intel a připojte ho tooour rámce brány.</span><span class="sxs-lookup"><span data-stu-id="424e9-413">Drag onto hello workflow hello Color Space Converter - Intel and connect it tooour frame gate.</span></span>

![Připojení úpravy místo barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

<span data-ttu-id="424e9-415">*Připojení úpravy místo barev*</span><span class="sxs-lookup"><span data-stu-id="424e9-415">*Connecting a Color Space Convertor*</span></span>

<span data-ttu-id="424e9-416">V okně vlastností hello vyberte hello BGR 24 položky ze seznamu přednastavených hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-416">In hello properties window, pick hello BGR 24 entry from hello Preset list.</span></span>

### <span data-ttu-id="424e9-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Zápis hello miniatur</span><span class="sxs-lookup"><span data-stu-id="424e9-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Writing hello thumbnails</span></span>
<span data-ttu-id="424e9-418">Liší od našich video MP4, hello JPG kodér výstup součástí více než jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="424e9-418">Different from our MP4 video's, hello JPG Encoder component will output more than one file.</span></span> <span data-ttu-id="424e9-419">V pořadí toodeal s tím, mohou být použity komponentu scény vyhledávání JPG souboru zapisovače: bude trvat hello příchozí miniatur JPG a zapsat je, každý název souboru se na konci pomocí jiné číslo.</span><span class="sxs-lookup"><span data-stu-id="424e9-419">In order toodeal with this, a Scene Search JPG File Writer component can be used: it will take hello incoming JPG thumbnails and write them out, each filename being suffixed by a different number.</span></span> <span data-ttu-id="424e9-420">(číslo hello obvykle určující hello počet sekund/jednotky v hello datový proud, který hello miniaturu byl čerpají z).</span><span class="sxs-lookup"><span data-stu-id="424e9-420">(hello number typically indicating hello number of seconds/units in hello stream which hello thumbnail was drawn from.)</span></span>

![Představení hello scény vyhledávání JPG souboru zapisovače](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

<span data-ttu-id="424e9-422">*Představení hello scény vyhledávání JPG souboru zapisovače*</span><span class="sxs-lookup"><span data-stu-id="424e9-422">*Introducing hello Scene Search JPG File Writer*</span></span>

<span data-ttu-id="424e9-423">Nastavení vlastnosti hello výstupní cesta ke složce s výrazem hello: ${ROOT_outputWriteDirectory}</span><span class="sxs-lookup"><span data-stu-id="424e9-423">Configure hello Output Folder Path property with hello expression: ${ROOT_outputWriteDirectory}</span></span>

<span data-ttu-id="424e9-424">a vlastnost Filename předponu s hello:</span><span class="sxs-lookup"><span data-stu-id="424e9-424">and hello Filename Prefix property with:</span></span>

    ${ROOT_sourceFileBaseName}_thumb_

<span data-ttu-id="424e9-425">Předpona Hello určí, jak jsou právě hello miniatur soubory pojmenovány.</span><span class="sxs-lookup"><span data-stu-id="424e9-425">hello prefix will determine how hello thumbnail files are being named.</span></span> <span data-ttu-id="424e9-426">Budou se měly na konci číslo naznačuje hello pozici v datovém proudu hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-426">They will be suffixed with a number indicating hello thumb's position in hello stream.</span></span>

![Vlastnosti scény zapisovače souboru JPG vyhledávání](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

<span data-ttu-id="424e9-428">*Vlastnosti scény zapisovače souboru JPG vyhledávání*</span><span class="sxs-lookup"><span data-stu-id="424e9-428">*Scene Search JPG File Writer properties*</span></span>

<span data-ttu-id="424e9-429">Připojte hello scény vyhledávání JPG souboru zapisovače toohello výstupní soubor nebo Asset uzlu.</span><span class="sxs-lookup"><span data-stu-id="424e9-429">Connect hello Scene Search JPG File Writer toohello Output File/Asset node.</span></span>

### <span data-ttu-id="424e9-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Zjištění chyb v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Detecting errors in a workflow</span></span>
<span data-ttu-id="424e9-431">Připojte hello vstup hello barva místo převaděč toohello nezpracovaná nekomprimované video výstupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-431">Connect hello input of hello color space converter toohello raw uncompressed video output.</span></span> <span data-ttu-id="424e9-432">Nyní testu místní spuštění pro pracovní postup hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-432">Now perform a local test run for hello workflow.</span></span> <span data-ttu-id="424e9-433">Je pracovní postup hello šance najednou zastavit provádění a znamenat s červenou outline hello komponenty došlo k chybě:</span><span class="sxs-lookup"><span data-stu-id="424e9-433">There's a good chance hello workflow will suddenly stop executing and indicate with a red outline on hello component that encountered an error:</span></span>

![Chyba místo převaděč barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

<span data-ttu-id="424e9-435">*Chyba místo převaděč barev*</span><span class="sxs-lookup"><span data-stu-id="424e9-435">*Color Space Converter error*</span></span>

<span data-ttu-id="424e9-436">Klepněte na ikonu "E" hello trochu red v hello pravém horním rohu hello barva místo převaděč součást toosee co je důvod hello hello kódování pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="424e9-436">Click hello little red "E" icon in hello top right corner of hello Color Space Converter component toosee what's hello reason hello encoding attempt failed.</span></span>

![Barva místo převaděč chybové dialogové okno](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

<span data-ttu-id="424e9-438">*Barva místo převaděč chybové dialogové okno*</span><span class="sxs-lookup"><span data-stu-id="424e9-438">*Color Space Converter error dialog*</span></span>

<span data-ttu-id="424e9-439">Zjistíte, jak můžete vidět, že má hello příchozí barevný prostor standard pro hello barva místo převaděč toobe rec601 pro naše požadovaný převod YUV tooRGB.</span><span class="sxs-lookup"><span data-stu-id="424e9-439">It turns out, as you can see, that hello incoming color space standard for hello color space converter has toobe rec601 for our requested conversion of YUV tooRGB.</span></span> <span data-ttu-id="424e9-440">Naše datový proud nepodporuje zjevně znamenat rec601 jeho.</span><span class="sxs-lookup"><span data-stu-id="424e9-440">Apparently our stream doesn't indicate it's rec601.</span></span> <span data-ttu-id="424e9-441">(Dop. 601 je standard pro kódování prokládaných analogovým video signály v digitální podobě videa.</span><span class="sxs-lookup"><span data-stu-id="424e9-441">(Rec 601 is a standard for encoding interlaced analog video signals in digital video form.</span></span> <span data-ttu-id="424e9-442">Určuje oblast active pokrývajících 720 světlostí a 360 chrominance vzorků na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="424e9-442">It specifies an active region covering 720 luminance samples and 360 chrominance samples per line.</span></span> <span data-ttu-id="424e9-443">Barva Hello kódování systému se označuje jako YCbCr 4:2:2.)</span><span class="sxs-lookup"><span data-stu-id="424e9-443">hello color encoding system is known as YCbCr 4:2:2.)</span></span>

<span data-ttu-id="424e9-444">toofix, budete jsme uvedli na hello metadata z našich datový proud, který jsme pracujete s rec601 obsah.</span><span class="sxs-lookup"><span data-stu-id="424e9-444">toofix this, we'll indicate on hello metadata of our stream that we're dealing with rec601 content.</span></span> <span data-ttu-id="424e9-445">toodo proto použijeme Video datový typ aktualizační součásti, kterou jsme budete put mezi naše nezpracovaná zdroje a hello barva místo součást převodu.</span><span class="sxs-lookup"><span data-stu-id="424e9-445">toodo so we'll use a Video Data Type Updater component, which we'll put in between our raw source and hello color space conversion component.</span></span> <span data-ttu-id="424e9-446">Tento datový typ aktualizační umožňuje hello ruční aktualizace určité video dat vlastnosti typu.</span><span class="sxs-lookup"><span data-stu-id="424e9-446">This data type updater allows for hello manual update of certain video data type properties.</span></span> <span data-ttu-id="424e9-447">Nakonfigurujte tooindicate místo barva standardní z "Rec 601".</span><span class="sxs-lookup"><span data-stu-id="424e9-447">Configure it tooindicate a Color Space Standard of "Rec 601".</span></span> <span data-ttu-id="424e9-448">To způsobí hello Video datový typ aktualizační tootag hello datového proudu s hello "Rec 601" barevný prostor Pokud se žádné barevný prostor ještě definice.</span><span class="sxs-lookup"><span data-stu-id="424e9-448">This will cause hello Video Data Type Updater tootag hello stream with hello "Rec 601" color space if there was no color space defined yet.</span></span> <span data-ttu-id="424e9-449">(Ho nebude přepsat všechny existující metadata, pokud byla zaškrtnutá políčka přepsání hello.)</span><span class="sxs-lookup"><span data-stu-id="424e9-449">(It will not override any existing metadata, unless hello Override checkbox was checked.)</span></span>

![Aktualizace barva místo standardní na hello aktualizační typ dat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

<span data-ttu-id="424e9-451">*Aktualizace barva místo standardní na hello aktualizační typ dat*</span><span class="sxs-lookup"><span data-stu-id="424e9-451">*Updating Color Space Standard on hello Data Type Updater*</span></span>

### <span data-ttu-id="424e9-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Finished Workflow</span></span>
<span data-ttu-id="424e9-453">Teď, když naše dokončení naše pracovního postupu, proveďte jiného testu toosee jej předat.</span><span class="sxs-lookup"><span data-stu-id="424e9-453">Now that our our workflow is finished, do another test run toosee it pass.</span></span>

![Dokončení pracovního postupu pro výstup více mp4 s miniaturami](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

<span data-ttu-id="424e9-455">*Dokončení pracovního postupu pro výstup více mp4 s miniaturami*</span><span class="sxs-lookup"><span data-stu-id="424e9-455">*Finished workflow for multi-mp4 output with thumbnails*</span></span>

## <span data-ttu-id="424e9-456"><a id="time_based_trim"></a>Oříznutí založené na čase multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="424e9-456"><a id="time_based_trim"></a>Time-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="424e9-457">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání video hello zdroje podle časová razítka.</span><span class="sxs-lookup"><span data-stu-id="424e9-457">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on time-stamps.</span></span>

### <span data-ttu-id="424e9-458"><a id="time_based_trim_start"></a>Pracovní postup přehled toostart oříznutí k přidání</span><span class="sxs-lookup"><span data-stu-id="424e9-458"><a id="time_based_trim_start"></a>Workflow overview toostart adding trimming to</span></span>
![Počáteční oříznutí tooadd pracovního postupu pro](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

<span data-ttu-id="424e9-460">*Počáteční oříznutí tooadd pracovního postupu pro*</span><span class="sxs-lookup"><span data-stu-id="424e9-460">*Starting workflow tooadd trimming to*</span></span>

### <span data-ttu-id="424e9-461"><a id="time_based_trim_use_stream_trimmer"></a>Pomocí hello oříznutí datového proudu</span><span class="sxs-lookup"><span data-stu-id="424e9-461"><a id="time_based_trim_use_stream_trimmer"></a>Using hello Stream Trimmer</span></span>
<span data-ttu-id="424e9-462">Oříznutí datového proudu součást Hello umožňuje tootrim hello začátek a konec vstupního datového proudu založit na informace o časování (sekund, minut,...). oříznutí Hello nepodporuje na základě rámce oříznutí.</span><span class="sxs-lookup"><span data-stu-id="424e9-462">hello Stream Trimmer component allows tootrim hello beginning and ending of an input stream base on timing information (seconds, minutes, ...). hello trimmer does not support frame-based trimming.</span></span>

![Oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

<span data-ttu-id="424e9-464">*Oříznutí datového proudu*</span><span class="sxs-lookup"><span data-stu-id="424e9-464">*Stream Trimmer*</span></span>

<span data-ttu-id="424e9-465">Místo přímo propojení hello AVC kodéry a mluvčího pozice přidělující uživatel toohello vstupní soubor média, jsme budete uveďte mezi tyto oříznutí hello datového proudu.</span><span class="sxs-lookup"><span data-stu-id="424e9-465">Instead of linking hello AVC encoders and speaker position assigner toohello Media File Input directly, we'll put in between those hello stream trimmer.</span></span> <span data-ttu-id="424e9-466">(Jeden pro hello signál video a jeden pro hello prokládaná zvukový signál.)</span><span class="sxs-lookup"><span data-stu-id="424e9-466">(One for hello video signal and one for hello interleaved audio signal.)</span></span>

![V rozmezí PUT oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

<span data-ttu-id="424e9-468">*V rozmezí PUT oříznutí datového proudu*</span><span class="sxs-lookup"><span data-stu-id="424e9-468">*Put Stream Trimmer in between*</span></span>

<span data-ttu-id="424e9-469">Umožňuje nakonfigurovat hello oříznutí tak, aby jsme pouze zpracovat videa a zvuku mezi 15 sekund a 60 sekund v hello video.</span><span class="sxs-lookup"><span data-stu-id="424e9-469">Let's configure hello trimmer so that we will only process video and audio between 15 seconds and 60 seconds in hello video.</span></span>

<span data-ttu-id="424e9-470">Přejděte toohello vlastnosti hello oříznutí datový proud videa a nakonfigurovat čas spuštění (15s) a vlastnosti čas ukončení (60s).</span><span class="sxs-lookup"><span data-stu-id="424e9-470">Go toohello properties of hello Video Stream Trimmer and configure both Start Time (15s) and End Time (60s) properties.</span></span> <span data-ttu-id="424e9-471">toomake se, že jak naše audio a video oříznutí jsou vždy nakonfigurované toohello stejné začínat a končit hodnoty, budeme publikovat těchto toohello kořenové hello pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-471">toomake sure both our audio and video trimmer are always configured toohello same start and end values, we will publish those toohello root of hello workflow.</span></span>

![Počáteční čas vlastnost z datového proudu oříznutí publikování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

<span data-ttu-id="424e9-473">*Počáteční čas vlastnost z datového proudu oříznutí publikování*</span><span class="sxs-lookup"><span data-stu-id="424e9-473">*Publish start time property from Stream Trimmer*</span></span>

![Publikování dialogové okno vlastností pro spuštění](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

<span data-ttu-id="424e9-475">*Publikování dialogové okno vlastností pro spuštění*</span><span class="sxs-lookup"><span data-stu-id="424e9-475">*Publish property dialog for start time*</span></span>

![Publikování dialogové okno vlastností pro koncový čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

<span data-ttu-id="424e9-477">*Publikování dialogové okno vlastností pro koncový čas*</span><span class="sxs-lookup"><span data-stu-id="424e9-477">*Publish property dialog for end time*</span></span>

<span data-ttu-id="424e9-478">Pokud jsme teď prověřit hello kořenové naše pracovního postupu, bude obě vlastnosti zobrazit a konfigurovat přehledně odtud.</span><span class="sxs-lookup"><span data-stu-id="424e9-478">If we now inspect hello root of our workflow, both properties will be neatly displayed and configurable from there.</span></span>

![Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

<span data-ttu-id="424e9-480">*Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři*</span><span class="sxs-lookup"><span data-stu-id="424e9-480">*Published properties available on root*</span></span>

<span data-ttu-id="424e9-481">Nyní otevřete vlastnosti oříznutí hello z hello zvuk oříznutí a nakonfigurujte počáteční a koncový čas s výrazem, který odkazuje toohello publikovaná vlastnosti v kořenovém adresáři hello naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-481">Now open hello trimming properties from hello audio trimmer and configure both start and end times with an expression that refers toohello published properties on hello root of our workflow.</span></span>

<span data-ttu-id="424e9-482">Čas zahájení pro hello zvuk oříznutí:</span><span class="sxs-lookup"><span data-stu-id="424e9-482">For hello audio trimming start time:</span></span>

    ${ROOT_TrimmingStartTime}

<span data-ttu-id="424e9-483">a pro její koncový čas:</span><span class="sxs-lookup"><span data-stu-id="424e9-483">and for its end time:</span></span>

    ${ROOT_TrimmingEndTime}

### <span data-ttu-id="424e9-484"><a id="time_based_trim_finish"></a>Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-484"><a id="time_based_trim_finish"></a>Finished Workflow</span></span>
![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

<span data-ttu-id="424e9-486">*Dokončení pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-486">*Finished Workflow*</span></span>

## <span data-ttu-id="424e9-487"><a id="scripting"></a>Představení hello skriptování součásti</span><span class="sxs-lookup"><span data-stu-id="424e9-487"><a id="scripting"></a>Introducing hello Scripted Component</span></span>
<span data-ttu-id="424e9-488">Skriptované součásti můžete spustit libovolný skripty během fáze provádění hello naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-488">Scripted Components can execute arbitrary scripts during hello execution phases of our workflow.</span></span> <span data-ttu-id="424e9-489">Existují čtyři jiné skripty, které mohou být provedeny, každý s konkrétními vlastnostmi a jejich vlastní místo v cyklu hello pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="424e9-489">There are four different scripts that can be executed, each with specific characteristics and their own place in hello workflow life-cycle:</span></span>

* <span data-ttu-id="424e9-490">**commandScript**</span><span class="sxs-lookup"><span data-stu-id="424e9-490">**commandScript**</span></span>
* <span data-ttu-id="424e9-491">**realizeScript**</span><span class="sxs-lookup"><span data-stu-id="424e9-491">**realizeScript**</span></span>
* <span data-ttu-id="424e9-492">**processInputScript**</span><span class="sxs-lookup"><span data-stu-id="424e9-492">**processInputScript**</span></span>
* <span data-ttu-id="424e9-493">**lifeCycleScript**</span><span class="sxs-lookup"><span data-stu-id="424e9-493">**lifeCycleScript**</span></span>

<span data-ttu-id="424e9-494">Hello dokumentaci hello skriptování součást přejde podrobněji pro každou z výše uvedených hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-494">hello documentation of hello Scripted Component goes in more detail for each of hello above.</span></span> <span data-ttu-id="424e9-495">V [hello následující části](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** skriptování součást je použité tooconstruct cliplist xml na hello chodu při spuštění pracovního postupu hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-495">In [hello following section](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** scripting component is used tooconstruct a cliplist xml on hello fly when hello workflow starts.</span></span> <span data-ttu-id="424e9-496">Tento skript je volána v průběhu instalace hello součástí, která se dělá jenom jednou v jeho průběhu životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="424e9-496">This script is called during hello component setup, which happens only once in it's lifecycle.</span></span>

### <span data-ttu-id="424e9-497"><a id="scripting_hello_world"></a>Skriptování v rámci pracovního postupu: hello, world</span><span class="sxs-lookup"><span data-stu-id="424e9-497"><a id="scripting_hello_world"></a>Scripting within a workflow: hello world</span></span>
<span data-ttu-id="424e9-498">Přetáhněte součást skripty na plochu návrháře hello a přejmenujte ji (například "SetClipListXML").</span><span class="sxs-lookup"><span data-stu-id="424e9-498">Drag a Scripted Component onto hello designer surface and rename it (for example, "SetClipListXML").</span></span>

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="424e9-500">*Přidání skriptované komponenty*</span><span class="sxs-lookup"><span data-stu-id="424e9-500">*Adding a Scripted Component*</span></span>

<span data-ttu-id="424e9-501">Když si prohlédnout vlastnosti hello hello skriptování součásti, hello, které se zobrazí čtyři typy jiný skript, každý konfigurovat tooa jiný skript.</span><span class="sxs-lookup"><span data-stu-id="424e9-501">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="424e9-503">*Skriptované vlastnosti komponenty*</span><span class="sxs-lookup"><span data-stu-id="424e9-503">*Scripted Component properties*</span></span>

<span data-ttu-id="424e9-504">Zrušte hello processInputScript a otevřete editor hello pro hello realizeScript.</span><span class="sxs-lookup"><span data-stu-id="424e9-504">Clear hello processInputScript and open hello editor for hello realizeScript.</span></span> <span data-ttu-id="424e9-505">Nyní nastavení a připravené toostart skriptování.</span><span class="sxs-lookup"><span data-stu-id="424e9-505">Now we're set up and ready toostart scripting.</span></span>

<span data-ttu-id="424e9-506">Jazyk Groovy dynamicky kompilované skriptovací jazyk pro platformu Java hello, který bude mít kompatibilitu s Java.</span><span class="sxs-lookup"><span data-stu-id="424e9-506">Scripts are written in Groovy, a dynamically compiled scripting language for hello Java platform that retains compatibility with Java.</span></span> <span data-ttu-id="424e9-507">Většinu kódu v jazyce Java ve skutečnosti, je platný Groovy kód.</span><span class="sxs-lookup"><span data-stu-id="424e9-507">Actually, most Java code is valid Groovy code.</span></span>

<span data-ttu-id="424e9-508">Můžete napsat skript groovy jednoduché hello world v kontextu hello naše realizeScript.</span><span class="sxs-lookup"><span data-stu-id="424e9-508">Let's write a simple hello world groovy script in hello context of our realizeScript.</span></span> <span data-ttu-id="424e9-509">Zadejte následující hello v editoru hello:</span><span class="sxs-lookup"><span data-stu-id="424e9-509">Enter hello following in hello editor:</span></span>

    node.log("hello world");

<span data-ttu-id="424e9-510">Teď spusťte místní testovací běh.</span><span class="sxs-lookup"><span data-stu-id="424e9-510">Now execute a local test run.</span></span> <span data-ttu-id="424e9-511">Po této spustit, zkontrolujte (prostřednictvím karty systém hello na hello skriptování součásti) hello vlastnost protokoly.</span><span class="sxs-lookup"><span data-stu-id="424e9-511">After this run, inspect (through hello System tab on hello Scripted Component) hello Logs property.</span></span>

![Výstup Hello world protokolu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

<span data-ttu-id="424e9-513">*Výstup Hello world protokolu*</span><span class="sxs-lookup"><span data-stu-id="424e9-513">*Hello world log output*</span></span>

<span data-ttu-id="424e9-514">objekt uzlu Hello, který jsme volat metodu protokolu hello, odkazuje tooour aktuální uzel"" nebo hello součást, kterou jsme se skriptování v rámci.</span><span class="sxs-lookup"><span data-stu-id="424e9-514">hello node object we call hello log method on, refers tooour current "node" or hello component we're scripting within.</span></span> <span data-ttu-id="424e9-515">Všechny komponenty jako takový má hello možnost toooutput protokoluje data, k dispozici prostřednictvím systému karta hello. V takovém případě jsme výstup hello řetězcový literál "hello, world".</span><span class="sxs-lookup"><span data-stu-id="424e9-515">Every component as such has hello ability toooutput logging data, available through hello system tab. In this case, we output hello string literal "hello world".</span></span> <span data-ttu-id="424e9-516">Důležité toounderstand tady je, že to může být toobe neocenitelnou pomocí ladicí nástroj vám poskytnou přehled na ve skutečnosti je to, jaké hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="424e9-516">Important toounderstand here is that this can prove toobe an invaluable debugging tool, providing you with insight on what hello script is actually doing.</span></span>

<span data-ttu-id="424e9-517">Z našich skriptovací prostředí máme také tooproperties přístup na dalších komponentách.</span><span class="sxs-lookup"><span data-stu-id="424e9-517">From within our scripting environment, we also have access tooproperties on other components.</span></span> <span data-ttu-id="424e9-518">Zkuste provést následující:</span><span class="sxs-lookup"><span data-stu-id="424e9-518">Try this:</span></span>

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

<span data-ttu-id="424e9-519">Naše protokolu okně se zobrazí nám hello následující:</span><span class="sxs-lookup"><span data-stu-id="424e9-519">Our log window will show us hello following:</span></span>

![Výstup protokolu pro přístup k uzlu cesty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

<span data-ttu-id="424e9-521">*Výstup protokolu pro přístup k uzlu cesty*</span><span class="sxs-lookup"><span data-stu-id="424e9-521">*Log output for accessing node paths*</span></span>

## <span data-ttu-id="424e9-522"><a id="frame_based_trim"></a>Na základě rámce oříznutí multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="424e9-522"><a id="frame_based_trim"></a>Frame-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="424e9-523">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání hello zdroje videa na základě počtu rámce.</span><span class="sxs-lookup"><span data-stu-id="424e9-523">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on frame counts.</span></span>

### <span data-ttu-id="424e9-524"><a id="frame_based_trim_start"></a>Plán, podle kterého přehled toostart oříznutí k přidání</span><span class="sxs-lookup"><span data-stu-id="424e9-524"><a id="frame_based_trim_start"></a>Blueprint overview toostart adding trimming to</span></span>
![Přidání oříznutí k toostart pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

<span data-ttu-id="424e9-526">*Přidání oříznutí k toostart pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-526">*Workflow toostart adding trimming to*</span></span>

### <span data-ttu-id="424e9-527"><a id="frame_based_trim_clip_list"></a>Pomocí hello klip seznamu XML</span><span class="sxs-lookup"><span data-stu-id="424e9-527"><a id="frame_based_trim_clip_list"></a>Using hello Clip List XML</span></span>
<span data-ttu-id="424e9-528">Ve všech předchozích kurzech pracovního postupu jsme použili vstupní soubor média součást hello jako naše vstupní zdroj videa.</span><span class="sxs-lookup"><span data-stu-id="424e9-528">In all previous workflow tutorials, we used hello Media File Input component as our video input source.</span></span> <span data-ttu-id="424e9-529">Pro tento konkrétní scénář, když budeme používat hello klip seznamu zdrojovou součástí místo.</span><span class="sxs-lookup"><span data-stu-id="424e9-529">For this specific scenario though, we'll be using hello Clip List Source component instead.</span></span> <span data-ttu-id="424e9-530">Všimněte si, že nemělo by se jednat hello upřednostňovaný způsob práce; používá jenom hello zdrojového seznamu klip, pokud je skutečný důvod toodo tak (stejně jako v nástroji hello níže případu, kdy provádíme za použití funkcí oříznutí seznamu klip hello).</span><span class="sxs-lookup"><span data-stu-id="424e9-530">Note that this should not be hello preferred way of working; only use hello Clip List Source when there's a real reason toodo so (like in hello below case, where we're making use of hello clip list trimming capabilities).</span></span>

<span data-ttu-id="424e9-531">tooswitch z našich vstupní soubor média toohello klip zdrojového seznamu, přetáhněte součást zdrojového seznamu klip hello na hello návrhová plocha a připojte se uzel XML seznamu klip hello klip seznamu XML pin toohello hello Návrháře pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-531">tooswitch from our Media File Input toohello Clip List Source, drag hello Clip List Source component onto hello design surface and connect hello Clip List XML pin toohello Clip List XML node of hello workflow designer.</span></span> <span data-ttu-id="424e9-532">To by měl naplnit hello zdrojového seznamu klip s výstupní kód PIN, podle tooour vstupní video.</span><span class="sxs-lookup"><span data-stu-id="424e9-532">This should populate hello Clip List Source with output pins, according tooour input video.</span></span> <span data-ttu-id="424e9-533">Teď připojit hello nekomprimované videa a zvuku nekomprimované PIN z hello hello zdrojového seznamu klip toohello příslušných AVC kodéry a Interleaver zvuk datového proudu.</span><span class="sxs-lookup"><span data-stu-id="424e9-533">Now connect hello Uncompressed Video and Uncompressed Audio pins from hello hello Clip List Source toohello respective AVC Encoders and Audio Stream Interleaver.</span></span> <span data-ttu-id="424e9-534">Nyní odeberte hello vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="424e9-534">Now remove hello Media File Input.</span></span>

![Nahradí hello zdrojového seznamu klip hello vstupní soubor média](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

<span data-ttu-id="424e9-536">*Nahradí hello zdrojového seznamu klip hello vstupní soubor média*</span><span class="sxs-lookup"><span data-stu-id="424e9-536">*Replaced hello Media File Input with hello Clip List Source*</span></span>

<span data-ttu-id="424e9-537">Hello klip seznamu zdrojovou součástí vezme jako vstupní "seznamu XML klip".</span><span class="sxs-lookup"><span data-stu-id="424e9-537">hello Clip List Source component takes as its input a "Clip List XML".</span></span> <span data-ttu-id="424e9-538">Když vyberete hello zdrojového souboru tootest s místně, tento klip seznamu xml je automaticky vyplněna za vás.</span><span class="sxs-lookup"><span data-stu-id="424e9-538">When selecting hello source file tootest with locally, this clip list xml is auto-populated for you.</span></span>

![Automaticky vyplněna klip seznamu XML – vlastnost](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

<span data-ttu-id="424e9-540">*Automaticky vyplněna klip seznamu XML – vlastnost*</span><span class="sxs-lookup"><span data-stu-id="424e9-540">*Auto-populated Clip List XML property*</span></span>

<span data-ttu-id="424e9-541">Vyhledávání poněkud blíže toohello xml, to je, jak vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="424e9-541">Looking a bit closer toohello xml, this is how it looks like:</span></span>

![Seznam klip dialogové okno Upravit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

<span data-ttu-id="424e9-543">*Seznam klip dialogové okno Upravit*</span><span class="sxs-lookup"><span data-stu-id="424e9-543">*Edit clip list dialog*</span></span>

<span data-ttu-id="424e9-544">To ale nemusí odpovídat hello možnosti hello klip seznamu xml.</span><span class="sxs-lookup"><span data-stu-id="424e9-544">This however does not reflect hello capabilities of hello clip list xml.</span></span> <span data-ttu-id="424e9-545">Jednou z možností, které máme je tooadd element "Trim" v části hello videa a audia zdroje, jako je to:</span><span class="sxs-lookup"><span data-stu-id="424e9-545">One option we have is tooadd a "Trim" element under both hello video and audio source, like this:</span></span>

![Přidávání seznamu klip toohello uvolnění dočasné paměti – element](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

<span data-ttu-id="424e9-547">*Přidávání seznamu klip toohello uvolnění dočasné paměti – element*</span><span class="sxs-lookup"><span data-stu-id="424e9-547">*Adding a trim element toohello clip list*</span></span>

<span data-ttu-id="424e9-548">Je-li upravit hello klip seznamu xml takto výše a místní spustit test, zobrazí se hello video správně byl oříznut mezi 10 a 20 sekund za hello video.</span><span class="sxs-lookup"><span data-stu-id="424e9-548">If you modify hello clip list xml like this above and perform a local test run, you'll see hello video correctly been trimmed between 10 and 20 seconds in hello video.</span></span>

<span data-ttu-id="424e9-549">Jinak zvláštní toowhat se stane, když provedete spuštění místní, i když tato konfigurace xml velmi stejné cliplist neměli hello stejné ovlivňuje při použití v pracovní postup, který běží v Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="424e9-549">Contrary toowhat happens when you do a local run though, this very same cliplist xml would not have hello same effect when applied in a workflow that runs in Azure Media Services.</span></span> <span data-ttu-id="424e9-550">Při vygenerování Azure Premium kodér spustí, hello cliplist xml pokaždé, když znovu podle hello vstupní soubor hello kódování úlohy byl zadán.</span><span class="sxs-lookup"><span data-stu-id="424e9-550">When Azure Premium Encoder starts, hello cliplist xml is generated every time again, based on hello input file hello encoding job was given.</span></span> <span data-ttu-id="424e9-551">To znamená, že by všechny změny, které jsme provést na hello xml bohužel přepsat.</span><span class="sxs-lookup"><span data-stu-id="424e9-551">This means that any changes we do on hello xml would unfortunately be overridden.</span></span>

<span data-ttu-id="424e9-552">toocounter hello cliplist xml vymazáním při kódování úloha spuštěna, jsme můžete znovu vygenerovat ho v chodu hello právě po spuštění hello naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="424e9-552">toocounter hello cliplist xml being wiped when an encoding job is started, we can re-generate it on hello fly just after hello start of our workflow.</span></span> <span data-ttu-id="424e9-553">Prostřednictvím co se nazývá "Skriptování Component" můžete provedeny takové vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="424e9-553">Such custom actions can be taken through what is called a "Scripted Component".</span></span> <span data-ttu-id="424e9-554">Další informace najdete v tématu [Introducing hello skriptování součást](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span><span class="sxs-lookup"><span data-stu-id="424e9-554">For more information, see [Introducing hello Scripted Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span></span>

<span data-ttu-id="424e9-555">Přetáhněte součást skripty na plochu návrháře hello a přejmenujte ji příliš "SetClipListXML".</span><span class="sxs-lookup"><span data-stu-id="424e9-555">Drag a Scripted Component onto hello designer surface and rename it too"SetClipListXML".</span></span>

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="424e9-557">*Přidání skriptované komponenty*</span><span class="sxs-lookup"><span data-stu-id="424e9-557">*Adding a Scripted Component*</span></span>

<span data-ttu-id="424e9-558">Když si prohlédnout vlastnosti hello hello skriptování součásti, hello, které se zobrazí čtyři typy jiný skript, každý konfigurovat tooa jiný skript.</span><span class="sxs-lookup"><span data-stu-id="424e9-558">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="424e9-560">*Skriptované vlastnosti komponenty*</span><span class="sxs-lookup"><span data-stu-id="424e9-560">*Scripted Component properties*</span></span>

### <span data-ttu-id="424e9-561"><a id="frame_based_trim_modify_clip_list"></a>Úprava seznamu klip hello z součásti skriptování</span><span class="sxs-lookup"><span data-stu-id="424e9-561"><a id="frame_based_trim_modify_clip_list"></a>Modifying hello clip list from a Scripted Component</span></span>
<span data-ttu-id="424e9-562">Než znovu jsme může zapisovat hello cliplist xml, který je generován během spouštění pracovního postupu, budeme potřebovat toohave přístup toohello cliplist xml vlastnosti a obsah.</span><span class="sxs-lookup"><span data-stu-id="424e9-562">Before we can re-write hello cliplist xml that is generated during workflow startup, we'll need toohave access toohello cliplist xml property and contents.</span></span> <span data-ttu-id="424e9-563">Jsme, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="424e9-563">We can do so like this:</span></span>

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Příchozí klip seznamu protokolována](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

<span data-ttu-id="424e9-565">*Příchozí klip seznamu protokolována*</span><span class="sxs-lookup"><span data-stu-id="424e9-565">*Incoming clip list being logged*</span></span>

<span data-ttu-id="424e9-566">Nejdřív potřebujeme toodetermine způsob, jak z bod, který do bod, který má být tootrim hello videa.</span><span class="sxs-lookup"><span data-stu-id="424e9-566">First we need a way toodetermine from which point till which point we want tootrim hello video.</span></span> <span data-ttu-id="424e9-567">publikování tohoto uživatele pohodlný toohello méně technical hello pracovního postupu, toomake dvě vlastnosti toohello kořenové hello grafu.</span><span class="sxs-lookup"><span data-stu-id="424e9-567">toomake this convenient toohello less-technical user of hello workflow, publish two properties toohello root of hello graph.</span></span> <span data-ttu-id="424e9-568">toodo, klikněte pravým tlačítkem na plochu návrháře hello a vyberte možnost "Přidat vlastnost":</span><span class="sxs-lookup"><span data-stu-id="424e9-568">toodo this, right click hello designer surface and select "Add Property":</span></span>

* <span data-ttu-id="424e9-569">První vlastnost: "ClippingTimeStart" typu: "Časový kód"</span><span class="sxs-lookup"><span data-stu-id="424e9-569">First property: "ClippingTimeStart" of type: "TIMECODE"</span></span>
* <span data-ttu-id="424e9-570">Druhé vlastnosti: "ClippingTimeEnd" typu: "Časový kód"</span><span class="sxs-lookup"><span data-stu-id="424e9-570">Second property: "ClippingTimeEnd" of type: "TIMECODE"</span></span>

![Přidat dialogové okno vlastností pro výstřižek počáteční čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

<span data-ttu-id="424e9-572">*Přidat dialogové okno vlastností pro výstřižek počáteční čas*</span><span class="sxs-lookup"><span data-stu-id="424e9-572">*Add Property dialog for clipping start time*</span></span>

![Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

<span data-ttu-id="424e9-574">*Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="424e9-574">*Published clipping time props on workflow root*</span></span>

<span data-ttu-id="424e9-575">Nakonfigurujte obě vlastnosti tooa vhodnou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="424e9-575">Configure both properties tooa suitable value:</span></span>

![Konfigurace hello výstřižek počáteční a koncové vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

<span data-ttu-id="424e9-577">*Konfigurace hello výstřižek počáteční a koncové vlastnosti*</span><span class="sxs-lookup"><span data-stu-id="424e9-577">*Configure hello clipping start and end properties*</span></span>

<span data-ttu-id="424e9-578">Nyní z v rámci naší skriptu jsme přístup k obě vlastnosti, jako je to:</span><span class="sxs-lookup"><span data-stu-id="424e9-578">Now, from within our script, we can access both properties, like this:</span></span>

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Okno Protokol zobrazující začátku a konci výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

<span data-ttu-id="424e9-580">*Okno Protokol zobrazující začátku a konci výstřižek*</span><span class="sxs-lookup"><span data-stu-id="424e9-580">*Log window showing start and end of clipping*</span></span>

<span data-ttu-id="424e9-581">Umožňuje analyzovat hello časový kód řetězce do pohodlnější toouse formuláře, pomocí jednoduchého regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="424e9-581">Let's parse hello timecode strings into a more convenient toouse form, using a simple regular expression:</span></span>

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Okno Protokol s výstupem Analyzovaná časový kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

<span data-ttu-id="424e9-583">*Okno Protokol s výstupem Analyzovaná časový kód*</span><span class="sxs-lookup"><span data-stu-id="424e9-583">*Log window with output of parsed timecode*</span></span>

<span data-ttu-id="424e9-584">Tyto informace po ruce jsme nyní můžete upravit hello cliplist xml tooreflect hello počáteční a koncový čas pro hello potřeby rámce přesné výstřižek film hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-584">With this information at hand, we can now modify hello cliplist xml tooreflect hello start and end times for hello desired frame-accurate clipping of hello movie.</span></span>

![Oříznutí elementů tooadd kód skriptu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

<span data-ttu-id="424e9-586">*Oříznutí elementů tooadd kód skriptu*</span><span class="sxs-lookup"><span data-stu-id="424e9-586">*Script code tooadd trim elements*</span></span>

<span data-ttu-id="424e9-587">K tomu bylo potřeba prostřednictvím operace manipulace s řetězci normální.</span><span class="sxs-lookup"><span data-stu-id="424e9-587">This was done through normal string manipulation operations.</span></span> <span data-ttu-id="424e9-588">Hello výsledné změny klip seznamu xml se nezapisují zpět vlastnost clipListXML toohello v kořenovém adresáři pracovního postupu hello prostřednictvím metody "SetProperty –" hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-588">hello resulting modified clip list xml is written back toohello clipListXML property on hello workflow root through hello "setProperty" method.</span></span> <span data-ttu-id="424e9-589">okno Protokol Hello po provedení jiného testu nám by zobrazit hello následující:</span><span class="sxs-lookup"><span data-stu-id="424e9-589">hello log window after another test run would show us hello following:</span></span>

![Protokolování výsledný seznam klip hello](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

<span data-ttu-id="424e9-591">*Protokolování výsledný seznam klip hello*</span><span class="sxs-lookup"><span data-stu-id="424e9-591">*Logging hello resulting clip list*</span></span>

<span data-ttu-id="424e9-592">Proveďte testovací běh toosee, jak mají byl oříznut datové proudy videa a audia hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-592">Do a test-run toosee how hello video and audio streams have been clipped.</span></span> <span data-ttu-id="424e9-593">Jako více než jeden testovací běh s různými hodnotami pro hello oříznutí body, můžete to udělat, můžete si všimnout, že ty, nebude vzít v úvahu ale!</span><span class="sxs-lookup"><span data-stu-id="424e9-593">As you'll do more than one test-run with different values for hello trimming points, you'll notice that those will not be taken into account however!</span></span> <span data-ttu-id="424e9-594">Důvod Hello je tento hello designer, na rozdíl od hello Azure runtime, nemá přepsání hello cliplist xml každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="424e9-594">hello reason for this is that hello designer, unlike hello Azure runtime, does NOT override hello cliplist xml every run.</span></span> <span data-ttu-id="424e9-595">To znamená, že pouze hello velmi poprvé nastavíte hello a odhlašování body, způsobí, že hello xml tootransform, všech dalších hello krát, naše klauzule ochrana (pokud (clipListXML.indexOf ("<trim>") == -1)) zabrání přidáním další trim hello pracovního postupu element, pokud je již jedna nachází.</span><span class="sxs-lookup"><span data-stu-id="424e9-595">This means that only hello very first time you have set hello in and out points, will cause hello xml tootransform, all hello other times, our guard clause (if(clipListXML.indexOf("<trim>") == -1)) will prevent hello workflow from adding another trim element when there's already one present.</span></span>

<span data-ttu-id="424e9-596">toomake naše pracovního postupu pohodlný tootest místně, jsme nejlepší přidejte některé údržby kód, který kontroluje, pokud element a uvolnění dočasné paměti byl již existuje.</span><span class="sxs-lookup"><span data-stu-id="424e9-596">toomake our workflow convenient tootest locally, we best add some house-keeping code that inspects if a trim element was already present.</span></span> <span data-ttu-id="424e9-597">Pokud ano, jsme ho odebrat před pokračováním změnou hello xml s novými hodnotami hello.</span><span class="sxs-lookup"><span data-stu-id="424e9-597">If so, we can remove it before continuing by modifying hello xml with hello new values.</span></span> <span data-ttu-id="424e9-598">Místo použití manipulace prostý řetězec, je pravděpodobně bezpečnější toodo to prostřednictvím skutečné xml objektu modelu, analýze.</span><span class="sxs-lookup"><span data-stu-id="424e9-598">Rather than using plain string manipulations, it's probably safer toodo this through real xml object model parsing.</span></span>

<span data-ttu-id="424e9-599">Předtím, než když jsme můžete přidat takový kód, budeme potřebovat tooadd naše skriptu nejdříve spustit počet prohlášení importu v hello:</span><span class="sxs-lookup"><span data-stu-id="424e9-599">Before we can add such code though, we'll need tooadd a number of import statements at hello start of our script first:</span></span>

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

<span data-ttu-id="424e9-600">Potom přidáme hello vyžaduje čištění kódu:</span><span class="sxs-lookup"><span data-stu-id="424e9-600">After this, we can add hello required cleaning code:</span></span>

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

<span data-ttu-id="424e9-601">Tento kód jenom překročí hello bodu, kdy přidáme hello trim elementy toohello cliplist xml.</span><span class="sxs-lookup"><span data-stu-id="424e9-601">This code goes just above hello point at which we add hello trim elements toohello cliplist xml.</span></span>

<span data-ttu-id="424e9-602">V tuto chvíli jsme můžete spustit a změnit naše pracovní postup, jak tolik, jako chceme přitom má hello změny někdy čas.</span><span class="sxs-lookup"><span data-stu-id="424e9-602">At this point, we can run and modify our workflow as much as we want while having hello changes applied ever time.</span></span>    

### <span data-ttu-id="424e9-603"><a id="frame_based_trim_clippingenabled_prop"></a>Přidání vlastnosti ClippingEnabled usnadnění práce</span><span class="sxs-lookup"><span data-stu-id="424e9-603"><a id="frame_based_trim_clippingenabled_prop"></a>Adding a ClippingEnabled convenience property</span></span>
<span data-ttu-id="424e9-604">Chcete nemusí vždy toohappen oříznutí, můžeme Dokončit vypnout naše pracovního postupu přidáním pohodlný logický příznak, který určuje, zda chceme tooenable ořezávání / výstřižek.</span><span class="sxs-lookup"><span data-stu-id="424e9-604">As you might not always want trimming toohappen, let's finish off our workflow by adding a convenient boolean flag that indicates whether or not we want tooenable trimming / clipping.</span></span>

<span data-ttu-id="424e9-605">Stejně jako dřív, a publikovat nové kořenové toohello vlastnost naše pracovního postupu názvem "ClippingEnabled" typu "Logická hodnota".</span><span class="sxs-lookup"><span data-stu-id="424e9-605">Just as before, publish a new property toohello root of our workflow called "ClippingEnabled" of type "BOOLEAN".</span></span>

![Publikovaná vlastnost pro povolení výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

<span data-ttu-id="424e9-607">*Publikovaná vlastnost pro povolení výstřižek*</span><span class="sxs-lookup"><span data-stu-id="424e9-607">*Published a property for enabling clipping*</span></span>

<span data-ttu-id="424e9-608">S hello níže klauzule jednoduché ochrana jsme můžete zkontrolujte, zda je vyžadován oříznutí a rozhodnout, pokud naše klip seznam jako takový musí toobe upravit, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="424e9-608">With hello below simple guard clause, we can check if trimming is required and decide if our clip list as such needs toobe modified or not.</span></span>

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <span data-ttu-id="424e9-609"><a id="code"></a>Kompletní kód</span><span class="sxs-lookup"><span data-stu-id="424e9-609"><a id="code"></a>Complete code</span></span>
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a><span data-ttu-id="424e9-610">Viz také</span><span class="sxs-lookup"><span data-stu-id="424e9-610">Also see</span></span>
[<span data-ttu-id="424e9-611">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="424e9-611">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[<span data-ttu-id="424e9-612">Jak tooUse Premium kódování ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="424e9-612">How tooUse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[<span data-ttu-id="424e9-613">Kódování obsahu na vyžádání pomocí Azure Media Service</span><span class="sxs-lookup"><span data-stu-id="424e9-613">Encoding On-Demand Content with Azure Media Service</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

[<span data-ttu-id="424e9-614">Formáty a kodeky pracovního postupu Media Encoderu Premium</span><span class="sxs-lookup"><span data-stu-id="424e9-614">Media Encoder Premium Workflow Formats and Codecs</span></span>](media-services-premium-workflow-encoder-formats.md)

[<span data-ttu-id="424e9-615">Ukázkové soubory pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="424e9-615">Sample workflow files</span></span>](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[<span data-ttu-id="424e9-616">Nástroj pro Azure Media Services Explorer</span><span class="sxs-lookup"><span data-stu-id="424e9-616">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="424e9-617">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="424e9-617">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="424e9-618">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="424e9-618">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
