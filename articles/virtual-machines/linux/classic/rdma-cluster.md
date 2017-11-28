---
title: "Nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI | Microsoft Docs"
description: "Vytvoření clusteru s podporou Linux velikosti H16r, H16mr, A8 nebo A9 virtuálních počítačů, které se použije ke spuštění aplikací MPI sítě Azure RDMA"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 4b2ceb64b1737918458f6d5c692fc2bfbc0f12ed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a><span data-ttu-id="fca3c-103">Nastavení clusteru Linux RDMA pro spouštění aplikací MPI</span><span class="sxs-lookup"><span data-stu-id="fca3c-103">Set up a Linux RDMA cluster to run MPI applications</span></span>
<span data-ttu-id="fca3c-104">Zjistěte, jak nastavit clusteru s podporou Linux RDMA v Azure pomocí [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ke spouštění paralelních aplikací Message Passing Interface (MPI).</span><span class="sxs-lookup"><span data-stu-id="fca3c-104">Learn how to set up a Linux RDMA cluster in Azure with [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to run parallel Message Passing Interface (MPI) applications.</span></span> <span data-ttu-id="fca3c-105">Tento článek obsahuje kroky k přípravě image Linux HPC ke spuštění v clusteru s podporou Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-105">This article provides steps to prepare a Linux HPC image to run Intel MPI on a cluster.</span></span> <span data-ttu-id="fca3c-106">Po přípravě nasazení clusteru virtuálních počítačů pomocí tuto bitovou kopii a jeden velikostí podporující RDMA virtuální počítač Azure (aktuálně H16r, H16mr, A8 a A9).</span><span class="sxs-lookup"><span data-stu-id="fca3c-106">After preparation, you deploy a cluster of VMs using this image and one of the RDMA-capable Azure VM sizes (currently H16r, H16mr, A8, or A9).</span></span> <span data-ttu-id="fca3c-107">Použijte cluster ke spouštění aplikací MPI, které efektivně komunikují přes síť s nízkou latencí, vysokou propustností založené na technologii do paměti vzdáleného přímý přístup do (počítače RDMA).</span><span class="sxs-lookup"><span data-stu-id="fca3c-107">Use the cluster to run MPI applications that communicate efficiently over a low-latency, high-throughput network based on remote direct memory access (RDMA) technology.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fca3c-108">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="fca3c-108">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="fca3c-109">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="fca3c-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="fca3c-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fca3c-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

## <a name="cluster-deployment-options"></a><span data-ttu-id="fca3c-111">Možnosti nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="fca3c-111">Cluster deployment options</span></span>
<span data-ttu-id="fca3c-112">Toto jsou metody, které můžete použít k vytvoření clusteru s podporou Linux RDMA s nebo bez Plánovač úloh.</span><span class="sxs-lookup"><span data-stu-id="fca3c-112">Following are methods you can use to create a Linux RDMA cluster with or without a job scheduler.</span></span>

* <span data-ttu-id="fca3c-113">**Azure CLI skripty**: Jak uvidíte později v tomto článku, použijte [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) (CLI) pro skript nasazení clusteru s podporou RDMA podporovat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fca3c-113">**Azure CLI scripts**: As shown later in this article, use the [Azure command-line interface](../../../cli-install-nodejs.md) (CLI) to script the deployment of a cluster of RDMA-capable VMs.</span></span> <span data-ttu-id="fca3c-114">Uzly clusteru rozhraní příkazového řádku v režimu správy služby sériově vytvoří v modelu nasazení classic, takže nasazení mnoho výpočetních uzlů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="fca3c-114">The CLI in Service Management mode creates the cluster nodes serially in the classic deployment model, so deploying many compute nodes might take several minutes.</span></span> <span data-ttu-id="fca3c-115">Chcete-li povolit síťového připojení RDMA při použití modelu nasazení classic, nasaďte virtuální počítače ve stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fca3c-115">To enable the RDMA network connection when you use the classic deployment model, deploy the VMs in the same cloud service.</span></span>
* <span data-ttu-id="fca3c-116">**Šablony Azure Resource Manageru**: modelu nasazení Resource Manager můžete také použít k nasazení clusteru s podporou RDMA podporovat virtuální počítače připojené k síti RDMA.</span><span class="sxs-lookup"><span data-stu-id="fca3c-116">**Azure Resource Manager templates**: You can also use the Resource Manager deployment model to deploy a cluster of RDMA-capable VMs that connects to the RDMA network.</span></span> <span data-ttu-id="fca3c-117">Můžete [vytvořit vlastní šablonu](../../../resource-group-authoring-templates.md), nebo zkontrolujte [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) pro šablony přispěli k nasazení řešení. Chcete společnosti Microsoft nebo komunitou.</span><span class="sxs-lookup"><span data-stu-id="fca3c-117">You can [create your own template](../../../resource-group-authoring-templates.md), or check the [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/) for templates contributed by Microsoft or the community to deploy the solution you want.</span></span> <span data-ttu-id="fca3c-118">Šablony Resource Manageru nabízejí rychlý a spolehlivý způsob, jak nasadit Linux cluster.</span><span class="sxs-lookup"><span data-stu-id="fca3c-118">Resource Manager templates can provide a fast and reliable way to deploy a Linux cluster.</span></span> <span data-ttu-id="fca3c-119">Pokud chcete povolit síťového připojení RDMA při použití modelu nasazení Resource Manager, nasaďte virtuální počítače ve stejné sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="fca3c-119">To enable the RDMA network connection when you use the Resource Manager deployment model, deploy the VMs in the same availability set.</span></span>
* <span data-ttu-id="fca3c-120">**HPC Pack**: vytvoření clusteru s podporou sady Microsoft HPC Pack v Azure a přidat podporu rdma výpočetní uzly, které využívají podporované distribuce systému Linux pro přístup k síti RDMA.</span><span class="sxs-lookup"><span data-stu-id="fca3c-120">**HPC Pack**: Create a Microsoft HPC Pack cluster in Azure and add RDMA-capable compute nodes that run a supported Linux distribution to access the RDMA network.</span></span> <span data-ttu-id="fca3c-121">Další informace najdete v tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="fca3c-121">For more information, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="sample-deployment-steps-in-the-classic-model"></a><span data-ttu-id="fca3c-122">Kroky nasazení ukázkové v klasickém modelu</span><span class="sxs-lookup"><span data-stu-id="fca3c-122">Sample deployment steps in the classic model</span></span>
<span data-ttu-id="fca3c-123">Následující kroky ukazují, jak používat rozhraní příkazového řádku Azure k nasazení virtuálního počítače SUSE Linux Enterprise Server (SLES) 12 SP1 HPC z Azure Marketplace, přizpůsobit a vytvořit vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-123">The following steps show how to use the Azure CLI to deploy a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM from the Azure Marketplace, customize it, and create a custom VM image.</span></span> <span data-ttu-id="fca3c-124">Pak můžete použít bitovou kopii pro skript nasazení clusteru s podporou RDMA podporovat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fca3c-124">Then you can use the image to script the deployment of a cluster of RDMA-capable VMs.</span></span>

> [!TIP]
> <span data-ttu-id="fca3c-125">Podobným způsobem použijte k nasazení clusteru s podporou RDMA podporovat virtuálních počítačů založené na imagích na základě CentOS HPC v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fca3c-125">Use similar steps to deploy a cluster of RDMA-capable VMs based on CentOS-based HPC images in the Azure Marketplace.</span></span> <span data-ttu-id="fca3c-126">Některé kroky poněkud, jak jsme uvedli lišit.</span><span class="sxs-lookup"><span data-stu-id="fca3c-126">Some steps differ slightly, as noted.</span></span> 
>
>

### <a name="prerequisites"></a><span data-ttu-id="fca3c-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fca3c-127">Prerequisites</span></span>
* <span data-ttu-id="fca3c-128">**Klientský počítač**: je třeba klientský počítač Mac, Linux nebo Windows ke komunikaci s Azure.</span><span class="sxs-lookup"><span data-stu-id="fca3c-128">**Client computer**: You need a Mac, Linux, or Windows client computer to communicate with Azure.</span></span> <span data-ttu-id="fca3c-129">Tento postup předpokládá, že používáte klienta Linux.</span><span class="sxs-lookup"><span data-stu-id="fca3c-129">These steps assume you are using a Linux client.</span></span>
* <span data-ttu-id="fca3c-130">**Předplatné Azure**: Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="fca3c-130">**Azure subscription**: If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="fca3c-131">Pro větší clustery zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu.</span><span class="sxs-lookup"><span data-stu-id="fca3c-131">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="fca3c-132">**Virtuální počítač velikost dostupnosti**: následující instance velikosti jsou podporující RDMA: H16r, H16mr, A8 a A9.</span><span class="sxs-lookup"><span data-stu-id="fca3c-132">**VM size availability**: The following instance sizes are RDMA capable: H16r, H16mr, A8, and A9.</span></span> <span data-ttu-id="fca3c-133">Zkontrolujte [produkty podle oblasti](https://azure.microsoft.com/regions/services/) pro dostupnost v oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="fca3c-133">Check [Products available by region](https://azure.microsoft.com/regions/services/) for availability in Azure regions.</span></span>
* <span data-ttu-id="fca3c-134">**Kvóta jader**: možná budete muset zvýšit kvótu jader, který má nasadit cluster virtuálních počítačů náročné.</span><span class="sxs-lookup"><span data-stu-id="fca3c-134">**Cores quota**: You might need to increase the quota of cores to deploy a cluster of compute-intensive VMs.</span></span> <span data-ttu-id="fca3c-135">Pokud chcete nasadit virtuální počítače 8 A9, jak je znázorněno v tomto článku se například potřebovat nejméně 128 jader.</span><span class="sxs-lookup"><span data-stu-id="fca3c-135">For example, you need at least 128 cores if you want to deploy 8 A9 VMs as shown in this article.</span></span> <span data-ttu-id="fca3c-136">Vaše předplatné může také omezit počet jader, který můžete nasadit v určité rodiny velikost virtuálního počítače, včetně H-series.</span><span class="sxs-lookup"><span data-stu-id="fca3c-136">Your subscription might also limit the number of cores you can deploy in certain VM size families, including the H-series.</span></span> <span data-ttu-id="fca3c-137">Požádat o zvýšení kvóty, [otevřete žádosti o podporu online zákazníka](../../../azure-supportability/how-to-create-azure-support-request.md) zdarma.</span><span class="sxs-lookup"><span data-stu-id="fca3c-137">To request a quota increase, [open an online customer support request](../../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
* <span data-ttu-id="fca3c-138">**Rozhraní příkazového řádku Azure**: [nainstalovat](../../../cli-install-nodejs.md) rozhraní příkazového řádku Azure a [připojení k předplatnému Azure](../../../xplat-cli-connect.md) z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-138">**Azure CLI**: [Install](../../../cli-install-nodejs.md) the Azure CLI and [connect to your Azure subscription](../../../xplat-cli-connect.md) from the client computer.</span></span>

### <a name="provision-an-sles-12-sp1-hpc-vm"></a><span data-ttu-id="fca3c-139">Zřizování virtuálních počítačů SLES 12 SP1 HPC</span><span class="sxs-lookup"><span data-stu-id="fca3c-139">Provision an SLES 12 SP1 HPC VM</span></span>
<span data-ttu-id="fca3c-140">Po přihlášení do Azure pomocí Azure CLI, spusťte `azure config list` potvrďte, že výstup zobrazuje režimu správy služby.</span><span class="sxs-lookup"><span data-stu-id="fca3c-140">After signing in to Azure with the Azure CLI, run `azure config list` to confirm that the output shows Service Management mode.</span></span> <span data-ttu-id="fca3c-141">Pokud ne, nastavte režim spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="fca3c-141">If it does not, set the mode by running this command:</span></span>

    azure config mode asm


<span data-ttu-id="fca3c-142">Zadejte následující příkaz k zobrazení seznamu všechny odběry, které mají oprávnění využívat:</span><span class="sxs-lookup"><span data-stu-id="fca3c-142">Type the following to list all the subscriptions you are authorized to use:</span></span>

    azure account list

<span data-ttu-id="fca3c-143">Aktuální aktivní předplatné, je označen `Current` nastavena na `true`.</span><span class="sxs-lookup"><span data-stu-id="fca3c-143">The current active subscription is identified with `Current` set to `true`.</span></span> <span data-ttu-id="fca3c-144">Pokud toto předplatné není ten, který chcete použít k vytvoření clusteru, nastavte ID příslušné předplatné jako aktivní předplatné:</span><span class="sxs-lookup"><span data-stu-id="fca3c-144">If this subscription isn't the one you want to use to create the cluster, set the appropriate subscription ID as the active subscription:</span></span>

    azure account set <subscription-Id>

<span data-ttu-id="fca3c-145">Pokud chcete zobrazit veřejně dostupné Image SLES 12 SP1 HPC v Azure, spusťte příkaz podobně jako tento, za předpokladu, že vaše prostředí podporuje prostředí **grep**:</span><span class="sxs-lookup"><span data-stu-id="fca3c-145">To see the publicly available SLES 12 SP1 HPC images in Azure, run a command like the following, assuming your shell environment supports **grep**:</span></span>

    azure vm image list | grep "suse.*hpc"

<span data-ttu-id="fca3c-146">Zřídit podporou RDMA virtuální počítač s bitovou kopii SLES 12 SP1 HPC spuštěním příkazu takto:</span><span class="sxs-lookup"><span data-stu-id="fca3c-146">Provision an RDMA-capable VM with a SLES 12 SP1 HPC image by running a command like the following:</span></span>

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

<span data-ttu-id="fca3c-147">Kde:</span><span class="sxs-lookup"><span data-stu-id="fca3c-147">Where:</span></span>

* <span data-ttu-id="fca3c-148">Velikost (v tomto příkladu A9) je jedním z velikosti virtuálních počítačů RDMA podporovat.</span><span class="sxs-lookup"><span data-stu-id="fca3c-148">The size (A9 in this example) is one of the RDMA-capable VM sizes.</span></span>
* <span data-ttu-id="fca3c-149">Externí číslo portu SSH (22 v tomto příkladu, což je výchozí SSH) je platné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="fca3c-149">The external SSH port number (22 in this example, which is the SSH default) is any valid port number.</span></span> <span data-ttu-id="fca3c-150">Interní číslo portu SSH je nastavena na 22.</span><span class="sxs-lookup"><span data-stu-id="fca3c-150">The internal SSH port number is set to 22.</span></span>
* <span data-ttu-id="fca3c-151">Vytvoří novou cloudovou službu se v oblasti Azure určeného umístění.</span><span class="sxs-lookup"><span data-stu-id="fca3c-151">A new cloud service is created in the Azure region specified by the location.</span></span> <span data-ttu-id="fca3c-152">Zadejte umístění, ve kterém je k dispozici velikost virtuálního počítače, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="fca3c-152">Specify a location in which the VM size you choose is available.</span></span>
* <span data-ttu-id="fca3c-153">Pro podporu s prioritou SUSE (který budou vám účtovány dodatečné poplatky) název bitové kopie SLES 12 SP1 aktuálně může být jedna z těchto dvou možností:</span><span class="sxs-lookup"><span data-stu-id="fca3c-153">For SUSE priority support (which incurs additional charges), the SLES 12 SP1 image name currently can be one of these two options:</span></span> 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-the-vm"></a><span data-ttu-id="fca3c-154">Přizpůsobení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fca3c-154">Customize the VM</span></span>
<span data-ttu-id="fca3c-155">Až virtuální počítač dokončí zřizování, SSH k virtuálnímu počítači pomocí externí IP adresu Virtuálního počítače (nebo název DNS) a externí port číslo jste nakonfigurovali a pak jej přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="fca3c-155">After the VM finishes provisioning, SSH to the VM by using the VM's external IP address (or DNS name) and the external port number you configured, and then customize it.</span></span> <span data-ttu-id="fca3c-156">Podrobnosti připojení najdete v tématu [přihlášení do virtuálního počítače se systémem Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fca3c-156">For connection details, see [How to log on to a virtual machine running Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="fca3c-157">Proveďte příkazy jako uživatel, který jste nakonfigurovali ve virtuálním počítači, pokud kořenový přístup je nutný k dokončení kroku.</span><span class="sxs-lookup"><span data-stu-id="fca3c-157">Perform commands as the user you configured on the VM, unless root access is required to complete a step.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fca3c-158">Microsoft Azure neposkytuje kořenový přístup k virtuálním počítačům systému Linux.</span><span class="sxs-lookup"><span data-stu-id="fca3c-158">Microsoft Azure does not provide root access to Linux VMs.</span></span> <span data-ttu-id="fca3c-159">Chcete-li získat přístup pro správu při připojení jako uživatel k virtuálnímu počítači, spusťte příkazy pomocí `sudo`.</span><span class="sxs-lookup"><span data-stu-id="fca3c-159">To gain administrative access when connected as a user to the VM, run commands by using `sudo`.</span></span>
>
>

* <span data-ttu-id="fca3c-160">**Aktualizace**: nainstalujte aktualizace pomocí zypper.</span><span class="sxs-lookup"><span data-stu-id="fca3c-160">**Updates**: Install updates by using zypper.</span></span> <span data-ttu-id="fca3c-161">Také můžete chtít nainstalovat nástroje pro systém souborů NFS.</span><span class="sxs-lookup"><span data-stu-id="fca3c-161">You might also want to install NFS utilities.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="fca3c-162">V SLES 12 SP1 HPC virtuální počítač doporučujeme nepoužijete jádra aktualizace, které může způsobit problémy s Linux RDMA ovladače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-162">In a SLES 12 SP1 HPC VM, we recommend that you don't apply kernel updates, which can cause issues with the Linux RDMA drivers.</span></span>
  >
  >
* <span data-ttu-id="fca3c-163">**Intel MPI**: dokončení instalace Intel MPI ve virtuálním počítači se systémem SLES 12 SP1 HPC spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="fca3c-163">**Intel MPI**: Complete the installation of Intel MPI on the SLES 12 SP1 HPC VM by running the following command:</span></span>

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* <span data-ttu-id="fca3c-164">**Uzamknout paměť**: přidat kódy pro MPI k uzamčení paměti k dispozici pro RDMA, nebo změnit následující nastavení v souboru /etc/security/limits.conf.</span><span class="sxs-lookup"><span data-stu-id="fca3c-164">**Lock memory**: For MPI codes to lock the memory available for RDMA, add or change the following settings in the /etc/security/limits.conf file.</span></span> <span data-ttu-id="fca3c-165">Musíte kořenový přístup k úpravě tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-165">You need root access to edit this file.</span></span>

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > <span data-ttu-id="fca3c-166">Pro účely testování můžete také nastavit memlock na neomezený.</span><span class="sxs-lookup"><span data-stu-id="fca3c-166">For testing purposes, you can also set memlock to unlimited.</span></span> <span data-ttu-id="fca3c-167">Například: `<User or group name>    hard    memlock unlimited`.</span><span class="sxs-lookup"><span data-stu-id="fca3c-167">For example: `<User or group name>    hard    memlock unlimited`.</span></span> <span data-ttu-id="fca3c-168">Další informace najdete v tématu [známé nejlepší metody pro nastavení uzamčení velikost paměti](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span><span class="sxs-lookup"><span data-stu-id="fca3c-168">For more information, see [Best known methods for setting locked memory size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span></span>
  >
  >
* <span data-ttu-id="fca3c-169">**Klíče SSH pro virtuální počítače SLES**: generování SSH klíče k navázání vztahu důvěryhodnosti pro váš uživatelský účet mezi výpočetní uzly v SLES clusteru při spouštění úloh MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-169">**SSH keys for SLES VMs**: Generate SSH keys to establish trust for your user account among the compute nodes in the SLES cluster when running MPI jobs.</span></span> <span data-ttu-id="fca3c-170">Pokud nasadíte virtuální počítač na základě CentOS HPC, nemusíte postupujte podle tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="fca3c-170">If you deployed a CentOS-based HPC VM, don't follow this step.</span></span> <span data-ttu-id="fca3c-171">Zobrazí pokyny později v tomto článku nastavit passwordless SSH vztah důvěryhodnosti mezi uzly clusteru po zachycení bitové kopie a nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-171">See instructions later in this article to set up passwordless SSH trust among the cluster nodes after you capture the image and deploy the cluster.</span></span>

    <span data-ttu-id="fca3c-172">K vytvoření klíčů SSH, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="fca3c-172">To create SSH keys, run the following command.</span></span> <span data-ttu-id="fca3c-173">Po zobrazení výzvy pro vstup, vyberte **Enter** ke generování klíče ve výchozím umístění bez nastavení hesla.</span><span class="sxs-lookup"><span data-stu-id="fca3c-173">When you are prompted for input, select **Enter** to generate the keys in the default location without setting a password.</span></span>

        ssh-keygen

    <span data-ttu-id="fca3c-174">Připojí veřejný klíč v souboru authorized_keys známé veřejných klíčů.</span><span class="sxs-lookup"><span data-stu-id="fca3c-174">Append the public key to the authorized_keys file for known public keys.</span></span>

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    <span data-ttu-id="fca3c-175">V adresáři ~/.ssh upravovat nebo vytvářet do konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-175">In the ~/.ssh directory, edit or create the config file.</span></span> <span data-ttu-id="fca3c-176">Zadejte rozsah IP adres privátní sítě, kterou chcete použít v Azure (v tomto příkladu 10.32.0.0/16):</span><span class="sxs-lookup"><span data-stu-id="fca3c-176">Provide the IP address range of the private network that you plan to use in Azure (10.32.0.0/16 in this example):</span></span>

        host 10.32.0.*
        StrictHostKeyChecking no

    <span data-ttu-id="fca3c-177">Případně seznam IP adresu privátní sítě jednotlivé virtuální počítače v clusteru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fca3c-177">Alternatively, list the private network IP address of each VM in your cluster as follows:</span></span>

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > <span data-ttu-id="fca3c-178">Konfigurace `StrictHostKeyChecking no` můžete vytvořit představuje potenciální bezpečnostní riziko, pokud není zadán konkrétní IP adresu nebo rozsah.</span><span class="sxs-lookup"><span data-stu-id="fca3c-178">Configuring `StrictHostKeyChecking no` can create a potential security risk when a specific IP address or range is not specified.</span></span>
  >
  >
* <span data-ttu-id="fca3c-179">**Aplikace**: Nainstalujte aplikace potřebují nebo provedení jiných úprav, než zachytit bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="fca3c-179">**Applications**: Install any applications you need or perform other customizations before you capture the image.</span></span>

### <a name="capture-the-image"></a><span data-ttu-id="fca3c-180">Zachycení bitové kopie</span><span class="sxs-lookup"><span data-stu-id="fca3c-180">Capture the image</span></span>
<span data-ttu-id="fca3c-181">K zachycení bitové kopie, nejprve spusťte následující příkaz na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="fca3c-181">To capture the image, first run the following command on the Linux VM.</span></span> <span data-ttu-id="fca3c-182">Tento příkaz deprovisions virtuální počítač ale uchovává uživatelské účty a klíče SSH, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="fca3c-182">This command deprovisions the VM but maintains user accounts and SSH keys that you set up.</span></span>

```
sudo waagent -deprovision
```

<span data-ttu-id="fca3c-183">Na klientském počítači spusťte následující příkazy rozhraní příkazového řádku Azure k zachycení bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="fca3c-183">From your client computer, run the following Azure CLI commands to capture the image.</span></span> <span data-ttu-id="fca3c-184">Další informace najdete v tématu [jak zachytit klasický Linuxový virtuální počítač jako image](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="fca3c-184">For more information, see [How to capture a classic Linux virtual machine as an image](capture-image.md).</span></span>  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

<span data-ttu-id="fca3c-185">Po spuštění těchto příkazů zachycenou image virtuálního počítače pro vaše použití a odstranění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-185">After you run these commands, the VM image is captured for your use and the VM is deleted.</span></span> <span data-ttu-id="fca3c-186">Nyní máte vlastní bitovou kopii připraveny k nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-186">Now you have your custom image ready to deploy a cluster.</span></span>

### <a name="deploy-a-cluster-with-the-image"></a><span data-ttu-id="fca3c-187">Nasazení clusteru s obrázkem</span><span class="sxs-lookup"><span data-stu-id="fca3c-187">Deploy a cluster with the image</span></span>
<span data-ttu-id="fca3c-188">Upravte následující skript Bash s příslušnými hodnotami pro vaše prostředí a spusťte jej z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-188">Modify the following Bash script with appropriate values for your environment and run it from your client computer.</span></span> <span data-ttu-id="fca3c-189">Protože Azure nasadí virtuálních počítačů sériově v modelu nasazení classic, trvá několik minut nasazení osm virtuálních počítačů A9 navržený v tento skript.</span><span class="sxs-lookup"><span data-stu-id="fca3c-189">Because Azure deploys the VMs serially in the classic deployment model, it takes a few minutes to deploy the eight A9 VMs suggested in this script.</span></span>

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a><span data-ttu-id="fca3c-190">Důležité informace pro cluster prostředí HPC CentOS</span><span class="sxs-lookup"><span data-stu-id="fca3c-190">Considerations for a CentOS HPC cluster</span></span>
<span data-ttu-id="fca3c-191">Pokud chcete nastavení clusteru na základě jedné z bitové kopie založené na CentOS HPC v Azure Marketplace místo SLES 12 pro prostředí HPC, postupujte podle obecné kroky v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="fca3c-191">If you want to set up a cluster based on one of the CentOS-based HPC images in the Azure Marketplace instead of SLES 12 for HPC, follow the general steps in the preceding section.</span></span> <span data-ttu-id="fca3c-192">Při zřizování a konfiguraci virtuálního počítače, Všimněte si následujících rozdílů:</span><span class="sxs-lookup"><span data-stu-id="fca3c-192">Note the following differences when you provision and configure the VM:</span></span>

- <span data-ttu-id="fca3c-193">Intel MPI je již nainstalován na virtuální počítač z bitové kopie založené na CentOS HPC zřízený.</span><span class="sxs-lookup"><span data-stu-id="fca3c-193">Intel MPI is already installed on a VM provisioned from a CentOS-based HPC image.</span></span>
- <span data-ttu-id="fca3c-194">Nastavení uzamčení paměti jsou již přidán do souboru /etc/security/limits.conf Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fca3c-194">Lock memory settings are already added in the VM's /etc/security/limits.conf file.</span></span>
- <span data-ttu-id="fca3c-195">Nedojde k vytvoření klíčů SSH na virtuálním počítači, zřídíte pro zachycení.</span><span class="sxs-lookup"><span data-stu-id="fca3c-195">Do not generate SSH keys on the VM you provision for capture.</span></span> <span data-ttu-id="fca3c-196">Místo toho doporučujeme nastavení ověřování na základě uživatele po nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-196">Instead, we recommend setting up user-based authentication after you deploy the cluster.</span></span> <span data-ttu-id="fca3c-197">Další informace najdete v následující části.</span><span class="sxs-lookup"><span data-stu-id="fca3c-197">For more information, see the following section.</span></span>  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a><span data-ttu-id="fca3c-198">Nastavení passwordless SSH důvěryhodnosti v clusteru</span><span class="sxs-lookup"><span data-stu-id="fca3c-198">Set up passwordless SSH trust on the cluster</span></span>
<span data-ttu-id="fca3c-199">V clusteru HPC na základě CentOS, existují dvě metody pro vytvoření vztahu důvěryhodnosti mezi výpočetní uzly: ověřování na základě hostitele a ověřování na základě uživatele.</span><span class="sxs-lookup"><span data-stu-id="fca3c-199">On a CentOS-based HPC cluster, there are two methods for establishing trust between the compute nodes: host-based authentication and user-based authentication.</span></span> <span data-ttu-id="fca3c-200">Ověřování na základě hostitele je mimo rámec tohoto článku a obecně je třeba provést pomocí skriptu rozšíření během nasazení.</span><span class="sxs-lookup"><span data-stu-id="fca3c-200">Host-based authentication is outside of the scope of this article and generally must be done through an extension script during deployment.</span></span> <span data-ttu-id="fca3c-201">Ověřování na základě uživatele je vhodné pro navázání vztahu důvěryhodnosti po nasazení a vyžaduje generování a sdílení klíčů SSH mezi výpočetní uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-201">User-based authentication is convenient for establishing trust after deployment and requires the generation and sharing of SSH keys among the compute nodes in the cluster.</span></span> <span data-ttu-id="fca3c-202">Tato metoda se běžně označuje jako passwordless přihlašování přes SSH a je požadován při spouštění úloh MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-202">This method is commonly known as passwordless SSH login and is required when running MPI jobs.</span></span>

<span data-ttu-id="fca3c-203">Ukázkový skript podílí od komunity je k dispozici na [Githubu](https://github.com/tanewill/utils/blob/master/user_authentication.sh) k povolení ověřování snadno uživatele na základě CentOS HPC clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-203">A sample script contributed from the community is available on [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) to enable easy user authentication on a CentOS-based HPC cluster.</span></span> <span data-ttu-id="fca3c-204">Stáhnout a použít tento skript pomocí následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="fca3c-204">Download and use this script by using the following steps.</span></span> <span data-ttu-id="fca3c-205">Můžete také upravit tento skript nebo použít jinou metodu pro vytvoření passwordless ověřování SSH mezi výpočetní uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-205">You can also modify this script or use any other method to establish passwordless SSH authentication between the cluster compute nodes.</span></span>

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

<span data-ttu-id="fca3c-206">Pokud chcete spustit skript, musíte znát Předpona podsítě IP adres.</span><span class="sxs-lookup"><span data-stu-id="fca3c-206">To run the script, you need to know the prefix for your subnet IP addresses.</span></span> <span data-ttu-id="fca3c-207">Předpona získáte spuštěním následujícího příkazu na jednom z uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-207">Get the prefix by running the following command on one of the cluster nodes.</span></span> <span data-ttu-id="fca3c-208">Výstup by měl vypadat podobně jako 10.1.3.5 a předpona je 10.1.3 část.</span><span class="sxs-lookup"><span data-stu-id="fca3c-208">Your output should look something like 10.1.3.5, and the prefix is the 10.1.3 portion.</span></span>

    ifconfig eth0 | grep -w inet | awk '{print $2}'

<span data-ttu-id="fca3c-209">Nyní spusťte skript pomocí tři parametry: běžné uživatelské jméno na výpočetních uzlech, společné heslo pro daného uživatele na výpočetní uzly a předponu podsítě, který byl vrácen z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="fca3c-209">Now run the script using three parameters: the common user name on the compute nodes, the common password for that user on the compute nodes, and the subnet prefix that was returned from the previous command.</span></span>

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

<span data-ttu-id="fca3c-210">Skript provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="fca3c-210">This script does the following:</span></span>

* <span data-ttu-id="fca3c-211">Vytvoří adresář na uzlu hostitele s názvem .ssh, což je vyžadováno pro passwordless přihlášení.</span><span class="sxs-lookup"><span data-stu-id="fca3c-211">Creates a directory on the host node named .ssh, which is required for passwordless login.</span></span>
* <span data-ttu-id="fca3c-212">Vytvoří konfigurační soubor v adresáři .ssh obsahující pokyn passwordless přihlášení k povolení přihlášení z libovolného uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-212">Creates a configuration file in the .ssh directory that instructs passwordless login to allow login from any node in the cluster.</span></span>
* <span data-ttu-id="fca3c-213">Vytvoří soubory obsahující názvy a uzel IP adresy pro všechny uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-213">Creates files containing the node names and node IP addresses for all the nodes in the cluster.</span></span> <span data-ttu-id="fca3c-214">Tyto soubory jsou ponechány po spuštění skriptu pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="fca3c-214">These files are left after the script is run for later reference.</span></span>
* <span data-ttu-id="fca3c-215">Vytvoří pár klíčů privátní a veřejné pro každý uzel clusteru (včetně uzlu hostitele) a vytvoří záznamy v souboru authorized_keys.</span><span class="sxs-lookup"><span data-stu-id="fca3c-215">Creates a private and public key pair for each cluster node (including the host node) and creates entries in the authorized_keys file.</span></span>

> [!WARNING]
> <span data-ttu-id="fca3c-216">Spuštěním tohoto skriptu můžete vytvořit představuje potenciální bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="fca3c-216">Running this script can create a potential security risk.</span></span> <span data-ttu-id="fca3c-217">Ujistěte se, že není informací veřejného klíče v ~/.ssh distribuován.</span><span class="sxs-lookup"><span data-stu-id="fca3c-217">Ensure that the public key information in ~/.ssh is not distributed.</span></span>
>
>

## <a name="configure-intel-mpi"></a><span data-ttu-id="fca3c-218">Konfigurace Intel MPI</span><span class="sxs-lookup"><span data-stu-id="fca3c-218">Configure Intel MPI</span></span>
<span data-ttu-id="fca3c-219">Pro spouštění aplikací MPI v Azure Linux RDMA, musíte nakonfigurovat některé specifické pro Intel MPI proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="fca3c-219">To run MPI applications on Azure Linux RDMA, you need to configure certain environment variables specific to Intel MPI.</span></span> <span data-ttu-id="fca3c-220">Tady je ukázkový skript Bash nakonfigurovat proměnné, které jsou potřebné ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="fca3c-220">Here is a sample Bash script to configure the variables needed to run an application.</span></span> <span data-ttu-id="fca3c-221">Změňte cestu k mpivars.sh podle potřeby pro instalaci nástroje Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-221">Change the path to mpivars.sh as needed for your installation of Intel MPI.</span></span>

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

<span data-ttu-id="fca3c-222">Formát souboru hostitele je následující.</span><span class="sxs-lookup"><span data-stu-id="fca3c-222">The format of the host file is as follows.</span></span> <span data-ttu-id="fca3c-223">Přidejte jeden řádek pro každý uzel v clusteru.</span><span class="sxs-lookup"><span data-stu-id="fca3c-223">Add one line for each node in your cluster.</span></span> <span data-ttu-id="fca3c-224">Zadejte, že privátní IP adresy z virtuální sítě definovaného dříve, není názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="fca3c-224">Specify private IP addresses from the virtual network defined earlier, not DNS names.</span></span> <span data-ttu-id="fca3c-225">Například na dva hostitele s IP adresami 10.32.0.1 a 10.32.0.2, soubor obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="fca3c-225">For example, for two hosts with IP addresses 10.32.0.1 and 10.32.0.2, the file contains the following:</span></span>

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a><span data-ttu-id="fca3c-226">Spustit MPI na Základní dvojuzlový cluster</span><span class="sxs-lookup"><span data-stu-id="fca3c-226">Run MPI on a basic two-node cluster</span></span>
<span data-ttu-id="fca3c-227">Pokud jste tak již neučinili, nejprve nastavení prostředí pro Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-227">If you haven't already done so, first set up the environment for Intel MPI.</span></span>

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a><span data-ttu-id="fca3c-228">Spusťte příkaz MPI</span><span class="sxs-lookup"><span data-stu-id="fca3c-228">Run an MPI command</span></span>
<span data-ttu-id="fca3c-229">Spusťte příkaz MPI na jednom z výpočetních uzlů k MPI, je správně nainstalována a může komunikovat mezi alespoň že dva výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="fca3c-229">Run an MPI command on one of the compute nodes to show that MPI is installed properly and can communicate between at least two compute nodes.</span></span> <span data-ttu-id="fca3c-230">Následující **mpirun** příkaz spustí **hostname** na dvou uzlech.</span><span class="sxs-lookup"><span data-stu-id="fca3c-230">The following **mpirun** command runs the **hostname** command on two nodes.</span></span>

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
<span data-ttu-id="fca3c-231">Vaše výstup by měl obsahovat názvy všech uzlů, které předávají jako vstup pro `-hosts`.</span><span class="sxs-lookup"><span data-stu-id="fca3c-231">Your output should list the names of all the nodes that you passed as input for `-hosts`.</span></span> <span data-ttu-id="fca3c-232">Například **mpirun** příkaz s dvěma uzly vrátí výstup takto:</span><span class="sxs-lookup"><span data-stu-id="fca3c-232">For example, an **mpirun** command with two nodes returns output like the following:</span></span>

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a><span data-ttu-id="fca3c-233">Spuštění MPI srovnávacího testu</span><span class="sxs-lookup"><span data-stu-id="fca3c-233">Run an MPI benchmark</span></span>
<span data-ttu-id="fca3c-234">Následující příkaz Intel MPI se spustí pingpong srovnávací test pro ověření konfigurace clusteru a připojení k síti RDMA.</span><span class="sxs-lookup"><span data-stu-id="fca3c-234">The following Intel MPI command runs a pingpong benchmark to verify the cluster configuration and connection to the RDMA network.</span></span>

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

<span data-ttu-id="fca3c-235">Na pracovní clusteru se dvěma uzly měli byste vidět výstup jako následující.</span><span class="sxs-lookup"><span data-stu-id="fca3c-235">On a working cluster with two nodes, you should see output like the following.</span></span> <span data-ttu-id="fca3c-236">V síti Azure RDMA očekávejte, že latenci nebo pod 3 mikrosekundách zprávy velikosti až 512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="fca3c-236">On the Azure RDMA network, expect latency at or below 3 microseconds for message sizes up to 512 bytes.</span></span>

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a><span data-ttu-id="fca3c-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fca3c-237">Next steps</span></span>
* <span data-ttu-id="fca3c-238">Nasazení a spuštění vaší Linux MPI aplikace na cluster systému Linux.</span><span class="sxs-lookup"><span data-stu-id="fca3c-238">Deploy and run your Linux MPI applications on your Linux cluster.</span></span>
* <span data-ttu-id="fca3c-239">Najdete v článku [dokumentaci ke knihovně MPI Intel](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) pokyny k Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="fca3c-239">See the [Intel MPI Library documentation](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) for guidance on Intel MPI.</span></span>
* <span data-ttu-id="fca3c-240">Zkuste [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) k vytvoření clusteru Intel Lustre pomocí bitové kopie založené na CentOS HPC.</span><span class="sxs-lookup"><span data-stu-id="fca3c-240">Try a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) to create an Intel Lustre cluster by using a CentOS-based HPC image.</span></span> <span data-ttu-id="fca3c-241">Podrobnosti najdete v tématu [nasazení Intel cloudu Edition pro počítače s Lustre v Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="fca3c-241">For details, see [Deploying Intel Cloud Edition for Lustre on Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span></span>
