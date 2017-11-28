---
title: "aaaReplicate vícevrstvé Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooprotect a obnovit Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a><span data-ttu-id="651c2-103">Replikovat vícevrstvé Citrix XenApp a XenDesktop nasazení pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="651c2-103">Replicate a multi-tier Citrix XenApp and XenDesktop deployment using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="651c2-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="651c2-104">Overview</span></span>

<span data-ttu-id="651c2-105">Citrix XenDesktop je řešení virtualizace plochy, které nabízí plochy a aplikace jako ondemand služby tooany uživatelé kdekoli.</span><span class="sxs-lookup"><span data-stu-id="651c2-105">Citrix XenDesktop is a desktop virtualization solution that delivers desktops and applications as an ondemand service tooany user, anywhere.</span></span> <span data-ttu-id="651c2-106">S technologií doručení FlexCast XenDesktop můžete rychle a bezpečně poskytovat aplikace a toousers stolních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651c2-106">With FlexCast delivery technology, XenDesktop can quickly and securely deliver applications and desktops toousers.</span></span>
<span data-ttu-id="651c2-107">V současné době Citrix XenApp neposkytuje žádné po havárii možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-107">Today, Citrix XenApp does not provide any disaster recovery capabilities.</span></span>

<span data-ttu-id="651c2-108">Řešení pro zotavení po havárii funkční, by měl Povolit modelování plány obnovení kolem hello výše architektury komplexní aplikace a také mít hello možnost tooadd přizpůsobit kroky toohandle aplikace mapování mezi různými úrovněmi proto poskytování jedním kliknutím zda snímek řešení v případě havárie úvodní tooa hello nižší RTO.</span><span class="sxs-lookup"><span data-stu-id="651c2-108">A good disaster recovery solution, should allow modeling of recovery plans around hello above complex application architectures and also have hello ability tooadd customized steps toohandle application mappings between various tiers hence providing a single-click sure shot solution in hello event of a disaster leading tooa lower RTO.</span></span>

<span data-ttu-id="651c2-109">Tento dokument obsahuje podrobné pokyny pro vytváření řešení zotavení po havárii pro místní nasazení Citrix XenApp na Hyper-V a VMware vSphere platformách.</span><span class="sxs-lookup"><span data-stu-id="651c2-109">This document provides a step-by-step guidance for building a disaster recovery solution for your on-premises Citrix XenApp deployments on Hyper-V and VMware vSphere platforms.</span></span> <span data-ttu-id="651c2-110">Tento dokument také popisuje, jak tooperform testovací převzetí služeb při selhání (postupu zotavení po havárii) a tooAzure neplánované převzetí služeb při selhání pomocí plánů obnovení, hello podporované konfigurace a požadavky.</span><span class="sxs-lookup"><span data-stu-id="651c2-110">This document also describes how tooperform a test failover(disaster recovery drill) and unplanned failover tooAzure using recovery plans, hello supported configurations and prerequisites.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="651c2-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="651c2-111">Prerequisites</span></span>

<span data-ttu-id="651c2-112">Než začnete, ujistěte se, že rozumíte hello následující:</span><span class="sxs-lookup"><span data-stu-id="651c2-112">Before you start, make sure you understand hello following:</span></span>

1. [<span data-ttu-id="651c2-113">Replikaci virtuálního počítače tooAzure</span><span class="sxs-lookup"><span data-stu-id="651c2-113">Replicating a virtual machine tooAzure</span></span>](site-recovery-vmware-to-azure.md)
1. <span data-ttu-id="651c2-114">Jak příliš[návrh k síti pro obnovení](site-recovery-network-design.md)</span><span class="sxs-lookup"><span data-stu-id="651c2-114">How too[design a recovery network](site-recovery-network-design.md)</span></span>
1. [<span data-ttu-id="651c2-115">Provádění tooAzure testovací převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="651c2-115">Doing a test failover tooAzure</span></span>](site-recovery-test-failover-to-azure.md)
1. [<span data-ttu-id="651c2-116">Provádění tooAzure převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="651c2-116">Doing a failover tooAzure</span></span>](site-recovery-failover.md)
1. <span data-ttu-id="651c2-117">Jak příliš[replikace řadiče domény](site-recovery-active-directory.md)</span><span class="sxs-lookup"><span data-stu-id="651c2-117">How too[replicate a domain controller](site-recovery-active-directory.md)</span></span>
1. <span data-ttu-id="651c2-118">Jak příliš[replikaci systému SQL Server](site-recovery-sql.md)</span><span class="sxs-lookup"><span data-stu-id="651c2-118">How too[replicate SQL Server](site-recovery-sql.md)</span></span>

