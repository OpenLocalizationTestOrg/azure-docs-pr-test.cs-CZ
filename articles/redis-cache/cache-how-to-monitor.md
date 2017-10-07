---
title: aaaHow toomonitor Azure Redis Cache | Microsoft Docs
description: "Zjistěte, jak toomonitor hello stavu a výkonu vaší instance služby Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: c474d485dfcbb109d5bb634a980f6db080598e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-azure-redis-cache"></a>Jak toomonitor Azure mezipaměti Redis
Používá Azure Redis Cache [Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) tooprovide několik možností pro monitorování vaše instance mezipaměti. Můžete zobrazit metriky, připnout metriky grafy toohello úvodní panel, přizpůsobení hello datum a čas rozsah monitorování grafy, přidat a metriky odebrání hello grafy a nastavit upozornění, pokud jsou splněny určité podmínky. Tyto nástroje povolte jste toomonitor hello stav vaší instance služby Azure Redis Cache a vám pomohou při správě aplikace ukládání do mezipaměti.

Metriky pro instance služby Azure Redis Cache jsou shromažďována pomocí hello Redis [informace o](http://redis.io/commands/info) příkaz přibližně dvakrát za minutu a automaticky uložené 30 dní (v tématu [exportovat mezipaměti metriky](#export-cache-metrics) tooconfigure jiné zásady uchovávání informací) tak, aby se zobrazí v grafech hello metriky a vyhodnocuje pravidla výstrah. Další informace o hello různé informace hodnoty používané pro jednotlivé metriky mezipaměti najdete v tématu [k dostupným metrikám a vytváření sestav intervaly](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

metriky mezipaměti tooview, [Procházet](cache-configure.md#configure-redis-cache-settings) tooyour instance mezipaměti v hello [portál Azure](https://portal.azure.com).  Azure Redis Cache nabízí několik předdefinovaných grafy na hello **přehled** okno a hello **Redis metriky** okno. Každý graf lze přizpůsobit přidáním nebo odebráním metriky a změna hello reporting intervalu.

![Metriky pro redis](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Zobrazení předem nakonfigurované metriky grafy

Hello **přehled** okno obsahuje následující předem nakonfigurovaná monitorování grafy hello.

* [Monitorování grafy](#monitoring-charts)
* [Použití grafů](#usage-charts)

### <a name="monitoring-charts"></a>Monitorování grafy
Hello **monitorování** část v hello **přehled** okno obsahuje **přístupy a nezdařených přístupů k**, **získá a nastaví**, **připojení**, a **celkový příkazy** grafy.

![Monitorování grafy](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Použití grafů
Hello **využití** část v hello **přehled** okno obsahuje **zatížení serveru Redis**, **využití paměti**, **šířka pásma sítě **, a **využití procesoru** grafy a také zobrazuje hello **cenová úroveň** pro instanci mezipaměti hello.

![Použití grafů](./media/cache-how-to-monitor/redis-cache-usage-part.png)

Hello **cenová úroveň** zobrazí hello mezipaměti cenovou úroveň a může sloužit příliš[škálování](cache-how-to-scale.md) hello mezipaměti tooa jinou cenovou úroveň.

## <a name="view-metrics-with-azure-monitor"></a>Zobrazit metriky s Azure monitorování
tooview Redis metriky a vytvářet vlastní grafy pomocí Azure monitorování, klikněte na tlačítko **metriky** z hello **prostředků nabídky**a graf pomocí hello potřeby metriky, vytváření sestav interval, typ grafu, přizpůsobit a Další.

![Metriky pro redis](./media/cache-how-to-monitor/redis-cache-monitor.png)

Další informace o práci s metriky pomocí Azure monitorování najdete v tématu [přehled metriky v Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Export mezipaměti metriky
Ve výchozím nastavení, mezipaměti metriky v Azure monitorování jsou [uchovávají po dobu 30 dnů](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) a odstraní. toopersist metrik vaší mezipaměti po dobu delší než 30 dní, můžete [určit účet úložiště](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) a zadejte **uchovávání dat (dny)** zásady pro vaše mezipaměť metriky. 

tooconfigure účet úložiště pro vaše metriky mezipaměti:

1. Klikněte na tlačítko **diagnostiky** z hello **prostředků nabídky** v hello **Redis Cache** okno.
2. Klikněte na tlačítko **na**.
3. Zkontrolujte **archivu účet úložiště tooa**.
4. Vyberte účet úložiště hello v které toostore hello mezipaměti metriky.
5. Zkontrolujte hello **1 minuta** zaškrtávací políčko a zadejte **uchovávání dat (dny)** zásad. Pokud nemáte má tooapply všechny zásady uchovávání informací a navždy zachování dat, nastavte **uchovávání dat (dny)** příliš**0**.
6. Klikněte na **Uložit**.

![Redis diagnostiky](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>V přidání tooarchiving toostorage vaší mezipaměti metriky, můžete také [stream je centra událostí tooan nebo poslat tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

tooaccess vaše metriky lze zobrazit v hello portálu Azure podle předchozích pokynů v tomto článku a můžete taky přejít pomocí hello [REST API služby Azure monitorování metriky](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

> [!NOTE]
> Pokud změníte účty úložiště, zůstane hello data v hello dříve nakonfigurovaný účet úložiště k dispozici ke stažení, ale se nezobrazí v hello portálu Azure.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>K dostupným metrikám a vytváření sestav intervaly
Metriky mezipaměti jsou hlásila použití intervalech několik sestav, včetně **po hodině**, **Dnes**, **za minulý týden**, a **vlastní**. Hello **metrika** pro každý metriky graf zobrazuje hello průměrné, minimální a maximální hodnoty pro jednotlivé metriky v grafu hello a některé metriky zobrazení souhrnu pro hello reporting intervalu. 

Jednotlivé metriky zahrnuje dvě verze. Jedna metrika měří výkonu pro celou mezipaměť hello a mezipaměti, které používají [clustering](cache-how-to-premium-clustering.md), druhá verze hello metriku, která zahrnuje `(Shard 0-9)` hello název míry výkonu pro jeden horizontálního oddílu v mezipaměti. Například pokud mezipaměť obsahuje 4 horizontálních oddílů `Cache Hits` je celková velikost hello přístupů do mezipaměti celý text hello, a `Cache Hits (Shard 3)` je právě hello přístupů pro tento horizontálního oddílu hello mezipaměti.

> [!NOTE]
> I v případě hello mezipaměti nečinný bez aktivních klientů připojených aplikací, může se zobrazit některé mezipaměti aktivitou, například s připojenými klienty, využití paměti a operace provádí. Tato aktivita je normální během operace hello instance Azure Redis Cache.
> 
> 

| Metrika | Popis |
| --- | --- |
| Přístupů k mezipaměti |Hello počet úspěšných hledání klíče během zadaného intervalu sestavy hello. To mapuje příliš`keyspace_hits` z hello Redis [informace](http://redis.io/commands/info) příkaz. |
| Neúspěšné přístupy do mezipaměti |Hello počet neúspěšných hledání klíče během zadaného intervalu sestavy hello. To mapuje příliš`keyspace_misses` z hello Redis informace o příkazu. Neúspěšné přístupy do mezipaměti neznamenají, že se vyskytl problém s mezipamětí hello. Například při použití hello mezipaměti vyhraďte programování vzor, aplikace vypadá první hello mezipaměti pro položku. Pokud hello položky není (neúspěšných přístupů do mezipaměti), hello položky se načítají z databáze hello a přidat toohello mezipaměti pro další použití. Neúspěšné přístupy do mezipaměti jsou normální chování hello mezipaměti vyhraďte programování vzor. Pokud se hello počet nezdařených přístupů k mezipaměti je vyšší, než se očekávalo, zkontrolujte hello aplikační logiky, která naplní a přečte z mezipaměti hello. Pokud jsou určený k vyloučení položek z mezipaměti hello z důvodu přetížení toomemory pak mohou být některé Neúspěšné přístupy do mezipaměti, ale lepší metriky toomonitor pro přetížení paměti by `Used Memory` nebo `Evicted Keys`. |
| Připojení klienti |Hello počet mezipaměti toohello připojení klienta během zadaného intervalu sestavy hello. To mapuje příliš`connected_clients` z hello Redis informace o příkazu. Jednou hello [limitu připojení](cache-configure.md#default-redis-server-configuration) je dosaženo následující pokusy o připojení se nezdaří toohello mezipaměti. Všimněte si, že i v případě, že neexistují žádné aktivní klientskou aplikaci, že může být několik instancí připojených klientů z důvodu toointernal procesy a připojení. |
| Vyřazené klíče |Hello položky vyřazování z mezipaměti hello během hello zadat počet sestav interval platnosti toohello `maxmemory` limit. To mapuje příliš`evicted_keys` z hello Redis informace o příkazu. |
| Vypršela platnost klíče |Hello počet položek, které vypršela z mezipaměti hello během zadaného intervalu sestavy hello. Tato hodnota mapuje příliš`expired_keys` z hello Redis informace o příkazu. |
| Celkový počet klíčů  | Hello maximální počet klíčů v mezipaměti hello během hello za časové období vytváření sestav. To mapuje příliš`keyspace` z hello Redis informace o příkazu. Z důvodu omezení tooa hello základní metriky systému pro mezipaměti s clusteringem povolené vrátí celkový počet klíčů hello maximální počet klíčů hello horizontálního oddílu, který měl hello maximální počet klíčů během hello reporting intervalu.  |
| Získá |Hello počet operací get z mezipaměti hello během zadaného intervalu sestavy hello. Tato hodnota je hello součet hello následující hodnoty z hello Redis informace o všech příkaz: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, a `cmdstat_getrange`, a je ekvivalentní toohello součet přístupů do mezipaměti a neúspěchy během hello reporting intervalu. |
| Zatížení serveru redis |Hello podíl cyklů, ve které hello serveru Redis se zpracováním a nečeká nečinnosti pro zprávy. Pokud tento čítač dosáhne 100, znamená to, narazil na serveru Redis hello mezní hodnoty výkonu a nemůže zpracovat hello procesoru fungovat všechny rychlejší. Pokud vidíte vysoké zatížení serveru Redis uvidíte výjimkám časového limitu v klientovi hello. V tomto případě zvažte vertikálním navýšení kapacity nebo dělení dat do více mezipamětí. |
| Nastaví |Hello počet sady operations toohello mezipaměti během hello zadaném intervalu generování sestav. Tato hodnota je hello součet hello následující hodnoty z hello Redis informace o všech příkaz: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , a `cmdstat_setnx`. |
| Celkový počet operací |Celkový počet příkazy, které jsou zpracovány serverem mezipaměti hello během hello Hello zadaném intervalu generování sestav. Tato hodnota mapuje příliš`total_commands_processed` z hello Redis informace o příkazu. Všimněte si, že při použití Azure Redis Cache výhradně pro protokol pub nebo sub bude žádné metriky pro `Cache Hits`, `Cache Misses`, `Gets`, nebo `Sets`, ale bude `Total Operations` metriky, které odráží hello využití mezipaměti pro operace pub nebo sub. |
| Použitá paměť |Hello velikost mezipaměti paměť použitá pro dvojice klíč/hodnota v mezipaměti hello v MB během hello zadána reporting intervalu. Tato hodnota mapuje příliš`used_memory` z hello Redis informace o příkazu. Nezahrnuje metadata nebo fragmentace. |
| Použitá paměť RSS |Hello množství paměti mezipaměti v MB během hello zadat interval vytváření sestav, včetně fragmentace a metadata. Tato hodnota mapuje příliš`used_memory_rss` z hello Redis informace o příkazu. |
| Procesor |Hello využití procesoru serveru Azure Redis Cache hello jako procento během hello zadaném intervalu generování sestav. Tato hodnota mapuje toohello operačního systému `\Processor(_Total)\% Processor Time` čítače výkonu. |
| Mezipaměti pro čtení |Hello množství dat načten z mezipaměti hello v MB za sekundu (MB/s) během hello zadaném intervalu generování sestav. Tato hodnota se odvozuje od hello síťových karet, které podporují hello virtuální počítač, který je hostitelem hello mezipaměti a není Redis konkrétní. **Tato hodnota odpovídá toohello šířku pásma sítě používané této mezipaměti. Pokud chcete tooset si výstrahy pro omezení šířky pásma sítě straně serveru, vytvořte ji pomocí této `Cache Read` čítače. V tématu [Tato tabulka](cache-faq.md#cache-performance) pro hello zjištěnými omezení šířky pásma pro různé mezipaměti cenové úrovně a velikosti.** |
| Mezipaměť zápisu |Hello množství dat zapsaných toohello mezipaměti v MB za sekundu (MB/s) během zadaného intervalu sestavy hello. Tato hodnota se odvozuje od hello síťových karet, které podporují hello virtuální počítač, který je hostitelem hello mezipaměti a není Redis konkrétní. Tato hodnota odpovídá toohello šířku pásma sítě dat odeslaných z klienta hello toohello mezipaměti. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Výstrahy
Můžete nakonfigurovat tooreceive výstrahy založené na protokolech metriky a aktivity. Azure monitorování umožňuje tooconfigure výstrahy toodo hello následující při aktivaci:

* Odeslat oznámení e-mailu
* Volat webhook, jehož
* Vyvolání aplikace logiky Azure

Klikněte na tlačítko pravidla výstrah tooconfigure pro mezipaměť, **výstrah pravidla** z hello **prostředků nabídky**.

![Monitorování](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Další informace o konfiguraci a používání výstrah najdete v tématu [přehled výstrah](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Protokoly aktivit
Protokoly aktivity získat přehled o hello operace, které byly provedeny na vaše instance služby Azure Redis Cache. Se dřív označovala jako "protokoly auditu" nebo "operační protokoly". Pomocí protokolů z aktivity, můžete určit hello ", kdo a kdy" pro všechny zápisu operace (PUT, POST, DELETE) na vaše instance služby Azure Redis Cache. 

> [!NOTE]
> Protokoly aktivity nezahrnují operace čtení (GET).
>
>

Klikněte na protokoly aktivity tooview pro mezipaměť, **protokoly aktivity** z hello **prostředků nabídky**.

Další informace o protokoly aktivity najdete v tématu [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).











