---
title: "aaaAzure Service Fabric kontejnery monitorování a Diagnostika | Microsoft Docs"
description: "Zjistěte, jak toomonitor a diagnostikovat kontejnery orchestrovat na Microsoft Azure Service Fabric na OMS kontejnery řešení."
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
ms.date: 05/10/2017
ms.author: dekapur
ms.openlocfilehash: cd79111cf78b9d76a60d489bb9953587aa06186d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Monitorování systému Windows Server kontejnery s OMS

## <a name="oms-containers-solution"></a>Řešení kontejnery OMS

tým služby Operations Management Suite (OMS) Hello publikovali kontejnery řešení pro diagnostiku a monitorování pro kontejnery. Spolu s jejich Service Fabric řešení je toto řešení nasazení kontejnerů skvělý nástroj toomonitor orchestrovat v Service Fabric. Tady je jednoduchý příklad jaké hello řídicího panelu řešení hello vypadá takto:

![Řídicí panel základní OMS](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Shromažďuje taky různé druhy protokoly, které může být dotazována v nástroj pro analýzu protokolu OMS hello, a může být použité toovisualize všechny metriky nebo události generován. typy Hello protokolu, které byly shromážděny jsou:

1. ContainerInventory: obsahuje informace o umístění kontejneru, název a obrázků
2. ContainerImageInventory: informace o nasazené bitové kopie, včetně ID nebo velikosti
3. ContainerLog: specifické chybové protokoly, protokoly docker (stdout atd.) a ostatní položky
4. ContainerServiceLog: docker démon příkazy, které byly spuštěny
5. Výkonu: čítače včetně kontejneru procesoru, paměti, síťového provozu v diskových operací, a vlastní metriky z hello hostiteli počítačů

Tento článek se zabývá hello kroky požadované tooset až monitorování pro váš cluster kontejneru. toolearn Další informace o řešení kontejnery na OMS, podívejte se na jejich [dokumentaci](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Nastavení clusteru Service Fabric

Vytvoření clusteru s podporou pomocí šablony Azure Resource Manager hello najít [zde](https://github.com/dkkapur/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample). Ujistěte se, že tooadd jedinečný název pracovní prostor OMS. Tato šablona také výchozí toodeploying cluster ve verzi preview hello sestavení Service Fabric (v255.255), což znamená, že nelze použít v produkčním prostředí a nemůže být upgradovány tooa jinou verzi Service Fabric. Pokud se rozhodnete toouse této šablony pro dlouhodobé nebo produkční použít, změňte číslo hello verze tooa stabilní verze.

Až nastavíte hello clusteru potvrďte nainstalovali hello příslušný certifikát a ujistěte se, že jsou možné tooconnect toohello clusteru.

Potvrďte, že je vaší skupiny prostředků pomocí záhlaví toohello správně nastavit [portál Azure](https://portal.azure.com/) a hledání hello nasazení. Skupina prostředků Hello by měl obsahovat všechny prostředky hello Service Fabric a také mít řešení Log Analytics, jakož i řešení Service Fabric.

Úpravy stávajícího clusteru Service Fabric:
* Zkontrolujte, zda je povoleno diagnostiky (Pokud ne, povolit pomocí [aktualizace škálovací sadu virtuálních počítačů hello](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Přidat pracovním prostorem OMS vytvořením "Service Fabric Analytics" řešení prostřednictvím hello Azure Marketplace
* Upravit zdroje dat hello hello Service Fabric řešení toopick zálohovat data z hello příslušné tabulky služby Azure Storage (nastavit tak, že WAD) v hello skupinu prostředků, která hello clusteru je v
* Přidat agenta hello jako [škálovací sadu virtuálních počítačů toohello rozšíření](/powershell/module/azurerm.compute/add-azurermvmssextension) prostřednictvím prostředí PowerShell nebo prostřednictvím aktualizace hello sada škálování virtuálních počítačů (stejného propojení, jak je uvedeno výše, šablony Resource Manageru hello toomodify)

## <a name="2-deploy-a-container"></a>2. Nasazení kontejneru

Jakmile hello clusteru je připravené a ověříte, že budete mít přístup, nasaďte tooit kontejneru. Pokud jste zvolili verze preview hello toouse jako sada hello šablony, můžete si také prostudovat nové docker Service Fabric tvoří funkce. Opatřeny si, že hello poprvé bitovou kopii kontejneru je nasazené tooa clusteru, bude trvat několik minut toodownload hello image v závislosti na jeho velikosti.

## <a name="3-add-hello-containers-solution"></a>3. Přidat řešení kontejnery hello

V hello portálu Azure, vytvořte prostředek kontejnery (v části hello monitorování + správu kategorie) prostřednictvím Azure Marketplace. 

![Přidání kontejnery řešení](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

V kroku vytváření hello požaduje pracovním prostorem OMS. Vyberte hello ten, který byl vytvořen s nasazením hello výše. Tento krok přidává řešení kontejnery v rámci pracovní prostor OMS a je automaticky zjišťován agentem OMS hello nasazené šablonou hello. Hello agent začne shromažďování dat na hello kontejnery procesů v clusteru hello a v asi 10 až 15 minut, měli byste vidět hello řešení světla až s daty jako obrázek hello řídicí panel hello výše.

## <a name="4-next-steps"></a>4. Další kroky

OMS nabízí různé nástroje v prostoru toomake hello Pokud užitečnější pro vás. Prozkoumejte hello následující možnosti toocustomize hello řešení tooyour potřeb:
- Získat familiarized s hello [vyhledávání a dotazování protokolu](../log-analytics/log-analytics-log-searches.md) funkcím poskytovaným jako součást analýzy protokolů
- Konfigurace toopick agenta OMS hello až konkrétních čítačích výkonu (přejděte toohello prostoru domovské > Nastavení > Data > čítačů výkonu systému Windows)
- Konfigurace OMS tooset až [automatizované výstrahy](../log-analytics/log-analytics-alerts.md) tooaid pravidla v zjišťování a Diagnostika
