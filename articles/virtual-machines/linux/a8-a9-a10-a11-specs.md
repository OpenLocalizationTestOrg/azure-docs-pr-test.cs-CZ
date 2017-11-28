---
title: "aaaAbout náročné virtuální počítače s Linuxem | Microsoft Docs"
description: "Získejte základní informace a požadavky pro používání hello H-series a A8, A9, A10 a A11 náročných velikostí pro virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 16cf6e2d-f401-4b22-af8c-9a337179f5f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 37636840a3f809ac19354a5a7993257216f675f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a><span data-ttu-id="c9e2a-103">O H-series a náročné virtuální počítače A-series pro Linux</span><span class="sxs-lookup"><span data-stu-id="c9e2a-103">About H-series and compute-intensive A-series VMs for Linux</span></span>
<span data-ttu-id="c9e2a-104">Tady je základní informace a některé informace o použití hello novější Azure H-series a hello starší A8, A9, A10 a A11 velikostí, také známé jako *náročné* instance.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-104">Here is background information and some considerations for using hello newer Azure H-series and hello earlier A8, A9, A10, and A11 sizes, also known as *compute-intensive* instances.</span></span> <span data-ttu-id="c9e2a-105">Tento článek se zaměřuje na používání těchto velikostí pro virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-105">This article focuses on using these sizes for Linux VMs.</span></span> <span data-ttu-id="c9e2a-106">Můžete také použít, je [virtuálních počítačů Windows](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-106">You can also use them for [Windows VMs](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="c9e2a-107">Základní specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-107">For basic specs, storage capacities, and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-toohello-rdma-network"></a><span data-ttu-id="c9e2a-108">Přístup k toohello RDMA sítě</span><span class="sxs-lookup"><span data-stu-id="c9e2a-108">Access toohello RDMA network</span></span>
<span data-ttu-id="c9e2a-109">Můžete vytvořit clustery s podporou RDMA virtuálních počítačů Linux, který spusťte jeden z následujících podporovaných distribucích systému Linux HPC a podporovaných využít tootake implementace MPI sítě Azure RDMA hello hello.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-109">You can create clusters of RDMA-capable Linux VMs that run one of hello following supported Linux HPC distributions and a supported MPI implementation tootake advantage of hello Azure RDMA network.</span></span> <span data-ttu-id="c9e2a-110">V tématu [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) pro možnosti nasazení a ukázkové kroky konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-110">See [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) for deployment options and sample configuration steps.</span></span>

* <span data-ttu-id="c9e2a-111">**Distribuce** – je nutné nasadit virtuální počítače z podporující RDMA SUSE Linux Enterprise Server (SLES) nebo hello podvodný Wave softwaru (dříve OpenLogic) na základě CentOS HPC obrázků v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-111">**Distributions** - You must deploy VMs from RDMA-capable SUSE Linux Enterprise Server (SLES) or Rogue Wave Software (formerly OpenLogic) CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="c9e2a-112">Hello následující obrázky Marketplace podpory RDMA připojení:</span><span class="sxs-lookup"><span data-stu-id="c9e2a-112">hello following Marketplace images support RDMA connectivity:</span></span>
  
    * <span data-ttu-id="c9e2a-113">SLES 12 SP1 pro prostředí HPC, SLES 12 SP1 pro HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="c9e2a-113">SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium)</span></span>
    
    * <span data-ttu-id="c9e2a-114">Na základě centOS 7.1 HPC, na základě CentOS verze 6.5 HPC</span><span class="sxs-lookup"><span data-stu-id="c9e2a-114">CentOS-based 7.1 HPC, CentOS-based 6.5 HPC</span></span>  
 
        > [!NOTE]
        > <span data-ttu-id="c9e2a-115">Pro virtuální počítače H-series doporučujeme, abyste buď SLES 12 SP1 pro bitovou kopii prostředí HPC, nebo na základě CentOS 7.1 HPC image.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-115">For H-series VMs, we recommend either a SLES 12 SP1 for HPC image or CentOS-based 7.1 HPC image.</span></span>
        >
        > <span data-ttu-id="c9e2a-116">Na hello Image na základě CentOS HPC, jsou zakázané jádra aktualizace v hello **yum** konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-116">On hello CentOS-based HPC images, kernel updates are disabled in hello **yum** configuration file.</span></span> <span data-ttu-id="c9e2a-117">Je to proto hello Linux RDMA ovladače distribuují jako balíček RPM a aktualizací ovladače nemusí fungovat, pokud se aktualizuje hello jádra.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-117">This is because hello Linux RDMA drivers are distributed as an RPM package, and driver updates might not work if hello kernel is updated.</span></span>
        > 
        > 
