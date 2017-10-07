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
# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a>Vytvoření clusteru se systémem Windows vysoce výkonné výpočty (HPC) s skript nasazení HPC Pack IaaS hello
Spusťte kompletní clusteru HPC Pack 2012 R2 pro úlohy Windows hello HPC Pack IaaS nasazení prostředí PowerShell skriptu toodeploy ve virtuálních počítačích Azure. Hello clusteru se skládá ze Active Directory připojený hlavního uzlu systémem Windows Server a Microsoft HPC Pack a dalších Windows výpočetní prostředky, které zadáte. Pokud chcete pro Linux úlohy toodeploy clusteru služby HPC Pack v Azure, najdete v části [vytvořit cluster Linux HPC s hello skript nasazení HPC Pack IaaS](../../linux/classic/hpcpack-cluster-powershell-script.md). Můžete použít také toodeploy šablony Azure Resource Manager clusteru služby HPC Pack. Příklady najdete v tématu [vytvoření clusteru prostředí HPC](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) a [vytvoření clusteru prostředí HPC s bitovou kopii vlastní výpočetní uzel](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

> [!IMPORTANT] 
> Hello skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
> Kromě toho hello skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Příklad konfigurační soubory
Následující příklady v hello nahraďte vlastní hodnoty pro Id předplatného nebo název a názvy hello účtu a služby.

### <a name="example-1"></a>Příklad 1
Hello následujícího konfiguračního souboru nasazení clusteru služby HPC Pack, který má hlavního uzlu s místní databází a operačním systémem Windows Server 2012 R2 hello pět výpočetních uzlů. Všechny hello cloudové služby jsou vytvořené přímo v hello umístění západní USA. Hello hlavního uzlu funguje jako řadič domény hello domény doménové struktury.

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

### <a name="example-2"></a>Příklad 2
Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény. Hello clusteru má 1 hlavního uzlu s místní databází a 12 výpočetní uzly s hello rozšíření BGInfo virtuálního počítače, které jsou použity.
Automatická instalace aktualizací systému Windows je zakázáno pro všechny hello virtuálních počítačů v doménové struktuře domény hello. Všechny hello cloudové služby jsou vytvořené přímo v umístění ve východní Asii. Hello výpočetní uzly jsou vytvořené v tři cloudové služby a tři účty úložiště: *MyHPCCN 0001* příliš*MyHPCCN 0005* v *MyHPCCNService01* a  *mycnstorage01*; *MyHPCCN-0006* příliš*MyHPCCN0010* v *MyHPCCNService02* a *mycnstorage02*; a  *MyHPCCN-0011* příliš*MyHPCCN 0012* v *MyHPCCNService03* a *mycnstorage03*). Hello výpočetní uzly jsou vytvořeny ze stávající privátní image zachyceného v výpočetního uzlu. Hello automaticky zvětšovat a zmenšovat s výchozím nastavení je povolena služba zvětšovat a zmenšovat intervalech.

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

### <a name="example-3"></a>Příklad 3
Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény. Hello cluster obsahuje jeden hlavní uzel, jeden server databáze s 500 GB datový disk, dva uzly zprostředkovatele spuštěný hello Windows Server 2012 R2 operační systém a operačním systémem Windows Server 2012 R2 hello pět výpočetních uzlů. cloudové služby MyHPCCNService se vytvoří ve skupině vztahů hello Hello *MyIBAffinityGroup*, a hello jiných cloudových služeb, vytvoří se ve skupině vztahů hello *MyAffinityGroup*. jsou povoleny Hello REST API Scheduleru úloh HPC a webového portálu HPC v hello hlavního uzlu.

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



### <a name="example-4"></a>Příklad 4
Hello následující konfigurační soubor nasadí cluster služby HPC Pack v existující doménové struktuře domény. Hello cluster má dvě hlavní uzel s místní databází, dvě šablony Azure uzlu vytvářejí a tři uzly Azure střední velikosti jsou vytvořeny pro šablonu Azure uzlu *AzureTemplate1*. Soubor skriptu se spouští hello hlavního uzlu po dokončení konfigurace hlavního uzlu hello.

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

## <a name="troubleshooting"></a>Řešení potíží
* **Chyba "Virtuální síť neexistuje"** – Pokud spustíte skript toodeploy hello víc clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou hello "virtuální sítě *VNet\_název* není Existují".
  Pokud k této chybě dojde, spusťte skript hello znovu pro nasazení hello se nezdařilo.
* **Přístup k problému hello Internetu z hello virtuální síť Azure** – Pokud vytvoříte cluster s nový řadič domény pomocí hello skriptu nasazení, nebo manuálně povýšit řadič toodomain hlavního uzlu virtuálního počítače, může dojít k potížím připojení virtuálních počítačů toohello hello Internetu. Tomuto problému může dojít, pokud na řadiči domény hello je automaticky nakonfigurovaný server DNS pro předávání a tento server DNS pro předávání nepřekládá správně.
  
    toowork vyřešit tento problém, přihlaste se na řadič domény toohello a nastavení konfigurace služby pro předávání hello buď odeberte nebo konfigurace serveru DNS platný server pro předávání. tooconfigure toto nastavení, ve Správci serveru klikněte na tlačítko **nástroje** >
    **DNS** tooopen Správce DNS a potom dvakrát klikněte na **předávání**.
* **Problémy s přístupem k síti RDMA z virtuálních počítačů náročné** – Pokud přidáte výpočetní systém Windows Server nebo zprostředkovatelský uzel virtuálních počítačů pomocí podporou RDMA velikost například A8 a A9, může zaznamenat problémy s připojením tyto sítě virtuálních počítačů toohello RDMA aplikace. Jedním z důvodů, že k tomuto problému dochází je, pokud hello HpcVmDrivers rozšíření není správně nainstalován, po přidání clusteru toohello hello virtuálních počítačů. Například rozšíření zablokovaná v hello stav instalace.
  
    toowork vyřešit tento problém, první kontrola stavu hello hello rozšíření ve virtuálních počítačích hello. Pokud rozšíření hello není nainstalován správně, zkuste odebrat hello uzlů z clusteru HPC hello a poté znovu přidejte hello uzly. Například můžete přidat výpočetním uzlu virtuální počítače pomocí skriptu přidat HpcIaaSNode.ps1 hello hello hlavního uzlu.

## <a name="next-steps"></a>Další kroky
* Zkuste spustit test zatížení v clusteru hello. Příklad najdete v tématu hello HPC Pack [Příručka Začínáme](https://technet.microsoft.com/library/jj884144).
* Kurz tooscript hello nasazení v clusteru a spustíte úlohy HPC, najdete v části [začít pracovat s clusteru služby HPC Pack v aplikaci Excel a SOA úlohy Azure toorun](../../virtual-machines-windows-excel-cluster-hpcpack.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Zkuste HPC Pack nástrojů toostart, zastavit, přidávat a odebírat výpočetních uzlů z clusteru, který vytvoříte. V tématu [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md).
* tooget nastavit toosubmit úlohy toohello clusteru z místního počítače, najdete v části [clusteru HPC odeslání úloh místní počítač tooan HPC Pack v Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