## <a name="deployment-patterns"></a><span data-ttu-id="651c2-119">Vzory pro nasazení</span><span class="sxs-lookup"><span data-stu-id="651c2-119">Deployment patterns</span></span>

<span data-ttu-id="651c2-120">Farmu Citrix XenApp a XenDesktop má obvykle hello následující vzor nasazení:</span><span class="sxs-lookup"><span data-stu-id="651c2-120">A Citrix XenApp and XenDesktop farm typically has hello following deployment pattern:</span></span>

<span data-ttu-id="651c2-121">**Vzor nasazení**</span><span class="sxs-lookup"><span data-stu-id="651c2-121">**Deployment pattern**</span></span>

<span data-ttu-id="651c2-122">Citrix XenApp a XenDesktop nasazením AD DNS serveru a SQL serveru, Citrix doručení řadiči výkladní skříň serveru, XenApp Master (VDA), Citrix XenApp licenční Server databáze</span><span class="sxs-lookup"><span data-stu-id="651c2-122">Citrix XenApp and XenDesktop deployment with AD DNS server, SQL database server, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server</span></span>

![Vzor nasazení 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a><span data-ttu-id="651c2-124">Podpora pro obnovení lokality</span><span class="sxs-lookup"><span data-stu-id="651c2-124">Site Recovery support</span></span>

<span data-ttu-id="651c2-125">Za účelem hello tohoto článku spravovat Citrix nasazení na virtuální počítače VMware vSphere 6.0 nebo System Center VMM 2012 R2 byly použité toosetup zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="651c2-125">For hello purpose of this article, Citrix deployments on VMware virtual machines managed by vSphere 6.0 / System Center VMM 2012 R2 were used toosetup DR.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="651c2-126">Zdroj a cíl</span><span class="sxs-lookup"><span data-stu-id="651c2-126">Source and target</span></span>

<span data-ttu-id="651c2-127">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="651c2-127">**Scenario**</span></span> | <span data-ttu-id="651c2-128">**tooa sekundární lokality**</span><span class="sxs-lookup"><span data-stu-id="651c2-128">**tooa secondary site**</span></span> | <span data-ttu-id="651c2-129">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="651c2-129">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="651c2-130">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="651c2-130">**Hyper-V**</span></span> | <span data-ttu-id="651c2-131">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="651c2-131">Not in scope</span></span> | <span data-ttu-id="651c2-132">Ano</span><span class="sxs-lookup"><span data-stu-id="651c2-132">Yes</span></span>
<span data-ttu-id="651c2-133">**VMware**</span><span class="sxs-lookup"><span data-stu-id="651c2-133">**VMware**</span></span> | <span data-ttu-id="651c2-134">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="651c2-134">Not in scope</span></span> | <span data-ttu-id="651c2-135">Ano</span><span class="sxs-lookup"><span data-stu-id="651c2-135">Yes</span></span>
<span data-ttu-id="651c2-136">**Fyzický server**</span><span class="sxs-lookup"><span data-stu-id="651c2-136">**Physical server**</span></span> | <span data-ttu-id="651c2-137">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="651c2-137">Not in scope</span></span> | <span data-ttu-id="651c2-138">Ano</span><span class="sxs-lookup"><span data-stu-id="651c2-138">Yes</span></span>

### <a name="versions"></a><span data-ttu-id="651c2-139">Verze</span><span class="sxs-lookup"><span data-stu-id="651c2-139">Versions</span></span>
<span data-ttu-id="651c2-140">Zákazníci můžou nasazovat XenApp součásti, jako virtuální počítače běžící na Hyper-V nebo VMware nebo fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="651c2-140">Customers can deploy XenApp components as Virtual Machines running on Hyper-V or VMware or as Physical Servers.</span></span> <span data-ttu-id="651c2-141">Azure Site Recovery chrání i tooAzure fyzické a virtuální nasazení.</span><span class="sxs-lookup"><span data-stu-id="651c2-141">Azure Site Recovery can protect both physical and virtual deployments tooAzure.</span></span>
<span data-ttu-id="651c2-142">Vzhledem k tomu, že XenApp 7,7 nebo novější je podporovaná v Azure, jenom nasazení tyto verze můžete převzít služby při selhání tooAzure pro migraci nebo obnovení po havárii.</span><span class="sxs-lookup"><span data-stu-id="651c2-142">Since XenApp 7.7 or later is supported in Azure, only deployments with these versions can be failed over tooAzure for Disaster Recovery or migration.</span></span>

### <a name="things-tookeep-in-mind"></a><span data-ttu-id="651c2-143">Tookeep věcí v paměti</span><span class="sxs-lookup"><span data-stu-id="651c2-143">Things tookeep in mind</span></span>

1. <span data-ttu-id="651c2-144">Ochrana a obnovení místního nasazení pomocí serveru operačního systému počítače toodeliver XenApp publikované aplikace a XenApp publikovaná stolní počítače podporována.</span><span class="sxs-lookup"><span data-stu-id="651c2-144">Protection and recovery of on-premises deployments using Server OS machines toodeliver XenApp published apps and XenApp published desktops is supported.</span></span>

2. <span data-ttu-id="651c2-145">Ochrana a obnovení místního nasazení pomocí plochy operačního systému počítače toodeliver klientských počítačů VDI pro virtuální plochy klientů, včetně Windows 10, není podporována.</span><span class="sxs-lookup"><span data-stu-id="651c2-145">Protection and recovery of on-premises deployments using desktop OS machines toodeliver Desktop VDI for client virtual desktops, including Windows 10, is not supported.</span></span> <span data-ttu-id="651c2-146">Toto je, protože automatické obnovení systému nepodporuje obnovení hello počítačů s plochou OS'es.</span><span class="sxs-lookup"><span data-stu-id="651c2-146">This is because ASR does not support hello recovery of machines with desktop OS’es.</span></span>  <span data-ttu-id="651c2-147">Kromě toho některé virtuální plochy klientů příchutě (např.</span><span class="sxs-lookup"><span data-stu-id="651c2-147">Also, some client virtual desktop flavours (eg.</span></span> <span data-ttu-id="651c2-148">Windows 7) se ještě nepodporují pro licencování v Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-148">Windows 7) are not yet supported for licensing in Azure.</span></span> <span data-ttu-id="651c2-149">[Další informace](https://azure.microsoft.com/pricing/licensing-faq/) týkající se licencování pro plochy klienta nebo serveru v Azure</span><span class="sxs-lookup"><span data-stu-id="651c2-149">[Learn More](https://azure.microsoft.com/pricing/licensing-faq/) about licensing for client/server desktops in Azure.</span></span>

3.  <span data-ttu-id="651c2-150">Azure Site Recovery nelze replikovat a chránit existující místní relace s více Připojeními nebo systémy současné hodnoty klonovat.</span><span class="sxs-lookup"><span data-stu-id="651c2-150">Azure Site Recovery cannot replicate and protect existing on-premises MCS or PVS clones.</span></span>
<span data-ttu-id="651c2-151">Je nutné toorecreate tyto klony pomocí Azure RM zřizování z řadiče doručení.</span><span class="sxs-lookup"><span data-stu-id="651c2-151">You need toorecreate these clones using Azure RM provisioning from Delivery controller.</span></span>

4. <span data-ttu-id="651c2-152">NetScaler nelze chránit pomocí Azure Site Recovery, protože NetScaler je založena na FreeBSD a Azure Site Recovery nepodporuje ochranu FreeBSD operačního systému.</span><span class="sxs-lookup"><span data-stu-id="651c2-152">NetScaler cannot be protected using Azure Site Recovery as NetScaler is based on FreeBSD and Azure Site Recovery does not support protection of FreeBSD OS.</span></span> <span data-ttu-id="651c2-153">Bude potřebovat toodeploy a po převzetí služeb při selhání tooAzure nakonfigurovat nové zařízení NetScaler z Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="651c2-153">You would need toodeploy and configure a new NetScaler appliance from Azure Market place after failover tooAzure.</span></span>


## <a name="replicating-virtual-machines"></a><span data-ttu-id="651c2-154">Replikace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="651c2-154">Replicating virtual machines</span></span>

<span data-ttu-id="651c2-155">následující součásti hello Citrix XenApp nasazení Hello potřebovat toobe chráněné tooenable replikace a obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-155">hello following components of hello Citrix XenApp deployment need toobe protected tooenable replication and recovery.</span></span>

* <span data-ttu-id="651c2-156">Ochrana serveru AD DNS</span><span class="sxs-lookup"><span data-stu-id="651c2-156">Protection of AD DNS server</span></span>
* <span data-ttu-id="651c2-157">Ochranu databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="651c2-157">Protection of SQL database server</span></span>
* <span data-ttu-id="651c2-158">Ochrana systému Citrix doručení řadiče</span><span class="sxs-lookup"><span data-stu-id="651c2-158">Protection of Citrix Delivery Controller</span></span>
* <span data-ttu-id="651c2-159">Ochrana serveru výkladní skříň.</span><span class="sxs-lookup"><span data-stu-id="651c2-159">Protection of StoreFront server.</span></span>
* <span data-ttu-id="651c2-160">Ochrana XenApp hlavního serveru (VDA)</span><span class="sxs-lookup"><span data-stu-id="651c2-160">Protection of XenApp Master (VDA)</span></span>
* <span data-ttu-id="651c2-161">Ochrana systému Citrix XenApp licenčního serveru</span><span class="sxs-lookup"><span data-stu-id="651c2-161">Protection of Citrix XenApp License Server</span></span>


<span data-ttu-id="651c2-162">**Replikace na serveru AD DNS**</span><span class="sxs-lookup"><span data-stu-id="651c2-162">**AD DNS server replication**</span></span>

<span data-ttu-id="651c2-163">Naleznete příliš[chránit Active Directory a DNS s Azure Site Recovery](site-recovery-active-directory.md) na pokyny pro replikaci a konfiguraci řadiče domény v Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-163">Please refer too[Protect Active Directory and DNS with Azure Site Recovery](site-recovery-active-directory.md) on guidance for replicating and configuring a domain controller in Azure.</span></span>

<span data-ttu-id="651c2-164">**Replikace databáze serveru SQL**</span><span class="sxs-lookup"><span data-stu-id="651c2-164">**SQL database Server replication**</span></span>

<span data-ttu-id="651c2-165">Naleznete příliš[chránit SQL Server s zotavení po havárii serveru SQL a Azure Site Recovery](site-recovery-sql.md) podrobné technické informace o hello doporučené možnosti pro ochranu serverů SQL.</span><span class="sxs-lookup"><span data-stu-id="651c2-165">Please refer too[Protect SQL Server with SQL Server disaster recovery and Azure Site Recovery](site-recovery-sql.md) for detailed technical guidance on hello recommended options for protecting SQL servers.</span></span>

<span data-ttu-id="651c2-166">Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) toostart replikace hello tooAzure ostatní součásti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651c2-166">Follow [this guidance](site-recovery-vmware-to-azure.md) toostart replicating hello other component virtual machines tooAzure.</span></span>

