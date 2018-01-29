---
title: "Konfigurace serverů na požadovaný stav a spravovat odlišily s Azure Automation | Microsoft Docs"
description: "Kurz – Správa konfigurací serveru s Azure Automation DSC."
services: automation
documentationcenter: automation
author: georgewallace
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: gwallace
ms.custom: 
ms.openlocfilehash: c510b2244dfa85b12ed08cad9dbab75067ebe41a
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Konfigurace serverů na požadovaný stav a spravovat odlišily

Konfigurace Azure Automation žádaný stavu (DSC) umožňuje určit konfigurace pro servery a zajistěte, aby tyto servery v zadaném stavu v čase.



> [!div class="checklist"]
> * Připojit virtuální počítač má být spravováno aplikací Azure Automation DSC.
> * Nahrání konfigurace do Azure Automation.
> * Zkompilování konfigurace do konfigurace uzlu
> * Přiřadit konfigurace uzlu spravovaný uzel
> * Zkontrolujte stav dodržování předpisů spravovaný uzel

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, budete potřebovat:

* Účet Azure Automation. Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Účet Spustit jako pro Azure](automation-sec-configure-azure-runas-account.md).
* Virtuální počítač Azure Resource Manager (ne Classic) systémem Windows Server 2008 R2 nebo novější. Pokyny k vytvoření virtuálního počítače najdete v tématu [Vytvoření vašeho prvního virtuálního počítače s Windows na webu Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
* Azure PowerShell verze modulu 3,6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Znalost DSC. Informace o DSC najdete v tématu [potřeby přehled stavu konfigurace prostředí Windows PowerShell](https://docs.microsoft.com/powershell/dsc/overview)

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k předplatnému Azure pomocí příkazu `Login-AzureRmAccount` a postupujte podle pokynů na obrazovce.

```powershell
Login-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Vytvoření a nahrání konfigurace pro Azure Automation

V tomto kurzu budeme používat jednoduchá konfigurace DSC, která zajišťuje, že ve virtuálním počítači je nainstalována služba IIS.

Informace o konfiguracích DSC najdete v tématu [konfigurace DSC](https://docs.microsoft.com/powershell/dsc/configurations).

V textovém editoru, zadejte následující příkaz a uložte ho místně jako `TestConfig.ps1`.

```powershell
configuration TestConfig {

   Node WebServer {

      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

Volání `Import-AzureRmAutomationDscConfiguration` rutiny nahrát konfigurace do vašeho účtu Automation:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Zkompilování konfigurace do konfigurace uzlu

Konfigurace DSC musí být zkompilovány do konfigurace uzlu, než může být přiřazena k uzlu.

Informace o kompilování konfigurace najdete v tématu [konfigurace DSC](https://docs.microsoft.com/powershell/dsc/configurations).

Volání `Start-AzureRmAutomationDscCompilationJob` rutiny zkompilovat `TestConfig` konfigurace do konfigurace uzlu:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Tím se vytvoří konfigurace uzlu s názvem `TestConfig.WebServer` ve vašem účtu Automation.

## <a name="register-a-vm-to-be-managed-by-dsc"></a>Registrace virtuálních počítačů lze spravovat pomocí DSC

Azure Automation DSC můžete použít ke správě virtuálních počítačích Azure (Classic i Resource Manager), místní virtuální počítače, Linux počítače, virtuální počítače AWS a místní fyzických počítačů. V tomto tématu nabídneme postup registrace jenom virtuální počítače Azure Resource Manager.
Informace o registraci jiné typy počítačů najdete v tématu [registrace počítačů pro správu Azure Automation DSC](automation-dsc-onboarding.md).

Volání `Register-AzureRmAutomationDscNode` rutinu pro registraci virtuálního počítače se Azure Automation DSC.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName "MyResourceGroup" -AutomationAccountName "myAutomationAccount" -AzureVMName "DscVm"
```

Takovém postupu zaregistruje zadaný virtuální počítač jako uzel DSC v účtu Azure Automation.

### <a name="specify-configuration-mode-settings"></a>Specifikace konfigurace nastavení režimu

Při registraci virtuálního počítače jako spravovaný uzel, můžete také zadat vlastnosti konfigurace.
Například můžete určit, že stav počítače bude použita pouze jednou (DSC se nebude pokoušet použít konfiguraci po počáteční kontroly) tak, že zadáte `ApplyOnly` jako hodnotu **ConfigurationMode** vlastnost :

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationMode 'ApplyOnly'
```

Můžete také určit, jak často DSC kontroluje stav konfigurace pomocí **ConfigurationModeFrequencyMins** vlastnost:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationModeFrequencyMins 60
```

Další informace o nastavení vlastnosti konfigurace pro spravované uzel najdete v tématu [Register-AzureRmAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-4.3.1&viewFallbackFrom=azurermps-4.2.0).

Další informace o nastavení konfigurace DSC najdete v tématu [konfigurace správce místní konfigurace](https://docs.microsoft.com/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Přiřadit konfigurace uzlu spravovaný uzel

Nyní jsme můžete přiřadit konfigurace kompilované uzlu na virtuální počítač, který chcete konfigurovat.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -Id $node.Id
```

Tím se přiřadí konfigurace uzlu s názvem `TestConfig.WebServer` k registrované uzlu DSC s názvem `DscVm`.
Ve výchozím nastavení je uzel DSC kontroluje na dodržování předpisů konfigurace uzlu každých 30 minut.
Informace o tom, jak změnit intervalu kontroly dodržování předpisů najdete v tématu [konfigurace správce místní konfigurace](https://docs.microsoft.com/PowerShell/DSC/metaConfig)

## <a name="check-the-compliance-status-of-a-managed-node"></a>Zkontrolujte stav dodržování předpisů spravovaný uzel

Sestavy o stavu dodržování předpisů uzlu DSC můžete získat voláním `Get-AzureRmAutomationDscNodeReport` rutiny:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Id $node.Id

# Display the most recent report
$reports[0]
```

## <a name="next-steps"></a>Další kroky

* Další informace jak pro zařadit uzly pro správu ve službě Azure Automation DSC, najdete v části [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)
* Další informace o použití portálu Azure Automation DSC použít, najdete v tématu [Začínáme s Azure Automation DSC.](automation-dsc-getting-started.md)
* Další informace o kompilaci konfigurace DSC, takže můžete je přiřadit k cílové uzly najdete v tématu [kompilování konfigurace v Azure Automation DSC.](automation-dsc-compile.md)
* Referenční informace o rutinách prostředí PowerShell pro Azure Automation DSC, najdete v části [rutiny Azure Automation DSC.](/powershell/module/azurerm.automation/#automation)
* Informace o cenách naleznete v tématu [ceny Azure Automation DSC.](https://azure.microsoft.com/pricing/details/automation/)
* Příklad použití Azure Automation DSC v kanálu průběžné nasazování, najdete v sekci [průběžné nasazování a Chocolatey IaaS virtuálních počítačů pomocí Azure Automation DSC](automation-dsc-cd-chocolatey.md)
