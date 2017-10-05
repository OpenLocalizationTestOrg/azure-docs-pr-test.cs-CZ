---
title: "Změnit velikost virtuální počítač s Windows v Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Změňte velikost virtuálního počítače s Windows vytvořené v modelu nasazení Resource Manager pomocí Azure Powershell."
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
ms.openlocfilehash: 742efd1496de9ce76b1e5636297ef30f546bd108
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="resize-a-windows-vm"></a><span data-ttu-id="e2ded-103">Změnit velikost virtuálního počítače systému Windows</span><span class="sxs-lookup"><span data-stu-id="e2ded-103">Resize a Windows VM</span></span>
<span data-ttu-id="e2ded-104">Tento článek ukazuje, jak změnit velikost virtuálního počítače Windows, vytvořené v modelu nasazení Resource Manager pomocí Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="e2ded-104">This article shows you how to resize a Windows VM, created in the Resource Manager deployment model using Azure Powershell.</span></span>

<span data-ttu-id="e2ded-105">Po vytvoření virtuálního počítače (VM), je možné škálovat virtuální počítač nahoru nebo dolů změnou velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-105">After you create a virtual machine (VM), you can scale the VM up or down by changing the VM size.</span></span> <span data-ttu-id="e2ded-106">V některých případech se musí nejprve navrácení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-106">In some cases, you must deallocate the VM first.</span></span> <span data-ttu-id="e2ded-107">To může dojít, pokud nová velikost není k dispozici na hardwaru clusteru, který je aktuálně hostuje virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e2ded-107">This can happen if the new size is not available on the hardware cluster that is currently hosting the VM.</span></span>

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a><span data-ttu-id="e2ded-108">Změnit velikost virtuálního počítače Windows není v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="e2ded-108">Resize a Windows VM not in an availability set</span></span>
1. <span data-ttu-id="e2ded-109">Zobrazí seznam velikosti virtuálních počítačů, které jsou k dispozici na hardwaru clusteru je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-109">List the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span> 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. <span data-ttu-id="e2ded-110">Pokud požadovaná velikost je uveden, spusťte následující příkazy ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-110">If the desired size is listed, run the following commands to resize the VM.</span></span> <span data-ttu-id="e2ded-111">Pokud není uvedené požadované velikosti, přejděte ke kroku 3.</span><span class="sxs-lookup"><span data-stu-id="e2ded-111">If the desired size is not listed, go on to step 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="e2ded-112">Pokud není uvedené požadované velikosti, spusťte následující příkazy se zrušit přidělení virtuálního počítače, jeho velikost a restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e2ded-112">If the desired size is not listed, run the following commands to deallocate the VM, resize it, and restart the VM.</span></span>
   
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
> <span data-ttu-id="e2ded-113">Rušení přidělení virtuálního počítače uvolní všechny dynamické IP adresy přiřazené k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e2ded-113">Deallocating the VM releases any dynamic IP addresses assigned to the VM.</span></span> <span data-ttu-id="e2ded-114">Ovlivněné nejsou disky operačního systému a data.</span><span class="sxs-lookup"><span data-stu-id="e2ded-114">The OS and data disks are not affected.</span></span> 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a><span data-ttu-id="e2ded-115">Změnit velikost virtuální počítač s Windows v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="e2ded-115">Resize a Windows VM in an availability set</span></span>
<span data-ttu-id="e2ded-116">Pokud velikost nového virtuálního počítače v nastavení dostupnosti není k dispozici na hardwaru cluster aktuálně hostuje virtuální počítač, všechny virtuální počítače v sadě dostupnosti bude muset být navrácena ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-116">If the new size for a VM in an availability set is not available on the hardware cluster currently hosting the VM, then all VMs in the availability set will need to be deallocated to resize the VM.</span></span> <span data-ttu-id="e2ded-117">Také může musíte aktualizovat velikost ostatních virtuálních počítačů ve skupině po změně velikosti jeden virtuální počítač dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e2ded-117">You also might need to update the size of other VMs in the availability set after one VM has been resized.</span></span> <span data-ttu-id="e2ded-118">Ke změně velikosti virtuálních počítačů v nastavení dostupnosti, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="e2ded-118">To resize a VM in an availability set, perform the following steps.</span></span>

1. <span data-ttu-id="e2ded-119">Zobrazí seznam velikosti virtuálních počítačů, které jsou k dispozici na hardwaru clusteru je hostitelem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-119">List the VM sizes that are available on the hardware cluster where the VM is hosted.</span></span>
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. <span data-ttu-id="e2ded-120">Pokud požadovaná velikost je uveden, spusťte následující příkazy ke změně velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2ded-120">If the desired size is listed, run the following commands to resize the VM.</span></span> <span data-ttu-id="e2ded-121">Pokud není v seznamu uvedena, přejděte ke kroku 3.</span><span class="sxs-lookup"><span data-stu-id="e2ded-121">If it is not listed, go to step 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="e2ded-122">Pokud není uvedené požadované velikosti, pokračujte následujícími kroky k navrácení všechny virtuální počítače v sadě dostupnosti, změně velikosti virtuálních počítačů a je restartovat.</span><span class="sxs-lookup"><span data-stu-id="e2ded-122">If the desired size is not listed, continue with the following steps to deallocate all VMs in the availability set, resize VMs, and restart them.</span></span>
4. <span data-ttu-id="e2ded-123">Zastavte všechny virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e2ded-123">Stop all VMs in the availability set.</span></span>
   
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
5. <span data-ttu-id="e2ded-124">Změní velikost a restartovat virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="e2ded-124">Resize and restart the VMs in the availability set.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="e2ded-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e2ded-125">Next steps</span></span>
* <span data-ttu-id="e2ded-126">Pro další škálovatelnost spustit více instancí virtuálního počítače a horizontální rozšíření kapacity.</span><span class="sxs-lookup"><span data-stu-id="e2ded-126">For additional scalability, run multiple VM instances and scale out.</span></span> <span data-ttu-id="e2ded-127">Další informace najdete v tématu [automaticky škálovat počítače s Windows v sadě škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="e2ded-127">For more information, see [Automatically scale Windows machines in a Virtual Machine Scale Set](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span></span>

