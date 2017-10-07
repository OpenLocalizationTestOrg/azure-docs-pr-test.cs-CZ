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
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Převést Azure spravované disky úložiště ze standardní toopremium a naopak

Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD). Umožňuje vám tooeasily přepínat mezi hello dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon. Tato funkce není k dispozici pro nespravovaná disky. Ale můžete snadno [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) tooeasily přepínat mezi hello dvě možnosti.

Tento článek ukazuje, jak spravovat tooconvert disky z standardní toopremium a naopak pomocí prostředí Azure PowerShell. Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Než začnete

* Hello převod vyžaduje restartování hello virtuálních počítačů, takže naplánovat hello migrace úložiště disků existující období údržby. 
* Pokud používáte nespravované disky, nejprve [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) toouse Tento článek tooswitch mezi hello dvě možnosti úložiště. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Převést všechny hello spravované disky virtuálního počítače z standardní toopremium a naopak

V následujícím příkladu hello, ukážeme, jak tooswitch všechny disky virtuálního počítače z úložiště standard toopremium hello. toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage. Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.

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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Převést se spravovaným diskem z standardní toopremium a naopak

Pro vývoj/testování úlohy můžete toohave směs standard a premium disky tooreduce vaše náklady. Můžete ji provést upgradem toopremium úložiště pouze hello disky, které vyžadují vyšší výkon. V následujícím příkladu hello, ukážeme, jak tooswitch jediný disk virtuálního počítače z úložiště standard toopremium a naopak. toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage. Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.

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

## <a name="next-steps"></a>Další kroky

Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).

