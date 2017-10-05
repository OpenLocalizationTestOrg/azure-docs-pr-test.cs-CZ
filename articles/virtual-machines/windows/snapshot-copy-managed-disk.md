---
title: "Vytvořit kopii diskem spravované Azure pro back up | Microsoft Docs"
description: "Naučte se vytvořit kopii Azure spravované disku pro back up nebo řešení potíží s disku."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: a7527b12f4f0d2b45713a0c0109d81ff51293fd8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky
Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku a jeho připojení k testovací virtuální počítač k řešení. Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů. Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk. Další informace o discích spravovaných najdete v tématu [disky spravované Azure – přehled](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi modulu prostředí AzureRM.Compute PowerShell. Spusťte následující příkaz k její instalaci.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).

## <a name="copy-the-vhd-with-a-snapshot"></a>Zkopírujte virtuální pevný disk s snímku
K pořízení snímku na Disk spravovaný pomocí portálu Azure nebo prostředí PowerShell.

### <a name="use-azure-portal-to-take-a-snapshot"></a>Pořízení snímku pomocí portálu Azure 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Spouštění v levém horním, klikněte na tlačítko **nový** a vyhledejte **snímku**.
3. V okně snímku, klikněte na tlačítko **vytvořit**.
4. Zadejte **název** pro snímku.
5. Vyberte existující [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků. 
6. Vyberte umístění datového centra Azure.  
7. Pro **zdrojový disk**, vyberte spravované Disk snímek.
8. Vyberte **typ účtu** používat k uložení snímku. Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.
9. Klikněte na možnost **Vytvořit**.

### <a name="use-powershell-to-take-a-snapshot"></a>Pořízení snímku pomocí prostředí PowerShell
Následující kroky vám ukážou, jak získat disku VHD zkopírovat, vytvořte snímek konfigurace, a pořízení snímku disku pomocí rutiny New-AzureRmSnapshot<!--Add link to cmdlet when available-->. 

1. Nastavte některé parametry. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Nahraďte hodnoty parametrů:
  -  "myResourceGroup" se skupinou prostředků Virtuálního počítače.
  -  "southeastasia" s zeměpisného umístění, kam chcete spravovat snímku uloženy. <!---How do you look these up? -->
  -  "ContosoMD_datadisk1" s název disku VHD, který chcete zkopírovat.
  -  "ContosoMD_datadisk1_snapshot1" s název, který chcete použít pro nový snímek.

2. Získáte virtuální pevný disk disku ke kopírování.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Vytvořte snímek konfigurace. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Vytvořte snímek.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Pokud máte v plánu používat k vytvoření disku spravované a připojte ji virtuální počítač, který musí být vysoké provádění snímku, použijte parametr `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot. Parametr vytvoří snímek tak, aby je uložena jako spravovaný Disk úrovně Premium. Premium spravované disky jsou dražší než Standard. Proto ujistěte se, že je skutečně potřebujete Premium před použitím tohoto parametru.


