---
title: "Vložení do aplikace HTML5 s DASH.js Video adaptivní streamování MPEG-DASH | Microsoft Docs"
description: "Toto téma ukazuje, jak pro vložení do aplikace HTML5 s DASH.js Video adaptivní streamování MPEG-DASH."
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
ms.openlocfilehash: 27ce6325773ba1f9fd9cd9ab9e07ea9f5e2488ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a><span data-ttu-id="03e72-103">Vložení do aplikace HTML5 s DASH.js Video adaptivní streamování MPEG-DASH</span><span class="sxs-lookup"><span data-stu-id="03e72-103">Embedding a MPEG-DASH Adaptive Streaming Video in an HTML5 Application with DASH.js</span></span>
## <a name="overview"></a><span data-ttu-id="03e72-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="03e72-104">Overview</span></span>
<span data-ttu-id="03e72-105">MPEG-DASH je standard ISO pro adaptivní streamování videa obsah, který nabízí významné výhody pro ty, kteří chtějí poskytování vysoce kvalitních, adaptivní video streamování výstup.</span><span class="sxs-lookup"><span data-stu-id="03e72-105">MPEG-DASH is an ISO standard for the adaptive streaming of video content, which offers significant benefits for those who wish to deliver high-quality, adaptive video streaming output.</span></span> <span data-ttu-id="03e72-106">S MPEG-DASH bude datový proud videa automaticky vyřadit nižší definice stane zahlcení sítě.</span><span class="sxs-lookup"><span data-stu-id="03e72-106">With MPEG-DASH, the video stream will automatically drop to a lower definition when the network becomes congested.</span></span> <span data-ttu-id="03e72-107">Tím se snižuje pravděpodobnost, že v prohlížeči zobrazuje "pozastavený" video při přehrávač stáhne další několik sekund přehrávání (neboli ukládání do vyrovnávací paměti).</span><span class="sxs-lookup"><span data-stu-id="03e72-107">This reduces the likelihood of the viewer seeing a "paused" video while the player downloads the next few seconds to play (aka buffering).</span></span> <span data-ttu-id="03e72-108">Jako zahlcení sítě snižuje, přehrávání videa pak vrátí datový proud s vyšší kvality.</span><span class="sxs-lookup"><span data-stu-id="03e72-108">As network congestion reduces, the video player will in turn return to a higher quality stream.</span></span> <span data-ttu-id="03e72-109">Tato schopnost přizpůsobit se šířky pásma požadované výsledkem také rychlejší čas spuštění videa.</span><span class="sxs-lookup"><span data-stu-id="03e72-109">This ability to adapt the bandwidth required also results in a faster start time for video.</span></span> <span data-ttu-id="03e72-110">To znamená, že první několik sekund můžete přehrávají v segment fast stažení nižší kvality a potom krok až vyšší jednou dostatečný obsah kvality má byla uložená do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="03e72-110">That means that the first few seconds can be played in a fast-to-download lower quality segment and then step up to a higher quality once sufficient content has been buffered.</span></span>

<span data-ttu-id="03e72-111">Dash.js je otevřeným zdrojem MPEG-DASH přehrávání videa napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="03e72-111">Dash.js is an open source MPEG-DASH video player written in JavaScript.</span></span> <span data-ttu-id="03e72-112">Jeho cílem je zajistit přehrávač robustní, a platformy, které můžete volně opakovaně použít v aplikacích, které vyžadují přehrávání videa.</span><span class="sxs-lookup"><span data-stu-id="03e72-112">Its goal is to provide a robust, cross-platform player that can be freely reused in applications that require video playback.</span></span> <span data-ttu-id="03e72-113">Poskytuje MPEG-DASH přehrávání v žádný prohlížeč, který podporuje dnes W3C média zdrojového rozšíření (MSE), je Chrome, Microsoft Edge a IE11 (dalších prohlížečích označili jejich záměr pro podporu MSE).</span><span class="sxs-lookup"><span data-stu-id="03e72-113">It provides MPEG-DASH playback in any browser that supports the W3C Media Source Extensions (MSE), today that is Chrome, Microsoft Edge and IE11 (other browsers have indicated their intent to support MSE).</span></span> <span data-ttu-id="03e72-114">Další informace o DASH.js js, najdete v části dash.js úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="03e72-114">For more information about DASH.js, js see the GitHub dash.js repository.</span></span>

