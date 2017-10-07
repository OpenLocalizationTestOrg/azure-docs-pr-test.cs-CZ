---
title: "aaaVariable prostředky ve službě Azure Automation | Microsoft Docs"
description: "Proměnné prostředky jsou hodnoty, které jsou k dispozici tooall runboocích a konfiguracích DSC ve službě Azure Automation.  Tento článek vysvětluje podrobnosti hello proměnných a jak toowork s nimi v textové a grafické vytváření."
services: automation
documentationcenter: 
author: mgoedtel
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
ms.openlocfilehash: f9daa49fc1dc883ffb218a9adf26e36df1d6bb27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="variable-assets-in-azure-automation"></a>Proměnné prostředky ve službě Azure Automation

Proměnné prostředky jsou hodnoty, které jsou k dispozici tooall runboocích a konfiguracích DSC v účtu automation. Budou se dají vytvořit, upravit a načíst prostřednictvím hello portál Azure, prostředí Windows PowerShell a v rámci sady runbook nebo konfigurace DSC. Proměnné služeb automatizace jsou užitečné pro hello následující scénáře:

- Sdílení hodnoty mezi několika runbooky nebo konfigurace DSC.

- Sdílení hodnoty mezi několika úlohami hello stejné sady runbook nebo konfigurace DSC.

- Správa hodnoty z portálu hello nebo z příkazového řádku prostředí Windows PowerShell hello, který je používán sady runbook nebo konfigurace DSC, jako je například sada běžné položky konfigurace jako konkrétní seznam názvů virtuálních počítačů, určité skupiny zdrojů, název domény Active Directory, atd.  

Proměnné služeb automatizace jsou nastavené jako trvalé, takže jsou toobe k dispozici i v případě, že sada runbook hello nebo konfigurace DSC se nezdaří.  To také umožňuje hodnotu toobe, nastavte jednom runbooku, který je pak použije v jiném nebo hello používá stejné sady runbook nebo hello konfigurace DSC při příštím spuštění.

Při vytvoření proměnné můžete určit, že se uloží šifrované.  Když je proměnná zašifrovaná, bezpečně se uloží ve službě Azure Automation a jeho hodnotu nelze načíst z hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) rutinu, která se dodává jako součást modulu Azure PowerShell hello.  Hello jediným způsobem, že šifrovaná hodnota se dá načíst je z hello **Get-AutomationVariable** aktivity v sady runbook nebo konfigurace DSC.

> [!NOTE]
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.

## <a name="variable-types"></a>Typy proměnných

Při vytváření proměnné pomocí portálu Azure hello, musíte zadat datový typ z rozevíracího seznamu hello, takže hello portálu můžete zobrazit hello vhodný ovládací prvek pro zadání hodnoty proměnné hello. Proměnná Hello není s omezeným přístupem toothis datového typu, ale musíte nastavit hello proměnné pomocí prostředí Windows PowerShell, pokud chcete, aby toospecify hodnotu jiného typu. Pokud zadáte **není definována**, pak hello hello proměnné se hodnota příliš**$null**, a je nutné nastavit hodnotu hello s hello [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) rutiny nebo **Set-AutomationVariable** aktivity.  Nelze vytvořit nebo změnit hello hodnotu pro komplexní proměnná typu hello portálu, ale můžete zadat hodnotu libovolného typu pomocí prostředí Windows PowerShell. Komplexní typy bude vrácen jako [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).

Vytváření zatřiďovací tabulky nebo pole a jeho uložením toohello proměnné můžete uložit více hodnot tooa jednu proměnnou.

seznam proměnných typy, které jsou dostupné ve službě Automation se Hello následující:

* Řetězec
* Integer
* Data a času
* Logická hodnota
* Hodnotu Null

## <a name="cmdlets-and-workflow-activities"></a>Rutiny a pracovní postup aktivity

Hello rutiny v následující tabulce hello jsou použité toocreate a správě proměnných Automation pomocí prostředí Windows PowerShell. Se dodávají jako součást hello [modul Azure PowerShell](../powershell-install-configure.md) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

|Rutiny|Popis|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Načte hodnotu existující proměnné hello.|
|[Nové AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Vytvoří novou proměnnou a nastaví její hodnotu.|
|[Odebrat AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Odebere existující proměnnou.|
|[Set-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|Nastaví hello hodnotu pro existující proměnnou.|

aktivity pracovního postupu Hello v následující tabulce hello jsou použité tooaccess automatizace proměnné v runbooku. Jsou dostupné pouze pro použití v runbooku nebo konfigurace DSC a se nedodává jako součást modulu Azure PowerShell hello.

|Aktivity pracovního postupu|Popis|
|:---|:---|
|Get-AutomationVariable|Načte hodnotu existující proměnné hello.|
|Set-AutomationVariable|Nastaví hello hodnotu pro existující proměnnou.|

> [!NOTE] 
> Byste neměli používat proměnné v hello – název parametru **Get-AutomationVariable** v sady runbook nebo konfigurace DSC, protože to může zkomplikovat zjišťování závislostí mezi runbooky nebo konfigurace DSC a automatizace proměnné v době návrhu.

## <a name="creating-a-new-automation-variable"></a>Vytvoření nové proměnné Automation

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a>toocreate novou proměnnou s hello portálu Azure

1. Z vašeho účtu Automation, klikněte na tlačítko hello **prostředky** dlaždici a potom na hello **prostředky** vyberte **proměnné**.
2. Na hello **proměnné** dlaždice, vyberte **přidat proměnnou**.
3. Dokončete hello možností na hello **nové proměnné** a klikněte na **vytvořit** uložení nové proměnné hello.

### <a name="toocreate-a-new-variable-with-windows-powershell"></a>toocreate nové proměnné pomocí prostředí Windows PowerShell

Hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) rutina vytvoří novou proměnnou a nastaví počáteční hodnoty. Můžete načíst pomocí hodnota hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx). Pokud hodnota hello je jednoduchý typ, je vrácena stejného typu. Pokud se jedná o komplexní typ, pak **PSCustomObject** je vrácen.

Hello následující ukázkové příkazy Zobrazit jak toocreate proměnné typu řetězec a pak se vraťte jeho hodnotu.

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

Hello následující ukázkové příkazy Zobrazit jak toocreate proměnná s komplexní typ a pak se vraťte jeho vlastnosti. V takovém případě objektu virtuálního počítače z **Get-AzureRmVm** se používá.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Použití proměnné v runbooku nebo konfigurace DSC

Použití hello **Set-AutomationVariable** aktivity tooset hello hodnoty proměnné automatizace sady runbook nebo konfigurace DSC a hello **Get-AutomationVariable** tooretrieve ho.  Neměli byste používat hello **Set-AzureAutomationVariable** nebo **Get-AzureAutomationVariable** rutiny v runbooku nebo konfigurace DSC vzhledem k tomu, že jsou míň efektivní než hello aktivity pracovního postupu.  Také nelze načíst hodnotu hello zabezpečené proměnných s **Get-AzureAutomationVariable**.  Hello pouze způsob toocreate nové proměnné z v rámci sady runbook nebo konfigurace DSC je toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) rutiny.


### <a name="textual-runbook-samples"></a>Ukázky textové runbooků

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Nastavení nebo načtení jednoduché hodnotu z proměnné

Hello následující ukázkové příkazy Zobrazit jak tooset a načíst proměnnou v textové runbooku. V této ukázce se předpokládá, že proměnné celočíselného typu s názvem *NumberOfIterations* a *NumberOfRunnings* a proměnná řetězcového typu nazvaná *SampleMessage* již byly vytvořeny.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Nastavení nebo načtení komplexní objekt v proměnné

Hello následující ukázkový kód ukazuje, jak tooupdate proměnná s komplexní hodnoty v textové runbooku. V této ukázce je načíst virtuální počítač Azure s **Get-AzureVM** a uložené tooan existující automatizace proměnnou.  Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako PSCustomObject.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


V hello následující kód je hodnota hello načtena z hello proměnné a slouží toostart hello virtuálního počítače.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Nastavení nebo načtení kolekce v proměnné

Hello následující ukázkový kód ukazuje, jak toouse proměnná s kolekce komplexní hodnoty v textové runbooku. V této ukázce se načítají více virtuálními počítači Azure s **Get-AzureVM** a uložené tooan existující automatizace proměnnou.  Jak je popsáno v [typy proměnných](#variable-types), to je uloženo jako soubor PSCustomObjects.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

V následujícím kódu hello hello kolekce se načítají hello proměnné a používat toostart každý virtuální počítač.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a>Ukázky grafický runbook

V grafický runbook přidáte hello **Get-AutomationVariable** nebo **Set-AutomationVariable** kliknutím pravým tlačítkem myši na hello proměnné v podokně knihovna hello hello grafického editoru a výběrem hello aktivity, které chcete.

![Přidejte proměnnou toocanvas](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Nastavení hodnot v proměnné
Hello následující obrázek znázorňuje ukázkové aktivity tooupdate proměnné s hodnotou jednoduché v grafický runbook. V této ukázce je načíst jediný virtuální počítač Azure s **Get-AzureRmVM** a název počítače hello po uložení tooan existující automatizace proměnné typu řetězec.  Nezávisle na tom, zda hello [odkaz je kanál, nebo pořadí](automation-graphical-authoring-intro.md#links-and-workflow) vzhledem k tomu, že pouze Očekáváme, že jeden objekt ve výstupu hello.

![Sada jednoduché proměnné](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Další kroky

* toolearn Další informace o připojení aktivity společně v vytváření grafického obsahu, najdete v části [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md) 

