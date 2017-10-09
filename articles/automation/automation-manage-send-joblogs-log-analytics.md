---
title: "aaaForward tooOMS data úlohy automatizace Azure Log Analytics | Microsoft Docs"
description: "Tento článek ukazuje, jak datové proudy toosend úlohy sady runbook a stav úlohy správy a další aspekty toodeliver tooMicrosoft Operations Management Suite Log Analytics."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/2017
ms.author: magoedte
ms.openlocfilehash: e78b6c6677d6502711ce828e2d32b7a91922ae26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-toolog-analytics-oms"></a>Předávání zpráv o stavu úlohy a datové proudy úlohy z automatizace tooLog Analytics (OMS)
Automatizace můžete odeslat runbook úlohy stavu a úlohu datové proudy tooyour Microsoft Operations Management Suite (OMS) pracovní prostor analýzy protokolů.  Protokoly úlohy a datové proudy úlohy jsou viditelné v hello portál Azure nebo v prostředí PowerShell pro jednotlivé úlohy a to vám umožní tooperform jednoduché šetření. Pomocí analýzy protokolů můžete nyní:

* Pohled na vaše úlohy automatizace
* Aktivační událost e-mailem nebo výstrahy podle runbook stav úlohy (například chybných nebo pozastavených)
* Zápis pokročilými dotazy napříč vaše datové proudy úlohy
* Vazbu mezi úlohy v účtech Automation
* Vizualizace historii úlohy v čase     

## <a name="prerequisites-and-deployment-considerations"></a>Požadavky a důležité informace o nasazení
toostart odesílání vašeho automatizace v protokolech tooLog Analytics, budete potřebovat:

