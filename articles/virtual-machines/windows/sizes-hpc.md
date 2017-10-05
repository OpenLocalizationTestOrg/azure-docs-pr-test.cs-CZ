---
title: "Azure velikosti virtuálních počítačů Windows - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostí, které jsou k dispozici pro Windows s vysokým výkonem virtuálních počítačů v Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: a0596d134e9c26877848f93d72f35bfd2c957570
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="high-performance-compute-vm-sizes"></a><span data-ttu-id="e7ffe-103">Velikosti virtuálních počítačů pro výpočetní vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="e7ffe-103">High performance compute VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="e7ffe-104">RDMA podporovat instancí</span><span class="sxs-lookup"><span data-stu-id="e7ffe-104">RDMA-capable instances</span></span>
<span data-ttu-id="e7ffe-105">Podmnožinu náročné instance (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-105">A subset of the compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="e7ffe-106">Toto rozhraní je kromě standardní Azure síťové rozhraní, které jsou dostupné další velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-106">This interface is in addition to the standard Azure network interface available to other VM sizes.</span></span> 
  
<span data-ttu-id="e7ffe-107">Toto rozhraní umožňuje podporu rdma instance komunikovat přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-107">This interface allows the RDMA-capable instances to communicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="e7ffe-108">Tyto funkce RDMA může zvýšit škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-108">These RDMA capabilities can boost the scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="e7ffe-109">Toto jsou požadavky pro podporu rdma virtuálních počítačů Windows pro přístup k síti Azure RDMA:</span><span class="sxs-lookup"><span data-stu-id="e7ffe-109">Following are requirements for RDMA-capable Windows VMs to access the Azure RDMA network:</span></span> 

* <span data-ttu-id="e7ffe-110">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="e7ffe-110">**Operating system**</span></span>
  
  <span data-ttu-id="e7ffe-111">Windows Server 2012 R2, Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="e7ffe-111">Windows Server 2012 R2, Windows Server 2012</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="e7ffe-112">Windows Server 2016 v současné době nepodporuje připojení RDMA v Azure.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-112">Currently, Windows Server 2016 does not support RDMA connectivity in Azure.</span></span>
  >

* <span data-ttu-id="e7ffe-113">**Skupinu dostupnosti nebo Cloudová služba** – nasazení podporující RDMA virtuální počítače ve stejné skupině dostupnosti (při použití modelu nasazení Azure Resource Manager) nebo stejné cloudové služby (při použití modelu nasazení classic).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-113">**Availability set or cloud service** – Deploy the RDMA-capable VMs in the same availability set (when you use the Azure Resource Manager deployment model) or the same cloud service (when you use the classic deployment model).</span></span> <span data-ttu-id="e7ffe-114">Pokud používáte Azure Batch, podporující RDMA virtuální počítače musí být ve stejném fondu.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-114">If you use Azure Batch, the RDMA-capable VMs must be in the same pool.</span></span>

* <span data-ttu-id="e7ffe-115">**MPI** -Microsoft MPI (MS-MPI) 2012 R2 nebo novější, Intel MPI knihovny 5.x</span><span class="sxs-lookup"><span data-stu-id="e7ffe-115">**MPI** - Microsoft MPI (MS-MPI) 2012 R2 or later, Intel MPI Library 5.x</span></span>

  <span data-ttu-id="e7ffe-116">Podporované MPI implementace rozhraní Microsoft Network Direct používají ke komunikaci mezi instancemi.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-116">Supported MPI implementations use the Microsoft Network Direct interface to communicate between instances.</span></span> 

* <span data-ttu-id="e7ffe-117">**Adresní prostor sítě RDMA** -The RDMA sítě v Azure si vyhrazuje 172.16.0.0/16 prostor adres.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-117">**RDMA network address space** - The RDMA network in Azure reserves the address space 172.16.0.0/16.</span></span> <span data-ttu-id="e7ffe-118">Pro spouštění aplikací MPI v instance nasazené v Azure virtuální síť, ujistěte se, že adresního prostoru virtuální sítě se nepřekrývá sítě RDMA.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-118">To run MPI applications on instances deployed in an Azure virtual network, make sure that the virtual network address space does not overlap the RDMA network.</span></span>

* <span data-ttu-id="e7ffe-119">**Rozšíření virtuálního počítače HpcVmDrivers** -na podporu rdma virtuálních počítačů, je nutné přidat rozšíření HpcVmDrivers instalovat ovladače zařízení systému Windows sítě pro připojení RDMA.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-119">**HpcVmDrivers VM extension** - On RDMA-capable VMs, you must add the HpcVmDrivers extension to install Windows network device drivers for RDMA connectivity.</span></span> <span data-ttu-id="e7ffe-120">(V určitých nasazeních A8 a A9 instancí HpcVmDrivers rozšíření se přidá automaticky.) Chcete-li přidat rozšíření virtuálního počítače pro virtuální počítač, můžete použít [prostředí Azure PowerShell](/powershell/azure/overview) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-120">(In certain deployments of A8 and A9 instances, the HpcVmDrivers extension is added automatically.) To add the VM extension to a VM, you can use [Azure PowerShell](/powershell/azure/overview) cmdlets.</span></span> 

  
  <span data-ttu-id="e7ffe-121">Tento příkaz nainstaluje nejnovější rozšíření HpcVMDrivers verze 1.1 na existující podporující RDMA virtuální počítač s názvem *Můjvp* nasazené ve skupině prostředků s názvem *myResourceGroup* v  *Západní USA* oblasti:</span><span class="sxs-lookup"><span data-stu-id="e7ffe-121">The following command installs the latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named *myVM* deployed in the resource group named *myResourceGroup* in the *West US* region:</span></span>

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  <span data-ttu-id="e7ffe-122">Další informace najdete v tématu [rozšíření virtuálního počítače a funkce](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-122">For more information, see [Virtual machine extensions and features](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="e7ffe-123">Můžete také pracovat se rozšíření pro virtuální počítače nasazené v [modelu nasazení classic](classic/manage-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-123">You can also work with extensions for VMs deployed in the [classic deployment model](classic/manage-extensions.md).</span></span>


## <a name="using-hpc-pack"></a><span data-ttu-id="e7ffe-124">Pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="e7ffe-124">Using HPC Pack</span></span>

<span data-ttu-id="e7ffe-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností vytvořit výpočetní cluster v Azure ke spouštění aplikací MPI systému Windows a další úlohy v prostředí HPC.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you to create a compute cluster in Azure to run Windows-based MPI applications and other HPC workloads.</span></span> <span data-ttu-id="e7ffe-126">HPC Pack 2012 R2 a novější verze zahrnují běhového prostředí pro MS-MPI, která využívá Azure RDMA síť při nasazení na virtuálních počítačích RDMA podporovat.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-126">HPC Pack 2012 R2 and later versions include a runtime environment for MS-MPI that uses the Azure RDMA network when deployed on RDMA-capable VMs.</span></span>




## <a name="other-sizes"></a><span data-ttu-id="e7ffe-127">Další velikosti</span><span class="sxs-lookup"><span data-stu-id="e7ffe-127">Other sizes</span></span>
- [<span data-ttu-id="e7ffe-128">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="e7ffe-128">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="e7ffe-129">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="e7ffe-129">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="e7ffe-130">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="e7ffe-130">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="e7ffe-131">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="e7ffe-131">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="e7ffe-132">Optimalizované z hlediska GPU</span><span class="sxs-lookup"><span data-stu-id="e7ffe-132">GPU optimized</span></span>](sizes-gpu.md)

## <a name="next-steps"></a><span data-ttu-id="e7ffe-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e7ffe-133">Next steps</span></span>

- <span data-ttu-id="e7ffe-134">Kontrolní seznamy pro použití instance náročné s HPC Pack v systému Windows Server, najdete v části [nastavení clusteru s podporou Windows RDMA pomocí sady HPC Pack ke spouštění aplikací MPI](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-134">For checklists to use the compute-intensive instances with HPC Pack on Windows Server, see [Set up a Windows RDMA cluster with HPC Pack to run MPI applications](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="e7ffe-135">Použití náročné instancí při spuštění aplikací MPI pomocí služby Azure Batch naleznete v části [pomocí úkolů s více instancemi ke spouštění aplikací rozhraní MPI (Message Passing) ve službě Azure Batch](../../batch/batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="e7ffe-135">To use compute-intensive instances when running MPI applications with Azure Batch, see [Use multi-instance tasks to run Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).</span></span>

- <span data-ttu-id="e7ffe-136">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="e7ffe-136">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




