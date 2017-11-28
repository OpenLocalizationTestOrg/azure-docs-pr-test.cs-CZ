---
title: "virtuální počítač s Linuxem aaaAzure velikosti - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro Linux s vysokým výkonem virtuálních počítačů v Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 0b02ee592902ff8097a71898faea1ac17a947d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a><span data-ttu-id="a92c3-103">Vysoký výkon výpočetní velikosti virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="a92c3-103">High performance compute Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a><span data-ttu-id="a92c3-104">RDMA podporovat instancí</span><span class="sxs-lookup"><span data-stu-id="a92c3-104">RDMA-capable instances</span></span>
<span data-ttu-id="a92c3-105">Podmnožinu hello náročné instancí (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="a92c3-105">A subset of hello compute-intensive instances (H16r, H16mr, A8, and A9) feature a network interface for remote direct memory access (RDMA) connectivity.</span></span> <span data-ttu-id="a92c3-106">Toto rozhraní je kromě velikosti virtuálních počítačů pro dostupné tooother toohello standardního Azure síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a92c3-106">This interface is in addition toohello standard Azure network interface available tooother VM sizes.</span></span> 
  
<span data-ttu-id="a92c3-107">Toto rozhraní umožňuje hello podporující RDMA instance toocommunicate přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a92c3-107">This interface allows hello RDMA-capable instances toocommunicate over an InfiniBand network, operating at FDR rates for H16r and H16mr virtual machines, and QDR rates for A8 and A9 virtual machines.</span></span> <span data-ttu-id="a92c3-108">Tyto funkce RDMA může zvýšit hello škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).</span><span class="sxs-lookup"><span data-stu-id="a92c3-108">These RDMA capabilities can boost hello scalability and performance of Message Passing Interface (MPI) applications.</span></span>

<span data-ttu-id="a92c3-109">Toto jsou požadavky pro virtuální počítače s podporou RDMA Linuxem tooaccess hello Azure RDMA sítě:</span><span class="sxs-lookup"><span data-stu-id="a92c3-109">Following are requirements for RDMA-capable Linux VMs tooaccess hello Azure RDMA network:</span></span>
 
* <span data-ttu-id="a92c3-110">**Distribuce** – je nutné nasadit virtuální počítače z podporující RDMA SUSE Linux Enterprise Server (SLES) nebo hello podvodný Wave softwaru (dříve OpenLogic) na základě CentOS HPC obrázků v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a92c3-110">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="a92c3-111">Hello následující obrázky Marketplace podpory RDMA připojení:</span><span class="sxs-lookup"><span data-stu-id="a92c3-111">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="a92c3-112">SLES 12 SP1 pro prostředí HPC nebo SLES 12 SP1 pro HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="a92c3-112">SLES 12 SP1 for HPC, or SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="a92c3-113">Na základě centOS 7.3 HPC, na základě CentOS 7.1 HPC, na základě CentOS 6.8 HPC nebo na základě CentOS verze 6.5 HPC</span><span class="sxs-lookup"><span data-stu-id="a92c3-113">CentOS-based 7.3 HPC, CentOS-based 7.1 HPC, CentOS-based 6.8 HPC, or CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="a92c3-114">Pro virtuální počítače H-series doporučujeme, abyste SLES 12 SP1 pro bitovou kopii prostředí HPC nebo na základě CentOS 7.1 nebo novější bitovou kopii prostředí HPC.</span><span class="sxs-lookup"><span data-stu-id="a92c3-114">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 or later HPC image.</span></span>
        >
        > <span data-ttu-id="a92c3-115">Na hello Image na základě CentOS HPC, jsou zakázané jádra aktualizace v hello **yum** konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="a92c3-115">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="a92c3-116">Je to proto hello Linux RDMA ovladače distribuují jako balíček RPM a aktualizací ovladače nemusí fungovat, pokud se aktualizuje hello jádra.</span><span class="sxs-lookup"><span data-stu-id="a92c3-116">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="a92c3-117">**MPI** -Intel MPI knihovny 5.x</span><span class="sxs-lookup"><span data-stu-id="a92c3-117">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="a92c3-118">V závislosti na hello Marketplace image si zvolíte, samostatnou licencování, instalaci, nebo konfigurace Intel MPI může být potřeba, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a92c3-118">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="a92c3-119">**SLES 12 SP1 pro bitovou kopii prostředí HPC** -Intel MPI balíčky jsou distribuovány na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a92c3-119">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="a92c3-120">Nainstalujte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a92c3-120">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="a92c3-121">**Na základě centOS Image HPC** -Intel MPI 5.1 je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="a92c3-121">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="a92c3-122">Konfigurace dalších systému je potřebné toorun úloh MPI na clusterových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="a92c3-122">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="a92c3-123">Například v clusteru virtuálních počítačů, je nutné tooestablish vztah důvěryhodnosti mezi hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="a92c3-123">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="a92c3-124">Typické nastavení, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="a92c3-124">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

### <a name="network-topology-considerations"></a><span data-ttu-id="a92c3-125">Aspekty topologie sítě</span><span class="sxs-lookup"><span data-stu-id="a92c3-125">Network topology considerations</span></span>
* <span data-ttu-id="a92c3-126">Na podporou RDMA virtuální počítače s Linuxem v Azure je Eth1 vyhrazený pro RDMA síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="a92c3-126">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="a92c3-127">Neměňte nastavení Eth1 nebo všechny informace v konfiguračním souboru hello odkazující toothis sítě.</span><span class="sxs-lookup"><span data-stu-id="a92c3-127">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="a92c3-128">Eth0 je vyhrazený pro regulární Azure síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="a92c3-128">Eth0 is reserved for regular Azure network traffic.</span></span>

* <span data-ttu-id="a92c3-129">V Azure není podporována IP přes InfiniBand (IB).</span><span class="sxs-lookup"><span data-stu-id="a92c3-129">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="a92c3-130">Je podporován pouze RDMA přes IB.</span><span class="sxs-lookup"><span data-stu-id="a92c3-130">Only RDMA over IB is supported.</span></span>

## <a name="using-hpc-pack"></a><span data-ttu-id="a92c3-131">Pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="a92c3-131">Using HPC Pack</span></span>
<span data-ttu-id="a92c3-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností pro vás toouse hello náročné instancí operačního systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a92c3-132">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, is one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="a92c3-133">nejnovější verze sady HPC Pack Hello podporovat několik Linuxových distribucích toorun na výpočetních uzlech nasazené ve virtuálních počítačích Azure, spravuje hlavního uzlu systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a92c3-133">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="a92c3-134">S podporou RDMA Linux výpočetní uzly systémem Intel MPI HPC Pack můžete naplánovat a spustit aplikací Linux MPI tuto síť RDMA hello přístup.</span><span class="sxs-lookup"><span data-stu-id="a92c3-134">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="a92c3-135">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a92c3-135">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="other-sizes"></a><span data-ttu-id="a92c3-136">Další velikosti</span><span class="sxs-lookup"><span data-stu-id="a92c3-136">Other sizes</span></span>
- [<span data-ttu-id="a92c3-137">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="a92c3-137">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="a92c3-138">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="a92c3-138">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="a92c3-139">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="a92c3-139">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="a92c3-140">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="a92c3-140">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="a92c3-141">GPU</span><span class="sxs-lookup"><span data-stu-id="a92c3-141">GPU</span></span>](../windows/sizes-gpu.md)


## <a name="next-steps"></a><span data-ttu-id="a92c3-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a92c3-142">Next steps</span></span>

- <span data-ttu-id="a92c3-143">tooget spuštění nasazení a používání náročné velikosti s RDMA pro systémy Linux, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a92c3-143">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

- <span data-ttu-id="a92c3-144">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="a92c3-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>