## <a name="creating-a-browser-based-streaming-video-player"></a><span data-ttu-id="03e72-115">Vytváření založené na prohlížeči streamování videa přehrávač</span><span class="sxs-lookup"><span data-stu-id="03e72-115">Creating a browser-based streaming video player</span></span>
<span data-ttu-id="03e72-116">K vytvoření jednoduché webové stránky, která zobrazuje přehrávání videa s očekávané řídí takové a play, pozastavení, rewind atd., budete muset:</span><span class="sxs-lookup"><span data-stu-id="03e72-116">To create a simple web page that displays a video player with the expected controls such a play, pause, rewind etc., you will need to:</span></span>

1. <span data-ttu-id="03e72-117">Vytvoření stránky HTML</span><span class="sxs-lookup"><span data-stu-id="03e72-117">Create an HTML page</span></span>
2. <span data-ttu-id="03e72-118">Přidat video značky</span><span class="sxs-lookup"><span data-stu-id="03e72-118">Add the video tag</span></span>
3. <span data-ttu-id="03e72-119">Přidat dash.js player</span><span class="sxs-lookup"><span data-stu-id="03e72-119">Add the dash.js player</span></span>
4. <span data-ttu-id="03e72-120">Inicializace player</span><span class="sxs-lookup"><span data-stu-id="03e72-120">Initialize the player</span></span>
5. <span data-ttu-id="03e72-121">Přidat některé stylu CSS</span><span class="sxs-lookup"><span data-stu-id="03e72-121">Add some CSS style</span></span>
6. <span data-ttu-id="03e72-122">Zobrazit výsledky v prohlížeči, který implementuje MSE</span><span class="sxs-lookup"><span data-stu-id="03e72-122">View the results in a browser that implements MSE</span></span>

<span data-ttu-id="03e72-123">Inicializace přehrávač může dokončit jenom pár řádků kódu JavaScript.</span><span class="sxs-lookup"><span data-stu-id="03e72-123">Initializing the player can be completed in just a handful of lines of JavaScript code.</span></span> <span data-ttu-id="03e72-124">Pomocí dash.js, ve skutečnosti je to jednoduché vložit video MPEG-DASH do aplikací využívajících prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="03e72-124">Using dash.js, it really is that simple to embed MPEG-DASH video in your browser based applications.</span></span>

## <a name="creating-the-html-page"></a><span data-ttu-id="03e72-125">Vytvoření stránky HTML</span><span class="sxs-lookup"><span data-stu-id="03e72-125">Creating the HTML Page</span></span>
<span data-ttu-id="03e72-126">Prvním krokem je vytvoření standardní stránku HTML obsahující **video** elementu, uložte tento soubor jako basicPlayer.html jako následující příklad znázorňuje:</span><span class="sxs-lookup"><span data-stu-id="03e72-126">The first step is to create a standard HTML page containing the **video** element, save this file as basicPlayer.html, as the following example illustrates:</span></span>

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-the-dashjs-player"></a><span data-ttu-id="03e72-127">Přidání DASH.js Player</span><span class="sxs-lookup"><span data-stu-id="03e72-127">Adding the DASH.js Player</span></span>
<span data-ttu-id="03e72-128">Pokud chcete přidat odkaz na implementaci dash.js do aplikace, budete muset získat soubor dash.all.js z verzi 1.0 dash.js projektu.</span><span class="sxs-lookup"><span data-stu-id="03e72-128">To add the dash.js reference implementation to the application, you’ll need to grab the dash.all.js file from the 1.0 release of dash.js project.</span></span> <span data-ttu-id="03e72-129">To má být uložen ve složce JavaScript vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="03e72-129">This should be saved in the JavaScript folder of your application.</span></span> <span data-ttu-id="03e72-130">Tento soubor je soubor pohodlí, který vrátí všechny nezbytné dash.js kód do jediného souboru.</span><span class="sxs-lookup"><span data-stu-id="03e72-130">This file is a convenience file that pulls together all the necessary dash.js code into a single file.</span></span> <span data-ttu-id="03e72-131">Pokud máte podívejte kolem dash.js úložiště, bude najít jednotlivých souborů, testování kódu a mnoho dalšího, ale pokud všechny chcete je použít dash.js a pak soubor dash.all.js je, co potřebujete.</span><span class="sxs-lookup"><span data-stu-id="03e72-131">If you have a look around the dash.js repository, you will find the individual files, test code and much more, but if all you want to do is use dash.js, then the dash.all.js file is what you need.</span></span>

