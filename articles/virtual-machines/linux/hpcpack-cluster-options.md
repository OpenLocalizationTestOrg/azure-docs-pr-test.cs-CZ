---
title: "Možnosti clusteru Linux HPC Pack v cloudu | Microsoft Docs"
description: "Další informace o možnostech pomocí sady Microsoft HPC Pack vytvářet a spravovat Linux vysokovýkonného výpočetního prostředí (HPC) clusteru v cloudu Azure"
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 616006d29149f7f47b03bd127cf3c83ad63a5c39
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a><span data-ttu-id="37429-103">Možnosti HPC Pack vytvářet a spravovat cluster prostředí HPC v Azure pro Linux úlohy</span><span class="sxs-lookup"><span data-stu-id="37429-103">Options with HPC Pack to create and manage an HPC cluster in Azure for Linux workloads</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="37429-104">Tento článek se zaměřuje na možnosti, které se použije ke spuštění úlohy Linux HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="37429-104">This article focuses on options to use HPC Pack to run Linux workloads.</span></span> <span data-ttu-id="37429-105">Existují také možnosti pro spuštění [úloh Windows HPC s HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37429-105">There are also options for running [Windows HPC workloads with HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="37429-106">Provozování clusteru HPC Pack ve virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="37429-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="37429-107">Šablony Azure</span><span class="sxs-lookup"><span data-stu-id="37429-107">Azure templates</span></span>
* <span data-ttu-id="37429-108">(Githubu) [Šablony clusteru HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="37429-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="37429-109">(Marketplace) [Clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span><span class="sxs-lookup"><span data-stu-id="37429-109">(Marketplace) [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)</span></span>
* <span data-ttu-id="37429-110">(Rychlý start) [Vytvoření clusteru prostředí HPC s Linux výpočetních uzlů](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span><span class="sxs-lookup"><span data-stu-id="37429-110">(Quickstart) [Create an HPC cluster with Linux compute nodes](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)</span></span>

### <a name="powershell-deployment-script"></a><span data-ttu-id="37429-111">Skript nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="37429-111">PowerShell deployment script</span></span>
* [<span data-ttu-id="37429-112">Vytvoření clusteru Linux HPC pomocí skriptu pro nasazení HPC Pack IaaS</span><span class="sxs-lookup"><span data-stu-id="37429-112">Create a Linux HPC cluster with the HPC Pack IaaS deployment script</span></span>](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="37429-113">Kurzy</span><span class="sxs-lookup"><span data-stu-id="37429-113">Tutorials</span></span>
* [<span data-ttu-id="37429-114">Kurz: Začínáme s Linux výpočetní uzly v clusteru služby HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-114">Tutorial: Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="37429-115">Kurz: Spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-115">Tutorial: Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="37429-116">Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-116">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="37429-117">Kurz: Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-117">Tutorial: Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="37429-118">Správa clusteru</span><span class="sxs-lookup"><span data-stu-id="37429-118">Cluster management</span></span>
* [<span data-ttu-id="37429-119">Odesílání úloh do clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-119">Submit jobs to an HPC Pack cluster in Azure</span></span>](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="37429-120">Správa úloh v prostředí HPC Pack</span><span class="sxs-lookup"><span data-stu-id="37429-120">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="37429-121">Vytvoření clusterů RDMA pro úlohy MPI</span><span class="sxs-lookup"><span data-stu-id="37429-121">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="37429-122">Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure</span><span class="sxs-lookup"><span data-stu-id="37429-122">Tutorial: Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="37429-123">Nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI</span><span class="sxs-lookup"><span data-stu-id="37429-123">Set up a Linux RDMA cluster to run MPI applications</span></span>](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

