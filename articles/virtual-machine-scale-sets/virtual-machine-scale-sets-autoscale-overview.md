---
title: "Přehled automatického škálování s sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Další informace o různých způsobech může automaticky škálovat virtuální počítač Azure škálování nastaveni na výkon nebo podle pevného plánu"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 868523a3aca441a47218297be2ce9f9e46dd84a1
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Nastaví přehled škálování s měřítkem virtuální počítač Azure
Sadu škálování virtuálního počítače Azure můžete automaticky zvýšit nebo snížit počet instancí virtuálního počítače, které spusťte aplikaci. Toto chování automatizované a elastické snižuje správní režii ke sledování a optimalizace výkonu vaší aplikace. Můžete vytvořit pravidla, která definují minimálně přijatelný výkon prostředí kladné zákazníka. Pokud jsou splněny tyto definované prahové hodnoty, pravidel škálování provést akci k úpravě kapacity škálovací sady. Můžete také naplánovat události pro automatické zvýšení nebo snížení kapacity sady škálování na pevné časy. Tento článek obsahuje přehled výkonu, které jsou k dispozici metriky a jaké akce škálování můžete provádět.


## <a name="benefits-of-autoscale"></a>Výhody škálování
Pokud vaše aplikace vyžádání zvyšuje, nastavit zatížení instancím virtuálních počítačů ve vaší škálování zvyšuje. Pokud je tato zvýšená zátěž konzistentní, ne jenom stručný vyžádání, můžete nakonfigurovat pravidla škálování zvýšit počet instancí virtuálního počítače ve škálovací sadě.

Při vytváření těchto instancí virtuálního počítače a aplikace nasadí, byly sadou škálování začne distribuovat provoz prostřednictvím nástroje pro vyrovnávání zatížení. Můžete určit, jaké metriky, které chcete monitorovat, jako je například procesoru nebo paměti, jak dlouho zatížení aplikace musí splňovat danou prahovou hodnotu a kolik instancí virtuálních počítačů pro přidání do měřítka nastavit.

Večer nebo o víkendech se mohou snížit, vyžádání vaší aplikace. Je-li tento ke snížení zatížení přes v časovém intervalu konzistentní, můžete nakonfigurovat pravidla automatického škálování pro snížení počtu instancí virtuálních počítačů v sadě škálování. Tato akce škálování v snižuje náklady na provozování vaší škálování nastavena jako spustíte se počet instancí, které jsou nutné ke splnění aktuální vyžádání.


## <a name="use-host-based-metrics"></a>Použít metriky na hostiteli
Tento předdefinované hostitele metriky, které jsou k dispozici můžete vytvořit pravidla škálování od instancí virtuálních počítačů. Metriky hostitele získáte přehled o výkonu instance virtuálních počítačů v škálování nastavit bez nutnosti pro instalaci nebo konfiguraci dalších agentů a shromažďování dat. Pravidla automatického škálování, které používají tyto metriky můžete škálovat limit nebo počet instancí virtuálního počítače v reakci na využití procesoru, paměti vyžádání nebo přístup k disku.

