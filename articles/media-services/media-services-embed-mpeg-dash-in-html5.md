---
title: "aaaEmbedding MPEG-DASH adaptivní streamování videa v aplikaci HTML5 s DASH.js | Microsoft Docs"
description: "Toto téma ukazuje, jak tooembed MPEG-DASH adaptivní streamování videa v aplikaci s DASH.js HTML5."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 5aa0e7b6-f5c3-4cc1-aa33-ed16ea4780c2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a73713d20f95262654532b94576ae9669d829354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a><span data-ttu-id="71b8c-103">Vložení do aplikace HTML5 s DASH.js Video adaptivní streamování MPEG-DASH</span><span class="sxs-lookup"><span data-stu-id="71b8c-103">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>
## <a name="overview"></a><span data-ttu-id="71b8c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="71b8c-104">Overview</span></span>
<span data-ttu-id="71b8c-105">MPEG-DASH je standard ISO pro adaptivní streamování videa obsahu, který nabízí významné výhody pro osoby, které chcete toodeliver vysoce kvalitní, adaptivní video streamování výstup hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-105">MPEG-DASH is an ISO standard for hello adaptive streaming of video content, which offers significant benefits for those who wish toodeliver high-quality, adaptive video streaming output.</span></span> <span data-ttu-id="71b8c-106">S MPEG-DASH bude datový proud videa hello automaticky vyřadit nižší definice tooa, stane se zahlcení sítě hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-106">With MPEG-DASH, hello video stream will automatically drop tooa lower definition when hello network becomes congested.</span></span> <span data-ttu-id="71b8c-107">Tím se snižuje pravděpodobnost hello hello prohlížeče zobrazuje "pozastavený" video při hello player stáhne hello vedle několik sekund tooplay (neboli ukládání do vyrovnávací paměti).</span><span class="sxs-lookup"><span data-stu-id="71b8c-107">This reduces hello likelihood of hello viewer seeing a "paused" video while hello player downloads hello next few seconds tooplay (aka buffering).</span></span> <span data-ttu-id="71b8c-108">Jako zahlcení sítě snižuje, přehrávání videa hello pak vrátí tooa vyšší kvality datového proudu.</span><span class="sxs-lookup"><span data-stu-id="71b8c-108">As network congestion reduces, hello video player will in turn return tooa higher quality stream.</span></span> <span data-ttu-id="71b8c-109">Tato možnost tooadapt hello šířky pásma požadované výsledkem také rychlejší čas spuštění videa.</span><span class="sxs-lookup"><span data-stu-id="71b8c-109">This ability tooadapt hello bandwidth required also results in a faster start time for video.</span></span> <span data-ttu-id="71b8c-110">Znamená, že hello první několik sekund lze přehrávat v segment fast stažení nižší kvality a pak postupujte nahoru vyšší kvality tooa po dostatečná obsah má byla uložená do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="71b8c-110">That means that hello first few seconds can be played in a fast-to-download lower quality segment and then step up tooa higher quality once sufficient content has been buffered.</span></span>

<span data-ttu-id="71b8c-111">Dash.js je otevřeným zdrojem MPEG-DASH přehrávání videa napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71b8c-111">Dash.js is an open source MPEG-DASH video player written in JavaScript.</span></span> <span data-ttu-id="71b8c-112">Jeho cílem je tooprovide přehrávač robustní, a platformy, které můžete volně opakovaně použít v aplikacích, které vyžadují přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="71b8c-112">Its goal is tooprovide a robust, cross-platform player that can be freely reused in applications that require video playback.</span></span> <span data-ttu-id="71b8c-113">Poskytuje MPEG-DASH přehrávání v žádný prohlížeč, který podporuje hello W3C média zdrojového rozšíření (MSE), který ještě dnes je Chrome, Microsoft Edge a IE11 (dalších prohlížečích označili jejich záměrné toosupport MSE).</span><span class="sxs-lookup"><span data-stu-id="71b8c-113">It provides MPEG-DASH playback in any browser that supports hello W3C Media Source Extensions (MSE), today that is Chrome, Microsoft Edge and IE11 (other browsers have indicated their intent toosupport MSE).</span></span> <span data-ttu-id="71b8c-114">Další informace o DASH.js js, najdete v části úložiště dash.js hello GitHub.</span><span class="sxs-lookup"><span data-stu-id="71b8c-114">For more information about DASH.js, js see hello GitHub dash.js repository.</span></span>

## <a name="creating-a-browser-based-streaming-video-player"></a><span data-ttu-id="71b8c-115">Vytváření založené na prohlížeči streamování videa přehrávač</span><span class="sxs-lookup"><span data-stu-id="71b8c-115">Creating a browser-based streaming video player</span></span>
<span data-ttu-id="71b8c-116">toocreate jednoduché webové stránky, která zobrazuje přehrávání videa s hello očekává řídí takové a play, pozastavení, rewind atd., budete muset:</span><span class="sxs-lookup"><span data-stu-id="71b8c-116">toocreate a simple web page that displays a video player with hello expected controls such a play, pause, rewind etc., you will need to:</span></span>

1. <span data-ttu-id="71b8c-117">Vytvoření stránky HTML</span><span class="sxs-lookup"><span data-stu-id="71b8c-117">Create an HTML page</span></span>
2. <span data-ttu-id="71b8c-118">Přidání značka video hello</span><span class="sxs-lookup"><span data-stu-id="71b8c-118">Add hello video tag</span></span>
3. <span data-ttu-id="71b8c-119">Přidat hello dash.js player</span><span class="sxs-lookup"><span data-stu-id="71b8c-119">Add hello dash.js player</span></span>
4. <span data-ttu-id="71b8c-120">Inicializace hello player</span><span class="sxs-lookup"><span data-stu-id="71b8c-120">Initialize hello player</span></span>
5. <span data-ttu-id="71b8c-121">Přidat některé stylu CSS</span><span class="sxs-lookup"><span data-stu-id="71b8c-121">Add some CSS style</span></span>
6. <span data-ttu-id="71b8c-122">Zobrazení výsledků hello v prohlížeči, který implementuje MSE</span><span class="sxs-lookup"><span data-stu-id="71b8c-122">View hello results in a browser that implements MSE</span></span>

<span data-ttu-id="71b8c-123">Inicializace hello player může dokončit jenom pár řádků kódu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71b8c-123">Initializing hello player can be completed in just a handful of lines of JavaScript code.</span></span> <span data-ttu-id="71b8c-124">Pomocí dash.js, ve skutečnosti je jednoduchý tooembed MPEG-DASH videa v aplikací využívajících prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="71b8c-124">Using dash.js, it really is that simple tooembed MPEG-DASH video in your browser based applications.</span></span>

## <a name="creating-hello-html-page"></a><span data-ttu-id="71b8c-125">Vytváření hello stránky HTML</span><span class="sxs-lookup"><span data-stu-id="71b8c-125">Creating hello HTML Page</span></span>
<span data-ttu-id="71b8c-126">prvním krokem Hello je stránka toocreate standardní HTML obsahující hello **video** elementu, uložte tento soubor jako basicPlayer.html jako hello následující příklad znázorňuje:</span><span class="sxs-lookup"><span data-stu-id="71b8c-126">hello first step is toocreate a standard HTML page containing hello **video** element, save this file as basicPlayer.html, as hello following example illustrates:</span></span>

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a><span data-ttu-id="71b8c-127">Přidání hello DASH.js Player</span><span class="sxs-lookup"><span data-stu-id="71b8c-127">Adding hello DASH.js Player</span></span>
<span data-ttu-id="71b8c-128">tooadd hello dash.js referenční implementace toohello aplikace, budete potřebovat soubor dash.all.js hello toograb z verze hello 1.0 dash.js projektu.</span><span class="sxs-lookup"><span data-stu-id="71b8c-128">tooadd hello dash.js reference implementation toohello application, you’ll need toograb hello dash.all.js file from hello 1.0 release of dash.js project.</span></span> <span data-ttu-id="71b8c-129">To má být uložen ve složce JavaScript hello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="71b8c-129">This should be saved in hello JavaScript folder of your application.</span></span> <span data-ttu-id="71b8c-130">Tento soubor je soubor pohodlí, který vrátí všechny nezbytné dash.js kód hello do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="71b8c-130">This file is a convenience file that pulls together all hello necessary dash.js code into a single file.</span></span> <span data-ttu-id="71b8c-131">Pokud máte podívejte kolem hello dash.js úložiště, se najde hello jednotlivé soubory, testování kódu a mnoho dalšího, ale pokud všechny budete chtít toodo je použití dash.js a pak soubor dash.all.js hello je, co potřebujete.</span><span class="sxs-lookup"><span data-stu-id="71b8c-131">If you have a look around hello dash.js repository, you will find hello individual files, test code and much more, but if all you want toodo is use dash.js, then hello dash.all.js file is what you need.</span></span>

