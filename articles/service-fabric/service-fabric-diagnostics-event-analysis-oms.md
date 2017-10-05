---
title: "Analýza události služby Azure Service Fabric s OMS | Microsoft Docs"
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
ms.openlocfilehash: 425c7a733a0a2383f01d2122e7155d3e3a9071be
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analýza události a vizualizace s OMS

Operations Management Suite (OMS) je kolekce služby správy, které pomáhají s monitorování a Diagnostika pro aplikace a služby hostované v cloudu. Chcete-li získat podrobnější přehled OMS a co nabízí, přečtěte si [co je OMS?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Analýzy protokolů a pracovním prostorem OMS

Analýzy protokolů shromažďuje data z spravované prostředky, včetně úložiště Azure table nebo agenta a udržuje v centrálním úložišti. Data lze poté použít pro analýzy, výstrahy a vizualizace nebo další export. Analýzy protokolů podporuje události, údaje o výkonu nebo jiná vlastní data.

Pokud je nakonfigurovaná OMS, budete mít přístup na konkrétní *pracovním prostorem OMS*, ze kterých můžete data dotaz nebo vizualizována ve řídicí panely.

Po přijetí dat podle analýzy protokolů OMS má několik *řešení pro správu* hotových řešení ke sledování příchozích dat, upravit tak, aby několik scénářů, které jsou. Mezi ně patří *Service Fabric Analytics* řešení a *kontejnery* řešení, které jsou dvě nejdůležitější ty diagnostiky a monitorování při použití clusterů Service Fabric. Existuje několik ostatní, a které je vhodné využít a OMS také umožňuje používat pro vytváření vlastních řešení, které další informace o [zde](../operations-management-suite/operations-management-suite-solutions.md). Každé řešení, které chcete použít pro cluster s podporou budou nakonfigurovány ve stejném pracovním prostorem OMS spolu s analýzy protokolů. Pracovní prostory umožňují vlastní řídicí panely a vizualizace dat a změny, které chcete shromažďovat, proces a analyzovat data.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-solution"></a>Nastavení pracovní prostor OMS s řešením Service Fabric

Doporučuje se zahrnout řešení prostředků infrastruktury služby do pracovního prostoru OMS vzhledem k tomu, že poskytuje užitečné řídicí panel, který ukazuje různé příchozí protokolu kanály z platforem a úrovni aplikace a možné do určitých protokolů a dotaz Service Fabric. Tady je poměrně jednoduché řešení prostředků infrastruktury služby vzhled, s jedné aplikace nasazené na clusteru:

![OMS SF řešení](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Existují dva způsoby, jak zřídit a nakonfigurovat pracovním prostorem OMS prostřednictvím šablony Resource Manageru nebo přímo z Azure Marketplace. Používejte první, když nasazujete cluster a pozdější Pokud již máte nasazení s diagnostikou clusteru povolena.

### <a name="deploying-oms-using-a-resource-management-template"></a>Nasazení OMS pomocí šablony Správa prostředků

To se stane, že ve fázi vytváření clusteru – při nasazování clusteru pomocí šablony Resource Manageru, řešení prostředků infrastruktury služby šablony můžete také vytvořit nový pracovní prostor OMS, přidejte do ní a nakonfigurujte ji číst data z tabulky odpovídající úložiště.

>[!NOTE]
>Pro tento postup musí být povoleno v pořadí pro úložiště Azure tabulky existovat pro OMS diagnostiky nebo pro čtení informací v z protokolu analýzy.

[Zde](https://azure.microsoft.com/resources/templates/service-fabric-oms/) je ukázka šablony, která můžete použít a změnit podle požadavků, který provádí výše akce. V případě, že chcete další volitelnost, jsou k dispozici několik další šablony, které poskytují různé možnosti v závislosti na tom, kde v procesu je možné nastavení pracovním prostorem OMS – najdete na [Service Fabric a OMS šablony](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

### <a name="deploying-oms-using-through-azure-marketplace"></a>Nasazení pomocí OMS prostřednictvím Azure Marketplace

Pokud chcete přidat pracovním prostorem OMS po nasazení clusteru, přejděte na Azure Marketplace a vyhledejte *"Service Fabric Analytics"*. Měla by existovat pouze jeden prostředek, který se zobrazí, v kategorii "Správa monitorování +", vidíte níže:

![Analýza SF OMS v Marketplace.](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

Kliknutím na tlačítko **vytvořit** zobrazí výzvu k pracovním prostorem OMS. Klikněte na tlačítko **vyberte pracovní prostor** a potom **vytvořit nový pracovní prostor**. Vyplnit požadované položky – tady Jediným požadavkem je, že předplatné pro cluster Service Fabric a pracovním prostorem OMS musí být stejná. Po ověření vaše záznamy vaším pracovním prostorem OMS nasadí za pár minut. Vytvoření okna Service Fabric řešení bude stále otevřete při dokončení nasazení. Ujistěte se, že ve stejném pracovním prostoru se zobrazí v části *pracovním prostorem OMS* a počtu **vytvořit** v dolní části, chcete-li přidat řešení Service Fabric do pracovního prostoru.

## <a name="using-the-oms-agent"></a>Pomocí agenta OMS

Je doporučené použít EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární přístup k diagnostiky a monitorování. Například pokud chcete změnit vaše výstupy z EventFlow, vyžaduje nijak nemění skutečné instrumentace, právě jednoduchých úprav konfiguračního souboru. Pokud, ale můžete rozhodnout investovat do pomocí OMS a chcete-li pokračovat v používání pro analýzu událostí (nemusí být jediným platformy, můžete použít, ale to spíše bude alespoň jeden z platformy), doporučujeme, abyste se seznámili nastavení [OMS ag trola](../log-analytics/log-analytics-windows-agents.md). Byste měli použít také OMS agent při nasazení kontejnerů do clusteru, jak je popsáno níže.

Proces tohoto postupu je poměrně jednoduché, protože právě musíte přidat agenta škálování virtuálního počítače nastavit rozšíření do šablony Resource Manageru jako zajistíte, že získá nainstalované na všech uzlů. Ukázka šablonu Resource Manager, která nasadí pracovní prostor OMS s řešením Service Fabric (jak je uvedeno výše) a přidá agenta do uzlů pro clustery se systémem nalezen [Windows](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows) nebo [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Výhody tohoto jsou následující:

* Data širší na straně čítače a metriky výkonu
* Snadno konfigurovat dat shromažďovaných z clusteru a provést změny bez opětovného nasazení aplikace nebo clusteru, protože změny v nastavení agenta lze provést v pracovním prostoru OMS a bude právě agenta automaticky resetovat. Konfigurace agenta OMS na vyzvednutí konkrétních čítačích výkonu, přejděte do pracovního prostoru **Domů > Nastavení > Data > čítačů výkonu systému Windows** a vyberte chcete najdete v části shromážděných dat
* Data rychleji, než ho museli být uloženy před se zachyceny OMS objeví / analýzy protokolů
* Monitorování kontejnery je mnohem jednodušší, protože rozpoznal docker protokoly (stdout, stderror) a statistiky (metriky výkonu na kontejner a uzel úrovně)

Hlavní význam tady je, že vzhledem k tomu, že je agent, bude nasazen v clusteru společně se vaše aplikace, tak budou některé minimálním dopadem na výkon vaší aplikace v clusteru.

## <a name="monitoring-containers"></a>Monitorování kontejnery

Při nasazení kontejnerů do clusteru Service Fabric, doporučujeme, aby clusteru byla nastavena s agentem OMS a že kontejnery řešení byl přidán k vašim pracovním prostorem OMS povolení monitorování a Diagnostika. Zde je, jak řešení kontejnery vypadá v pracovním prostoru:

![Řídicí panel základní OMS](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Agenta umožňuje kolekce několik specifické pro kontejner protokoly, které mohou být dotazována v OMS, nebo použít k ukazatele vizualizovaných výkonu. Typy protokolu, které byly shromážděny jsou:

* ContainerInventory: obsahuje informace o umístění kontejneru, název a obrázků
* ContainerImageInventory: informace o nasazené bitové kopie, včetně ID nebo velikosti
* ContainerLog: specifické chybové protokoly, protokoly docker (stdout atd.) a ostatní položky
* ContainerServiceLog: docker démon příkazy, které byly spuštěny
* Výkonu: čítače včetně kontejneru vstupně-výstupních operací a vlastní metriky z hostitelských počítačích disku procesoru, paměti, síťového provozu,

Tento článek popisuje kroky potřebné k nastavení kontejneru monitorování pro váš cluster. Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

Nastavit řešení kontejneru v pracovním prostoru, ujistěte se, že máte agenta OMS nasazené na uzly clusteru na podle kroků uvedených výše. Jakmile clusteru je připraven, nasaďte kontejner k němu. Berte v úvahu, že při prvním nasazení kontejneru image do clusteru, ho trvá několik minut stáhnout bitovou kopii v závislosti na jeho velikosti.

V Azure Marketplace vyhledejte *kontejnery* a vytvořte prostředek kontejnery (v oddíle monitorování a správu kategorie).

![Přidání kontejnery řešení](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

V kroku vytváření požaduje pracovním prostorem OMS. Vyberte ten, který byl vytvořen s nasazením výše. Tento krok přidává řešení kontejnery v rámci pracovní prostor OMS a je automaticky zjišťován agentem OMS nasazení šablony. Agent začne shromažďování dat na kontejnery procesů v clusteru a méně než 15 minut nebo Ano, měli byste vidět světla až s daty, jako obrázek na řídicím panelu výše uvedené řešení.


## <a name="next-steps"></a>Další kroky

Prozkoumejte následující OMS nástrojů a možností pro přizpůsobení pracovního prostoru vašim potřebám:

* Pro místní clusterů nabízí OMS brány (dál server Proxy protokolu HTTP), který slouží k odesílání dat do OMS. Další informace o v [propojíte počítače bez připojení k Internetu pomocí brány OMS OMS](../log-analytics/log-analytics-oms-gateway.md)
* Konfigurace OMS nastavit [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) které pomáhají při zjišťování a Diagnostika
* Získat familiarized s [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů