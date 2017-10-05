---
title: "Mapování sítě pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak k mapování sítě při replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a><span data-ttu-id="886c6-103">Krok 7: Mapovat sítě pro replikaci virtuálního počítače technologie Hyper-V do sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="886c6-103">Step 7: Map networks for Hyper-V VM replication to a secondary site</span></span>


<span data-ttu-id="886c6-104">Po nastavení [zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md) replikaci virtuálních počítačů technologie Hyper-V (VM) do sekundární lokality System Center Virtual Machine Manager (VMM), použijte tento článek ke konfiguraci mapování sítě pro replikaci virtuálního počítače (VM) technologie Hyper-V do sekundární lokality, pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="886c6-104">After setting up [source and target settings](vmm-to-vmm-walkthrough-source-target.md) for replicating Hyper-V virtual machines (VMs) to a secondary System Center Virtual Machine Manager (VMM) site, use this article to configure network mapping for Hyper-V virtual machine (VM) replication to a secondary site, using  [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="886c6-105">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="886c6-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="886c6-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="886c6-106">Before you start</span></span>

- <span data-ttu-id="886c6-107">Další informace o [mapování sítě](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="886c6-107">Learn about [network mapping](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) before you start.</span></span>
- <span data-ttu-id="886c6-108">Zkontrolujte, zda jsou virtuální počítače na serverech VMM připojen k síti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="886c6-108">Verify that virtual machines on VMM servers are connected to a VM network.</span></span>

## <a name="configure-network-mapping"></a><span data-ttu-id="886c6-109">Konfigurace mapování sítě</span><span class="sxs-lookup"><span data-stu-id="886c6-109">Configure network mapping</span></span>

1. <span data-ttu-id="886c6-110">V **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.</span><span class="sxs-lookup"><span data-stu-id="886c6-110">In **Network Mapping** > **Network mappings**, click **+Network Mapping**.</span></span>
2. <span data-ttu-id="886c6-111">V **přidání mapování sítě** vyberte zdrojové a cílové servery VMM.</span><span class="sxs-lookup"><span data-stu-id="886c6-111">In **Add network mapping** tab, select the source and target VMM servers.</span></span> <span data-ttu-id="886c6-112">Sítě virtuálních počítačů, které jsou přidružené k serverům VMM jsou načteny.</span><span class="sxs-lookup"><span data-stu-id="886c6-112">The VM networks associated with the VMM servers are retrieved.</span></span>
3. <span data-ttu-id="886c6-113">V **zdrojové síti**, vyberte síť, kterou chcete použít v seznamu sítě virtuálních počítačů spojené s primárním serverem VMM.</span><span class="sxs-lookup"><span data-stu-id="886c6-113">In **Source network**, select the network you want to use from the list of VM networks associated with the primary VMM server.</span></span>
4. <span data-ttu-id="886c6-114">V **Cílová síť**, vyberte síť, kterou chcete použít na sekundárním serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="886c6-114">In **Target network**, select the network you want to use on the secondary VMM server.</span></span> <span data-ttu-id="886c6-115">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="886c6-115">Then click **OK**.</span></span>

    ![Mapování sítě](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="886c6-117">Když se začne mapovat síť, dojde k tomuto:</span><span class="sxs-lookup"><span data-stu-id="886c6-117">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="886c6-118">Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů se připojí k cílové síti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="886c6-118">Any existing replica virtual machines that correspond to the source VM network will be connected to the target VM network.</span></span>
* <span data-ttu-id="886c6-119">Nové virtuální počítače, které jsou připojené ke zdrojové síti virtuálních počítačů se připojí k namapované cílové síti po replikaci.</span><span class="sxs-lookup"><span data-stu-id="886c6-119">New virtual machines that are connected to the source VM network will be connected to the target mapped network after replication.</span></span>
* <span data-ttu-id="886c6-120">Pokud upravíte existující mapování s novou sítí, virtuální počítače repliky budou připojené pomocí nového nastavení.</span><span class="sxs-lookup"><span data-stu-id="886c6-120">If you modify an existing mapping with a new network, replica virtual machines will be connected using the new settings.</span></span>
* <span data-ttu-id="886c6-121">Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="886c6-121">If the target network has multiple subnets and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine will be connected to that target subnet after failover.</span></span> <span data-ttu-id="886c6-122">Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.</span><span class="sxs-lookup"><span data-stu-id="886c6-122">If there’s no target subnet with a matching name, the virtual machine will be connected to the first subnet in the network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="886c6-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="886c6-123">Next steps</span></span>

<span data-ttu-id="886c6-124">Přejděte na [krok 8: Nakonfigurujte zásady replikace](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="886c6-124">Go to [Step 8: Configure a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>