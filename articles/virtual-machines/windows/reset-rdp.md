---
title: "aaaReset hello heslo nebo konfigurace vzdálené plochy na virtuální počítač s Windows | Microsoft Docs"
description: "Zjistěte, jak tooreset heslo k účtu nebo služeb vzdálené plochy na virtuální počítač s Windows pomocí hello portál Azure nebo Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 5258df7196621f0adb50debd08dd248922a966de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Jak tooreset hello služby Vzdálená plocha nebo jeho heslo pro přihlášení do systému Windows virtuálního počítače
Pokud se nemůžete připojit tooa Windows virtuální počítač (VM), můžete resetovat heslo místního správce hello nebo resetovat konfiguraci služby Vzdálená plocha hello. Můžete buď hello Azure portal nebo hello rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell tooreset hello heslo. Pokud používáte prostředí PowerShell, ujistěte se, že máte hello [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášeni tooyour předplatného Azure. Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Classic hello](reset-rdp.md).

## <a name="ways-tooreset-configuration-or-credentials"></a>Konfigurace způsobů tooreset nebo přihlašovací údaje
Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:

- [Resetovat pomocí hello portálu Azure](#azure-portal)
- [Resetovat pomocí Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>portál Azure
Klikněte na tlačítko hello tři řádky v levém horním rohu hello tooexpand hello nabídce portálu a potom klikněte na **virtuální počítače**:

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a>**Resetovat heslo účtu místního správce hello**

Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**. Zobrazí se okno pro resetování hesla Hello:

![Stránka pro resetování hesla](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Zadejte uživatelské jméno hello a nové heslo a potom klikněte na tlačítko **aktualizace**. Zkuste se znovu připojit tooyour virtuálních počítačů.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Resetovat konfiguraci služby Vzdálená plocha hello**

Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**. Zobrazí se okno pro resetování hesla Hello. 

![Resetování konfigurace RDP](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Vyberte **jenom resetování konfigurace** hello rozevírací nabídce klikněte **aktualizace**. Zkuste se znovu připojit tooyour virtuálních počítačů.


## <a name="vmaccess-extension-and-powershell"></a>Rozšíření VMAccess a prostředí PowerShell
Ujistěte se, že máte hello [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášeni tooyour předplatné s hello `Login-AzureRmAccount` rutiny.

### <a name="reset-hello-local-administrator-account-password"></a>**Resetovat heslo účtu místního správce hello**
Resetování hello heslo nebo uživatelské jméno správce s hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell. Vytvořte přihlašovací údaje účtu takto:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> Pokud zadáte jiný název než hello aktuální účet místního správce na vašem virtuálním počítači, hello rozšíření VMAccess přejmenuje hello účet místního správce, přiřadí účtu toothat zadané heslo a vydá událost odhlášení vzdálené plochy. Pokud je zakázána hello účet místního správce na vašem virtuálním počítači, povolí hello rozšíření VMAccess ho.

Následující příklad aktualizace Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup` toohello přihlašovací údaje.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Resetovat konfiguraci služby Vzdálená plocha hello**
Resetovat vzdálený přístup tooyour virtuálních počítačů s hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell. Hello následující příklad resetuje rozšíření hello přístup s názvem `myVMAccess` na hello virtuálního počítače s názvem `myVM` v hello `myResourceGroup` skupiny prostředků:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Kdykoli virtuální počítač může mít pouze jednoho agenta virtuálního počítače přístup. Vlastnosti agenta přístup virtuálních počítačů hello tooset úspěšně, hello `-ForceRerun` možnost lze použít. Při použití `-ForceRerun`, zajistěte, aby toouse hello stejný název pro hello agenta virtuálního počítače přístup v rámci všech předchozích příkazů.

Pokud se pořád nemůžete připojit vzdáleně tooyour virtuálního počítače, najdete v části Další kroky tootry v [řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Další kroky
Pokud neodpovídá hello rozšíření přístup k virtuálnímu počítači Azure a jsou nelze tooreset hello heslo, můžete [resetování hello místní heslo systému Windows do režimu offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tato metoda je pokročilejší proces a vyžaduje, abyste tooconnect hello virtuální pevný disk hello problematické tooanother virtuálních počítačů VM. Postupujte podle hello kroků popsaných v tomto článku nejprve a pouze pokusí hello offline heslo resetovat metoda jako poslední možnost.

[Rozšíření virtuálního počítače Azure a funkce](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Připojení pomocí protokolu RDP nebo SSH tooan virtuální počítač Azure](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

