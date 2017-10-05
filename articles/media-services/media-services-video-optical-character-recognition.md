---
title: "Digitalizace textu pomocí Azure Media Analytics rozpoznávání znaků | Microsoft Docs"
description: "Rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků) umožňuje převést textového obsahu v video soubory upravovat, vyhledávat digitální text.  To umožňuje automatizovat extrakce smysluplný metadata z video signál média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 43f5b3a9bbec243e668c79702045094fcfedbdda
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a><span data-ttu-id="55ffb-104">Použití Azure Media Analytics k převodu textového obsahu v videosouborů na digitální text</span><span class="sxs-lookup"><span data-stu-id="55ffb-104">Use Azure Media Analytics to convert text content in video files into digital text</span></span>
## <a name="overview"></a><span data-ttu-id="55ffb-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="55ffb-105">Overview</span></span>
<span data-ttu-id="55ffb-106">Pokud potřebujete k extrahování obsahu text z video soubory a generování upravovat, vyhledávat digitální text, měli byste použít rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků).</span><span class="sxs-lookup"><span data-stu-id="55ffb-106">If you need to extract text content from your video files and generate an editable, searchable digital text, you should use Azure Media Analytics OCR (optical character recognition).</span></span> <span data-ttu-id="55ffb-107">Tento procesor médií Azure zjistí textového obsahu v video soubory a vygeneruje textových souborů pro vaše použití.</span><span class="sxs-lookup"><span data-stu-id="55ffb-107">This Azure Media Processor detects text content in your video files and generates text files for your use.</span></span> <span data-ttu-id="55ffb-108">Rozpoznávání znaků umožňuje automatizovat extrakce smysluplný metadata z video signál média.</span><span class="sxs-lookup"><span data-stu-id="55ffb-108">OCR enables you to automate the extraction of meaningful metadata from the video signal of your media.</span></span>

<span data-ttu-id="55ffb-109">Při použití ve spojení s vyhledávacího webu, můžete snadno indexu médiu podle textu a vylepšit možnosti rozpoznání obsahu.</span><span class="sxs-lookup"><span data-stu-id="55ffb-109">When used in conjunction with a search engine, you can easily index your media by text, and enhance the discoverability of your content.</span></span> <span data-ttu-id="55ffb-110">To je velmi užitečné v vysoce textovou video, jako je záznam videa nebo snímek obrazovky prezentace prezentace.</span><span class="sxs-lookup"><span data-stu-id="55ffb-110">This is extremely useful in highly textual video, like a video recording or screen-capture of a slideshow presentation.</span></span> <span data-ttu-id="55ffb-111">Procesor médií rozpoznávání znaků Azure je optimalizovaná pro digitální text.</span><span class="sxs-lookup"><span data-stu-id="55ffb-111">The Azure OCR Media Processor is optimized for digital text.</span></span>

<span data-ttu-id="55ffb-112">**Rozpoznávání Azure Media znaků** procesor médií je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="55ffb-112">The **Azure Media OCR** media processor is currently in Preview.</span></span>

