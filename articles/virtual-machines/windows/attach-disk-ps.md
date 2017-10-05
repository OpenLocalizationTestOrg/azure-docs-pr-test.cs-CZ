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
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Připojit datový disk pro virtuální počítač s Windows pomocí prostředí PowerShell

Tento článek ukazuje, jak přiřadit nové i stávající disky virtuálního počítače s Windows pomocí prostředí PowerShell. Pokud virtuální počítač používá spravované disky, můžete provést připojení dalších spravovaných datových disků. Nespravované datových disků můžete také připojit k virtuálnímu počítači, který používá nespravované disky v účtu úložiště.

Než to uděláte, projděte si tyto tipy:
* Velikost virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Pokud chcete používat úložiště úrovně Premium, budete potřebovat Storage úrovně Premium povolená velikost virtuálního počítače jako virtuální počítač DS-series nebo GS-series. Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell. Spusťte následující příkaz k její instalaci.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Přidat prázdný datový disk k virtuálnímu počítači

Tento příklad ukazuje, jak přidat prázdný datový disk do existujícího virtuálního počítače.

### <a name="using-managed-disks"></a>Pomocí spravovaného disků

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

### <a name="using-unmanaged-disks-in-a-storage-account"></a>Použití nespravovaného disky v účtu úložiště

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-the-disk"></a>Inicializujte disk

Po přidání prázdný disk, budete muset provést jeho inicializaci. K chybě při inicializaci disku, můžete přihlásit k virtuálnímu počítači a použít program Správa disku. Pokud jste povolili WinRM a certifikát ve virtuálním počítači, když jste ho vytvářeli, můžete inicializovat disk vzdáleného prostředí PowerShell. Můžete také použít rozšíření vlastních skriptů: 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Soubor skriptu může obsahovat něco podobného jako tento kód pro inicializaci disky:

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


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Připojit stávající datový disk k virtuálnímu počítači

Můžete také připojit existující virtuální pevný disk jako spravované datový disk k virtuálnímu počítači. 

### <a name="using-managed-disks"></a>Pomocí spravovaného disků

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

## <a name="next-steps"></a>Další kroky

Vytvoření [snímku](snapshot-copy-managed-disk.md).
