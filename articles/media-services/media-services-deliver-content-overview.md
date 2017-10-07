---
title: obsahu toocustomers aaaDelivering | Microsoft Docs
description: "Toto téma poskytuje přehled o postup při doručování obsahu pomocí služby Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 89ede54a-6a9c-4814-9858-dcfbb5f4fed5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 0570bd62d9d42633df0132f9449b357e2abb4086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deliver-content-toocustomers"></a>Doručování obsahu toocustomers
Pokud jste doručování streamování nebo na vyžádání video-on-demand obsahu toocustomers, vaším cílem je toodeliver vysokou kvalitu videa toovarious zařízení v různých síťových podmínkách.

tooachieve tohoto cíle, můžete:

* Kódovat datový proud videa datového proudu tooa více přenosovými rychlostmi (adaptivní přenosová rychlost). To se postará o kvalitu i síťové podmínky.
* Použít Microsoft Azure Media Services [dynamické balení](media-services-dynamic-packaging-overview.md) toodynamically znovu zabalte datový proud do různých protokolů. To se postará o streamování na různá zařízení. Služba Media Services podporuje doručování následujících adaptivní přenosové rychlosti streamování technologie hello: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG-DASH.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

Tento článek nabízí přehled konceptů důležité doručování obsahu.

najdete v části Známé problémy, toocheck [známé problémy](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamické balení
Při dynamickém balení hello této služba Media Services poskytuje, že abyste mohli zajistit obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG-DASH, HLS, technologie Smooth Streaming) bez nutnosti toore balíček do těchto formátů streamování. Doporučujeme, abyste doručování obsahu při dynamickém balení.

tootake výhod dynamického balení, budete potřebovat tooencode váš soubor mezzanine (zdrojový) soubor do sady souborů MP4 s adaptivní přenosovou rychlostí nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.

Při dynamickém balení, ukládání a platit hello soubory v jednom úložném formátu. Služba Media Services bude sestavovat a dodávat hello odpovídající reakci na vaše požadavky.

Dynamické balení je k dispozici pro standard a premium koncové body streamování. 

