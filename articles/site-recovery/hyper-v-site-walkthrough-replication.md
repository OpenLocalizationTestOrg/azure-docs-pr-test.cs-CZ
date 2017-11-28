---
title: "aaaSet se zásada replikace pro virtuální počítač Hyper-V tooAzure replikace (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset si zásadu replikace při replikaci virtuálních počítačů Hyper-V tooAzure úložiště"
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
ms.openlocfilehash: 4bd7161f4a0f015da0ecf595fbc6861ede5d68b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-tooazure"></a><span data-ttu-id="4829f-103">Krok 9: Nastavení zásad replikace pro tooAzure replikace virtuálního počítače technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="4829f-103">Step 9: Set up a replication policy for Hyper-V VM replication tooAzure</span></span>

<span data-ttu-id="4829f-104">Tento článek popisuje, jak tooset si zásadu replikace při replikaci tooAzure virtuálních počítačů Hyper-V (bez System Center VMM) pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4829f-104">This article describes how tooset up a replication policy when you're replicating Hyper-V VMs tooAzure (without System Center VMM) using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="4829f-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4829f-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="about-snapshots"></a><span data-ttu-id="4829f-106">O snímky</span><span class="sxs-lookup"><span data-stu-id="4829f-106">About snapshots</span></span>

<span data-ttu-id="4829f-107">Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4829f-107">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span>
    - <span data-ttu-id="4829f-108">Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="4829f-108">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span>
    - <span data-ttu-id="4829f-109">Pokud povolíte snímky konzistentní s aplikacemi, ovlivní hello výkon aplikací běžících na zdrojových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="4829f-109">If you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="4829f-110">Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="4829f-110">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>

## <a name="set-up-a-replication-policy"></a><span data-ttu-id="4829f-111">Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="4829f-111">Set up a replication policy</span></span>

1. <span data-ttu-id="4829f-112">Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.</span><span class="sxs-lookup"><span data-stu-id="4829f-112">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Síť](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="4829f-114">V části **Vytvořit a přidružit zásady** zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="4829f-114">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="4829f-115">V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).</span><span class="sxs-lookup"><span data-stu-id="4829f-115">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    > <span data-ttu-id="4829f-116">30 druhý frekvence nepodporuje při replikaci toopremium úložiště.</span><span class="sxs-lookup"><span data-stu-id="4829f-116">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="4829f-117">omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="4829f-117">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> <span data-ttu-id="4829f-118">[Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span><span class="sxs-lookup"><span data-stu-id="4829f-118">[Learn more](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).</span></span>

4. <span data-ttu-id="4829f-119">V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho hello intervalem pro uchovávání dat je pro každý bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="4829f-119">In **Recovery point retention**, specify in hours how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="4829f-120">Virtuální počítače mohou být obnovena tooany bodu v rámci časového období.</span><span class="sxs-lookup"><span data-stu-id="4829f-120">VMs can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="4829f-121">V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="4829f-121">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots are created.</span></span>
6. <span data-ttu-id="4829f-122">V **čas spuštění počáteční replikace**, zadejte, kdy toostart hello počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="4829f-122">In **Initial replication start time**, specify when toostart hello initial replication.</span></span> <span data-ttu-id="4829f-123">Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky.</span><span class="sxs-lookup"><span data-stu-id="4829f-123">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span> <span data-ttu-id="4829f-124">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4829f-124">Then click **OK**.</span></span>

    ![Zásady replikace](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

<span data-ttu-id="4829f-126">Když vytvoříte novou zásadu, je automaticky přidružená hello web Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="4829f-126">When you create a new policy, it's automatically associated with hello Hyper-V site.</span></span> <span data-ttu-id="4829f-127">Budete moct přidružit k víc zásad replikace v lokalitě Hyper-V (a hello virtuálních počítačů v ní) **replikace** > název zásady > **přidružit web Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="4829f-127">You can associate a Hyper-V site (and hello VMs in it) with multiple replication policies in **Replication** > policy-name > **Associate Hyper-V Site**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="4829f-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4829f-128">Next steps</span></span>

<span data-ttu-id="4829f-129">Přejděte příliš[krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4829f-129">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>
