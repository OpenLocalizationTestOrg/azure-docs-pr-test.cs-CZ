---
title: "Použití Windows řešení potíží s virtuálních počítačů v prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak k řešení potíží virtuální počítač s Windows v Azure připojením disk operačního systému k obnovení virtuálního počítače pomocí prostředí Azure PowerShell"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 8b7821b4285e73d461af426bfdfb3fdcc4454517
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a><span data-ttu-id="68dce-103">Řešení potíží s virtuální počítač s Windows pomocí disk operačního systému se připojuje k obnovení virtuálního počítače pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="68dce-103">Troubleshoot a Windows VM by attaching the OS disk to a recovery VM using Azure PowerShell</span></span>
<span data-ttu-id="68dce-104">Pokud Windows virtuálního počítače (VM) v prostředí Azure dojde k chybě spouštěcí nebo disk, musíte provést na virtuálním pevném disku, sám sebe pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="68dce-105">Běžným příkladem bude aplikaci, která selhala aktualizace, která brání virtuálního počítače nebudou moct úspěšně spustil.</span><span class="sxs-lookup"><span data-stu-id="68dce-105">A common example would be a failed application update that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="68dce-106">Tento článek podrobné informace o tom, jak pomocí prostředí Azure PowerShell pro připojení k jiným virtuálním Počítačem Windows opravte případné chyby a pak znovu vytvořte původní virtuální počítač virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-106">This article details how to use Azure PowerShell to connect your virtual hard disk to another Windows VM to fix any errors, then re-create your original VM.</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="68dce-107">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="68dce-107">Recovery process overview</span></span>
<span data-ttu-id="68dce-108">Proces řešení potíží je následující:</span><span class="sxs-lookup"><span data-stu-id="68dce-108">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="68dce-109">Odstraňte virtuální počítač, na zjištění problémy, zachovat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="68dce-109">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="68dce-110">Připojte a připojit virtuální pevný disk na jiný virtuální počítač s Windows pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-110">Attach and mount the virtual hard disk to another Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="68dce-111">Připojení k virtuálnímu počítači pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-111">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="68dce-112">Úpravy souborů nebo spustit žádné nástroje na opravte problémy v původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-112">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="68dce-113">Odpojení virtuálního pevného disku od virtuálního počítače pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-113">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="68dce-114">Vytvoření virtuálního počítače pomocí původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-114">Create a VM using the original virtual hard disk.</span></span>

<span data-ttu-id="68dce-115">Ujistěte se, že máte [nejnovější prostředí Azure PowerShell](/powershell/azure/overview) nainstalován a přihlášení k vašemu předplatnému:</span><span class="sxs-lookup"><span data-stu-id="68dce-115">Make sure that you have [the latest Azure PowerShell](/powershell/azure/overview) installed and logged in to your subscription:</span></span>

```powershell
Login-AzureRMAccount
```

<span data-ttu-id="68dce-116">V následujících příkladech nahraďte názvy parametrů s vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="68dce-116">In the following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="68dce-117">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="68dce-117">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="68dce-118">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="68dce-118">Determine boot issues</span></span>
<span data-ttu-id="68dce-119">Zobrazí se snímek virtuálního počítače v Azure k řešení potíží spouštěcí.</span><span class="sxs-lookup"><span data-stu-id="68dce-119">You can view a screenshot of your VM in Azure to help troubleshoot boot issues.</span></span> <span data-ttu-id="68dce-120">Tento snímek obrazovky může pomoct identifikovat, proč virtuální počítač se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="68dce-120">This screenshot can help identify why a VM fails to boot.</span></span> <span data-ttu-id="68dce-121">Následující příklad získá na snímku obrazovky z virtuálního počítače Windows s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-121">The following example gets the screenshot from the Windows VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

<span data-ttu-id="68dce-122">Zkontrolujte na snímku obrazovky, chcete-li zjistit, proč se nedaří spustit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="68dce-122">Review the screenshot to determine why the VM is failing to boot.</span></span> <span data-ttu-id="68dce-123">Poznámka: všechny specifické chybové zprávy nebo kódy chyb, které jsou dispozici.</span><span class="sxs-lookup"><span data-stu-id="68dce-123">Note any specific error messages or error codes provided.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="68dce-124">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="68dce-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="68dce-125">Než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk, musíte určit název virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="68dce-125">Before you can attach your virtual hard disk to another VM, you need to identify the name of the virtual hard disk (VHD).</span></span>

<span data-ttu-id="68dce-126">Následující příklad načte informace pro virtuální počítač s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-126">The following example gets information for the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="68dce-127">Vyhledejte `Vhd URI` v rámci `StorageProfile` části z výstupu předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="68dce-127">Look for `Vhd URI` within the `StorageProfile` section from the output of the preceding command.</span></span> <span data-ttu-id="68dce-128">Zkrácená následující příklad ukazuje výstup `Vhd URI` na konci bloku kódu:</span><span class="sxs-lookup"><span data-stu-id="68dce-128">The following truncated example output shows the `Vhd URI` towards the end of the code block:</span></span>

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a><span data-ttu-id="68dce-129">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="68dce-129">Delete existing VM</span></span>
<span data-ttu-id="68dce-130">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="68dce-130">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="68dce-131">Virtuální pevný disk je, kde jsou uloženy samotného operačního systému, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="68dce-131">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="68dce-132">Virtuální počítač je jenom metadata, která definuje velikosti či umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="68dce-132">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="68dce-133">Každý virtuální pevný disk má zapůjčení přiřazen při připojen k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="68dce-133">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="68dce-134">Přestože datové disky je možné připojovat a odpojovat dokonce i za běhu virtuálního počítače, disk s operačním systémem není možné odpojit, dokud se neodstraní prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68dce-134">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="68dce-135">Zapůjčení i nadále i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated přidružení disk operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68dce-135">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="68dce-136">Prvním krokem k obnovení virtuálního počítače je odstranit samotné prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68dce-136">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="68dce-137">Když odstraníte virtuální počítač, virtuální pevné disky zůstanou ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="68dce-137">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="68dce-138">Po odstranění virtuálního počítače připojit virtuální pevný disk k jiným virtuálním Počítačem vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="68dce-138">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="68dce-139">Následující příklad odstraní virtuální počítač s názvem `myVM` ze skupiny prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-139">The following example deletes the VM named `myVM` from the resource group named `myResourceGroup`:</span></span>

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="68dce-140">Počkejte, dokud je virtuální počítač dokončí odstraňování před připojit virtuální pevný disk k jiným virtuálním Počítačem.</span><span class="sxs-lookup"><span data-stu-id="68dce-140">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="68dce-141">Zapůjčení na virtuální pevný disk, který přidruží k němu virtuální počítač je nutné uvolnit předtím, než k jiným virtuálním Počítačem můžete připojit virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-141">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="68dce-142">Připojit existující virtuální pevný disk k jiným virtuálním Počítačem</span><span class="sxs-lookup"><span data-stu-id="68dce-142">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="68dce-143">Pro několika dalších krocích použijete jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-143">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="68dce-144">Existující virtuální pevný disk se připojit k řešení potíží VM na Procházet a upravovat obsah na disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-144">You attach the existing virtual hard disk to this troubleshooting VM to browse and edit the disk's content.</span></span> <span data-ttu-id="68dce-145">Tento proces umožňuje opravte všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů protokolu, např.</span><span class="sxs-lookup"><span data-stu-id="68dce-145">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="68dce-146">Vyberte nebo vytvořte jiným virtuálním Počítačem používat pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-146">Choose or create another VM to use for troubleshooting purposes.</span></span>

<span data-ttu-id="68dce-147">Když připojíte existující virtuální pevný disk, zadejte adresu URL na disk získaných v předchozím `Get-AzureRmVM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="68dce-147">When you attach the existing virtual hard disk, specify the URL to the disk obtained in the preceding `Get-AzureRmVM` command.</span></span> <span data-ttu-id="68dce-148">Následující příklad připojí k řešení potíží virtuální počítač s názvem existujícího virtuálního pevného disku `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-148">The following example attaches an existing virtual hard disk to the troubleshooting VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> <span data-ttu-id="68dce-149">Přidání disku vyžaduje, abyste zadejte velikost disku.</span><span class="sxs-lookup"><span data-stu-id="68dce-149">Adding a disk requires you to specify the size of the disk.</span></span> <span data-ttu-id="68dce-150">Jak jsme připojit stávající disk, `-DiskSizeInGB` je zadán jako `$null`.</span><span class="sxs-lookup"><span data-stu-id="68dce-150">As we attach an existing disk, the `-DiskSizeInGB` is specified as `$null`.</span></span> <span data-ttu-id="68dce-151">Tato hodnota zajistí datový disk je správně připojený a bez nutnosti určit true velikost datový disk.</span><span class="sxs-lookup"><span data-stu-id="68dce-151">This value ensures the data disk is correctly attached, and without the need to determine the true size of data disk.</span></span>


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="68dce-152">Připojit disk připojená data</span><span class="sxs-lookup"><span data-stu-id="68dce-152">Mount the attached data disk</span></span>

