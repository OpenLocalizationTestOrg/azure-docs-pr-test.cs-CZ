---
title: "Replikovat vícevrstvé Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak k ochraně a obnovování Citrix XenDesktop a XenApp nasazení pomocí Azure Site Recovery."
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
ms.openlocfilehash: dc064352b1841ff346b705dc63186b12d79350b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a><span data-ttu-id="b699d-103">Replikovat vícevrstvé Citrix XenApp a XenDesktop nasazení pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="b699d-103">Replicate a multi-tier Citrix XenApp and XenDesktop deployment using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="b699d-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b699d-104">Overview</span></span>

<span data-ttu-id="b699d-105">Citrix XenDesktop je řešení virtualizace plochy, které nabízí plochy a aplikace jako služba ondemand s žádným uživatelem, kdekoli.</span><span class="sxs-lookup"><span data-stu-id="b699d-105">Citrix XenDesktop is a desktop virtualization solution that delivers desktops and applications as an ondemand service to any user, anywhere.</span></span> <span data-ttu-id="b699d-106">S technologií doručení FlexCast XenDesktop můžete rychle a bezpečně poskytovat aplikacím a plochám pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="b699d-106">With FlexCast delivery technology, XenDesktop can quickly and securely deliver applications and desktops to users.</span></span>
<span data-ttu-id="b699d-107">V současné době Citrix XenApp neposkytuje žádné po havárii možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-107">Today, Citrix XenApp does not provide any disaster recovery capabilities.</span></span>

<span data-ttu-id="b699d-108">Řešení pro zotavení po havárii funkční, by měl Povolit modelování plány obnovení kolem výše architektury komplexní aplikace a také mít možnost přidat vlastní postup zpracování aplikace mapování mezi různými úrovněmi proto opravdu poskytování jedním kliknutím v případě havárie vedoucí k nižší RTO snímek řešení.</span><span class="sxs-lookup"><span data-stu-id="b699d-108">A good disaster recovery solution, should allow modeling of recovery plans around the above complex application architectures and also have the ability to add customized steps to handle application mappings between various tiers hence providing a single-click sure shot solution in the event of a disaster leading to a lower RTO.</span></span>

<span data-ttu-id="b699d-109">Tento dokument obsahuje podrobné pokyny pro vytváření řešení zotavení po havárii pro místní nasazení Citrix XenApp na Hyper-V a VMware vSphere platformách.</span><span class="sxs-lookup"><span data-stu-id="b699d-109">This document provides a step-by-step guidance for building a disaster recovery solution for your on-premises Citrix XenApp deployments on Hyper-V and VMware vSphere platforms.</span></span> <span data-ttu-id="b699d-110">Tento dokument také popisuje, jak provést testovací převzetí služeb při selhání (postupu zotavení po havárii) a neplánované převzetí služeb při selhání do Azure pomocí plánů obnovení, podporované konfigurace a požadavky.</span><span class="sxs-lookup"><span data-stu-id="b699d-110">This document also describes how to perform a test failover(disaster recovery drill) and unplanned failover to Azure using recovery plans, the supported configurations and prerequisites.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b699d-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b699d-111">Prerequisites</span></span>

<span data-ttu-id="b699d-112">Než začnete, ujistěte se, že rozumíte následující:</span><span class="sxs-lookup"><span data-stu-id="b699d-112">Before you start, make sure you understand the following:</span></span>

1. [<span data-ttu-id="b699d-113">Replikaci virtuálního počítače do Azure</span><span class="sxs-lookup"><span data-stu-id="b699d-113">Replicating a virtual machine to Azure</span></span>](site-recovery-vmware-to-azure.md)
1. <span data-ttu-id="b699d-114">Postup [návrh k síti pro obnovení](site-recovery-network-design.md)</span><span class="sxs-lookup"><span data-stu-id="b699d-114">How to [design a recovery network](site-recovery-network-design.md)</span></span>
1. [<span data-ttu-id="b699d-115">Provádění převzetí služeb při selhání do Azure</span><span class="sxs-lookup"><span data-stu-id="b699d-115">Doing a test failover to Azure</span></span>](site-recovery-test-failover-to-azure.md)
1. [<span data-ttu-id="b699d-116">Provádění převzetí služeb při selhání do Azure</span><span class="sxs-lookup"><span data-stu-id="b699d-116">Doing a failover to Azure</span></span>](site-recovery-failover.md)
1. <span data-ttu-id="b699d-117">Postup [replikace řadiče domény](site-recovery-active-directory.md)</span><span class="sxs-lookup"><span data-stu-id="b699d-117">How to [replicate a domain controller](site-recovery-active-directory.md)</span></span>
1. <span data-ttu-id="b699d-118">Postup [replikaci systému SQL Server](site-recovery-sql.md)</span><span class="sxs-lookup"><span data-stu-id="b699d-118">How to [replicate SQL Server](site-recovery-sql.md)</span></span>

