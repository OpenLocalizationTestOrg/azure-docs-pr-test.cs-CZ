---
title: aaaHow tooconfigure Azure Redis Cache | Microsoft Docs
description: "Rady pro pochopení hello výchozí konfiguraci Redis pro Azure Redis Cache a zjistěte, jak tooconfigure vaší služby Azure Redis instancích mezipaměti"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: sdanie
ms.openlocfilehash: 46bffb74cdf40e0e0a99c3a83dbe06d6fe1ea65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-azure-redis-cache"></a>Jak tooconfigure Azure mezipaměti Redis
Toto téma popisuje, jak tooreview a aktualizace hello konfigurace pro vaše instance služby Azure Redis Cache a konfigurace serveru zahrnuje hello výchozí Redis pro instance služby Azure Redis Cache.

> [!NOTE]
> Další informace o konfiguraci a použití prémiových funkcí mezipaměti najdete v tématu [jak trvalost tooconfigure](cache-how-to-premium-persistence.md), [jak tooconfigure clustering](cache-how-to-premium-clustering.md), a [jak podporují tooconfigure virtuální sítě ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Konfigurace nastavení mezipaměti Redis
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Nastavení Azure Redis Cache jsou zobrazit a konfigurovat na hello **Redis Cache** okno pomocí hello **prostředků nabídky**.

![Nastavení mezipaměti redis](./media/cache-configure/redis-cache-settings.png)

Můžete zobrazit a konfigurovat následující nastavení pomocí hello hello **prostředků nabídky**.

* [Přehled](#overview)
* [Protokol aktivit](#activity-log)
* [Řízení přístupu (IAM)](#access-control-iam)
* [Značky](#tags)
* [Diagnóza a řešení problémů](#diagnose-and-solve-problems)
* [Nastavení](#settings)
    * [Přístupové klávesy](#access-keys)
    * [Upřesnit nastavení](#advanced-settings)
    * [Advisor mezipaměti redis](#redis-cache-advisor)
    * [Škálování](#scale)
    * [Velikost clusteru redis](#cluster-size)
    * [Trvalosti dat redis](#redis-data-persistence)
    * [Plán aktualizací](#schedule-updates)
    * [Geografická replikace](#geo-replication)
    * [Virtual Network](#virtual-network)
    * [Brána firewall](#firewall)
    * [Vlastnosti](#properties)
    * [Zámky.](#locks)
    * [Skriptu pro automatizaci](#automation-script)
* [Správa](#administration)
    * [Umožňuje importovat data](#importexport)
    * [Export dat](#importexport)
    * [Restartování](#reboot)
* [Monitorování](#monitoring)
    * [Metriky pro redis](#redis-metrics)
    * [Pravidla výstrah](#alert-rules)
    * [Diagnostika](#diagnostics)
* [Podporovat & řešení potíží s nastavení](#support-amp-troubleshooting-settings)
    * [Stav prostředků](#resource-health)
    * [Nová žádost o podporu](#new-support-request)


## <a name="overview"></a>Přehled

**Přehled** poskytuje k základní informace o mezipaměti, například název, porty, cenová úroveň a metriky vybrané mezipaměti.

### <a name="activity-log"></a>Protokol aktivit

Klikněte na tlačítko **protokol aktivit** tooview akce provádět v mezipaměti. Můžete také použít filtrování tooexpand tato zobrazení tooinclude jiné prostředky. Další informace o práci s protokoly auditu najdete v tématu [auditovat operace s Resource Managerem](../azure-resource-manager/resource-group-audit.md). Další informace o sledování událostí Azure Redis Cache najdete v tématu [operace a výstrahy](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Řízení přístupu (IAM)

Hello **přístup k ovládacímu prvku (IAM)** části poskytuje podporu pro řízení přístupu na základě role (RBAC) v hello Azure portálu toohelp organizace splňují požadavky na správu jejich přístup jednoduše a přesně. Další informace najdete v tématu [řízení přístupu na základě Role v portálu Azure hello](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Značky

Hello **značky** část umožňuje uspořádání prostředků. Další informace najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Diagnostikovat a řešit problémy

Klikněte na tlačítko **Diagnostikujte a řešení problémů** toobe součástí běžné problémy a strategie pro jejich řešení.



## <a name="settings"></a>Nastavení
Hello **nastavení** části vám umožní tooaccess a nakonfigurujte následující nastavení pro mezipaměť hello.

* [Přístupové klávesy](#access-keys)
* [Upřesnit nastavení](#advanced-settings)
* [Advisor mezipaměti redis](#redis-cache-advisor)
* [Škálování](#scale)
* [Velikost clusteru redis](#cluster-size)
* [Trvalosti dat redis](#redis-data-persistence)
* [Plán aktualizací](#schedule-updates)
* [Geografická replikace](#geo-replication)
* [Virtual Network](#virtual-network)
* [Brána firewall](#firewall)
* [Vlastnosti](#properties)
* [Zámky.](#locks)
* [Skriptu pro automatizaci](#automation-script)



### <a name="access-keys"></a>Přístupové klíče
Klikněte na tlačítko **přístupové klíče** tooview nebo znovu vygenerovat hello přístupových klíčů pro mezipaměť. Tyto klíče jsou používány hello klienti připojení tooyour mezipaměti.

![Přístupové klíče mezipaměti redis](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Upřesnit nastavení
Hello se konfiguruje následující nastavení na hello **upřesňující nastavení** okno.

* [Přístupové porty](#access-ports)
* [Zásady paměti](#memory-policies)
* [Oznámení keyspace (rozšířené nastavení)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Přístupové porty
Přístup bez SSL je ve výchozím nastavení pro nové mezipaměti zakázaný. tooenable hello bez SSL port, klikněte na tlačítko **ne** pro **povolit přístup jen přes SSL** na hello **upřesňující nastavení** okna a pak klikněte na tlačítko **Uložit**.

![Přístupové porty mezipaměti redis](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Zásady paměti
Hello **Maxmemory zásad**, **vyhrazené maxmemory**, a **vyhrazené maxfragmentationmemory** nastavení na hello **upřesňující nastavení**okno nakonfigurovat zásady hello paměť pro mezipaměť hello.

![Zásady Maxmemory mezipaměti redis](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Zásady Maxmemory** nakonfiguruje zásady vyřazení hello hello mezipaměti a umožňuje vám toochoose z hello následující zásady vyřazení:

* `volatile-lru`-Toto je výchozí hello.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Další informace o `maxmemory` zásady, najdete v části [zásady vyřazení](http://redis.io/topics/lru-cache#eviction-policies).

Hello **maxmemory vyhrazené** nastavení konfiguruje hello množství paměti v MB, která je vyhrazena pro operace bez mezipaměti, jako je například replikace během převzetí služeb při selhání. Nastavení této hodnoty můžete toohave více konzistentního prostředí serveru Redis při zatížení se liší. Tato hodnota je třeba nastavit vyšší pro úlohy, které jsou zapsat náročné. Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.

Hello **maxfragmentationmemory vyhrazené** nastavení konfiguruje hello množství paměti v MB, který je vyhrazený tooaccommodate pro fragmentace paměti. Nastavení této hodnoty můžete toohave více konzistentního prostředí serveru Redis při je hello mezipaměť plná nebo zavřít toofull a hello fragmentace poměr je také vysokou. Když paměti je vyhrazena pro takové operace, není k dispozici pro úložiště dat uložených v mezipaměti.

Jednou z věcí tooconsider při výběru novou hodnotu rezervace paměti (**vyhrazené maxmemory** nebo **vyhrazené maxfragmentationmemory**) je, jak tato změna může ovlivnit mezipaměti, která je již spuštěn s velké objemy dat v něm. Například pokud mají mezipaměť 53 GB s 49 GB dat, pak změňte hello rezervace hodnota too8 GB, bude vyřadit hello maximální dostupné paměti pro systém hello dolů too45 GB. Pokud buď vaše aktuální `used_memory` , vaše `used_memory_rss` hodnoty jsou vyšší, než nový limit hello 45 GB a pak hello systému budou mít tooevict data dokud `used_memory` a `used_memory_rss` jsou pod 45 GB. Vyřazení může zvýšit fragmentace zatížení a paměti serveru. Další informace o metrikách mezipaměti jako `used_memory` a `used_memory_rss`, najdete v části [k dostupným metrikám a vytváření sestav intervaly](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> Hello **vyhrazené maxmemory** a **maxfragmentationmemory vyhrazené** nastavení jsou dostupná jenom pro Standard a Premium ukládá do mezipaměti.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Oznámení keyspace (rozšířené nastavení)
Redis keyspace oznámení jsou nakonfigurované na hello **upřesňující nastavení** okno. Oznámení keyspace umožňují klienti tooreceive oznámení při určité události.

![Upřesňující nastavení mezipaměti redis](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspace oznámení a hello **oznámení události keyspace** nastavení jsou dostupná jenom pro Cache úrovní Standard a Premium.
> 
> 

Další informace najdete v tématu [Redis oznámení Keyspace](http://redis.io/topics/notifications). Ukázkový kód, najdete v části hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) souboru v hello [Hello, world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) ukázka.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Advisor mezipaměti redis
Hello **Redis Cache Advisor** zobrazuje doporučení pro mezipaměť. Během normálních operací zobrazí se žádná doporučení. 

![Doporučení](./media/cache-configure/redis-cache-no-recommendations.png)

Pokud jakékoliv podmínky, ke kterým došlo během operace hello svojí mezipaměti, jako je například velké množství paměti, šířky pásma sítě nebo zatížení serveru, zobrazí se výstraha na hello **Redis Cache** okno.

![Doporučení](./media/cache-configure/redis-cache-recommendations-alert.png)

Další informace naleznete na hello **doporučení** okno.

![Doporučení](./media/cache-configure/redis-cache-recommendations.png)

Můžete sledovat tyto metriky na hello [tabulek sledování](cache-how-to-monitor.md#monitoring-charts) a [využití grafy](cache-how-to-monitor.md#usage-charts) části hello **Redis Cache** okno.

Každá cenová úroveň má jiné limity pro připojení klientů, paměti a šířky pásma. Pokud vaše mezipaměť blíží maximální kapacity pro tyto metriky po dobu delší dobu, vytvoří se doporučení. Další informace o hello metriky a omezení zkontrolovány uživatelem hello **doporučení** nástroje najdete v tématu hello následující tabulka:

| Metrika mezipaměti redis | Další informace |
| --- | --- |
| Využití šířky pásma sítě |[Výkon mezipaměti - dostupnou šířku pásma](cache-faq.md#cache-performance) |
| Připojení klienti |[Výchozí konfigurace serveru Redis - maxclients](#maxclients) |
| Zatížení serveru |[Použití grafů – zatížení serveru Redis](cache-how-to-monitor.md#usage-charts) |
| Využití paměti |[Výkon mezipaměti - velikost](cache-faq.md#cache-performance) |

tooupgrade mezipaměti, klikněte na tlačítko **upgradovat nyní** toochange hello cenová úroveň a [škálování](#scale) svojí mezipaměti. Další informace o výběru cenovou úroveň, najdete v části [jaké mezipaměť Redis nabídky a velikosti použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Měřítko
Klikněte na tlačítko **škálování** tooview nebo změňte hello cenovou úroveň pro mezipaměť. Další informace o škálování najdete v tématu [jak tooScale Azure mezipaměti Redis](cache-how-to-scale.md).

![Cenová úroveň mezipaměti redis](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Velikost clusteru redis
Klikněte na tlačítko **velikost clusteru Redis (PREVIEW)** toochange hello velikost clusteru pro spuštěné premium mezipaměti s povoleným clusteringem.

> [!NOTE]
> Všimněte si, že při hello Azure Redis Cache Premium vrstvy byl vydané tooGeneral dostupnosti, hello Redis velikost clusteru funkce je aktuálně ve verzi preview.
> 
> 

![Velikost clusteru redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

velikost clusteru hello toochange, použijte hello posuvníku nebo zadejte číslo mezi 1 a 10 hello **počet horizontálních** textového pole a klikněte na tlačítko **OK** toosave.

> [!IMPORTANT]
> Redis clustering je dostupná jenom pro prémiových mezipamětí. Další informace najdete v tématu [jak tooconfigure clusteringu pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Trvalosti dat redis
Klikněte na tlačítko **trvalosti dat Redis** tooenable, zakázat nebo konfigurace trvalosti dat pro mezipaměť premium. Azure Redis Cache nabízí buď pomocí trvalosti Redis [RDB trvalost](cache-how-to-premium-persistence.md#configure-rdb-persistence) nebo [AOF trvalost](cache-how-to-premium-persistence.md#configure-aof-persistence).

Další informace najdete v tématu [jak tooconfigure trvalosti pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Trvalosti dat redis je dostupná jenom pro prémiových mezipamětí. 
> 
> 

### <a name="schedule-updates"></a>Aktualizace plánu
Hello **naplánovat aktualizace** okno vám umožní toodesignate údržby pro aktualizace serveru Redis ke svojí mezipaměti. 

> [!IMPORTANT]
> Hello časové období údržby platí pouze tooRedis serveru aktualizace a ne tooany Azure aktualizace nebo aktualizace operačního systému toohello hello virtuálních počítačů, které jsou hostiteli hello mezipaměti.
> 
> 

![Aktualizace plánu](./media/cache-configure/redis-schedule-updates.png)

toospecify údržby, zkontrolujte hello potřeby dnů hodina spouštění údržby okno hello za každý den, zadejte a klikněte na tlačítko **OK**. Všimněte si, že hello časového období údržby se ve standardu UTC. 

> [!IMPORTANT]
> Hello **naplánovat aktualizace** funkce je dostupná jenom pro prémiových mezipamětí vrstvy. Další informace a pokyny najdete v tématu [správy Azure Redis Cache - naplánovat aktualizace](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Geografická replikace

Hello **geografická replikace** okno poskytuje mechanismus pro propojení dvě instance vrstvy Azure Redis Cache Premium. Jeden mezipaměti je určený jako primární propojené mezipaměti hello a hello jiných jako sekundární propojené mezipaměti hello. Hello sekundární propojené mezipaměti se stane, jen pro čtení a data napsané toohello primární mezipaměť je replikují toohello sekundární propojené mezipaměti. Tuto funkci lze použít tooreplicate mezipaměti nad oblastmi Azure.

> [!IMPORTANT]
> **Geografická replikace** je dostupná jenom u prémiových mezipamětí vrstvy. Další informace a pokyny najdete v tématu [jak tooconfigure geografická replikace pro Azure Redis Cache](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Virtual Network
Hello **virtuální sítě** část vám tooconfigure hello nastavení virtuální sítě pro mezipaměť. Informace týkající se vytváření cache ve verzi premium se virtuální síť podporovat a aktualizuje jeho nastavení, najdete v tématu [jak tooconfigure podpora virtuální sítě pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Nastavení virtuální sítě jsou dostupné pouze pro prémiových mezipamětí, které byly nakonfigurovány s podporou virtuální sítě během vytváření mezipaměti. 
> 
> 

### <a name="firewall"></a>Brána firewall

Klikněte na tlačítko **brány Firewall** tooview a konfigurace pravidel brány firewall pro vaše mezipaměť Azure Redis Cache Premium.

![Brána firewall](./media/cache-configure/redis-firewall-rules.png)

Pravidla brány firewall můžete zadat s počáteční a koncové rozsah IP adres. Pokud jsou nakonfigurovaná pravidla brány firewall, připojení klienta pouze z hello zadané rozsahy IP adres můžete připojit toohello mezipaměti. Při uložení pravidlo brány firewall není chvíli trvat, než pravidla hello je platná. Toto opoždění je obvykle méně než jedna minuta.

> [!IMPORTANT]
> Připojení z Azure Redis Cache monitorování systémů jsou vždy povoleny, i když jsou nakonfigurovaná pravidla brány firewall.
> 
> Pravidla brány firewall jsou dostupná jenom pro prémiových mezipamětí vrstvy.
> 
> 

### <a name="properties"></a>Vlastnosti
Klikněte na tlačítko **vlastnosti** tooview informace o mezipaměti, včetně hello koncový bod mezipaměti a portů.

![Vlastnosti mezipaměti redis](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Zámky
Hello **zamkne** část vám toolock předplatné, skupinu prostředků nebo prostředek tooprevent jiných uživatelů ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků. Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Skriptu pro automatizaci

Klikněte na tlačítko **skriptu pro automatizaci** toobuild a exportovat šablonu vaše nasazené prostředky pro budoucí nasazení. Další informace o práci se šablonami najdete v tématu [nasazení prostředků pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Nastavení správy
Hello nastavení v hello **správy** části umožňují tooperform hello následující úlohy správy pro mezipaměť. 

![Správa](./media/cache-configure/redis-cache-administration.png)

* [Umožňuje importovat data](#importexport)
* [Export dat](#importexport)
* [Restartování](#reboot)


### <a name="importexport"></a>Import/export
Import a Export je operace správy Azure Redis Cache data, která vám umožní tooimport a export dat v mezipaměti hello import a Export snímku databáze Redis Cache (RDB) ze stránky premium mezipaměti tooa blob v účtu úložiště Azure. Import a Export vám umožní toomigrate mezi různými instancemi Azure Redis Cache nebo naplňte hello mezipaměť s daty před použitím.

Import lze použít toobring Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších. Import dat se snadný způsob toocreate mezipaměti s předem vyplněná daty. Během procesu importu hello Azure Redis Cache načte hello RDB soubory z úložiště Azure do paměti a pak vloží hello klíčů do mezipaměti hello.

Export umožňuje tooexport hello data uložená v Azure Redis Cache tooRedis kompatibilní RDB soubory. Můžete použít tuto funkci toomove data tooanother instanci Azure Redis Cache či tooanother serveru Redis. Během procesu exportu hello dočasný soubor vytvořen na hello virtuálních počítačů, hostitelů hello instanci serveru Azure Redis Cache a soubor hello je nahrané toohello určený účet úložiště. Po dokončení operace exportu hello se stavem úspěch nebo selhání hello dočasný soubor bude odstraněn.

> [!IMPORTANT]
> Import a Export je dostupná jenom pro prémiových mezipamětí vrstvy. Další informace a pokyny najdete v tématu [importu a exportu dat ve službě Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Restartování
Hello **restartovat** okno vám umožní tooreboot hello uzly svojí mezipaměti. Tato možnost restartování umožňuje vám tootest Pokud dojde k selhání uzlu mezipaměti aplikace pro odolnost proti chybám.

![Restartování](./media/cache-configure/redis-cache-reboot.png)

Pokud máte s povoleným clusteringem cache ve verzi premium, můžete vybrat které horizontálních tooreboot mezipaměti hello.

![Restartování](./media/cache-configure/redis-cache-reboot-cluster.png)

tooreboot jeden nebo více uzlů vaší mezipaměti, vyberte uzel hello potřeby a klikněte na tlačítko **restartovat**. Pokud máte cache ve verzi premium s povoleným clusteringem, vyberte hello shard(s) tooreboot a pak klikněte na **restartovat**. Po několika minutách hello restartování vybrané uzly a jsou zpět do režimu online několik minut později.

> [!IMPORTANT]
> Restart je nyní k dispozici pro všechny cenové úrovně. Další informace a pokyny najdete v tématu [správy Azure Redis Cache - restartování](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Monitorování

Hello **monitorování** část vám tooconfigure diagnostiky a monitorování pro mezipaměť Redis Cache. Další informace o Azure Redis Cache monitorování a Diagnostika, najdete v části [jak toomonitor Azure mezipaměti Redis](cache-how-to-monitor.md).

![Diagnostika](./media/cache-configure/redis-cache-diagnostics.png)

* [Metriky pro redis](#redis-metrics)
* [Pravidla výstrah](#alert-rules)
* [Diagnostika](#diagnostics)

### <a name="redis-metrics"></a>Metriky pro redis
Klikněte na tlačítko **Redis metriky** příliš[metriky zobrazit](cache-how-to-monitor.md#view-cache-metrics) ke svojí mezipaměti.

### <a name="alert-rules"></a>Pravidla výstrah

Klikněte na tlačítko **výstrah pravidla** tooconfigure výstrahy založené na metriky pro Redis Cache. Další informace najdete v tématu [výstrahy](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Diagnostika

Ve výchozím nastavení, mezipaměti metriky v Azure monitorování jsou [uchovávají po dobu 30 dnů](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) a odstraní. Klikněte na tlačítko toopersist metriky vaší mezipaměti po dobu delší než 30 dní, **diagnostiky** příliš[konfigurace účtu úložiště hello](cache-how-to-monitor.md#export-cache-metrics) používá toostore diagnostiku mezipaměti.

>[!NOTE]
>V přidání tooarchiving toostorage vaší mezipaměti metriky, můžete také [stream je centra událostí tooan nebo poslat tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

## <a name="support--troubleshooting-settings"></a>Podporovat & řešení potíží s nastavení
Hello nastavení v hello **podpory a řešení potíží s** části poskytují možnosti pro řešení problémů s mezipamětí.

![Podpora + řešení potíží](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Stav prostředků](#resource-health)
* [Nová žádost o podporu](#new-support-request)

### <a name="resource-health"></a>Stav prostředků
**Stav prostředku** sleduje prostředku a zjistíte, zda je spuštěn podle očekávání. Další informace o hello služby stavu prostředků Azure najdete v tématu [přehled stavu prostředků Azure](../resource-health/resource-health-overview.md).

> [!NOTE]
> Stav prostředku je momentálně nemůže tooreport na dobrý stav instance služby Azure Redis Cache hostované ve virtuální síti. Další informace najdete v tématu [všechny funkce mezipaměti fungují při hostování mezipaměti ve virtuální síti?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Nová žádost o podporu
Klikněte na tlačítko **nová žádost o podporu** tooopen žádost o podporu pro mezipaměť.





## <a name="default-redis-server-configuration"></a>Výchozí konfigurace serveru Redis
Nové instance služby Azure Redis Cache jsou nakonfigurovány s následující výchozí hodnoty konfigurace Redis hello.

> [!NOTE]
> Hello nastavení v této části nelze změnit pomocí hello `StackExchange.Redis.IServer.ConfigSet` metoda. Pokud tato metoda je volána s jedním z hello příkazy v této části, je vyvolána následující výjimce podobné toohello:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Všechny hodnoty, které se dají konfigurovat jako **max paměti policy**, se dají konfigurovat prostřednictvím hello portál Azure nebo nástroje příkazového řádku pro správu, například Azure CLI nebo prostředí PowerShell.
> 
> 

| Nastavení | Výchozí hodnota | Popis |
| --- | --- | --- |
| `databases` |16 |Hello výchozí počet databází je 16, ale můžete nakonfigurovat různých čísel na základě hello cenová úroveň. <sup>1</sup> hello výchozí databáze je databáze 0, můžete vybrat jinou na základě jednotlivých připojení pomocí `connection.GetDatabase(dbid)` kde `dbid` je číslo v rozsahu od `0` a `databases - 1`. |
| `maxclients` |Závisí na hello cenová úroveň<sup>2</sup> |Toto je hello maximální počet připojených klientů, které jsou povolené v hello stejný čas. Po dosažení limitu hello Redis zavře všechny hello nová připojení, vrátíte k chybě 'maximální počet klientů dosaženo'. |
| `maxmemory-policy` |`volatile-lru` |Zásady Maxmemory je hello nastavení pro jak Redis vybere co tooremove při `maxmemory` je dosaženo (hello velikost mezipaměti hello nabídky, jste vybrali při vytvoření mezipaměti hello). S Azure Redis Cache hello výchozí nastavení je `volatile-lru`, které odebere hello klíče s vypršení platnosti nastavit pomocí algoritmu hodnoty nejdelšího Nepoužití. Toto nastavení můžete nakonfigurovat v hello portálu Azure. Další informace najdete v tématu [paměti zásady](#memory-policies). |
| `maxmemory-samples` |3 |paměť toosave, LRU a minimální hodnota TTL algoritmy jsou přibližný algoritmy místo přesné algoritmy. Ve výchozím nastavení Redis kontroly tři klíče a vyskladnění hello jeden použitou méně nedávno. |
| `lua-time-limit` |5,000 |Maximální doba provádění skriptu Lua v milisekundách. Když je dosaženo maximální dobu spuštění hello, zaznamená Redis, je stále v provádění po hello maximální povolená doba skript a tooreply tooqueries začíná k chybě. |
| `lua-event-limit` |500 |Maximální velikost fronty událostí skriptu. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |Hello klienta výstupní vyrovnávací paměť omezení může být použit tooforce odpojení klientů, které nejsou čtení dat ze serveru hello dostatečně rychle z nějakého důvodu (obvyklým důvodem je, že klienta Pub nebo Sub tak rychle, jak je může vytvořit vydavatele hello nelze využívat zprávy). Další informace najdete v tématu [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>hello limit pro `databases` se liší pro každý Azure Redis Cache cenová úroveň a můžete nastavit při vytváření mezipaměti. Pokud žádné `databases` nastavení zadat během vytváření mezipaměti hello výchozí hodnota je 16.

* Základní a standardní mezipaměti
  * C0 (250 MB) mezipaměti - až too16 databáze
  * C1 mezipaměti (1 GB) – až too16 databáze
  * C2 mezipaměti (2,5 GB) – až too16 databáze
  * C3 (6 GB) mezipaměti - až too16 databáze
  * C4 (13 GB) mezipaměti - až too32 databáze
  * C5 (26 GB) mezipaměti - až too48 databáze
  * C6 (53 GB) mezipaměti - až too64 databáze
* Ukládá do mezipaměti Premium
  * P1 (6 GB - 60 GB) – až too16 databáze
  * P2 (13 GB - 130 GB) – až too32 databáze
  * P3 (26 GB - 260 GB) – až too48 databáze
  * P4 (53 GB - 530 GB) – až too64 databáze
  * Všechny prémiových mezipamětí s clusterem Redis povoleny – Redis clusteru podporuje použití databáze 0 tak hello `databases` omezení pro všechny premium mezipaměť Redis clusteru povolena je efektivně 1 a hello [vyberte](http://redis.io/commands/select) není povolen příkaz. Další informace najdete v tématu [potřebuji toomake žádné změny toomy klienta aplikace toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Další informace o databáze najdete v tématu [co jsou databáze Redis?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> Hello `databases` nastavení může být nakonfigurovaný jenom během vytváření mezipaměti a pouze pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných klientů pro správu. Příklad konfigurace `databases` během vytváření mezipaměti pomocí prostředí PowerShell, najdete v části [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` se liší pro každý Azure Redis Cache cenová úroveň.

* Základní a standardní mezipaměti
  * C0 (250 MB) mezipaměti - až too256 připojení
  * C1 mezipaměti (1 GB) – až too1, 000 připojení
  * C2 mezipaměti (2,5 GB) – až too2, 000 připojení
  * C3 (6 GB) mezipaměti - až too5, 000 připojení
  * C4 (13 GB) mezipaměti - až too10, 000 připojení
  * C5 (26 GB) mezipaměti - až too15, 000 připojení
  * C6 (53 GB) mezipaměti - až too20, 000 připojení
* Ukládá do mezipaměti Premium
  * P1 (6 GB - 60 GB) – až too7, 500 připojení
  * P2 (13 GB - 130 GB) – až too15 000 připojení
  * P3 (26 GB - 260 GB) – až too30 000 připojení
  * P4 (53 GB - 530 GB) – až too40 000 připojení

> [!NOTE]
> Při každém velikost mezipaměti umožňuje *až* určitý počet připojení, každý tooRedis připojení má režie spojená s ním spojená. Příkladem takových režie může být využití procesoru a paměti v důsledku šifrování pomocí protokolu TLS/SSL. Hello připojení maximální limit pro velikost daného mezipaměti předpokládá lehkým načíst mezipaměti. Pokud načíst z režijní náklady na připojení *plus* zatížení z klientské operace překročí kapacitu pro systém hello, hello mezipaměti můžete mít problémy kapacity, i pokud nebyl překročen limit pro aktuální velikost mezipaměti hello hello připojení.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis příkazy nejsou podporované ve službě Azure Redis Cache
> [!IMPORTANT]
> Vzhledem k tomu, že konfigurace a Správa instance služby Azure Redis Cache spravuje Microsoft, hello následující příkazy jsou zakázány. Pokud se pokusíte tooinvoke je příliš zobrazí chybová zpráva`"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * KONFIGURACE
> * LADĚNÍ
> * MIGRACE
> * ULOŽIT
> * VYPNUTÍ
> * SLAVEOF
> * CLUSTER - clusteru zápisu, které jsou příkazy, ale jen pro čtení clusteru příkazy jsou povoleny.
> 
> 

Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Konzola redis
Můžete bezpečně vydávat příkazy tooyour instancí Azure Redis Cache pomocí hello **konzola Redis**, která je dostupná v hello portál Azure pro všechny úrovně mezipaměti.

> [!IMPORTANT]
> - Hello Redis konzola nefunguje s [VNET](cache-how-to-premium-vnet.md). Pokud vaše mezipaměť je součástí virtuální sítě, můžete přístup jenom pro klienty v hello virtuální síť hello mezipaměti. Vzhledem k tomu konzola Redis běží v prohlížeči místní, což je mimo hello virtuální sítě, se nemůže připojit tooyour mezipaměti.
> - Ne všechny příkazy Redis jsou podporovány ve službě Azure Redis Cache. Seznam příkazů Redis, kteří jsou zakázáni pro Azure Redis Cache najdete v tématu hello předchozí [Redis příkazy nejsou podporované ve službě Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) části. Další informace o příkazech Redis najdete v tématu [http://redis.io/commands](http://redis.io/commands).
> 
> 

Klikněte na tlačítko tooaccess hello konzola Redis **konzoly** z hello **Redis Cache** okno.

![Konzola redis](./media/cache-configure/redis-console-menu.png)

příkazy tooissue proti instance mezipaměti, jednoduše zadejte hello požadovaný příkaz do konzoly hello.

![Konzola redis](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a>Pomocí hello konzola Redis se prémiový mezipaměť clusteru

Když mezipaměť hello konzola Redis pomocí prémiový clusteru, můžete použít příkazy tooa jeden horizontálních hello mezipaměti. tooissue příkaz tooa konkrétní horizontálního oddílu, nejdřív připojit požadované horizontálního oddílu toohello kliknutím na výběr hello horizontálního oddílu.

![Konzola redis](./media/cache-configure/redis-console-premium-cluster.png)

Když zkusíte tooaccess klíč, který je uložen v různých horizontálního oddílu než hello připojené horizontálního oddílu, obdržíte chybové zprávy podobné toohello následující zprávou.

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

V předchozím příkladu hello horizontálního oddílu 1 je hello vybrané horizontálního oddílu, ale `myKey` se nachází v horizontálního oddílu 0, jak je indikován hello `(shard 0)` část hello chybová zpráva. V tomto příkladu tooaccess `myKey`, vyberte pomocí horizontálního oddílu 0 hello horizontálního oddílu výběr a potom příkaz hello potřeby problém.


## <a name="move-your-cache-tooa-new-subscription"></a>Přesunout nového předplatného tooa mezipaměti
Kliknutím můžete přesunout nového předplatného tooa mezipaměti **přesunout**.

![Přesunout mezipaměti Redis](./media/cache-configure/redis-cache-move.png)

Informace o přesun prostředků z jednoho prostředku skupiny tooanother a z tooanother jedno předplatné, najdete v části [přesunout skupiny prostředků toonew prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Další kroky
* Další informace o práci s příkazy Redis najdete v tématu [jak můžete spouštět příkazy Redis?](cache-faq.md#how-can-i-run-redis-commands)

