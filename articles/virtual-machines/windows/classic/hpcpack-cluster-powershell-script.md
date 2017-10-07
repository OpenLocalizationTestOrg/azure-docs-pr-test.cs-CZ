---
title: cluster Windows HPC toodeploy skriptu aaaPowerShell | Microsoft Docs
description: "Spustit cluster Windows HPC Pack 2012 R2 toodeploy skript prostředí PowerShell ve virtuálních počítačích Azure"
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
ms.openlocfilehash: 10ce1e9bc4e98954b955549bd72aaaf6106c69fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a><span data-ttu-id="acfab-103">Vytvoření clusteru se systémem Windows vysoce výkonné výpočty (HPC) s skript nasazení HPC Pack IaaS hello</span><span class="sxs-lookup"><span data-stu-id="acfab-103">Create a Windows high-performance computing (HPC) cluster with hello HPC Pack IaaS deployment script</span></span>
<span data-ttu-id="acfab-104">Spusťte kompletní clusteru HPC Pack 2012 R2 pro úlohy Windows hello HPC Pack IaaS nasazení prostředí PowerShell skriptu toodeploy ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="acfab-104">Run hello HPC Pack IaaS deployment PowerShell script toodeploy a complete HPC Pack 2012 R2 cluster for Windows workloads in Azure virtual machines.</span></span> <span data-ttu-id="acfab-105">Hello clusteru se skládá ze Active Directory připojený hlavního uzlu systémem Windows Server a Microsoft HPC Pack a dalších Windows výpočetní prostředky, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="acfab-105">hello cluster consists of an Active Directory-joined head node running Windows Server and Microsoft HPC Pack, and additional Windows compute resources you specify.</span></span> <span data-ttu-id="acfab-106">Pokud chcete pro Linux úlohy toodeploy clusteru služby HPC Pack v Azure, najdete v části [vytvořit cluster Linux HPC s hello skript nasazení HPC Pack IaaS](../../linux/classic/hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="acfab-106">If you want toodeploy an HPC Pack cluster in Azure for Linux workloads, see [Create a Linux HPC cluster with hello HPC Pack IaaS deployment script](../../linux/classic/hpcpack-cluster-powershell-script.md).</span></span> <span data-ttu-id="acfab-107">Můžete použít také toodeploy šablony Azure Resource Manager clusteru služby HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="acfab-107">You can also use an Azure Resource Manager template toodeploy an HPC Pack cluster.</span></span> <span data-ttu-id="acfab-108">Příklady najdete v tématu [vytvoření clusteru prostředí HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) a [vytvoření clusteru prostředí HPC s bitovou kopii vlastní výpočetní uzel](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span><span class="sxs-lookup"><span data-stu-id="acfab-108">For examples, see [Create an HPC cluster](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) and [Create an HPC cluster with a custom compute node image](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="acfab-109">Hello skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-109">hello PowerShell script described in this article creates a Microsoft HPC Pack 2012 R2 cluster in Azure using hello classic deployment model.</span></span> <span data-ttu-id="acfab-110">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="acfab-111">Kromě toho hello skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="acfab-111">In addition, hello script described in this article does not support HPC Pack 2016.</span></span>

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a><span data-ttu-id="acfab-112">Příklad konfigurační soubory</span><span class="sxs-lookup"><span data-stu-id="acfab-112">Example configuration files</span></span>
<span data-ttu-id="acfab-113">Následující příklady v hello nahraďte vlastní hodnoty pro Id předplatného nebo název a názvy hello účtu a služby.</span><span class="sxs-lookup"><span data-stu-id="acfab-113">In hello following examples, substitute your own values for your subscription Id or name and hello account and service names.</span></span>

### <a name="example-1"></a><span data-ttu-id="acfab-114">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="acfab-114">Example 1</span></span>
<span data-ttu-id="acfab-115">Hello následujícího konfiguračního souboru nasazení clusteru služby HPC Pack, který má hlavního uzlu s místní databází a operačním systémem Windows Server 2012 R2 hello pět výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="acfab-115">hello following configuration file deploys an HPC Pack cluster that has a head node with local databases and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="acfab-116">Všechny hello cloudové služby jsou vytvořené přímo v hello umístění západní USA.</span><span class="sxs-lookup"><span data-stu-id="acfab-116">All hello cloud services are created directly in hello West US location.</span></span> <span data-ttu-id="acfab-117">Hello hlavního uzlu funguje jako řadič domény hello domény doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="acfab-117">hello head node acts as domain controller of hello domain forest.</span></span>

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

### <a name="example-2"></a><span data-ttu-id="acfab-118">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="acfab-118">Example 2</span></span>
<span data-ttu-id="acfab-119">Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="acfab-119">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="acfab-120">Hello clusteru má 1 hlavního uzlu s místní databází a 12 výpočetní uzly s hello rozšíření BGInfo virtuálního počítače, které jsou použity.</span><span class="sxs-lookup"><span data-stu-id="acfab-120">hello cluster has 1 head node with local databases and 12 compute nodes with hello BGInfo VM extension applied.</span></span>
<span data-ttu-id="acfab-121">Automatická instalace aktualizací systému Windows je zakázáno pro všechny hello virtuálních počítačů v doménové struktuře domény hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-121">Automatic installation of Windows updates is disabled for all hello VMs in hello domain forest.</span></span> <span data-ttu-id="acfab-122">Všechny hello cloudové služby jsou vytvořené přímo v umístění ve východní Asii.</span><span class="sxs-lookup"><span data-stu-id="acfab-122">All hello cloud services are created directly in the East Asia location.</span></span> <span data-ttu-id="acfab-123">Hello výpočetní uzly jsou vytvořené v tři cloudové služby a tři účty úložiště: *MyHPCCN 0001* příliš*MyHPCCN 0005* v *MyHPCCNService01* a  *mycnstorage01*; *MyHPCCN-0006* příliš*MyHPCCN0010* v *MyHPCCNService02* a *mycnstorage02*; a  *MyHPCCN-0011* příliš*MyHPCCN 0012* v *MyHPCCNService03* a *mycnstorage03*).</span><span class="sxs-lookup"><span data-stu-id="acfab-123">hello compute nodes are created in three cloud services and three storage accounts: *MyHPCCN-0001* too*MyHPCCN-0005* in *MyHPCCNService01* and *mycnstorage01*; *MyHPCCN-0006* too*MyHPCCN0010* in *MyHPCCNService02* and *mycnstorage02*; and *MyHPCCN-0011* too*MyHPCCN-0012* in *MyHPCCNService03* and *mycnstorage03*).</span></span> <span data-ttu-id="acfab-124">Hello výpočetní uzly jsou vytvořeny ze stávající privátní image zachyceného v výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="acfab-124">hello compute nodes are created from an existing private image captured from a compute node.</span></span> <span data-ttu-id="acfab-125">Hello automaticky zvětšovat a zmenšovat s výchozím nastavení je povolena služba zvětšovat a zmenšovat intervalech.</span><span class="sxs-lookup"><span data-stu-id="acfab-125">hello auto grow and shrink service is enabled with default grow and shrink intervals.</span></span>

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

### <a name="example-3"></a><span data-ttu-id="acfab-126">Příklad 3</span><span class="sxs-lookup"><span data-stu-id="acfab-126">Example 3</span></span>
<span data-ttu-id="acfab-127">Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="acfab-127">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="acfab-128">Hello cluster obsahuje jeden hlavní uzel, jeden server databáze s 500 GB datový disk, dva uzly zprostředkovatele spuštěný hello Windows Server 2012 R2 operační systém a operačním systémem Windows Server 2012 R2 hello pět výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="acfab-128">hello cluster contains one head node, one database server with a 500 GB data disk, two broker nodes running hello Windows Server 2012 R2 operating system, and five compute nodes running hello Windows Server 2012 R2 operating system.</span></span> <span data-ttu-id="acfab-129">cloudové služby MyHPCCNService se vytvoří ve skupině vztahů hello Hello *MyIBAffinityGroup*, a hello jiných cloudových služeb, vytvoří se ve skupině vztahů hello *MyAffinityGroup*.</span><span class="sxs-lookup"><span data-stu-id="acfab-129">hello cloud service MyHPCCNService is created in hello affinity group *MyIBAffinityGroup*, and hello other cloud services are created in hello affinity group *MyAffinityGroup*.</span></span> <span data-ttu-id="acfab-130">jsou povoleny Hello REST API Scheduleru úloh HPC a webového portálu HPC v hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="acfab-130">hello HPC Job Scheduler REST API and HPC web portal are enabled on hello head node.</span></span>

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



### <a name="example-4"></a><span data-ttu-id="acfab-131">Příklad 4</span><span class="sxs-lookup"><span data-stu-id="acfab-131">Example 4</span></span>
<span data-ttu-id="acfab-132">Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény.</span><span class="sxs-lookup"><span data-stu-id="acfab-132">hello following configuration file deploys an HPC Pack cluster in an existing domain forest.</span></span> <span data-ttu-id="acfab-133">Hello cluster má dvě hlavní uzel s místní databází, dvě šablony Azure uzlu vytvářejí a tři uzly Azure střední velikosti jsou vytvořeny pro šablonu Azure uzlu *AzureTemplate1*.</span><span class="sxs-lookup"><span data-stu-id="acfab-133">hello cluster has two head node with local databases, two Azure node templates are created, and three size Medium Azure nodes are created for Azure node template *AzureTemplate1*.</span></span> <span data-ttu-id="acfab-134">Soubor skriptu se spouští hello hlavního uzlu po dokončení konfigurace hlavního uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-134">A script file runs on hello head node after hello head node is configured.</span></span>

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

## <a name="troubleshooting"></a><span data-ttu-id="acfab-135">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="acfab-135">Troubleshooting</span></span>
* <span data-ttu-id="acfab-136">**Chyba "Virtuální síť neexistuje"** – Pokud spustíte skript toodeploy hello víc clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou hello "virtuální sítě *VNet\_název* není Existují".</span><span class="sxs-lookup"><span data-stu-id="acfab-136">**“VNet doesn’t exist” error** - If you run hello script toodeploy multiple clusters in Azure concurrently under one subscription, one or more deployments may fail with hello error “VNet *VNet\_Name* doesn't exist”.</span></span>
  <span data-ttu-id="acfab-137">Pokud k této chybě dojde, spusťte skript hello znovu pro nasazení hello se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="acfab-137">If this error occurs, run hello script again for hello failed deployment.</span></span>
* <span data-ttu-id="acfab-138">**Přístup k problému hello Internetu z hello virtuální síť Azure** – Pokud vytvoříte cluster s nový řadič domény pomocí hello skriptu nasazení, nebo manuálně povýšit řadič toodomain hlavního uzlu virtuálního počítače, může dojít k potížím připojení virtuálních počítačů toohello hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="acfab-138">**Problem accessing hello Internet from hello Azure virtual network** - If you create a cluster with a new domain controller by using hello deployment script, or you manually promote a head node VM toodomain controller, you may experience problems connecting hello VMs toohello Internet.</span></span> <span data-ttu-id="acfab-139">Tomuto problému může dojít, pokud na řadiči domény hello je automaticky nakonfigurovaný server DNS pro předávání a tento server DNS pro předávání nepřekládá správně.</span><span class="sxs-lookup"><span data-stu-id="acfab-139">This problem can occur if a forwarder DNS server is automatically configured on hello domain controller, and this forwarder DNS server doesn’t resolve properly.</span></span>
  
    <span data-ttu-id="acfab-140">toowork vyřešit tento problém, přihlaste se na řadič domény toohello a nastavení konfigurace služby pro předávání hello buď odeberte nebo konfigurace serveru DNS platný server pro předávání.</span><span class="sxs-lookup"><span data-stu-id="acfab-140">toowork around this problem, log on toohello domain controller and either remove hello forwarder configuration setting or configure a valid forwarder DNS server.</span></span> <span data-ttu-id="acfab-141">tooconfigure toto nastavení, ve Správci serveru klikněte na tlačítko **nástroje** >
    **DNS** tooopen Správce DNS a potom dvakrát klikněte na **předávání**.</span><span class="sxs-lookup"><span data-stu-id="acfab-141">tooconfigure this setting, in Server Manager click **Tools** >
    **DNS** tooopen DNS Manager, and then double-click **Forwarders**.</span></span>
* <span data-ttu-id="acfab-142">**Problémy s přístupem k síti RDMA z virtuálních počítačů náročné** – Pokud přidáte výpočetní systém Windows Server nebo zprostředkovatelský uzel virtuálních počítačů pomocí podporou RDMA velikost například A8 a A9, může zaznamenat problémy s připojením tyto sítě virtuálních počítačů toohello RDMA aplikace.</span><span class="sxs-lookup"><span data-stu-id="acfab-142">**Problem accessing RDMA network from compute-intensive VMs** - If you add Windows Server compute or broker node VMs using an RDMA-capable size such as A8 or A9, you may experience problems connecting those VMs toohello RDMA application network.</span></span> <span data-ttu-id="acfab-143">Jedním z důvodů, že k tomuto problému dochází je, pokud hello HpcVmDrivers rozšíření není správně nainstalován, po přidání clusteru toohello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="acfab-143">One reason this problem occurs is if hello HpcVmDrivers extension is not properly installed when hello VMs are added toohello cluster.</span></span> <span data-ttu-id="acfab-144">Například rozšíření zablokovaná v hello stav instalace.</span><span class="sxs-lookup"><span data-stu-id="acfab-144">For example, the extension might be stuck in hello installing state.</span></span>
  
    <span data-ttu-id="acfab-145">toowork vyřešit tento problém, první kontrola stavu hello hello rozšíření ve virtuálních počítačích hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-145">toowork around this problem, first check hello state of hello extension in hello VMs.</span></span> <span data-ttu-id="acfab-146">Pokud rozšíření hello není nainstalován správně, zkuste odebrat hello uzlů z clusteru HPC hello a poté znovu přidejte hello uzly.</span><span class="sxs-lookup"><span data-stu-id="acfab-146">If hello extension is not properly installed, try removing hello nodes from hello HPC cluster and then add hello nodes again.</span></span> <span data-ttu-id="acfab-147">Například můžete přidat výpočetním uzlu virtuální počítače pomocí skriptu přidat HpcIaaSNode.ps1 hello hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="acfab-147">For example, you can add compute node VMs by running hello Add-HpcIaaSNode.ps1 script on hello head node.</span></span>

## <a name="next-steps"></a><span data-ttu-id="acfab-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="acfab-148">Next steps</span></span>
* <span data-ttu-id="acfab-149">Zkuste spustit test zatížení v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="acfab-149">Try running a test workload on hello cluster.</span></span> <span data-ttu-id="acfab-150">Příklad najdete v tématu hello HPC Pack [Příručka Začínáme](https://technet.microsoft.com/library/jj884144).</span><span class="sxs-lookup"><span data-stu-id="acfab-150">For an example, see hello HPC Pack [getting started guide](https://technet.microsoft.com/library/jj884144).</span></span>
* <span data-ttu-id="acfab-151">Kurz tooscript hello nasazení v clusteru a spustíte úlohy HPC, najdete v části [začít pracovat s clusteru služby HPC Pack v aplikaci Excel a SOA úlohy Azure toorun](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="acfab-151">For a tutorial tooscript hello cluster deployment and run an HPC workload, see [Get started with an HPC Pack cluster in Azure toorun Excel and SOA workloads](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="acfab-152">Zkuste HPC Pack nástrojů toostart, zastavit, přidávat a odebírat výpočetních uzlů z clusteru, který vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="acfab-152">Try HPC Pack's tools toostart, stop, add, and remove compute nodes from a cluster you create.</span></span> <span data-ttu-id="acfab-153">V tématu [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md).</span><span class="sxs-lookup"><span data-stu-id="acfab-153">See [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md).</span></span>
* <span data-ttu-id="acfab-154">tooget nastavit toosubmit úlohy toohello clusteru z místního počítače, najdete v části [clusteru HPC odeslání úloh místní počítač tooan HPC Pack v Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="acfab-154">tooget set up toosubmit jobs toohello cluster from a local computer, see [Submit HPC jobs from an on-premises computer tooan HPC Pack cluster in Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

