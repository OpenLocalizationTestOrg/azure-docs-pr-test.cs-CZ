---
title: "Povolte replikaci pro virtuální počítače Azure na jinou oblast Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nutné k povolení replikace jiné oblasti Azure pro virtuální počítače Azure pomocí služby Azure Site Recovery"
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
ms.openlocfilehash: f426ba4341e61d3c7da820d7d5097b217e94ca0e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a><span data-ttu-id="ff1eb-103">Krok 5: Povolení replikace pro virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="ff1eb-103">Step 5: Enable replication for Azure VMs</span></span>


<span data-ttu-id="ff1eb-104">Po nastavení [trezor služeb zotavení](azure-to-azure-walkthrough-vault.md), použijte tento článek k povolení replikace virtuálních počítačů (VM), do jiné oblasti Azure, s [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ff1eb-104">After setting up a [Recovery Services vault](azure-to-azure-walkthrough-vault.md), use this article to enable replication of virtual machines (VMs), to another Azure region, with [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="ff1eb-105">Povolení replikace, můžete nastavit zdroje a cíle, ověřte zásady replikace a vyberte virtuální počítače, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-105">To enable replication, you set up source and target settings, verify the replication policy, and select VMs you want to replicate.</span></span>

- <span data-ttu-id="ff1eb-106">Po dokončení článek, virtuální počítače Azure by měla replikace do sekundární oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-106">When you finish the article, your Azure VMs should be replicating to the secondary Azure region.</span></span>
- <span data-ttu-id="ff1eb-107">Odeslat všechny komentáře v dolní části tohoto článku nebo pokládání dotazů v [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="ff1eb-107">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="ff1eb-108">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-108">Azure VM replication is currently in preview.</span></span>


## <a name="select-the-source"></a><span data-ttu-id="ff1eb-109">Vyberte zdroj</span><span class="sxs-lookup"><span data-stu-id="ff1eb-109">Select the source</span></span> 

1. <span data-ttu-id="ff1eb-110">V trezory služeb zotavení, klikněte na název trezoru > **+ replikovat**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-110">In Recovery Services vaults, click the vault name > **+Replicate**.</span></span>
2. <span data-ttu-id="ff1eb-111">V **zdroj**, vyberte **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-111">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="ff1eb-112">V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-112">In **Source location**, select the source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="ff1eb-113">Vyberte **modelu nasazení virtuálního počítače Azure** pro virtuální počítače: **Resource Manager** nebo **Classic**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-113">Select the **Azure virtual machine deployment model** for VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="ff1eb-114">Vyberte **zdrojové skupiny prostředků** pro správce prostředků virtuálních počítačů, nebo **Cloudová služba** pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-114">Select the **Source resource group** for Resource Manager VMs, or **cloud service** for classic VMs.</span></span>
5. <span data-ttu-id="ff1eb-115">Kliknutím na **OK** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-115">Click **OK** to save the settings.</span></span>

    ![Nastavit zdroj](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-the-vms"></a><span data-ttu-id="ff1eb-117">Vyberte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="ff1eb-117">Select the VMs</span></span>

<span data-ttu-id="ff1eb-118">Site Recovery načte seznam virtuálních počítačů spojené s předplatného a skupiny nebo cloudové služby prostředků.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-118">Site Recovery retrieves a list of the VMs associated with the subscription and resource group/cloud service.</span></span>

1. <span data-ttu-id="ff1eb-119">V **virtuální počítače**, vyberte virtuální počítače, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-119">In **Virtual Machines**, select the VMs you want to replicate.</span></span>
2. <span data-ttu-id="ff1eb-120">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-120">Click **OK**.</span></span>

    ![Vyberte virtuální počítače](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a><span data-ttu-id="ff1eb-122">Konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="ff1eb-122">Configure settings</span></span>

<span data-ttu-id="ff1eb-123">Zřizuje obnovení lokality výchozí nastavení pro cílová oblast (podle nastavení oblasti zdroje) a zásady replikace:</span><span class="sxs-lookup"><span data-stu-id="ff1eb-123">Site Recovery provisions default settings for the target region (based on the source region settings), and the replication policy:</span></span>

   - <span data-ttu-id="ff1eb-124">**Cílové umístění**: cílová oblast, kterou chcete použít pro zotavení po havárii.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-124">**Target location**: The target region you want to use for disaster recovery.</span></span> <span data-ttu-id="ff1eb-125">Doporučujeme vám, že cílové umístění odpovídá umístění trezoru Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-125">We recommend that the target location matches the location of the Site Recovery vault.</span></span>
   - <span data-ttu-id="ff1eb-126">**Cílová skupina prostředků**: skupinu prostředků, ke které virtuální počítače Azure v cílové oblasti patřit po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-126">**Target resource group**: Resource group to which Azure VMs in the target region will belong after failover.</span></span> <span data-ttu-id="ff1eb-127">Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v cílové oblasti s příponou "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="ff1eb-127">By default, Site Recovery creates a new resource group in the target region with an "asr" suffix.</span></span> 
   - <span data-ttu-id="ff1eb-128">**Cílová virtuální síť**: sítě, ve které virtuálních počítačích Azure v cílové oblasti budou umístěné po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-128">**Target virtual network**: The network in which Azure VMs in the target region will be located after failover.</span></span> <span data-ttu-id="ff1eb-129">Ve výchozím nastavení Site Recovery vytvoří nové virtuální sítě (a podsítě) v oblasti cíl s příponou "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="ff1eb-129">By default, Site Recovery creates a new virtual network (and subnets) in the target region with an "asr" suffix.</span></span> <span data-ttu-id="ff1eb-130">Tato síť je namapována na zdrojovou síť.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-130">This network is mapped to your source network.</span></span> <span data-ttu-id="ff1eb-131">Všimněte si, že po převzetí služeb při selhání virtuálního počítače, můžete přiřadit konkrétní IP adresu, pokud je potřeba zachovat stejnou IP adresu ve zdrojové a cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-131">Note that you can assign a specific IP address after failover of a VM, if you need to retain the same IP address in the source and target locations.</span></span> 
   - <span data-ttu-id="ff1eb-132">**Účty úložiště mezipaměti**: Site Recovery používá účet úložiště v oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-132">**Cache storage accounts**: Site Recovery uses a storage account in the source region.</span></span> <span data-ttu-id="ff1eb-133">Změny na zdrojové virtuální počítače jsou odesílány tento účet, než replikaci do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-133">Changes on source VMs are sent to this account, before replication to the target location.</span></span> 
   - <span data-ttu-id="ff1eb-134">**Cíl účty úložiště**: ve výchozím nastavení, Site Recovery vytvoří nový účet úložiště v oblasti cíl pro zrcadlení zdrojový účet úložiště virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-134">**Target storage accounts**: By default, Site Recovery creates a new storage account in the target region, to mirror the source VM storage account.</span></span>
   -  <span data-ttu-id="ff1eb-135">**Cílové skupiny dostupnosti**: ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve cílová oblast s příponou "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="ff1eb-135">**Target availability sets**: By default, Site Recovery creates a new availability set in the target region, with the "asr" suffix.</span></span> 
   - <span data-ttu-id="ff1eb-136">**Název zásady replikace**: název zásady.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-136">**Replication policy name**: Policy name.</span></span>
   - <span data-ttu-id="ff1eb-137">**Uchování bodu obnovení**: ve výchozím nastavení Site Recovery zachová body obnovení po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-137">**Recovery point retention**: By default Site Recovery keeps recovery points for 24 hours.</span></span> <span data-ttu-id="ff1eb-138">Můžete nakonfigurovat hodnotu v rozmezí 1 až 72 hodin.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-138">You can configure a value between 1 and 72 hours.</span></span>
   - <span data-ttu-id="ff1eb-139">**Frekvence snímkování konzistentní aplikace vzhledem**: ve výchozím nastavení trvá Site Recovery snímky konzistentní s aplikací každé 4 hodiny.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-139">**App-consistent snapshot frequency**: By default Site Recovery takes an app-consistent snapshot every 4 hours.</span></span> <span data-ttu-id="ff1eb-140">Můžete nakonfigurovat libovolnou hodnotu mezi 1 a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-140">You can configure any value between 1 and 12 hours.</span></span> <span data-ttu-id="ff1eb-141">Data se replikují nepřetržitě:</span><span class="sxs-lookup"><span data-stu-id="ff1eb-141">Data is replicated continuously:</span></span>
    - <span data-ttu-id="ff1eb-142">Body obnovení konzistentní při selhání zachování konzistentních dat zápisu pořadí při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-142">Crash-consistent recovery points maintain consistent data write-order when created.</span></span> <span data-ttu-id="ff1eb-143">Tento typ bod obnovení je obvykle stačí, pokud vaše aplikace je navržená pro zotavit havárie bez nekonzistenci dat.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-143">This type of recovery point is usually sufficient if your app is designed to recover from a crash without data inconsistencies</span></span>
    - <span data-ttu-id="ff1eb-144">Body obnovení konzistentní při selhání jsou generovány každých několik minut.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-144">Crash-consistent recovery points are generated every few minutes.</span></span> <span data-ttu-id="ff1eb-145">Pomocí těchto bodů obnovení pro převzetí služeb při selhání a obnovení virtuálních počítačů poskytuje obnovení bodu cíl (RPO) v řádu minut.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-145">Using these recovery points to fail over and recover your VMs provides a Recovery Point Objective (RPO) in the order of minutes.</span></span>
    - <span data-ttu-id="ff1eb-146">Body obnovení konzistentní vzhledem k aplikaci (kromě zápisu pořadí konzistence) zkontrolujte, zda spuštěné aplikace dokončení všech operací a vyprázdní vyrovnávací paměti na disk (uvedení aplikace).</span><span class="sxs-lookup"><span data-stu-id="ff1eb-146">App-consistent recovery points (in addition to write-order consistency) ensure that running apps complete all operations and flush buffers to disk (application quiescing).</span></span> <span data-ttu-id="ff1eb-147">Doporučujeme že použít tyto body obnovení pro databáze aplikace, jako je například SQL Server, Oracle a Exchange.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-147">We recommend you use these recovery points for database apps such as SQL Server, Oracle, and Exchange.</span></span>
        
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a><span data-ttu-id="ff1eb-149">Upravit nastavení</span><span class="sxs-lookup"><span data-stu-id="ff1eb-149">Modify settings</span></span>

<span data-ttu-id="ff1eb-150">Pokud chcete upravit nastavení zásad cíle a replikace, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ff1eb-150">If you want to modify target and replication policy settings, do the following:</span></span>

1. <span data-ttu-id="ff1eb-151">Chcete-li zobrazit nebo upravit nastavení cíle, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-151">To view or modify target settings, click **Settings**.</span></span>
2. <span data-ttu-id="ff1eb-152">Chcete-li přepsat výchozí nastavení cíl, klikněte na tlačítko **přizpůsobit**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-152">To override the default target settings, click **Customize**.</span></span> <span data-ttu-id="ff1eb-153">Můžete zadat cílová skupina prostředků, virtuální sítě, skupinu dostupnosti a cílový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-153">You can specify a target resource group, virtual network, availability set, and target storage account.</span></span> <span data-ttu-id="ff1eb-154">Skupiny dostupnosti můžete přidat pouze pokud virtuální počítače jsou součástí sady v oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-154">You can only add availability sets if VMs are part of a set in the source region.</span></span>

    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. <span data-ttu-id="ff1eb-156">Chcete-li přepsat nastavení replikace pro body obnovení a snímky konzistentní s aplikací, klikněte na tlačítko **přizpůsobit** vedle **zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-156">To override replication settings for recovery points and app-consistent snapshots, click **Customize** next to **Replication Policy**.</span></span>
 
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. <span data-ttu-id="ff1eb-158">Chcete-li spustit zřizování cílové prostředky, klikněte na tlačítko **vytvoření cílové prostředky**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-158">To start provisioning the target resources, click **Create target resources**.</span></span> <span data-ttu-id="ff1eb-159">Zřizování trvá minutu.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-159">Provisioning takes a minute or so.</span></span> <span data-ttu-id="ff1eb-160">Nemáte zavřete okno při zřizování, nebo budete muset začít znovu.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-160">Don't close the blade during provisioning, or you'll have to start over.</span></span>




## <a name="enable-replication"></a><span data-ttu-id="ff1eb-161">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="ff1eb-161">Enable replication</span></span>

1. <span data-ttu-id="ff1eb-162">V **nastavení**, klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-162">In **Settings**, click **Enable replication**.</span></span> <span data-ttu-id="ff1eb-163">To umožňuje počáteční replikace virtuálních počítačů, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-163">This enables initial replication of the VMs you selected.</span></span> <span data-ttu-id="ff1eb-164">Aktualizovat stav počáteční replikace může chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-164">Initial replication status might take some time to refresh.</span></span> <span data-ttu-id="ff1eb-165">Klikněte na tlačítko **aktualizovat** a získat nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-165">Click **Refresh** to get the latest status.</span></span>

2. <span data-ttu-id="ff1eb-166">Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-166">You can track progress of the **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

3. <span data-ttu-id="ff1eb-167">V **nastavení** > **replikované položky**, můžete zobrazit stav virtuálních počítačů a probíhá počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-167">In **Settings** > **Replicated Items**, you can view the status of VMs and the initial replication progress.</span></span> <span data-ttu-id="ff1eb-168">Klikněte na virtuální počítač můžete rozbalit jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="ff1eb-168">Click the VM to drill down into its settings.</span></span>



## <a name="next-steps"></a><span data-ttu-id="ff1eb-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ff1eb-169">Next steps</span></span>

<span data-ttu-id="ff1eb-170">Přejděte na [krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="ff1eb-170">Go to [Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>
