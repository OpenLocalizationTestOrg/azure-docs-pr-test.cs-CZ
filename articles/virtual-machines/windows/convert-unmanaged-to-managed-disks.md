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
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="77f96-103">Převod virtuálního počítače s Windows nespravovaných disky toomanaged disky</span><span class="sxs-lookup"><span data-stu-id="77f96-103">Convert a Windows virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="77f96-104">Pokud máte existující Windows virtuální počítače (VM) používající nespravované disky, můžete převést disky toouse spravované hello virtuálních počítačů prostřednictvím hello [Azure spravované disky](managed-disks-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="77f96-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="77f96-105">Tento proces převede disk hello operačního systému a všechny připojené datových disků.</span><span class="sxs-lookup"><span data-stu-id="77f96-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="77f96-106">Tento článek ukazuje, jak tooconvert virtuálních počítačů pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="77f96-106">This article shows you how tooconvert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="77f96-107">Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="77f96-107">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="77f96-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="77f96-108">Before you begin</span></span>


* <span data-ttu-id="77f96-109">Zkontrolujte [plánování migrace hello tooManaged disky](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="77f96-109">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="77f96-110">Převést virtuální počítače jednou instancí</span><span class="sxs-lookup"><span data-stu-id="77f96-110">Convert single-instance VMs</span></span>
<span data-ttu-id="77f96-111">Tato část popisuje, jak tooconvert jedné instance virtuální počítače Azure z nespravovaných disky toomanaged disky.</span><span class="sxs-lookup"><span data-stu-id="77f96-111">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="77f96-112">(Pokud jsou vaše virtuální počítače v nastavení dostupnosti, viz další část hello.)</span><span class="sxs-lookup"><span data-stu-id="77f96-112">(If your VMs are in an availability set, see hello next section.)</span></span> 

1. <span data-ttu-id="77f96-113">Deallocate hello virtuálních počítačů pomocí hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny.</span><span class="sxs-lookup"><span data-stu-id="77f96-113">Deallocate hello VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="77f96-114">Hello následující příklad zruší přidělení hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77f96-114">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="77f96-115">Převést disky toomanaged hello virtuálních počítačů pomocí hello [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) rutiny.</span><span class="sxs-lookup"><span data-stu-id="77f96-115">Convert hello VM toomanaged disks by using hello [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="77f96-116">Hello následující proces převede hello předchozí virtuálních počítačů, včetně disku hello operačního systému a všechny datové disky:</span><span class="sxs-lookup"><span data-stu-id="77f96-116">hello following process converts hello previous VM, including hello OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="77f96-117">Spustit hello virtuálních počítačů po hello převod toomanaged disky pomocí [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="77f96-117">Start hello VM after hello conversion toomanaged disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="77f96-118">Následující příklad restartování Hello hello předchozí virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="77f96-118">hello following example restarts hello previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="77f96-119">Převést virtuální počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="77f96-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="77f96-120">Pokud hello virtuálních počítačů, které chcete tooconvert toomanaged disky jsou v nastavení dostupnosti, je nutné nejprve tooconvert hello dostupnost sady tooa spravovat sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="77f96-120">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

1. <span data-ttu-id="77f96-121">Převést hello dostupnosti pomocí hello [aktualizace AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) rutiny.</span><span class="sxs-lookup"><span data-stu-id="77f96-121">Convert hello availability set by using hello [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="77f96-122">Následující ukázka aktualizace hello sadu s názvem dostupnosti Hello `myAvailabilitySet` v hello skupinu prostředků s názvem `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="77f96-122">hello following example updates hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="77f96-123">Pokud hello oblasti, kde se nachází váš sady dostupnosti. má pouze 2 domén spravované selhání, ale hello počet domén selhání nespravované 3, tento příkaz zobrazí chybu podobné příliš "hello zadaný počet domén selhání 3 musí být v rozsahu 1 too2 hello."</span><span class="sxs-lookup"><span data-stu-id="77f96-123">If hello region where your availability set is located has only 2 managed fault domains but hello number of unmanaged fault domains is 3, this command shows an error similar too"hello specified fault domain count 3 must fall in hello range 1 too2."</span></span> <span data-ttu-id="77f96-124">tooresolve hello chyba, too2 domény selhání hello aktualizace a aktualizace `Sku` příliš`Aligned` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="77f96-124">tooresolve hello error, update hello fault domain too2 and update `Sku` too`Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="77f96-125">Deallocate a převést hello virtuálních počítačů v nastavení dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="77f96-125">Deallocate and convert hello VMs in hello availability set.</span></span> <span data-ttu-id="77f96-126">Hello následující skript zruší přidělení každý virtuální počítač pomocí hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) rutiny, převede ji na základě [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)a restartuje s použitím [Start-AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="77f96-126">hello following script deallocates each VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

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


## <a name="troubleshooting"></a><span data-ttu-id="77f96-127">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="77f96-127">Troubleshooting</span></span>

<span data-ttu-id="77f96-128">Pokud dojde k chybě při převodu, nebo pokud je virtuální počítač ve stavu selhání kvůli problémům v předchozí převod, spusťte hello `ConvertTo-AzureRmVMManagedDisk` rutinu znovu.</span><span class="sxs-lookup"><span data-stu-id="77f96-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run hello `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="77f96-129">Jednoduché opakování obvykle odblokuje hello situaci.</span><span class="sxs-lookup"><span data-stu-id="77f96-129">A simple retry usually unblocks hello situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="77f96-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="77f96-130">Next steps</span></span>

[<span data-ttu-id="77f96-131">Převést standardní disky spravované toopremium</span><span class="sxs-lookup"><span data-stu-id="77f96-131">Convert standard managed disks toopremium</span></span>](convert-disk-storage.md)

<span data-ttu-id="77f96-132">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="77f96-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

