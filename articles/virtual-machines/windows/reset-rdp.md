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
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="fb916-103">Jak tooreset hello služby Vzdálená plocha nebo jeho heslo pro přihlášení do systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fb916-103">How tooreset hello Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="fb916-104">Pokud se nemůžete připojit tooa Windows virtuální počítač (VM), můžete resetovat heslo místního správce hello nebo resetovat konfiguraci služby Vzdálená plocha hello.</span><span class="sxs-lookup"><span data-stu-id="fb916-104">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="fb916-105">Můžete buď hello Azure portal nebo hello rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell tooreset hello heslo.</span><span class="sxs-lookup"><span data-stu-id="fb916-105">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span> <span data-ttu-id="fb916-106">Pokud používáte prostředí PowerShell, ujistěte se, že máte hello [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášeni tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fb916-106">If you are using PowerShell, make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription.</span></span> <span data-ttu-id="fb916-107">Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Classic hello](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="fb916-107">You can also [perform these steps for VMs created with hello Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="fb916-108">Konfigurace způsobů tooreset nebo přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="fb916-108">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="fb916-109">Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:</span><span class="sxs-lookup"><span data-stu-id="fb916-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="fb916-110">Resetovat pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fb916-110">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="fb916-111">Resetovat pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb916-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="fb916-112">portál Azure</span><span class="sxs-lookup"><span data-stu-id="fb916-112">Azure portal</span></span>
<span data-ttu-id="fb916-113">Klikněte na tlačítko hello tři řádky v levém horním rohu hello tooexpand hello nabídce portálu a potom klikněte na **virtuální počítače**:</span><span class="sxs-lookup"><span data-stu-id="fb916-113">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines**:</span></span>

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="fb916-115">**Resetovat heslo účtu místního správce hello**</span><span class="sxs-lookup"><span data-stu-id="fb916-115">**Reset hello local administrator account password**</span></span>

<span data-ttu-id="fb916-116">Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="fb916-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="fb916-117">Zobrazí se okno pro resetování hesla Hello:</span><span class="sxs-lookup"><span data-stu-id="fb916-117">hello password reset blade is displayed:</span></span>

![Stránka pro resetování hesla](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="fb916-119">Zadejte uživatelské jméno hello a nové heslo a potom klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="fb916-119">Enter hello username and a new password, then click **Update**.</span></span> <span data-ttu-id="fb916-120">Zkuste se znovu připojit tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb916-120">Try connecting tooyour VM again.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="fb916-121">**Resetovat konfiguraci služby Vzdálená plocha hello**</span><span class="sxs-lookup"><span data-stu-id="fb916-121">**Reset hello Remote Desktop service configuration**</span></span>

<span data-ttu-id="fb916-122">Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="fb916-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="fb916-123">Zobrazí se okno pro resetování hesla Hello.</span><span class="sxs-lookup"><span data-stu-id="fb916-123">hello password reset blade is displayed.</span></span> 

![Resetování konfigurace RDP](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="fb916-125">Vyberte **jenom resetování konfigurace** hello rozevírací nabídce klikněte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="fb916-125">Select **Reset configuration only** from hello drop-down menu, then click **Update**.</span></span> <span data-ttu-id="fb916-126">Zkuste se znovu připojit tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fb916-126">Try connecting tooyour VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="fb916-127">Rozšíření VMAccess a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb916-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="fb916-128">Ujistěte se, že máte hello [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášeni tooyour předplatné s hello `Login-AzureRmAccount` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fb916-128">Make sure that you have hello [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in tooyour Azure subscription with hello `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="fb916-129">**Resetovat heslo účtu místního správce hello**</span><span class="sxs-lookup"><span data-stu-id="fb916-129">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="fb916-130">Resetování hello heslo nebo uživatelské jméno správce s hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb916-130">Reset hello administrator password or user name with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="fb916-131">Vytvořte přihlašovací údaje účtu takto:</span><span class="sxs-lookup"><span data-stu-id="fb916-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="fb916-132">Pokud zadáte jiný název než hello aktuální účet místního správce na vašem virtuálním počítači, hello rozšíření VMAccess přejmenuje hello účet místního správce, přiřadí účtu toothat zadané heslo a vydá událost odhlášení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fb916-132">If you type a different name than hello current local administrator account on your VM, hello VMAccess extension renames hello local administrator account, assigns your specified password toothat account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="fb916-133">Pokud je zakázána hello účet místního správce na vašem virtuálním počítači, povolí hello rozšíření VMAccess ho.</span><span class="sxs-lookup"><span data-stu-id="fb916-133">If hello local administrator account on your VM is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="fb916-134">Následující příklad aktualizace Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup` toohello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fb916-134">hello following example updates hello VM named `myVM` in hello resource group named `myResourceGroup` toohello credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="fb916-135">**Resetovat konfiguraci služby Vzdálená plocha hello**</span><span class="sxs-lookup"><span data-stu-id="fb916-135">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="fb916-136">Resetovat vzdálený přístup tooyour virtuálních počítačů s hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb916-136">Reset remote access tooyour VM with hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="fb916-137">Hello následující příklad resetuje rozšíření hello přístup s názvem `myVMAccess` na hello virtuálního počítače s názvem `myVM` v hello `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="fb916-137">hello following example resets hello access extension named `myVMAccess` on hello VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="fb916-138">Kdykoli virtuální počítač může mít pouze jednoho agenta virtuálního počítače přístup.</span><span class="sxs-lookup"><span data-stu-id="fb916-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="fb916-139">Vlastnosti agenta přístup virtuálních počítačů hello tooset úspěšně, hello `-ForceRerun` možnost lze použít.</span><span class="sxs-lookup"><span data-stu-id="fb916-139">tooset hello VM access agent properties successfully, hello `-ForceRerun` option can be used.</span></span> <span data-ttu-id="fb916-140">Při použití `-ForceRerun`, zajistěte, aby toouse hello stejný název pro hello agenta virtuálního počítače přístup v rámci všech předchozích příkazů.</span><span class="sxs-lookup"><span data-stu-id="fb916-140">When using `-ForceRerun`, make sure toouse hello same name for hello VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="fb916-141">Pokud se pořád nemůžete připojit vzdáleně tooyour virtuálního počítače, najdete v části Další kroky tootry v [řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fb916-141">If you still can't connect remotely tooyour virtual machine, see more steps tootry at [Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="fb916-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb916-142">Next steps</span></span>
<span data-ttu-id="fb916-143">Pokud neodpovídá hello rozšíření přístup k virtuálnímu počítači Azure a jsou nelze tooreset hello heslo, můžete [resetování hello místní heslo systému Windows do režimu offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fb916-143">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="fb916-144">Tato metoda je pokročilejší proces a vyžaduje, abyste tooconnect hello virtuální pevný disk hello problematické tooanother virtuálních počítačů VM.</span><span class="sxs-lookup"><span data-stu-id="fb916-144">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="fb916-145">Postupujte podle hello kroků popsaných v tomto článku nejprve a pouze pokusí hello offline heslo resetovat metoda jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="fb916-145">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="fb916-146">Rozšíření virtuálního počítače Azure a funkce</span><span class="sxs-lookup"><span data-stu-id="fb916-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="fb916-147">Připojení pomocí protokolu RDP nebo SSH tooan virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="fb916-147">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="fb916-148">Řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="fb916-148">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

