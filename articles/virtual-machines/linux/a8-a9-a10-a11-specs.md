---
title: "O náročné virtuální počítače s Linuxem | Microsoft Docs"
description: "Získejte základní informace a požadavky pro používání H-series a A8, A9, A10 a A11 náročných velikosti pro virtuální počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 16cf6e2d-f401-4b22-af8c-9a337179f5f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a2307f7055966ec7146b5da0b4daf1ad469abe2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="about-h-series-and-compute-intensive-a-series-vms-for-linux"></a>O H-series a náročné virtuální počítače A-series pro Linux
Tady je základní informace a některé předpoklady pro použití novější Azure H-series a starší velikosti A8, A9, A10 a A11, také známé jako *náročné* instance. Tento článek se zaměřuje na používání těchto velikostí pro virtuální počítače s Linuxem. Můžete také použít, je [virtuálních počítačů Windows](../windows/a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Základní specifikace, kapacity úložiště a disku podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Přístup k síti RDMA
Můžete vytvořit clustery s podporou RDMA virtuálních počítačů Linux, spusťte jeden z následujících podporované distribuce systému Linux HPC a podporovaných implementace MPI využívat výhod sítě Azure RDMA. V tématu [nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) pro možnosti nasazení a ukázkové kroky konfigurace.

* **Distribuce** -je nutné nasadit virtuální počítače z Image podporující RDMA SUSE Linux Enterprise Server (SLES) nebo na základě CentOS HPC podvodný Wave softwaru (dříve OpenLogic) v Azure Marketplace. Na následujících obrázcích Marketplace podpory RDMA připojení:
  
    * SLES 12 SP1 pro prostředí HPC, SLES 12 SP1 pro HPC (Premium)
    
    * Na základě centOS 7.1 HPC, na základě CentOS verze 6.5 HPC  
 
        > [!NOTE]
        > Pro virtuální počítače H-series doporučujeme, abyste buď SLES 12 SP1 pro bitovou kopii prostředí HPC, nebo na základě CentOS 7.1 HPC image.
        >
        > Na základě CentOS HPC Image, jsou zakázané jádra aktualizace v **yum** konfigurační soubor. Je to proto, že jsou ovladače Linux RDMA distribuován jako balíček RPM a aktualizací ovladače nemusí fungovat, pokud se aktualizuje jádra.
        > 
        > 
* **MPI** -Intel MPI knihovny 5.x
  
    V závislosti na bitovou kopii Marketplace si zvolíte, samostatnou licencování, instalaci, nebo konfigurace Intel MPI může být potřeba, následujícím způsobem: 
  
  * **SLES 12 SP1 pro bitovou kopii prostředí HPC** -Intel MPI balíčků distribuováno do virtuálního počítače. Nainstalujte spuštěním následujícího příkazu:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **Na základě centOS Image HPC** -Intel MPI 5.1 je již nainstalován.  
    
    Konfigurace dalších systému je potřeba k spouštění úloh MPI na clusterových virtuálních počítačích. Například v clusteru virtuálních počítačů, musíte vytvořit vztah důvěryhodnosti mezi výpočetní uzly. Typické nastavení, najdete v části [nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="considerations-for-hpc-pack-and-linux"></a>Důležité informace týkající se sady HPC Pack a Linux
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), řešení společnosti Microsoft volné HPC clusteru a úlohy správy, poskytuje jednu možnost budete muset používat náročné instancí operačního systému Linux. Nejnovější verze sady HPC Pack podporu několik distribucí Linux ke spuštění na výpočetních uzlech nasazené ve virtuálních počítačích Azure, spravuje hlavního uzlu systému Windows Server. S podporou RDMA Linux výpočetní uzly systémem Intel MPI HPC Pack můžete naplánovat a spustit Linux MPI aplikace, které přístup k síti RDMA. Abyste mohli začít, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="network-topology-considerations"></a>Aspekty topologie sítě
* Na podporou RDMA virtuální počítače s Linuxem v Azure je Eth1 vyhrazený pro RDMA síťový provoz. Neměňte nastavení Eth1 nebo jakékoli informace v souboru konfigurace odkazující na tuto síť. Eth0 je vyhrazený pro regulární Azure síťový provoz.
* V Azure není podporována IP přes InfiniBand (IB). Je podporován pouze RDMA přes IB.



## <a name="next-steps"></a>Další kroky
* Podrobnosti o dostupnosti a ceny velikostí náročné najdete v tématu [ceny virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).
* Podrobnosti o disku a kapacity úložiště najdete v tématu [velikosti virtuálních počítačů](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Abyste mohli začít, nasazení a používání náročné velikosti s RDMA v systému Linux, najdete v části [nastavení clusteru s podporou Linux RDMA ke spuštění aplikací MPI](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

