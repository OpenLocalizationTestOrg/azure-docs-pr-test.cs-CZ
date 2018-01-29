---
title: "Proměnné prostředky ve službě Azure Automation | Microsoft Docs"
description: "Proměnné prostředky jsou hodnoty, které jsou k dispozici pro všechny runbooky a konfigurace DSC ve službě Azure Automation.  Tento článek vysvětluje podrobnosti proměnné a postupy pro práci s nimi v textové a grafické vytváření."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: e38d2b751090cfdc078de4e8c683c6bb9b48fac3
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
# <a name="variable-assets-in-azure-automation"></a>Proměnné prostředky ve službě Azure Automation

Proměnné prostředky jsou hodnoty, které jsou k dispozici pro všechny runbooky a konfigurace DSC v účtu automation. Budou se dají vytvořit, upravit a načíst z portálu Azure, prostředí Windows PowerShell a v rámci sady runbook nebo konfigurace DSC. Proměnné služeb automatizace jsou užitečné pro následující scénáře:

- Sdílení hodnoty mezi několika runbooky nebo konfigurace DSC.

- Sdílení hodnoty mezi několika úlohami ze stejné sady runbook nebo konfigurace DSC.

- Správa hodnoty z portálu nebo z příkazového řádku prostředí Windows PowerShell, který je používán sady runbook nebo konfigurace DSC, jako je například sada běžné položky konfigurace jako konkrétní seznam názvů virtuálních počítačů, určité skupiny zdrojů, název domény Active Directory, atd.  

Proměnné služeb automatizace jsou trvalé, takže budou nadále k dispozici i v případě, že sada runbook nebo konfigurace DSC se nezdaří.  To také umožňuje nastavit jednom runbooku, který je pak použije v jiném, nebo se používá stejné sady runbook nebo konfigurace DSC při příštím spuštění hodnotu.

Při vytvoření proměnné můžete určit, že se uloží šifrované.  Když je proměnná zašifrovaná, bezpečně se uloží ve službě Azure Automation a jeho hodnotu nelze načíst z [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) rutinu, která se dodává jako součást modulu Azure PowerShell.  Jediným způsobem, že šifrovaná hodnota se dá načíst je z **Get-AutomationVariable** aktivity v sady runbook nebo konfigurace DSC.

> [!NOTE]
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené ve službě Azure Automation pomocí jedinečný klíč, který se vygeneruje pro každý účet automation. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před ukládání o zabezpečený prostředek, klíč pro účet služby automation jsou dešifrována pomocí hlavního certifikátu a pak se použije k zašifrování asset.

## <a name="variable-types"></a>Typy proměnných

Při vytváření proměnné pomocí portálu Azure, musíte zadat datový typ z rozevíracího seznamu, tak na portálu můžete zobrazit příslušný ovládací prvek pro zadání hodnotu proměnné. Proměnná není omezen na tento typ dat, ale musíte nastavit proměnnou pomocí prostředí Windows PowerShell, pokud chcete zadat hodnotu jiného typu. Pokud zadáte **není definována**, bude nastavena hodnotu proměnné **$null**, a je nutné nastavit hodnotu s [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) rutiny nebo **Set-AutomationVariable** aktivity.  Nelze vytvořit nebo změnit hodnotu pro proměnnou komplexní typ na portálu, ale můžete zadat hodnotu libovolného typu pomocí prostředí Windows PowerShell. Komplexní typy bude vrácen jako [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).

Vytváření zatřiďovací tabulky nebo pole a jeho uložením do proměnné můžete ukládat víc hodnot do jedné proměnné.

Následuje seznam proměnných typy, které jsou dostupné ve službě Automation:

* Řetězec
* Integer
* Datum a čas
* Logická hodnota
* Null

## <a name="scripting-the-creation-and-management-of-variables"></a>Vytváření a správu proměnných skriptování

