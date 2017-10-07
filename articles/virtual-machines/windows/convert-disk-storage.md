---
title: "aaaConvert Azure spravované disky úložiště ze standardní toopremium a naopak | Microsoft Docs"
description: "Jak tooconvert Azure spravují disky z standardní toopremium a naopak a pomocí prostředí Azure PowerShell."
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
ms.openlocfilehash: 11f35cde216e91c0599d3619682686e8eb162fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="77cad-103">Převést Azure spravované disky úložiště ze standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="77cad-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="77cad-104">Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD).</span><span class="sxs-lookup"><span data-stu-id="77cad-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="77cad-105">Umožňuje vám tooeasily přepínat mezi hello dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="77cad-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="77cad-106">Tato funkce není k dispozici pro nespravovaná disky.</span><span class="sxs-lookup"><span data-stu-id="77cad-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="77cad-107">Ale můžete snadno [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) tooeasily přepínat mezi hello dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="77cad-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="77cad-108">Tento článek ukazuje, jak spravovat tooconvert disky z standardní toopremium a naopak pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="77cad-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure PowerShell.</span></span> <span data-ttu-id="77cad-109">Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="77cad-109">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="77cad-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="77cad-110">Before you begin</span></span>

* <span data-ttu-id="77cad-111">Hello převod vyžaduje restartování hello virtuálních počítačů, takže naplánovat hello migrace úložiště disků existující období údržby.</span><span class="sxs-lookup"><span data-stu-id="77cad-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="77cad-112">Pokud používáte nespravované disky, nejprve [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) toouse Tento článek tooswitch mezi hello dvě možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="77cad-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="77cad-113">Převést všechny hello spravované disky virtuálního počítače z standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="77cad-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="77cad-114">V následujícím příkladu hello, ukážeme, jak tooswitch všechny disky virtuálního počítače z úložiště standard toopremium hello.</span><span class="sxs-lookup"><span data-stu-id="77cad-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="77cad-115">toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="77cad-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="77cad-116">Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="77cad-116">This example also switches tooa size that supports premium storage.</span></span>

```powershell
# Name of hello resource group that contains hello VM
$rgName = 'yourResourceGroup'

# Name of hello your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard toopremium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate hello VM before changing hello size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in hello resource group of hello VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong toohello selected VM, convert toopremium storage
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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="77cad-117">Převést se spravovaným diskem z standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="77cad-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="77cad-118">Pro vývoj/testování úlohy můžete toohave směs standard a premium disky tooreduce vaše náklady.</span><span class="sxs-lookup"><span data-stu-id="77cad-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="77cad-119">Můžete ji provést upgradem toopremium úložiště pouze hello disky, které vyžadují vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="77cad-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="77cad-120">V následujícím příkladu hello, ukážeme, jak tooswitch jediný disk virtuálního počítače z úložiště standard toopremium a naopak.</span><span class="sxs-lookup"><span data-stu-id="77cad-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="77cad-121">toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="77cad-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="77cad-122">Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="77cad-122">This example also switches tooa size that supports premium storage.</span></span>

```powershell

$diskName = 'yourDiskName'
# resource group that contains hello managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get hello ARM resource tooget name and resource group of hello VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate hello VM before changing hello storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update hello storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a><span data-ttu-id="77cad-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77cad-123">Next steps</span></span>

<span data-ttu-id="77cad-124">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="77cad-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

