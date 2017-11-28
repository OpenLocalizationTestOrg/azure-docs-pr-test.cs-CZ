---
title: aplikace aaaReplicate (Azure tooAzure) | Microsoft Docs
description: "Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná v jedné oblasti Azure příliš jiné oblasti v Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a><span data-ttu-id="74604-103">Replikovat virtuální počítače Azure tooanother oblast Azure</span><span class="sxs-lookup"><span data-stu-id="74604-103">Replicate Azure virtual machines tooanother Azure region</span></span>



>[!NOTE]
>
> <span data-ttu-id="74604-104">Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="74604-104">Site Recovery replication for Azure virtual machines is currently in preview.</span></span>

<span data-ttu-id="74604-105">Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná v jedné oblasti Azure tooanother oblast Azure.</span><span class="sxs-lookup"><span data-stu-id="74604-105">This article describes how tooset up replication of virtual machines running in one Azure region tooanother Azure region.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74604-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74604-106">Prerequisites</span></span>

* <span data-ttu-id="74604-107">Hello článek předpokládá, že již víte o Site Recovery a trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="74604-107">hello article assumes that you already know about Site Recovery and Recovery Services Vault.</span></span> <span data-ttu-id="74604-108">Je nutné toohave po předem trezoru služeb zotavení vytvořili.</span><span class="sxs-lookup"><span data-stu-id="74604-108">You need toohave a 'Recovery services vault' pre created.</span></span>

    >[!NOTE]
    >
    > <span data-ttu-id="74604-109">Doporučuje se vytvořit hello trezoru služeb zotavení v hello umístění, kam má tooreplicate vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="74604-109">It is recommended that you create hello 'Recovery services vault' in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="74604-110">Například pokud cílové umístění, střed USA", vytvořte trezor v, střed USA".</span><span class="sxs-lookup"><span data-stu-id="74604-110">For example, if your target location is 'Central US', create vault in 'Central US'.</span></span>

