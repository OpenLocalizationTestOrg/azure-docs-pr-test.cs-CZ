---
title: "Skript prostředí PowerShell pro nasazení clusteru Windows HPC | Microsoft Docs"
description: "Spusťte skript prostředí PowerShell pro nasazení clusteru Windows HPC Pack 2012 R2 ve virtuálních počítačích Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 286b2be8-2533-40df-b02a-26156b1f1133
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 85b125ab19671b61d2541af6378c95feb88bf952
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="49efe-103">Vytvoření Windows vysoce výkonné výpočty (HPC) clusteru pomocí skriptu pro nasazení HPC Pack IaaS</span><span class="sxs-lookup"><span data-stu-id="49efe-103">Create a Windows high-performance computing (HPC) cluster with the HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="49efe-104">Spusťte nasazení HPC Pack IaaS skript Powershellu pro nasazení dokončení clusteru HPC Pack 2012 R2 pro úlohy Windows ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="49efe-104">Run the HPC Pack IaaS deployment PowerShell script to deploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="49efe-105">Cluster se skládá z služby Active Directory připojené k hlavnímu uzlu systémem Windows Server a Microsoft HPC Pack a dalších Windows výpočetní prostředky, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="49efe-105">The cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="49efe-106">Pokud chcete nasazení clusteru HPC Pack v Azure pro Linux zatížení, přečtěte si téma [vytvořit cluster Linux HPC pomocí skriptu pro nasazení HPC Pack IaaS](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="49efe-106">If you want to deploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with the HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="49efe-107">Šablonu Azure Resource Manager můžete také použít k nasazení clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="49efe-107">You can also use an Azure Resource Manager template to deploy an HPC Pack cluster.</span></span> <span data-ttu-id="49efe-108">Příklady najdete v tématu [vytvoření clusteru prostředí HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) a [vytvoření clusteru prostředí HPC s bitovou kopii vlastní výpočetní uzel](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="49efe-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="49efe-109">Skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="49efe-109">The PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using the classic deployment model.</span></span> <span data-ttu-id="49efe-110">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="49efe-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="49efe-111">Kromě toho skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="49efe-111">In addition, the script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="49efe-112">Příklad konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="49efe-112">Example configuration files</span></span>
<span data-ttu-id="49efe-113">V následujících příkladech nahraďte vlastními hodnotami pro Id předplatného nebo název a název účtu a služby.</span><span class="sxs-lookup"><span data-stu-id="49efe-113">In the following examples, substitute your own values for your subscription Id or name and the account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="49efe-114">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="49efe-114">Example 1</span></span>
<span data-ttu-id="49efe-115">Následující konfigurační soubor nasazení clusteru služby HPC Pack, která má hlavního uzlu s místní databází a pět výpočetní uzly s operačním systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="49efe-115">The following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="49efe-116">Cloudové služby jsou vytvořené přímo v umístění západní USA.</span><span class="sxs-lookup"><span data-stu-id="49efe-116">All the cloud services are created directly in the West US location.</span></span> <span data-ttu-id="49efe-117">Z hlavního uzlu funguje jako řadič domény v doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="49efe-117">The head node acts as domain controller of the domain forest.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a><span data-ttu-id="49efe-118">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="49efe-118">Example 2</span></span>
<span data-ttu-id="49efe-119">Následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="49efe-119">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="49efe-120">Cluster má 1 hlavního uzlu s místní databází a 12 výpočetní uzly s příponou BGInfo virtuálních počítačů použít.</span><span class="sxs-lookup"><span data-stu-id="49efe-120">The cluster has 1 head node with local databases and 12 compute nodes with the BGInfo VM extension applied.</span></span>
<span data-ttu-id="49efe-121">Automatická instalace aktualizací systému Windows je zakázáno pro všechny virtuální počítače v doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="49efe-121">Automatic installation of Windows updates is disabled for all the VMs in the domain forest.</span></span> <span data-ttu-id="49efe-122">Cloudové služby jsou vytvořené přímo v umístění ve východní Asii.</span><span class="sxs-lookup"><span data-stu-id="49efe-122">All the cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="49efe-123">Výpočetní uzly jsou vytvořené v tři cloudové služby a tři účty úložiště: *MyHPCCN 0001* k *MyHPCCN 0005* v *MyHPCCNService01* a  *mycnstorage01*; *MyHPCCN-0006* k *MyHPCCN0010* v *MyHPCCNService02* a *mycnstorage02*; a  *MyHPCCN-0011* k *MyHPCCN 0012* v *MyHPCCNService03* a *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="49efe-123">The compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* to *MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* to *MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* to *MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="49efe-124">Výpočetní uzly jsou vytvořeny ze stávající privátní image zachyceného v výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="49efe-124">The compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="49efe-125">Automatického zvětšovat a zmenšovat s výchozím nastavení je povolena služba zvětšovat a zmenšovat intervalech.</span><span class="sxs-lookup"><span data-stu-id="49efe-125">The auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a><span data-ttu-id="49efe-126">Příklad 3</span><span class="sxs-lookup"><span data-stu-id="49efe-126">Example 3</span></span>
<span data-ttu-id="49efe-127">Následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="49efe-127">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="49efe-128">Cluster obsahuje jeden hlavní uzel, jeden databázový server s 500 GB datový disk, dvě zprostředkovatelské uzly s operačním systémem Windows Server 2012 R2 a pět výpočetní uzly s operačním systémem Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="49efe-128">The cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running the Windows Server 2012 R2 operating system, and five compute nodes running the Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="49efe-129">Cloudové služby MyHPCCNService se vytvoří ve skupině vztahů *MyIBAffinityGroup*, a vytvoří se ve skupině vztahů jiných cloudových služeb *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="49efe-129">The cloud service MyHPCCNService is created in the affinity group *MyIBAffinityGroup*, and the other cloud services are created in the affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="49efe-130">Jsou povoleny REST API Scheduleru úloh HPC a webového portálu HPC v hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="49efe-130">The HPC Job Scheduler REST API and HPC web portal are enabled on the head node.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a><span data-ttu-id="49efe-131">Příklad 4</span><span class="sxs-lookup"><span data-stu-id="49efe-131">Example 4</span></span>
<span data-ttu-id="49efe-132">Následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="49efe-132">The following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="49efe-133">Cluster má dvě hlavní uzel s místní databází, dvě šablony Azure uzlu vytvářejí a tři uzly Azure střední velikosti jsou vytvořeny pro šablonu Azure uzlu *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="49efe-133">The cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="49efe-134">Soubor skriptu se spouští z hlavního uzlu po dokončení konfigurace hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="49efe-134">A script file runs on the head node after the head node is configured.</span></span>

```Xml
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a><span data-ttu-id="49efe-135">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="49efe-135">Troubleshooting</span></span>
* <span data-ttu-id="49efe-136">**Chyba "Virtuální síť neexistuje"** – Pokud spustíte skript, který chcete nasadit více clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou "virtuální sítě *VNet\_název* neexistuje ".</span><span class="sxs-lookup"><span data-stu-id="49efe-136">**“VNet doesn’t exist” error** - If you run the script to deploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with the error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="49efe-137">Pokud k této chybě dojde, spusťte skript znovu pro neúspěšné nasazení.</span><span class="sxs-lookup"><span data-stu-id="49efe-137">If this error occurs, run the script again for the failed deployment.</span></span>
* <span data-ttu-id="49efe-138">**Problémy s přístupem k Internetu z virtuální síť Azure** – Pokud vytvoříte cluster s nový řadič domény pomocí skriptu nasazení, nebo manuálně povýšit hlavního uzlu virtuálního počítače pro řadič domény, může zaznamenat problémy s připojením Virtuální počítače k Internetu.</span><span class="sxs-lookup"><span data-stu-id="49efe-138">**Problem accessing the Internet from the Azure virtual network** - If you create a cluster with a new domain controller by using the deployment script, or you manually promote a head node VM to domain controller, you may experience problems connecting the VMs to the Internet.</span></span> <span data-ttu-id="49efe-139">Tomuto problému může dojít, pokud je automaticky nakonfigurovaný server DNS pro předávání na řadiči domény, a tento server DNS pro předávání nepřekládá správně.</span><span class="sxs-lookup"><span data-stu-id="49efe-139">This problem can occur if a forwarder DNS server is automatically configured on the domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="49efe-140">K tomuto problému, přihlaste se k řadiči domény a buď odebrat nastavení konfigurace služby pro předávání nebo konfigurace serveru DNS platný server pro předávání.</span><span class="sxs-lookup"><span data-stu-id="49efe-140">To work around this problem, log on to the domain controller and either remove the forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="49efe-141">Toto nastavení nakonfigurovat ve Správci serveru klikněte na **nástroje** >
    **DNS** otevřít Správce DNS, a potom dvakrát klikněte na **předávání**.</span><span class="sxs-lookup"><span data-stu-id="49efe-141">To configure this setting, in Server Manager click **Tools** >
    **DNS** to open DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="49efe-142">**Problémy s přístupem k síti RDMA z virtuálních počítačů náročné** – Pokud přidáte výpočetní systém Windows Server nebo zprostředkovatelský uzel virtuálních počítačů pomocí podporou RDMA velikost například A8 a A9, může zaznamenat problémy s připojením k síti aplikace RDMA těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="49efe-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs to the RDMA application network.</span></span> <span data-ttu-id="49efe-143">Jedním z důvodů, že k tomuto problému dochází, je, pokud rozšíření HpcVmDrivers není správně nainstalován, pokud jsou virtuální počítače přidat do clusteru.</span><span class="sxs-lookup"><span data-stu-id="49efe-143">One reason this problem occurs is if the HpcVmDrivers extension is not properly installed when the VMs are added to the cluster.</span></span> <span data-ttu-id="49efe-144">Například rozšíření zablokovaná v instalaci stavu.</span><span class="sxs-lookup"><span data-stu-id="49efe-144">For example, the extension might be stuck in the installing state.</span></span>
  
    <span data-ttu-id="49efe-145">Chcete-li tento problém obejít, nejprve zkontrolujte stav rozšíření ve virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="49efe-145">To work around this problem, first check the state of the extension in the VMs.</span></span> <span data-ttu-id="49efe-146">Pokud rozšíření není nainstalován správně, zkuste odebrat uzel z clusteru HPC a poté znovu přidejte uzly.</span><span class="sxs-lookup"><span data-stu-id="49efe-146">If the extension is not properly installed, try removing the nodes from the HPC cluster and then add the nodes again.</span></span> <span data-ttu-id="49efe-147">Například můžete přidat výpočetním uzlu virtuální počítače pomocí skriptu přidat HpcIaaSNode.ps1 z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="49efe-147">For example, you can add compute node VMs by running the Add-HpcIaaSNode.ps1 script on the head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49efe-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="49efe-148">Next steps</span></span>
* <span data-ttu-id="49efe-149">Zkuste spustit test zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="49efe-149">Try running a test workload on the cluster.</span></span> <span data-ttu-id="49efe-150">Příklad najdete v tématu HPC Pack [Příručka Začínáme](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="49efe-150">For an example, see the HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="49efe-151">Skript nasazení clusteru a spuštění úlohy HPC, na adrese [začít pracovat s clusteru služby HPC Pack v Azure a spuštění aplikace Excel a SOA úloh](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49efe-151">For a tutorial to script the cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure to run Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="49efe-152">Zkuste nástroje HPC Pack spustit, zastavit, přidat a odebrat výpočetních uzlů z clusteru, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="49efe-152">Try HPC Pack's tools to start, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="49efe-153">V tématu [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="49efe-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="49efe-154">Získat k odesílání úloh do clusteru z místního počítače, naleznete v tématu [clusteru HPC odeslání úloh na místní počítač do sady HPC Pack v Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="49efe-154">To get set up to submit jobs to the cluster from a local computer, see [Submit HPC jobs from an on-premises computer to an HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

