---
title: "Shromažďovat a analyzovat čítače výkonu v Azure Log Analytics | Microsoft Docs"
description: "Čítače výkonu jsou shromážděny prostřednictvím analýzy protokolů pro analýzu výkonu na agentů systému Windows a Linux.  Tento článek popisuje postup konfigurace shromažďování čítačů výkonu pro systému Windows a Linux agentů, podrobnosti o jejich jsou uloženy v úložišti OMS a jak analyzovat je na portálu OMS."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte
ms.openlocfilehash: 953bb453b0a9635627fbbb6c3913d0cd757101c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Systém Windows a Linux zdroje dat výkonu v analýzy protokolů
Čítače výkonu v systému Windows a Linux získat přehled o výkonu hardwarové součásti, operačních systémů a aplikací.  Analýzy protokolů můžete shromáždit čítače výkonu v pravidelných intervalech pro analýzu téměř reálném čase (NRT) kromě agregování dat výkonu pro delší období analýzu a vytváření sestav.

![Čítače výkonu](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurace čítačů výkonu
Konfigurovat na portálu OMS z čítače výkonu [nabídce Data v nastavení analýzy protokolů](log-analytics-data-sources.md#configuring-data-sources).

Při první konfiguraci systému Windows nebo Linux výkonu čítače pro nový pracovní prostor OMS, budete mít možnost rychle vytvořit několik běžných čítačů.  Jsou uvedeny s zaškrtávací políčko vedle každého.  Zajistěte, aby všechny čítače, které chcete vytvořit původně jsou zaškrtnutá políčka a pak klikněte na tlačítko **přidat vybrané čítače výkonu**.

Pro čítače výkonu systému Windows můžete konkrétní instance jednotlivých čítačů výkonu. Pro čítače výkonu systému Linux instanci jednotlivé čítače, který zvolíte, se vztahuje na všechny podřízené čítače nadřazené čítače. V následující tabulce jsou uvedeny běžné instancí, které jsou k dispozici pro Linux a Windows čítače výkonu.

| Název instance | Popis |
| --- | --- |
| \_Celkový počet |Celkový počet všech instancí |
| \* |Všechny instance |
| (/ &#124; / var) |Odpovídá instancí s názvem: / nebo /var |

### <a name="windows-performance-counters"></a>Čítače výkonu Windows

![Konfigurovat čítačů výkonu systému Windows](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Pomocí následujícího postupu přidejte nový čítač výkonu systému Windows ke shromažďování.

1. Zadejte název čítače v textovém poli ve formátu *objektu (instance) \counter*.  Když začnete psát, se zobrazí seznam běžných čítačů odpovídající.  Čítače můžete buď vybrat ze seznamu nebo zadejte svůj vlastní.  Můžete se taky vrátit všechny instance konkrétní čítače zadáním *object\counter*.  

    Při shromažďování čítače výkonu pro SQL Server z pojmenované instance, všechny s názvem instance čítače začněte s *MSSQL$* a následuje název instance.  Například shromažďovat čítač poměr přístupů do mezipaměti protokolu pro všechny databáze z objektu výkonu databáze s názvem SQL instance INST2, zadejte `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Klikněte na tlačítko  **+**  nebo stiskněte klávesu **Enter** přidat čítač do seznamu.
3. Když přidáte čítače, použije výchozí hodnotu 10 sekund, jeho **ukázkového intervalu**.  Můžete to na vyšší hodnotu až 1 800 sekund (30 minut) Pokud chcete snížit požadavky na úložiště dat shromážděných výkonu.
4. Po dokončení přidávání čítače, klikněte **Uložit** tlačítka v horní části obrazovky, čímž konfiguraci uložíte.

### <a name="linux-performance-counters"></a>Čítače výkonu systému Linux

![Konfigurovat čítačů výkonu systému Linux](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Pomocí následujícího postupu přidejte nový čítač výkonu Linux shromažďovat.

1. Ve výchozím nastavení všechny změny konfigurace automaticky odesílají na všechny agenty.  Pro agenty Linux konfigurační soubor posílá kolekcí dat Fluentd.  Pokud chcete upravit soubor ručně na každý agenta systému Linux, poté zrušte zaškrtnutí políčka *použít dole uvedenou konfiguraci u mých Linuxových počítačů* a postupujte podle pokynů níže.
2. Zadejte název čítače v textovém poli ve formátu *objektu (instance) \counter*.  Když začnete psát, se zobrazí seznam běžných čítačů odpovídající.  Čítače můžete buď vybrat ze seznamu nebo zadejte svůj vlastní.  
3. Klikněte na tlačítko  **+**  nebo stiskněte klávesu **Enter** přidat do seznamu jiných čítačů pro objekt, čítač.
4. Všechny čítače pro objekt používají stejné **ukázkového intervalu**.  Výchozí hodnota je 10 sekund.  Můžete změnit na vyšší hodnotu až 1 800 sekund (30 minut) Pokud chcete snížit požadavky na úložiště dat shromážděných výkonu.
5. Po dokončení přidávání čítače, klikněte **Uložit** tlačítka v horní části obrazovky, čímž konfiguraci uložíte.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Konfigurovat čítačů výkonu systému Linux v konfiguračním souboru
Namísto konfigurace čítače výkonu systému Linux pomocí portálu OMS, máte možnost úprav konfiguračních souborů na agenta systému Linux.  Metriky výkonu ke shromažďování jsou řízeny konfigurace v **/etc/opt/microsoft/omsagent/\<id pracovního prostoru\>/conf/omsagent.conf**.

Každý objekt nebo kategorie metrik výkonu ke shromažďování by měl být definována v konfiguračním souboru jako jeden `<source>` elementu. Syntaxe následující níže.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


V následující tabulce jsou popsány parametry v tomto elementu.

| Parametry | Popis |
|:--|:--|
| objekt\_název | Název objektu pro kolekci. |
| instance\_regex |  A *regulární výraz* definování instance, které ke shromažďování. Hodnota: `.*` Určuje všechny instance. Ke shromažďování metrik procesoru pro pouze \_celkový počet instancí, můžete zadat `_Total`. Ke shromažďování metrik proces pro pouze crond nebo sshd instancí, můžete zadat: ' (crond\|sshd)'. |
| Čítač\_název\_regex | A *regulární výraz* definice, které ke shromažďování čítače (pro objekt). Chcete-li shromažďovat všechny čítače pro objekt, zadejte: `.*`. Pokud chcete shromažďovat pouze čítače místa odkládacího souboru paměti objektu, například můžete zadat:`.+Swap.+` |
| Interval | Frekvence, při které se shromažďují objektu čítače. |


Následující tabulka uvádí objekty a čítače, které můžete zadat v konfiguračním souboru.  Existují další čítače pro konkrétní aplikace k dispozici jak je popsáno v [shromáždit čítače výkonu pro Linux aplikace v analýzy protokolů](log-analytics-data-sources-linux-applications.md).

| Název objektu | Název čítače |
|:--|:--|
| Logický Disk | % Volných uzlů Inode |
| Logický Disk | % Volného místa |
| Logický Disk | % Použitých uzlů Inode |
| Logický Disk | Využitý prostor v % |
| Logický Disk | Čtení z disku bajtů/s |
| Logický Disk | Čtení disku/s |
| Logický Disk | Přenosy disku/s |
| Logický Disk | Bajty zapisování na disk/s |
| Logický Disk | Zápis disku/s |
| Logický Disk | Volné megabajty |
| Logický Disk | Bajtů logického disku/s |
| Memory (Paměť) | Dostupná paměť v % |
| Memory (Paměť) | % Dostupného odkládacího prostoru |
| Memory (Paměť) | % Použité paměti |
| Memory (Paměť) | Využitý prostor záměny v % |
| Memory (Paměť) | Dostupná paměť v MB |
| Memory (Paměť) | Dostupná paměť odkládacího souboru |
| Memory (Paměť) | Čtení stránek/s |
| Memory (Paměť) | Zápisy stránek/s |
| Memory (Paměť) | Stránky/s |
| Memory (Paměť) | Použitý odkládací prostor v MB místa |
| Memory (Paměť) | Využitá paměť v MB |
| Síť | Celkový počet bajtů přenesených |
| Síť | Celkový počet přijatých bajtů |
| Síť | Celkový počet bajtů |
| Síť | Celkový počet paketů odeslaných |
| Síť | Celkový počet přijatých paketů |
| Síť | Celkový počet Rx chyby |
| Síť | Celkový počet Tx chyby |
| Síť | Celkový počet kolizí |
| Fyzický Disk | Střední Doba disku/čtení |
| Fyzický Disk | Střední Doba disku/přenos |
| Fyzický Disk | Střední Doba disku/zápis |
| Fyzický Disk | Bajtů fyzického disku/s |
| Proces | Protokol PCT privilegovaný čas |
| Proces | Protokol PCT uživatelského času |
| Proces | Využité paměti kB |
| Proces | Sdílené virtuální paměti |
| Procesor | % Času DPC |
| Procesor | % Času nečinnosti |
| Procesor | % Času přerušení |
| Procesor | Doba čekání % vstupně-výstupní operace |
| Procesor | Dobrý čas v % |
| Procesor | % Privilegovaného času |
| Procesor | % Času procesoru |
| Procesor | % Uživatelského času |
| Systémový | Volná fyzická paměť |
| Systémový | Volné místo ve stránkovacích souborech |
| Systémový | Volné virtuální paměti |
| Systémový | Procesy |
| Systémový | Velikost uložená ve stránkovacích souborech |
| Systémový | Doba provozu |
| Systémový | Uživatelé |


Toto je výchozí konfigurace pro metriku výkonu.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Shromažďování dat
Analýzy protokolů shromažďuje všechny čítače výkonu zadaný v jejich zadaný vzorek intervalu na všech agentů, kteří mají nainstalovaných čítač.  Data nejsou agregovány a dobu trvání určeného předplatného OMS je k dispozici ve všech zobrazeních vyhledávání protokolu nezpracovaná data.

## <a name="performance-record-properties"></a>Vlastnosti záznamu výkonu
Zaznamenává výkonu mít typ **výkonu** a mít vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Počítač |Počítač, který událost nebyla shromážděna z. |
| Název_čítače |Název čítače výkonu |
| Cesta_k_čítači |Úplná cesta čítače ve formě \\ \\ \<počítače >\\objekt(instance)\\čítače. |
| Přepočtené |Číselná hodnota čítače. |
| InstanceName |Název instance událostí.  Prázdný, pokud není žádná instance. |
| Název objektu |Název objektu výkonu |
| SourceSystem |Typ agenta data nebyla shromážděna z. <br><br>Připojit OpsManager – agent systému Windows, buď přímo nebo SCOM <br> Linux – všechny agenty Linux  <br> Azurestorage – Azure Diagnostics. |
| TimeGenerated |Datum a čas, který byl Vzorkovat data. |

## <a name="sizing-estimates"></a>Odhad velikosti
 Odhad pro kolekci jednotlivých čítačů v intervalech po 10 sekundách je přibližně 1 MB za den za instance.  Chcete-li odhadnout požadavky na úložiště jednotlivých čítačů pomocí následujícího vzorce.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Protokol hledání se záznamy výkonu
Následující tabulka obsahuje různé příklady vyhledávání protokolu, která načítají záznamy výkonu.

| Dotaz | Popis |
|:--- |:--- |
| Typ = výkonu |Všechny údaje o výkonu |
| Typ = výkonu počítač = "Tento počítač" |Všechny údaje o výkonu z určitého počítače |
| Typ = výkonu název_čítače = "Aktuální délka fronty disku" |Všechny údaje o výkonu pro konkrétní čítač |
| Typ = výkonu (ObjectName = procesoru) název_čítače = "čas procesoru v %" InstanceName = _celkem &#124; míra Avg(Average) jako AVGCPU počítačem. |Průměrné využití procesoru pro všechny počítače |
| Typ = výkonu (název_čítače = "čas procesoru v %") &#124;  míra max(Max) počítačem. |Maximální využití procesoru pro všechny počítače |
| Typ = výkonu ObjectName = logický disk název_čítače = "Aktuální délka fronty na disku" počítač = "MyComputerName" &#124; míra Avg(Average) podle InstanceName |Průměrná délka fronty disku pro aktuální napříč všemi instancemi daný počítač |
| Typ = výkonu název_čítače = "DiskTransfers za sekundu" &#124; míra percentile95(Average) počítačem. |95. percentil z přenosy disku/s pro všechny počítače |
| Typ = výkonu název_čítače = "čas procesoru v %" InstanceName = "_Total" &#124; měření avg(CounterValue) počítače interval 1 hodina |Hodinové průměr využití procesoru pro všechny počítače |
| Typ = výkonu počítač = "Tento počítač" název_čítače = % * InstanceName = _celkem &#124; míra percentile70(CounterValue) interval název_čítače 1 hodina. |Hodinové 70 percentilu každých % procentuální hodnoty čítače pro konkrétní počítač. |
| Typ = výkonu název_čítače = "čas procesoru v %" InstanceName = "_Total" (počítač = "Tento počítač") &#124; měření min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) počítače interval 1 hodina |Hodinové průměr, minimální, maximální a 75 percentilu využití procesoru pro určitý počítač |
| Typ = výkonu ObjectName = "MSSQL$ INST2: databáze" InstanceName = hlavní | Všechny údaje o výkonu z objektu výkonu databáze pro hlavní databázi z pojmenované instance systému SQL Server INST2.  

>[!NOTE]
> Pokud byl váš pracovní prostor upgradován na [nový dotazovací jazyk Log Analytics](log-analytics-log-search-upgrade.md), výše uvedené dotazy se změní na následující.

> | Dotaz | Popis |
|:--- |:--- |
| Výkonu |Všechny údaje o výkonu |
| Výkonu &#124; kde počítač == "Tento počítač" |Všechny údaje o výkonu z určitého počítače |
| Výkonu &#124; kde CounterName == "Aktuální délka fronty disku" |Všechny údaje o výkonu pro konkrétní čítač |
| Výkonu &#124; kde ObjectName == "Procesor" a název_čítače == "% času procesoru" a InstanceName == "_Total" &#124; shrnout AVGCPU = avg(Average) počítačem. |Průměrné využití procesoru pro všechny počítače |
| Výkonu &#124; kde CounterName == "% času procesoru" &#124; shrnout AggregatedValue = max(Max) počítačem. |Maximální využití procesoru pro všechny počítače |
| Výkonu &#124; kde ObjectName == "Logický disk" a název_čítače == "Aktuální délka fronty disku" a počítač == "MyComputerName" &#124; shrnout AggregatedValue = avg(Average) podle InstanceName |Průměrná délka fronty disku pro aktuální napříč všemi instancemi daný počítač |
| Výkonu &#124; kde CounterName == "DiskTransfers za sekundu" &#124; shrnout AggregatedValue = percentilu (průměru, 95) podle počítače |95. percentil z přenosy disku/s pro všechny počítače |
| Výkonu &#124; kde CounterName == "% času procesoru" a InstanceName == "_Total" &#124; shrnout AggregatedValue = avg(CounterValue) podle bin (TimeGenerated, 1 hod), počítač |Hodinové průměr využití procesoru pro všechny počítače |
| Výkonu &#124; kde počítač == "Tento počítač" a název_čítače startswith_cs "%" a InstanceName == "_Total" &#124; shrnout AggregatedValue = percentilu (přepočtené 70) podle bin (TimeGenerated, 1 hod), název_čítače | Hodinové 70 percentilu každých % procentuální hodnoty čítače pro konkrétní počítač. |
| Výkonu &#124; kde CounterName == "% času procesoru" a InstanceName == "_Total" a počítač == "Tento počítač" &#124; shrnout ["min(CounterValue)"] = min(CounterValue), ["avg(CounterValue)"] = avg(CounterValue), ["percentile75(CounterValue)"] = percentilu (přepočtené, 75), ["max(CounterValue)"] = max(CounterValue) podle bin (TimeGenerated, 1 hod), počítač |Hodinové průměr, minimální, maximální a 75 percentilu využití procesoru pro určitý počítač |
| Výkonu &#124; kde ObjectName == "MSSQL$ INST2: databáze" a InstanceName == "hlavní" | Všechny údaje o výkonu z objektu výkonu databáze pro hlavní databázi z pojmenované instance systému SQL Server INST2.  

## <a name="viewing-performance-data"></a>Zobrazení dat výkonu
Při spuštění vyhledávání protokolu pro data o výkonu, **seznamu** zobrazení se zobrazí ve výchozím nastavení.  Chcete-li zobrazit data v grafických formulářů, klikněte na tlačítko **metriky**.  Podrobné grafické zobrazení, klikněte na  **+**  vedle čítače.  

![Zobrazení metriky sbalené](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Agregovat data o výkonu v hledání protokolů, najdete v článku [metriky agregace na vyžádání a vizualizace v OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).


## <a name="next-steps"></a>Další kroky
* [Shromáždit čítače výkonu z aplikace Linux](log-analytics-data-sources-linux-applications.md) včetně MySQL a serveru Apache HTTP Server.
* Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.  
* Exportovat shromážděná data do [Power BI](log-analytics-powerbi.md) pro další vizualizaci a analýzu.
