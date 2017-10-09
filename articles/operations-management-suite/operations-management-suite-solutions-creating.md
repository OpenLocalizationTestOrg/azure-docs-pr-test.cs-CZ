---
title: "aaaBuild řešení pro správu v OMS | Microsoft Docs"
description: "Řešení pro správu rozšíření hello funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů správy zabalené, aby zákazníci můžete přidat pracovní prostor OMS tootheir.  Tento článek obsahuje informace o tom, jak můžete vytvořit toobe řešení správy použít ve svém vlastním prostředí nebo provedené dostupné tooyour zákazníků."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dea4c0d9e608d9fe4aa41088705958c9fe999372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-build-a-management-solution-in-operations-management-suite-oms-preview"></a>Návrh a vytvoření řešení správy v Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.

[Řešení pro správu](operations-management-suite-solutions.md) rozšířit hello funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů správy zabalené, aby zákazníci můžete přidat pracovní prostor OMS tootheir.  Tento článek představuje základní proces toodesign a vytvoření řešení správy, který je vhodný pro nejběžnější požadavky.  Pokud jste nové řešení pro správu toobuilding můžete tento proces použijte jako výchozí bod a pak využít hello koncepty pro složitější řešení jako momentální vašim požadavkům.

## <a name="what-is-a-management-solution"></a>Co je řešení pro správu?

Řešení pro správu obsahovat OMS a prostředky Azure, které vzájemně spolupracují tooachieve určitého scénáře monitorování.  Jsou implementovány jako [Správa prostředků šablony](../azure-resource-manager/resource-manager-template-walkthrough.md) obsahující podrobnosti o tom, tooinstall a nakonfigurovat jejich obsažených prostředků při instalaci hello řešení.

Hello základní strategie je toostart řešení pro správu podle budovy hello jednotlivých součástí v prostředí Azure.  Jakmile máte hello funkce funguje správně, potom můžete spustit do balení [soubor řešení správy](operations-management-suite-solutions-solution-file.md). 


## <a name="design-your-solution"></a>Navrhněte svoje řešení
hello následující diagram ukazuje Hello Nejběžnější vzor jako řešení pro správu.  Hello různé součásti v tomto vzoru se zabývá hello níže.

![Přehled řešení OMS](media/operations-management-suite-solutions/solution-overview.png)


### <a name="data-sources"></a>Zdroje dat
Hello prvním krokem při navrhování řešení je určení hello data, která vyžadují z úložiště analýzy protokolů hello.  Tato data můžou shromáždit pomocí [zdroj dat](../log-analytics/log-analytics-data-sources.md) nebo [jiné řešení](operations-management-suite-solutions.md), nebo řešení může být nutné tooprovide hello proces toocollect ho.

Existuje několik způsobů zdrojů dat, které můžou shromažďovat hello úložiště analýzy protokolů, jak je popsáno v [zdroje dat v analýzy protokolů](../log-analytics/log-analytics-data-sources.md).  To zahrnuje události v protokolu událostí systému Windows hello nebo generované Syslog kromě tooperformance čítače pro klienty systému Windows a Linux.  Můžete také shromažďovat data z prostředků Azure, které shromažďují Azure monitorování.  

Pokud požadujete data, která není přístupná prostřednictvím libovolného hello dostupné zdroje dat, pak můžete použít hello [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md) který vám umožní úložiště analýzy protokolů toohello toowrite data z libovolného klienta, který můžete volat REST ROZHRANÍ API.  Hello nejběžnější znamená kolekce vlastních dat v rámci řešení pro správu je toocreate [runbook ve službě Azure Automation](../automation/automation-runbook-types.md) který shromažďuje hello vyžaduje data z Azure nebo externí prostředky a používá hello toowrite rozhraní API sady kolekcí dat toohello úložiště.  

