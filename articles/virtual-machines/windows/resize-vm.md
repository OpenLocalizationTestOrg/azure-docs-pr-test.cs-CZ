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
# <a name="resize-a-windows-vm"></a><span data-ttu-id="5de5a-103">Změnit velikost virtuálního počítače systému Windows</span><span class="sxs-lookup"><span data-stu-id="5de5a-103">Resize a Windows VM</span></span>
<span data-ttu-id="5de5a-104">Tento článek ukazuje, jak tooresize virtuální počítač s Windows, vytvořené v modelu nasazení Resource Manager hello pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="5de5a-104">This article shows you how tooresize a Windows VM, created in hello Resource Manager deployment model using Azure Powershell.</span></span>

<span data-ttu-id="5de5a-105">Po vytvoření virtuálního počítače (VM), je možné škálovat hello virtuálních počítačů nahoru nebo dolů tak, že změníte velikost virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-105">After you create a virtual machine (VM), you can scale hello VM up or down by changing hello VM size.</span></span> <span data-ttu-id="5de5a-106">V některých případech se musí nejprve navrácení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="5de5a-107">To může dojít, pokud hello novou velikost není k dispozici v clusteru hello hardwaru, která je aktuálně hostitelem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-107">This can happen if hello new size is not available on hello hardware cluster that is currently hosting hello VM.</span></span>

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a><span data-ttu-id="5de5a-108">Změnit velikost virtuálního počítače Windows není v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="5de5a-108">Resize a Windows VM not in an availability set</span></span>
1. <span data-ttu-id="5de5a-109">Zobrazí seznam hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello je hostitelem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-109">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span> 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. <span data-ttu-id="5de5a-110">V případě, že je uvedena velikost potřeby hello spusťte následující příkazy tooresize hello virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-110">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="5de5a-111">V případě potřeby hello velikost není uvedený, přejděte na toostep 3.</span><span class="sxs-lookup"><span data-stu-id="5de5a-111">If hello desired size is not listed, go on toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="5de5a-112">V případě potřeby hello není uvedena velikost spusťte hello následující příkazy toodeallocate hello virtuálního počítače, jeho velikost a restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-112">If hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and restart hello VM.</span></span>
   
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
> <span data-ttu-id="5de5a-113">Rušení přidělení hello virtuálních počítačů uvolní všechny dynamické IP adresy přiřazené toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-113">Deallocating hello VM releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="5de5a-114">ovlivněné nejsou Hello operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="5de5a-114">hello OS and data disks are not affected.</span></span> 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a><span data-ttu-id="5de5a-115">Změnit velikost virtuální počítač s Windows v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="5de5a-115">Resize a Windows VM in an availability set</span></span>
<span data-ttu-id="5de5a-116">Pokud hello novou velikost virtuálního počítače v nastavení dostupnosti není k dispozici na hello hardwaru clusteru hello aktuálně hostuje virtuální počítač, pak všechny virtuální počítače ve skupině dostupnosti hello bude nutné toobe navrácena tooresize hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-116">If hello new size for a VM in an availability set is not available on hello hardware cluster currently hosting hello VM, then all VMs in hello availability set will need toobe deallocated tooresize hello VM.</span></span> <span data-ttu-id="5de5a-117">Můžete také potřebovat tooupdate hello velikost ostatních virtuálních počítačů ve skupině po změně velikosti jeden virtuální počítač dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-117">You also might need tooupdate hello size of other VMs in hello availability set after one VM has been resized.</span></span> <span data-ttu-id="5de5a-118">tooresize virtuálního počítače v nastavení dostupnosti, proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-118">tooresize a VM in an availability set, perform hello following steps.</span></span>

1. <span data-ttu-id="5de5a-119">Zobrazí seznam hello velikosti virtuálních počítačů, které jsou k dispozici v clusteru hardwaru hello je hostitelem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5de5a-119">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. <span data-ttu-id="5de5a-120">V případě, že je uvedena velikost potřeby hello spusťte následující příkazy tooresize hello virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-120">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="5de5a-121">Pokud ne, přejděte toostep 3.</span><span class="sxs-lookup"><span data-stu-id="5de5a-121">If it is not listed, go toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="5de5a-122">V případě potřeby hello není uvedena velikost pokračovat hello následující kroky toodeallocate všechny virtuální počítače ve skupině dostupnosti hello, změně velikosti virtuálních počítačů a je restartovat.</span><span class="sxs-lookup"><span data-stu-id="5de5a-122">If hello desired size is not listed, continue with hello following steps toodeallocate all VMs in hello availability set, resize VMs, and restart them.</span></span>
4. <span data-ttu-id="5de5a-123">Zastavte všechny virtuální počítače ve skupině dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-123">Stop all VMs in hello availability set.</span></span>
   
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
5. <span data-ttu-id="5de5a-124">Změnit velikost a restartujte hello virtuální počítače v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5de5a-124">Resize and restart hello VMs in hello availability set.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="5de5a-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5de5a-125">Next steps</span></span>
* <span data-ttu-id="5de5a-126">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity. Další informace najdete v tématu [automaticky škálovat počítače s Windows v sadě škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="5de5a-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Windows machines in a Virtual Machine Scale Set](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span></span>

