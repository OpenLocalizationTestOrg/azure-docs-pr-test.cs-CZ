---
title: "aaaAzure Service Fabric platformy úroveň monitorování | Microsoft Docs"
description: "Další informace o platformě úrovně protokoly a události použité toomonitor a diagnostikovat Azure Service Fabric clustery."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: f8fb1c8b546e05c517ae12c91906acc04cd6eaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="platform-level-event-and-log-generation"></a>Úroveň události a protokolu generování platformy

## <a name="monitoring-hello-cluster"></a>Monitorování hello clusteru

Je důležité toomonitor na toodetermine úrovni hello platformy, jestli hardware a cluster chovají podle očekávání. Když Service Fabric můžete ponechat aplikací, které běží při selhání hardwaru, ale potřebujete stále toodiagnose, zda dochází k chybě v aplikaci nebo v základní infrastruktuře hello. Také měli byste sledovat plánu toobetter clustery kapacitu, pomoc při rozhodování o o přidání nebo odebrání hardwaru.

Service Fabric nabízí pět různých protokolových kanály out-of-the-box, generovat hello následující události:

* Provozní kanál: vysoké úrovně operací prováděných Service Fabric a hello clusteru, včetně událostí pro uzel objevuje, novou aplikaci nasazuje, nebo SF upgrade vrácení zpět, atd.
* Provozní kanál - podrobné: sestavy o stavu a rozhodnutí o vyrovnávání zatížení
* Zasílání zpráv & datový kanál: kritické protokoly a události vygenerované v našem zasílání zpráv (aktuálně pouze hello ReverseProxy) a cesta k datům (spolehlivé služby modelů)
* Zasílání zpráv & datový kanál - podrobné: podrobné kanálu, který obsahuje všechny protokoly nekritické hello z dat a zasílání zpráv v clusteru hello (v tomto kanálu má velmi velký objem událostí)   
* [Spolehlivé služby události](service-fabric-reliable-services-diagnostics.md): programovací model určité události
* [Spolehlivé aktéři události](service-fabric-reliable-actors-diagnostics.md): programovací model konkrétní události a čítače výkonu
* Podporovat protokoly: protokoly systému vygenerované nám použitou při poskytování podpory pouze toobe Service Fabric

Většinu platformy hello úrovně se protokolování, který se doporučuje se věnují tyto různé kanály. tooimprove platformy úroveň protokolování, zvažte investici lepší pochopení hello stavu modelu a přidání sestavy vlastní stavu a přidání vlastní **čítače výkonu** toobuild dopadu v reálném čase pochopení hello služby a aplikace v clusteru hello.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric stavu a vytváření sestav zatížení

