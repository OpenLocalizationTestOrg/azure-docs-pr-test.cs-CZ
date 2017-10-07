---
title: aaaAzure Media Services Telemetrie | Microsoft Docs
description: "Tento článek nabízí přehled Azure Media Services telemetrie."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 659e1c947a77aad0e4acacb541d95714da4775ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services Telemetrie

Azure Media Services (AMS) umožňuje data telemetrie/metriky tooaccess pro jeho služby. aktuální verze Hello AMS umožňuje za provozu shromažďování telemetrických dat pro **kanál**, **StreamingEndpoint**a za chodu **archivu** entity. 

Telemetrie je napsána v účtu Azure Storage, který zadáte tabulku úložiště tooa (většinou použijete účet úložiště hello přidružené k účtu AMS). 

Hello telemetrie systému nespravuje uchovávání. Stará data telemetrie hello můžete odebrat odstraněním hello úložiště tabulek.

Toto téma popisuje, jak tooconfigure a využívat hello AMS telemetrie.

## <a name="configuring-telemetry"></a>Konfigurace telemetrie

Telemetrie můžete konfigurovat na úrovni rozlišením součásti. Existují dvě úrovně podrobností "Normální" a "Podrobné". V současné době obou úrovních vrátit hello stejné informace. Je doporučeno toouse "normální. 

Následující témata zobrazit jak Hello tooenable telemetrie:

[Povolení telemetrie s rozhraním .NET](media-services-dotnet-telemetry.md) 

