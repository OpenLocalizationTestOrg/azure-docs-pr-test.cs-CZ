---
title: "Převést Azure spravované disky úložiště ze standardní, Premium a naopak | Microsoft Docs"
description: "Postup převedení Azure spravovat disky z standardní, Premium a naopak a pomocí prostředí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 9e5c73ceb0ff7d9c18c9cf7128b69e40b9796874
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="3b5ca-103">Převést Azure spravované disky úložiště ze standardní, Premium a naopak</span><span class="sxs-lookup"><span data-stu-id="3b5ca-103">Convert Azure managed disks storage from standard to premium, and vice versa</span></span>

<span data-ttu-id="3b5ca-104">Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD).</span><span class="sxs-lookup"><span data-stu-id="3b5ca-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="3b5ca-105">Umožňuje snadno přepínat mezi dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-105">It allows you to easily switch between the two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="3b5ca-106">Tato funkce není k dispozici pro nespravovaná disky.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="3b5ca-107">Ale můžete snadno [převést na spravované disky](convert-unmanaged-to-managed-disks.md) snadno přepínat mezi dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-107">But you can easily [convert to managed disks](convert-unmanaged-to-managed-disks.md) to easily switch between the two options.</span></span>

<span data-ttu-id="3b5ca-108">Tento článek ukazuje, jak převést spravované disky z standardní, Premium a naopak pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-108">This article shows you how to convert managed disks from standard to premium, and vice versa by using Azure PowerShell.</span></span> <span data-ttu-id="3b5ca-109">Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3b5ca-109">If you need to install or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3b5ca-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3b5ca-110">Before you begin</span></span>

* <span data-ttu-id="3b5ca-111">Převod vyžaduje restartování virtuálního počítače, takže naplánovat migraci úložiště disků existující období údržby.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-111">The conversion requires a restart of the VM, so schedule the migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="3b5ca-112">Pokud používáte nespravované disky, nejprve [převést na spravované disky](convert-unmanaged-to-managed-disks.md) používat v tomto článku přepínat mezi dvě možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-112">If you are using unmanaged disks, first [convert to managed disks](convert-unmanaged-to-managed-disks.md) to use this article to switch between the two storage options.</span></span> 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="3b5ca-113">Převeďte všechny spravované disky virtuálního počítače ze standardní premium a naopak</span><span class="sxs-lookup"><span data-stu-id="3b5ca-113">Convert all the managed disks of a VM from standard to premium, and vice versa</span></span>

<span data-ttu-id="3b5ca-114">V následujícím příkladu ukážeme, jak přepínat všechny disky virtuálního počítače z standard do úložiště úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-114">In the following example, we show how to switch all the disks of a VM from standard to premium storage.</span></span> <span data-ttu-id="3b5ca-115">Spravované prémiové disky, virtuální počítač vyžaduje použití [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-115">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="3b5ca-116">Tento příklad také přepne na velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-116">This example also switches to a size that supports premium storage.</span></span>

```powershell
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate the VM before changing the size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="3b5ca-117">Převést standardní spravovaných disků na premium a naopak</span><span class="sxs-lookup"><span data-stu-id="3b5ca-117">Convert a managed disk from standard to premium, and vice versa</span></span>

<span data-ttu-id="3b5ca-118">Pro vývoj/testování úlohy můžete mít směs standard a premium disky na snížení nákladů na vaše.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-118">For your dev/test workload, you may want to have mixture of standard and premium disks to reduce your cost.</span></span> <span data-ttu-id="3b5ca-119">Můžete ji provést upgradem do úložiště úrovně premium, pouze disky, které vyžadují vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-119">You can accomplish it by upgrading to premium storage, only the disks that require better performance.</span></span> <span data-ttu-id="3b5ca-120">V následujícím příkladu ukážeme, jak přepínat jediný disk virtuálního počítače z standard do úložiště úrovně premium a naopak.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-120">In the following example, we show how to switch a single disk of a VM from standard to premium storage, and vice versa.</span></span> <span data-ttu-id="3b5ca-121">Spravované prémiové disky, virtuální počítač vyžaduje použití [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-121">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="3b5ca-122">Tento příklad také přepne na velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="3b5ca-122">This example also switches to a size that supports premium storage.</span></span>

```powershell

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get the ARM resource to get name and resource group of the VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a><span data-ttu-id="3b5ca-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b5ca-123">Next steps</span></span>

<span data-ttu-id="3b5ca-124">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="3b5ca-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

