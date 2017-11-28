---
title: "aaaResources pro batch a prostředí HPC v cloudu Azure hello | Microsoft Docs"
description: "Uvádí technické zdroje toohelp spuštění vaší rozsáhlé paralelní, batch a vysokovýkonného výpočetního prostředí (HPC) úlohy v Azure."
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
ms.openlocfilehash: ab8ba24678bd7ec090306b501d29ca63c4fb83ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a><span data-ttu-id="4324f-103">Velké výpočetní v Azure: technických prostředcích pro batch a vysoce výkonné výpočty</span><span class="sxs-lookup"><span data-stu-id="4324f-103">Big Compute in Azure: Technical resources for batch and high-performance computing</span></span>
<span data-ttu-id="4324f-104">Toto je toohelp Průvodce tootechnical prostředky spouštění vaše rozsáhlé paralelní, batch a vysoce výkonné výpočty (HPC) úlohy v Azure.</span><span class="sxs-lookup"><span data-stu-id="4324f-104">This is a guide tootechnical resources toohelp you run your large-scale parallel, batch, and high-performance computing (HPC) workloads in Azure.</span></span> <span data-ttu-id="4324f-105">Rozšířit existující batch nebo toohello úlohy HPC cloudu Azure nebo vytvářet nové řešení Big Compute pomocí služby rozsah Azure.</span><span class="sxs-lookup"><span data-stu-id="4324f-105">Extend your existing batch or HPC workloads toohello Azure cloud, or build new Big Compute solutions using a range of Azure services.</span></span>

## <a name="solutions-options"></a><span data-ttu-id="4324f-106">Možnosti řešení</span><span class="sxs-lookup"><span data-stu-id="4324f-106">Solutions options</span></span>
<span data-ttu-id="4324f-107">Další informace o možnostech Big Compute v Azure a zvolte hello správný přístup pro vaše úlohy a obchodní potřeby.</span><span class="sxs-lookup"><span data-stu-id="4324f-107">Learn about Big Compute options in Azure, and choose hello right approach for your workload and business need.</span></span>

* [<span data-ttu-id="4324f-108">Řešení pro batch a HPC</span><span class="sxs-lookup"><span data-stu-id="4324f-108">Batch and HPC solutions</span></span>](batch-hpc-solutions.md)
* [<span data-ttu-id="4324f-109">Video: Big Compute v hello cloudu s Azure a prostředí HPC</span><span class="sxs-lookup"><span data-stu-id="4324f-109">Video: Big Compute in hello cloud with Azure and HPC</span></span>](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)

