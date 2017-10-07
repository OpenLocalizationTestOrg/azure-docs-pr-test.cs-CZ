---
title: "Přehled zpracování média aaaScaling | Microsoft Docs"
description: "Toto téma je přehled škálování zpracování média pomocí služby Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d17531f79d4c1e0d0fa544c4fb5c083684e706fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-media-processing-overview"></a>Škálování zpracování média – přehled
Tato stránka bude stručně charakterizovat, jak a proč zpracování tooscale média. 

## <a name="overview"></a>Přehled
Účet Media Services je přidružen vyhrazené jednotky typu, který určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy. Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**. Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu. Další informace najdete v tématu hello [vyhrazené typy jednotek](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision účtu s jednotky rezervované. Hello počet jednotek rezervovaných pro zřízené určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu. Například pokud potom pět média úlohy bude běžet současně, pokud má váš účet jednotky rezervované pro pět, jako jsou toobe úlohy zpracování. zbývající úlohy Hello bude čekat ve frontě hello a bude získat zachyceny pro zpracování postupně po dokončení spuštěná úloha. Pokud účet nemá žádné jednotky rezervované pro zřízení, pak úlohy bude být zachyceny postupně. V takovém případě hello čekací doba mezi jeden dokončení úkolů a hello další jeden spuštění bude záviset na hello dostupnost prostředků systému hello.

## <a name="choosing-between-different-reserved-unit-types"></a>Vybrat jinou jednotku rezervovanou typy
Následující tabulka Hello pomáhá zajistit rozhodnutí při výběru mezi různými rychlostmi kódování. Také poskytuje několik případů srovnávacího testu a poskytuje SAS adresy URL, kterou můžete použít toodownload videa, na kterých můžete provádět vlastní testy:

| Scénáře | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Zamýšlený případ použití |Kódování jednou přenosovou rychlostí. <br/>Soubory SD nebo pod řešení, není čas citlivé, nízkými náklady. |Jednou přenosovou rychlostí a více kódování přenosovou rychlostí.<br/>Normálního využití pro SD i HD kódování. |Jednou přenosovou rychlostí a více kódování přenosovou rychlostí.<br/>Úplné HD a ovladači 4K videa řešení. Čas, citlivé a rychlejší vyřízení kódování. |
| Srovnávacího testu |[Vstupní soubor: 5 minut dlouhé 640x360p v 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Kódování souboru tooa jednou přenosovou rychlostí MP4, v hello stejné rozlišení, trvá přibližně 11 minut. |[Vstupní soubor: 5 minut dlouhé 1280x720p v 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>S "H264 jednou přenosovou rychlostí 720p" přednastavení kódování trvá přibližně 5 minut.<br/><br/>Kódování s "H264 Multiple Bitrate 720p" přednastavených trvá přibližně 11.5 minut. |[Vstupní soubor: 5 minut dlouhé 1920x1080p v 29,97 snímků za sekundu](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>S "H264 jeden Bitrate 1080p" přednastavení kódování trvá přibližně 2.7 minut.<br/><br/>Kódování s "H264 Multiple Bitrate 1080p" přednastavených trvá přibližně 5.7 minut. |

## <a name="considerations"></a>Požadavky
> [!IMPORTANT]
> Prostudujte si důležité informace, které jsou popsané v této části.  
> 
> 

* Jednotky rezervované pro fungovat pro paralelním prováděním veškeré zpracování médií, včetně indexování úloh pomocí Azure Media Indexer.  Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.
* Pokud používáte sdílené hello fondu, který je bez jednotky rezervované pro pak mít vaše úkoly kódovat hello stejného výkonu, stejně jako u S1 RUs. Ale neexistuje žádný čas toohello horní mez, může vaše úkoly strávené v zařazených do fronty stavu a v každém okamžiku, bude spuštěna maximálně pouze jednu úlohu.
* Hello následujících datových centrech nenabízejí hello **S2** vyhrazený typ jednotky: Brazílie – jih a Indie – západ.
* Hello následující datového centra nenabízí hello **S3** vyhrazený typ jednotky: Indie – západ.

## <a name="billing"></a>Fakturace

Budou se vám účtovat skutečné minuty využití rezervovaných jednotek médií. Podrobné vysvětlení, najdete v tématu hello – nejčastější dotazy hello [Media Services ceny](https://azure.microsoft.com/pricing/details/media-services/) stránky.   

## <a name="quotas-and-limitations"></a>Kvóty a omezení
Informace o kvóty a omezení a jak zjistit, tooopen lístek podpory [kvóty a omezení](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Další krok
Dosáhnout hello škálování média zpracování úlohy s některou z těchto technologií: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Azure Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

