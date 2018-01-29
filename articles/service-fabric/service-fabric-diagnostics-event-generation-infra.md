---
title: "Platformě Azure Service Fabric na úrovni monitorování | Microsoft Docs"
description: "Další informace o protokoly použít k monitorování a diagnostice clusterů Azure Service Fabric a úroveň události platformy."
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
ms.date: 11/20/2017
ms.author: dekapur
ms.openlocfilehash: 8452b5ae733b21254b0beecaec44a968897ae491
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="platform-level-event-and-log-generation"></a>Úroveň události a protokolu generování platformy

## <a name="monitoring-the-cluster"></a>Monitorování clusteru

Je důležité ke sledování na úrovni platformy k určení, zda hardware a cluster chovají podle očekávání. Když Service Fabric můžete ponechat aplikací, které běží při selhání hardwaru, ale potřebujete diagnostikovat, zda k chybě dochází v aplikaci nebo v odpovídající infrastruktury. Také měli byste sledovat clusterů lépe plánovat kapacitu, pomoc při rozhodování o o přidání nebo odebrání hardwaru.

Service Fabric nabízí následující protokolu kanály out-of-the-box:
* Provozní kanál: vysoké úrovně operací prováděných Service Fabric a cluster, včetně události pro uzel objevuje, nové nasazení aplikace nebo upgradu vrácení zpět, atd.
* Provozní kanál - podrobné: sestavy o stavu a rozhodnutí o vyrovnávání zatížení
* Zasílání zpráv & datový kanál: kritické protokoly a události vygenerované v zasílání zpráv (aktuálně pouze ReverseProxy) a cesta k datům (spolehlivé služby modelů)
* Zasílání zpráv & datový kanál - podrobné: podrobné kanálu, který obsahuje všechny nekritické protokoly z dat a zasílání zpráv v clusteru (v tomto kanálu má velmi velký objem událostí)   

Kromě toho existují dvě strukturovaných EventSource kanály, které poskytuje, jakož i protokoly, které shromažďujeme pro účely podpory.
* [Spolehlivé služby události](service-fabric-reliable-services-diagnostics.md): programovací model určité události
* [Spolehlivé aktéři události](service-fabric-reliable-actors-diagnostics.md): programovací model konkrétní události a čítače výkonu
* Podporovat protokoly: protokoly systému vygenerované Service Fabric jenom na které nás použije při poskytování podpory

Tyto různé kanály zahrnují většinu platformy úroveň protokolování, který se doporučuje. Pokud chcete zvýšit úroveň protokolování platformy, zvažte investici lepší vysvětlení stavu modelu a přidání sestavy vlastní stavu a přidání vlastní **čítače výkonu** k sestavení v reálném čase pochopení dopadu vaší služby a aplikace v clusteru.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric stavu a vytváření sestav zatížení

Service Fabric má svou vlastní stavu modelu, který je podrobně popsaná v těchto článcích:
- [Úvod do monitorování stavu Service Fabric](service-fabric-health-introduction.md)
- [Hlášení a kontrola stavu služeb](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Přidat vlastní sestavy stavu Service Fabric](service-fabric-report-health.md)
- [Zobrazit sestavy stavu Service Fabric](service-fabric-view-entities-aggregated-health.md)

Sledování stavu je velmi důležité několik aspektů operačního služby. Sledování stavu je obzvláště důležité, když Service Fabric provede upgradu s názvem aplikace. Po každé upgradované domény služby upgradu a je k dispozici pro vaše zákazníky, domény upgradu musí projít kontroly stavu, než nasazení přikročí k další upgradovací doméně. Pokud nelze dosáhnout dobrý stav, nasazení je vrácena, tak, aby aplikace je ve stavu známé a funkční. I když někteří zákazníci mohou mít vliv na předtím, než budou vráceny služeb, nebude většina zákazníků zaznamenat problém. Navíc řešení dojde k poměrně rychle a bez nutnosti čekat na akce z lidské obsluhy. Další kontroly stavu, které jsou součástí kódu, pružnější více služby je problémy při nasazení.

Další aspekt stav služby je generování sestav metriky ze služby. Metriky jsou důležité v Service Fabric, protože se používají k vyrovnávání využití prostředků. Metriky taky může být indikátor stavu systému. Například můžete mít aplikace, která má mnoho služeb, a každá instance sestavy požadavků za druhé metrika (RPS). Pokud se jedna služba používá více prostředků než jiné službě, Service Fabric přesune instance služby kolem clusteru, a zkuste udržovat využití i prostředků. Podrobnější vysvětlení, jak funguje využití prostředků najdete v tématu [spravovat spotřeby prostředků a načte v Service Fabric s metriky](service-fabric-cluster-resource-manager-metrics.md).

Metriky taky může pomoct získáte přehled o výkonu služby. V čase můžete zkontrolovat, že služba nyní pracuje v rámci očekávaných parametrů metriky. Například pokud trendy ukazují, že v 9: 00 v pondělí ráno se průměr RPS 1000, pak může nastavíte sestavy stavu, která vás upozorní, pokud RPS je menší než 500 nebo vyšší než 1 500. Všechno, co může být perfektně dobře, ale může být vhodné vypadá a ujistěte se, že vaši zákazníci mají optimálního uživatelského prostředí. Služby můžete definovat sadu metriky, které mohou být oznámeny pro účely kontroly stavu, ale který neovlivňuje vyrovnávání prostředků clusteru. K tomuto účelu nastavte váha metriky na nulu. Doporučujeme spustit všechny metriky s váhou nula a není zvýšit váhu, dokud si nejste jisti, pochopit, jak vážení metriky ovlivňuje pro váš cluster vyrovnávání prostředků.