### <a name="log-searches"></a>Protokol hledání
[Přihlaste se hledání](../log-analytics/log-analytics-log-searches.md) jsou použité tooextract a analyzovat data v úložišti analýzy protokolů hello.  Používají se v zobrazeních a výstrahy v přidání tooallowing hello uživatele tooperform ad hoc analýzy dat v úložišti hello.  

Měli byste všechny dotazy, které si myslíte, že budou užitečné toohello uživatele, i když nejsou použity žádné zobrazení nebo výstrahy.  Tyto budou k dispozici toothem jako uložená hledání hello portálu, a může zahrnovat i je [seznamu dotazy vizualizace část](../log-analytics/log-analytics-view-designer-parts.md#list-of-queries-part) ve vaší vlastní zobrazení.

### <a name="alerts"></a>Výstrahy
[Výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md) identifikovat problémy prostřednictvím [protokolu hledání](#log-searches) proti hello dat v úložišti hello.  Jsou buď oznámit hello uživateli nebo automaticky spustit akci v odpovědi. Abyste uvedli jinou výstrahy podmínky pro aplikaci a zahrnout do souboru řešení odpovídající pravidla výstrah.

Pokud hello problém lze napravit potenciálně s automatizovaného procesu, poté obvykle vytvoříte sadu runbook v Azure Automation tooperform tento nápravy.  Většina Azure services lze spravovat pomocí [rutiny](/powershell/azure/overview) které hello runbook by využívat tooperform takové funkce.

Pokud vaše řešení vyžaduje externí funkce v odpovědi tooan výstraha, pak můžete použít [webhooku odpovědi](../log-analytics/log-analytics-alerts-actions.md).  To vám umožní toocall posílejte informace o výstraze hello externí webovou službu.

### <a name="views"></a>Zobrazení
Zobrazení analýzy protokolů jsou použité toovisualize data z úložiště analýzy protokolů hello.  Každé řešení bude obvykle obsahovat jediné zobrazení s [dlaždici](../log-analytics/log-analytics-view-designer-tiles.md) , se zobrazí na hlavním řídicím hello uživatele.  zobrazení Hello může obsahovat libovolný počet [vizualizace částí](../log-analytics/log-analytics-view-designer-parts.md) tooprovide různé vizualizace hello shromážděná data toohello uživatele.

Můžete [vytvářet vlastní zobrazení pomocí hello Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) který můžete později exportovat zahrnuty v souboru řešení.  


## <a name="create-solution-file"></a>Vytvořit soubor řešení
Po nakonfigurování a testované hello součásti, které budou součástí vašeho řešení, můžete [vytvořte svůj soubor řešení](operations-management-suite-solutions-solution-file.md).  Budete implementovat hello řešení součásti v [šablony Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) , který obsahuje [řešení prostředků](operations-management-suite-solutions-solution-file.md#solution-resource) s toohello vztahy hello ostatní prostředky v souboru.  


## <a name="test-your-solution"></a>Testování řešení
Při vývoji řešení, budete potřebovat tooinstall a otestovat ji v pracovním prostoru.  To provedete pomocí libovolné dostupné metody hello příliš[testování a instalace šablon Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="publish-your-solution"></a>Publikování řešení
Po dokončení a testovat řešení, můžete jej nastavit k dispozici toocustomers prostřednictvím buď hello následující zdroje.

- **Šablony Azure rychlý Start**.  [Šablony Azure rychlý Start](https://azure.microsoft.com/resources/templates/) je sada šablon Resource Manageru přispěli hello komunity přes GitHub.  Můžete zpřístupnit řešení podle následující informace v hello [příspěvku průvodce](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE).
- **Azure Marketplace**.  Hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) vám umožní toodistribute a prodeje řešení ISV, tooother vývojáře a IT profesionály.  Dozvíte, jak toopublish vaše řešení tooAzure Marketplace na [jak toopublish a spravovat nabídku v hello Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).



## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[vytvořte soubor řešení](operations-management-suite-solutions-solution-file.md) řešení pro správu.
* Další podrobnosti o hello [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).
* Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.