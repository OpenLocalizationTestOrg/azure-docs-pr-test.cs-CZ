---
title: "aaaLinux výpočetní virtuálních počítačů v clusteru služby HPC Pack | Microsoft Docs"
description: "Zjistěte, jak toocreate a použití HPC Pack clusteru v Azure pro Linux s vysokým výkonem úloh (prostředí HPC)"
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
ms.openlocfilehash: 9ed20d6cd69a6472a00666caf8965e9d022698a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="ebaff-103">Začínáme s výpočetními uzly Linuxu v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="ebaff-103">Get started with Linux compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="ebaff-104">Nastavení [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) clusteru v Azure, který obsahuje hlavního uzlu se systémem Windows Server a několika výpočetních uzlech systémem podporované distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="ebaff-104">Set up a [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) cluster in Azure that contains a head node running Windows Server and several compute nodes running a supported Linux distribution.</span></span> <span data-ttu-id="ebaff-105">Prozkoumejte možnosti toomove dat mezi uzly Linux hello a hello Windows hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-105">Explore options toomove data among hello Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="ebaff-106">Zjistěte, jak toosubmit Linux HPC úlohy toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaff-106">Learn how toosubmit Linux HPC jobs toohello cluster.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="ebaff-107">Na vysoké úrovni, hello následující diagram znázorňuje clusteru HPC Pack hello vytváření a práci s.</span><span class="sxs-lookup"><span data-stu-id="ebaff-107">At a high level, hello following diagram shows hello HPC Pack cluster you create and work with.</span></span>

![Cluster HPC Pack s uzly Linux][scenario]

<span data-ttu-id="ebaff-109">Pro další možnosti toorun Linux HPC najdete v části úlohy v Azure, [technických prostředcích pro batch a vysoce výkonné výpočty](../../../batch/big-compute-resources.md).</span><span class="sxs-lookup"><span data-stu-id="ebaff-109">For other options toorun Linux HPC workloads in Azure, see [Technical resources for batch and high-performance computing](../../../batch/big-compute-resources.md).</span></span>

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a><span data-ttu-id="ebaff-110">Nasazení clusteru HPC Pack s Linux výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="ebaff-110">Deploy an HPC Pack cluster with Linux compute nodes</span></span>
<span data-ttu-id="ebaff-111">Tento článek popisuje dvě možnosti toodeploy clusteru HPC Pack služby v Azure, s Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-111">This article shows you two options toodeploy an HPC Pack cluster in Azure with Linux compute nodes.</span></span> <span data-ttu-id="ebaff-112">Obě metody použít image pořízenou prostřednictvím Marketplace systému Windows Server s HPC Pack toocreate hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-112">Both methods use a Marketplace image of Windows Server with HPC Pack toocreate hello head node.</span></span> 

