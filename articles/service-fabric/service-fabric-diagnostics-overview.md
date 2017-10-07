---
title: "aaaAzure Přehled diagnostiky a monitorování prostředků infrastruktury služby | Microsoft Docs"
description: "Další informace o monitorování a Diagnostika pro clustery, aplikace a služby Azure Service Fabric."
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
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 1ef6419863b056b76d81e915ab78df4facae88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorovací a diagnostické pro Azure Service Fabric

Monitorovací a diagnostické jsou kritické toodeveloping, testování a nasazení aplikace a služby v jakémkoli prostředí. Řešení Service Fabric fungují lépe, když plánování a implementace monitorování a diagnostiky, které pomáhají zajistit aplikace a služby jsou funguje podle očekávání v místním vývojovém prostředí, nebo v produkčním prostředí.

Hello hlavní cíle monitorování a Diagnostika mají:
* Najít a diagnostikovat problémy s hardwarem a infrastruktury
* Rozpoznat problémy s softwaru a aplikace, zkrátit dobu prostojů při služby
* Informace k prostředku využívání a nápovědy jednotky operations rozhodování o
* Optimalizace výkonu aplikace, služby a infrastruktury
* Generovat podnikových statistik a identifikujte oblasti zlepšování


Hello celkového pracovního postupu monitorování a Diagnostika se skládá ze tří kroků:

1. **Generování událostí**: Jedná se o událostí (protokoly, trasování, vlastní události) na hello infrastruktury (cluster), platformy a aplikace / service úrovni
2. **Agregace událostí**: vygenerovaných událostí potřebovat toobe shromážděných a agregovat před jejich zobrazením
3. **Analýza**: události potřebovat toobe vizualizovaných a v některých formát, tooallow pro analýzu a zobrazení podle potřeby

Více produkty jsou k dispozici, které zahrnují tyto tři oblasti a jsou volné toochoose různých technologií pro každou. Je důležité toomake jisti, který hello různých částí pracovní společně toodeliver řešení monitorování v začátku do konce pro váš cluster.

## <a name="event-generation"></a>Generování událostí

prvním krokem Hello v pracovním postupu monitorování a Diagnostika hello je vytvoření hello a generování událostí a protokolů. Tyto události, protokoly a trasování jsou generovány na dvou úrovních: hello vrstvy platformy (včetně hello cluster, hello počítače nebo akce Service Fabric) nebo hello aplikační vrstvu (všechny instrumentace přidává tooapps a služby nasadit toohello clusteru). Události na všech těchto úrovních jsou přizpůsobitelné, i když Service Fabric poskytuje některé instrumentace ve výchozím nastavení.

Další informace o [úroveň události platformy](service-fabric-diagnostics-event-generation-infra.md) a [události na úrovni aplikace](service-fabric-diagnostics-event-generation-app.md) toounderstand, co jsou poskytovány a jak tooadd další instrumentace.

Po rozhodování o protokolování zprostředkovatele chcete toouse hello, musíte toomake, jestli bude vaše protokoly agregovat a uložené správně.

## <a name="event-aggregation"></a>Agregace událostí

Pro shromažďování protokolů hello a událostí generovaných aplikací a cluster, obvykle doporučujeme používat [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) (více podobné kolekce na základě tooagent protokolu) nebo [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)(v procesu protokolu kolekce).

Shromažďování protokolů aplikací pomocí rozšíření diagnostiky Azure je vhodný pro služby Service Fabric nastaveného hello protokolu zdroje a cíle se nemění často a existuje přehledné mapování mezi zdroji hello a jejich cíle. Hello důvod pro toto je konfigurace Azure Diagnostics se odehrává na vrstvě Resource Manager hello, takže provedení významných změn toohello konfigurace vyžaduje, aktualizace nebo opětovného nasazení clusteru hello. Kromě toho je nejlépe použity při ověřování, protokoly se ukládají někde další trvalé, ze které lze k nim podle různých platformách analysis. To znamená, že končí až se kanálu méně efektivní, než budete s možností jako EventFlow.