<span data-ttu-id="03e72-132">Pokud chcete přidat dash.js player do aplikací, přidejte do části head basicPlayer.html značky script:</span><span class="sxs-lookup"><span data-stu-id="03e72-132">To add the dash.js player to your applications, add a script tag to the head section of basicPlayer.html:</span></span>

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


<span data-ttu-id="03e72-133">Dál vytvořte funkci k chybě při inicializaci přehrávač při načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="03e72-133">Next, create a function to initialize the player when the page loads.</span></span> <span data-ttu-id="03e72-134">Přidejte následující skript po řádek, ve kterém můžete načíst dash.all.js:</span><span class="sxs-lookup"><span data-stu-id="03e72-134">Add the following script after the line in which you load dash.all.js:</span></span>

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

<span data-ttu-id="03e72-135">Tato funkce nejprve vytvoří DashContext.</span><span class="sxs-lookup"><span data-stu-id="03e72-135">This function first creates a DashContext.</span></span> <span data-ttu-id="03e72-136">Slouží ke konfiguraci aplikace pro konkrétní běhového prostředí.</span><span class="sxs-lookup"><span data-stu-id="03e72-136">This is used to configure the application for a specific runtime environment.</span></span> <span data-ttu-id="03e72-137">Z technického hlediska definuje třídy, které rozhraní vkládání závislostí musí použít při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="03e72-137">From a technical point of view, it defines the classes that the dependency injection framework should use when constructing the application.</span></span> <span data-ttu-id="03e72-138">Ve většině případů budete používat Dash.di.DashContext.</span><span class="sxs-lookup"><span data-stu-id="03e72-138">In most cases, you will use Dash.di.DashContext.</span></span>

<span data-ttu-id="03e72-139">Dále vytvořte instanci primární třídu rozhraní dash.js Media Player.</span><span class="sxs-lookup"><span data-stu-id="03e72-139">Next, instantiate the primary class of the dash.js framework, MediaPlayer.</span></span> <span data-ttu-id="03e72-140">Tato třída obsahuje metody, jako například potřeby přehrání a pozastavit, spravuje relaci s video element a také výklad soubor média prezentace popis (MPD), který popisuje video má být přehráván základní.</span><span class="sxs-lookup"><span data-stu-id="03e72-140">This class contains the core methods needed such as play and pause, manages the relationship with the video element and also manages the interpretation of the Media Presentation Description (MPD) file which describes the video to be played.</span></span>

