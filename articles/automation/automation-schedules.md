---
title: "aaaSchedules ve službě Azure Automation | Microsoft Docs"
description: "Plány služeb automatizace jsou použité tooschedule sady runbook v Azure Automation toostart automaticky. Popisuje, jak toocreate a spravovat plánu v tak, aby může automaticky spustit sadu runbook v určitém čase nebo podle plánu opakování."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1c2da639-ad20-4848-920b-88e471b2e1d9
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2016
ms.author: magoedte
ms.openlocfilehash: 888a5d15fd3442a2b8ab18dd8b0eb4ab9ad0c0d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Naplánování runbooku v Azure Automation
tooschedule sady runbook v Azure Automation toostart v zadanou dobu, můžete ho propojit tooone nebo více plánů. Plán může být nakonfigurované tooeither spusťte jednou nebo na nadále hodinové nebo denní plán pro sady runbook ve hello portál Azure classic a pro sady runbook ve hello portálu Azure, můžete také naplánovat je na týdně, měsíčně, konkrétní dny v týdnu hello nebo dnů, za které hello měsíc, nebo určitý den v měsíci hello.  Sada runbook může být propojené toomultiple plány a plán může mít několik tooit runbooků.

> [!NOTE]
> Plány konfigurace Azure Automation DSC aktuálně nepodporují.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell
Hello rutiny v následující tabulce hello jsou použité toocreate a spravovat plány pomocí prostředí Windows PowerShell ve službě Azure Automation. Se dodávají jako součást hello [modul Azure PowerShell](/powershell/azure/overview).

