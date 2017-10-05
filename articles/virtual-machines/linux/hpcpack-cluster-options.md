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
# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Možnosti HPC Pack vytvářet a spravovat cluster prostředí HPC v Azure pro Linux úlohy
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tento článek se zaměřuje na možnosti, které se použije ke spuštění úlohy Linux HPC Pack. Existují také možnosti pro spuštění [úloh Windows HPC s HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Provozování clusteru HPC Pack ve virtuálních počítačích Azure
### <a name="azure-templates"></a>Šablony Azure
* (Githubu) [Šablony clusteru HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [Clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Rychlý start) [Vytvoření clusteru prostředí HPC s Linux výpočetních uzlů](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>Skript nasazení prostředí PowerShell
* [Vytvoření clusteru Linux HPC pomocí skriptu pro nasazení HPC Pack IaaS](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Kurzy
* [Kurz: Začínáme s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: Spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Správa clusteru
* [Odesílání úloh do clusteru HPC Pack v Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Správa úloh v prostředí HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Vytvoření clusterů RDMA pro úlohy MPI
* [Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