Rutiny v následující tabulce se používají k vytváření a správě proměnných Automation pomocí prostředí Windows PowerShell. Se dodávají jako součást [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

|Rutiny|Popis|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Načte hodnotu existující proměnné.|
|[Nové AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Vytvoří novou proměnnou a nastaví její hodnotu.|
|[Odebrat AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Odebere existující proměnnou.|
|[Set-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|Nastaví hodnotu existující proměnné.|

Aktivity pracovního postupu v následující tabulce se používají pro přístup k proměnným automatizace v sadě runbook. Jsou dostupné pouze pro použití v runbooku nebo konfigurace DSC a se nedodává jako součást modulu Azure PowerShell.

|Aktivity pracovního postupu|Popis|
|:---|:---|
|Get-AutomationVariable|Načte hodnotu existující proměnné.|
|Set-AutomationVariable|Nastaví hodnotu existující proměnné.|

> [!NOTE] 
> Byste neměli používat proměnné v parametru – Name **Get-AutomationVariable** v sady runbook nebo konfigurace DSC, protože to může zkomplikovat zjišťování závislostí mezi runbooky nebo konfigurace DSC a proměnné služeb automatizace v době návrhu.

Funkce v následující tabulce se používají pro přístup k a načíst proměnné v runbooku Python2. 

|Funkce Python2|Popis|
|:---|:---|
|automationassets.get_automation_variable|Načte hodnotu existující proměnné. |
|automationassets.set_automation_variable|Nastaví hodnotu existující proměnné. |

> [!NOTE] 
> Chcete-li přístup funkce asset musí naimportovat modul "automationassets" v horní části runbookem Python.

## <a name="creating-a-new-automation-variable"></a>Vytvoření nové proměnné Automation

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Chcete-li vytvořit nové proměnné pomocí portálu Azure

1. Z vašeho účtu Automation, klikněte na tlačítko **prostředky** dlaždici a potom na **prostředky** vyberte **proměnné**.
2. Na **proměnné** dlaždice, vyberte **přidat proměnnou**.
3. Dokončete možností na **nové proměnné** a klikněte na **vytvořit** uložení nové proměnné.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Chcete-li vytvořit nové proměnné pomocí prostředí Windows PowerShell

[New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) rutina vytvoří novou proměnnou a nastaví počáteční hodnoty. Můžete načíst pomocí hodnota [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx). Pokud je hodnota jednoduchého typu, je vrácena stejného typu. Pokud se jedná o komplexní typ, pak **PSCustomObject** je vrácen.

Následující vzorové příkazy znázorňují postup vytvoření proměnné typu řetězec a pak se vraťte jeho hodnotu.

    New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

Následující vzorové příkazy znázorňují postup vytvoření proměnné s komplexní typ a pak se vraťte jeho vlastnosti. V takovém případě objektu virtuálního počítače z **Get-AzureRmVm** se používá.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Použití proměnné v runbooku nebo konfigurace DSC

Použití **Set-AutomationVariable** aktivity nastavíte hodnotu proměnné automatizace v prostředí PowerShell runbook nebo konfigurace DSC a **Get-AutomationVariable** ho chcete zjistit.  Neměli byste používat **Set-AzureAutomationVariable** nebo **Get-AzureAutomationVariable** rutiny v runbooku nebo konfigurace DSC vzhledem k tomu, že jsou míň efektivní než aktivity pracovního postupu.  Také nelze načíst hodnotu proměnných zabezpečené se **Get-AzureAutomationVariable**.  Jediným způsobem, jak vytvořit nové proměnné z v rámci sady runbook nebo konfigurace DSC se má používat [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) rutiny.


### <a name="textual-runbook-samples"></a>Ukázky textové runbooků

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Nastavení nebo načtení jednoduché hodnotu z proměnné

Následující vzorové příkazy ukazují, jak nastavit a načíst proměnnou v textový. V této ukázce se předpokládá, že proměnné celočíselného typu s názvem *NumberOfIterations* a *NumberOfRunnings* a proměnná řetězcového typu nazvaná *SampleMessage* již byly vytvořeny.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Nastavení nebo načtení komplexní objekt v proměnné

Následující vzorový kód ukazuje, jak aktualizovat proměnnou s komplexní hodnoty v textové runbooku. V této ukázce je načíst virtuální počítač Azure s **Get-AzureVM** a uložit do existující automatizace proměnné.  Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako PSCustomObject.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

V následujícím kódu je hodnota načítají proměnnou a používá ke spuštění virtuálního počítače.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Nastavení nebo načtení kolekce v proměnné

Následující vzorový kód ukazuje, jak používat proměnné s kolekcí komplexní hodnoty v textové runbooku. V této ukázce se načítají více virtuálními počítači Azure s **Get-AzureVM** a uložit do existující automatizace proměnné.  Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako soubor PSCustomObjects.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

V následujícím kódu je kolekce načítají proměnnou a používá ke spuštění každého virtuálního počítače.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }
    
#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Nastavení nebo načtení proměnnou v Python2
Následující vzorový kód ukazuje, jak používat proměnné, nastavte proměnnou a zpracování výjimky pro neexistující proměnné v runbooku Python2.

    import automationassets
    from automationassets import AutomationAssetNotFound

    # get a variable
    value = automationassets.get_automation_variable("test-variable")
    print value

    # set a variable (value can be int/bool/string)
    automationassets.set_automation_variable("test-variable", True)
    automationassets.set_automation_variable("test-variable", 4)
    automationassets.set_automation_variable("test-variable", "test-string")

    # handle a non-existent variable exception
    try:
        value = automationassets.get_automation_variable("non-existing variable")
    except AutomationAssetNotFound:
        print "variable not found"


### <a name="graphical-runbook-samples"></a>Ukázky grafický runbook

V grafický runbook přidáte **Get-AutomationVariable** nebo **Set-AutomationVariable** kliknutím pravým tlačítkem myši na proměnnou v podokně knihovna grafického editoru a výběrem aktivity, které chcete.

![Přidání proměnné na plátno](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Nastavení hodnot v proměnné
Následující obrázek znázorňuje ukázkové aktivity se aktualizovat proměnnou s hodnotou jednoduché v grafický runbook. V této ukázce je načíst jediný virtuální počítač Azure s **Get-AzureRmVM** a název počítače je uložen do existující automatizace proměnnou s typem řetězce.  Nezávisle na tom, zda [odkaz je kanál, nebo pořadí](automation-graphical-authoring-intro.md#links-and-workflow) vzhledem k tomu, že pouze Očekáváme, že jeden objekt ve výstupu.

![Sada jednoduché proměnné](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Další kroky

* Další informace o připojení aktivity společně v vytváření grafického obsahu najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)
* První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md). 

