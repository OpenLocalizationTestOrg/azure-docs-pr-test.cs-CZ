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
# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-hello-hpc-pack-iaas-deployment-script"></a>Vytvořte cluster s vysoce výkonné výpočty (HPC) Linux se skript nasazení HPC Pack IaaS hello
Spusťte hello HPC Pack IaaS nasazení prostředí PowerShell skriptu toodeploy dokončení clusteru HPC Pack 2012 R2 pro Linux zatížení ve virtuálních počítačích Azure. Hello clusteru se skládá z služby Active Directory připojené k hlavnímu uzlu systémem Windows Server a Microsoft HPC Pack a výpočetní uzly, které spusťte jeden z hello distribucí Linux podporované sadou HPC Pack. Pokud chcete, aby toodeploy clusteru služby HPC Pack v úlohy Azure pro Windows, přečtěte si téma [vytvořit cluster Windows HPC s hello skript nasazení HPC Pack IaaS](../../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Můžete použít také toodeploy šablony Azure Resource Manager clusteru služby HPC Pack. Příklad, naleznete v části [vytvoření clusteru prostředí HPC s Linux výpočetní uzly](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

> [!IMPORTANT] 
> Hello skript prostředí PowerShell popsaný v tomto článku vytváří cluster s podporou Microsoft HPC Pack 2012 R2 v Azure pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
> Kromě toho hello skriptu popsaného v tomto článku nepodporuje HPC Pack 2016.

[!INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Příklad konfiguračního souboru
Hello následující konfigurační soubor vytvoří řadič domény a doménové struktury domény a nasadí cluster služby HPC Pack, který má jeden hlavního uzlu s místní databází a 10 Linuxových výpočetních uzlů. Všechny hello cloudové služby jsou vytvořené přímo v umístění východní Asie hello. Hello Linux výpočetní uzly jsou vytvořeny ve dvou cloudové služby a dva účty úložiště (tedy *MyLnxCN 0001* k *MyLnxCN 0005* v *MyLnxCNService01* a *mylnxstorage01*, a *MyLnxCN-0006* k *MyLnxCN 0010* v *MyLnxCNService02* a *mylnxstorage02* ). Hello výpočetní uzly jsou vytvořeny z image OpenLogic CentOS Linux verze 7.0. 

Nahraďte vlastními hodnotami pro název odběru a názvy hello účtu a služby.

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
## <a name="troubleshooting"></a>Řešení potíží
* **Chyba "Virtuální síť neexistuje"**. Pokud spustíte hello HPC Pack IaaS nasazení skriptu toodeploy víc clusterů v Azure souběžně v rámci jednoho předplatného, jeden nebo více nasazení může selhat s chybou hello "virtuální sítě *VNet\_název* neexistuje".
  Pokud k této chybě dojde, znovu spusťte hello skriptu pro nasazení hello se nezdařilo.
* **Přístup k problému hello Internetu z hello virtuální síť Azure**. Pokud vytváření clusteru HPC Pack s nový řadič domény pomocí skriptu nasazení hello nebo manuálně povýšit hlavního uzlu řadiče toodomain virtuálních počítačů, může zaznamenat problémy s připojením hello virtuální počítače ve virtuální síti Azure toohello hello Internetu. Tato situace může nastat, pokud na řadiči domény hello je automaticky nakonfigurovaný server DNS pro předávání a tento server DNS pro předávání nepřekládá správně.
  
    toowork vyřešit tento problém, přihlaste se na řadič domény toohello a nastavení konfigurace služby pro předávání hello buď odeberte nebo konfigurace serveru DNS platný server pro předávání. toodo, ve Správci serveru klikněte na **nástroje** > **DNS** tooopen Správce DNS a potom dvakrát klikněte na **předávání**.

## <a name="next-steps"></a>Další kroky
* V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md) informace o podporovaných Linuxových distribucích přesouvání dat a odesílání úloh tooan HPC Pack clusteru s Linuxem výpočetních uzlů.
* Kurzy, které používají hello skriptu toocreate cluster a spustit úlohy HPC pro Linux najdete v tématu:
  * [Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-namd.md)
  * [Spuštění OpenFOAM pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-openfoam.md)
  * [Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure](hpcpack-cluster-starccm.md)

