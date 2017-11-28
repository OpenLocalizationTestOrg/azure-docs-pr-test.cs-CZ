---
title: "aaaSmooth Streaming modul plug-in pro hello otevřené rozhraní zdrojového média"
description: "Zjistěte, jak toouse hello Azure Media Services technologie Smooth Streaming modul plug-in pro hello Adobe otevřený zdroj Media Framework."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6068151f-b6b0-4507-9346-f03416d3d572
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 3cf8e4679279344cf79c3f0e5b28f63adf88179d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a><span data-ttu-id="f5598-103">Jak tooUse hello modulu plug-in pro technologie Smooth Streaming Microsoft pro hello Adobe otevřený zdroj Media Framework</span><span class="sxs-lookup"><span data-stu-id="f5598-103">How tooUse hello Microsoft Smooth Streaming Plugin for hello Adobe Open Source Media Framework</span></span>
## <a name="overview"></a><span data-ttu-id="f5598-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="f5598-104">Overview</span></span>
<span data-ttu-id="f5598-105">Hello Microsoft technologie Smooth Streaming modulu plug-in pro otevřený zdroj Media Framework 2.0 (SS pro OSMF) rozšiřuje možnosti výchozí hello OSMF a přidá Microsoft technologie Smooth Streaming toonew přehrávání obsahu a existující OSMF přehrávače.</span><span class="sxs-lookup"><span data-stu-id="f5598-105">hello Microsoft Smooth Streaming plugin for Open Source Media Framework 2.0 (SS for OSMF) extends hello default capabilities of OSMF and adds Microsoft Smooth Streaming content playback toonew and existing OSMF players.</span></span> <span data-ttu-id="f5598-106">modul plug-in Hello také přidá technologie Smooth Streaming tooStrobe možnosti přehrávání média přehrávání (SMP).</span><span class="sxs-lookup"><span data-stu-id="f5598-106">hello plugin also adds Smooth Streaming playback capabilities tooStrobe Media Playback (SMP).</span></span>

<span data-ttu-id="f5598-107">SS pro OSMF zahrnuje dvě verze modulu plug-in:</span><span class="sxs-lookup"><span data-stu-id="f5598-107">SS for OSMF includes two versions of plugin:</span></span>

* <span data-ttu-id="f5598-108">Statické technologie Smooth Streaming modul plug-in pro OSMF (.swc)</span><span class="sxs-lookup"><span data-stu-id="f5598-108">Static Smooth Streaming plugin for OSMF (.swc)</span></span>
* <span data-ttu-id="f5598-109">Dynamické technologie Smooth Streaming modul plug-in pro OSMF (SWF)</span><span class="sxs-lookup"><span data-stu-id="f5598-109">Dynamic Smooth Streaming plugin for OSMF (.swf)</span></span>

