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
# <a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Vložení do aplikace HTML5 s DASH.js Video adaptivní streamování MPEG-DASH
## <a name="overview"></a>Přehled
MPEG-DASH je standard ISO pro adaptivní streamování videa obsahu, který nabízí významné výhody pro osoby, které chcete toodeliver vysoce kvalitní, adaptivní video streamování výstup hello. S MPEG-DASH bude datový proud videa hello automaticky vyřadit nižší definice tooa, stane se zahlcení sítě hello. Tím se snižuje pravděpodobnost hello hello prohlížeče zobrazuje "pozastavený" video při hello player stáhne hello vedle několik sekund tooplay (neboli ukládání do vyrovnávací paměti). Jako zahlcení sítě snižuje, přehrávání videa hello pak vrátí tooa vyšší kvality datového proudu. Tato možnost tooadapt hello šířky pásma požadované výsledkem také rychlejší čas spuštění videa. Znamená, že hello první několik sekund lze přehrávat v segment fast stažení nižší kvality a pak postupujte nahoru vyšší kvality tooa po dostatečná obsah má byla uložená do vyrovnávací paměti.

Dash.js je otevřeným zdrojem MPEG-DASH přehrávání videa napsané v jazyce JavaScript. Jeho cílem je tooprovide přehrávač robustní, a platformy, které můžete volně opakovaně použít v aplikacích, které vyžadují přehrávání videa. Poskytuje MPEG-DASH přehrávání v žádný prohlížeč, který podporuje hello W3C média zdrojového rozšíření (MSE), který ještě dnes je Chrome, Microsoft Edge a IE11 (dalších prohlížečích označili jejich záměrné toosupport MSE). Další informace o DASH.js js, najdete v části úložiště dash.js hello GitHub.

## <a name="creating-a-browser-based-streaming-video-player"></a>Vytváření založené na prohlížeči streamování videa přehrávač
toocreate jednoduché webové stránky, která zobrazuje přehrávání videa s hello očekává řídí takové a play, pozastavení, rewind atd., budete muset:

1. Vytvoření stránky HTML
2. Přidání značka video hello
3. Přidat hello dash.js player
4. Inicializace hello player
5. Přidat některé stylu CSS
6. Zobrazení výsledků hello v prohlížeči, který implementuje MSE

Inicializace hello player může dokončit jenom pár řádků kódu JavaScript. Pomocí dash.js, ve skutečnosti je jednoduchý tooembed MPEG-DASH videa v aplikací využívajících prohlížeč.

## <a name="creating-hello-html-page"></a>Vytváření hello stránky HTML
prvním krokem Hello je stránka toocreate standardní HTML obsahující hello **video** elementu, uložte tento soubor jako basicPlayer.html jako hello následující příklad znázorňuje:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

## <a name="adding-hello-dashjs-player"></a>Přidání hello DASH.js Player
tooadd hello dash.js referenční implementace toohello aplikace, budete potřebovat soubor dash.all.js hello toograb z verze hello 1.0 dash.js projektu. To má být uložen ve složce JavaScript hello vaší aplikace. Tento soubor je soubor pohodlí, který vrátí všechny nezbytné dash.js kód hello do jediného souboru. Pokud máte podívejte kolem hello dash.js úložiště, se najde hello jednotlivé soubory, testování kódu a mnoho dalšího, ale pokud všechny budete chtít toodo je použití dash.js a pak soubor dash.all.js hello je, co potřebujete.

tooadd hello dash.js player tooyour aplikací, přidejte skript značky toohello head oddíl basicPlayer.html:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Dále vytvořte přehrávač funkce tooinitialize hello při načtení stránky hello. Přidejte následující skript po hello řádku, ve kterém můžete načíst dash.all.js hello:

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

Tato funkce nejprve vytvoří DashContext. Toto je aplikace hello tooconfigure použitých pro konkrétní běhového prostředí. Z technického hlediska definuje hello třídy, které hello framework vkládání závislostí musí použít při vytváření aplikace hello. Ve většině případů budete používat Dash.di.DashContext.

V dalším kroku doložit třídu primární hello hello dash.js prostředí, Media Player. Tato třída obsahuje hello základní metody, jako například potřeby přehrání a pozastavit, spravuje hello vztah s hello video element a také hello výklad hello soubor média prezentace popis (MPD), který popisuje přehrát video toobe hello.

Funkce startup() Hello hello Media Player třídy se nazývá tooensure které player hello je připraven tooplay videa. Mimo jiné tato funkce zajišťuje, že všechny potřebné třídy hello (podle definice v kontextu hello) byly načteny. Jakmile hello player je připraven, můžete připojit tooit video element hello pomocí funkce attachView() hello. To umožňuje hello Media Player tooinject hello datový proud videa na hello element a také ovládat přehrávání podle potřeby.

Předat hello URL hello MPD souboru toohello Media Player tak, že ví o hello video očekává se funkce setupVideo() tooplay.hello právě vytvořili, bude nutné toobe provést po stránku hello plně načetl. To provést pomocí události při načtení hello hello textu elementu. Změna vaší <body> elementu, který chcete:

    <body onload="setupVideo()">

Nakonec nastavte velikost hello element video hello pomocí šablon stylů CSS. V prostředí adaptivní streamování totiž zvlášť důležité hello velikost hello video přehrávání může změnit při přehrávání přizpůsobuje toochanging síťové podmínky. V této ukázce jednoduché jednoduše vynuťte hello video element toobe 80 % k dispozici prohlížeč okna hello přidáním hello následující šablon stylů CSS toohello head oddílu hello stránky:

    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

## <a name="playing-a-video"></a>Přehrávání videa
tooplay video, přejděte v prohlížeči na hello basicPlayback.html souboru a klikněte na tlačítko Přehrát v přehrávání videa hello zobrazí.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Viz také
[Vývoj aplikací videopřehrávače](media-services-develop-video-players.md)

[Dash.js úložiště GitHub](https://github.com/Dash-Industry-Forum/dash.js) 