## <a name="deployment-patterns"></a><span data-ttu-id="b699d-119">Vzory pro nasazení</span><span class="sxs-lookup"><span data-stu-id="b699d-119">Deployment patterns</span></span>

<span data-ttu-id="b699d-120">Farmu Citrix XenApp a XenDesktop má obvykle vzoru následující nasazení:</span><span class="sxs-lookup"><span data-stu-id="b699d-120">A Citrix XenApp and XenDesktop farm typically has the following deployment pattern:</span></span>

<span data-ttu-id="b699d-121">**Vzor nasazení**</span><span class="sxs-lookup"><span data-stu-id="b699d-121">**Deployment pattern**</span></span>

<span data-ttu-id="b699d-122">Citrix XenApp a XenDesktop nasazením AD DNS serveru a SQL serveru, Citrix doručení řadiči výkladní skříň serveru, XenApp Master (VDA), Citrix XenApp licenční Server databáze</span><span class="sxs-lookup"><span data-stu-id="b699d-122">Citrix XenApp and XenDesktop deployment with AD DNS server, SQL database server, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server</span></span>

![Vzor nasazení 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a><span data-ttu-id="b699d-124">Podpora pro obnovení lokality</span><span class="sxs-lookup"><span data-stu-id="b699d-124">Site Recovery support</span></span>

<span data-ttu-id="b699d-125">Pro účely tohoto článku spravovat Citrix nasazení na virtuální počítače VMware vSphere 6.0 nebo System Center VMM 2012 R2 jste použili k nastavení zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="b699d-125">For the purpose of this article, Citrix deployments on VMware virtual machines managed by vSphere 6.0 / System Center VMM 2012 R2 were used to setup DR.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="b699d-126">Zdroj a cíl</span><span class="sxs-lookup"><span data-stu-id="b699d-126">Source and target</span></span>

<span data-ttu-id="b699d-127">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="b699d-127">**Scenario**</span></span> | <span data-ttu-id="b699d-128">**Sekundární lokality**</span><span class="sxs-lookup"><span data-stu-id="b699d-128">**To a secondary site**</span></span> | <span data-ttu-id="b699d-129">**Do Azure**</span><span class="sxs-lookup"><span data-stu-id="b699d-129">**To Azure**</span></span>
--- | --- | ---
<span data-ttu-id="b699d-130">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="b699d-130">**Hyper-V**</span></span> | <span data-ttu-id="b699d-131">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="b699d-131">Not in scope</span></span> | <span data-ttu-id="b699d-132">Ano</span><span class="sxs-lookup"><span data-stu-id="b699d-132">Yes</span></span>
<span data-ttu-id="b699d-133">**VMware**</span><span class="sxs-lookup"><span data-stu-id="b699d-133">**VMware**</span></span> | <span data-ttu-id="b699d-134">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="b699d-134">Not in scope</span></span> | <span data-ttu-id="b699d-135">Ano</span><span class="sxs-lookup"><span data-stu-id="b699d-135">Yes</span></span>
<span data-ttu-id="b699d-136">**Fyzický server**</span><span class="sxs-lookup"><span data-stu-id="b699d-136">**Physical server**</span></span> | <span data-ttu-id="b699d-137">Není v oboru</span><span class="sxs-lookup"><span data-stu-id="b699d-137">Not in scope</span></span> | <span data-ttu-id="b699d-138">Ano</span><span class="sxs-lookup"><span data-stu-id="b699d-138">Yes</span></span>

### <a name="versions"></a><span data-ttu-id="b699d-139">Verze</span><span class="sxs-lookup"><span data-stu-id="b699d-139">Versions</span></span>
<span data-ttu-id="b699d-140">Zákazníci můžou nasazovat XenApp součásti, jako virtuální počítače běžící na Hyper-V nebo VMware nebo fyzických serverů.</span><span class="sxs-lookup"><span data-stu-id="b699d-140">Customers can deploy XenApp components as Virtual Machines running on Hyper-V or VMware or as Physical Servers.</span></span> <span data-ttu-id="b699d-141">Azure Site Recovery můžete chránit fyzické i virtuální nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-141">Azure Site Recovery can protect both physical and virtual deployments to Azure.</span></span>
<span data-ttu-id="b699d-142">Vzhledem k tomu, že XenApp 7,7 nebo novější je podporovaná v Azure, jenom nasazení s těmito verzemi mohou být přebrány do Azure pro zotavení po havárii nebo migrace.</span><span class="sxs-lookup"><span data-stu-id="b699d-142">Since XenApp 7.7 or later is supported in Azure, only deployments with these versions can be failed over to Azure for Disaster Recovery or migration.</span></span>

### <a name="things-to-keep-in-mind"></a><span data-ttu-id="b699d-143">Co je potřeba mějte na paměti</span><span class="sxs-lookup"><span data-stu-id="b699d-143">Things to keep in mind</span></span>

1. <span data-ttu-id="b699d-144">Ochrana a obnovení místního nasazení pomocí serveru operačního systému počítače, které chcete poskytovat XenApp publikované aplikace a XenApp publikovaná stolní počítače podporována.</span><span class="sxs-lookup"><span data-stu-id="b699d-144">Protection and recovery of on-premises deployments using Server OS machines to deliver XenApp published apps and XenApp published desktops is supported.</span></span>

2. <span data-ttu-id="b699d-145">Ochrana a obnovení místního nasazení pomocí stolních počítačích operačního systému k poskytování klientských počítačů VDI pro klienta virtuálních ploch, včetně Windows 10, není podporována.</span><span class="sxs-lookup"><span data-stu-id="b699d-145">Protection and recovery of on-premises deployments using desktop OS machines to deliver Desktop VDI for client virtual desktops, including Windows 10, is not supported.</span></span> <span data-ttu-id="b699d-146">Toto je, protože automatické obnovení systému nepodporuje obnovení počítače pomocí plochy OS'es.</span><span class="sxs-lookup"><span data-stu-id="b699d-146">This is because ASR does not support the recovery of machines with desktop OS’es.</span></span>  <span data-ttu-id="b699d-147">Kromě toho některé virtuální plochy klientů příchutě (např.</span><span class="sxs-lookup"><span data-stu-id="b699d-147">Also, some client virtual desktop flavours (eg.</span></span> <span data-ttu-id="b699d-148">Windows 7) se ještě nepodporují pro licencování v Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-148">Windows 7) are not yet supported for licensing in Azure.</span></span> <span data-ttu-id="b699d-149">[Další informace](https://azure.microsoft.com/pricing/licensing-faq/) týkající se licencování pro plochy klienta nebo serveru v Azure</span><span class="sxs-lookup"><span data-stu-id="b699d-149">[Learn More](https://azure.microsoft.com/pricing/licensing-faq/) about licensing for client/server desktops in Azure.</span></span>

3.  <span data-ttu-id="b699d-150">Azure Site Recovery nelze replikovat a chránit existující místní relace s více Připojeními nebo systémy současné hodnoty klonovat.</span><span class="sxs-lookup"><span data-stu-id="b699d-150">Azure Site Recovery cannot replicate and protect existing on-premises MCS or PVS clones.</span></span>
<span data-ttu-id="b699d-151">Budete muset znovu vytvořit tyto klony pomocí Azure RM zřizování z řadiče doručení.</span><span class="sxs-lookup"><span data-stu-id="b699d-151">You need to recreate these clones using Azure RM provisioning from Delivery controller.</span></span>

4. <span data-ttu-id="b699d-152">NetScaler nelze chránit pomocí Azure Site Recovery, protože NetScaler je založena na FreeBSD a Azure Site Recovery nepodporuje ochranu FreeBSD operačního systému.</span><span class="sxs-lookup"><span data-stu-id="b699d-152">NetScaler cannot be protected using Azure Site Recovery as NetScaler is based on FreeBSD and Azure Site Recovery does not support protection of FreeBSD OS.</span></span> <span data-ttu-id="b699d-153">Potřebovali byste k nasazení a konfiguraci nové zařízení NetScaler z Azure Marketplace po převzetí služeb při selhání do Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-153">You would need to deploy and configure a new NetScaler appliance from Azure Market place after failover to Azure.</span></span>


## <a name="replicating-virtual-machines"></a><span data-ttu-id="b699d-154">Replikace virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b699d-154">Replicating virtual machines</span></span>

<span data-ttu-id="b699d-155">Následující komponenty aplikace Citrix XenApp nasazení musí být chráněny povolit replikaci a obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-155">The following components of the Citrix XenApp deployment need to be protected to enable replication and recovery.</span></span>

* <span data-ttu-id="b699d-156">Ochrana serveru AD DNS</span><span class="sxs-lookup"><span data-stu-id="b699d-156">Protection of AD DNS server</span></span>
* <span data-ttu-id="b699d-157">Ochranu databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="b699d-157">Protection of SQL database server</span></span>
* <span data-ttu-id="b699d-158">Ochrana systému Citrix doručení řadiče</span><span class="sxs-lookup"><span data-stu-id="b699d-158">Protection of Citrix Delivery Controller</span></span>
* <span data-ttu-id="b699d-159">Ochrana serveru výkladní skříň.</span><span class="sxs-lookup"><span data-stu-id="b699d-159">Protection of StoreFront server.</span></span>
* <span data-ttu-id="b699d-160">Ochrana XenApp hlavního serveru (VDA)</span><span class="sxs-lookup"><span data-stu-id="b699d-160">Protection of XenApp Master (VDA)</span></span>
* <span data-ttu-id="b699d-161">Ochrana systému Citrix XenApp licenčního serveru</span><span class="sxs-lookup"><span data-stu-id="b699d-161">Protection of Citrix XenApp License Server</span></span>


<span data-ttu-id="b699d-162">**Replikace na serveru AD DNS**</span><span class="sxs-lookup"><span data-stu-id="b699d-162">**AD DNS server replication**</span></span>

<span data-ttu-id="b699d-163">Naleznete [chránit Active Directory a DNS s Azure Site Recovery](site-recovery-active-directory.md) na pokyny pro replikaci a konfiguraci řadiče domény v Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-163">Please refer to [Protect Active Directory and DNS with Azure Site Recovery](site-recovery-active-directory.md) on guidance for replicating and configuring a domain controller in Azure.</span></span>

<span data-ttu-id="b699d-164">**Replikace databáze serveru SQL**</span><span class="sxs-lookup"><span data-stu-id="b699d-164">**SQL database Server replication**</span></span>

<span data-ttu-id="b699d-165">Naleznete [chránit SQL Server s zotavení po havárii serveru SQL a Azure Site Recovery](site-recovery-sql.md) podrobné technické informace o doporučené možnosti pro ochranu serverů SQL.</span><span class="sxs-lookup"><span data-stu-id="b699d-165">Please refer to [Protect SQL Server with SQL Server disaster recovery and Azure Site Recovery](site-recovery-sql.md) for detailed technical guidance on the recommended options for protecting SQL servers.</span></span>

<span data-ttu-id="b699d-166">Postupujte podle [v tomto návodu](site-recovery-vmware-to-azure.md) k zahájení replikace ostatní součásti virtuálních počítačů do Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-166">Follow [this guidance](site-recovery-vmware-to-azure.md) to start replicating the other component virtual machines to Azure.</span></span>

![Ochranu XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

<span data-ttu-id="b699d-168">**Nastavení výpočtu a sítě**</span><span class="sxs-lookup"><span data-stu-id="b699d-168">**Compute and Network Settings**</span></span>

<span data-ttu-id="b699d-169">Poté, co jsou chráněné na počítače (stav zobrazuje jako "Chráněné" v části replikované položky), nastavení výpočtů a sítě je potřeba nakonfigurovat ji tak.</span><span class="sxs-lookup"><span data-stu-id="b699d-169">After the machines are protected (status shows as “Protected” under Replicated Items), the Compute and Network settings need to be configured.</span></span>
<span data-ttu-id="b699d-170">V výpočty a síť > výpočetní vlastnosti, můžete zadat název a cílovou velikost virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-170">In Compute and Network > Compute properties, you can specify the Azure VM name and target size.</span></span>
<span data-ttu-id="b699d-171">Název pro dosažení souladu s požadavky na Azure, pokud potřebujete změňte.</span><span class="sxs-lookup"><span data-stu-id="b699d-171">Modify the name to comply with Azure requirements if you need to.</span></span> <span data-ttu-id="b699d-172">Můžete také zobrazit a přidejte informace o Cílová síť, podsíť a IP adresu, která bude přiřazena virtuálnímu počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-172">You can also view and add information about the target network, subnet, and IP address that will be assigned to the Azure VM.</span></span>

<span data-ttu-id="b699d-173">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="b699d-173">Note the following:</span></span>

* <span data-ttu-id="b699d-174">Můžete nastavit cílovou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="b699d-174">You can set the target IP address.</span></span> <span data-ttu-id="b699d-175">Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP.</span><span class="sxs-lookup"><span data-stu-id="b699d-175">If you don't provide an address, the failed over machine will use DHCP.</span></span> <span data-ttu-id="b699d-176">Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="b699d-176">If you set an address that isn't available at failover, the failover won't work.</span></span> <span data-ttu-id="b699d-177">Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="b699d-177">The same target IP address can be used for test failover if the address is available in the test failover network.</span></span>

* <span data-ttu-id="b699d-178">Pro server AD a DNS ponechá adresu místní umožňuje zadejte stejnou adresu jako server DNS pro Azure Virtual network.</span><span class="sxs-lookup"><span data-stu-id="b699d-178">For the AD/DNS server, retaining the on-premises address lets you specify the same address as the DNS server for the Azure Virtual network.</span></span>

<span data-ttu-id="b699d-179">Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="b699d-179">The number of network adapters is dictated by the size you specify for the target virtual machine, as follows:</span></span>

*   <span data-ttu-id="b699d-180">Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="b699d-180">If the number of network adapters on the source machine is less than or equal to the number of adapters allowed for the target machine size, then the target will have the same number of adapters as the source.</span></span>
*   <span data-ttu-id="b699d-181">Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.</span><span class="sxs-lookup"><span data-stu-id="b699d-181">If the number of adapters for the source virtual machine exceeds the number allowed for the target size then the target size maximum will be used.</span></span>
* <span data-ttu-id="b699d-182">Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry.</span><span class="sxs-lookup"><span data-stu-id="b699d-182">For example, if a source machine has two network adapters and the target machine size supports four, the target machine will have two adapters.</span></span> <span data-ttu-id="b699d-183">Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.</span><span class="sxs-lookup"><span data-stu-id="b699d-183">If the source machine has two adapters but the supported target size only supports one then the target machine will have only one adapter.</span></span>
*   <span data-ttu-id="b699d-184">Pokud má virtuální počítač více síťových adaptérů připojí se všechny ke stejné síti.</span><span class="sxs-lookup"><span data-stu-id="b699d-184">If the virtual machine has multiple network adapters they will all connect to the same network.</span></span>
*   <span data-ttu-id="b699d-185">Pokud virtuální počítač má několik síťových adaptérů, uvedené v seznamu první z nich stane výchozí síťový adaptér ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-185">If the virtual machine has multiple network adapters, then the first one shown in the list becomes the Default network adapter in the Azure virtual machine.</span></span>


## <a name="creating-a-recovery-plan"></a><span data-ttu-id="b699d-186">Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="b699d-186">Creating a recovery plan</span></span>

<span data-ttu-id="b699d-187">Po povolení replikace pro virtuální počítače XenApp součásti, dalším krokem je vytvoření plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-187">After replication is enabled for the XenApp component VMs, the next step is to create a recovery plan.</span></span>
<span data-ttu-id="b699d-188">Obnovení naplánujte skupiny společně virtuálních počítačů s podobné požadavky pro převzetí služeb při selhání a obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-188">A recovery plan groups together virtual machines with similar requirements for failover and recovery.</span></span>  

<span data-ttu-id="b699d-189">**Postup vytvoření plánu obnovení**</span><span class="sxs-lookup"><span data-stu-id="b699d-189">**Steps to create a recovery plan**</span></span>

1. <span data-ttu-id="b699d-190">Přidejte XenApp součást virtuální počítače v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-190">Add the XenApp component virtual machines in the Recovery Plan.</span></span>
2. <span data-ttu-id="b699d-191">Klikněte na tlačítko plány obnovení -> + plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-191">Click Recovery Plans -> + Recovery Plan.</span></span> <span data-ttu-id="b699d-192">Zadejte název intuitivní pro plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-192">Provide an intuitive name for the recovery plan.</span></span>
3. <span data-ttu-id="b699d-193">U virtuálních počítačů VMware: Zvolit zdroj jako procesní server VMware, cíl jako Microsoft Azure a modelu nasazení Resource Manager a klikněte na možnost vybrat položky.</span><span class="sxs-lookup"><span data-stu-id="b699d-193">For VMware virtual machines: Select source as VMware process server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items.</span></span>
4. <span data-ttu-id="b699d-194">Pro virtuální počítače Hyper-V: Vyberte zdroj jako VMM server, jako Microsoft Azure a modelu nasazení Resource Manager jako cíl a klikněte na možnost vybrat položky a pak vyberte XenApp nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="b699d-194">For Hyper-V virtual machines: Select source as VMM server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items and then select the XenApp deployment VMs.</span></span>

### <a name="adding-virtual-machines-to-failover-groups"></a><span data-ttu-id="b699d-195">Probíhá přidávání virtuálních počítačů do skupin převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="b699d-195">Adding virtual machines to failover groups</span></span>

<span data-ttu-id="b699d-196">Přidání skupin převzetí služeb při selhání pro konkrétní spuštění pořadí, skripty a ručně prováděné akce lze přizpůsobit plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-196">Recovery plans can be customized to add failover groups for specific startup order, scripts or manual actions.</span></span> <span data-ttu-id="b699d-197">Následující skupiny je třeba přidat do plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-197">The following groups need to be added to the recovery plan.</span></span>

1. <span data-ttu-id="b699d-198">Group1 převzetí služeb při selhání: DNS AD</span><span class="sxs-lookup"><span data-stu-id="b699d-198">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="b699d-199">Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL</span><span class="sxs-lookup"><span data-stu-id="b699d-199">Failover Group2: SQL Server VMs</span></span>
2. <span data-ttu-id="b699d-200">Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b699d-200">Failover Group3: VDA Master Image VM</span></span>
3. <span data-ttu-id="b699d-201">Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru</span><span class="sxs-lookup"><span data-stu-id="b699d-201">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>


### <a name="adding-scripts-to-the-recovery-plan"></a><span data-ttu-id="b699d-202">Probíhá přidávání skriptů do plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="b699d-202">Adding scripts to the recovery plan</span></span>

<span data-ttu-id="b699d-203">Skripty, můžete spustit před nebo po určité skupiny v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="b699d-203">Scripts can be run before or after a specific group in a recovery plan.</span></span> <span data-ttu-id="b699d-204">Ruční akce může být také být zahrnutý a prováděné během převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="b699d-204">Manual actions can be also be included and performed during failover.</span></span>

<span data-ttu-id="b699d-205">Plán obnovení přizpůsobené vypadá níže:</span><span class="sxs-lookup"><span data-stu-id="b699d-205">The customized recovery plan looks like the below:</span></span>

1. <span data-ttu-id="b699d-206">Group1 převzetí služeb při selhání: DNS AD</span><span class="sxs-lookup"><span data-stu-id="b699d-206">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="b699d-207">Skupina2 převzetí služeb při selhání: Virtuální počítače serveru SQL</span><span class="sxs-lookup"><span data-stu-id="b699d-207">Failover Group2: SQL Server VMs</span></span>
3. <span data-ttu-id="b699d-208">Převzetí služeb při selhání skupina3: VDA hlavní bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b699d-208">Failover Group3: VDA Master Image VM</span></span>

   >[!NOTE]     
   ><span data-ttu-id="b699d-209">Kroky 4, 6 a 7 obsahující ruční nebo skript akce platí pro pouze místní XenApp > prostředí s MCS/systémy současné hodnoty katalogů.</span><span class="sxs-lookup"><span data-stu-id="b699d-209">Steps 4, 6 and 7 containing manual or script actions are applicable to only an on-premises XenApp >environment with MCS/PVS catalogs.</span></span>

4. <span data-ttu-id="b699d-210">Ruční nebo skript akce skupina 3: vypnutí hlavní VDA virtuálních počítačů hlavní VDA virtuální počítač při převzetí služeb při selhání do Azure bude ve spuštěném stavu.</span><span class="sxs-lookup"><span data-stu-id="b699d-210">Group 3 Manual or script action: Shutdown master VDA VM The Master VDA VM when failed over to Azure will be in a running state.</span></span> <span data-ttu-id="b699d-211">Pokud chcete vytvořit novou relaci s více Připojeními katalogů pomocí Azure ARM hostování, je hlavní VDA virtuální počítač nemusí být v zastaveném (de přidělené) stavu.</span><span class="sxs-lookup"><span data-stu-id="b699d-211">To create new MCS catalogs using Azure ARM hosting, the master VDA VM is required to be in Stopped (de allocated) state.</span></span> <span data-ttu-id="b699d-212">Vypněte virtuální počítač z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-212">Shutdown the VM from Azure Portal.</span></span>

5. <span data-ttu-id="b699d-213">Skupina 4 převzetí služeb při selhání: Řadič doručování a virtuální počítače výkladní skříň serveru</span><span class="sxs-lookup"><span data-stu-id="b699d-213">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>
6. <span data-ttu-id="b699d-214">Skupina3 ruční nebo skript akce 1:</span><span class="sxs-lookup"><span data-stu-id="b699d-214">Group3 manual or script action 1:</span></span>

    <span data-ttu-id="b699d-215">***Přidat připojení hostitele Azure RM***</span><span class="sxs-lookup"><span data-stu-id="b699d-215">***Add Azure RM host connection***</span></span>

    <span data-ttu-id="b699d-216">Vytvořte připojení hostitele Azure ARM v doručení řadiče počítače zřídit nové relace s více Připojeními katalogů v Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-216">Create Azure ARM host connection in Delivery Controller machine to provision new MCS catalogs in Azure.</span></span> <span data-ttu-id="b699d-217">Postupujte podle kroků, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span><span class="sxs-lookup"><span data-stu-id="b699d-217">Follow the steps as explained in this [article](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span></span>

7. <span data-ttu-id="b699d-218">Skupina3 ruční nebo skript akce 2:</span><span class="sxs-lookup"><span data-stu-id="b699d-218">Group3 manual or script action 2:</span></span>

    <span data-ttu-id="b699d-219">***Znovu vytvořit relace s více Připojeními katalogů v Azure***</span><span class="sxs-lookup"><span data-stu-id="b699d-219">***Re-create MCS Catalogs in Azure***</span></span>

    <span data-ttu-id="b699d-220">Existující relace s více Připojeními nebo systémy současné hodnoty klony v primární lokalitě, nebude možné replikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-220">The existing MCS or PVS clones on the primary site will not be replicated to Azure.</span></span> <span data-ttu-id="b699d-221">Budete muset znovu vytvořit tyto klony pomocí replikované hlavní VDA a Azure ARM zřizování z řadiče doručení. Postupujte podle kroků, jak je popsáno v tomto [článku](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) k vytvoření relace s více Připojeními katalogů v Azure.</span><span class="sxs-lookup"><span data-stu-id="b699d-221">You need to recreate these clones using the replicated master VDA and Azure ARM provisioning from Delivery controller.Follow the steps as explained in this [article](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) to create MCS catalogs in Azure.</span></span>

![Plán obnovení pro XenApp součásti](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   ><span data-ttu-id="b699d-223">Můžete použít skripty v [umístění](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) k aktualizaci služby DNS s nové IP adresy došlo přes > virtuální počítače nebo pro připojení služby Vyrovnávání zatížení na neúspěšný přes virtuální počítač, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b699d-223">You can use scripts at [location](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) to update the DNS with the new IPs of the failed over >virtual machines or to attach a load balancer on the failed over virtual machine, if needed.</span></span>


## <a name="doing-a-test-failover"></a><span data-ttu-id="b699d-224">Provádění testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="b699d-224">Doing a test failover</span></span>

<span data-ttu-id="b699d-225">Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) provedete testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="b699d-225">Follow [this guidance](site-recovery-test-failover-to-azure.md) to do a test failover.</span></span>

![Plán obnovení](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a><span data-ttu-id="b699d-227">Převzetím služeb</span><span class="sxs-lookup"><span data-stu-id="b699d-227">Doing a failover</span></span>

<span data-ttu-id="b699d-228">Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="b699d-228">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b699d-229">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b699d-229">Next steps</span></span>

<span data-ttu-id="b699d-230">Můžete [Další](https://aka.ms/citrix-xenapp-xendesktop-with-asr) o replikaci Citrix XenApp a XenDesktop nasazení v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="b699d-230">You can [learn more](https://aka.ms/citrix-xenapp-xendesktop-with-asr) about replicating Citrix XenApp and XenDesktop deployments  in this white paper.</span></span> <span data-ttu-id="b699d-231">Podívejte se na pokyny, které [replikovat jiné aplikace](site-recovery-workload.md) pomocí Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="b699d-231">Look at the guidance to [replicate other applications](site-recovery-workload.md) using Site Recovery.</span></span>
