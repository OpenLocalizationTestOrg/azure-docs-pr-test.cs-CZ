---
title: "aaaDigitize textu pomocí Azure Media Analytics rozpoznávání znaků | Microsoft Docs"
description: "Rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků) umožňuje tooconvert textového obsahu v video soubory do upravovat, vyhledávat digitální textu.  To vám umožní tooautomate hello extrakce smysluplný metadata z hello signál video média."
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
ms.openlocfilehash: 0476c3ba3942b2c5182a34a429909adbf5c75ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a><span data-ttu-id="1610b-104">Pomocí Azure Media Analytics tooconvert textového obsahu v video soubory do digitální textu</span><span class="sxs-lookup"><span data-stu-id="1610b-104">Use Azure Media Analytics tooconvert text content in video files into digital text</span></span>
## <a name="overview"></a><span data-ttu-id="1610b-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="1610b-105">Overview</span></span>
<span data-ttu-id="1610b-106">Pokud potřebujete tooextract textového obsahu z video soubory a generovat upravovat, vyhledávat digitální text, měli byste použít rozpoznávání Azure Media Analytics znaků (optické rozpoznávání znaků).</span><span class="sxs-lookup"><span data-stu-id="1610b-106">If you need tooextract text content from your video files and generate an editable, searchable digital text, you should use Azure Media Analytics OCR (optical character recognition).</span></span> <span data-ttu-id="1610b-107">Tento procesor médií Azure zjistí textového obsahu v video soubory a vygeneruje textových souborů pro vaše použití.</span><span class="sxs-lookup"><span data-stu-id="1610b-107">This Azure Media Processor detects text content in your video files and generates text files for your use.</span></span> <span data-ttu-id="1610b-108">Rozpoznávání znaků umožňuje vám tooautomate hello extrakce smysluplný metadata z hello signál video média.</span><span class="sxs-lookup"><span data-stu-id="1610b-108">OCR enables you tooautomate hello extraction of meaningful metadata from hello video signal of your media.</span></span>

<span data-ttu-id="1610b-109">Při použití ve spojení s vyhledávacího webu, můžete snadno indexu médiu podle textu a vylepšit možnosti rozpoznání hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="1610b-109">When used in conjunction with a search engine, you can easily index your media by text, and enhance hello discoverability of your content.</span></span> <span data-ttu-id="1610b-110">To je velmi užitečné v vysoce textovou video, jako je záznam videa nebo snímek obrazovky prezentace prezentace.</span><span class="sxs-lookup"><span data-stu-id="1610b-110">This is extremely useful in highly textual video, like a video recording or screen-capture of a slideshow presentation.</span></span> <span data-ttu-id="1610b-111">Hello procesor médií rozpoznávání znaků Azure je optimalizovaná pro digitální text.</span><span class="sxs-lookup"><span data-stu-id="1610b-111">hello Azure OCR Media Processor is optimized for digital text.</span></span>

<span data-ttu-id="1610b-112">Hello **rozpoznávání Azure Media znaků** procesor médií je aktuálně ve verzi Preview.</span><span class="sxs-lookup"><span data-stu-id="1610b-112">hello **Azure Media OCR** media processor is currently in Preview.</span></span>

