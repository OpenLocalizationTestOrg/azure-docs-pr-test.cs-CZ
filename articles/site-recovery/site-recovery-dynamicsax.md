---
title: "Replikovat pomocí Azure Site Recovery nasazení Dynamics AX vícevrstvé | Microsoft Docs"
description: "Tento článek popisuje, jak se budou replikovat a chránit pomocí Azure Site Recovery Dynamics AX"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: 03127c8f4841b67436c4819628319705af0b2cd5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="68624-103">Replikovat vícevrstvé aplikace Dynamics AX pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="68624-103">Replicate a multi-tier Dynamics AX application using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="68624-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="68624-104">Overview</span></span>


<span data-ttu-id="68624-105">Aplikace Microsoft Dynamics AX je jedním z nejoblíbenějších řešení ERP mezi podnikům standardizované proces v umístění, spravovat prostředky a zjednodušit dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="68624-105">Microsoft Dynamics AX is one of the most popular ERP solution among enterprises to standardized process across locations, manage resources and simplifying compliance.</span></span> <span data-ttu-id="68624-106">Considering aplikace je důležité pro organizaci obchodní je velmi důležité mít jistotu, že pokud žádné po havárii, aplikace musí být spuštěná ve minimální hodnota času.</span><span class="sxs-lookup"><span data-stu-id="68624-106">Considering the application is business critical to an organization it is very important to be sure that if any disaster, application should be up and running in minimum time.</span></span>

<span data-ttu-id="68624-107">Dnes Microsoft Dynamics AX neposkytuje žádné možnosti obnovení po havárii se na pole.</span><span class="sxs-lookup"><span data-stu-id="68624-107">Today, Microsoft Dynamics AX  does not provide any out-of-the-box disaster recovery capabilities.</span></span> <span data-ttu-id="68624-108">Aplikace Microsoft Dynamics AX se skládá z mnoha součásti serveru jako aplikace objektu, Active Directory (AD), SQL databázový Server, SharePoint Server sestav serveru atd. Ke správě po havárii je obnovení každou z těchto součástí ručně pouze nákladná, ale také k chybám.</span><span class="sxs-lookup"><span data-stu-id="68624-108">Microsoft Dynamics AX consists of many server components like Application Object Server, Active Directory (AD), SQL Database Server, SharePoint Server, Reporting Server etc. To manage the disaster recovery of each of these components manually is not only expensive but also error-prone.</span></span>

