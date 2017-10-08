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
# <a name="options-with-hpc-pack-toocreate-and-manage-a-windows-hpc-cluster-in-azure"></a>Možnosti s HPC Pack toocreate a spravovat cluster Windows HPC v Azure
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tento článek se zaměřuje na možnosti toocreate HPC Pack úlohy Windows toorun clustery. Existují také možnosti pro vytváření HPC Pack clusterů toorun [úloh prostředí HPC Linux](../linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Provozování clusteru HPC Pack ve virtuálních počítačích Azure
### <a name="azure-templates"></a>Šablony Azure
* (Githubu) [Šablony clusteru HPC Pack 2016](https://github.com/MsHpcPack/HPCPack2016)
* (Marketplace) [HPC Pack clusteru pro úlohy Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)
* (Marketplace) [Clusteru HPC Pack pro aplikaci Excel úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)
* (Rychlý start) [Vytvoření clusteru prostředí HPC](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)
* (Rychlý start) [Vytvoření clusteru prostředí HPC s bitovou kopií vlastní výpočetního uzlu](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure Image virtuálních počítačů
* [HPC Pack 2016 hlavního uzlu na Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016 výpočetním uzlu na Windows Server 2016](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2016?tab=Overview)
* [HPC Pack 2016 hlavního uzlu v systému Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016HeadNodeonWindowsServer2012R2?tab=Overview)
* [HPC Pack 2016 výpočetní uzel v systému Windows Server 2012 R2](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.HPCPack2016ComputeNodeonWindowsServer2012R2?tab=Overview)
* [Hlavního uzlu HPC Pack 2012 R2 na Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)
* [Výpočetním uzlu HPC Pack 2012 R2 na Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)
* [HPC Pack výpočetního uzlu pomocí aplikace Excel v systému Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### <a name="powershell-deployment-script"></a>Skript nasazení prostředí PowerShell
* [Vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Kurzy
* [Kurz: Nasazení clusteru HPC Pack 2016 v Azure](hpcpack-2016-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Kurz: Začínáme s clusteru služby HPC Pack v Azure toorun aplikace Excel a SOA úlohami](excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="manual-deployment-with-hello-azure-portal"></a>Ruční nasazení s hello portálu Azure
* [Nastavit hello hlavního uzlu clusteru HPC Pack virtuální počítač Azure](hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="cluster-management"></a>Správa clusteru
* [Spravovat výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster-node-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Zvětšovat a zmenšovat Azure výpočetních prostředků v clusteru služby HPC Pack](classic/hpcpack-cluster-node-autogrowshrink.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Odeslání clusteru HPC Pack tooan úlohy v Azure](hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Správa úloh v prostředí HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)
* [Spravovat cluster služby HPC Pack v Azure s Azure Active Directory](hpcpack-cluster-active-directory.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="add-worker-role-nodes-tooan-hpc-pack-cluster"></a>Přidat pracovní role uzly tooan HPC Pack clusteru
* [Rozšíření tooAzure instancí pracovního procesu pomocí sady HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [Kurz: Nastavení clusteru hybridní pomocí sady HPC Pack v Azure](../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)
* [Přidání Azure "rozšíření" uzly tooan HPC Pack hlavního uzlu v Azure](classic/hpcpack-cluster-node-burst.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="integrate-with-azure-batch"></a>Integraci se službou Azure Batch
* [Shluků tooAzure Batch pomocí sady HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Vytvoření clusterů RDMA pro úlohy MPI
* [Nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun HPC Pack](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