* <span data-ttu-id="c9e2a-118">**MPI** -Intel MPI knihovny 5.x</span><span class="sxs-lookup"><span data-stu-id="c9e2a-118">**MPI** - Intel MPI Library 5.x</span></span>
  
    <span data-ttu-id="c9e2a-119">V závislosti na hello Marketplace image si zvolíte, samostatnou licencování, instalaci, nebo konfigurace Intel MPI může být potřeba, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c9e2a-119">Depending on hello Marketplace image you choose, separate licensing, installation, or configuration of Intel MPI may be needed, as follows:</span></span> 
  
  * <span data-ttu-id="c9e2a-120">**SLES 12 SP1 pro bitovou kopii prostředí HPC** -Intel MPI balíčky jsou distribuovány na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-120">**SLES 12 SP1 for HPC image** - Intel MPI packages are distributed on hello VM.</span></span> <span data-ttu-id="c9e2a-121">Nainstalujte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c9e2a-121">Install by running hello following command:</span></span>

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * <span data-ttu-id="c9e2a-122">**Na základě centOS Image HPC** -Intel MPI 5.1 je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-122">**CentOS-based HPC images**  - Intel MPI 5.1 is already installed.</span></span>  
    
    <span data-ttu-id="c9e2a-123">Konfigurace dalších systému je potřebné toorun úloh MPI na clusterových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-123">Additional system configuration is needed toorun MPI jobs on clustered VMs.</span></span> <span data-ttu-id="c9e2a-124">Například v clusteru virtuálních počítačů, je nutné tooestablish vztah důvěryhodnosti mezi hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-124">For example, on a cluster of VMs, you need tooestablish trust among hello compute nodes.</span></span> <span data-ttu-id="c9e2a-125">Typické nastavení, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-125">For typical settings, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="considerations-for-hpc-pack-and-linux"></a><span data-ttu-id="c9e2a-126">Důležité informace týkající se sady HPC Pack a Linux</span><span class="sxs-lookup"><span data-stu-id="c9e2a-126">Considerations for HPC Pack and Linux</span></span>
<span data-ttu-id="c9e2a-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), řešení společnosti Microsoft volné HPC clusteru a úlohy správy, poskytuje jednu možnost toouse hello náročné instancí operačního systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-127">[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft’s free HPC cluster and job management solution, provides one option for you toouse hello compute-intensive instances with Linux.</span></span> <span data-ttu-id="c9e2a-128">nejnovější verze sady HPC Pack Hello podporovat několik Linuxových distribucích toorun na výpočetních uzlech nasazené ve virtuálních počítačích Azure, spravuje hlavního uzlu systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-128">hello latest releases of HPC Pack support several Linux distributions toorun on compute nodes deployed in Azure VMs, managed by a Windows Server head node.</span></span> <span data-ttu-id="c9e2a-129">S podporou RDMA Linux výpočetní uzly systémem Intel MPI HPC Pack můžete naplánovat a spustit aplikací Linux MPI tuto síť RDMA hello přístup.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-129">With RDMA-capable Linux compute nodes running Intel MPI, HPC Pack can schedule and run Linux MPI applications that access hello RDMA network.</span></span> <span data-ttu-id="c9e2a-130">tooget začít, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-130">tooget started, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="network-topology-considerations"></a><span data-ttu-id="c9e2a-131">Aspekty topologie sítě</span><span class="sxs-lookup"><span data-stu-id="c9e2a-131">Network topology considerations</span></span>
* <span data-ttu-id="c9e2a-132">Na podporou RDMA virtuální počítače s Linuxem v Azure je Eth1 vyhrazený pro RDMA síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-132">On RDMA-enabled Linux VMs in Azure, Eth1 is reserved for RDMA network traffic.</span></span> <span data-ttu-id="c9e2a-133">Neměňte nastavení Eth1 nebo všechny informace v konfiguračním souboru hello odkazující toothis sítě.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-133">Do not change any Eth1 settings or any information in hello configuration file referring toothis network.</span></span> <span data-ttu-id="c9e2a-134">Eth0 je vyhrazený pro regulární Azure síťový provoz.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-134">Eth0 is reserved for regular Azure network traffic.</span></span>
* <span data-ttu-id="c9e2a-135">V Azure není podporována IP přes InfiniBand (IB).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-135">In Azure, IP over InfiniBand (IB) is not supported.</span></span> <span data-ttu-id="c9e2a-136">Je podporován pouze RDMA přes IB.</span><span class="sxs-lookup"><span data-stu-id="c9e2a-136">Only RDMA over IB is supported.</span></span>



## <a name="next-steps"></a><span data-ttu-id="c9e2a-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c9e2a-137">Next steps</span></span>
* <span data-ttu-id="c9e2a-138">Podrobnosti o dostupnosti a ceny velikostí náročné hello najdete v tématu [ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-138">For details about availability and pricing of hello compute-intensive sizes, see [Virtual Machines pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span>
* <span data-ttu-id="c9e2a-139">Podrobnosti o disku a kapacity úložiště najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-139">For storage capacities and disk details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="c9e2a-140">tooget spuštění nasazení a používání náročné velikosti s RDMA pro systémy Linux, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c9e2a-140">tooget started deploying and using compute-intensive sizes with RDMA on Linux, see [Set up a Linux RDMA cluster toorun MPI applications](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

