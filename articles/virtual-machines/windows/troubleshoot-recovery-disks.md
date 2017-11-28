---
title: "aaaUse a řešení potíží s virtuálních počítačů v prostředí Azure PowerShell systému Windows | Microsoft Docs"
description: "Zjistěte, jak problémy tootroubleshoot virtuální počítač s Windows v Azure připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí prostředí Azure PowerShell"
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
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a><span data-ttu-id="3c9ee-103">Řešení potíží s virtuální počítač s Windows připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="3c9ee-103">Troubleshoot a Windows VM by attaching hello OS disk tooa recovery VM using Azure PowerShell</span></span>
<span data-ttu-id="3c9ee-104">Pokud Windows virtuálního počítače (VM) v prostředí Azure dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="3c9ee-105">Běžným příkladem bude aplikaci, která selhala aktualizace, která brání hello virtuální počítač schopný tooboot úspěšně.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-105">A common example would be a failed application update that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="3c9ee-106">Tento článek podrobnosti o tom, jak tooconnect toouse prostředí Azure PowerShell vaše virtuální pevný disk tooanother virtuální počítač s Windows toofix všechny chyby, potom je znovu vytvořit původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-106">This article details how toouse Azure PowerShell tooconnect your virtual hard disk tooanother Windows VM toofix any errors, then re-create your original VM.</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="3c9ee-107">Přehled procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="3c9ee-107">Recovery process overview</span></span>
<span data-ttu-id="3c9ee-108">řešení potíží s procesem Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="3c9ee-109">Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="3c9ee-110">Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Windows pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-110">Attach and mount hello virtual hard disk tooanother Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="3c9ee-111">Připojte toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="3c9ee-112">Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="3c9ee-113">Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="3c9ee-114">Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-114">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="3c9ee-115">Ujistěte se, že máte [hello nejnovější prostředí Azure PowerShell](/powershell/azure/overview) nainstalován a přihlášení tooyour předplatného:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-115">Make sure that you have [hello latest Azure PowerShell](/powershell/azure/overview) installed and logged in tooyour subscription:</span></span>

```powershell
Login-AzureRMAccount
```

<span data-ttu-id="3c9ee-116">V hello následujících příkladech nahraďte vlastními hodnotami názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-116">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="3c9ee-117">Zahrnout názvy parametrů příklad `myResourceGroup`, `mystorageaccount`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-117">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="3c9ee-118">Určení spouštěcí problémy</span><span class="sxs-lookup"><span data-stu-id="3c9ee-118">Determine boot issues</span></span>
<span data-ttu-id="3c9ee-119">Zobrazí se snímek virtuálního počítače v Azure toohelp potíží při spouštění systému.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-119">You can view a screenshot of your VM in Azure toohelp troubleshoot boot issues.</span></span> <span data-ttu-id="3c9ee-120">Tento snímek obrazovky může pomoct identifikovat, proč tooboot selže virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-120">This screenshot can help identify why a VM fails tooboot.</span></span> <span data-ttu-id="3c9ee-121">Hello následující příklad načte hello – snímek obrazovky ze hello virtuální počítač s Windows s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-121">hello following example gets hello screenshot from hello Windows VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

<span data-ttu-id="3c9ee-122">Zkontrolujte toodetermine – snímek obrazovky hello proč hello virtuálního počítače selhává tooboot.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-122">Review hello screenshot toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="3c9ee-123">Poznámka: všechny specifické chybové zprávy nebo kódy chyb, které jsou dispozici.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-123">Note any specific error messages or error codes provided.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="3c9ee-124">Zobrazení podrobností existující virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="3c9ee-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="3c9ee-125">Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span>

<span data-ttu-id="3c9ee-126">Hello následující příklad načte informace o hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-126">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="3c9ee-127">Vyhledejte `Vhd URI` v rámci hello `StorageProfile` části z výstupu hello hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-127">Look for `Vhd URI` within hello `StorageProfile` section from hello output of hello preceding command.</span></span> <span data-ttu-id="3c9ee-128">Hello následující zkrácený příklad výstupu zobrazuje následující stavy hello `Vhd URI` hello konci hello blok kódu:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-128">hello following truncated example output shows hello `Vhd URI` towards hello end of hello code block:</span></span>

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


