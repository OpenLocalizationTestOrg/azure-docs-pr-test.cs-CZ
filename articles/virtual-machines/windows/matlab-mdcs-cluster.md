---
title: "MATLAB clusterů na virtuálních počítačích | Microsoft Docs"
description: "Použijte virtuální počítače Microsoft Azure k vytvoření clusterů MATLAB distribuovaná výpočetní Server ke spuštění úlohy náročné paralelní MATLAB"
services: virtual-machines-windows
documentationcenter: 
author: mscurrell
manager: timlt
editor: 
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: b302c6b3c6acbb8552796e7fb1bfd153d23dceb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a><span data-ttu-id="121f2-103">Vytvoření MATLAB distribuovaný Server výpočetních clusterů na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="121f2-103">Create MATLAB Distributed Computing Server clusters on Azure VMs</span></span>
<span data-ttu-id="121f2-104">Pomocí virtuální počítače Microsoft Azure můžete vytvořit jeden nebo více clusterů MATLAB distribuovaná výpočetní serveru spouštět paralelní MATLAB úlohy náročné na výkon.</span><span class="sxs-lookup"><span data-stu-id="121f2-104">Use Microsoft Azure virtual machines to create one or more MATLAB Distributed Computing Server clusters to run your compute-intensive parallel MATLAB workloads.</span></span> <span data-ttu-id="121f2-105">Instalaci softwaru MATLAB distribuovaný Server Computing na virtuálním počítači používat jako základní bitové kopie a použít šablonu Azure rychlý start nebo skript Azure PowerShell (k dispozici na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) k nasazení a správě clusteru.</span><span class="sxs-lookup"><span data-stu-id="121f2-105">Install your MATLAB Distributed Computing Server software on a VM to use as a base image and use an Azure quickstart template or Azure PowerShell script (available on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) to deploy and manage the cluster.</span></span> <span data-ttu-id="121f2-106">Po nasazení připojte se ke clusteru ke spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="121f2-106">After deployment, connect to the cluster to run your workloads.</span></span>

