---
title: "Virtuální počítač Azure s Linuxem velikostí - GPU | Microsoft Docs"
description: "Seznamy různých GPU optimalizované velikosti, které jsou k dispozici pro virtuální počítače s Linuxem v Azure."
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
ms.openlocfilehash: 5c9bf89feba519147b07f2810fe4da882664e89e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="gpu-linux-vm-sizes"></a><span data-ttu-id="fab3c-103">Velikost virtuálního počítače s Linuxem GPU</span><span class="sxs-lookup"><span data-stu-id="fab3c-103">GPU Linux VM sizes</span></span>

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

<span data-ttu-id="fab3c-104">Postup instalace a ověření ovladačů najdete v tématu [instalaci ovladačů N-series pro Linux](n-series-driver-setup.md).</span><span class="sxs-lookup"><span data-stu-id="fab3c-104">For driver installation and verification steps, see [N-series driver setup for Linux](n-series-driver-setup.md).</span></span>

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* <span data-ttu-id="fab3c-105">Nedoporučujeme instalaci X server nebo jiné systémy, které používat ovladač nouveau na virtuálních počítačích Ubuntu názvového kontextu.</span><span class="sxs-lookup"><span data-stu-id="fab3c-105">We don't recommend installing X server or other systems that use the nouveau driver on Ubuntu NC VMs.</span></span> <span data-ttu-id="fab3c-106">Před instalací NVIDIA GPU ovladače, je nutné zakázat nouveau ovladače.</span><span class="sxs-lookup"><span data-stu-id="fab3c-106">Before installing NVIDIA GPU drivers, you need to disable the nouveau driver.</span></span>  

## <a name="other-sizes"></a><span data-ttu-id="fab3c-107">Další velikosti</span><span class="sxs-lookup"><span data-stu-id="fab3c-107">Other sizes</span></span>
- [<span data-ttu-id="fab3c-108">Obecné účely</span><span class="sxs-lookup"><span data-stu-id="fab3c-108">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="fab3c-109">Optimalizované z hlediska výpočetních služeb</span><span class="sxs-lookup"><span data-stu-id="fab3c-109">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="fab3c-110">Optimalizované z hlediska paměti</span><span class="sxs-lookup"><span data-stu-id="fab3c-110">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="fab3c-111">Optimalizované z hlediska úložiště</span><span class="sxs-lookup"><span data-stu-id="fab3c-111">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="fab3c-112">Vysokovýkonné výpočetní prostředí</span><span class="sxs-lookup"><span data-stu-id="fab3c-112">High performance compute</span></span>](sizes-hpc.md)

## <a name="next-steps"></a><span data-ttu-id="fab3c-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fab3c-113">Next steps</span></span>
<span data-ttu-id="fab3c-114">Další informace o [Azure výpočetní jednotky (ACU)](acu.md) můžete porovnat výpočetní výkon v Azure SKU.</span><span class="sxs-lookup"><span data-stu-id="fab3c-114">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>