<span data-ttu-id="68624-109">Tento článek podrobně vysvětluje o tom, jak můžete vytvořit řešení zotavení po havárii pro aplikaci Dynamics AX pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="68624-109">This article explains in detail about how you can create a disaster recovery solution for your Dynamics AX application using [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="68624-110">Také vysvětluje plánované/neplánované nebo testovací převzetí služeb při selhání pomocí plánu obnovení jedním kliknutím, podporované konfigurace a požadavky.</span><span class="sxs-lookup"><span data-stu-id="68624-110">It also covers planned/unplanned/test failovers using one-click recovery plan, supported configurations, and prerequisites.</span></span>
<span data-ttu-id="68624-111">Řešení zotavení po havárii Azure Site Recovery na základě plně otestovat, certifikaci a doporučení Microsoft Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="68624-111">Azure Site Recovery based disaster recovery solution is fully tested, certified, and recommended by Microsoft Dynamics AX.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="68624-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="68624-112">Prerequisites</span></span>

<span data-ttu-id="68624-113">Implementace zotavení po havárii pro aplikaci Dynamics AX pomocí Azure Site Recovery vyžaduje následující požadavky byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="68624-113">Implementing disaster recovery for Dynamics AX application using Azure Site Recovery requires the following pre-requisites completed.</span></span>

<span data-ttu-id="68624-114">• Nasazení Dynamics AX místní byla nastavena</span><span class="sxs-lookup"><span data-stu-id="68624-114">•   An on-premises Dynamics AX deployment has been set up</span></span>

<span data-ttu-id="68624-115">• Služeb azure Site Recovery vault byla vytvořena v rámci předplatného Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="68624-115">•   Azure Site Recovery Services vault has been created in Microsoft Azure subscription</span></span>

<span data-ttu-id="68624-116">• Pokud je váš web obnovení Azure spustit nástroj hodnocení připravenosti virtuální počítač Azure na virtuálních počítačích zajistit, aby byly kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="68624-116">•   If Azure is your recovery site, run the Azure Virtual Machine Readiness Assessment tool  on VMs to ensure that they are compatible with Azure VMs and Azure Site Recovery Services</span></span>


## <a name="site-recovery-support"></a><span data-ttu-id="68624-117">Podpora pro obnovení lokality</span><span class="sxs-lookup"><span data-stu-id="68624-117">Site Recovery support</span></span>

<span data-ttu-id="68624-118">Pro účely vytváření tohoto článku, používaly virtuální počítače VMware s 2012R3 Dynamics AX na Windows Server 2012 R2 Enterprise.</span><span class="sxs-lookup"><span data-stu-id="68624-118">For the purpose of creating this article, VMware virtual machines with Dynamics AX  2012R3 on Windows Server 2012 R2 Enterprise were used.</span></span> <span data-ttu-id="68624-119">Replikace obnovení lokality je nezávislá na aplikace, doporučení uvedená tady se očekává počkejte také následující scénáře.</span><span class="sxs-lookup"><span data-stu-id="68624-119">As site recovery replication is application agnostic, the recommendations provided here are expected to hold on following scenarios as well.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="68624-120">Zdroj a cíl</span><span class="sxs-lookup"><span data-stu-id="68624-120">Source and target</span></span>

<span data-ttu-id="68624-121">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="68624-121">**Scenario**</span></span> | <span data-ttu-id="68624-122">**Sekundární lokality**</span><span class="sxs-lookup"><span data-stu-id="68624-122">**To a secondary site**</span></span> | <span data-ttu-id="68624-123">**Do Azure**</span><span class="sxs-lookup"><span data-stu-id="68624-123">**To Azure**</span></span>
--- | --- | ---
<span data-ttu-id="68624-124">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="68624-124">**Hyper-V**</span></span> | <span data-ttu-id="68624-125">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-125">Yes</span></span> | <span data-ttu-id="68624-126">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-126">Yes</span></span>
<span data-ttu-id="68624-127">**VMware**</span><span class="sxs-lookup"><span data-stu-id="68624-127">**VMware**</span></span> | <span data-ttu-id="68624-128">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-128">Yes</span></span> | <span data-ttu-id="68624-129">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-129">Yes</span></span>
<span data-ttu-id="68624-130">**Fyzický server**</span><span class="sxs-lookup"><span data-stu-id="68624-130">**Physical server**</span></span> | <span data-ttu-id="68624-131">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-131">Yes</span></span> | <span data-ttu-id="68624-132">Ano</span><span class="sxs-lookup"><span data-stu-id="68624-132">Yes</span></span>

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="68624-133">Povolit zotavení po Havárii Dynamics AX aplikace pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="68624-133">Enable DR of Dynamics AX application using Azure Site Recovery</span></span>
### <a name="protect-your-dynamics-ax-application"></a><span data-ttu-id="68624-134">Ochrana aplikace Dynamics AX</span><span class="sxs-lookup"><span data-stu-id="68624-134">Protect your Dynamics AX application</span></span>
<span data-ttu-id="68624-135">Jednotlivé komponenty Dynamics AX je potřeba chránit povolit aplikace dokončena a replikace a obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-135">Each component of the Dynamics AX needs to be protected to enable the complete application replication and recovery.</span></span> <span data-ttu-id="68624-136">Tato část zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="68624-136">This section covers:</span></span>

<span data-ttu-id="68624-137">**1. Ochrana služby Active Directory**</span><span class="sxs-lookup"><span data-stu-id="68624-137">**1. Protection of Active Directory**</span></span>

<span data-ttu-id="68624-138">**2. Ochrana SQL vrstvy**</span><span class="sxs-lookup"><span data-stu-id="68624-138">**2. Protection of SQL Tier**</span></span>

<span data-ttu-id="68624-139">**3. Ochrana a vrstvy webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="68624-139">**3. Protection of App and Web Tiers**</span></span>

<span data-ttu-id="68624-140">**4. Konfigurace sítě**</span><span class="sxs-lookup"><span data-stu-id="68624-140">**4. Networking configuration**</span></span>

<span data-ttu-id="68624-141">**5. Plán obnovení**</span><span class="sxs-lookup"><span data-stu-id="68624-141">**5. Recovery Plan**</span></span>

### <a name="1-setup-ad-and-dns-replication"></a><span data-ttu-id="68624-142">1. Instalační program AD a replikaci DNS</span><span class="sxs-lookup"><span data-stu-id="68624-142">1. Setup AD and DNS replication</span></span>

<span data-ttu-id="68624-143">Na webu zotavení po Havárii pro aplikaci Dynamics AX do funkce se vyžaduje služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="68624-143">Active Directory is required on the DR site for Dynamics AX application to function.</span></span> <span data-ttu-id="68624-144">Existují dvě možnosti doporučené podle složitosti zákazníka v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="68624-144">There are two recommended choices based on the complexity of the customer’s on-premises environment.</span></span>

<span data-ttu-id="68624-145">**Možnost 1**</span><span class="sxs-lookup"><span data-stu-id="68624-145">**Option 1**</span></span>

<span data-ttu-id="68624-146">Pokud zákazník má malý počet aplikací a jeden řadič domény pro jeho celý místní lokality a bude selhání celý web společně, pak doporučujeme používat automatické obnovení systému replikace pro replikaci počítače řadiče domény do sekundární lokality (platí pro jak na lokalitu a lokality do Azure).</span><span class="sxs-lookup"><span data-stu-id="68624-146">If the customer has a small number of applications and a single domain controller for his entire on-premises site and will be failing over the entire site together, then we recommend using ASR-Replication to replicate the DC machine to secondary site (applicable for both Site to Site and Site to Azure).</span></span>

<span data-ttu-id="68624-147">**Možnost 2**</span><span class="sxs-lookup"><span data-stu-id="68624-147">**Option 2**</span></span>

<span data-ttu-id="68624-148">Pokud zákazník má velký počet aplikací a běží doménové struktury služby Active Directory a bude převzetí služeb při selhání-několik aplikací najednou, pak doporučujeme nastavení dalšího řadiče domény v lokalitě zotavení po Havárii (sekundární lokality nebo v Azure).</span><span class="sxs-lookup"><span data-stu-id="68624-148">If the customer has a large number of applications and is running an Active Directory forest and will fail-over few applications at a time, then we recommend setting up an additional domain controller on the DR site (secondary site or in Azure).</span></span>

<span data-ttu-id="68624-149">Naleznete [doprovodná příručka na zpřístupnění řadič domény v lokalitě zotavení po Havárii](site-recovery-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="68624-149">Please refer to [companion guide on making a domain controller available on DR site](site-recovery-active-directory.md).</span></span> <span data-ttu-id="68624-150">Pro zbývající část tohoto dokumentu budeme předpokládat, že řadič domény k dispozici na webu zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="68624-150">For remainder of this document we will assume a DC is available on DR site.</span></span>

### <a name="2-setup-sql-server-replication"></a><span data-ttu-id="68624-151">2. Nastavení replikace systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="68624-151">2. Setup SQL Server replication</span></span>
<span data-ttu-id="68624-152">Doprovodná příručka podrobné technické informace na doporučené možnosti pro ochranu naleznete [vrstvy SQL](site-recovery-sql.md).</span><span class="sxs-lookup"><span data-stu-id="68624-152">Please refer to companion guide  for detailed technical guidance on the recommended option for protecting [SQL tier](site-recovery-sql.md).</span></span>

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a><span data-ttu-id="68624-153">3. Povolit ochranu pro Dynamics AX klienta a AOS virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="68624-153">3. Enable protection for Dynamics AX client and AOS VMs</span></span>
<span data-ttu-id="68624-154">Provést příslušné konfigurace Azure Site Recovery na tom, zda jsou virtuální počítače nasazené na základě [technologie Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo na [VMware](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="68624-154">Perform relevant Azure Site Recovery configuration based on whether the VMs are deployed on [Hyper-V](site-recovery-hyper-v-site-to-azure.md) or on [VMware](site-recovery-vmware-to-azure.md).</span></span>

> [!TIP]
> <span data-ttu-id="68624-155">Doporučená konzistentní frekvence havárií konfigurace je 15 minut.</span><span class="sxs-lookup"><span data-stu-id="68624-155">Recommended Crash consistent frequency to configure is 15 minutes.</span></span>
>

<span data-ttu-id="68624-156">Následující snímek zobrazuje stav ochrany virtuálních počítačů, Dynamics součásti v případě ochrany lokalita VMware do Azure.</span><span class="sxs-lookup"><span data-stu-id="68624-156">The below snapshot shows the protection status of Dynamics component VMs in ‘VMware site to Azure’ protection scenario.</span></span>
<span data-ttu-id="68624-157">![Chráněné položky](./media/site-recovery-dynamics-ax/protecteditems.png)</span><span class="sxs-lookup"><span data-stu-id="68624-157">![Protected items ](./media/site-recovery-dynamics-ax/protecteditems.png)</span></span>

### <a name="4-configure-networking"></a><span data-ttu-id="68624-158">4. Konfigurace sítí</span><span class="sxs-lookup"><span data-stu-id="68624-158">4. Configure Networking</span></span>
<span data-ttu-id="68624-159">Konfigurace virtuálního počítače výpočty a síť nastavení</span><span class="sxs-lookup"><span data-stu-id="68624-159">Configure VM Compute and Network Settings</span></span>

<span data-ttu-id="68624-160">Pro klienta AX a AOS virtuálních počítačů nakonfigurovat nastavení sítě v Azure Site Recovery, aby získat sítě virtuálních počítačů připojený k správného síťového zotavení po Havárii po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-160">For the AX client and AOS VMs configure network settings in Azure Site Recovery so that the VM networks get attached to the right DR network after failover.</span></span> <span data-ttu-id="68624-161">Zkontrolujte, zda síť zotavení po Havárii pro tyto vrstvy směrovat vrstvě SQL.</span><span class="sxs-lookup"><span data-stu-id="68624-161">Ensure the DR network for these tiers is routable to the SQL tier.</span></span>

<span data-ttu-id="68624-162">Virtuální počítač můžete vybrat v replikované položky konfigurace nastavení sítě, jak je znázorněno níže snímku.</span><span class="sxs-lookup"><span data-stu-id="68624-162">You can select the VM in the replicated items to configure the network settings as shown in the snapshot below.</span></span>

* <span data-ttu-id="68624-163">Pro servery AOS vyberte skupiny dostupnosti správný.</span><span class="sxs-lookup"><span data-stu-id="68624-163">For AOS servers select the correct availability set.</span></span>

* <span data-ttu-id="68624-164">Pokud používáte statickou IP adresu můžete zadat IP adresa, která má virtuální počítač má provést **cílová IP adresa** pole ![nastavení sítě](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span><span class="sxs-lookup"><span data-stu-id="68624-164">If you are using a static IP then specify the IP that you want the virtual machine to take in the **Target IP** field ![Network Settings ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span></span>



### <a name="5-creating-a-recovery-plan"></a><span data-ttu-id="68624-165">5. Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="68624-165">5. Creating a recovery plan</span></span>

<span data-ttu-id="68624-166">Ve službě Azure Site Recovery pro automatizaci procesu převzetí služeb při selhání můžete vytvořit plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-166">You can create a recovery plan in Azure Site Recovery to automate the failover process.</span></span> <span data-ttu-id="68624-167">Přidáte aplikaci vrstvy a webovou vrstvu v plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-167">Add app tier and web tier in the Recovery Plan.</span></span> <span data-ttu-id="68624-168">Pořadí je v různých skupinách, aby vrstvy front-end vypnutí před aplikací.</span><span class="sxs-lookup"><span data-stu-id="68624-168">Order them in different groups so that the front-end shutdown before app tier.</span></span>

1)  <span data-ttu-id="68624-169">Vyberte trezor Azure Site Recovery ve vašem předplatném a klikněte na dlaždici plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-169">Select the Azure Site Recovery vault in your subscription and click on ‘Recovery Plans’ tile.</span></span>

2)  <span data-ttu-id="68624-170">Klikněte na ' + plánování a zadat název obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-170">Click on ‘+ Recovery plan and specify a name.</span></span>

3)  <span data-ttu-id="68624-171">Vyberte 'Source' a "Target".</span><span class="sxs-lookup"><span data-stu-id="68624-171">Select the ‘Source’ and ‘Target’.</span></span> <span data-ttu-id="68624-172">Cílem může být Azure nebo sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="68624-172">The target can be Azure or secondary site.</span></span> <span data-ttu-id="68624-173">V případě, že zvolíte Azure, je nutné zadat model nasazení</span><span class="sxs-lookup"><span data-stu-id="68624-173">In case you choose Azure, you must specify the deployment model</span></span>

![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  <span data-ttu-id="68624-175">Vyberte AOS a klientských virtuálních počítačů do plánu obnovení a klikněte na tlačítko ✓.</span><span class="sxs-lookup"><span data-stu-id="68624-175">Select the AOS and client VMs to the recovery plan and click ✓.</span></span>
<span data-ttu-id="68624-176">![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/selectvms.png)</span><span class="sxs-lookup"><span data-stu-id="68624-176">![Create Recovery Plan](./media/site-recovery-dynamics-ax/selectvms.png)</span></span>


![Plán obnovení](./media/site-recovery-dynamics-ax/recoveryplan.png)

<span data-ttu-id="68624-178">Plán obnovení pro aplikaci Dynamics AX můžete přizpůsobit přidáním různé kroky podle níže uvedeného postupu.</span><span class="sxs-lookup"><span data-stu-id="68624-178">You can customize the recovery plan for Dynamics AX application by adding various steps as detailed below.</span></span> <span data-ttu-id="68624-179">Výše uvedený snímek zobrazuje plán dokončení obnovení po přidání všech kroků.</span><span class="sxs-lookup"><span data-stu-id="68624-179">The above snapshot shows the complete recovery plan after adding all the steps.</span></span>

<span data-ttu-id="68624-180">*Pomocí následujících kroků:*</span><span class="sxs-lookup"><span data-stu-id="68624-180">*Steps:*</span></span>

<span data-ttu-id="68624-181">*1. SQL Server převzít kroky*</span><span class="sxs-lookup"><span data-stu-id="68624-181">*1. SQL Server fail over steps*</span></span>

<span data-ttu-id="68624-182">Odkazovat na ['Řešení zotavení po Havárii serveru SQL'](site-recovery-sql.md) doprovodná příručka podrobné informace o krocích obnovení konkrétní k systému SQL server.</span><span class="sxs-lookup"><span data-stu-id="68624-182">Refer to [‘SQL Server DR Solution’](site-recovery-sql.md) companion guide  for details about recovery steps specific to SQL server.</span></span>

<span data-ttu-id="68624-183">*2. Převzetí služeb při selhání skupina 1: Převzetí služeb při selhání AOS virtuální počítače*</span><span class="sxs-lookup"><span data-stu-id="68624-183">*2. Failover Group 1: Fail over the AOS VMs*</span></span>

<span data-ttu-id="68624-184">Ujistěte se, že je vybraný bod obnovení co nejblíže k databázi PIT, ale není dopředu.</span><span class="sxs-lookup"><span data-stu-id="68624-184">Make sure that the recovery point selected is as close as possible to the database PIT but not ahead.</span></span>

<span data-ttu-id="68624-185">*3. Skript: Nástroj pro vyrovnávání zatížení přidat (pouze E-A)* přidat skript (prostřednictvím služby Azure automation) po skupiny virtuálních počítačů AOS zobrazí, můžete do ní přidejte služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="68624-185">*3. Script: Add load balancer (Only E-A)* Add a script (via Azure automation) after AOS VM group comes up to add a load balancer to it.</span></span> <span data-ttu-id="68624-186">K provedení této úlohy můžete použít skript.</span><span class="sxs-lookup"><span data-stu-id="68624-186">You can use a script to do this task.</span></span> <span data-ttu-id="68624-187">Najdete v článku [postup přidání služby Vyrovnávání zatížení pro zotavení po Havárii vícevrstvé aplikace](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span><span class="sxs-lookup"><span data-stu-id="68624-187">Refer article [how to add load balancer for multi-tier application DR](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span></span>

<span data-ttu-id="68624-188">*4. Skupiny převzetí služeb při selhání 2: Převzít služby AX klientské virtuální počítače.*</span><span class="sxs-lookup"><span data-stu-id="68624-188">*4. Failover Group 2: Fail over the AX client VMs.*</span></span>
<span data-ttu-id="68624-189">Při selhání se webová úroveň virtuální počítače jako součást plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-189">Fail over the web tier VMs as part of the recovery plan.</span></span>


### <a name="doing-a-test-failover"></a><span data-ttu-id="68624-190">Provádění testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="68624-190">Doing a test failover</span></span>

<span data-ttu-id="68624-191">Viz 'Řešení zotavení po Havárii AD' a 'Řešení zotavení po Havárii serveru SQL' doprovodné příručky informace o konkrétní do služby AD a SQL server v uvedeném pořadí během testovacího převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-191">Refer to ‘AD DR Solution ’ and ‘SQL Server DR solution ’ companion guides for considerations specific to AD and SQL server respectively during Test Failover.</span></span>

1.  <span data-ttu-id="68624-192">Přejděte na portál Azure a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="68624-192">Go to Azure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="68624-193">Kliknutím na vytvořit pro Dynamics AX plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-193">Click on the recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="68624-194">Klikněte na "Testovací převzetí služeb".</span><span class="sxs-lookup"><span data-stu-id="68624-194">Click on ‘Test Failover’.</span></span>
4.  <span data-ttu-id="68624-195">Vyberte virtuální síť, ke spuštění procesu testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-195">Select the virtual network to start the test fail-over process.</span></span>
5.  <span data-ttu-id="68624-196">Jakmile sekundární prostředí zapnutý, můžete provést vaše ověření.</span><span class="sxs-lookup"><span data-stu-id="68624-196">Once the secondary environment is up, you can perform your validations.</span></span>
6.  <span data-ttu-id="68624-197">Po dokončení se k ověření, můžete vybrat ověření dokončení a bude se vyčistit testovací převzetí služeb při selhání prostředí.</span><span class="sxs-lookup"><span data-stu-id="68624-197">Once the validations are complete, you can select ‘Validations complete’ and the test failover environment will be cleaned.</span></span>

<span data-ttu-id="68624-198">Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) provedete testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="68624-198">Follow [this guidance](site-recovery-test-failover-to-azure.md) to do a test failover.</span></span>

### <a name="doing-a-failover"></a><span data-ttu-id="68624-199">Převzetím služeb</span><span class="sxs-lookup"><span data-stu-id="68624-199">Doing a failover</span></span>

1.  <span data-ttu-id="68624-200">Přejděte na portál Azure a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="68624-200">Go to Azure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="68624-201">Kliknutím na vytvořit pro Dynamics AX plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-201">Click on the recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="68624-202">Klikněte na 'převzetí služeb při selhání a vyberte 'Převzetí služeb při selhání'.</span><span class="sxs-lookup"><span data-stu-id="68624-202">Click on ‘Failover’ and select ‘ Failover’.</span></span>
4.  <span data-ttu-id="68624-203">Vyberte cílová síť a klikněte na ✓ zahájíte proces převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-203">Select the target network and click ✓ to start the failover process.</span></span>

<span data-ttu-id="68624-204">Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-204">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

### <a name="perform-a-failback"></a><span data-ttu-id="68624-205">Provést navrácení služeb po obnovení</span><span class="sxs-lookup"><span data-stu-id="68624-205">Perform a Failback</span></span>

<span data-ttu-id="68624-206">Doprovodná příručka 'řešení zotavení po Havárii serveru SQL, informace o konkrétní k systému SQL server naleznete během navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-206">Refer to ‘SQL Server DR Solution ’ companion guide for considerations specific to SQL server during Failback.</span></span>

1.  <span data-ttu-id="68624-207">Přejděte na portál Azure a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="68624-207">Go to Azure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="68624-208">Kliknutím na vytvořit pro Dynamics AX plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-208">Click on the recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="68624-209">Klikněte na 'převzetí služeb při selhání a vyberte převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68624-209">Click on ‘Failover’ and select failover.</span></span>
4.  <span data-ttu-id="68624-210">Klikněte na 'Změnu Direction'.</span><span class="sxs-lookup"><span data-stu-id="68624-210">Click on ‘Change Direction’.</span></span>
5.  <span data-ttu-id="68624-211">Vyberte požadované možnosti - synchronizace dat a možností vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="68624-211">Select the appropriate options - data synchronization and VM creation options</span></span>
6.  <span data-ttu-id="68624-212">Klikněte na tlačítko ✓ ke spuštění procesu, navrácení služeb po obnovení'.</span><span class="sxs-lookup"><span data-stu-id="68624-212">Click ✓ to start the ‘Failback’ process.</span></span>


<span data-ttu-id="68624-213">Postupujte podle [v tomto návodu](site-recovery-failback-azure-to-vmware.md) při dělají navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="68624-213">Follow [this guidance](site-recovery-failback-azure-to-vmware.md) when you are doing a failback.</span></span>

##<a name="summary"></a><span data-ttu-id="68624-214">Souhrn</span><span class="sxs-lookup"><span data-stu-id="68624-214">Summary</span></span>
<span data-ttu-id="68624-215">Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro vaši aplikaci Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="68624-215">Using Azure Site Recovery, you can create a complete automated disaster recovery plan for your Dynamics AX application.</span></span> <span data-ttu-id="68624-216">Můžete spustit převzetí služeb při selhání během několika sekund z libovolného místa v případě přerušení a získání aplikace fungovaly v minutách.</span><span class="sxs-lookup"><span data-stu-id="68624-216">You can initiate the failover within seconds from anywhere in the event of a disruption and get the application up and running in minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68624-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68624-217">Next steps</span></span>
<span data-ttu-id="68624-218">Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) Další informace o ochraně podnikové úlohy s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="68624-218">Read [What workloads can I protect?](site-recovery-workload.md) to learn more about protecting enterprise workloads with Azure Site Recovery.</span></span>
