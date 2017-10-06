---
title: "aaaForward tooOMS generování sestav dat analýzy protokolů Azure Automation DSC. | Microsoft Docs"
description: "Tento článek ukazuje, jak toosend požadovaného stavu generování sestav dat tooMicrosoft Operations Management Suite Log Analytics toodeliver další aspekty a správy konfigurace (DSC)."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.openlocfilehash: 21f78d5549d53ba3d7e237f55d9086f380cf3351
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="forward-azure-automation-dsc-reporting-data-toooms-log-analytics"></a>Předávání sestav dat tooOMS analýzy protokolů Azure Automation DSC.

Automatizace můžete odeslat DSC uzlu stav dat tooyour pracovní prostor analýzy protokolů Microsoft Operations Management Suite (OMS).  
Stav dodržování předpisů je viditelný v hello portál Azure nebo v prostředí PowerShell pro uzly a pro jednotlivé prostředky DSC v konfigurace uzlu. Pomocí analýzy protokolů můžete:

* Získat informace o dodržování předpisů pro spravované uzly a jednotlivé prostředky
* Aktivovat e-mail nebo upozornění na základě stavu dodržování předpisů
* Zápis pokročilými dotazy napříč spravované uzly
* Vazbu mezi stav dodržování předpisů v účtech Automation
* Vizualizace historii uzlu dodržování předpisů v čase

## <a name="prerequisites"></a>Požadavky

toostart odesílání Automation DSC sestavy tooLog Analytics, budete potřebovat:

* Hello listopadu 2016 nebo novější vydání [prostředí Azure PowerShell](/powershell/azure/overview) (v2.3.0).
* Účet Azure Automation. Další informace najdete v tématu [Začínáme s Azure Automation.](automation-offering-get-started.md)
* Pracovní prostor analýzy protokolů se **automatizace a řízení** nabídky služeb. Další informace najdete v tématu [začít pracovat s analýzy protokolů](../log-analytics/log-analytics-get-started.md).
* Nejméně jeden uzel Azure Automation DSC. Další informace najdete v tématu [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md) 

## <a name="set-up-integration-with-log-analytics"></a>Nastavení integrace s analýzy protokolů

toobegin importování dat z Azure Automation DSC do analýzy protokolů, dokončení hello následující kroky:

1. Přihlaste se tooyour účet Azure v prostředí PowerShell. V tématu [přihlásit pomocí prostředí Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Získat hello _ResourceId_ účtu automation tak, že spustíte následující příkaz prostředí PowerShell hello: (Pokud máte více než jeden účet automation, zvolte hello _ResourceID_ hello účtu chcete tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Získat hello _ResourceId_ pracovního prostoru analýzy protokolů tak, že spustíte následující příkaz prostředí PowerShell hello: (Pokud máte více než jednoho pracovního prostoru, vyberte hello _ResourceID_ pro pracovní prostor hello chcete tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. Hello spusťte následující příkaz prostředí PowerShell, nahraďte `<AutomationResourceId>` a `<WorkspaceResourceId>` s hello _ResourceId_ hodnoty z každé hello předchozí kroky:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Pokud chcete toostop importování dat z Azure Automation DSC do analýzy protokolů, spusťte následující příkaz prostředí PowerShell hello.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-hello-dsc-logs"></a>Zobrazit protokoly hello DSC

Po nastavení integrace s analýzy protokolů pro data Automation DSC, **hledání protokolů** tlačítko se zobrazí na hello **uzly DSC** okno účtu automation. Klikněte na tlačítko hello **hledání protokolů** tlačítko tooview hello protokoly pro data uzlu DSC.

![Tlačítko vyhledat protokolu](media/automation-dsc-diagnostics/log-search-button.png)

Hello **hledání protokolů** otevře se okno a zobrazí **DscNodeStatusData** operaci pro každý uzel DSC a **DscResourceStatusData** operace pro každou [DSC prostředek](https://msdn.microsoft.com/powershell/dsc/resources) volat v hello uzlu Konfigurace použité toothat uzlu.

Hello **DscResourceStatusData** operace obsahuje informace o chybě pro veškeré prostředky DSC, které se nezdařilo.

Klikněte na tlačítko jednotlivých operací ve hello seznamu toosee hello data pro tuto operaci.

Můžete také zobrazit protokoly hello [hledání v analýzy protokolů. V tématu [najít data pomocí protokolu hledání](../log-analytics/log-analytics-log-searches.md).
Typ hello následující dotaz toofind vaše DSC protokoly:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

Můžete také hello dotazu zúžit název operace hello. Například: ' typ = AzureDiagnostics ResourceProvider = "MICROSOFT. Kategorie AUTOMATIZACE"="DscNodeStatus"OperationName ="DscNodeStatusData"

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>Odeslat e-mail, když se nezdaří kontrola dodržování předpisů DSC

Jeden z našich žádostem nejvyšší zákazníků je pro hello možnost toosend e-mailu nebo textový při vyskytne problém s konfigurací DSC.   

pravidlo výstrahy, začněte vytvořením hledání protokolů pro hello DSC sestavy záznamy, které by měla vyvolat výstrahu hello toocreate.  Klikněte na tlačítko hello **výstraha** tlačítko toocreate a nakonfigurujte pravidlo výstrahy hello.

1. Na stránce Přehled protokolu Analytics hello, klikněte na tlačítko **hledání protokolů**.
1. Vytvoření vyhledávací dotaz protokolu pro upozornění zadáním hello následující hledání do pole hello dotazu:`Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`

  Pokud jste nastavili protokoly z více než jeden účet nebo předplatné tooyour pracovního prostoru automatizace, můžete je seskupovat vaše předplatné a účet Automation.  
  Název účtu Automation může být odvozen od hello prostředků pole v hello hledání DscNodeStatusData.  
1. tooopen hello **přidat pravidlo výstrahy** obrazovky, klikněte na tlačítko **výstrahy** hello horní části stránky hello. Další informace o hello možnosti tooconfigure hello výstrahy, najdete v části [výstrahy v analýzy protokolů](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Vyhledání chybných prostředky DSC pro všechny uzly

Jednou z výhod použití analýzy protokolů je, že můžete vyhledat selhání kontroly mezi uzly.
toofind všechny instance prostředků DSC, které se nezdařilo.

1. Na stránce Přehled protokolu Analytics hello, klikněte na tlačítko **hledání protokolů**.
1. Vytvoření vyhledávací dotaz protokolu pro upozornění zadáním hello následující hledání do pole hello dotazu:`Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`

### <a name="view-historical-dsc-node-status"></a>Zobrazit historická DSC uzlu stav

Nakonec můžete toovisualize historii stavu uzlu DSC v čase.  
Tento dotaz toosearch můžete použít pro hello stav váš stav uzlu DSC v čase.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Tato akce zobrazí graf hello uzlu stav průběhu času.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics

Diagnostika z Azure Automation vytvoří dvě kategorie záznamů v analýzy protokolů.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas, kdy byla spuštěna kontrola dodržování zásad hello. |
| OperationName |DscNodeStatusData |
| ResultType |Zda je uzel hello kompatibilní. |
| NodeName_s |Název Hello hello spravovaný uzel. |
| NodeComplianceStatus_s |Zda je uzel hello kompatibilní. |
| DscReportStatus |Zkontrolujte, zda dodržování předpisů hello proběhla úspěšně. |
| ConfigurationMode | Jak hello konfigurace je použité toohello uzlu. Možné hodnoty jsou __"ApplyOnly"__,__"ApplyandMonitior"__, a __"ApplyandAutoCorrect"__. <ul><li>__ApplyOnly__: DSC hello konfigurace a nemá žádnou další akci, pokud nová konfigurace se posune toohello cílový uzel nebo když je stažen novou konfiguraci ze serveru. Po počáteční aplikaci novou konfiguraci DSC nekontroluje odlišily z dříve nakonfigurované stavu. DSC pokusí tooapply hello konfigurace, dokud nebude úspěšná, až poté __ApplyOnly__ projeví. </li><li> __ApplyAndMonitor__: Toto je výchozí hodnota hello. Hello LCM se vztahuje na všechny nové konfigurace. Po počáteční aplikaci novou konfiguraci Pokud cílový uzel hello drifts ze hello požadovaného stavu sestavy DSC hello nesoulad mezi databází protokoly. DSC pokusí tooapply hello konfigurace, dokud nebude úspěšná, až poté __ApplyAndMonitor__ projeví.</li><li>__ApplyAndAutoCorrect__: platí všechny nové konfigurace DSC. Po počáteční aplikaci novou konfiguraci Pokud cílový uzel hello drifts ze hello požadovaného stavu DSC sestavy hello nesoulad mezi databází protokoly a pak znovu použije aktuální konfiguraci hello.</li></ul> |
| HostName_s | Název Hello hello spravovaný uzel. |
| IP adresa | IPv4 adresu Hello hello spravovaný uzel. |
| Kategorie | DscNodeStatus |
| Prostředek | Název Hello hello účet Azure Automation. |
| Tenant_g | Identifikátor GUID, který identifikuje hello klienta pro hello volajícího. |
| NodeId_g |Identifikátor GUID, který identifikuje hello spravovaný uzel. |
| DscReportId_g |Identifikátor GUID, který identifikuje hello sestavy. |
| LastSeenTime_t |Datum a čas, kdy posledního zobrazení sestavy hello. |
| ReportStartTime_t |Datum a čas spuštění sestavy hello. |
| ReportEndTime_t |Datum a čas dokončení hello sestavy. |
| NumberOfResources_d |volá se v uzlu toohello použitá konfigurace hello Hello počet prostředků DSC. |
| SourceSystem | Jak analýzy protokolů shromážděných dat hello. Vždy *Azure* Azure Diagnostics. |
| ID prostředku |Určuje účet Azure Automation hello. |
| ResultDescription | Hello popis pro tuto operaci. |
| SubscriptionId | Hello předplatné Azure Id (GUID) pro hello účet Automation. |
| ResourceGroup | Název skupiny prostředků hello hello účet Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Identifikátor GUID, který je hello Id korelace hello sestavy dodržování předpisů. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Vlastnost | Popis |
| --- | --- |
| TimeGenerated |Datum a čas, kdy byla spuštěna kontrola dodržování zásad hello. |
| OperationName |DscResourceStatusData|
| ResultType |Zda je prostředek hello kompatibilní. |
| NodeName_s |Název Hello hello spravovaný uzel. |
| Kategorie | DscNodeStatus |
| Prostředek | Název Hello hello účet Azure Automation. |
| Tenant_g | Identifikátor GUID, který identifikuje hello klienta pro hello volajícího. |
| NodeId_g |Identifikátor GUID, který identifikuje hello spravovaný uzel. |
| DscReportId_g |Identifikátor GUID, který identifikuje hello sestavy. |
| DscResourceId_s |Hello název instance prostředku hello DSC. |
| DscResourceName_s |Hello název prostředku hello DSC. |
| DscResourceStatus_s |Jestli hello prostředek DSC se dodržování předpisů. |
| DscModuleName_s |Hello název modulu prostředí PowerShell text hello, který obsahuje prostředek DSC hello. |
| DscModuleVersion_s |Hello verze modulu PowerShell text hello, který obsahuje prostředek DSC hello. |
| DscConfigurationName_s |Název Hello hello konfigurace použít toohello uzlu. |
| ErrorCode_s | Kód chyby Hello, pokud hello prostředků se nezdařilo. |
| ErrorMessage_s |Hello chybová zpráva, pokud hello prostředků se nezdařilo. |
| DscResourceDuration_d |Hello čas v sekundách, které byly spuštěny hello DSC prostředků. |
| SourceSystem | Jak analýzy protokolů shromážděných dat hello. Vždy *Azure* Azure Diagnostics. |
| ID prostředku |Určuje účet Azure Automation hello. |
| ResultDescription | Hello popis pro tuto operaci. |
| SubscriptionId | Hello předplatné Azure Id (GUID) pro hello účet Automation. |
| ResourceGroup | Název skupiny prostředků hello hello účet Automation. |
| ResourceProvider | SPOLEČNOSTI MICROSOFT. AUTOMATIZACE |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |Identifikátor GUID, který je hello Id korelace hello sestavy dodržování předpisů. |

## <a name="summary"></a>Souhrn

Odesláním vaší Automation DSC data tooLog Analytics můžete získat lepší přehled o stavu hello uzlů Automation DSC podle:

* Nastavení výstrah toonotify můžete při se vyskytl problém
* Pomocí vlastních zobrazení a hledání dotazy toovisualize vaše výsledky sady runbook, stav úlohy sady runbook a další související klíče indikátory nebo metriky.  

Log Analytics poskytuje větší provozní viditelnost tooyour Automation DSC data a může pomoct adresu incidenty rychleji.  

## <a name="next-steps"></a>Další kroky

* toolearn informace o tom, jak tooconstruct jiný vyhledávací dotazy a zkontrolujte hello Automation DSC protokoly s analýzy protokolů, najdete v části [hledání přihlásit analýzy protokolů](../log-analytics/log-analytics-log-searches.md)
* toolearn Další informace o použití Azure Automation DSC, najdete v části [Začínáme s Azure Automation DSC.](automation-dsc-getting-started.md)
* toolearn informace o OMS analýzy protokolů a datových zdrojů kolekce, najdete v části [shromažďování Azure úložiště dat v přehledu analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)

