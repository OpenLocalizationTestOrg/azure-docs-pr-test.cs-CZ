---
title: "aaaEnable replikace pro virtuální počítače Azure tooanother oblast Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooenable replikace tooanother oblast Azure pro virtuální počítače Azure pomocí služby Azure Site Recovery hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a><span data-ttu-id="3d435-103">Krok 5: Povolení replikace pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="3d435-103">Step 5: Enable replication for Azure VMs</span></span>


<span data-ttu-id="3d435-104">Po nastavení [trezor služeb zotavení](azure-to-azure-walkthrough-vault.md), použijte tento článek tooenable replikace virtuálních počítačů (VM), tooanother oblast Azure, s [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3d435-104">After setting up a [Recovery Services vault](azure-to-azure-walkthrough-vault.md), use this article tooenable replication of virtual machines (VMs), tooanother Azure region, with [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="3d435-105">tooenable replikace, můžete nastavit zdroje a cíle, ověřte hello zásadu replikace a vyberte virtuální počítače chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="3d435-105">tooenable replication, you set up source and target settings, verify hello replication policy, and select VMs you want tooreplicate.</span></span>

- <span data-ttu-id="3d435-106">Po dokončení hello článek, virtuální počítače Azure musí být replikace toohello sekundární oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="3d435-106">When you finish hello article, your Azure VMs should be replicating toohello secondary Azure region.</span></span>
- <span data-ttu-id="3d435-107">Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="3d435-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="3d435-108">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="3d435-108">Azure VM replication is currently in preview.</span></span>


## <a name="select-hello-source"></a><span data-ttu-id="3d435-109">Vyberte zdroj hello</span><span class="sxs-lookup"><span data-stu-id="3d435-109">Select hello source</span></span> 

1. <span data-ttu-id="3d435-110">V trezory služeb zotavení, klikněte na název trezoru hello > **+ replikovat**.</span><span class="sxs-lookup"><span data-stu-id="3d435-110">In Recovery Services vaults, click hello vault name > **+Replicate**.</span></span>
2. <span data-ttu-id="3d435-111">V **zdroj**, vyberte **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="3d435-111">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="3d435-112">V **umístění zdroje**, vyberte zdroj hello oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="3d435-112">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="3d435-113">Vyberte hello **modelu nasazení virtuálního počítače Azure** pro virtuální počítače: **Resource Manager** nebo **Classic**.</span><span class="sxs-lookup"><span data-stu-id="3d435-113">Select hello **Azure virtual machine deployment model** for VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="3d435-114">Vyberte hello **zdrojové skupiny prostředků** pro správce prostředků virtuálních počítačů, nebo **Cloudová služba** pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3d435-114">Select hello **Source resource group** for Resource Manager VMs, or **cloud service** for classic VMs.</span></span>
5. <span data-ttu-id="3d435-115">Klikněte na tlačítko **OK** toosave hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="3d435-115">Click **OK** toosave hello settings.</span></span>

    ![Nakonfigurujte zdroj hello](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a><span data-ttu-id="3d435-117">Vyberte virtuální počítače hello</span><span class="sxs-lookup"><span data-stu-id="3d435-117">Select hello VMs</span></span>

<span data-ttu-id="3d435-118">Site Recovery načte seznam hello, které virtuální počítače spojené s hello předplatného a skupiny nebo cloudové služby prostředků.</span><span class="sxs-lookup"><span data-stu-id="3d435-118">Site Recovery retrieves a list of hello VMs associated with hello subscription and resource group/cloud service.</span></span>

1. <span data-ttu-id="3d435-119">V **virtuální počítače**, vyberte virtuální počítače hello chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="3d435-119">In **Virtual Machines**, select hello VMs you want tooreplicate.</span></span>
2. <span data-ttu-id="3d435-120">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d435-120">Click **OK**.</span></span>

    ![Vyberte virtuální počítače](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a><span data-ttu-id="3d435-122">Konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="3d435-122">Configure settings</span></span>

<span data-ttu-id="3d435-123">Site Recovery poskytuje výchozí nastavení pro cílová oblast hello (podle nastavení oblasti zdroje hello) a zásady replikace hello:</span><span class="sxs-lookup"><span data-stu-id="3d435-123">Site Recovery provisions default settings for hello target region (based on hello source region settings), and hello replication policy:</span></span>

   - <span data-ttu-id="3d435-124">**Cílové umístění**: cílová oblast hello chcete toouse pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="3d435-124">**Target location**: hello target region you want toouse for disaster recovery.</span></span> <span data-ttu-id="3d435-125">Doporučujeme vám, že hello cílové umístění odpovídá hello umístění hello trezoru Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3d435-125">We recommend that hello target location matches hello location of hello Site Recovery vault.</span></span>
   - <span data-ttu-id="3d435-126">**Cílová skupina prostředků**: toowhich skupiny prostředků virtuálních počítačů Azure ve hello cílová oblast bude patřit po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="3d435-126">**Target resource group**: Resource group toowhich Azure VMs in hello target region will belong after failover.</span></span> <span data-ttu-id="3d435-127">Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v hello cílová oblast s příponou "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="3d435-127">By default, Site Recovery creates a new resource group in hello target region with an "asr" suffix.</span></span> 
   - <span data-ttu-id="3d435-128">**Cílová virtuální síť**: hello sítě, ve které virtuálních počítačích Azure v hello cílová oblast budou umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="3d435-128">**Target virtual network**: hello network in which Azure VMs in hello target region will be located after failover.</span></span> <span data-ttu-id="3d435-129">Ve výchozím nastavení Site Recovery vytvoří nové virtuální sítě (a podsítě) v oblasti cíl hello s příponou "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="3d435-129">By default, Site Recovery creates a new virtual network (and subnets) in hello target region with an "asr" suffix.</span></span> <span data-ttu-id="3d435-130">Tato síť je namapované tooyour zdrojovou síť.</span><span class="sxs-lookup"><span data-stu-id="3d435-130">This network is mapped tooyour source network.</span></span> <span data-ttu-id="3d435-131">Všimněte si, že po převzetí služeb při selhání virtuálního počítače můžete přiřadit konkrétní IP adresu, pokud potřebujete tooretain hello stejné IP adres v hello zdrojové a cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="3d435-131">Note that you can assign a specific IP address after failover of a VM, if you need tooretain hello same IP address in hello source and target locations.</span></span> 
   - <span data-ttu-id="3d435-132">**Účty úložiště mezipaměti**: Site Recovery používá účet úložiště v oblasti zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="3d435-132">**Cache storage accounts**: Site Recovery uses a storage account in hello source region.</span></span> <span data-ttu-id="3d435-133">Změny na zdrojové virtuální počítače se budou odesílat toothis účtu před replikace toohello cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="3d435-133">Changes on source VMs are sent toothis account, before replication toohello target location.</span></span> 
   - <span data-ttu-id="3d435-134">**Cíl účty úložiště**: ve výchozím nastavení, Site Recovery vytvoří nový účet úložiště v hello cílová oblast toomirror hello zdrojový účet úložiště virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3d435-134">**Target storage accounts**: By default, Site Recovery creates a new storage account in hello target region, toomirror hello source VM storage account.</span></span>
   -  <span data-ttu-id="3d435-135">**Cílové skupiny dostupnosti**: ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve hello cílová oblast s příponou "Automatické obnovení systému" hello.</span><span class="sxs-lookup"><span data-stu-id="3d435-135">**Target availability sets**: By default, Site Recovery creates a new availability set in hello target region, with hello "asr" suffix.</span></span> 
   - <span data-ttu-id="3d435-136">**Název zásady replikace**: název zásady.</span><span class="sxs-lookup"><span data-stu-id="3d435-136">**Replication policy name**: Policy name.</span></span>
   - <span data-ttu-id="3d435-137">**Uchování bodu obnovení**: ve výchozím nastavení Site Recovery zachová body obnovení po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="3d435-137">**Recovery point retention**: By default Site Recovery keeps recovery points for 24 hours.</span></span> <span data-ttu-id="3d435-138">Můžete nakonfigurovat hodnotu v rozmezí 1 až 72 hodin.</span><span class="sxs-lookup"><span data-stu-id="3d435-138">You can configure a value between 1 and 72 hours.</span></span>
   - <span data-ttu-id="3d435-139">**Frekvence snímkování konzistentní aplikace vzhledem**: ve výchozím nastavení trvá Site Recovery snímky konzistentní s aplikací každé 4 hodiny.</span><span class="sxs-lookup"><span data-stu-id="3d435-139">**App-consistent snapshot frequency**: By default Site Recovery takes an app-consistent snapshot every 4 hours.</span></span> <span data-ttu-id="3d435-140">Můžete nakonfigurovat libovolnou hodnotu mezi 1 a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="3d435-140">You can configure any value between 1 and 12 hours.</span></span> <span data-ttu-id="3d435-141">Data se replikují nepřetržitě:</span><span class="sxs-lookup"><span data-stu-id="3d435-141">Data is replicated continuously:</span></span>
    - <span data-ttu-id="3d435-142">Body obnovení konzistentní při selhání zachování konzistentních dat zápisu pořadí při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="3d435-142">Crash-consistent recovery points maintain consistent data write-order when created.</span></span> <span data-ttu-id="3d435-143">Tento typ bod obnovení je obvykle stačí, pokud je vaše aplikace navrženou toorecover z havárie bez nekonzistenci dat.</span><span class="sxs-lookup"><span data-stu-id="3d435-143">This type of recovery point is usually sufficient if your app is designed toorecover from a crash without data inconsistencies</span></span>
    - <span data-ttu-id="3d435-144">Body obnovení konzistentní při selhání jsou generovány každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="3d435-144">Crash-consistent recovery points are generated every few minutes.</span></span> <span data-ttu-id="3d435-145">Pomocí těchto toofail body obnovení a obnovit poskytuje virtuální počítače obnovení bodu cíl (RPO) v pořadí hello minut.</span><span class="sxs-lookup"><span data-stu-id="3d435-145">Using these recovery points toofail over and recover your VMs provides a Recovery Point Objective (RPO) in hello order of minutes.</span></span>
    - <span data-ttu-id="3d435-146">Body obnovení konzistentní vzhledem k aplikaci (v přidání toowrite pořadí konzistence) zkontrolujte, zda spuštěné aplikace dokončení všech operací a vyprázdní vyrovnávací paměti toodisk (uvedení aplikace).</span><span class="sxs-lookup"><span data-stu-id="3d435-146">App-consistent recovery points (in addition toowrite-order consistency) ensure that running apps complete all operations and flush buffers toodisk (application quiescing).</span></span> <span data-ttu-id="3d435-147">Doporučujeme že použít tyto body obnovení pro databáze aplikace, jako je například SQL Server, Oracle a Exchange.</span><span class="sxs-lookup"><span data-stu-id="3d435-147">We recommend you use these recovery points for database apps such as SQL Server, Oracle, and Exchange.</span></span>
        
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a><span data-ttu-id="3d435-149">Upravit nastavení</span><span class="sxs-lookup"><span data-stu-id="3d435-149">Modify settings</span></span>

<span data-ttu-id="3d435-150">Pokud chcete nastavení zásad toomodify cíle a replikace, hello následující:</span><span class="sxs-lookup"><span data-stu-id="3d435-150">If you want toomodify target and replication policy settings, do hello following:</span></span>

1. <span data-ttu-id="3d435-151">tooview nebo upravit nastavení cíle, klikněte na **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3d435-151">tooview or modify target settings, click **Settings**.</span></span>
2. <span data-ttu-id="3d435-152">toooverride hello výchozí cílový nastavení, klikněte na tlačítko **přizpůsobit**.</span><span class="sxs-lookup"><span data-stu-id="3d435-152">toooverride hello default target settings, click **Customize**.</span></span> <span data-ttu-id="3d435-153">Můžete zadat cílová skupina prostředků, virtuální sítě, skupinu dostupnosti a cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3d435-153">You can specify a target resource group, virtual network, availability set, and target storage account.</span></span> <span data-ttu-id="3d435-154">Skupiny dostupnosti můžete přidat pouze pokud virtuální počítače jsou součástí sady v oblasti zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="3d435-154">You can only add availability sets if VMs are part of a set in hello source region.</span></span>

    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. <span data-ttu-id="3d435-156">nastavení replikace toooverride pro body obnovení a snímky konzistentní s aplikací, klikněte na tlačítko **přizpůsobit** další příliš**zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="3d435-156">toooverride replication settings for recovery points and app-consistent snapshots, click **Customize** next too**Replication Policy**.</span></span>
 
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. <span data-ttu-id="3d435-158">Klikněte na tlačítko toostart zřizování hello cílové prostředky, **vytvoření cílové prostředky**.</span><span class="sxs-lookup"><span data-stu-id="3d435-158">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="3d435-159">Zřizování trvá minutu.</span><span class="sxs-lookup"><span data-stu-id="3d435-159">Provisioning takes a minute or so.</span></span> <span data-ttu-id="3d435-160">Nemáte zavřete okno hello při zřizování, nebo budete mít toostart přes.</span><span class="sxs-lookup"><span data-stu-id="3d435-160">Don't close hello blade during provisioning, or you'll have toostart over.</span></span>




## <a name="enable-replication"></a><span data-ttu-id="3d435-161">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="3d435-161">Enable replication</span></span>

1. <span data-ttu-id="3d435-162">V **nastavení**, klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="3d435-162">In **Settings**, click **Enable replication**.</span></span> <span data-ttu-id="3d435-163">To umožňuje počáteční replikace hello virtuálních počítačů, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="3d435-163">This enables initial replication of hello VMs you selected.</span></span> <span data-ttu-id="3d435-164">Počáteční replikace stavu může trvat některé toorefresh čas.</span><span class="sxs-lookup"><span data-stu-id="3d435-164">Initial replication status might take some time toorefresh.</span></span> <span data-ttu-id="3d435-165">Klikněte na tlačítko **aktualizovat** tooget hello nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="3d435-165">Click **Refresh** tooget hello latest status.</span></span>

2. <span data-ttu-id="3d435-166">Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="3d435-166">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

3. <span data-ttu-id="3d435-167">V **nastavení** > **replikované položky**, můžete zobrazit stav hello virtuálních počítačů a hello probíhá počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="3d435-167">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="3d435-168">Klikněte na tlačítko hello virtuálních počítačů toodrill dolů do jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="3d435-168">Click hello VM toodrill down into its settings.</span></span>



## <a name="next-steps"></a><span data-ttu-id="3d435-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3d435-169">Next steps</span></span>

<span data-ttu-id="3d435-170">Přejděte příliš[krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="3d435-170">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>