[Povolení telemetrie se zbytkem](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Využívání telemetrické informace

Telemetrie je napsán tooan Azure Storage Table v hello účet úložiště, který jste zadali při konfiguraci telemetrie hello účtu Media Services. Tato část popisuje hello úložiště tabulek pro hello metriky.

Můžete využívat telemetrická data v jednom z následujících způsobů hello:

- Číst data přímo z úložiště tabulek Azure (např. pomocí hello sada SDK úložiště). Popis hello telemetrie úložiště tabulek naleznete v tématu hello **využívání telemetrické informace** v [to](https://msdn.microsoft.com/library/mt742089.aspx) tématu.

Nebo

- Používat podporu hello v hello sady Media Services .NET SDK pro čtení dat úložiště, jak je popsáno v [to](media-services-dotnet-telemetry.md) tématu. 


schéma telemetrie Hello popsané dole je dobrý výkon navrženou toogive v souladu s limity hello Azure Table Storage:

- Data je rozdělena na oddíly pomocí účtu ID a ID služby tooallow telemetrie z každé služby toobe dotaz nezávisle.
- Oddíly obsahují hello datum toogive přiměřené horní mez na velikost oddílu hello.
- Řádek klíče jsou v zpětné čas pořadí tooallow hello nejaktuálnějších telemetrii položky toobe dotaz pro danou službu.

To by mělo umožnit řadu hello běžné dotazy toobe efektivní:

- Paralelní, nezávislé stahování dat pro samostatné služby.
- Načítání všech dat pro danou službu v časovém období.
- Načítání hello nejnovější data pro službu.

### <a name="telemetry-table-storage-output-schema"></a>Telemetrie tabulky úložiště výstupního schématu

Telemetrická data jsou uložena v agregační funkci v jedné tabulce, "TelemetryMetrics20160321", kde je "20160321" datum vytvoření hello tabulky. Telemetrie systém vytvoří pro každý nový den založené na UTC 00:00 do samostatné tabulky. Tabulka Hello se používá toostore opakované hodnoty jako například ingestování přenosovou rychlostí v rámci daného časového období čas, počet odeslaných bajtů, atd. 

Vlastnost|Hodnota|Příklady a poznámky
---|---|---
Klíč oddílu|{ID účtu} _ {entity ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66 < br /<br/>Hello účet ID je zahrnuta v hello oddílu klíče toosimplify pracovních postupech, kde jsou více účtů Media Services zápis toohello stejný účet úložiště.
RowKey|{sekund toomidnight} _ {náhodná hodnota}|01688_00199<br/><br/>klíč řádku Hello začíná hello počet sekund toomidnight tooallow top n styl dotazů v rámci oddílu. Další informace najdete v tématu [to](../cosmos-db/table-storage-design-guide.md#log-tail-pattern) článku. 
časové razítko|Datum a čas|Automatické časové razítko z hello tabulky Azure 2016-09-09T22:43:42.241Z
Typ|Hello typ entity hello poskytuje data telemetrie|Kanál/StreamingEndpoint/archivu<br/><br/>Typ události je právě hodnotu řetězce.
Name (Název)|Název Hello hello telemetrická data události|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|Hello čas hello telemetrie událostí došlo k chybě (UTC)|2016-09-09T22:42:36.924Z<br/><br/>Hello zaznamenali, že čas zajišťuje hello entity odesílání hello telemetrických dat (například kanál). Může být v době Přibližná problémy synchronizace mezi součástmi, tato hodnota je
ServiceID|{ID služby}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Vlastnosti specifické pro entitu|Podle definice hello událostí|StreamName: stream1, přenosovou rychlostí 10123...<br/><br/>ostatní vlastnosti Hello jsou definovány pro hello daného typu události. Azure obsahu tabulky je párů klíčových hodnot.  (to znamená, že různé řádky v tabulce hello mají různé sady vlastností).

### <a name="entity-specific-schema"></a>Specifické pro entitu schématu

Existují tři typy položek telemetrickými záznamovými data specifická pro entity, které každý vložena se hello následující frekvence:

- Koncové body streamování: každých 30 sekund.
- Live kanály: každou minutu
- Live archivu: každou minutu

**Koncový bod streamování**

Vlastnost|Hodnota|Příklady
---|---|---
Klíč oddílu|Klíč oddílu|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
časové razítko|časové razítko|Automatické časové razítko z Azure Table 2016-09-09T22:43:42.241Z
Typ|Typ|StreamingEndpoint
Name (Název)|Name (Název)|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|ID služby|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Název hostitele|Název hostitele koncového bodu hello|builddemoserver.Origin.mediaservices.Windows.NET
statusCode|Stav záznamů protokolu HTTP|200
resultCode|Podrobnosti výsledku kódu|S_OK
RequestCount|Celkový počet požadavku v hello agregace|3
BytesSent|Agregované počet odeslaných bajtů|2987358
ServerLatency|Server Průměrná latence (včetně úložiště)|129
E2ELatency|Průměrná latence začátku do konce|250

**Živého kanálu**

Vlastnost|Hodnota|Příklady a poznámky
---|---|---
Klíč oddílu|Klíč oddílu|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
časové razítko|časové razítko|Automatické časové razítko z hello tabulky Azure 2016-09-09T22:43:42.241Z
Typ|Typ|Kanál
Name (Název)|Name (Název)|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|ID služby|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Typ sledování video nebo zvuk nebo textu|video nebo zvuk
TrackName|Název sledování hello|video nebo audio_1
Přenosovou rychlostí|Sledování přenosovou rychlostí|785000
CustomAttributes –||   
IncomingBitrate|Skutečné příchozí přenosovou rychlostí|784548
OverlapCount|Ingestování překrývají v hello|0
DiscontinuityCount|Nespojitost pro sledování|0
LastTimestamp|Časové razítko poslední ingestovaný dat|1800488800
NonincreasingCount|Počet fragmentů zahozeny z důvodu zvýšení toonon časové razítko|2
UnalignedKeyFrames|Zda dostali jsme fragment(s) (v rámci úrovně kvality), kde klíč rámce nejsou zarovnány |True
UnalignedPresentationTime|Jestli dostali jsme fragment(s) (v rámci úrovně kvality nebo sleduje), kde není zarovnána čas prezentace|True
UnexpectedBitrate|Hodnotu True pokud vypočítat skutečné přenosovou rychlostí pro zvuk a video sledovat > 40 000 počtu bitů za sekundu a IncomingBitrate == nebo IncomingBitrate a actualBitrate lišit o 50 % 0 |True
V pořádku|Hodnota TRUE, pokud <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> jsou všechny 0|True<br/><br/>V pořádku je composite funkce, která vrátí hodnotu NEPRAVDA, pokud všechny následující podmínky uchování hello:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Za provozu archivu**

Vlastnost|Hodnota|Příklady a poznámky
---|---|---
Klíč oddílu|Klíč oddílu|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
časové razítko|časové razítko|Automatické časové razítko z hello tabulky Azure 2016-09-09T22:43:42.241Z
Typ|Typ|Archiv
Name (Název)|Name (Název)|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|ID služby|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Adresa url programu|Asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4BD2-8c01-a92a2b38c9ba.ISM
TrackName|Název sledování hello|audio_1
TrackType|Typ sledování hello|Zvuku a videa
Atribut CustomAttribute|Hex řetězec, který se odlišuje mezi různé sledování se stejným názvem a přenosovou rychlostí (více fotoaparát úhel)|
Přenosovou rychlostí|Sledování přenosovou rychlostí|785000
V pořádku|Hodnota TRUE, pokud FragmentDiscardedCount == 0 & & ArchiveAcquisitionError hodnotu false|True (tyto dvě hodnoty se nenacházejí v hello metrika, ale jsou přítomna v hello zdroj události)<br/><br/>V pořádku je composite funkce, která vrátí hodnotu NEPRAVDA, pokud všechny následující podmínky uchování hello:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Obecné otázky a odpovědi

### <a name="how-tooconsume-metrics-data"></a>Jak tooconsume metriky dat?

Metriky data se ukládají jako řadu tabulky Azure v účtu úložiště hello zákazníka. Tato data mohou být využívány pomocí hello následující nástroje:

- AMS SDK
- Microsoft Azure Storage Explorer (podporuje formát exportu toocomma oddělovači a zpracování v aplikaci Excel)
- REST API

### <a name="how-toofind-average-bandwidth-consumption"></a>Jak toofind průměrná spotřeba šířky pásma?

průměrné využití šířky pásma Hello je hello průměr BytesSent přes časový rozsah, v.

### <a name="how-toodefine-streaming-unit-count"></a>Jak počet jednotek streamování toodefine?

počet jednotek streamování Hello může být definováno jako hello ve špičce propustnost ze služby hello koncových bodů streamování dělený hello ve špičce propustnost jeden koncový bod streamování. Hello ve špičce použitelné propustnost jeden koncový bod streamování je 160 MB/s.
Předpokládejme například, že propustnost ve špičce hello ze služby zákazníka je 40 MB/s (hello maximální hodnotu BytesSent přes časový rozsah, v). Počet jednotek streamování hello je rovna too(40 MBps) * (8 bitů/bajtů) /(160 Mbps) = 2 jednotek streamování.

### <a name="how-toofind-average-requestssecond"></a>Jak toofind průměrný počet požadavků za sekundu?

toofind hello průměrný počet požadavků za sekundu, výpočetní hello průměrný počet požadavků (RequestCount) časový rozsah, v.

### <a name="how-toodefine-channel-health"></a>Jak toodefine kanálu stavu?

Stav kanálu je možné definovat jako složené logická funkce tak, že je false v případě, že některé z následujících podmínek hello uložení:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-toodetect-discontinuities"></a>Jak toodetect nespojitosti?

toodetect nespojitosti najít všechny záznamy dat kanál kde DiscontinuityCount > 0. Hello odpovídající časové razítko ObservedTime označuje hello časy, kdy hello nespojitosti došlo.

### <a name="how-toodetect-timestamp-overlaps"></a>Jak se překrývá toodetect časové razítko?

toodetect časové razítko překrytí, najít všechny záznamy dat kanál kde OverlapCount > 0. Hello odpovídající časové razítko ObservedTime označuje hello časy, ve které hello časové razítko překrývá došlo k chybě.

### <a name="how-toofind-streaming-request-failures-and-reasons"></a>Jak toofind streamování požadavku selhání a z důvodů?

toofind streamování chyby požadavků a z důvodů najít všechny záznamy dat koncový bod streamování, kde ResultCode není rovno tooS_OK. odpovídající pole StatusCode Hello označuje hello důvodem selhání požadavku hello.

### <a name="how-tooconsume-data-with-external-tools"></a>Jak tooconsume dat s externími nástroji?

Telemetrickými záznamovými dat můžete zpracovat a vizualizována s hello následující nástroje:

- PowerBI
- Application Insights
- Azure monitorování (dříve materiálů uložených)
- AMS Live řídicí panel
- Portál Azure (čeká na uvolnění)

### <a name="how-toomanage-data-retention"></a>Jak toomanage uchovávání dat?

Hello telemetrie systému neposkytuje Správa uchovávání dat nebo automatické odstranění starých záznamů. Proto potřebovat toomanage a ručně odstranit staré záznamy z tabulky úložiště hello. Jak se může vztahovat toostorage SDK toodo ho.

## <a name="next-steps"></a>Další kroky

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