<span data-ttu-id="f5598-110">Tento dokument předpokládá, že čtečka hello obsahuje obecné praktické znalosti OSMF a OSMF moduly plug-in. Další informace o OSMF, naleznete v dokumentaci hello na hello [oficiální web OSMF](http://osmf.org/).</span><span class="sxs-lookup"><span data-stu-id="f5598-110">This document assumes that hello reader has a general working knowledge of OSMF and OSMF plug-ins. For more information about OSMF, please see hello documentation on hello [official OSMF site](http://osmf.org/).</span></span>

### <a name="smooth-streaming-plugin-for-osmf-20"></a><span data-ttu-id="f5598-111">Modul plug-in technologie Smooth Streaming pro OSMF 2.0</span><span class="sxs-lookup"><span data-stu-id="f5598-111">Smooth Streaming plugin for OSMF 2.0</span></span>
<span data-ttu-id="f5598-112">modul plug-in Hello podporuje načítání a přehrávání obsahu na vyžádání funkce Smooth Streaming s hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="f5598-112">hello plugin supports loading and playback of on-demand Smooth Streaming content with hello following features:</span></span>

* <span data-ttu-id="f5598-113">Přehrávání technologie Smooth Streaming na vyžádání (Play, pozastavení, hledání, zastavte)</span><span class="sxs-lookup"><span data-stu-id="f5598-113">On-demand Smooth Streaming playback (Play, Pause, Seek, Stop)</span></span>
* <span data-ttu-id="f5598-114">Přehrávání živé vysílání funkce Smooth Streaming (Play)</span><span class="sxs-lookup"><span data-stu-id="f5598-114">Live Smooth Streaming playback (Play)</span></span>
* <span data-ttu-id="f5598-115">Funkce formátu DVR (pozastavení, hledání, přehrávání formátu DVR, přejděte to-Live) za provozu</span><span class="sxs-lookup"><span data-stu-id="f5598-115">Live DVR functions (Pause, Seek, DVR Playback, Go-to-Live)</span></span>
* <span data-ttu-id="f5598-116">Podpora pro video kodeky - H.264</span><span class="sxs-lookup"><span data-stu-id="f5598-116">Support for video codecs - H.264</span></span>
* <span data-ttu-id="f5598-117">Podpora pro zvuk kodeky - AAC</span><span class="sxs-lookup"><span data-stu-id="f5598-117">Support for Audio codecs - AAC</span></span>
* <span data-ttu-id="f5598-118">Přepínání s rozhraními API předdefinované OSMF více jazyků zvuk</span><span class="sxs-lookup"><span data-stu-id="f5598-118">Multiple audio language switching with OSMF built-in APIs</span></span>
* <span data-ttu-id="f5598-119">Maximální počet přehrávání kvality výběr OSMF integrované rozhraní API</span><span class="sxs-lookup"><span data-stu-id="f5598-119">Max playback quality selection with OSMF built-in APIs</span></span>
* <span data-ttu-id="f5598-120">Něho uzavřený titulky s titulky OSMF modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="f5598-120">Sidecar closed captions with OSMF captions plugin</span></span>
* <span data-ttu-id="f5598-121">Adobe&reg; Flash&reg; Player 11.4 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f5598-121">Adobe&reg; Flash&reg; Player 11.4 or higher.</span></span>
* <span data-ttu-id="f5598-122">Tato verze podporuje pouze OSMF 2.0.</span><span class="sxs-lookup"><span data-stu-id="f5598-122">This version only supports OSMF 2.0.</span></span>

## <a name="supported-features-and-known-issues"></a><span data-ttu-id="f5598-123">Podporované funkce a známé problémy</span><span class="sxs-lookup"><span data-stu-id="f5598-123">Supported features and known issues</span></span>
<span data-ttu-id="f5598-124">Úplný seznam podporovaných funkcích, nepodporované funkce a známé problémy, najdete v části příliš[tento dokument](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span><span class="sxs-lookup"><span data-stu-id="f5598-124">For a full list of supported features, unsupported features and known issues, refer too[this document](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).</span></span>

## <a name="loading-hello-plugin"></a><span data-ttu-id="f5598-125">Načítání hello modulu plug-in</span><span class="sxs-lookup"><span data-stu-id="f5598-125">Loading hello Plugin</span></span>
<span data-ttu-id="f5598-126">Moduly plug-in OSMF mohou být načteny staticky (v době kompilace) nebo dynamicky (za běhu).</span><span class="sxs-lookup"><span data-stu-id="f5598-126">OSMF plugins can be loaded statically (at compile time) or dynamically (at run-time).</span></span> <span data-ttu-id="f5598-127">Hello technologie Smooth Streaming plugin ke stažení OSMF včetně dynamických a statických verzí.</span><span class="sxs-lookup"><span data-stu-id="f5598-127">hello Smooth Streaming plugin for OSMF download includes both dynamic and static versions.</span></span>

* <span data-ttu-id="f5598-128">Statické načítání: tooload staticky, statické knihovny (SWC) vyžaduje se soubor.</span><span class="sxs-lookup"><span data-stu-id="f5598-128">Static loading: tooload statically, a static library (SWC) file is required.</span></span> <span data-ttu-id="f5598-129">Statické modulů plug-in jsou přidány jako odkaz toohello projekty a merge uvnitř závěrečný výstup hello soubor v době kompilace hello.</span><span class="sxs-lookup"><span data-stu-id="f5598-129">Static plugins are added as a reference toohello projects and merge inside hello final output file at hello compile time.</span></span>
* <span data-ttu-id="f5598-130">Dynamické načtení: tooload dynamicky předkompilovaných (SWF) vyžaduje se soubor.</span><span class="sxs-lookup"><span data-stu-id="f5598-130">Dynamic loading: tooload dynamically, a precompiled (SWF) file is required.</span></span> <span data-ttu-id="f5598-131">Dynamických modulů plug-in jsou načtena v hello runtime a není součástí projektu výstup hello.</span><span class="sxs-lookup"><span data-stu-id="f5598-131">Dynamic plugins are loaded in hello runtime and not included in hello project output.</span></span> <span data-ttu-id="f5598-132">(Kompilované výstup) Dynamických modulů plug-in můžete načíst pomocí protokolů HTTP a souboru.</span><span class="sxs-lookup"><span data-stu-id="f5598-132">(Compiled output) Dynamic plugins can be loaded using HTTP and FILE protocols.</span></span>

<span data-ttu-id="f5598-133">Další informace o statické a dynamické načítání, najdete v oficiální hello [stránka modulů plug-in webu OSMF](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span><span class="sxs-lookup"><span data-stu-id="f5598-133">For more information on static and dynamic loading, see hello official [OSMF plugin page](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).</span></span>

### <a name="ss-for-osmf-static-loading"></a><span data-ttu-id="f5598-134">SS pro statické načítání OSMF</span><span class="sxs-lookup"><span data-stu-id="f5598-134">SS for OSMF Static Loading</span></span>
<span data-ttu-id="f5598-135">Hello následující fragment kódu ukazuje, jak tooload hello SS modulu plug-in pro OSMF staticky a přehrávání videa základní použití OSMF MediaFactory třídy.</span><span class="sxs-lookup"><span data-stu-id="f5598-135">hello code snippet below shows how tooload hello SS plugin for OSMF statically and play a basic video using OSMF MediaFactory class.</span></span> <span data-ttu-id="f5598-136">Před zahrnutím hello SS pro OSMF kód, zkontrolujte, že hello odkaz na projekt obsahuje statické modulu plug-in hello "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc".</span><span class="sxs-lookup"><span data-stu-id="f5598-136">Before including hello SS for OSMF code, please ensure that hello project reference includes hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" static plugin.</span></span>

```
package 
{

    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;



    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;

            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.
            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")

        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;

            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
}
```


### <a name="ss-for-osmf-dynamic-loading"></a><span data-ttu-id="f5598-137">SS pro dynamické načítání OSMF</span><span class="sxs-lookup"><span data-stu-id="f5598-137">SS for OSMF Dynamic Loading</span></span>
<span data-ttu-id="f5598-138">Hello následující fragment kódu ukazuje, jak tooload hello SS modulu plug-in pro OSMF dynamicky a přehrávání videa základní použití třídy OSMF MediaFactory hello.</span><span class="sxs-lookup"><span data-stu-id="f5598-138">hello code snippet below shows how tooload hello SS plugin for OSMF dynamically and play a basic video using hello OSMF MediaFactory class.</span></span> <span data-ttu-id="f5598-139">Před zahrnutím hello SS pro OSMF kód, zkopírujte složky projektu toohello dynamických modulů plug-in "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" hello, chcete-li tooload pomocí souboru protokolu, nebo zkopírujte pod webový server, pro zatížení HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5598-139">Before including hello SS for OSMF code, copy hello "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamic plugin toohello project folder if you want tooload using FILE protocol, or copy under a web server for HTTP load.</span></span> <span data-ttu-id="f5598-140">Neexistuje žádné tooinclude nutné "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" v odkazy na projekt hello.</span><span class="sxs-lookup"><span data-stu-id="f5598-140">There is no need tooinclude "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in hello project references.</span></span>

<span data-ttu-id="f5598-141">{balíčku</span><span class="sxs-lookup"><span data-stu-id="f5598-141">package {</span></span>

    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;


    //Sets hello size of hello SWF

    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;


        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }

        private function initMediaPlayer():void
        {

            // Create hello container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds hello container toohello stage
            addChild(_mediaPlayerSprite);

            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();

            // Add hello listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );

            // Load hello plugin class 
            loadAdaptiveStreamingPlugin( );  

        }

        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs toohost a valid crossdomain.xml file tooallow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }

        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // hello plugin is loaded successfully.

            // Your web server needs toohost a valid crossdomain.xml file tooallow plugin toodownload Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }

        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // hello plugin is failed tooload ...
        }


        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;

            state =  event.state;

            switch (state)
            {
                case MediaPlayerState.LOADING: 

                    // A new source is started tooload.

                    break;

                case  MediaPlayerState.READY :   
                    // Add code toodeal with Player Ready when it is hit hello first load after a source is loaded. 

                    break;

                case MediaPlayerState.BUFFERING :

                    break;

                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }

        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }

        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it toohello page.

            var resource:URLResource= new URLResource( sourceURL );

            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add hello media element
            _mediaPlayerSprite.media = element;
        }     

    }