![Ochranu XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

<span data-ttu-id="651c2-168">**Nastavení výpočtu a sítě**</span><span class="sxs-lookup"><span data-stu-id="651c2-168">**Compute and Network Settings**</span></span>

<span data-ttu-id="651c2-169">Poté, co jsou chráněné počítače hello výpočetní hello (stav zobrazí jako "Chráněné" v části replikované položky), a nastavení sítě potřebovat toobe nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="651c2-169">After hello machines are protected (status shows as “Protected” under Replicated Items), hello Compute and Network settings need toobe configured.</span></span>
<span data-ttu-id="651c2-170">V výpočty a síť > výpočetní vlastnosti, můžete zadat název a cílovou velikost virtuálního počítače Azure hello.</span><span class="sxs-lookup"><span data-stu-id="651c2-170">In Compute and Network > Compute properties, you can specify hello Azure VM name and target size.</span></span>
<span data-ttu-id="651c2-171">Upravte název toocomply hello s požadavky na Azure, pokud potřebujete.</span><span class="sxs-lookup"><span data-stu-id="651c2-171">Modify hello name toocomply with Azure requirements if you need to.</span></span> <span data-ttu-id="651c2-172">Můžete také zobrazit a přidejte informace o hello Cílová síť, podsíť a IP adresu, která bude přiřazena toohello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-172">You can also view and add information about hello target network, subnet, and IP address that will be assigned toohello Azure VM.</span></span>

<span data-ttu-id="651c2-173">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="651c2-173">Note hello following:</span></span>

* <span data-ttu-id="651c2-174">Můžete nastavit hello cílová IP adresa.</span><span class="sxs-lookup"><span data-stu-id="651c2-174">You can set hello target IP address.</span></span> <span data-ttu-id="651c2-175">Pokud adresu nezadáte, použije hello převzal počítač DHCP.</span><span class="sxs-lookup"><span data-stu-id="651c2-175">If you don't provide an address, hello failed over machine will use DHCP.</span></span> <span data-ttu-id="651c2-176">Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání hello nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="651c2-176">If you set an address that isn't available at failover, hello failover won't work.</span></span> <span data-ttu-id="651c2-177">Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.</span><span class="sxs-lookup"><span data-stu-id="651c2-177">hello same target IP address can be used for test failover if hello address is available in hello test failover network.</span></span>

* <span data-ttu-id="651c2-178">Pro server hello AD a DNS ponechá hello místní adresu umožňují určit hello stejné adresy jako hello server DNS pro Azure Virtual network hello.</span><span class="sxs-lookup"><span data-stu-id="651c2-178">For hello AD/DNS server, retaining hello on-premises address lets you specify hello same address as hello DNS server for hello Azure Virtual network.</span></span>

<span data-ttu-id="651c2-179">Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="651c2-179">hello number of network adapters is dictated by hello size you specify for hello target virtual machine, as follows:</span></span>

*   <span data-ttu-id="651c2-180">Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="651c2-180">If hello number of network adapters on hello source machine is less than or equal toohello number of adapters allowed for hello target machine size, then hello target will have hello same number of adapters as hello source.</span></span>
*   <span data-ttu-id="651c2-181">Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.</span><span class="sxs-lookup"><span data-stu-id="651c2-181">If hello number of adapters for hello source virtual machine exceeds hello number allowed for hello target size then hello target size maximum will be used.</span></span>
* <span data-ttu-id="651c2-182">Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry.</span><span class="sxs-lookup"><span data-stu-id="651c2-182">For example, if a source machine has two network adapters and hello target machine size supports four, hello target machine will have two adapters.</span></span> <span data-ttu-id="651c2-183">Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.</span><span class="sxs-lookup"><span data-stu-id="651c2-183">If hello source machine has two adapters but hello supported target size only supports one then hello target machine will have only one adapter.</span></span>
*   <span data-ttu-id="651c2-184">Pokud hello virtuální počítač více síťových adaptérů připojí se všechny toohello stejné síti.</span><span class="sxs-lookup"><span data-stu-id="651c2-184">If hello virtual machine has multiple network adapters they will all connect toohello same network.</span></span>
*   <span data-ttu-id="651c2-185">Pokud hello virtuální počítač více síťových adaptérů, pak hello první z nich uvedené v seznamu hello stává hello výchozí síťový adaptér v hello virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-185">If hello virtual machine has multiple network adapters, then hello first one shown in hello list becomes hello Default network adapter in hello Azure virtual machine.</span></span>


## <a name="creating-a-recovery-plan"></a><span data-ttu-id="651c2-186">Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="651c2-186">Creating a recovery plan</span></span>

<span data-ttu-id="651c2-187">Po povolení replikace pro virtuální počítače součást XenApp hello hello dalším krokem je toocreate plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-187">After replication is enabled for hello XenApp component VMs, hello next step is toocreate a recovery plan.</span></span>
<span data-ttu-id="651c2-188">Obnovení naplánujte skupiny společně virtuálních počítačů s podobné požadavky pro převzetí služeb při selhání a obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-188">A recovery plan groups together virtual machines with similar requirements for failover and recovery.</span></span>  

<span data-ttu-id="651c2-189">**Kroky toocreate plán obnovení**</span><span class="sxs-lookup"><span data-stu-id="651c2-189">**Steps toocreate a recovery plan**</span></span>

1. <span data-ttu-id="651c2-190">Přidání hello XenApp součásti virtuálních počítačů v hello plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-190">Add hello XenApp component virtual machines in hello Recovery Plan.</span></span>
2. <span data-ttu-id="651c2-191">Klikněte na tlačítko plány obnovení -> + plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-191">Click Recovery Plans -> + Recovery Plan.</span></span> <span data-ttu-id="651c2-192">Zadejte název intuitivní pro plán obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="651c2-192">Provide an intuitive name for hello recovery plan.</span></span>
3. <span data-ttu-id="651c2-193">U virtuálních počítačů VMware: Zvolit zdroj jako procesní server VMware, cíl jako Microsoft Azure a modelu nasazení Resource Manager a klikněte na možnost vybrat položky.</span><span class="sxs-lookup"><span data-stu-id="651c2-193">For VMware virtual machines: Select source as VMware process server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items.</span></span>
4. <span data-ttu-id="651c2-194">Pro virtuální počítače Hyper-V: Vyberte zdroj jako VMM server, jako Microsoft Azure a modelu nasazení Resource Manager jako cíl a klikněte na možnost vybrat položky a pak vyberte hello XenApp nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651c2-194">For Hyper-V virtual machines: Select source as VMM server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items and then select hello XenApp deployment VMs.</span></span>

### <a name="adding-virtual-machines-toofailover-groups"></a><span data-ttu-id="651c2-195">Přidání skupin toofailover virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="651c2-195">Adding virtual machines toofailover groups</span></span>

<span data-ttu-id="651c2-196">Plány obnovení může být vlastní tooadd převzetí služeb při selhání skupiny pro konkrétní spuštění pořadí, skripty nebo ruční akce.</span><span class="sxs-lookup"><span data-stu-id="651c2-196">Recovery plans can be customized tooadd failover groups for specific startup order, scripts or manual actions.</span></span> <span data-ttu-id="651c2-197">Následující skupiny Hello potřebovat plán obnovení přidané toohello toobe.</span><span class="sxs-lookup"><span data-stu-id="651c2-197">hello following groups need toobe added toohello recovery plan.</span></span>

1. <span data-ttu-id="651c2-198">Group1 převzetí služeb při selhání: DNS AD</span><span class="sxs-lookup"><span data-stu-id="651c2-198">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="651c2-199">Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL</span><span class="sxs-lookup"><span data-stu-id="651c2-199">Failover Group2: SQL Server VMs</span></span>
2. <span data-ttu-id="651c2-200">Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="651c2-200">Failover Group3: VDA Master Image VM</span></span>
3. <span data-ttu-id="651c2-201">Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru</span><span class="sxs-lookup"><span data-stu-id="651c2-201">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>


### <a name="adding-scripts-toohello-recovery-plan"></a><span data-ttu-id="651c2-202">Přidávání skriptů toohello plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="651c2-202">Adding scripts toohello recovery plan</span></span>

<span data-ttu-id="651c2-203">Skripty, můžete spustit před nebo po určité skupiny v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="651c2-203">Scripts can be run before or after a specific group in a recovery plan.</span></span> <span data-ttu-id="651c2-204">Ruční akce může být také být zahrnutý a prováděné během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="651c2-204">Manual actions can be also be included and performed during failover.</span></span>

<span data-ttu-id="651c2-205">plán obnovení přizpůsobené Hello vypadá hello níže:</span><span class="sxs-lookup"><span data-stu-id="651c2-205">hello customized recovery plan looks like hello below:</span></span>

1. <span data-ttu-id="651c2-206">Group1 převzetí služeb při selhání: DNS AD</span><span class="sxs-lookup"><span data-stu-id="651c2-206">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="651c2-207">Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL</span><span class="sxs-lookup"><span data-stu-id="651c2-207">Failover Group2: SQL Server VMs</span></span>
3. <span data-ttu-id="651c2-208">Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="651c2-208">Failover Group3: VDA Master Image VM</span></span>

   >[!NOTE]     
   ><span data-ttu-id="651c2-209">Kroky 4, 6 a 7 obsahující ruční nebo skript akce jsou příslušné tooonly místní XenApp > prostředí s MCS/systémy současné hodnoty katalogů.</span><span class="sxs-lookup"><span data-stu-id="651c2-209">Steps 4, 6 and 7 containing manual or script actions are applicable tooonly an on-premises XenApp >environment with MCS/PVS catalogs.</span></span>

4. <span data-ttu-id="651c2-210">Ruční nebo skript akce skupina 3: vypnutí hlavní počítač VDA hello hlavní VDA virtuálních počítačů, když převzal tooAzure bude ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="651c2-210">Group 3 Manual or script action: Shutdown master VDA VM hello Master VDA VM when failed over tooAzure will be in a running state.</span></span> <span data-ttu-id="651c2-211">toocreate nové relace s více Připojeními katalogy pomocí Azure ARM hostování, hlavní hello VDA virtuální počítač je požadovaná toobe v zastaveném (de přidělené) stavu.</span><span class="sxs-lookup"><span data-stu-id="651c2-211">toocreate new MCS catalogs using Azure ARM hosting, hello master VDA VM is required toobe in Stopped (de allocated) state.</span></span> <span data-ttu-id="651c2-212">Vypnutí hello virtuální počítač z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-212">Shutdown hello VM from Azure Portal.</span></span>

5. <span data-ttu-id="651c2-213">Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru</span><span class="sxs-lookup"><span data-stu-id="651c2-213">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>
6. <span data-ttu-id="651c2-214">Skupina3 ruční nebo skript akce 1:</span><span class="sxs-lookup"><span data-stu-id="651c2-214">Group3 manual or script action 1:</span></span>

    <span data-ttu-id="651c2-215">***Přidat připojení hostitele Azure RM***</span><span class="sxs-lookup"><span data-stu-id="651c2-215">***Add Azure RM host connection***</span></span>

    <span data-ttu-id="651c2-216">Vytvořte připojení hostitele Azure ARM v doručení řadiče počítač tooprovision nové relace s více Připojeními katalogů v Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-216">Create Azure ARM host connection in Delivery Controller machine tooprovision new MCS catalogs in Azure.</span></span> <span data-ttu-id="651c2-217">Postupujte podle kroků hello, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span><span class="sxs-lookup"><span data-stu-id="651c2-217">Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span></span>

7. <span data-ttu-id="651c2-218">Skupina3 ruční nebo skript akce 2:</span><span class="sxs-lookup"><span data-stu-id="651c2-218">Group3 manual or script action 2:</span></span>

    <span data-ttu-id="651c2-219">***Znovu vytvořit relace s více Připojeními katalogů v Azure***</span><span class="sxs-lookup"><span data-stu-id="651c2-219">***Re-create MCS Catalogs in Azure***</span></span>

    <span data-ttu-id="651c2-220">Hello existující relace s více Připojeními nebo systémy současné hodnoty klony v primární lokalitě hello nebudou replikované tooAzure.</span><span class="sxs-lookup"><span data-stu-id="651c2-220">hello existing MCS or PVS clones on hello primary site will not be replicated tooAzure.</span></span> <span data-ttu-id="651c2-221">Je nutné toorecreate tyto klony pomocí hello replikovaný hlavní VDA a Azure ARM zřizování z řadiče doručení. Postupujte podle kroků hello, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate relace s více Připojeními katalogizuje v Azure.</span><span class="sxs-lookup"><span data-stu-id="651c2-221">You need toorecreate these clones using hello replicated master VDA and Azure ARM provisioning from Delivery controller.Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS catalogs in Azure.</span></span>

![Plán obnovení pro XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   ><span data-ttu-id="651c2-223">Můžete použít skripty v [umístění](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS s hello převzetí služeb při selhání nové IP adresy z hello > virtuálních počítačů nebo tooattach Vyrovnávání zatížení na hello převzít služby při selhání virtuálního počítače, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="651c2-223">You can use scripts at [location](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS with hello new IPs of hello failed over >virtual machines or tooattach a load balancer on hello failed over virtual machine, if needed.</span></span>


## <a name="doing-a-test-failover"></a><span data-ttu-id="651c2-224">Provádění testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="651c2-224">Doing a test failover</span></span>

<span data-ttu-id="651c2-225">Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="651c2-225">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

![Plán obnovení](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a><span data-ttu-id="651c2-227">Převzetím služeb</span><span class="sxs-lookup"><span data-stu-id="651c2-227">Doing a failover</span></span>

<span data-ttu-id="651c2-228">Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="651c2-228">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="651c2-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="651c2-229">Next steps</span></span>

<span data-ttu-id="651c2-230">Můžete [Další](https://aka.ms/citrix-xenapp-xendesktop-with-asr) o replikaci Citrix XenApp a XenDesktop nasazení v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="651c2-230">You can [learn more](https://aka.ms/citrix-xenapp-xendesktop-with-asr) about replicating Citrix XenApp and XenDesktop deployments  in this white paper.</span></span> <span data-ttu-id="651c2-231">Podívejte se na pokyny hello příliš[replikovat jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="651c2-231">Look at hello guidance too[replicate other applications](site-recovery-workload.md) using Site Recovery.</span></span>
