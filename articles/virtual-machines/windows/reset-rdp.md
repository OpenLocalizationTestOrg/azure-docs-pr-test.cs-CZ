---
title: "Resetovat heslo nebo konfigurace vzdálené plochy na virtuální počítač s Windows | Microsoft Docs"
description: "Zjistěte, jak resetovat heslo k účtu nebo služeb vzdálené plochy na virtuální počítač s Windows pomocí portálu Azure nebo Azure PowerShell."
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
ms.openlocfilehash: 2e002e3f336422b8fa1eceece889cd083e355a68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a><span data-ttu-id="f7ebe-103">Obnovení služby Vzdálená plocha nebo jeho heslo pro přihlášení do systému Windows virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f7ebe-103">How to reset the Remote Desktop service or its login password in a Windows VM</span></span>
<span data-ttu-id="f7ebe-104">Pokud se nemůžete připojit k virtuálnímu počítači (VM) systému Windows, můžete resetovat heslo místního správce nebo resetovat konfiguraci služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-104">If you can't connect to a Windows virtual machine (VM), you can reset the local administrator password or reset the Remote Desktop service configuration.</span></span> <span data-ttu-id="f7ebe-105">Portál Azure nebo rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell můžete použít k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-105">You can use either the Azure portal or the VM Access extension in Azure PowerShell to reset the password.</span></span> <span data-ttu-id="f7ebe-106">Pokud používáte prostředí PowerShell, ujistěte se, že máte [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášení k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-106">If you are using PowerShell, make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription.</span></span> <span data-ttu-id="f7ebe-107">Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Classic](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="f7ebe-107">You can also [perform these steps for VMs created with the Classic deployment model](reset-rdp.md).</span></span>

## <a name="ways-to-reset-configuration-or-credentials"></a><span data-ttu-id="f7ebe-108">Jak obnovit konfiguraci nebo přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="f7ebe-108">Ways to reset configuration or credentials</span></span>
<span data-ttu-id="f7ebe-109">Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:</span><span class="sxs-lookup"><span data-stu-id="f7ebe-109">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="f7ebe-110">Resetovat pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f7ebe-110">Reset using the Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="f7ebe-111">Resetovat pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7ebe-111">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="f7ebe-112">portál Azure</span><span class="sxs-lookup"><span data-stu-id="f7ebe-112">Azure portal</span></span>
<span data-ttu-id="f7ebe-113">Rozbalte nabídku portálu, klikněte na tři řádky v levém horním rohu a pak klikněte na **virtuální počítače**:</span><span class="sxs-lookup"><span data-stu-id="f7ebe-113">To expand the portal menu, click the three bars in the upper left corner and then click **Virtual machines**:</span></span>

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="f7ebe-115">**Resetovat heslo k účtu místního správce**</span><span class="sxs-lookup"><span data-stu-id="f7ebe-115">**Reset the local administrator account password**</span></span>

<span data-ttu-id="f7ebe-116">Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-116">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="f7ebe-117">Zobrazí se okno resetování hesla:</span><span class="sxs-lookup"><span data-stu-id="f7ebe-117">The password reset blade is displayed:</span></span>

![Stránka pro resetování hesla](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

<span data-ttu-id="f7ebe-119">Zadejte uživatelské jméno a nové heslo a potom klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-119">Enter the username and a new password, then click **Update**.</span></span> <span data-ttu-id="f7ebe-120">Zkuste znovu připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-120">Try connecting to your VM again.</span></span>

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="f7ebe-121">**Resetovat konfiguraci služby Vzdálená plocha**</span><span class="sxs-lookup"><span data-stu-id="f7ebe-121">**Reset the Remote Desktop service configuration**</span></span>

<span data-ttu-id="f7ebe-122">Vyberte virtuální počítač se systémem Windows a klikněte na **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-122">Select your Windows virtual machine then click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="f7ebe-123">Zobrazí se okno resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-123">The password reset blade is displayed.</span></span> 

![Resetování konfigurace RDP](./media/reset-rdp/Portal-RM-RDP-Reset.png)

<span data-ttu-id="f7ebe-125">Vyberte **jenom resetování konfigurace** v rozevírací nabídce klikněte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-125">Select **Reset configuration only** from the drop-down menu, then click **Update**.</span></span> <span data-ttu-id="f7ebe-126">Zkuste znovu připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-126">Try connecting to your VM again.</span></span>


## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="f7ebe-127">Rozšíření VMAccess a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7ebe-127">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="f7ebe-128">Ujistěte se, že máte [nejnovější modul prostředí PowerShell nainstalovaný a nakonfigurovaný](/powershell/azure/overview) a přihlášení k předplatnému Azure s `Login-AzureRmAccount` rutiny.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-128">Make sure that you have the [latest PowerShell module installed and configured](/powershell/azure/overview) and are signed in to your Azure subscription with the `Login-AzureRmAccount` cmdlet.</span></span>

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="f7ebe-129">**Resetovat heslo k účtu místního správce**</span><span class="sxs-lookup"><span data-stu-id="f7ebe-129">**Reset the local administrator account password**</span></span>
<span data-ttu-id="f7ebe-130">Resetování správce heslo nebo uživatelské jméno s [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-130">Reset the administrator password or user name with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="f7ebe-131">Vytvořte přihlašovací údaje účtu takto:</span><span class="sxs-lookup"><span data-stu-id="f7ebe-131">Create your account credentials as follows:</span></span>

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> <span data-ttu-id="f7ebe-132">Pokud zadáte jiný název než aktuální účet místního správce na vašem virtuálním počítači, rozšíření VMAccess Přejmenuje účet místního správce, přiřadí zadané heslo k tomuto účtu a vydá událost odhlášení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-132">If you type a different name than the current local administrator account on your VM, the VMAccess extension renames the local administrator account, assigns your specified password to that account, and issues a Remote Desktop logoff event.</span></span> <span data-ttu-id="f7ebe-133">Pokud je účet místního správce na vašem virtuálním počítači vypnutá, rozšíření VMAccess povolí ho.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-133">If the local administrator account on your VM is disabled, the VMAccess extension enables it.</span></span>

<span data-ttu-id="f7ebe-134">Následující příklad aktualizace virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup` k zadané přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-134">The following example updates the VM named `myVM` in the resource group named `myResourceGroup` to the credentials specified.</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="f7ebe-135">**Resetovat konfiguraci služby Vzdálená plocha**</span><span class="sxs-lookup"><span data-stu-id="f7ebe-135">**Reset the Remote Desktop service configuration**</span></span>
<span data-ttu-id="f7ebe-136">Obnovte vzdálený přístup k virtuálnímu počítači pomocí [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-136">Reset remote access to your VM with the [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet.</span></span> <span data-ttu-id="f7ebe-137">Následující příklad resetuje rozšíření pro přístup s názvem `myVMAccess` ve virtuálním počítači s názvem `myVM` v `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="f7ebe-137">The following example resets the access extension named `myVMAccess` on the VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> <span data-ttu-id="f7ebe-138">Kdykoli virtuální počítač může mít pouze jednoho agenta virtuálního počítače přístup.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-138">At any point, a VM can have only a single VM access agent.</span></span> <span data-ttu-id="f7ebe-139">Chcete-li nastavit virtuální počítač přístup k vlastnosti agenta úspěšně, `-ForceRerun` možnost lze použít.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-139">To set the VM access agent properties successfully, the `-ForceRerun` option can be used.</span></span> <span data-ttu-id="f7ebe-140">Při použití `-ForceRerun`, nezapomeňte použít stejný název pro přístup agenta virtuálního počítače v rámci všech předchozích příkazů.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-140">When using `-ForceRerun`, make sure to use the same name for the VM access agent as used in any previous commands.</span></span>

<span data-ttu-id="f7ebe-141">Pokud se pořád nedá vzdáleně připojit k virtuálnímu počítači, najdete v části Další kroky opakujte [řešení potíží s připojení ke vzdálené ploše systému Windows Azure virtuálnímu počítači](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f7ebe-141">If you still can't connect remotely to your virtual machine, see more steps to try at [Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="next-steps"></a><span data-ttu-id="f7ebe-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7ebe-142">Next steps</span></span>
<span data-ttu-id="f7ebe-143">Pokud neodpovídá rozšíření přístup k virtuálnímu počítači Azure a nemůžete resetovat heslo, můžete [resetovat offline místní heslo pro Windows](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f7ebe-143">If the Azure VM access extension does not respond and you are unable to reset the password, you can [reset the local Windows password offline](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="f7ebe-144">Tato metoda je pokročilejší proces a vyžaduje, abyste připojit virtuální pevný disk problematické virtuálního počítače k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-144">This method is a more advanced process and requires you to connect the virtual hard disk of the problematic VM to another VM.</span></span> <span data-ttu-id="f7ebe-145">Postupujte podle kroků popsaných v tomto článku nejprve a pouze pokusí metodu offline heslo resetovat jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="f7ebe-145">Follow the steps documented in this article first, and only attempt the offline password reset method as a last resort.</span></span>

[<span data-ttu-id="f7ebe-146">Rozšíření virtuálního počítače Azure a funkce</span><span class="sxs-lookup"><span data-stu-id="f7ebe-146">Azure VM extensions and features</span></span>](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="f7ebe-147">Připojit k virtuální počítač Azure s protokolu RDP nebo SSH</span><span class="sxs-lookup"><span data-stu-id="f7ebe-147">Connect to an Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="f7ebe-148">Řešení potíží s připojení ke vzdálené ploše do systému Windows Azure virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="f7ebe-148">Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine</span></span>](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