<span data-ttu-id="1610b-113">Toto téma uvádí podrobnosti o **rozpoznávání Azure Media znaků** a ukazuje, jak toouse ho pomocí sady Media Services SDK pro .NET.</span><span class="sxs-lookup"><span data-stu-id="1610b-113">This topic gives details about  **Azure Media OCR** and shows how toouse it with Media Services SDK for .NET.</span></span> <span data-ttu-id="1610b-114">Další informace a příklady naleznete v tématu [tomto blogu](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span><span class="sxs-lookup"><span data-stu-id="1610b-114">For additional information and examples, see [this blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span></span>

## <a name="ocr-input-files"></a><span data-ttu-id="1610b-115">Vstupní soubory rozpoznávání znaků</span><span class="sxs-lookup"><span data-stu-id="1610b-115">OCR input files</span></span>
<span data-ttu-id="1610b-116">Video soubory.</span><span class="sxs-lookup"><span data-stu-id="1610b-116">Video files.</span></span> <span data-ttu-id="1610b-117">V současné době jsou podporovány následující formáty hello: MP4, MOV a WMV.</span><span class="sxs-lookup"><span data-stu-id="1610b-117">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration"></a><span data-ttu-id="1610b-118">Konfigurace úlohy</span><span class="sxs-lookup"><span data-stu-id="1610b-118">Task configuration</span></span>
<span data-ttu-id="1610b-119">Konfigurace úlohy (přednastavených).</span><span class="sxs-lookup"><span data-stu-id="1610b-119">Task configuration (preset).</span></span> <span data-ttu-id="1610b-120">Při vytváření úlohy s **rozpoznávání Azure Media znaků**, je nutné zadat konfiguraci přednastavení pomocí XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="1610b-120">When creating a task with **Azure Media OCR**, you must specify a configuration preset using JSON  or XML.</span></span> 

>[!NOTE]
><span data-ttu-id="1610b-121">modul rozpoznávání znaků Hello pouze vezme oblast bitové kopie s minimální pixelů toomaximum 32000 40 pixelů jako platnou hodnotu v obou výšky a šířky.</span><span class="sxs-lookup"><span data-stu-id="1610b-121">hello OCR engine only takes an image region with minimum 40 pixels toomaximum 32000 pixels as a valid input in both height/width.</span></span>
>

### <a name="attribute-descriptions"></a><span data-ttu-id="1610b-122">Atribut popisy</span><span class="sxs-lookup"><span data-stu-id="1610b-122">Attribute descriptions</span></span>
| <span data-ttu-id="1610b-123">Název atributu</span><span class="sxs-lookup"><span data-stu-id="1610b-123">Attribute name</span></span> | <span data-ttu-id="1610b-124">Popis</span><span class="sxs-lookup"><span data-stu-id="1610b-124">Description</span></span> |
| --- | --- |
|<span data-ttu-id="1610b-125">AdvancedOutput</span><span class="sxs-lookup"><span data-stu-id="1610b-125">AdvancedOutput</span></span>| <span data-ttu-id="1610b-126">Pokud jste nastavili AdvancedOutput tootrue, budou obsahovat výstup JSON hello poziční data pro každou jednoho slova (v přidání toophrases a oblasti).</span><span class="sxs-lookup"><span data-stu-id="1610b-126">If you set AdvancedOutput tootrue, hello JSON output will contain positional data for every single word (in addition toophrases and regions).</span></span> <span data-ttu-id="1610b-127">Pokud nechcete, aby toosee tyto podrobnosti, nastavte příznak toofalse hello.</span><span class="sxs-lookup"><span data-stu-id="1610b-127">If you do not want toosee these details, set hello flag toofalse.</span></span> <span data-ttu-id="1610b-128">Hello výchozí hodnota je false.</span><span class="sxs-lookup"><span data-stu-id="1610b-128">hello default value is false.</span></span> <span data-ttu-id="1610b-129">Další informace najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span><span class="sxs-lookup"><span data-stu-id="1610b-129">For more information, see [this blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span></span>|
| <span data-ttu-id="1610b-130">Jazyk</span><span class="sxs-lookup"><span data-stu-id="1610b-130">Language</span></span> |<span data-ttu-id="1610b-131">(volitelné) popisuje hello jazyk textu, pro které toolook.</span><span class="sxs-lookup"><span data-stu-id="1610b-131">(optional) describes hello language of text for which toolook.</span></span> <span data-ttu-id="1610b-132">Jedna z následujících hello: AutoDetect (výchozí), Arabské, ChineseSimplified, ChineseTraditional, čeština dánština, holandština, angličtina, finština, francouzština, němčina, řečtina, maďarština, italština, japonština, korejština, norština, polština, portugalština, rumunština, ruština, SerbianCyrillic, SerbianLatin, slovenština, španělština, švédština, turečtina.</span><span class="sxs-lookup"><span data-stu-id="1610b-132">One of hello following: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech Danish, Dutch, English, Finnish, French, German,  Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.</span></span> |
| <span data-ttu-id="1610b-133">TextOrientation</span><span class="sxs-lookup"><span data-stu-id="1610b-133">TextOrientation</span></span> |<span data-ttu-id="1610b-134">(volitelné) popisuje hello orientaci textu, pro které toolook.</span><span class="sxs-lookup"><span data-stu-id="1610b-134">(optional) describes hello orientation of text for which toolook.</span></span>  <span data-ttu-id="1610b-135">"Left" prostředky, které hello horní části všechna písmena jsou odkazoval směrem doleva hello.</span><span class="sxs-lookup"><span data-stu-id="1610b-135">"Left" means that hello top of all letters are pointed towards hello left.</span></span>  <span data-ttu-id="1610b-136">Výchozí text (např., které lze nalézt v podobě knihy) je možné volat "Nahoru" orientované.</span><span class="sxs-lookup"><span data-stu-id="1610b-136">Default text (like that which can be found in a book) can be called "Up" oriented.</span></span>  <span data-ttu-id="1610b-137">Jedna z následujících hello: AutoDetect (výchozí), až, vpravo, dolů, doleva.</span><span class="sxs-lookup"><span data-stu-id="1610b-137">One of hello following: AutoDetect (default), Up, Right, Down, Left.</span></span> |
| <span data-ttu-id="1610b-138">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="1610b-138">TimeInterval</span></span> |<span data-ttu-id="1610b-139">(volitelné) popisuje hello vzorkovací frekvenci.</span><span class="sxs-lookup"><span data-stu-id="1610b-139">(optional) describes hello sampling rate.</span></span>  <span data-ttu-id="1610b-140">Výchozí hodnota je každou sekundu 1/2.</span><span class="sxs-lookup"><span data-stu-id="1610b-140">Default is every 1/2 second.</span></span><br/><span data-ttu-id="1610b-141">Formát JSON – hh: mm:. Služby Zabezpečené úložiště (výchozí 00:00:00.500)</span><span class="sxs-lookup"><span data-stu-id="1610b-141">JSON format – HH:mm:ss.SSS (default 00:00:00.500)</span></span><br/><span data-ttu-id="1610b-142">Formát XML – doba trvání primitivní W3C XSD (výchozí PT0.5)</span><span class="sxs-lookup"><span data-stu-id="1610b-142">XML format – W3C XSD duration primitive (default PT0.5)</span></span> |
| <span data-ttu-id="1610b-143">DetectRegions</span><span class="sxs-lookup"><span data-stu-id="1610b-143">DetectRegions</span></span> |<span data-ttu-id="1610b-144">(volitelné) Pole objektů DetectRegion určení oblasti v rámci video hello v textu, který toodetect.</span><span class="sxs-lookup"><span data-stu-id="1610b-144">(optional) An array of DetectRegion objects specifying regions within hello video frame in which toodetect text.</span></span><br/><span data-ttu-id="1610b-145">Objekt DetectRegion je tvořen hello následující čtyři celočíselné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="1610b-145">A DetectRegion object is made of hello following four integer values:</span></span><br/><span data-ttu-id="1610b-146">Vlevo – pixelů z levého okraje hello</span><span class="sxs-lookup"><span data-stu-id="1610b-146">Left – pixels from hello left-margin</span></span><br/><span data-ttu-id="1610b-147">TOP – pixelů z hello horní margin</span><span class="sxs-lookup"><span data-stu-id="1610b-147">Top – pixels from hello top-margin</span></span><br/><span data-ttu-id="1610b-148">Šířka – Šířka hello oblast v pixelech</span><span class="sxs-lookup"><span data-stu-id="1610b-148">Width – width of hello region in pixels</span></span><br/><span data-ttu-id="1610b-149">Výška – výšku oblasti hello v pixelech</span><span class="sxs-lookup"><span data-stu-id="1610b-149">Height – height of hello region in pixels</span></span> |

#### <a name="json-preset-example"></a><span data-ttu-id="1610b-150">Příklad přednastavené JSON</span><span class="sxs-lookup"><span data-stu-id="1610b-150">JSON preset example</span></span>

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


#### <a name="xml-preset-example"></a><span data-ttu-id="1610b-151">Příklad přednastavené XML</span><span class="sxs-lookup"><span data-stu-id="1610b-151">XML preset example</span></span>
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

## <a name="ocr-output-files"></a><span data-ttu-id="1610b-152">Rozpoznávání znaků výstupní soubory</span><span class="sxs-lookup"><span data-stu-id="1610b-152">OCR output files</span></span>
<span data-ttu-id="1610b-153">výstup Hello hello rozpoznávání znaků média procesoru je soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="1610b-153">hello output of hello OCR media processor is a JSON file.</span></span>

### <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="1610b-154">Elementy výstupního souboru JSON, hello</span><span class="sxs-lookup"><span data-stu-id="1610b-154">Elements of hello output JSON file</span></span>
<span data-ttu-id="1610b-155">výstup Hello Video rozpoznávání znaků poskytuje segmentované čas data hello znaků, které jsou součástí videa.</span><span class="sxs-lookup"><span data-stu-id="1610b-155">hello Video OCR output provides time-segmented data on hello characters found in your video.</span></span>  <span data-ttu-id="1610b-156">Atributy, jako je například jazyk nebo orientaci toohone-in můžete použít na přesně hello slova, že máte zájem analýza.</span><span class="sxs-lookup"><span data-stu-id="1610b-156">You can use attributes such as language or orientation toohone-in on exactly hello words that you are interested in analyzing.</span></span> 

<span data-ttu-id="1610b-157">výstup Hello obsahuje hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="1610b-157">hello output contains hello following attributes:</span></span>

| <span data-ttu-id="1610b-158">Element</span><span class="sxs-lookup"><span data-stu-id="1610b-158">Element</span></span> | <span data-ttu-id="1610b-159">Popis</span><span class="sxs-lookup"><span data-stu-id="1610b-159">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1610b-160">Časová osa</span><span class="sxs-lookup"><span data-stu-id="1610b-160">Timescale</span></span> |<span data-ttu-id="1610b-161">"rysky" za sekundu hello videa</span><span class="sxs-lookup"><span data-stu-id="1610b-161">"ticks" per second of hello video</span></span> |
| <span data-ttu-id="1610b-162">Posun</span><span class="sxs-lookup"><span data-stu-id="1610b-162">Offset</span></span> |<span data-ttu-id="1610b-163">časového posunu pro časová razítka.</span><span class="sxs-lookup"><span data-stu-id="1610b-163">time offset for timestamps.</span></span> <span data-ttu-id="1610b-164">Ve verzi 1.0 rozhraní API, Video bude vždy 0.</span><span class="sxs-lookup"><span data-stu-id="1610b-164">In version 1.0 of Video APIs, this will always be 0.</span></span> |
| <span data-ttu-id="1610b-165">kmitočet snímků</span><span class="sxs-lookup"><span data-stu-id="1610b-165">Framerate</span></span> |<span data-ttu-id="1610b-166">Počet snímků za sekundu hello video</span><span class="sxs-lookup"><span data-stu-id="1610b-166">Frames per second of hello video</span></span> |
| <span data-ttu-id="1610b-167">Šířka</span><span class="sxs-lookup"><span data-stu-id="1610b-167">width</span></span> |<span data-ttu-id="1610b-168">Šířka hello videa v pixelech</span><span class="sxs-lookup"><span data-stu-id="1610b-168">width of hello video in pixels</span></span> |
| <span data-ttu-id="1610b-169">Výška</span><span class="sxs-lookup"><span data-stu-id="1610b-169">height</span></span> |<span data-ttu-id="1610b-170">Výška hello videa v pixelech</span><span class="sxs-lookup"><span data-stu-id="1610b-170">height of hello video in pixels</span></span> |
| <span data-ttu-id="1610b-171">fragmenty</span><span class="sxs-lookup"><span data-stu-id="1610b-171">Fragments</span></span> |<span data-ttu-id="1610b-172">pole založené na čase bloků dat videa, do které hello metadata blokové</span><span class="sxs-lookup"><span data-stu-id="1610b-172">array of time-based chunks of video into which hello metadata is chunked</span></span> |
| <span data-ttu-id="1610b-173">start</span><span class="sxs-lookup"><span data-stu-id="1610b-173">start</span></span> |<span data-ttu-id="1610b-174">Počáteční čas fragment v "rysky"</span><span class="sxs-lookup"><span data-stu-id="1610b-174">start time of a fragment in "ticks"</span></span> |
| <span data-ttu-id="1610b-175">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="1610b-175">duration</span></span> |<span data-ttu-id="1610b-176">Délka fragment v "rysky"</span><span class="sxs-lookup"><span data-stu-id="1610b-176">length of a fragment in "ticks"</span></span> |
| <span data-ttu-id="1610b-177">interval</span><span class="sxs-lookup"><span data-stu-id="1610b-177">interval</span></span> |<span data-ttu-id="1610b-178">Interval jednotlivých událostí v rámci hello zadaný fragment</span><span class="sxs-lookup"><span data-stu-id="1610b-178">interval of each event within hello given fragment</span></span> |
| <span data-ttu-id="1610b-179">stránka events</span><span class="sxs-lookup"><span data-stu-id="1610b-179">events</span></span> |<span data-ttu-id="1610b-180">pole obsahující oblastí</span><span class="sxs-lookup"><span data-stu-id="1610b-180">array containing regions</span></span> |
| <span data-ttu-id="1610b-181">Oblast</span><span class="sxs-lookup"><span data-stu-id="1610b-181">region</span></span> |<span data-ttu-id="1610b-182">objekt představující zjistil slova nebo fráze</span><span class="sxs-lookup"><span data-stu-id="1610b-182">object representing detected words or phrases</span></span> |
| <span data-ttu-id="1610b-183">Jazyk</span><span class="sxs-lookup"><span data-stu-id="1610b-183">language</span></span> |<span data-ttu-id="1610b-184">jazyk textu hello zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="1610b-184">language of hello text detected within a region</span></span> |
| <span data-ttu-id="1610b-185">orientace</span><span class="sxs-lookup"><span data-stu-id="1610b-185">orientation</span></span> |<span data-ttu-id="1610b-186">orientaci textu hello zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="1610b-186">orientation of hello text detected within a region</span></span> |
| <span data-ttu-id="1610b-187">řádky</span><span class="sxs-lookup"><span data-stu-id="1610b-187">lines</span></span> |<span data-ttu-id="1610b-188">pole řádků textu zjistil v rámci oblasti</span><span class="sxs-lookup"><span data-stu-id="1610b-188">array of lines of text detected within a region</span></span> |
| <span data-ttu-id="1610b-189">Text</span><span class="sxs-lookup"><span data-stu-id="1610b-189">text</span></span> |<span data-ttu-id="1610b-190">vlastní text Hello</span><span class="sxs-lookup"><span data-stu-id="1610b-190">hello actual text</span></span> |

### <a name="json-output-example"></a><span data-ttu-id="1610b-191">Příklad výstupu JSON</span><span class="sxs-lookup"><span data-stu-id="1610b-191">JSON output example</span></span>
<span data-ttu-id="1610b-192">Hello následující příklad výstupu obsahuje obecné informace video hello a několik video fragmenty.</span><span class="sxs-lookup"><span data-stu-id="1610b-192">hello following output example contains hello general video information and several video fragments.</span></span> <span data-ttu-id="1610b-193">V každé video fragmentu obsahuje každou oblast, který je zjišťován pomocí MP rozpoznávání znaků s jazykem hello a jeho orientaci textu.</span><span class="sxs-lookup"><span data-stu-id="1610b-193">In every video fragment, it contains every region which is detected by OCR MP with hello language and its text orientation.</span></span> <span data-ttu-id="1610b-194">Hello oblast také obsahuje každý řádek aplikace word v této oblasti s textem hello řádku, pozice řádku hello a každý word informace (word obsahu, pozice a spolehlivosti) v tomto řádku.</span><span class="sxs-lookup"><span data-stu-id="1610b-194">hello region also contains every word line in this region with hello line’s text, hello line’s position, and every word information (word content, position and confidence) in this line.</span></span> <span data-ttu-id="1610b-195">Následuje příklad Hello a umístíte některé vložené komentáře.</span><span class="sxs-lookup"><span data-stu-id="1610b-195">hello following is an example, and I put some comments inline.</span></span>

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
                "interval": 90000,  // hello time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // hello detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including hello text and hello position
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

## <a name="net-sample-code"></a><span data-ttu-id="1610b-196">Ukázkový kód rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1610b-196">.NET sample code</span></span>

<span data-ttu-id="1610b-197">ukazuje programu Hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="1610b-197">hello following program shows how to:</span></span>

1. <span data-ttu-id="1610b-198">Vytvořte asset a nahrajte soubor média do hello asset.</span><span class="sxs-lookup"><span data-stu-id="1610b-198">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="1610b-199">Vytvoření úlohy se souborem konfigurace nebo přednastavených rozpoznávání znaků.</span><span class="sxs-lookup"><span data-stu-id="1610b-199">Create a job with an OCR configuration/preset file.</span></span>
3. <span data-ttu-id="1610b-200">Stáhněte soubory JSON výstup hello.</span><span class="sxs-lookup"><span data-stu-id="1610b-200">Download hello output JSON files.</span></span> 
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="1610b-201">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="1610b-201">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="1610b-202">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="1610b-202">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="1610b-203">Příklad</span><span class="sxs-lookup"><span data-stu-id="1610b-203">Example</span></span>

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

                // Run hello OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference tooAzure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="1610b-204">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="1610b-204">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1610b-205">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1610b-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="1610b-206">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="1610b-206">Related links</span></span>
[<span data-ttu-id="1610b-207">Azure Media Services Analytics – přehled</span><span class="sxs-lookup"><span data-stu-id="1610b-207">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

