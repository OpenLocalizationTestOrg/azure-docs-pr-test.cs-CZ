---
title: "Správa Azure disky pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – spravovat Azure disky pomocí prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 6f1bc9361745adc211f22416a7ba8ac1b8dc614e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-disks-with-powershell"></a>Správa Azure disky pomocí prostředí PowerShell

Virtuální počítače Azure pomocí disků ukládat virtuální počítače operačního systému, aplikace a data. Při vytváření virtuálního počítače je důležité, abyste zvolili velikost disku a konfigurace, které jsou vhodné pro očekávané úlohy. Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů. Informace o:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="default-azure-disks"></a>Výchozí disky systému Azure

Vytvoření virtuálního počítače Azure má dva disky jsou automaticky připojena k virtuálnímu počítači. 

**Disk s operačním systémem** -provoz systémových disků může mít velikost až 1 terabajt a hostuje virtuální počítače operačního systému.  Disk operačního systému je přiřazeno písmeno jednotky *c:* ve výchozím nastavení. Disk ukládání do mezipaměti konfigurace disku operačního systému je optimalizovaná pro výkon operačního systému. Disk operačního systému **neměli** hostitelem aplikace nebo data. Pro aplikace a data použijte datový disk, který je podrobně popsán později v tomto článku.

**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na stejném hostiteli Azure jako virtuální počítač. Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování. Pokud se virtuální počítač přesune do nového hostitele, je odebrat všechna data uložená na dočasném disku. Velikost dočasné disku je určen podle velikosti virtuálního počítače. Dočasné disky přiřazené písmeno jednotky *d:* ve výchozím nastavení.

### <a name="temporary-disk-sizes"></a>Velikosti dočasné disků

| Typ | Velikost virtuálního počítače | Maximální velikost dočasného disk (GB) |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 800 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 800 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 6144 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 5630 |
| [GPU](sizes-gpu.md) | N řady | 1440 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 2000 |

## <a name="azure-data-disks"></a>Azure datových disků

Pro instalaci aplikací a ukládání dat přidáním dalších datových disků. Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště. Každý datový disk má maximální kapacita 1 terabajt. Velikost virtuálního počítače určuje, kolik datových disků lze připojit k virtuálnímu počítači. Pro každý základní virtuální počítač se dá připojit dvěma datovými disky. 

### <a name="max-data-disks-per-vm"></a>Maximální počet datových disků na virtuální počítač

| Typ | Velikost virtuálního počítače | Maximální počet datových disků na virtuální počítač |
|----|----|----|
| [Obecné účely](sizes-general.md) | A a řady D | 32 |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md) | Řady F | 32 |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md) | Série D a G | 64 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md) | Řada L | 64 |
| [GPU](sizes-gpu.md) | N řady | 48 |
| [Vysoký výkon](sizes-hpc.md) | A a řady H | 32 |

## <a name="vm-disk-types"></a>Typy disků virtuálních počítačů

Azure nabízí dva typy disku.

### <a name="standard-disk"></a>Disků na úrovni Standard

Služba Storage úrovně Standard je založená na jednotkách HDD a poskytuje nákladově efektivní úložiště se zachováním výkonu. Standardní disky jsou ideální pro finančně efektivní vývoj a testování pracovního vytížení.

### <a name="premium-disk"></a>Premium disku

Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku. Ideální pro virtuální počítače se systémem produkční zatížení. Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series. Pro prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost disku určuje typ disku. Když vyberete, je velikost disku hodnota zaokrouhlený nahoru na další typ. Například pokud je velikost je menší než 128 GB typ disku bude P10, mezi 129 a 512 P20 a více než 512 P30. 

### <a name="premium-disk-performance"></a>Výkon disku Premium

|Typ disku úložiště Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Velikost disku (round nahoru) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Vstupně-výstupní operace za sekundu / disk | 500 | 2,300 | 5,000 |
Propustnost / disk | 100 MB/s | 150 MB/s | 200 MB/s |

Při výše uvedené tabulce jsou uvedeny maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků. Například 64 datových disků se dá připojit k virtuálnímu počítači Standard_GS5. Pokud každý z těchto disků jsou dimenzované jako P30, se dá dosáhnout maximálně 80 000 IOPS. Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](./sizes.md).

## <a name="create-and-attach-disks"></a>Vytvořte a připojte disky

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

Vytvoření počáteční konfigurace s [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). Následující příklad konfiguruje disk, který je 128 GB velikost.

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Vytvořit datový disk se [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) příkaz.

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Získat virtuální počítač, který chcete přidat datový disk do s [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Přidat datový disk pro konfiguraci virtuálního počítače s [přidat AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Aktualizovat virtuální počítač s [aktualizace-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Příprava datových disků

Jakmile byla přiřazena disk k virtuálnímu počítači, musí se nakonfigurovat na použití disku operačního systému. Následující příklad ukazuje, jak ručně nakonfigurovat první disk přidat do virtuálního počítače. Tento proces je také možné automatizovat pomocí [rozšíření vlastních skriptů](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Ruční konfigurace

Vytvořte připojení ke vzdálené ploše virtuálního počítače. Otevřete prostředí PowerShell a spusťte tento skript.

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli o tématech disky virtuálních počítačů, jako:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků

Přechodu na další informace o automatizaci konfigurace virtuálního počítače v dalším kurzu.

> [!div class="nextstepaction"]
> [Automatizace konfigurace virtuálních počítačů](./tutorial-automate-vm-deployment.md)