Pomocí [EventFlow](https://github.com/Azure/diagnostics-eventflow) umožňuje toohave služby odešlete protokoly přímo tooan analýzy a vizualizace platformy, anebo toostorage. Může být možné používat další knihovny (objektu ILogger, Serilog atd.) pro hello stejný účel, ale EventFlow má výhodu hello s byl určený speciálně pro v procesu protokolu kolekce a toosupport Service Fabric služby. To obvykle toohave potenciální několik výhod:

* Jednoduchá konfigurace a nasazení
    * Konfigurace Hello shromažďování diagnostických dat je právě součástí konfigurace služby hello. Je snadno tooalways zachovat ji "synchronizována" s hello rest aplikace hello
    * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelný
    * Konfigurace cíle dat prostřednictvím EventFlow stačí hello odpovídající balíček NuGet pro přidání a změna hello *eventFlowConfig.json* souboru
* Flexibilita
    * Hello aplikace může odesílat hello data bez ohledu na potřebuje toogo, dokud není klientské knihovny, která podporuje systém úložiště dat hello cílové. Nový port nelze přidat podle potřeby
    * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována
* Data aplikací toointernal přístup a kontext
    * diagnostické subsystému Hello běžících v rámci procesu aplikace nebo služby hello můžete snadno posílení hello trasování s kontextové informace

Jednu věc toonote je, že tyto dvě možnosti se navzájem nevylučují a když je možné tooget podobné úlohy provádějí s jedním nebo jiné hello, se může také smysl pro vás tooset nahoru i. Ve většině případů kombinování agenta se v procesu kolekce by mohlo vést tooa spolehlivější monitorování pracovního postupu. Hello rozšíření diagnostiky Azure (agent) může být vaše vybrané cesty pro platformu úrovně protokoly při můžete použít EventFlow (v rámci procesu shromažďování) pro svoje protokoly úrovni aplikace. Jakmile budete mít započítáno se co nejlépe vám vyhovuje, je čas toothink o tom, jak chcete, aby vaše data toobe zobrazit a analyzovat.

## <a name="event-analysis"></a>Analýza události

Existuje několik skvělé platforem, které existují v hello trhu při přechodu do toohello analýzy a vizualizace dat monitorování a Diagnostika. Hello dva, doporučujeme jsou [OMS](service-fabric-diagnostics-event-analysis-oms.md) a [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) kvůli tootheir lepší integraci s Service Fabric, ale měli také viděl hello [elastické zásobníku](https://www.elastic.co/products) (obzvláště pokud uvažujete o spuštění clusteru v režimu offline), [Splunk](https://www.splunk.com/), nebo jakoukoli jinou platformu vaši volbu.

Hello klíčové body pro libovolnou platformu vyberete by měla obsahovat jak pohodlné jste s uživatelským rozhraním hello a dotazování možnosti hello možnost toovisualize dat a vytváření snadno čitelné řídicích panelů a hello další nástroje poskytují tooenhance vaší monitorování, jako je například automatizované výstrahy.

Kromě toohello platformu, kterou zvolíte, když vytváříte cluster Service Fabric jako prostředek služby Azure, můžete také získat přístup tooAzure out box monitorování pro počítače, které může být užitečná pro konkrétní výkonu a metriky monitorování.

### <a name="azure-monitor"></a>Azure Monitor

Můžete použít [Azure monitorování](../monitoring-and-diagnostics/monitoring-overview.md) toomonitor řadu hello prostředky Azure, na kterých je vytvořen cluster Service Fabric. Sadu metriky pro hello [škálovací sadu virtuálních počítačů](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) a jednotlivých [virtuální počítače](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) se automaticky shromažďovat a zobrazí v hello portálu Azure. tooview hello shromažďují informace v hello portál Azure, vyberte hello skupinu prostředků, která obsahuje clusteru Service Fabric hello. Potom vyberte hello škálovací sady virtuálních počítačů, které chcete tooview. V hello **monitorování** vyberte **metriky** tooview graf hello hodnot.

![Zobrazení portálu Azure shromažďovat informace o metrikách](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

toocustomize hello grafy, postupujte podle pokynů hello v [metriky v Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Také můžete vytvořit oznámení založené na tyto metriky, jak je popsáno v [vytvoření výstrahy v Azure monitorování pro služby Azure](../monitoring-and-diagnostics/insights-alerts-portal.md). Můžete odeslat upozornění tooa oznámení služby pomocí webové háky, jak je popsáno v [nakonfigurovat webové háku na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure monitorování podporuje pouze jedno předplatné. Pokud potřebujete toomonitor více předplatných, nebo pokud potřebujete další funkce, [analýzy protokolů](https://azure.microsoft.com/documentation/services/log-analytics/), součást nástroje Microsoft Operations Management Suite nabízí komplexní řešení pro správu IT pro místní i cloudové infrastruktura. Je možné směrovat dat z Azure monitorování přímo tooLog analýzy, abyste viděli, metriky a protokoly pro celé prostředí na jednom místě.

## <a name="next-steps"></a>Další kroky

### <a name="watchdogs"></a>Watchdogs

Sledovací zařízení je samostatný služba, která může sledovat stav a zatížení v rámci služeb a sestavy stavu pro všechno, co je v hierarchii modelu stavu hello. To může pomoct zabránit chybám, které by se na základě zobrazení hello jedné služby. Watchdogs jsou také vhodné místo toohost kód, který provede nápravné akce bez zásahu uživatele (například čištění souborů protokolů v úložiště v určitých časových intervalech). Můžete najít implementaci služby sledovací zařízení ukázka [zde](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Začínáme s pochopení, jak získat generovány události a protokoly v hello [platformy úroveň](service-fabric-diagnostics-event-generation-infra.md) a hello [úrovni aplikace](service-fabric-diagnostics-event-generation-app.md).
