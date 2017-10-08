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
# <a name="how-tooreset-hello-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-hello-classic-deployment-model"></a><span data-ttu-id="0458f-103">Služba Vzdálená plocha hello tooreset nebo jeho heslo pro přihlášení do virtuálního počítače Windows vytváření pomocí modelu nasazení Classic hello</span><span class="sxs-lookup"><span data-stu-id="0458f-103">How tooreset hello Remote Desktop service or its login password in a Windows VM created using hello Classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0458f-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="0458f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="0458f-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="0458f-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="0458f-107">Můžete také [proveďte tyto kroky pro virtuální počítače vytvořené pomocí modelu nasazení Resource Manager hello](../reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="0458f-107">You can also [perform these steps for VMs created with hello Resource Manager deployment model](../reset-rdp.md).</span></span>

<span data-ttu-id="0458f-108">Pokud se nemůžete připojit tooa Windows virtuální počítač (VM), můžete resetovat heslo místního správce hello nebo resetovat konfiguraci služby Vzdálená plocha hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-108">If you can't connect tooa Windows virtual machine (VM), you can reset hello local administrator password or reset hello Remote Desktop service configuration.</span></span> <span data-ttu-id="0458f-109">Můžete buď hello Azure portal nebo hello rozšíření pro přístup virtuálních počítačů v prostředí Azure PowerShell tooreset hello heslo.</span><span class="sxs-lookup"><span data-stu-id="0458f-109">You can use either hello Azure portal or hello VM Access extension in Azure PowerShell tooreset hello password.</span></span>

## <a name="ways-tooreset-configuration-or-credentials"></a><span data-ttu-id="0458f-110">Konfigurace způsobů tooreset nebo přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="0458f-110">Ways tooreset configuration or credentials</span></span>
<span data-ttu-id="0458f-111">Vzdálená plocha a přihlašovací údaje můžete resetovat několika různými způsoby, v závislosti na vašich potřeb:</span><span class="sxs-lookup"><span data-stu-id="0458f-111">You can reset Remote Desktop services and credentials in a few different ways, depending on your needs:</span></span>

