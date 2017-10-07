---
title: "aaaCreate kopii diskem spravované Azure pro zálohování | Microsoft Docs"
description: "Zjistěte, jak toocreate kopii toouse Azure spravované disku pro disk zpět nahoru nebo řešení problémů s problémy."
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
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Vytvoření kopie virtuálního pevného disku uložené jako spravované disku Azure s využitím spravované snímky
Pořízení snímku spravované disku pro zálohu nebo vytvoření disku spravované ze snímku hello a připojte ji tooa testovací virtuální počítač tootroubleshoot. Spravované snímek je úplné v daném okamžiku kopie disku spravovaných virtuálních počítačů. Ji vytvoří kopii svůj disk VHD jen pro čtení a ve výchozím nastavení, ukládá jako standardní spravované Disk. Další informace o discích spravovaných najdete v tématu [disky spravované Azure – přehled](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

Informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Než začnete
Pokud používáte prostředí PowerShell, ujistěte se, že máte nejnovější verzi hello modulu AzureRM.Compute PowerShell hello. Spustit hello následující příkaz tooinstall.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).

## <a name="copy-hello-vhd-with-a-snapshot"></a>Zkopírujte hello virtuálního pevného disku s snímku
Použijte hello portál Azure nebo PowerShell tootake snímek hello spravované disku.

### <a name="use-azure-portal-tootake-a-snapshot"></a>Použití portálu Azure tootake snímku 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Spuštění v levé horní části hello, klikněte na tlačítko **nový** a vyhledejte **snímku**.
3. V okně hello snímku, klikněte na tlačítko **vytvořit**.
4. Zadejte **název** pro vytvoření snímku hello.
5. Vyberte existující [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové. 
6. Vyberte umístění datového centra Azure.  
7. Pro **zdrojový disk**, vyberte hello toosnapshot spravované disku.
8. Vyberte hello **typ účtu** toouse toostore hello snímku. Doporučujeme, abyste **Standard_LRS** Pokud to uložená na vysokou provádění disku potřebujete.
9. Klikněte na možnost **Vytvořit**.

### <a name="use-powershell-tootake-a-snapshot"></a>Pomocí prostředí PowerShell tootake snímku
Hello následující kroky ukazují, jak tooget hello virtuální pevný disk disku toobe zkopírovat, vytvořte snímek konfigurace hello a pořízení snímku hello disku pomocí rutiny New-AzureRmSnapshot hello<!--Add link toocmdlet when available-->. 

1. Nastavte některé parametry. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Nahraďte hodnoty parametrů hello:
  -  "myResourceGroup" se skupinou prostředků hello Virtuálního počítače.
  -  "southeastasia" s hello zeměpisné umístění, kam má spravované snímku uloženy. <!---How do you look these up? -->
  -  "ContosoMD_datadisk1" s název hello hello disku VHD, které chcete toocopy.
  -  "ContosoMD_datadisk1_snapshot1" s hello, kterou si chcete toouse pro nový snímek hello.

2. Získáte toobe disku VHD hello zkopírovali.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Vytvoření snímku konfigurace hello. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Pořízení snímku hello.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Pokud plánování toouse hello snímku toocreate Disk spravované a připojte ji virtuální počítač, který potřebuje toobe vysoké provádění, použijte parametr hello `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot hello. Parametr Hello vytvoří snímek hello tak, aby je uložena jako spravovaný Disk úrovně Premium. Premium spravované disky jsou dražší než Standard. Proto ujistěte se, že je skutečně potřebujete Premium před použitím tohoto parametru.


