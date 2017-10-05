---
title: Replikovat aplikace (Azure do Azure) | Microsoft Docs
description: "Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných v jedné oblasti Azure do jiné oblasti v Azure."
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
ms.openlocfilehash: f9f97cf840b722c8cfee169dd1640e0682f287ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a><span data-ttu-id="9f03a-103">Jiné oblasti Azure replikovat virtuální počítače Azure</span><span class="sxs-lookup"><span data-stu-id="9f03a-103">Replicate Azure virtual machines to another Azure region</span></span>



>[!NOTE]
>
> <span data-ttu-id="9f03a-104">Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="9f03a-104">Site Recovery replication for Azure virtual machines is currently in preview.</span></span>

<span data-ttu-id="9f03a-105">Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných v jedné oblasti Azure jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="9f03a-105">This article describes how to set up replication of virtual machines running in one Azure region to another Azure region.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f03a-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f03a-106">Prerequisites</span></span>

* <span data-ttu-id="9f03a-107">Článek předpokládá, že již víte o Site Recovery a trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9f03a-107">The article assumes that you already know about Site Recovery and Recovery Services Vault.</span></span> <span data-ttu-id="9f03a-108">Je potřeba mít po předem trezoru služeb zotavení vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9f03a-108">You need to have a 'Recovery services vault' pre created.</span></span>

    >[!NOTE]
    >
    > <span data-ttu-id="9f03a-109">Doporučuje se vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9f03a-109">It is recommended that you create the 'Recovery services vault' in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="9f03a-110">Například pokud cílové umístění, střed USA", vytvořte trezor v, střed USA".</span><span class="sxs-lookup"><span data-stu-id="9f03a-110">For example, if your target location is 'Central US', create vault in 'Central US'.</span></span>

* <span data-ttu-id="9f03a-111">Pokud používáte k řízení přístupu k odchozí připojení k Internetu na virtuálních počítačích Azure, pravidla skupiny zabezpečení sítě (NSG) nebo proxy server brány firewall, ujistěte se, že je seznam povolených adres požadované adresy URL nebo IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9f03a-111">If you are using Network Security Groups (NSG) rules or firewall proxy to control access to outbound internet connectivity on the Azure VMs, ensure that you whitelist the required URLs or IPs.</span></span> <span data-ttu-id="9f03a-112">Odkazovat na [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9f03a-112">Refer to [Networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md) for more details.</span></span>