| Rutiny | Popis |
|:--- |:--- |
| **Rutiny Azure Resource Manager** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Načte plán. |
| [Nové AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Vytvoří nový plán. |
| [Odebrat AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Odebere plánu. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Nastaví hello vlastnosti pro existující plán. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Načte naplánovat sady runbook. |
| [Registrace AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Přidruží sady runbook s plánem. |
| [Zrušit registraci AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Dissociates sady runbook z plánu. |
| **Rutiny Azure Service Management** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Načte plán. |
| [Nové AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Vytvoří nový plán. |
| [Odebrat AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Odebere plánu. |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Nastaví hello vlastnosti pro existující plán. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Načte naplánovat sady runbook. |
| [Registrace AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Přidruží sady runbook s plánem. |
| [Zrušit registraci AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Dissociates sady runbook z plánu. |

## <a name="creating-a-schedule"></a>Vytvoření plánu
V hello portál Azure, můžete vytvořit nový plán pro sady runbook na portálu classic hello, nebo pomocí prostředí Windows PowerShell. Máte také možnost hello vytvoření nového plánu, když připojujete runbook tooa plánu pomocí hello Azure classic nebo portálu Azure.

> [!NOTE]
> Služby Azure Automation bude používat nejnovější moduly hello ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.  tooavoid sady runbook a hello, které mají vliv procesy jejich automatizovat, měli byste nejdřív otestovat všechny sady runbook, které jste propojili plány s účet Automation, který je vyhrazený pro testování.  To se ověření vašeho naplánované sady runbook pokračovat toowork správně, a pokud ne, můžete další řešení a aplikovat všechny změny požadované před migrací tooproduction verze sady runbook hello aktualizovat.  
>  Účtu Automation, nezískáte automaticky nové verze modulů Pokud budete mít aktualizovat ručně tak, že vyberete hello [moduly Azure aktualizace](automation-update-azure-modules.md) možnost hello **moduly** okno. 
>  

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a>toocreate nového plánu v hello portálu Azure
1. V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.
2. Klikněte na tlačítko hello **plány** dlaždice tooopen hello **plány** okno.
3. Klikněte na tlačítko **přidat plán** hello horní části okna hello.
4. Na hello **nový plán** okno, zadejte **název** a volitelně **popis** pro hello nový plán.
5. Vyberte, zda plán hello se spustí jednou, nebo podle plánu opakovaném výběrem **jednou** nebo **opakování**.  Pokud vyberete **jednou** zadejte **počáteční čas** a pak klikněte na **vytvořit**.  Pokud vyberete **opakování**, zadejte **počáteční čas** a četnost hello pro interval hello runbook toorepeat - nástrojem **hodinu**, **den**, **týden**, nebo pomocí **měsíc**.  Pokud vyberete **týden** nebo **měsíc** z rozevíracího seznamu hello hello **opakování možnost** se zobrazí v okně hello a při výběru, hello **opakování možnost** zobrazí okno a hello den v týdnu můžete vybrat, pokud jste vybrali **týden**.  Pokud jste vybrali **měsíc**, můžete **dny v týdnu** nebo konkrétní dny v měsíci hello na hello kalendáře a nakonec chcete toorun ho na hello poslední den v měsíci hello nebo Ne a pak klikněte na tlačítko **OK** .   

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a>toocreate nového plánu v hello portál Azure classic
1. V hello portál Azure classic vyberte automatizace a pak vyberte název hello účet Automation.
2. Vyberte hello **prostředky** kartě.
3. V dolní části hello hello okna, klikněte na tlačítko **přidat nastavení**.
4. Klikněte na tlačítko **přidat plán**.
5. Zadejte **název** a volitelně **popis** pro nové schedule.your hello bude spuštěn plán **jednou**, **hodinové**, **Denní**, **týdenní**, nebo **měsíční**.
6. Zadejte **čas spuštění** a další možnosti v závislosti na typu hello plánu, který jste vybrali.

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a>toocreate nového plánu pomocí prostředí Windows PowerShell
Můžete použít hello [New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) rutiny toocreate nový plán ve službě Azure Automation pro classic sady runbook, nebo [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) rutiny pro sady runbook ve hello Azure portál. Je nutné zadat počáteční čas hello hello plánu a četnosti hello, který se má spustit.

Hello následující ukázkové příkazy Zobrazit jak toocreate plán pro hello 15 a 30 v každém měsíci rutinou služby Správce prostředků Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Hello následující vzorové příkazy ukazují, jak toocreate nový plán, spouští každý den ve 3:30 20 leden 2015 počínaje rutiny Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-tooa-runbook"></a>Propojování runbook tooa plán
Sada runbook může být propojené toomultiple plány a plán může mít několik tooit runbooků. Pokud runbook obsahuje parametry, můžete zadat hodnoty pro ně. Zadejte hodnoty všech povinných parametrů a může poskytnout hodnoty pro všechny volitelné parametry.  Tyto hodnoty se použije při každém spuštění runbooku hello podle tohoto plánu.  Můžete připojit hello stejné tooanother plán sad runbook a zadejte jiné hodnoty parametru.

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a>toolink runbook tooa plán s hello portálu Azure
1. V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **Runbooky** dlaždice tooopen hello **Runbooky** okno.
2. Klikněte na název hello hello runbook tooschedule.
3. Pokud hello runbook není aktuálně propojené tooa plán, bude se daný hello možnost toocreate nový plán nebo na odkaz tooan existující plán.  
4. Pokud hello runbook obsahuje parametry, můžete vybrat možnost hello **upravit nastavení spouštění (výchozí: Azure)** a hello **parametry** okno se zobrazí, kde můžete zadat informace hello odpovídajícím způsobem.  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a>toolink runbook tooa plán s hello portál Azure classic
1. V hello portál Azure classic, vyberte **automatizace** a pak klikněte na název hello účet Automation.
2. Vyberte hello **Runbooky** kartě.
3. Klikněte na název hello hello runbook tooschedule.
4. Klikněte na tlačítko hello **plán** kartě.
5. Pokud hello runbook není aktuálně propojené tooa plán, pak budete mít možnost hello příliš**odkaz tooa nový plán** nebo **odkaz tooan existující plán**.  Pokud je hello runbook aktuálně propojené tooa plán, klikněte na tlačítko **odkaz** v hello dolní části okna tooaccess hello tyto možnosti.
6. Pokud hello runbook obsahuje parametry, budete vyzváni k jejich hodnot.  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a>toolink runbook tooa plánu pomocí prostředí Windows PowerShell
Můžete použít hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink classic runbook tooa plán nebo [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) rutiny pro sady runbook ve hello portálu Azure.  Můžete zadat hodnoty pro parametry runbooku hello s parametrem parametry hello. V tématu [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) Další informace o zadání hodnot parametrů.

Hello následující ukázkové příkazy Zobrazit jak toolink plán tooa runbooku pomocí rutiny Azure Resource Manager s parametry.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Hello následující ukázkové příkazy Zobrazit jak toolink plánu pomocí rutiny Azure Service Management s parametry.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Zakázání plánu
Při zakázání plánu všechny runbooky propojené tooit nebude možné spustit na tento plán. Můžete ručně zakázání plánu nebo můžete nastavit dobu vypršení platnosti plány s frekvencí při jejich vytváření. Když je dosaženo času vypršení platnosti hello, hello plán zakázán.

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a>toodisable plánu z hello portálu Azure
1. V hello portál Azure, z vašeho účtu automation, klikněte na tlačítko hello **prostředky** dlaždice tooopen hello **prostředky** okno.
2. Klikněte na tlačítko hello **plány** dlaždice tooopen hello **plány** okno.
3. Klikněte na název hello okno Podrobnosti plánu tooopen hello.
4. Změna **povoleno** příliš**ne**.

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a>toodisable plánu z hello portál Azure classic
Můžete zakázat plán v hello portál Azure classic na stránce hello podrobnosti plánu pro plán hello.

1. V hello portál Azure classic vyberte automatizace a pak klikněte na název hello účet Automation.
2. Vyberte kartu prostředky hello.
3. Klikněte na název hello plán tooopen stránku s jeho podrobnostmi.
4. Změna **povoleno** příliš**ne**.

### <a name="toodisable-a-schedule-with-windows-powershell"></a>toodisable plánu pomocí prostředí Windows PowerShell
Můžete použít hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) rutiny toochange hello vlastnosti existující plán pro classic sadu runbook nebo [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) rutiny pro sady runbook ve hello Azure portál. toodisable hello naplánovat, zadejte **false** pro hello **hodnotu IsEnabled** parametr.

Hello následující ukázkové příkazy Zobrazit jak toodisable plán pro sady runbook pomocí rutiny Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Hello následující vzorové příkazy ukazují, jak hello toodisable plán pomocí rutiny Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Další kroky
* tooget kroky s runbooky ve službě Azure Automation najdete v části [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) 