## <a name="delete-existing-vm"></a><span data-ttu-id="3c9ee-129">Odstraňte existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="3c9ee-129">Delete existing VM</span></span>
<span data-ttu-id="3c9ee-130">Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-130">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="3c9ee-131">Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-131">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="3c9ee-132">Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-132">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="3c9ee-133">Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-133">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="3c9ee-134">I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-134">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="3c9ee-135">Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-135">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="3c9ee-136">první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-136">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="3c9ee-137">Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-137">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="3c9ee-138">Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-138">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="3c9ee-139">Následující příklad odstranění Hello hello virtuálního počítače s názvem `myVM` z hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-139">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="3c9ee-140">Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-140">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="3c9ee-141">Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-141">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="3c9ee-142">Připojit existující virtuální pevný disk tooanother virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="3c9ee-142">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="3c9ee-143">Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-143">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="3c9ee-144">Připojte hello existující virtuální pevný disk toothis řešení potíží s toobrowse virtuálních počítačů a upravit obsah hello disku.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-144">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="3c9ee-145">Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-145">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="3c9ee-146">Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-146">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="3c9ee-147">Když připojíte hello existující virtuální pevný disk, zadejte hello URL toohello disk získaných v předchozím hello `Get-AzureRmVM` příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-147">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `Get-AzureRmVM` command.</span></span> <span data-ttu-id="3c9ee-148">Hello následující příklad připojí existující virtuální pevný disk toohello řešení potíží s virtuálním počítačem s názvem `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-148">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> <span data-ttu-id="3c9ee-149">Přidání disku vyžaduje toospecify hello velikost disku hello.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-149">Adding a disk requires you toospecify hello size of hello disk.</span></span> <span data-ttu-id="3c9ee-150">Jak jsme připojit stávající disk, hello `-DiskSizeInGB` je zadán jako `$null`.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-150">As we attach an existing disk, hello `-DiskSizeInGB` is specified as `$null`.</span></span> <span data-ttu-id="3c9ee-151">Tato hodnota zajistí hello datový disk je správně připojena a bez hello potřebovat toodetermine hello true velikost datového disku.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-151">This value ensures hello data disk is correctly attached, and without hello need toodetermine hello true size of data disk.</span></span>


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="3c9ee-152">Připojte disk připojená data hello</span><span class="sxs-lookup"><span data-stu-id="3c9ee-152">Mount hello attached data disk</span></span>

