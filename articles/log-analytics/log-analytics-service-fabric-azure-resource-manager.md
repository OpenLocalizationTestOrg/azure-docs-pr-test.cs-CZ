---
title: "aaaAssess aplikace Service Fabric pomocí analýzy protokolů hello portálu Azure | Microsoft Docs"
description: "V analýzy protokolů pomocí hello Azure portálu tooassess hello riziko a stavu aplikace Service Fabric, můžete použít Service Fabric řešení hello micro-services, uzly a clustery."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a>Hodnocení aplikace Service Fabric a micro-services s hello portálu Azure

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Symbol Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Tento článek popisuje, jak toouse hello řešení Service Fabric v toohelp Log Analytics identifikovat a řešit problémy mezi cluster Service Fabric.

Hello Service Fabric řešení používá Azure Diagnostics data z virtuálních počítačů služby infrastruktury, shromažďováním těchto dat z Azure WAD tabulek. Analýzy protokolů potom načte události framework Service Fabric, včetně **spolehlivé události služby**, **události objektu Actor**, **provozní události**, a **vlastní Události trasování událostí**. S hello řídicí panel řešení jsou možné tooview významné problémy a relevantní události ve vašem prostředí Service Fabric.

tooget začít s hello řešení, je nutné tooconnect pracovní prostor analýzy protokolů tooa clusteru Service Fabric. Tady jsou tři tooconsider scénáře:

1. Pokud jste nenasadili cluster Service Fabric, použijte kroky hello v ***nasadit pracovní prostor analýzy protokolů Cluster Service Fabric připojené tooa*** toodeploy do nového clusteru a když jste nakonfigurovali tooreport tooLog Analytics.
2. Pokud potřebujete toocollect čítače výkonu z vaší hostitelů toouse jiných OMS řešení, jako je například zabezpečení na váš Cluster Service Fabric, postupujte podle kroků hello v ***nasadit Cluster Service Fabric připojené tooa pracovní prostor analýzy protokolů pomocí rozšíření virtuálního počítače nainstalovat.***
3. Pokud jste už jste nasadili cluster Service Fabric a chcete tooconnect ho tooLog analýzy, postupujte podle kroků hello v ***přidání existující účet úložiště tooLog Analytics.***

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a>Nasazení pracovní prostor analýzy protokolů tooa Cluster Service Fabric připojen.
Tato šablona hello následující:

1. Pracovní prostor analýzy protokolů služby Azure Service Fabric clusteru už připojená tooa nasadí. Možnost toocreate hello máte při nasazování šablony hello nebo vstupní hello název již existující pracovní prostor analýzy protokolů nový pracovní prostor.
2. Přidá hello diagnostické úložiště účet toohello pracovní prostor analýzy protokolů.
3. Umožňuje řešení hello Service Fabric v pracovním prostoru analýzy protokolů.

