---
title: "aaaSet až aplikací MPI toorun clusteru Linux RDMA | Microsoft Docs"
description: "Vytvořit cluster Linux velikost H16r, H16mr, A8 a A9 VMs toouse hello Azure RDMA sítě toorun MPI aplikace"
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
ms.openlocfilehash: 3199317a37b095e80718d6724954687d30aea3a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-linux-rdma-cluster-toorun-mpi-applications"></a><span data-ttu-id="adc4e-103">Nastavení aplikací MPI toorun clusteru Linux RDMA</span><span class="sxs-lookup"><span data-stu-id="adc4e-103">Set up a Linux RDMA cluster toorun MPI applications</span></span>
<span data-ttu-id="adc4e-104">Zjistěte, jak tooset až Linux RDMA cluster v Azure s [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun paralelních aplikací Message Passing Interface (MPI).</span><span class="sxs-lookup"><span data-stu-id="adc4e-104">Learn how tooset up a Linux RDMA cluster in Azure with [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toorun parallel Message Passing Interface (MPI) applications.</span></span> <span data-ttu-id="adc4e-105">Tento článek obsahuje kroky tooprepare toorun bitové kopie prostředí HPC Linux Intel MPI v clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc4e-105">This article provides steps tooprepare a Linux HPC image toorun Intel MPI on a cluster.</span></span> <span data-ttu-id="adc4e-106">Po přípravě nasazení clusteru virtuálních počítačů pomocí tuto bitovou kopii a jeden velikostí hello podporující RDMA virtuálních počítačů Azure (aktuálně H16r, H16mr, A8 a A9).</span><span class="sxs-lookup"><span data-stu-id="adc4e-106">After preparation, you deploy a cluster of VMs using this image and one of hello RDMA-capable Azure VM sizes (currently H16r, H16mr, A8, or A9).</span></span> <span data-ttu-id="adc4e-107">Použití hello clusteru toorun aplikací MPI, které efektivně komunikovat přes nízkou latencí, vysokou propustnost sítě na základě přímého přístupu do paměti (vzdáleného počítače RDMA) technologie.</span><span class="sxs-lookup"><span data-stu-id="adc4e-107">Use hello cluster toorun MPI applications that communicate efficiently over a low-latency, high-throughput network based on remote direct memory access (RDMA) technology.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adc4e-108">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic.</span><span class="sxs-lookup"><span data-stu-id="adc4e-108">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="adc4e-109">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="adc4e-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

## <a name="cluster-deployment-options"></a><span data-ttu-id="adc4e-111">Možnosti nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="adc4e-111">Cluster deployment options</span></span>
<span data-ttu-id="adc4e-112">Toto jsou metody toocreate clusteru s podporou Linux RDMA můžete použít s nebo bez Plánovač úloh.</span><span class="sxs-lookup"><span data-stu-id="adc4e-112">Following are methods you can use toocreate a Linux RDMA cluster with or without a job scheduler.</span></span>

* <span data-ttu-id="adc4e-113">**Azure CLI skripty**: Jak uvidíte později v tomto článku, použijte hello [rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md) (CLI) tooscript hello nasazení clusteru s podporou RDMA podporovat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-113">**Azure CLI scripts**: As shown later in this article, use hello [Azure command-line interface](../../../cli-install-nodejs.md) (CLI) tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span> <span data-ttu-id="adc4e-114">Hello rozhraní příkazového řádku v režimu správy služby se vytvoří hello uzly clusteru sériově v modelu nasazení classic hello, takže nasazení mnoho výpočetních uzlů může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="adc4e-114">hello CLI in Service Management mode creates hello cluster nodes serially in hello classic deployment model, so deploying many compute nodes might take several minutes.</span></span> <span data-ttu-id="adc4e-115">tooenable hello síťového připojení RDMA při použití modelu nasazení classic hello, nasazení virtuálních počítačů hello v hello stejné cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="adc4e-115">tooenable hello RDMA network connection when you use hello classic deployment model, deploy hello VMs in hello same cloud service.</span></span>
* <span data-ttu-id="adc4e-116">**Šablony Azure Resource Manageru**: Můžete taky hello Resource Manager nasazení modelu toodeploy clusteru podporující RDMA virtuálních počítačů, který se připojuje toohello RDMA sítě.</span><span class="sxs-lookup"><span data-stu-id="adc4e-116">**Azure Resource Manager templates**: You can also use hello Resource Manager deployment model toodeploy a cluster of RDMA-capable VMs that connects toohello RDMA network.</span></span> <span data-ttu-id="adc4e-117">Můžete [vytvořit vlastní šablonu](../../../resource-group-authoring-templates.md), nebo zkontrolujte hello [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/) pro šablony přispěli Microsoft hello komunity toodeploy hello řešení nebo chcete.</span><span class="sxs-lookup"><span data-stu-id="adc4e-117">You can [create your own template](../../../resource-group-authoring-templates.md), or check hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/) for templates contributed by Microsoft or hello community toodeploy hello solution you want.</span></span> <span data-ttu-id="adc4e-118">Šablony Resource Manageru nabízejí rychlý a spolehlivý toodeploy Linux cluster.</span><span class="sxs-lookup"><span data-stu-id="adc4e-118">Resource Manager templates can provide a fast and reliable way toodeploy a Linux cluster.</span></span> <span data-ttu-id="adc4e-119">tooenable hello síťového připojení RDMA při použití hello modelu nasazení Resource Manager, nasazení virtuálních počítačů hello v hello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="adc4e-119">tooenable hello RDMA network connection when you use hello Resource Manager deployment model, deploy hello VMs in hello same availability set.</span></span>
* <span data-ttu-id="adc4e-120">**HPC Pack**: vytvoření clusteru s podporou sady Microsoft HPC Pack v Azure a přidat podporu rdma výpočetních uzlech, které spustit podporované hello RDMA síť tooaccess distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="adc4e-120">**HPC Pack**: Create a Microsoft HPC Pack cluster in Azure and add RDMA-capable compute nodes that run a supported Linux distribution tooaccess hello RDMA network.</span></span> <span data-ttu-id="adc4e-121">Další informace najdete v tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="adc4e-121">For more information, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="sample-deployment-steps-in-hello-classic-model"></a><span data-ttu-id="adc4e-122">Ukázka nasazení kroky v modelu classic hello</span><span class="sxs-lookup"><span data-stu-id="adc4e-122">Sample deployment steps in hello classic model</span></span>
<span data-ttu-id="adc4e-123">Hello následující kroky ukazují, jak přizpůsobit toouse hello rozhraní příkazového řádku Azure toodeploy virtuální počítač SUSE Linux Enterprise Server (SLES) 12 SP1 HPC z hello Azure Marketplace a vytvořit vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="adc4e-123">hello following steps show how toouse hello Azure CLI toodeploy a SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM from hello Azure Marketplace, customize it, and create a custom VM image.</span></span> <span data-ttu-id="adc4e-124">Potom můžete image hello tooscript hello nasazení clusteru s podporou RDMA podporovat virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-124">Then you can use hello image tooscript hello deployment of a cluster of RDMA-capable VMs.</span></span>

> [!TIP]
> <span data-ttu-id="adc4e-125">Použijte podobné toodeploy kroky, které clusteru s podporou RDMA podporovat virtuálních počítačů založené na imagích na základě CentOS HPC v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="adc4e-125">Use similar steps toodeploy a cluster of RDMA-capable VMs based on CentOS-based HPC images in hello Azure Marketplace.</span></span> <span data-ttu-id="adc4e-126">Některé kroky poněkud, jak jsme uvedli lišit.</span><span class="sxs-lookup"><span data-stu-id="adc4e-126">Some steps differ slightly, as noted.</span></span> 
>
>

### <a name="prerequisites"></a><span data-ttu-id="adc4e-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="adc4e-127">Prerequisites</span></span>
* <span data-ttu-id="adc4e-128">**Klientský počítač**: budete potřebovat toocommunicate počítače Mac, Linux nebo Windows klienta s Azure.</span><span class="sxs-lookup"><span data-stu-id="adc4e-128">**Client computer**: You need a Mac, Linux, or Windows client computer toocommunicate with Azure.</span></span> <span data-ttu-id="adc4e-129">Tento postup předpokládá, že používáte klienta Linux.</span><span class="sxs-lookup"><span data-stu-id="adc4e-129">These steps assume you are using a Linux client.</span></span>
* <span data-ttu-id="adc4e-130">**Předplatné Azure**: Pokud nemáte předplatné, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="adc4e-130">**Azure subscription**: If you don't have a subscription, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span> <span data-ttu-id="adc4e-131">Pro větší clustery zvažte průběžnými platbami předplatné nebo jiné možnosti nákupu.</span><span class="sxs-lookup"><span data-stu-id="adc4e-131">For larger clusters, consider a pay-as-you-go subscription or other purchase options.</span></span>
* <span data-ttu-id="adc4e-132">**Virtuální počítač velikost dostupnosti**: hello následující instance velikosti jsou podporující RDMA: H16r, H16mr, A8 a A9.</span><span class="sxs-lookup"><span data-stu-id="adc4e-132">**VM size availability**: hello following instance sizes are RDMA capable: H16r, H16mr, A8, and A9.</span></span> <span data-ttu-id="adc4e-133">Zkontrolujte [produkty podle oblasti](https://azure.microsoft.com/regions/services/) pro dostupnost v oblastech Azure.</span><span class="sxs-lookup"><span data-stu-id="adc4e-133">Check [Products available by region](https://azure.microsoft.com/regions/services/) for availability in Azure regions.</span></span>
* <span data-ttu-id="adc4e-134">**Kvóta jader**: může být nutné tooincrease hello kvóty jader toodeploy cluster virtuálních počítačů náročné.</span><span class="sxs-lookup"><span data-stu-id="adc4e-134">**Cores quota**: You might need tooincrease hello quota of cores toodeploy a cluster of compute-intensive VMs.</span></span> <span data-ttu-id="adc4e-135">Pokud chcete, aby toodeploy 8 A9 virtuálních počítačů, jak je znázorněno v tomto článku se například potřebovat nejméně 128 jader.</span><span class="sxs-lookup"><span data-stu-id="adc4e-135">For example, you need at least 128 cores if you want toodeploy 8 A9 VMs as shown in this article.</span></span> <span data-ttu-id="adc4e-136">Vaše předplatné může také omezit hello počet jader, který můžete nasadit v určité rodiny velikost virtuálního počítače, včetně hello H-series.</span><span class="sxs-lookup"><span data-stu-id="adc4e-136">Your subscription might also limit hello number of cores you can deploy in certain VM size families, including hello H-series.</span></span> <span data-ttu-id="adc4e-137">toorequest kvótu zvýšit, [otevřete žádosti o podporu online zákazníka](../../../azure-supportability/how-to-create-azure-support-request.md) zdarma.</span><span class="sxs-lookup"><span data-stu-id="adc4e-137">toorequest a quota increase, [open an online customer support request](../../../azure-supportability/how-to-create-azure-support-request.md) at no charge.</span></span>
* <span data-ttu-id="adc4e-138">**Rozhraní příkazového řádku Azure**: [nainstalovat](../../../cli-install-nodejs.md) hello rozhraní příkazového řádku Azure a [připojit tooyour předplatného Azure](../../../xplat-cli-connect.md) z hello klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="adc4e-138">**Azure CLI**: [Install](../../../cli-install-nodejs.md) hello Azure CLI and [connect tooyour Azure subscription](../../../xplat-cli-connect.md) from hello client computer.</span></span>

### <a name="provision-an-sles-12-sp1-hpc-vm"></a><span data-ttu-id="adc4e-139">Zřizování virtuálních počítačů SLES 12 SP1 HPC</span><span class="sxs-lookup"><span data-stu-id="adc4e-139">Provision an SLES 12 SP1 HPC VM</span></span>
<span data-ttu-id="adc4e-140">Po přihlášení tooAzure s hello příkazového řádku Azure CLI, spusťte `azure config list` tooconfirm, který hello výstup ukazuje režimu správy služby.</span><span class="sxs-lookup"><span data-stu-id="adc4e-140">After signing in tooAzure with hello Azure CLI, run `azure config list` tooconfirm that hello output shows Service Management mode.</span></span> <span data-ttu-id="adc4e-141">Pokud ne, nastavení režimu hello spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="adc4e-141">If it does not, set hello mode by running this command:</span></span>

    azure config mode asm


<span data-ttu-id="adc4e-142">Zadejte všechny odběry hello jsou autorizovaní toouse hello toolist následující:</span><span class="sxs-lookup"><span data-stu-id="adc4e-142">Type hello following toolist all hello subscriptions you are authorized toouse:</span></span>

    azure account list

<span data-ttu-id="adc4e-143">Hello aktuální aktivní předplatné, je označen `Current` nastavit příliš`true`.</span><span class="sxs-lookup"><span data-stu-id="adc4e-143">hello current active subscription is identified with `Current` set too`true`.</span></span> <span data-ttu-id="adc4e-144">Pokud toto předplatné není hello, kterou má být toouse toocreate hello cluster, nastavit ID hello příslušné předplatné jako aktivní předplatné hello:</span><span class="sxs-lookup"><span data-stu-id="adc4e-144">If this subscription isn't hello one you want toouse toocreate hello cluster, set hello appropriate subscription ID as hello active subscription:</span></span>

    azure account set <subscription-Id>

<span data-ttu-id="adc4e-145">toosee hello veřejně dostupné SLES 12 SP1 HPC bitové kopie v Azure, spusťte příkaz jako hello následující, za předpokladu, že vaše prostředí podporuje prostředí **grep**:</span><span class="sxs-lookup"><span data-stu-id="adc4e-145">toosee hello publicly available SLES 12 SP1 HPC images in Azure, run a command like hello following, assuming your shell environment supports **grep**:</span></span>

    azure vm image list | grep "suse.*hpc"

<span data-ttu-id="adc4e-146">Zřídit podporou RDMA virtuální počítač s bitovou kopii SLES 12 SP1 HPC spuštěním příkazu jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="adc4e-146">Provision an RDMA-capable VM with a SLES 12 SP1 HPC image by running a command like hello following:</span></span>

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

<span data-ttu-id="adc4e-147">Kde:</span><span class="sxs-lookup"><span data-stu-id="adc4e-147">Where:</span></span>

* <span data-ttu-id="adc4e-148">Hello velikost (v tomto příkladu A9) je jedním z velikosti virtuálních počítačů hello RDMA podporovat.</span><span class="sxs-lookup"><span data-stu-id="adc4e-148">hello size (A9 in this example) is one of hello RDMA-capable VM sizes.</span></span>
* <span data-ttu-id="adc4e-149">Hello externí SSH port číslo (22 v tomto příkladu, což je výchozí SSH hello) je platné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="adc4e-149">hello external SSH port number (22 in this example, which is hello SSH default) is any valid port number.</span></span> <span data-ttu-id="adc4e-150">číslo portu SSH interní Hello nastavena too22.</span><span class="sxs-lookup"><span data-stu-id="adc4e-150">hello internal SSH port number is set too22.</span></span>
* <span data-ttu-id="adc4e-151">Nová Cloudová služba je vytvořená ve hello určeného umístění hello oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="adc4e-151">A new cloud service is created in hello Azure region specified by hello location.</span></span> <span data-ttu-id="adc4e-152">Zadejte umístění, ve které hello virtuálního počítače je k dispozici velikost, které zvolíte.</span><span class="sxs-lookup"><span data-stu-id="adc4e-152">Specify a location in which hello VM size you choose is available.</span></span>
* <span data-ttu-id="adc4e-153">Pro podporu s prioritou SUSE (který budou vám účtovány dodatečné poplatky) název bitové kopie hello SLES 12 SP1 aktuálně může být jedna z těchto dvou možností:</span><span class="sxs-lookup"><span data-stu-id="adc4e-153">For SUSE priority support (which incurs additional charges), hello SLES 12 SP1 image name currently can be one of these two options:</span></span> 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-hello-vm"></a><span data-ttu-id="adc4e-154">Přizpůsobení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="adc4e-154">Customize hello VM</span></span>
<span data-ttu-id="adc4e-155">Po dokončení zřizování hello virtuálních počítačů SSH toohello virtuálních počítačů pomocí hello externí IP adresu Virtuálního počítače (nebo název DNS) a hello externí port číslo jste nakonfigurovali a pak ho přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="adc4e-155">After hello VM finishes provisioning, SSH toohello VM by using hello VM's external IP address (or DNS name) and hello external port number you configured, and then customize it.</span></span> <span data-ttu-id="adc4e-156">Podrobnosti připojení najdete v tématu [jak toolog na tooa virtuální počítač se systémem Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="adc4e-156">For connection details, see [How toolog on tooa virtual machine running Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="adc4e-157">Proveďte příkazy hello uživatele, které jste nakonfigurovali na hello virtuálních počítačů, pokud je kořenový přístup vyžaduje toocomplete krok.</span><span class="sxs-lookup"><span data-stu-id="adc4e-157">Perform commands as hello user you configured on hello VM, unless root access is required toocomplete a step.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="adc4e-158">Microsoft Azure neposkytuje kořenový přístup tooLinux virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-158">Microsoft Azure does not provide root access tooLinux VMs.</span></span> <span data-ttu-id="adc4e-159">toogain přístup pro správu při připojení jako uživatel toohello virtuálního počítače, spusťte příkazy pomocí `sudo`.</span><span class="sxs-lookup"><span data-stu-id="adc4e-159">toogain administrative access when connected as a user toohello VM, run commands by using `sudo`.</span></span>
>
>

* <span data-ttu-id="adc4e-160">**Aktualizace**: nainstalujte aktualizace pomocí zypper.</span><span class="sxs-lookup"><span data-stu-id="adc4e-160">**Updates**: Install updates by using zypper.</span></span> <span data-ttu-id="adc4e-161">Také můžete chtít tooinstall nástrojů systému souborů NFS.</span><span class="sxs-lookup"><span data-stu-id="adc4e-161">You might also want tooinstall NFS utilities.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="adc4e-162">V SLES 12 SP1 HPC virtuální počítač doporučujeme nepoužijete jádra aktualizace, které může způsobit problémy s hello Linux RDMA ovladače.</span><span class="sxs-lookup"><span data-stu-id="adc4e-162">In a SLES 12 SP1 HPC VM, we recommend that you don't apply kernel updates, which can cause issues with hello Linux RDMA drivers.</span></span>
  >
  >
* <span data-ttu-id="adc4e-163">**Intel MPI**: dokončí instalaci hello Intel MPI v hello SLES 12 SP1 HPC virtuálních počítačů tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="adc4e-163">**Intel MPI**: Complete hello installation of Intel MPI on hello SLES 12 SP1 HPC VM by running hello following command:</span></span>

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* <span data-ttu-id="adc4e-164">**Uzamknout paměť**: MPI kódy toolock hello paměti k dispozici pro RDMA, přidat nebo změnit následující nastavení v souboru /etc/security/limits.conf hello hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-164">**Lock memory**: For MPI codes toolock hello memory available for RDMA, add or change hello following settings in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="adc4e-165">Budete potřebovat kořenový přístup tooedit tento soubor.</span><span class="sxs-lookup"><span data-stu-id="adc4e-165">You need root access tooedit this file.</span></span>

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > <span data-ttu-id="adc4e-166">Pro účely testování můžete také nastavit memlock toounlimited.</span><span class="sxs-lookup"><span data-stu-id="adc4e-166">For testing purposes, you can also set memlock toounlimited.</span></span> <span data-ttu-id="adc4e-167">Například: `<User or group name>    hard    memlock unlimited`.</span><span class="sxs-lookup"><span data-stu-id="adc4e-167">For example: `<User or group name>    hard    memlock unlimited`.</span></span> <span data-ttu-id="adc4e-168">Další informace najdete v tématu [známé nejlepší metody pro nastavení uzamčení velikost paměti](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span><span class="sxs-lookup"><span data-stu-id="adc4e-168">For more information, see [Best known methods for setting locked memory size](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).</span></span>
  >
  >
* <span data-ttu-id="adc4e-169">**Klíče SSH pro virtuální počítače SLES**: generování SSH klíče tooestablish vztahu důvěryhodnosti pro váš uživatelský účet mezi hello výpočetní uzly v clusteru SLES hello při spouštění úloh MPI.</span><span class="sxs-lookup"><span data-stu-id="adc4e-169">**SSH keys for SLES VMs**: Generate SSH keys tooestablish trust for your user account among hello compute nodes in hello SLES cluster when running MPI jobs.</span></span> <span data-ttu-id="adc4e-170">Pokud nasadíte virtuální počítač na základě CentOS HPC, nemusíte postupujte podle tohoto kroku.</span><span class="sxs-lookup"><span data-stu-id="adc4e-170">If you deployed a CentOS-based HPC VM, don't follow this step.</span></span> <span data-ttu-id="adc4e-171">Po zachycení bitové kopie hello a nasazení hello clusteru, najdete v pokynech později v tooset Tento článek se passwordless SSH vztah důvěryhodnosti mezi uzly clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-171">See instructions later in this article tooset up passwordless SSH trust among hello cluster nodes after you capture hello image and deploy hello cluster.</span></span>

    <span data-ttu-id="adc4e-172">klíče SSH toocreate, spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-172">toocreate SSH keys, run hello following command.</span></span> <span data-ttu-id="adc4e-173">Po zobrazení výzvy pro vstup, vyberte **Enter** toogenerate hello klíčů ve výchozí umístění hello bez nastavení hesla.</span><span class="sxs-lookup"><span data-stu-id="adc4e-173">When you are prompted for input, select **Enter** toogenerate hello keys in hello default location without setting a password.</span></span>

        ssh-keygen

    <span data-ttu-id="adc4e-174">Připojí hello veřejného klíče toohello authorized_keys souboru známé veřejných klíčů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-174">Append hello public key toohello authorized_keys file for known public keys.</span></span>

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    <span data-ttu-id="adc4e-175">V adresáři ~/.ssh hello upravovat nebo vytvářet hello konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="adc4e-175">In hello ~/.ssh directory, edit or create hello config file.</span></span> <span data-ttu-id="adc4e-176">Zadejte rozsah IP adres hello hello privátní sítě, abyste naplánovali toouse v Azure (v tomto příkladu 10.32.0.0/16):</span><span class="sxs-lookup"><span data-stu-id="adc4e-176">Provide hello IP address range of hello private network that you plan toouse in Azure (10.32.0.0/16 in this example):</span></span>

        host 10.32.0.*
        StrictHostKeyChecking no

    <span data-ttu-id="adc4e-177">Alternativně seznamu hello privátní síť IP adresu každého virtuálního počítače v clusteru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="adc4e-177">Alternatively, list hello private network IP address of each VM in your cluster as follows:</span></span>

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > <span data-ttu-id="adc4e-178">Konfigurace `StrictHostKeyChecking no` můžete vytvořit představuje potenciální bezpečnostní riziko, pokud není zadán konkrétní IP adresu nebo rozsah.</span><span class="sxs-lookup"><span data-stu-id="adc4e-178">Configuring `StrictHostKeyChecking no` can create a potential security risk when a specific IP address or range is not specified.</span></span>
  >
  >
* <span data-ttu-id="adc4e-179">**Aplikace**: Nainstalujte aplikace potřebují nebo provedení jiných úprav, než zaznamenáte hello image.</span><span class="sxs-lookup"><span data-stu-id="adc4e-179">**Applications**: Install any applications you need or perform other customizations before you capture hello image.</span></span>

### <a name="capture-hello-image"></a><span data-ttu-id="adc4e-180">Zachycení bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="adc4e-180">Capture hello image</span></span>
<span data-ttu-id="adc4e-181">Obrázek hello toocapture nejprve spusťte následující příkaz na hello virtuálního počítače s Linuxem hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-181">toocapture hello image, first run hello following command on hello Linux VM.</span></span> <span data-ttu-id="adc4e-182">Tento příkaz deprovisions hello virtuálních počítačů, ale spravuje uživatelské účty a klíče SSH, které nastavíte.</span><span class="sxs-lookup"><span data-stu-id="adc4e-182">This command deprovisions hello VM but maintains user accounts and SSH keys that you set up.</span></span>

```
sudo waagent -deprovision
```

<span data-ttu-id="adc4e-183">Z klientského počítače spusťte hello následující obrázek hello toocapture příkazy rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="adc4e-183">From your client computer, run hello following Azure CLI commands toocapture hello image.</span></span> <span data-ttu-id="adc4e-184">Další informace najdete v tématu [jak toocapture klasické virtuální počítač s Linuxem jako obrázek](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="adc4e-184">For more information, see [How toocapture a classic Linux virtual machine as an image](capture-image.md).</span></span>  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

<span data-ttu-id="adc4e-185">Po spuštění těchto příkazů zachycenou image virtuálního počítače hello pro vaše použití a hello virtuálních počítačů se odstraní.</span><span class="sxs-lookup"><span data-stu-id="adc4e-185">After you run these commands, hello VM image is captured for your use and hello VM is deleted.</span></span> <span data-ttu-id="adc4e-186">Nyní máte připravené toodeploy vaše vlastní image clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc4e-186">Now you have your custom image ready toodeploy a cluster.</span></span>

### <a name="deploy-a-cluster-with-hello-image"></a><span data-ttu-id="adc4e-187">Nasazení clusteru s bitovou kopií hello</span><span class="sxs-lookup"><span data-stu-id="adc4e-187">Deploy a cluster with hello image</span></span>
<span data-ttu-id="adc4e-188">Upravit hello následující skript Bash s příslušnými hodnotami pro vaše prostředí a spusťte jej z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="adc4e-188">Modify hello following Bash script with appropriate values for your environment and run it from your client computer.</span></span> <span data-ttu-id="adc4e-189">Protože Azure nasadí virtuálních počítačů hello sériově v modelu nasazení classic hello, trvá několik minut toodeploy hello osm virtuálních počítačů A9 navržený v tento skript.</span><span class="sxs-lookup"><span data-stu-id="adc4e-189">Because Azure deploys hello VMs serially in hello classic deployment model, it takes a few minutes toodeploy hello eight A9 VMs suggested in this script.</span></span>

```
#!/bin/bash -x
# Script toocreate a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where hello VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All hello compute-intensive instances need toobe in hello same cloud service for Linux RDMA toowork across InfiniBand.
# Note: hello current maximum number of VMs in a cloud service is 50. If you need tooprovision more than 50 VMs in hello same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want tooturn off external ports and use only internal ports toocommunicate between compute nodes via port 22, don’t use this option. Since port numbers up too10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 toocluster18. Specify your captured image in <image-name>. Specify hello username and password you used when creating hello SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment tooprovision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a><span data-ttu-id="adc4e-190">Důležité informace pro cluster prostředí HPC CentOS</span><span class="sxs-lookup"><span data-stu-id="adc4e-190">Considerations for a CentOS HPC cluster</span></span>
<span data-ttu-id="adc4e-191">Pokud chcete, aby tooset clusteru na základě jedné z bitové kopie založené na CentOS HPC hello v hello Azure Marketplace místo SLES 12 pro prostředí HPC, postupujte podle hello obecné kroky v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-191">If you want tooset up a cluster based on one of hello CentOS-based HPC images in hello Azure Marketplace instead of SLES 12 for HPC, follow hello general steps in hello preceding section.</span></span> <span data-ttu-id="adc4e-192">Všimněte si hello následující rozdíly při zřídíte a nakonfigurujete hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="adc4e-192">Note hello following differences when you provision and configure hello VM:</span></span>

- <span data-ttu-id="adc4e-193">Intel MPI je již nainstalován na virtuální počítač z bitové kopie založené na CentOS HPC zřízený.</span><span class="sxs-lookup"><span data-stu-id="adc4e-193">Intel MPI is already installed on a VM provisioned from a CentOS-based HPC image.</span></span>
- <span data-ttu-id="adc4e-194">Nastavení uzamčení paměti jsou již přidán do souboru /etc/security/limits.conf hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="adc4e-194">Lock memory settings are already added in hello VM's /etc/security/limits.conf file.</span></span>
- <span data-ttu-id="adc4e-195">Nedojde k vytvoření klíčů SSH na hello zřídíte virtuální počítač na zachycení.</span><span class="sxs-lookup"><span data-stu-id="adc4e-195">Do not generate SSH keys on hello VM you provision for capture.</span></span> <span data-ttu-id="adc4e-196">Místo toho doporučujeme nastavení ověřování na základě uživatele po nasazení clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-196">Instead, we recommend setting up user-based authentication after you deploy hello cluster.</span></span> <span data-ttu-id="adc4e-197">Další informace najdete v tématu hello následující části.</span><span class="sxs-lookup"><span data-stu-id="adc4e-197">For more information, see hello following section.</span></span>  

### <a name="set-up-passwordless-ssh-trust-on-hello-cluster"></a><span data-ttu-id="adc4e-198">Nastavení passwordless SSH důvěryhodnosti v clusteru hello</span><span class="sxs-lookup"><span data-stu-id="adc4e-198">Set up passwordless SSH trust on hello cluster</span></span>
<span data-ttu-id="adc4e-199">V clusteru HPC na základě CentOS, existují dvě metody pro vytvoření vztahu důvěryhodnosti mezi hello výpočetní uzly: ověřování na základě hostitele a ověřování na základě uživatele.</span><span class="sxs-lookup"><span data-stu-id="adc4e-199">On a CentOS-based HPC cluster, there are two methods for establishing trust between hello compute nodes: host-based authentication and user-based authentication.</span></span> <span data-ttu-id="adc4e-200">Ověřování na základě hostitele je mimo rámec tohoto článku hello a obecně je třeba provést pomocí skriptu rozšíření během nasazení.</span><span class="sxs-lookup"><span data-stu-id="adc4e-200">Host-based authentication is outside of hello scope of this article and generally must be done through an extension script during deployment.</span></span> <span data-ttu-id="adc4e-201">Ověřování na základě uživatele je vhodné pro navázání vztahu důvěryhodnosti po nasazení a vyžaduje hello generování a sdílení klíčů SSH mezi hello výpočetní uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-201">User-based authentication is convenient for establishing trust after deployment and requires hello generation and sharing of SSH keys among hello compute nodes in hello cluster.</span></span> <span data-ttu-id="adc4e-202">Tato metoda se běžně označuje jako passwordless přihlašování přes SSH a je požadován při spouštění úloh MPI.</span><span class="sxs-lookup"><span data-stu-id="adc4e-202">This method is commonly known as passwordless SSH login and is required when running MPI jobs.</span></span>

<span data-ttu-id="adc4e-203">Ukázkový skript podílí z komunity hello je k dispozici na [Githubu](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable snadno uživatele ověřování na základě CentOS HPC clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc4e-203">A sample script contributed from hello community is available on [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) tooenable easy user authentication on a CentOS-based HPC cluster.</span></span> <span data-ttu-id="adc4e-204">Stáhnout a použít tento skript pomocí hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="adc4e-204">Download and use this script by using hello following steps.</span></span> <span data-ttu-id="adc4e-205">Můžete také upravit tento skript nebo použít jakékoli jiné metoda tooestablish passwordless SSH ověřování mezi hello clusteru výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-205">You can also modify this script or use any other method tooestablish passwordless SSH authentication between hello cluster compute nodes.</span></span>

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

<span data-ttu-id="adc4e-206">toorun hello skriptu, musíte tooknow hello předponu pro podsíť IP adresy.</span><span class="sxs-lookup"><span data-stu-id="adc4e-206">toorun hello script, you need tooknow hello prefix for your subnet IP addresses.</span></span> <span data-ttu-id="adc4e-207">Předpona hello získáte tak, že spustíte následující příkaz na jeden z uzlů clusteru hello hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-207">Get hello prefix by running hello following command on one of hello cluster nodes.</span></span> <span data-ttu-id="adc4e-208">Výstup by měl vypadat podobně jako 10.1.3.5 a hello předpona je hello 10.1.3 část.</span><span class="sxs-lookup"><span data-stu-id="adc4e-208">Your output should look something like 10.1.3.5, and hello prefix is hello 10.1.3 portion.</span></span>

    ifconfig eth0 | grep -w inet | awk '{print $2}'

<span data-ttu-id="adc4e-209">Nyní spusťte skript hello pomocí tři parametry: hello běžné uživatelské jméno v hello výpočetní uzly, hello společné heslo pro tohoto uživatele na hello výpočetních uzlů a předpony hello podsítě, která byla vrácena z předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-209">Now run hello script using three parameters: hello common user name on hello compute nodes, hello common password for that user on hello compute nodes, and hello subnet prefix that was returned from hello previous command.</span></span>

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

<span data-ttu-id="adc4e-210">Tento skript hello následující:</span><span class="sxs-lookup"><span data-stu-id="adc4e-210">This script does hello following:</span></span>

* <span data-ttu-id="adc4e-211">Vytvoří adresář na uzel hello hostitele s názvem .ssh, což je vyžadováno pro passwordless přihlášení.</span><span class="sxs-lookup"><span data-stu-id="adc4e-211">Creates a directory on hello host node named .ssh, which is required for passwordless login.</span></span>
* <span data-ttu-id="adc4e-212">Vytvoří konfigurační soubor v adresáři .ssh hello, která nastaví přihlášení tooallow passwordless přihlášení z libovolného uzlu v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-212">Creates a configuration file in hello .ssh directory that instructs passwordless login tooallow login from any node in hello cluster.</span></span>
* <span data-ttu-id="adc4e-213">Vytvoří soubory obsahující názvy hello a uzel IP adresy pro všechny hello uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-213">Creates files containing hello node names and node IP addresses for all hello nodes in hello cluster.</span></span> <span data-ttu-id="adc4e-214">Tyto soubory jsou ponechány po spuštění skriptu hello pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="adc4e-214">These files are left after hello script is run for later reference.</span></span>
* <span data-ttu-id="adc4e-215">Vytvoří pár klíčů privátní a veřejné pro každý uzel clusteru (včetně uzlu hostitele hello) a vytvoří záznamy v souboru authorized_keys hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-215">Creates a private and public key pair for each cluster node (including hello host node) and creates entries in hello authorized_keys file.</span></span>

> [!WARNING]
> <span data-ttu-id="adc4e-216">Spuštěním tohoto skriptu můžete vytvořit představuje potenciální bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="adc4e-216">Running this script can create a potential security risk.</span></span> <span data-ttu-id="adc4e-217">Ujistěte se, že není hello informace o veřejném klíči v ~/.ssh distribuován.</span><span class="sxs-lookup"><span data-stu-id="adc4e-217">Ensure that hello public key information in ~/.ssh is not distributed.</span></span>
>
>

## <a name="configure-intel-mpi"></a><span data-ttu-id="adc4e-218">Konfigurace Intel MPI</span><span class="sxs-lookup"><span data-stu-id="adc4e-218">Configure Intel MPI</span></span>
<span data-ttu-id="adc4e-219">toorun aplikací MPI na Azure Linux RDMA, je nutné tooconfigure konkrétní tooIntel určité prostředí proměnné MPI.</span><span class="sxs-lookup"><span data-stu-id="adc4e-219">toorun MPI applications on Azure Linux RDMA, you need tooconfigure certain environment variables specific tooIntel MPI.</span></span> <span data-ttu-id="adc4e-220">Zde je ukázkové Bash skriptu tooconfigure hello proměnných potřebných toorun aplikace.</span><span class="sxs-lookup"><span data-stu-id="adc4e-220">Here is a sample Bash script tooconfigure hello variables needed toorun an application.</span></span> <span data-ttu-id="adc4e-221">Podle potřeby pro instalaci nástroje Intel MPI, změňte cestu toompivars.sh hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-221">Change hello path toompivars.sh as needed for your installation of Intel MPI.</span></span>

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting hello variable tooshm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line toorun hello job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path toohello application exe> <arguments specific toohello application>

#end
```

<span data-ttu-id="adc4e-222">Hello formát souboru hostitele hello je následující.</span><span class="sxs-lookup"><span data-stu-id="adc4e-222">hello format of hello host file is as follows.</span></span> <span data-ttu-id="adc4e-223">Přidejte jeden řádek pro každý uzel v clusteru.</span><span class="sxs-lookup"><span data-stu-id="adc4e-223">Add one line for each node in your cluster.</span></span> <span data-ttu-id="adc4e-224">Zadejte, že privátní IP adresy z virtuální sítě hello definovaného dříve, není názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="adc4e-224">Specify private IP addresses from hello virtual network defined earlier, not DNS names.</span></span> <span data-ttu-id="adc4e-225">Například pro dva hostitele s IP adresami 10.32.0.1 a 10.32.0.2 hello soubor obsahuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="adc4e-225">For example, for two hosts with IP addresses 10.32.0.1 and 10.32.0.2, hello file contains hello following:</span></span>

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a><span data-ttu-id="adc4e-226">Spustit MPI na Základní dvojuzlový cluster</span><span class="sxs-lookup"><span data-stu-id="adc4e-226">Run MPI on a basic two-node cluster</span></span>
<span data-ttu-id="adc4e-227">Pokud jste tak již neučinili, nejprve nastavte hello prostředí pro Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="adc4e-227">If you haven't already done so, first set up hello environment for Intel MPI.</span></span>

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a><span data-ttu-id="adc4e-228">Spusťte příkaz MPI</span><span class="sxs-lookup"><span data-stu-id="adc4e-228">Run an MPI command</span></span>
<span data-ttu-id="adc4e-229">Spusťte příkaz MPI na jednom z tooshow hello výpočetní uzly, který MPI je správně nainstalována a může komunikovat mezi aspoň dva výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="adc4e-229">Run an MPI command on one of hello compute nodes tooshow that MPI is installed properly and can communicate between at least two compute nodes.</span></span> <span data-ttu-id="adc4e-230">Následující Hello **mpirun** příkaz spustí hello **hostname** na dvou uzlech.</span><span class="sxs-lookup"><span data-stu-id="adc4e-230">hello following **mpirun** command runs hello **hostname** command on two nodes.</span></span>

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
<span data-ttu-id="adc4e-231">Vaše výstup by měl obsahovat hello názvy všech hello uzlů, které předávají jako vstup pro `-hosts`.</span><span class="sxs-lookup"><span data-stu-id="adc4e-231">Your output should list hello names of all hello nodes that you passed as input for `-hosts`.</span></span> <span data-ttu-id="adc4e-232">Například **mpirun** příkaz s dvěma uzly vrátí výstup jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="adc4e-232">For example, an **mpirun** command with two nodes returns output like hello following:</span></span>

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a><span data-ttu-id="adc4e-233">Spuštění MPI srovnávacího testu</span><span class="sxs-lookup"><span data-stu-id="adc4e-233">Run an MPI benchmark</span></span>
<span data-ttu-id="adc4e-234">Následující příkaz Intel MPI Hello spustí pingpong srovnávacího testu tooverify hello síť s clustery konfiguraci a připojení toohello RDMA.</span><span class="sxs-lookup"><span data-stu-id="adc4e-234">hello following Intel MPI command runs a pingpong benchmark tooverify hello cluster configuration and connection toohello RDMA network.</span></span>

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

<span data-ttu-id="adc4e-235">Na pracovní clusteru se dvěma uzly měli byste vidět výstup podobný následující hello.</span><span class="sxs-lookup"><span data-stu-id="adc4e-235">On a working cluster with two nodes, you should see output like hello following.</span></span> <span data-ttu-id="adc4e-236">V síti Azure RDMA hello očekávejte latenci nebo pod 3 mikrosekundách zpráva velikostí až too512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="adc4e-236">On hello Azure RDMA network, expect latency at or below 3 microseconds for message sizes up too512 bytes.</span></span>

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
# hello number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected toobe exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through hello flag => -time

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
# List of Benchmarks toorun:
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



## <a name="next-steps"></a><span data-ttu-id="adc4e-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="adc4e-237">Next steps</span></span>
* <span data-ttu-id="adc4e-238">Nasazení a spuštění vaší Linux MPI aplikace na cluster systému Linux.</span><span class="sxs-lookup"><span data-stu-id="adc4e-238">Deploy and run your Linux MPI applications on your Linux cluster.</span></span>
* <span data-ttu-id="adc4e-239">V tématu hello [dokumentaci ke knihovně MPI Intel](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) pokyny k Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="adc4e-239">See hello [Intel MPI Library documentation](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) for guidance on Intel MPI.</span></span>
* <span data-ttu-id="adc4e-240">Zkuste [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate Lustre Intel clusteru pomocí bitové kopie založené na CentOS HPC.</span><span class="sxs-lookup"><span data-stu-id="adc4e-240">Try a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) toocreate an Intel Lustre cluster by using a CentOS-based HPC image.</span></span> <span data-ttu-id="adc4e-241">Podrobnosti najdete v tématu [nasazení Intel cloudu Edition pro počítače s Lustre v Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="adc4e-241">For details, see [Deploying Intel Cloud Edition for Lustre on Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).</span></span>
