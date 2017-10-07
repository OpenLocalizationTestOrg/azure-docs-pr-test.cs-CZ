---
title: "aaaEnable replikace pro virtuální počítače VMware replikace tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooAzure tooenable replikace pro virtuální počítače VMware pomocí služby Azure Site Recovery hello"
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 490782bbbfa3dd92c626d3985c75d771df53d566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-tooazure"></a><span data-ttu-id="7586a-103">Krok 11: Povolení replikace pro virtuální počítače tooAzure VMware</span><span class="sxs-lookup"><span data-stu-id="7586a-103">Step 11: Enable replication for VMware virtual machines tooAzure</span></span>


<span data-ttu-id="7586a-104">Tento článek popisuje, jak tooenable replikace pro VMware místní virtuální počítače tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7586a-104">This article describes how tooenable replication for on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="7586a-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7586a-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="before-you-start"></a><span data-ttu-id="7586a-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7586a-106">Before you start</span></span>

- <span data-ttu-id="7586a-107">Virtuální počítače VMware musí mít hello [nainstalovat službu mobility](vmware-walkthrough-install-mobility.md).</span><span class="sxs-lookup"><span data-stu-id="7586a-107">VMware VMs must have hello [Mobility service component installed](vmware-walkthrough-install-mobility.md).</span></span> <span data-ttu-id="7586a-108">– Pokud je virtuální počítač připravený na nabízenou instalaci, hello procesový server automaticky nainstaluje služba Mobility hello při povolení replikace.</span><span class="sxs-lookup"><span data-stu-id="7586a-108">- If a VM is prepared for push installation, hello process server automatically installs hello Mobility service when you enable replication.</span></span>
- <span data-ttu-id="7586a-109">Azure uživatelský účet potřebuje konkrétní [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci tooAzure virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7586a-109">Your Azure user account needs specific [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of a VM tooAzure</span></span>
- <span data-ttu-id="7586a-110">Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až too15 minut nebo déle pro změny tootake účinek a jejich tooappear hello portálu.</span><span class="sxs-lookup"><span data-stu-id="7586a-110">When you add or modify VMs, it can take up too15 minutes or longer for changes tootake effect, and for them tooappear in hello portal.</span></span>
- <span data-ttu-id="7586a-111">Čas poslední zjištěné hello můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.</span><span class="sxs-lookup"><span data-stu-id="7586a-111">You can check hello last discovered time for VMs in **Configuration Servers** > **Last Contact At**.</span></span>
- <span data-ttu-id="7586a-112">tooadd virtuálních počítačů bez čekání na hello naplánovaného zjišťování, zvýraznění hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="7586a-112">tooadd VMs without waiting for hello scheduled discovery, highlight hello configuration server (don’t click it), and click **Refresh**.</span></span>



## <a name="exclude-disks-from-replication"></a><span data-ttu-id="7586a-113">Vyloučení disků z replikace</span><span class="sxs-lookup"><span data-stu-id="7586a-113">Exclude disks from replication</span></span>

<span data-ttu-id="7586a-114">Ve výchozím nastavení jsou všechny disky na počítači replikují.</span><span class="sxs-lookup"><span data-stu-id="7586a-114">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="7586a-115">Disky můžete z replikace vyloučit.</span><span class="sxs-lookup"><span data-stu-id="7586a-115">You can exclude disks from replication.</span></span> <span data-ttu-id="7586a-116">Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="7586a-116">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="7586a-117">Další informace</span><span class="sxs-lookup"><span data-stu-id="7586a-117">Learn more</span></span>](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a><span data-ttu-id="7586a-118">Replikace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7586a-118">Replicate VMs</span></span>

<span data-ttu-id="7586a-119">Než začnete, podívejte se na rychlé video s přehledem</span><span class="sxs-lookup"><span data-stu-id="7586a-119">Before you start, watch a quick video overview</span></span>

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. <span data-ttu-id="7586a-120">Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.</span><span class="sxs-lookup"><span data-stu-id="7586a-120">Click **Step 2: Replicate application** > **Source**.</span></span>
2. <span data-ttu-id="7586a-121">V **zdroj**vyberte hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="7586a-121">In **Source**, select hello configuration server.</span></span>
3. <span data-ttu-id="7586a-122">V **počítač typ**, vyberte **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="7586a-122">In **Machine type**, select **Virtual Machines**.</span></span>
4. <span data-ttu-id="7586a-123">V **vCenter vSphere Hypervisor**, vyberte hello vCenter server, který spravuje hostitelů vSphere hello nebo vyberte hello hostitele.</span><span class="sxs-lookup"><span data-stu-id="7586a-123">In **vCenter/vSphere Hypervisor**, select hello vCenter server that manages hello vSphere host, or select hello host.</span></span>
5. <span data-ttu-id="7586a-124">Vyberte procesový server hello.</span><span class="sxs-lookup"><span data-stu-id="7586a-124">Select hello process server.</span></span> <span data-ttu-id="7586a-125">Pokud jste nevytvořili žádné servery další proces bude hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="7586a-125">If you haven't created any additional process servers this will be hello configuration server.</span></span> <span data-ttu-id="7586a-126">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586a-126">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. <span data-ttu-id="7586a-128">V **cíl**, vyberte předplatné hello a hello skupinu prostředků, ve kterém chcete toocreate hello převzal virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7586a-128">In **Target**, select hello subscription and hello resource group in which you want toocreate hello failed over VMs.</span></span> <span data-ttu-id="7586a-129">Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management), pro hello převzal virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="7586a-129">Choose hello deployment model that you want toouse in Azure (classic or resource management), for hello failed over VMs.</span></span>


7. <span data-ttu-id="7586a-130">Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat.</span><span class="sxs-lookup"><span data-stu-id="7586a-130">Select hello Azure storage account you want toouse for replicating data.</span></span> <span data-ttu-id="7586a-131">Pokud nechcete, aby toouse účet, který jste již nastavili, můžete vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="7586a-131">If you don't want toouse an account you've already set up, you can create a new one.</span></span>

8. <span data-ttu-id="7586a-132">Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7586a-132">Select hello Azure network and subnet toowhich Azure VMs will connect, when they're created after failover.</span></span> <span data-ttu-id="7586a-133">Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu.</span><span class="sxs-lookup"><span data-stu-id="7586a-133">Select **Configure now for selected machines**, tooapply hello network setting tooall machines you select for protection.</span></span> <span data-ttu-id="7586a-134">Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač.</span><span class="sxs-lookup"><span data-stu-id="7586a-134">Select **Configure later** tooselect hello Azure network per machine.</span></span> <span data-ttu-id="7586a-135">Pokud nechcete, aby toouse existující síť, můžete vytvořit jeden.</span><span class="sxs-lookup"><span data-stu-id="7586a-135">If you don't want toouse an existing network, you can create one.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. <span data-ttu-id="7586a-137">V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="7586a-137">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="7586a-138">Můžete vybrat pouze počítače, pro které je možné povolit replikaci.</span><span class="sxs-lookup"><span data-stu-id="7586a-138">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="7586a-139">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586a-139">Then click **OK**.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. <span data-ttu-id="7586a-141">V **vlastnosti** > **konfigurovat vlastnosti**vyberte hello účet, který se použije hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="7586a-141">In **Properties** > **Configure properties**, select hello account that will be used by hello process server tooautomatically install hello Mobility service on hello machine.</span></span>
11. <span data-ttu-id="7586a-142">Ve výchozím nastavení jsou všechny disky replikovat.</span><span class="sxs-lookup"><span data-stu-id="7586a-142">By default all disks are replicated.</span></span> <span data-ttu-id="7586a-143">Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="7586a-143">Click **All Disks** and clear any disks you don't want tooreplicate.</span></span> <span data-ttu-id="7586a-144">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586a-144">Then click **OK**.</span></span> <span data-ttu-id="7586a-145">Později můžete nastavit další vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7586a-145">You can set additional VM properties later.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. <span data-ttu-id="7586a-147">V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána.</span><span class="sxs-lookup"><span data-stu-id="7586a-147">In **Replication settings** > **Configure replication settings**, verify that hello correct replication policy is selected.</span></span> <span data-ttu-id="7586a-148">Pokud změníte zásady, změny budou použité tooreplicating počítač a toonew počítače.</span><span class="sxs-lookup"><span data-stu-id="7586a-148">If you modify a policy, changes will be applied tooreplicating machine, and toonew machines.</span></span>
12. <span data-ttu-id="7586a-149">Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="7586a-149">Enable **Multi-VM consistency** if you want toogather machines into a replication group, and specify a name for hello group.</span></span> <span data-ttu-id="7586a-150">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7586a-150">Then click **OK**.</span></span> <span data-ttu-id="7586a-151">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="7586a-151">Note that:</span></span>

    * <span data-ttu-id="7586a-152">Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7586a-152">Machines in replication groups replicate together, and have shared crash-consistent and app-consistent recovery points when they fail over.</span></span>
    * <span data-ttu-id="7586a-153">Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="7586a-153">We recommend that you gather VMs and physical servers together so that they mirror your workloads.</span></span> <span data-ttu-id="7586a-154">Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží hello stejné úlohy a potřebujete konzistence.</span><span class="sxs-lookup"><span data-stu-id="7586a-154">Enabling multi-VM consistency can impact workload performance, and should only be used if machines are running hello same workload and you need consistency.</span></span>

    ![Povolení replikace](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. <span data-ttu-id="7586a-156">Klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="7586a-156">Click **Enable Replication**.</span></span> <span data-ttu-id="7586a-157">Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="7586a-157">You can track progress of hello **Enable Protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span> <span data-ttu-id="7586a-158">Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7586a-158">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7586a-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7586a-159">Next steps</span></span>

<span data-ttu-id="7586a-160">Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="7586a-160">Go too[Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
