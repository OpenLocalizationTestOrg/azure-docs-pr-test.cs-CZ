---
title: "aaaSaved hledání a upozornění v OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat uložená hledání v analýzy protokolů tooanalyze data shromažďovaná společností hello řešení.  Mohou také definovat výstrahy toonotify hello uživatele i automaticky akce v odpovědi tooa kritický problém.  Tento článek popisuje, jak toodefine analýzy protokolů uložené hledání a upozornění v šablony ARM, můžou být součástí řešení pro správu."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a>Přidání analýzy protokolů uložené hledání a výstrahy tooOMS řešení pro správu (Preview)

> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.   


[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat [uložená hledání](../log-analytics/log-analytics-log-searches.md) v analýzy protokolů tooanalyze data shromažďovaná společností hello řešení.  Může také definovat [výstrahy](../log-analytics/log-analytics-alerts.md) toonotify hello uživatele nebo automaticky provést akce v odpovědi tooa kritický problém.  Tento článek popisuje, jak uložit toodefine analýzy protokolů hledání a upozornění v [Správa prostředků šablony](../resource-manager-template-walkthrough.md) tak můžou být je součástí [řešení pro správu](operations-management-suite-solutions-creating.md).

> [!NOTE]
> Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)  

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již obeznámeni s postupy příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md) a struktuře hello [šablony ARM](../resource-group-authoring-templates.md) a soubor řešení.


## <a name="log-analytics-workspace"></a>Pracovní prostor analýzy protokolů
Všechny prostředky v analýzy protokolů jsou součástí [prostoru](../log-analytics/log-analytics-manage-access.md).  Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello prostoru není zahrnutý v řešení pro správu hello ale musí existovat před instalací hello řešení.  Pokud není k dispozici, se nezdaří instalace řešení hello.

Název Hello hello pracovního prostoru se hello název každého prostředku analýzy protokolů.  To se provádí v hello řešení s hello **prostoru** parametr jako hello následující ukázka savedsearch prostředku.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a>Uložená hledání
Zahrnout [uložená hledání](../log-analytics/log-analytics-log-searches.md) v řešení tooallow uživatelé tooquery dat shromážděných vaše řešení.  Uložené hledání se zobrazí v části **Oblíbené** na portálu OMS hello a **uložená hledání** v hello portálu Azure.  Uložené hledání je také nutný pro každou výstrahu.   

[Analýzy protokolů uložené hledání](../log-analytics/log-analytics-log-searches.md) prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches` a mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



Ke každé z vlastností hello uloženého hledání jsou popsané v následující tabulce hello. 

| Vlastnost | Popis |
|:--- |:--- |
| category | Hello kategorii pro uložené hledání hello.  Žádné uložená hledání v hello často budou sdílet stejnou řešení jedné kategorii, jsou seskupeny dohromady v konzole hello. |
| DisplayName | Název toodisplay pro hello uložené hledání hello portálu. |
| query | Toorun dotazu. |

> [!NOTE]
> Toouse řídicí znaky v hello dotazu může být nutné, pokud obsahuje znaky, které je možné interpretovat jako JSON.  Například, pokud se dotaz **typu: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, by měly být zapsány v souboru hello řešení jako **typu: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Výstrahy
[Protokolu analýzy výstrahy](../log-analytics/log-analytics-alerts.md) jsou vytvořené pomocí pravidla výstrah, které běží uloženého hledání v pravidelných intervalech.  Pokud hello výsledky dotazu hello odpovídají zadaným kritériím, se vytvoří záznam výstrah a spouštějí jednu nebo více akcí.  

Pravidla výstrah v rámci řešení pro správu jsou tvořeny hello následující tři různé prostředky.

- **Uložené hledání.**  Definuje hello protokolu vyhledávání, který se má spustit.  Více pravidla výstrah můžete sdílet jeden uloženého hledání.
- **Plán.**  Definuje, jak často hello protokol hledání se spustí.  Každé pravidlo výstrahy budou mít pouze jeden plán.
- **Výstrahy akce.**  Každé pravidlo výstrahy bude mít jeden prostředek akce s typem **výstraha** hello podrobnosti výstrahy hello například hello kritéria pro při záznamu výstrah se vytvoří a hello závažnost výstrahy, který definuje.  Hello akce prostředků bude volitelně definovat odpověď e-mailu a sady runbook.
- **Akce Webhooku (volitelné).**  Pokud pravidlo výstrahy hello bude volat webhook, jehož, pak vyžaduje prostředek další akce s typem **Webhooku**.    

Uložené hledání, které prostředky jsou popsané výše.  Hello další prostředky jsou popsané níže.


### <a name="schedule-resource"></a>Plán prostředku

Uložené hledání může mít jeden nebo více plánů s každý plán reprezentující samostatné pravidlo výstrahy. Hello plán definuje, jak často hello vyhledávání běží a hello časový interval, přes které hello jsou načtena data.  Plán prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` a mít hello strukturu. Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



Hello vlastnosti pro plán prostředky jsou popsané v následující tabulka hello.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| povoleno       | Ano | Určuje, zda text hello upozornění je povolené, když je vytvořeno. |
| interval      | Ano | Jak často hello spuštění dotazu v minutách. |
| QueryTimeSpan | Ano | Časový interval v minutách, přes které tooevaluate výsledky. |

prostředek Hello plán by měl záviset na hello uložené hledání tak, aby se vytvoří před hello plán.


