---
title: "aaaAzure Redis Cache – nejčastější dotazy | Microsoft Docs"
description: "Zjistěte, zda text hello odpovědi toocommon otázky, vzory a osvědčené postupy pro Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 2c6ed2f65f755bd08f04857b7af31f520cf4f158
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-redis-cache-faq"></a>Nejčastější dotazy k Azure Redis Cache
Zjistěte, zda text hello odpovědi toocommon otázky, vzorce a osvědčené postupy pro Azure Redis Cache.

## <a name="what-if-my-question-isnt-answered-here"></a>Co když není zde odpovědi můj dotaz?
Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď.

* Můžete odeslat dotaz v komentářích hello na hello konci tohoto článku a zapojit tým Azure Cache hello a ostatními členy komunity o tomto článku.
* tooreach širší cílové skupiny, můžete odeslat dotaz na hello [fórum MSDN mezipaměti Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) a zapojit týmu Azure Cache hello i ostatním členům komunity hello.
* Pokud chcete toomake žádosti o funkci, můžete odeslat požadavky a nápady příliš[Azure Redis Cache User Voice](https://feedback.azure.com/forums/169382-cache).
* Můžete také odeslat e-mailu toous v [externí zpětnou vazbu mezipaměti Azure](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Základy služby Azure Redis Cache
Hello nejčastější dotazy v této části se věnují některé hello Základy služby Azure Redis Cache.

* [Co je Azure Redis Cache?](#what-is-azure-redis-cache)
* [Jak můžete můžu začít pracovat s Azure Redis Cache?](#how-can-i-get-started-with-azure-redis-cache)

Hello zahrnují základní koncepty a otázky týkající se Azure Redis Cache následující nejčastější dotazy a odpovědi v jednom z hello ostatní části Nejčastější dotazy.

* [Jaké nabídky a velikosti Redis Cache mám použít?](#what-redis-cache-offering-and-size-should-i-use)
* [Jaké klientů Redis cache mám použít?](#what-redis-cache-clients-can-i-use)
* [Je k dispozici místní emulátor pro Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Jak monitorování hello stav a výkon Moje mezipaměti?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Plánování nejčastější dotazy
* [Jaké nabídky a velikosti Redis Cache mám použít?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Redis Cache výkonu](#azure-redis-cache-performance)
* [V oblasti, které by měl I najít Můj mezipaměť?](#in-what-region-should-i-locate-my-cache)
* [Jak se fakturuje pro Azure Redis Cache?](#how-am-i-billed-for-azure-redis-cache)
* [Můžete použít Azure Redis Cache pomocí Azure Cloud vlády, Čína cloudu Azure nebo Microsoft Azure v Německu?](#can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Vývoj nejčastější dotazy
* [Co udělat možnosti konfigurace StackExchange.Redis hello?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Jaké klientů Redis cache mám použít?](#what-redis-cache-clients-can-i-use)
* [Je k dispozici místní emulátor pro Azure Redis Cache?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Jak můžete spouštět příkazy Redis?](#how-can-i-run-redis-commands)
* [Proč Azure Redis Cache nemá webu MSDN knihovny tříd jako část hello jinými službami Azure?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Můžete použít Azure Redis Cache jako mezipaměť relace PHP?](#can-i-use-azure-redis-cache-as-a-php-session-cache)
* [Jaké jsou databáze Redis?](#what-are-redis-databases)

## <a name="security-faqs"></a>Nejčastější dotazy k zabezpečení
* [Pokud by měl povolit hello port bez SSL pro připojení tooRedis?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Nejčastější dotazy k produkční
* [Jaké jsou některé z osvědčených postupů produkční?](#what-are-some-production-best-practices)
* [Jaké jsou některé aspekty hello při pomocí běžných příkazů Redis?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Jak mohu srovnávací test a otestovat hello výkonu Moje mezipaměti?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Důležité podrobnosti o fondu růst](#important-details-about-threadpool-growth)
* [Povolit serveru globálního katalogu tooget další propustnosti na klientovi hello, pokud používáte StackExchange.Redis](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Faktory ovlivňující výkon kolem připojení](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>Monitorování a řešení potíží s nejčastější dotazy
Nejčastější dotazy k Hello v této části titulní běžné monitorování a řešení problémů s dotazy. Další informace o monitorování a řešení potíží s vaší instancí Azure Redis Cache najdete v tématu [jak toomonitor Azure mezipaměti Redis](cache-how-to-monitor.md) a [jak tootroubleshoot Azure mezipaměti Redis](cache-how-to-troubleshoot.md).

* [Jak monitorování hello stav a výkon Moje mezipaměti?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Proč se zobrazuje časové limity?](#why-am-i-seeing-timeouts)
* [Proč byl klient odpojen od hello mezipaměti?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Nejčastější dotazy k předchozí mezipaměti nabídky
* [Jaké nabídky Azure Cache je pro mě nejlepší?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-redis-cache"></a>Co je Azure Redis Cache?
Azure Redis Cache je založená na hello oblíbených open-source [Redis cache](http://redis.io). Nabízí přístup k tooa zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem a přístupné ze všech aplikací v rámci Azure. Podrobnější přehled najdete v tématu hello [Azure Redis Cache](https://azure.microsoft.com/services/cache/) stránky produktu na Azure.com.

### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Jak můžete můžu začít pracovat s Azure Redis Cache?
Existuje několik způsobů, které můžete začít používat s Azure Redis Cache.

* Můžete zkontrolovat jednou z našich kurzů k dispozici pro [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), a [Python](cache-python-get-started.md).
* Můžete sledovat [jak tooBuild vysoce výkonné aplikace pomocí Microsoft Azure Redis Cache](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* Můžete zkontrolovat na dokumentaci ke klientu hello hello klientům, kteří odpovídají hello vývoj jazyk vašeho projektu toosee jak toouse Redis. Existuje mnoho klientů Redis, které lze použít s Azure Redis Cache. Seznam klientů Redis, naleznete v části [http://redis.io/clients](http://redis.io/clients).

Pokud účet Azure nemáte, můžete:

* [Otevřít bezplatný účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Získáte kredity, které se dají použít tootry na placené služby Azure. I po hello kredity vyčerpáte, můžete zachovat hello účet a používat bezplatné služby Azure a funkce.
* [Aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Díky předplatnému MSDN každý měsíc získáváte kredity, které můžete použít pro placené služby Azure.

<a name="cache-size"></a>

### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Jakou velikost a kterou nabídku Redis Cache mám použít?
Každou nabídku Azure Redis Cache poskytuje různé úrovně **velikost**, **šířky pásma**, **vysokou dostupnost**, a **SLA** možnosti.

Hello následují faktory pro výběr nabídku mezipaměti.

* **Paměť**: úrovně Basic a Standard hello nabízejí 250 MB – 53 GB. úroveň Premium Hello nabízí až too530 GB. Další informace najdete v tématu [Azure Redis Cache ceny](https://azure.microsoft.com/pricing/details/cache/).
* **Výkon sítě**: Pokud máte zatížení, která vyžaduje vysokou propustnost, úroveň Premium hello nabízí další tooStandard porovnání šířky pásma nebo Basic. Také v jednotlivých vrstvách mít větší velikost mezipaměti větší šířku pásma z důvodu hello základní virtuální počítač, který je hostitelem hello mezipaměti. V tématu hello [následující tabulky](#cache-performance) Další informace.
* **Propustnost**: hello úroveň Premium poskytuje maximální propustnost dostupné hello. Pokud hello mezipaměti serveru nebo klienta dosáhne hello omezení šířky pásma, může se zobrazit časové limity na straně klienta hello. Další informace najdete v tématu hello následující tabulka.
* **Vysoká dostupnost nebo SLA**: Azure Redis Cache zaručuje, že mezipaměť Standard nebo Premium je k dispozici minimálně 99,9 % času hello. najdete v části toolearn Další informace o našich SLA [Azure Redis Cache ceny](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). Hello SLA se vztahuje pouze připojení toohello mezipaměti koncové body. Hello SLA nepokrývá ochranu před únikem. Doporučujeme používat funkce trvalosti dat Redis hello v hello Premium vrstvy tooincrease odolnost proti ztrátě dat.
* **Trvalosti dat redis**: úroveň Premium hello můžete data do mezipaměti hello toopersist v účtu Azure Storage. V mezipaměti Basic nebo Standard všechna data hello uloženo pouze v paměti. Pokud jsou základní problémy infrastruktury může být potenciální ztrátě dat. Doporučujeme používat funkce trvalosti dat Redis hello v hello Premium vrstvy tooincrease odolnost proti ztrátě dat. Azure Redis Cache nabízí RDB a AOF (už brzy) možnosti v trvalosti Redis. Další informace najdete v tématu [jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).
* **Redis Cluster**: toocreate ukládá do mezipaměti větší než 53 GB nebo tooshard data mezi různými uzly Redis, můžete použít Redis clustering, která je dostupná v úrovni Premium hello. Každý uzel se skládá z dvojice primární/replika mezipaměti pro vysokou dostupnost. Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).
* **Rozšířené zabezpečení a sítě izolace**: nasazení virtuální sítě Azure (VNET) poskytuje lepší zabezpečení a izolaci pro vaši Azure Redis Cache, stejně jako podsítě, zásady řízení přístupu a další funkce toofurther omezení přístupu. Další informace najdete v tématu [jak podporují tooconfigure virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md).
* **Konfigurace Redis**: V hello Standard a Premium vrstvy, můžete nakonfigurovat pro oznámení Keyspace Redis.
* **Maximální počet připojení klienta**: úroveň Premium hello nabízí hello maximální počet klientů, které se můžou připojit tooRedis, s vyšší počet připojení pro větší velikosti mezipaměti. Další informace najdete v tématu [cenách Azure Redis Cache](https://azure.microsoft.com/pricing/details/cache/).
* **Vyhrazené jádra serveru Redis**: V úrovni Premium hello, všech velikostí mezipaměti mají vyhrazené jádra pro Redis. Na úrovních Basic nebo Standard hello hello C1 velikost a vyšší mít vyhrazený jádro serveru Redis.
* **Redis je jedním podprocesem** tak s více než dvě jádra neposkytuje Další výhodou oproti má jenom dvě jádra, ale větší velikosti virtuálních počítačů obvykle mají větší šířku pásma než menší velikost. Pokud hello mezipaměti serveru nebo klienta dosáhne hello omezení šířky pásma, obdržíte vypršení časových limitů na straně klienta hello.
* **Vylepšení výkonu**: mezipaměti v úrovni Premium hello jsou nasazeny na hardware, který má rychlejších procesorů, která poskytuje lepší porovnání toohello základní nebo standardní úroveň výkonu. Premium úroveň mezipaměti mají vyšší propustnost a nižší latenci.

<a name="cache-performance"></a>

### <a name="azure-redis-cache-performance"></a>Azure Redis Cache výkonu
Hello následující tabulka uvádí hello maximální šířka pásma hodnotami zjištěnými při testování různých velikostí Standard a Premium ukládá do mezipaměti pomocí `redis-benchmark.exe` z virtuálního počítače Iaas koncový bod Azure Redis Cache hello. 

>[!NOTE] 
>Tyto hodnoty se nezaručuje a neexistuje žádný SLA pro tyto čísla, ale musí být typické. By se měly načíst otestovat vlastní aplikaci toodetermine hello správné velikost mezipaměti pro vaši aplikaci.
>
>

Z této tabulky jsme kreslení hello následující závěry:

* Propustnost pro hello mezipamětí, které jsou hello, které je vyšší ve stejnou velikost hello úroveň Premium jako porovnání toohello úrovně Standard. Například s 6 GB mezipaměti, je propustnost P1 180 000 RPS jako porovnání too49 000 pro C3.
* S Redisem clustering propustnost zvyšuje lineárně zvýšit hello počet horizontálních oddílů (uzlů) v clusteru hello. Například pokud vytvoříte cluster P4 10 horizontálními oddíly, pak hello k dispozici propustnost je 400,000 * 10 = 4 miliony RPS.
* Propustnost pro větší velikosti klíče je vyšší v úrovni Premium hello jako porovnání toohello úrovně Standard.

| Cenová úroveň | Velikost | Procesorová jádra | Dostupná šířka pásma | Velikost hodnoty 1 KB |
| --- | --- | --- | --- | --- |
| **Standardní mezipaměti velikosti** | | |**Megabity za sekundu (Mb/s) nebo megabajtů za sekundu (MB/s)** |**Počet požadavků za sekundu (RPS)** |
| C0 |250 MB |Shared |5 / 0.625 |600 |
| C1 |1 GB |1 |100 / 12.5 |12,200 |
| C2 |2,5 GB |2 |200 / 25 |24,000 |
| C3 |6 GB |4 |400 / 50 |49,000 |
| C4 |13 GB |2 |500 / 62.5 |61,000 |
| C5 |26 GB |4 |1,000 / 125 |115,000 |
| C6 |53 GB |8 |2,000 / 250 |150,000 |
| **Velikost mezipaměti Premium** | |**Jader procesoru na horizontální oddíl** | **Megabity za sekundu (Mb/s) nebo megabajtů za sekundu (MB/s)** |**Počet požadavků za sekundu (RPS), na horizontální oddíl** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |
| P2 |13 GB |4 |3,000 / 375 |360,000 |
| P3 |26 GB |4 |3,000 / 375 |360,000 |
| P4 |53 GB |8 |6,000 / 750 |400 000 |

Pro pokyny ke stahování hello Redis nástroje, jako `redis-benchmark.exe`, najdete v části hello [jak můžete spouštět příkazy Redis?](#cache-commands) části.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>V oblasti, které by měl I najít Můj mezipaměť?
Pro nejlepší výkon a co nejnižší latencí, vyhledejte Azure Redis Cache v hello stejné oblasti jako vaše klientská aplikace mezipaměti.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-redis-cache"></a>Jak se fakturuje pro Azure Redis Cache?
Ceny služby Azure Redis Cache je [zde](https://azure.microsoft.com/pricing/details/cache/). Hello stránce s cenami uvádí ceny jako hodinové sazby. Na základě za minutu z hello čas vytvoření mezipaměti hello dokud hello dobu, po kterou se odstraní mezipaměti se účtují mezipamětí. Neexistuje žádná možnost pro zastavení nebo pozastavení hello fakturace mezipaměti.

### <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Můžete použít Azure Redis Cache pomocí Azure Cloud vlády, Čína cloudu Azure nebo Microsoft Azure v Německu?
Ano, Azure Redis Cache je dostupná v cloudu Azure Government, Čína cloudu Azure a Microsoft Azure v Německu. Hello adresy URL pro přístup a správu Azure Redis Cache se liší v těchto cloudech ve srovnání s veřejném cloudu Azure. 

| Cloud   | Přípona DNS pro Redis            |
|---------|---------------------------------|
| Veřejné  | *. redis.cache.windows.net       |
| Vláda USA  | *. redis.cache.usgovcloudapi.net |
| Německo | *. redis.cache.cloudapi.de       |
| Čína   | *. redis.cache.chinacloudapi.cn  |

Další informace o informace týkající se použití Azure Redis Cache s ostatních cloudů najdete v části hello následující odkazy.

- [Azure Government databází – mezipaměť Redis systému Azure](../azure-government/documentation-government-services-database.md#azure-redis-cache)
- [Cloud Azure Čína - mezipaměť Redis systému Azure](https://www.azure.cn/documentation/services/redis-cache/)
- [Microsoft Azure v Německu](https://azure.microsoft.com/overview/clouds/germany/)

Informace o používání Azure Redis Cache pomocí prostředí PowerShell v cloudu Azure Government, Čína cloudu Azure a Microsoft Azure v Německu najdete v tématu [jak tooconnect tooother cloudy - Azure Redis Cache PowerShell](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-hello-stackexchangeredis-configuration-options-do"></a>Co udělat možnosti konfigurace StackExchange.Redis hello?
StackExchange.Redis má mnoho možností. Tato část pojednává o některých běžných nastavení hello. Podrobné informace o možnostech StackExchange.Redis, najdete v části [StackExchange.Redis konfigurace](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Popis | Doporučení |
| --- | --- | --- |
| AbortOnConnectFail |Pokud nastavíte tootrue, hello připojení nebude znovu připojit po selhání sítě. |Nastavte toofalse a nechat StackExchange.Redis automaticky znovu připojit. |
| ConnectRetry |Hello počet pokusů o připojení toorepeat během počáteční připojení. |V tématu hello následující poznámky k pokyny. |
| ConnectTimeout |Časový limit v ms pro operace connect. |V tématu hello následující poznámky k pokyny. |

Výchozí hodnoty hello hello klienta jsou obvykle dostatečná. Můžete upřesnit možnosti hello podle velikosti pracovní zátěže.

* **Opakování**
  * ConnectRetry a ConnectTimeout najdete obecné pokyny hello je toofail rychlé a zkuste znovu. V tomto návodu je založena na úlohy a jak dlouho na průměrná ho trvá pro vašeho klienta tooissue příkaz Redis a obdrží odpověď.
  * Umožní StackExchange.Redis automaticky znovu připojila, namísto Kontrola stavu připojení a připojení sami. **Nepoužívejte hello ConnectionMultiplexer.IsConnected vlastnost**.
  * Snowballing - někdy může narazíte na problém, kde jsou opakování a hello opakování snowball a nikdy obnoví. Pokud dojde k snowballing, měli byste zvážit použití algoritmu opakování exponenciálního omezení rychlosti jak je popsáno v [opakujte obecné pokyny](../best-practices-retry-general.md) publikováno skupina Microsoft Patterns & postupy hello.
* **Hodnoty časového limitu**
  * Vezměte v úvahu vaše úlohy a nastavovat hodnoty hello odpovídajícím způsobem. Pokud ukládáte velké hodnoty, nastavte vyšší hodnotu tooa hello časového limitu.
  * Nastavit `AbortOnConnectFail` toofalse a nechat StackExchange.Redis znovu připojit za vás.
  * Pomocí jedné instance ConnectionMultiplexer aplikace hello. Můžete použít LazyConnection toocreate jediné instance, který je vrácen vlastností připojení, jak je znázorněno v [připojit pomocí třídy ConnectionMultiplexer hello mezipaměti toohello](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Sada hello `ConnectionMultiplexer.ClientName` vlastnost tooan instance jedinečný název aplikace k diagnostickým účelům.
  * Použití více `ConnectionMultiplexer` instance pro vlastní úlohy.
      * Tento model můžete provést, pokud máte různých zatížení ve vaší aplikaci. Například:
      * Můžete mít jednu multiplexor pro práci s velké klíče.
      * Můžete mít jednu multiplexor pro práci s malé klíče.
      * Můžete nastavit různé hodnoty pro vypršení časových limitů připojení a logika opakovaných pokusů pro každou ConnectionMultiplexer, který používáte.
      * Sada hello `ClientName` vlastnost na každý multiplexor toohelp s diagnostikou.
      * V tomto návodu může způsobit, že toomore zjednodušený latence za `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Jaké klientů Redis cache mám použít?
Jedním z hello užitečných funkcí Redis je, že existuje mnoho klientů podporuje mnoho různých programovacích jazyků. Aktuální seznam klientů najdete v tématu [Redis klienti](http://redis.io/clients). Podrobné pokyny, které zahrnují několik různých jazycích a klienty, najdete v části [jak toouse Azure mezipaměti Redis](cache-dotnet-how-to-use-azure-redis-cache.md) a klikněte na tlačítko jazyka hello potřeby z hello jazyk přepínači hello horní části článku hello.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Je k dispozici místní emulátor pro Azure Redis Cache?
Neexistuje žádné místní emulátor pro Azure Redis Cache, ale může spouštět hello MSOpenTech verzi redis server.exe z hello [nástroje příkazového řádku Redis](https://github.com/MSOpenTech/redis/releases/) ve vašem místním počítači a připojit tooit tooget podobné prostředí tooa místní mezipaměti emulátor, jak je znázorněno v hello následující ukázka:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect tooa locally running instance of Redis toosimulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


Volitelně můžete nakonfigurovat [redis.conf](http://redis.io/topics/config) toomore souboru přesně odpovídat hello [výchozí nastavení mezipaměti](cache-configure.md#default-redis-server-configuration) pro vaše online Azure Redis Cache v případě potřeby.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Jak můžete spouštět příkazy Redis?
Můžete použít některý z příkazů hello uvedený v [Redis příkazy](http://redis.io/commands#) s výjimkou hello příkazy uvedené v [Redis příkazy nejsou podporované ve službě Azure Redis Cache](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Máte několik možností toorun Redis příkazy.

* Pokud máte mezipaměti Standard nebo Premium, můžete spouštět příkazy Redis pomocí hello [konzola Redis](cache-configure.md#redis-console). Konzola Redis Hello poskytuje bezpečný toorun Redis příkazů v hello portálu Azure.
* Můžete také použít nástroje příkazového řádku Redis hello. toouse, provádět hello následující kroky:
* Stáhnout hello [nástroje příkazového řádku Redis](https://github.com/MSOpenTech/redis/releases/).
* Připojení pomocí mezipaměti toohello `redis-cli.exe`. Předejte koncový bod mezipaměti hello pomocí přepínače -h hello a hello klíče pomocí - a jak je znázorněno v hello následující ukázka:
* `redis-cli -h <your cache="" name="">
  .redis.cache.windows.net -a <key>
  `

> [!NOTE]
> Hello nástroje příkazového řádku Redis nefungují s hello SSL port, ale můžete pomocí nástroje, jako `stunnel` toosecurely připojit port SSL toohello nástroje hello podle následujících pokynů hello v hello [uvedení ASP.NET poskytovatele stavu relace pro Redis verze Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) příspěvku na blogu.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-hello-other-azure-services"></a>Proč Azure Redis Cache nemá webu MSDN knihovny tříd jako část hello jinými službami Azure?
Microsoft Azure Redis Cache je založená na populární open source hello Redis Cache a je přístupný pomocí široké škály [Redis klienti](http://redis.io/clients) pro řadě programovacích jazyků. Každý klient má svou vlastní rozhraní API, díky volá instance mezipaměti Redis toohello pomocí [Redis příkazy](http://redis.io/commands).

Protože každého klienta se liší, není jeden centralizované třída odkaz na webu MSDN a každý klient udržuje vlastní referenční dokumentaci k nástroji. Kromě toho toohello referenční dokumentace, existuje několik kurzy znázorňující, jak tooget pracovat s Azure Redis Cache pomocí různých jazycích a klientů mezipaměti. Tyto kurzy tooaccess najdete v části [jak toouse Azure mezipaměti Redis](cache-dotnet-how-to-use-azure-redis-cache.md) a klikněte na tlačítko jazyka hello potřeby z hello jazyk přepínači hello horní části článku hello.

### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Můžete použít Azure Redis Cache jako mezipaměť relace PHP?
Ano, toouse Azure Redis Cache jako mezipaměť relace na PHP, zadejte instanci služby Azure Redis Cache tooyour řetězec připojení hello v `session.save_path`.

> [!IMPORTANT]
> Při použití Azure Redis Cache jako mezipaměť relace PHP, je nutné, aby adresa URL kódování hello zabezpečení klíčů používaných tooconnect toohello mezipaměti, jak je znázorněno v hello následující ukázka:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Pokud není klíč hello kódovaná adresou URL, se může zobrazit výjimka zprávu jako:`Failed tooparse session.save_path`
>
>

Další informace o používání Redis Cache jako mezipaměť relace PHP hello PhpRedis klienta najdete v tématu [obslužné rutiny PHP relace](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Jaké jsou databáze Redis?

Databáze redis jsou právě logického oddělení dat v rámci hello stejnou instanci Redis. mezipaměť Hello jsou sdílena mezi všechny databáze hello a skutečný paměťové nároky na danou databázi závisí na hello klíče nebo hodnotami uloženými v databázi. Například C6 mezipaměti má 53 GB paměti. Můžete zvolit tooput všechny 53 GB do jedné databáze nebo je možné ho rozdělit mezi více databází. 

> [!NOTE]
> Při použití mezipaměť Azure Redis Cache Premium s povoleným clusteringem, je k dispozici pouze databázi 0. Toto omezení vnitřní omezení Redis a není konkrétní tooAzure Redis Cache. Další informace najdete v tématu [potřebuji toomake žádné změny toomy klienta aplikace toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-hello-non-ssl-port-for-connecting-tooredis"></a>Pokud by měl povolit hello port bez SSL pro připojení tooRedis?
Redis serveru nativně nepodporuje protokol SSL, ale v Azure Redis Cache. Pokud se připojujete tooAzure Redis Cache a váš klient podporuje protokol SSL, jako je StackExchange.Redis, musí používat protokol SSL.

>[!NOTE]
>port bez SSL Hello vypnutá ve výchozím nastavení pro nové instance služby Azure Redis Cache. Pokud váš klient nepodporuje protokol SSL, pak je nutné povolit port bez SSL hello podle následujících pokynů hello v hello [přístup k portům](cache-configure.md#access-ports) části hello [konfigurace mezipaměti ve službě Azure Redis Cache](cache-configure.md) článku.
>
>

Nástroje, jako redis `redis-cli` nefungují s hello SSL port, ale můžete pomocí nástroje, jako `stunnel` toosecurely připojit port SSL toohello nástroje hello podle následujících pokynů hello v hello [uvedení poskytovatele stavu relace ASP.NET Redis verzi Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) příspěvku na blogu.

Pokyny ke stahování hello Redis nástroje, najdete v části hello [jak můžete spouštět příkazy Redis?](#cache-commands) části.

### <a name="what-are-some-production-best-practices"></a>Jaké jsou některé z osvědčených postupů produkční?
* [Osvědčené postupy StackExchange.Redis](#stackexchangeredis-best-practices)
* [Konfigurace a koncepty](#configuration-and-concepts)
* [Testování výkonu](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>Osvědčené postupy StackExchange.Redis
* Nastavit `AbortConnect` toofalse, nechte hello ConnectionMultiplexer automaticky se znovu nepřipojí. [Podrobnosti najdete v](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* Znovu použít hello ConnectionMultiplexer – nevytvářejte novou pro každý požadavek. Hello `Lazy<ConnectionMultiplexer>` vzor [tady uvedené](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) se doporučuje.
* Redis funguje nejlépe s menší hodnoty, takže zvažte dělení, jsou větší data do více klíčů. V [toto pojednání Redis](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb se považuje za velké. Čtení [v tomto článku](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) pro problém s příklad, který může být způsobeno velké hodnoty.
* Konfigurace vaší [nastavení fondu podprocesů](#important-details-about-threadpool-growth) tooavoid vypršení časových limitů.
* Použití alespoň hello výchozí connectTimeout 5 sekund. Tento interval, získáte dostatek času toore StackExchange.Redis-připojení hello, v případě blip sítě.
* Uvědomte si, hello výkonu náklady spojené s různé operace, které používáte. Například hello `KEYS` příkaz je operace O(n) a je nutno. Hello [redis.io lokality](http://redis.io/commands/) obsahuje podrobnosti kolem hello složitost čas pro každou operaci, kterou podporuje. Klikněte na každý příkaz toosee hello složitost pro každou operaci.

#### <a name="configuration-and-concepts"></a>Konfigurace a koncepty
* Použijte Standard nebo Premium vrstvy pro produkční systémy. Hello úroveň Basic je systém jednoho uzlu se žádná data replikace a žádné SLA. Rovněž použijte alespoň C1 mezipaměti. C0 mezipamětí jsou obvykle používány pro scénáře vývoje/testování jednoduché.
* Mějte na paměti, že je Redis **v paměti** úložišti. Čtení [v tomto článku](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) tak, že jste si vědomi scénáře, kde může dojít ke ztrátě dat..
* Vývoj systému tak, aby ji může zpracovat připojení blips [kvůli toopatching a převzetí služeb při selhání](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Testování výkonu
* Začněte tím, že pomocí `redis-benchmark.exe` tooget chování pro možné propustnost před zápisem testy výkonu. Protože `redis-benchmark` nepodporuje protokol SSL, musíte [povolení portu bez SSL hello prostřednictvím portálu Azure hello](cache-configure.md#access-ports) předtím, než spustíte hello test. Příklady najdete v tématu [jak mohu srovnávací test a otestovat hello výkonu Moje mezipaměti?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* Hello klienta virtuálního počítače používat pro testování by měla být v hello stejné oblasti jako instance mezipaměti Redis.
* Doporučujeme používat pro vašeho klienta, dokud budou mít lepší hardwaru a získat hello nejlepších výsledků dosáhnete pomocí řady Dv2 virtuálních počítačů.
* Zkontrolujte, zda váš klient zvolíte virtuální počítač má alespoň tolik schopnosti výpočetních a šířky pásma jako hello mezipaměti, který testujete.
* Povolte VRSS na klientský počítač hello, pokud jste v systému Windows. [Podrobnosti najdete v](https://technet.microsoft.com/library/dn383582.aspx).
* Úroveň Premium Redis instance mít lepší sítě latence a propustnosti vzhledem k tomu, že jsou spuštěny v lepší hardwaru pro procesoru a sítě.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-hello-considerations-when-using-common-redis-commands"></a>Jaké jsou některé aspekty hello při pomocí běžných příkazů Redis?
* Nespouštějte určité Redis příkazy, které trvat toocomplete dlouhou dobu, neměňte hello dopad těchto příkazů.
  * Například se nespustí hello [klíče](http://redis.io/commands/keys) příkaz v produkčním prostředí, protože to může trvat dlouhou dobu tooreturn v závislosti na hello počet klíčů. Redis je server jednovláknové a zpracovává příkazy jeden najednou. Pokud máte další příkazy vydané po klíče, nebudou zpracovány až Redis zpracuje příkaz hello klíče. Hello [redis.io lokality](http://redis.io/commands/) obsahuje podrobnosti kolem hello složitost čas pro každou operaci, kterou podporuje. Klikněte na každý příkaz toosee hello složitost pro každou operaci.
* Velikosti klíče - použít malé klíč/hodnota nebo velké klíč/hodnota? Obecně platí závisí na scénáři hello. Pokud váš scénář vyžaduje větší klíčů, můžete upravit hello ConnectionTimeout a opakujte hodnot a upravit logika opakovaných pokusů. Z hlediska serveru Redis menší hodnoty jsou dodržovány toohave lepší výkon.
* Tyto aspekty nemáte znamenat vyšší hodnoty nelze uložit do Redis; musíte být vědomi hello následující aspekty. Latence bude vyšší. Pokud máte jednu sadu dat, která je větší a ten, který je menší, můžete použít několik instancí ConnectionMultiplexer, každý nakonfigurován s jinou sadu hodnot časového limitu a zkuste to znovu, jak je popsáno v předchozí hello [co hello Možnosti konfigurace StackExchange.Redis provést](#cache-configuration) části.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-hello-performance-of-my-cache"></a>Jak mohu srovnávací test a otestovat hello výkonu Moje mezipaměti?
* [Povolte diagnostiku mezipaměti](cache-how-to-monitor.md#enable-cache-diagnostics) , abyste mohli [monitorování](cache-how-to-monitor.md) hello stav svojí mezipaměti. Můžete zobrazit hello metriky ve hello portál Azure a může taky [stáhnout a revidovat](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) je pomocí hello nástrojů dle vašeho výběru.
* Můžete vytvořit testovací tooload redis benchmark.exe serveru Redis.
* Zajistěte, aby byly v hello hello zatížení testování klienta a mezipaměť Redis hello stejné oblasti.
* Použijte redis cli.exe a monitorovat hello mezipaměti pomocí hello informace o příkazu.
* Pokud zatížení způsobuje fragmentace velkého množství paměti, měli škálovat tooa větší velikost mezipaměti.
* Pokyny ke stahování hello Redis nástroje, najdete v části hello [jak můžete spouštět příkazy Redis?](#cache-commands) části.

Hello následující příkazy uveďte příklad použití redis benchmark.exe. Pro přesné výsledky, tyto příkazy spustit z virtuálního počítače v hello stejné oblasti jako vaše mezipaměť.

* Požadavky testovacího zřetězena SET pomocí datovou část 1 kB

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test zřetězena získat požadavků pomocí datovou část 1 kB.
  Poznámka: Spuštění testu sady hello uvedené výše první toopopulate mezipaměti

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>Důležité podrobnosti o fondu růst
Hello zachovalo CLR má dva typy vláken - "Pracovník" a "Portu dokončení vstupně-výstupních operací" (neboli portu IOCP) vláken.

* Pracovní vlákna se používají při pro takové věci, jako zpracování `Task.Run(…)` nebo `ThreadPool.QueueUserWorkItem(…)` metody. Tyto vláken používají různé součásti v hello CLR taky, když pracovní potřebuje toohappen na vlákna na pozadí.
* Vlákna portu IOCP se používají při asynchronní vstupně-výstupní operace se stane (např. čtení z hello sítě).

fondu vláken Hello poskytuje nové pracovních vláken nebo vláken dokončení vstupně-výstupních operací na vyžádání (bez žádné omezení), dokud nedosáhne hello "Minimální" nastavení pro každý typ přístup z více vláken. Ve výchozím nastavení je minimální počet vláken hello hodnotu toohello počet procesorů v systému.

Jednou hello počet existující (zaneprázdněn) vláken přístupů hello "minimální" počet vláken, hello fondu budou omezení hello rychlost, jakou se vloží nové vlákno tooone vláken na 500 milisekund. Obvykle se váš systém získá shluku práce portu IOCP přístup z více vláken, která potřebuje, zpracována bude tento pracovní velmi rychle. Ale pokud hello shluků práce je větší než hello nakonfigurovanou nastavení "Minimální", budou existovat některé zpoždění při zpracování některých pracovních hello jako hello zachovalo čeká na jednu z toohappen dvě věci.

1. Existující vlákno stane volné tooprocess hello práci.
2. Žádné existující vlákno neuvolní pro 500ms, takže se vytvoří nové vlákno.

V zásadě platí znamená to, že když hello počet vytížených vláken je vyšší než minimální vláken, pravděpodobně platíte 500ms zpoždění dříve, než je pomocí aplikace hello síťový provoz. Také je důležité toonote, že pokud stávající vlákno zůstává nečinné po dobu delší než 15 sekund (podle I zapamatovat), se vyčistí a tento cyklus růstu a redukce, můžete opakovat.

Pokud se podíváme na příklad chybovou zprávu z StackExchange.Redis (sestavení 1.0.450 nebo novější), zobrazí se, že nyní vytiskne statistiky fondu (viz níže portu IOCP a pracovní podrobnosti).

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

V předchozím příkladu hello můžete zobrazit, pro přístup z více vláken portu IOCP jsou 6 zaneprázdněn vláken a systém hello je minimální počet vláken činí nakonfigurované tooallow 4. V takovém případě hello klienta by pravděpodobně viděli jste dva zpoždění 500 ms protože 6 > 4.

Všimněte si, že můžete StackExchange.Redis Pokud získá omezuje růst portu IOCP nebo pracovních vláken dosáhl vypršení časových limitů.

### <a name="recommendation"></a>Doporučení
Zvážení všech informací, důrazně doporučujeme, aby zákazníci nastavit hodnotu minimální konfigurace hello pro portu IOCP a pracovních vláken toosomething větší než hello výchozí hodnota. Jsme nelze udělit univerzálně pokyny k co tato hodnota by měla být protože hello správné hodnoty pro jednu aplikaci bude mít příliš vysokou a nízkým pro jinou aplikaci. Tato nastavení mohou ovlivnit výkon hello dalších částí složitých aplikací, takže každý zákazník musí toofine tune, musí toto nastavení tootheir konkrétní. Je dobré počáteční se 200 nebo 300, pak otestovat a upravit podle potřeby.

Jak tooconfigure toto nastavení:

* V technologii ASP.NET, použijte hello [nastavení konfigurace "minIoThreads"] [ "minIoThreads" configuration setting] pod hello `<processModel>` konfiguračního prvku v souboru web.config. Pokud používáte uvnitř weby Azure, toto nastavení nebude vystavena, prostřednictvím možností konfigurace hello. Však přesto doporučuje se mít tooconfigure toto nastavení prostřednictvím kódu programu (viz níže) z metodu Application_Start v global.asax.cs.

  > [!NOTE] 
  > Hello hodnota zadaná v tomto elementu konfigurace je *za jádra* nastavení. Například pokud máte 4jádrový počítač a chcete vaší minIOThreads nastavení toobe 200 za běhu, byste použili `<processModel minIoThreads="50"/>`.
  >

* Mimo prostředí ASP.NET, použijte hello [ThreadPool.SetMinThreads(...) ](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) Rozhraní API.

<a name="server-gc"></a>

### <a name="enable-server-gc-tooget-more-throughput-on-hello-client-when-using-stackexchangeredis"></a>Povolit serveru globálního katalogu tooget další propustnosti na klientovi hello, pokud používáte StackExchange.Redis
Povolení serveru globálního katalogu, můžete optimalizovat hello klienta a poskytují lepší výkon a propustnost při používání StackExchange.Redis. Další informace o serveru globálního katalogu a jak tooenable, najdete v části hello následující články:

* [tooenable server globálního katalogu](https://msdn.microsoft.com/library/ms229357.aspx)
* [Základy kolekce paměti](https://msdn.microsoft.com/library/ee787088.aspx)
* [Uvolňování paměti a výkon](https://msdn.microsoft.com/library/ee851764.aspx)


### <a name="performance-considerations-around-connections"></a>Faktory ovlivňující výkon kolem připojení

Každá cenová úroveň má jiné limity pro připojení klientů, paměti a šířky pásma. Při každém velikost mezipaměti umožňuje *až* určitý počet připojení, každý tooRedis připojení má režie spojená s ním spojená. Příkladem takových režie může být využití procesoru a paměti v důsledku šifrování pomocí protokolu TLS/SSL. Hello připojení maximální limit pro velikost daného mezipaměti předpokládá lehkým načíst mezipaměti. Pokud načíst z režijní náklady na připojení *plus* zatížení z klientské operace překročí kapacitu pro systém hello, hello mezipaměti můžete mít problémy kapacity, i pokud nebyl překročen limit pro aktuální velikost mezipaměti hello hello připojení.

Další informace o omezeních hello různá připojení pro každou vrstvu najdete v tématu [cenách Azure Redis Cache](https://azure.microsoft.com/pricing/details/cache/). Další informace o připojení a další výchozí konfigurace najdete v tématu [Redis výchozí konfigurace serveru](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-hello-health-and-performance-of-my-cache"></a>Jak monitorování hello stav a výkon Moje mezipaměti?
Instance služby Microsoft Azure Redis Cache lze sledovat v hello [portál Azure](https://portal.azure.com). Můžete zobrazit metriky, připnout metriky grafy toohello úvodní panel, přizpůsobení hello datum a čas rozsah monitorování grafy, přidat a metriky odebrání hello grafy a nastavit upozornění, pokud jsou splněny určité podmínky. Další informace najdete v tématu [monitorování Azure Redis Cache](cache-how-to-monitor.md).

Hello Redis Cache **prostředků nabídky** také obsahuje několik nástrojů pro monitorování a řešení potíží s své mezipaměti.

* **Diagnostika a řešení problémů** poskytuje informace o běžných problémech a strategie pro jejich řešení.
* **Stav prostředku** sleduje prostředku a zjistíte, zda je spuštěn podle očekávání. Další informace o hello služby stavu prostředků Azure najdete v tématu [přehled stavu prostředků Azure](../resource-health/resource-health-overview.md).
* **Nová žádost o podporu** poskytuje možnosti tooopen žádosti o podporu pro mezipaměť.

Tyto nástroje povolte jste toomonitor hello stav vaší instance služby Azure Redis Cache a vám pomohou při správě aplikace ukládání do mezipaměti. Další informace najdete v části hello "Podpora & odstraňování problémů s nastavením" s [jak tooconfigure Azure mezipaměti Redis](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Proč se zobrazuje časové limity?
Časové limity dojde v hello klienta použít tootalk tooRedis. Příkaz odeslán toohello serveru Redis, příkaz hello je zařazen do fronty a nakonec převezme hello příkaz serveru Redis a spustí jej. Ale hello klienta můžete vypršení časového limitu během tohoto procesu a pokud ano výjimku se vyvolá při volání metody straně hello. Další informace o řešení potíží s vypršení časového limitu, najdete v části [řešení potíží s klientskou](cache-how-to-troubleshoot.md#client-side-troubleshooting) a [výjimkám časového limitu StackExchange.Redis](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-hello-cache"></a>Proč byl klient odpojen od hello mezipaměti?
Hello Toto jsou některé běžné důvod pro odpojení mezipaměti.

* Způsobí, že na straně klienta
  * klientská aplikace Hello byl znovu nasazena.
  * klientská aplikace Hello provedla operaci škálování.
    * V případě hello cloudových služeb nebo webových aplikací Důvodem mohou být tooauto škálování.
  * Hello síťovou vrstvou na straně klienta hello změnit.
  * V klientovi hello nebo hello síťových uzlů mezi hello klient a hello server došlo k přechodné chyby.
  * bylo dosaženo Hello šířky pásma prahové limity.
  * Procesor vázaný operace trvalo příliš dlouho toocomplete.
* Způsobí, že na straně serveru
  * Hello služby Azure Redis Cache na hello standardní nabídku služby cache, inicializovat převzetí služeb při selhání sekundárního uzlu toohello hello primárního uzlu.
  * Azure byla opravy hello instance, kde byla nasazena hello mezipaměti
    * To může být pro aktualizace serveru Redis nebo Obecná údržba virtuálního počítače.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Kterou z variant Mezipaměti Azure si mám vybrat?
> [!IMPORTANT]
> Podle poslední rok [oznámení](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), služby Azure spravované mezipaměti Service a Azure v roli Cache **mít bylo vyřazeno z provozu** 30. listopadu 2016. Naše doporučení je toouse [Azure Redis Cache](https://azure.microsoft.com/services/cache/). Informace o migraci naleznete v tématu [migrací z tooAzure spravované služby mezipaměti Redis Cache](cache-migrate-to-redis.md).
>
>

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis Cache je všeobecně dostupná ve velikosti až too53 GB a má SLA 99,9 % dostupnosti. Hello nové [úroveň premium](cache-premium-tier-intro.md) nabízí velikosti too530 GB a podpora pro clustering, virtuální síť a trvalost s SLA 99,9 %.

Azure Redis Cache poskytuje zákazníkům hello možnost toouse zabezpečené, vyhrazené mezipaměti Redis spravované microsoftem. Pomocí této nabídky získáte tooleverage hello bohaté sadě funkcí a ekosystém poskytované Redis a spolehlivého hostování a monitorování od Microsoftu.

Na rozdíl od tradičních mezipaměti, která se týkají jenom s páry klíč hodnota je pro jeho vysokou původce datové typy Redis. Redis také podporuje běh atomické operací pro tyto typy, jako je připojení tooa řetězec; přírůstkovou hodnotou hello v hodnotu hash; vkládání tooa seznamu; výpočetní průnik sady, union a rozdíl; nebo člen získávání hello s nejvyšším pořadím v seřazené sady. Další funkce zahrnují podporu pro transakce, pub nebo sub, Lua skriptování, klíče s omezenou time to live a konfigurace nastavení toomake Redis více chovat jako tradiční mezipaměti.

Jiné úspěch tooRedis klíčovým prvkem je hello s otevřeným zdrojem v pořádku, živoucí ekosystém vytvořené kolem něj. To se projeví v hello různého typu klientů Redis k dispozici v několika jazycích. Tento ekosystém a širokou škálu klientům povolit Azure Redis Cache toobe používané skoro všechny úlohy, jež by sestavení v rámci Azure.

Další informace o seznámení se s Azure Redis Cache najdete v tématu [jak tooUse Azure mezipaměti Redis](cache-dotnet-how-to-use-azure-redis-cache.md) a [dokumentace k Azure Redis Cache](index.md).

### <a name="managed-cache-service"></a>Spravované služby mezipaměti
[Spravované mezipaměti služby vyřazenou 30. listopadu 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

tooview archivovat dokumentace, najdete v části [archivovaný spravované dokumentaci mezipaměti služby](https://msdn.microsoft.com/library/azure/dn386094.aspx).

### <a name="in-role-cache"></a>Mezipaměť hostovaná v instanci role
[Mezipaměť hostovaná v instanci Role vyřazenou 30. listopadu 2016.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

tooview archivovat dokumentace, najdete v části [archivovaný dokumentace mezipaměť hostovaná v instanci Role](https://msdn.microsoft.com/library/azure/dn386103.aspx).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
