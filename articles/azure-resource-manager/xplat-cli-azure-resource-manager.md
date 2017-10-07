---
title: "aaaManage prostředky s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Použít toomanage hello rozhraní příkazového řádku Azure (CLI) Azure prostředků a skupin"
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
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a>Použití Azure CLI toomanage hello Azure skupiny prostředků a prostředky
> [!div class="op_single_selector"]
> * [Azure Portal](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST API](resource-manager-rest-api.md)
> 
> 

Hello rozhraní příkazového řádku Azure (Azure CLI) je jedním z několika nástrojů můžete použít toodeploy a spravovat prostředky pomocí Resource Manageru. Tento článek představuje běžné způsoby toomanage Azure prostředků a skupin prostředků pomocí hello rozhraní příkazového řádku Azure v režimu Resource Manager. Informace o použití prostředků toodeploy hello rozhraní příkazového řádku najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md). Informace o prostředků Azure a Resource Manager, najdete v článku hello [přehled Azure Resource Manageru](resource-group-overview.md).

> [!NOTE]
> toomanage Azure prostředky s hello příkazového řádku Azure CLI, je třeba příliš[nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md), a [přihlásit tooAzure](../xplat-cli-connect.md) pomocí hello `azure login` příkaz. Ujistěte se, hello rozhraní příkazového řádku je v režimu Resource Manager (Spustit `azure config mode arm`). Pokud jste provádí tyto věci, jste toogo připraven.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Skupiny prostředků a prostředky
### <a name="resource-groups"></a>Skupiny prostředků
Tento příkaz Spustit tooget seznam všech skupin prostředků v předplatného a jejich umístění.

    azure group list


### <a name="resources"></a>Zdroje
 Název všechny prostředky ve skupině, například ten, který se toolist *testRG*, použijte následující příkaz hello:

    azure resource list testRG

tooview jednotlivé zdroje v rámci skupiny hello, jako je například virtuální počítač s názvem *MyUbuntuVM*, použijte příkaz jako hello následující:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Všimněte si hello **Microsoft.Compute/virtualMachines** parametr. Tento parametr určuje typ hello hello prostředku, který požadujete informace na.

> [!NOTE]
> Při použití hello **prostředků azure** příkazy než hello **seznamu** příkaz, je nutné zadat hello rozhraní API verze hello prostředku s hello **-o** parametr. Pokud si nejste jisti o hello verze rozhraní API, projděte si soubor šablony hello a najít hello apiVersion pole pro prostředek hello. Další informace o verzích rozhraní API ve službě Správce prostředků najdete v tématu [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).
> 
> 

Při zobrazení podrobností na prostředek, je často užitečné toouse hello `--json` parametr. Tento parametr umožňuje hello výstup čitelnější, protože některé hodnoty jsou vnořené struktury nebo kolekce. Hello následující příklad ukazuje vracejících hello výsledky hello **zobrazit** příkaz jako dokument JSON.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Pokud chcete uložit toofile data JSON hello pomocí hello &gt; znak toodirect hello výstupní tooa soubor. Například:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Značky
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Správa prostředků
tooadd prostředků, jako je například úložiště účet tooa skupinu prostředků, spusťte příkaz podobný:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Kromě toho toospecifying hello verze rozhraní API hello prostředku s hello **-o** parametr, použijte hello **-p** řetězec formátu JSON toopass parametrů s jakékoli požadované nebo další vlastnosti.

toodelete na existující prostředek, jako je například prostředek virtuálního počítače, použijte příkaz jako hello následující:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello **přesunutí prostředku azure** příkaz. Následující příklad ukazuje, jak Hello toomove Redis Cache tooa novou skupinu prostředků. V hello **-i** parametr zadejte čárkami oddělený seznam id prostředku hello toomove.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a>Řízení přístupu tooresources
Můžete použít rozhraní příkazového řádku Azure toocreate hello a spravovat zásady toocontrol přístup tooAzure prostředky. Pro informace o definice zásady a přiřazení zásad tooresources viz [používat prostředky toomanage zásad a kontrolujte přístup](resource-manager-policy.md).

