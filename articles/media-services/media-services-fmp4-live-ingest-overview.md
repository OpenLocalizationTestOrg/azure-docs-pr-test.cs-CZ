---
title: "Media Services aaaAzure fragmentovaných MP4 live ingestování specifikace | Microsoft Docs"
description: "Tato specifikace popisuje protokol hello a formát pro fragmentovaných na základě MP4 živé streamování přijímání pro Azure Media Services. Můžete použít Azure Media Services toostream živé události a všesměrového vysílání obsah v reálném čase pomocí služby Azure jako cloudová platforma hello. Tento dokument taky popisuje osvědčené postupy pro vytváření vysoce redundantní a robustní za provozu ingestování mechanismy."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 0c191f8d6c5a595621feaba0e571fb984b666f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Azure Media Services fragmentovaných MP4 live ingestování specifikace
Tato specifikace popisuje protokol hello a formát pro fragmentovaných na základě MP4 živé streamování přijímání pro Azure Media Services. Služba Media Services poskytuje služba živého streamování, můžete použít toostream živé události a vysílání obsah v reálném čase pomocí služby Azure jako hello Cloudová platforma zákazníků. Tento dokument taky popisuje osvědčené postupy pro vytváření vysoce redundantní a robustní za provozu ingestování mechanismy.

## <a name="1-conformance-notation"></a>1. Zápis shoda
Hello klíčová slova "musí," "musí není," "VYŽADOVÁNA," "SHALL", "se ne," "SHOULD", "By neměl,", "Doporučené", "PRAVDĚPODOBNĚ," a "Volitelné" v tomto dokumentu jsou toobe interpretují jako jsou popsány v dokumentu RFC 2119.

## <a name="2-service-diagram"></a>2. Diagram služby
Hello následující diagram znázorňuje hello Architektura vysoké úrovně hello živé streamování služby ve službě Media Services:

1. Za provozu kodér doručí toochannels za provozu informační kanály, které jsou vytvořeny a zřízené prostřednictvím hello Azure Media Services SDK.
2. V cloudu kanály, programů a koncových bodů streamování v Media Services popisovač všechny hello živé streamování funkce, včetně ingestování, formátování, DVR, zabezpečení, škálovatelnosti a redundance.
3. Zákazníci Volitelně můžete vybrat toodeploy vrstvu Azure Content Delivery Network mezi hello streamování koncový bod a koncové body klienta hello.
4. Datový proud koncové body klienta z koncového bodu streamování pomocí protokolů HTTP adaptivní streamování hello. Mezi příklady patří Microsoft technologie Smooth Streaming, dynamické adaptivní datové proudy přes protokol HTTP (DASH nebo MPEG-DASH) a Apple HTTP Live Streaming (HLS).

![ingestování toku][image1]

## <a name="3-bitstream-format--iso-14496-12-fragmented-mp4"></a>3. Bitový proud formátu – ISO 14496-12 fragmentovaný soubor MP4
Hello přenosový formát pro živé streamování ingestování popsané v tomto dokumentu je založena na [ISO-14496-12]. Podrobné vysvětlení fragmentovaných formátu MP4 a rozšíření jak pro soubory na vyžádání video-on-demand a přijímat živé streamování najdete v tématu [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx).

### <a name="live-ingest-format-definitions"></a>Za provozu ingestování definice formátu
Hello následující seznam popisuje speciální formát, který definice, které se vztahují toolive ingestování do služby Azure Media Services:

1. Hello **ftyp**, **Live pole Manifest serveru**, a **moov** polí, musí se poslat spolu s každou žádostí (HTTP POST). V těchto polích, musí se poslat od začátku hello hello datového proudu a ingestování kdykoli hello kodér musíte znovu připojit ke tooresume datového proudu. Další informace najdete v části 6 v [1].
2. Oddíl 3.3.2 v [1] definuje volitelné pole názvem **StreamManifestBox** pro za provozu ingestování. Z důvodu toohello směrování logiku pro vyrovnávání zatížení Azure hello pomocí toto pole je zastaralé. pole Hello by neměl být při příjem ve službě Media Services. Pokud toto pole je přítomen, Media Services je bezobslužně ignoruje.
3. Hello **TrackFragmentExtendedHeaderBox** musí být k dispozici pro každý fragment pole definovaná v 3.2.3.2 v [1].
4. Verze 2 hello **TrackFragmentExtendedHeaderBox** pole by MĚL být segmentech použité toogenerate média, které mají stejné adresy URL v několika datových centrech. pole indexu fragment Hello je REQUIRED mezi datacenter převzetí služeb při selhání na základě indexu streamování formátů, jako je například Apple HLS a na základě indexu MPEG-DASH. tooenable mezi datacenter převzetí služeb při selhání, hello fragment index musí synchronizovat napříč více kodéry a zvýší o 1 pro každý fragment následných média, dokonce i v jiných kodér restartování nebo selhání.
5. Oddíl 3.3.6 v [1] definuje pole s názvem **MovieFragmentRandomAccessBox** (**mfra**), mohou být odeslány na konci hello za provozu přijímání tooindicate end datového proudu (SESTAVENÁ) toohello kanálu. Kvůli toohello ingestování logiku Media Services pomocí SESTAVENÁ je zastaralá a hello **mfra** pole pro přijímání za provozu by neměla být odeslána. Pokud se odeslat, Media Services ji bezobslužně ignoruje. Stav hello tooreset hello ingestování bodu, doporučujeme vám, že používáte [resetování kanálu](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels). Také doporučujeme použít [zastavit Program](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) tooend prezentace a datový proud.
6. Doba trvání fragment Hello MP4 by měla být konstantní, velikost hello tooreduce hello manifesty klientů. Konstantní trvání fragment MP4 taky zlepšuje heuristiky stažení klienta prostřednictvím použití hello opakování značky. Doba trvání Hello může kolísá toocompensate pro kmitočet celé číslo.
7. Doba trvání Hello MP4 fragment, musí být mezi přibližně 2 až 6 sekund.
8. MP4 fragmentovat časová razítka a indexy (**TrackFragmentExtendedHeaderBox** `fragment_ absolute_ time` a `fragment_index`) by MĚL doručení ve vzestupném pořadí. Fragmenty odolné tooduplicate sice Media Services má omezenou fragmenty tooreorder možnost podle časová osa toohello média.

## <a name="4-protocol-format--http"></a>4. Formát protokolu – HTTP
ISO fragmentovaný soubor MP4 na základě za provozu ingestování pro Media Services použije standardní dlouho běžící HTTP POST požadavek tootransmit kódovaný média data, která je součástí fragmentovaných služby toohello formátu MP4. Každý HTTP POST odešle úplná fragmentovaný proud MP4 ("stream"), od začátku hello s polí hlavičky (**ftyp**, **Live pole Manifest serveru**, a **moov** polí) a pokračujte v sekvenci fragmenty (**moof** a **mdat** polí). Syntaxe adresy URL pro požadavek HTTP POST hello naleznete v části in 9.2 [1]. Je například hello POST URL: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Požadavky
Tady jsou hello podrobné požadavky:

1. Hello kodér by se MĚL spustit hello vysílání odesláním požadavku HTTP POST s prázdnou "text" (nulové délky obsahu) pomocí hello stejná adresa URL pro přijímání. To může pomoct hello kodér rychle zjistit, jestli koncový bod za provozu přijímání hello je platný, a pokud neexistují žádné ověřování nebo jinými podmínkami vyžaduje. Na protokolu HTTP hello server nelze odeslat zpět odpovědi HTTP dokud hello celý, včetně textu hello POST, obdrží žádost. Zadané hello dlouho běžící povaha živé události, bez tohoto kroku hello kodér nemusí být možné toodetect všechny chyby, dokud nedokončí odeslání všech dat hello.
2. Kodér Hello musí zpracovat žádné chyby nebo výzev ověřování z důvodu (1). Pokud (1) úspěšné odpovědi 200, pokračovat.
3. Kodér Hello musí začínat hello fragmentovaný soubor MP4 stream nový požadavek HTTP POST. datová část Hello musí začínat polí hlavičky hello, za nímž následuje fragmenty. Všimněte si, že hello **ftyp**, **Live pole Manifest serveru**, a **moov** polí (v tomto pořadí), musí se poslat s každou žádostí, i v případě, že kodér hello musíte znovu připojit, protože hello předchozí požadavek byl ukončen před toohello konec datového proudu hello. 
4. Kodér Hello musí používat blokové kódování pro odesílání přenosu, protože je možné toopredict hello celého obsahu délce hello živé události.
5. Pokud se hello událostí myši, po odeslání poslední fragment hello, hello kodér musí končit řádně přenos hello blokové kódování sekvenci zpráv (většina zásobníky klienta HTTP zpracování ji automaticky). Kodér Hello musí počkejte hello služby tooreturn hello konečné odpovědi kód a pak ukončit připojení hello. 
6. Kodér Hello nesmí použít hello `Events()` podstatné jméno, jak je popsáno v 9.2 in [1] pro přijímání za provozu ve službě Media Services.
7. Pokud požadavek HTTP POST hello ukončí nebo časy se se koncem předchozí toohello chyba TCP hello datového proudu, hello kodér musí předal nový požadavek POST s použitím nového připojení a postupujte podle hello předchozí požadavky. Kromě toho musí hello kodér znovu odeslal hello předchozí dva MP4 fragmenty pro každý sledování v datovém proudu hello a pokračovat bez zavedení nespojitost v ose média hello. Odešlete hello posledních dvou MP4 fragmenty pro každý sledování zajišťuje, že nedochází ke ztrátě dat. Jinými slovy pokud obsahuje datového proudu audio a video sledovat a aktuální požadavek POST hello selže, hello kodér musíte znovu připojit ke a znovu odeslal hello posledních dvou fragmenty pro hello zvuk sledování, které byly dříve úspěšně odeslán, a hello posledních dvou fragmenty pro Hello video sledování, které byly dříve úspěšně odeslán, tooensure, že je dispozici nedošlo ke ztrátě dat. Kodér Hello musí zachovat "forward" vyrovnávací paměti fragmenty média, které opětovně odešle při opětovném připojení.

