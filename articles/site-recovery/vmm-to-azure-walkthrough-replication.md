---
title: "Nastavení zásad replikace pro virtuální počítač Hyper-V (s nástrojem VMM) replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak nastavit zásadu replikace pro virtuální počítač Hyper-V (s nástrojem VMM) replikaci do Azure s Azure Site Recovery"
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
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a><span data-ttu-id="c6779-103">Krok 10: Nastavení zásad replikace pro replikaci virtuálních počítačů technologie Hyper-V (s nástrojem VMM) do Azure.</span><span class="sxs-lookup"><span data-stu-id="c6779-103">Step 10: Set up a replication policy for Hyper-V VM replication (with VMM) to Azure</span></span>


<span data-ttu-id="c6779-104">Po nastavení [mapování sítě](vmm-to-azure-walkthrough-network-mapping.md), použijte tento článek nakonfigurovat zásadu replikace T\to replikovat místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6779-104">After setting up [network mapping](vmm-to-azure-walkthrough-network-mapping.md), use this article to configure a replication policy T\to replicate on-premises Hyper-V virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="c6779-105">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c6779-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



## <a name="create-a-policy"></a><span data-ttu-id="c6779-106">Vytvořit zásadu</span><span class="sxs-lookup"><span data-stu-id="c6779-106">Create a policy</span></span>

1. <span data-ttu-id="c6779-107">Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.</span><span class="sxs-lookup"><span data-stu-id="c6779-107">To create a new replication policy, click **Prepare infrastructure** > **Replication Settings** > **+Create and associate**.</span></span>

    ![Síť](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. <span data-ttu-id="c6779-109">V části **Vytvořit a přidružit zásady** zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="c6779-109">In **Create and associate policy**, specify a policy name.</span></span>
3. <span data-ttu-id="c6779-110">V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).</span><span class="sxs-lookup"><span data-stu-id="c6779-110">In **Copy frequency**, specify how often you want to replicate delta data after the initial replication (every 30 seconds, 5 or 15 minutes).</span></span>

    > [!NOTE]
    >  <span data-ttu-id="c6779-111">Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium.</span><span class="sxs-lookup"><span data-stu-id="c6779-111">A 30 second frequency isn't supported when replicating to premium storage.</span></span> <span data-ttu-id="c6779-112">Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje.</span><span class="sxs-lookup"><span data-stu-id="c6779-112">The limitation is determined by the number of snapshots per blob (100) supported by premium storage.</span></span> [<span data-ttu-id="c6779-113">Další informace</span><span class="sxs-lookup"><span data-stu-id="c6779-113">Learn more</span></span>](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. <span data-ttu-id="c6779-114">V části **Uchování bodu obnovení** zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení.</span><span class="sxs-lookup"><span data-stu-id="c6779-114">In **Recovery point retention**, specify in hours how long the retention window will be for each recovery point.</span></span> <span data-ttu-id="c6779-115">Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu.</span><span class="sxs-lookup"><span data-stu-id="c6779-115">Protected machines can be recovered to any point within a window.</span></span>
5. <span data-ttu-id="c6779-116">V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="c6779-116">In **App-consistent snapshot frequency**, specify how frequently (1-12 hours) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="c6779-117">Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku.</span><span class="sxs-lookup"><span data-stu-id="c6779-117">Hyper-V uses two types of snapshots — a standard snapshot that provides an incremental snapshot of the entire virtual machine, and an application-consistent snapshot that takes a point-in-time snapshot of the application data inside the virtual machine.</span></span> <span data-ttu-id="c6779-118">Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="c6779-118">Application-consistent snapshots use Volume Shadow Copy Service (VSS) to ensure that applications are in a consistent state when the snapshot is taken.</span></span> <span data-ttu-id="c6779-119">Pokud povolíte snímky konzistentní vzhledem k aplikacím, bude to mít vliv na výkon aplikací běžících na zdrojových virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="c6779-119">Note that if you enable application-consistent snapshots, it will affect the performance of applications running on source virtual machines.</span></span> <span data-ttu-id="c6779-120">Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.</span><span class="sxs-lookup"><span data-stu-id="c6779-120">Ensure that the value you set is less than the number of additional recovery points you configure.</span></span>
6. <span data-ttu-id="c6779-121">V nastavení **Čas spuštění počáteční replikace** určete, kdy se má spustit počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="c6779-121">In **Initial replication start time**, indicate when to start the initial replication.</span></span> <span data-ttu-id="c6779-122">Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky.</span><span class="sxs-lookup"><span data-stu-id="c6779-122">The replication occurs over your internet bandwidth so you might want to schedule it outside your busy hours.</span></span>
7. <span data-ttu-id="c6779-123">V nastavení **Šifrovat úložiště dat ve službě Azure** určete, jestli chcete v úložišti Azure šifrovat neaktivní data.</span><span class="sxs-lookup"><span data-stu-id="c6779-123">In **Encrypt data stored on Azure**, specify whether to encrypt at rest data in Azure storage.</span></span> <span data-ttu-id="c6779-124">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6779-124">Then click **OK**.</span></span>

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="c6779-126">Když vytvoříte novou zásadu, automaticky se přidruží ke cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="c6779-126">When you create a new policy it's automatically associated with the VMM cloud.</span></span> <span data-ttu-id="c6779-127">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6779-127">Click **OK**.</span></span> <span data-ttu-id="c6779-128">K této zásadě replikace můžete přidružit další cloudy VMM (a virtuální počítače v nich) v části **Replikace** > název_zásady > **Přidružit cloud VMM**.</span><span class="sxs-lookup"><span data-stu-id="c6779-128">You can associate additional VMM clouds (and the VMs in them) with this replication policy in **Replication** > policy name > **Associate VMM Cloud**.</span></span>

    ![Zásady replikace](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a><span data-ttu-id="c6779-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6779-130">Next steps</span></span>

<span data-ttu-id="c6779-131">Přejděte na [krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="c6779-131">Go to [Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>