<span data-ttu-id="71b8c-132">tooadd hello dash.js player tooyour aplikací, přidejte skript značky toohello head oddíl basicPlayer.html:</span><span class="sxs-lookup"><span data-stu-id="71b8c-132">tooadd hello dash.js player tooyour applications, add a script tag toohello head section of basicPlayer.html:</span></span>

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


<span data-ttu-id="71b8c-133">Dále vytvořte přehrávač funkce tooinitialize hello při načtení stránky hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-133">Next, create a function tooinitialize hello player when hello page loads.</span></span> <span data-ttu-id="71b8c-134">Přidejte následující skript po hello řádku, ve kterém můžete načíst dash.all.js hello:</span><span class="sxs-lookup"><span data-stu-id="71b8c-134">Add hello following script after hello line in which you load dash.all.js:</span></span>

    <script>
    // setup hello video element and attach it toohello Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

<span data-ttu-id="71b8c-135">Tato funkce nejprve vytvoří DashContext.</span><span class="sxs-lookup"><span data-stu-id="71b8c-135">This function first creates a DashContext.</span></span> <span data-ttu-id="71b8c-136">Toto je aplikace hello tooconfigure použitých pro konkrétní běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="71b8c-136">This is used tooconfigure hello application for a specific runtime environment.</span></span> <span data-ttu-id="71b8c-137">Z technického hlediska definuje hello třídy, které hello framework vkládání závislostí musí použít při vytváření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-137">From a technical point of view, it defines hello classes that hello dependency injection framework should use when constructing hello application.</span></span> <span data-ttu-id="71b8c-138">Ve většině případů budete používat Dash.di.DashContext.</span><span class="sxs-lookup"><span data-stu-id="71b8c-138">In most cases, you will use Dash.di.DashContext.</span></span>

<span data-ttu-id="71b8c-139">V dalším kroku doložit třídu primární hello hello dash.js prostředí, Media Player.</span><span class="sxs-lookup"><span data-stu-id="71b8c-139">Next, instantiate hello primary class of hello dash.js framework, MediaPlayer.</span></span> <span data-ttu-id="71b8c-140">Tato třída obsahuje hello základní metody, jako například potřeby přehrání a pozastavit, spravuje hello vztah s hello video element a také hello výklad hello soubor média prezentace popis (MPD), který popisuje přehrát video toobe hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-140">This class contains hello core methods needed such as play and pause, manages hello relationship with hello video element and also manages hello interpretation of hello Media Presentation Description (MPD) file which describes hello video toobe played.</span></span>