1. Hello listopadu 2016 nebo novější vydání [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Pracovní prostor analýzy protokolů. Další informace najdete v tématu [začít pracovat s analýzy protokolů](../log-analytics/log-analytics-get-started.md). 
3. Hello ResourceId pro váš účet Azure Automation.

toofind hello ResourceId pro váš účet Azure Automation a pracovní prostor analýzy protokolů, spusťte hello následující prostředí PowerShell:

```powershell
# Find hello ResourceId for hello Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find hello ResourceId for hello Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Pokud máte více účtů Automation nebo pracovní prostory, v hello výstup hello předchozích příkazů, najde hello *název* potřebovat tooconfigure a zkopírujte hodnotu hello *ResourceId*.

Pokud potřebujete toofind hello *název* účtu Automation v hello portálu Azure vyberte svůj účet Automation z hello **účet Automation** a vyberte **všechna nastavení**.  Z hello **všechna nastavení** okno, v části **nastavení účtu** vyberte **vlastnosti**.  V hello **vlastnosti** okno, můžete si poznamenejte tyto hodnoty.<br> ![Vlastnosti účtu Automation](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Nastavení integrace s analýzy protokolů
1. V počítači, spusťte **prostředí Windows PowerShell** z hello **spustit** obrazovky.  
2. Zkopírujte a vložte následující prostředí PowerShell hello a upravit hodnotu hello hello `$workspaceId` a `$automationAccountId`.  Pro hello `-Environment` parametr platné hodnoty jsou *AzureCloud* nebo *AzureUSGovernment* v závislosti na práci v prostředí cloudu hello.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Po spuštění tohoto skriptu, zobrazí se záznamy v analýzy protokolů během deseti minut nové JobLogs nebo JobStreams zapisovaný.

toosee hello protokoly, spusťte následující dotaz ve vyhledávání protokolu analýzy protokolů hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Ověření konfigurace
pracovní prostor analýzy protokolů tooyour v protokolech tooconfirm, který odesílá účtu Automation, zkontrolujte, jestli jsou na hello účtu Automation pomocí prostředí PowerShell následující hello správně nastavené diagnostics:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Ve výstupu hello zajistěte, aby:
+ V části *protokoly*, hello hodnotu *povoleno* je *True*
+ Hello hodnotu *WorkspaceId* nastavena toohello ResourceId pracovní prostor analýzy protokolů


## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Diagnostika z Azure Automation vytvoří dva typy záznamů v analýzy protokolů a jsou označené jako **typ = AzureDiagnostics**.

### <a name="job-logs"></a>V protokolech úloh
| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas, kdy spustit úlohy runbooku hello. |
| RunbookName_s |Hello název sady runbook hello. |
| Caller_s |Kdo je inicioval hello operaci.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| Tenant_g | Identifikátor GUID, který identifikuje hello klienta pro hello volajícího. |
| JobId_g |Identifikátor GUID, který je hello Id úlohy runbooku hello. |
| ResultType |Hello stav úlohy runbooku hello.  Možné hodnoty:<br>- Spuštěno<br>- Zastaveno<br>- Pozastaveno<br>- Neúspěch<br>-Byla dokončena |
| Kategorie | Klasifikace datový typ hello.  Pro automatizaci hello hodnota je JobLogs. |
| OperationName | Určuje typ hello operaci provést, v Azure.  Pro automatizaci je hodnota hello úlohy. |
| Prostředek | Název hello účet Automation. |
| SourceSystem | Jak analýzy protokolů shromážděných dat hello. Vždy *Azure* Azure Diagnostics. |
| ResultDescription |Popisuje stav výsledek úlohy sady runbook hello.  Možné hodnoty:<br>- Úloha se spustila<br>- Zpracování úlohy se nezdařilo<br>- Úloha je dokončená |
| CorrelationId |Identifikátor GUID, který je hello Id korelace hello úlohy sady runbook. |
| ID prostředku |Určuje id prostředků účtu Azure Automation hello hello sady runbook. |
| SubscriptionId | Hello předplatné Azure Id (GUID) pro hello účet Automation. |
| ResourceGroup | Název skupiny prostředků hello hello účet Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Datové proudy úlohy
| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas, kdy spustit úlohy runbooku hello. |
| RunbookName_s |Hello název sady runbook hello. |
| Caller_s |Kdo je inicioval hello operaci.  Možnou hodnotou je e-mailová adresa nebo systém pro naplánované úlohy. |
| StreamType_s |Typ Hello stream úloh. Možné hodnoty:<br>- Průběh<br>- Výstup<br>- Varování<br>- Chyba<br>- Ladění<br>- Podrobné |
| Tenant_g | Identifikátor GUID, který identifikuje hello klienta pro hello volajícího. |
| JobId_g |Identifikátor GUID, který je hello Id úlohy runbooku hello. |
| ResultType |Hello stav úlohy runbooku hello.  Možné hodnoty:<br>– V průběhu |
| Kategorie | Klasifikace datový typ hello.  Pro automatizaci hello hodnota je JobStreams. |
| OperationName | Určuje typ hello operaci provést, v Azure.  Pro automatizaci je hodnota hello úlohy. |
| Prostředek | Název hello účet Automation. |
| SourceSystem | Jak analýzy protokolů shromážděných dat hello. Vždy *Azure* Azure Diagnostics. |
| ResultDescription |Zahrnuje hello výstupního datového proudu z runbooku hello. |
| CorrelationId |Identifikátor GUID, který je hello Id korelace hello úlohy sady runbook. |
| ID prostředku |Určuje id prostředků účtu Azure Automation hello hello sady runbook. |
| SubscriptionId | Hello předplatné Azure Id (GUID) pro hello účet Automation. |
| ResourceGroup | Název skupiny prostředků hello hello účet Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Zobrazení automatizace přihlásí analýzy protokolů
Teď, když jste spustili odesílání vašeho automatizace úloh protokoly tooLog analýzy, podíváme se, co můžete dělat s tyto protokoly uvnitř analýzy protokolů.

toosee hello protokoly, spusťte následující dotaz hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Odeslat e-mail, dojde k selhání úlohy runbooku nebo pozastaví
Jeden z našich zákazníků nejvyšší požádá, je pro hello možnost toosend e-mailu nebo textový při něco nepovede k úloze runbooku.   

toocreate výstrahu pravidel, začněte vytvořením hledání protokolů pro sady runbook hello záznamů úlohy, které by měla vyvolat výstrahu hello.  Klikněte na tlačítko hello **výstraha** tlačítko toocreate a nakonfigurujte pravidlo výstrahy hello.

1. Na stránce Přehled protokolu Analytics hello, klikněte na tlačítko **hledání protokolů**.
2. Vytvoření vyhledávací dotaz protokolu pro upozornění zadáním hello následující hledání do pole dotazu hello: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` můžete taky Seskupit podle hello RunbookName pomocí:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Pokud jste nastavili protokoly z více než jeden účet nebo předplatné tooyour pracovního prostoru automatizace, můžete je seskupovat vaše předplatné a účet Automation.  Název účtu Automation může být odvozen od hello prostředků pole v hello hledání JobLogs.  
3. tooopen hello **přidat pravidlo výstrahy** obrazovky, klikněte na tlačítko **výstrahy** hello horní části stránky hello. Další informace o hello možnosti tooconfigure hello výstrahu v [výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Najít všechny úlohy, které byly dokončeny s chybami
Kromě toho tooalerting o selhání, můžete najít při úlohy runbooku se neukončující chybu. V těchto případech prostředí PowerShell vytvoří chybový proud, ale hello neukončující chyby způsobit, že vaše toosuspend úlohy nebo selhání.    

1. V pracovním prostoru analýzy protokolů, klikněte na tlačítko **hledání protokolů**.
2. V poli hello dotazů, zadejte `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` a pak klikněte na **vyhledávání**.

### <a name="view-job-streams-for-a-job"></a>Zobrazení datové proudy úlohy pro úlohu
Když ladíte úlohu, můžete také toolook do datové proudy úlohy hello.  Hello následující dotaz zobrazí všechny datové proudy hello pro jednu úlohu s identifikátorem GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Zobrazit stav historie úlohy
Nakonec můžete toovisualize historii úlohy v čase.  Tento dotaz toosearch pro hello stav úloh můžete použít v čase.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS historie úlohy stavu grafu](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Souhrn
Odesláním vaší automatizace úlohy stavu a datový proud dat tooLog Analytics můžete získat lepší přehled o hello stav automatizace úloh podle:
+ Nastavení výstrah toonotify můžete při se vyskytl problém
+ Pomocí vlastních zobrazení a hledání dotazy toovisualize vaše výsledky sady runbook, stav úlohy sady runbook a další související klíče indikátory nebo metriky.  

Analýzy protokolů poskytuje větší provozní viditelnost tooyour automatizace úloh a může pomoct adresu incidenty rychlejší.  

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o tom, jak tooconstruct jiný vyhledávací dotazy a zkontrolujte hello automatizace úlohy protokoly s analýzy protokolů, najdete v části [hledání přihlásit analýzy protokolů](../log-analytics/log-analytics-log-searches.md)
* jak zjistit, toocreate a načtení výstupní a chybové zprávy ze sady runbook, toounderstand [Runbook výstup a zprávy](automation-runbook-output-and-messages.md)
* Další informace o spuštění sady runbook, jak toomonitor úlohy a další technické podrobnosti najdete v tématu toolearn [sledovat úlohy runbooku](automation-runbook-execution.md)
* toolearn informace o OMS analýzy protokolů a datových zdrojů kolekce, najdete v části [shromažďování Azure úložiště dat v přehledu analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