Další informace najdete v tématu [dynamické balení](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filtry a dynamické manifestů
Můžete definovat filtry pro vaše prostředky pomocí služby Media Services. Tyto filtry jsou serverové pravidla, která umožní vašim zákazníkům provést akce, jako přehrání určitou část videa nebo určete podmnožinu interpretace audia a videa, které může zpracovat vašeho zákazníka zařízení (ne všechny interpretace hello, které jsou přidruženy hello asset ). Tento filtrování je dosaženo pomocí *dynamické manifesty* který vytvářejí, když váš zákazník požaduje toostream video na základě jedné nebo více určenými filtry.

Další informace najdete v tématu [filtry a dynamické manifesty](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Lokátory
tooprovide uživatelů s adresou URL, která se dá použít toostream nebo stažení vašeho obsahu, musíte nejprve toopublish asset vytvořením lokátoru. Lokátor poskytuje vstupní bod tooaccess hello soubory, které jsou obsaženy v assetu. Služba Media Services podporuje dva typy lokátorů:

* Lokátory OnDemandOrigin. Tyto jsou použité toostream média (například MPEG-DASH, HLS nebo technologie Smooth Streaming) nebo progresivně stahovat soubory.
* Lokátory URL sdílený přístupový podpis (SAS). Toto jsou použité toodownload média soubory tooyour místního počítače.

*Zásada přístupu* je použité toodefine hello oprávnění (například pro čtení, zápisu a seznamu) a doby trvání, pro kterou má klient přístup pro konkrétní prostředek. Všimněte si, že hello seznam oprávnění (AccessPermissions.List) by neměla být použité při vytváření Lokátor OrDemandOrigin.

Lokátory mají datum vypršení platnosti. Hello portál Azure nastaví datum vypršení platnosti 100 let v hello budoucí pro lokátory.

> [!NOTE]
> Pokud jste použili hello Azure portálu toocreate lokátorů před březnem 2015, byly tyto lokátory nastavené tooexpire po dvou letech.
> 
> 

tooupdate na datum vypršení platnosti lokátoru, použijte [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) rozhraní API. Všimněte si, že při aktualizaci hello datum vypršení platnosti lokátoru SAS hello se změní adresa URL.

Lokátory nejsou určené toomanage řízení přístupu na uživatele. Uživatelé s právy tooindividual můžete zajistit jiný přístup pomocí řešení správy digitálních práv (DRM). Další informace najdete v tématu [zabezpečení média](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Při vytváření lokátoru, může uběhnout 30 sekund kvůli toorequired úložiště a šíření procesy ve službě Azure Storage.

## <a name="adaptive-streaming"></a>Adaptivní streamování
Technologie adaptivní přenosovou rychlostí povolit aplikací pro přehrávání videa toodetermine síťové podmínky a vybrat z několika přenosových rychlostí. Když dojde ke zhoršení síťové komunikace, hello klient může vybrat nižší přenosovou rychlostí, takže přehrávání můžete pokračovat s nižší kvalitu videa. Jak zlepšit síťové podmínky, klient hello přepínat tooa vyšší přenosovou rychlostí s lepší kvalitu videa. Azure Media Services podporuje následující technologie adaptivní přenosové rychlosti hello: HTTP Live Streaming (HLS), technologie Smooth Streaming a MPEG-DASH.

Uživatelé tooprovide s adresami URL streamování, je nejprve nutné vytvořit lokátor OnDemandOrigin. Vytváření hello Lokátor poskytuje hello základní cesta toohello asset, který obsahuje obsah hello chcete toostream. Však možné toostream toobe tento obsah, je nutné toomodify další tuto cestu. Úplná adresa URL toohello streamování soubor manifestu, musí řetězení hello Lokátor cesty tooconstruct hodnota a hello (filename.ism) název souboru manifestu. Pak připojit **/Manifest** a cestu Lokátor toohello příslušný formát (v případě potřeby).

> [!NOTE]
> Můžete také Streamovat obsah pomocí připojení SSL. toodo, zkontrolujte, že spustíte adresy URL streamování s protokolem HTTPS. Všimněte si, že v současné době AMS nepodporuje SSL s vlastní domény.  
> 


Pouze proudy přes protokol SSL Pokud hello koncový bod, ze kterého doručení obsahu streamování byl vytvořen po 10. září 2014. Pokud vaše adresy URL streamování založený na hello streamování koncové body vytvořené po 10. září 2014 hello adresa URL obsahuje "streaming.mediaservices.windows.net." Streamování adresy URL, které obsahují "origin.mediaservices.windows.net" (starý formát hello) nepodporují SSL. Pokud je adresa URL v původním formátu hello a chcete mít toostream toobe přes protokol SSL, vytvořte nový koncový bod streamování. Použití adresy URL na základě hello nové toostream koncový bod streamování vašeho obsahu přes protokol SSL.

## <a name="streaming-url-formats"></a>Adresa URL formátů streamování
### <a name="mpeg-dash-format"></a>Formátu MPEG-DASH
{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)

### <a name="apple-http-live-streaming-hls-v4-format"></a>Formát V4 Apple HTTP Live Streaming (HLS)
{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Formát Apple HTTP Live Streaming (HLS) V3
{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Formát Apple HTTP Live Streaming (HLS) s filtrem jen zvuk
Ve výchozím nastavení jsou pouze sleduje součástí hello HLS manifest. To je potřeba pro certifikaci Apple Store pro mobilní sítě. V takovém případě pokud je klient nemá dostatečnou šířku pásma nebo je připojený prostřednictvím připojení 2G, přehrávání přepínačů jen tooaudio. To pomůže tookeep obsah streamování bez ukládání do vyrovnávací paměti, ale žádné video. V některých scénářích může být player ukládání do vyrovnávací paměti upřednostňované přes jen zvukovém souboru. Pokud chcete sledovat pouze hello tooremove, přidejte **pouze = false** toohello adresy URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3,Audio-only=false)

Další informace najdete v tématu [dynamické složení Manifest podporu a HLS výstupní další funkce](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

### <a name="smooth-streaming-format"></a>Funkce Smooth Streaming formátu
{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Příklad:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Technologie Smooth Streaming 2.0 manifest (starší verze manifestu)
Ve výchozím nastavení technologie Smooth Streaming manifestu formátu obsahuje hello opakování značky (r-tag). Některé přehrávače však nepodporují hello r-tag. Klienti s tyto přehrávače můžete použít formát, který zakáže hello r-tag:

{streamování koncový bod služby media název účtu name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

## <a name="progressive-download"></a>Progresivní stahování
S progresivní stahování můžete spustit přehrávání média, než byl stažen hello celý soubor. .Ism * (ismv isma, ismt soubory nebo ismc) nemůže progresivně stahovat.

tooprogressively stahování obsahu, použijte hello OnDemandOrigin Typ lokátoru. Hello následující příklad ukazuje hello URL, která je založena na hello Lokátor OnDemandOrigin typ:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Musíte dešifrovat všechny prostředky šifrovat úložiště, které chcete toostream z hello počátek služby pro progresivní stahování.

## <a name="download"></a>Ke stažení
toodownload obsahu tooa klientském zařízení, musíte vytvořit lokátor SAS. poskytuje Lokátor SAS Hello přístup kontejneru toohello Azure Storage, kde je soubor uložený. Adresa URL pro toobuild hello stahování, máte název souboru hello tooembed mezi hostiteli hello a podpis SAS.

Hello následující příklad ukazuje hello URL, která je založena na lokátoru SAS hello:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

použít Hello následující aspekty:

* Musíte dešifrovat všechny prostředky šifrovat úložiště, které chcete toostream z hello počátek služby pro progresivní stahování.
* Stažení, který nebyl dokončen v rámci 12 hodin se nezdaří.

## <a name="streaming-endpoints"></a>Koncové body streamování

Koncový bod streamování představuje streamování služba, která může poskytnout obsah přímo tooa klientský přehrávač aplikace nebo tooa sítě pro doručování obsahu (CDN) pro další distribuci. výstupní datový proud Hello z datových proudů koncový bod služby může být živý datový proud nebo asset videa na přání ve vašem účtu Media Services. Existují dva typy koncových bodů, streamování **standardní** a **premium**. Další informace najdete v tématu [Streaming koncové body přehled](media-services-streaming-endpoints-overview.md).

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 

## <a name="known-issues"></a>Známé problémy
### <a name="changes-toosmooth-streaming-manifest-version"></a>Změny tooSmooth Streaming verze manifestu
Před vrácením hello verze služby července 2016 – když manifest prostředků vyprodukované Media Encoder Standard, Media Encoder Premium pracovního postupu nebo hello starší Azure Media Encoder byly streamování pomocí dynamické balení – hello technologie Smooth Streaming by odpovídat. tooversion 2.0. Ve verzi 2.0 nepoužívejte hello fragment doby hello takzvané opakovat (r) značky. Například:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

V hello verze služby července 2016 technologie Smooth Streaming manifest hello generované vyhovuje tooversion 2.2, s dobami trvání fragment pomocí opakování značek. Například:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Některé hello starších verzí klientů technologie Smooth Streaming nemusí podporovat hello opakování značky a nebude tooload hello manifestu. toomitigate-li tento problém, můžete použít parametr starší verze manifestu formátu hello **(formát = fmp4 v20)** nebo aktualizovat vaše klienta toohello nejnovější verzi, která podporuje opakování značky. Další informace najdete v tématu [funkce Smooth Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Související témata
[Po vrácení klíče úložiště aktualizuje lokátory Media Services](media-services-roll-storage-access-keys.md)