<span data-ttu-id="55ffb-113">Toto téma uvádí podrobnosti o **rozpoznávání Azure Media znaků** a ukazuje, jak pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="55ffb-113">This topic gives details about  **Azure Media OCR** and shows how to use it with Media Services SDK for .NET.</span></span> <span data-ttu-id="55ffb-114">Další informace a příklady naleznete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span><span class="sxs-lookup"><span data-stu-id="55ffb-114">For additional information and examples, see [this blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span></span>

## <a name="ocr-input-files"></a><span data-ttu-id="55ffb-115">Vstupní soubory rozpoznávání znaků</span><span class="sxs-lookup"><span data-stu-id="55ffb-115">OCR input files</span></span>
<span data-ttu-id="55ffb-116">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="55ffb-116">Video files.</span></span> <span data-ttu-id="55ffb-117">V současné době jsou podporovány následující formáty: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="55ffb-117">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration"></a><span data-ttu-id="55ffb-118">Konfigurace úlohy</span><span class="sxs-lookup"><span data-stu-id="55ffb-118">Task configuration</span></span>
<span data-ttu-id="55ffb-119">Konfigurace úlohy (přednastavených).</span><span class="sxs-lookup"><span data-stu-id="55ffb-119">Task configuration (preset).</span></span> <span data-ttu-id="55ffb-120">Při vytváření úlohy s **rozpoznávání Azure Media znaků**, je nutné zadat konfiguraci přednastavení pomocí XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="55ffb-120">When creating a task with **Azure Media OCR**, you must specify a configuration preset using JSON  or XML.</span></span> 

>[!NOTE]
><span data-ttu-id="55ffb-121">Modul rozpoznávání znaků pouze jako platný vstup v obou výška a šířka trvá oblast bitové kopie s minimální 40 pixelů na maximální délku 32 000 pixelů.</span><span class="sxs-lookup"><span data-stu-id="55ffb-121">The OCR engine only takes an image region with minimum 40 pixels to maximum 32000 pixels as a valid input in both height/width.</span></span>
>

### <a name="attribute-descriptions"></a><span data-ttu-id="55ffb-122">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="55ffb-122">Attribute descriptions</span></span>
| <span data-ttu-id="55ffb-123">Název atributu</span><span class="sxs-lookup"><span data-stu-id="55ffb-123">Attribute name</span></span> | <span data-ttu-id="55ffb-124">Popis</span><span class="sxs-lookup"><span data-stu-id="55ffb-124">Description</span></span> |
| --- | --- |
|<span data-ttu-id="55ffb-125">AdvancedOutput</span><span class="sxs-lookup"><span data-stu-id="55ffb-125">AdvancedOutput</span></span>| <span data-ttu-id="55ffb-126">Pokud nastavíte AdvancedOutput na hodnotu true, budou obsahovat výstup JSON poziční data pro každou jednoho slova (kromě frází a oblasti).</span><span class="sxs-lookup"><span data-stu-id="55ffb-126">If you set AdvancedOutput to true, the JSON output will contain positional data for every single word (in addition to phrases and regions).</span></span> <span data-ttu-id="55ffb-127">Pokud nechcete tyto podrobnosti zobrazíte, nastavte příznak na hodnotu false.</span><span class="sxs-lookup"><span data-stu-id="55ffb-127">If you do not want to see these details, set the flag to false.</span></span> <span data-ttu-id="55ffb-128">Výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="55ffb-128">The default value is false.</span></span> <span data-ttu-id="55ffb-129">Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span><span class="sxs-lookup"><span data-stu-id="55ffb-129">For more information, see [this blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span></span>|
| <span data-ttu-id="55ffb-130">Jazyk</span><span class="sxs-lookup"><span data-stu-id="55ffb-130">Language</span></span> |<span data-ttu-id="55ffb-131">(volitelné) popisuje jazyk textu, pro které chcete hledat.</span><span class="sxs-lookup"><span data-stu-id="55ffb-131">(optional) describes the language of text for which to look.</span></span> <span data-ttu-id="55ffb-132">Jeden z následujících: AutoDetect (výchozí), Arabské, ChineseSimplified, ChineseTraditional, čeština dánština, holandština, angličtina, finština, francouzština, němčina, řečtina, maďarština, italština, japonština, korejština, norština, polština, portugalština, rumunština, ruština, SerbianCyrillic, SerbianLatin, slovenština, španělština, švédština, turečtina.</span><span class="sxs-lookup"><span data-stu-id="55ffb-132">One of the following: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech Danish, Dutch, English, Finnish, French, German,  Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.</span></span> |
| <span data-ttu-id="55ffb-133">TextOrientation</span><span class="sxs-lookup"><span data-stu-id="55ffb-133">TextOrientation</span></span> |<span data-ttu-id="55ffb-134">(volitelné) popisuje orientaci textu, pro které chcete hledat.</span><span class="sxs-lookup"><span data-stu-id="55ffb-134">(optional) describes the orientation of text for which to look.</span></span>  <span data-ttu-id="55ffb-135">"Vlevo" znamená horní části všechna písmena jsou nasměruje levé straně.</span><span class="sxs-lookup"><span data-stu-id="55ffb-135">"Left" means that the top of all letters are pointed towards the left.</span></span>  <span data-ttu-id="55ffb-136">Výchozí text (např., které lze nalézt v podobě knihy) je možné volat "Nahoru" orientované.</span><span class="sxs-lookup"><span data-stu-id="55ffb-136">Default text (like that which can be found in a book) can be called "Up" oriented.</span></span>  <span data-ttu-id="55ffb-137">Jeden z následujících: AutoDetect (výchozí), až, vpravo, dolů, doleva.</span><span class="sxs-lookup"><span data-stu-id="55ffb-137">One of the following: AutoDetect (default), Up, Right, Down, Left.</span></span> |
| <span data-ttu-id="55ffb-138">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="55ffb-138">TimeInterval</span></span> |<span data-ttu-id="55ffb-139">(volitelné) popisuje míry vzorkování.</span><span class="sxs-lookup"><span data-stu-id="55ffb-139">(optional) describes the sampling rate.</span></span>  <span data-ttu-id="55ffb-140">Výchozí hodnota je každou sekundu 1/2.</span><span class="sxs-lookup"><span data-stu-id="55ffb-140">Default is every 1/2 second.</span></span><br/><span data-ttu-id="55ffb-141">Formát JSON – hh: mm:. Služby Zabezpečené úložiště (výchozí 00:00:00.500)</span><span class="sxs-lookup"><span data-stu-id="55ffb-141">JSON format – HH:mm:ss.SSS (default 00:00:00.500)</span></span><br/><span data-ttu-id="55ffb-142">Formát XML – doba trvání primitivní W3C XSD (výchozí PT0.5)</span><span class="sxs-lookup"><span data-stu-id="55ffb-142">XML format – W3C XSD duration primitive (default PT0.5)</span></span> |
| <span data-ttu-id="55ffb-143">DetectRegions</span><span class="sxs-lookup"><span data-stu-id="55ffb-143">DetectRegions</span></span> |<span data-ttu-id="55ffb-144">(volitelné) Pole objektů DetectRegion zadáte oblasti v rámci video rámce, ve kterém k detekci text.</span><span class="sxs-lookup"><span data-stu-id="55ffb-144">(optional) An array of DetectRegion objects specifying regions within the video frame in which to detect text.</span></span><br/><span data-ttu-id="55ffb-145">Objekt DetectRegion sestávající ze čtyř logického:</span><span class="sxs-lookup"><span data-stu-id="55ffb-145">A DetectRegion object is made of the following four integer values:</span></span><br/><span data-ttu-id="55ffb-146">Vlevo – pixelů z levého okraje</span><span class="sxs-lookup"><span data-stu-id="55ffb-146">Left – pixels from the left-margin</span></span><br/><span data-ttu-id="55ffb-147">TOP – pixelů z horní okraj</span><span class="sxs-lookup"><span data-stu-id="55ffb-147">Top – pixels from the top-margin</span></span><br/><span data-ttu-id="55ffb-148">Šířka – Šířka oblasti v pixelech</span><span class="sxs-lookup"><span data-stu-id="55ffb-148">Width – width of the region in pixels</span></span><br/><span data-ttu-id="55ffb-149">Výška – výšku oblasti v pixelech</span><span class="sxs-lookup"><span data-stu-id="55ffb-149">Height – height of the region in pixels</span></span> |

#### <a name="json-preset-example"></a><span data-ttu-id="55ffb-150">Příklad přednastavené JSON</span><span class="sxs-lookup"><span data-stu-id="55ffb-150">JSON preset example</span></span>

    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }


#### <a name="xml-preset-example"></a><span data-ttu-id="55ffb-151">Příklad přednastavené XML</span><span class="sxs-lookup"><span data-stu-id="55ffb-151">XML preset example</span></span>
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

## <a name="ocr-output-files"></a><span data-ttu-id="55ffb-152">Rozpoznávání znaků výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="55ffb-152">OCR output files</span></span>
<span data-ttu-id="55ffb-153">Výstup procesor médií rozpoznávání znaků je soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="55ffb-153">The output of the OCR media processor is a JSON file.</span></span>

### <a name="elements-of-the-output-json-file"></a><span data-ttu-id="55ffb-154">Elementy výstupního souboru JSON</span><span class="sxs-lookup"><span data-stu-id="55ffb-154">Elements of the output JSON file</span></span>
<span data-ttu-id="55ffb-155">Výstup Video rozpoznávání znaků poskytuje segmentované čas data na znaky v videa nalezen.</span><span class="sxs-lookup"><span data-stu-id="55ffb-155">The Video OCR output provides time-segmented data on the characters found in your video.</span></span>  <span data-ttu-id="55ffb-156">Atributy, jako je například jazyk nebo orientaci můžete použít k hone-in na přesně slova, že máte zájem analýza.</span><span class="sxs-lookup"><span data-stu-id="55ffb-156">You can use attributes such as language or orientation to hone-in on exactly the words that you are interested in analyzing.</span></span> 

<span data-ttu-id="55ffb-157">Výstup obsahuje následující atributy:</span><span class="sxs-lookup"><span data-stu-id="55ffb-157">The output contains the following attributes:</span></span>

| <span data-ttu-id="55ffb-158">Element</span><span class="sxs-lookup"><span data-stu-id="55ffb-158">Element</span></span> | <span data-ttu-id="55ffb-159">Popis</span><span class="sxs-lookup"><span data-stu-id="55ffb-159">Description</span></span> |
| --- | --- |
| <span data-ttu-id="55ffb-160">Časová osa</span><span class="sxs-lookup"><span data-stu-id="55ffb-160">Timescale</span></span> |<span data-ttu-id="55ffb-161">"rysky" za sekundu videa</span><span class="sxs-lookup"><span data-stu-id="55ffb-161">"ticks" per second of the video</span></span> |
| <span data-ttu-id="55ffb-162">Posun</span><span class="sxs-lookup"><span data-stu-id="55ffb-162">Offset</span></span> |<span data-ttu-id="55ffb-163">časového posunu pro časová razítka.</span><span class="sxs-lookup"><span data-stu-id="55ffb-163">time offset for timestamps.</span></span> <span data-ttu-id="55ffb-164">Ve verzi 1.0 rozhraní API, Video bude vždy 0.</span><span class="sxs-lookup"><span data-stu-id="55ffb-164">In version 1.0 of Video APIs, this will always be 0.</span></span> |
| <span data-ttu-id="55ffb-165">kmitočet snímků</span><span class="sxs-lookup"><span data-stu-id="55ffb-165">Framerate</span></span> |<span data-ttu-id="55ffb-166">Počet snímků za sekundu videa</span><span class="sxs-lookup"><span data-stu-id="55ffb-166">Frames per second of the video</span></span> |
| <span data-ttu-id="55ffb-167">Šířka</span><span class="sxs-lookup"><span data-stu-id="55ffb-167">width</span></span> |<span data-ttu-id="55ffb-168">Šířka videa v pixelech</span><span class="sxs-lookup"><span data-stu-id="55ffb-168">width of the video in pixels</span></span> |
| <span data-ttu-id="55ffb-169">Výška</span><span class="sxs-lookup"><span data-stu-id="55ffb-169">height</span></span> |<span data-ttu-id="55ffb-170">Výška videa v pixelech</span><span class="sxs-lookup"><span data-stu-id="55ffb-170">height of the video in pixels</span></span> |
| <span data-ttu-id="55ffb-171">fragmenty</span><span class="sxs-lookup"><span data-stu-id="55ffb-171">Fragments</span></span> |<span data-ttu-id="55ffb-172">pole založené na čase bloků dat videa, do kterého je blokové metadata</span><span class="sxs-lookup"><span data-stu-id="55ffb-172">array of time-based chunks of video into which the metadata is chunked</span></span> |
| <span data-ttu-id="55ffb-173">start</span><span class="sxs-lookup"><span data-stu-id="55ffb-173">start</span></span> |<span data-ttu-id="55ffb-174">Počáteční čas fragment v "rysky"</span><span class="sxs-lookup"><span data-stu-id="55ffb-174">start time of a fragment in "ticks"</span></span> |
| <span data-ttu-id="55ffb-175">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="55ffb-175">duration</span></span> |<span data-ttu-id="55ffb-176">Délka fragment v "rysky"</span><span class="sxs-lookup"><span data-stu-id="55ffb-176">length of a fragment in "ticks"</span></span> |
| <span data-ttu-id="55ffb-177">Interval</span><span class="sxs-lookup"><span data-stu-id="55ffb-177">interval</span></span> |<span data-ttu-id="55ffb-178">Interval jednotlivých událostí v rámci dané fragment</span><span class="sxs-lookup"><span data-stu-id="55ffb-178">interval of each event within the given fragment</span></span> |
| <span data-ttu-id="55ffb-179">stránka events</span><span class="sxs-lookup"><span data-stu-id="55ffb-179">events</span></span> |<span data-ttu-id="55ffb-180">pole obsahující oblastí</span><span class="sxs-lookup"><span data-stu-id="55ffb-180">array containing regions</span></span> |
| <span data-ttu-id="55ffb-181">Oblast</span><span class="sxs-lookup"><span data-stu-id="55ffb-181">region</span></span> |<span data-ttu-id="55ffb-182">objekt představující zjistil slova nebo fráze</span><span class="sxs-lookup"><span data-stu-id="55ffb-182">object representing detected words or phrases</span></span> |
| <span data-ttu-id="55ffb-183">Jazyk</span><span class="sxs-lookup"><span data-stu-id="55ffb-183">language</span></span> |<span data-ttu-id="55ffb-184">jazyk textu zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="55ffb-184">language of the text detected within a region</span></span> |
| <span data-ttu-id="55ffb-185">orientace</span><span class="sxs-lookup"><span data-stu-id="55ffb-185">orientation</span></span> |<span data-ttu-id="55ffb-186">orientaci textu zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="55ffb-186">orientation of the text detected within a region</span></span> |
| <span data-ttu-id="55ffb-187">řádky</span><span class="sxs-lookup"><span data-stu-id="55ffb-187">lines</span></span> |<span data-ttu-id="55ffb-188">pole řádků textu zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="55ffb-188">array of lines of text detected within a region</span></span> |
| <span data-ttu-id="55ffb-189">Text</span><span class="sxs-lookup"><span data-stu-id="55ffb-189">text</span></span> |<span data-ttu-id="55ffb-190">vlastní text</span><span class="sxs-lookup"><span data-stu-id="55ffb-190">the actual text</span></span> |

### <a name="json-output-example"></a><span data-ttu-id="55ffb-191">Příklad výstupu JSON</span><span class="sxs-lookup"><span data-stu-id="55ffb-191">JSON output example</span></span>
<span data-ttu-id="55ffb-192">Následující příklad výstupu obsahuje obecné informace videa a několik video fragmenty.</span><span class="sxs-lookup"><span data-stu-id="55ffb-192">The following output example contains the general video information and several video fragments.</span></span> <span data-ttu-id="55ffb-193">V každé video fragmentu obsahuje každou oblast, který je zjišťován pomocí MP rozpoznávání znaků s jazyk a jeho orientaci textu.</span><span class="sxs-lookup"><span data-stu-id="55ffb-193">In every video fragment, it contains every region which is detected by OCR MP with the language and its text orientation.</span></span> <span data-ttu-id="55ffb-194">Oblast také obsahuje každý řádek aplikace word v této oblasti na řádku textu, pozice na řádku a každý word informace (word obsahu, pozice a spolehlivosti) v tomto řádku.</span><span class="sxs-lookup"><span data-stu-id="55ffb-194">The region also contains every word line in this region with the line’s text, the line’s position, and every word information (word content, position and confidence) in this line.</span></span> <span data-ttu-id="55ffb-195">Následuje příklad a umístíte některé vložené komentáře.</span><span class="sxs-lookup"><span data-stu-id="55ffb-195">The following is an example, and I put some comments inline.</span></span>

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="net-sample-code"></a><span data-ttu-id="55ffb-196">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="55ffb-196">.NET sample code</span></span>

<span data-ttu-id="55ffb-197">Program zobrazí následující postup:</span><span class="sxs-lookup"><span data-stu-id="55ffb-197">The following program shows how to:</span></span>

1. <span data-ttu-id="55ffb-198">Vytvořte asset a nahrajte soubor média do assetu.</span><span class="sxs-lookup"><span data-stu-id="55ffb-198">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="55ffb-199">Vytvoření úlohy se souborem konfigurace nebo přednastavených rozpoznávání znaků.</span><span class="sxs-lookup"><span data-stu-id="55ffb-199">Create a job with an OCR configuration/preset file.</span></span>
3. <span data-ttu-id="55ffb-200">Stáhněte soubory JSON výstupu.</span><span class="sxs-lookup"><span data-stu-id="55ffb-200">Download the output JSON files.</span></span> 
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="55ffb-201">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="55ffb-201">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="55ffb-202">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="55ffb-202">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="55ffb-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="55ffb-203">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace OCR
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

                // Run the OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference to Azure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="55ffb-204">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="55ffb-204">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="55ffb-205">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="55ffb-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="55ffb-206">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="55ffb-206">Related links</span></span>
[<span data-ttu-id="55ffb-207">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="55ffb-207">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