## <a name="about-matlab-and-matlab-distributed-computing-server"></a><span data-ttu-id="121f2-107">O MATLAB a MATLAB distribuované Computing serveru</span><span class="sxs-lookup"><span data-stu-id="121f2-107">About MATLAB and MATLAB Distributed Computing Server</span></span>
<span data-ttu-id="121f2-108">[MATLAB](http://www.mathworks.com/products/matlab/) platformy je optimalizovaná pro řešení problémů technické a vědecké.</span><span class="sxs-lookup"><span data-stu-id="121f2-108">The [MATLAB](http://www.mathworks.com/products/matlab/) platform is optimized for solving engineering and scientific problems.</span></span> <span data-ttu-id="121f2-109">Uživatelé MATLAB s rozsáhlé simulace a úloh zpracování dat můžete použít společnost MathWorks paralelní výpočty produktů pro urychlení jejich výpočetně náročných úloh využitím výpočetní clustery a mřížky služeb.</span><span class="sxs-lookup"><span data-stu-id="121f2-109">MATLAB users with large-scale simulations and data processing tasks can use MathWorks parallel computing products to speed up their compute-intensive workloads by taking advantage of compute clusters and grid services.</span></span> <span data-ttu-id="121f2-110">[Paralelní výpočty nástrojů](http://www.mathworks.com/products/parallel-computing/) umožňuje uživatelům MATLAB paralelní aplikace a využít výhod více jádry grafickými procesory a výpočetní clustery.</span><span class="sxs-lookup"><span data-stu-id="121f2-110">[Parallel Computing Toolbox](http://www.mathworks.com/products/parallel-computing/) lets MATLAB users parallelize applications and take advantage of multi-core processors, GPUs, and compute clusters.</span></span> <span data-ttu-id="121f2-111">[MATLAB distribuovaný Server Computing](http://www.mathworks.com/products/distriben/) umožňuje uživatelům MATLAB využívat mnoho počítačů ve výpočetním clusteru.</span><span class="sxs-lookup"><span data-stu-id="121f2-111">[MATLAB Distributed Computing Server](http://www.mathworks.com/products/distriben/) enables MATLAB users to utilize many computers in a compute cluster.</span></span>

<span data-ttu-id="121f2-112">Pomocí virtuální počítače Azure můžete vytvořit MATLAB distribuovaný Server výpočetních clusterů, které mají stejné mechanismy, které jsou k dispozici pro odeslání paralelní práce jako místní clustery, jako je například interaktivní úlohy, úlohy batch, nezávislé úlohy a komunikaci úlohy.</span><span class="sxs-lookup"><span data-stu-id="121f2-112">By using Azure virtual machines, you can create MATLAB Distributed Computing Server clusters that have all the same mechanisms available to submit parallel work as on-premises clusters, such as interactive jobs, batch jobs, independent tasks, and communicating tasks.</span></span> <span data-ttu-id="121f2-113">Použití Azure ve spojení s platformou MATLAB má mnoho výhod oproti zřizování a pomocí tradiční místní hardware: velikostí rozsah virtuálního počítače, vytváření clusterů na vyžádání, platíte jenom za výpočetní prostředky, které používáte, a možnost otestovat modely ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="121f2-113">Using Azure in conjunction with the MATLAB platform has many benefits compared to provisioning and using traditional on-premises hardware: a range of virtual machine sizes, creation of clusters on-demand so you pay only for the compute resources you use, and the ability to test models at scale.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="121f2-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="121f2-114">Prerequisites</span></span>
* <span data-ttu-id="121f2-115">**Klientský počítač** -založené na Windows klientský počítač komunikovat s Azure a MATLAB distribuovaný Server výpočetní cluster po nasazení budete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="121f2-115">**Client computer** - You'll need a Windows-based client computer to communicate with Azure and the MATLAB Distributed Computing Server cluster after deployment.</span></span>
* <span data-ttu-id="121f2-116">**Prostředí Azure PowerShell** -najdete v části [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) k její instalaci do klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="121f2-116">**Azure PowerShell** - See [How to install and configure Azure PowerShell](/powershell/azure/overview) to install it on your client computer.</span></span>
* <span data-ttu-id="121f2-117">**Předplatné Azure** – Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="121f2-117">**Azure subscription** - If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="121f2-118">Pro větší clustery zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu.</span><span class="sxs-lookup"><span data-stu-id="121f2-118">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="121f2-119">**Kvóta jader** -možná budete muset zvýšit kvótu základní chcete nasadit více než jeden cluster MATLAB distribuovaný Server Computing nebo velké clusteru.</span><span class="sxs-lookup"><span data-stu-id="121f2-119">**Cores quota** - You might need to increase the core quota to deploy a large cluster or more than one MATLAB Distributed Computing Server cluster.</span></span> <span data-ttu-id="121f2-120">Pokud chcete zvýšit kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.</span><span class="sxs-lookup"><span data-stu-id="121f2-120">To increase a quota, [open an online customer support request](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) at no charge.</span></span>
* <span data-ttu-id="121f2-121">**MATLAB, paralelní výpočty sada nástrojů a MATLAB distribuovaný Server Computing licence** -skripty předpokládat, že [správce licencí hostované společnost MathWorks](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) se používá pro všechny licence.</span><span class="sxs-lookup"><span data-stu-id="121f2-121">**MATLAB, Parallel Computing Toolbox, and MATLAB Distributed Computing Server licenses** - The scripts assume that the [MathWorks Hosted License Manager](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) is used for all licenses.</span></span>  
* <span data-ttu-id="121f2-122">**Software MATLAB distribuovaný Server Computing** -bude nainstalována na virtuální počítač, který se použije jako základní image virtuálního počítače pro cluster virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="121f2-122">**MATLAB Distributed Computing Server software** - Will be installed on a VM that will be used as the base VM image for the cluster VMs.</span></span>

## <a name="high-level-steps"></a><span data-ttu-id="121f2-123">Kroky vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="121f2-123">High level steps</span></span>
<span data-ttu-id="121f2-124">Pokud chcete používat virtuální počítače Azure pro své clustery serverů distribuované Computing MATLAB, jsou požadovány následující postup vysoké úrovně.</span><span class="sxs-lookup"><span data-stu-id="121f2-124">To use Azure virtual machines for your MATLAB Distributed Computing Server clusters, the following high-level steps are required.</span></span> <span data-ttu-id="121f2-125">Podrobné pokyny naleznete v dokumentaci doprovázející aplikaci rychlý start šablonu a skripty na [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).</span><span class="sxs-lookup"><span data-stu-id="121f2-125">Detailed instructions are in the documentation accompanying the quickstart template and scripts on [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).</span></span>

1. <span data-ttu-id="121f2-126">**Vytvořte základní image virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="121f2-126">**Create a base VM image**</span></span>  

   * <span data-ttu-id="121f2-127">Stáhněte a nainstalujte software MATLAB distribuovaný Server Computing na tento virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="121f2-127">Download and install MATLAB Distributed Computing Server software onto this VM.</span></span>

     > [!NOTE]
     > <span data-ttu-id="121f2-128">Tento proces může trvat několik hodin, ale budete muset provést po pro každou verzi MATLAB použijete.</span><span class="sxs-lookup"><span data-stu-id="121f2-128">This process can take a couple of hours, but you only have to do it once for each version of MATLAB you use.</span></span>   
     >
     >
2. <span data-ttu-id="121f2-129">**Vytvořte jeden nebo více clusterů**</span><span class="sxs-lookup"><span data-stu-id="121f2-129">**Create one or more clusters**</span></span>  

   * <span data-ttu-id="121f2-130">Zadaný skript prostředí PowerShell použít nebo vytvořit cluster z základní image virtuálního počítače pomocí šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="121f2-130">Use the supplied PowerShell script or use the quickstart template to create a cluster from the base VM image.</span></span>   
   * <span data-ttu-id="121f2-131">Správa clusterů pomocí zadaný skript prostředí PowerShell, které umožňuje seznamu, pozastavit, obnovit a odstranění clusterů.</span><span class="sxs-lookup"><span data-stu-id="121f2-131">Manage the clusters using the supplied PowerShell script which allows you to list, pause, resume, and delete clusters.</span></span>

## <a name="cluster-configurations"></a><span data-ttu-id="121f2-132">Konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="121f2-132">Cluster configurations</span></span>
<span data-ttu-id="121f2-133">V současné době skriptu pro vytváření clusteru a šablony umožňují vytvořit jeden Server distribuované Computing MATLAB topologie.</span><span class="sxs-lookup"><span data-stu-id="121f2-133">Currently, the cluster creation script and template enable you to create a single MATLAB Distributed Computing Server topology.</span></span> <span data-ttu-id="121f2-134">Pokud chcete, vytvořte jeden nebo více dalších clusterů s každý cluster s odlišným počtem pracovní virtuální počítače, pomocí různých velikostí virtuálních počítačů a tak dále.</span><span class="sxs-lookup"><span data-stu-id="121f2-134">If you want, create one or more additional clusters, with each cluster having a different number of worker VMs, using different VM sizes, and so on.</span></span>

### <a name="matlab-client-and-cluster-in-azure"></a><span data-ttu-id="121f2-135">MATLAB klienta a cluster v Azure</span><span class="sxs-lookup"><span data-stu-id="121f2-135">MATLAB client and cluster in Azure</span></span>
<span data-ttu-id="121f2-136">MATLAB klientský uzel, uzel MATLAB plánovače úloh a MATLAB distribuovaný Server Computing "pracovní" uzly jsou všechny nakonfigurované jako virtuální počítače Azure ve virtuální síti, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="121f2-136">The MATLAB client node, MATLAB Job Scheduler node, and MATLAB Distributed Computing Server "worker" nodes are all configured as Azure VMs in a virtual network, as shown in the following figure.</span></span>


* <span data-ttu-id="121f2-137">Pokud chcete použít clusteru, připojte k uzlu klienta pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="121f2-137">To use the cluster, connect by Remote Desktop to the client node.</span></span> <span data-ttu-id="121f2-138">Uzel klient spustí MATLAB klienta.</span><span class="sxs-lookup"><span data-stu-id="121f2-138">The client node runs the MATLAB client.</span></span>
* <span data-ttu-id="121f2-139">Klientský uzel má sdílení souborů, které mají přístup všechny pracovní procesy.</span><span class="sxs-lookup"><span data-stu-id="121f2-139">The client node has a file share that can be accessed by all workers.</span></span>
* <span data-ttu-id="121f2-140">Správce licencí hostované společnost MathWorks se používá pro kontroly licence pro veškerý software MATLAB.</span><span class="sxs-lookup"><span data-stu-id="121f2-140">MathWorks Hosted License Manager is used for the license checks for all MATLAB software.</span></span>
* <span data-ttu-id="121f2-141">Ve výchozím nastavení v pracovním procesu virtuální počítače se vytvoří jeden pracovní proces MATLAB distribuovaný Server Computing za jádra, ale můžete zadat všechny číslo.</span><span class="sxs-lookup"><span data-stu-id="121f2-141">By default, one MATLAB Distributed Computing Server worker per core is created on the worker VMs, but you can specify any number.</span></span>

## <a name="use-an-azure-based-cluster"></a><span data-ttu-id="121f2-142">Použití clusteru služby založené na Azure</span><span class="sxs-lookup"><span data-stu-id="121f2-142">Use an Azure-based Cluster</span></span>
<span data-ttu-id="121f2-143">Jako s jinými typy MATLAB distribuovaný Server výpočetních clusterů, budete muset použít profil Správce clusteru v klientovi MATLAB (na straně klienta VM) k vytvoření profilu clusteru MATLAB plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="121f2-143">As with other types of MATLAB Distributed Computing Server clusters, you need to use the Cluster Profile Manager in the MATLAB client (on the client VM) to create a MATLAB Job Scheduler cluster profile.</span></span>

![Profil Správce clusteru](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a><span data-ttu-id="121f2-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="121f2-145">Next steps</span></span>
* <span data-ttu-id="121f2-146">Podrobné pokyny k nasazení a správě clusterů MATLAB distribuovaný Server výpočty v Azure najdete v tématu [Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) úložiště, který obsahuje šablony a skripty.</span><span class="sxs-lookup"><span data-stu-id="121f2-146">For detailed instructions to deploy and manage MATLAB Distributed Computing Server clusters in Azure, see the [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) repository containing the templates and scripts.</span></span>
* <span data-ttu-id="121f2-147">Přejděte na [společnost MathWorks lokality](http://www.mathworks.com/) pro podrobnou dokumentaci pro MATLAB a MATLAB distribuovaný Server Computing.</span><span class="sxs-lookup"><span data-stu-id="121f2-147">Go to the [MathWorks site](http://www.mathworks.com/) for detailed documentation for MATLAB and MATLAB Distributed Computing Server.</span></span>