* <span data-ttu-id="9f03a-113">Pokud máte ExpressRoute nebo VPN připojení mezi místními a umístění zdroje v Azure, postupujte podle [obnovení lokality požadavky pro Azure ExpressRoute místní nebo konfiguraci sítě VPN](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-113">If you have an ExpressRoute or a VPN connection between on-premises and the source location in Azure, follow [Site Recovery Considerations for Azure to on-premises ExpressRoute / VPN configuration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.</span></span>

* <span data-ttu-id="9f03a-114">Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9f03a-114">Your Azure user account needs to have certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of an Azure virtual machine.</span></span>

* <span data-ttu-id="9f03a-115">Lze povolit vašeho předplatného Azure k vytvoření virtuálních počítačů v cílovém umístění, které chcete použít jako oblast zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="9f03a-115">Your Azure subscription should be enabled to create VMs in the target location you want to use as DR region.</span></span> <span data-ttu-id="9f03a-116">Obraťte se na podporu, aby umožnil požadované kvótu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-116">You can contact support to enable the required quota.</span></span>

## <a name="enable-replication-from-azure-site-recovery-vault"></a><span data-ttu-id="9f03a-117">Povolit replikaci z trezoru Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="9f03a-117">Enable replication from Azure Site Recovery vault</span></span>
<span data-ttu-id="9f03a-118">Pro tento obrázek jsme bude replikovat virtuální počítače spuštěné ve východní Asie Azure umístění, do umístění, jihovýchodní Asie '.</span><span class="sxs-lookup"><span data-stu-id="9f03a-118">For this illustration, we will replicate VMs running  in the ‘East Asia’ Azure location to the ‘South East Asia’ location.</span></span> <span data-ttu-id="9f03a-119">Kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9f03a-119">The steps are as follows:</span></span>

 <span data-ttu-id="9f03a-120">Klikněte na tlačítko **+ replikovat** v trezoru povolíte replikaci pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9f03a-120">Click **+Replicate** in the vault to enable replication for the virtual machines.</span></span>

1. <span data-ttu-id="9f03a-121">**Zdroj:** odkazuje na bod původu počítačů, které v tomto případě je **Azure**.</span><span class="sxs-lookup"><span data-stu-id="9f03a-121">**Source:** It refers to the point of origin of the machines which in this case is **Azure**.</span></span>

2. <span data-ttu-id="9f03a-122">**Umístění zdroje:** je oblast Azure, ze které chcete chránit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="9f03a-122">**Source location:** It is the Azure region from where you want to protect your virtual machines.</span></span> <span data-ttu-id="9f03a-123">Pro tento obrázek bude umístění zdroje, východní Asie.</span><span class="sxs-lookup"><span data-stu-id="9f03a-123">For this illustration, the source location will be 'East Asia'</span></span>

3. <span data-ttu-id="9f03a-124">**Model nasazení:** odkazuje na modelu nasazení Azure zdrojový počítač.</span><span class="sxs-lookup"><span data-stu-id="9f03a-124">**Deployment model:** It refers to the Azure deployment model of the source machines.</span></span> <span data-ttu-id="9f03a-125">Můžete vybrat buď classic nebo resource manager a počítače patřící do určité modelu budou uvedené pro ochranu v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="9f03a-125">You can select either classic or resource manager and machines belonging to the specific model will be listed for protection in the next step.</span></span>

      >[!NOTE]
      >
      > <span data-ttu-id="9f03a-126">Můžete replikovat klasické virtuální počítač a obnovit jako virtuální počítač s classic.</span><span class="sxs-lookup"><span data-stu-id="9f03a-126">You can only replicate a classic virtual machine and recover it as a classic virtual machine.</span></span> <span data-ttu-id="9f03a-127">Nelze obnovit jako virtuální počítač Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9f03a-127">You cannot recover it as a Resource Manager virtual machine.</span></span>

4. <span data-ttu-id="9f03a-128">**Skupina prostředků:** je skupina prostředků, do které patří zdroj virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9f03a-128">**Resource Group:** It’s the resource group to which your source virtual machines belong.</span></span> <span data-ttu-id="9f03a-129">Objeví se všechny virtuální počítače v části s vybranou skupinou prostředků pro ochranu v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="9f03a-129">All the VMs under the selected resource group will be listed for protection in the next step.</span></span>

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

<span data-ttu-id="9f03a-131">V **virtuální počítače > vyberte virtuální počítače**, klikněte na tlačítko a vyberte každý počítač, který chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="9f03a-131">In **Virtual Machines > Select virtual machines**, click and select each machine you want to replicate.</span></span> <span data-ttu-id="9f03a-132">Můžete vybrat pouze počítače, pro které je možné povolit replikaci.</span><span class="sxs-lookup"><span data-stu-id="9f03a-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="9f03a-133">Pak klikněte na OK.</span><span class="sxs-lookup"><span data-stu-id="9f03a-133">Then click OK.</span></span>
    <span data-ttu-id="9f03a-134">![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span><span class="sxs-lookup"><span data-stu-id="9f03a-134">![Enable replication](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span></span>


<span data-ttu-id="9f03a-135">V části nastavení můžete nakonfigurovat vlastnosti cílové lokality</span><span class="sxs-lookup"><span data-stu-id="9f03a-135">Under Settings section you can configure target site properties</span></span>

1. <span data-ttu-id="9f03a-136">**Cílové umístění:** Toto je umístění, kde bude replikován zdrojová data virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9f03a-136">**Target Location:**  This is the location where your source virtual machine data will be replicated.</span></span> <span data-ttu-id="9f03a-137">V závislosti na vaší umístění vybrané počítače se Site Recovery poskytuje seznam vhodný cílové oblasti.</span><span class="sxs-lookup"><span data-stu-id="9f03a-137">Depending upon your selected machines location, Site Recovery will provide you the list of suitable target regions.</span></span>

    > [!TIP]
    > <span data-ttu-id="9f03a-138">Doporučuje se zachovat cílové umístění stejné od vaší obnovení služeb trezoru.</span><span class="sxs-lookup"><span data-stu-id="9f03a-138">It is recommended to keep target location same as of your recovery services vault.</span></span>

2. <span data-ttu-id="9f03a-139">**Cílová skupina prostředků:** je skupina prostředků, na kterém jsou všechny bude patřit replikovaných virtuálních počítačů. Ve výchozím nastavení automatické obnovení systému vytvoří novou skupinu prostředků v cílová oblast s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="9f03a-139">**Target resource group :** It is the resource group to which all your replicated virtual machines will belong.By default ASR will create a new resource group in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="9f03a-140">V případě, že skupiny prostředků vytvořené automatické obnovení systému již existuje, bude znovu. Můžete také přizpůsobit, jak je znázorněno v následující části.</span><span class="sxs-lookup"><span data-stu-id="9f03a-140">In case resource group created by ASR already exist, it will be reused.You can also choose to customize it as shown in the section below.</span></span>    
3. <span data-ttu-id="9f03a-141">**Cílová virtuální síť:** ve výchozím nastavení, automatické obnovení systému vytvoří novou virtuální síť v oblasti cíl s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="9f03a-141">**Target Virtual Network:** By default, ASR will create a new virtual network in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="9f03a-142">To budou mapována na zdrojové síti a bude se používat pro všechny budoucí ochranu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-142">This will be mapped to your source network and will be used for any future protection.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9f03a-143">[Sítě v podrobnostech](site-recovery-network-mapping-azure-to-azure.md) Další informace o mapování sítě.</span><span class="sxs-lookup"><span data-stu-id="9f03a-143">[Check networking details](site-recovery-network-mapping-azure-to-azure.md) to know more about network mapping.</span></span>

4. <span data-ttu-id="9f03a-144">**Cílové úložiště účtů:** ve výchozím nastavení, automatické obnovení systému se vytvoří nový účet cílového úložiště mimicking konfiguraci zdrojového virtuálního počítače úložiště.</span><span class="sxs-lookup"><span data-stu-id="9f03a-144">**Target Storage accounts:** By default, ASR will create the new target storage account mimicking your source VM storage configuration.</span></span> <span data-ttu-id="9f03a-145">V případě, že účet úložiště, které jsou vytvořené automatické obnovení systému již existuje, bude znovu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-145">In case storage account created by ASR already exist, it will be reused.</span></span>

5. <span data-ttu-id="9f03a-146">**Účty úložiště do mezipaměti:** automatické obnovení systému potřebuje účet úložiště s názvem úložiště mezipaměti v oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="9f03a-146">**Cache Storage accounts:** ASR needs extra storage account called cache storage in the source region.</span></span> <span data-ttu-id="9f03a-147">Všechny změny děje na zdrojové virtuální počítače jsou sledovány a odesílat k účtu úložiště mezipaměti před replikaci do cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="9f03a-147">All the changes happening on the source VMs are tracked and sent to cache storage account before replicating those to the target location.</span></span>

6. <span data-ttu-id="9f03a-148">**Skupina dostupnosti:** ve výchozím nastavení, vytvoří nástroj Automatické obnovení systému novou skupinou dostupnosti ve cílová oblast s názvem, který má příponu "Automatické obnovení systému".</span><span class="sxs-lookup"><span data-stu-id="9f03a-148">**Availability set :** By default, ASR will create a new availability set in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="9f03a-149">V případě, že skupina dostupnosti vytvořené automatické obnovení systému již existuje, bude znovu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-149">In case availability set created by ASR already exist, it will be reused.</span></span>

7.  <span data-ttu-id="9f03a-150">**Zásady replikace:** definuje nastavení pro obnovení bodu uchování historie a aplikace snímky konzistentní s četnost.</span><span class="sxs-lookup"><span data-stu-id="9f03a-150">**Replication Policy:** It defines the settings for recovery point retention history and app consistent snapshot frequency.</span></span> <span data-ttu-id="9f03a-151">Ve výchozím nastavení bude automatické obnovení systému vytvořte novou zásadu replikace s výchozím nastavením: 24 hodin se pro uchování bodu obnovení a ' 60 minut frekvence snímky konzistentní s aplikací.</span><span class="sxs-lookup"><span data-stu-id="9f03a-151">By default, ASR will create a new replication policy with default settings of ‘24 hours’ for recovery point retention and ’60 minutes’ for app consistent snapshot frequency.</span></span>

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a><span data-ttu-id="9f03a-153">Přizpůsobení cílové prostředky</span><span class="sxs-lookup"><span data-stu-id="9f03a-153">Customize target resources</span></span>

<span data-ttu-id="9f03a-154">V případě, že chcete změnit výchozí nastavení používané automatické obnovení systému, můžete změnit nastavení na základě potřeb.</span><span class="sxs-lookup"><span data-stu-id="9f03a-154">In case you want to change the defaults used by ASR, you can change the settings based on your needs.</span></span>

1. <span data-ttu-id="9f03a-155">**Přizpůsobení:** klikněte na něj chcete-li změnit výchozí hodnoty používané automatické obnovení systému.</span><span class="sxs-lookup"><span data-stu-id="9f03a-155">**Customize:** Click it to change the defaults used by ASR.</span></span>

2. <span data-ttu-id="9f03a-156">**Cílová skupina prostředků:** skupiny prostředků můžete vybrat ze seznamu všech skupin prostředků existující v cílovém umístění v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="9f03a-156">**Target resource group :**  You can select the resource group from the list of all the resource groups existing in the target location within the subscription.</span></span>

3. <span data-ttu-id="9f03a-157">**Cílová virtuální síť:** seznamu všechny virtuální sítě můžete najít v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="9f03a-157">**Target Virtual Network:** You can find the list of all the virtual network in the target location.</span></span>

4. <span data-ttu-id="9f03a-158">**Skupina dostupnosti:** nastavení sad dostupnosti můžete přidat pouze pro virtuální počítače, které jsou součástí dostupnosti v oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="9f03a-158">**Availability set :** You can only add availability sets settings to the virtual machines which are a part of availability in source region.</span></span>

5. <span data-ttu-id="9f03a-159">**Cílem účtů úložiště:**</span><span class="sxs-lookup"><span data-stu-id="9f03a-159">**Target Storage accounts:**</span></span>

<span data-ttu-id="9f03a-160">![Povolit replikaci](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klikněte na **vytvořit cílový prostředek** a zapnout replikaci</span><span class="sxs-lookup"><span data-stu-id="9f03a-160">![Enable replication](./media/site-recovery-replicate-azure-to-azure/customize.PNG) Click on **Create target resource** and Enable Replication</span></span>


<span data-ttu-id="9f03a-161">Jakmile jsou chráněné virtuální počítače můžete zkontrolovat stav stavu virtuálních počítačů v rámci **replikované položky**</span><span class="sxs-lookup"><span data-stu-id="9f03a-161">Once virtual machines are protected you can check the status of VMs health under **Replicated items**</span></span>

>[!NOTE]
><span data-ttu-id="9f03a-162">V době počáteční replikace existuje může možnost, že stav trvá určitou dobu k aktualizaci a nevidíte průběh nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="9f03a-162">During the time of initial replication there could a possibility that status takes time to refresh and you don't see progress for some time.</span></span> <span data-ttu-id="9f03a-163">Kliknutím na tlačítko Aktualizovat nahoře v okně a získat nejnovější stav.</span><span class="sxs-lookup"><span data-stu-id="9f03a-163">You can click the Refresh button on the top of the blade to get the latest status.</span></span>
>

![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a><span data-ttu-id="9f03a-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f03a-165">Next steps</span></span>
- <span data-ttu-id="9f03a-166">[Další informace](site-recovery-test-failover-to-azure.md) o spuštění převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9f03a-166">[Learn more](site-recovery-test-failover-to-azure.md) about running a test failover.</span></span>
- <span data-ttu-id="9f03a-167">[Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak je spouštět.</span><span class="sxs-lookup"><span data-stu-id="9f03a-167">[Learn more](site-recovery-failover.md) about different types of failovers, and how to run them.</span></span>
- <span data-ttu-id="9f03a-168">Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) ke snížení RTO.</span><span class="sxs-lookup"><span data-stu-id="9f03a-168">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) to reduce RTO.</span></span>
- <span data-ttu-id="9f03a-169">Další informace o [opětovnou ochranu virtuálních počítačů Azure](site-recovery-how-to-reprotect.md) po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9f03a-169">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>
