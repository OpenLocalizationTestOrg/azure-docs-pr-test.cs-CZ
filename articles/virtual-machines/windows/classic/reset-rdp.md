---
title: "Resetovat heslo nebo konfigurace vzdálené plochy na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak resetovat heslo k účtu nebo služeb vzdálené plochy na virtuální počítač s Windows vytvořené pomocí modelu nasazení Classic pomocí portálu Azure nebo Azure PowerShell."
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
ms.openlocfilehash: 43e5cf1ab3bc3121d7e3915ea0785998e0ee2fc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a><span data-ttu-id="98c96-103">Obnovení služby Vzdálená plocha nebo jeho heslo pro přihlášení do systému Windows virtuálního počítače, vytvořené pomocí modelu nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="98c96-103">How to reset the Remote Desktop service or its login password in a Windows VM created using the Classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="98c96-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="98c96-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="98c96-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="98c96-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="98c96-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="98c96-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="98c96-107">Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Resource Manager](../reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="98c96-107">You can also [perform these steps for VMs created with the Resource Manager deployment model](../reset-rdp.md).</span></span>

<span data-ttu-id="98c96-108">Pokud se nemůžete připojit k virtuálnímu počítači (VM) systému Windows, můžete resetovat heslo místního správce nebo resetovat konfiguraci služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="98c96-108">If you can't connect to a Windows virtual machine (VM), you can reset the local administrator password or reset the Remote Desktop service configuration.</span></span> <span data-ttu-id="98c96-109">Portál Azure nebo rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell můžete použít k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="98c96-109">You can use either the Azure portal or the VM Access extension in Azure PowerShell to reset the password.</span></span>

## <a name="ways-to-reset-configuration-or-credentials"></a><span data-ttu-id="98c96-110">Jak obnovit konfiguraci nebo přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="98c96-110">Ways to reset configuration or credentials</span></span>
<span data-ttu-id="98c96-111">Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:</span><span class="sxs-lookup"><span data-stu-id="98c96-111">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="98c96-112">Resetovat pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="98c96-112">Reset using the Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="98c96-113">Resetovat pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="98c96-113">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="98c96-114">portál Azure</span><span class="sxs-lookup"><span data-stu-id="98c96-114">Azure portal</span></span>
<span data-ttu-id="98c96-115">Můžete použít [portál Azure](https://portal.azure.com) resetovat služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="98c96-115">You can use the [Azure portal](https://portal.azure.com) to reset the Remote Desktop service.</span></span> <span data-ttu-id="98c96-116">Rozbalte nabídku portálu, klikněte na tři řádky v levém horním rohu a pak klikněte na **virtuálních počítačů (klasické)**:</span><span class="sxs-lookup"><span data-stu-id="98c96-116">To expand the portal menu, click the three bars in the upper left corner and then click **Virtual machines (classic)**:</span></span>

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-Classic-VM.png)

<span data-ttu-id="98c96-118">Vyberte virtuální počítač se systémem Windows a pak klikněte na tlačítko **resetovat vzdálený...** .</span><span class="sxs-lookup"><span data-stu-id="98c96-118">Select your Windows virtual machine and then click **Reset Remote...**.</span></span> <span data-ttu-id="98c96-119">Resetování konfigurace vzdálené plochy se zobrazí dialogové okno následující:</span><span class="sxs-lookup"><span data-stu-id="98c96-119">The following dialog appears to reset the Remote Desktop configuration:</span></span>

![Stránka Konfigurace resetování protokolu RDP](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

<span data-ttu-id="98c96-121">Můžete taky resetovat uživatelské jméno a heslo účtu místního správce.</span><span class="sxs-lookup"><span data-stu-id="98c96-121">You can also reset the username and password of the local administrator account.</span></span> <span data-ttu-id="98c96-122">Z virtuálního počítače, klikněte na tlačítko **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="98c96-122">From your VM, click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="98c96-123">Zobrazí se okno resetování hesla:</span><span class="sxs-lookup"><span data-stu-id="98c96-123">The password reset blade is displayed:</span></span>

![Stránka pro resetování hesla](./media/reset-rdp/Portal-PW-Reset-Windows.png)

<span data-ttu-id="98c96-125">Když zadáte nové uživatelské jméno a heslo, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="98c96-125">After you enter the new user name and password, click **Save**.</span></span>

## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="98c96-126">Rozšíření VMAccess a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="98c96-126">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="98c96-127">Zkontrolujte, zda že je na virtuálním počítači nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="98c96-127">Make sure the VM Agent is installed on the virtual machine.</span></span> <span data-ttu-id="98c96-128">Rozšíření VMAccess není potřeba nainstalovat, abyste mohli používat, dokud Agent virtuálního počítače je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="98c96-128">The VMAccess extension doesn't need to be installed before you can use it, as long as the VM Agent is available.</span></span> <span data-ttu-id="98c96-129">Ověřte, zda je Agent virtuálního počítače již nainstalován pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="98c96-129">Verify that the VM Agent is already installed by using the following command.</span></span> <span data-ttu-id="98c96-130">(Nahraďte "myCloudService" a "Můjvp" názvy cloudové služby a virtuální počítač, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="98c96-130">(Replace "myCloudService" and "myVM" by the names of your cloud service and your VM, respectively.</span></span> <span data-ttu-id="98c96-131">Dozvíte se tak, že spustíte tyto názvy `Get-AzureVM` bez parametrů.)</span><span class="sxs-lookup"><span data-stu-id="98c96-131">You can learn these names by running `Get-AzureVM` without any parameters.)</span></span>

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="98c96-132">Pokud **zápisu hostitele** příkaz zobrazí **True**, je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="98c96-132">If the **write-host** command displays **True**, the VM Agent is installed.</span></span> <span data-ttu-id="98c96-133">Pokud se zobrazí **False**, najdete pokyny a odkaz na stažení v [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="98c96-133">If it displays **False**, see the instructions and a link to the download in the [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog post.</span></span>

<span data-ttu-id="98c96-134">Pokud vytvoříte virtuální počítač pomocí portálu, zkontrolujte, zda `$vm.GetInstance().ProvisionGuestAgent` vrátí **True**.</span><span class="sxs-lookup"><span data-stu-id="98c96-134">If you created the virtual machine by using the portal, check whether `$vm.GetInstance().ProvisionGuestAgent` returns **True**.</span></span> <span data-ttu-id="98c96-135">Pokud ne, můžete ho nastavit pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="98c96-135">If not, you can set it by using this command:</span></span>

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

<span data-ttu-id="98c96-136">Tento příkaz zabrání k následující chybě, když používáte **Set-AzureVMExtension** příkazu v dalších krocích: "Zřízení Agent hosta musí být povolené na objekt virtuálního počítače před nastavením rozšíření pro přístup virtuálních počítačů IaaS."</span><span class="sxs-lookup"><span data-stu-id="98c96-136">This command prevents the following error when you're running the **Set-AzureVMExtension** command in the next steps: “Provision Guest Agent must be enabled on the VM object before setting IaaS VM Access Extension.”</span></span>

### <a name="reset-the-local-administrator-account-password"></a><span data-ttu-id="98c96-137">**Resetovat heslo k účtu místního správce**</span><span class="sxs-lookup"><span data-stu-id="98c96-137">**Reset the local administrator account password**</span></span>
<span data-ttu-id="98c96-138">Vytvoření pověření přihlášení pomocí aktuální název účtu místního správce a nové heslo a pak spusťte `Set-AzureVMAccessExtension` následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="98c96-138">Create a sign-in credential with the current local administrator account name and a new password, and then run the `Set-AzureVMAccessExtension` as follows.</span></span>

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

<span data-ttu-id="98c96-139">Pokud zadáte jiný název než aktuálního účtu, rozšíření VMAccess Přejmenuje účet místního správce, přiřadí heslo k tomuto účtu a vydá odhlašování vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="98c96-139">If you type a different name than the current account, the VMAccess extension renames the local administrator account, assigns the password to that account, and issues a Remote Desktop sign-out.</span></span> <span data-ttu-id="98c96-140">Pokud je účet místního správce zakázán, rozšíření VMAccess povolí ho.</span><span class="sxs-lookup"><span data-stu-id="98c96-140">If the local administrator account is disabled, the VMAccess extension enables it.</span></span>

<span data-ttu-id="98c96-141">Tyto příkazy také obnovit konfiguraci služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="98c96-141">These commands also reset the Remote Desktop service configuration.</span></span>

### <a name="reset-the-remote-desktop-service-configuration"></a><span data-ttu-id="98c96-142">**Resetovat konfiguraci služby Vzdálená plocha**</span><span class="sxs-lookup"><span data-stu-id="98c96-142">**Reset the Remote Desktop service configuration**</span></span>
<span data-ttu-id="98c96-143">Chcete-li obnovit konfiguraci služby Vzdálená plocha, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="98c96-143">To reset the Remote Desktop service configuration, run the following command:</span></span>

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

<span data-ttu-id="98c96-144">Rozšíření VMAccess běží na virtuálním počítači dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="98c96-144">The VMAccess extension runs two commands on the virtual machine:</span></span>

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

<span data-ttu-id="98c96-145">Tento příkaz povolí předdefinovaná skupina brány Windows Firewall, která umožní příchozí provoz vzdálené plochy, který používá TCP port 3389.</span><span class="sxs-lookup"><span data-stu-id="98c96-145">This command enables the built-in Windows Firewall group that allows incoming Remote Desktop traffic, which uses TCP port 3389.</span></span>

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

<span data-ttu-id="98c96-146">Tento příkaz nastaví fDenyTSConnections hodnotu registru na 0, povolení připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="98c96-146">This command sets the fDenyTSConnections registry value to 0, enabling Remote Desktop connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98c96-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98c96-147">Next steps</span></span>
<span data-ttu-id="98c96-148">Pokud neodpovídá rozšíření přístup k virtuálnímu počítači Azure a nemůžete resetovat heslo, můžete [resetovat offline místní heslo pro Windows](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98c96-148">If the Azure VM access extension does not respond and you are unable to reset the password, you can [reset the local Windows password offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="98c96-149">Tato metoda je pokročilejší proces a vyžaduje, abyste připojit virtuální pevný disk problematické virtuálního počítače k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="98c96-149">This method is a more advanced process and requires you to connect the virtual hard disk of the problematic VM to another VM.</span></span> <span data-ttu-id="98c96-150">Postupujte podle kroků popsaných v tomto článku nejprve a pouze pokusí metodu offline heslo resetovat jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="98c96-150">Follow the steps documented in this article first, and only attempt the offline password reset method as a last resort.</span></span>

[<span data-ttu-id="98c96-151">Rozšíření virtuálního počítače Azure a funkce</span><span class="sxs-lookup"><span data-stu-id="98c96-151">Azure VM extensions and features</span></span>](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="98c96-152">Připojit k virtuální počítač Azure s protokolu RDP nebo SSH</span><span class="sxs-lookup"><span data-stu-id="98c96-152">Connect to an Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="98c96-153">Řešení potíží s připojení ke vzdálené ploše do systému Windows Azure virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="98c96-153">Troubleshoot Remote Desktop connections to a Windows-based Azure virtual machine</span></span>](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