<span data-ttu-id="71b8c-141">Funkce startup() Hello hello Media Player třídy se nazývá tooensure které player hello je připraven tooplay videa.</span><span class="sxs-lookup"><span data-stu-id="71b8c-141">hello startup() function of hello MediaPlayer class is called tooensure that hello player is ready tooplay video.</span></span> <span data-ttu-id="71b8c-142">Mimo jiné tato funkce zajišťuje, že všechny potřebné třídy hello (podle definice v kontextu hello) byly načteny.</span><span class="sxs-lookup"><span data-stu-id="71b8c-142">Amongst other things this function ensures that all hello necessary classes (as defined by hello context) have been loaded.</span></span> <span data-ttu-id="71b8c-143">Jakmile hello player je připraven, můžete připojit tooit video element hello pomocí funkce attachView() hello.</span><span class="sxs-lookup"><span data-stu-id="71b8c-143">Once hello player is ready, you can attach hello video element tooit using hello attachView() function.</span></span> <span data-ttu-id="71b8c-144">To umožňuje hello Media Player tooinject hello datový proud videa na hello element a také ovládat přehrávání podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="71b8c-144">This enables hello MediaPlayer tooinject hello video stream into hello element and also control playback as necessary.</span></span>

<span data-ttu-id="71b8c-145">Předat hello URL hello MPD souboru toohello Media Player tak, že ví o hello video očekává se funkce setupVideo() tooplay.hello právě vytvořili, bude nutné toobe provést po stránku hello plně načetl.</span><span class="sxs-lookup"><span data-stu-id="71b8c-145">Pass hello URL of hello MPD file toohello MediaPlayer so that it knows about hello video it is expected tooplay.hello setupVideo() function just created will need toobe executed once hello page has fully loaded.</span></span> <span data-ttu-id="71b8c-146">To provést pomocí události při načtení hello hello textu elementu.</span><span class="sxs-lookup"><span data-stu-id="71b8c-146">Do this by using hello onload event of hello body element.</span></span> <span data-ttu-id="71b8c-147">Změna vaší <body> elementu, který chcete:</span><span class="sxs-lookup"><span data-stu-id="71b8c-147">Change your <body> element to:</span></span>

    <body onload="setupVideo()">

<span data-ttu-id="71b8c-148">Nakonec nastavte velikost hello element video hello pomocí šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="71b8c-148">Finally, set hello size of hello video element using CSS.</span></span> <span data-ttu-id="71b8c-149">V prostředí adaptivní streamování totiž zvlášť důležité hello velikost hello video přehrávání může změnit při přehrávání přizpůsobuje toochanging síťové podmínky.</span><span class="sxs-lookup"><span data-stu-id="71b8c-149">In an adaptive streaming environment, this is especially important because hello size of hello video being played may change as playback adapts toochanging network conditions.</span></span> <span data-ttu-id="71b8c-150">V této ukázce jednoduché jednoduše vynuťte hello video element toobe 80 % k dispozici prohlížeč okna hello přidáním hello následující šablon stylů CSS toohello head oddílu hello stránky:</span><span class="sxs-lookup"><span data-stu-id="71b8c-150">In this simple demo simply force hello video element toobe 80% of hello available browser window by adding hello following CSS toohello head section of hello page:</span></span>

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a><span data-ttu-id="71b8c-151">Přehrávání videa</span><span class="sxs-lookup"><span data-stu-id="71b8c-151">Playing a Video</span></span>
<span data-ttu-id="71b8c-152">tooplay video, přejděte v prohlížeči na hello basicPlayback.html souboru a klikněte na tlačítko Přehrát v přehrávání videa hello zobrazí.</span><span class="sxs-lookup"><span data-stu-id="71b8c-152">tooplay a video, point your browser at hello basicPlayback.html file and click play on hello video player displayed.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="71b8c-153">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="71b8c-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="71b8c-154">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="71b8c-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="71b8c-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="71b8c-155">See Also</span></span>
[<span data-ttu-id="71b8c-156">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="71b8c-156">Develop video player applications</span></span>](media-services-develop-video-players.md)

[<span data-ttu-id="71b8c-157">Dash.js úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="71b8c-157">GitHub dash.js repository</span></span>](https://github.com/Dash-Industry-Forum/dash.js) 