Service Fabric má svou vlastní stavu modelu, který je podrobně popsaná v těchto článcích:
- [Monitorování stavu infrastruktury tooService Úvod](service-fabric-health-introduction.md)
- [Hlášení a kontrola stavu služeb](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Přidat vlastní sestavy stavu Service Fabric](service-fabric-report-health.md)
- [Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

Sledování stavu je důležité toomultiple aspektů operačního služby. Sledování stavu je obzvláště důležité, když Service Fabric provede upgradu s názvem aplikace. Po každé upgradované domény služby hello upgradu a je k dispozici tooyour zákazníků, hello upgradovací doméně musí uplynout kontroly stavu, než nasazení hello přesune toohello další upgradovací doméně. Pokud nelze dosáhnout dobrý stav, hello nasazení je vrácena, tak, aby aplikace hello je ve stavu známé a funkční. I když někteří zákazníci mohou mít vliv na předtím, než budou vráceny hello služby, nebude většina zákazníků zaznamenat problém. Navíc řešení dojde k poměrně rychle a bez nutnosti toowait pro akci z lidské obsluhy. Hello další kontroly stavu, které jsou součástí kódu, hello pružnější že služby je toodeployment problémy.

Další aspekt stav služby je generování sestav metriky ze služby hello. Metriky jsou důležité v Service Fabric, protože jsou použité toobalance využití prostředků. Metriky taky může být indikátor stavu systému. Například můžete mít aplikace, která má mnoho služeb, a každá instance sestavy požadavků za druhé metrika (RPS). Pokud se jedna služba používá více prostředků než jiné službě, Service Fabric přesune instance služby kolem hello clusteru využití tootry toomaintain i prostředků. Podrobnější vysvětlení, jak funguje využití prostředků najdete v tématu [spravovat spotřeby prostředků a načte v Service Fabric s metriky](service-fabric-cluster-resource-manager-metrics.md).

Metriky taky může pomoct získáte přehled o výkonu služby. V čase můžete použít toocheck metriky, které hello služba nyní pracuje v rámci očekávaných parametrů. Například pokud trendy ukazují, že v 9: 00 v pondělí ráno hello průměrná RPS je 1 000, pak můžete nastavit sestavy stavu, která vás upozorní, pokud hello RPS menší než 500 nebo vyšší než 1 500. Všechno, co může být perfektně dobře, ale může být vhodné toobe podívejte se, že vaši zákazníci mají optimálního uživatelského prostředí. Služby můžete definovat sadu metriky, které mohou být oznámeny pro účely kontroly stavu, ale který neovlivňuje hello vyrovnávání prostředků clusteru hello. toodo se toozero sadu hello váha metriky. Doporučujeme spustit všechny metriky s váhou nula a není zvýšit hello váhy, dokud si nejste jisti, pochopit, jak vážení hello metriky ovlivňuje pro váš cluster vyrovnávání prostředků.

> [!TIP]
> Nepoužívejte příliš mnoho vyvážené metriky. Může být obtížné toounderstand, proč se přesouvání instance služby pro vyrovnávání. Několik metriky můžete vyhledat celou!

Veškeré informace, které mohou znamenat hello stavu a výkonu vaší aplikace je kandidátem na metriky a stavu sestavy. Čítače výkonu procesoru se dozvíte, jak využívat vaše uzlu, ale neříká můžete jestli konkrétní službu je v pořádku, protože více služeb můžou běžet na jednom uzlu. Však metriky, například RPS, položky, které jsou zpracovány, a všechny doba vyřízení požadavku může naznačovat hello stavu specifické služby.

Stav tooreport, podobně jako toothis použití kódu:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

tooreport metriky, použijte podobné toothis kódu:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>Podpora protokolů Service Fabric

Pokud potřebujete toocontact podporu společnosti Microsoft o pomoc k vašemu clusteru Azure Service Fabric, podporu protokoly jsou téměř vždy vyžaduje. Pokud je váš cluster hostovaná v Azure, jsou automaticky konfigurovány a shromážděné v průběhu vytváření clusteru s podporou podpora protokolů. Hello protokoly se ukládají do vyhrazeného úložiště účet ve skupině prostředků vašeho clusteru. účet úložiště Hello nemá pevnou název, ale v hello účtu, uvidíte kontejnery objektů blob a tabulek s názvy začínajícími *fabric*. Informace o nastavení protokolu kolekce pro cluster s podporou samostatné najdete v tématu [vytvořit a spravovat cluster Azure Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a [nastavení konfigurace pro samostatný Windows clusteru](service-fabric-cluster-manifest.md). Pro samostatné instance Service Fabric musí být hello protokoly zasílána tooa místní sdílené složky. Jste **požadované** toohave tyto protokoly pro podporu, ale nejsou určený toobe použitelné ve kýmkoli mimo tým podpory zákazníků společnosti Microsoft hello.

## <a name="enabling-diagnostics-for-a-cluster"></a>Povolení diagnostiky pro cluster

V pořadí tootake výhod tyto protokoly důrazně doporučujeme při vytváření clusteru zapnutá "Diagnostika". Zapnutím diagnostiky, při nasazení clusteru hello Windows Azure Diagnostics je možné tooacknowledge hello funkčnosti, Reliable Services a Reliable actors kanály a další data hello úložiště, jak je vysvětleno v [agregovat události s Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).

Jak je vidět výše, je také volitelné pole tooadd kód instrumentace Application Insights (AI). Pokud se rozhodnete toouse AI pro nějakou analýzu událostí (Další informace najdete v [analýza události s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)), patří sem hello AppInsights prostředků instrumentationKey (GUID).


Pokud chcete toodeploy kontejnery tooyour clusteru, povolte WAD toopick až docker statistiky přidáním této tooyour "> WadCfg DiagnosticMonitorConfiguration":

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Měření výkonu

Míra výkonu clusteru vám pomohou pochopit, jak je možné toohandle zatížení a jednotku rozhodnutí ohledně škálování clusteru (informace o škálování clusteru najdete v části [v Azure](service-fabric-cluster-scale-up-down.md), nebo [místní](service-fabric-cluster-windows-server-add-remove-nodes.md)). Údaje o výkonu je také užitečné při porovnání tooactions vám nebo vaší aplikace a služby pravděpodobně v režimu, při analýze protokolů v hello budoucí. 

Seznam toocollect čítače výkonu při použití Service Fabric najdete v tématu [čítače výkonu v Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Tady jsou dvě běžné způsoby, ve kterých můžete nastavit shromažďování dat o výkonu pro váš cluster:

* Pomocí agenta: to je hello upřednostňovaný způsob shromažďování výkonu z počítače, protože agenti mají obvykle seznam možných výkonu metriky, které se můžou shromažďovat, a je proces je poměrně snadné toochoose hello metriky má toocollect či změnit . Přečtěte si informace o [jak tooconfigure hello OMS pro Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) a [nastavení agenta OMS Windows hello](../log-analytics/log-analytics-windows-agents.md) články toolearn více informací o hello OMS agenta, který je jeden takový monitorování agent, který je možné toopick nahoru údaje o výkonu pro virtuální počítače clusteru a nasazené kontejnery.

* Konfigurace diagnostiky toowrite výkonu čítače tooa tabulky: pro clustery v Azure, to znamená, změna hello Azure Diagnostics konfigurace toopick až hello příslušné čítače z hello virtuálních počítačů v clusteru a povolení toopick nahoru Pokud budete nasazovat žádné kontejnery docker statistiky. Přečtěte si informace o konfiguraci [čítače výkonu v WAD](service-fabric-diagnostics-event-aggregation-wad.md) v Service Fabric tooset až kolekci čítačů výkonu.

## <a name="next-steps"></a>Další kroky

Protokoly a události potřebovat toobe agregovat před jejich odesláním tooany analysis platformy. Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter pochopit hello doporučené možnosti.