## <a name="azure-batch"></a><span data-ttu-id="4324f-110">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="4324f-110">Azure Batch</span></span>
<span data-ttu-id="4324f-111">[Batch](https://azure.microsoft.com/services/batch/) je služba platformy, která umožňuje snadno toocloud povolit vaše aplikace Linux a Windows a spuštění úlohy bez nastavení a správu clusteru a úlohy plánovače.</span><span class="sxs-lookup"><span data-stu-id="4324f-111">[Batch](https://azure.microsoft.com/services/batch/) is a platform service that makes it easy toocloud-enable your Linux and Windows applications and run jobs without setting up and managing a cluster and job scheduler.</span></span> <span data-ttu-id="4324f-112">Používání hello SDK toointegrate klientských aplikací s Azure Batch prostřednictvím různých jazyků, fáze tooAzure dat a sestavte kanály provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="4324f-112">Use hello SDK toointegrate client applications with Azure Batch through various languages, stage data tooAzure, and build job execution pipelines.</span></span>

* [<span data-ttu-id="4324f-113">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="4324f-113">Documentation</span></span>](https://azure.microsoft.com/documentation/services/batch/)
* <span data-ttu-id="4324f-114">[Rozhraní .NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), a [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) referenční dokumentace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="4324f-114">[.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/), and [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) API reference</span></span>
* <span data-ttu-id="4324f-115">[Knihovny batch management .NET](https://msdn.microsoft.com/library/mt463120.aspx) odkaz</span><span class="sxs-lookup"><span data-stu-id="4324f-115">[Batch management .NET library](https://msdn.microsoft.com/library/mt463120.aspx) reference</span></span>
* <span data-ttu-id="4324f-116">Kurzy: Začínáme s [Azure Batch library pro .NET](batch-dotnet-get-started.md) a [klientem Batch Python](batch-python-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="4324f-116">Tutorials: Get started with [Azure Batch library for .NET](batch-dotnet-get-started.md) and [Batch Python client](batch-python-tutorial.md)</span></span>
* [<span data-ttu-id="4324f-117">Fórum batch</span><span class="sxs-lookup"><span data-stu-id="4324f-117">Batch forum</span></span>](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)
* [<span data-ttu-id="4324f-118">Batch videa</span><span class="sxs-lookup"><span data-stu-id="4324f-118">Batch videos</span></span>](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a><span data-ttu-id="4324f-119">Řešení clusteru prostředí HPC</span><span class="sxs-lookup"><span data-stu-id="4324f-119">HPC cluster solutions</span></span>
<span data-ttu-id="4324f-120">Nasaďte, nebo rozšířit vaše stávající Windows nebo Linux HPC clusteru tooAzure toorun vaše úloh náročných na výkon.</span><span class="sxs-lookup"><span data-stu-id="4324f-120">Deploy or extend your existing Windows or Linux HPC cluster tooAzure toorun your compute intensive workloads.</span></span>  

### <a name="microsoft-hpc-pack"></a><span data-ttu-id="4324f-121">Sady Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4324f-121">Microsoft HPC Pack</span></span>
<span data-ttu-id="4324f-122">HPC Pack je společnosti Microsoft volné HPC řešení založen na technologiích Microsoft Azure a Windows Server, schopný spustit Windows a Linux HPC úlohy.</span><span class="sxs-lookup"><span data-stu-id="4324f-122">HPC Pack is Microsoft's free HPC solution built on Microsoft Azure and Windows Server technologies, capable of running Windows and Linux HPC workloads.</span></span>  

* [<span data-ttu-id="4324f-123">Stažení sady HPC Pack 2016</span><span class="sxs-lookup"><span data-stu-id="4324f-123">Download HPC Pack 2016</span></span>](https://www.microsoft.com/download/details.aspx?id=54507)
* [<span data-ttu-id="4324f-124">Stáhnout prostředí HPC Pack 2012 R2 s aktualizací 3</span><span class="sxs-lookup"><span data-stu-id="4324f-124">Download HPC Pack 2012 R2 Update 3</span></span>](https://www.microsoft.com/download/details.aspx?id=49922)
* [<span data-ttu-id="4324f-125">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="4324f-125">Documentation</span></span>](https://technet.microsoft.com/library/jj899572.aspx)
* <span data-ttu-id="4324f-126">Možnosti clusteru HPC Pack v Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="4324f-126">HPC Pack cluster options in Azure: [Linux](../virtual-machines/linux/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) and [Windows](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span> 
* [<span data-ttu-id="4324f-127">Rozšíření tooAzure instancí pracovního procesu pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4324f-127">Burst tooAzure worker instances with HPC Pack</span></span>](https://technet.microsoft.com/library/gg481749.aspx)
* [<span data-ttu-id="4324f-128">Shluků tooAzure Batch pomocí sady HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4324f-128">Burst tooAzure  Batch with HPC Pack</span></span>](https://technet.microsoft.com/library/mt612877.aspx)
* [<span data-ttu-id="4324f-129">Fóra Windows HPC</span><span class="sxs-lookup"><span data-stu-id="4324f-129">Windows HPC forums</span></span>](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a><span data-ttu-id="4324f-130">Řešení clusteru pro Linux a operačních systémů</span><span class="sxs-lookup"><span data-stu-id="4324f-130">Linux and OSS cluster solutions</span></span>
<span data-ttu-id="4324f-131">Pomocí těchto toodeploy šablony Azure, které clusterů Linux HPC.</span><span class="sxs-lookup"><span data-stu-id="4324f-131">Use these Azure templates toodeploy Linux HPC clusters.</span></span>

* <span data-ttu-id="4324f-132">[Začne pracovat SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) a [příspěvku na blogu](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="4324f-132">[Spin up a SLURM cluster](https://azure.microsoft.com/documentation/templates/slurm/) and [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)</span></span>
* [<span data-ttu-id="4324f-133">Číselníku Torque clusteru</span><span class="sxs-lookup"><span data-stu-id="4324f-133">Spin up a Torque cluster</span></span>](https://azure.microsoft.com/documentation/templates/torque-cluster/)
* [<span data-ttu-id="4324f-134">Výpočetní mřížky šablony s PBS Professional</span><span class="sxs-lookup"><span data-stu-id="4324f-134">Compute grid templates with PBS Professional</span></span>](https://github.com/xpillons/azure-hpc/tree/master/Compute-Grid-Infra)

## <a name="hpc-storage"></a><span data-ttu-id="4324f-135">HPC úložiště</span><span class="sxs-lookup"><span data-stu-id="4324f-135">HPC storage</span></span>
* [<span data-ttu-id="4324f-136">Systémy souborů paralelní HPC úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="4324f-136">Parallel file systems for HPC storage on Azure</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/03/17/parallel-file-systems-for-hpc-storage-on-azure/)
* [<span data-ttu-id="4324f-137">Intel cloudu Edition pro počítače s Lustre Software – zkušební verze</span><span class="sxs-lookup"><span data-stu-id="4324f-137">Intel Cloud Edition for Lustre Software - Eval</span></span>](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)
* [<span data-ttu-id="4324f-138">BeeGFS v šabloně CentOS 7.2</span><span class="sxs-lookup"><span data-stu-id="4324f-138">BeeGFS on CentOS 7.2 template</span></span>](https://github.com/smith1511/hpc/tree/master/beegfs-shared-on-centos7.2)




## <a name="microsoft-mpi"></a><span data-ttu-id="4324f-139">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="4324f-139">Microsoft MPI</span></span>
<span data-ttu-id="4324f-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) je implementace Microsoft hello zpráva předávání rozhraní standard pro vývoj a spouštění paralelních aplikací na platformu Windows hello.</span><span class="sxs-lookup"><span data-stu-id="4324f-140">[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) is a Microsoft implementation of hello Message Passing Interface standard for developing and running parallel applications on hello Windows platform.</span></span>

* [<span data-ttu-id="4324f-141">Stáhněte si MS MPI</span><span class="sxs-lookup"><span data-stu-id="4324f-141">Download MS-MPI</span></span>](http://go.microsoft.com/FWLink/p/?LinkID=389556)
* [<span data-ttu-id="4324f-142">MS-MPI odkaz</span><span class="sxs-lookup"><span data-stu-id="4324f-142">MS-MPI reference</span></span>](https://msdn.microsoft.com/library/dn473458.aspx)
* [<span data-ttu-id="4324f-143">Fórum MPI</span><span class="sxs-lookup"><span data-stu-id="4324f-143">MPI forum</span></span>](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a><span data-ttu-id="4324f-144">Instance náročné na výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="4324f-144">Compute-intensive instances</span></span>
<span data-ttu-id="4324f-145">Azure nabízí [velikosti rozsah virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), včetně [náročné H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instancí, které lze připojit síť RDMA tooa back-end, toorun úlohy Linux a Windows HPC.</span><span class="sxs-lookup"><span data-stu-id="4324f-145">Azure offers a [range of VM sizes](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), including [compute-intensive H-series](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) instances capable of connecting tooa back-end RDMA network, toorun your Linux and Windows HPC workloads.</span></span> 

* [<span data-ttu-id="4324f-146">Nastavení aplikací MPI toorun clusteru Linux RDMA</span><span class="sxs-lookup"><span data-stu-id="4324f-146">Set up a Linux RDMA cluster toorun MPI applications</span></span>](../virtual-machines/linux/classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="4324f-147">Nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="4324f-147">Set up a Windows RDMA cluster with Microsoft HPC Pack toorun MPI applications</span></span>](../virtual-machines/windows/classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

<span data-ttu-id="4324f-148">Pro úlohy náročné na grafický procesor, podívejte se na [velikosti NC a vs](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span><span class="sxs-lookup"><span data-stu-id="4324f-148">For GPU-intensive workloads, check out [NC and NV sizes](https://azure.microsoft.com/blog/azure-n-series-general-availability-on-december-1/).</span></span>

## <a name="samples-and-demos"></a><span data-ttu-id="4324f-149">Ukázky a ukázky</span><span class="sxs-lookup"><span data-stu-id="4324f-149">Samples and demos</span></span>
* [<span data-ttu-id="4324f-150">Ukázky kódu Azure Batch C# a Python</span><span class="sxs-lookup"><span data-stu-id="4324f-150">Azure Batch C# and Python code samples</span></span>](https://github.com/Azure/azure-batch-samples)
* <span data-ttu-id="4324f-151">[Batch loděnice](https://azure.github.io/batch-shipyard/) nástrojů pro snadné nasazení stylu batch Dockerized úlohy tooAzure Batch</span><span class="sxs-lookup"><span data-stu-id="4324f-151">[Batch Shipyard](https://azure.github.io/batch-shipyard/) toolkit for easy deployment of batch-style Dockerized workloads tooAzure Batch</span></span>
* <span data-ttu-id="4324f-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) balíček R, postavená na Azure Batch</span><span class="sxs-lookup"><span data-stu-id="4324f-152">[doAzureParallel](http://www.github.com/Azure/doAzureParallel) R package, built on top of Azure Batch</span></span>
* [<span data-ttu-id="4324f-153">Vyzkoušejte SUSE Linux Enterprise Server pro HPC</span><span class="sxs-lookup"><span data-stu-id="4324f-153">Test drive SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a><span data-ttu-id="4324f-154">Související služby Azure</span><span class="sxs-lookup"><span data-stu-id="4324f-154">Related Azure services</span></span>
* [<span data-ttu-id="4324f-155">Data Factory</span><span class="sxs-lookup"><span data-stu-id="4324f-155">Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)
* [<span data-ttu-id="4324f-156">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4324f-156">Machine Learning</span></span>](https://azure.microsoft.com/documentation/services/machine-learning/)
* [<span data-ttu-id="4324f-157">HDInsight</span><span class="sxs-lookup"><span data-stu-id="4324f-157">HDInsight</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="4324f-158">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="4324f-158">Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="4324f-159">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4324f-159">Virtual Machine Scale Sets</span></span>](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)
* [<span data-ttu-id="4324f-160">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="4324f-160">Cloud Services</span></span>](https://azure.microsoft.com/documentation/services/cloud-services/)
* [<span data-ttu-id="4324f-161">App Service</span><span class="sxs-lookup"><span data-stu-id="4324f-161">App Service</span></span>](https://azure.microsoft.com/documentation/services/app-service/)
* [<span data-ttu-id="4324f-162">Media Services</span><span class="sxs-lookup"><span data-stu-id="4324f-162">Media Services</span></span>](https://azure.microsoft.com/documentation/services/media-services/)
* [<span data-ttu-id="4324f-163">Functions</span><span class="sxs-lookup"><span data-stu-id="4324f-163">Functions</span></span>](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a><span data-ttu-id="4324f-164">Plány architektury</span><span class="sxs-lookup"><span data-stu-id="4324f-164">Architecture blueprints</span></span>
* <span data-ttu-id="4324f-165">[Orchestration HPC a data pomocí Azure Batch a s Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) a [článku](../data-factory/data-factory-data-processing-using-batch.md)</span><span class="sxs-lookup"><span data-stu-id="4324f-165">[HPC and data orchestration using Azure Batch and Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) and [article](../data-factory/data-factory-data-processing-using-batch.md)</span></span>

## <a name="industry-solutions"></a><span data-ttu-id="4324f-166">Odvětví řešení</span><span class="sxs-lookup"><span data-stu-id="4324f-166">Industry solutions</span></span>
* [<span data-ttu-id="4324f-167">Bankovnictví a kapitálové trzích</span><span class="sxs-lookup"><span data-stu-id="4324f-167">Banking and capital markets</span></span>](https://finance.azure.com/)
* [<span data-ttu-id="4324f-168">Simulace inženýrství</span><span class="sxs-lookup"><span data-stu-id="4324f-168">Engineering simulations</span></span>](https://simulation.azure.com/) 

## <a name="customer-stories"></a><span data-ttu-id="4324f-169">Příběhy zákazníků</span><span class="sxs-lookup"><span data-stu-id="4324f-169">Customer stories</span></span>
* [<span data-ttu-id="4324f-170">ANEO</span><span class="sxs-lookup"><span data-stu-id="4324f-170">ANEO</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 
* [<span data-ttu-id="4324f-171">d3View</span><span class="sxs-lookup"><span data-stu-id="4324f-171">d3View</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)
* [<span data-ttu-id="4324f-172">Ludwig Institute výzkumu rakoviny</span><span class="sxs-lookup"><span data-stu-id="4324f-172">Ludwig Institute of Cancer Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)
* [<span data-ttu-id="4324f-173">Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="4324f-173">Microsoft Research</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)
* [<span data-ttu-id="4324f-174">Milliman</span><span class="sxs-lookup"><span data-stu-id="4324f-174">Milliman</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)
* [<span data-ttu-id="4324f-175">Mezinárodní cenné Mitsubishi UFJ</span><span class="sxs-lookup"><span data-stu-id="4324f-175">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)
* [<span data-ttu-id="4324f-176">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="4324f-176">Schlumberger</span></span>](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
* [<span data-ttu-id="4324f-177">Towers Watson</span><span class="sxs-lookup"><span data-stu-id="4324f-177">Towers Watson</span></span>](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)
* [<span data-ttu-id="4324f-178">UberCloud</span><span class="sxs-lookup"><span data-stu-id="4324f-178">UberCloud</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

## <a name="next-steps"></a><span data-ttu-id="4324f-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4324f-179">Next steps</span></span>
* <span data-ttu-id="4324f-180">Hello nejnovější oznámení, naleznete v části hello [blog týmu Microsoft HPC a Batch](http://blogs.technet.com/b/windowshpc/) a hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span><span class="sxs-lookup"><span data-stu-id="4324f-180">For hello latest announcements, see hello [Microsoft HPC and Batch team blog](http://blogs.technet.com/b/windowshpc/) and hello [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>
* <span data-ttu-id="4324f-181">Viz také [co je nového ve službě Batch](https://azure.microsoft.com/updates/?service=batch) nebo přihlášení k odběru toohello [informačního kanálu RSS](https://azure.microsoft.com/updates/feed/?service=batch).</span><span class="sxs-lookup"><span data-stu-id="4324f-181">Also see [what's new in Batch](https://azure.microsoft.com/updates/?service=batch) or subscribe toohello [RSS feed](https://azure.microsoft.com/updates/feed/?service=batch).</span></span>

