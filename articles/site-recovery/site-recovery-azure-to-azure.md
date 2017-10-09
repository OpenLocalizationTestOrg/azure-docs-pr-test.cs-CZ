---
title: "Replikovat virtuální počítače Azure mezi oblastmi Azure pro zotavení po havárii vyžaduje: Azure tooAzure | Microsoft Docs"
description: "Shrnuje kroky hello musíte virtuální počítače Azure tooreplicate mezi oblastmi Azure (Azure Azure) se službou Azure Site Recovery hello pro potřebovat obnovení po havárii."
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
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="da684-103">Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="da684-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

>[!NOTE]
>
> <span data-ttu-id="da684-104">Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="da684-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

<span data-ttu-id="da684-105">Tento článek popisuje, jak tooreplicate virtuálních počítačích Azure mezi oblastmi Azure pomocí hello [Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="da684-105">This article describes how tooreplicate Azure VMs between Azure regions by using hello [Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="da684-106">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="da684-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="disaster-recovery-in-azure"></a><span data-ttu-id="da684-107">Zotavení po havárii v Azure</span><span class="sxs-lookup"><span data-stu-id="da684-107">Disaster recovery in Azure</span></span>

<span data-ttu-id="da684-108">Předdefinované infrastrukturu Azure možností a funkcí přispívat strategie pro dostupnost robustní a odolné tooa pro úlohy, které běží na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="da684-108">Built-in Azure infrastructure capabilities and features contribute tooa robust and resilient availability strategy for workloads that run on Azure VMs.</span></span> <span data-ttu-id="da684-109">Existují však mnoha důvodů, proč potřebujete tooplan pro zotavení po havárii mezi oblastmi Azure sami:</span><span class="sxs-lookup"><span data-stu-id="da684-109">However, there are many reasons why you need tooplan for disaster recovery between Azure regions yourself:</span></span>

- <span data-ttu-id="da684-110">Potřebujete toomeet předpisy pro konkrétní aplikace a úlohy, které vyžadují kontinuity podnikových procesů a strategie po havárii (BCDR).</span><span class="sxs-lookup"><span data-stu-id="da684-110">You need toomeet compliance guidelines for specific apps and workloads that require a business continuity and disaster recovery (BCDR) strategy.</span></span>
- <span data-ttu-id="da684-111">Chcete hello možnost tooprotect a obnovení virtuálních počítačů Azure podle obchodních rozhodnutí a to není pouze podle integrované funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="da684-111">You want hello ability tooprotect and recover Azure VMs based on your business decisions, and not only based on inbuilt Azure functionality.</span></span>
- <span data-ttu-id="da684-112">Potřebujete v souladu s potřebám vaší firmy a dodržování předpisů bez dopadu na produkční tootest převzetí služeb při selhání a obnovení.</span><span class="sxs-lookup"><span data-stu-id="da684-112">You need tootest failover and recovery in accordance with your business and compliance needs, with no impact on production.</span></span>
- <span data-ttu-id="da684-113">Potřebují toofail přes toohello oblasti obnovení v případě havárie hello a bezproblémově nezdaří back toohello původní zdrojový oblast.</span><span class="sxs-lookup"><span data-stu-id="da684-113">You need toofail over toohello recovery region in hello event of a disaster and fail back toohello original source region seamlessly.</span></span>

<span data-ttu-id="da684-114">Pomocí Site Recovery pro virtuální počítač Azure Azure replikace toohelp provést tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="da684-114">Use Site Recovery for Azure-to-Azure VM replication toohelp you do all these tasks.</span></span>


## <a name="why-use-site-recovery"></a><span data-ttu-id="da684-115">Proč používat Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="da684-115">Why use Site Recovery?</span></span>      

<span data-ttu-id="da684-116">Site Recovery poskytuje jednoduchý způsob tooreplicate virtuálních počítačích Azure mezi oblastmi:</span><span class="sxs-lookup"><span data-stu-id="da684-116">Site Recovery provides a simple way tooreplicate Azure VMs between regions:</span></span>

- <span data-ttu-id="da684-117">**Automatické nasazení**.</span><span class="sxs-lookup"><span data-stu-id="da684-117">**Automatic deployment**.</span></span> <span data-ttu-id="da684-118">Na rozdíl od model replikace aktivní aktivní není nutné pro složité a nákladné infrastruktury v sekundární oblasti hello.</span><span class="sxs-lookup"><span data-stu-id="da684-118">Unlike an active-active replication model, there's no need for an expensive and complex infrastructure in hello secondary region.</span></span> <span data-ttu-id="da684-119">Když povolíte replikaci, Site Recovery ve automaticky vytvoří hello požadované prostředky hello cílová oblast, na základě nastavení oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="da684-119">When you enable replication, Site Recovery automatically creates hello required resources in hello target region, based on source region settings.</span></span>
- <span data-ttu-id="da684-120">**Řízení oblasti**.</span><span class="sxs-lookup"><span data-stu-id="da684-120">**Control regions**.</span></span> <span data-ttu-id="da684-121">Pomocí Site Recovery můžete replikovat z libovolné oblasti tooany oblasti v rámci kontinentě.</span><span class="sxs-lookup"><span data-stu-id="da684-121">With Site Recovery, you can replicate from any region tooany region within a continent.</span></span> <span data-ttu-id="da684-122">Toto porovnání s přístupem pro čtení geograficky redundantní úložiště, která asynchronně replikuje mezi standard [spárovat oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) pouze.</span><span class="sxs-lookup"><span data-stu-id="da684-122">Compare this with read-access geo-redundant storage, which replicates asynchronously between standard [paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) only.</span></span> <span data-ttu-id="da684-123">Geograficky redundantní úložiště s přístupem pro čtení poskytuje přístup jen pro čtení toohello data v hello cílová oblast.</span><span class="sxs-lookup"><span data-stu-id="da684-123">Read-access geo-redundant storage provides read-only access toohello data in hello target region.</span></span>
- <span data-ttu-id="da684-124">**Automatizované replikace**.</span><span class="sxs-lookup"><span data-stu-id="da684-124">**Automated replication**.</span></span> <span data-ttu-id="da684-125">Site Recovery poskytuje automatizované průběžnou replikaci.</span><span class="sxs-lookup"><span data-stu-id="da684-125">Site Recovery provides automated continuous replication.</span></span> <span data-ttu-id="da684-126">Převzetí služeb při selhání a navrácení služeb po obnovení můžete spustit s jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="da684-126">Failover and failback can be triggered with a single click.</span></span>
- <span data-ttu-id="da684-127">**RTO a plánovaný bod obnovení**.</span><span class="sxs-lookup"><span data-stu-id="da684-127">**RTO and RPO**.</span></span> <span data-ttu-id="da684-128">Site Recovery využívá výhod hello Azure síťové infrastruktuře propojující oblasti tookeep RTO a velmi nízké RPO.</span><span class="sxs-lookup"><span data-stu-id="da684-128">Site Recovery takes advantage of hello Azure network infrastructure that connects regions tookeep RTO and RPO very low.</span></span>
- <span data-ttu-id="da684-129">**Testování**.</span><span class="sxs-lookup"><span data-stu-id="da684-129">**Testing**.</span></span> <span data-ttu-id="da684-130">Cvičení zotavení po havárii můžete spustit s na vyžádání testovací převzetí služeb při selhání podle potřeby, aniž by to ovlivňovalo produkční zatížení nebo probíhající replikace.</span><span class="sxs-lookup"><span data-stu-id="da684-130">You can run disaster-recovery drills with on-demand test failovers, as and when needed, without affecting your production workloads or ongoing replication.</span></span>
- <span data-ttu-id="da684-131">**Plány obnovení**.</span><span class="sxs-lookup"><span data-stu-id="da684-131">**Recovery plans**.</span></span> <span data-ttu-id="da684-132">Můžete použít obnovení plány tooorchestrate převzetí služeb při selhání a navrácení služeb po obnovení hello celé aplikace běžící na několika virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="da684-132">You can use recovery plans tooorchestrate failover and failback of hello entire application running on multiple VMs.</span></span> <span data-ttu-id="da684-133">Funkce plán obnovení Hello má bohaté prvotřídní integrace s runbooky služby Azure automation.</span><span class="sxs-lookup"><span data-stu-id="da684-133">hello recovery plan feature has rich first-class integration with Azure automation runbooks.</span></span>


## <a name="deployment-summary"></a><span data-ttu-id="da684-134">Souhrn nasazení</span><span class="sxs-lookup"><span data-stu-id="da684-134">Deployment summary</span></span>

<span data-ttu-id="da684-135">Zde je uveden seznam, co potřebujete toodo tooset replikaci virtuálních počítačů mezi oblastmi Azure:</span><span class="sxs-lookup"><span data-stu-id="da684-135">Here's a summary of what you need toodo tooset up replication of VMs between Azure regions:</span></span>

1. <span data-ttu-id="da684-136">Vytvořte trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="da684-136">Create a Recovery Services vault.</span></span> <span data-ttu-id="da684-137">Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.</span><span class="sxs-lookup"><span data-stu-id="da684-137">hello vault contains configuration settings and orchestrates replication.</span></span>
2. <span data-ttu-id="da684-138">Povolení replikace pro virtuální počítače Azure hello.</span><span class="sxs-lookup"><span data-stu-id="da684-138">Enable replication for hello Azure VMs.</span></span>
3. <span data-ttu-id="da684-139">Spusťte toomake testovací převzetí služeb při selhání, se, zda vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="da684-139">Run a test failover toomake sure that everything's working as expected.</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="da684-140">Můžete zkontrolovat hello [matici podpory pro replikaci virtuálního počítače Azure](./site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="da684-140">You can check hello [support matrix for Azure VM replication](./site-recovery-support-matrix-azure-to-azure.md).</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="da684-141">Informace o tom, jak tooconfigure hello požadované odchozí připojení k síti pro virtuální počítače Azure Site Recovery replikace najdete v tématu hello [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="da684-141">For information on how tooconfigure hello required network outbound connectivity for Azure VMs for Site Recovery replication, see hello [networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md).</span></span>

### <a name="before-you-start"></a><span data-ttu-id="da684-142">Než začnete</span><span class="sxs-lookup"><span data-stu-id="da684-142">Before you start</span></span>

* <span data-ttu-id="da684-143">Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="da684-143">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>
* <span data-ttu-id="da684-144">Vaše předplatné Azure musí být povoleno toocreate virtuální počítače v cílovém umístění hello, které chcete toouse jako oblast pro obnovení po havárii hello.</span><span class="sxs-lookup"><span data-stu-id="da684-144">Your Azure subscription should be enabled toocreate VMs in hello target location that you want toouse as hello disaster recovery region.</span></span> <span data-ttu-id="da684-145">Obraťte se na podporu tooenable hello požadované kvóty.</span><span class="sxs-lookup"><span data-stu-id="da684-145">Contact support tooenable hello required quota.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="da684-146">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="da684-146">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="da684-147">Doporučujeme vytvořit trezor služeb zotavení hello v hello umístění, kam má tooreplicate vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="da684-147">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="da684-148">Například pokud cílové umístění je hello střed USA, vytvořte trezor hello v **střed USA**.</span><span class="sxs-lookup"><span data-stu-id="da684-148">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="da684-149">Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="da684-149">Enable replication</span></span>

<span data-ttu-id="da684-150">V **trezory služeb zotavení**, klikněte na název trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="da684-150">In **Recovery Services vaults**, click hello vault name.</span></span> <span data-ttu-id="da684-151">V hello trezoru, klikněte na tlačítko hello **+ replikovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="da684-151">In hello vault, click hello **+Replicate** button.</span></span>

### <a name="step-1-configure-hello-source"></a><span data-ttu-id="da684-152">Krok 1.</span><span class="sxs-lookup"><span data-stu-id="da684-152">Step 1.</span></span> <span data-ttu-id="da684-153">Nakonfigurujte zdroj hello</span><span class="sxs-lookup"><span data-stu-id="da684-153">Configure hello source</span></span>
1. <span data-ttu-id="da684-154">V **zdroj**, vyberte **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="da684-154">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="da684-155">V **umístění zdroje**, vyberte zdroj hello oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.</span><span class="sxs-lookup"><span data-stu-id="da684-155">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="da684-156">Model nasazení vyberte hello virtuální počítače: **Resource Manager** nebo **Classic**.</span><span class="sxs-lookup"><span data-stu-id="da684-156">Select hello deployment model of your VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="da684-157">Vyberte hello **zdrojové skupiny prostředků** pro virtuální počítače správce prostředků nebo **Cloudová služba** pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="da684-157">Select hello **Source resource group** for Resource Manager VMs or **cloud service** for classic VMs.</span></span>

    ![Nakonfigurujte zdroj hello](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a><span data-ttu-id="da684-159">Krok 2.</span><span class="sxs-lookup"><span data-stu-id="da684-159">Step 2.</span></span> <span data-ttu-id="da684-160">Vyberte virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="da684-160">Select virtual machines</span></span>

* <span data-ttu-id="da684-161">Vyberte virtuální počítače hello tooreplicate a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="da684-161">Select hello VMs you want tooreplicate, and then click **OK**.</span></span>

    ![Vyberte virtuální počítače](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a><span data-ttu-id="da684-163">Krok 3.</span><span class="sxs-lookup"><span data-stu-id="da684-163">Step 3.</span></span> <span data-ttu-id="da684-164">Konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="da684-164">Configure settings</span></span>

1. <span data-ttu-id="da684-165">Výchozí hello toooverride cíle nastavení a zadejte nastavení hello podle vaší volby, klikněte na tlačítko **přizpůsobit**.</span><span class="sxs-lookup"><span data-stu-id="da684-165">toooverride hello default target settings and specify hello settings of your choice, click **Customize**.</span></span> <span data-ttu-id="da684-166">Další informace najdete v tématu [přizpůsobit cílové prostředky](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span><span class="sxs-lookup"><span data-stu-id="da684-166">For more information, see [Customize target resources](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span></span>

    ![Konfigurace nastavení](./media/site-recovery-azure-to-azure/settings.png)


2. <span data-ttu-id="da684-168">Site Recovery ve výchozím nastavení vytvoří zásadu replikace, která přebírá snímky konzistentní aplikace každé 4 hodiny a uchovává body obnovení po dobu 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="da684-168">By default, Site Recovery creates a replication policy that takes app-consistent snapshots every 4 hours and retains recovery points for 24 hours.</span></span> <span data-ttu-id="da684-169">toocreate zásad s různá nastavení, klikněte na tlačítko **přizpůsobit** další příliš**zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="da684-169">toocreate a policy with different settings, click **Customize** next too**Replication Policy**.</span></span>

    ![Úpravy zásad](./media/site-recovery-azure-to-azure/customize-policy.png)

3. <span data-ttu-id="da684-171">Klikněte na tlačítko toostart zřizování hello cílové prostředky, **vytvoření cílové prostředky**.</span><span class="sxs-lookup"><span data-stu-id="da684-171">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="da684-172">Zřizování trvá minutu.</span><span class="sxs-lookup"><span data-stu-id="da684-172">Provisioning takes a minute or so.</span></span> <span data-ttu-id="da684-173">Nemáte zavřete okno hello při zřizování nebo potřebujete toostart než.</span><span class="sxs-lookup"><span data-stu-id="da684-173">Don't close hello blade during provisioning, or you need toostart over.</span></span>

4. <span data-ttu-id="da684-174">tootrigger replikaci hello vybraný virtuální počítač, klikněte na tlačítko **povolit replikaci**.</span><span class="sxs-lookup"><span data-stu-id="da684-174">tootrigger replication of hello selected VM, click **Enable replication**.</span></span>

5. <span data-ttu-id="da684-175">Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="da684-175">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

6. <span data-ttu-id="da684-176">V **nastavení** > **replikované položky**, můžete zobrazit stav hello virtuálních počítačů a hello probíhá počáteční replikace.</span><span class="sxs-lookup"><span data-stu-id="da684-176">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="da684-177">Klikněte na tlačítko hello virtuálních počítačů toodrill dolů do jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="da684-177">Click hello VM toodrill down into its settings.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="da684-178">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="da684-178">Run a test failover</span></span>

<span data-ttu-id="da684-179">Po nastavení všechno spustit toomake testovací převzetí služeb při selhání, zda vše funguje podle očekávání:</span><span class="sxs-lookup"><span data-stu-id="da684-179">After you set up everything, run a test failover toomake sure everything's working as expected:</span></span>

1. <span data-ttu-id="da684-180">toofail přes jeden počítač, v **nastavení** > **replikované položky**, klikněte na tlačítko hello virtuálních počítačů **+ testovací převzetí služeb při selhání** ikonu.</span><span class="sxs-lookup"><span data-stu-id="da684-180">toofail over a single machine, in **Settings** > **Replicated Items**, click hello VM **+Test Failover** icon.</span></span>

2. <span data-ttu-id="da684-181">plánování toofail přes obnovení, **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na hello plán **testovací převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="da684-181">toofail over a recovery plan, in **Settings** > **Recovery Plans**, right-click hello plan **Test Failover**.</span></span> <span data-ttu-id="da684-182">plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="da684-182">toocreate a recovery plan, [follow these instructions](site-recovery-create-recovery-plans.md).</span></span> 

3. <span data-ttu-id="da684-183">V **testovací převzetí služeb při selhání**, vyberte připojení hello cílový virtuální síť Azure toowhich virtuální počítače Azure po převzetí služeb při selhání hello.</span><span class="sxs-lookup"><span data-stu-id="da684-183">In **Test Failover**, select hello target Azure virtual network toowhich Azure VMs are connected after hello failover occurs.</span></span>

4. <span data-ttu-id="da684-184">toostart hello převzetí služeb při selhání, klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="da684-184">toostart hello failover, click **OK**.</span></span> <span data-ttu-id="da684-185">tootrack průběhu, klikněte na virtuální počítač tooopen hello jeho vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="da684-185">tootrack progress, click hello VM tooopen its properties.</span></span> <span data-ttu-id="da684-186">Nebo můžete kliknout na hello **testovací převzetí služeb při selhání** úlohy v název trezoru hello > **nastavení** > **úlohy** > **úlohy Site Recovery**.</span><span class="sxs-lookup"><span data-stu-id="da684-186">Or you can click hello **Test Failover** job in hello vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>

5. <span data-ttu-id="da684-187">Po hello dokončení převzetí služeb při selhání, hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**.</span><span class="sxs-lookup"><span data-stu-id="da684-187">After hello failover finishes, hello replica Azure machine appears in hello Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="da684-188">Zajistěte, aby byl tento hello virtuálních počítačů hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.</span><span class="sxs-lookup"><span data-stu-id="da684-188">Make sure that hello VM is hello appropriate size, that it's connected toohello appropriate network, and that it's running.</span></span>

6. <span data-ttu-id="da684-189">Klikněte na tlačítko toodelete hello virtuálních počítačů, které byly vytvořeny během hello testovací převzetí služeb při selhání, **vyčistit testovací převzetí služeb při selhání** na hello replikované položky nebo hello plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="da684-189">toodelete hello VMs that were created during hello test failover, click **Cleanup test failover** on hello replicated item or hello recovery plan.</span></span> <span data-ttu-id="da684-190">V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="da684-190">In **Notes**, record and save any observations associated with hello test failover.</span></span> 

<span data-ttu-id="da684-191">[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="da684-191">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>


## <a name="next-steps"></a><span data-ttu-id="da684-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da684-192">Next steps</span></span>

<span data-ttu-id="da684-193">Po testovací nasazení hello:</span><span class="sxs-lookup"><span data-stu-id="da684-193">After you test hello deployment:</span></span>

- <span data-ttu-id="da684-194">[Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.</span><span class="sxs-lookup"><span data-stu-id="da684-194">[Learn more](site-recovery-failover.md) about different types of failovers and how toorun them.</span></span>
- <span data-ttu-id="da684-195">Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) tooreduce RTO.</span><span class="sxs-lookup"><span data-stu-id="da684-195">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
