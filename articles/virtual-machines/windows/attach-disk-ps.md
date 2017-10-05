---
title: "Připojit datový disk pro virtuální počítač s Windows v Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Jak připojit nové nebo stávající datový disk k virtuálnímu počítači Windows PowerShell pomocí modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: cynthn
ms.openlocfilehash: 486e6a27fa28ec63001d824fe9f59c03a7aea5a7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a><span data-ttu-id="db63e-103">Připojit datový disk pro virtuální počítač s Windows pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="db63e-103">Attach a data disk to a Windows VM using PowerShell</span></span>

<span data-ttu-id="db63e-104">Tento článek ukazuje, jak přiřadit nové i stávající disky virtuálního počítače s Windows pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db63e-104">This article shows you how to attach both new and existing disks to a Windows virtual machine using PowerShell.</span></span> <span data-ttu-id="db63e-105">Pokud virtuální počítač používá spravované disky, můžete provést připojení dalších spravovaných datových disků.</span><span class="sxs-lookup"><span data-stu-id="db63e-105">If your VM uses managed disks, you can attach additional managed data disks.</span></span> <span data-ttu-id="db63e-106">Nespravované datových disků můžete také připojit k virtuálnímu počítači, který používá nespravované disky v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="db63e-106">You can also attach unmanaged data disks to a VM that uses unmanaged disks in a storage account.</span></span>

<span data-ttu-id="db63e-107">Než to uděláte, projděte si tyto tipy:</span><span class="sxs-lookup"><span data-stu-id="db63e-107">Before you do this, review these tips:</span></span>
* <span data-ttu-id="db63e-108">Velikost virtuálního počítače určuje, kolik datových disků můžete připojit.</span><span class="sxs-lookup"><span data-stu-id="db63e-108">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="db63e-109">Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="db63e-109">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="db63e-110">Pokud chcete používat úložiště úrovně Premium, budete potřebovat Storage úrovně Premium povolená velikost virtuálního počítače jako virtuální počítač DS-series nebo GS-series.</span><span class="sxs-lookup"><span data-stu-id="db63e-110">To use Premium storage, you'll need a Premium Storage enabled VM size like the DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="db63e-111">Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="db63e-111">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="db63e-112">Storage úrovně Premium je k dispozici v určité oblasti.</span><span class="sxs-lookup"><span data-stu-id="db63e-112">Premium storage is available in certain regions.</span></span> <span data-ttu-id="db63e-113">Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="db63e-113">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="db63e-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="db63e-114">Before you begin</span></span>
<span data-ttu-id="db63e-115">Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db63e-115">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="db63e-116">Spusťte následující příkaz k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="db63e-116">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="db63e-117">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="db63e-117">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a><span data-ttu-id="db63e-118">Přidat prázdný datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="db63e-118">Add an empty data disk to a virtual machine</span></span>

<span data-ttu-id="db63e-119">Tento příklad ukazuje, jak přidat prázdný datový disk do existujícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="db63e-119">This example shows how to add an empty data disk to an existing virtual machine.</span></span>

### <a name="using-managed-disks"></a><span data-ttu-id="db63e-120">Pomocí spravovaného disků</span><span class="sxs-lookup"><span data-stu-id="db63e-120">Using managed disks</span></span>

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128

$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="db63e-121">Použití nespravovaného disky v účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="db63e-121">Using unmanaged disks in a storage account</span></span>

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-the-disk"></a><span data-ttu-id="db63e-122">Inicializujte disk</span><span class="sxs-lookup"><span data-stu-id="db63e-122">Initialize the disk</span></span>

<span data-ttu-id="db63e-123">Po přidání prázdný disk, budete muset provést jeho inicializaci.</span><span class="sxs-lookup"><span data-stu-id="db63e-123">After you add an empty disk, you need to initialize it.</span></span> <span data-ttu-id="db63e-124">K chybě při inicializaci disku, můžete přihlásit k virtuálnímu počítači a použít program Správa disku.</span><span class="sxs-lookup"><span data-stu-id="db63e-124">To initialize the disk, you can log in to a VM and use disk management.</span></span> <span data-ttu-id="db63e-125">Pokud jste povolili WinRM a certifikát ve virtuálním počítači, když jste ho vytvářeli, můžete inicializovat disk vzdáleného prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db63e-125">If you enabled WinRM and a certificate on the VM when you created it, you can use remote PowerShell to initialize the disk.</span></span> <span data-ttu-id="db63e-126">Můžete také použít rozšíření vlastních skriptů:</span><span class="sxs-lookup"><span data-stu-id="db63e-126">You can also use a custom script extension:</span></span> 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
<span data-ttu-id="db63e-127">Soubor skriptu může obsahovat něco podobného jako tento kód pro inicializaci disky:</span><span class="sxs-lookup"><span data-stu-id="db63e-127">The script file can contain something like this code to initialize the disks:</span></span>

```powershell
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a><span data-ttu-id="db63e-128">Připojit stávající datový disk k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="db63e-128">Attach an existing data disk to a VM</span></span>

<span data-ttu-id="db63e-129">Můžete také připojit existující virtuální pevný disk jako spravované datový disk k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="db63e-129">You can also attach an existing VHD as a managed data disk to a virtual machine.</span></span> 

### <a name="using-managed-disks"></a><span data-ttu-id="db63e-130">Pomocí spravovaného disků</span><span class="sxs-lookup"><span data-stu-id="db63e-130">Using managed disks</span></span>

```powershell
$rgName = 'myRG'
$vmName = 'ContosoMdPir3'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk2'
$dataVhdUri = 'https://mystorageaccount.blob.core.windows.net/vhds/managed_data_disk.vhd' 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $dataVhdUri -DiskSizeGB 128

$dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk2.Id -Lun 2

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a><span data-ttu-id="db63e-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db63e-131">Next steps</span></span>

<span data-ttu-id="db63e-132">Vytvoření [snímku](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="db63e-132">Create a [snapshot](snapshot-copy-managed-disk.md).</span></span>
