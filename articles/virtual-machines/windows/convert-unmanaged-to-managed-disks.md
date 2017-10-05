---
title: "Převod virtuálního počítače s Windows nespravované disky na spravovaných disků - disků spravované Azure | Microsoft Docs"
description: "Jak převést virtuální počítač s Windows z nespravovaných disků na discích spravovaných pomocí prostředí PowerShell v modelu nasazení Resource Manager"
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
ms.openlocfilehash: 54afcf1e37f696979bfe270a473c72aedf20dc43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a><span data-ttu-id="bf6d2-103">Převod virtuálního počítače s Windows nespravovaných disky na spravované disky</span><span class="sxs-lookup"><span data-stu-id="bf6d2-103">Convert a Windows virtual machine from unmanaged disks to managed disks</span></span>

<span data-ttu-id="bf6d2-104">Pokud máte existující Windows virtuální počítače (VM) používající nespravované disky, můžete převést virtuální počítače použít spravované disky prostřednictvím [Azure spravované disky](managed-disks-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert the VMs to use managed disks through the [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="bf6d2-105">Tento proces převede disk operačního systému a všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-105">This process converts both the OS disk and any attached data disks.</span></span>

<span data-ttu-id="bf6d2-106">Tento článek ukazuje, jak převést virtuální počítače pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-106">This article shows you how to convert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="bf6d2-107">Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="bf6d2-107">If you need to install or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bf6d2-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="bf6d2-108">Before you begin</span></span>


* <span data-ttu-id="bf6d2-109">Zkontrolujte [plánování migrace na spravované disky](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="bf6d2-109">Review [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="bf6d2-110">Převést virtuální počítače jednou instancí</span><span class="sxs-lookup"><span data-stu-id="bf6d2-110">Convert single-instance VMs</span></span>
<span data-ttu-id="bf6d2-111">Tato část popisuje jak převést virtuální počítače Azure jednou instancí z nespravovaných disků na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-111">This section covers how to convert single-instance Azure VMs from unmanaged disks to managed disks.</span></span> <span data-ttu-id="bf6d2-112">(Pokud jsou vaše virtuální počítače v nastavení dostupnosti, najdete v další části.)</span><span class="sxs-lookup"><span data-stu-id="bf6d2-112">(If your VMs are in an availability set, see the next section.)</span></span> 

1. <span data-ttu-id="bf6d2-113">Zrušit přidělení virtuálního počítače pomocí [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-113">Deallocate the VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="bf6d2-114">Následující příklad zruší přidělení virtuálního počítače s názvem `myVM` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bf6d2-114">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="bf6d2-115">Převeďte virtuální počítač na disky spravované pomocí [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) rutiny.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-115">Convert the VM to managed disks by using the [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="bf6d2-116">Následující proces převede předchozí virtuálních počítačů, včetně disku operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="bf6d2-116">The following process converts the previous VM, including the OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="bf6d2-117">Spusťte virtuální počítač po převodu na spravované disky pomocí [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="bf6d2-117">Start the VM after the conversion to managed disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="bf6d2-118">Následující příklad restartuje předchozí virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="bf6d2-118">The following example restarts the previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="bf6d2-119">Převést virtuální počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="bf6d2-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="bf6d2-120">Pokud virtuální počítače, které chcete převést na spravované disky jsou v nastavení dostupnosti, je nutné nejprve převést skupinu dostupnosti do skupiny spravované dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-120">If the VMs that you want to convert to managed disks are in an availability set, you first need to convert the availability set to a managed availability set.</span></span>

1. <span data-ttu-id="bf6d2-121">Převést skupinu dostupnosti pomocí [aktualizace AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) rutiny.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-121">Convert the availability set by using the [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="bf6d2-122">Následující příklad aktualizuje skupinu dostupnosti s názvem `myAvailabilitySet` ve skupině prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="bf6d2-122">The following example updates the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="bf6d2-123">Pokud oblast, kde vaše dostupnosti nastavit nachází má pouze 2 domén spravované selhání, ale počet domén selhání nespravované 3, tento příkaz zobrazí chybu podobná "zadaný počet domén selhání 3 musí spadat do rozsahu 1 až 2."</span><span class="sxs-lookup"><span data-stu-id="bf6d2-123">If the region where your availability set is located has only 2 managed fault domains but the number of unmanaged fault domains is 3, this command shows an error similar to "The specified fault domain count 3 must fall in the range 1 to 2."</span></span> <span data-ttu-id="bf6d2-124">Opravte případné chyby, aktualizujte doméně selhání 2 a update `Sku` k `Aligned` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="bf6d2-124">To resolve the error, update the fault domain to 2 and update `Sku` to `Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="bf6d2-125">Deallocate a převést virtuální počítače v sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-125">Deallocate and convert the VMs in the availability set.</span></span> <span data-ttu-id="bf6d2-126">Následující skript zruší přidělení každý virtuální počítač pomocí [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny, převede ji na základě [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)a restartuje s použitím [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="bf6d2-126">The following script deallocates each VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

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


## <a name="troubleshooting"></a><span data-ttu-id="bf6d2-127">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="bf6d2-127">Troubleshooting</span></span>

<span data-ttu-id="bf6d2-128">Pokud dojde k chybě při převodu, nebo pokud je virtuální počítač ve stavu selhání kvůli problémům v předchozí převod, spusťte `ConvertTo-AzureRmVMManagedDisk` rutinu znovu.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run the `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="bf6d2-129">Jednoduché opakování obvykle odblokuje situaci.</span><span class="sxs-lookup"><span data-stu-id="bf6d2-129">A simple retry usually unblocks the situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="bf6d2-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf6d2-130">Next steps</span></span>

[<span data-ttu-id="bf6d2-131">Převést standardní spravovaných disků na premium</span><span class="sxs-lookup"><span data-stu-id="bf6d2-131">Convert standard managed disks to premium</span></span>](convert-disk-storage.md)

<span data-ttu-id="bf6d2-132">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="bf6d2-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

