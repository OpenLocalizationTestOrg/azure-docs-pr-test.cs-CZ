---
title: "aaaUse prostředí PowerShell tooresize virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Změňte velikost virtuálního počítače s Windows vytvořené v modelu nasazení Resource Manager hello, pomocí Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: a4a80f3bc99911e4f1a095f0ce63aca00fa50694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm"></a>Změnit velikost virtuálního počítače systému Windows
Tento článek ukazuje, jak tooresize virtuální počítač s Windows, vytvořené v modelu nasazení Resource Manager hello pomocí Azure Powershell.

Po vytvoření virtuálního počítače (VM), je možné škálovat hello virtuálních počítačů nahoru nebo dolů tak, že změníte velikost virtuálního počítače hello. V některých případech se musí nejprve navrácení hello virtuálních počítačů. To může dojít, pokud hello novou velikost není k dispozici v clusteru hello hardwaru, která je aktuálně hostitelem hello virtuálních počítačů.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Změnit velikost virtuálního počítače Windows není v nastavení dostupnosti
1. Zobrazí seznam hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello je hostitelem hello virtuálních počítačů. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. V případě, že je uvedena velikost potřeby hello spusťte následující příkazy tooresize hello virtuálních počítačů hello. V případě potřeby hello velikost není uvedený, přejděte na toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. V případě potřeby hello není uvedena velikost spusťte hello následující příkazy toodeallocate hello virtuálního počítače, jeho velikost a restartujte hello virtuálních počítačů.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Rušení přidělení hello virtuálních počítačů uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů. ovlivněné nejsou Hello operačního systému a datové disky. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Změnit velikost virtuální počítač s Windows v nastavení dostupnosti
Pokud hello novou velikost virtuálního počítače v nastavení dostupnosti není k dispozici na hello hardwaru clusteru hello aktuálně hostuje virtuální počítač, pak všechny virtuální počítače ve skupině dostupnosti hello bude nutné toobe navrácena tooresize hello virtuálních počítačů. Můžete také potřebovat tooupdate hello velikost ostatních virtuálních počítačů ve skupině po změně velikosti jeden virtuální počítač dostupnosti hello. tooresize virtuálního počítače v nastavení dostupnosti, proveďte následující kroky hello.

1. Zobrazí seznam hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello je hostitelem hello virtuálních počítačů.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. V případě, že je uvedena velikost potřeby hello spusťte následující příkazy tooresize hello virtuálních počítačů hello. Pokud ne, přejděte toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. V případě potřeby hello není uvedena velikost pokračovat hello následující kroky toodeallocate všechny virtuální počítače ve skupině dostupnosti hello, změně velikosti virtuálních počítačů a je restartovat.
4. Zastavte všechny virtuální počítače ve skupině dostupnosti hello.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Změnit velikost a restartujte hello virtuální počítače v sadě dostupnosti hello.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Další kroky
* Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače s Windows v sadě škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

