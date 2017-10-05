---
title: "Kurzy Avanced Media Encoder Premium pracovního postupu"
description: "Tento dokument obsahuje postupů, které ukazují, jak provádět pokročilé úlohy s pracovním postupem Premium kodér médií a také jak vytvořit komplexní pracovní postupy pomocí Návrháře pracovního postupu."
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
ms.openlocfilehash: 565497bd5a35e3c4d69d29512307cf3ca2364bdd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a><span data-ttu-id="bb0b5-103">Pokročilé kurzy Media Encoder Premium pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-103">Advanced Media Encoder Premium Workflow tutorials</span></span>
## <a name="overview"></a><span data-ttu-id="bb0b5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bb0b5-104">Overview</span></span>
<span data-ttu-id="bb0b5-105">Tento dokument obsahuje návody, které ukazují, jak přizpůsobit pracovní postupy s **Návrhář postupu provádění**.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-105">This document contains walkthroughs that show how to customize workflows with  **Workflow Designer**.</span></span> <span data-ttu-id="bb0b5-106">Můžete najít soubory skutečné pracovního postupu [zde](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-106">You can find the actual workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span></span>  

## <a name="toc"></a><span data-ttu-id="bb0b5-107">TOC</span><span class="sxs-lookup"><span data-stu-id="bb0b5-107">TOC</span></span>
<span data-ttu-id="bb0b5-108">Jsou pokryta následující témata:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-108">The following topics are covered:</span></span>

