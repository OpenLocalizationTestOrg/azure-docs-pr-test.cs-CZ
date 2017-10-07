---
title: "aaaMap sítě pro virtuální počítač Hyper-V replikace tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toomap sítě při replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM s Azure Site Recovery."
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
ms.openlocfilehash: d4f621df4ce08ae055bc6809daea0b71b76754ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-tooa-secondary-site"></a><span data-ttu-id="aa00d-103">Krok 7: Mapování sítě pro virtuální počítač Hyper-V replikace tooa sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="aa00d-103">Step 7: Map networks for Hyper-V VM replication tooa secondary site</span></span>


<span data-ttu-id="aa00d-104">Po nastavení [zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md) pro replikaci technologie Hyper-V virtuální počítače (VM) tooa sekundární System Center Virtual Machine Manager (VMM) lokality, použijte toto mapování článku tooconfigure sítě pro virtuální technologii Hyper-V počítač (VM) replikace tooa sekundární lokality, pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa00d-104">After setting up [source and target settings](vmm-to-vmm-walkthrough-source-target.md) for replicating Hyper-V virtual machines (VMs) tooa secondary System Center Virtual Machine Manager (VMM) site, use this article tooconfigure network mapping for Hyper-V virtual machine (VM) replication tooa secondary site, using  [Azure Site Recovery](site-recovery-overview.md).</span></span>

<span data-ttu-id="aa00d-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="aa00d-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="aa00d-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="aa00d-106">Before you start</span></span>

- <span data-ttu-id="aa00d-107">Další informace o [mapování sítě](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) před zahájením.</span><span class="sxs-lookup"><span data-stu-id="aa00d-107">Learn about [network mapping](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) before you start.</span></span>
- <span data-ttu-id="aa00d-108">Ověřte, zda jsou virtuální počítače na serverech VMM připojené tooa síť virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aa00d-108">Verify that virtual machines on VMM servers are connected tooa VM network.</span></span>

## <a name="configure-network-mapping"></a><span data-ttu-id="aa00d-109">Konfigurace mapování sítě</span><span class="sxs-lookup"><span data-stu-id="aa00d-109">Configure network mapping</span></span>

1. <span data-ttu-id="aa00d-110">V **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.</span><span class="sxs-lookup"><span data-stu-id="aa00d-110">In **Network Mapping** > **Network mappings**, click **+Network Mapping**.</span></span>
2. <span data-ttu-id="aa00d-111">V **přidání mapování sítě** vyberte hello zdrojové a cílové servery VMM.</span><span class="sxs-lookup"><span data-stu-id="aa00d-111">In **Add network mapping** tab, select hello source and target VMM servers.</span></span> <span data-ttu-id="aa00d-112">Hello sítě virtuálních počítačů spojené s servery VMM hello jsou načteny.</span><span class="sxs-lookup"><span data-stu-id="aa00d-112">hello VM networks associated with hello VMM servers are retrieved.</span></span>
3. <span data-ttu-id="aa00d-113">V **zdrojové síti**vyberte hello sítě chcete toouse ze seznamu hello sítě virtuálních počítačů spojené s primárním serverem VMM hello.</span><span class="sxs-lookup"><span data-stu-id="aa00d-113">In **Source network**, select hello network you want toouse from hello list of VM networks associated with hello primary VMM server.</span></span>
4. <span data-ttu-id="aa00d-114">V **Cílová síť**vyberte hello sítě chcete toouse na sekundárním serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="aa00d-114">In **Target network**, select hello network you want toouse on hello secondary VMM server.</span></span> <span data-ttu-id="aa00d-115">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa00d-115">Then click **OK**.</span></span>

    ![Mapování sítě](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="aa00d-117">Když se začne mapovat síť, dojde k tomuto:</span><span class="sxs-lookup"><span data-stu-id="aa00d-117">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="aa00d-118">Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů toohello bude síť virtuálních počítačů připojených toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="aa00d-118">Any existing replica virtual machines that correspond toohello source VM network will be connected toohello target VM network.</span></span>
* <span data-ttu-id="aa00d-119">Nové virtuální počítače, které jsou připojené toohello zdrojové síti virtuálních počítačů bude po replikaci připojené toohello cíl namapované síťové.</span><span class="sxs-lookup"><span data-stu-id="aa00d-119">New virtual machines that are connected toohello source VM network will be connected toohello target mapped network after replication.</span></span>
* <span data-ttu-id="aa00d-120">Pokud upravíte existující mapování u nové sítě, virtuální počítače repliky připojí pomocí nového nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="aa00d-120">If you modify an existing mapping with a new network, replica virtual machines will be connected using hello new settings.</span></span>
* <span data-ttu-id="aa00d-121">Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello nachází stejný název jako podsíť, na které hello zdrojového virtuálního počítače a potom virtuální počítač repliky hello budou po převzetí služeb při selhání připojené toothat cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="aa00d-121">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="aa00d-122">Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.</span><span class="sxs-lookup"><span data-stu-id="aa00d-122">If there’s no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="aa00d-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa00d-123">Next steps</span></span>

<span data-ttu-id="aa00d-124">Přejděte příliš[krok 8: Nakonfigurujte zásady replikace](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="aa00d-124">Go too[Step 8: Configure a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>
