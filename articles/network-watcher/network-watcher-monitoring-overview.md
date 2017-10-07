---
title: "aaaIntroduction tooAzure sledovací proces sítě | Microsoft Docs"
description: "Tato stránka obsahuje přehled hello sledovací proces sítě služby pro monitorování a detekční síť připojená prostředky ve službě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 14bc2266-99e3-42a2-8d19-bd7257fec35e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: 283b3fa6add05d9bad6d5dbdae1524344d1bfc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-monitoring-overview"></a>Přehled monitorování sítě Azure

Zákazníci vytvářet síť začátku do konce v Azure tak, že orchestruje a skládání různé jednotlivých síťovým prostředkům, například virtuální síť, ExpressRoute, aplikační bránu, nástroje pro vyrovnávání zatížení a další. Monitorování je k dispozici na každém hello síťových prostředků. Označujeme toothis monitorování jako sledování na úrovni prostředků.

komplexní konfigurace a interakce mezi prostředky, vytváření komplexní scénáře, které je třeba na základě scénáře monitorování prostřednictvím sledovací proces sítě, může mít síťové tooend Hello.

Tento článek popisuje scénář a sledování na úrovni prostředků. Monitorování sítě v Azure je komplexní a zahrnuje dvě rozsáhlé kategorie:

* [**Sledovací proces sítě** ](#network-watcher) – na základě scénáře monitorování se poskytuje s funkcí hello v sledovací proces sítě. Tato služba obsahuje zachytáváním paketů, další směrování, IP tok ověření, zobrazení skupiny zabezpečení, tok protokolů NSG. Scénář úrovně monitorování obsahuje zobrazení tooend end síťovým prostředkům v monitorování kontrast tooindividual sítě prostředků.
* [**Sledování prostředků** ](#network-resource-level-monitoring) -sledování na úrovni prostředků se skládá ze čtyř funkce, diagnostické protokoly, metriky, řešení potíží s a stav prostředků. Všechny tyto funkce jsou postavené na úrovni prostředků síťového hello.

## <a name="network-watcher"></a>Network Watcher

Sledovací proces sítě je místní služba, která umožňuje vám toomonitor a diagnostikovat podmínky na úrovni scénář sítě, do a z Azure. Diagnostika sítě a k dispozici sledovací proces sítě vizualizace nástroje vám pomůžou pochopit, diagnostikovat a získat statistiky tooyour sítě v Azure.

Sledovací proces sítě má aktuálně hello následující možnosti:

* **[Topologie](network-watcher-topology-overview.md)**  -poskytuje síti úrovně zobrazení zobrazující hello různé propojení a přidružení mezi síťovým prostředkům ve skupině prostředků.
* **[Proměnné zachytáváním paketů](network-watcher-packet-capture-overview.md)**  -zaznamená data paketů do/z virtuálního počítače. Pokročilé možnosti a podrobně nastavit ovládací prvky, jako je například se může tooset čas filtrování a univerzálnost zadejte omezení velikosti. Hello paketů data mohou být uložena v úložišti objektů blob nebo na místní disk hello ve formátu CAP.
* **[Ověřte IP toku](network-watcher-ip-flow-verify-overview.md)**  – ověří, zda je paket povolený nebo zakázaný na základě toku informace 5 řazené kolekce členů paketu parametrů (cílovou IP adresu, zdrojové IP adresy, cílový Port, zdrojový Port a protokol). Pokud paket hello je zakázané skupiny zabezpečení, hello pravidlo a skupiny, který odepřen hello paketů je vrácena.
* **[Další směrování](network-watcher-next-hop-overview.md)**  -určuje hello další segment směrování paketů směrovány v hello prostředky infrastruktury sítě Azure, umožňuje toodiagnose všechny nesprávně nakonfigurované uživatelem definované trasy.
* **[Zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)**  -získá hello zabezpečení efektivní a použitých pravidel, která se použijí na virtuálním počítači.
* **[Protokolování toku NSG](network-watcher-nsg-flow-logging-overview.md)**  -toku protokoly pro skupinu zabezpečení sítě umožňují protokoly toocapture související se tootraffic, které jsou povolené nebo zakázané pravidla zabezpečení hello ve skupině hello. tok Hello je definována informací o 5-n-tice – zdrojové adresy IP, cílovou IP adresu, zdrojový Port, cílový Port a protokol.
* **[Brána virtuální sítě a řešení potíží s připojení](network-watcher-troubleshoot-manage-rest.md)**  -poskytuje možnost tootroubleshoot hello brány virtuální sítě a připojení.
* **[Sítě limity předplatného](#network-subscription-limits)**  -umožňuje využití prostředků sítě tooview omezení.
* **[Konfigurace protokolu diagnostiky](#diagnostic-logs)**  – poskytuje jedno podokno tooenable nebo zakázat diagnostické protokoly pro síťové prostředky ve skupině prostředků.
* **[Připojení (Preview)](network-watcher-connectivity-overview.md)**  -ověřuje hello možnost pro přímé připojení TCP z virtuálního počítače tooa, zadaný koncový bod.

### <a name="role-based-access-control-rbac-in-network-watcher"></a>Řízení přístupu na základě role (RBAC) v sledovací proces sítě

Sledovací proces sítě používá hello [model řízení řízení přístupu (RBAC)](../active-directory/role-based-access-control-what-is.md). Hello následující oprávnění vyžadují hello sledovací proces sítě. Je důležité toomake se, že má této role hello používá k zahájení rozhraní API sledovací proces sítě nebo pomocí sledovací proces sítě z portálu hello hello vyžaduje přístup.

|Prostředek| Oprávnění|
|---|---| 
|Microsoft.Storage/ |Čtení|
|Microsoft.Authorization/| Čtení| 
|Microsoft.Resources/subscriptions/resourceGroups/| Čtení|
|Microsoft.Storage/storageAccounts/listServiceSas/ | Akce|
|Microsoft.Storage/storageAccounts/listAccountSas/ |Akce|
|Microsoft.Storage/storageAccounts/listKeys/ | Akce|
|Microsoft.Compute/virtualMachines/ |Čtení|
|Microsoft.Compute/virtualMachines/ |Zápis|
|Microsoft.Compute/virtualMachineScaleSets/ |Čtení|
|Microsoft.Compute/virtualMachineScaleSets/ |Zápis|
|Microsoft.Network/networkWatchers/packetCaptures/ |Čtení|
|Microsoft.Network/networkWatchers/packetCaptures/| Zápis|
|Microsoft.Network/networkWatchers/packetCaptures/| Odstranění|
|Microsoft.Network/networkWatchers/ |Zápis |
|Microsoft.Network/networkWatchers/| Čtení |
|Microsoft.Insights/alertRules/ |*|
|Microsoft.Support/ | *|

### <a name="network-subscription-limits"></a>Limity předplatného sítě

Limity předplatného sítě poskytují podrobnosti o využití hello jednotlivých hello síťovému prostředku v rámci předplatného v oblasti proti hello maximální počet prostředků, které jsou k dispozici.

![limitu předplatného pro sítě][nsl]

## <a name="network-resource-level-monitoring"></a>Úrovně monitorování sítě prostředků

Hello následující funkce jsou k dispozici pro monitorování na úrovni prostředků:

### <a name="audit-log"></a>Protokol auditování

Operace provedené v rámci konfigurace hello sítí přihlášeni. Tyto protokoly můžete zobrazit v hello portál Azure nebo pomocí nástroje Microsoft, jako je Power BI nebo nástroje třetích stran. Protokoly auditu jsou k dispozici prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku a Rest API. Další informace o protokolů auditu najdete v tématu [auditovat operace s Resource Managerem](../resource-group-audit.md)

Protokoly auditu jsou k dispozici pro operace udělat na všechny síťové prostředky.

### <a name="metrics"></a>Metriky

Metriky jsou měření výkonu a čítače, které jsou shromážděny prostřednictvím v časovém intervalu. Metriky jsou aktuálně dostupné pro službu Application Gateway. Metriky lze použít tootrigger výstrahy podle prahovou hodnotu. V tématu [Application Diagnostics brány](../application-gateway/application-gateway-diagnostics.md) tooview jak metriky lze použít toocreate výstrahy.

![zobrazení metriky][metrics]

### <a name="diagnostic-logs"></a>Diagnostické protokoly

Pravidelné a spontánních události jsou vytvořené síťové prostředky a protokolovány v účtech úložiště, odeslané tooan centra událostí nebo analýzy protokolů. Tyto protokoly poskytují přehled o stavu hello prostředku. Tyto protokoly můžete zobrazit v nástrojů, jako je Power BI a analýzy protokolů. jak tooview diagnostické protokoly, navštivte toolearn [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md).

Diagnostické protokoly jsou k dispozici pro [nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-monitor-log.md), [skupin zabezpečení sítě](../virtual-network/virtual-network-nsg-manage-log.md), trasy a [Application Gateway](../application-gateway/application-gateway-diagnostics.md).

Sledovací proces sítě poskytuje že diagnostické protokoly zobrazení. Toto zobrazení obsahuje všechny síťové prostředky, které podporují protokolování diagnostiky. Z tohoto hlediska můžete povolit nebo zakázat síťových prostředků, snadno a rychle.

![Protokoly][logs]

### <a name="troubleshooting"></a>Řešení potíží

řešení potíží s okně prostředí portálu hello Hello je k dispozici na síťové prostředky dnes toodiagnose běžných problémů souvisejících s jednotlivé zdroje. Toto prostředí je k dispozici pro následující síťovým prostředkům - ExpressRoute, brána sítě VPN, aplikační bránu, protokoly zabezpečení sítě, trasy, DNS, nástroj pro vyrovnávání zatížení a Traffic Manager hello. toolearn Další informace o prostředku úrovni řešení potíží, navštivte [Diagnostikujte a vyřešte problémy s řešení potíží s Azure](https://azure.microsoft.com/blog/azure-troubleshoot-diagonse-resolve-issues/)

![informace o řešení potíží][TS]

### <a name="resource-health"></a>Stav prostředků

Stav Hello síťovému prostředku je k dispozici v pravidelných intervalech. Tyto prostředky zahrnují VPN Gateway a tunelového připojení sítě VPN. Stav prostředku je dostupné na portálu Azure hello. Navštivte toolearn Další informace o stavu prostředků [přehled stavu prostředků](../resource-health/resource-health-overview.md)

## <a name="next-steps"></a>Další kroky

Po získání informací o sledovací proces sítě, můžete další informace pro:

Provést zachytáváním paketů na vašem virtuálním počítači tak, že navštívíte [zachytáváním paketů proměnné v hello portálu Azure](network-watcher-packet-capture-manage-portal.md)

Provádění Proaktivní monitorování a diagnostiku pomocí [výstraha zachytáváním paketů](network-watcher-alert-triggered-packet-capture.md).

Zjištění chyb zabezpečení s [analýza zachytáváním paketů s Wireshark](network-watcher-deep-packet-inspection.md), pomocí nástroje s otevřeným zdrojem.

Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.

<!--Image references-->
[TS]: ./media/network-watcher-monitoring-overview/troubleshooting.png
[logs]: ./media/network-watcher-monitoring-overview/logs.png
[metrics]: ./media/network-watcher-monitoring-overview/metrics.png
[nsl]: ./media/network-watcher-monitoring-overview/nsl.png











