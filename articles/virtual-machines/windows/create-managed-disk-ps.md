---
title: "aaaCreate se spravovaným diskem z disku VHD v Azure | Microsoft Docs"
description: "Vytvoření spravovaného disku z disku VHD, který je aktuálně v účtu úložiště Azure pomocí modelu nasazení Resource Manager hello."
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
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Vytvoření spravované disky z nespravovaných disků v účtu úložiště

Spravovaný disk lze vytvořit ze stávající datový disk nebo disk s operačním systémem, který je aktuálně v účtu úložiště Azure. Můžete také vytvořit prázdný disk, který slouží jako nový datový disk pro virtuální počítač. 

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Vytvoření spravovaného disku z disku VHD v účtu úložiště Azure

V příkladu hello jsme vytvoření disku z virtuálního pevného disku jako spravovaných disků a přiřaďte ho toohello parametr **$ disk 1** toouse později. 

Hello spravovaného disku budou vytvořeny v hello **USA – západ** umístění, ve skupině prostředků s názvem **myResourceGroup**. bude mít název disku Hello **myDisk** a vytvoří se z virtuálního pevného disku soubor s názvem **myDisk.vhd** jsme předtím nahrála tooa účet úložiště s názvem **můj_účet_úložiště**. místně redundantní úložiště (LRS) úrovně premium vytvoří spravovaných disků na Hello. StandardLRS a PremiumLRS jsou pouze hello **- AccountType** možnosti jsou dostupné pro spravované disky. 

1.  Některé parametry nastavit.

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Vytvořte hello datový disk. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Vytvoření prázdný datový disk jako spravované disk

V příkladu hello jsme vytvořte prázdný datový disk jako spravovaných disků a přiřaďte ho toohello parametr **$dataDisk2** toouse později. Prázdný datový disk bude nutné toobe inicializovat protokolování v toohello virtuálních počítačů a pomocí diskmgmt.msc nebo [vzdáleně přes WinRM a skript](attach-disk-ps.md#initialize-the-disk), až bude připojený tooa spuštění virtuálního počítače.

Hello prázdný datový disk se vytvoří hello **– Západ střední USA** umístění, ve skupině prostředků s názvem **myResourceGroup**. bude mít název disku Hello **myEmptyDataDisk**. prázdný disk Hello budou vytvořeny v premium místně redundantní úložiště (LRS). StandardLRS a PremiumLRS jsou pouze hello **- AccountType** možnosti jsou dostupné pro spravované disky.

velikost disku Hello v tomto příkladu je 128GB, ale měli byste vybrat velikost, která vyhovuje potřebám hello všech aplikací běžících na vašem virtuálním počítači.

1.  Některé parametry nastavit.

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Vytvořte hello datový disk.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Další kroky   
- Pokud již máte virtuální počítač, můžete [připojit datový disk](attach-disk-portal.md).
