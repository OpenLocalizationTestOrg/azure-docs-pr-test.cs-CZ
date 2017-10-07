---
title: "aaaAzure virtuálního počítače s Linuxem velikostí - GPU | Microsoft Docs"
description: "Uvádí hello různé GPU optimalizované velikosti k dispozici pro virtuální počítače s Linuxem v Azure."
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
ms.openlocfilehash: e98f720499be37df4048aeb513aa4f6b187b7335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="gpu-linux-vm-sizes"></a>Velikost virtuálního počítače s Linuxem GPU

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Postup instalace a ověření ovladačů najdete v tématu [instalaci ovladačů N-series pro Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Nedoporučujeme instalaci X server nebo jiné systémy, které používat ovladač nouveau hello na virtuálních počítačích Ubuntu názvového kontextu. Před instalací NVIDIA GPU ovladačů, musíte toodisable hello nouveau ovladačů.  

## <a name="other-sizes"></a>Další velikosti
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](sizes-memory.md)
- [Optimalizované z hlediska úložiště](sizes-storage.md)
- [Vysokovýkonné výpočetní prostředí](sizes-hpc.md)

## <a name="next-steps"></a>Další kroky
Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.