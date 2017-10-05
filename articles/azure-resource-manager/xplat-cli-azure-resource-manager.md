---
title: "Správa prostředků pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Použití rozhraní příkazového řádku Azure (CLI) ke správě prostředků Azure a skupiny"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3ad4e68b90979fd7f9d3ddf5278e65e19cb07152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Použití rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků
> [!div class="op_single_selector"]
> * [Azure Portal](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
> 
> 

Rozhraní příkazového řádku Azure (Azure CLI) je jedním z několik nástrojů, které můžete použít k nasazení a správě prostředků pomocí Resource Manageru. Tento článek představuje běžné způsoby ke správě prostředků Azure a skupiny prostředků pomocí rozhraní příkazového řádku Azure v režimu Resource Manager. Informace o použití rozhraní příkazového řádku pro nasazení prostředků najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md). Informace o prostředků Azure a Resource Manager, najdete v článku [přehled Azure Resource Manageru](resource-group-overview.md).

> [!NOTE]
> Ke správě prostředků Azure pomocí Azure CLI, budete muset [nainstalovat Azure CLI](../cli-install-nodejs.md), a [přihlášení k Azure](../xplat-cli-connect.md) pomocí `azure login` příkaz. Zkontrolujte, zda je rozhraní příkazového řádku v režimu Resource Manager (Spustit `azure config mode arm`). Pokud jste provádí tyto věci, jste připravení přejít.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Skupiny prostředků a prostředky
### <a name="resource-groups"></a>Skupiny prostředků
Chcete-li získat seznam všech skupin prostředků ve vašem předplatném a jejich umístění, spusťte tento příkaz.

    azure group list


### <a name="resources"></a>Zdroje
 Chcete-li vypsat všechny prostředky ve skupině, například ten, který s názvem *testRG*, použijte následující příkaz:

    azure resource list testRG

Chcete-li zobrazit jednotlivé zdroje v rámci skupiny, například virtuální počítač s názvem *MyUbuntuVM*, použijte příkaz takto:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Upozornění **Microsoft.Compute/virtualMachines** parametr. Tento parametr určuje typ prostředku, který požadujete informace na.

> [!NOTE]
> Při použití **prostředků azure** příkazy jiným než **seznamu** příkaz, musíte zadat verze rozhraní API prostředku s **-o** parametr. Pokud si nejste jisti o verze rozhraní API, projděte si soubor šablony a najít pole apiVersion pro prostředek. Další informace o verzích rozhraní API ve službě Správce prostředků najdete v tématu [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).
> 
> 

Při zobrazení podrobností na prostředek, je často užitečné používat `--json` parametr. Tento parametr umožňuje výstup čitelnější, protože některé hodnoty jsou vnořené struktury nebo kolekce. Následující příklad ukazuje, vrátí výsledky **zobrazit** příkaz jako dokument JSON.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Pokud chcete, ukládání dat JSON do souboru pomocí &gt; znak, který má přesměrujte výstup do souboru. Například:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Značky
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Správa prostředků
Přidat prostředek, jako je například účet úložiště do skupiny prostředků, spusťte příkaz podobný:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Kromě určení verze rozhraní API prostředku s **-o** parametr, použijte **-p** parametr předat řetězec formátu JSON s jakékoli požadované nebo další vlastnosti.

Pokud chcete odstranit existující prostředek například prostředek virtuálního počítače, použijte příkaz takto:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte **přesunutí prostředku azure** příkaz. Následující příklad ukazuje, jak přesunout Redis Cache do nové skupiny prostředků. V **-i** parametr zadejte čárkami oddělený seznam id prostředku je přesunout.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Řízení přístupu k prostředkům
Rozhraní příkazového řádku Azure můžete vytvořit a spravovat zásady pro řízení přístupu k prostředkům Azure. Pro informace o definice zásady a přiřazení zásad k prostředkům, viz [použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).

