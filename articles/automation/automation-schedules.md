---
title: "Plánů ve službě Azure Automation | Microsoft Docs"
description: "Plány služeb automatizace se používají k plánování sady runbook ve službě Azure Automation na automatické spouštění. Popisuje, jak vytvořit a spravovat plánu v tak, aby může automaticky spustit sadu runbook v určitém čase nebo podle plánu opakování."
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
ms.openlocfilehash: 140bea93c4563666e8cfdf356eaf87500c1aca8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Naplánování runbooku v Azure Automation
Pokud chcete naplánovat spuštění runbooku ve službě Azure Automation spustit v zadanou dobu, můžete ho propojit s jedním nebo víc plány. Plán můžete nakonfigurovat pro spuštění jednou nebo na nadále hodinové nebo denní plán pro sady runbook na portálu Azure classic a pro sady runbook na portálu Azure, můžete také naplánovat je na týdně, měsíčně, konkrétní dny v týdnu nebo dny v měsíci nebo konkrétní den v měsíci.  Sady runbook mohou být spojeny s víc plány a plán může mít víc runbooků.

> [!NOTE]
> Plány konfigurace Azure Automation DSC aktuálně nepodporují.
> 
> 

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell
Rutiny v následující tabulce se používají k vytváření a správě plánů v prostředí Windows PowerShell ve službě Azure Automation. Se dodávají jako součást [modul Azure PowerShell](/powershell/azure/overview).

