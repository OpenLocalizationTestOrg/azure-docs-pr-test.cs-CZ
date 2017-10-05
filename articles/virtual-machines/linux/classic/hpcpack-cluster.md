---
title: "Linuxových výpočetních virtuálních počítačů v clusteru služby HPC Pack | Microsoft Docs"
description: "Postup vytvoření a používání clusteru HPC Pack v Azure pro Linux s vysokým výkonem úloh (prostředí HPC)"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 4d080fdd-5ffe-4f54-a78d-4c818f6eb3fb
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/12/2016
ms.author: danlep
ms.openlocfilehash: 809d3944311badf265117d353b65642e044d900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="ce707-103">Začínáme s výpočetními uzly Linuxu v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="ce707-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="ce707-104">Nastavení [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) clusteru v Azure, který obsahuje hlavního uzlu se systémem Windows Server a několika výpočetních uzlech systémem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="ce707-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="ce707-105">Prozkoumejte možnosti pro přesun dat mezi uzly Linux a Windows hlavnímu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-105">Explore options to move data among the Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="ce707-106">Zjistěte, jak k odesílání úloh Linux HPC do clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-106">Learn how to submit Linux HPC jobs to the cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="ce707-107">Následující diagram znázorňuje na vysoké úrovni clusteru HPC Pack vytváření a práci s.</span><span class="sxs-lookup"><span data-stu-id="ce707-107">At a high level, the following diagram shows the HPC Pack cluster you create and work with.</span></span>

![Cluster HPC Pack s uzly Linux][scenario]