1. <span data-ttu-id="3c9ee-153">Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-153">RDP tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="3c9ee-154">Hello následující příklad soubory ke stažení souboru připojení hello RDP pro virtuální počítač s názvem hello `myVMRecovery` v hello skupinu prostředků s názvem `myResourceGroup`a stáhne je příliš`C:\Users\ops\Documents`"</span><span class="sxs-lookup"><span data-stu-id="3c9ee-154">hello following example downloads hello RDP connection file for hello VM named `myVMRecovery` in hello resource group named `myResourceGroup`, and downloads it too`C:\Users\ops\Documents`"</span></span>

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. <span data-ttu-id="3c9ee-155">Hello datový disk je automaticky zjistil a připojené.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-155">hello data disk is automatically detected and attached.</span></span> <span data-ttu-id="3c9ee-156">Zobrazte seznam hello podle písmene jednotky hello toodetermine připojené svazky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-156">View hello list of attached volumes toodetermine hello drive letter as follows:</span></span>

    ```powershell
    Get-Disk
    ```

    <span data-ttu-id="3c9ee-157">Hello následující příklad výstupu zobrazuje hello virtuální pevný disk připojený disk **2**.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-157">hello following example output shows hello virtual hard disk connected a disk **2**.</span></span> <span data-ttu-id="3c9ee-158">(Můžete také použít `Get-Volume` písmeno jednotky tooview hello):</span><span class="sxs-lookup"><span data-stu-id="3c9ee-158">(You can also use `Get-Volume` tooview hello drive letter):</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="3c9ee-159">Vyřešte problémy na původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="3c9ee-159">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="3c9ee-160">S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-160">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="3c9ee-161">Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-161">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="3c9ee-162">Odpojte Image a odpojit původní virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="3c9ee-162">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="3c9ee-163">Jakmile jsou vaše chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-163">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="3c9ee-164">Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-164">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="3c9ee-165">Z v rámci relace RDP, odpojte hello datový disk na váš virtuální počítač pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-165">From within your RDP session, unmount hello data disk on your recovery VM.</span></span> <span data-ttu-id="3c9ee-166">Je třeba číslo disku hello z hello předchozí `Get-Disk` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-166">You need hello disk number from hello previous `Get-Disk` cmdlet.</span></span> <span data-ttu-id="3c9ee-167">Poté použijte `Set-Disk` tooset hello jako v režimu offline disku:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-167">Then, use `Set-Disk` tooset hello disk as offline:</span></span>

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    <span data-ttu-id="3c9ee-168">Potvrďte hello disk je teď nastavená jako offline pomocí `Get-Disk` znovu.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-168">Confirm hello disk is now set as offline using `Get-Disk` again.</span></span> <span data-ttu-id="3c9ee-169">Hello následující příklad výstupu zobrazuje hello disk je teď nastavená jako offline:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-169">hello following example output shows hello disk is now set as offline:</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. <span data-ttu-id="3c9ee-170">Ukončete relaci protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-170">Exit your RDP session.</span></span> <span data-ttu-id="3c9ee-171">Z relace prostředí Azure PowerShell odeberte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-171">From your Azure PowerShell session, remove hello virtual hard disk from hello troubleshooting VM.</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="3c9ee-172">Vytvoření virtuálního počítače z původního pevného disku</span><span class="sxs-lookup"><span data-stu-id="3c9ee-172">Create VM from original hard disk</span></span>
<span data-ttu-id="3c9ee-173">použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-173">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="3c9ee-174">Šablona JSON skutečné Hello je na hello následující odkaz:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-174">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="3c9ee-175">https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/Master/201-VM-Specialized-VHD-Existing-vnet/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="3c9ee-175">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json</span></span>

<span data-ttu-id="3c9ee-176">Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-176">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="3c9ee-177">Hello následující příklad nasadí hello šablony toohello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-177">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

<span data-ttu-id="3c9ee-178">Odpověď hello vyzve k zadání šablony hello například název virtuálního počítače, typ operačního systému a velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-178">Answer hello prompts for hello template such as VM name, OS type, and VM size.</span></span> <span data-ttu-id="3c9ee-179">Hello `osDiskVhdUri` hello je stejný jako použil při připojení hello existující virtuální pevný disk toohello řešení potíží s virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-179">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span>


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="3c9ee-180">Opětovné povolení Diagnostika spouštění</span><span class="sxs-lookup"><span data-stu-id="3c9ee-180">Re-enable boot diagnostics</span></span>

<span data-ttu-id="3c9ee-181">Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené.</span><span class="sxs-lookup"><span data-stu-id="3c9ee-181">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="3c9ee-182">Hello následující příklad povolí rozšíření diagnostiky hello na hello virtuálního počítače s názvem `myVMDeployed` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="3c9ee-182">hello following example enables hello diagnostic extension on hello VM named `myVMDeployed` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a><span data-ttu-id="3c9ee-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3c9ee-183">Next steps</span></span>
<span data-ttu-id="3c9ee-184">Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [tooan řešení potíží s RDP připojení virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-184">If you are having issues connecting tooyour VM, see [Troubleshoot RDP connections tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3c9ee-185">Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuálním počítači Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-185">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="3c9ee-186">Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c9ee-186">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
