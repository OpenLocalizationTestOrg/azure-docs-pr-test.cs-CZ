---
title: "aaaManage Azure disky s hello prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – Správa Azure disků s hello prostředí Azure PowerShell"
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
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a>Správa Azure disky pomocí prostředí PowerShell

Virtuální počítače Azure pomocí disků toostore hello virtuální počítače operačního systému, aplikace a data. Při vytváření virtuálního počítače je důležité toochoose velikost disku a příslušné toohello očekává úlohy konfigurace. Tento kurz se zaměřuje na nasazení a správě disky virtuálních počítačů. Informace o:

> [!div class="checklist"]
> * Operační systém a dočasný disky
> * Datové disky
> * Standard a Premium disky
> * Výkon disku
> * Připojení a příprava datových disků

Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější. Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze. Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="default-azure-disks"></a>Výchozí disky systému Azure

Když je vytvořen virtuální počítač Azure, jsou dva disky automaticky připojené toohello virtuálního počítače. 

**Disk s operačním systémem** – disků operačního systému může mít velikost až too1 terabajt a hello hostitelů virtuálních počítačů operačního systému.  disk Hello operačního systému je přiřazeno písmeno jednotky *c:* ve výchozím nastavení. ukládání do mezipaměti konfigurace disku operačního systému hello disku Hello je optimalizovaná pro výkon operačního systému. Hello operačního systému disku **neměli** hostitelem aplikace nebo data. Pro aplikace a data použijte datový disk, který je podrobně popsán později v tomto článku.

**Dočasným diskovým** -dočasné disky používají jednotkou SSD, který je umístěný na hello stejného Azure hostitele jako hello virtuálních počítačů. Dočasné disky jsou vysoce původce a mohou být použity pro operací, jako je dočasná data zpracování. Pokud hello virtuální počítač přesunutý tooa nového hostitele, je odebrat všechna data uložená na dočasném disku. Hello velikost dočasné disku hello je určen podle hello velikost virtuálního počítače. Dočasné disky přiřazené písmeno jednotky *d:* ve výchozím nastavení.

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

Pro instalaci aplikací a ukládání dat přidáním dalších datových disků. Datové disky by měl použít v každé situaci, kde je žádoucí, odolné a dobře reagovaly datové úložiště. Každý datový disk má maximální kapacita 1 terabajt. velikost Hello hello virtuálního počítače určuje, kolik datových disků může být připojené tooa virtuálních počítačů. Pro každý základní virtuální počítač se dá připojit dvěma datovými disky. 

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

Pro prémiové disky jsou zajišťované založená na SSD vysoce výkonné, nízkou latencí disku. Ideální pro virtuální počítače se systémem produkční zatížení. Premium Storage podporuje DS-series, DSv2-series, GS-series a virtuálních počítačů služby FS-series. Prémiové disky se musí uvést ve třech typech (P10 P20, P30), velikost hello hello disku určuje typ disku hello. Když vyberete, hodnota hello velikosti disku zaokrouhlit toohello další typ. Například pokud hello velikost je menší než 128 GB typ disku hello bude P10, mezi 129 a 512 P20 a více než 512 P30. 

### <a name="premium-disk-performance"></a>Výkon disku Premium

|Typ disku úložiště Premium | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Velikost disku (round nahoru) | 128 GB | 512 GB | 1024 GB (1 TB) |
| Vstupně-výstupní operace za sekundu / disk | 500 | 2,300 | 5,000 |
Propustnost / disk | 100 MB/s | 150 MB/s | 200 MB/s |

Při hello výše tabulky identifikuje maximální IOPS na disku, vyšší úroveň výkonu můžete dosáhnout prokládání více datových disků. Například 64 dat, který může být disky připojené tooStandard_GS5 virtuálních počítačů. Pokud každý z těchto disků jsou dimenzované jako P30, se dá dosáhnout maximálně 80 000 IOPS. Podrobné informace o maximální IOPS na virtuálních počítačů najdete v tématu [velikosti virtuálního počítače s Linuxem](./sizes.md).

## <a name="create-and-attach-disks"></a>Vytvořte a připojte disky

Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.

Vytvoření hello počáteční konfigurace s [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). Následující ukázka Hello nakonfiguruje disk, který je 128 GB velikost.

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

Vytvoření hello datový disk s hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) příkaz.

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

Get hello virtuálního počítače, které chcete tooadd hello datového disku toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) příkaz.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Přidat hello data toohello virtuální počítač konfigurace disku s hello [přidat AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

Aktualizovat hello virtuální počítač s hello [aktualizace-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) příkaz.

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a>Příprava datových disků

Jakmile disk virtuálního počítače připojené toohello, už hello operačního systému musí toobe nakonfigurované toouse hello disku. Hello následující příklad ukazuje, jak nakonfigurovat toomanually hello první disk přidat toohello virtuálních počítačů. Tento proces je také možné automatizovat pomocí hello [rozšíření vlastních skriptů](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>Ruční konfigurace

Vytvořte připojení ke vzdálené ploše s hello virtuálního počítače. Otevřete prostředí PowerShell a spusťte tento skript.

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

Posunutí další kurz toolearn toohello o automatizaci konfigurace virtuálního počítače.

> [!div class="nextstepaction"]
> [Automatizace konfigurace virtuálních počítačů](./tutorial-automate-vm-deployment.md)
