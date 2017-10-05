---
title: "Replikovat virtuální počítače Azure mezi oblastmi Azure pro zotavení po havárii vyžaduje: Azure do Azure | Microsoft Docs"
description: "Shrnuje kroky je potřeba replikovat virtuální počítače Azure mezi oblastmi Azure (Azure Azure) se službou Azure Site Recovery pro potřebovat obnovení po havárii."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="742ac-103">Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="742ac-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

>[!NOTE]
>
> <span data-ttu-id="742ac-104">Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="742ac-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

<span data-ttu-id="742ac-105">Tento článek popisuje, jak replikovat virtuální počítače Azure mezi oblastmi Azure pomocí [Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="742ac-105">This article describes how to replicate Azure VMs between Azure regions by using the [Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="742ac-106">POST dotazy a v dolní části tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="742ac-106">Post comments and questions at the bottom of this article or on the [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="disaster-recovery-in-azure"></a><span data-ttu-id="742ac-107">Zotavení po havárii v Azure</span><span class="sxs-lookup"><span data-stu-id="742ac-107">Disaster recovery in Azure</span></span>

<span data-ttu-id="742ac-108">Předdefinované infrastrukturu Azure možností a funkcí přispívat k strategie dostupnosti robustní a odolné pro úlohy, které běží na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="742ac-108">Built-in Azure infrastructure capabilities and features contribute to a robust and resilient availability strategy for workloads that run on Azure VMs.</span></span> <span data-ttu-id="742ac-109">Existují však mnoha důvodů, proč potřebujete k vytvoření plánu zotavení po havárii mezi oblastmi Azure sami:</span><span class="sxs-lookup"><span data-stu-id="742ac-109">However, there are many reasons why you need to plan for disaster recovery between Azure regions yourself:</span></span>

- <span data-ttu-id="742ac-110">Je třeba splňovat pokyny dodržování předpisů pro konkrétní aplikace a úlohy, které vyžadují kontinuity podnikových procesů a strategie po havárii (BCDR).</span><span class="sxs-lookup"><span data-stu-id="742ac-110">You need to meet compliance guidelines for specific apps and workloads that require a business continuity and disaster recovery (BCDR) strategy.</span></span>
- <span data-ttu-id="742ac-111">Chcete mít možnost k ochraně a obnovování virtuálních počítačích Azure, podle obchodních rozhodnutí a to není pouze podle integrované funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="742ac-111">You want the ability to protect and recover Azure VMs based on your business decisions, and not only based on inbuilt Azure functionality.</span></span>
- <span data-ttu-id="742ac-112">Je potřeba provést testování převzetí služeb při selhání a obnovení v souladu s potřebám vaší firmy a dodržování předpisů bez dopadu na produkční.</span><span class="sxs-lookup"><span data-stu-id="742ac-112">You need to test failover and recovery in accordance with your business and compliance needs, with no impact on production.</span></span>
- <span data-ttu-id="742ac-113">Budete muset převzetí služeb při selhání oblasti obnovení v případě havárie a navrácení služeb po obnovení původního zdrojového oblast bezproblémově.</span><span class="sxs-lookup"><span data-stu-id="742ac-113">You need to fail over to the recovery region in the event of a disaster and fail back to the original source region seamlessly.</span></span>

<span data-ttu-id="742ac-114">Pomocí Site Recovery pro replikaci virtuálního počítače Azure do Azure můžete provést tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="742ac-114">Use Site Recovery for Azure-to-Azure VM replication to help you do all these tasks.</span></span>


## <a name="why-use-site-recovery"></a><span data-ttu-id="742ac-115">Proč používat Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="742ac-115">Why use Site Recovery?</span></span>      

<span data-ttu-id="742ac-116">Site Recovery poskytuje jednoduchý způsob, jak replikovat virtuální počítače Azure mezi oblastmi:</span><span class="sxs-lookup"><span data-stu-id="742ac-116">Site Recovery provides a simple way to replicate Azure VMs between regions:</span></span>

- <span data-ttu-id="742ac-117">**Automatické nasazení**.</span><span class="sxs-lookup"><span data-stu-id="742ac-117">**Automatic deployment**.</span></span> <span data-ttu-id="742ac-118">Na rozdíl od model replikace aktivní aktivní není nutné pro složité a nákladné infrastruktury v sekundární oblasti.</span><span class="sxs-lookup"><span data-stu-id="742ac-118">Unlike an active-active replication model, there's no need for an expensive and complex infrastructure in the secondary region.</span></span> <span data-ttu-id="742ac-119">Když povolíte replikaci, Site Recovery ve automaticky vytvoří požadované prostředky cílová oblast na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="742ac-119">When you enable replication, Site Recovery automatically creates the required resources in the target region, based on source region settings.</span></span>
- <span data-ttu-id="742ac-120">**Řízení oblasti**.</span><span class="sxs-lookup"><span data-stu-id="742ac-120">**Control regions**.</span></span> <span data-ttu-id="742ac-121">Pomocí Site Recovery můžete pro všechny oblasti v rámci kontinentě replikaci z libovolné oblasti.</span><span class="sxs-lookup"><span data-stu-id="742ac-121">With Site Recovery, you can replicate from any region to any region within a continent.</span></span> <span data-ttu-id="742ac-122">Toto porovnání s přístupem pro čtení geograficky redundantní úložiště, která asynchronně replikuje mezi standard [spárovat oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) pouze.</span><span class="sxs-lookup"><span data-stu-id="742ac-122">Compare this with read-access geo-redundant storage, which replicates asynchronously between standard [paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) only.</span></span> <span data-ttu-id="742ac-123">Geograficky redundantní úložiště s přístupem pro čtení poskytuje přístup jen pro čtení k datům v cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="742ac-123">Read-access geo-redundant storage provides read-only access to the data in the target region.</span></span>
- <span data-ttu-id="742ac-124">**Automatizované replikace**.</span><span class="sxs-lookup"><span data-stu-id="742ac-124">**Automated replication**.</span></span> <span data-ttu-id="742ac-125">Site Recovery poskytuje automatizované průběžnou replikaci.</span><span class="sxs-lookup"><span data-stu-id="742ac-125">Site Recovery provides automated continuous replication.</span></span> <span data-ttu-id="742ac-126">Převzetí služeb při selhání a navrácení služeb po obnovení můžete spustit s jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="742ac-126">Failover and failback can be triggered with a single click.</span></span>
- <span data-ttu-id="742ac-127">**RTO a plánovaný bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="742ac-127">**RTO and RPO**.</span></span> <span data-ttu-id="742ac-128">Site Recovery se využívá Azure síťové infrastruktury, která se připojuje oblasti zachovat velmi nízkou RTO a plánovaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="742ac-128">Site Recovery takes advantage of the Azure network infrastructure that connects regions to keep RTO and RPO very low.</span></span>
- <span data-ttu-id="742ac-129">**Testování**.</span><span class="sxs-lookup"><span data-stu-id="742ac-129">**Testing**.</span></span> <span data-ttu-id="742ac-130">Cvičení zotavení po havárii můžete spustit s na vyžádání testovací převzetí služeb při selhání podle potřeby, aniž by to ovlivňovalo produkční zatížení nebo probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="742ac-130">You can run disaster-recovery drills with on-demand test failovers, as and when needed, without affecting your production workloads or ongoing replication.</span></span>
- <span data-ttu-id="742ac-131">**Plány obnovení**.</span><span class="sxs-lookup"><span data-stu-id="742ac-131">**Recovery plans**.</span></span> <span data-ttu-id="742ac-132">Plány obnovení můžete použít k orchestraci převzetí služeb při selhání a navrácení služeb po obnovení celé aplikace běžící na několika virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="742ac-132">You can use recovery plans to orchestrate failover and failback of the entire application running on multiple VMs.</span></span> <span data-ttu-id="742ac-133">Funkce plán obnovení má bohaté prvotřídní integrace s runbooky služby Azure automation.</span><span class="sxs-lookup"><span data-stu-id="742ac-133">The recovery plan feature has rich first-class integration with Azure automation runbooks.</span></span>


## <a name="deployment-summary"></a><span data-ttu-id="742ac-134">Souhrn nasazení</span><span class="sxs-lookup"><span data-stu-id="742ac-134">Deployment summary</span></span>

<span data-ttu-id="742ac-135">Zde je uveden seznam, co musíte udělat pro nastavení replikace virtuálních počítačů mezi oblastmi Azure:</span><span class="sxs-lookup"><span data-stu-id="742ac-135">Here's a summary of what you need to do to set up replication of VMs between Azure regions:</span></span>

1. <span data-ttu-id="742ac-136">Vytvořte trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="742ac-136">Create a Recovery Services vault.</span></span> <span data-ttu-id="742ac-137">Trezor obsahuje nastavení konfigurace a orchestruje replikaci.</span><span class="sxs-lookup"><span data-stu-id="742ac-137">The vault contains configuration settings and orchestrates replication.</span></span>
2. <span data-ttu-id="742ac-138">Povolení replikace pro virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="742ac-138">Enable replication for the Azure VMs.</span></span>
3. <span data-ttu-id="742ac-139">Spusťte převzetí služeb při selhání a ujistěte se, zda vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="742ac-139">Run a test failover to make sure that everything's working as expected.</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="742ac-140">Můžete zkontrolovat [matici podpory pro replikaci virtuálního počítače Azure](./site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="742ac-140">You can check the [support matrix for Azure VM replication](./site-recovery-support-matrix-azure-to-azure.md).</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="742ac-141">Informace o tom, jak nakonfigurovat požadované síťové odchozí připojení pro virtuální počítače Azure Site Recovery replikace najdete v tématu [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="742ac-141">For information on how to configure the required network outbound connectivity for Azure VMs for Site Recovery replication, see the [networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md).</span></span>

### <a name="before-you-start"></a><span data-ttu-id="742ac-142">Než začnete</span><span class="sxs-lookup"><span data-stu-id="742ac-142">Before you start</span></span>

* <span data-ttu-id="742ac-143">Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="742ac-143">Your Azure user account needs to have certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of an Azure virtual machine.</span></span>
* <span data-ttu-id="742ac-144">Vašeho předplatného Azure by měly být povoleny k vytvoření virtuálních počítačů v cílovém umístění, které chcete použít jako oblasti obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="742ac-144">Your Azure subscription should be enabled to create VMs in the target location that you want to use as the disaster recovery region.</span></span> <span data-ttu-id="742ac-145">Obraťte se na podporu, aby umožnil požadované kvótu.</span><span class="sxs-lookup"><span data-stu-id="742ac-145">Contact support to enable the required quota.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="742ac-146">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="742ac-146">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="742ac-147">Doporučujeme vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="742ac-147">We recommend that you create the Recovery Services vault in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="742ac-148">Například, pokud cílové umístění je střed USA, vytvořte trezor v **střed USA**.</span><span class="sxs-lookup"><span data-stu-id="742ac-148">For example, if your target location is the central US, create the vault in **Central US**.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="742ac-149">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="742ac-149">Enable replication</span></span>

<span data-ttu-id="742ac-150">V **trezory služeb zotavení**, klikněte na název trezoru.</span><span class="sxs-lookup"><span data-stu-id="742ac-150">In **Recovery Services vaults**, click the vault name.</span></span> <span data-ttu-id="742ac-151">V trezoru, klikněte **+ replikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="742ac-151">In the vault, click the **+Replicate** button.</span></span>

### <a name="step-1-configure-the-source"></a><span data-ttu-id="742ac-152">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="742ac-152">Step 1.</span></span> <span data-ttu-id="742ac-153">Nastavit zdroj</span><span class="sxs-lookup"><span data-stu-id="742ac-153">Configure the source</span></span>
1. <span data-ttu-id="742ac-154">V **zdroj**, vyberte **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="742ac-154">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="742ac-155">V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="742ac-155">In **Source location**, select the source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="742ac-156">Vyberte model nasazení virtuálních počítačů: **Resource Manager** nebo **Classic**.</span><span class="sxs-lookup"><span data-stu-id="742ac-156">Select the deployment model of your VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="742ac-157">Vyberte **zdrojové skupiny prostředků** pro virtuální počítače správce prostředků nebo **Cloudová služba** pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="742ac-157">Select the **Source resource group** for Resource Manager VMs or **cloud service** for classic VMs.</span></span>

    ![Nastavit zdroj](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a><span data-ttu-id="742ac-159">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="742ac-159">Step 2.</span></span> <span data-ttu-id="742ac-160">Vyberte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="742ac-160">Select virtual machines</span></span>

* <span data-ttu-id="742ac-161">Vyberte virtuální počítače, které chcete replikovat a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="742ac-161">Select the VMs you want to replicate, and then click **OK**.</span></span>

    ![Vyberte virtuální počítače](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a><span data-ttu-id="742ac-163">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="742ac-163">Step 3.</span></span> <span data-ttu-id="742ac-164">Konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="742ac-164">Configure settings</span></span>

1. <span data-ttu-id="742ac-165">Pokud chcete přepsat výchozí nastavení cílové a zadejte nastavení podle vaší volby, klikněte na tlačítko **přizpůsobit**.</span><span class="sxs-lookup"><span data-stu-id="742ac-165">To override the default target settings and specify the settings of your choice, click **Customize**.</span></span> <span data-ttu-id="742ac-166">Další informace najdete v tématu [přizpůsobit cílové prostředky](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span><span class="sxs-lookup"><span data-stu-id="742ac-166">For more information, see [Customize target resources](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span></span>

    ![Konfigurace nastavení](./media/site-recovery-azure-to-azure/settings.png)


2. <span data-ttu-id="742ac-168">Site Recovery ve výchozím nastavení vytvoří zásadu replikace, která přebírá snímky konzistentní aplikace každé 4 hodiny a uchovává body obnovení po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="742ac-168">By default, Site Recovery creates a replication policy that takes app-consistent snapshots every 4 hours and retains recovery points for 24 hours.</span></span> <span data-ttu-id="742ac-169">Chcete-li vytvořit zásadu s různá nastavení, klikněte na tlačítko **přizpůsobit** vedle **zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="742ac-169">To create a policy with different settings, click **Customize** next to **Replication Policy**.</span></span>

    ![Úpravy zásad](./media/site-recovery-azure-to-azure/customize-policy.png)

3. <span data-ttu-id="742ac-171">Chcete-li spustit zřizování cílové prostředky, klikněte na tlačítko **vytvoření cílové prostředky**.</span><span class="sxs-lookup"><span data-stu-id="742ac-171">To start provisioning the target resources, click **Create target resources**.</span></span> <span data-ttu-id="742ac-172">Zřizování trvá minutu.</span><span class="sxs-lookup"><span data-stu-id="742ac-172">Provisioning takes a minute or so.</span></span> <span data-ttu-id="742ac-173">Nemáte zavřete okno při zřizování, nebo je nutné začít znovu.</span><span class="sxs-lookup"><span data-stu-id="742ac-173">Don't close the blade during provisioning, or you need to start over.</span></span>

4. <span data-ttu-id="742ac-174">Chcete-li aktivovat replikaci vybraného virtuálního počítače, klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="742ac-174">To trigger replication of the selected VM, click **Enable replication**.</span></span>

5. <span data-ttu-id="742ac-175">Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="742ac-175">You can track progress of the **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

6. <span data-ttu-id="742ac-176">V **nastavení** > **replikované položky**, můžete zobrazit stav virtuálních počítačů a probíhá počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="742ac-176">In **Settings** > **Replicated Items**, you can view the status of VMs and the initial replication progress.</span></span> <span data-ttu-id="742ac-177">Klikněte na virtuální počítač můžete rozbalit jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="742ac-177">Click the VM to drill down into its settings.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="742ac-178">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="742ac-178">Run a test failover</span></span>

<span data-ttu-id="742ac-179">Po nastavení vše, spusťte převzetí služeb při selhání a ujistěte se, zda že vše funguje podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="742ac-179">After you set up everything, run a test failover to make sure everything's working as expected:</span></span>

1. <span data-ttu-id="742ac-180">K převzetí služeb při selhání jednom počítači, v **nastavení** > **replikované položky**, klikněte na virtuální počítač **+ testovací převzetí služeb při selhání** ikonu.</span><span class="sxs-lookup"><span data-stu-id="742ac-180">To fail over a single machine, in **Settings** > **Replicated Items**, click the VM **+Test Failover** icon.</span></span>

2. <span data-ttu-id="742ac-181">K převzetí služeb při selhání plánu obnovení v **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na plán **testovací převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="742ac-181">To fail over a recovery plan, in **Settings** > **Recovery Plans**, right-click the plan **Test Failover**.</span></span> <span data-ttu-id="742ac-182">Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="742ac-182">To create a recovery plan, [follow these instructions](site-recovery-create-recovery-plans.md).</span></span> 

3. <span data-ttu-id="742ac-183">V **testovací převzetí služeb při selhání**, vyberte cíl virtuální síť Azure ke které virtuální počítače Azure připojeni potom, co dojde převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="742ac-183">In **Test Failover**, select the target Azure virtual network to which Azure VMs are connected after the failover occurs.</span></span>

4. <span data-ttu-id="742ac-184">Chcete-li spustit převzetí služeb při selhání, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="742ac-184">To start the failover, click **OK**.</span></span> <span data-ttu-id="742ac-185">Chcete-li sledovat průběh, klikněte na virtuálním počítači otevřete jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="742ac-185">To track progress, click the VM to open its properties.</span></span> <span data-ttu-id="742ac-186">Nebo můžete kliknout **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="742ac-186">Or you can click the **Test Failover** job in the vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>

5. <span data-ttu-id="742ac-187">Po dokončení převzetí repliky počítač Azure se zobrazí na portálu Azure > **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="742ac-187">After the failover finishes, the replica Azure machine appears in the Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="742ac-188">Ujistěte se, že virtuální počítač odpovídající velikost, byl připojený k příslušné síti, a jestli je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="742ac-188">Make sure that the VM is the appropriate size, that it's connected to the appropriate network, and that it's running.</span></span>

6. <span data-ttu-id="742ac-189">Chcete-li odstranit virtuální počítače, které byly vytvořeny během testu převzetí služeb, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** na replikované položky nebo plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="742ac-189">To delete the VMs that were created during the test failover, click **Cleanup test failover** on the replicated item or the recovery plan.</span></span> <span data-ttu-id="742ac-190">V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="742ac-190">In **Notes**, record and save any observations associated with the test failover.</span></span> 

<span data-ttu-id="742ac-191">[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="742ac-191">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>


## <a name="next-steps"></a><span data-ttu-id="742ac-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="742ac-192">Next steps</span></span>

<span data-ttu-id="742ac-193">Po testovací nasazení:</span><span class="sxs-lookup"><span data-stu-id="742ac-193">After you test the deployment:</span></span>

- <span data-ttu-id="742ac-194">Přečtěte si [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a o tom, jak je spustit.</span><span class="sxs-lookup"><span data-stu-id="742ac-194">[Learn more](site-recovery-failover.md) about different types of failovers and how to run them.</span></span>
- <span data-ttu-id="742ac-195">Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) ke snížení RTO.</span><span class="sxs-lookup"><span data-stu-id="742ac-195">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) to reduce RTO.</span></span>
