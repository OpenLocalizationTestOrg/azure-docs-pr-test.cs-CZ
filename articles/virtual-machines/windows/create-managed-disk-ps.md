---
title: "Vytvoření spravovaného disku z virtuálního pevného disku v Azure | Microsoft Docs"
description: "Vytvoření spravovaného disku z disku VHD, který je aktuálně v účtu úložiště Azure pomocí modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Vytvoření spravované disky z nespravovaných disků v účtu úložiště

Spravovaný disk lze vytvořit ze stávající datový disk nebo disk s operačním systémem, který je aktuálně v účtu úložiště Azure. Můžete také vytvořit prázdný disk, který slouží jako nový datový disk pro virtuální počítač. 

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell. Spusťte následující příkaz k její instalaci.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Vytvoření spravovaného disku z disku VHD v účtu úložiště Azure

V příkladu jsme vytvoření disku z virtuálního pevného disku jako spravovaných disků a přiřaďte ho do parametru **disk 1 $** pro pozdější použití. 

Se vytvoří spravovaných disků **USA – západ** umístění, ve skupině prostředků s názvem **myResourceGroup**. Na disku budou pojmenované **myDisk** a vytvoří se z virtuálního pevného disku soubor s názvem **myDisk.vhd** jsme předtím nahrála do účtu úložiště s názvem **můj_účet_úložiště**. Spravovaný disk bude vytvořen v premium místně redundantní úložiště (LRS). StandardLRS a PremiumLRS jsou jediné **- AccountType** možnosti jsou dostupné pro spravované disky. 

1.  Některé parametry nastavit.

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Vytvořte datový disk. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Vytvoření prázdný datový disk jako spravované disk

V příkladu jsme prázdný datový disk jako spravovaného disku vytvořte a přiřaďte ho do parametru **$dataDisk2** pro pozdější použití. Prázdný datový disk bude muset být inicializovaného přihlášení k virtuálnímu počítači a pomocí diskmgmt.msc nebo [vzdáleně přes WinRM a skript](attach-disk-ps.md#initialize-the-disk), jakmile je připojen k spuštění virtuálního počítače.

Prázdný datový disk se vytvoří v **– Západ střední USA** umístění, ve skupině prostředků s názvem **myResourceGroup**. Na disku budou pojmenované **myEmptyDataDisk**. Prázdný disk bude vytvořen v premium místně redundantní úložiště (LRS). StandardLRS a PremiumLRS jsou jediné **- AccountType** možnosti jsou dostupné pro spravované disky.

Velikost disku v tomto příkladu je 128GB, ale měli byste vybrat velikost, která vyhovuje potřebám všechny aplikace běžící na vašem virtuálním počítači.

1.  Některé parametry nastavit.

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Vytvořte datový disk.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Další kroky   
- Pokud již máte virtuální počítač, můžete [připojit datový disk](attach-disk-portal.md).
