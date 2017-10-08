---
title: "aaaExpand datový disk připojen tooa virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Rozbalte hello velikost datový disk, který je připojený tooa Windows virtuálního počítače pomocí prostředí PowerShell."
services: virtual-machines-windows
documentationcenter: na
author: cynthn
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.openlocfilehash: b16ad0da9cff9dfffc9dc9ec7dd72891e7ddd745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="increase-hello-size-of-a-data-disk-attached-tooa-windows-vm"></a><span data-ttu-id="ef3f4-103">Zvýšení velikosti hello datový disk připojen tooa virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="ef3f4-103">Increase hello size of a data disk attached tooa Windows VM</span></span>

<span data-ttu-id="ef3f4-104">Pokud potřebujete tooincrease hello velikost hello datový disk připojený tooyour virtuálního počítače, můžete zvýšit velikost hello pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-104">If you need tooincrease hello size of hello data disk attached tooyour virtual machine, you can increase hello size using PowerShell.</span></span> <span data-ttu-id="ef3f4-105">Jakmile zvýšíte hello velikost hello datový disk v nastavení virtuálního počítače Azure hello, musíte taky tooallocate hello nový diskový prostor v rámci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-105">After you increase hello size of hello data disk in hello Azure VM settings, you also need tooallocate hello new disk space within hello VM.</span></span>


## <a name="use-powershell-tooincrease-hello-size-of-a-managed-data-disk"></a><span data-ttu-id="ef3f4-106">Pomocí prostředí Powershell tooincrease hello velikost disku spravovaná data</span><span class="sxs-lookup"><span data-stu-id="ef3f4-106">Use Powershell tooincrease hello size of a managed data disk</span></span>

<span data-ttu-id="ef3f4-107">velikost hello tooincrease spravované datový disk, hello pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ef3f4-107">tooincrease hello size of a managed data disk, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="ef3f4-108">Get-AzureRMReseourceGroup</span><span class="sxs-lookup"><span data-stu-id="ef3f4-108">Get-AzureRMReseourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [<span data-ttu-id="ef3f4-109">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-109">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="ef3f4-110">Stop-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-110">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                        | [<span data-ttu-id="ef3f4-111">Aktualizace AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="ef3f4-111">Update-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [<span data-ttu-id="ef3f4-112">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-112">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

<span data-ttu-id="ef3f4-113">Hello následující skript vás provede procesem získávání hello informace o virtuálních počítačů, výběrem hello datový disk a určení hello nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-113">hello following script will walk you through getting hello VM information, selecting hello data disk and specifying hello new size.</span></span>

```powershell
# Select resource group

    $rg = Get-AzureRMResourceGroup | Out-GridView `
        -Title "Select hello resource group" `
        -PassThru

    $rgName = $rg.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM -ResourceGroupName $rgName `
        | Out-GridView `
            -Title "Select a VM" `
             -PassThru

# Select data disk

    $disk = $vm.StorageProfile.DataDisks | Out-GridView `
        -Title "Select a data disk" `
        -PassThru

# Specify a larger size for hello data disk

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    $diskUpdateConfig = New-AzureRmDiskUpdateConfig -DiskSizeGB $size

# Update hello configuration in Azure

    $managedDisk = Get-AzureRmResource -ResourceId $disk.ManagedDisk.Id
    Update-AzureRmDisk -DiskName $managedDisk.ResourceName -ResourceGroupName $managedDisk.ResourceGroupName -DiskUpdate $diskUpdateConfig

# Start hello VM

    Start-AzureRmVM -ResourceGroupName $rgName -VMName $vm.name
