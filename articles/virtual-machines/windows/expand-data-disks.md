---
title: "Rozbalte datový disk připojen k virtuálnímu počítači Windows v Azure | Microsoft Docs"
description: "Zvětšení velikosti datový disk, který je připojen k virtuálního počítače s Windows pomocí prostředí PowerShell."
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
ms.openlocfilehash: 5529856c2ffcd2942fe3fc2b438f7e3fd16a67b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="increase-the-size-of-a-data-disk-attached-to-a-windows-vm"></a><span data-ttu-id="5d51b-103">Zvětšete velikost datový disk připojen k virtuálnímu počítači Windows</span><span class="sxs-lookup"><span data-stu-id="5d51b-103">Increase the size of a data disk attached to a Windows VM</span></span>

<span data-ttu-id="5d51b-104">Pokud je potřeba zvýšit velikost datový disk připojen k virtuálnímu počítači, můžete zvýšit velikost pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5d51b-104">If you need to increase the size of the data disk attached to your virtual machine, you can increase the size using PowerShell.</span></span> <span data-ttu-id="5d51b-105">Jakmile zvýšíte velikost datového disku v nastavení virtuálního počítače Azure, musíte také přidělit nový diskový prostor v rámci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5d51b-105">After you increase the size of the data disk in the Azure VM settings, you also need to allocate the new disk space within the VM.</span></span>


## <a name="use-powershell-to-increase-the-size-of-a-managed-data-disk"></a><span data-ttu-id="5d51b-106">Zvětšete velikost spravované datový disk pomocí prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="5d51b-106">Use Powershell to increase the size of a managed data disk</span></span>

<span data-ttu-id="5d51b-107">Chcete-li zvýšit velikost spravované datový disk, použijte následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5d51b-107">To increase the size of a managed data disk, use the following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="5d51b-108">Get-AzureRMReseourceGroup</span><span class="sxs-lookup"><span data-stu-id="5d51b-108">Get-AzureRMReseourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [<span data-ttu-id="5d51b-109">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-109">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="5d51b-110">Stop-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-110">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                        | [<span data-ttu-id="5d51b-111">Aktualizace AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="5d51b-111">Update-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [<span data-ttu-id="5d51b-112">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-112">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

<span data-ttu-id="5d51b-113">Následující skript provede vás získávání informací o virtuálních počítačů, výběrem datový disk a určení nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="5d51b-113">The following script will walk you through getting the VM information, selecting the data disk and specifying the new size.</span></span>

```powershell
# Select resource group

    $rg = Get-AzureRMResourceGroup | Out-GridView `
        -Title "Select the resource group" `
        -PassThru

    $rgName = $rg.ResourceGroupName

# Select the VM

    $vm = Get-AzureRMVM -ResourceGroupName $rgName `
        | Out-GridView `
            -Title "Select a VM" `
             -PassThru

# Select data disk

    $disk = $vm.StorageProfile.DataDisks | Out-GridView `
        -Title "Select a data disk" `
        -PassThru

# Specify a larger size for the data disk

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior to resizing data disk

    $vm | Stop-AzureRMVM -Force

# Set the new disk size

    $diskUpdateConfig = New-AzureRmDiskUpdateConfig -DiskSizeGB $size

# Update the configuration in Azure

    $managedDisk = Get-AzureRmResource -ResourceId $disk.ManagedDisk.Id
    Update-AzureRmDisk -DiskName $managedDisk.ResourceName -ResourceGroupName $managedDisk.ResourceGroupName -DiskUpdate $diskUpdateConfig

# Start the VM

    Start-AzureRmVM -ResourceGroupName $rgName -VMName $vm.name
