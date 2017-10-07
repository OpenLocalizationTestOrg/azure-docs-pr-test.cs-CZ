---
title: "aaaAzure analýza události Service Fabric s OMS | Microsoft Docs"
description: "Další informace o vizualizaci a analýzu událostí pomocí OMS pro monitorování a Diagnostika Azure Service Fabric clusterů."
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
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 526519293e70982c95e31241465b87f190096f74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analýza události a vizualizace s OMS

Operations Management Suite (OMS) je kolekce služby správy, které pomáhají s monitorování a Diagnostika pro aplikace a služby hostované v cloudu hello. Přečtěte si tooget podrobnější přehled OMS a co nabízí, [co je OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-hello-oms-workspace"></a>Analýza a hello pracovním prostorem OMS protokolu

Analýzy protokolů shromažďuje data z spravované prostředky, včetně úložiště Azure table nebo agenta a udržuje v centrálním úložišti. Hello potom lze data používá pro analýzy, výstrahy a vizualizace nebo další export. Analýzy protokolů podporuje události, údaje o výkonu nebo jiná vlastní data.

Pokud je nakonfigurovaná OMS, budete mít přístup tooa konkrétní *pracovním prostorem OMS*, ze kterých můžete data dotaz nebo vizualizována ve řídicí panely.

Po přijetí dat podle analýzy protokolů OMS má několik *řešení pro správu* hotových řešení toomonitor příchozích dat, přizpůsobené tooseveral scénáře, které jsou. Mezi ně patří *Service Fabric Analytics* řešení a *kontejnery* řešení, které jsou hello dvě nejdůležitější ty toodiagnostics a monitorování při použití clusterů Service Fabric. Existuje několik ostatní, a které je vhodné využít a OMS také umožňuje hello vytváření vlastních řešení, které další informace o [zde](../operations-management-suite/operations-management-suite-solutions.md). Každé řešení, abyste zvolili toouse pro cluster nakonfiguruje v hello stejný pracovní prostor OMS, spolu s analýzy protokolů. Pracovní prostory umožňují pro vlastní řídicí panely a vizualizace dat a úpravy toohello data, která chcete toocollect, zpracovávat a analyzovat.

## <a name="setting-up-an-oms-workspace-with-hello-service-fabric-solution"></a>Nastavení pracovní prostor OMS s hello řešení prostředků infrastruktury služby

Je doporučeno, můžete zahrnout hello řešení prostředků infrastruktury služby vaším pracovním prostorem OMS, vzhledem k tomu, že poskytuje užitečné řídicí panel, zobrazuje hello různé příchozí kanály protokolů z platformy a aplikace úroveň hello a mít tooquery hello Service Fabric konkrétní protokoly. Tady je poměrně jednoduché řešení prostředků infrastruktury služby vzhled, s jedné aplikace nasazené na clusteru hello:

![OMS SF řešení](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Existují dva způsoby tooprovision a nakonfigurovat pracovním prostorem OMS prostřednictvím šablony Resource Manageru nebo přímo z Azure Marketplace. Hello dřívějším použijte, pokud nasazujete cluster a hello druhé, pokud již máte nasazení s diagnostikou clusteru povolena.

### <a name="deploying-oms-using-a-resource-management-template"></a>Nasazení OMS pomocí šablony Správa prostředků

K tomu dojde při fázi vytváření clusteru hello – při nasazení clusteru pomocí šablony Resource Manageru, hello šablony můžete také vytvořit nový pracovní prostor OMS, přidejte hello řešení prostředků infrastruktury služby tooit a nakonfigurujte ji tooread data z příslušné úložiště hello tabulky.

>[!NOTE]
>Pro tento toowork diagnostiky má toobe povolit, aby pro tooexist tabulky hello úložiště Azure pro OMS / Log Analytics tooread informace v z.

[Zde](https://azure.microsoft.com/resources/templates/service-fabric-oms/) je ukázka šablony, která můžete použít a změnit podle požadavků, který provádí výše akce. V případě hello, že chcete další volitelnost, jsou k dispozici několik další šablony, které poskytují různé možnosti v závislosti na tom, kde v procesu hello je možné nastavení pracovním prostorem OMS – najdete na [Service Fabric a OMS šablony](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Nasazení pomocí OMS prostřednictvím Azure Marketplace

Pokud dáváte přednost tooadd pracovním prostorem OMS po nasazení clusteru s podporou, zamiřte tooAzure Marketplace a vyhledejte *"Service Fabric Analytics"*. Měla by existovat pouze jeden prostředek, který se zobrazí, v rámci kategorie "Správa monitorování +" hello, vidíte níže:

![Analýza SF OMS v Marketplace.](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

Kliknutím na tlačítko **vytvořit** zobrazí výzvu k pracovním prostorem OMS. Klikněte na tlačítko **vyberte pracovní prostor** a potom **vytvořit nový pracovní prostor**. Vyplňte hello požadované položky – hello zde Jediným požadavkem je, že hello hello předplatné pro hello Service Fabric clusteru a hello pracovním prostorem OMS by měla být stejné. Po ověření vaše záznamy vaším pracovním prostorem OMS nasadí za pár minut. Při dokončení nasazení hello vytvoření hello Service Fabric řešení okna stále zůstane otevřené. Ujistěte se, který hello stejné ukazuje prostoru si v části *pracovním prostorem OMS* a počtu **vytvořit** hello dole na tooadd hello Service Fabric řešení toohello prostoru.

## <a name="using-hello-oms-agent"></a>Pomocí hello agenta OMS

Je doporučeno toouse EventFlow WAD jako řešení agregace, protože umožňují pro více modulární toodiagnostics přístup a monitorování. Například pokud chcete toochange vaše výstupy z EventFlow, vyžaduje žádná změna tooyour skutečné instrumentace, právě soubor jednoduchých úprav tooyour konfigurace. Pokud však rozhodnout tooinvest pomocí OMS a se toocontinue pomocí pro analýzu událostí (nemá toobe hello jen platformy, můžete použít, ale to spíše bude alespoň jedna z platformy hello), doporučujeme, abyste prozkoumávání nastavení hello [</C0>agentaOMS<spanclass="notranslate">](../log-analytics/log-analytics-windows-agents.md).</span> Byste měli použít také hello OMS agent při nasazování clusteru tooyour kontejnery, jak je popsáno níže.

proces Hello tohoto postupu je poměrně jednoduché, vzhledem k tomu, že právě máte tooadd hello agenta jako sady škálování virtuálních počítačů šablony Resource Manageru tooyour rozšíření, zajistíte, že získá nainstalované na všech uzlů. Ukázkové šablonu Resource Manager, která nasadí hello pracovní prostor OMS s hello Service Fabric řešení (jak je uvedeno výše) a přidá hello agenta tooyour uzly pro clustery se systémem nalezen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) nebo [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

výhod Hello jsou hello následující:

* Data širší na straně čítače a metriky výkonu hello
* Snadno tooconfigure dat shromažďovaných od hello clusteru a proveďte změny tooit bez opětovného nasazení aplikace, nebo hello clusteru, protože změny nastavení toohello hello agenta se dá udělat z pracovní prostor OMS hello a bude právě resetovat hello agenta automaticky. tooconfigure hello OMS agenta toopick až konkrétních čítačích výkonu, přejděte toohello prostoru **Domů > Nastavení > Data > čítačů výkonu systému Windows** a vyberte chcete toosee shromažďovaných dat hello
* Data rychleji, než ho s toobe uložené před se zachyceny OMS objeví / analýzy protokolů
* Monitorování kontejnery je mnohem jednodušší, protože rozpoznal docker protokoly (stdout, stderror) a statistiky (metriky výkonu na kontejner a uzel úrovně)

Hello zde hlavní aspektem je, že vzhledem k tomu, že je agent, bude nasazen v clusteru společně se vaše aplikace, tak budou některé minimálním dopadem toohello výkon aplikací v clusteru hello.

## <a name="monitoring-containers"></a>Monitorování kontejnery

Při nasazování clusteru Service Fabric tooa kontejnery, doporučujeme, aby hello clusteru byl nastaven s agentem OMS hello a že hello kontejnery řešení byla přidaná tooyour OMS prostoru tooenable monitorování a Diagnostika. Tady je co hello kontejnery řešení vypadá v pracovním prostoru:

![Řídicí panel základní OMS](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Hello agenta umožňuje hello kolekce několik protokolů specifické pro kontejner, které může být dotazována v OMS, nebo použít toovisualized ukazatele výkonu. typy Hello protokolu, které byly shromážděny jsou:

* ContainerInventory: obsahuje informace o umístění kontejneru, název a obrázků
* ContainerImageInventory: informace o nasazené bitové kopie, včetně ID nebo velikosti
* ContainerLog: specifické chybové protokoly, protokoly docker (stdout atd.) a ostatní položky
* ContainerServiceLog: docker démon příkazy, které byly spuštěny
* Výkonu: čítače včetně kontejneru procesoru, paměti, síťového provozu v diskových operací, a vlastní metriky z hello hostiteli počítačů

Tento článek se zabývá hello kroky požadované tooset až monitorování pro váš cluster kontejneru. toolearn Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

tooset až hello kontejneru řešení v pracovním prostoru, ujistěte se, že máte agenta OMS hello nasazené na uzly clusteru na postupem hello uvedených výše. Jakmile hello clusteru je připraven, nasaďte tooit kontejneru. Opatřeny si, že hello poprvé bitovou kopii kontejneru je nasazené tooa clusteru, bude trvat několik minut toodownload hello image v závislosti na jeho velikosti.

V Azure Marketplace vyhledejte *kontejnery* a vytvořte prostředek kontejnery (v části hello monitorování + správu kategorie).

![Přidání kontejnery řešení](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

V kroku vytváření hello požaduje pracovním prostorem OMS. Vyberte hello ten, který byl vytvořen s nasazením hello výše. Tento krok přidává řešení kontejnery v rámci pracovní prostor OMS a je automaticky zjišťován agentem OMS hello nasazené šablonou hello. Hello agent začne shromažďování dat na hello kontejnery procesů v clusteru hello a méně než 15 minut nebo Ano, měli byste vidět hello řešení světla až s daty, jako obrázek hello řídicí panel hello výše.


## <a name="next-steps"></a>Další kroky

Prozkoumejte hello následující OMS nástroje a možnosti toocustomize tooyour prostoru musí:

* Pro místní clusterů nabízí OMS brány (dál server Proxy protokolu HTTP), které můžou být použité toosend data tooOMS. Další informace o v [propojení počítačů bez tooOMS přístupu k Internetu pomocí brány OMS hello](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurace OMS tooset až [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) tooaid v zjišťování a Diagnostika
* Získat familiarized s hello [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů
