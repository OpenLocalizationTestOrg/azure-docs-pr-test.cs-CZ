---
title: "aaaEnable replikace pro virtuální počítače Hyper-V replikuje tooAzure (bez System Center VMM) s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooAzure tooenable replikace pro virtuální počítače Hyper-V pomocí služby Azure Site Recovery hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1589cb7aa1fe954e075cb7bf1f4a4ec199ed3ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-tooazure"></a><span data-ttu-id="7e4ae-103">Krok 10: Povolení replikace pro virtuální počítače Hyper-V replikuje tooAzure</span><span class="sxs-lookup"><span data-stu-id="7e4ae-103">Step 10: Enable replication for Hyper-V VMs replicating tooAzure</span></span>


<span data-ttu-id="7e4ae-104">Tento článek popisuje, jak tooenable replikace pro místní virtuální počítače Hyper-V (nejsou spravována službou System Center VMM) tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-104">This article describes how tooenable replication for on-premises Hyper-V virtual machines (not managed by System Center VMM) tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="7e4ae-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7e4ae-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="before-you-start"></a><span data-ttu-id="7e4ae-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7e4ae-106">Before you start</span></span>

<span data-ttu-id="7e4ae-107">Než začnete, zajistěte, aby účtu uživatele Azure hello požadované [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-107">Before you start, ensure that your Azure user account has hello required [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of a new virtual machine tooAzure.</span></span>

## <a name="exclude-disks-from-replication"></a><span data-ttu-id="7e4ae-108">Vyloučení disků z replikace</span><span class="sxs-lookup"><span data-stu-id="7e4ae-108">Exclude disks from replication</span></span>

<span data-ttu-id="7e4ae-109">Ve výchozím nastavení jsou všechny disky na počítači replikují.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-109">By default all disks on a machine are replicated.</span></span> <span data-ttu-id="7e4ae-110">Disky můžete z replikace vyloučit.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-110">You can exclude disks from replication.</span></span> <span data-ttu-id="7e4ae-111">Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).</span><span class="sxs-lookup"><span data-stu-id="7e4ae-111">For example you might not want tooreplicate disks with temporary data, or data that's refreshed each time a machine or application restarts (for example pagefile.sys or SQL Server tempdb).</span></span> [<span data-ttu-id="7e4ae-112">Další informace</span><span class="sxs-lookup"><span data-stu-id="7e4ae-112">Learn more</span></span>](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a><span data-ttu-id="7e4ae-113">Replikace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="7e4ae-113">Replicate VMs</span></span>

<span data-ttu-id="7e4ae-114">Povolení replikace pro virtuální počítače takto:</span><span class="sxs-lookup"><span data-stu-id="7e4ae-114">Enable replication for VMs as follows:</span></span>          

1. <span data-ttu-id="7e4ae-115">Klikněte na tlačítko **replikujte aplikaci** > **zdroj**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-115">Click **Replicate application** > **Source**.</span></span> <span data-ttu-id="7e4ae-116">Poté, co jste nastavili replikaci hello poprvé, můžete kliknout na **+ replikovat** tooenable replikaci pro další počítače.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-116">After you've set up replication for hello first time, you can click **+Replicate** tooenable replication for additional machines.</span></span>

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. <span data-ttu-id="7e4ae-118">V **zdroj**, vyberte lokality hello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-118">In **Source**, select hello Hyper-V site.</span></span> <span data-ttu-id="7e4ae-119">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-119">Then click **OK**.</span></span>
3. <span data-ttu-id="7e4ae-120">V **cíl**, vyberte předplatné trezoru hello a hello převzetí služeb při selhání modelu chcete toouse v Azure (klasický nebo prostředek management), po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-120">In **Target**, select hello vault subscription, and hello failover model you want toouse in Azure (classic or resource management) after failover.</span></span>
4. <span data-ttu-id="7e4ae-121">Vyberte účet úložiště hello, že chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-121">Select hello storage account you want toouse.</span></span> <span data-ttu-id="7e4ae-122">Pokud nemáte účet, který chcete toouse, můžete [vytvořit](#set-up-an-azure-storage-account).</span><span class="sxs-lookup"><span data-stu-id="7e4ae-122">If you don't have an account you want toouse, you can [create one](#set-up-an-azure-storage-account).</span></span> <span data-ttu-id="7e4ae-123">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-123">Then click **OK**.</span></span>
5. <span data-ttu-id="7e4ae-124">Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když vytváří převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-124">Select hello Azure network and subnet toowhich Azure VMs will connect when they're created failover.</span></span>

    - <span data-ttu-id="7e4ae-125">Vyberte tooapply hello nastavení tooall počítače sítě můžete povolit pro replikaci, **nakonfigurovat pro vybrané počítače**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-125">tooapply hello network settings tooall machines you enable for replication, select **Configure now for selected machines**.</span></span>
    - <span data-ttu-id="7e4ae-126">Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-126">Select **Configure later** tooselect hello Azure network per machine.</span></span>
    - <span data-ttu-id="7e4ae-127">Pokud nemáte síť chcete toouse, můžete [vytvořit](#set-up-an-azure-network).</span><span class="sxs-lookup"><span data-stu-id="7e4ae-127">If you don't have a network you want toouse, you can [create one](#set-up-an-azure-network).</span></span> <span data-ttu-id="7e4ae-128">V odpovídajícím případě vyberte podsíť.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-128">Select a subnet if applicable.</span></span> <span data-ttu-id="7e4ae-129">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-129">Then click **OK**.</span></span>

   ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. <span data-ttu-id="7e4ae-131">V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-131">In **Virtual Machines** > **Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="7e4ae-132">Můžete vybrat pouze počítače, pro které je možné povolit replikaci.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="7e4ae-133">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-133">Then click **OK**.</span></span>

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. <span data-ttu-id="7e4ae-135">V **vlastnosti** > **konfigurovat vlastnosti**, vyberte hello operační systém pro virtuální počítače hello vybrané a hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-135">In **Properties** > **Configure properties**, select hello operating system for hello selected VMs, and hello OS disk.</span></span>
8. <span data-ttu-id="7e4ae-136">Ověřte, že hello název virtuálního počítače Azure (název cílové) v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="7e4ae-136">Verify that hello Azure VM name (target name) complies with [Azure virtual machine requirements](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>
9. <span data-ttu-id="7e4ae-137">Ve výchozím nastavení jsou vybrané všechny disky hello hello virtuálních počítačů pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-137">By default all hello disks of hello VM are selected for replication.</span></span> <span data-ttu-id="7e4ae-138">Vymazat disky tooexclude je.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-138">Clear disks tooexclude them.</span></span>
10. <span data-ttu-id="7e4ae-139">Klikněte na tlačítko **OK** toosave změny.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-139">Click **OK** toosave changes.</span></span> <span data-ttu-id="7e4ae-140">Později můžete nastavit další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-140">You can set additional properties later.</span></span>

    ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. <span data-ttu-id="7e4ae-142">V **nastavení replikace** > **nakonfigurujete nastavení replikace**, vyberte zásadu replikace hello chcete tooapply hello chráněné virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-142">In **Replication settings** > **Configure replication settings**, select hello replication policy you want tooapply for hello protected VMs.</span></span> <span data-ttu-id="7e4ae-143">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-143">Then click **OK**.</span></span> <span data-ttu-id="7e4ae-144">Můžete změnit zásady replikace hello v **zásady replikace** > název zásady > **upravit nastavení**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-144">You can modify hello replication policy in **Replication policies** > policy-name > **Edit Settings**.</span></span> <span data-ttu-id="7e4ae-145">Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-145">Changes you apply will be used for machines that are already replicating, and new machines.</span></span>


   ![Povolení replikace](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

<span data-ttu-id="7e4ae-147">Můžete sledovat průběh hello **povolení ochrany** úlohy v **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-147">You can track progress of hello **Enable Protection** job in **Jobs** > **Site Recovery jobs**.</span></span> <span data-ttu-id="7e4ae-148">Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="7e4ae-148">After hello **Finalize Protection** job runs hello machine is ready for failover.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7e4ae-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e4ae-149">Next steps</span></span>


<span data-ttu-id="7e4ae-150">Přejděte příliš[krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="7e4ae-150">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
