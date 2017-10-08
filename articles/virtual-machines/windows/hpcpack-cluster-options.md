---
title: "Možnosti v cloudu hello clusteru HPC Pack aaaWindows | Microsoft Docs"
description: "Další informace o možnosti sady Microsoft HPC Pack toocreate a spravovat Windows vysokovýkonného výpočetního prostředí (HPC) clusteru v hello cloudu Azure"
services: virtual-machines-windows,cloud-services,batch
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 02c5566d-2129-483c-9ecf-0d61030442d7
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/06/2017
ms.author: danlep
ms.openlocfilehash: 6158d9dd0cecda38b14f85a2b2080163f18e8cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a><span data-ttu-id="c3f53-103">Možnosti s HPC Pack toocreate a spravovat cluster Windows HPC v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-103">Options with HPC Pack toocreate and manage a Windows HPC cluster in Azure</span></span>
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

<span data-ttu-id="c3f53-104">Tento článek se zaměřuje na možnosti toocreate HPC Pack úlohy Windows toorun clustery.</span><span class="sxs-lookup"><span data-stu-id="c3f53-104">This article focuses on options toocreate HPC Pack clusters toorun Windows workloads.</span></span> <span data-ttu-id="c3f53-105">Existují také možnosti pro vytváření HPC Pack clusterů toorun [úloh prostředí HPC Linux](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c3f53-105">There are also options for creating HPC Pack clusters toorun [Linux HPC workloads](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a><span data-ttu-id="c3f53-106">Provozování clusteru HPC Pack ve virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-106">Run an HPC Pack cluster in Azure VMs</span></span>
### <a name="azure-templates"></a><span data-ttu-id="c3f53-107">Šablony Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-107">Azure templates</span></span>
* <span data-ttu-id="c3f53-108">(Githubu) [Šablony clusteru HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016)</span><span class="sxs-lookup"><span data-stu-id="c3f53-108">(GitHub) [HPC Pack 2016 cluster templates](https://github.com/MsHpcPack/HPCPack2016)</span></span>
* <span data-ttu-id="c3f53-109">(Marketplace) [HPC Pack clusteru pro úlohy Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span><span class="sxs-lookup"><span data-stu-id="c3f53-109">(Marketplace) [HPC Pack cluster for Windows workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)</span></span>
* <span data-ttu-id="c3f53-110">(Marketplace) [Clusteru HPC Pack pro aplikaci Excel úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span><span class="sxs-lookup"><span data-stu-id="c3f53-110">(Marketplace) [HPC Pack cluster for Excel workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)</span></span>
* <span data-ttu-id="c3f53-111">(Rychlý start) [Vytvoření clusteru prostředí HPC](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span><span class="sxs-lookup"><span data-stu-id="c3f53-111">(Quickstart) [Create an HPC cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)</span></span>
* <span data-ttu-id="c3f53-112">(Rychlý start) [Vytvoření clusteru prostředí HPC s bitovou kopií vlastní výpočetního uzlu](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span><span class="sxs-lookup"><span data-stu-id="c3f53-112">(Quickstart) [Create an HPC cluster with custom compute node image](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)</span></span>

### <a name="azure-vm-images"></a><span data-ttu-id="c3f53-113">Azure Image virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="c3f53-113">Azure VM images</span></span>
* [<span data-ttu-id="c3f53-114">HPC Pack 2016 hlavního uzlu na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="c3f53-114">HPC Pack 2016 head node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="c3f53-115">HPC Pack 2016 výpočetním uzlu na Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="c3f53-115">HPC Pack 2016 compute node on Windows Server 2016</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [<span data-ttu-id="c3f53-116">HPC Pack 2016 hlavního uzlu v systému Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c3f53-116">HPC Pack 2016 head node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="c3f53-117">HPC Pack 2016 výpočetní uzel v systému Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c3f53-117">HPC Pack 2016 compute node on Windows Server 2012 R2</span></span>](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [<span data-ttu-id="c3f53-118">Hlavního uzlu HPC Pack 2012 R2 na Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c3f53-118">HPC Pack 2012 R2 head node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [<span data-ttu-id="c3f53-119">Výpočetním uzlu HPC Pack 2012 R2 na Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c3f53-119">HPC Pack 2012 R2 compute node on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [<span data-ttu-id="c3f53-120">HPC Pack výpočetního uzlu pomocí aplikace Excel v systému Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="c3f53-120">HPC Pack compute node with Excel on Windows Server 2012 R2</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a><span data-ttu-id="c3f53-121">Skript nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c3f53-121">PowerShell deployment script</span></span>
* [<span data-ttu-id="c3f53-122">Vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS</span><span class="sxs-lookup"><span data-stu-id="c3f53-122">Create an HPC cluster with hello HPC Pack IaaS deployment script</span></span>](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a><span data-ttu-id="c3f53-123">Kurzy</span><span class="sxs-lookup"><span data-stu-id="c3f53-123">Tutorials</span></span>
* [<span data-ttu-id="c3f53-124">Kurz: Nasazení clusteru HPC Pack 2016 v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-124">Tutorial: Deploy an HPC Pack 2016 cluster in Azure</span></span>](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c3f53-125">Kurz: Začínáme s clusteru služby HPC Pack v Azure toorun aplikace Excel a SOA úlohami</span><span class="sxs-lookup"><span data-stu-id="c3f53-125">Tutorial: Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads</span></span>](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a><span data-ttu-id="c3f53-126">Ruční nasazení s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-126">Manual deployment with hello Azure portal</span></span>
* [<span data-ttu-id="c3f53-127">Nastavit hello hlavního uzlu clusteru HPC Pack virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-127">Set up hello head node of an HPC Pack cluster in an Azure VM</span></span>](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a><span data-ttu-id="c3f53-128">Správa clusteru</span><span class="sxs-lookup"><span data-stu-id="c3f53-128">Cluster management</span></span>
* [<span data-ttu-id="c3f53-129">Spravovat výpočetní uzly v clusteru služby HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-129">Manage compute nodes in an HPC Pack cluster in Azure</span></span>](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="c3f53-130">Zvětšovat a zmenšovat Azure výpočetních prostředků v clusteru služby HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c3f53-130">Grow and shrink Azure compute resources in an HPC Pack cluster</span></span>](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="c3f53-131">Odeslání clusteru HPC Pack tooan úlohy v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-131">Submit jobs tooan HPC Pack cluster in Azure</span></span>](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c3f53-132">Správa úloh v prostředí HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c3f53-132">Job management in HPC Pack</span></span>](https://technet.microsoft.com/library/jj899585.aspx)
* [<span data-ttu-id="c3f53-133">Spravovat cluster služby HPC Pack v Azure s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c3f53-133">Manage an HPC Pack cluster in Azure with Azure Active Directory</span></span>](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a><span data-ttu-id="c3f53-134">Přidat pracovní role uzly tooan HPC Pack clusteru</span><span class="sxs-lookup"><span data-stu-id="c3f53-134">Add worker role nodes tooan HPC Pack cluster</span></span>
* [<span data-ttu-id="c3f53-135">Rozšíření tooAzure instancí pracovního procesu pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c3f53-135">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="c3f53-136">Kurz: Nastavení clusteru hybridní pomocí sady HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-136">Tutorial: Set up a hybrid cluster with HPC Pack in Azure</span></span>](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [<span data-ttu-id="c3f53-137">Přidání Azure "rozšíření" uzly tooan HPC Pack hlavního uzlu v Azure</span><span class="sxs-lookup"><span data-stu-id="c3f53-137">Add Azure "burst" nodes tooan HPC Pack head node in Azure</span></span>](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a><span data-ttu-id="c3f53-138">Integraci se službou Azure Batch</span><span class="sxs-lookup"><span data-stu-id="c3f53-138">Integrate with Azure Batch</span></span>
* [<span data-ttu-id="c3f53-139">Shluků tooAzure Batch pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c3f53-139">Burst tooAzure Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a><span data-ttu-id="c3f53-140">Vytvoření clusterů RDMA pro úlohy MPI</span><span class="sxs-lookup"><span data-stu-id="c3f53-140">Create RDMA clusters for MPI workloads</span></span>
* [<span data-ttu-id="c3f53-141">Nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c3f53-141">Set up a Windows RDMA cluster with HPC Pack toorun MPI applications</span></span>](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

