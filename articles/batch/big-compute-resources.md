---
title: "Prostředky pro batch a prostředí HPC v cloudu Azure | Microsoft Docs"
description: "Uvádí technické zdroje pro pomoc při spuštění vaší rozsáhlé paralelní, batch a vysokovýkonného výpočetního prostředí (HPC) úlohy v Azure."
services: batch, cloud-services, virtual-machines
documentationcenter: 
author: dlepow
manager: timlt
editor: 
ms.assetid: 6f8be911-c841-41ae-88d3-3bcfc029eb7f
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: big-compute
ms.date: 03/17/2017
ms.author: danlep
ms.openlocfilehash: 18be9f503b57117a7e8f5f0a4e9c93614cc7755b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a>Velké výpočetní v Azure: technických prostředcích pro batch a vysoce výkonné výpočty
Toto je Průvodce technické zdroje pro pomoc při spuštění vaše rozsáhlé paralelní, batch a vysoce výkonné výpočty (HPC) úlohy v Azure. Rozšířit existující batch nebo úlohy v prostředí HPC do cloudu Azure nebo vytvářet nové řešení Big Compute pomocí služby rozsah Azure.

## <a name="solutions-options"></a>Možnosti řešení
Další informace o možnosti Big Compute v Azure a vyberte správný přístup pro vaše úlohy a obchodní potřeby.

* [Řešení pro batch a HPC](batch-hpc-solutions.md)
* [Video: Big Compute v cloudu pomocí Azure a prostředí HPC](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a>Azure Batch
[Batch](https://azure.microsoft.com/services/batch/) je služba platformy, která usnadňuje povolení cloudové aplikace Linux a Windows a spusťte úlohy bez nastavení a správu clusteru a úlohy plánovače. Použití sady SDK k integraci klientských aplikací s Azure Batch pomocí různých jazyků, fáze dat do Azure a sestavte kanály provádění úlohy.

* [Dokumentace](https://azure.microsoft.com/documentation/services/batch/)
* [Rozhraní .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), a [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) referenční dokumentace rozhraní API
* [Knihovny batch management .NET](https://msdn.microsoft.com/library/mt463120.aspx) odkaz
* Kurzy: Začínáme s [Azure Batch library pro .NET](batch-dotnet-get-started.md) a [klientem Batch Python](batch-python-tutorial.md)
* [Fórum batch](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [Batch videa](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a>Řešení clusteru prostředí HPC
Nasaďte, nebo rozšířit existující cluster Windows nebo Linux HPC k Azure a spustit výpočetní zatížení s intenzivním.  

### <a name="microsoft-hpc-pack"></a>Sady Microsoft HPC Pack
HPC Pack je společnosti Microsoft volné HPC řešení založen na technologiích Microsoft Azure a Windows Server, schopný spustit Windows a Linux HPC úlohy.  

* [Stažení sady HPC Pack 2016](https://www.microsoft.com/download/details.aspx?id=54507)
* [Stáhnout prostředí HPC Pack 2012 R2 s aktualizací 3](https://www.microsoft.com/download/details.aspx?id=49922)
* [Dokumentace](https://technet.microsoft.com/library/jj899572.aspx)
* Možnosti clusteru HPC Pack v Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 
* [Rozšíření do instancí pracovního procesu systému Azure pomocí sady HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)
* [Rozšíření do Azure Batch pomocí sady HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)
* [Fóra Windows HPC](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a>Řešení clusteru pro Linux a operačních systémů
Použijte tyto šablony Azure k nasazení clusterů Linux HPC.

* [Začne pracovat SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) a [příspěvku na blogu](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)
* [Číselníku Torque clusteru](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [Výpočetní mřížky šablony s PBS Professional](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a>HPC úložiště
* [Systémy souborů paralelní HPC úložiště v Azure](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [Intel cloudu Edition pro počítače s Lustre Software – zkušební verze](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [BeeGFS v šabloně CentOS 7.2](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a>Microsoft MPI
[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) je standardní rozhraní předávání zpráv pro vývoj a spouštění paralelní aplikace na platformě Windows implementace společnosti Microsoft.

* [Stáhněte si MS MPI](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [MS-MPI odkaz](https://msdn.microsoft.com/library/dn473458.aspx)
* [Fórum MPI](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a>Instance náročné na výpočetní prostředí
Azure nabízí [velikosti rozsah virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), včetně [náročné H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instancí, které lze připojit k back-end RDMA síť, ke spuštění úlohy Linux a Windows HPC. 

* [Nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Nastavení clusteru s podporou Windows RDMA pomocí sady Microsoft HPC Pack ke spouštění aplikací MPI](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

Pro úlohy náročné na grafický procesor, podívejte se na [velikosti NC a vs](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).

## <a name="samples-and-demos"></a>Ukázky a ukázky
* [Ukázky kódu Azure Batch C# a Python](https://github.com/Azure/azure-batch-samples)
* [Batch loděnice](https://azure.github.io/batch-shipyard/) nástrojů pro snadné nasazení stylu batch Dockerized úloh do Azure Batch
* [doAzureParallel](http://www.github.com/Azure/doAzureParallel) balíček R, postavená na Azure Batch
* [Vyzkoušejte SUSE Linux Enterprise Server pro HPC](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a>Související služby Azure
* [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)
* [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)
* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Škálovací sady virtuálních počítačů](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/)
* [App Service](https://azure.microsoft.com/documentation/services/app-service/)
* [Media Services](https://azure.microsoft.com/documentation/services/media-services/)
* [Functions](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a>Plány architektury
* [Orchestration HPC a data pomocí Azure Batch a s Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) a [článku](../data-factory/data-factory-data-processing-using-batch.md)

## <a name="industry-solutions"></a>Odvětví řešení
* [Bankovnictví a kapitálové trzích](https://finance.azure.com/)
* [Simulace inženýrství](https://simulation.azure.com/) 

## <a name="customer-stories"></a>Příběhy zákazníků
* [ANEO](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [d3View](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [Ludwig Institute výzkumu rakoviny](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [Microsoft Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [Milliman](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [Mezinárodní cenné Mitsubishi UFJ](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [UberCloud](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a>Další kroky
* Podívejte se na aktuální novinky na [blogu týmu pro Microsoft HPC a Batch](http://blogs.technet.com/b/windowshpc/) a [blogu Azure](https://azure.microsoft.com/blog/tag/hpc/).
* Viz také [co je nového ve službě Batch](https://azure.microsoft.com/updates/?service=batch) nebo přihlášení k odběru [informačního kanálu RSS](https://azure.microsoft.com/updates/feed/?service=batch).

