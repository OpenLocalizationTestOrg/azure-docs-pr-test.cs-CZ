---
title: "aaaSet se zásada replikace pro virtuální počítač Hyper-V (s nástrojem VMM) tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooset se zásada replikace pro virtuální počítač Hyper-V (s nástrojem VMM) tooAzure replikace s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: e1579fde559ca34eca19a01e740fec28a0df2f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-tooazure"></a><span data-ttu-id="0ee63-103">Krok 10: Nastavení zásad replikace pro virtuální počítač Hyper-V tooAzure replikace (s VMM)</span><span class="sxs-lookup"><span data-stu-id="0ee63-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) tooAzure</span></span>


<span data-ttu-id="0ee63-104">Po nastavení [mapování sítě](vmm-to-azure-walkthrough-network-mapping.md), použijte tento článek tooconfigure zásadu replikace T\tooreplicate místní virtuální počítače Hyper-V spravované v cloudech tooAzure System Center Virtual Machine Manager (VMM), pomocí hello [ Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0ee63-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article tooconfigure a replication policy T\tooreplicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="0ee63-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0ee63-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="0ee63-106">Vytvořit zásadu</span><span class="sxs-lookup"><span data-stu-id="0ee63-106">Create a policy</span></span>

1. <span data-ttu-id="0ee63-107">Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.</span><span class="sxs-lookup"><span data-stu-id="0ee63-107">toocreate a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Síť](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="0ee63-109">V části **Vytvořit a přidružit zásady** zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="0ee63-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="0ee63-110">V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).</span><span class="sxs-lookup"><span data-stu-id="0ee63-110">In **Copy frequency**, specify how often you want tooreplicate delta data after hello initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="0ee63-111">30 druhý frekvence nepodporuje při replikaci toopremium úložiště.</span><span class="sxs-lookup"><span data-stu-id="0ee63-111">A 30 second frequency isn't supported when replicating toopremium storage.</span></span> <span data-ttu-id="0ee63-112">omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="0ee63-112">hello limitation is determined by hello number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="0ee63-113">Další informace</span><span class="sxs-lookup"><span data-stu-id="0ee63-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="0ee63-114">V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho bude hello intervalem pro uchovávání dat se pro každý bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="0ee63-114">In **Recovery point retention**, specify in hours how long hello retention window will be for each recovery point.</span></span> <span data-ttu-id="0ee63-115">Chráněné počítače může být obnovena tooany bodu v rámci časového období.</span><span class="sxs-lookup"><span data-stu-id="0ee63-115">Protected machines can be recovered tooany point within a window.</span></span>
5. <span data-ttu-id="0ee63-116">V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="0ee63-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="0ee63-117">Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="0ee63-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of hello entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of hello application data inside hello virtual machine.</span></span> <span data-ttu-id="0ee63-118">Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="0ee63-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) tooensure that applications are in a consistent state when hello snapshot is taken.</span></span> <span data-ttu-id="0ee63-119">Všimněte si, že pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv hello výkon aplikací běžících na zdrojových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="0ee63-119">Note that if you enable application-consistent snapshots, it will affect hello performance of applications running on source virtual machines.</span></span> <span data-ttu-id="0ee63-120">Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="0ee63-120">Ensure that hello value you set is less than hello number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="0ee63-121">V **čas spuštění počáteční replikace**, indikovat, kdy toostart hello počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="0ee63-121">In **Initial replication start time**, indicate when toostart hello initial replication.</span></span> <span data-ttu-id="0ee63-122">Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky.</span><span class="sxs-lookup"><span data-stu-id="0ee63-122">hello replication occurs over your internet bandwidth so you might want tooschedule it outside your busy hours.</span></span>
7. <span data-ttu-id="0ee63-123">V **šifrovat data uložená v Azure**, určit, zda tooencrypt neaktivní data v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="0ee63-123">In **Encrypt data stored on Azure**, specify whether tooencrypt at rest data in Azure storage.</span></span> <span data-ttu-id="0ee63-124">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ee63-124">Then click **OK**.</span></span>

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="0ee63-126">Když vytvoříte novou zásadu je přidružená automaticky hello cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="0ee63-126">When you create a new policy it's automatically associated with hello VMM cloud.</span></span> <span data-ttu-id="0ee63-127">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0ee63-127">Click **OK**.</span></span> <span data-ttu-id="0ee63-128">K této zásadě replikace můžete přidružit další cloudy VMM (a hello virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.</span><span class="sxs-lookup"><span data-stu-id="0ee63-128">You can associate additional VMM clouds (and hello VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="0ee63-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0ee63-130">Next steps</span></span>

<span data-ttu-id="0ee63-131">Přejděte příliš[krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="0ee63-131">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