<span data-ttu-id="03e72-141">K zajištění, že přehrávač je připraven k přehrávání videa je volána funkce startup() třídy Media Player.</span><span class="sxs-lookup"><span data-stu-id="03e72-141">The startup() function of the MediaPlayer class is called to ensure that the player is ready to play video.</span></span> <span data-ttu-id="03e72-142">Mimo jiné tato funkce zajišťuje, že všechny potřebné třídy (podle definice v kontextu) byly načteny.</span><span class="sxs-lookup"><span data-stu-id="03e72-142">Amongst other things this function ensures that all the necessary classes (as defined by the context) have been loaded.</span></span> <span data-ttu-id="03e72-143">Jakmile přehrávač je připraven, můžete připojit video element se pomocí funkce attachView().</span><span class="sxs-lookup"><span data-stu-id="03e72-143">Once the player is ready, you can attach the video element to it using the attachView() function.</span></span> <span data-ttu-id="03e72-144">To umožňuje Media Player vložit do elementu datový proud videa a také řídit přehrávání podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="03e72-144">This enables the MediaPlayer to inject the video stream into the element and also control playback as necessary.</span></span>

<span data-ttu-id="03e72-145">Předejte adresu URL souboru MPD Media Player tak, že o video ví, že se očekává, přehrávání. Funkce setupVideo() právě vytvořili, bude nutné provést po plně načtení stránky.</span><span class="sxs-lookup"><span data-stu-id="03e72-145">Pass the URL of the MPD file to the MediaPlayer so that it knows about the video it is expected to play.The setupVideo() function just created will need to be executed once the page has fully loaded.</span></span> <span data-ttu-id="03e72-146">To provést pomocí události při načtení textu elementu.</span><span class="sxs-lookup"><span data-stu-id="03e72-146">Do this by using the onload event of the body element.</span></span> <span data-ttu-id="03e72-147">Změna vaší <body> elementu, který chcete:</span><span class="sxs-lookup"><span data-stu-id="03e72-147">Change your <body> element to:</span></span>

    <body onload="setupVideo()">

<span data-ttu-id="03e72-148">Nakonec nastavte velikost element video pomocí šablon stylů CSS.</span><span class="sxs-lookup"><span data-stu-id="03e72-148">Finally, set the size of the video element using CSS.</span></span> <span data-ttu-id="03e72-149">V adaptivní streamování prostředí je to možnost obzvláště důležité, protože velikost přehrávání videa může změnit při přehrávání přizpůsobuje na měnící se síťové podmínky.</span><span class="sxs-lookup"><span data-stu-id="03e72-149">In an adaptive streaming environment, this is especially important because the size of the video being played may change as playback adapts to changing network conditions.</span></span> <span data-ttu-id="03e72-150">V této ukázce jednoduché jednoduše vynuťte element video být 80 % okna prohlížeče dostupné přidáním následujících šablon stylů CSS do části head stránky:</span><span class="sxs-lookup"><span data-stu-id="03e72-150">In this simple demo simply force the video element to be 80% of the available browser window by adding the following CSS to the head section of the page:</span></span>

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a><span data-ttu-id="03e72-151">Přehrávání videa</span><span class="sxs-lookup"><span data-stu-id="03e72-151">Playing a Video</span></span>
<span data-ttu-id="03e72-152">Pokud chcete přehrát video, přejděte v prohlížeči na soubor basicPlayback.html a kliknutím na tlačítko Přehrát na přehrávání videa zobrazí.</span><span class="sxs-lookup"><span data-stu-id="03e72-152">To play a video, point your browser at the basicPlayback.html file and click play on the video player displayed.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="03e72-153">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="03e72-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="03e72-154">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="03e72-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="03e72-155">Viz také</span><span class="sxs-lookup"><span data-stu-id="03e72-155">See Also</span></span>
[<span data-ttu-id="03e72-156">Vývoj aplikací videopřehrávače</span><span class="sxs-lookup"><span data-stu-id="03e72-156">Develop video player applications</span></span>](media-services-develop-video-players.md)

[<span data-ttu-id="03e72-157">Dash.js úložiště GitHub</span><span class="sxs-lookup"><span data-stu-id="03e72-157">GitHub dash.js repository</span></span>](https://github.com/Dash-Industry-Forum/dash.js) 