* <span data-ttu-id="ebaff-113">**Šablona Azure Resource Manageru** -použít šablonu z Azure Marketplace hello nebo šabloně pro rychlý start z hello komunity, tooautomate vytváření clusteru hello v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-113">**Azure Resource Manager template** - Use a template from hello Azure Marketplace, or a quickstart template from hello community, tooautomate creation of hello cluster in hello Resource Manager deployment model.</span></span> <span data-ttu-id="ebaff-114">Například hello [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony v hello Azure Marketplace vytvoří kompletní infrastruktura clusteru HPC Pack pro Linux HPC úlohy.</span><span class="sxs-lookup"><span data-stu-id="ebaff-114">For example, hello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace creates a complete HPC Pack cluster infrastructure for Linux HPC workloads.</span></span>
* <span data-ttu-id="ebaff-115">**Skript prostředí PowerShell** -použití hello [skript nasazení Microsoft HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate nasazení clusterů v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-115">**PowerShell script** - Use hello [Microsoft HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) (**New-HpcIaaSCluster.ps1**) tooautomate a complete cluster deployment in hello classic deployment model.</span></span> <span data-ttu-id="ebaff-116">Tento skript prostředí Azure PowerShell používá bitovou kopii prostředí HPC Pack virtuálních počítačů v hello Azure Marketplace pro rychlé nasazení a poskytuje komplexní sadu toodeploy parametry konfigurace Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-116">This Azure PowerShell script uses an HPC Pack VM image in hello Azure Marketplace for fast deployment and provides a comprehensive set of configuration parameters toodeploy Linux compute nodes.</span></span>

<span data-ttu-id="ebaff-117">Další informace o možnostech nasazení clusteru HPC Pack v Azure najdete v tématu [možnosti toocreate a spravovat cluster vysoce výkonné výpočty (HPC) v Azure pomocí sady Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ebaff-117">For more information about HPC Pack cluster deployment options in Azure, see [Options toocreate and manage a high-performance computing (HPC) cluster in Azure with Microsoft HPC Pack](../hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ebaff-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ebaff-118">Prerequisites</span></span>
* <span data-ttu-id="ebaff-119">**Předplatné Azure** -v buď hello globální Azure nebo Azure China služby uživatelům pomocí odběru.</span><span class="sxs-lookup"><span data-stu-id="ebaff-119">**Azure subscription** - You can use a subscription in either hello Azure Global or Azure China service.</span></span> <span data-ttu-id="ebaff-120">Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="ebaff-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.</span></span>
* <span data-ttu-id="ebaff-121">**Kvóta jader** -může být nutné tooincrease hello kvóty jader, obzvláště pokud se rozhodnete toodeploy několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-121">**Cores quota** - You might need tooincrease hello quota of cores, especially if you choose toodeploy several cluster nodes with multicore VM sizes.</span></span> <span data-ttu-id="ebaff-122">tooincrease kvótu, otevřete žádosti o podporu online zákazníka zdarma.</span><span class="sxs-lookup"><span data-stu-id="ebaff-122">tooincrease a quota, open an online customer support request at no charge.</span></span>
* <span data-ttu-id="ebaff-123">**Linuxových distribucích** -aktuálně HPC Pack podporuje hello následující Linuxových distribucích pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="ebaff-123">**Linux distributions** - Currently HPC Pack supports hello following Linux distributions for compute nodes.</span></span> <span data-ttu-id="ebaff-124">Můžete použít Marketplace verze těchto distribuce, pokud jsou k dispozici, nebo zadat vlastní.</span><span class="sxs-lookup"><span data-stu-id="ebaff-124">You can use Marketplace versions of these distributions where available, or supply your own.</span></span>
  
  * <span data-ttu-id="ebaff-125">**Na základě centOS**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, verze 6.5 HPC, 7.1 HPC</span><span class="sxs-lookup"><span data-stu-id="ebaff-125">**CentOS-based**: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC</span></span>
  * <span data-ttu-id="ebaff-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span><span class="sxs-lookup"><span data-stu-id="ebaff-126">**Red Hat Enterprise Linux**: 6.7, 6.8, 7.2</span></span>
  * <span data-ttu-id="ebaff-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 pro prostředí HPC, SLES 12 pro HPC (Premium)</span><span class="sxs-lookup"><span data-stu-id="ebaff-127">**SUSE Linux Enterprise Server**: SLES 12, SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 for HPC, SLES 12 for HPC (Premium)</span></span>
  * <span data-ttu-id="ebaff-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="ebaff-128">**Ubuntu Server**: 14.04 LTS, 16.04 LTS</span></span>
    
    > [!TIP]
    > <span data-ttu-id="ebaff-129">toouse hello Azure RDMA síť s jedním velikostí virtuálních počítačů hello podporu rdma, zadejte SUSE Linux Enterprise Server 12 HPC nebo na základě CentOS HPC bitovou kopii z hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ebaff-129">toouse hello Azure RDMA network with one of hello RDMA-capable VM sizes, specify a SUSE Linux Enterprise Server 12 HPC or CentOS-based HPC image from hello Azure Marketplace.</span></span> <span data-ttu-id="ebaff-130">Další informace najdete v tématu [vysokovýkonné výpočetní velikosti virtuálních počítačů](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ebaff-130">For more information, see [High performance compute VM sizes](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

<span data-ttu-id="ebaff-131">Další požadavky toodeploy hello clusteru pomocí skriptu nasazení HPC Pack IaaS hello:</span><span class="sxs-lookup"><span data-stu-id="ebaff-131">Additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="ebaff-132">**Klientský počítač** -je nutné klienta se systémem Windows toorun hello clusteru skript nasazení do počítače.</span><span class="sxs-lookup"><span data-stu-id="ebaff-132">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="ebaff-133">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="ebaff-133">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="ebaff-134">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="ebaff-134">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="ebaff-135">Verze hello hello skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="ebaff-135">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="ebaff-136">Tento článek je založen na verzi 4.4.1 nebo později hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-136">This article is based on version 4.4.1 or later of hello script.</span></span>

### <a name="deployment-option-1-use-a-resource-manager-template"></a><span data-ttu-id="ebaff-137">Možnost nasazení 1.</span><span class="sxs-lookup"><span data-stu-id="ebaff-137">Deployment option 1.</span></span> <span data-ttu-id="ebaff-138">Pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ebaff-138">Use a Resource Manager template</span></span>
1. <span data-ttu-id="ebaff-139">Přejděte toohello [clusteru HPC Pack pro Linux úlohy](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) šablony v hello Azure Marketplace a klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="ebaff-139">Go toohello [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) template in hello Azure Marketplace, and click **Deploy**.</span></span>
2. <span data-ttu-id="ebaff-140">V hello portálu Azure, zkontrolujte hello informace a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ebaff-140">In hello Azure portal, review hello information and then click **Create**.</span></span>
   
    ![Vytvoření portálu][portal]
3. <span data-ttu-id="ebaff-142">Na hello **Základy** okno, zadejte název clusteru hello, což také názvy hello hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebaff-142">On hello **Basics** blade, enter a name for hello cluster, which also names hello head node VM.</span></span> <span data-ttu-id="ebaff-143">Můžete vybrat existující skupinu prostředků nebo vytvořte skupinu pro hello nasazení v umístění, které je k dispozici tooyou.</span><span class="sxs-lookup"><span data-stu-id="ebaff-143">You can choose an existing resource group or create a group for hello deployment in a location that's available tooyou.</span></span> <span data-ttu-id="ebaff-144">Hello umístění má vliv na dostupnost hello určité velikosti virtuálních počítačů a dalším službám Azure (viz [produkty podle oblasti](https://azure.microsoft.com/regions/services/)).</span><span class="sxs-lookup"><span data-stu-id="ebaff-144">hello location affects hello availability of certain VM sizes and other Azure services (see [Products available by region](https://azure.microsoft.com/regions/services/)).</span></span>
4. <span data-ttu-id="ebaff-145">Na hello **hlavního uzlu nastavení** okně pro první nasazení, můžete obvykle přijmout výchozí nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-145">On hello **Head node settings** blade, for a first deployment, you can generally accept hello default settings.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ebaff-146">Hello **adresu URL skriptu po konfiguraci** je volitelné nastavení toospecify veřejně dostupné skript prostředí Windows PowerShell, které mají toorun hello hlavního uzlu virtuálního počítače po je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="ebaff-146">hello **Post-configuration script URL** is an optional setting toospecify a publicly available Windows PowerShell script that you want toorun on hello head node VM after it is running.</span></span> 
   > 
   > 
5. <span data-ttu-id="ebaff-147">Na hello **výpočetní uzel nastavení** okně vyberte vzoru pro pojmenovávání pro hello uzly, hello počet a velikost uzlů hello a hello toodeploy distribuce systému Linux.</span><span class="sxs-lookup"><span data-stu-id="ebaff-147">On hello **Compute node settings** blade, select a naming pattern for hello nodes, hello number and size of hello nodes, and hello Linux distribution toodeploy.</span></span>
6. <span data-ttu-id="ebaff-148">Na hello **nastavení infrastruktury** okno, zadejte pro hello virtuální sítě a služby Active Directory název domény, domény a přihlašovací údaje Správce virtuálních počítačů a vzoru pro pojmenovávání pro účty úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-148">On hello **Infrastructure settings** blade, enter names for hello virtual network and Active Directory domain, domain and VM administrator credentials, and a naming pattern for hello storage accounts.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ebaff-149">HPC Pack používá hello služby Active Directory domény tooauthenticate clusteru uživatele.</span><span class="sxs-lookup"><span data-stu-id="ebaff-149">HPC Pack uses hello Active Directory domain tooauthenticate cluster users.</span></span> 
   > 
   > 
7. <span data-ttu-id="ebaff-150">Po spuštění hello ověřovací testy a zkontrolujte hello podmínky použití, klikněte na tlačítko **nákupu**.</span><span class="sxs-lookup"><span data-stu-id="ebaff-150">After hello validation tests run and you review hello terms of use, click **Purchase**.</span></span>

### <a name="deployment-option-2-use-hello-iaas-deployment-script"></a><span data-ttu-id="ebaff-151">Možnost nasazení 2.</span><span class="sxs-lookup"><span data-stu-id="ebaff-151">Deployment option 2.</span></span> <span data-ttu-id="ebaff-152">Použít skript nasazení IaaS hello</span><span class="sxs-lookup"><span data-stu-id="ebaff-152">Use hello IaaS deployment script</span></span>
<span data-ttu-id="ebaff-153">Toto jsou další požadavky toodeploy hello clusteru pomocí skriptu nasazení HPC Pack IaaS hello:</span><span class="sxs-lookup"><span data-stu-id="ebaff-153">Following are additional prerequisites toodeploy hello cluster by using hello HPC Pack IaaS deployment script:</span></span>

* <span data-ttu-id="ebaff-154">**Klientský počítač** -je nutné klienta se systémem Windows toorun hello clusteru skript nasazení do počítače.</span><span class="sxs-lookup"><span data-stu-id="ebaff-154">**Client computer** - You need a Windows-based client computer toorun hello cluster deployment script.</span></span>
* <span data-ttu-id="ebaff-155">**Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="ebaff-155">**Azure PowerShell** - [Install and configure Azure PowerShell](/powershell/azure/overview) (version 0.8.10 or later) on your client computer.</span></span>
* <span data-ttu-id="ebaff-156">**Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="ebaff-156">**HPC Pack IaaS deployment script** - Download and unpack hello latest version of hello script from hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949).</span></span> <span data-ttu-id="ebaff-157">Verze hello hello skriptu můžete zkontrolovat spuštěním `.\New-HPCIaaSCluster.ps1 –Version`.</span><span class="sxs-lookup"><span data-stu-id="ebaff-157">You can check hello version of hello script by running `.\New-HPCIaaSCluster.ps1 –Version`.</span></span> <span data-ttu-id="ebaff-158">Tento článek je založen na verzi 4.4.1 nebo později hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-158">This article is based on version 4.4.1 or later of hello script.</span></span>

<span data-ttu-id="ebaff-159">**Konfigurační soubor XML.**</span><span class="sxs-lookup"><span data-stu-id="ebaff-159">**XML configuration file**</span></span>

<span data-ttu-id="ebaff-160">Hello skript nasazení HPC Pack IaaS používá konfigurační soubor XML jako vstupní toodescribe clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-160">hello HPC Pack IaaS deployment script uses an XML configuration file as input toodescribe hello  HPC cluster.</span></span> <span data-ttu-id="ebaff-161">Hello následující vzorový konfigurační soubor Určuje malé cluster obsahuje hlavnímu uzlu HPC Pack a dvě velikost A7 CentOS 7.0 Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-161">hello following sample configuration file specifies a small cluster consisting of an HPC Pack head node and two size A7 CentOS 7.0 Linux compute nodes.</span></span> 

<span data-ttu-id="ebaff-162">Upravte soubor hello podle potřeby pro vaše prostředí a konfiguraci požadovaných clusteru a uložte s názvem, jako je například HPCDemoConfig.xml.</span><span class="sxs-lookup"><span data-stu-id="ebaff-162">Modify hello file as needed for your environment and desired cluster configuration, and save it with a name such as HPCDemoConfig.xml.</span></span> <span data-ttu-id="ebaff-163">Například musíte toosupply název předplatného a jedinečný název účtu úložiště a název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="ebaff-163">For example, you need toosupply your subscription name and a unique storage account name and cloud service name.</span></span> <span data-ttu-id="ebaff-164">Kromě toho můžete chtít podporovaném toochoose jinou bitovou kopii systému Linux pro hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-164">Additionally, you might want toochoose a different supported Linux image for hello compute nodes.</span></span> <span data-ttu-id="ebaff-165">Další informace o hello elementy v konfiguračním souboru hello najdete v tématu hello Manual.rtf souboru ve složce hello skriptu a [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ebaff-165">For more information about hello elements in hello configuration file, see hello Manual.rtf file in hello script folder and [Create an HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

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

<span data-ttu-id="ebaff-166">**toorun hello skript nasazení HPC Pack IaaS**</span><span class="sxs-lookup"><span data-stu-id="ebaff-166">**toorun hello HPC Pack IaaS deployment script**</span></span>

1. <span data-ttu-id="ebaff-167">Otevřete prostředí Windows PowerShell v klientském počítači hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="ebaff-167">Open Windows PowerShell on hello client computer as an administrator.</span></span>
2. <span data-ttu-id="ebaff-168">Změna toohello adresář, kde je skript hello nainstalovaný (E:\IaaSClusterScript v tomto příkladu).</span><span class="sxs-lookup"><span data-stu-id="ebaff-168">Change directory toohello folder where hello script is installed (E:\IaaSClusterScript in this example).</span></span>
   
    ```powershell
    cd E:\IaaSClusterScript
    ```
3. <span data-ttu-id="ebaff-169">Spusťte následující příkaz toodeploy hello HPC Pack clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-169">Run hello following command toodeploy hello HPC Pack cluster.</span></span> <span data-ttu-id="ebaff-170">Tento příklad předpokládá, že hello konfigurační soubor se nachází v E:\HPCDemoConfig.xml</span><span class="sxs-lookup"><span data-stu-id="ebaff-170">This example assumes that hello configuration file is located in E:\HPCDemoConfig.xml</span></span>
   
    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```
   
    <span data-ttu-id="ebaff-171">a.</span><span class="sxs-lookup"><span data-stu-id="ebaff-171">a.</span></span> <span data-ttu-id="ebaff-172">Protože hello **AdminPassword** není zadán v hello předcházející příkaz, jsou výzvami tooenter hello heslo pro uživatele *MyAdminName*.</span><span class="sxs-lookup"><span data-stu-id="ebaff-172">Because hello **AdminPassword** is not specified in hello preceding command, you are prompted tooenter hello password for user *MyAdminName*.</span></span>
   
    <span data-ttu-id="ebaff-173">b.</span><span class="sxs-lookup"><span data-stu-id="ebaff-173">b.</span></span> <span data-ttu-id="ebaff-174">skript Hello pak spustí toovalidate hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="ebaff-174">hello script then starts toovalidate hello configuration file.</span></span> <span data-ttu-id="ebaff-175">To může trvat až tooseveral minut v závislosti na hello síťové připojení.</span><span class="sxs-lookup"><span data-stu-id="ebaff-175">It can take up tooseveral minutes depending on hello network connection.</span></span>
   
    ![Ověření][validate]
   
    <span data-ttu-id="ebaff-177">c.</span><span class="sxs-lookup"><span data-stu-id="ebaff-177">c.</span></span> <span data-ttu-id="ebaff-178">Po ověření úspěšně, uvádí hello skriptu toocreate prostředky clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-178">After validations pass, hello script lists hello cluster resources toocreate.</span></span> <span data-ttu-id="ebaff-179">Zadejte *Y* toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ebaff-179">Enter *Y* toocontinue.</span></span>
   
    ![Zdroje][resources]
   
    <span data-ttu-id="ebaff-181">d.</span><span class="sxs-lookup"><span data-stu-id="ebaff-181">d.</span></span> <span data-ttu-id="ebaff-182">skript Hello clusteru HPC Pack hello toodeploy při spuštění a dokončení konfigurace hello bez další ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="ebaff-182">hello script starts toodeploy hello HPC Pack cluster and completes hello configuration without further manual steps.</span></span> <span data-ttu-id="ebaff-183">Hello skript můžete spustit několik minut.</span><span class="sxs-lookup"><span data-stu-id="ebaff-183">hello script can run for several minutes.</span></span>
   
    ![Nasazení][deploy]
   
   > [!NOTE]
   > <span data-ttu-id="ebaff-185">V tomto příkladu hello skript generuje soubor protokolu automaticky od hello **- LogFile** není zadán parametr.</span><span class="sxs-lookup"><span data-stu-id="ebaff-185">In this example, hello script generates a log file automatically since hello **-LogFile** parameter isn't specified.</span></span> <span data-ttu-id="ebaff-186">protokoly Hello nezapisují v reálném čase, ale se shromažďují na konci hello hello vyhodnocení a nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-186">hello logs aren't written in real time, but are collected at hello end of hello validation and hello deployment.</span></span> <span data-ttu-id="ebaff-187">Pokud hello procesu prostředí PowerShell je zastavena v průběhu hello skript spuštěn, budou ztraceny některé protokoly.</span><span class="sxs-lookup"><span data-stu-id="ebaff-187">If hello PowerShell process is stopped while hello script is running, some logs are lost.</span></span>
   > 
   > 

## <a name="connect-toohello-head-node"></a><span data-ttu-id="ebaff-188">Připojit toohello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="ebaff-188">Connect toohello head node</span></span>
<span data-ttu-id="ebaff-189">Po nasazení clusteru HPC Pack hello v Azure, [připojit pomocí vzdálené plochy](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello hlavního uzlu virtuální počítač pomocí hello pověření domény, které jste zadali při nasazení clusteru hello (například *hpc\\ clusteradmin*).</span><span class="sxs-lookup"><span data-stu-id="ebaff-189">After you deploy hello HPC Pack cluster in Azure, [connect by Remote Desktop](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toohello head node VM using hello domain credentials you provided when you deployed hello cluster (for example, *hpc\\clusteradmin*).</span></span> <span data-ttu-id="ebaff-190">Spravujete hello clusteru z hlavního uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-190">You manage hello cluster from hello head node.</span></span>

<span data-ttu-id="ebaff-191">Hello hlavního uzlu spusťte Správce clusteru HPC toocheck hello stav clusteru HPC Pack hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-191">On hello head node, start HPC Cluster Manager toocheck hello status of hello HPC Pack cluster.</span></span> <span data-ttu-id="ebaff-192">Můžete spravovat a monitorovat Linux výpočetní uzly hello stejným způsobem jako pracujete s Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-192">You can manage and monitor Linux compute nodes hello same way you work with Windows compute nodes.</span></span> <span data-ttu-id="ebaff-193">Například můžete zjistit uzly Linux hello uvedené v **Správa prostředků** (tyto uzly se nasadí s hello **LinuxNode** šablony).</span><span class="sxs-lookup"><span data-stu-id="ebaff-193">For example, you see hello Linux nodes listed in **Resource Management** (these nodes are deployed with hello **LinuxNode** template).</span></span>

![Uzel správy][management]

<span data-ttu-id="ebaff-195">Zobrazí také hello Linuxových uzlů v hello **Heat mapa** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="ebaff-195">You also see hello Linux nodes in hello **Heat Map** view.</span></span>

![Heat mapa][heatmap]

## <a name="how-toomove-data-in-a-cluster-with-linux-nodes"></a><span data-ttu-id="ebaff-197">Jak toomove data v clusteru s uzly Linux</span><span class="sxs-lookup"><span data-stu-id="ebaff-197">How toomove data in a cluster with Linux nodes</span></span>
<span data-ttu-id="ebaff-198">Máte několik možností toomove dat mezi uzly Linux a hello Windows hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-198">You have several choices toomove data among Linux nodes and hello Windows head node of hello cluster.</span></span> <span data-ttu-id="ebaff-199">Tady jsou tři běžné metody popsané v podrobněji hello následující části:</span><span class="sxs-lookup"><span data-stu-id="ebaff-199">Here are three common methods, described in more detail in hello following sections:</span></span>

* <span data-ttu-id="ebaff-200">**Azure File** -zpřístupňuje spravované sdílené složky toostore dat souboru protokolu SMB soubory v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="ebaff-200">**Azure File** - Exposes a managed SMB file share toostore data files in Azure storage.</span></span> <span data-ttu-id="ebaff-201">Uzly Windows a Linux uzly můžete připojit Azure sdílené složky jako jednotky nebo složku v hello stejný čas, i v případě, že nasazené v různých virtuálních sítích.</span><span class="sxs-lookup"><span data-stu-id="ebaff-201">Windows nodes and Linux nodes can mount an Azure File share as a drive or folder at hello same time, even if they're deployed in different virtual networks.</span></span>
* <span data-ttu-id="ebaff-202">**Sdílet hlavního uzlu SMB** -připojí standardní sdílené složky Windows hello hlavního uzlu na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-202">**Head node SMB share** - Mounts a standard Windows shared folder of hello head node on Linux nodes.</span></span>
* <span data-ttu-id="ebaff-203">**Server systému souborů NFS uzlu HEAD** -poskytuje řešení sdílení souborů pro smíšená prostředí systému Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="ebaff-203">**Head node NFS server**  - Provides a file-sharing solution for a mixed Windows and Linux environment.</span></span>

### <a name="azure-file-storage"></a><span data-ttu-id="ebaff-204">Úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="ebaff-204">Azure File storage</span></span>
<span data-ttu-id="ebaff-205">Hello [Azure File](https://azure.microsoft.com/services/storage/files/) služby zpřístupní sdílené složky pomocí standardní protokol SMB 2.1 hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-205">hello [Azure File](https://azure.microsoft.com/services/storage/files/) service exposes file shares using hello standard SMB 2.1 protocol.</span></span> <span data-ttu-id="ebaff-206">Virtuální počítače Azure a cloudových služeb můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce prostřednictvím hello API služby File storage.</span><span class="sxs-lookup"><span data-stu-id="ebaff-206">Azure VMs and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share through hello File storage API.</span></span> 

<span data-ttu-id="ebaff-207">Podrobné kroky toocreate Azure File sdílet a připojit ho hello hlavního uzlu, najdete v části [Začínáme s Azure File storage ve Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span><span class="sxs-lookup"><span data-stu-id="ebaff-207">For detailed steps toocreate an Azure File share and mount it on hello head node, see [Get started with Azure File storage on Windows](../../../storage/files/storage-how-to-use-files-windows.md).</span></span> <span data-ttu-id="ebaff-208">sdílenou složku Azure File hello toomount na uzlech hello Linux, najdete v části [jak toouse Azure File storage s Linuxem](../../../storage/files/storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ebaff-208">toomount hello Azure File share on hello Linux nodes, see [How toouse Azure File storage with Linux](../../../storage/files/storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="ebaff-209">tooset až zachování připojení, najdete v části [Persisting připojení tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebaff-209">tooset up persisting connections, see [Persisting connections tooMicrosoft Azure Files](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).</span></span>

<span data-ttu-id="ebaff-210">V následujícím příkladu hello vytvoření účtu úložiště Azure sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="ebaff-210">In hello following example, create an Azure File share on a storage account.</span></span> <span data-ttu-id="ebaff-211">toomount hello sdílet hello hlavního uzlu, otevřete příkazový řádek a zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ebaff-211">toomount hello share on hello head node, open a Command Prompt and enter hello following commands:</span></span>

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

<span data-ttu-id="ebaff-212">V tomto příkladu allvhdsje je váš název účtu úložiště, storageaccountkey je klíč účtu úložiště a rdma je název sdílené složky Azure File hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-212">In this example, allvhdsje is your storage account name, storageaccountkey is your storage account key, and rdma is hello Azure File share name.</span></span> <span data-ttu-id="ebaff-213">sdílenou složku Azure File Hello je připojit jako Z: hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-213">hello Azure File share is mounted as Z: on hello head node.</span></span>

<span data-ttu-id="ebaff-214">sdílenou složku Azure File hello toomount na uzlech Linux, spusťte **clusrun** příkaz hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-214">toomount hello Azure File share on Linux nodes, run a **clusrun** command on hello head node.</span></span> <span data-ttu-id="ebaff-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)**  je užitečné toocarry nástroj HPC Pack Správce úloh ve více uzlech.</span><span class="sxs-lookup"><span data-stu-id="ebaff-215">**[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)** is a useful HPC Pack tool toocarry out administrative tasks on multiple nodes.</span></span> <span data-ttu-id="ebaff-216">(Viz také [Clusrun pro Linuxové uzly](#Clusrun-for-Linux-nodes) v tomto článku.)</span><span class="sxs-lookup"><span data-stu-id="ebaff-216">(See also [Clusrun for Linux nodes](#Clusrun-for-Linux-nodes) in this article.)</span></span>

<span data-ttu-id="ebaff-217">Otevřete okno prostředí Windows PowerShell a zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ebaff-217">Open a Windows PowerShell window and enter hello following commands:</span></span>

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

<span data-ttu-id="ebaff-218">První příkaz Hello vytvoří složku s názvem /rdma ve všech uzlech v hello LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ebaff-218">hello first command creates a folder named /rdma on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="ebaff-219">druhý příkaz Hello připojí allvhdsjw.file.core.windows.net/rdma sdílenou složku Azure File hello do složky /rdma hello s dir a soubor too777 sadu bitů režimu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-219">hello second command mounts hello Azure File share allvhdsjw.file.core.windows.net/rdma onto hello /rdma folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="ebaff-220">V druhém příkazu hello allvhdsje je váš název účtu úložiště a storageaccountkey je klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ebaff-220">In hello second command, allvhdsje is your storage account name and storageaccountkey is your storage account key.</span></span>

> [!NOTE]
> <span data-ttu-id="ebaff-221">Hello "\\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ebaff-221">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="ebaff-222">"\\`,"znamená, že hello"," (čárku) je součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-222">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="head-node-share"></a><span data-ttu-id="ebaff-223">Sdílené složky hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="ebaff-223">Head node share</span></span>
<span data-ttu-id="ebaff-224">Alternativně připojte sdílenou složku hello hlavního uzlu na Linuxových uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-224">Alternatively, mount a shared folder of hello head node on Linux nodes.</span></span> <span data-ttu-id="ebaff-225">Sdílené složky poskytuje hello nejjednodušší způsob, jak tooshare soubory, ale hello hlavního uzlu a všech uzlech Linux musí být nasazený v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="ebaff-225">A share provides hello simplest way tooshare files, but hello head node and all Linux nodes must be deployed in hello same virtual network.</span></span> <span data-ttu-id="ebaff-226">Tady jsou kroky hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-226">Here are hello steps.</span></span>

1. <span data-ttu-id="ebaff-227">Vytvořte složku hello hlavního uzlu a sdílet tooEveryone se oprávnění pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="ebaff-227">Create a folder on hello head node and share it tooEveryone with Read/Write permissions.</span></span> <span data-ttu-id="ebaff-228">Například sdílet D:\OpenFOAM hello hlavního uzlu jako \\CentOS7RDMA HN\OpenFOAM.</span><span class="sxs-lookup"><span data-stu-id="ebaff-228">For example, share D:\OpenFOAM on hello head node as \\CentOS7RDMA-HN\OpenFOAM.</span></span> <span data-ttu-id="ebaff-229">Zde je CentOS7RDMA HN hello hostname hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-229">Here CentOS7RDMA-HN is hello hostname of hello head node.</span></span>
   
    ![Oprávnění ke sdílené složce][fileshareperms]
   
    ![Sdílení souborů][filesharing]
2. <span data-ttu-id="ebaff-232">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="ebaff-232">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

<span data-ttu-id="ebaff-233">První příkaz Hello vytvoří složku s názvem /openfoam ve všech uzlech v hello LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ebaff-233">hello first command creates a folder named /openfoam on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="ebaff-234">druhý příkaz Hello připojí //CentOS7RDMA-HN/OpenFOAM hello sdílené složky do složky hello s dir a soubor too777 sadu bitů režimu.</span><span class="sxs-lookup"><span data-stu-id="ebaff-234">hello second command mounts hello shared folder //CentOS7RDMA-HN/OpenFOAM onto hello folder with dir and file mode bits set too777.</span></span> <span data-ttu-id="ebaff-235">Hello uživatelské jméno a heslo v příkazu hello by měl být hello uživatelské jméno a heslo uživatele hello hlavního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaff-235">hello username and password in hello command should be hello username and password of a cluster user on hello head node.</span></span> <span data-ttu-id="ebaff-236">(Viz [přidat nebo odebrat uživatele clusteru](https://technet.microsoft.com/library/ff919330.aspx).)</span><span class="sxs-lookup"><span data-stu-id="ebaff-236">(See [Add or remove cluster users](https://technet.microsoft.com/library/ff919330.aspx).)</span></span>

> [!NOTE]
> <span data-ttu-id="ebaff-237">Hello "\\`" symbol v druhém příkazu hello je symbol řídicí pro prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ebaff-237">hello “\\`” symbol in hello second command is an escape symbol for PowerShell.</span></span> <span data-ttu-id="ebaff-238">"\\`,"znamená, že hello"," (čárku) je součástí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-238">“\\`,” means that hello “,” (comma character) is a part of hello command.</span></span>
> 
> 

### <a name="nfs-server"></a><span data-ttu-id="ebaff-239">Server systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="ebaff-239">NFS server</span></span>
<span data-ttu-id="ebaff-240">Hello systému souborů NFS služby vám umožní tooshare nebo migrace souborů mezi počítače se systémem Windows Server 2012 hello operačního systému pomocí protokolu SMB hello a počítačů se systémem Linux pomocí protokolu NFS hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-240">hello NFS service enables you tooshare and migrate files between computers running hello Windows Server 2012 operating system using hello SMB protocol and Linux-based computers using hello NFS protocol.</span></span> <span data-ttu-id="ebaff-241">Hello systému souborů NFS server a všechny ostatní uzly mají toobe nasazené v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="ebaff-241">hello NFS server and all other nodes have toobe deployed in hello same virtual network.</span></span> <span data-ttu-id="ebaff-242">Poskytuje lepší kompatibilitu s Linux uzly ve srovnání s sdílené složce SMB.</span><span class="sxs-lookup"><span data-stu-id="ebaff-242">It provides better compatibility with Linux nodes compared with an SMB share.</span></span> <span data-ttu-id="ebaff-243">Například podporuje odkazy souboru.</span><span class="sxs-lookup"><span data-stu-id="ebaff-243">For example, it supports file links.</span></span>

1. <span data-ttu-id="ebaff-244">tooinstall a nastavení serveru NFS, postupujte podle kroků hello v [Server pro systém první sdílení souborů pro kompletní](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebaff-244">tooinstall and set up an NFS server, follow hello steps in [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).</span></span>
   
    <span data-ttu-id="ebaff-245">Můžete například vytvořte sdílené složky NFS s názvem systému souborů nfs se hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ebaff-245">For example, create an NFS share named nfs with hello following properties:</span></span>
   
    ![Autorizace systému souborů NFS][nfsauth]
   
    ![Oprávnění k sdíleným složkám NFS][nfsshare]
   
    ![Oprávnění NTFS pro systém souborů NFS][nfsperm]
   
    ![Vlastnosti správy systému souborů NFS][nfsmanage]
2. <span data-ttu-id="ebaff-250">Otevřete okno prostředí Windows PowerShell a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="ebaff-250">Open a Windows PowerShell window and run hello following commands:</span></span>
   
    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
   
    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```
   
   <span data-ttu-id="ebaff-251">První příkaz Hello vytvoří složku s názvem /nfsshared ve všech uzlech v hello LinuxNodes skupiny.</span><span class="sxs-lookup"><span data-stu-id="ebaff-251">hello first command creates a folder named /nfsshared on all nodes in hello LinuxNodes group.</span></span> <span data-ttu-id="ebaff-252">Hello druhý příkaz připojení zařízení hello systému souborů NFS sdílet CentOS7RDMA HN: / systému souborů nfs na hello složky.</span><span class="sxs-lookup"><span data-stu-id="ebaff-252">hello second command mounts hello NFS share CentOS7RDMA-HN:/nfs onto hello folder.</span></span> <span data-ttu-id="ebaff-253">Zde CentOS7RDMA HN: systém souborů nfs je hello vzdálené cestě sdílené složky NFS.</span><span class="sxs-lookup"><span data-stu-id="ebaff-253">Here CentOS7RDMA-HN:/nfs is hello remote path of your NFS share.</span></span>

## <a name="how-toosubmit-jobs"></a><span data-ttu-id="ebaff-254">Jak toosubmit úlohy</span><span class="sxs-lookup"><span data-stu-id="ebaff-254">How toosubmit jobs</span></span>
<span data-ttu-id="ebaff-255">Existuje několik způsobů toosubmit úlohy toohello HPC Pack clusteru:</span><span class="sxs-lookup"><span data-stu-id="ebaff-255">There are several ways toosubmit jobs toohello HPC Pack cluster:</span></span>

* <span data-ttu-id="ebaff-256">Správce clusteru HPC nebo grafického uživatelského rozhraní Správce úloh HPC</span><span class="sxs-lookup"><span data-stu-id="ebaff-256">HPC Cluster Manager or HPC Job Manager GUI</span></span>
* <span data-ttu-id="ebaff-257">Webového portálu HPC</span><span class="sxs-lookup"><span data-stu-id="ebaff-257">HPC web portal</span></span>
* <span data-ttu-id="ebaff-258">REST API</span><span class="sxs-lookup"><span data-stu-id="ebaff-258">REST API</span></span>

<span data-ttu-id="ebaff-259">Odeslání úlohy clusteru toohello v Azure přes HPC Pack grafického uživatelského rozhraní nástroje a webového portálu HPC hello jsou hello stejné jako u Windows výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebaff-259">Job submission toohello cluster in Azure via HPC Pack GUI tools and hello HPC web portal are hello same as for Windows compute nodes.</span></span> <span data-ttu-id="ebaff-260">V tématu [Správce úloh HPC Pack](https://technet.microsoft.com/library/ff919691.aspx) a [jak toosubmit úlohy z klientského počítače k místní](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ebaff-260">See [HPC Pack Job Manager](https://technet.microsoft.com/library/ff919691.aspx) and [How toosubmit jobs from an on-premises client computer](../../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="ebaff-261">úlohy toosubmit prostřednictvím hello REST API najdete příliš[vytváření a odesílání úloh pomocí rozhraní API REST v Microsoft HPC Pack hello](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebaff-261">toosubmit jobs via hello REST API, refer too[Creating and Submitting Jobs by Using hello REST API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).</span></span> <span data-ttu-id="ebaff-262">toosubmit úlohy z klienta Linux najdete také toohello Python vzorkováním hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span><span class="sxs-lookup"><span data-stu-id="ebaff-262">toosubmit jobs from a Linux client, also refer toohello Python sample in hello [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).</span></span>

## <a name="clusrun-for-linux-nodes"></a><span data-ttu-id="ebaff-263">Clusrun pro Linuxové uzly</span><span class="sxs-lookup"><span data-stu-id="ebaff-263">Clusrun for Linux nodes</span></span>
<span data-ttu-id="ebaff-264">Hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) nástroj lze použít tooexecute příkazů na Linuxových uzlů buď prostřednictvím příkazového řádku nebo Správce clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="ebaff-264">hello HPC Pack [clusrun](https://technet.microsoft.com/library/cc947685.aspx) tool can be used tooexecute commands on Linux nodes either through a Command Prompt or HPC Cluster Manager.</span></span> <span data-ttu-id="ebaff-265">Tady jsou některé základní příklady.</span><span class="sxs-lookup"><span data-stu-id="ebaff-265">Following are some basic examples.</span></span>

* <span data-ttu-id="ebaff-266">Zobrazit aktuální uživatelská jména na všech uzlech v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ebaff-266">Show current user names on all nodes in hello cluster.</span></span>
  
    ```command
    clusrun whoami
    ```
* <span data-ttu-id="ebaff-267">Nainstalujte hello **gdb** ladicí nástroj s **yum** ve všech uzlech v hello linuxnodes skupiny a pak restartujte hello uzly po 10 minutách.</span><span class="sxs-lookup"><span data-stu-id="ebaff-267">Install hello **gdb** debugger tool with **yum** on all nodes in hello linuxnodes group and then restart hello nodes after 10 minutes.</span></span>
  
    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```
* <span data-ttu-id="ebaff-268">Vytvořit zobrazení všechna čísla 1 až 10 pro jednu sekundu v každém Linux uzlu v clusteru hello skript prostředí, spustit a zobrazit výstup hello uzlů okamžitě.</span><span class="sxs-lookup"><span data-stu-id="ebaff-268">Create a shell script displaying each number 1 through 10 for one second on each Linux node in hello cluster, run it, and show output from hello nodes immediately.</span></span>
  
    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

> [!NOTE]
> <span data-ttu-id="ebaff-269">Může být nutné toouse určité řídicí znaky v **clusrun** příkazy.</span><span class="sxs-lookup"><span data-stu-id="ebaff-269">You might need toouse certain escape characters in **clusrun** commands.</span></span> <span data-ttu-id="ebaff-270">Jak je znázorněno v tomto příkladu, použijte ^ v příkazovém řádku tooescape hello ">" symbol.</span><span class="sxs-lookup"><span data-stu-id="ebaff-270">As shown in this example, use ^ in a Command Prompt tooescape hello ">" symbol.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="ebaff-271">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebaff-271">Next steps</span></span>
* <span data-ttu-id="ebaff-272">Zkuste vertikálním navýšení kapacity hello clusteru tooa větší počet uzlů, nebo zkuste spustit úlohu Linux na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ebaff-272">Try scaling up hello cluster tooa larger number of nodes, or try running a Linux workload on hello cluster.</span></span> <span data-ttu-id="ebaff-273">Příklad, naleznete v části [spustit NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-namd.md).</span><span class="sxs-lookup"><span data-stu-id="ebaff-273">For an example, see [Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure](hpcpack-cluster-namd.md).</span></span>
* <span data-ttu-id="ebaff-274">Zkuste cluster s [virtuální počítače podporuje RDMA, náročné](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) úlohy MPI toorun.</span><span class="sxs-lookup"><span data-stu-id="ebaff-274">Try a cluster with [RDMA-capable, compute-intensive VMs](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toorun MPI workloads.</span></span> <span data-ttu-id="ebaff-275">Příklad, naleznete v části [spustit OpenFOAM pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure](hpcpack-cluster-openfoam.md).</span><span class="sxs-lookup"><span data-stu-id="ebaff-275">For an example, see [Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure](hpcpack-cluster-openfoam.md).</span></span>
* <span data-ttu-id="ebaff-276">Pokud vás zajímá při práci s Linux uzly v clusteru HPC Pack služby místně, najdete v části hello [TechNet pokyny](https://technet.microsoft.com/library/mt595803.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebaff-276">If you are interested in working with Linux nodes in an on-premises HPC Pack cluster, see hello [TechNet guidance](https://technet.microsoft.com/library/mt595803.aspx).</span></span>

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
