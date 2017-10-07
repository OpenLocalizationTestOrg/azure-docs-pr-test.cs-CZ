---
title: "virtuální počítač s Linuxem aaaAzure velikosti - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro Linux s vysokým výkonem virtuálních počítačů v Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 0b02ee592902ff8097a71898faea1ac17a947d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Vysoký výkon výpočetní velikosti virtuálního počítače s Linuxem

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA podporovat instancí
Podmnožinu hello náročné instancí (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA). Toto rozhraní je kromě velikosti virtuálních počítačů pro dostupné tooother toohello standardního Azure síťového rozhraní. 
  
Toto rozhraní umožňuje hello podporující RDMA instance toocommunicate přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů. Tyto funkce RDMA může zvýšit hello škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).

Toto jsou požadavky pro virtuální počítače s podporou RDMA Linuxem tooaccess hello Azure RDMA sítě:
 
* **Distribuce** – je nutné nasadit virtuální počítače z podporující RDMA SUSE Linux Enterprise Server (SLES) nebo hello podvodný Wave softwaru (dříve OpenLogic) na základě CentOS HPC obrázků v Azure Marketplace. Hello následující obrázky Marketplace podpory RDMA připojení:
  
    * SLES 12 SP1 pro prostředí HPC nebo SLES 12 SP1 pro HPC (Premium)
    
    * Na základě centOS 7.3 HPC, na základě CentOS 7.1 HPC, na základě CentOS 6.8 HPC nebo na základě CentOS verze 6.5 HPC  
 
        > [!NOTE]
        > Pro virtuální počítače H-series doporučujeme, abyste SLES 12 SP1 pro bitovou kopii prostředí HPC nebo na základě CentOS 7.1 nebo novější bitovou kopii prostředí HPC.
        >
        > Na hello Image na základě CentOS HPC, jsou zakázané jádra aktualizace v hello **yum** konfigurační soubor. Je to proto hello Linux RDMA ovladače distribuují jako balíček RPM a aktualizací ovladače nemusí fungovat, pokud se aktualizuje hello jádra.
        > 
        > 
* **MPI** -Intel MPI knihovny 5.x
  
    V závislosti na hello Marketplace image si zvolíte, samostatnou licencování, instalaci, nebo konfigurace Intel MPI může být potřeba, následujícím způsobem: 
  
  * **SLES 12 SP1 pro bitovou kopii prostředí HPC** -Intel MPI balíčky jsou distribuovány na hello virtuálních počítačů. Nainstalujte spuštěním hello následující příkaz:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **Na základě centOS Image HPC** -Intel MPI 5.1 je již nainstalován.  
    
    Konfigurace dalších systému je potřebné toorun úloh MPI na clusterových virtuálních počítačích. Například v clusteru virtuálních počítačů, je nutné tooestablish vztah důvěryhodnosti mezi hello výpočetních uzlů. Typické nastavení, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="network-topology-considerations"></a>Aspekty topologie sítě
* Na podporou RDMA virtuální počítače s Linuxem v Azure je Eth1 vyhrazený pro RDMA síťový provoz. Neměňte nastavení Eth1 nebo všechny informace v konfiguračním souboru hello odkazující toothis sítě. Eth0 je vyhrazený pro regulární Azure síťový provoz.

* V Azure není podporována IP přes InfiniBand (IB). Je podporován pouze RDMA přes IB.

## <a name="using-hpc-pack"></a>Pomocí sady HPC Pack
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností pro vás toouse hello náročné instancí operačního systému Linux. nejnovější verze sady HPC Pack Hello podporovat několik Linuxových distribucích toorun na výpočetních uzlech nasazené ve virtuálních počítačích Azure, spravuje hlavního uzlu systému Windows Server. S podporou RDMA Linux výpočetní uzly systémem Intel MPI HPC Pack můžete naplánovat a spustit aplikací Linux MPI tuto síť RDMA hello přístup. V tématu [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Další velikosti
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](sizes-memory.md)
- [Optimalizované z hlediska úložiště](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Další kroky

- tooget spuštění nasazení a používání náročné velikosti s RDMA pro systémy Linux, najdete v části [nastavení aplikací MPI toorun clusteru Linux RDMA](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.




