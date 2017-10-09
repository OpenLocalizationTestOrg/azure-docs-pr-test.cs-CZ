---
title: "velikosti aaaWindows virtuálních počítačů v Azure | Microsoft Docs"
description: "Obsahuje seznam různých velikostech hello k dispozici pro virtuální počítače s Windows v Azure."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Velikosti pro virtuální počítače s Windows v Azure

Tento článek popisuje hello dostupných velikostí a možnosti pro hello virtuální počítače Azure můžete toorun vaše aplikace pro Windows a úlohy. Nabízí taky toobe aspekty nasazení vědět, když plánujete toouse tyto prostředky.  Tento článek je také k dispozici pro [virtuální počítače s Linuxem](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Obecné účely](sizes-general.md)          | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7 | Vyvážený poměr procesorů k paměti. Ideální pro testování a vývoj, malé toomedium databáze a nízkou toomedium provoz webové servery. |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)        | Služby FS, F             | Vysoký poměr procesorů k paměti. Vhodné pro webové servery se středním provozem, síťová zařízení, dávkové procesy a aplikační servery.        |
| [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Vysoký poměr paměť procesoru. Výborně hodí pro servery relační databáze, střední toolarge mezipaměti a analýzy v paměti.                 |
| [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)        | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| [GPU](sizes-gpu.md)            | VS, NC            | Specializované virtuální počítače cílené pro velkou grafické vykreslování a úpravy videa. K dispozici jeden nebo více grafickými procesory.       |
| [Vysokovýkonné výpočetní prostředí](sizes-hpc.md) | H, A8 11          | Naše nejrychlejší a procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA). 

<br> 

- Informace o cenách z hello různé velikosti najdete v tématu [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- Obecná omezení toosee na virtuálních počítačích Azure, najdete v části [předplatného Azure a omezení služby, kvóty a omezení](../../azure-subscription-service-limits.md).
- Náklady na úložiště se počítá samostatně na základě použitých stránek v účtu úložiště hello. Podrobnosti najdete [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).
- Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.



## <a name="rest-api"></a>Rozhraní REST API

Informace o používání tooquery hello REST API pro velikosti virtuálních počítačů najdete v tématu hello následující:

- [Seznam dostupných velikostí virtuálních počítačů pro změnu velikosti](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Seznam dostupných velikostí virtuálních počítačů pro předplatné](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [Seznam dostupných velikostí virtuálních počítačů v nastavení dostupnosti](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.

## <a name="next-steps"></a>Další kroky

Další informace o hello různé velikosti virtuálních počítačů, které jsou k dispozici:
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](../virtual-machines-windows-sizes-memory.md)
- [Optimalizované z hlediska úložiště](../virtual-machines-windows-sizes-storage.md)
- [Optimalizované z hlediska GPU](sizes-gpu.md)
- [Vysokovýkonné výpočetní prostředí](sizes-hpc.md)