```

## <a name="use-powershell-tooincrease-hello-size-of-an-unmanaged-data-disk"></a><span data-ttu-id="ef3f4-114">Pomocí prostředí PowerShell tooincrease hello velikost disku nespravovaná data</span><span class="sxs-lookup"><span data-stu-id="ef3f4-114">Use PowerShell tooincrease hello size of an unmanaged data disk</span></span>

<span data-ttu-id="ef3f4-115">velikost hello tooincrease nespravované datové disky v účtu úložiště, hello pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ef3f4-115">tooincrease hello size of unmanaged data disks in a storage account, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="ef3f4-116">Get AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="ef3f4-116">Get-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [<span data-ttu-id="ef3f4-117">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-117">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="ef3f4-118">Stop-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-118">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                       | [<span data-ttu-id="ef3f4-119">Set-AzureRmVMDataDisk</span><span class="sxs-lookup"><span data-stu-id="ef3f4-119">Set-AzureRmVMDataDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [<span data-ttu-id="ef3f4-120">Update-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-120">Update-AzureRmVM</span></span>](/powershell/module/azurerm.compute/update-azurermvm)                   | [<span data-ttu-id="ef3f4-121">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="ef3f4-121">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

<span data-ttu-id="ef3f4-122">Hello následující skript vás provede procesem získávání informací o účtu hello virtuálních počítačů a úložiště, výběrem hello datový disk a určení hello nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-122">hello following script will walk you through getting hello VM and storage account information, selecting hello data disk and specifying hello new size.</span></span>

```powershell

# Select Azure Storage Account

    $storageAccount =
        Get-AzureRMStorageAccount | Out-GridView `
            -Title "Select Azure Storage Account" `
            -PassThru

    $rgName = $storageAccount.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM `
    -ResourceGroupName $rgName | Out-GridView `
            -Title "Select a VM …" `
            -PassThru

# Select Data Disk tooresize

    $disk =
        $vm.DataDiskNames | Out-GridView `
            -Title "Select a data disk tooresize" `
            -PassThru


# Specify a larger data disk size

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" `
        -DiskSizeInGB $size

# Update hello configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start hello VM
    Start-AzureRmVM -ResourceGroupName $rgName `
    -VMName $vm.name

```

## <a name="allocate-hello-unallocated-disk-space"></a><span data-ttu-id="ef3f4-123">Přidělit hello volné místo na disku</span><span class="sxs-lookup"><span data-stu-id="ef3f4-123">Allocate hello unallocated disk space</span></span>

<span data-ttu-id="ef3f4-124">Po provedení hello jednotky větší, bude třeba tooallocate hello nové volného místa z v rámci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-124">Once you have made hello drive larger, you need tooallocate hello new unallocated space from within hello VM.</span></span> <span data-ttu-id="ef3f4-125">tooallocate hello místa, můžete připojit toohello virtuálních počítačů pomocí nástroje Správa disků (diskmgmt.msc).</span><span class="sxs-lookup"><span data-stu-id="ef3f4-125">tooallocate hello space, you can connect toohello VM use Disk Management (diskmgmt.msc).</span></span> <span data-ttu-id="ef3f4-126">Nebo, pokud jste povolili WinRM a certifikát na hello virtuálních počítačů, když jste ho vytvářeli, můžete použít vzdálený disk hello tooinitialize prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef3f4-126">Or, if you enabled WinRM and a certificate on hello VM when you created it, you can use remote PowerShell tooinitialize hello disk.</span></span> <span data-ttu-id="ef3f4-127">Můžete také použít rozšíření vlastních skriptů:</span><span class="sxs-lookup"><span data-stu-id="ef3f4-127">You can also use a custom script extension:</span></span>

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

<span data-ttu-id="ef3f4-128">soubor skriptu Hello může obsahovat něco podobného jako tento kód tooincrease hello jednotky přidělení toohello maximální velikost hello disky:</span><span class="sxs-lookup"><span data-stu-id="ef3f4-128">hello script file can contain something like this code tooincrease hello drive allocation toohello maximum size hello disks:</span></span>

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a><span data-ttu-id="ef3f4-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef3f4-129">Next Steps</span></span>
- [<span data-ttu-id="ef3f4-130">Další informace o disky a virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="ef3f4-130">Learn more about disks and VHDs</span></span>](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
