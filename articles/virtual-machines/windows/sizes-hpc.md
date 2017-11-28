---
title: "velikost virtuálního počítače s Windows aaaAzure - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro Windows s vysokým výkonem virtuálních počítačů v Azure."
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
ms.openlocfilehash: 092bc32cfe048f439ad833911bef4ed17eda7977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-vm-sizes"></a><span data-ttu-id="6145a-103">Velikosti virtuálních počítačů pro výpočetní vysoký výkon</span><span class="sxs-lookup"><span data-stu-id="6145a-103">High performance compute VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="6145a-104">RDMA podporovat instancí</span><span class="sxs-lookup"><span data-stu-id="6145a-104">RDMA-capable instances</span></span>
<span data-ttu-id="6145a-105">Podmnožinu hello náročné instancí (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="6145a-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="6145a-106">Toto rozhraní je kromě velikosti virtuálních počítačů pro dostupné tooother toohello standardního Azure síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6145a-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="6145a-107">Toto rozhraní umožňuje hello podporující RDMA instance toocommunicate přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6145a-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="6145a-108">Tyto funkce RDMA může zvýšit hello škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).</span><span class="sxs-lookup"><span data-stu-id="6145a-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="6145a-109">Toto jsou požadavky pro tooaccess podporující RDMA virtuálních počítačů Windows hello Azure RDMA sítě:</span><span class="sxs-lookup"><span data-stu-id="6145a-109">Following are requirements for RDMA-capable Windows VMs tooaccess hello Azure RDMA network:</span></span> 

* <span data-ttu-id="6145a-110">**Operační systém**</span><span class="sxs-lookup"><span data-stu-id="6145a-110">**Operating system**</span></span>
  
  <span data-ttu-id="6145a-111">Windows Server 2012 R2, Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="6145a-111">Windows Server 2012 R2, Windows Server 2012</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="6145a-112">Windows Server 2016 v současné době nepodporuje připojení RDMA v Azure.</span><span class="sxs-lookup"><span data-stu-id="6145a-112">Currently, Windows Server 2016 does not support RDMA connectivity in Azure.</span></span>
  >

* <span data-ttu-id="6145a-113">**Skupinu dostupnosti nebo Cloudová služba** – nasadit hello podporující RDMA virtuálních počítačů v hello stejné dostupnosti (při použití modelu nasazení Azure Resource Manager hello) nebo hello stejné cloudové služby (při použití modelu nasazení classic hello).</span><span class="sxs-lookup"><span data-stu-id="6145a-113">**Availability set or cloud service** – Deploy hello RDMA-capable VMs in hello same availability set (when you use hello Azure Resource Manager deployment model) or hello same cloud service (when you use hello classic deployment model).</span></span> <span data-ttu-id="6145a-114">Pokud používáte Azure Batch, hello podporující RDMA virtuální počítače musí být v hello stejného fondu.</span><span class="sxs-lookup"><span data-stu-id="6145a-114">If you use Azure Batch, hello RDMA-capable VMs must be in hello same pool.</span></span>

* <span data-ttu-id="6145a-115">**MPI** -Microsoft MPI (MS-MPI) 2012 R2 nebo novější, Intel MPI knihovny 5.x</span><span class="sxs-lookup"><span data-stu-id="6145a-115">**MPI** - Microsoft MPI (MS-MPI) 2012 R2 or later, Intel MPI Library 5.x</span></span>

  <span data-ttu-id="6145a-116">Podporované implementace MPI použijte hello Microsoft Network Direct toocommunicate rozhraní mezi instancemi.</span><span class="sxs-lookup"><span data-stu-id="6145a-116">Supported MPI implementations use hello Microsoft Network Direct interface toocommunicate between instances.</span></span> 

* <span data-ttu-id="6145a-117">**Adresní prostor sítě RDMA** -hello RDMA sítě v Azure si vyhrazuje 172.16.0.0/16 prostoru adres hello.</span><span class="sxs-lookup"><span data-stu-id="6145a-117">**RDMA network address space** - hello RDMA network in Azure reserves hello address space 172.16.0.0/16.</span></span> <span data-ttu-id="6145a-118">aplikací MPI toorun v instancích nasazený ve virtuální síť Azure, ujistěte se, že adresního prostoru virtuální sítě hello nepřekrývá hello RDMA sítě.</span><span class="sxs-lookup"><span data-stu-id="6145a-118">toorun MPI applications on instances deployed in an Azure virtual network, make sure that hello virtual network address space does not overlap hello RDMA network.</span></span>

* <span data-ttu-id="6145a-119">**Rozšíření virtuálního počítače HpcVmDrivers** -na podporu rdma virtuálních počítačích, musíte přidat hello HpcVmDrivers rozšíření tooinstall síťové ovladače zařízení systému Windows pro připojení RDMA.</span><span class="sxs-lookup"><span data-stu-id="6145a-119">**HpcVmDrivers VM extension** - On RDMA-capable VMs, you must add hello HpcVmDrivers extension tooinstall Windows network device drivers for RDMA connectivity.</span></span> <span data-ttu-id="6145a-120">(V určitých nasazeních A8 a A9 instancí hello HpcVmDrivers rozšíření se přidá automaticky.) tooadd hello virtuálního počítače rozšíření tooa virtuálních počítačů, můžete použít [prostředí Azure PowerShell](/powershell/azure/overview) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6145a-120">(In certain deployments of A8 and A9 instances, hello HpcVmDrivers extension is added automatically.) tooadd hello VM extension tooa VM, you can use [Azure PowerShell](/powershell/azure/overview) cmdlets.</span></span> 

  
  <span data-ttu-id="6145a-121">Dobrý den, následující příkaz nainstaluje hello nejnovější verze 1.1 HpcVMDrivers rozšíření na existující podporující RDMA virtuální počítač s názvem *Můjvp* nasazené v hello skupinu prostředků s názvem *myResourceGroup* v hello  *Západní USA* oblasti:</span><span class="sxs-lookup"><span data-stu-id="6145a-121">hello following command installs hello latest version 1.1 HpcVMDrivers extension on an existing RDMA-capable VM named *myVM* deployed in hello resource group named *myResourceGroup* in hello *West US* region:</span></span>

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  <span data-ttu-id="6145a-122">Další informace najdete v tématu [rozšíření virtuálního počítače a funkce](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6145a-122">For more information, see [Virtual machine extensions and features](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="6145a-123">Můžete také pracovat se rozšíření pro virtuální počítače nasazené v hello [modelu nasazení classic](classic/manage-extensions.md).</span><span class="sxs-lookup"><span data-stu-id="6145a-123">You can also work with extensions for VMs deployed in hello [classic deployment model](classic/manage-extensions.md).</span></span>


## <a name="using-hpc-pack"></a><span data-ttu-id="6145a-124">Pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="6145a-124">Using HPC Pack</span></span>

<span data-ttu-id="6145a-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností pro toocreate výpočetní cluster aplikací MPI systému Windows Azure toorun a další úlohy HPC.</span><span class="sxs-lookup"><span data-stu-id="6145a-125">[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toocreate a compute cluster in Azure toorun Windows-based MPI applications and other HPC workloads.</span></span> <span data-ttu-id="6145a-126">HPC Pack 2012 R2 a novější verze zahrnují běhového prostředí pro MS-MPI využívající síť Azure RDMA hello při nasazení na virtuálních počítačích RDMA podporovat.</span><span class="sxs-lookup"><span data-stu-id="6145a-126">HPC Pack 2012 R2 and later versions include a runtime environment for MS-MPI that uses hello Azure RDMA network when deployed on RDMA-capable VMs.</span></span>




## <a name="other-sizes"></a><span data-ttu-id="6145a-127">Další velikosti</span><span class="sxs-lookup"><span data-stu-id="6145a-127">Other sizes</span></span>
- [<span data-ttu-id="6145a-128">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="6145a-128">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="6145a-129">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="6145a-129">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="6145a-130">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="6145a-130">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="6145a-131">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="6145a-131">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="6145a-132">Optimalizované z hlediska GPU</span><span class="sxs-lookup"><span data-stu-id="6145a-132">GPU optimized</span></span>](sizes-gpu.md)

## <a name="next-steps"></a><span data-ttu-id="6145a-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6145a-133">Next steps</span></span>

- <span data-ttu-id="6145a-134">Kontrolní seznamy toouse hello náročné instancí pomocí sady HPC Pack v systému Windows Server, najdete v části [nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun HPC Pack](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6145a-134">For checklists toouse hello compute-intensive instances with HPC Pack on Windows Server, see [Set up a Windows RDMA cluster with HPC Pack toorun MPI applications](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="6145a-135">toouse náročné instancí při spuštění aplikací MPI pomocí služby Azure Batch, najdete v části [použití s více instancemi úloh toorun aplikací rozhraní MPI (Message Passing) v Azure Batch](../../batch/batch-mpi.md).</span><span class="sxs-lookup"><span data-stu-id="6145a-135">toouse compute-intensive instances when running MPI applications with Azure Batch, see [Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Azure Batch](../../batch/batch-mpi.md).</span></span>

- <span data-ttu-id="6145a-136">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="6145a-136">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




