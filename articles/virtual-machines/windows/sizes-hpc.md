---
title: "velikost virtuálního počítače s Windows aaaAzure - HPC | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro Windows s vysokým výkonem virtuálních počítačů v Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 092bc32cfe048f439ad833911bef4ed17eda7977
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-performance-compute-vm-sizes"></a>Velikosti virtuálních počítačů pro výpočetní vysoký výkon

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA podporovat instancí
Podmnožinu hello náročné instancí (H16r, H16mr, A8 a A9) funkce síťové rozhraní pro připojení do paměti vzdáleného přímý přístup do (počítače RDMA). Toto rozhraní je kromě velikosti virtuálních počítačů pro dostupné tooother toohello standardního Azure síťového rozhraní. 
  
Toto rozhraní umožňuje hello podporující RDMA instance toocommunicate přes síť InfiniBand, provoz se FDR sazby za H16r a H16mr virtuální počítače a QDR sazby A8 a A9 virtuálních počítačů. Tyto funkce RDMA může zvýšit hello škálovatelnost a výkon aplikací rozhraní MPI (Message Passing).

Toto jsou požadavky pro tooaccess podporující RDMA virtuálních počítačů Windows hello Azure RDMA sítě: 

* **Operační systém**
  
  Windows Server 2012 R2, Windows Server 2012
  
  > [!NOTE]
  > Windows Server 2016 v současné době nepodporuje připojení RDMA v Azure.
  >

* **Skupinu dostupnosti nebo Cloudová služba** – nasadit hello podporující RDMA virtuálních počítačů v hello stejné dostupnosti (při použití modelu nasazení Azure Resource Manager hello) nebo hello stejné cloudové služby (při použití modelu nasazení classic hello). Pokud používáte Azure Batch, hello podporující RDMA virtuální počítače musí být v hello stejného fondu.

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 nebo novější, Intel MPI knihovny 5.x

  Podporované implementace MPI použijte hello Microsoft Network Direct toocommunicate rozhraní mezi instancemi. 

* **Adresní prostor sítě RDMA** -hello RDMA sítě v Azure si vyhrazuje 172.16.0.0/16 prostoru adres hello. aplikací MPI toorun v instancích nasazený ve virtuální síť Azure, ujistěte se, že adresního prostoru virtuální sítě hello nepřekrývá hello RDMA sítě.

* **Rozšíření virtuálního počítače HpcVmDrivers** -na podporu rdma virtuálních počítačích, musíte přidat hello HpcVmDrivers rozšíření tooinstall síťové ovladače zařízení systému Windows pro připojení RDMA. (V určitých nasazeních A8 a A9 instancí hello HpcVmDrivers rozšíření se přidá automaticky.) tooadd hello virtuálního počítače rozšíření tooa virtuálních počítačů, můžete použít [prostředí Azure PowerShell](/powershell/azure/overview) rutiny. 

  
  Dobrý den, následující příkaz nainstaluje hello nejnovější verze 1.1 HpcVMDrivers rozšíření na existující podporující RDMA virtuální počítač s názvem *Můjvp* nasazené v hello skupinu prostředků s názvem *myResourceGroup* v hello  *Západní USA* oblasti:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Další informace najdete v tématu [rozšíření virtuálního počítače a funkce](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Můžete také pracovat se rozšíření pro virtuální počítače nasazené v hello [modelu nasazení classic](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>Pomocí sady HPC Pack

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), volné HPC clusteru a úlohy řešení správy společnosti Microsoft, je jednou z možností pro toocreate výpočetní cluster aplikací MPI systému Windows Azure toorun a další úlohy HPC. HPC Pack 2012 R2 a novější verze zahrnují běhového prostředí pro MS-MPI využívající síť Azure RDMA hello při nasazení na virtuálních počítačích RDMA podporovat.




## <a name="other-sizes"></a>Další velikosti
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)
- [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)
- [Optimalizované z hlediska GPU](sizes-gpu.md)

## <a name="next-steps"></a>Další kroky

- Kontrolní seznamy toouse hello náročné instancí pomocí sady HPC Pack v systému Windows Server, najdete v části [nastavení clusteru s podporou Windows RDMA s aplikací MPI toorun HPC Pack](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- toouse náročné instancí při spuštění aplikací MPI pomocí služby Azure Batch, najdete v části [použití s více instancemi úloh toorun aplikací rozhraní MPI (Message Passing) v Azure Batch](../../batch/batch-mpi.md).

- Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.