[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Jakmile vyberete hello nasazení výše uvedené tlačítko, hello Azure otevře portál s parametry, můžete tooedit. Být jisti toocreate novou skupinu prostředků, pokud je zadat nový název pracovního prostoru analýzy protokolů:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Přijměte právní podmínky hello a klikněte na **vytvořit** toostart hello nasazení. Po dokončení nasazení hello by měl vidět hello nový pracovní prostor a vytvoření clusteru a hello WADServiceFabric * událostí, WADWindowsEventLogs a WADETWEvent tabulek, které jsou přidány:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a>Pracovní prostor analýzy protokolů Cluster Service Fabric připojené tooa nasaďte s nainstalovat rozšíření virtuálního počítače.

Tato šablona hello následující:

1. Pracovní prostor analýzy protokolů služby Azure Service Fabric clusteru už připojená tooa nasadí. Můžete vytvořit nový pracovní prostor nebo použijte existující.
2. Přidá hello pracovní prostor analýzy protokolů toohello diagnostické úložiště účtů.
3. Umožňuje řešení hello Service Fabric v pracovní prostor analýzy protokolů hello.
4. Nainstaluje rozšíření agenta MMA hello v jednotlivých sad v clusteru Service Fabric škálování virtuálního počítače. Instalaci agenta MMA hello jsou metriky výkonu možné tooview o uzly.

[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Následující text hello stejný postup uvedený výš vstupní hello potřebné parametry a ji nasazení. Měli byste vidět znovu WAD tabulek všechny vytvořené, clusteru a nový pracovní prostor hello:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Zobrazení dat výkonu

tooview dat výkonu z uzlů:


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Spusťte pracovní prostor analýzy protokolů hello z hello portálu Azure.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Přejděte v levém podokně hello tooSettings a vyberte Data >> čítačů výkonu systému Windows >> "hello přidat vybrané čítače výkonu": ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- V hledání protokolů použijte následující dotazy toodelve do klíčových metrik týkajících se uzly hello:

    a. Porovnání hello průměrné využití procesoru pro všechny uzly v hello poslední hodinu toosee uzlů, které jsou problémy s a v jaké časovém intervalu uzlu měl Špička:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Zobrazit podobné spojnicových grafů pro dostupná paměť na každý uzel k tomuto dotazu:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    tooview seznam všech uzlech, zobrazující hello přesný průměrnou hodnotu pro počet MB k dispozici pro každý uzel, použijte tento dotaz:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. V případě hello má toodrill dolů na konkrétním uzlu tak, že prověří hello hodinové průměr minimální, maximální a 75 percentilu využití procesoru, budete moct toodo tím, že pomocí tohoto dotazu (nahraďte pole počítače):

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Další informace o metriky výkonu v analýzy protokolů hello [Operations Management Suite blog](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-toolog-analytics"></a>Přidání existující účet úložiště tooLog Analytics

Tato šablona jednoduše přidá vaše stávající úložiště účtů tooa nový nebo existující pracovní prostor analýzy protokolů.

[![Nasazení tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Výběr skupiny prostředků, pokud pracujete s již existující pracovní prostor analýzy protokolů, vyberte "Použití existující" a vyhledejte hello skupinu prostředků obsahující pracovní prostor analýzy protokolů hello. Vytvořit nový jeden Pokud jinak.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Po nasazení této šablony, bude možné toosee hello úložiště účet připojené tooyour pracovní prostor analýzy protokolů. V tomto případě přidané jeden další úložiště účet toohello Exchange prostoru nově vytvořené výše.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Zobrazit události Service Fabric

Po dokončení nasazení hello a povolil hello řešení Service Fabric v pracovním prostoru, vybrat hello **Service Fabric** dlaždici na řídicím panelu hello analýzy protokolů portálu toolaunch hello Service Fabric. řídicí panel Hello obsahuje sloupce hello hello následující tabulka. Každý sloupec uvádí hello top 10 události pomocí počet odpovídajících, že kritéria sloupce pro hello zadaný časový rozsah. Můžete spustit hledání protokolu, které poskytuje celý seznam hello kliknutím **zobrazit všechny** v pravém dolním hello jednotlivých sloupců, nebo klikněte na záhlaví sloupce hello.

| **Události Service Fabric** | **Popis** |
| --- | --- |
| Významné problémy |Zobrazí problémy, jako je RunAsyncFailures RunAsynCancellations a seznamy uzlu. |
| Provozní události |Významné provozní události, jako je například upgradu aplikace a nasazení. |
| Události spolehlivé služby |Události významné spolehlivé služby takové Runasyncinvocations. |
| Události objektu actor |Významné objektu actor události vygenerované modulem vaší micro služby, jako je výjimky vyvolané metodu objektu actor, objektu actor aktivací a deaktivací a tak dále. |
| Události aplikace |Všechny vlastní události trasování událostí generovaných aplikací. |

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Řídicí panel Service Fabric](./media/log-analytics-service-fabric/sf4.png)

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro Service Fabric.

| Platforma | Přímé agenta | Agent nástroje Operations Manager | Azure Storage | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 minut |

> [!NOTE]
> Kliknutím můžete změnit rozsah hello z těchto událostí v hello Service Fabric řešení **dat podle posledních 7 dnů** hello horní části řídicího panelu hello. Můžete také zobrazit události generované v rámci hello posledních sedmi dnů, jeden den nebo šest hodin. Nebo můžete vybrat **vlastní** toospecify vlastní období.
>
>

## <a name="next-steps"></a>Další kroky

* Použití [protokolu hledání v analýzy protokolů](log-analytics-log-searches.md) tooview podrobná data události Service Fabric.
