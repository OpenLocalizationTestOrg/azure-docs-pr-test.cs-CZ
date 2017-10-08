---
title: "aaaReset hello heslo nebo konfigurace vzdálené plochy na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak hello tooreset heslo k účtu nebo služby Vzdálená plocha na virtuální počítač s Windows vytvořeného pomocí modelu nasazení Classic hello pomocí portálu Azure nebo Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 1721a91fc6c89b46df74e76dfcf918b1c4c77a4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a>Služba Vzdálená plocha hello tooreset nebo jeho heslo pro přihlášení do virtuálního počítače Windows vytváření pomocí modelu nasazení Classic hello
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Resource Manager hello](../reset-rdp.md).

Pokud se nemůžete připojit tooa Windows virtuální počítač (VM), můžete resetovat heslo místního správce hello nebo resetovat konfiguraci služby Vzdálená plocha hello. Můžete buď hello Azure portal nebo hello rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell tooreset hello heslo.

## <a name="ways-tooreset-configuration-or-credentials"></a>Konfigurace způsobů tooreset nebo přihlašovací údaje
Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:

- [Resetovat pomocí hello portálu Azure](#azure-portal)
- [Resetovat pomocí Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>portál Azure
Můžete použít hello [portál Azure](https://portal.azure.com) tooreset hello služby Vzdálená plocha. Klikněte na tlačítko hello tři řádky v levém horním rohu hello tooexpand hello nabídce portálu a potom klikněte na **virtuálních počítačů (klasické)**:

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-Classic-VM.png)

Vyberte virtuální počítač se systémem Windows a pak klikněte na tlačítko **resetovat vzdálený...** . hello následující otevře se dialogové okno Konfigurace vzdálené plochy tooreset hello:

![Stránka Konfigurace resetování protokolu RDP](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Můžete taky resetovat hello uživatelské jméno a heslo účtu místního správce hello. Z virtuálního počítače, klikněte na tlačítko **podporu + Poradce při potížích s** > **resetovat heslo**. Zobrazí se okno pro resetování hesla Hello:

![Stránka pro resetování hesla](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Po zadání hello nové uživatelské jméno a heslo, klikněte na tlačítko **Uložit**.

## <a name="vmaccess-extension-and-powershell"></a>Rozšíření VMAccess a prostředí PowerShell
Ujistěte se, zda text hello, Agent virtuálního počítače je nainstalována na virtuálním počítači hello. Hello rozšíření VMAccess nepotřebuje toobe nainstalovat, abyste mohli používat, dokud hello agenta virtuálního počítače je k dispozici. Ověřte, že hello agenta virtuálního počítače je již nainstalována pomocí hello následující příkaz. (Nahraďte "myCloudService" a "Můjvp" hello názvy cloudové služby a virtuální počítač, v uvedeném pořadí. Dozvíte se tak, že spustíte tyto názvy `Get-AzureVM` bez parametrů.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Pokud hello **zápisu hostitele** příkaz zobrazí **True**, hello je nainstalovaný Agent virtuálního počítače. Pokud se zobrazí **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure příspěvku na blogu.

Pokud jste vytvořili hello virtuálního počítače pomocí portálu hello, zkontrolujte, zda `$vm.GetInstance().ProvisionGuestAgent` vrátí **True**. Pokud ne, můžete ho nastavit pomocí tohoto příkazu:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Tento příkaz zabrání hello následující chyba, když používáte hello **Set-AzureVMExtension** v dalších krocích hello: "Zřízení Agent hosta musí být povolené na objekt virtuálního počítače hello před nastavením rozšíření pro přístup virtuálních počítačů IaaS."

### <a name="reset-hello-local-administrator-account-password"></a>**Resetovat heslo účtu místního správce hello**
Vytvoření pověření přihlášení pomocí hello aktuální název účtu místního správce a nové heslo a pak spusťte hello `Set-AzureVMAccessExtension` následujícím způsobem.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Pokud zadáte jiný název než hello aktuálního účtu, hello rozšíření VMAccess přejmenuje hello účet místního správce, přiřadí hello heslo toothat účet a vydá odhlašování vzdálené plochy. Pokud je účet místního správce hello vypnutá, hello rozšíření VMAccess povolí ho.

Tyto příkazy také obnovit konfiguraci služby Vzdálená plocha hello.

### <a name="reset-hello-remote-desktop-service-configuration"></a>**Resetovat konfiguraci služby Vzdálená plocha hello**
tooreset hello konfiguraci služby Vzdálená plocha, spusťte následující příkaz hello:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

Hello rozšíření VMAccess běží na virtuálním počítači hello dva příkazy:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Tento příkaz povolí hello předdefinovaná brány Windows Firewall skupina, která umožní příchozí provoz vzdálené plochy, který používá TCP port 3389.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Tento příkaz nastaví hello fDenyTSConnections registru hodnotu too0, povolení připojení ke vzdálené ploše.

## <a name="next-steps"></a>Další kroky
Pokud neodpovídá hello rozšíření přístup k virtuálnímu počítači Azure a jsou nelze tooreset hello heslo, můžete [resetování hello místní heslo systému Windows do režimu offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Tato metoda je pokročilejší proces a vyžaduje, abyste tooconnect hello virtuální pevný disk hello problematické tooanother virtuálních počítačů VM. Postupujte podle hello kroků popsaných v tomto článku nejprve a pouze pokusí hello offline heslo resetovat metoda jako poslední možnost.

[Rozšíření virtuálního počítače Azure a funkce](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Připojení pomocí protokolu RDP nebo SSH tooan virtuální počítač Azure](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