1. <span data-ttu-id="68dce-153">K řešení potíží virtuální počítač příslušná pověření pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="68dce-153">RDP to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="68dce-154">Následující příklad soubory ke stažení souboru připojení RDP pro virtuální počítač s názvem `myVMRecovery` ve skupině prostředků s názvem `myResourceGroup`a stáhne jeho `C:\Users\ops\Documents`"</span><span class="sxs-lookup"><span data-stu-id="68dce-154">The following example downloads the RDP connection file for the VM named `myVMRecovery` in the resource group named `myResourceGroup`, and downloads it to `C:\Users\ops\Documents`"</span></span>

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. <span data-ttu-id="68dce-155">Datový disk je automaticky zjistil a připojené.</span><span class="sxs-lookup"><span data-stu-id="68dce-155">The data disk is automatically detected and attached.</span></span> <span data-ttu-id="68dce-156">Zobrazení seznamu připojených svazků určit písmeno jednotky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="68dce-156">View the list of attached volumes to determine the drive letter as follows:</span></span>

    ```powershell
    Get-Disk
    ```

    <span data-ttu-id="68dce-157">Následující příklad výstupu zobrazuje virtuální pevný disk připojený disk **2**.</span><span class="sxs-lookup"><span data-stu-id="68dce-157">The following example output shows the virtual hard disk connected a disk **2**.</span></span> <span data-ttu-id="68dce-158">(Můžete také použít `Get-Volume` zobrazíte písmeno jednotky):</span><span class="sxs-lookup"><span data-stu-id="68dce-158">(You can also use `Get-Volume` to view the drive letter):</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="68dce-159">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="68dce-159">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="68dce-160">S existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="68dce-160">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="68dce-161">Jakmile vyřešíte problémy, pokračujte následujícími kroky.</span><span class="sxs-lookup"><span data-stu-id="68dce-161">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="68dce-162">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="68dce-162">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="68dce-163">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-163">Once your errors are resolved, you unmount and detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="68dce-164">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání zapůjčení virtuální pevný disk se připojuje k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="68dce-164">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="68dce-165">Z v rámci relace RDP, odpojte datový disk na váš virtuální počítač pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="68dce-165">From within your RDP session, unmount the data disk on your recovery VM.</span></span> <span data-ttu-id="68dce-166">Je třeba číslo disku z předchozí `Get-Disk` rutiny.</span><span class="sxs-lookup"><span data-stu-id="68dce-166">You need the disk number from the previous `Get-Disk` cmdlet.</span></span> <span data-ttu-id="68dce-167">Poté použijte `Set-Disk` nastavit jako v režimu offline:</span><span class="sxs-lookup"><span data-stu-id="68dce-167">Then, use `Set-Disk` to set the disk as offline:</span></span>

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    <span data-ttu-id="68dce-168">Zkontrolujte disk je teď nastavená jako offline pomocí `Get-Disk` znovu.</span><span class="sxs-lookup"><span data-stu-id="68dce-168">Confirm the disk is now set as offline using `Get-Disk` again.</span></span> <span data-ttu-id="68dce-169">Následující příklad výstupu zobrazuje, že disk je teď nastavená jako offline:</span><span class="sxs-lookup"><span data-stu-id="68dce-169">The following example output shows the disk is now set as offline:</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. <span data-ttu-id="68dce-170">Ukončete relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="68dce-170">Exit your RDP session.</span></span> <span data-ttu-id="68dce-171">Z relace prostředí Azure PowerShell odeberte virtuální pevný disk z virtuálního počítače, řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="68dce-171">From your Azure PowerShell session, remove the virtual hard disk from the troubleshooting VM.</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="68dce-172">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="68dce-172">Create VM from original hard disk</span></span>
<span data-ttu-id="68dce-173">Chcete-li vytvořit virtuální počítač z původní virtuální pevný disk, použijte [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="68dce-173">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="68dce-174">Skutečné šablona JSON je na následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="68dce-174">The actual JSON template is at the following link:</span></span>

- <span data-ttu-id="68dce-175">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD-Existing-vnet/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="68dce-175">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json</span></span>

<span data-ttu-id="68dce-176">Šablona nasadí virtuální počítač do existující virtuální síť pomocí adresy URL virtuálního pevného disku z dřívějších příkazu.</span><span class="sxs-lookup"><span data-stu-id="68dce-176">The template deploys a VM into an existing virtual network, using the VHD URL from the earlier command.</span></span> <span data-ttu-id="68dce-177">Následující příklad nasadí šablony do skupiny prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-177">The following example deploys the template to the resource group named `myResourceGroup`:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

<span data-ttu-id="68dce-178">Odpovězte pokynů pro šablonu, například název virtuálního počítače, typ operačního systému a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="68dce-178">Answer the prompts for the template such as VM name, OS type, and VM size.</span></span> <span data-ttu-id="68dce-179">`osDiskVhdUri` Je stejný jako použil při připojení existujícího virtuálního pevného disku k řešení potíží virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="68dce-179">The `osDiskVhdUri` is the same as previously used when attaching the existing virtual hard disk to the troubleshooting VM.</span></span>


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="68dce-180">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="68dce-180">Re-enable boot diagnostics</span></span>

<span data-ttu-id="68dce-181">Při vytváření virtuálního počítače z existujícího virtuálního pevného disku, nemusí být Diagnostika spouštění automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="68dce-181">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="68dce-182">Následující příklad povolí diagnostiky rozšíření ve virtuálním počítači s názvem `myVMDeployed` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="68dce-182">The following example enables the diagnostic extension on the VM named `myVMDeployed` in the resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a><span data-ttu-id="68dce-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68dce-183">Next steps</span></span>
<span data-ttu-id="68dce-184">Pokud máte problémy s připojením k virtuálnímu počítači, přečtěte si téma [připojení řešení potíží s RDP na virtuální počítač Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="68dce-184">If you are having issues connecting to your VM, see [Troubleshoot RDP connections to an Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="68dce-185">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuálním počítači Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="68dce-185">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="68dce-186">Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="68dce-186">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
