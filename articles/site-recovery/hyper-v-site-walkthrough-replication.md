---
title: "Nastavení zásad replikace pro replikaci virtuálních počítačů technologie Hyper-V (bez System Center VMM) do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které je třeba nastavit zásady replikace při replikaci virtuálních počítačů Hyper-V do úložiště Azure"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: ca5bec5cf1152e6259b9fe7a869edd2d62b88e1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a><span data-ttu-id="9def1-103">Krok 9: Nastavení zásad replikace pro virtuální počítač Hyper-V replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="9def1-103">Step 9: Set up a replication policy for Hyper-V VM replication to Azure</span></span>

<span data-ttu-id="9def1-104">Tento článek popisuje, jak nastavit zásadu replikace při replikaci virtuálních počítačů Hyper-V do Azure (bez System Center VMM) pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9def1-104">This article describes how to set up a replication policy when you're replicating Hyper-V VMs to Azure (without System Center VMM) using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="9def1-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9def1-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="about-snapshots"></a><span data-ttu-id="9def1-106">O snímky</span><span class="sxs-lookup"><span data-stu-id="9def1-106">About snapshots</span></span>

<span data-ttu-id="9def1-107">Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="9def1-107">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of the entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of the application data inside the virtual machine.</span></span>
    - <span data-ttu-id="9def1-108">Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="9def1-108">Application-consistent snapshots use Volume Shadow Copy Service (VSS) to ensure that applications are in a consistent state when the snapshot is taken.</span></span>
    - <span data-ttu-id="9def1-109">Pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv výkon aplikací běžících na zdrojových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="9def1-109">If you enable application-consistent snapshots, it will affect the performance of applications running on source virtual machines.</span></span> <span data-ttu-id="9def1-110">Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="9def1-110">Ensure that the value you set is less than the number of additional recovery points you configure.</span></span>

## <a name="set-up-a-replication-policy"></a><span data-ttu-id="9def1-111">Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="9def1-111">Set up a replication policy</span></span>

1. <span data-ttu-id="9def1-112">Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.</span><span class="sxs-lookup"><span data-stu-id="9def1-112">To create a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Síť](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="9def1-114">V části **Vytvořit a přidružit zásady** zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="9def1-114">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="9def1-115">V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).</span><span class="sxs-lookup"><span data-stu-id="9def1-115">In **Copy frequency**, specify how often you want to replicate delta data after the initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    > <span data-ttu-id="9def1-116">Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="9def1-116">A 30 second frequency isn't supported when replicating to premium storage.</span></span> <span data-ttu-id="9def1-117">Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje.</span><span class="sxs-lookup"><span data-stu-id="9def1-117">The limitation is determined by the number of snapshots per blob (100) supported by premium storage.</span></span> <span data-ttu-id="9def1-118">[Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span><span class="sxs-lookup"><span data-stu-id="9def1-118">[Learn more](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span></span>

4. <span data-ttu-id="9def1-119">V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho je okno uchování pro každý bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="9def1-119">In **Recovery point retention**, specify in hours how long the retention window is for each recovery point.</span></span> <span data-ttu-id="9def1-120">Virtuální počítače můžete obnovit do libovolného bodu v rámci časového období.</span><span class="sxs-lookup"><span data-stu-id="9def1-120">VMs can be recovered to any point within a window.</span></span>
5. <span data-ttu-id="9def1-121">V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="9def1-121">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots are created.</span></span>
6. <span data-ttu-id="9def1-122">V **čas spuštění počáteční replikace**, určete, kdy má spustit počáteční replikaci.</span><span class="sxs-lookup"><span data-stu-id="9def1-122">In **Initial replication start time**, specify when to start the initial replication.</span></span> <span data-ttu-id="9def1-123">Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky.</span><span class="sxs-lookup"><span data-stu-id="9def1-123">The replication occurs over your internet bandwidth so you might want to schedule it outside your busy hours.</span></span> <span data-ttu-id="9def1-124">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9def1-124">Then click **OK**.</span></span>

    ![Zásady replikace](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

<span data-ttu-id="9def1-126">Když vytvoříte novou zásadu, je automaticky spojen s web Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="9def1-126">When you create a new policy, it's automatically associated with the Hyper-V site.</span></span> <span data-ttu-id="9def1-127">Je možné přidružit web Hyper-V (a virtuální počítače v něm) víc zásad replikace v **replikace** > název zásady > **přidružit web Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="9def1-127">You can associate a Hyper-V site (and the VMs in it) with multiple replication policies in **Replication** > policy-name > **Associate Hyper-V Site**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="9def1-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9def1-128">Next steps</span></span>

<span data-ttu-id="9def1-129">Přejděte na [krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="9def1-129">Go to [Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>