* <span data-ttu-id="74604-111">Pokud používáte pravidel skupiny zabezpečení sítě (NSG) nebo brány firewall proxy toocontrol přístup toooutbound připojení k Internetu na hello virtuálních počítačích Azure, zajistěte, že je seznam povolených adres hello požadované adresy URL nebo IP adresy.</span><span class="sxs-lookup"><span data-stu-id="74604-111">If you are using Network Security Groups (NSG) rules or firewall proxy toocontrol access toooutbound internet connectivity on hello Azure VMs, ensure that you whitelist hello required URLs or IPs.</span></span> <span data-ttu-id="74604-112">Odkazovat příliš[sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="74604-112">Refer too[Networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md) for more details.</span></span>

* <span data-ttu-id="74604-113">Pokud máte ExpressRoute nebo VPN připojení mezi místními a hello zdrojové umístění v Azure, postupujte podle [aspekty obnovení lokality pro místní tooon Azure ExpressRoute nebo konfiguraci sítě VPN](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="74604-113">If you have an ExpressRoute or a VPN connection between on-premises and hello source location in Azure, follow [Site Recovery Considerations for Azure tooon-premises ExpressRoute / VPN configuration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.</span></span>

* <span data-ttu-id="74604-114">Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="74604-114">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>

* <span data-ttu-id="74604-115">Vaše předplatné Azure musí být povoleno toocreate virtuální počítače v cílovém umístění hello chcete toouse jako oblast zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="74604-115">Your Azure subscription should be enabled toocreate VMs in hello target location you want toouse as DR region.</span></span> <span data-ttu-id="74604-116">Obraťte se na podporu tooenable hello požadované kvóty.</span><span class="sxs-lookup"><span data-stu-id="74604-116">You can contact support tooenable hello required quota.</span></span>

## <a name="enable-replication-from-azure-site-recovery-vault"></a><span data-ttu-id="74604-117">Povolit replikaci z trezoru Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="74604-117">Enable replication from Azure Site Recovery vault</span></span>
<span data-ttu-id="74604-118">Pro tento obrázek jsme bude replikovat virtuální počítače spuštěné v toohello umístění Azure, východní Asie, hello, jihovýchodní Asie, umístění.</span><span class="sxs-lookup"><span data-stu-id="74604-118">For this illustration, we will replicate VMs running  in hello ‘East Asia’ Azure location toohello ‘South East Asia’ location.</span></span> <span data-ttu-id="74604-119">Hello kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="74604-119">hello steps are as follows:</span></span>

 <span data-ttu-id="74604-120">Klikněte na tlačítko **+ replikovat** v hello trezoru tooenable replikace pro virtuální počítače hello.</span><span class="sxs-lookup"><span data-stu-id="74604-120">Click **+Replicate** in hello vault tooenable replication for hello virtual machines.</span></span>

1. <span data-ttu-id="74604-121">**Zdroj:** odkazuje toohello bod počátku hello počítače, který v tomto případě je **Azure**.</span><span class="sxs-lookup"><span data-stu-id="74604-121">**Source:** It refers toohello point of origin of hello machines which in this case is **Azure**.</span></span>

2. <span data-ttu-id="74604-122">**Umístění zdroje:** je hello oblast Azure, ze kterého má tooprotect virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="74604-122">**Source location:** It is hello Azure region from where you want tooprotect your virtual machines.</span></span> <span data-ttu-id="74604-123">Pro tento obrázek umístění zdroje hello budou, východní Asie.</span><span class="sxs-lookup"><span data-stu-id="74604-123">For this illustration, hello source location will be 'East Asia'</span></span>

3. <span data-ttu-id="74604-124">**Model nasazení:** odkazuje model nasazení Azure toohello hello zdrojového počítače.</span><span class="sxs-lookup"><span data-stu-id="74604-124">**Deployment model:** It refers toohello Azure deployment model of hello source machines.</span></span> <span data-ttu-id="74604-125">Můžete vybrat buď classic nebo resource manager a patřící toohello konkrétní model počítače budou uvedené pro ochranu v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="74604-125">You can select either classic or resource manager and machines belonging toohello specific model will be listed for protection in hello next step.</span></span>

      >[!NOTE]
      >
      > <span data-ttu-id="74604-126">Můžete replikovat klasické virtuální počítač a obnovit jako virtuální počítač s classic.</span><span class="sxs-lookup"><span data-stu-id="74604-126">You can only replicate a classic virtual machine and recover it as a classic virtual machine.</span></span> <span data-ttu-id="74604-127">Nelze obnovit jako virtuální počítač Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="74604-127">You cannot recover it as a Resource Manager virtual machine.</span></span>

4. <span data-ttu-id="74604-128">**Skupina prostředků:** ho je toowhich skupiny prostředků hello patří zdroj virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="74604-128">**Resource Group:** It’s hello resource group toowhich your source virtual machines belong.</span></span> <span data-ttu-id="74604-129">Pro ochranu v dalším kroku hello se objeví hello všechny virtuální počítače v části hello vybranou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="74604-129">All hello VMs under hello selected resource group will be listed for protection in hello next step.</span></span>

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

<span data-ttu-id="74604-131">V **virtuální počítače > vyberte virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="74604-131">In **Virtual Machines > Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="74604-132">Můžete vybrat pouze počítače, pro které je možné povolit replikaci.</span><span class="sxs-lookup"><span data-stu-id="74604-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="74604-133">Pak klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="74604-133">Then click OK.</span></span>
    <span data-ttu-id="74604-134">![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span><span class="sxs-lookup"><span data-stu-id="74604-134">![Enable replication](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span></span>


<span data-ttu-id="74604-135">V části nastavení můžete nakonfigurovat vlastnosti cílové lokality</span><span class="sxs-lookup"><span data-stu-id="74604-135">Under Settings section you can configure target site properties</span></span>

1. <span data-ttu-id="74604-136">**Cílové umístění:** jde hello umístění, kde bude replikován zdrojová data virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="74604-136">**Target Location:**  This is hello location where your source virtual machine data will be replicated.</span></span> <span data-ttu-id="74604-137">V závislosti na vaší umístění vybrané počítače se Site Recovery zajistí, že hello seznam vhodnou cílovou oblastí.</span><span class="sxs-lookup"><span data-stu-id="74604-137">Depending upon your selected machines location, Site Recovery will provide you hello list of suitable target regions.</span></span>

    > [!TIP]
    > <span data-ttu-id="74604-138">Je doporučeno tookeep cílové umístění stejné od vaší obnovení služeb trezoru.</span><span class="sxs-lookup"><span data-stu-id="74604-138">It is recommended tookeep target location same as of your recovery services vault.</span></span>

2. <span data-ttu-id="74604-139">**Cílová skupina prostředků:** je toowhich skupiny prostředků hello bude patřit všechny replikované virtuální počítače. Ve výchozím nastavení automatické obnovení systému vytvoří novou skupinu prostředků v hello cílová oblast s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="74604-139">**Target resource group :** It is hello resource group toowhich all your replicated virtual machines will belong.By default ASR will create a new resource group in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="74604-140">V případě, že skupiny prostředků vytvořené automatické obnovení systému již existuje, bude znovu. Můžete také toocustomize ho jak je znázorněno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="74604-140">In case resource group created by ASR already exist, it will be reused.You can also choose toocustomize it as shown in hello section below.</span></span>    
3. <span data-ttu-id="74604-141">**Cílová virtuální síť:** ve výchozím nastavení, automatické obnovení systému vytvoří nové virtuální sítě v hello cílová oblast s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="74604-141">**Target Virtual Network:** By default, ASR will create a new virtual network in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="74604-142">To bude namapované tooyour zdrojové síti a bude se používat pro všechny budoucí ochranu.</span><span class="sxs-lookup"><span data-stu-id="74604-142">This will be mapped tooyour source network and will be used for any future protection.</span></span>

    > [!NOTE]
    > <span data-ttu-id="74604-143">[Sítě v podrobnostech](site-recovery-network-mapping-azure-to-azure.md) tooknow více informací o mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="74604-143">[Check networking details](site-recovery-network-mapping-azure-to-azure.md) tooknow more about network mapping.</span></span>

4. <span data-ttu-id="74604-144">**Cílové úložiště účtů:** ve výchozím nastavení, automatické obnovení systému vytvoří hello nový cílový účet úložiště mimicking konfiguraci zdrojového virtuálního počítače úložiště.</span><span class="sxs-lookup"><span data-stu-id="74604-144">**Target Storage accounts:** By default, ASR will create hello new target storage account mimicking your source VM storage configuration.</span></span> <span data-ttu-id="74604-145">V případě, že účet úložiště, které jsou vytvořené automatické obnovení systému již existuje, bude znovu.</span><span class="sxs-lookup"><span data-stu-id="74604-145">In case storage account created by ASR already exist, it will be reused.</span></span>

5. <span data-ttu-id="74604-146">**Účty úložiště do mezipaměti:** automatické obnovení systému potřebuje účet úložiště s názvem úložiště mezipaměti v oblasti zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="74604-146">**Cache Storage accounts:** ASR needs extra storage account called cache storage in hello source region.</span></span> <span data-ttu-id="74604-147">Všechny hello změny děje na hello zdrojové virtuální počítače jsou sledovány a odesílat účet úložiště toocache před replikace těchto toohello cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="74604-147">All hello changes happening on hello source VMs are tracked and sent toocache storage account before replicating those toohello target location.</span></span>

6. <span data-ttu-id="74604-148">**Skupina dostupnosti:** ve výchozím nastavení, vytvoří nástroj Automatické obnovení systému novou skupinou dostupnosti ve hello cílová oblast s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="74604-148">**Availability set :** By default, ASR will create a new availability set in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="74604-149">V případě, že skupina dostupnosti vytvořené automatické obnovení systému již existuje, bude znovu.</span><span class="sxs-lookup"><span data-stu-id="74604-149">In case availability set created by ASR already exist, it will be reused.</span></span>

7.  <span data-ttu-id="74604-150">**Zásady replikace:** definuje hello nastavení pro obnovení bodu uchování historie a aplikace snímky konzistentní s četnost.</span><span class="sxs-lookup"><span data-stu-id="74604-150">**Replication Policy:** It defines hello settings for recovery point retention history and app consistent snapshot frequency.</span></span> <span data-ttu-id="74604-151">Ve výchozím nastavení bude automatické obnovení systému vytvořte novou zásadu replikace s výchozím nastavením: 24 hodin se pro uchování bodu obnovení a ' 60 minut frekvence snímky konzistentní s aplikací.</span><span class="sxs-lookup"><span data-stu-id="74604-151">By default, ASR will create a new replication policy with default settings of ‘24 hours’ for recovery point retention and ’60 minutes’ for app consistent snapshot frequency.</span></span>

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a><span data-ttu-id="74604-153">Přizpůsobení cílové prostředky</span><span class="sxs-lookup"><span data-stu-id="74604-153">Customize target resources</span></span>

<span data-ttu-id="74604-154">V případě, že chcete toochange hello výchozí hodnoty používané automatické obnovení systému, můžete změnit nastavení hello na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="74604-154">In case you want toochange hello defaults used by ASR, you can change hello settings based on your needs.</span></span>

1. <span data-ttu-id="74604-155">**Přizpůsobení:** klikněte na něj toochange hello výchozí hodnoty používané automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="74604-155">**Customize:** Click it toochange hello defaults used by ASR.</span></span>

2. <span data-ttu-id="74604-156">**Cílová skupina prostředků:** můžete vybrat skupinu prostředků hello z hello seznam všech skupin prostředků hello existující v cílovém umístění hello v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="74604-156">**Target resource group :**  You can select hello resource group from hello list of all hello resource groups existing in hello target location within hello subscription.</span></span>

3. <span data-ttu-id="74604-157">**Cílová virtuální síť:** hello seznam všech hello virtuální sítě můžete najít v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="74604-157">**Target Virtual Network:** You can find hello list of all hello virtual network in hello target location.</span></span>

4. <span data-ttu-id="74604-158">**Skupina dostupnosti:** lze přidat pouze dostupnost sady nastavení toohello virtuální počítače, které jsou součástí dostupnosti v oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="74604-158">**Availability set :** You can only add availability sets settings toohello virtual machines which are a part of availability in source region.</span></span>

5. <span data-ttu-id="74604-159">**Cílem účtů úložiště:**</span><span class="sxs-lookup"><span data-stu-id="74604-159">**Target Storage accounts:**</span></span>

<span data-ttu-id="74604-160">![Povolit replikaci](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klikněte na **vytvořit cílový prostředek** a zapnout replikaci</span><span class="sxs-lookup"><span data-stu-id="74604-160">![Enable replication](./media/site-recovery-replicate-azure-to-azure/customize.PNG) Click on **Create target resource** and Enable Replication</span></span>


<span data-ttu-id="74604-161">Jakmile jsou virtuální počítače chráněné můžete zkontrolovat stav hello stavu virtuální počítače v části **replikované položky**</span><span class="sxs-lookup"><span data-stu-id="74604-161">Once virtual machines are protected you can check hello status of VMs health under **Replicated items**</span></span>

>[!NOTE]
><span data-ttu-id="74604-162">Během hello může době počáteční replikace existuje možnost, že stav trvá toorefresh čas a nevidíte průběh nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="74604-162">During hello time of initial replication there could a possibility that status takes time toorefresh and you don't see progress for some time.</span></span> <span data-ttu-id="74604-163">Můžete klikněte na tlačítko Aktualizovat hello hello nahoře hello okno tooget hello nejnovější stavu.</span><span class="sxs-lookup"><span data-stu-id="74604-163">You can click hello Refresh button on hello top of hello blade tooget hello latest status.</span></span>
>

![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a><span data-ttu-id="74604-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74604-165">Next steps</span></span>
- <span data-ttu-id="74604-166">[Další informace](site-recovery-test-failover-to-azure.md) o spuštění převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="74604-166">[Learn more](site-recovery-test-failover-to-azure.md) about running a test failover.</span></span>
- <span data-ttu-id="74604-167">[Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.</span><span class="sxs-lookup"><span data-stu-id="74604-167">[Learn more](site-recovery-failover.md) about different types of failovers, and how toorun them.</span></span>
- <span data-ttu-id="74604-168">Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) tooreduce RTO.</span><span class="sxs-lookup"><span data-stu-id="74604-168">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
- <span data-ttu-id="74604-169">Další informace o [opětovnou ochranu virtuálních počítačů Azure](site-recovery-how-to-reprotect.md) po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="74604-169">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>
