---
title: "prostředky služby Automation aaaAzure OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat sady runbook v Azure Automation tooautomate procesy, jako je shromažďování a zpracování dat monitorování.  Tento článek popisuje, jak tooinclude sady runbook a jejich související prostředky v řešení."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a>Přidání řešení správy OMS tooan prostředky Azure Automation (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.   


[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat sady runbook v Azure Automation tooautomate procesy, jako je shromažďování a zpracování dat monitorování.  Kromě toorunbooks, účty služby Automation obsahuje prostředky, jako jsou proměnné a plány, které podporují sady runbook hello používá v řešení hello.  Tento článek popisuje, jak tooinclude sady runbook a jejich související prostředky v řešení.

> [!NOTE]
> Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již obeznámeni s hello následující informace.

- Jak příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md).
- Hello struktura [soubor řešení](operations-management-suite-solutions-solution-file.md).
- Jak příliš[vytváření šablon Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Účet Automation
Všechny prostředky ve službě Azure Automation jsou součástí [účet Automation](../automation/automation-security-overview.md#automation-account-overview).  Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) není zahrnutý v řešení pro správu hello hello účet Automation, ale musí existovat před instalací hello řešení.  Pokud není k dispozici, se nezdaří instalace řešení hello.

Hello název každého prostředku automatizace obsahuje název hello jeho účtu Automation.  To se provádí v hello řešení s hello **accountName** parametr jako hello následující ukázka runbook prostředku.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooky
By měly obsahovat všechny sady runbook používá hello řešení v souboru hello řešení tak, aby se vytváří při instalaci hello řešení.  Hello textu hello sady runbook v šabloně hello nemůže obsahovat Přestože, proto byste měli publikovat hello runbook tooa veřejném místě kde je přístupná žádný uživatel instalace řešení.

[Azure Automation runbook](../automation/automation-runbook-types.md) prostředky mít typ **Microsoft.Automation/automationAccounts/runbooks** a mít hello strukturu. Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


Hello vlastnosti pro sady runbook jsou popsány v následující tabulce hello.

| Vlastnost | Popis |
|:--- |:--- |
| runbookType |Určuje typy hello hello sady runbook. <br><br> Skript - skript prostředí PowerShell <br>PowerShell – pracovní postup prostředí PowerShell <br> GraphPowerShell - runbook skriptu grafické prostředí PowerShell <br> GraphPowerShellWorkflow - runbook pracovního postupu grafické prostředí PowerShell |
| logProgress |Určuje, zda [průběhu záznamy](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován hello sady runbook. |
| logVerbose |Určuje, zda [podrobných záznamů](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován hello sady runbook. |
| description |Volitelný popis pro sadu runbook hello. |
| publishContentLink |Určuje hello obsah sady runbook hello. <br><br>identifikátor URI - toohello obsah identifikátoru Uri hello sady runbook.  Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.  <br> verze - verzi hello runbook pro vlastní sledování. |


## <a name="automation-jobs"></a>Automatizace úloh
Když spustíte runbook ve službě Azure Automation, vytvoří úlohu automatizace.  Můžete přidat automatizaci úlohy prostředků tooyour řešení tooautomatically spuštění sady runbook při instalaci hello řešení pro správu.  Tato metoda je obvykle používanými toostart sady runbook, které se používají pro počáteční konfiguraci hello řešení.  Vytvoření toostart sady runbook v pravidelných intervalech [plán](#schedules) a [plán úloh](#job-schedules)

Úloha prostředky mít typ **Microsoft.Automation/automationAccounts/jobs** a mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

Hello vlastnosti pro automatizaci úloh jsou popsané v následující tabulce hello.

| Vlastnost | Popis |
|:--- |:--- |
| sady runbook |Jeden název entity s názvem hello hello runbook toostart. |
| parameters |Entita pro každou hodnotu parametru vyžadovanou hello runbook. |

Úloha Hello zahrnuje hello název sady runbook a všechny toobe hodnoty parametru odeslané toohello runbook.  úlohy Hello by [závisí na](operations-management-suite-solutions-solution-file.md#resources) hello runbook, která se spouští od hello runbook musí být vytvořeny před hello úlohy.  Pokud máte více sad runbook, který by měl být spuštěn můžete definovat jejich pořadí tak, že úloha závisí na jiné úlohy, které by měl být spuštěn první.

Hello název prostředku úlohy musí obsahovat identifikátor GUID, který je obvykle přiřadila parametr.  Další informace o parametrech identifikátor GUID v [vytváření řešení v Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Certifikáty
[Azure Automation certifikáty](../automation/automation-certificates.md) mít typ **Microsoft.Automation/automationAccounts/certificates** a mít hello strukturu. Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



Hello vlastnosti prostředků certifikáty jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--- |:--- |
| base64Value |Hodnota Base 64 hello certifikátu. |
| Kryptografický otisk |Kryptografický otisk certifikátu hello. |



## <a name="credentials"></a>Přihlašovací údaje
[Přihlašovací údaje Azure Automation](../automation/automation-credentials.md) mít typ **Microsoft.Automation/automationAccounts/credentials** a mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

Hello vlastnosti prostředků přihlašovacích údajů jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--- |:--- |
| Uživatelské jméno |Uživatelské jméno pro přihlašovací údaje hello. |
| heslo |Heslo pro přihlašovací údaje hello. |


## <a name="schedules"></a>Plány
[Azure Automation plány](../automation/automation-schedules.md) mít typ **Microsoft.Automation/automationAccounts/schedules** a mít hello hello strukturu. Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

Hello vlastnosti pro plán prostředky jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--- |:--- |
| description |Volitelný popis pro plán hello. |
| startTime |Určuje hello čas spuštění plánu jako objekt data a času. Řetězec lze zadat, pokud lze převedený tooa platný datový typ DateTime. |
| Hodnotu IsEnabled |Určuje, zda je povoleno hello plán. |
| interval |Hello typ intervalu pro plán hello.<br><br>Den<br>Hodina |
| frequency |Četnost hello plán by měl fire počtu dnů nebo hodin. |

Plány musí mít počáteční čas s hodnotou větší než hello aktuální čas.  Tuto hodnotu nelze poskytnout proměnné, vzhledem k tomu, že by měla mít žádný způsob, jak zjistit, kdy je toobe má nainstalovaný.

Použijte jeden z následujících dvou strategie při použití plánu prostředky v řešení hello.

- Použijte parametr pro hello čas spuštění plánu hello.  Zobrazí se výzva hello uživatele tooprovide hodnotu při instalaci hello řešení.  Pokud máte více plánů, můžete použít jeden parametr hodnotu pro více než jeden z nich.
- Vytvořte plány hello pomocí sady runbook, který se spustí při řešení hello je nainstalována.  Odebere hello požadavek hello toospecify uživatele na dobu, ale nemůže obsahovat hello plán ve vašem řešení, když dojde k odebrání hello řešení bude odebrán.


### <a name="job-schedules"></a>Plány úlohy
Prostředky plán úlohy propojit sady runbook s plánem.  Mají typ **Microsoft.Automation/automationAccounts/jobSchedules** a mít hello hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


Hello vlastnosti pro plány úloh jsou popsané v následující tabulce hello.

| Vlastnost | Popis |
|:--- |:--- |
| Název plánu |Jeden **název** entity s názvem hello hello plánu. |
| Název sady Runbook  |Jeden **název** entity s názvem hello hello sady runbook.  |



## <a name="variables"></a>Proměnné
[Azure Automation proměnné](../automation/automation-variables.md) mít typ **Microsoft.Automation/automationAccounts/variables** a mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

Hello vlastnosti pro proměnné prostředky jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--- |:--- |
| description | Volitelný popis pro proměnnou hello. |
| isEncrypted | Určuje, jestli by se šifrovat hello proměnné. |
| type | Tato vlastnost aktuálně nemá žádný vliv.  datový typ Hello hello proměnné určí hello počáteční hodnota. |
| hodnota | Hodnotu pro proměnnou hello. |

> [!NOTE]
> Hello **typ** vlastnost aktuálně nemá žádný vliv na proměnnou hello vytváří.  Hello datový typ pro proměnnou hello určí hodnotu hello.  

Pokud jste nastavili hello počáteční hodnotu pro proměnnou hello, musí být nakonfigurované jako hello správného datového typu.  Hello následující tabulka obsahuje různé datové typy hello povolená a jejich syntaxi.  Všimněte si, jestli jsou hodnoty ve formátu JSON očekávané tooalways být uzavřena v uvozovkách s žádné speciální znaky v rámci hello uvozovky.  Například by být řetězcová hodnota určena hello řetězec v uvozovkách (pomocí hello řídicí znak (\\)) číselnou hodnotu by zadán s jednu sadu uvozovky.

| Datový typ | Popis | Příklad | Přeloží příliš|
|:--|:--|:--|:--|
| Řetězec   | Vložte hodnotu do dvojitých uvozovek.  | "\"Hello, world\"" | "Hello, world" |
| číselné  | Číselná hodnota se jednoduchých uvozovkách.| "64" | 64 |
| Logická hodnota  | **Hodnota TRUE,** nebo **false** v uvozovkách.  Všimněte si, že tato hodnota musí být malými písmeny. | "true" | Hodnota TRUE |
| Data a času | Hodnota serializovaná data.<br>Tato hodnota hello ConvertTo-Json rutiny v prostředí PowerShell toogenerate můžete použít pro konkrétní datum.<br>Příklad: get datum "5/24/2017 13:14:57" \| ConvertTo-Json | "\\/Date(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Moduly
Řešení pro správu nemusí toodefine [globální moduly](../automation/automation-integration-modules.md) použít ve vašich sadách runbook, protože se budou vždy k dispozici ve vašem účtu Automation.  Potřebujete tooinclude prostředek pro ostatní moduly používané vaší sady runbook.

[Integrační moduly](../automation/automation-integration-modules.md) mít typ **Microsoft.Automation/automationAccounts/modules** a mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


Hello vlastnosti pro modul prostředky jsou popsané v následující tabulka hello.

| Vlastnost | Popis |
|:--- |:--- |
| contentLink |Určuje obsah hello hello modulu. <br><br>identifikátor URI - toohello obsah identifikátoru Uri hello modulu.  Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.  <br> verze - verze hello modulu pro vlastní sledování. |

Hello runbook by měl závisí na hello modulu prostředků tooensure vytvořené před hello runbook.

### <a name="updating-modules"></a>Aktualizace moduly
Pokud aktualizujete řešení pro správu, která zahrnuje sadu runbook, která používá plánu a hello novou verzi řešení má nové modulu používá dané sady runbook, může používat hello runbook hello starší verzi modulu hello.  Musí zahrnovat následující sady runbook ve vašem řešení hello a vytvořit toorun úlohy je před jiné runbooky.  Tím bude zajištěno, že jsou všechny moduly aktualizovat, protože požadované před hello, které se načítají sady runbook.

* [Aktualizace ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zajistí, že všechny hello moduly používané v sadách runbook ve vašem řešení hello nejnovější verzi.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) bude znovu registrovat všechny hello plán prostředky tooensure že hello runbooky propojené toothem s použití hello nejnovější moduly.




## <a name="sample"></a>Ukázka
Tady je ukázka řešení, které zahrnují zahrnující hello následující prostředky:

- Sady Runbook.  Toto je uložen v úložišti GitHub, které veřejné vzorové sady runbook.
- Úlohu automatizace, který se spustí hello runbook při řešení hello je nainstalována.
- Plán a úlohy naplánovat v pravidelných intervalech toostart hello runbook.
- Certifikát.
- Přihlašovací údaje.
- Proměnná.
- Modul.  Toto je hello [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) pro zápis dat tooLog Analytics. 

Hello používá ukázka [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení, jako byl proti toohardcoding hodnoty v definicích prostředků hello.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Další kroky
* [Přidat řešení tooyour zobrazení](operations-management-suite-solutions-resources-views.md) toovisualize shromažďovat data.