### <a name="actions"></a>Akce
Existují dva typy akce prostředku zadaného parametrem hello **typ** vlastnost.  Plán vyžaduje jednu **výstraha** akce, který definuje hello podrobnosti hello pravidlo výstrahy a jaké akce jsou provedeny, když se vytvoří výstraha.  Může také zahrnovat **Webhooku** akce, pokud by se měla volat webhook, jehož hello výstraze.  

Akce prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

#### <a name="alert-actions"></a>Akce upozornění

Každý plán bude mít jeden **výstrahy** akce.  Definuje hello podrobnosti výstrahy hello a volitelně oznámení a nápravné akce.  Odešle oznámení tooone e-mailu nebo víc adres.  Nápravy spustí sadu runbook v Azure Automation tooattempt tooremediate hello zjistil problém.

Akce výstrah mít hello strukturu.  Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello. 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

Hello vlastnosti výstrahy akce prostředků jsou popsané v následující tabulky hello.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| Typ | Ano | Typ akce hello.  Bude jím **výstraha** pro akce výstrah. |
| Name (Název) | Ano | Zobrazovaný název pro hello výstrahu.  Toto je hello název, který se zobrazí v konzole hello pro pravidlo výstrahy hello. |
| Popis | Ne | Volitelný popis výstrahy hello. |
| Závažnost | Ano | Závažnost výstrahy záznam hello z hello následující hodnoty:<br><br> **Kritické**<br>**Upozornění**<br>**Informační** |


##### <a name="threshold"></a>Prahová hodnota
Tento oddíl je povinný.  Definuje vlastnosti hello hello prahové hodnoty pro výstrahu.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| Operátor | Ano | Operátor pro porovnání hello z hello následující hodnoty:<br><br>**gt = větší než<br>lt = menší než** |
| Hodnota | Ano | Hello hodnota toocompare hello výsledky. |


##### <a name="metricstrigger"></a>MetricsTrigger
Tato část je volitelné.  Zahrňte pro upozornění na metriky měření.

> [!NOTE]
> Metriky měření výstrahy jsou aktuálně ve verzi public preview. 

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| TriggerCondition | Ano | Určuje, zda text hello prahová hodnota pro celkový počet narušení nebo po sobě jdoucích narušení z hello následující hodnoty:<br><br>**Celkový počet<br>po sobě jdoucích** |
| Operátor | Ano | Operátor pro porovnání hello z hello následující hodnoty:<br><br>**gt = větší než<br>lt = menší než** |
| Hodnota | Ano | Počet hello časy hello kritéria musí být výstraha nesplnění tootrigger hello. |

##### <a name="throttling"></a>Omezování
Tato část je volitelné.  Pokud chcete, aby toosuppress výstrahy z hello stejná pravidla pro určitou dobu, po vytvoření výstrahy, zahrnují v této části.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| Doba trvání v minutách | Ano, pokud omezení zahrnuté – element | Počet minut toosuppress výstrahy po jedné z hello, že se vytvoří stejné pravidlo výstrahy. |

##### <a name="emailnotification"></a>EmailNotification
 Tato část je volitelné zahrnout jej chcete-li hello tooone výstrahy toosend e-mailu nebo další příjemce.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| Příjemce | Ano | Čárkami oddělený seznam e-mailové adresy toosend oznámení při výstrahu jako ve hello následující ukázka.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Předmět | Ano | Řádek předmětu e-mailu hello. |
| Přílohy | Ne | Přílohy nejsou aktuálně podporovány.  Pokud tento element je zahrnuto, by měla být **žádné**. |


##### <a name="remediation"></a>Nápravy
Tato část je volitelný ho zahrňte, pokud chcete runbook toostart v odpovědi toohello výstrahy. |

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| RunbookName | Ano | Název sady runbook toostart hello. |
| WebhookUri | Ano | Identifikátor URI hello webhooku pro sadu runbook hello. |
| Vypršení platnosti | Ne | Datum a čas, který hello nápravy vyprší. |

#### <a name="webhook-actions"></a>Akce Webhooku

Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části toobe, odeslána. Jsou podobné tooRemediation akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook. Obsahují taky hello další možnost poskytnout vzdáleného procesu toohello toobe doručovat datové části.

Pokud upozornění bude volat webhook, jehož, pak bude je nutné prostředek akce s typem **Webhooku** v přidání toohello **výstraha** akce prostředků.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

Hello vlastnosti Webhooku akce prostředků jsou popsané v následujících tabulkách hello.

| Název elementu | Požaduje se | Popis |
|:--|:--|:--|
| type | Ano | Typ akce hello.  Bude jím **Webhooku** pro akce webhooku. |
| jméno | Ano | Zobrazovaný název pro akci hello.  To se nezobrazí v konzole hello. |
| wehookUri | Ano | Identifikátor URI pro hello webhook. |
| CustomPayload | Ne | Webhooku toohello toobe odeslat vlastní datovou část. Formát Hello bude záviset na jaké hello webhooku očekává. |




## <a name="sample"></a>Ukázka

Tady je ukázka řešení, které zahrnují zahrnující hello následující prostředky:

- Uložené hledání
- Plán
- Akce oznámení
- Akce Webhooku

Hello používá ukázka [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení, jako byl proti toohardcoding hodnoty v definicích prostředků hello.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for hello email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
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
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
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
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


Hello následující soubor parametrů obsahuje hodnoty ukázky pro toto řešení.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a>Další kroky
* [Přidání zobrazení](operations-management-suite-solutions-resources-views.md) tooyour řešení pro správu.
* [Přidat runbooků služeb automatizace a dalším prostředkům](operations-management-suite-solutions-resources-automation.md) tooyour řešení pro správu.

