---
title: "aaaConvert virtuálního počítače s Windows z nespravovaných disky toomanaged disků - disků spravované Azure | Microsoft Docs"
description: "Jak tooconvert virtuální počítač s Windows z nespravovaných disky toomanaged disky pomocí prostředí PowerShell v modelu nasazení Resource Manager hello"
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
ms.date: 06/23/2017
ms.author: cynthn
ms.openlocfilehash: e8ed8694b0e776d22df26261e2fc8340bfe5cafa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>Převod virtuálního počítače s Windows nespravovaných disky toomanaged disky

Pokud máte existující Windows virtuální počítače (VM) používající nespravované disky, můžete převést disky toouse spravované hello virtuálních počítačů prostřednictvím hello [Azure spravované disky](managed-disks-overview.md) služby. Tento proces převede disk hello operačního systému a všechny připojené datových disků.

Tento článek ukazuje, jak tooconvert virtuálních počítačů pomocí Azure PowerShell. Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Než začnete


* Zkontrolujte [plánování migrace hello tooManaged disky](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Převést virtuální počítače jednou instancí
Tato část popisuje, jak tooconvert jedné instance virtuální počítače Azure z nespravovaných disky toomanaged disky. (Pokud jsou vaše virtuální počítače v nastavení dostupnosti, viz další část hello.) 

1. Deallocate hello virtuálních počítačů pomocí hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny. Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`: 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Převést disky toomanaged hello virtuálních počítačů pomocí hello [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) rutiny. Hello následující proces převede hello předchozí virtuálních počítačů, včetně disku hello operačního systému a všechny datové disky:

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Spustit hello virtuálních počítačů po hello převod toomanaged disky pomocí [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). Následující příklad restartování Hello hello předchozí virtuálních počítačů:

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Převést virtuální počítače v nastavení dostupnosti

Pokud hello virtuálních počítačů, které chcete tooconvert toomanaged disky jsou v nastavení dostupnosti, je nutné nejprve tooconvert hello dostupnost sady tooa spravovat sady dostupnosti.

1. Převést hello dostupnosti pomocí hello [aktualizace AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) rutiny. Následující ukázka aktualizace hello sadu s názvem dostupnosti Hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Pokud hello oblasti, kde se nachází váš sady dostupnosti. má pouze 2 domén spravované selhání, ale hello počet domén selhání nespravované 3, tento příkaz zobrazí chybu podobné příliš "hello zadaný počet domén selhání 3 musí být v rozsahu 1 too2 hello." tooresolve hello chyba, too2 domény selhání hello aktualizace a aktualizace `Sku` příliš`Aligned` následujícím způsobem:

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Deallocate a převést hello virtuálních počítačů v nastavení dostupnosti hello. Hello následující skript zruší přidělení každý virtuální počítač pomocí hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny, převede ji na základě [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)a restartuje s použitím [Start-AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```


## <a name="troubleshooting"></a>Řešení potíží

Pokud dojde k chybě při převodu, nebo pokud je virtuální počítač ve stavu selhání kvůli problémům v předchozí převod, spusťte hello `ConvertTo-AzureRmVMManagedDisk` rutinu znovu. Jednoduché opakování obvykle odblokuje hello situaci.


## <a name="next-steps"></a>Další kroky

[Převést standardní disky spravované toopremium](convert-disk-storage.md)

Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).