Například hello následující zásady toodeny definovat všechny požadavky, které není umístění západní USA nebo Sever střední USA a uložte ho policy.json soubor definice zásady toohello:

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

Spusťte hello **vytvoření definice zásady** příkaz:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Tento příkaz zobrazí výstup podobný toohello následující:

    + Vytvořením definice zásady MyPolicy data: PolicyName: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data: Třída PolicyType: vlastní data: DisplayName: není definovaná data: Popis: není definovaná data: PolicyRule: pole = umístění, v = [westus, northcentralus], účinek = Odepřít

 tooassign zásadu v oboru hello chcete, použijte hello **PolicyDefinitionId** vrácená z předchozí příkaz hello. V následujícím příkladu hello tento obor je hello předplatné, ale můžete určit obor skupin tooresource nebo jednotlivé prostředky:

    Vytvoření přiřazení zásady Azure MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Můžete získat, změnit nebo odebrat definice zásady pomocí hello **Zobrazit definice zásady**, **definici sady zásad**, a **odstranit definice zásady** příkazy.

Podobně můžete získat, změnit nebo odebrat přiřazení zásad pomocí hello **zobrazit přiřazení zásad**, **sady přiřazení zásad**, a **odstranit přiřazení zásady** příkazy .

## <a name="export-a-resource-group-as-a-template"></a>Export skupiny prostředků jako šablonu
Pro existující skupinu prostředků můžete zobrazit hello šablony Resource Manageru pro skupinu prostředků hello. Export šablony hello nabízí dvě výhody:

1. Snadno můžete automatizovat budoucí nasazení řešení hello vzhledem k tomu, že všechny infrastruktury hello je definována v šabloně hello.
2. Můžete seznámit se syntaxí šablony prohlížením hello formátu JSON, který představuje vaše řešení.

Pomocí hello rozhraní příkazového řádku Azure, můžete buď vyexportovat šablonu, která představuje hello aktuální stav vaší skupiny prostředků, nebo stáhnout hello šablonu, která byla použita pro konkrétní nasazení.

* **Export hello šablony pro skupinu prostředků** – to je užitečné, když jste provedli změny skupiny prostředků tooa a potřebovat tooretrieve hello reprezentace JSON v jejím aktuálním stavu. Vygenerované šablony hello však obsahuje pouze minimální počet parametrů a žádné proměnné. Většina hodnot hello v šabloně hello jsou pevně. Před nasazením hello vygenerované šablony, můžete tooconvert více hodnot hello do parametrů, můžete přizpůsobit hello nasazení pro různá prostředí.
  
    tooexport hello šablony pro prostředek skupiny tooa místní adresář, spusťte hello `azure group export` příkaz, jak ukazuje následující příklad hello. (Nahraďte do místního adresáře, které jsou vhodné pro vaše prostředí operačního systému.)
  
        azure group export testRG ~/azure/templates/
* **Stáhnout hello šablonu pro konkrétní nasazení** – to je užitečné, když potřebujete hello tooview skutečné se šablonu, která byla toodeploy využité prostředky. Šablona Hello zahrnuje všechny parametry a proměnné definované pro původní nasazení hello. Pokud někdo ve vaší organizaci provedené změny skupiny prostředků toohello mimo hello definice v šabloně hello, ale tato šablona nepředstavuje hello aktuální stav skupiny prostředků hello.
  
    toodownload hello Šablona používaná pro konkrétní nasazení tooa místní adresář, spusťte hello `azure group deployment template download` příkaz. Například:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Export šablony je ve verzi preview, a ne všechny typy prostředků v současné době podporují export šablony. Při pokusu o tooexport šablonu, může se zobrazit chyba s oznámením, že některé prostředky nebyly exportovat. V případě potřeby ručně definovat tyto prostředky ve vaší šabloně po stáhnout.
> 
> 

## <a name="next-steps"></a>Další kroky
* Podrobnosti o tooget operace nasazení a řešení chyby nasazení s hello rozhraní příkazového řádku Azure najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* Pokud chcete toouse hello rozhraní příkazového řádku tooset aplikace nebo skriptu tooaccess prostředky, najdete v části [použití Azure CLI toocreate objekt služby prostředků tooaccess](resource-group-authenticate-service-principal-cli.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