<span data-ttu-id="f5598-142">}</span><span class="sxs-lookup"><span data-stu-id="f5598-142">}</span></span>

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a><span data-ttu-id="f5598-143">Přehrávání zábleskové médií s hello dynamických modulů plug-in ODMF SS</span><span class="sxs-lookup"><span data-stu-id="f5598-143">Strobe Media  Playback with hello SS ODMF Dynamic Plugin</span></span>
<span data-ttu-id="f5598-144">technologie Smooth Streaming Hello OSMF dynamických modulů plug-in není kompatibilní s [zábleskové média přehrávání (SMP)](http://osmf.org/strobe_mediaplayback.html).</span><span class="sxs-lookup"><span data-stu-id="f5598-144">hello Smooth Streaming for OSMF dynamic plugin is compatible with [Strobe Media Playback (SMP)](http://osmf.org/strobe_mediaplayback.html).</span></span> <span data-ttu-id="f5598-145">Hello SS můžete použít pro OSMF modulu plug-in tooadd technologie Smooth Streaming přehrávání obsahu tooSMP.</span><span class="sxs-lookup"><span data-stu-id="f5598-145">You can use hello SS for OSMF plugin tooadd Smooth Streaming content playback tooSMP.</span></span> <span data-ttu-id="f5598-146">toodo se kopie "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" v části webového serveru pro zatížení HTTP pomocí hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5598-146">toodo this, copy "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" under a web server for HTTP load using hello following steps:</span></span>

1. <span data-ttu-id="f5598-147">Procházet hello [stránce instalace přehrávání médií zábleskové](http://osmf.org/dev/2.0gm/setup.html).</span><span class="sxs-lookup"><span data-stu-id="f5598-147">Browse hello [Strobe Media Playback setup page](http://osmf.org/dev/2.0gm/setup.html).</span></span> 
2. <span data-ttu-id="f5598-148">Nastavení hello src tooa technologie Smooth Streaming zdroje, (například http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span><span class="sxs-lookup"><span data-stu-id="f5598-148">Set hello src tooa Smooth Streaming source, (e.g. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest)</span></span> 
3. <span data-ttu-id="f5598-149">Provádění změn konfigurace hello potřeby a klikněte na verzi Preview a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f5598-149">Make hello desired configuration changes and click Preview and Update.</span></span>
   
   <span data-ttu-id="f5598-150">**Poznámka:** obsahu webového serveru musí platný crossdomain.xml.</span><span class="sxs-lookup"><span data-stu-id="f5598-150">**Note** Your content web server needs a valid crossdomain.xml.</span></span> 
4. <span data-ttu-id="f5598-151">Zkopírujte a vložte hello kód tooa jednoduchá stránka HTML pomocí svém oblíbeném textovém editoru, například v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="f5598-151">Copy and paste hello code tooa simple HTML page using your favorite text editor, such as in hello following example:</span></span>

        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



1. <span data-ttu-id="f5598-152">Přidání modulu plug-in toohello technologie Smooth Streaming OSMF kód pro vložení a uložte.</span><span class="sxs-lookup"><span data-stu-id="f5598-152">Add Smooth Streaming OSMF plugin toohello embed code and save.</span></span>
   
        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>
2. <span data-ttu-id="f5598-153">Uložte stránku HTML a publikovat tooa webový server.</span><span class="sxs-lookup"><span data-stu-id="f5598-153">Save your HTML page and publish tooa web server.</span></span> <span data-ttu-id="f5598-154">Procházet toohello publikované webové stránce pomocí vašeho oblíbeného Flash&reg; Player povoleno internetového prohlížeče (Internet Explorer, Chrome, Firefox, atd.).</span><span class="sxs-lookup"><span data-stu-id="f5598-154">Browse toohello published web page using your favorite Flash&reg; Player enabled Internet browser (Internet Explorer, Chrome, Firefox, so on).</span></span>
3. <span data-ttu-id="f5598-155">Získejte technologie Smooth Streaming obsah uvnitř aplikace Adobe&reg; Flash&reg; přehrávač.</span><span class="sxs-lookup"><span data-stu-id="f5598-155">Enjoy Smooth Streaming content inside Adobe&reg; Flash&reg; Player.</span></span>

<span data-ttu-id="f5598-156">Další informace o obecné OSMF vývoj, najdete v tématu hello oficiální [OSMF vývoj stránky](http://osmf.org/resources.html).</span><span class="sxs-lookup"><span data-stu-id="f5598-156">For more information on general OSMF development, please see hello official [OSMF development page](http://osmf.org/resources.html).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="f5598-157">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="f5598-157">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f5598-158">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f5598-158">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="f5598-159">Viz také</span><span class="sxs-lookup"><span data-stu-id="f5598-159">See Also</span></span>
[<span data-ttu-id="f5598-160">Microsoft adaptivní datové proudy modulu plug-in pro aktualizaci OSMF</span><span class="sxs-lookup"><span data-stu-id="f5598-160">Microsoft Adaptive Streaming Plugin for OSMF Update</span></span>](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

