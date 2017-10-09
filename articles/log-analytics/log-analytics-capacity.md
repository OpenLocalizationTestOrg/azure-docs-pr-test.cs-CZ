---
title: "aaaCapacity a výkonu řešení v Azure Log Analytics | Microsoft Docs"
description: "Použití hello kapacitu a výkon řešení v toohelp analýzy protokolů porozumíte hello kapacity vašich serverů Hyper-V."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: c47bb1e8bb9d4460b0241e89a616f3b356844b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-hello-capacity-and-performance-solution-preview"></a>Plánování kapacity virtuálního počítače technologie Hyper-V s hello kapacitu a výkon řešení (Preview)

![Symbol kapacitu a výkon](./media/log-analytics-capacity/capacity-solution.png)

Můžete použít hello kapacitu a výkon řešení v toohelp analýzy protokolů, které porozumíte hello kapacity vašich serverů Hyper-V. Hello řešení poskytuje přehled o prostředí Hyper-V můžete zobrazením hello celkové využití (procesoru, paměti a disku) hello hostitelů a hello virtuálních počítačů spuštěných na těchto hostitelích Hyper-V. Metriky se shromažďují pro procesoru, paměti a disky mezi hostiteli a hello virtuálních počítačů spuštěných na ně.

Hello řešení:

-   Zobrazí hostitelů s nejnižší a nejvyšší využití procesoru a paměti
-   Zobrazuje virtuální počítače s nejnižší a nejvyšší využití procesoru a paměti
-   Zobrazuje virtuální počítače s nejnižší a nejvyšší využití IOPS a propustnosti
-   Zobrazuje, které běží na které hostitele virtuálních počítačů
-   Zobrazuje hello nejvyšší disky s vysokou propustností, IOPS, a sdílené svazky latence v clusteru
- Umožňuje vám toocustomize a filtrování podle skupin

> [!NOTE]
> Požadovaná Hello předchozí verze hello kapacitu a výkon řešení správy kapacity volá System Center Operations Manager a System Center Virtual Machine Manager. Toto řešení aktualizované nemá těchto závislostí.


## <a name="connected-sources"></a>Připojené zdroje

Hello následující tabulka popisuje hello připojené zdroje, které podporuje toto řešení.

| Připojený zdroj | Podpora | Popis |
|---|---|---|
| [Agenti systému Windows](log-analytics-windows-agents.md) | Ano | řešení Hello shromažďuje informace kapacitu a výkon data z agentů v systému Windows. |
| [Agenti systému Linux](log-analytics-linux-agents.md) | Ne    | řešení Hello neshromažďuje data informace kapacitu a výkon od agentů přímé Linux.|
| [Skupiny správy nástroje SCOM](log-analytics-om-agents.md) | Ano |řešení Hello shromažďuje údaje o kapacitu a výkon od agentů v připojené skupině pro správu SCOM. Přímé připojení z hello SCOM agenta tooOMS se nevyžaduje. Data se předají z hello správy skupiny toohello OMS úložiště.|
| [Účet služby Azure Storage](log-analytics-azure-storage.md) | Ne | Úložiště Azure nezahrnuje data kapacitu a výkon.|

## <a name="prerequisites"></a>Požadavky

- Windows nebo agenty nástroje Operations Manager musí být nainstalován na Windows Server 2012 nebo vyšší hostitelů Hyper-V, není virtuálních počítačů.


## <a name="configuration"></a>Konfigurace

Proveďte následující krok tooadd hello kapacitu a výkon řešení tooyour prostoru hello.

- Přidat hello kapacitu a výkon řešení tooyour pracovním prostorem OMS hello procesem popsaným v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Sady Management Pack

Pokud skupinu správy nástroje SCOM pracovním prostorem OMS připojené tooyour, pak hello následující sady management Pack bude nainstalována v aplikaci SCOM při přidání tohoto řešení. Není potřeba žádná konfigurace ani údržba těchto sad Management Pack.

- Microsoft.IntelligencePacks.CapacityPerformance

podobná událost 1201 Hello:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Při aktualizaci hello kapacitu a výkon řešení, změní se číslo verze hello.