> [!TIP]
> Nepoužívejte příliš mnoho vyvážené metriky. Může být obtížné zjistit, proč se přesouvání instance služby pro vyrovnávání. Několik metriky můžete vyhledat celou!

Veškeré informace, které může svědčit o stavu a výkonu vaší aplikace je kandidátem na metriky a stavu sestavy. Čítače výkonu procesoru se dozvíte, jak využívat vaše uzlu, ale neříká můžete jestli konkrétní službu je v pořádku, protože více služeb můžou běžet na jednom uzlu. Však metriky, například RPS, položky, které jsou zpracovány, a všechny doba vyřízení požadavku může svědčit o stavu specifické služby.

Do sestav stavu, použít kód podobná této:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

Nahlásit metriky, použijte kód podobná této:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>Podpora protokolů Service Fabric

Pokud potřebujete, kontaktujte podporu společnosti Microsoft pro pomoc s cluster Azure Service Fabric, podporu protokoly jsou téměř vždy vyžaduje. Pokud je váš cluster hostovaná v Azure, jsou automaticky konfigurovány a shromážděné v průběhu vytváření clusteru s podporou podpora protokolů. Protokoly jsou uloženy v účtu vyhrazeného úložiště ve skupině prostředků vašeho clusteru. Účet úložiště nemá pevnou název, ale v účtu, uvidíte kontejnery objektů blob a tabulek s názvy začínajícími *fabric*. Informace o nastavení protokolu kolekce pro cluster s podporou samostatné najdete v tématu [vytvořit a spravovat cluster Azure Service Fabric samostatné](service-fabric-cluster-creation-for-windows-server.md) a [nastavení konfigurace pro samostatný Windows clusteru](service-fabric-cluster-manifest.md). Pro samostatné instance Service Fabric by měly být odeslány protokoly do místní sdílené složky. Jste **požadované** do mají tyto protokoly pro podporu, ale nejsou určeny k být použitelná ve kýmkoli mimo tým podpory zákazníků společnosti Microsoft.

## <a name="enabling-diagnostics-for-a-cluster"></a>Povolení diagnostiky pro cluster

Aby bylo možné využít výhod tyto protokoly, důrazně doporučujeme při vytváření clusteru zapnutá "Diagnostika". Zapnutím diagnostiky při nasazování clusteru Windows Azure Diagnostics je možné potvrdit funkčnosti, spolehlivé Services a Reliable actors kanály a uložení dat jako další vysvětleno v [agregaci událostí v Azure Diagnostika](service-fabric-diagnostics-event-aggregation-wad.md).

Jak je vidět výše, je také volitelné pole přidat kód instrumentace Application Insights (AI). Pokud chcete použít pro všechny události analýzu AI (Další informace najdete v [analýza události s Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)), patří sem instrumentationKey prostředků AppInsights (GUID).


Pokud chcete nasazení kontejnerů do clusteru, povolte WAD zachycení statistiky docker přidáním tohoto vaší "> DiagnosticMonitorConfiguration WadCfg":

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Měření výkonu

Míra výkonu clusteru vám pomohou pochopit, jak je schopna zpracovávat zatížení a jednotku rozhodnutí ohledně škálování clusteru (informace o škálování clusteru najdete v části [v Azure](service-fabric-cluster-scale-up-down.md), nebo [místní](service-fabric-cluster-windows-server-add-remove-nodes.md)). Údaje o výkonu je také užitečné ve srovnání s akcemi, které vám nebo vaší aplikace a služby mohou být přijata, při analýze protokolů v budoucnu. 

Seznam čítačů výkonu, které mají shromažďovat při pomocí Service Fabric najdete v tématu [čítače výkonu v Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Tady jsou dvě běžné způsoby, ve kterých můžete nastavit shromažďování dat o výkonu pro váš cluster:

* Pomocí agenta: Toto je upřednostňovaný způsob shromažďování výkonu z počítače, protože agenti mají obvykle seznam možných výkonu metriky, které se můžou shromažďovat, a je poměrně snadné proces vybrat metriky, které chcete shromažďovat nebo změnit. Přečtěte si informace o [konfigurace pro Service Fabric OMS](service-fabric-diagnostics-event-analysis-oms.md) a [nastavení agenta Windows OMS](../log-analytics/log-analytics-windows-agent.md) články Další informace o agenta OMS, což je jedna takové monitorování agent, který je schopen vyzvednutí výkonu data pro virtuální počítače clusteru a nasazené kontejnery.

* Konfigurace diagnostiky k zápisu čítače výkonu do tabulky: pro clustery v Azure, to znamená, změna konfigurace Azure Diagnostics načíst čítače výkonu odpovídající z virtuálních počítačů v clusteru a povolení se vyzvedávat docker statistiky, pokud budete nasazovat žádné kontejnery. Přečtěte si informace o konfiguraci [čítače výkonu v WAD](service-fabric-diagnostics-event-aggregation-wad.md) v Service Fabric nastavení kolekce čítače výkonu.

## <a name="next-steps"></a>Další kroky

Třeba agregovat před odesláním pro žádnou platformu analýzy protokolů a událostí. Přečtěte si informace o [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) a [WAD](service-fabric-diagnostics-event-aggregation-wad.md) lépe pochopit některé doporučené možnosti.