Pravidla automatického škálování využívající metriky hostitele je možné vytvořit pomocí některého z následujících nástrojů:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Chcete-li vytvořit pravidel škálování, které používají podrobnějších metrik výkonu, můžete [instalaci a konfiguraci rozšíření diagnostiky Azure](#in-guest-vm-metrics-with-the-azure-diagnostics-extension) na instancích virtuálních počítačů, nebo [konfigurace používání aplikace App Insights](#application-level-metrics-with-app-insights).

Pravidla automatického škálování, která používají metriky na hostiteli, metriky v hostovi virtuálního počítače s rozšíření diagnostiky Azure a App Insights můžete použít následující nastavení konfigurace.

### <a name="metric-sources"></a>Metriky zdroje
Škálování pravidla můžete použít metriky na jednom z následujících zdrojů:

| Metriky zdroje        | Případ použití                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| Aktuální sadě škálování    | Pro metriky na hostiteli, které nevyžadují další agenty nainstalovány nebo nakonfigurovány.                                  |
| Účet úložiště      | Rozšíření diagnostiky Azure zapíše do úložiště Azure, která se pak využívá k aktivaci pravidel škálování metrik výkonu. |
| Frontou Service Bus    | Vaše aplikace ani jiné součásti může přenášet zprávy ve frontě Azure Service Bus pravidlům aktivační události.                   |
| Application Insights | Balíček instrumentace nainstalovaný ve vaší aplikaci, která datové proudy metriky přímo z aplikace.                         |


### <a name="autoscale-rule-criteria"></a>Kritéria pravidla automatického škálování
Následující metriky na hostiteli jsou k dispozici pro použití při vytváření pravidel škálování. Pokud používáte rozšíření diagnostiky Azure nebo statistiky aplikace, můžete definovat metriky, které ke sledování a pomocí pravidel automatického škálování.

| Název metriky               |
|---------------------------|
| Procento využití procesoru            |
| Sítě v                |
| Sítě Out               |
| Bajty čtení disku           |
| Bajty zápisu disku          |
| Čtení z disku operace/s  |
| Operace zápisu disku/s |
| Zbývající kredit procesoru     |
| Použít kredity procesoru      |

Při vytváření pravidel automatického škálování pro danou metriku sledování, podívejte se na jednu z následujících akcí agregace metriky pravidla:

| Typ agregace |
|------------------|
| Průměr          |
| Minimální          |
| Maximální počet          |
| Celkem            |
| poslední             |
| Počet            |

Pravidla automatického škálování jsou potom aktivuje, když metriky se porovná s vaší definovanou prahovou hodnotu s jedním z následující operátory:

| Operátor                 |
|--------------------------|
| Více než             |
| Větší než nebo rovno |
| Méně než                |
| Menší než nebo rovno    |
| Rovno                 |
| Není rovno             |


### <a name="actions-when-rules-trigger"></a>Akce při aktivaci pravidla
Když aktivační události pravidlo škálování škálovací sadu může automaticky škálovat v jednom z následujících způsobů:

| Operace škálování     | Případ použití                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Zvýšení počtu podle   | Pevný počet instancí virtuálních počítačů k vytvoření. Užitečné v sady škálování s menší počet virtuálních počítačů.                                           |
| Procentuální podle zvýšení | Na základě procenta zvýšení instance virtuálních počítačů. Dobré horizontálního větší nastaví, kde fixního zvýšení nemusí výkon výrazně snížit. |
| Zvyšte počet   | Vytvořte, protože velký počet instancí virtuálního počítače jsou potřeba k dosažení požadované maximální velikost.                                                            |
| Snižte počet   | Pevný počet instancí virtuálního počítače odebrat. Užitečné v sady škálování s menší počet virtuálních počítačů.                                           |
| Procento snížení podle | Na základě procento snížení instance virtuálních počítačů. Dobré horizontálního větší nastaví, kde fixního zvýšení nemusí výrazně snížit náklady a spotřeba prostředků. |
| Snižte počet   | Odeberte, protože velký počet instancí virtuálního počítače jsou potřeba k dosažení požadovanou minimální velikost.                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Metriky v hostovi virtuálního počítače s příponou Azure diagnostics
Rozšíření Azure diagnostics není agenta, který běží v rámci instance virtuálního počítače. Agent sleduje a uloží metriky výkonu do úložiště Azure. Tyto metriky výkonu obsahují podrobnější informace o stavu virtuálního počítače, jako například *AverageReadTime* pro disky nebo *PercentIdleTime* pro procesor. Můžete vytvořit pravidla automatického škálování, které jsou založené na podrobnější informace o výkonu virtuálního počítače, nikoli pouze procento využití nebo paměť spotřeby procesoru.

Pokud chcete používat rozšíření Azure diagnostiky, musí vytvořit účet úložiště Azure pro vaše instance virtuálního počítače, nainstalovat agenta Azure diagnostics a pak konfigurace virtuálních počítačů do datového proudu konkrétních čítačích výkonu k účtu úložiště.

Další informace najdete v článcích popisujících povolení diagnostického rozšíření Azure na [virtuálním počítači s Linuxem](../virtual-machines/linux/diagnostic-extension.md) nebo [virtuálním počítači s Windows](../virtual-machines/windows/ps-extensions-diagnostics.md).


## <a name="application-level-metrics-with-app-insights"></a>Metriky na úrovni aplikace s Statistika aplikace
K získání větší přehled v na výkon aplikací, můžete použít Application Insights. Můžete nainstalovat balíček malé instrumentace ve vaší aplikaci, která monitoruje aplikace a odesílá telemetrická data do Azure. Počty relace a metriky například odezvy vaší aplikace, stav zatížení stránky, můžete monitorovat. Tyto metriky aplikace lze použít k vytvoření pravidel škálování granulární a vložené úrovni, jak aktivovat pravidla založená na prakticky využitelné informace, které můžou mít vliv zkušeností zákazníků.

Další informace o službě App Insights najdete v tématu [Co je Application Insights](../application-insights/app-insights-overview.md).


## <a name="scheduled-autoscale"></a>Naplánované škálování
Můžete také vytvořit pravidla automatické škálování podle plánů. Tato pravidla na základě plánu umožňují automaticky škálování počtu instancí virtuálních počítačů na pevné časy. S pravidly na základě výkonu na aplikaci před aktivační událost pravidel škálování může být dopad na výkon a jsou zřídit nové instance virtuálního počítače. Pokud očekáváte takové vyžádání, další instance virtuálních počítačů jsou zřízené a připravené pro další použití a aplikace poptávka.

Následující příklady jsou scénáře, které mohou mít prospěch použití pravidla na základě plánu škálování:

- Automaticky škálovat počet instancí virtuálního počítače na začátku pracovního dne, kdy zvyšuje poptávku zákazníků. Na konci pracovního dne automatické škálování počtu instancí virtuálních počítačů přes noc minimalizovat náklady na zdroje při nízkém využívání aplikací.
- Pokud aplikace používá oddělení výraznou na určité části měsíci nebo fiskálním cyklus, automatické škálování počtu instancí virtuálních počítačů pro přizpůsobení jejich další požadavky.
- Když je marketing události, zvýšení úrovně nebo svátek prodej, může automaticky škálovat počet instancí virtuálního počítače před předpokládaného poptávka. 


## <a name="next-steps"></a>Další kroky
Můžete vytvořit pravidla automatického škálování, které používají metriky na hostiteli s jedním z následujících nástrojů:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Tento přehled podrobné postupy pomocí pravidel automatického škálování můžete škálovat horizontálně a zvýšit nebo snížit *číslo* instance virtuálních počítačů ve vaší škálování nastavit. Můžete taky škálovat svisle zvýšení nebo snížení instance virtuálního počítače *velikost*. Další informace najdete v tématu [svislé škálování s sady škálování virtuálního počítače](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Informace o tom, jak spravovat instancím virtuálních počítačů najdete v tématu [sadách škálování virtuálních počítačů spravovat pomocí prostředí Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zjistěte, jak vygenerovat upozornění, když vaše škálování pravidla aktivační událost, najdete v tématu [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). Můžete také [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).