Například definovat tyto zásady tak, aby odepřel všechny požadavky, které není umístění západní USA nebo Sever střední USA a uložte ho policy.json soubor definice zásady:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Spusťte **vytvoření definice zásady** příkaz:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Tento příkaz zobrazí výstup podobný následujícímu:

    + Vytvořením definice zásady MyPolicy data: PolicyName: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data: Třída PolicyType: vlastní data: DisplayName: není definovaná data: Popis: není definovaná data: PolicyRule: pole = umístění, v = [westus, northcentralus], účinek = Odepřít

 K přiřazení zásady v oboru, který chcete, použijte **PolicyDefinitionId** vrácená z předchozí příkaz. V následujícím příkladu se tento obor je předplatné, ale můžete určit obor skupin prostředků nebo jednotlivé prostředky:

    Vytvoření přiřazení zásady Azure MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Můžete získat, změnit nebo odebrat definice zásady pomocí **Zobrazit definice zásady**, **definici sady zásad**, a **odstranit definice zásady** příkazy.

Podobně můžete získat, změnit nebo odebrat přiřazení zásad pomocí **zobrazit přiřazení zásad**, **sady přiřazení zásad**, a **odstranit přiřazení zásady** příkazy.

## <a name="export-a-resource-group-as-a-template"></a>Export skupiny prostředků jako šablonu
Pro existující skupinu prostředků můžete zobrazit šablony Resource Manageru pro skupinu prostředků. Export šablony nabízí dvě výhody:

1. Vzhledem k tomu, že všechny infrastruktury je definována v šabloně můžete snadno automatizovat budoucí nasazení řešení.
2. Můžete seznámit se syntaxí šablony prohlížením formátu JSON, který představuje vaše řešení.

Pomocí rozhraní příkazového řádku Azure, můžete buď vyexportovat šablonu, který představuje aktuální stav vaší skupiny prostředků, nebo stáhnout šablonu, která byla použita pro konkrétní nasazení.

* **Vyexportování šablony pro skupinu prostředků** – to je užitečné, když provedené změny do skupiny prostředků a načtení reprezentaci JSON jejím aktuálním stavu. Vygenerované šablony však obsahuje pouze minimální počet parametrů a žádné proměnné. Většina hodnot v šabloně jsou pevně. Před nasazením vygenerované šablony, můžete chtít převést více hodnot parametrů, můžete přizpůsobit nasazení pro různá prostředí.
  
    Chcete-li exportovat šablonu pro skupinu prostředků do místního adresáře, spusťte `azure group export` příkaz, jak je znázorněno v následujícím příkladu. (Nahraďte do místního adresáře, které jsou vhodné pro vaše prostředí operačního systému.)
  
        azure group export testRG ~/azure/templates/
* **Stáhněte si šablonu pro konkrétní nasazení** – to je užitečné, když potřebujete zobrazit skutečné šablony, která byla použita k nasazení prostředků. Šablona obsahuje všechny parametry a proměnné pro původní nasazení definována. Ale pokud někdo ve vaší organizaci provedené změny ve skupině prostředků mimo definice v šabloně, tato šablona nemá představují aktuální stav skupiny prostředků.
  
    Chcete-li stáhnout šablonu použít pro konkrétní nasazení do místního adresáře, spusťte `azure group deployment template download` příkaz. Například:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Export šablony je ve verzi preview, a ne všechny typy prostředků v současné době podporují export šablony. Při pokusu o export šablony, může se zobrazit chyba s oznámením, že některé prostředky nebyly exportovat. V případě potřeby ručně definovat tyto prostředky ve vaší šabloně po stáhnout.
> 
> 

## <a name="next-steps"></a>Další kroky
* Podrobnosti o operace nasazení a řešení chyb nasazení pomocí rozhraní příkazového řádku Azure najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* Pokud chcete používat rozhraní příkazového řádku k nastavení aplikace nebo skriptu pro přístup k prostředkům, najdete v části [pomocí rozhraní příkazového řádku Azure k vytvoření objektu služby pro přístup k prostředkům](resource-group-authenticate-service-principal-cli.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

