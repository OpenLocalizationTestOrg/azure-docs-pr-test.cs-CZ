---
title: "řešení pro správu aaaCreating v Operations Management Suite (OMS) | Microsoft Docs"
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
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a>Vytvoření souboru řešení pro správu v Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.  

Řešení pro správu v Operations Management Suite (OMS) jsou implementované jako [šablony Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md).  hlavní úloh Hello dozvědět, jak je řešení pro správu tooauthor učení jak příliš[vytvořit šablonu](../azure-resource-manager/resource-group-authoring-templates.md).  Tento článek obsahuje jedinečné Podrobnosti šablony použité pro řešení a jak tooconfigure typické řešení prostředky.


## <a name="tools"></a>Nástroje

Všechny toowork textového editoru můžete používat se soubory řešení, ale doporučujeme, abyste využití hello funkce poskytované v sadě Visual Studio nebo Visual Studio Code, jak je popsáno v následujících článcích hello.

- [Vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Práce s šablony Azure Resource Manageru v sadě Visual Studio kódu](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>Struktura
Základní struktura Hello soubor řešení správy je hello stejné jako [šablony Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#template-format) tedy následujícím způsobem.  Každý z níže uvedených částech hello popisuje elementy hello nejvyšší úrovně a a jejich obsah v řešení.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametry
[Parametry](../azure-resource-manager/resource-group-authoring-templates.md#parameters) jsou hodnoty, které požadujete od hello uživatele při instalaci hello řešení pro správu.  Jsou standardní parametry, které budou mít všechna řešení a podle potřeby můžete přidat další parametry pro vaše konkrétní řešení.  Jak budou uživatelé zadali hodnoty parametrů při instalaci řešení bude záviset na konkrétní parametr hello a jak se instaluje hello řešení.

Když uživatel nainstaluje řešení pro správu prostřednictvím hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) nebo [šablony Azure QuickStart](operations-management-suite-solutions.md#finding-and-installing-management-solutions) jsou výzvami tooselect [OMS pracovní prostor a účet Automation. ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).  Toto jsou hodnoty používané toopopulate hello každé standardní parametry hello.  Hello uživatel nebude vyzván k toodirectly zadejte hodnoty pro parametry hello standardní, ale jsou výzvami tooprovide hodnoty pro žádné další parametry.

Když uživatel hello nainstaluje řešení [jinou metodu](operations-management-suite-solutions.md#finding-and-installing-management-solutions), musí zadat hodnotu pro všechny standardní parametry a všech dalších parametrů.

Ukázka parametrů jsou uvedeny níže.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Hello následující tabulka popisuje atributy hello parametru.

| Atribut | Popis |
|:--- |:--- |
| type |Datový typ pro parametr hello. Hello vstupního ovládacího prvku zobrazuje pro uživatele hello závisí na typu dat hello.<br><br>BOOL – rozevíracího seznamu<br>String – textové pole<br>int – textové pole<br>SecureString - pole pro heslo<br> |
| category |Volitelné kategorie pro parametr hello.  Parametry v hello stejné kategorii jsou seskupeny dohromady. |
| Ovládací prvek |Další funkce pro parametry řetězce.<br><br>Zobrazí se datum a čas - datum a čas řízení.<br>identifikátor GUID – hodnota identifikátoru Guid je generován automaticky, a parametr hello se nezobrazí. |
| description |Volitelný popis pro parametr hello.  Zobrazí informace o bublinách další toohello parametr. |

### <a name="standard-parameters"></a>Standardní parametry
Hello následující tabulka uvádí hello standardní parametry pro všechna řešení pro správu.  Tyto hodnoty jsou naplněny pro uživatele hello místo dotaz na ně při řešení z Azure Marketplace nebo rychlý start šablon hello.  Pokud řešení hello se instaluje s jinou metodu, musí uživatel Hello zadat hodnoty pro ně.

> [!NOTE]
> Hello uživatelské rozhraní v šablonách Azure Marketplace a rychlý start hello očekává hello názvy parametrů v tabulce hello.  Pokud používáte jiný parametr názvy pak hello uživateli zobrazí výzva pro ně, a nebude se automaticky vyplní.
>
>

| Parametr | Typ | Popis |
|:--- |:--- |:--- |
| název účtu |Řetězec |Název účtu Azure Automation. |
| pricingTier |Řetězec |Cenová úroveň pracovní prostor analýzy protokolů a účet Azure Automation. |
| regionId |Řetězec |Oblast hello účet Azure Automation. |
| Název řešení SolutionName |Řetězec |Název řešení hello.  Pokud nasazujete řešení prostřednictvím šablony rychlý start, pak byste měli definovat název řešení solutionName jako parametr, můžete definovat místo nutnosti toospecify hello uživatele, jeden řetězec. |
| workspaceName |Řetězec |Název pracovního prostoru analýzy protokolů |
| workspaceRegionId |Řetězec |Oblast pracovního prostoru analýzy protokolů hello. |


Následuje hello struktura hello standardní parametry, které můžete zkopírovat a vložit do souboru řešení.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Odkazovat tooparameter hodnoty v další prvky hello řešení se syntaxí hello **parametry (název parametru)**.  Například tooaccess hello název pracovního prostoru, byste použili **parameters('workspaceName')**

## <a name="variables"></a>Proměnné
[Proměnné](../azure-resource-manager/resource-group-authoring-templates.md#variables) jsou hodnoty, které budete používat v hello zbytek hello řešení pro správu.  Tyto hodnoty nejsou zveřejněné toohello uživatel, který instaluje hello řešení.  Jsou to určený tooprovide hello Autor na jednom místě, kde můžete spravovat hodnoty, které se dají použít více než jednou v celé řešení hello. Řešení konkrétních tooyour hodnoty měli umístit do proměnné jako názvem na rozdíl od toohard kódování je v hello **prostředky** elementu.  To usnadňuje srozumitelnější hello kódu a umožňuje vám tooeasily změnit tyto hodnoty v novějších verzích.

Následuje příklad **proměnné** element s typické parametry použité v řešení.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Najdete hodnoty toovariable prostřednictvím řešení hello se syntaxí hello **proměnné ('název proměnné')**.  Například tooaccess hello název řešení SolutionName proměnné, byste použili **variables('SolutionName')**.

Můžete také definovat komplexní proměnné tohoto několik sad hodnot.  Tyto jsou zvláště užitečné při řešení pro správu, které definujete více vlastností pro různé typy prostředků.  Například může změnit strukturu proměnné hello řešení uvedené výše toohello následující.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

V takovém případě najdete hodnoty toovariable prostřednictvím řešení hello se syntaxí hello **variables('variable name').property**.  Například tooaccess hello řešení název proměnné, byste použili **variables('Solution'). Název**.

## <a name="resources"></a>Zdroje
[Prostředky](../azure-resource-manager/resource-group-authoring-templates.md#resources) definovat hello různé prostředky, které nainstaluje a nakonfiguruje vaše řešení pro správu.  Bude jím hello největší a těch nejsložitějších část hello šablony.  Můžete získat hello strukturu a úplný popis elementů prostředků v [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Různé prostředky, které se obvykle definují, jsou popsané v další články v této dokumentaci. 


### <a name="dependencies"></a>Závislosti
Hello **dependsOn** určuje elementy [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na jiný prostředek.  Při instalaci hello řešení prostředku se nevytvoří, dokud všechny jeho závislé součásti byly vytvořeny.  Například může být vaše řešení [spuštění sady runbook](operations-management-suite-solutions-resources-automation.md#runbooks) při instalaci pomocí [úlohy prostředků](operations-management-suite-solutions-resources-automation.md#automation-jobs).  prostředek úlohy Hello by být závislá na hello runbook prostředků toomake se, že dané sady runbook hello je vytvořen, než bude vytvořena úloha hello.

### <a name="oms-workspace-and-automation-account"></a>Pracovní prostor OMS a účet Automation.
Vyžaduje řešení pro správu [pracovním prostorem OMS](../log-analytics/log-analytics-manage-access.md) toocontain zobrazení a [účet Automation](../automation/automation-security-overview.md#automation-account-overview) toocontain sady runbook a související prostředky.  Toto musí být k dispozici před hello prostředky v řešení hello vytvářejí a nesmí být definována v hello řešení sám sebe.  Hello uživatele bude [zadejte prostoru a účet](operations-management-suite-solutions.md#oms-workspace-and-automation-account) při jejich nasazování svého řešení, ale jako autor hello byste měli zvážit následující body hello.

## <a name="solution-resource"></a>Řešení prostředků
Každé řešení vyžaduje záznam prostředků v hello **prostředky** element, který definuje hello řešení sám sebe.  To bude mít typ **Microsoft.OperationsManagement/solutions** a mít hello strukturu. To zahrnuje [standardní parametry](#parameters) a [proměnné](#variables) , které jsou obvykle používanými toodefine vlastnosti hello řešení.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Závislosti
musí mít Hello řešení prostředků [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na každý jiný prostředek v řešení hello vzhledem k tomu, že potřebují tooexist před vytvořením hello řešení.  To uděláte tak, že přidáte položku pro všechny prostředky v hello **dependsOn** elementu.

### <a name="properties"></a>Vlastnosti
Hello řešení prostředků má hello vlastnosti v hello následující tabulka.  To zahrnuje hello prostředky odkazovat a obsažených hello řešení, která definuje, jak se spravuje hello prostředků po instalaci hello řešení.  Všechny prostředky v řešení hello by měl být uvedený v buď hello **referencedResources** nebo hello **containedResources** vlastnost.

| Vlastnost | Popis |
|:--- |:--- |
| workspaceResourceId |ID pracovního prostoru analýzy protokolů hello v podobě hello  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<název pracovního prostoru\>*. |
| referencedResources |Seznam prostředků v hello řešení, které by se neměly odebírat, když dojde k odebrání hello řešení. |
| containedResources |Seznam prostředků v hello řešení, které byste měli odebrat, když dojde k odebrání hello řešení. |

výše uvedený příklad Hello je řešení s sady runbook, plán a zobrazení.  plán Hello a sady runbook jsou *odkazované* v hello **vlastnosti** element tak nejsou odebrány při odebrání hello řešení.  zobrazení Hello *obsažené* proto je odebrána, když dojde k odebrání hello řešení.

### <a name="plan"></a>Plánování
Hello **plán** entity hello řešení prostředku má hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| jméno |Název řešení hello. |
| Verze |Verze hello řešení, počítáno od autora hello. |
| Produktu |Jedinečné řetězce tooidentify hello řešení. |
| Vydavatele |Vydavatel hello řešení. |



## <a name="sample"></a>Ukázka
Ukázky soubory řešení s prostředek řešení můžete zobrazit v hello následující umístění.

- [Prostředky služby Automation](operations-management-suite-solutions-resources-automation.md#sample)
- [Hledání a výstraha prostředky](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Další kroky
* [Přidat uložená hledání a výstrahy](operations-management-suite-solutions-resources-searches-alerts.md) tooyour řešení pro správu.
* [Přidání zobrazení](operations-management-suite-solutions-resources-views.md) tooyour řešení pro správu.
* [Přidat sady runbook a dalším prostředkům Automation](operations-management-suite-solutions-resources-automation.md) tooyour řešení pro správu.
* Další podrobnosti o hello [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).
* Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.
