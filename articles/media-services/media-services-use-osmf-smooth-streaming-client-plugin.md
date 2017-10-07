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
# <a name="how-toouse-hello-microsoft-smooth-streaming-plugin-for-hello-adobe-open-source-media-framework"></a>Jak tooUse hello modulu plug-in pro technologie Smooth Streaming Microsoft pro hello Adobe otevřený zdroj Media Framework
## <a name="overview"></a>Přehled
Hello Microsoft technologie Smooth Streaming modulu plug-in pro otevřený zdroj Media Framework 2.0 (SS pro OSMF) rozšiřuje možnosti výchozí hello OSMF a přidá Microsoft technologie Smooth Streaming toonew přehrávání obsahu a existující OSMF přehrávače. modul plug-in Hello také přidá technologie Smooth Streaming tooStrobe možnosti přehrávání média přehrávání (SMP).

SS pro OSMF zahrnuje dvě verze modulu plug-in:

* Statické technologie Smooth Streaming modul plug-in pro OSMF (.swc)
* Dynamické technologie Smooth Streaming modul plug-in pro OSMF (SWF)

Tento dokument předpokládá, že čtečka hello obsahuje obecné praktické znalosti OSMF a OSMF moduly plug-in. Další informace o OSMF, naleznete v dokumentaci hello na hello [oficiální web OSMF](http://osmf.org/).

### <a name="smooth-streaming-plugin-for-osmf-20"></a>Modul plug-in technologie Smooth Streaming pro OSMF 2.0
modul plug-in Hello podporuje načítání a přehrávání obsahu na vyžádání funkce Smooth Streaming s hello následující funkce:

* Přehrávání technologie Smooth Streaming na vyžádání (Play, pozastavení, hledání, zastavte)
* Přehrávání živé vysílání funkce Smooth Streaming (Play)
* Funkce formátu DVR (pozastavení, hledání, přehrávání formátu DVR, přejděte to-Live) za provozu
* Podpora pro video kodeky - H.264
* Podpora pro zvuk kodeky - AAC
* Přepínání s rozhraními API předdefinované OSMF více jazyků zvuk
* Maximální počet přehrávání kvality výběr OSMF integrované rozhraní API
* Něho uzavřený titulky s titulky OSMF modulu plug-in
* Adobe&reg; Flash&reg; Player 11.4 nebo vyšší.
* Tato verze podporuje pouze OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Podporované funkce a známé problémy
Úplný seznam podporovaných funkcích, nepodporované funkce a známé problémy, najdete v části příliš[tento dokument](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).

## <a name="loading-hello-plugin"></a>Načítání hello modulu plug-in
Moduly plug-in OSMF mohou být načteny staticky (v době kompilace) nebo dynamicky (za běhu). Hello technologie Smooth Streaming plugin ke stažení OSMF včetně dynamických a statických verzí.

* Statické načítání: tooload staticky, statické knihovny (SWC) vyžaduje se soubor. Statické modulů plug-in jsou přidány jako odkaz toohello projekty a merge uvnitř závěrečný výstup hello soubor v době kompilace hello.
* Dynamické načtení: tooload dynamicky předkompilovaných (SWF) vyžaduje se soubor. Dynamických modulů plug-in jsou načtena v hello runtime a není součástí projektu výstup hello. (Kompilované výstup) Dynamických modulů plug-in můžete načíst pomocí protokolů HTTP a souboru.

Další informace o statické a dynamické načítání, najdete v oficiální hello [stránka modulů plug-in webu OSMF](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

### <a name="ss-for-osmf-static-loading"></a>SS pro statické načítání OSMF
Hello následující fragment kódu ukazuje, jak tooload hello SS modulu plug-in pro OSMF staticky a přehrávání videa základní použití OSMF MediaFactory třídy. Před zahrnutím hello SS pro OSMF kód, zkontrolujte, že hello odkaz na projekt obsahuje statické modulu plug-in hello "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc".

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


### <a name="ss-for-osmf-dynamic-loading"></a>SS pro dynamické načítání OSMF
Hello následující fragment kódu ukazuje, jak tooload hello SS modulu plug-in pro OSMF dynamicky a přehrávání videa základní použití třídy OSMF MediaFactory hello. Před zahrnutím hello SS pro OSMF kód, zkopírujte složky projektu toohello dynamických modulů plug-in "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swf" hello, chcete-li tooload pomocí souboru protokolu, nebo zkopírujte pod webový server, pro zatížení HTTP. Neexistuje žádné tooinclude nutné "MSAdaptiveStreamingPlugin v1.0.3 osmf2.0.swc" v odkazy na projekt hello.

{balíčku

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
}

## <a name="strobe-media--playback-with-hello-ss-odmf-dynamic-plugin"></a>Přehrávání zábleskové médií s hello dynamických modulů plug-in ODMF SS
technologie Smooth Streaming Hello OSMF dynamických modulů plug-in není kompatibilní s [zábleskové média přehrávání (SMP)](http://osmf.org/strobe_mediaplayback.html). Hello SS můžete použít pro OSMF modulu plug-in tooadd technologie Smooth Streaming přehrávání obsahu tooSMP. toodo se kopie "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" v části webového serveru pro zatížení HTTP pomocí hello následující kroky:

1. Procházet hello [stránce instalace přehrávání médií zábleskové](http://osmf.org/dev/2.0gm/setup.html). 
2. Nastavení hello src tooa technologie Smooth Streaming zdroje, (například http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3. Provádění změn konfigurace hello potřeby a klikněte na verzi Preview a aktualizace.
   
   **Poznámka:** obsahu webového serveru musí platný crossdomain.xml. 
4. Zkopírujte a vložte hello kód tooa jednoduchá stránka HTML pomocí svém oblíbeném textovém editoru, například v hello následující ukázka:

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



1. Přidání modulu plug-in toohello technologie Smooth Streaming OSMF kód pro vložení a uložte.
   
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
2. Uložte stránku HTML a publikovat tooa webový server. Procházet toohello publikované webové stránce pomocí vašeho oblíbeného Flash&reg; Player povoleno internetového prohlížeče (Internet Explorer, Chrome, Firefox, atd.).
3. Získejte technologie Smooth Streaming obsah uvnitř aplikace Adobe&reg; Flash&reg; přehrávač.

Další informace o obecné OSMF vývoj, najdete v tématu hello oficiální [OSMF vývoj stránky](http://osmf.org/resources.html).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Microsoft adaptivní datové proudy modulu plug-in pro aktualizaci OSMF](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 