Další informace o tom, jak jsou aktualizovány sady management Pack řešení najdete v tématu [tooLog připojení nástroje Operations Manager Analytics](log-analytics-om-agents.md).

## <a name="using-hello-solution"></a>Pomocí řešení hello

Když přidáte hello kapacitu a výkon řešení tooyour prostoru, se přidá toohello řídicí panel Přehled hello kapacitu a výkon. Tuto dlaždici zobrazí počet hello počet aktuálně aktivních hostitelů Hyper-V a hello počet aktivních virtuálních počítačů, které byly monitorovány dobu hello vybrané období.

![Dlaždice kapacitu a výkon](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Zkontrolujte využití

Klikněte na hello kapacity a výkonu dlaždice tooopen hello kapacitu a výkon řídicí panel. řídicí panel Hello obsahuje sloupce hello hello následující tabulka. Každý sloupec uvádí až tooten položky odpovídající, že zadané sloupce kritéria pro hello oboru a časový rozsah. Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** dolnímu hello hello sloupce nebo kliknutím na záhlaví sloupce hello.

- **Hostitelé**
    - **Využití procesoru hostitele** zobrazuje grafické zobrazení trendu využití procesoru hello hostitelských počítačů a seznamu hostitelů, v závislosti na hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na tlačítko Hledat všechny hostitele tooopen název protokolu a zobrazit podrobnosti čítače procesoru pro hostované virtuální počítače.
    - **Hostování využití paměti** zobrazuje grafické zobrazení trendu hello využití paměti v hostitelských počítačích a seznamu hostitelů, v závislosti na hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na všechny hostitele název tooopen vyhledávání a zobrazení paměti čítače podrobnosti protokolu pro hostované virtuální počítače.
- **Virtual Machines**
    - **Využití procesoru virtuálního počítače** zobrazuje grafické zobrazení trendu hello procesoru využití virtuálních počítačů a seznam virtuálních počítačů, podle hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase pro hello top 3 virtuální počítače. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na tlačítko Hledat protokolu tooopen název všech virtuálních počítačů a agregované podrobnosti čítač procesoru pro hello virtuálních počítačů.
    - **Využití paměti virtuálního počítače** zobrazuje grafické zobrazení trendu využití paměti hello virtuálních počítačů a seznam virtuálních počítačů, podle hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase pro hello top 3 virtuální počítače. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na tlačítko Hledat protokolu tooopen název všech virtuálních počítačů a zobrazte podrobnosti čítače agregované paměti pro hello virtuálních počítačů.
    - **Celkový počet IOPS disku virtuálního počítače** zobrazuje grafické zobrazení trendu hello celkový disk IOPS pro virtuální počítače a seznam virtuálních počítačů s hello IOPS pro každou, podle hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase pro hello top 3 virtuální počítače. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na možnost všechny hledání protokolů tooopen název virtuálního počítače a zobrazení agregované disku IOPS Čítač Podrobnosti hello virtuálních počítačů.
    - **Celková propustnost disku virtuálního počítače** zobrazuje grafické zobrazení trendu propustnosti hello celkový disku pro virtuální počítače a seznam virtuálních počítačů s propustností hello celkový disku pro každou, na základě hello vybrané časové období. Pozastavte ukazatel myši nad hello řádku grafu tooview podrobnosti pro určitý bod v čase pro hello top 3 virtuální počítače. Další podrobnosti v protokolu vyhledávání klikněte na tlačítko tooview hello grafu. Klikněte na tlačítko Hledat protokolu tooopen název všech virtuálních počítačů a zobrazení agregovat podrobnosti čítače propustnost disku celkem pro hello virtuálních počítačů.
- **Sdílené svazky clusteru**
    - **Celková propustnost** ukazuje hello součet obou čte a zapisuje na sdílené svazky clusteru.
    - **Celkový počet IOPS** ukazuje hello součet vstupně výstupních operací za sekundu na sdílené svazky clusteru.
    - **Celková latence** zobrazuje celkový počet latence hello na sdílené svazky clusteru.
- **Hostování hustotu** nejvyšší dlaždice hello zobrazuje celkový počet hostitelů a virtuálních počítačů k dispozici toohello řešení hello. Klikněte na tlačítko hello nejvyšší dlaždice tooview další podrobnosti v protokolu vyhledávání. Také uvádí všechny hostitele a hello počet virtuálních počítačů, které jsou hostovány. Klikněte na tlačítko toodrill hostitele do výsledků hello virtuálních počítačů do protokolu hledání.


![okno hostitele řídicí panel](./media/log-analytics-capacity/dashboard-hosts.png)

![okno řídicího panelu virtuální počítače](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Vyhodnocení výkonu

Provozní výpočetní prostředí se výrazně liší od tooanother jedné organizace. Navíc může kapacitu a výkon úloh závisí na tom, jak vaše virtuální počítače jsou spuštěné, a co byste zvážit normální. Konkrétní postupy toohelp měření výkonu by pravděpodobně nelze použít tooyour prostředí. Takže více zobecněn doporučený postup je vhodný toohelp lépe. Společnost Microsoft publikuje různých z toohelp články doporučený postup měření výkonu.

toosummarize, řešení hello shromažďuje kapacitu a výkon data z různých zdrojů včetně čítače výkonu. Použijte kapacitu a výkon data, která se zobrazí v různých ploch v hello řešení a porovnat vaše výsledky toothose v hello [měření výkonu u Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) článku. I když hello článek byl publikován dřívějška, hello metriku, důležité informace a pokyny jsou stále platné. Hello článek obsahuje odkazy tooother užitečné zdroje.


## <a name="sample-log-searches"></a>Ukázky hledání v protokolech

Hello následující tabulka obsahuje ukázkový protokol hledání pro kapacitu a výkon data shromážděna a vypočítána toto řešení.

| Dotaz | Popis |
|---|---|
| Všechny konfigurace paměti hostitele | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Všechny konfigurace paměti virtuálního počítače | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Rozdělení celkový počet IOPS Disk napříč všechny virtuální počítače | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Rozdělení celková propustnost disku napříč všechny virtuální počítače | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Rozdělení celkový počet IOPS napříč všechny sdílené svazky clusteru | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Rozdělení celková propustnost mezi všechny sdílené svazky clusteru | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Rozdělení nízkou celkovou latenci mezi všechny sdílené svazky clusteru | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |

>[!NOTE]
> Pokud pracovní prostor byl upgradovaný toohello [nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md), pak změní následující toohello hello výše dotazy.

> | Dotaz | Popis |
|:--- |:--- |
| Všechny konfigurace paměti hostitele | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a název_čítače == "Hostitele přiřazené MB paměti" &#124; shrnout MB = avg(CounterValue) podle InstanceName |
| Všechny konfigurace paměti virtuálního počítače | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a název_čítače == "MB přiřazené paměti virtuálního počítače" &#124; shrnout MB = avg(CounterValue) podle InstanceName |
| Rozdělení celkový počet IOPS Disk napříč všechny virtuální počítače | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a (název_čítače == "Čtení virtuálního pevného disku/s" nebo název_čítače == "Zápisy virtuálního pevného disku/s") &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), název_čítače, InstanceName |
| Rozdělení celková propustnost disku napříč všechny virtuální počítače | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a (název_čítače == "Pro čtení virtuálního pevného disku MB/s" nebo název_čítače == "VHD zápisu MB/s") &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), název_čítače, InstanceName |
| Rozdělení celkový počet IOPS napříč všechny sdílené svazky clusteru | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a (název_čítače == "Čtení sdíleného svazku clusteru nebo s" nebo název_čítače == "CSV zápisy nebo s") &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), název_čítače, InstanceName |
| Rozdělení celková propustnost mezi všechny sdílené svazky clusteru | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a (název_čítače == "Čtení sdíleného svazku clusteru nebo s" nebo název_čítače == "CSV zápisy nebo s") &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), název_čítače, InstanceName |
| Rozdělení nízkou celkovou latenci mezi všechny sdílené svazky clusteru | Výkonu &#124; kde ObjectName == "Kapacitu a výkon" a (název_čítače == "Latenci pro čtení sdíleného svazku clusteru" nebo název_čítače == "CSV zápisu latence") &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), název_čítače, InstanceName |


## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobná data kapacity a výkonu.