- [<span data-ttu-id="0458f-112">Resetovat pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0458f-112">Reset using hello Azure portal</span></span>](#azure-portal)
- [<span data-ttu-id="0458f-113">Resetovat pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0458f-113">Reset using Azure PowerShell</span></span>](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a><span data-ttu-id="0458f-114">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0458f-114">Azure portal</span></span>
<span data-ttu-id="0458f-115">Můžete použít hello [portál Azure](https://portal.azure.com) tooreset hello služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="0458f-115">You can use hello [Azure portal](https://portal.azure.com) tooreset hello Remote Desktop service.</span></span> <span data-ttu-id="0458f-116">Klikněte na tlačítko hello tři řádky v levém horním rohu hello tooexpand hello nabídce portálu a potom klikněte na **virtuálních počítačů (klasické)**:</span><span class="sxs-lookup"><span data-stu-id="0458f-116">tooexpand hello portal menu, click hello three bars in hello upper left corner and then click **Virtual machines (classic)**:</span></span>

![Procházením vyhledejte virtuálních počítačů Azure](./media/reset-rdp/Portal-Select-Classic-VM.png)

<span data-ttu-id="0458f-118">Vyberte virtuální počítač se systémem Windows a pak klikněte na tlačítko **resetovat vzdálený...** . hello následující otevře se dialogové okno Konfigurace vzdálené plochy tooreset hello:</span><span class="sxs-lookup"><span data-stu-id="0458f-118">Select your Windows virtual machine and then click **Reset Remote...**. hello following dialog appears tooreset hello Remote Desktop configuration:</span></span>

![Stránka Konfigurace resetování protokolu RDP](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

<span data-ttu-id="0458f-120">Můžete taky resetovat hello uživatelské jméno a heslo účtu místního správce hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-120">You can also reset hello username and password of hello local administrator account.</span></span> <span data-ttu-id="0458f-121">Z virtuálního počítače, klikněte na tlačítko **podporu + Poradce při potížích s** > **resetovat heslo**.</span><span class="sxs-lookup"><span data-stu-id="0458f-121">From your VM, click **Support + Troubleshooting** > **Reset password**.</span></span> <span data-ttu-id="0458f-122">Zobrazí se okno pro resetování hesla Hello:</span><span class="sxs-lookup"><span data-stu-id="0458f-122">hello password reset blade is displayed:</span></span>

![Stránka pro resetování hesla](./media/reset-rdp/Portal-PW-Reset-Windows.png)

<span data-ttu-id="0458f-124">Po zadání hello nové uživatelské jméno a heslo, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0458f-124">After you enter hello new user name and password, click **Save**.</span></span>

## <a name="vmaccess-extension-and-powershell"></a><span data-ttu-id="0458f-125">Rozšíření VMAccess a prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0458f-125">VMAccess extension and PowerShell</span></span>
<span data-ttu-id="0458f-126">Ujistěte se, zda text hello, Agent virtuálního počítače je nainstalována na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-126">Make sure hello VM Agent is installed on hello virtual machine.</span></span> <span data-ttu-id="0458f-127">Hello rozšíření VMAccess nepotřebuje toobe nainstalovat, abyste mohli používat, dokud hello agenta virtuálního počítače je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0458f-127">hello VMAccess extension doesn't need toobe installed before you can use it, as long as hello VM Agent is available.</span></span> <span data-ttu-id="0458f-128">Ověřte, že hello agenta virtuálního počítače je již nainstalována pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0458f-128">Verify that hello VM Agent is already installed by using hello following command.</span></span> <span data-ttu-id="0458f-129">(Nahraďte "myCloudService" a "Můjvp" hello názvy cloudové služby a virtuální počítač, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="0458f-129">(Replace "myCloudService" and "myVM" by hello names of your cloud service and your VM, respectively.</span></span> <span data-ttu-id="0458f-130">Dozvíte se tak, že spustíte tyto názvy `Get-AzureVM` bez parametrů.)</span><span class="sxs-lookup"><span data-stu-id="0458f-130">You can learn these names by running `Get-AzureVM` without any parameters.)</span></span>

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="0458f-131">Pokud hello **zápisu hostitele** příkaz zobrazí **True**, hello je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0458f-131">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="0458f-132">Pokud se zobrazí **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="0458f-132">If it displays **False**, see hello instructions and a link toohello download in hello [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog post.</span></span>

<span data-ttu-id="0458f-133">Pokud jste vytvořili hello virtuálního počítače pomocí portálu hello, zkontrolujte, zda `$vm.GetInstance().ProvisionGuestAgent` vrátí **True**.</span><span class="sxs-lookup"><span data-stu-id="0458f-133">If you created hello virtual machine by using hello portal, check whether `$vm.GetInstance().ProvisionGuestAgent` returns **True**.</span></span> <span data-ttu-id="0458f-134">Pokud ne, můžete ho nastavit pomocí tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="0458f-134">If not, you can set it by using this command:</span></span>

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

<span data-ttu-id="0458f-135">Tento příkaz zabrání hello následující chyba, když používáte hello **Set-AzureVMExtension** v dalších krocích hello: "Zřízení Agent hosta musí být povolené na objekt virtuálního počítače hello před nastavením rozšíření pro přístup virtuálních počítačů IaaS."</span><span class="sxs-lookup"><span data-stu-id="0458f-135">This command prevents hello following error when you're running hello **Set-AzureVMExtension** command in hello next steps: “Provision Guest Agent must be enabled on hello VM object before setting IaaS VM Access Extension.”</span></span>

### <a name="reset-hello-local-administrator-account-password"></a><span data-ttu-id="0458f-136">**Resetovat heslo účtu místního správce hello**</span><span class="sxs-lookup"><span data-stu-id="0458f-136">**Reset hello local administrator account password**</span></span>
<span data-ttu-id="0458f-137">Vytvoření pověření přihlášení pomocí hello aktuální název účtu místního správce a nové heslo a pak spusťte hello `Set-AzureVMAccessExtension` následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0458f-137">Create a sign-in credential with hello current local administrator account name and a new password, and then run hello `Set-AzureVMAccessExtension` as follows.</span></span>

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

<span data-ttu-id="0458f-138">Pokud zadáte jiný název než hello aktuálního účtu, hello rozšíření VMAccess přejmenuje hello účet místního správce, přiřadí hello heslo toothat účet a vydá odhlašování vzdálené plochy. Pokud je účet místního správce hello vypnutá, hello rozšíření VMAccess povolí ho.</span><span class="sxs-lookup"><span data-stu-id="0458f-138">If you type a different name than hello current account, hello VMAccess extension renames hello local administrator account, assigns hello password toothat account, and issues a Remote Desktop sign-out. If hello local administrator account is disabled, hello VMAccess extension enables it.</span></span>

<span data-ttu-id="0458f-139">Tyto příkazy také obnovit konfiguraci služby Vzdálená plocha hello.</span><span class="sxs-lookup"><span data-stu-id="0458f-139">These commands also reset hello Remote Desktop service configuration.</span></span>

### <a name="reset-hello-remote-desktop-service-configuration"></a><span data-ttu-id="0458f-140">**Resetovat konfiguraci služby Vzdálená plocha hello**</span><span class="sxs-lookup"><span data-stu-id="0458f-140">**Reset hello Remote Desktop service configuration**</span></span>
<span data-ttu-id="0458f-141">tooreset hello konfiguraci služby Vzdálená plocha, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0458f-141">tooreset hello Remote Desktop service configuration, run hello following command:</span></span>

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

<span data-ttu-id="0458f-142">Hello rozšíření VMAccess běží na virtuálním počítači hello dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="0458f-142">hello VMAccess extension runs two commands on hello virtual machine:</span></span>

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

<span data-ttu-id="0458f-143">Tento příkaz povolí hello předdefinovaná brány Windows Firewall skupina, která umožní příchozí provoz vzdálené plochy, který používá TCP port 3389.</span><span class="sxs-lookup"><span data-stu-id="0458f-143">This command enables hello built-in Windows Firewall group that allows incoming Remote Desktop traffic, which uses TCP port 3389.</span></span>

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

<span data-ttu-id="0458f-144">Tento příkaz nastaví hello fDenyTSConnections registru hodnotu too0, povolení připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="0458f-144">This command sets hello fDenyTSConnections registry value too0, enabling Remote Desktop connections.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0458f-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0458f-145">Next steps</span></span>
<span data-ttu-id="0458f-146">Pokud neodpovídá hello rozšíření přístup k virtuálnímu počítači Azure a jsou nelze tooreset hello heslo, můžete [resetování hello místní heslo systému Windows do režimu offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0458f-146">If hello Azure VM access extension does not respond and you are unable tooreset hello password, you can [reset hello local Windows password offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="0458f-147">Tato metoda je pokročilejší proces a vyžaduje, abyste tooconnect hello virtuální pevný disk hello problematické tooanother virtuálních počítačů VM.</span><span class="sxs-lookup"><span data-stu-id="0458f-147">This method is a more advanced process and requires you tooconnect hello virtual hard disk of hello problematic VM tooanother VM.</span></span> <span data-ttu-id="0458f-148">Postupujte podle hello kroků popsaných v tomto článku nejprve a pouze pokusí hello offline heslo resetovat metoda jako poslední možnost.</span><span class="sxs-lookup"><span data-stu-id="0458f-148">Follow hello steps documented in this article first, and only attempt hello offline password reset method as a last resort.</span></span>

[<span data-ttu-id="0458f-149">Rozšíření virtuálního počítače Azure a funkce</span><span class="sxs-lookup"><span data-stu-id="0458f-149">Azure VM extensions and features</span></span>](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[<span data-ttu-id="0458f-150">Připojení pomocí protokolu RDP nebo SSH tooan virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="0458f-150">Connect tooan Azure virtual machine with RDP or SSH</span></span>](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[<span data-ttu-id="0458f-151">Řešení potíží s vzdálené plochy připojení tooa systému Windows virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="0458f-151">Troubleshoot Remote Desktop connections tooa Windows-based Azure virtual machine</span></span>](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

