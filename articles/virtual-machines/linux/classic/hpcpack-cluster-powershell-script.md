---
title: skript toodeploy aaaPowerShell clusteru HPC pro Linux | Microsoft Docs
description: "Spustit cluster Linux HPC Pack 2012 R2 toodeploy skript prostředí PowerShell ve virtuálních počítačích Azure"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 73041960-58d3-4ecf-9540-d7e1a612c467
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 885b03fa2fd604827dc388803fc21debab730979
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="ebfdf-103">Vytvořte cluster s vysoce výkonné výpočty (HPC) Linux se skript nasazení HPC Pack IaaS hello</span><span class="sxs-lookup"><span data-stu-id="ebfdf-103">Create a Linux high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="ebfdf-104">Spusťte hello HPC Pack IaaS nasazení prostředí PowerShell skriptu toodeploy dokončení clusteru HPC Pack 2012 R2 pro Linux zatížení ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Linux workloads in Azure virtual machines.</span></span> <span data-ttu-id="ebfdf-105">Hello clusteru se skládá z služby Active Directory připojené k hlavnímu uzlu systémem Windows Server a Microsoft HPC Pack a výpočetní uzly, které spusťte jeden z hello distribucí Linux podporované sadou HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and compute nodes that run one of hello Linux distributions supported by HPC Pack.</span></span> <span data-ttu-id="ebfdf-106">Pokud chcete, aby toodeploy clusteru služby HPC Pack v úlohy Azure pro Windows, přečtěte si téma [vytvořit cluster Windows HPC s hello skript nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ebfdf-106">If you want toodeploy an HPC Pack cluster in Azure for Windows workloads, see [Create a Windows HPC cluster with hello HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="ebfdf-107">Můžete použít také toodeploy šablony Azure Resource Manager clusteru služby HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="ebfdf-108">Příklad, naleznete v části [vytvoření clusteru prostředí HPC s Linux výpočetní uzly](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span><span class="sxs-lookup"><span data-stu-id="ebfdf-108">For an example, see [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ebfdf-109">Hello skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="ebfdf-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="ebfdf-111">Kromě toho hello skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a><span data-ttu-id="ebfdf-112">Příklad konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="ebfdf-112">Example configuration file</span></span>
<span data-ttu-id="ebfdf-113">Hello následující konfigurační soubor vytvoří řadič domény a doménové struktury domény a nasadí cluster služby HPC Pack, který má jeden hlavního uzlu s místní databází a 10 Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-113">hello following configuration file creates a domain controller and domain forest and deploys an HPC Pack cluster which has one head node with local databases and 10 Linux compute nodes.</span></span> <span data-ttu-id="ebfdf-114">Všechny hello cloudové služby jsou vytvořené přímo v umístění východní Asie hello.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-114">All hello cloud services are created directly in hello East Asia location.</span></span> <span data-ttu-id="ebfdf-115">Hello Linux výpočetní uzly jsou vytvořeny ve dvou cloudové služby a dva účty úložiště (tedy *MyLnxCN 0001* k *MyLnxCN 0005* v *MyLnxCNService01* a *mylnxstorage01*, a *MyLnxCN-0006* k *MyLnxCN 0010* v *MyLnxCNService02* a *mylnxstorage02* ).</span><span class="sxs-lookup"><span data-stu-id="ebfdf-115">hello Linux compute nodes are created in two cloud services and two storage accounts (that is, *MyLnxCN-0001* to *MyLnxCN-0005* in *MyLnxCNService01* and *mylnxstorage01*, and *MyLnxCN-0006* to *MyLnxCN-0010* in *MyLnxCNService02* and *mylnxstorage02*).</span></span> <span data-ttu-id="ebfdf-116">Hello výpočetní uzly jsou vytvořeny z image OpenLogic CentOS Linux verze 7.0.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-116">hello compute nodes are created from an OpenLogic CentOS version 7.0 Linux image.</span></span> 

<span data-ttu-id="ebfdf-117">Nahraďte vlastními hodnotami pro název odběru a názvy hello účtu a služby.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-117">Substitute your own values for your subscription name and hello account and service names.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a><span data-ttu-id="ebfdf-118">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ebfdf-118">Troubleshooting</span></span>
* <span data-ttu-id="ebfdf-119">**Chyba "Virtuální síť neexistuje"**.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-119">**“VNet doesn’t exist” error**.</span></span> <span data-ttu-id="ebfdf-120">Pokud spustíte hello HPC Pack IaaS nasazení skriptu toodeploy víc clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou hello "virtuální sítě *VNet\_název* neexistuje".</span><span class="sxs-lookup"><span data-stu-id="ebfdf-120">If you run hello HPC Pack IaaS deployment script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="ebfdf-121">Pokud k této chybě dojde, znovu spusťte hello skriptu pro nasazení hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-121">If this error occurs, rerun hello script for hello failed deployment.</span></span>
* <span data-ttu-id="ebfdf-122">**Přístup k problému hello Internetu z hello virtuální síť Azure**.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-122">**Problem accessing hello Internet from hello Azure virtual network**.</span></span> <span data-ttu-id="ebfdf-123">Pokud vytváření clusteru HPC Pack s nový řadič domény pomocí skriptu nasazení hello nebo manuálně povýšit hlavního uzlu řadiče toodomain virtuálních počítačů, může zaznamenat problémy s připojením hello virtuální počítače ve virtuální síti Azure toohello hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-123">If you create an HPC Pack cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs in hello Azure virtual network toohello Internet.</span></span> <span data-ttu-id="ebfdf-124">Tato situace může nastat, pokud na řadiči domény hello je automaticky nakonfigurovaný server DNS pro předávání a tento server DNS pro předávání nepřekládá správně.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-124">This can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="ebfdf-125">toowork vyřešit tento problém, přihlaste se na řadič domény toohello a nastavení konfigurace služby pro předávání hello buď odeberte nebo konfigurace serveru DNS platný server pro předávání.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-125">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="ebfdf-126">toodo, ve Správci serveru klikněte na **nástroje** > **DNS** tooopen Správce DNS a potom dvakrát klikněte na **předávání**.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-126">toodo this, in Server Manager click **Tools** > **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebfdf-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebfdf-127">Next steps</span></span>
* <span data-ttu-id="ebfdf-128">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) informace o podporovaných Linuxových distribucích přesouvání dat a odesílání úloh tooan HPC Pack clusteru s Linuxem výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="ebfdf-128">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for information about supported Linux distributions, moving data, and submitting jobs tooan HPC Pack cluster with Linux compute nodes.</span></span>
* <span data-ttu-id="ebfdf-129">Kurzy, které používají hello skriptu toocreate cluster a spustit úlohy HPC pro Linux najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="ebfdf-129">For tutorials that use hello script toocreate a cluster and run a Linux HPC workload, see:</span></span>
  * [<span data-ttu-id="ebfdf-130">Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="ebfdf-130">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
  * [<span data-ttu-id="ebfdf-131">Spuštění OpenFOAM pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="ebfdf-131">Run OpenFOAM with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-openfoam.md)
  * [<span data-ttu-id="ebfdf-132">Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="ebfdf-132">Run STAR-CCM+ with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-starccm.md)

