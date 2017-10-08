---
title: "aaaVirtual počítače rozšíření a funkce pro Windows v Azure | Microsoft Docs"
description: "Zjistěte, jaká rozšíření jsou k dispozici pro virtuální počítače Azure, seskupené podle co se poskytují nebo zvýšit."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61ccfd696b38e9be1026d836d5796c2346fd650f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Rozšíření virtuálního počítače a funkce pro Windows

Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure. Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače může být použité toocomplete tyto úlohy. Rozšíření virtuálního počítače Azure můžete spustit pomocí hello rozhraní příkazového řádku Azure, prostředí PowerShell, šablon Azure Resource Manageru a hello portálu Azure. Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.

Tento dokument obsahuje přehled rozšíření virtuálního počítače, požadavky na tom, jak spravovat toodetect a odeberte rozšíření virtuálního počítače pomocí rozšíření virtuálního počítače a pokyny. Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný. Podrobnosti o konkrétní rozšíření najdete v každé jednotlivé rozšíření konkrétní toohello dokumentu.

## <a name="use-cases-and-samples"></a>Případy použití a ukázky

Nejsou k dispozici mnoho různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití. Jsou některé případy použití příklad:

- Použijte PowerShell požadovaná stav konfigurace tooa virtuálního počítače pomocí rozšíření hello DSC pro systém Windows. Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](extensions-dsc-overview.md).
- Konfigurace monitorování virtuálních počítačů pomocí rozšíření virtuálního počítače agenta monitorování Microsoft hello. Další informace najdete v tématu [tooLog virtuální počítače Azure připojit Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).
- Konfigurace sledování infrastruktury Azure s hello Datadog rozšíření. Další informace najdete v tématu hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfigurace virtuálního počítače Azure pomocí Chef. Další informace najdete v tématu [automatizace Azure nasazení virtuálního počítače s Chef](chef-automation.md).

Kromě toho tooprocess konkrétní rozšíření, rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux. rozšíření vlastních skriptů pro Windows Hello umožňuje žádné toobe skript prostředí PowerShell spustit na virtuálním počítači. To je užitečné při návrhu Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout. Další informace najdete v tématu [rozšíření skriptů vlastní virtuální počítač Windows](extensions-customscript.md).


## <a name="prerequisites"></a>Požadavky

Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků. Například hello rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux. Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci pro konkrétní rozšíření hello.

### <a name="azure-vm-agent"></a>Agent virtuálního počítače Azure
agent virtuálního počítače Azure Hello spravuje interakci mezi virtuální počítač Azure a prostředků infrastruktury Azure řadiče hello. agent virtuálního počítače Hello je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače. agent virtuálního počítače Azure Hello je předinstalován v Azure Marketplace bitové kopie a může být nainstalována na podporovaných operačních systémech.

Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Zjistit rozšíření virtuálního počítače
Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure. toosee úplný seznam, spusťte následující příkaz Powershellu pro Azure Resource Manager modulem hello hello. Pokud používáte tento příkaz, zkontrolujte že toospecify hello požadovaného umístění.

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>Spuštění rozšíření virtuálního počítače

Rozšíření virtuálního počítače Azure můžete spustit na existující virtuální počítače, což je užitečné, když potřebujete toomake změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače. Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager. Pomocí rozšíření pomocí šablony Resource Manageru můžete povolit toobe virtuální počítače Azure, nasazení a nakonfigurování hello nevyžaduje zásah po nasazení.

Hello následující metody se dá použít toorun rozšíření proti existujícího virtuálního počítače.

### <a name="powershell"></a>PowerShell

Existuje několik příkazů prostředí PowerShell pro spouštění jednotlivých rozšíření. toosee seznamu, spusťte následující příkazy prostředí PowerShell hello.

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

To poskytuje podobné toohello následující výstup:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

Hello následující příklad používá hello vlastní skript rozšíření toodownload skript z úložiště Githubu na hello cílový virtuální počítač a spusťte skript hello. Další informace o hello rozšíření vlastních skriptů najdete v tématu [přehled rozšíření vlastních skriptů](extensions-customscript.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

V tomto příkladu je hello rozšíření pro přístup virtuálních počítačů používaných tooreset hello hesla pro správu systému Windows virtuálního počítače. Další informace o hello rozšíření pro přístup virtuálních počítačů najdete v tématu [služby Vzdálená plocha resetovat ve virtuálním počítači Windows](reset-rdp.md).

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

Hello `Set-AzureRmVMExtension` příkaz lze použít toostart žádné rozšíření virtuálního počítače. Další informace najdete v tématu hello [odkaz na sadu AzureRmVMExtension](https://msdn.microsoft.com/en-us/library/mt603745.aspx).


### <a name="azure-portal"></a>portál Azure

Rozšíření virtuálního počítače může být použité tooan existujícího virtuálního počítače prostřednictvím hello portálu Azure. toodo tedy vyberte hello virtuální počítač má toouse, zvolte **rozšíření**a klikněte na tlačítko **přidat**. To poskytuje seznam dostupných rozšíření. Vyberte hello jeden chcete a postupujte podle kroků hello v Průvodci hello.

Hello následující obrázek znázorňuje instalaci hello hello rozšíření Microsoft Antimalware z hello portálu Azure.

![Nainstalujte rozšíření proti malwaru](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Rozšíření virtuálního počítače může být přidané tooan šablony Azure Resource Manageru a provést s hello nasazení šablony hello. Nasazení rozšíření pomocí šablony je užitečné pro vytváření kompletně nakonfigurovaný Azure nasazení. Například hello, pořízení následujícím kódu JSON ze šablony Resource Manageru, která nasadí sadu Vyrovnávání zatížení sítě virtuálních počítačů a Azure SQL database a poté nainstaluje aplikace .NET Core na každém virtuálním počítači. Instalace softwaru hello postará Hello rozšíření virtuálního počítače.

Další informace najdete v tématu hello [úplné šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru pomocí rozšíření virtuálního počítače Windows](template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Zabezpečení dat rozšíření virtuálního počítače

Když používáte rozšíření virtuálního počítače, může být nutné tooinclude citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště. Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř hello cílového virtuálního počítače. Každé rozšíření obsahuje schéma konkrétní chráněné konfigurace, které budou popsané v dokumentaci konkrétní rozšíření.

Hello následující příklad ukazuje instanci hello rozšíření vlastních skriptů pro Windows. Všimněte si, že tento příkaz tooexecute hello zahrnuje sadu přihlašovacích údajů. V tomto příkladu hello příkaz tooexecute se šifrovat nebude.


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Zabezpečené spouštění řetězec hello přesunutím hello **příkaz tooexecute** vlastnost toohello **chráněné** konfigurace.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a>Řešení potíží s rozšířeními virtuálního počítače

Každé rozšíření virtuálního počítače může mít specifické pro řešení potíží. Například pokud používáte rozšíření vlastních skriptů hello, podrobnosti provádění skriptu naleznete místně na hello virtuálního počítače, na kterém byl spuštěn hello rozšíření. Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.

Hello následující kroky řešení potíží použít tooall rozšíření virtuálního počítače.

### <a name="view-extension-status"></a>Zobrazit stav rozšíření

Po spuštění rozšíření virtuálního počítače pro virtuální počítač, použijte následující příkaz prostředí PowerShell, tooreturn stav rozšíření hello. Názvy parametrů příkladu nahraďte vlastními hodnotami. Hello `Name` přebírá parametr hello název rozšíření toohello v době provedení.

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

výstup Hello vypadá hello následující:

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

Stav spuštění rozšíření naleznete také v hello portálu Azure. Stav hello tooview rozšíření, vyberte hello virtuálního počítače, zvolte **rozšíření**, a vyberte hello požadované rozšíření.

### <a name="rerun-vm-extensions"></a>Znovu spustit, rozšíření virtuálního počítače

Můžou nastat případy, ve kterých rozšíření virtuálního počítače potřebuje toobe spusťte znovu. To provedete tak, že rozšíření hello s metodu provádění zvoleného zajistit opětovné spuštění rozšíření hello. tooremove rozšíření, spusťte následující příkaz s modul Azure PowerShell hello hello. Názvy parametrů příkladu nahraďte vlastními hodnotami.

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Rozšíření může být odebrán také pomocí hello portálu Azure. toodo tak:

1. Vyberte virtuální počítač.
2. Vyberte **rozšíření**.
3. Zvolte rozšíření hello potřeby.
4. Vyberte **odinstalovat**.

## <a name="common-vm-extensions-reference"></a>Běžné odkaz rozšíření virtuálního počítače
| Název rozšíření | Popis | Další informace |
| --- | --- | --- |
| Rozšíření vlastních skriptů pro Windows |Spouštění skriptů na virtuálním počítači Azure |[Rozšíření vlastních skriptů pro Windows](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Rozšíření DSC pro Windows |Rozšíření prostředí PowerShell DSC (Desired State Configuration) |[Rozšíření DSC pro Windows](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Rozšíření Azure Diagnostics |Správa Azure Diagnostics |[Rozšíření diagnostiky Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Rozšíření pro přístup virtuálních počítačů Azure |Spravovat uživatele a přihlašovací údaje |[Rozšíření pro přístup virtuálních počítačů pro Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
