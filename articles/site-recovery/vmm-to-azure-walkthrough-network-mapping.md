---
title: "aaaConfigure mapování sítě pro replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak mapování sítě tooconfigure při replikaci virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 081a9fdb0ffa4114099e9bcb9c1b1e43ad26ecbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="38562-103">Krok 9: Konfigurace mapování sítě pro tooAzure replikace (s VMM) technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="38562-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="38562-104">Po nastavení hello [zdrojové a cílové nastavení replikace](vmm-to-azure-walkthrough-source-target.md), použijte tento článek tooconfigure sítě mapování toomap mezi místními sítěmi virtuálních počítačů nástroje VMM a sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="38562-104">After you set up hello [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article tooconfigure network mapping toomap between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="38562-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="38562-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="38562-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="38562-106">Before you start</span></span>

- <span data-ttu-id="38562-107">Další informace o [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="38562-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="38562-108">[Příprava mapování sítě VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="38562-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="38562-109">Ověřte, že jsou virtuální počítače na serveru VMM hello připojené tooa síť virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="38562-109">Verify that virtual machines on hello VMM server are connected tooa VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="38562-110">Více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.</span><span class="sxs-lookup"><span data-stu-id="38562-110">Multiple VM networks can be mapped tooa single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="38562-111">Konfigurace mapování</span><span class="sxs-lookup"><span data-stu-id="38562-111">Configure mapping</span></span>

<span data-ttu-id="38562-112">Nakonfigurujte mapování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="38562-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="38562-113">V **infrastruktura Site Recovery** > **mapování sítí** > **mapování sítě**, klikněte na tlačítko hello **+ mapování sítě**  ikonu.</span><span class="sxs-lookup"><span data-stu-id="38562-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click hello **+Network Mapping** icon.</span></span>

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="38562-115">V **přidání mapování sítě**, vyberte hello zdrojový server VMM, a **Azure** jako cíl hello.</span><span class="sxs-lookup"><span data-stu-id="38562-115">In **Add network mapping**, select hello source VMM server, and **Azure** as hello target.</span></span>
3. <span data-ttu-id="38562-116">Ověřte předplatné hello a hello model nasazení po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="38562-116">Verify hello subscription and hello deployment model after failover.</span></span>
4. <span data-ttu-id="38562-117">V **zdrojové síti**vyberte hello zdrojovou místní síť virtuálních počítačů má toomap hello seznamu přidružené k serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="38562-117">In **Source network**, select hello source on-premises VM network you want toomap from hello list associated with hello VMM server.</span></span>
5. <span data-ttu-id="38562-118">V **Cílová síť**, vyberte hello síť Azure, ve které repliky virtuálních počítačů Azure budou umístěné při vytváří.</span><span class="sxs-lookup"><span data-stu-id="38562-118">In **Target network**, select hello Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="38562-119">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="38562-119">Then click **OK**.</span></span>

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="38562-121">Když se začne mapovat síť, dojde k tomuto:</span><span class="sxs-lookup"><span data-stu-id="38562-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="38562-122">Existující virtuální počítače ve zdrojové síti virtuálních počítačů hello jsou připojené toohello cílové síti, když začne mapování.</span><span class="sxs-lookup"><span data-stu-id="38562-122">Existing VMs on hello source VM network are connected toohello target network when mapping begins.</span></span> <span data-ttu-id="38562-123">Nová síť virtuálních počítačů připojených toohello zdrojové virtuální počítače jsou připojené toohello namapované síti Azure, když dojde k replikaci.</span><span class="sxs-lookup"><span data-stu-id="38562-123">New VMs connected toohello source VM network are connected toohello mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="38562-124">Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojují pomocí hello nové nastavení.</span><span class="sxs-lookup"><span data-stu-id="38562-124">If you modify an existing network mapping, replica virtual machines connect using hello new settings.</span></span>
* <span data-ttu-id="38562-125">Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač nachází, pak virtuální počítač repliky hello připojí po převzetí služeb při selhání toothat cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="38562-125">If hello target network has multiple subnets, and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine connects toothat target subnet after failover.</span></span>
* <span data-ttu-id="38562-126">Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač připojí toohello první podsíť v síti hello.</span><span class="sxs-lookup"><span data-stu-id="38562-126">If there’s no target subnet with a matching name, hello virtual machine connects toohello first subnet in hello network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="38562-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38562-127">Next steps</span></span>

<span data-ttu-id="38562-128">Přejděte příliš[krok 10: vytvoření zásady replikace](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="38562-128">Go too[Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