## <a name="5-timescale"></a>5. Časová osa
[[MS-SSTR] ](https://msdn.microsoft.com/library/ff469518.aspx) popisuje využití hello časové osy pro **SmoothStreamingMedia** (část 2.2.2.1) a **StreamElement** (část 2.2.2.3) a **StreamFragmentElement**(Část 2.2.2.6), a **LiveSMIL** (část 2.2.7.3.1). Pokud hodnota časový rámec hello není přítomný, hello použita výchozí hodnota 10 000 000 (10 MHz). I když hello technologie Smooth Streaming specifikaci formátu nebrání použití jiných hodnot časový rámec, většina implementace kodér používat toto výchozí nastavení toogenerate hodnotu (10 MHz), technologie Smooth Streaming ingestovat data. Kvůli toohello [Azure Media dynamické balení](media-services-dynamic-packaging-overview.md) funkce, doporučujeme použít za 90 KHz časový rámec pro datové proudy videa a 44,1 KHz nebo 48.1 KHz pro zvukové datové proudy. Pokud jiný časový rámec hodnoty se používají pro různé datové proudy, musí se poslat hello datového proudu úrovni časový rámec. Další informace najdete v tématu [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. Definice "stream"
Datový proud je hello je základní jednotkou operace v za provozu přijímání pro sestavování za provozu prezentací, zpracování datových proudů převzetí služeb při selhání a redundance scénáře. Datový proud je definován jako jeden jedinečný, fragmentovaných MP4 proud, který může obsahovat jediné stopy nebo několik stop. Prezentace úplné online může obsahovat jeden nebo více datových proudů, v závislosti na konfiguraci hello kodéry hello. Hello následující příklady znázorňují různé možnosti používání toocompose datové proudy živého prezentace.

**Příklad:** 

Zákazník chce toocreate živé streamování prezentace, které zahrnuje následující přenosových rychlostí zvuku a videa hello:

Video – 3000 kB/s, 1 500 kb/s, 750 kb/s

Zvuk – 128 kb/s

### <a name="option-1-all-tracks-in-one-stream"></a>Možnost 1: Všechny sleduje v jeden datový proud
V tuto možnost jeden kodér generuje všechny sleduje zvuku a videa a pak je do jedné fragmentovaný proud MP4 obsahuje ureitou. Hello fragmentovaný soubor MP4 proud se pak odešlou prostřednictvím jednoho připojení HTTP POST. V tomto příkladu je pouze jeden datový proud pro prezentaci online.

![Sledování datové proudy: 1][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Možnost 2: Každý sledování v samostatných datového proudu
V tuto možnost hello kodér umístí jeden sledování do každé proud fragment MP4 a odešle všechny hello datové proudy přes samostatné připojení prostřednictvím protokolu HTTP. To lze provést pomocí kodéru jeden nebo více kodéry. za provozu přijímání Hello uvidí prezentaci online jako se skládá z čtyři datových proudů.

![Sleduje samostatné datové proudy][image3]

### <a name="option-3-bundle-audio-track-with-hello-lowest-bitrate-video-track-into-one-stream"></a>Možnost 3: Sady zvukové stopy s hello nejnižší přenosovou rychlostí video sledování do jednoho datového proudu
Tuto možnost zvolí hello zákazníka toobundle hello zvuk sledování s hello sledovat video nejnižší přenosovou rychlostí v jedné fragment MP4 proud a nechte hello další dvě video sleduje jako samostatné datové proudy. 

![Sleduje datové proudy zvuk a video][image4]

### <a name="summary"></a>Souhrn
Nejedná se o vyčerpávající seznam všech možných přijímání možností v tomto příkladu. Jako matter z skutečnosti za provozu přijímání podporuje všechny seskupení sleduje do datových proudů. Zákazníci a dodavatelé encoder můžete zvolit vlastní implementací na základě engineering složitost, kodér kapacitu a redundanci a důležité informace o převzetí služeb při selhání. Ale ve většině případů není pouze jeden zvuk sledovat hello celé za provozu prezentaci. Ano jeho důležité tooensure hello healthiness Dobrý den ingestování datový proud, který obsahuje zvukové stopy hello. Tento aspekt často vede k uvedení hello zvukové stopy v vlastní datový proud (jako možnost 2) nebo sdružování s sledovat videa hello nejnižší přenosovou rychlostí (jako možnost 3). Navíc lepší redundanci a odolnost proti chybám, odesílání hello stejné zvuk sledovat ve dvou různých datových proudů (možnost 2 s redundantní zvukových stop) nebo instalujícího hello zvukové stopy s alespoň dvě z video sleduje hello nejnižší přenosovou rychlostí (možnost 3 s zvuk dodávat v na minimálně dvě datové proudy videa) důrazně doporučujeme pro za provozu ingestování ve službě Media Services.

## <a name="7-service-failover"></a>7. Převzetí služeb při selhání služby
Zadaný hello povaha živé vysílání datového proudu je důležité pro zajištění dostupnosti hello služby hello podporu funkční převzetí služeb při selhání. Služba Media Services je navrženou toohandle různých typů chyb, včetně chyby sítě, server chyb a problémů s úložištěm. Při použití ve spojení s logikou správné převzetí služeb při selhání ze strany hello kodér za provozu, můžete dosáhnout zákazníkům vysoce spolehlivá služba živého streamování z cloudu hello.

V této části probereme scénáře převzetí služeb při selhání služby. V takovém případě se stane chyba hello někde v rámci služby hello a projeví se to jako chybu v síti. Tady jsou některá doporučení pro implementaci hello kodéru pro zpracování převzetí služeb při selhání služby:

1. Použijte 10 sekundu vypršení časového limitu pro vytvoření připojení protokolu TCP hello. Pokud připojení k pokusu o tooestablish hello trvá déle než 10 sekund, přerušit hello operace a akci opakujte. 
2. Použijte krátký časový limit pro odesílání bloky zpráv hello HTTP žádosti. Pokud doba trvání fragment hello cíl MP4 po dobu N sekund, použijte časový limit odeslání N až 2 po dobu N sekund; Například pokud hello MP4 fragment doba je 6 sekund, použijte vypršení časového limitu 6 too12 sekund. Pokud dojde k vypršení časového limitu, resetování hello připojení, otevřete nové připojení a obnovení datového proudu ingestování na hello nové připojení. 
3. Udržujte postupného vyrovnávací paměť, která má hello posledních dvou fragmenty pro každý sledování odeslané úspěšně a úplně toohello služby.  Pokud hello požadavku HTTP POST pro datový proud je ukončen nebo časového limitu předchozí toohello konec datového proudu hello, otevřít nové připojení a začít další požadavek HTTP POST, znovu odeslat hello datového proudu hlavičky, znovu odeslat hello posledních dvou fragmenty pro každý sledovat a obnovit datového proudu hello bez Představení nespojitost v ose hello média. Tím se snižuje riziko ztráty dat hello.
4. Doporučujeme, že kodér hello omezit hello počet opakování tooestablish připojení nebo obnovit streamování potom, co dojde k chybě protokolu TCP.
5. Po chybě TCP:
  
    a. MUSÍ být uzavřeny Hello aktuální připojení a pro nový požadavek HTTP POST je třeba vytvořit nové připojení.

    b. hello Hello nové HTTP POST adresa URL musí být stejný jako hello počáteční adresa URL POST.
  
    c. Hello nové HTTP POST musí obsahovat záhlaví datového proudu (**ftyp**, **Live pole Manifest serveru**, a **moov** polí), které jsou identické toohello datového proudu hlavičky v hello počáteční POST.
  
    d. poslední dva fragmenty Hello odeslané pro každý sledovat nutné odeslat znovu, a vysílání datového proudu musí pokračovat bez zavedení nespojitost v ose média hello. Hello časová razítka fragment MP4 musíte zvýšit nepřetržitě, i přes požadavky HTTP POST.
6. Kodér Hello by MĚL skončí požadavku HTTP POST hello, pokud se data neodesílají rychlostí úměrná hello MP4 fragment trvání.  Požadavek HTTP POST, který neodesílá data zabránit rychle se odpojuje od hello kodér v události hello aktualizace služby Media Services. Z tohoto důvodu hello HTTP POST pro zhuštěné (signál ad) sleduje by měla být krátkodobou, hned, jak se odesílají zhuštěných fragment hello se ukončuje.

## <a name="8-encoder-failover"></a>8. Kodér převzetí služeb při selhání
Kodér převzetí služeb při selhání je hello druhého typu scénáře převzetí služeb při selhání, který potřebuje toobe řešit pro živé streamování doručení začátku do konce. V tomto scénáři hello chybovému stavu dojde na straně kodér hello. 

![Kodér převzetí služeb při selhání][image5]

když kodér převzetí služeb při selhání se stane, platí Hello následující očekávání z koncového bodu za provozu přijímání hello:

1. Kodér vytvořit novou instanci, by měla být toocontinue streamování, jak ukazuje následující diagram hello (datový proud pro 3000 tisíc video s přerušovanou čáru).
2. nové kodér Hello musí použít hello stejné adresy URL pro metodu POST HTTP požadavků jako hello se nezdařilo instance.
3. Hello požadavek POST nové kodér musí zahrnovat hello stejné jako hello nezdařených instancí fragmentovaný soubor MP4 polí hlavičky.
4. nové kodér Hello musí správně synchronizovat se všechny ostatní spuštěné kodéry pro hello stejné toogenerate za provozu prezentace synchronizovat ukázky zvuku a videa s zarovnaný fragment hranice.
5. Hello nového datového proudu musí být sémanticky ekvivalentní s hello předchozí datového proudu a zaměňovat úrovni záhlaví a fragment hello.
6. nové kodér Hello opakovat ztráty dat toominimize. Hello `fragment_absolute_time` a `fragment_index` média měli zvýšit fragmenty z hello bodu, kde poslední zastavení kodér hello. Hello `fragment_absolute_time` a `fragment_index` měli zvýšit průběžně, ale je povolený toointroduce nespojitost, v případě potřeby. Služba Media Services ignoruje fragmenty, které má již přijme a zpracuje, tak, aby byl lépe tooerr na straně hello odešlete fragmenty než toointroduce nespojitosti v ose hello média. 

## <a name="9-encoder-redundancy"></a>9. Kodér redundance
V případě důležitých živých událostí, vyžádání i vyšší dostupnost a kvalitní uživatelský dojem, doporučujeme použít redundantní kodéry v režimu aktivní aktivní tooachieve bezproblémové převzetí služeb při selhání bez ztráty dat..

![Kodér redundance][image6]

Jak je ukázáno v tomto diagramu, dvě skupiny kodéry dvě kopie každé datového proudu současně vkládání do provozu služby hello. Tato instalace je podporována, protože Media Services můžete filtrovat podle ID a fragment datového proudu časové razítko duplicitní fragmenty. Hello výsledné živý datový proud a archivu je jedna kopie všechny datové proudy hello, který je nejlepší možný agregace hello ze dvou zdrojů hello. Například v případě hypotetický extreme, dokud je jeden kodér (nemá toobe hello stejné jeden) spuštěna v libovolném bodě v čase pro každý datový proud hello výsledná živý datový proud z hello služby je souvislý bez ztráty dat. 

Hello požadavky pro tento scénář jsou téměř stejné hello jako hello požadavky v případě "Kodér převzetí" hello, s výjimkou hello, který hello druhá sada kodéry jsou spuštěné v hello stejný čas hello primární kodéry.

## <a name="10-service-redundancy"></a>10. Služba redundance
Pro vysoce redundantní globální distribuci někdy musí mít místní havárie zálohování toohandle mezi oblastmi. Rozšíření na topologii "Kodér redundance" hello, zákazníků můžete zvolit toohave nasazení redundantní služby v jiné oblasti, která je propojená s druhou sadu kodéry hello. Zákazníci také můžete pracovat s toodeploy zprostředkovatele Content Delivery Network globální Traffic Manager před hello dvě služby nasazení tooseamlessly směrovat Klientský provoz. Hello požadavky hello kodéry jsou, stejně jako hello případ "Kodér redundance" hello. Hello pouze výjimkou je hello druhá sada kodéry potřebám toobe za provozu odkazoval tooa různých ingestování koncový bod. Hello následující diagram znázorňuje tento instalační program:

![Služba redundance][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Speciální typy přijímání formáty
Tato část popisuje speciální typy provozu přijímání formátů, které jsou navržené toohandle konkrétních scénářů.

### <a name="sparse-track"></a>Zhuštěný sledování
Při předvádění živé streamování prezentace plně funkčního klienta prostředí, často je nutné tootransmit synchronizovat čas události nebo signály integrovaná s daty hlavní média hello. Příkladem je vkládání reklam dynamické za provozu. Tento typ signalizace událostí se liší od regulární zvuku a videa streamování z důvodu jeho zhuštěných povahy. Jinými slovy hello signalizace data obvykle neodehrává nepřetržitě a hello interval může být pevný toopredict. Hello konceptu zhuštěných sledovat byl navrženou tooingest a vysílání vzdálené signalizační data.

Hello následující kroky jsou doporučené implementace pro příjem zhuštěných sledovat:

1. Vytvořte samostatný fragmentovaný proud MP4, obsahující pouze zhuštěných sleduje bez sleduje zvuku a videa.
2. V hello **Live pole Manifest serveru** definuje v části 6 [1], použijte hello *parentTrackName* parametr toospecify hello název nadřazené dráhy hello. Další informace najdete v části 4.2.1.2.1.2 v [1].
3. V hello **Live pole Manifest serveru**, **manifestOutput** musí být nastaven příliš**true**.
4. Zadané hello zhuštěných povaha hello signalizace událostí, doporučujeme následující hello:
   
    a. Na hello začátku hello živé události, odešle hello kodér hello počáteční záhlaví polí toohello služby, která umožňuje hello služby tooregister hello zhuštěných sledování v manifestu hello klienta.
   
    b. Kodér Hello musí ukončit požadavku HTTP POST hello, když se data neodesílají. Rychle se odpojuje od hello kodér v hello události restartu služby update nebo serveru služby Media Services můžete zabránit HTTP POST dlouho běžící, který není odesílat data. V těchto případech je server mediálních hello dočasně blokovaných v operace příjmu soketem hello.
   
    c. Během doby hello, pokud není k dispozici signalizace dat zavřete hello kodér hello požadavku HTTP POST. Při požadavku POST hello je aktivní, měli hello kodér odesílat data.

    d. Při odesílání zhuštěných fragmenty, hello encoder můžete nastavit explicitní hlavičku content-length, pokud je k dispozici.

    e. Při odesílání zhuštěných fragmenty se nové připojení, by MĚL hello kodér zahájit odesílání z polí hlavičky hello následuje nové fragmenty hello. Se pro případy, ve které převzetí služeb při selhání se stane nevede a hello nové zhuštěných připojení, která má být navázáno tooa nový server, který nebylo nikdy hello zhuštěných sledovat před.

    f. fragment Hello zhuštěných sledování se změní klienta k dispozici toohello při hello odpovídající nadřazené sledovat fragment, který má hodnotu větší nebo rovno časové razítko je k dispozici toohello klienta. Například pokud zhuštěných fragment hello obsahuje časové razítko t = 1000, očekává se, které po hello klienta vidí "video" (za předpokladu, že název sledování nadřazené hello je "video") fragmentovat časové razítko 1000 nebo i mimo můžete stáhnout hello zhuštěných fragment t = 1 000. Všimněte si, že hello signál skutečné by mohly být použity na jiné místo v ose prezentace hello k určené účelu. V tomto příkladu to je možné, že hello zhuštěných fragment t = 1 000 má datovou část XML, který je pro vkládání ad do pozice, který je několik sekund později.

    g. Hello datovou část zhuštěných sledovat fragmenty může být v různých formátech (například XML, text nebo binární), v závislosti na scénáři hello.

### <a name="redundant-audio-track"></a>Redundantní zvuk sledování
Typický HTTP adaptivní streamování scénář (například technologie Smooth Streaming nebo pomlčkou) často má jenom jeden zvukové stopy v celé prezentace hello. Na rozdíl od sleduje video, které mají více úrovní kvality pro klienta toochoose hello z v chybové stavy, může sledovat zvuk hello být jediný bod selhání, pokud hello přijímání hello datový proud, který obsahuje zvukové stopy hello je poškozená. 

Tyto potíže odstranit, služba Media Services podporuje toosolve live přijímání redundantní zvukových stop. Hello cílem je, že hello stejné zvuk sledovat lze odeslat vícekrát v různých datových proudů. I když hello služba pouze zaregistruje zvuk sledovat hello jednou v manifestu hello klienta, může použít redundantní zvukových stop jako záložní pro načítání zvuku fragmenty, pokud primární zvuk sledovat hello má problémy. redundantní zvukových stop tooingest hello kodér musí:

1. Vytvořte hello stejné zvuk sledovat více fragment MP4 bitstreams. redundantní zvukových stop Hello musí být sémanticky ekvivalentní, s hello stejné fragmentovat časová razítka a být zaměnitelné na úrovních fragment a hello záhlaví.
2. Zkontrolujte tuto položku "zvuk" hello v hello Live serveru Manifest (část 6 v [1]) je hello stejné pro všechny redundantní zvukových stop.

Hello následující implementace se doporučuje pro redundantní zvukových stop:

1. Odeslat každý jedinečný zvukové stopy v datovém proudu samostatně. Navíc odesílat redundantní datový proud pro každou z těchto datových proudů zvuk sledovat, pokud druhý datový proud hello liší od hello nejprve pouze o identifikátor hello v hello adresy URL protokolu HTTP POST: {protokolu} :// {serveru address} / {publikování path}/Streams({identifier}) bodu.
2. Použijte samostatné datové proudy toosend hello dvě nejnižší video přenosových rychlostí. Každý z těchto datových proudů by obsahovat také kopii každý jedinečný zvuk sledovat. Například když podporuje více jazyky, těchto datových proudů by MĚLO obsahovat zvuk sleduje pro jednotlivé jazyky.
3. Používat samostatný server (kodér) instancí tooencode a odesílat hello redundantní datové proudy uvedený v (1) a (2). 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