* [<span data-ttu-id="bb0b5-109">Kódování MXF do MP4 s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="bb0b5-109">Encoding MXF into a single bitrate MP4</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [<span data-ttu-id="bb0b5-110">Spuštění nového pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-110">Starting a new workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [<span data-ttu-id="bb0b5-111">Pomocí vstupní soubor média</span><span class="sxs-lookup"><span data-stu-id="bb0b5-111">Using the Media File Input</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [<span data-ttu-id="bb0b5-112">Probíhá kontrola mediální datové proudy</span><span class="sxs-lookup"><span data-stu-id="bb0b5-112">Inspecting media streams</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [<span data-ttu-id="bb0b5-113">Přidání video kodéru pro. Generování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-113">Adding a video encoder for .MP4 file generation</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [<span data-ttu-id="bb0b5-114">Kódování zvukový datový proud</span><span class="sxs-lookup"><span data-stu-id="bb0b5-114">Encoding the audio stream</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [<span data-ttu-id="bb0b5-115">Multiplexní Audio a Video datových proudů do kontejner MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-115">Multiplexing Audio and Video streams into an MP4 container</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [<span data-ttu-id="bb0b5-116">Zapisování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-116">Writing the MP4 file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [<span data-ttu-id="bb0b5-117">Vytváření Asset Media Services z výstupního souboru</span><span class="sxs-lookup"><span data-stu-id="bb0b5-117">Creating a Media Services Asset from the output file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [<span data-ttu-id="bb0b5-118">Testování místně dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-118">Test the finished workflow locally</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [<span data-ttu-id="bb0b5-119">Kódování MXF do multibitrate soubory MP4 s rychlostmi – dynamické balení povoleno</span><span class="sxs-lookup"><span data-stu-id="bb0b5-119">Encoding MXF into multibitrate MP4s - dynamic packaging enabled</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [<span data-ttu-id="bb0b5-120">Přidání další jeden nebo více MP4 výstupy</span><span class="sxs-lookup"><span data-stu-id="bb0b5-120">Adding one or more additional MP4 outputs</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [<span data-ttu-id="bb0b5-121">Konfigurace názvů výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="bb0b5-121">Configuring the file output names</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [<span data-ttu-id="bb0b5-122">Přidání samostatných sledovat zvuk</span><span class="sxs-lookup"><span data-stu-id="bb0b5-122">Adding a separate Audio Track</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [<span data-ttu-id="bb0b5-123">Přidání. Soubor SMIL ISM</span><span class="sxs-lookup"><span data-stu-id="bb0b5-123">Adding the .ISM SMIL File</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [<span data-ttu-id="bb0b5-124">Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého</span><span class="sxs-lookup"><span data-stu-id="bb0b5-124">Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [<span data-ttu-id="bb0b5-125">Přehled pracovního postupu k vylepšení</span><span class="sxs-lookup"><span data-stu-id="bb0b5-125">Workflow overview to enhance</span></span>](#workflow-overview-to-enhance)
  * [<span data-ttu-id="bb0b5-126">Konvence pro pojmenování souborů</span><span class="sxs-lookup"><span data-stu-id="bb0b5-126">File Naming Conventions</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [<span data-ttu-id="bb0b5-127">Publikování vlastnosti komponenty do kořenové pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-127">Publishing component properties onto the workflow root</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [<span data-ttu-id="bb0b5-128">Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="bb0b5-128">Have generated output file names rely on published property values</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [<span data-ttu-id="bb0b5-129">Přidání miniatury do multibitrate MP4 výstup</span><span class="sxs-lookup"><span data-stu-id="bb0b5-129">Adding thumbnails to multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [<span data-ttu-id="bb0b5-130">Přehled pracovního postupu přidat miniatury pro</span><span class="sxs-lookup"><span data-stu-id="bb0b5-130">Workflow overview to add thumbnails to</span></span>](#workflow-overview-to-add-thumbnails-to)
  * [<span data-ttu-id="bb0b5-131">Přidání JPG kódování</span><span class="sxs-lookup"><span data-stu-id="bb0b5-131">Adding JPG Encoding</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [<span data-ttu-id="bb0b5-132">Práci s barevný prostor převod</span><span class="sxs-lookup"><span data-stu-id="bb0b5-132">Dealing with Color Space conversion</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [<span data-ttu-id="bb0b5-133">Zápis miniatur</span><span class="sxs-lookup"><span data-stu-id="bb0b5-133">Writing the thumbnails</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [<span data-ttu-id="bb0b5-134">Zjištění chyb v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-134">Detecting errors in a workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [<span data-ttu-id="bb0b5-135">Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-135">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [<span data-ttu-id="bb0b5-136">Oříznutí založené na čase multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-136">Time-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [<span data-ttu-id="bb0b5-137">Přehled pracovního postupu, pokud chcete začít přidávat oříznutí na</span><span class="sxs-lookup"><span data-stu-id="bb0b5-137">Workflow overview to start adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [<span data-ttu-id="bb0b5-138">Použití oříznutí datového proudu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-138">Using the Stream Trimmer</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [<span data-ttu-id="bb0b5-139">Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-139">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [<span data-ttu-id="bb0b5-140">Představení komponentu pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-140">Introducing the Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [<span data-ttu-id="bb0b5-141">Skriptování v rámci pracovního postupu: hello, world</span><span class="sxs-lookup"><span data-stu-id="bb0b5-141">Scripting within a workflow: hello world</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [<span data-ttu-id="bb0b5-142">Na základě rámce oříznutí multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-142">Frame-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [<span data-ttu-id="bb0b5-143">Přehled plán, podle kterého chcete začít přidávat oříznutí na</span><span class="sxs-lookup"><span data-stu-id="bb0b5-143">Blueprint overview to start adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [<span data-ttu-id="bb0b5-144">Pomocí seznamu klip XML</span><span class="sxs-lookup"><span data-stu-id="bb0b5-144">Using the Clip List XML</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [<span data-ttu-id="bb0b5-145">Úprava seznamu klip z součásti skriptování</span><span class="sxs-lookup"><span data-stu-id="bb0b5-145">Modifying the clip list from a Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [<span data-ttu-id="bb0b5-146">Přidání vlastnosti ClippingEnabled usnadnění práce</span><span class="sxs-lookup"><span data-stu-id="bb0b5-146">Adding a ClippingEnabled convenience property</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <span data-ttu-id="bb0b5-147"><a id="MXF_to_MP4"></a>Kódování MXF do MP4 s jednou přenosovou rychlostí</span><span class="sxs-lookup"><span data-stu-id="bb0b5-147"><a id="MXF_to_MP4"></a>Encoding MXF into a single bitrate MP4</span></span>
<span data-ttu-id="bb0b5-148">V tomto návodu vytvoříme s jednou přenosovou rychlostí. Soubor MP4 s AAC-HE kódovaný zvuk ze. MXF vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-148">In this walkthrough we'll create a single bitrate .MP4 file with AAC-HE encoded audio from an .MXF input file.</span></span>

### <span data-ttu-id="bb0b5-149"><a id="MXF_to_MP4_start_new"></a>Spuštění nového pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-149"><a id="MXF_to_MP4_start_new"></a>Starting a new workflow</span></span>
<span data-ttu-id="bb0b5-150">Otevřete návrháře pracovních postupů a vyberte "Soubor"-"nový pracovní prostor"-"převod plán, podle kterého"</span><span class="sxs-lookup"><span data-stu-id="bb0b5-150">Open Workflow Designer and select "File"-"New Workspace"-"Transcode Blueprint"</span></span>

<span data-ttu-id="bb0b5-151">Nový pracovní postup bude zobrazovat 3 prvků:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-151">The new workflow will show 3 elements:</span></span>

* <span data-ttu-id="bb0b5-152">Primární zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="bb0b5-152">Primary Source File</span></span>
* <span data-ttu-id="bb0b5-153">Seznam klip ve formátu XML</span><span class="sxs-lookup"><span data-stu-id="bb0b5-153">Clip List XML</span></span>
* <span data-ttu-id="bb0b5-154">Výstupní soubor nebo prostředek</span><span class="sxs-lookup"><span data-stu-id="bb0b5-154">Output File/Asset</span></span>  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

<span data-ttu-id="bb0b5-156">*Nový pracovní postup kódování*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-156">*New Encoding Workflow*</span></span>

### <span data-ttu-id="bb0b5-157"><a id="MXF_to_MP4_with_file_input"></a>Pomocí vstupní soubor média</span><span class="sxs-lookup"><span data-stu-id="bb0b5-157"><a id="MXF_to_MP4_with_file_input"></a>Using the Media File Input</span></span>
<span data-ttu-id="bb0b5-158">Aby bylo možné přijímat naše vstupními médii souborů, jeden začíná přidání komponentu vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-158">In order to accept our input media file, one starts with adding a Media File Input component.</span></span> <span data-ttu-id="bb0b5-159">Chcete-li přidejte součást do pracovního postupu, podívejte se do vyhledávacího pole úložiště a přetáhněte na požadovanou položku na podokně návrháře.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-159">To add a component to the workflow, look for it in the Repository search box and drag the desired entry onto the designer pane.</span></span> <span data-ttu-id="bb0b5-160">To udělat pro vstupní soubor média a připojte komponentu primární zdrojový soubor vstupní připnete Filename ze vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-160">Do this for the Media File Input and connect the Primary Source File component to the Filename input pin from the Media File Input.</span></span>

![Připojených mediálních souborů vstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

<span data-ttu-id="bb0b5-162">*Připojených mediálních souborů vstup*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-162">*Connected Media File Input*</span></span>

<span data-ttu-id="bb0b5-163">Jsme můžete provést, budete nejprve musíme pokyn k návrháře pracovních postupů co ukázkový soubor jsme chtěli použít k návrhu naše pracovního postupu se.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-163">Before we can do much else, we'll first need to indicate to the workflow designer what sample file we'd like to use to design our workflow with.</span></span> <span data-ttu-id="bb0b5-164">Uděláte to tak, klepněte na pozadí návrháře podokně a pro vlastnost primární zdrojový soubor v podokně pravém vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-164">To do so, click the designer pane background and look for the Primary Source File property on the right-hand property pane.</span></span> <span data-ttu-id="bb0b5-165">Klikněte na ikonu složky a vyberte požadovaný soubor k testování pracovního postupu se.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-165">Click the folder icon and select the desired file to test the workflow with.</span></span> <span data-ttu-id="bb0b5-166">Jakmile to uděláte, bude komponentu vstupní soubor média zkontrolujte soubor a naplnit její výstup PIN tak, aby odrážela soubor, který ho prověřovány.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-166">As soon as this is done, the Media File Input component will inspect the file and populate its output pins to reflect the file it inspected.</span></span>

![Vstupní soubor vyplněná média](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

<span data-ttu-id="bb0b5-168">*Vstupní soubor vyplněná média*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-168">*Populated Media File Input*</span></span>

<span data-ttu-id="bb0b5-169">Když tato hodnota určuje, s jakou vstup bychom rádi pracovat, neříká ještě kde kódovaného výstupu by měli přejít na.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-169">While this specifies with what input we'd like to work with, it doesn't tell yet where the encoded output should go to.</span></span> <span data-ttu-id="bb0b5-170">Podobným způsobem primární zdrojový soubor byl nakonfigurován, teď nakonfigurovat vlastnost výstupní složky proměnné pod ním.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-170">Similar to how the Primary Source File was configured, now configure the Output Folder Variable property, just below it.</span></span>

![Nakonfigurovaný vstupní a výstupní vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

<span data-ttu-id="bb0b5-172">*Nakonfigurovaný vstupní a výstupní vlastnosti*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-172">*Configured Input and Output properties*</span></span>

### <span data-ttu-id="bb0b5-173"><a id="MXF_to_MP4_streams"></a>Probíhá kontrola mediální datové proudy</span><span class="sxs-lookup"><span data-stu-id="bb0b5-173"><a id="MXF_to_MP4_streams"></a>Inspecting media streams</span></span>
<span data-ttu-id="bb0b5-174">Často je žádoucí vědět, jak datového proudu vypadá jako je například toků v pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-174">Often it's desired to know how the stream looks like that flows through the workflow.</span></span> <span data-ttu-id="bb0b5-175">Chcete-li prověřit datový proud v libovolném bodě v pracovním postupu, stačí kliknout na výstupu nebo vstupní PIN kódu na některé z těchto komponent.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-175">To inspect a stream at any point in the workflow, just click an output or input pin on any of the components.</span></span> <span data-ttu-id="bb0b5-176">V takovém případě zkuste klepnout na pin výstup nekomprimované Video z naší vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-176">In this case, try clicking on the Uncompressed Video output pin from our Media File Input.</span></span> <span data-ttu-id="bb0b5-177">Zobrazí se dialogové okno otevřete umožňuje kontrolovat odchozí video.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-177">A dialog will open up that allows to inspect the outbound video.</span></span>

![Probíhá kontrola pin výstup nekomprimované Video](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

<span data-ttu-id="bb0b5-179">*Probíhá kontrola pin výstup nekomprimované Video*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-179">*Inspecting the Uncompressed Video output pin*</span></span>

<span data-ttu-id="bb0b5-180">V našem případě sdělí nám například jsme se zabývají 1920 × 1080 vstupu v 24 snímků za sekundu v 4:2:2 vzorkování video o téměř 2 minut.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-180">In our case, it tells us for example that we're dealing with a 1920x1080 input at 24 frames per second in 4:2:2 sampling for a video of almost 2 minutes.</span></span>

### <span data-ttu-id="bb0b5-181"><a id="MXF_to_MP4_file_generation"></a>Přidání video kodéru pro. Generování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-181"><a id="MXF_to_MP4_file_generation"></a>Adding a video encoder for .MP4 file generation</span></span>
<span data-ttu-id="bb0b5-182">Všimněte si, že nyní, nekomprimované Video a více nekomprimované zvuk výstup kódy PIN nejsou k dispozici pro použití na našich vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-182">Note that now, an Uncompressed Video and multiple Uncompressed Audio output pins are available for use on our Media File Input.</span></span> <span data-ttu-id="bb0b5-183">Pokud chcete zakódovat příchozí video, je třeba komponentu kódování – v takovém případě pro generování. Soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-183">In order to encode the inbound video, we need an encoding component - in this case for generating .MP4 files.</span></span>

<span data-ttu-id="bb0b5-184">Ke kódování datový proud videa na H.264, přidejte komponentu AVC kodér videa na plochu návrháře.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-184">To encode the video stream to H.264, add the AVC Video Encoder component to the designer surface.</span></span> <span data-ttu-id="bb0b5-185">Tato součást využívá uncompress datový proud videa jako vstup a poskytuje AVC komprimovaný datový proud videa na jeho výstupní kód pin.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-185">This component takes an uncompress video stream as input and delivers an AVC compressed video stream on its output pin.</span></span>

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

<span data-ttu-id="bb0b5-187">*Bez připojení kodér AVC*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-187">*Unconnected AVC Encoder*</span></span>

<span data-ttu-id="bb0b5-188">Jeho vlastnosti určují, jak přesně kódování se stane.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-188">Its properties determine how the encoding exactly happens.</span></span> <span data-ttu-id="bb0b5-189">Pojďme Podíváme se na některých důležitější nastavení:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-189">Let's have a look at some of the more important settings:</span></span>

* <span data-ttu-id="bb0b5-190">Výstupní šířky a výšky výstup: Tyto určují řešení kódovaného videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-190">Output width and Output height: these determine the resolution of the encoded video.</span></span> <span data-ttu-id="bb0b5-191">V našem případě přejdeme s 640 x 360</span><span class="sxs-lookup"><span data-stu-id="bb0b5-191">In our case let's go with 640x360</span></span>
* <span data-ttu-id="bb0b5-192">Obnovovací frekvence: Pokud nastavíte hodnotu průchozí je právě zavede obnovovací frekvence zdroje, je možné, když ji přepsat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-192">Frame Rate: when set to passthrough it will just adopt the source frame rate, it's possible to override this though.</span></span> <span data-ttu-id="bb0b5-193">Všimněte si, že takový převod kmitočet snímků není pohybu vyrovnanou.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-193">Note that such framerate conversion is not motion-compensated.</span></span>
* <span data-ttu-id="bb0b5-194">Profil a úroveň: Tyto určují profil AVC a úroveň.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-194">Profile and Level: these determine the AVC profile and level.</span></span> <span data-ttu-id="bb0b5-195">Pohodlně získat další informace o různých úrovních a profily, klikněte na ikonu otazníku na komponentu AVC kodér videa a na stránce nápovědy se zobrazit více podrobností o jednotlivých úrovní.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-195">To conveniently get more information about the different levels and profiles, click the question mark icon on the AVC Video Encoder component and the help page will show more detail about each of the levels.</span></span> <span data-ttu-id="bb0b5-196">Pro naše ukázka přejděme s profilem hlavní na úrovni 3.2 (výchozí).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-196">For our sample, let's go with Main Profile at level 3.2 (the default).</span></span>
* <span data-ttu-id="bb0b5-197">Míra režimu řízení a přenosovou rychlostí (kb/s): v tomto scénáři jsme zvolit konstantní přenosovou (CBR) výstup rychlostí 1 200 kb/s</span><span class="sxs-lookup"><span data-stu-id="bb0b5-197">Rate Control Mode and Bitrate (kbps): in our scenario we opt for a constant bitrate (CBR) output at 1200 kbps</span></span>
* <span data-ttu-id="bb0b5-198">Video formát: Jedná se o VUI (Video použitelnost informace), který získá zápisu do datového proudu H.264 (straně informace, které můžou být použité ve decoder k vylepšení zobrazení, ale není nutná správně dekódovat):</span><span class="sxs-lookup"><span data-stu-id="bb0b5-198">Video Format: this is about the VUI (Video Usability Information) that gets written into the H.264 stream (side information that might be used by a decoder to enhance the display but not essential to correctly decode):</span></span>
* <span data-ttu-id="bb0b5-199">NTSC (typická pro USA nebo Japonsko, pomocí 30 fps)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-199">NTSC (typical for US or Japan, using 30 fps)</span></span>
* <span data-ttu-id="bb0b5-200">PAL (typická pro Evropu, pomocí 25 fps)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-200">PAL (typical for Europe, using 25 fps)</span></span>
* <span data-ttu-id="bb0b5-201">Režim velikost GOP: nakonfigurujeme pevné velikosti GOP pro naše účely s intervalu klíč 2 sekund s GOPs uzavřen.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-201">GOP Size Mode: we'll configure Fixed GOP Size for our purposes with a Key Interval of 2 seconds with Closed GOPs.</span></span> <span data-ttu-id="bb0b5-202">Tím se zajistí, že poskytuje kompatibilitu s dynamické balení Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-202">This ensures compatibility with the dynamic packaging Azure Media Services provides.</span></span>

<span data-ttu-id="bb0b5-203">Ke kanálu naše AVC kodér, připojte k vstupní pin nekomprimované Video z kodéru AVC pin výstup nekomprimované Video z média souboru vstupní komponenty.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-203">To feed our AVC encoder, connect the Uncompressed Video output pin from the Media File Input component to the Uncompressed Video input pin from the AVC encoder.</span></span>

![Kodér připojené AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

<span data-ttu-id="bb0b5-205">*Kodér připojené hlavní AVC*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-205">*Connected AVC Main encoder*</span></span>

### <span data-ttu-id="bb0b5-206"><a id="MXF_to_MP4_audio"></a>Kódování zvukový datový proud</span><span class="sxs-lookup"><span data-stu-id="bb0b5-206"><a id="MXF_to_MP4_audio"></a>Encoding the audio stream</span></span>
<span data-ttu-id="bb0b5-207">V tomto okamžiku budeme mít kódování video ale původní nekomprimované zvukový datový proud musí komprimovat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-207">At this point, we have encoded video but the original uncompressed audio stream still needs to be compressed.</span></span> <span data-ttu-id="bb0b5-208">Pro tento budeme věnovat AAC kódování komponentou kodér AAC (Dolby).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-208">For this we'll go with AAC encoding by the AAC Encoder (Dolby) component.</span></span> <span data-ttu-id="bb0b5-209">Přidejte ji do pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-209">Add it to the workflow.</span></span>

![Bez připojení kodér AVC](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

<span data-ttu-id="bb0b5-211">*Bez připojení AAC kodér*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-211">*Unconnected AAC encoder*</span></span>

<span data-ttu-id="bb0b5-212">Teď je nekompatibility: při více než pravděpodobně vstupní soubor média bude mít dva různé nekomprimované zvuk datového proudu k dispozici je pouze jeden nekomprimované zvuk vstupní pin z kodéru AAC: jeden pro levý zvuk kanál a jeden pro vpravo.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-212">Now there's an incompatibility: there's only a single uncompressed audio input pin from the AAC Encoder while more than likely the Media File Input will have two different uncompressed audio stream available: one for the left audio channel and one for the right.</span></span> <span data-ttu-id="bb0b5-213">(V případě, že pracujete s prostorový zvuk, který je 6 kanálů.) Proto není možné k přímému připojení zvukovém souboru ze zdroje vstupní soubor média do zvuk kodér AAC.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-213">(If you're dealing with surround sound, that's 6 channels.) So it's not possible to directly connect the audio from the Media File Input source into the AAC audio encoder.</span></span> <span data-ttu-id="bb0b5-214">Součást AAC očekává takzvané "prokládaná" zvuk datový proud: jeden datový proud, který má vlevo a vpravo kanály prokládaný mezi sebou.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-214">The AAC component expects a so-called "interleaved" audio stream: a single stream that has both the left and the right channels interleaved with each other.</span></span> <span data-ttu-id="bb0b5-215">Po víme z našich zdrojový soubor média jsou zvukové stopy, na jaké pozici ve zdrojovém jsme může generovat takové prokládaná zvuk datový proud s správně přiřazené mluvčího pozice pro doleva a doprava.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-215">Once we know from our source media file which audio tracks are on what position in the source, we can generate such interleaved audio stream with the correctly assigned speaker positions for left and right.</span></span>

<span data-ttu-id="bb0b5-216">Nejdřív jednu chcete vygenerovat prokládaná datového proudu z požadovaných zdrojových zvukové kanály.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-216">First one will want to generated an interleaved stream from the required source audio channels.</span></span> <span data-ttu-id="bb0b5-217">Abychom to bude zpracovávat Interleaver zvuk datového proudu součást.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-217">The Audio Stream Interleaver component will handle this for us.</span></span> <span data-ttu-id="bb0b5-218">Přidejte ji do pracovního postupu a připojte zvuk výstupy ze vstupu soubor média do ní.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-218">Add it to the workflow and connect the audio outputs from the Media File Input into it.</span></span>

![Připojení Interleaver zvukový datový proud](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

<span data-ttu-id="bb0b5-220">*Připojení Interleaver zvukový datový proud*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-220">*Connected Audio Stream Interleaver*</span></span>

<span data-ttu-id="bb0b5-221">Teď, když máme prokládaná zvukový datový proud, jsme ještě nebyla určete, kam pozice mluvčího doleva nebo doprava, které chcete přiřadit.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-221">Now that we have an interleaved audio stream, we still didn't specify where to assign the left or right speaker positions to.</span></span> <span data-ttu-id="bb0b5-222">Aby bylo možné tuto verzi uveďte, můžeme využít přidělující mluvčího pozice uživatel.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-222">In order to specify this, we can leverage the Speaker Position Assigner.</span></span>

![Přidání přidělující uživatel mluvčího pozice](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

<span data-ttu-id="bb0b5-224">*Přidání přidělující uživatel mluvčího pozice*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-224">*Adding a Speaker Position Assigner*</span></span>

<span data-ttu-id="bb0b5-225">Nakonfigurujte přidělující uživatel mluvčího pozice pro použití s stereo vstupního datového proudu prostřednictvím filtr přednastavení kodér "Vlastní" a předvolby Channel, které se nazývá "2.0 (L, R)".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-225">Configure the Speaker Position Assigner for use with a stereo input stream through an Encoder Preset Filter of "Custom" and the Channel Preset called "2.0 (L,R)".</span></span> <span data-ttu-id="bb0b5-226">(To přiřadí pozice levého mluvčího do kanálu 1 a pozice správné mluvčího kanálu 2.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-226">(This will assign the left speaker position to channel 1 and the right speaker position to channel 2.)</span></span>

<span data-ttu-id="bb0b5-227">Připojte ke vstupu kodéru AAC výstup přidělující mluvčího pozice uživatel.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-227">Connect the output of the Speaker Position Assigner to the input of the AAC Encoder.</span></span> <span data-ttu-id="bb0b5-228">Potom je nutné určit AAC kodéru pro práci s "2.0 (L, R)" kanál předvolby, aby věděli, jak nakládat s stereofonní zvuk jako vstup.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-228">Then, tell the AAC Encoder to work with a "2.0 (L,R)" Channel Preset, so it knows to deal with stereo audio as input.</span></span>

### <span data-ttu-id="bb0b5-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexní Audio a Video datových proudů do kontejner MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexing Audio and Video streams into an MP4 container</span></span>
<span data-ttu-id="bb0b5-230">Zadaný naše AVC kódovaného datový proud videa a naše AAC kódovaný zvukový datový proud, jsme můžete zaznamenat do. Kontejner MP4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-230">Given our AVC encoded video stream and our AAC encoded audio stream, we can capture both into an .MP4 container.</span></span> <span data-ttu-id="bb0b5-231">Kombinování různých datových proudů v jeden proces se nazývá "multiplexní" (nebo "muxing").</span><span class="sxs-lookup"><span data-stu-id="bb0b5-231">The process of mixing different streams into a single one is called "multiplexing" (or "muxing").</span></span> <span data-ttu-id="bb0b5-232">V takovém případě jsme se prokládání zvuk a video datové proudy v jediném souvislé. Balíček MP4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-232">In this case we're interleaving the audio and the video streams in a single coherent .MP4 package.</span></span> <span data-ttu-id="bb0b5-233">Komponenty, která koordinuje to pro. Kontejner MP4 se nazývá multiplexor ISO MPEG-4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-233">The component that coordinates this for an .MP4 container is called the ISO MPEG-4 Multiplexer.</span></span> <span data-ttu-id="bb0b5-234">Přidat na plochu návrháře a připojte se k jeho vstupů kodér videa AVC i AAC kodér.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-234">Add one to the designer surface and connect both the AVC Video Encoder and the AAC Encoder to its inputs.</span></span>

![Připojené MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

<span data-ttu-id="bb0b5-236">*Připojené MPEG4 multiplexor*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-236">*Connected MPEG4 Multiplexer*</span></span>

### <span data-ttu-id="bb0b5-237"><a id="MXF_to_MP4_writing_mp4"></a>Zapisování souboru MP4</span><span class="sxs-lookup"><span data-stu-id="bb0b5-237"><a id="MXF_to_MP4_writing_mp4"></a>Writing the MP4 file</span></span>
<span data-ttu-id="bb0b5-238">Při zápisu výstupního souboru, komponentu výstupního souboru se používá.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-238">When writing an output file, the File Output component is used.</span></span> <span data-ttu-id="bb0b5-239">Jsme můžete připojit to k výstupu modulu multiplexor ISO MPEG-4, aby získá jeho výstup zapsán na disk.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-239">We can connect this to the output of the ISO MPEG-4 Multiplexer so that its output gets written to disk.</span></span> <span data-ttu-id="bb0b5-240">K tomu, připojte k zápisu kódu pin vstupní soubor výstupu pin výstup kontejneru (MPEG-4).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-240">To do this, connect the Container (MPEG-4) output pin to the Write input pin of the File Output.</span></span>

![Připojení výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

<span data-ttu-id="bb0b5-242">*Připojení výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-242">*Connected File Output*</span></span>

<span data-ttu-id="bb0b5-243">Název souboru, který bude použit je určen vlastností souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-243">The filename that will be used is determined by the File property.</span></span> <span data-ttu-id="bb0b5-244">Tuto vlastnost mohou být pevně zakódované zadanou hodnotu, pravděpodobně jeden chtít nastavit pomocí výrazu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-244">While that property can be hardcoded to a given value, most likely one will want to set it through an expression instead.</span></span>

<span data-ttu-id="bb0b5-245">Pracovní postup automaticky určit výstup souboru název vlastnosti z výrazu, klikněte na tlačítka vedle názvu souboru (vedle ikony složky).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-245">To have the workflow automatically determine the output File name property from an expression, click the buton next to the File name (next to the folder icon).</span></span> <span data-ttu-id="bb0b5-246">Z rozevírací nabídky vyberte "Výraz".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-246">From the drop down menu then select "Expression".</span></span> <span data-ttu-id="bb0b5-247">Tím se otevře editor výrazů.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-247">This will bring up the expression editor.</span></span> <span data-ttu-id="bb0b5-248">Nejprve vymažte obsah editoru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-248">Clear the contents of the editor first.</span></span>

![Prázdný výraz editoru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

<span data-ttu-id="bb0b5-250">*Prázdný výraz editoru*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-250">*Empty Expression Editor*</span></span>

<span data-ttu-id="bb0b5-251">Výraz editor umožňuje zadat všechny literálovou hodnotou, smíšený s minimálně jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-251">The expression editor allows to enter any literal value, mixed with one or more variables.</span></span> <span data-ttu-id="bb0b5-252">Proměnné začínat znak dolaru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-252">Variables start with a dollar sign.</span></span> <span data-ttu-id="bb0b5-253">Jako zásahu klíč $ editoru zobrazí rozevírací pole s volbou dostupných proměnných.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-253">As you hit the $ key, the editor will show a dropdown box with a choice of available variables.</span></span> <span data-ttu-id="bb0b5-254">V našem případě použijeme kombinaci proměnnou directory výstupní a vstupní soubor základní název proměnné:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-254">In our case we'll use a combination of the output directory variable and the base input file name variable:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Naplní se Editor výrazů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

<span data-ttu-id="bb0b5-256">*Naplní se Editor výrazů*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-256">*Filled out Expression Editor*</span></span>

> [!NOTE]
> <span data-ttu-id="bb0b5-257">Chcete-li zobrazit najdete v části výstupní soubor kódování úlohy v Azure, je nutné zadat hodnotu v editoru výraz.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-257">In order to see see an output file of your encoding job in Azure, you must provide a value in the expression editor.</span></span>
>
>

<span data-ttu-id="bb0b5-258">Jakmile potvrdíte výraz zasažení ok, bude v okně Vlastnosti náhled na co hodnotu vyřeší vlastnost souboru v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-258">When you confirm the expression by hitting ok, the property window will preview to what value the File property resolves at this point in time.</span></span>

![Výraz souboru přeloží dir výstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

<span data-ttu-id="bb0b5-260">*Výraz souboru přeloží dir výstup*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-260">*File Expression resolves output dir*</span></span>

### <span data-ttu-id="bb0b5-261"><a id="MXF_to_MP4_asset_from_output"></a>Vytváření Asset Media Services z výstupního souboru</span><span class="sxs-lookup"><span data-stu-id="bb0b5-261"><a id="MXF_to_MP4_asset_from_output"></a>Creating a Media Services Asset from the output file</span></span>
<span data-ttu-id="bb0b5-262">Když jsme napsali výstupní soubor MP4, potřebujeme ještě znamenat, že tento soubor patří na výstupní asset, který služba media services bude generovat v důsledku spuštění tento pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-262">While we have written an MP4 output file, we still need to indicate that this file belongs to the output asset which media services will generate as a result of executing this workflow.</span></span> <span data-ttu-id="bb0b5-263">Za tímto účelem se používá uzlu výstupní soubor nebo Asset na plátně pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-263">To this end, the Output File/Asset node on the workflow canvas is used.</span></span> <span data-ttu-id="bb0b5-264">Všechny příchozí soubory do tohoto uzlu bude součástí výsledné asset Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-264">All incoming files into this node will make part of the resulting Azure Media Services asset.</span></span>

<span data-ttu-id="bb0b5-265">Komponentu výstupního souboru se připojte k výstupu. soubor/Asset součást dokončení pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-265">Connect the File Output component to the Output File/Asset component to finish the workflow.</span></span>

![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

<span data-ttu-id="bb0b5-267">*Dokončení pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-267">*Finished Workflow*</span></span>

### <span data-ttu-id="bb0b5-268"><a id="MXF_to_MP4_test"></a>Testování místně dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-268"><a id="MXF_to_MP4_test"></a>Test the finished workflow locally</span></span>
<span data-ttu-id="bb0b5-269">K testování pracovního postupu místně, stiskněte tlačítko Přehrát akci na panelu nástrojů v horní části.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-269">To test the workflow locally, hit the play button in the toolbar at the top.</span></span> <span data-ttu-id="bb0b5-270">Po dokončení provádění pracovního postupu zkontrolujte výstup generovaný do nakonfigurované výstupní složky.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-270">When the workflow finished executing, inspect the output generated in the configured output folder.</span></span> <span data-ttu-id="bb0b5-271">Uvidíte dokončení výstupního souboru MP4, který kódované ze souboru MXF vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-271">You'll see the finished MP4 output file that was encoded from the MXF input source file.</span></span>

## <span data-ttu-id="bb0b5-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Kódování MXF do MP4 - multibitrate povolené dynamické balení</span><span class="sxs-lookup"><span data-stu-id="bb0b5-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Encoding MXF into MP4 - multibitrate dynamic packaging enabled</span></span>
<span data-ttu-id="bb0b5-273">V tomto návodu vytvoříme sadu souborů MP4 s více přenosovou rychlostí s kódováním AAC zvuk z jedné. MXF vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-273">In this walkthrough we'll create a set of multiple bitrate MP4 files with AAC encoded audio from a single .MXF input file.</span></span>

<span data-ttu-id="bb0b5-274">Pokud výstupní asset více přenosovými rychlostmi požadované pro použití v kombinaci s funkcemi dynamické balení nabízené službou Azure Media Services, více souborů MP4 zarovnaný GOP jednotlivých potřebné pro různé přenosovou rychlostí a řešení má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-274">When a multi-bitrate asset output is desired for use in combination with the Dynamic Packaging features offered by Azure Media Services, multiple GOP-aligned MP4 files of each a different bitrate and resolution will need to be generated.</span></span> <span data-ttu-id="bb0b5-275">Uděláte to tak, [kódování MXF do MP4 s jednou přenosovou rychlostí](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) návod poskytuje nám to dobrý výchozí bod.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-275">To do so, the [Encoding MXF into a single bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) walkthrough provides us with a good starting point.</span></span>

![Spuštění pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

<span data-ttu-id="bb0b5-277">*Spuštění pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-277">*Starting Workflow*</span></span>

### <span data-ttu-id="bb0b5-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Přidání další jeden nebo více MP4 výstupy</span><span class="sxs-lookup"><span data-stu-id="bb0b5-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Adding one or more additional MP4 outputs</span></span>
<span data-ttu-id="bb0b5-279">Každý soubor MP4 v našem výsledné asset Azure Media Services bude podporovat různé přenosovou rychlostí a řešení.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-279">Every MP4 file in our resulting Azure Media Services asset will support a different bitrate and resolution.</span></span> <span data-ttu-id="bb0b5-280">Umožňuje přidat jeden nebo více souborů MP4 výstup do pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-280">Let's add one or more MP4 output files to the workflow.</span></span>

<span data-ttu-id="bb0b5-281">Pokud chcete mít jistotu, že jsme všechny naše video kodéry vytvořené pomocí stejné nastavení, je nejvhodnější duplicitní stávající kodér videa AVC a nakonfigurovat jiné kombinace překlad IP adres a přenosovou rychlostí (přidejme mezi 960 x 540 na 25 snímků za sekundu při 2,5 MB/s ).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-281">To make sure we have all our video encoders created with the same settings, it's most convenient to duplicate the already existing AVC Video Encoder and configure another combination of resolution and bitrate (let's add one of 960 x 540 at 25 frames per second at 2,5 Mbps).</span></span> <span data-ttu-id="bb0b5-282">Duplicitní stávajícího kodéru, kopírování vložte jej na plochu návrháře.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-282">To duplicate the existing encoder, copy paste it on the designer surface.</span></span>

<span data-ttu-id="bb0b5-283">Připojte pin výstup nekomprimované Video vstupní soubor média do naší nové AVC komponenty.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-283">Connect the Uncompressed Video output pin of the Media File Input into our new AVC component.</span></span>

![Druhý AVC kodér připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

<span data-ttu-id="bb0b5-285">*Druhý AVC kodér připojení*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-285">*Second AVC encoder connected*</span></span>

<span data-ttu-id="bb0b5-286">Nyní přizpůsobíte konfiguraci pro naše nové kodér AVC výstup 960 x 540 2,5 MB/s.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-286">Now adapt the configuration for our new AVC encoder to output 960x540 at 2,5 Mbps.</span></span> <span data-ttu-id="bb0b5-287">(Použijte jeho vlastnosti "výstup šířka", "Výstup výšky" a "Přenosovou rychlostí (kbps)" pro tuto.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-287">(Use its properties "Output width", "Output height", and "Bitrate (kbps)" for this.)</span></span>

<span data-ttu-id="bb0b5-288">Zadané chceme používat výsledné asset společně s Azure Media Services dynamické balení, koncový bod streamování musí být schopný vytvářet z těchto souborů MP4 s fragmenty HLS nebo fragmentovaný soubor MP4/DASH, které přesně odpovídají navzájem způsobem, který klienti, kteří jsou přepínání mezi různých přenosových rychlostí získat jediného smooth průběžné videa a zvuku rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-288">Given we want to use the resulting asset together with Azure Media Services' dynamic packaging, the streaming endpoint needs to be capable of generating from these MP4 files HLS/Fragmented MP4/DASH fragments that are exactly aligned to each other in a way that clients that are switching between different bitrates get a single smooth continuous video and audio experience.</span></span> <span data-ttu-id="bb0b5-289">Chcete-li toho docílit, je potřeba ujistěte, že ve vlastnostech obou AVC kodéry GOP ("skupina obrázky") je velikost pro oba soubory MP4 nastavena na 2 sekund, což lze provést:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-289">To make that happen, we need to ensure that, in the properties of both AVC encoders the GOP ("group of pictures") size for both MP4 files is set to 2 seconds, which can be done by:</span></span>

* <span data-ttu-id="bb0b5-290">nastavení režimu velikost GOP GOP pevnou velikost a</span><span class="sxs-lookup"><span data-stu-id="bb0b5-290">setting the GOP Size Mode to Fixed GOP size and</span></span>
* <span data-ttu-id="bb0b5-291">Interval rámce klíč, který má dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-291">the Key Frame Interval to two seconds.</span></span>
* <span data-ttu-id="bb0b5-292">také nastavit řízení IDR GOP na uzavřený GOP zajistit všechny GOP stojící na své vlastní bez závislosti</span><span class="sxs-lookup"><span data-stu-id="bb0b5-292">also set the GOP IDR Control to Closed GOP to ensure all GOP's are standing on their own without dependencies</span></span>

<span data-ttu-id="bb0b5-293">Pokud chcete, aby naše pracovního postupu vhodné pochopit, přejmenujte první kodér AVC k "kodér videa AVC 640 x 360 1200 kb/s" a druhý kodér AVC "kodér videa AVC 960 x 540 2 500 kb/s".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-293">To make our workflow convenient to understand, rename the first AVC encoder to "AVC Video Encoder 640x360 1200kbps" and the second AVC encoder "AVC Video Encoder 960x540 2500 kbps".</span></span>

<span data-ttu-id="bb0b5-294">Přidáte druhý multiplexor ISO MPEG-4 a druhý výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-294">Now add a second ISO MPEG-4 Multiplexer and a second File Output.</span></span> <span data-ttu-id="bb0b5-295">Připojení k nové kodér AVC multiplexor a ujistěte se, že její výstup se přesměruje do výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-295">Connect the multiplexer to the new AVC encoder and make sure its output is directed into the File Output.</span></span> <span data-ttu-id="bb0b5-296">Pak také připojte AAC zvuk kodér výstup do nového multiplexor na vstup.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-296">Then also connect the AAC audio encoder output to the new multiplexer's input.</span></span> <span data-ttu-id="bb0b5-297">Výstupní soubor pak můžete pak být připojen k uzlu výstupní soubor nebo Asset tím ho přidáte do služby Asset média, který bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-297">The File Output in turn can then be connected to the Output File/Asset node to add it to the Media Services Asset that will be created.</span></span>

![Druhý multiplexor a výstup souboru připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

<span data-ttu-id="bb0b5-299">*Druhý multiplexor a výstup souboru připojení*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-299">*Second Muxer and File Output connected*</span></span>

<span data-ttu-id="bb0b5-300">Zajištění kompatibility se službou Azure Media Services dynamické balení nakonfigurujte multiplexor na režimu bloku na hodnotu GOP count nebo doba trvání a nastavte GOPs za bloku na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-300">For compatibility with Azure Media Services dynamic packaging, configure the multiplexer's Chunk Mode to GOP count or duration and set the GOPs per chunk to 1.</span></span> <span data-ttu-id="bb0b5-301">(To by měl být výchozí.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-301">(This should be the default.)</span></span>

![Režimy multiplexor bloku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

<span data-ttu-id="bb0b5-303">*Režimy multiplexor bloku*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-303">*Muxer Chunk Modes*</span></span>

<span data-ttu-id="bb0b5-304">Poznámka: můžete tento postup opakujte pro další přenosovou rychlostí a rozlišení kombinace, které chcete přidali do výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-304">Note: you may want to repeat this process for any other bitrate and resolution combinations you want to have added to the asset output.</span></span>

### <span data-ttu-id="bb0b5-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurace názvů výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="bb0b5-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Configuring the file output names</span></span>
<span data-ttu-id="bb0b5-306">Máme více než jeden jeden soubor přidán na výstupní asset.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-306">We have more than one single file added to the output asset.</span></span> <span data-ttu-id="bb0b5-307">To poskytuje potřeba Ujistěte se, že názvy souborů pro každou výstupní soubory se liší od sebe navzájem a možná i použít zadávání názvů tak, že se z názvu souboru se zabývajících se.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-307">This provides a need to make sure the filenames for each of the output files are different from each other and maybe even apply a file-naming convention so it becomes clear from the file name what you're dealing with.</span></span>

<span data-ttu-id="bb0b5-308">Názvy výstupního souboru se dá řídit přes výrazy v návrháři.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-308">File output naming can be controlled through expressions in the designer.</span></span> <span data-ttu-id="bb0b5-309">Otevřete podokno vlastností pro jednu ze součástí výstup souboru a otevřete editor výraz pro vlastnost souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-309">Open the property pane for one of the File Output components and open the expression editor for the File property.</span></span> <span data-ttu-id="bb0b5-310">Naše první výstupní soubor byl nakonfigurovaný pomocí následující výraz (projděte si kurz pro přecházející z [MXF k výstupu MP4 s jednou přenosovou rychlostí](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span><span class="sxs-lookup"><span data-stu-id="bb0b5-310">Our first output file was configured through the following expression (see the tutorial for going from [MXF to a single bitrate MP4 output](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

<span data-ttu-id="bb0b5-311">To znamená, že naše filename je dáno dvou proměnných: se zapisovat do výstupního adresáře a základní název zdrojového souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-311">This means that our filename is determined by two variables: the output directory to write in and the source file base name.</span></span> <span data-ttu-id="bb0b5-312">První je zpřístupněná jako vlastnost v kořenovém adresáři pracovního postupu a je určen příchozích souborů.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-312">The former is exposed as a property on the workflow root and the latter is determined by the incoming file.</span></span> <span data-ttu-id="bb0b5-313">Upozorňujeme, že výstupní adresář je použít pro místní testování; Tato vlastnost bude přepsáno modul pracovních postupů při spouští pracovní postup procesoru založené na cloudu média ve službě Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-313">Note that the output directory is what you use for local testing; this property will be overridden by the workflow engine when the workflow is executed by the cloud-based media processor in Azure Media Services.</span></span>
<span data-ttu-id="bb0b5-314">Konzistentní výstup pojmenování poskytnout i naše výstupní soubory, změňte první soubor pojmenování výraz, který se:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-314">To give both our output files a consistent output naming, change the first file naming expression to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="bb0b5-315">a druhou pro:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-315">and the second to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="bb0b5-316">Spusťte test zprostředkující spustit a ujistěte se generují správně obou výstupní soubory MP4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-316">Execute an intermediate test run to make sure both MP4 output files are properly generated.</span></span>

### <span data-ttu-id="bb0b5-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Přidání samostatných sledovat zvuk</span><span class="sxs-lookup"><span data-stu-id="bb0b5-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Adding a separate Audio Track</span></span>
<span data-ttu-id="bb0b5-318">Jako ukážeme později při se vygeneruje soubor .ism pomocí našich výstupní soubory MP4, bude také vyžadujeme pouze zvukový soubor MP4 jako zvuk sledování pro naše adaptivní streamování.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-318">As we'll see later when we generate an .ism file to go with our MP4 output files, we will also require a audio-only MP4 file as the audio track for our adaptive streaming.</span></span> <span data-ttu-id="bb0b5-319">K vytvoření tohoto souboru, přidejte další multiplexor do pracovního postupu (multiplexor ISO-MPEG-4) a připojit kodér AAC výstup pin s jeho vstupní PIN kódu pro sledování 1.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-319">To create this file, add an additional muxer to the workflow (ISO-MPEG-4 Multiplexer) and connect the AAC encoder's output pin with its input pin for Track 1.</span></span>

![Zvuk multiplexor přidán](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

<span data-ttu-id="bb0b5-321">*Zvuk multiplexor přidán*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-321">*Audio Muxer Added*</span></span>

<span data-ttu-id="bb0b5-322">Vytvořte třetí výstup souboru součást, kterou je výstup výstupní datový proud z multiplexor a nakonfigurovat souboru pojmenování výraz jako:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-322">Create a third File Output component to output the outbound stream from the muxer and configure the file naming expression as:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Zvuk multiplexor vytvoření výstupního souboru](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

<span data-ttu-id="bb0b5-324">*Zvuk multiplexor vytvoření výstupního souboru*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-324">*Audio Muxer creating File Output*</span></span>

### <span data-ttu-id="bb0b5-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Přidání. Soubor SMIL ISM</span><span class="sxs-lookup"><span data-stu-id="bb0b5-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Adding the .ISM SMIL File</span></span>
<span data-ttu-id="bb0b5-326">Dynamické balení pro práci v kombinaci s MP4 soubory (i pouze MP4) v našem asset Media Services, je také nutné soubor manifestu (také označovaný jako soubor "SMIL": synchronizované multimédia integrace jazyka).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-326">For the dynamic packaging to work in combination with both MP4 files (and the audio-only MP4) in our Media Services asset, we also need a manifest file (also called a "SMIL" file: Synchronized Multimedia Integration Language).</span></span> <span data-ttu-id="bb0b5-327">Tento soubor k Azure Media Services určuje, jaké soubory MP4 jsou dostupné pro dynamické balení a které z nich vzít v úvahu pro zvuk streamování.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-327">This file indicates to Azure Media Services what MP4 files are available for dynamic packaging and which of those to consider for the audio streaming.</span></span> <span data-ttu-id="bb0b5-328">Typické souboru manifestu sady na MP4 s jeden datový proud zvuku vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-328">A typical manifest file for a set of MP4's with a single audio stream looks like this:</span></span>

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

<span data-ttu-id="bb0b5-329">Soubor .ism obsahuje v rámci příkazu switch, odkaz na každé jednotlivé video soubory MP4 a navíc tyto odkazy zvukový soubor také jeden (nebo více) k MP4, který obsahuje pouze zvukovém souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-329">The .ism file contains within a switch statement, a reference to each of the individual MP4 video files and in addition to those also one (or more) audio file references to an MP4 that only contains the audio.</span></span>

<span data-ttu-id="bb0b5-330">Generování souboru manifestu pro náš sadu MP4 na lze provést prostřednictvím komponenty s názvem "AMS Manifest zapisovač".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-330">Generating the manifest file for our set of MP4's can be done through a component called the "AMS Manifest Writer".</span></span> <span data-ttu-id="bb0b5-331">Pokud chcete použít, přetáhněte ji na plochu a připojit "Zápis úplná" PIN výstup ze tří součástí výstup souboru Manifest zapisovače AMS vstupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-331">To use it, drag it onto the surface and connect the "Write Complete" output pins from the three File Output components to the AMS Manifest Writer input.</span></span> <span data-ttu-id="bb0b5-332">Ujistěte se, připojit výstup AMS Manifest zapisovače pro výstupní soubor nebo Asset.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-332">Then make sure to connect the output of the AMS Manifest Writer to the Output File/Asset.</span></span>

<span data-ttu-id="bb0b5-333">Stejně jako u našich souboru výstup součásti, nakonfigurujte název výstupního souboru .ism s výrazem:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-333">As with our other file output components, configure the .ism file output name with an expression:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

<span data-ttu-id="bb0b5-334">Naše dokončení pracovního postupu vypadá níže:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-334">Our finished workflow looks like the below:</span></span>

![Dokončení MXF multibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

<span data-ttu-id="bb0b5-336">*Dokončení MXF multibitrate MP4 pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-336">*Finished MXF to multibitrate MP4 workflow*</span></span>

## <span data-ttu-id="bb0b5-337"><a id="MXF_to__multibitrate_MP4"></a>Kódování MXF do multibitrate MP4 - rozšířené plán, podle kterého</span><span class="sxs-lookup"><span data-stu-id="bb0b5-337"><a id="MXF_to__multibitrate_MP4"></a>Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>
<span data-ttu-id="bb0b5-338">V [předchozího návodu pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) jste viděli, jak jeden vstupní asset MXF, mohou být převedeny na výstupní asset s souborů MP4 s více přenosovými rychlostmi, pouze zvukový soubor MP4 a soubor manifestu pro použití ve spojení s Azure Media Services dynamické balení.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-338">In the [previous workflow walkthrough](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we've seen how a single MXF input asset can be converted into an output asset with multi-bitrate MP4 files, an audio-only MP4 file and a manifest file for use in conjunction with Azure Media Services dynamic packaging.</span></span>

<span data-ttu-id="bb0b5-339">Tento názorný postup se zobrazí, jak některé charakteristik jdou vylepšit a provedené pohodlnější.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-339">This walkthrough will show how some of the aspects can be enhanced and made more convenient.</span></span>

### <span data-ttu-id="bb0b5-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Přehled pracovního postupu k vylepšení</span><span class="sxs-lookup"><span data-stu-id="bb0b5-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Workflow overview to enhance</span></span>
![Pracovní postup Multibitrate MP4 k vylepšení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

<span data-ttu-id="bb0b5-342">*Pracovní postup Multibitrate MP4 k vylepšení*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-342">*Multibitrate MP4 workflow to enhance*</span></span>

### <span data-ttu-id="bb0b5-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>Konvence pro pojmenování souborů</span><span class="sxs-lookup"><span data-stu-id="bb0b5-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>File Naming Conventions</span></span>
<span data-ttu-id="bb0b5-344">V předchozím postupu jsme jednoduchý výraz zadaný jako základ pro generování názvy výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-344">In the previous workflow we specified a simple expression as the basis for generating output file names.</span></span> <span data-ttu-id="bb0b5-345">Když máme některé duplikace: všechny komponenty jednotlivých výstupní soubor zadaný takové výraz.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-345">We have some duplication though: all of the the individual output file components specified such expression.</span></span>

<span data-ttu-id="bb0b5-346">Například naše součást výstupní soubor pro první videosoubor nakonfigurovaná s tímto výrazem:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-346">For example, our file output component for the first video file is configured with this expression:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="bb0b5-347">Když pro druhý výstupu videa máme výraz jako:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-347">While for the second output video, we have an expression like:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="bb0b5-348">Nebylo by čisticí méně chyba náchylné k chybám a pohodlnější, pokud jsme může odeberte některé z této duplikace a ujistěte se, co konfigurovatelnější místo?</span><span class="sxs-lookup"><span data-stu-id="bb0b5-348">Wouldn't it be cleaner, less error prone and more convenient if we could remove some of this duplication and make things more configurable instead?</span></span> <span data-ttu-id="bb0b5-349">Naštěstí můžeme: Možnosti nástroje designer výrazu v kombinaci s možnost vytvářet vlastní vlastnosti v kořenovém adresáři naše pracovního postupu nám umožní úroveň pohodlí.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-349">Luckily we can: the designer's expression capabilities in combination with the ability to create custom properties on our workflow root will give us an added layer of convenience.</span></span>

<span data-ttu-id="bb0b5-350">Předpokládejme, že jsme budete jednotka filename konfigurace z přenosových rychlostí jednotlivých souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-350">Let's assume we'll drive filename configuration from the bitrates of the individual MP4 files.</span></span> <span data-ttu-id="bb0b5-351">Tyto přenosových rychlostí, které budete usilujeme o nakonfigurovat na jednom místě (v kořenovém naše grafu), kde budete mít přístup ke konfiguraci a jednotky generování názvů souborů.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-351">These bitrates we'll aim to configure in one central place (on the root of our graph), from where they'll be accessed to configure and drive file name generation.</span></span> <span data-ttu-id="bb0b5-352">K tomuto účelu jsme začít publikování vlastnost přenosovou rychlostí z obou AVC kodérů do kořenového adresáře naše pracovního postupu, takže bude přístupný z obou kořene také kvůli kodéry AVC.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-352">To do this, we start by publishing the bitrate property from both AVC encoders to the root of our workflow, so that it becomes accessible from both the root as well as from the AVC encoders.</span></span> <span data-ttu-id="bb0b5-353">(I když se zobrazí v dva různé body, se jenom jedna nadřazená hodnota.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-353">(Even if displayed in two different spots, there's only one underlying value.)</span></span>

### <span data-ttu-id="bb0b5-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publikování vlastnosti komponenty do kořenové pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publishing component properties onto the workflow root</span></span>
<span data-ttu-id="bb0b5-355">Otevřete první kodér AVC, přejděte na vlastnost přenosovou rychlostí (kbps) a z rozevíracího seznamu vyberte publikovat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-355">Open the first AVC encoder, go to the Bitrate (kbps) property and from the dropdown choose Publish.</span></span>

![Publikování vlastnost přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

<span data-ttu-id="bb0b5-357">*Publikování vlastnost přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-357">*Publishing the bitrate property*</span></span>

<span data-ttu-id="bb0b5-358">Dialogové okno publikování publikovat do kořenového adresáře naše grafu pracovního postupu nakonfigurujte publikované název "video1bitrate" a "1 Video přenosovou rychlostí" čitelné zobrazovaný název.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-358">Configure the publish dialog to publish to the root of our workflow graph, with a published name of "video1bitrate" and a readable display name of "Video 1 Bitrate".</span></span> <span data-ttu-id="bb0b5-359">Nakonfigurovat vlastní název skupiny označuje "Streamování přenosových rychlostí" a stiskněte tlačítko Publikovat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-359">Configure a custom group name called "Streaming Bitrates" and hit Publish.</span></span>

![Publikování vlastnost přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

<span data-ttu-id="bb0b5-361">*Dialogové okno Vlastnosti přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-361">*Publishing dialog for bitrate property*</span></span>

<span data-ttu-id="bb0b5-362">Opakujte stejný pro vlastnost přenosovou rychlostí druhý kodér AVC s názvem "video2bitrate" s názvem "Video přenosovou rychlostí 2," a zobrazení ve stejné skupině vlastní "Streamování přenosových rychlostí".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-362">Repeat the same for the bitrate property of the second AVC encoder and name it "video2bitrate" with a display name of "Video 2 Bitrate", in the same custom group "Streaming Bitrates".</span></span>

<span data-ttu-id="bb0b5-363">Pokud jsme teď prověřit vlastnosti kořenové pracovního postupu, uvidíme naše vlastní skupiny s dvě vlastnosti publikovaných objeví.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-363">If we now inspect the workflow root properties, we'll see our custom group with the two published properties show up.</span></span> <span data-ttu-id="bb0b5-364">Obě jsou odrážející hodnotu jejich odpovídajících přenosovou rychlostí AVC kodér.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-364">Both are reflecting the value of their respective AVC encoder bitrate.</span></span>

![Props publikované přenosovou rychlostí v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

<span data-ttu-id="bb0b5-366">Pokaždé, když jsme potřeba přístup k tyto vlastnosti z kódu nebo z výrazu, jsme tak postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-366">Whenever we want to access these properties from code or from an expression, we can do so like this:</span></span>

* <span data-ttu-id="bb0b5-367">z vloženého kódu z komponenty vpravo pod kořenovým adresářem: node.getPropertyAsString('.. / video1bitrate ", null)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-367">from inline code from a component right below the root: node.getPropertyAsString('../video1bitrate',null)</span></span>
* <span data-ttu-id="bb0b5-368">ve výrazu: ${ROOT_video1bitrate}</span><span class="sxs-lookup"><span data-stu-id="bb0b5-368">within an expression: ${ROOT_video1bitrate}</span></span>

<span data-ttu-id="bb0b5-369">Umožňuje dokončit skupině "Streamování přenosových rychlostí" pomocí publikování naše zvuk sledovat přenosovou rychlostí na něm také.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-369">Let's complete the "Streaming Bitrates" group by publishing our audio track bitrate on it as well.</span></span> <span data-ttu-id="bb0b5-370">V rámci vlastností kodér AAC vyhledejte nastavení přenosovou rychlostí a publikovat vyberte z rozevíracího seznamu vedle sebe.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-370">Within the properties of the AAC Encoder, search for the Bitrate setting and select Publish from the dropdown next to it.</span></span> <span data-ttu-id="bb0b5-371">Publikovat do kořenového adresáře grafu s názvem "audio1bitrate" a zobrazované jméno "1 zvuk přenosovou rychlostí" v rámci naší vlastní skupiny "Streamování přenosových rychlostí".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-371">Publish to the root of the graph with name "audio1bitrate" and display name "Audio 1 Bitrate" within our custom group "Streaming Bitrates".</span></span>

![Dialogové okno pro zvuk přenosovou rychlostí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

<span data-ttu-id="bb0b5-373">*Dialogové okno pro zvuk přenosovou rychlostí*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-373">*Publishing dialog for audio bitrate*</span></span>

![Výsledný videa a zvuku props v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

<span data-ttu-id="bb0b5-375">*Výsledný videa a zvuku props v kořenovém adresáři*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-375">*Resulting video and audio props on root*</span></span>

<span data-ttu-id="bb0b5-376">Upozorňujeme, že některé z těchto tří změna hodnoty také znovu nakonfiguruje a změní hodnoty na příslušné komponenty jsou propojeny s (a případně pomocí publikovat).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-376">Note that changing any of those three values also re-configures and changes the values on the respective components they are linked with (and where published from).</span></span>

### <span data-ttu-id="bb0b5-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Vygenerování výstupního souboru, jejichž názvy na hodnoty publikovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="bb0b5-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Have generated output file names rely on published property values</span></span>
<span data-ttu-id="bb0b5-378">Místo hardcoding naše názvy generovaný soubor jsme nyní můžete změnit výraz naše filename jednotlivých součástí výstupního souboru se spoléhat na vlastnosti přenosovou rychlostí, kterou jsme právě publikovaný v kořenovém adresáři grafu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-378">Instead of hardcoding our generated file names, we can now change our filename expression on each of the File Output components to rely on the bitrate properties we just published on the graph root.</span></span> <span data-ttu-id="bb0b5-379">Počínaje naše první výstupního souboru, najít vlastnost soubor a upravit výraz takto:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-379">Starting with our first file output, find the File property and edit the expression like this:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

<span data-ttu-id="bb0b5-380">Různé parametry v tomto výrazu můžete získat přístup a zadá stiskne dolaru na klávesnici v okně výrazu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-380">The different parameters in this expression can be accessed and entered by hitting the dollar sign on the keyboard while in the expression window.</span></span> <span data-ttu-id="bb0b5-381">Jeden z dostupných parametrů je naše video1bitrate vlastnosti, které jsme dříve publikované.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-381">One of the available parameters is our video1bitrate property which we published earlier.</span></span>

![Přístup k parametry v rámci výrazu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

<span data-ttu-id="bb0b5-383">*Přístup k parametry v rámci výrazu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-383">*Accessing parameters within an expression*</span></span>

<span data-ttu-id="bb0b5-384">Proveďte stejný pro výstup souboru pro naše druhý video:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-384">Do the same for the file output for our second video:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

<span data-ttu-id="bb0b5-385">a pro výstup pouze zvukový soubor:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-385">and for the audio-only file output:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

<span data-ttu-id="bb0b5-386">Pokud nyní Změníme přenosovou rychlostí pro některý ze souborů video nebo zvuk, příslušné kodér konfigurace bude změněna a konvence název souboru na základě přenosovou rychlostí se dodržení vše probíhá automaticky.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-386">If we now change the bitrate for any of the video or audio files, the respective encoder will be reconfigured and the bitrate-based file name convention will be honored all automatic.</span></span>

## <span data-ttu-id="bb0b5-387"><a id="thumbnails_to__multibitrate_MP4"></a>Přidání miniatury do multibitrate MP4 výstup</span><span class="sxs-lookup"><span data-stu-id="bb0b5-387"><a id="thumbnails_to__multibitrate_MP4"></a>Adding thumbnails to multibitrate MP4 output</span></span>
<span data-ttu-id="bb0b5-388">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do přidávání miniatur do výstupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-388">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into adding thumbnails to the output.</span></span>

### <span data-ttu-id="bb0b5-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Přehled pracovního postupu přidat miniatury pro</span><span class="sxs-lookup"><span data-stu-id="bb0b5-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Workflow overview to add thumbnails to</span></span>
![Multibitrate MP4 pracovní postup spustit z](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

<span data-ttu-id="bb0b5-391">*Multibitrate MP4 pracovní postup spustit z*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-391">*Multibitrate MP4 workflow to start from*</span></span>

### <span data-ttu-id="bb0b5-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Přidání JPG kódování</span><span class="sxs-lookup"><span data-stu-id="bb0b5-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Adding JPG Encoding</span></span>
<span data-ttu-id="bb0b5-393">Srdcem naše miniatur generování bude komponentu kodér JPG, můžete provést výstup JPG soubory.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-393">The heart of our thumbnail generation will be the JPG Encoder component, able to output JPG files.</span></span>

![Kodér JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

<span data-ttu-id="bb0b5-395">*Kodér JPG*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-395">*JPG Encoder*</span></span>

<span data-ttu-id="bb0b5-396">Nelze připojit ale přímo naší nekomprimované Video datového proudu ze vstupu soubor média do kodér JPG.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-396">We cannot however directly connect our Uncompressed Video stream from the Media File Input into the JPG encoder.</span></span> <span data-ttu-id="bb0b5-397">Místo toho očekává možné předat jednotlivé snímky.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-397">Instead, it expects to be handed individual frames.</span></span> <span data-ttu-id="bb0b5-398">To, provedeme prostřednictvím brány rámce Video komponent.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-398">This, we can do through the Video Frame Gate component.</span></span>

![Připojení brány rámce kodéru, JPG](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

<span data-ttu-id="bb0b5-400">*Připojení brány rámce kodéru, JPG*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-400">*Connecting a frame gate to the JPG encoder*</span></span>

<span data-ttu-id="bb0b5-401">Pro bránu rámce každých mnoho sekund nebo rámce umožňuje snímek videa a předat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-401">The frame gate once every so many seconds or frames allows a video frame to pass.</span></span> <span data-ttu-id="bb0b5-402">Intervalu a časového posunu s který tomu je možné konfigurovat ve vlastnostech.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-402">The interval and the time offset with which this happens is configurable in the properties.</span></span>

![Vlastnosti videa rámce brány](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

<span data-ttu-id="bb0b5-404">*Vlastnosti videa rámce brány*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-404">*Video Frame Gate properties*</span></span>

<span data-ttu-id="bb0b5-405">Umožňuje vytvořit miniaturu každou minutu nastavení režimu (v sekundách) a Interval, který má 60.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-405">Let's create a thumbnail every minute by setting the mode to Time (seconds) and the Interval to 60.</span></span>

### <span data-ttu-id="bb0b5-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Práci s barevný prostor převod</span><span class="sxs-lookup"><span data-stu-id="bb0b5-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Dealing with Color Space conversion</span></span>
<span data-ttu-id="bb0b5-407">Když se jeví logické, že oba PIN kódy nekomprimované Video rámce brány a vstupní soubor média lze nyní připojit, jsme by by tak v takovém případě se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-407">While it would seem logical both Uncompressed Video pins of the frame gate and the Media File Input can now be connected, we would get a warning if we would do so.</span></span>

![Vstupní barva místo chyby](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

<span data-ttu-id="bb0b5-409">*Vstupní barva místo chyby*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-409">*Input color space error*</span></span>

<span data-ttu-id="bb0b5-410">Je to proto, že způsob barvy, která je reprezentována informace v našem původní nezpracovaná nekomprimované datový proud videa, pocházejících z našich MXF se liší od očekává kodér JPG.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-410">This is because the way in which colour information is represented in our original raw uncompressed video stream, coming from our MXF, is different from what the JPG Encoder is expecting.</span></span> <span data-ttu-id="bb0b5-411">Přesněji řečeno takzvané "barvu místo" "RGB" nebo "Ve stupních šedi" se očekává v toku.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-411">More specifically, a so-called "color space" of "RGB" or "Grayscale" is expected to flow in.</span></span> <span data-ttu-id="bb0b5-412">To znamená, že Video rámce brány datový proud videa příchozí muset mít převodu z použije první týkající se jeho barevný prostor.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-412">This means that the Video Frame Gate's inbound video stream will need to have a conversion applied regarding its color space first.</span></span>

<span data-ttu-id="bb0b5-413">Přetáhněte na pracovního postupu místo převaděč barva - Intel a připojte ho k naší rámce brány.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-413">Drag onto the workflow the Color Space Converter - Intel and connect it to our frame gate.</span></span>

![Připojení úpravy místo barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

<span data-ttu-id="bb0b5-415">*Připojení úpravy místo barev*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-415">*Connecting a Color Space Convertor*</span></span>

<span data-ttu-id="bb0b5-416">V okně Vlastnosti vyberte položku BGR 24 ze seznamu přednastavení.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-416">In the properties window, pick the BGR 24 entry from the Preset list.</span></span>

### <span data-ttu-id="bb0b5-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Zápis miniatur</span><span class="sxs-lookup"><span data-stu-id="bb0b5-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Writing the thumbnails</span></span>
<span data-ttu-id="bb0b5-418">Liší od našich video MP4, komponentu JPG kodér bude výstup více než jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-418">Different from our MP4 video's, the JPG Encoder component will output more than one file.</span></span> <span data-ttu-id="bb0b5-419">Aby bylo možné zpracovat takové, lze použít komponentu scény vyhledávání JPG souboru zapisovače: bude trvat příchozí miniatur JPG a zapsat je, každý název souboru se na konci pomocí jiné číslo.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-419">In order to deal with this, a Scene Search JPG File Writer component can be used: it will take the incoming JPG thumbnails and write them out, each filename being suffixed by a different number.</span></span> <span data-ttu-id="bb0b5-420">(Číslo obvykle určující počet sekund/jednotky v datovém proudu, který byl miniaturu čerpají z.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-420">(The number typically indicating the number of seconds/units in the stream which the thumbnail was drawn from.)</span></span>

![Představení zapisovač souboru JPG scény vyhledávání](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

<span data-ttu-id="bb0b5-422">*Představení zapisovač souboru JPG scény vyhledávání*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-422">*Introducing the Scene Search JPG File Writer*</span></span>

<span data-ttu-id="bb0b5-423">Nakonfigurovat vlastnost výstupní cesta ke složce ve výrazu: ${ROOT_outputWriteDirectory}</span><span class="sxs-lookup"><span data-stu-id="bb0b5-423">Configure the Output Folder Path property with the expression: ${ROOT_outputWriteDirectory}</span></span>

<span data-ttu-id="bb0b5-424">a vlastnost Filename předponu s:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-424">and the Filename Prefix property with:</span></span>

    ${ROOT_sourceFileBaseName}_thumb_

<span data-ttu-id="bb0b5-425">Předpona, která určí, jak jsou právě pojmenované miniatur soubory.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-425">The prefix will determine how the thumbnail files are being named.</span></span> <span data-ttu-id="bb0b5-426">Budou se měly na konci číslo určující pozici v datovém proudu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-426">They will be suffixed with a number indicating the thumb's position in the stream.</span></span>

![Vlastnosti scény zapisovače souboru JPG vyhledávání](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

<span data-ttu-id="bb0b5-428">*Vlastnosti scény zapisovače souboru JPG vyhledávání*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-428">*Scene Search JPG File Writer properties*</span></span>

<span data-ttu-id="bb0b5-429">Zapisovač souboru JPG vyhledávání scény připojte k uzlu výstupní soubor nebo Asset.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-429">Connect the Scene Search JPG File Writer to the Output File/Asset node.</span></span>

### <span data-ttu-id="bb0b5-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Zjištění chyb v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Detecting errors in a workflow</span></span>
<span data-ttu-id="bb0b5-431">Vstupní barva převaděč místa se připojte k nezpracované nekomprimované výstupu videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-431">Connect the input of the color space converter to the raw uncompressed video output.</span></span> <span data-ttu-id="bb0b5-432">Nyní testu místní spuštění pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-432">Now perform a local test run for the workflow.</span></span> <span data-ttu-id="bb0b5-433">Je velmi pravděpodobné pracovní postup bude najednou zastavit provádění a znamenat s červenou outline na komponentu došlo k chybě:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-433">There's a good chance the workflow will suddenly stop executing and indicate with a red outline on the component that encountered an error:</span></span>

![Chyba místo převaděč barev](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

<span data-ttu-id="bb0b5-435">*Chyba místo převaděč barev*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-435">*Color Space Converter error*</span></span>

<span data-ttu-id="bb0b5-436">Klepněte na malé červenou ikonu "E" v horním pravém rohu komponentu barva místo převaděč vidět co je z důvodu kódování pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-436">Click the little red "E" icon in the top right corner of the Color Space Converter component to see what's the reason the encoding attempt failed.</span></span>

![Barva místo převaděč chybové dialogové okno](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

<span data-ttu-id="bb0b5-438">*Barva místo převaděč chybové dialogové okno*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-438">*Color Space Converter error dialog*</span></span>

<span data-ttu-id="bb0b5-439">Zjistíte, jak můžete vidět, že příchozí standard pro převaděč místo barva barevný prostor musí být rec601 pro naše požadovaný převod YUV RGB.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-439">It turns out, as you can see, that the incoming color space standard for the color space converter has to be rec601 for our requested conversion of YUV to RGB.</span></span> <span data-ttu-id="bb0b5-440">Naše datový proud nepodporuje zjevně znamenat rec601 jeho.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-440">Apparently our stream doesn't indicate it's rec601.</span></span> <span data-ttu-id="bb0b5-441">(Dop. 601 je standard pro kódování prokládaných analogovým video signály v digitální podobě videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-441">(Rec 601 is a standard for encoding interlaced analog video signals in digital video form.</span></span> <span data-ttu-id="bb0b5-442">Určuje oblast active pokrývajících 720 světlostí a 360 chrominance vzorků na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-442">It specifies an active region covering 720 luminance samples and 360 chrominance samples per line.</span></span> <span data-ttu-id="bb0b5-443">Barevné kódování systému se označuje jako YCbCr 4:2:2.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-443">The color encoding system is known as YCbCr 4:2:2.)</span></span>

<span data-ttu-id="bb0b5-444">Chcete-li odstranit tento problém, jsme budete znamenat na metadata z našich datový proud, který jsme pracujete s rec601 obsah.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-444">To fix this, we'll indicate on the metadata of our stream that we're dealing with rec601 content.</span></span> <span data-ttu-id="bb0b5-445">Uděláte to tak použijeme Video datový typ aktualizační součásti, kterou jsme budete put mezi naše neformátovaná a součást převod místo barev.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-445">To do so we'll use a Video Data Type Updater component, which we'll put in between our raw source and the color space conversion component.</span></span> <span data-ttu-id="bb0b5-446">Tento datový typ aktualizační umožňuje ruční aktualizace určité video dat vlastnosti typu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-446">This data type updater allows for the manual update of certain video data type properties.</span></span> <span data-ttu-id="bb0b5-447">Nakonfigurujte označíte, místo barva standardní z "Rec 601".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-447">Configure it to indicate a Color Space Standard of "Rec 601".</span></span> <span data-ttu-id="bb0b5-448">To způsobí, že aktualizační typ dat Video k označení datový proud s barevný prostor "Rec 601", pokud se žádné barevný prostor ještě definice.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-448">This will cause the Video Data Type Updater to tag the stream with the "Rec 601" color space if there was no color space defined yet.</span></span> <span data-ttu-id="bb0b5-449">(Ho nebude přepsat všechny existující metadata, pokud přepsání zaškrtnuto.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-449">(It will not override any existing metadata, unless the Override checkbox was checked.)</span></span>

![Aktualizace barva místo Standard na aktualizační typ dat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

<span data-ttu-id="bb0b5-451">*Aktualizace barva místo Standard na aktualizační typ dat*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-451">*Updating Color Space Standard on the Data Type Updater*</span></span>

### <span data-ttu-id="bb0b5-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Finished Workflow</span></span>
<span data-ttu-id="bb0b5-453">Teď, když naše dokončení naše pracovního postupu, proveďte další testovací spustit nevidíte předat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-453">Now that our our workflow is finished, do another test run to see it pass.</span></span>

![Dokončení pracovního postupu pro výstup více mp4 s miniaturami](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

<span data-ttu-id="bb0b5-455">*Dokončení pracovního postupu pro výstup více mp4 s miniaturami*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-455">*Finished workflow for multi-mp4 output with thumbnails*</span></span>

## <span data-ttu-id="bb0b5-456"><a id="time_based_trim"></a>Oříznutí založené na čase multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-456"><a id="time_based_trim"></a>Time-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="bb0b5-457">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání video zdroje podle časová razítka.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-457">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming the source video based on time-stamps.</span></span>

### <span data-ttu-id="bb0b5-458"><a id="time_based_trim_start"></a>Přehled pracovního postupu, pokud chcete začít přidávat oříznutí na</span><span class="sxs-lookup"><span data-stu-id="bb0b5-458"><a id="time_based_trim_start"></a>Workflow overview to start adding trimming to</span></span>
![Spouští pracovní postup pro přidání oříznutí na](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

<span data-ttu-id="bb0b5-460">*Spouští pracovní postup pro přidání oříznutí na*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-460">*Starting workflow to add trimming to*</span></span>

### <span data-ttu-id="bb0b5-461"><a id="time_based_trim_use_stream_trimmer"></a>Použití oříznutí datového proudu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-461"><a id="time_based_trim_use_stream_trimmer"></a>Using the Stream Trimmer</span></span>
<span data-ttu-id="bb0b5-462">Oříznutí datového proudu součást umožňuje trim na začátek a konec vstupního datového proudu základní na časování informace (sekund, minut,...). Oříznutí nepodporuje na základě rámce oříznutí.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-462">The Stream Trimmer component allows to trim the beginning and ending of an input stream base on timing information (seconds, minutes, ...). The trimmer does not support frame-based trimming.</span></span>

![Oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

<span data-ttu-id="bb0b5-464">*Oříznutí datového proudu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-464">*Stream Trimmer*</span></span>

<span data-ttu-id="bb0b5-465">Místo propojení AVC kodéry a přidělující mluvčího pozice uživatel do vstupní soubor média přímo, jsme budete uveďte mezi ty oříznutí datového proudu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-465">Instead of linking the AVC encoders and speaker position assigner to the Media File Input directly, we'll put in between those the stream trimmer.</span></span> <span data-ttu-id="bb0b5-466">(Jeden pro video signál a jeden pro prokládaná zvukový signál.)</span><span class="sxs-lookup"><span data-stu-id="bb0b5-466">(One for the video signal and one for the interleaved audio signal.)</span></span>

![V rozmezí PUT oříznutí datového proudu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

<span data-ttu-id="bb0b5-468">*V rozmezí PUT oříznutí datového proudu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-468">*Put Stream Trimmer in between*</span></span>

<span data-ttu-id="bb0b5-469">Umožňuje nakonfigurovat oříznutí tak, aby jsme pouze zpracovat videa a zvuku mezi 15 sekund a ve videu 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-469">Let's configure the trimmer so that we will only process video and audio between 15 seconds and 60 seconds in the video.</span></span>

<span data-ttu-id="bb0b5-470">Přejděte do vlastností oříznutí datový proud videa a nakonfigurovat čas spuštění (15s) a vlastnosti čas ukončení (60s).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-470">Go to the properties of the Video Stream Trimmer and configure both Start Time (15s) and End Time (60s) properties.</span></span> <span data-ttu-id="bb0b5-471">Pokud chcete mít jistotu, že jsou obě naše audio a video oříznutí vždy nakonfigurovaná na stejné počáteční a koncová hodnota, bude publikujeme těch, které se s kořenem pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-471">To make sure both our audio and video trimmer are always configured to the same start and end values, we will publish those to the root of the workflow.</span></span>

![Počáteční čas vlastnost z datového proudu oříznutí publikování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

<span data-ttu-id="bb0b5-473">*Počáteční čas vlastnost z datového proudu oříznutí publikování*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-473">*Publish start time property from Stream Trimmer*</span></span>

![Publikování dialogové okno vlastností pro spuštění](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

<span data-ttu-id="bb0b5-475">*Publikování dialogové okno vlastností pro spuštění*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-475">*Publish property dialog for start time*</span></span>

![Publikování dialogové okno vlastností pro koncový čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

<span data-ttu-id="bb0b5-477">*Publikování dialogové okno vlastností pro koncový čas*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-477">*Publish property dialog for end time*</span></span>

<span data-ttu-id="bb0b5-478">Pokud jsme teď prověřit kořenu naše pracovního postupu, bude obě vlastnosti zobrazit a konfigurovat přehledně odtud.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-478">If we now inspect the root of our workflow, both properties will be neatly displayed and configurable from there.</span></span>

![Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

<span data-ttu-id="bb0b5-480">*Publikované vlastnosti, které jsou k dispozici v kořenovém adresáři*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-480">*Published properties available on root*</span></span>

<span data-ttu-id="bb0b5-481">Nyní otevřete vlastnosti oříznutí z zvuk oříznutí a nakonfigurujte počátečního a koncového času s výrazem, který odkazuje na publikovaná vlastnosti v kořenovém adresáři naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-481">Now open the trimming properties from the audio trimmer and configure both start and end times with an expression that refers to the published properties on the root of our workflow.</span></span>

<span data-ttu-id="bb0b5-482">Audio ořezávání čas spuštění:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-482">For the audio trimming start time:</span></span>

    ${ROOT_TrimmingStartTime}

<span data-ttu-id="bb0b5-483">a pro její koncový čas:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-483">and for its end time:</span></span>

    ${ROOT_TrimmingEndTime}

### <span data-ttu-id="bb0b5-484"><a id="time_based_trim_finish"></a>Dokončení pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-484"><a id="time_based_trim_finish"></a>Finished Workflow</span></span>
![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

<span data-ttu-id="bb0b5-486">*Dokončení pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-486">*Finished Workflow*</span></span>

## <span data-ttu-id="bb0b5-487"><a id="scripting"></a>Představení komponentu pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-487"><a id="scripting"></a>Introducing the Scripted Component</span></span>
<span data-ttu-id="bb0b5-488">Skriptované součásti můžete spustit libovolný skripty během provádění fáze naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-488">Scripted Components can execute arbitrary scripts during the execution phases of our workflow.</span></span> <span data-ttu-id="bb0b5-489">Existují čtyři jiné skripty, které mohou být provedeny, každý s konkrétními vlastnostmi a jejich vlastní místo v životním cyklu pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-489">There are four different scripts that can be executed, each with specific characteristics and their own place in the workflow life-cycle:</span></span>

* <span data-ttu-id="bb0b5-490">**commandScript**</span><span class="sxs-lookup"><span data-stu-id="bb0b5-490">**commandScript**</span></span>
* <span data-ttu-id="bb0b5-491">**realizeScript**</span><span class="sxs-lookup"><span data-stu-id="bb0b5-491">**realizeScript**</span></span>
* <span data-ttu-id="bb0b5-492">**processInputScript**</span><span class="sxs-lookup"><span data-stu-id="bb0b5-492">**processInputScript**</span></span>
* <span data-ttu-id="bb0b5-493">**lifeCycleScript**</span><span class="sxs-lookup"><span data-stu-id="bb0b5-493">**lifeCycleScript**</span></span>

<span data-ttu-id="bb0b5-494">V dokumentaci komponentu skriptování přejde podrobněji pro každou z výše.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-494">The documentation of the Scripted Component goes in more detail for each of the above.</span></span> <span data-ttu-id="bb0b5-495">V [v následující části](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), **realizeScript** skriptování součásti se používá pro konstrukci cliplist xml za chodu při spuštění pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-495">In [the following section](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), the **realizeScript** scripting component is used to construct a cliplist xml on the fly when the workflow starts.</span></span> <span data-ttu-id="bb0b5-496">Tento skript je volána v průběhu instalace součástí, která se dělá jenom jednou v jeho průběhu životního cyklu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-496">This script is called during the component setup, which happens only once in it's lifecycle.</span></span>

### <span data-ttu-id="bb0b5-497"><a id="scripting_hello_world"></a>Skriptování v rámci pracovního postupu: hello, world</span><span class="sxs-lookup"><span data-stu-id="bb0b5-497"><a id="scripting_hello_world"></a>Scripting within a workflow: hello world</span></span>
<span data-ttu-id="bb0b5-498">Přetáhněte součást skripty na plochu návrháře a přejmenujte ji (například "SetClipListXML").</span><span class="sxs-lookup"><span data-stu-id="bb0b5-498">Drag a Scripted Component onto the designer surface and rename it (for example, "SetClipListXML").</span></span>

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="bb0b5-500">*Přidání skriptované komponenty*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-500">*Adding a Scripted Component*</span></span>

<span data-ttu-id="bb0b5-501">Pokud si prohlédnout vlastnosti komponenty skriptování, čtyři typy jiný skript bude uvedené, se každý konfigurovat, a další skript.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-501">When you inspect the properties of the Scripted Component, the four different script types will be shown, each configurable to a different script.</span></span>

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="bb0b5-503">*Skriptované vlastnosti komponenty*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-503">*Scripted Component properties*</span></span>

<span data-ttu-id="bb0b5-504">Vymažte processInputScript a otevřete editor pro realizeScript.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-504">Clear the processInputScript and open the editor for the realizeScript.</span></span> <span data-ttu-id="bb0b5-505">Nyní jsme se nastavené a připravené ke spuštění skriptování.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-505">Now we're set up and ready to start scripting.</span></span>

<span data-ttu-id="bb0b5-506">Jazyk Groovy dynamicky kompilované skriptovací jazyk pro platformu Java, která uchovává kompatibilitu s Java.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-506">Scripts are written in Groovy, a dynamically compiled scripting language for the Java platform that retains compatibility with Java.</span></span> <span data-ttu-id="bb0b5-507">Většinu kódu v jazyce Java ve skutečnosti, je platný Groovy kód.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-507">Actually, most Java code is valid Groovy code.</span></span>

<span data-ttu-id="bb0b5-508">Můžete napsat skript groovy jednoduché hello world v kontextu naše realizeScript.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-508">Let's write a simple hello world groovy script in the context of our realizeScript.</span></span> <span data-ttu-id="bb0b5-509">V editoru zadejte následující údaje:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-509">Enter the following in the editor:</span></span>

    node.log("hello world");

<span data-ttu-id="bb0b5-510">Teď spusťte místní testovací běh.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-510">Now execute a local test run.</span></span> <span data-ttu-id="bb0b5-511">Po této spustit zkontrolujte (prostřednictvím kartě systému na komponentu skripty) vlastnost protokoly.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-511">After this run, inspect (through the System tab on the Scripted Component) the Logs property.</span></span>

![Výstup Hello world protokolu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

<span data-ttu-id="bb0b5-513">*Výstup Hello world protokolu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-513">*Hello world log output*</span></span>

<span data-ttu-id="bb0b5-514">Objekt uzlu, který jsme volat metodu protokolu, odkazuje na našich aktuální "uzel" nebo součást, kterou jsme se skriptování v rámci.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-514">The node object we call the log method on, refers to our current "node" or the component we're scripting within.</span></span> <span data-ttu-id="bb0b5-515">Všechny komponenty jako takový má schopnost výstupní protokolování data, k dispozici prostřednictvím kartě systému. V takovém případě jsme výstupní řetězcový literál "hello, world".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-515">Every component as such has the ability to output logging data, available through the system tab. In this case, we output the string literal "hello world".</span></span> <span data-ttu-id="bb0b5-516">Důležité si uvědomit, zde je, že to může být být neocenitelnou pomocí ladicí nástroj vám poskytnou přehled na skript je ve skutečnosti činnosti.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-516">Important to understand here is that this can prove to be an invaluable debugging tool, providing you with insight on what the script is actually doing.</span></span>

<span data-ttu-id="bb0b5-517">Z našich skriptovací prostředí máme také přístup k vlastnostem na dalších komponentách.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-517">From within our scripting environment, we also have access to properties on other components.</span></span> <span data-ttu-id="bb0b5-518">Zkuste provést následující:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-518">Try this:</span></span>

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up to other nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

<span data-ttu-id="bb0b5-519">Naše protokolů v okně se zobrazí nám následující:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-519">Our log window will show us the following:</span></span>

![Výstup protokolu pro přístup k uzlu cesty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

<span data-ttu-id="bb0b5-521">*Výstup protokolu pro přístup k uzlu cesty*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-521">*Log output for accessing node paths*</span></span>

## <span data-ttu-id="bb0b5-522"><a id="frame_based_trim"></a>Na základě rámce oříznutí multibitrate MP4 výstupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-522"><a id="frame_based_trim"></a>Frame-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="bb0b5-523">Od pracovní postup, který generuje [multibitrate MP4 výstup z MXF vstup](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), jsme se teď vyhledávání do ořezávání zdroj videa na základě počtu rámce.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-523">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming the source video based on frame counts.</span></span>

### <span data-ttu-id="bb0b5-524"><a id="frame_based_trim_start"></a>Přehled plán, podle kterého chcete začít přidávat oříznutí na</span><span class="sxs-lookup"><span data-stu-id="bb0b5-524"><a id="frame_based_trim_start"></a>Blueprint overview to start adding trimming to</span></span>
![Pracovní postup, pokud chcete začít přidávat oříznutí na](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

<span data-ttu-id="bb0b5-526">*Pracovní postup, pokud chcete začít přidávat oříznutí na*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-526">*Workflow to start adding trimming to*</span></span>

### <span data-ttu-id="bb0b5-527"><a id="frame_based_trim_clip_list"></a>Pomocí seznamu klip XML</span><span class="sxs-lookup"><span data-stu-id="bb0b5-527"><a id="frame_based_trim_clip_list"></a>Using the Clip List XML</span></span>
<span data-ttu-id="bb0b5-528">Ve všech předchozích kurzech pracovního postupu jsme použili komponentu vstupní soubor média jako naše vstupní zdroj videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-528">In all previous workflow tutorials, we used the Media File Input component as our video input source.</span></span> <span data-ttu-id="bb0b5-529">Pro tento konkrétní scénář, když budeme používat klip seznamu zdrojovou součástí místo.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-529">For this specific scenario though, we'll be using the Clip List Source component instead.</span></span> <span data-ttu-id="bb0b5-530">Všimněte si, že nemělo by se jednat upřednostňovaný způsob práce; Zdroj seznamu klip používat jenom po skutečný důvod k tomu (stejně jako v nástroji níže případu, kdy provádíme za využívat možnosti klip seznamu oříznutí).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-530">Note that this should not be the preferred way of working; only use the Clip List Source when there's a real reason to do so (like in the below case, where we're making use of the clip list trimming capabilities).</span></span>

<span data-ttu-id="bb0b5-531">Přepnutí z našich vstupní soubor média ke zdroji klip seznamu, přetáhněte klip seznamu zdrojovou součástí na návrhovou plochu a PIN kód XML seznamu klip se připojit k uzlu XML seznamu klip Návrháře pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-531">To switch from our Media File Input to the Clip List Source, drag the Clip List Source component onto the design surface and connect the Clip List XML pin to the Clip List XML node of the workflow designer.</span></span> <span data-ttu-id="bb0b5-532">Zdroj seznamu klip s výstupní kód PIN, to by měl naplnit podle našich vstup videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-532">This should populate the Clip List Source with output pins, according to our input video.</span></span> <span data-ttu-id="bb0b5-533">Teď připojit nekomprimované videa a zvuku nekomprimované PIN kódů z zdroji klip seznamu příslušné AVC kodéry a Interleaver zvuk datového proudu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-533">Now connect the Uncompressed Video and Uncompressed Audio pins from the the Clip List Source to the respective AVC Encoders and Audio Stream Interleaver.</span></span> <span data-ttu-id="bb0b5-534">Nyní odeberte vstupní soubor média.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-534">Now remove the Media File Input.</span></span>

![Vstupní soubor média nahradí zdroji klip seznamu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

<span data-ttu-id="bb0b5-536">*Vstupní soubor média nahradí zdroji klip seznamu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-536">*Replaced the Media File Input with the Clip List Source*</span></span>

<span data-ttu-id="bb0b5-537">Klip seznamu zdrojovou součástí vezme jako vstupní "seznamu XML klip".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-537">The Clip List Source component takes as its input a "Clip List XML".</span></span> <span data-ttu-id="bb0b5-538">Když vyberete zdrojový soubor, který testování s místně, tento klip seznamu xml je automaticky vyplněna za vás.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-538">When selecting the source file to test with locally, this clip list xml is auto-populated for you.</span></span>

![Automaticky vyplněna klip seznamu XML – vlastnost](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

<span data-ttu-id="bb0b5-540">*Automaticky vyplněna klip seznamu XML – vlastnost*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-540">*Auto-populated Clip List XML property*</span></span>

<span data-ttu-id="bb0b5-541">Vyhledávání poněkud blíže xml, to je, jak vypadá jako:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-541">Looking a bit closer to the xml, this is how it looks like:</span></span>

![Seznam klip dialogové okno Upravit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

<span data-ttu-id="bb0b5-543">*Seznam klip dialogové okno Upravit*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-543">*Edit clip list dialog*</span></span>

<span data-ttu-id="bb0b5-544">To ale nemusí odpovídat možnosti klip seznamu xml.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-544">This however does not reflect the capabilities of the clip list xml.</span></span> <span data-ttu-id="bb0b5-545">Jednou z možností, které máme je přidání "Trim" prvek v rámci obou videa a zvuku zdroj, například takto:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-545">One option we have is to add a "Trim" element under both the video and audio source, like this:</span></span>

![Přidání do seznamu klip element a uvolnění dočasné paměti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

<span data-ttu-id="bb0b5-547">*Přidání do seznamu klip element a uvolnění dočasné paměti*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-547">*Adding a trim element to the clip list*</span></span>

<span data-ttu-id="bb0b5-548">Je-li upravit xml seznamu klip takto výše a místní spustit test, zobrazí se na video správně byl oříznut mezi 10 a 20 sekund na videu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-548">If you modify the clip list xml like this above and perform a local test run, you'll see the video correctly been trimmed between 10 and 20 seconds in the video.</span></span>

<span data-ttu-id="bb0b5-549">Tato velmi stejné xml cliplist rozporu s touto co se stane, když provedete místní spustit, když, neměli stejného efektu při použití v pracovní postup, který běží v Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-549">Contrary to what happens when you do a local run though, this very same cliplist xml would not have the same effect when applied in a workflow that runs in Azure Media Services.</span></span> <span data-ttu-id="bb0b5-550">Při spuštění služby Azure Premium kodér cliplist xml je generována pokaždé, když znovu podle vstupní soubor, který byl zadán úlohy kódování.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-550">When Azure Premium Encoder starts, the cliplist xml is generated every time again, based on the input file the encoding job was given.</span></span> <span data-ttu-id="bb0b5-551">To znamená, že by všechny změny, které jsme provést na xml bohužel přepsat.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-551">This means that any changes we do on the xml would unfortunately be overridden.</span></span>

<span data-ttu-id="bb0b5-552">Pro čítače xml cliplist vymazáním při kódování úloha se spustila, jsme lze znovu vygenerovat za chodu právě po začátku naše pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-552">To counter the cliplist xml being wiped when an encoding job is started, we can re-generate it on the fly just after the start of our workflow.</span></span> <span data-ttu-id="bb0b5-553">Prostřednictvím co se nazývá "Skriptování Component" můžete provedeny takové vlastní akce.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-553">Such custom actions can be taken through what is called a "Scripted Component".</span></span> <span data-ttu-id="bb0b5-554">Další informace najdete v tématu [představení komponentu skripty](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span><span class="sxs-lookup"><span data-stu-id="bb0b5-554">For more information, see [Introducing the Scripted Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span></span>

<span data-ttu-id="bb0b5-555">Přetáhněte součást skripty na plochu návrháře a přejmenujte jej na "SetClipListXML".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-555">Drag a Scripted Component onto the designer surface and rename it to "SetClipListXML".</span></span>

![Přidání skriptované komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="bb0b5-557">*Přidání skriptované komponenty*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-557">*Adding a Scripted Component*</span></span>

<span data-ttu-id="bb0b5-558">Pokud si prohlédnout vlastnosti komponenty skriptování, čtyři typy jiný skript bude uvedené, se každý konfigurovat, a další skript.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-558">When you inspect the properties of the Scripted Component, the four different script types will be shown, each configurable to a different script.</span></span>

![Skriptované vlastnosti komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="bb0b5-560">*Skriptované vlastnosti komponenty*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-560">*Scripted Component properties*</span></span>

### <span data-ttu-id="bb0b5-561"><a id="frame_based_trim_modify_clip_list"></a>Úprava seznamu klip z součásti skriptování</span><span class="sxs-lookup"><span data-stu-id="bb0b5-561"><a id="frame_based_trim_modify_clip_list"></a>Modifying the clip list from a Scripted Component</span></span>
<span data-ttu-id="bb0b5-562">Než znovu jsme může zapisovat cliplist xml, který je generován během spouštění pracovního postupu, budeme potřebovat přístup k cliplist xml vlastnosti a obsah.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-562">Before we can re-write the cliplist xml that is generated during workflow startup, we'll need to have access to the cliplist xml property and contents.</span></span> <span data-ttu-id="bb0b5-563">Jsme, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-563">We can do so like this:</span></span>

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Příchozí klip seznamu protokolována](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

<span data-ttu-id="bb0b5-565">*Příchozí klip seznamu protokolována*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-565">*Incoming clip list being logged*</span></span>

<span data-ttu-id="bb0b5-566">Nejdřív potřebujeme způsob, jak určit z bod, který do bod, který má být trim videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-566">First we need a way to determine from which point till which point we want to trim the video.</span></span> <span data-ttu-id="bb0b5-567">Chcete-li to vhodné pro uživatele bez technical pracovního postupu, publikujte dvě vlastnosti do kořenového adresáře grafu.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-567">To make this convenient to the less-technical user of the workflow, publish two properties to the root of the graph.</span></span> <span data-ttu-id="bb0b5-568">Chcete-li to provést, klikněte pravým tlačítkem na plochu návrháře a vyberte "Přidat vlastnost":</span><span class="sxs-lookup"><span data-stu-id="bb0b5-568">To do this, right click the designer surface and select "Add Property":</span></span>

* <span data-ttu-id="bb0b5-569">První vlastnost: "ClippingTimeStart" typu: "Časový kód"</span><span class="sxs-lookup"><span data-stu-id="bb0b5-569">First property: "ClippingTimeStart" of type: "TIMECODE"</span></span>
* <span data-ttu-id="bb0b5-570">Druhé vlastnosti: "ClippingTimeEnd" typu: "Časový kód"</span><span class="sxs-lookup"><span data-stu-id="bb0b5-570">Second property: "ClippingTimeEnd" of type: "TIMECODE"</span></span>

![Přidat dialogové okno vlastností pro výstřižek počáteční čas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

<span data-ttu-id="bb0b5-572">*Přidat dialogové okno vlastností pro výstřižek počáteční čas*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-572">*Add Property dialog for clipping start time*</span></span>

![Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

<span data-ttu-id="bb0b5-574">*Publikovaná výstřižek props čas v kořenovém adresáři pracovního postupu*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-574">*Published clipping time props on workflow root*</span></span>

<span data-ttu-id="bb0b5-575">Nakonfigurujte obě vlastnosti vhodný hodnota:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-575">Configure both properties to a suitable value:</span></span>

![Nakonfigurujte výstřižek spuštění a ukončení vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

<span data-ttu-id="bb0b5-577">*Nakonfigurujte výstřižek spuštění a ukončení vlastnosti*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-577">*Configure the clipping start and end properties*</span></span>

<span data-ttu-id="bb0b5-578">Nyní z v rámci naší skriptu jsme přístup k obě vlastnosti, jako je to:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-578">Now, from within our script, we can access both properties, like this:</span></span>

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Okno Protokol zobrazující začátku a konci výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

<span data-ttu-id="bb0b5-580">*Okno Protokol zobrazující začátku a konci výstřižek*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-580">*Log window showing start and end of clipping*</span></span>

<span data-ttu-id="bb0b5-581">Umožňuje analyzovat řetězce časový kód do pohodlnější použití formuláře pomocí jednoduchého regulární výraz:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-581">Let's parse the timecode strings into a more convenient to use form, using a simple regular expression:</span></span>

    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Okno Protokol s výstupem Analyzovaná časový kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

<span data-ttu-id="bb0b5-583">*Okno Protokol s výstupem Analyzovaná časový kód*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-583">*Log window with output of parsed timecode*</span></span>

<span data-ttu-id="bb0b5-584">Pomocí těchto informací po ruce jsme nyní můžete upravit cliplist xml tak, aby odrážela počáteční a koncový čas pro požadovanou rámce přesné výstřižek videa.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-584">With this information at hand, we can now modify the cliplist xml to reflect the start and end times for the desired frame-accurate clipping of the movie.</span></span>

![Kód skriptu přidat trim elementy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

<span data-ttu-id="bb0b5-586">*Kód skriptu přidat trim elementy*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-586">*Script code to add trim elements*</span></span>

<span data-ttu-id="bb0b5-587">K tomu bylo potřeba prostřednictvím operace manipulace s řetězci normální.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-587">This was done through normal string manipulation operations.</span></span> <span data-ttu-id="bb0b5-588">Výsledný seznam xml upravené klip se nezapisují zpět na vlastnost clipListXML v kořenovém adresáři pracovního postupu prostřednictvím metody "SetProperty –".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-588">The resulting modified clip list xml is written back to the clipListXML property on the workflow root through the "setProperty" method.</span></span> <span data-ttu-id="bb0b5-589">Okno Protokol po provedení jiného testu by zobrazit nám následující:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-589">The log window after another test run would show us the following:</span></span>

![V rozevíracím seznamu klip protokolování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

<span data-ttu-id="bb0b5-591">*V rozevíracím seznamu klip protokolování*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-591">*Logging the resulting clip list*</span></span>

<span data-ttu-id="bb0b5-592">Proveďte spuštění testu chcete zobrazit, jak mají byl oříznut datové proudy videa a zvuku.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-592">Do a test-run to see how the video and audio streams have been clipped.</span></span> <span data-ttu-id="bb0b5-593">Jako více než jeden testovací běh s různými hodnotami pro body oříznutí, můžete to udělat, můžete si všimnout, že ty, nebude vzít v úvahu ale!</span><span class="sxs-lookup"><span data-stu-id="bb0b5-593">As you'll do more than one test-run with different values for the trimming points, you'll notice that those will not be taken into account however!</span></span> <span data-ttu-id="bb0b5-594">Důvodem je, že designer, na rozdíl od Azure modulu runtime nemůže přepsat cliplist xml každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-594">The reason for this is that the designer, unlike the Azure runtime, does NOT override the cliplist xml every run.</span></span> <span data-ttu-id="bb0b5-595">To znamená, že pouze při prvním jste nastavili počáteční body a způsobí, že soubor xml k transformaci, všechny ostatní časy naše klauzule ochrana (pokud (clipListXML.indexOf ("<trim>") == -1)) zabránit pracovní postup přidání jiný element uvolnění dočasné paměti při je již jedna nachází.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-595">This means that only the very first time you have set the in and out points, will cause the xml to transform, all the other times, our guard clause (if(clipListXML.indexOf("<trim>") == -1)) will prevent the workflow from adding another trim element when there's already one present.</span></span>

<span data-ttu-id="bb0b5-596">Pokud chcete, aby naše pracovního postupu vhodné k testování místně, nejlepší přidáme některé údržby kód, který kontroluje, pokud element a uvolnění dočasné paměti byl již existuje.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-596">To make our workflow convenient to test locally, we best add some house-keeping code that inspects if a trim element was already present.</span></span> <span data-ttu-id="bb0b5-597">Pokud ano, jsme ho odebrat před pokračováním změnou xml s novými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-597">If so, we can remove it before continuing by modifying the xml with the new values.</span></span> <span data-ttu-id="bb0b5-598">Místo použití manipulace prostý řetězec, je pravděpodobně bezpečnější to provést prostřednictvím skutečné xml objektový model analýze.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-598">Rather than using plain string manipulations, it's probably safer to do this through real xml object model parsing.</span></span>

<span data-ttu-id="bb0b5-599">Předtím, než takový kód jsme můžete přidat, i když budeme potřebovat nejprve přidat počet příkazů pro import na začátku naše skriptu:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-599">Before we can add such code though, we'll need to add a number of import statements at the start of our script first:</span></span>

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

<span data-ttu-id="bb0b5-600">Potom přidáme kód vyžaduje čištění:</span><span class="sxs-lookup"><span data-stu-id="bb0b5-600">After this, we can add the required cleaning code:</span></span>

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

<span data-ttu-id="bb0b5-601">Tento kód jenom překročí bodu, na které přidáme trim elementy k cliplist xml.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-601">This code goes just above the point at which we add the trim elements to the cliplist xml.</span></span>

<span data-ttu-id="bb0b5-602">V tuto chvíli jsme můžete spustit a změnit naše pracovní postup, jak tolik, jako chceme přitom má někdy použity změny času.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-602">At this point, we can run and modify our workflow as much as we want while having the changes applied ever time.</span></span>    

### <span data-ttu-id="bb0b5-603"><a id="frame_based_trim_clippingenabled_prop"></a>Přidání vlastnosti ClippingEnabled usnadnění práce</span><span class="sxs-lookup"><span data-stu-id="bb0b5-603"><a id="frame_based_trim_clippingenabled_prop"></a>Adding a ClippingEnabled convenience property</span></span>
<span data-ttu-id="bb0b5-604">Jak je možné, že chcete, aby ořezávání provést, jsou umožňuje dokončit vypnout naše pracovního postupu přidáním pohodlný logický příznak, který označuje, zda chcete povolit ořezávání / výstřižek.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-604">As you might not always want trimming to happen, let's finish off our workflow by adding a convenient boolean flag that indicates whether or not we want to enable trimming / clipping.</span></span>

<span data-ttu-id="bb0b5-605">Stejně jako dřív, a publikovat nové vlastnosti do kořenového adresáře naše pracovní postup volá "ClippingEnabled" z typu "logická hodnota".</span><span class="sxs-lookup"><span data-stu-id="bb0b5-605">Just as before, publish a new property to the root of our workflow called "ClippingEnabled" of type "BOOLEAN".</span></span>

![Publikovaná vlastnost pro povolení výstřižek](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

<span data-ttu-id="bb0b5-607">*Publikovaná vlastnost pro povolení výstřižek*</span><span class="sxs-lookup"><span data-stu-id="bb0b5-607">*Published a property for enabling clipping*</span></span>

<span data-ttu-id="bb0b5-608">Pomocí níže jednoduché ochrana klauzule jsme můžete zkontrolujte, zda je vyžadován oříznutí a rozhodnout, pokud naše klip seznam jako takový musí upravit, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="bb0b5-608">With the below simple guard clause, we can check if trimming is required and decide if our clip list as such needs to be modified or not.</span></span>

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <span data-ttu-id="bb0b5-609"><a id="code"></a>Kompletní kód</span><span class="sxs-lookup"><span data-stu-id="bb0b5-609"><a id="code"></a>Complete code</span></span>
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

    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from the clip list xml by parsing the xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
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

    //add trim elements to cliplist xml
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


## <a name="also-see"></a><span data-ttu-id="bb0b5-610">Viz také</span><span class="sxs-lookup"><span data-stu-id="bb0b5-610">Also see</span></span>
[<span data-ttu-id="bb0b5-611">Představení Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="bb0b5-611">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[<span data-ttu-id="bb0b5-612">Jak používat Premium kódování v Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="bb0b5-612">How to Use Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[<span data-ttu-id="bb0b5-613">Kódování obsahu na vyžádání pomocí Azure Media Service</span><span class="sxs-lookup"><span data-stu-id="bb0b5-613">Encoding On-Demand Content with Azure Media Service</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

[<span data-ttu-id="bb0b5-614">Formáty a kodeky pracovního postupu Media Encoderu Premium</span><span class="sxs-lookup"><span data-stu-id="bb0b5-614">Media Encoder Premium Workflow Formats and Codecs</span></span>](media-services-premium-workflow-encoder-formats.md)

[<span data-ttu-id="bb0b5-615">Ukázkové soubory pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bb0b5-615">Sample workflow files</span></span>](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[<span data-ttu-id="bb0b5-616">Nástroj pro Azure Media Services Explorer</span><span class="sxs-lookup"><span data-stu-id="bb0b5-616">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="bb0b5-617">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="bb0b5-617">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bb0b5-618">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="bb0b5-618">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