<span data-ttu-id="ce707-109">Další možnosti spuštění úloh HPC pro Linux v Azure najdete v tématu [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ce707-109">For other options to run Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="ce707-110">Nasazení clusteru HPC Pack s Linux výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="ce707-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="ce707-111">Tento článek ukazuje, že jste dvě možnosti pro nasazení clusteru HPC Pack v Azure s Linuxem výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-111">This article shows you two options to deploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="ce707-112">Obě metody k vytvoření hlavního uzlu použít image pořízenou prostřednictvím Marketplace systému Windows Server s HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ce707-112">Both methods use a Marketplace image of Windows Server with HPC Pack to create the head node.</span></span> 

* <span data-ttu-id="ce707-113">**Šablona Azure Resource Manageru** -použít šablonu z Azure Marketplace nebo šabloně pro rychlý start od komunity, k automatizovanému vytvoření clusteru v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ce707-113">**Azure Resource Manager template** - Use a template from the Azure Marketplace, or a quickstart template from the community, to automate creation of the cluster in the Resource Manager deployment model.</span></span> <span data-ttu-id="ce707-114">Například [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony v Azure Marketplace vytvoří kompletní infrastruktura clusteru HPC Pack pro Linux HPC úlohy.</span><span class="sxs-lookup"><span data-stu-id="ce707-114">For example, the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="ce707-115">**Skript prostředí PowerShell** -použití [skript nasazení Microsoft HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) k automatizaci nasazení clusterů v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="ce707-115">**PowerShell script** - Use the [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) to automate a complete cluster deployment in the classic deployment model.</span></span> <span data-ttu-id="ce707-116">Tento skript prostředí Azure PowerShell používá bitovou kopii prostředí HPC Pack virtuálních počítačů v Azure Marketplace pro rychlé nasazení a poskytuje komplexní sadu parametrů konfigurace pro nasazení systému Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-116">This Azure PowerShell script uses an HPC Pack VM image in the Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters to deploy Linux compute nodes.</span></span>

<span data-ttu-id="ce707-117">Další informace o možnostech nasazení clusteru HPC Pack v Azure najdete v tématu [clusteru možnosti k vytváření a správě vysoce výkonné výpočty (HPC) v Azure pomocí sady Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce707-117">For more information about HPC Pack cluster deployment options in Azure, see [Options to create and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ce707-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce707-118">Prerequisites</span></span>
* <span data-ttu-id="ce707-119">**Předplatné Azure** -předplatné můžete použít ve službě Azure globální nebo Azure China.</span><span class="sxs-lookup"><span data-stu-id="ce707-119">**Azure subscription** - You can use a subscription in either the Azure Global or Azure China service.</span></span> <span data-ttu-id="ce707-120">Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="ce707-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="ce707-121">**Kvóta jader** -může být potřeba zvýšit kvótu jádra, obzvláště pokud se rozhodnete nasadit několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ce707-121">**Cores quota** - You might need to increase the quota of cores, especially if you choose to deploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="ce707-122">Chcete-li zvýšit kvótu, otevřete žádosti o podporu online zákazníka zdarma.</span><span class="sxs-lookup"><span data-stu-id="ce707-122">To increase a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="ce707-123">**Linuxových distribucích** -aktuálně HPC Pack podporuje následující Linuxových distribucích pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="ce707-123">**Linux distributions** - Currently HPC Pack supports the following Linux distributions for compute nodes.</span></span> <span data-ttu-id="ce707-124">Můžete použít Marketplace verze těchto distribuce, pokud jsou k dispozici, nebo zadat vlastní.</span><span class="sxs-lookup"><span data-stu-id="ce707-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="ce707-125">**Na základě centOS**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, verze 6.5 HPC, 7.1 HPC</span><span class="sxs-lookup"><span data-stu-id="ce707-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="ce707-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span><span class="sxs-lookup"><span data-stu-id="ce707-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="ce707-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 pro prostředí HPC, SLES 12 pro HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="ce707-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="ce707-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="ce707-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="ce707-129">Pro použití sítě Azure RDMA s jedním z velikosti virtuálních počítačů podporuje RDMA, zadejte SUSE Linux Enterprise Server 12 HPC nebo na základě CentOS HPC bitovou kopii z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ce707-129">To use the Azure RDMA network with one of the RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from the Azure Marketplace.</span></span> <span data-ttu-id="ce707-130">Další informace najdete v tématu [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce707-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="ce707-131">Další požadavky na nasazení clusteru pomocí skriptu prostředí HPC Pack IaaS nasazení:</span><span class="sxs-lookup"><span data-stu-id="ce707-131">Additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="ce707-132">**Klientský počítač** -potřebujete počítač klienta se systémem Windows pro spuštění skriptu nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-132">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="ce707-133">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="ce707-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="ce707-134">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte nejnovější verzi skript z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="ce707-134">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="ce707-135">Verze skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="ce707-135">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="ce707-136">Tento článek je založen na verzi 4.4.1 nebo později skript.</span><span class="sxs-lookup"><span data-stu-id="ce707-136">This article is based on version 4.4.1 or later of the script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="ce707-137">Možnost nasazení 1.</span><span class="sxs-lookup"><span data-stu-id="ce707-137">Deployment option 1.</span></span> <span data-ttu-id="ce707-138">Pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ce707-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="ce707-139">Přejděte na [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony Azure Marketplace, a klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="ce707-139">Go to the [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in the Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="ce707-140">Na portálu Azure, zkontrolujte informace a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ce707-140">In the Azure portal, review the information and then click **Create**.</span></span>
   
    ![Vytvoření portálu][portal]
3. <span data-ttu-id="ce707-142">Na **Základy** okno, zadejte název clusteru, který také názvy hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ce707-142">On the **Basics** blade, enter a name for the cluster, which also names the head node VM.</span></span> <span data-ttu-id="ce707-143">Můžete vybrat existující skupinu prostředků nebo vytvořte skupinu pro nasazení v umístění, které je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ce707-143">You can choose an existing resource group or create a group for the deployment in a location that's available to you.</span></span> <span data-ttu-id="ce707-144">Umístění má vliv na dostupnost určité velikosti virtuálních počítačů a dalším službám Azure (viz [produkty podle oblasti](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="ce707-144">The location affects the availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="ce707-145">Na **hlavního uzlu nastavení** okně pro první nasazení, můžete obvykle přijmout výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="ce707-145">On the **Head node settings** blade, for a first deployment, you can generally accept the default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ce707-146">**Adresu URL skriptu po konfiguraci** je volitelné nastavení k určení veřejně dostupné skript prostředí Windows PowerShell, který chcete spustit z hlavního uzlu virtuálního počítače, jakmile je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="ce707-146">The **Post-configuration script URL** is an optional setting to specify a publicly available Windows PowerShell script that you want to run on the head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="ce707-147">Na **výpočetní uzel nastavení** okně vyberte pojmenování vzor pro uzly, počet a velikost uzlů a distribuci systému Linux k nasazení.</span><span class="sxs-lookup"><span data-stu-id="ce707-147">On the **Compute node settings** blade, select a naming pattern for the nodes, the number and size of the nodes, and the Linux distribution to deploy.</span></span>
6. <span data-ttu-id="ce707-148">Na **nastavení infrastruktury** okno, zadejte názvy pro virtuální sítě a služby Active Directory domény, domény a přihlašovací údaje Správce virtuálních počítačů a vzoru pro pojmenovávání pro účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="ce707-148">On the **Infrastructure settings** blade, enter names for the virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for the storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ce707-149">HPC Pack používá domény služby Active Directory k ověřování uživatelů clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-149">HPC Pack uses the Active Directory domain to authenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="ce707-150">Po spuštění ověřovacích testů a přečtěte si podmínky použití, klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="ce707-150">After the validation tests run and you review the terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-the-iaas-deployment-script"></a><span data-ttu-id="ce707-151">Možnost nasazení 2.</span><span class="sxs-lookup"><span data-stu-id="ce707-151">Deployment option 2.</span></span> <span data-ttu-id="ce707-152">Pomocí tohoto skriptu nasazení IaaS</span><span class="sxs-lookup"><span data-stu-id="ce707-152">Use the IaaS deployment script</span></span>
<span data-ttu-id="ce707-153">Toto jsou další požadavky na nasazení clusteru pomocí skriptu prostředí HPC Pack IaaS nasazení:</span><span class="sxs-lookup"><span data-stu-id="ce707-153">Following are additional prerequisites to deploy the cluster by using the HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="ce707-154">**Klientský počítač** -potřebujete počítač klienta se systémem Windows pro spuštění skriptu nasazení clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-154">**Client computer** - You need a Windows-based client computer to run the cluster deployment script.</span></span>
* <span data-ttu-id="ce707-155">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="ce707-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="ce707-156">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte nejnovější verzi skript z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="ce707-156">**HPC Pack IaaS deployment script** - Download and unpack the latest version of the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="ce707-157">Verze skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="ce707-157">You can check the version of the script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="ce707-158">Tento článek je založen na verzi 4.4.1 nebo později skript.</span><span class="sxs-lookup"><span data-stu-id="ce707-158">This article is based on version 4.4.1 or later of the script.</span></span>

<span data-ttu-id="ce707-159">**Konfigurační soubor XML.**</span><span class="sxs-lookup"><span data-stu-id="ce707-159">**XML configuration file**</span></span>

<span data-ttu-id="ce707-160">Skript nasazení HPC Pack IaaS používá k popisu clusteru HPC konfigurační soubor XML jako vstup.</span><span class="sxs-lookup"><span data-stu-id="ce707-160">The HPC Pack IaaS deployment script uses an XML configuration file as input to describe the  HPC cluster.</span></span> <span data-ttu-id="ce707-161">Následující vzorový konfigurační soubor Určuje malé cluster obsahuje hlavnímu uzlu HPC Pack a dvě velikost A7 CentOS 7.0 Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-161">The following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="ce707-162">Upravte soubor podle potřeby pro vaše prostředí a konfiguraci požadovaných clusteru a uložte s názvem, jako je například HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="ce707-162">Modify the file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="ce707-163">Například musíte zadat název vašeho odběru a název účtu úložiště jedinečný a cloudových název služby.</span><span class="sxs-lookup"><span data-stu-id="ce707-163">For example, you need to supply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="ce707-164">Kromě toho můžete vybrat jiný obrázek Linux podporované pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="ce707-164">Additionally, you might want to choose a different supported Linux image for the compute nodes.</span></span> <span data-ttu-id="ce707-165">Další informace o elementy v konfiguračním souboru, najdete v souboru Manual.rtf ve složce skriptu a [vytvoření clusteru prostředí HPC pomocí skriptu pro nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce707-165">For more information about the elements in the configuration file, see the Manual.rtf file in the script folder and [Create an HPC cluster with the HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

<span data-ttu-id="ce707-166">**Pro spuštění skriptu nasazení HPC Pack IaaS**</span><span class="sxs-lookup"><span data-stu-id="ce707-166">**To run the HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="ce707-167">Otevřete prostředí Windows PowerShell v klientském počítači jako správce.</span><span class="sxs-lookup"><span data-stu-id="ce707-167">Open Windows PowerShell on the client computer as an administrator.</span></span>
2. <span data-ttu-id="ce707-168">Změnit adresář na složku, kde je skript nainstalovaný (E:\IaaSClusterScript v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="ce707-168">Change directory to the folder where the script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="ce707-169">Spusťte následující příkaz k nasazení clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ce707-169">Run the following command to deploy the HPC Pack cluster.</span></span> <span data-ttu-id="ce707-170">Tento příklad předpokládá, že konfigurační soubor nachází v E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="ce707-170">This example assumes that the configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="ce707-171">a.</span><span class="sxs-lookup"><span data-stu-id="ce707-171">a.</span></span> <span data-ttu-id="ce707-172">Protože **AdminPassword** není zadané v předchozí příkaz, zobrazí se výzva k zadání hesla pro uživatele *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="ce707-172">Because the **AdminPassword** is not specified in the preceding command, you are prompted to enter the password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="ce707-173">b.</span><span class="sxs-lookup"><span data-stu-id="ce707-173">b.</span></span> <span data-ttu-id="ce707-174">Chcete-li ověřit konfigurační soubor pak spustí skript.</span><span class="sxs-lookup"><span data-stu-id="ce707-174">The script then starts to validate the configuration file.</span></span> <span data-ttu-id="ce707-175">Může trvat až několik minut v závislosti na síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="ce707-175">It can take up to several minutes depending on the network connection.</span></span>
   
    ![Ověření][validate]
   
    <span data-ttu-id="ce707-177">c.</span><span class="sxs-lookup"><span data-stu-id="ce707-177">c.</span></span> <span data-ttu-id="ce707-178">Po ověření úspěšně, skript zobrazí seznam prostředků clusteru k vytvoření.</span><span class="sxs-lookup"><span data-stu-id="ce707-178">After validations pass, the script lists the cluster resources to create.</span></span> <span data-ttu-id="ce707-179">Zadejte *Y* pokračujte.</span><span class="sxs-lookup"><span data-stu-id="ce707-179">Enter *Y* to continue.</span></span>
   
    ![Zdroje][resources]
   
    <span data-ttu-id="ce707-181">d.</span><span class="sxs-lookup"><span data-stu-id="ce707-181">d.</span></span> <span data-ttu-id="ce707-182">Skript spustí nasazení clusteru HPC Pack a dokončení konfigurace bez další ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="ce707-182">The script starts to deploy the HPC Pack cluster and completes the configuration without further manual steps.</span></span> <span data-ttu-id="ce707-183">Tento skript můžete spustit několik minut.</span><span class="sxs-lookup"><span data-stu-id="ce707-183">The script can run for several minutes.</span></span>
   
    ![Nasazení][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="ce707-185">V tomto příkladu skript generuje soubor protokolu automaticky od **- LogFile** není zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="ce707-185">In this example, the script generates a log file automatically since the **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="ce707-186">Protokoly nezapisují v reálném čase, ale se shromažďují na konci ověření a nasazení.</span><span class="sxs-lookup"><span data-stu-id="ce707-186">The logs aren't written in real time, but are collected at the end of the validation and the deployment.</span></span> <span data-ttu-id="ce707-187">Pokud proces prostředí PowerShell je zastavena, když je skript spuštěn, některé protokoly jsou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="ce707-187">If the PowerShell process is stopped while the script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-to-the-head-node"></a><span data-ttu-id="ce707-188">Připojení k hlavnímu uzlu</span><span class="sxs-lookup"><span data-stu-id="ce707-188">Connect to the head node</span></span>
<span data-ttu-id="ce707-189">Po nasazení clusteru HPC Pack v Azure, [připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) k hlavnímu uzlu virtuálních počítačů využívající tuto doménu můžete zadané údaje po nasazení clusteru (například *hpc\\clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="ce707-189">After you deploy the HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to the head node VM using the domain credentials you provided when you deployed the cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="ce707-190">Spravujete z hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-190">You manage the cluster from the head node.</span></span>

<span data-ttu-id="ce707-191">Z hlavního uzlu spusťte Správce clusteru HPC a zkontrolujte stav clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ce707-191">On the head node, start HPC Cluster Manager to check the status of the HPC Pack cluster.</span></span> <span data-ttu-id="ce707-192">Můžete spravovat a monitorování Linuxových výpočetních uzlů stejným způsobem jako pracujete s Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-192">You can manage and monitor Linux compute nodes the same way you work with Windows compute nodes.</span></span> <span data-ttu-id="ce707-193">Například zobrazí Linuxových uzlů uvedené v **Správa prostředků** (tyto uzly jsou nasazeny pomocí **LinuxNode** šablony).</span><span class="sxs-lookup"><span data-stu-id="ce707-193">For example, you see the Linux nodes listed in **Resource Management** (these nodes are deployed with the **LinuxNode** template).</span></span>

![Uzel správy][management]

<span data-ttu-id="ce707-195">Zobrazí také Linux uzlů **Heat mapa** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ce707-195">You also see the Linux nodes in the **Heat Map** view.</span></span>

![Heat mapa][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="ce707-197">Jak přesunout data v clusteru s uzly Linux</span><span class="sxs-lookup"><span data-stu-id="ce707-197">How to move data in a cluster with Linux nodes</span></span>
<span data-ttu-id="ce707-198">Máte několik možností pro přesun dat mezi uzly Linux a Windows hlavnímu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-198">You have several choices to move data among Linux nodes and the Windows head node of the cluster.</span></span> <span data-ttu-id="ce707-199">Tady jsou tři běžné metody další podrobně popsány v následujících částech:</span><span class="sxs-lookup"><span data-stu-id="ce707-199">Here are three common methods, described in more detail in the following sections:</span></span>

* <span data-ttu-id="ce707-200">**Azure File** -zpřístupní spravované sdílení souborů SMB pro ukládají datové soubory v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="ce707-200">**Azure File** - Exposes a managed SMB file share to store data files in Azure storage.</span></span> <span data-ttu-id="ce707-201">Uzly Windows a Linux uzly můžete připojit Azure sdílené složky jako jednotky nebo složku ve stejnou dobu, i v případě, že nasazené v různých virtuálních sítích.</span><span class="sxs-lookup"><span data-stu-id="ce707-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at the same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="ce707-202">**Sdílet hlavního uzlu SMB** -připojí standardní sdílené složky systému Windows z hlavního uzlu na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-202">**Head node SMB share** - Mounts a standard Windows shared folder of the head node on Linux nodes.</span></span>
* <span data-ttu-id="ce707-203">**Server systému souborů NFS uzlu HEAD** -poskytuje řešení sdílení souborů pro smíšená prostředí systému Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="ce707-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="ce707-204">Úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="ce707-204">Azure File storage</span></span>
<span data-ttu-id="ce707-205">[Azure File](https://azure.microsoft.com/services/storage/files/) služby zpřístupní sdílené složky, které používají standardní protokol SMB 2.1.</span><span class="sxs-lookup"><span data-stu-id="ce707-205">The [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using the standard SMB 2.1 protocol.</span></span> <span data-ttu-id="ce707-206">Virtuální počítače Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím rozhraní API úložiště File.</span><span class="sxs-lookup"><span data-stu-id="ce707-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through the File storage API.</span></span> 

<span data-ttu-id="ce707-207">Podrobné pokyny k vytvoření Azure sdílené složky a připojte ho z hlavního uzlu, najdete v části [Začínáme s Azure File storage ve Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ce707-207">For detailed steps to create an Azure File share and mount it on the head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="ce707-208">Připojit sdílenou složku Azure File na Linuxových uzlů, najdete v části [postup používání Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ce707-208">To mount the Azure File share on the Linux nodes, see [How to use Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="ce707-209">Nastavení trvalých připojení, naleznete v tématu [Persisting připojení k Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce707-209">To set up persisting connections, see [Persisting connections to Microsoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="ce707-210">V následujícím příkladu vytvoření účtu úložiště Azure sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="ce707-210">In the following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="ce707-211">Chcete-li připojit sdílenou složku z hlavního uzlu, otevřete příkazový řádek a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ce707-211">To mount the share on the head node, open a Command Prompt and enter the following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="ce707-212">V tomto příkladu allvhdsje je váš název účtu úložiště, storageaccountkey je klíč účtu úložiště a rdma je název sdílené složky Azure File.</span><span class="sxs-lookup"><span data-stu-id="ce707-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is the Azure File share name.</span></span> <span data-ttu-id="ce707-213">Sdílenou složku Azure File je připojit jako Z: z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ce707-213">The Azure File share is mounted as Z: on the head node.</span></span>

<span data-ttu-id="ce707-214">Chcete-li připojit sdílenou složku Azure File na uzlech Linux, spusťte **clusrun** příkaz z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ce707-214">To mount the Azure File share on Linux nodes, run a **clusrun** command on the head node.</span></span> <span data-ttu-id="ce707-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  je užitečným nástrojem HPC Pack provádět úlohy správy ve více uzlech.</span><span class="sxs-lookup"><span data-stu-id="ce707-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool to carry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="ce707-216">(Viz také [Clusrun pro Linuxové uzly](#Clusrun-for-Linux-nodes) v tomto článku.)</span><span class="sxs-lookup"><span data-stu-id="ce707-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="ce707-217">Otevřete okno prostředí Windows PowerShell a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ce707-217">Open a Windows PowerShell window and enter the following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="ce707-218">První příkaz vytvoří složku s názvem /rdma ve všech uzlech v LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ce707-218">The first command creates a folder named /rdma on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="ce707-219">V druhém příkazu připojí allvhdsjw.file.core.windows.net/rdma sdílenou složku Azure File do složky /rdma s dir a souborů režimu bits nastavený na 777.</span><span class="sxs-lookup"><span data-stu-id="ce707-219">The second command mounts the Azure File share allvhdsjw.file.core.windows.net/rdma onto the /rdma folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="ce707-220">V druhém příkazu allvhdsje je váš název účtu úložiště a storageaccountkey je klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ce707-220">In the second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="ce707-221">"\\`" Symbol v druhém příkazu je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ce707-221">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="ce707-222">"\\`," znamená, že "," (čárku) se část příkazu.</span><span class="sxs-lookup"><span data-stu-id="ce707-222">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="ce707-223">Sdílené složky hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="ce707-223">Head node share</span></span>
<span data-ttu-id="ce707-224">Alternativně připojte sdílenou složku hlavního uzlu na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-224">Alternatively, mount a shared folder of the head node on Linux nodes.</span></span> <span data-ttu-id="ce707-225">Sdílené složky poskytuje nejjednodušší způsob, jak sdílet soubory, ale hlavního uzlu a všech uzlech Linux musí být nasazeny ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="ce707-225">A share provides the simplest way to share files, but the head node and all Linux nodes must be deployed in the same virtual network.</span></span> <span data-ttu-id="ce707-226">Tady jsou kroky.</span><span class="sxs-lookup"><span data-stu-id="ce707-226">Here are the steps.</span></span>

1. <span data-ttu-id="ce707-227">Vytvořte složku z hlavního uzlu a sdílet ho pro všechny uživatele s oprávněními pro čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="ce707-227">Create a folder on the head node and share it to Everyone with Read/Write permissions.</span></span> <span data-ttu-id="ce707-228">Například sdílet D:\OpenFOAM z hlavního uzlu jako \\CentOS7RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="ce707-228">For example, share D:\OpenFOAM on the head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="ce707-229">Zde CentOS7RDMA HN je název hostitele hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ce707-229">Here CentOS7RDMA-HN is the hostname of the head node.</span></span>
   
    ![Oprávnění ke sdílené složce][fileshareperms]
   
    ![Sdílení souborů][filesharing]
2. <span data-ttu-id="ce707-232">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ce707-232">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="ce707-233">První příkaz vytvoří složku s názvem /openfoam ve všech uzlech v LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ce707-233">The first command creates a folder named /openfoam on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="ce707-234">V druhém příkazu připojí //CentOS7RDMA-HN/OpenFOAM sdílené složky do složky s dir a souborů režimu bits nastavený na 777.</span><span class="sxs-lookup"><span data-stu-id="ce707-234">The second command mounts the shared folder //CentOS7RDMA-HN/OpenFOAM onto the folder with dir and file mode bits set to 777.</span></span> <span data-ttu-id="ce707-235">Uživatelské jméno a heslo v příkazu musí být uživatelské jméno a heslo uživatele z hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-235">The username and password in the command should be the username and password of a cluster user on the head node.</span></span> <span data-ttu-id="ce707-236">(Viz [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="ce707-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="ce707-237">"\\`" Symbol v druhém příkazu je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ce707-237">The “\\`” symbol in the second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="ce707-238">"\\`," znamená, že "," (čárku) se část příkazu.</span><span class="sxs-lookup"><span data-stu-id="ce707-238">“\\`,” means that the “,” (comma character) is a part of the command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="ce707-239">Server systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="ce707-239">NFS server</span></span>
<span data-ttu-id="ce707-240">Služby systému souborů NFS můžete sdílet a migrace souborů mezi počítači s operačním systémem Windows Server 2012 pomocí protokolu SMB a počítačů se systémem Linux pomocí protokolu NFS.</span><span class="sxs-lookup"><span data-stu-id="ce707-240">The NFS service enables you to share and migrate files between computers running the Windows Server 2012 operating system using the SMB protocol and Linux-based computers using the NFS protocol.</span></span> <span data-ttu-id="ce707-241">Server systému souborů NFS a všechny ostatní uzly mají být nasazeny ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="ce707-241">The NFS server and all other nodes have to be deployed in the same virtual network.</span></span> <span data-ttu-id="ce707-242">Poskytuje lepší kompatibilitu s Linux uzly ve srovnání s sdílené složce SMB.</span><span class="sxs-lookup"><span data-stu-id="ce707-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="ce707-243">Například podporuje odkazy souboru.</span><span class="sxs-lookup"><span data-stu-id="ce707-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="ce707-244">K instalaci a nastavení serveru NFS, postupujte podle kroků v [Server pro systém první sdílení souborů pro kompletní](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce707-244">To install and set up an NFS server, follow the steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="ce707-245">Můžete například vytvořte sdílené složky NFS s názvem systému souborů nfs s následujícími vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="ce707-245">For example, create an NFS share named nfs with the following properties:</span></span>
   
    ![Autorizace systému souborů NFS][nfsauth]
   
    ![Oprávnění k sdíleným složkám NFS][nfsshare]
   
    ![Oprávnění NTFS pro systém souborů NFS][nfsperm]
   
    ![Vlastnosti správy systému souborů NFS][nfsmanage]
2. <span data-ttu-id="ce707-250">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ce707-250">Open a Windows PowerShell window and run the following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="ce707-251">První příkaz vytvoří složku s názvem /nfsshared ve všech uzlech v LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ce707-251">The first command creates a folder named /nfsshared on all nodes in the LinuxNodes group.</span></span> <span data-ttu-id="ce707-252">V druhém příkazu připojí CentOS7RDMA HN sdílené složky NFS: nebo systému souborů nfs na složku.</span><span class="sxs-lookup"><span data-stu-id="ce707-252">The second command mounts the NFS share CentOS7RDMA-HN:/nfs onto the folder.</span></span> <span data-ttu-id="ce707-253">Zde CentOS7RDMA HN: systém souborů nfs je složka vzdálené sdílené složky NFS.</span><span class="sxs-lookup"><span data-stu-id="ce707-253">Here CentOS7RDMA-HN:/nfs is the remote path of your NFS share.</span></span>

## <a name="how-to-submit-jobs"></a><span data-ttu-id="ce707-254">Postup odeslání úlohy</span><span class="sxs-lookup"><span data-stu-id="ce707-254">How to submit jobs</span></span>
<span data-ttu-id="ce707-255">K odesílání úloh do clusteru HPC Pack několika způsoby:</span><span class="sxs-lookup"><span data-stu-id="ce707-255">There are several ways to submit jobs to the HPC Pack cluster:</span></span>

* <span data-ttu-id="ce707-256">Správce clusteru HPC nebo grafického uživatelského rozhraní Správce úloh HPC</span><span class="sxs-lookup"><span data-stu-id="ce707-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="ce707-257">Webového portálu HPC</span><span class="sxs-lookup"><span data-stu-id="ce707-257">HPC web portal</span></span>
* <span data-ttu-id="ce707-258">REST API</span><span class="sxs-lookup"><span data-stu-id="ce707-258">REST API</span></span>

<span data-ttu-id="ce707-259">Odesílání úloh do clusteru v Azure pomocí nástroje HPC Pack grafickým uživatelským rozhraním a webového portálu HPC jsou stejné jako u Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ce707-259">Job submission to the cluster in Azure via HPC Pack GUI tools and the HPC web portal are the same as for Windows compute nodes.</span></span> <span data-ttu-id="ce707-260">V tématu [Správce úloh HPC Pack](https://technet.microsoft.com/library/ff919691.aspx) a [postup odesílání úloh z klientského počítače k místní](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ce707-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How to submit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="ce707-261">Odesílání úloh přes REST API, najdete v tématu [vytváření a odesílání úloh pomocí rozhraní API REST v Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce707-261">To submit jobs via the REST API, refer to [Creating and Submitting Jobs by Using the REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="ce707-262">K odesílání úloh z klienta systému Linux, se také podívat na vzorku Python v [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="ce707-262">To submit jobs from a Linux client, also refer to the Python sample in the [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="ce707-263">Clusrun pro Linuxové uzly</span><span class="sxs-lookup"><span data-stu-id="ce707-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="ce707-264">HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) nástroj lze použít ke spuštění příkazů na Linuxových uzlů buď prostřednictvím příkazového řádku nebo Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="ce707-264">The HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used to execute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="ce707-265">Tady jsou některé základní příklady.</span><span class="sxs-lookup"><span data-stu-id="ce707-265">Following are some basic examples.</span></span>

* <span data-ttu-id="ce707-266">Zobrazit aktuální uživatelská jména na všech uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-266">Show current user names on all nodes in the cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="ce707-267">Nainstalujte **gdb** ladicí nástroj s **yum** na všechny uzly v linuxnodes skupiny a pak restartujte uzly po 10 minutách.</span><span class="sxs-lookup"><span data-stu-id="ce707-267">Install the **gdb** debugger tool with **yum** on all nodes in the linuxnodes group and then restart the nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="ce707-268">Vytvořit skript prostředí zobrazení všechna čísla 1 až 10 pro jednu sekundu v každém Linux uzlu v clusteru, spusťte ji a zobrazit výstup z uzlů okamžitě.</span><span class="sxs-lookup"><span data-stu-id="ce707-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in the cluster, run it, and show output from the nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="ce707-269">Možná budete muset použít určité řídicí znaky v **clusrun** příkazy.</span><span class="sxs-lookup"><span data-stu-id="ce707-269">You might need to use certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="ce707-270">Jak je znázorněno v tomto příkladu, použijte ^ v příkazovém řádku, abyste se vyhnuli ">" symbol.</span><span class="sxs-lookup"><span data-stu-id="ce707-270">As shown in this example, use ^ in a Command Prompt to escape the ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ce707-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce707-271">Next steps</span></span>
* <span data-ttu-id="ce707-272">Zkuste škálování na větší počet uzlů clusteru, nebo zkuste spustit úlohu Linux v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ce707-272">Try scaling up the cluster to a larger number of nodes, or try running a Linux workload on the cluster.</span></span> <span data-ttu-id="ce707-273">Příklad, naleznete v části [spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="ce707-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="ce707-274">Zkuste cluster s [virtuální počítače podporuje RDMA, náročné](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ke spuštění úlohy MPI.</span><span class="sxs-lookup"><span data-stu-id="ce707-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to run MPI workloads.</span></span> <span data-ttu-id="ce707-275">Příklad, naleznete v části [spustit OpenFOAM pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="ce707-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="ce707-276">Pokud vás zajímá při práci s Linux uzly v clusteru HPC Pack služby místně, najdete v článku [TechNet pokyny](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce707-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see the [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

<!--Image references-->
[scenario]:media/hpcpack-cluster/scenario.png
[portal]:media/hpcpack-cluster/portal.png
[validate]:media/hpcpack-cluster/validate.png
[resources]:media/hpcpack-cluster/resources.png
[deploy]:media/hpcpack-cluster/deploy.png
[management]:media/hpcpack-cluster/management.png
[heatmap]:media/hpcpack-cluster/heatmap.png
[fileshareperms]:media/hpcpack-cluster/fileshare1.png
[filesharing]:media/hpcpack-cluster/fileshare2.png
[nfsauth]:media/hpcpack-cluster/nfsauth.png
[nfsshare]:media/hpcpack-cluster/nfsshare.png
[nfsperm]:media/hpcpack-cluster/nfsperm.png
[nfsmanage]:media/hpcpack-cluster/nfsmanage.png
