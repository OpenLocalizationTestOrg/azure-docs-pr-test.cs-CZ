---
title: "Skript prostředí PowerShell pro nasazení clusteru HPC pro Linux | Microsoft Docs"
description: "Spusťte skript prostředí PowerShell pro nasazení clusteru Linux HPC Pack 2012 R2 ve virtuálních počítačích Azure"
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
ms.openlocfilehash: c15dc66718a855e22f8109448cb8c8a23787b9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="7dd0b-103">Vytvoření systémem Linux vysoce výkonné výpočty (HPC) clusteru pomocí skriptu pro nasazení HPC Pack IaaS</span><span class="sxs-lookup"><span data-stu-id="7dd0b-103">Create a Linux high-performance computing (HPC) cluster with the HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="7dd0b-104">Spusťte nasazení HPC Pack IaaS skript Powershellu pro nasazení dokončení clusteru HPC Pack 2012 R2 pro Linux zatížení ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-104">Run the HPC Pack IaaS deployment PowerShell script to deploy a complete HPC Pack 2012 R2 cluster for Linux workloads in Azure virtual machines.</span></span> <span data-ttu-id="7dd0b-105">Cluster se skládá z služby Active Directory připojené k hlavnímu uzlu systémem Windows Server a Microsoft HPC Pack a výpočetní uzly, které spusťte jeden z Linuxových distribucích podporována sadou HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-105">The cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and compute nodes that run one of the Linux distributions supported by HPC Pack.</span></span> <span data-ttu-id="7dd0b-106">Pokud chcete nasazení clusteru HPC Pack v úlohách Azure pro Windows, přečtěte si téma [vytvořit cluster Windows HPC pomocí skriptu pro nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7dd0b-106">If you want to deploy an HPC Pack cluster in Azure for Windows workloads, see [Create a Windows HPC cluster with the HPC Pack IaaS deployment script](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="7dd0b-107">Šablonu Azure Resource Manager můžete také použít k nasazení clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-107">You can also use an Azure Resource Manager template to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="7dd0b-108">Příklad, naleznete v části [vytvoření clusteru prostředí HPC s Linux výpočetní uzly](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span><span class="sxs-lookup"><span data-stu-id="7dd0b-108">For an example, see [Create an HPC cluster with Linux compute nodes](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="7dd0b-109">Skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-109">The PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using the classic deployment model.</span></span> <span data-ttu-id="7dd0b-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="7dd0b-111">Kromě toho skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-111">In addition, the script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a><span data-ttu-id="7dd0b-112">Příklad konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="7dd0b-112">Example configuration file</span></span>
<span data-ttu-id="7dd0b-113">Následující konfigurační soubor vytvoří řadič domény a doménové struktury domény a nasadí cluster služby HPC Pack, který má jeden hlavního uzlu s místní databází a 10 Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-113">The following configuration file creates a domain controller and domain forest and deploys an HPC Pack cluster which has one head node with local databases and 10 Linux compute nodes.</span></span> <span data-ttu-id="7dd0b-114">Cloudové služby jsou vytvořené přímo v umístění ve východní Asii.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-114">All the cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="7dd0b-115">Výpočetní uzly Linux jsou vytvořené v dvě cloudové služby a dva účty úložiště (tedy *MyLnxCN 0001* k *MyLnxCN 0005* v *MyLnxCNService01* a *mylnxstorage01*, a *MyLnxCN-0006* k *MyLnxCN 0010* v *MyLnxCNService02* a *mylnxstorage02* ).</span><span class="sxs-lookup"><span data-stu-id="7dd0b-115">The Linux compute nodes are created in two cloud services and two storage accounts (that is, *MyLnxCN-0001* to *MyLnxCN-0005* in *MyLnxCNService01* and *mylnxstorage01*, and *MyLnxCN-0006* to *MyLnxCN-0010* in *MyLnxCNService02* and *mylnxstorage02*).</span></span> <span data-ttu-id="7dd0b-116">Výpočetní uzly jsou vytvořeny z image OpenLogic CentOS Linux verze 7.0.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-116">The compute nodes are created from an OpenLogic CentOS version 7.0 Linux image.</span></span> 

<span data-ttu-id="7dd0b-117">Nahraďte vlastními hodnotami pro vaše předplatné jméno a název účtu a služby.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-117">Substitute your own values for your subscription name and the account and service names.</span></span>

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
## <a name="troubleshooting"></a><span data-ttu-id="7dd0b-118">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="7dd0b-118">Troubleshooting</span></span>
* <span data-ttu-id="7dd0b-119">**Chyba "Virtuální síť neexistuje"**.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-119">**“VNet doesn’t exist” error**.</span></span> <span data-ttu-id="7dd0b-120">Pokud spustíte skript nasazení HPC Pack IaaS, který chcete nasadit více clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou "virtuální sítě *VNet\_název* neexistuje".</span><span class="sxs-lookup"><span data-stu-id="7dd0b-120">If you run the HPC Pack IaaS deployment script to deploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with the error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="7dd0b-121">Pokud k této chybě dojde, znovu spusťte skript pro neúspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-121">If this error occurs, rerun the script for the failed deployment.</span></span>
* <span data-ttu-id="7dd0b-122">**Problémy s přístupem k Internetu z virtuální síť Azure**.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-122">**Problem accessing the Internet from the Azure virtual network**.</span></span> <span data-ttu-id="7dd0b-123">Pokud vytvoříte clusteru služby HPC Pack s nový řadič domény pomocí skriptu nasazení, nebo manuálně povýšit hlavního uzlu virtuálního počítače pro řadič domény, může zaznamenat problémy s připojením k Internetu virtuální počítače ve virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-123">If you create an HPC Pack cluster with a new domain controller by using the deployment script, or you manually promote a head node VM to domain controller, you may experience problems connecting the VMs in the Azure virtual network to the Internet.</span></span> <span data-ttu-id="7dd0b-124">Tato situace může nastat, pokud je automaticky nakonfigurovaný server DNS pro předávání na řadiči domény, a tento server DNS pro předávání nepřekládá správně.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-124">This can occur if a forwarder DNS server is automatically configured on the domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="7dd0b-125">K tomuto problému, přihlaste se k řadiči domény a buď odebrat nastavení konfigurace služby pro předávání nebo konfigurace serveru DNS platný server pro předávání.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-125">To work around this problem, log on to the domain controller and either remove the forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="7dd0b-126">K tomu, ve Správci serveru klikněte na **nástroje** > **DNS** otevřít Správce DNS, a potom dvakrát klikněte na **předávání**.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-126">To do this, in Server Manager click **Tools** > **DNS** to open DNS Manager, and then double-click **Forwarders**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dd0b-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7dd0b-127">Next steps</span></span>
* <span data-ttu-id="7dd0b-128">V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) informace o podporovaných Linuxových distribucích přesouvání dat a odesílání úloh do clusteru HPC Pack operačního systému Linux výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="7dd0b-128">See [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md) for information about supported Linux distributions, moving data, and submitting jobs to an HPC Pack cluster with Linux compute nodes.</span></span>
* <span data-ttu-id="7dd0b-129">Kurzy, které pomocí skriptu vytvořte cluster a spustit úlohy Linux HPC najdete v části:</span><span class="sxs-lookup"><span data-stu-id="7dd0b-129">For tutorials that use the script to create a cluster and run a Linux HPC workload, see:</span></span>
  * [<span data-ttu-id="7dd0b-130">Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="7dd0b-130">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
  * [<span data-ttu-id="7dd0b-131">Spuštění OpenFOAM pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="7dd0b-131">Run OpenFOAM with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-openfoam.md)
  * [<span data-ttu-id="7dd0b-132">Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="7dd0b-132">Run STAR-CCM+ with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-starccm.md)

