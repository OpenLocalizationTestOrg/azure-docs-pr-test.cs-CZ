---
title: "Možnosti clusteru HPC Pack aaaLinux v cloudu hello | Microsoft Docs"
description: "Další informace o možnosti sady Microsoft HPC Pack toocreate a spravovat Linux vysokovýkonného výpočetního prostředí (HPC) clusteru v hello cloudu Azure"
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
ms.openlocfilehash: 60d093b466f44a0391815842c421c328e8c7a0d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="options-with-hpc-pack-toocreate-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Možnosti s HPC Pack toocreate a spravovat cluster prostředí HPC v Azure pro Linux úlohy
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tento článek se zaměřuje na možnosti toouse HPC Pack toorun Linux úlohy. Existují také možnosti pro spuštění [úloh Windows HPC s HPC Pack](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Provozování clusteru HPC Pack ve virtuálních počítačích Azure
### <a name="azure-templates"></a>Šablony Azure
* (Githubu) [Šablony clusteru HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [Clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Rychlý start) [Vytvoření clusteru prostředí HPC s Linux výpočetních uzlů](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script"></a>Skript nasazení prostředí PowerShell
* [Vytvoření clusteru Linux HPC s hello skript nasazení HPC Pack IaaS](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Kurzy
* [Kurz: Začínáme s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: Spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Kurz: Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Správa clusteru
* [Odeslání clusteru HPC Pack tooan úlohy v Azure](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Správa úloh v prostředí HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Vytvoření clusterů RDMA pro úlohy MPI
* [Kurz: OpenFOAM spustit pomocí sady Microsoft HPC Pack na clusteru s podporou Linux RDMA v Azure](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