| Rutiny | Popis |
|:--- |:--- |
| **Rutiny Azure Resource Manager** | |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Načte plán. |
| [Nové AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Vytvoří nový plán. |
| [Odebrat AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Odebere plánu. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Nastaví vlastnosti pro existující plán. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Načte naplánovat sady runbook. |
| [Registrace AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Přidruží sady runbook s plánem. |
| [Zrušit registraci AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Dissociates sady runbook z plánu. |
| **Rutiny Azure Service Management** | |
| [Get-AzureAutomationSchedule](/powershell/module/azure/get-azureautomationschedule?view=azuresmps-3.7.0) |Načte plán. |
| [Nové AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) |Vytvoří nový plán. |
| [Odebrat AzureAutomationSchedule](/powershell/module/azure/remove-azureautomationschedule?view=azuresmps-3.7.0) |Odebere plánu. |
| [Set-AzureAutomationSchedule](/powershell/module/azure/set-azureautomationschedule?view=azuresmps-3.7.0) |Nastaví vlastnosti pro existující plán. |
| [Get-AzureAutomationScheduledRunbook](/powershell/module/azure/get-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Načte naplánovat sady runbook. |
| [Registrace AzureAutomationScheduledRunbook](/powershell/module/azure/register-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Přidruží sady runbook s plánem. |
| [Zrušit registraci AzureAutomationScheduledRunbook](/powershell/module/azure/unregister-azureautomationscheduledrunbook?view=azuresmps-3.7.0) |Dissociates sady runbook z plánu. |

## <a name="creating-a-schedule"></a>Vytvoření plánu
Můžete vytvořit nový plán pro sady runbook na portálu Azure, na klasickém portálu nebo pomocí prostředí Windows PowerShell. Máte také možnost vytvořit nový plán, když připojujete runbook k plánu pomocí portálu Azure classic nebo Azure.

> [!NOTE]
> Služby Azure Automation bude používat nejnovější modulů ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.  Nechcete-li vaše sady runbook a procesy, které budou automatizovat, které mají vliv, byste měli nejprve otestovat všechny sady runbook, které jste propojili plány s účet Automation, který je vyhrazený pro testování.  To se ověření vašeho naplánované sady runbook i nadále fungovat správně a pokud ne, můžete další řešení a použít všechny změny požadované před migrací verze aktualizované sady runbook do produkčního prostředí.  
>  Účtu Automation, nezískáte automaticky nové verze modulů Pokud budete mít aktualizovat ručně tak, že vyberete [moduly Azure aktualizace](automation-update-azure-modules.md) možnost z **moduly** okno. 
>  

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Chcete-li vytvořit nový plán na portálu Azure
1. Na portálu Azure z vašeho účtu automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.
2. Klikněte **plány** dlaždici otevřete **plány** okno.
3. Klikněte na tlačítko **přidat plán** v horní části okna.
4. Na **nový plán** okno, zadejte **název** a volitelně **popis** pro nový plán.
5. Vyberte jestli plán poběží jednorázově nebo podle plánu opakovaném výběrem **jednou** nebo **opakování**.  Pokud vyberete **jednou** zadejte **počáteční čas** a pak klikněte na **vytvořit**.  Pokud vyberete **opakování**, zadejte **počáteční čas** a jak často chcete runbooku opakovat - nástrojem frekvenci **hodinu**, **den**, **týden**, nebo pomocí **měsíc**.  Pokud vyberete **týden** nebo **měsíc** z rozevíracího seznamu **opakování možnost** se zobrazí v okně a při výběru, **opakování možnost** zobrazí okno a den v týdnu můžete vybrat, pokud jste vybrali **týden**.  Pokud jste vybrali **měsíc**, můžete **dny v týdnu** nebo konkrétní dny v měsíci v kalendáři a nakonec chcete spustit poslední den v měsíci, nebo Ne, a potom klikněte na **OK**.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Chcete-li vytvořit nový plán na portálu Azure classic
1. Na portálu Azure classic vyberte automatizace a pak vyberte název účtu Automation.
2. Vyberte **prostředky** kartě.
3. V dolní části okna klikněte na tlačítko **přidat nastavení**.
4. Klikněte na tlačítko **přidat plán**.
5. Zadejte **název** a volitelně **popis** pro nové schedule.your bude spuštěn plán **jednou**, **hodinové**, **denní**, **týdenní**, nebo **měsíční**.
6. Zadejte **čas spuštění** a další možnosti v závislosti na typu plánu, který jste vybrali.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>K vytvoření nového plánu pomocí prostředí Windows PowerShell
Můžete použít [New-AzureAutomationSchedule](/powershell/module/azure/new-azureautomationschedule?view=azuresmps-3.7.0) vytvořte nový plán ve službě Azure Automation pro classic sady runbook, nebo [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) rutiny pro sady runbook na portálu Azure. Musíte zadat čas zahájení pro plán a četnosti, který se má spustit.

Následující vzorové příkazy ukazují, jak vytvořit plán pro 15. dne a 30 v každém měsíci rutinou služby Správce prostředků Azure.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

Následující vzorové příkazy ukazují, jak vytvořit nový plán, který spouští každý den ve 3:30 20 leden 2015 počínaje rutiny Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Propojování plánu k sadě runbook
Sady runbook mohou být spojeny s víc plány a plán může mít víc runbooků. Pokud runbook obsahuje parametry, můžete zadat hodnoty pro ně. Zadejte hodnoty všech povinných parametrů a může poskytnout hodnoty pro všechny volitelné parametry.  Tyto hodnoty se použije při každém spuštění runbooku podle tohoto plánu.  Můžete přiřadit stejné sady runbook jiný plán a zadejte jiné hodnoty parametru.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Pro připojení plánu k sadě runbook pomocí portálu Azure
1. Na portálu Azure z vašeho účtu automation, klikněte na tlačítko **Runbooky** dlaždici otevřete **sady Runbook** okno.
2. Klikněte na název sady runbook k plánu.
3. Pokud sada runbook není aktuálně propojena k plánu, pak budete mít možnost vytvořit nový plán nebo odkaz na existující plán.  
4. Pokud má runbook parametry, můžete vybrat možnost **upravit nastavení spouštění (výchozí: Azure)** a **parametry** okno se zobrazí, kde můžete zadat informace odpovídajícím způsobem.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Pro připojení plánu k sadě runbook pomocí portálu Azure classic
1. Na portálu Azure classic, vyberte **automatizace** a pak klikněte na název účtu Automation.
2. Vyberte **Runbooky** kartě.
3. Klikněte na název sady runbook k plánu.
4. Klikněte **plán** kartě.
5. Pokud sada runbook není aktuálně propojený s plán, pak budete mít možnost **odkaz na nový plán** nebo **odkaz na existující plán**.  Pokud runbook už připojený k plánu, klikněte na tlačítko **odkaz** v dolní části okna pro přístup k tyto možnosti.
6. Pokud sada runbook obsahuje parametry, zobrazí se výzva pro jejich hodnot.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Pro připojení plánu k sadě runbook pomocí prostředí Windows PowerShell
Můžete použít [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) pro připojení plánu k sadě runbook classic nebo [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) rutiny pro sady runbook na portálu Azure.  S parametrem parametrů můžete zadat hodnoty pro parametry runbooku. V tématu [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) Další informace o zadání hodnot parametrů.

Následující vzorové příkazy ukazují, jak pro připojení plánu k sadě runbook s parametry rutinou služby Správce prostředků Azure.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
Následující vzorové příkazy ukazují, jak propojit plán s rutinou služby Azure Service Management s parametry.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Zakázání plánu
Při zakázání plánu všechny runbooky propojené s ho nebude možné spustit na tento plán. Můžete ručně zakázání plánu nebo můžete nastavit dobu vypršení platnosti plány s frekvencí při jejich vytváření. Když je dosaženo času vypršení platnosti, bude plán zakázán.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Zakázání plánu z portálu Azure
1. Na portálu Azure z vašeho účtu automation, klikněte **prostředky** dlaždici otevřete **prostředky** okno.
2. Klikněte **plány** dlaždici otevřete **plány** okno.
3. Klikněte na název plánu a otevřete okno Podrobnosti.
4. Změna **povoleno** k **ne**.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Zakázání plánu z portálu Azure classic
Můžete zakázat plán na portálu Azure classic na stránce Podrobnosti plánu pro plán.

1. Na portálu Azure classic vyberte automatizace a pak klikněte na název účtu Automation.
2. Vyberte kartu prostředky.
3. Klikněte na název plán, který chcete otevřít stránku s jeho podrobnostmi.
4. Změna **povoleno** k **ne**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Zakázání plánu pomocí prostředí Windows PowerShell
Můžete použít [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) rutiny změnit vlastnosti existující plán pro classic sadu runbook nebo [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) rutiny pro sady runbook na portálu Azure. Pokud chcete zakázat plán, zadejte **false** pro **hodnotu IsEnabled** parametr.

Následující vzorové příkazy ukazují, jak zakázat plán pro sady runbook pomocí rutiny Azure Resource Manager.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

Následující vzorové příkazy znázorňují postup zakázání plánu pomocí rutiny Azure Service Management.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Další kroky
* Chcete-li začít pracovat se sadami runbook ve službě Azure Automation, přečtěte si téma [spuštění sady Runbook ve službě Azure Automation](automation-starting-a-runbook.md) 