```

## <a name="use-powershell-to-increase-the-size-of-an-unmanaged-data-disk"></a><span data-ttu-id="5d51b-114">Rozšiřte disk pro nespravovaná data pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5d51b-114">Use PowerShell to increase the size of an unmanaged data disk</span></span>

<span data-ttu-id="5d51b-115">Chcete-li zvýšit velikost nespravované datové disky v účtu úložiště, použijte následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="5d51b-115">To increase the size of unmanaged data disks in a storage account, use the following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="5d51b-116">Get AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="5d51b-116">Get-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [<span data-ttu-id="5d51b-117">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-117">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="5d51b-118">Stop-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-118">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                       | [<span data-ttu-id="5d51b-119">Set-AzureRmVMDataDisk</span><span class="sxs-lookup"><span data-stu-id="5d51b-119">Set-AzureRmVMDataDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [<span data-ttu-id="5d51b-120">Update-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-120">Update-AzureRmVM</span></span>](/powershell/module/azurerm.compute/update-azurermvm)                   | [<span data-ttu-id="5d51b-121">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="5d51b-121">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

<span data-ttu-id="5d51b-122">Následující skript vás provede získávání virtuálních počítačů a úložiště informací o účtu, vyberte datový disk a určení nové velikosti.</span><span class="sxs-lookup"><span data-stu-id="5d51b-122">The following script will walk you through getting the VM and storage account information, selecting the data disk and specifying the new size.</span></span>

```powershell

# Select Azure Storage Account

    $storageAccount =
        Get-AzureRMStorageAccount | Out-GridView `
            -Title "Select Azure Storage Account" `
            -PassThru

    $rgName = $storageAccount.ResourceGroupName

# Select the VM

    $vm = Get-AzureRMVM `
    -ResourceGroupName $rgName | Out-GridView `
            -Title "Select a VM …" `
            -PassThru

# Select Data Disk to resize

    $disk =
        $vm.DataDiskNames | Out-GridView `
            -Title "Select a data disk to resize" `
            -PassThru


# Specify a larger data disk size

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior to resizing data disk

    $vm | Stop-AzureRMVM -Force

# Set the new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" `
        -DiskSizeInGB $size

# Update the configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start the VM
    Start-AzureRmVM -ResourceGroupName $rgName `
    -VMName $vm.name

```

## <a name="allocate-the-unallocated-disk-space"></a><span data-ttu-id="5d51b-123">Přidělit volné místo na disku</span><span class="sxs-lookup"><span data-stu-id="5d51b-123">Allocate the unallocated disk space</span></span>

<span data-ttu-id="5d51b-124">Po provedení jednotku větší, musíte přidělit nový nepřidělené místo z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5d51b-124">Once you have made the drive larger, you need to allocate the new unallocated space from within the VM.</span></span> <span data-ttu-id="5d51b-125">Přidělit prostor, můžete připojit k používání virtuálních počítačů správu disků (diskmgmt.msc).</span><span class="sxs-lookup"><span data-stu-id="5d51b-125">To allocate the space, you can connect to the VM use Disk Management (diskmgmt.msc).</span></span> <span data-ttu-id="5d51b-126">Nebo, pokud jste povolili WinRM a certifikát ve virtuálním počítači, když jste ho vytvářeli, vzdáleného prostředí PowerShell můžete inicializovat disk.</span><span class="sxs-lookup"><span data-stu-id="5d51b-126">Or, if you enabled WinRM and a certificate on the VM when you created it, you can use remote PowerShell to initialize the disk.</span></span> <span data-ttu-id="5d51b-127">Můžete také použít rozšíření vlastních skriptů:</span><span class="sxs-lookup"><span data-stu-id="5d51b-127">You can also use a custom script extension:</span></span>

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

<span data-ttu-id="5d51b-128">Soubor skriptu něco podobného jako tento kód a zvyšte tak přidělení disku pro maximální velikost může obsahovat disky:</span><span class="sxs-lookup"><span data-stu-id="5d51b-128">The script file can contain something like this code to increase the drive allocation to the maximum size the disks:</span></span>

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a><span data-ttu-id="5d51b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5d51b-129">Next Steps</span></span>
- [<span data-ttu-id="5d51b-130">Další informace o disky a virtuální pevné disky</span><span class="sxs-lookup"><span data-stu-id="5d51b-130">Learn more about disks and VHDs</span></span>](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
