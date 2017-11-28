---
title: "aaaReplicate nasazení Dynamics AX vícevrstvé pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate a chránit pomocí Azure Site Recovery Dynamics AX"
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
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="8eeb4-103">Replikovat vícevrstvé aplikace Dynamics AX pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="8eeb4-103">Replicate a multi-tier Dynamics AX application using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="8eeb4-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8eeb4-104">Overview</span></span>


<span data-ttu-id="8eeb4-105">Aplikace Microsoft Dynamics AX je jedním z hello nejoblíbenější řešení ERP mezi podniky toostandardized proces v umístění, spravovat prostředky a zjednodušit dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-105">Microsoft Dynamics AX is one of hello most popular ERP solution among enterprises toostandardized process across locations, manage resources and simplifying compliance.</span></span> <span data-ttu-id="8eeb4-106">Considering aplikace hello je obchodní kritické tooan organizace je velmi důležité, aby toobe, pokud žádné po havárii, aplikace by spuštěná minimální hodnota času.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-106">Considering hello application is business critical tooan organization it is very important toobe sure that if any disaster, application should be up and running in minimum time.</span></span>

<span data-ttu-id="8eeb4-107">Dnes Microsoft Dynamics AX neposkytuje žádné možnosti obnovení po havárii se na pole.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-107">Today, Microsoft Dynamics AX  does not provide any out-of-the-box disaster recovery capabilities.</span></span> <span data-ttu-id="8eeb4-108">Aplikace Microsoft Dynamics AX se skládá z mnoha součásti serveru jako aplikace objektu, Active Directory (AD), SQL databázový Server, SharePoint Server, vytváření sestav atd. Server toomanage hello zotavení po havárii každou z těchto součástí je ručně nejen nákladná, ale také k chybám.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-108">Microsoft Dynamics AX consists of many server components like Application Object Server, Active Directory (AD), SQL Database Server, SharePoint Server, Reporting Server etc. toomanage hello disaster recovery of each of these components manually is not only expensive but also error-prone.</span></span>

<span data-ttu-id="8eeb4-109">Tento článek podrobně vysvětluje o tom, jak můžete vytvořit řešení zotavení po havárii pro aplikaci Dynamics AX pomocí [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-109">This article explains in detail about how you can create a disaster recovery solution for your Dynamics AX application using [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="8eeb4-110">Také vysvětluje plánované/neplánované nebo testovací převzetí služeb při selhání pomocí plánu obnovení jedním kliknutím, podporované konfigurace a požadavky.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-110">It also covers planned/unplanned/test failovers using one-click recovery plan, supported configurations, and prerequisites.</span></span>
<span data-ttu-id="8eeb4-111">Řešení zotavení po havárii Azure Site Recovery na základě plně otestovat, certifikaci a doporučení Microsoft Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-111">Azure Site Recovery based disaster recovery solution is fully tested, certified, and recommended by Microsoft Dynamics AX.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="8eeb4-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8eeb4-112">Prerequisites</span></span>

<span data-ttu-id="8eeb4-113">Implementace zotavení po havárii pro aplikaci Dynamics AX pomocí Azure Site Recovery vyžaduje hello následující předpoklady byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-113">Implementing disaster recovery for Dynamics AX application using Azure Site Recovery requires hello following pre-requisites completed.</span></span>

<span data-ttu-id="8eeb4-114">• Nasazení Dynamics AX místní byla nastavena</span><span class="sxs-lookup"><span data-stu-id="8eeb4-114">•   An on-premises Dynamics AX deployment has been set up</span></span>

<span data-ttu-id="8eeb4-115">• Služeb azure Site Recovery vault byla vytvořena v rámci předplatného Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8eeb4-115">•   Azure Site Recovery Services vault has been created in Microsoft Azure subscription</span></span>

<span data-ttu-id="8eeb4-116">• Pokud je váš web obnovení Azure hello posouzení připravenosti na virtuální počítač Azure v spusťte nástroj tooensure virtuálních počítačů, které jsou kompatibilní s virtuálními počítači Azure a služeb Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="8eeb4-116">•   If Azure is your recovery site, run hello Azure Virtual Machine Readiness Assessment tool  on VMs tooensure that they are compatible with Azure VMs and Azure Site Recovery Services</span></span>


## <a name="site-recovery-support"></a><span data-ttu-id="8eeb4-117">Podpora pro obnovení lokality</span><span class="sxs-lookup"><span data-stu-id="8eeb4-117">Site Recovery support</span></span>

<span data-ttu-id="8eeb4-118">Za účelem hello vytvoření v tomto článku používaly virtuální počítače VMware s 2012R3 Dynamics AX na Windows Server 2012 R2 Enterprise.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-118">For hello purpose of creating this article, VMware virtual machines with Dynamics AX  2012R3 on Windows Server 2012 R2 Enterprise were used.</span></span> <span data-ttu-id="8eeb4-119">Replikace obnovení lokality je nezávislá na aplikace, pokud hello doporučení tady jsou očekávané toohold také následujících scénářů.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-119">As site recovery replication is application agnostic, hello recommendations provided here are expected toohold on following scenarios as well.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="8eeb4-120">Zdroj a cíl</span><span class="sxs-lookup"><span data-stu-id="8eeb4-120">Source and target</span></span>

<span data-ttu-id="8eeb4-121">**Scénář**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-121">**Scenario**</span></span> | <span data-ttu-id="8eeb4-122">**tooa sekundární lokality**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-122">**tooa secondary site**</span></span> | <span data-ttu-id="8eeb4-123">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-123">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="8eeb4-124">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-124">**Hyper-V**</span></span> | <span data-ttu-id="8eeb4-125">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-125">Yes</span></span> | <span data-ttu-id="8eeb4-126">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-126">Yes</span></span>
<span data-ttu-id="8eeb4-127">**VMware**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-127">**VMware**</span></span> | <span data-ttu-id="8eeb4-128">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-128">Yes</span></span> | <span data-ttu-id="8eeb4-129">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-129">Yes</span></span>
<span data-ttu-id="8eeb4-130">**Fyzický server**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-130">**Physical server**</span></span> | <span data-ttu-id="8eeb4-131">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-131">Yes</span></span> | <span data-ttu-id="8eeb4-132">Ano</span><span class="sxs-lookup"><span data-stu-id="8eeb4-132">Yes</span></span>

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="8eeb4-133">Povolit zotavení po Havárii Dynamics AX aplikace pomocí Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="8eeb4-133">Enable DR of Dynamics AX application using Azure Site Recovery</span></span>
### <a name="protect-your-dynamics-ax-application"></a><span data-ttu-id="8eeb4-134">Ochrana aplikace Dynamics AX</span><span class="sxs-lookup"><span data-stu-id="8eeb4-134">Protect your Dynamics AX application</span></span>
<span data-ttu-id="8eeb4-135">Jednotlivé komponenty hello Dynamics AX potřebám toobe chráněný tooenable hello aplikace dokončena a replikace a obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-135">Each component of hello Dynamics AX needs toobe protected tooenable hello complete application replication and recovery.</span></span> <span data-ttu-id="8eeb4-136">Tato část zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="8eeb4-136">This section covers:</span></span>

<span data-ttu-id="8eeb4-137">**1. Ochrana služby Active Directory**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-137">**1. Protection of Active Directory**</span></span>

<span data-ttu-id="8eeb4-138">**2. Ochrana SQL vrstvy**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-138">**2. Protection of SQL Tier**</span></span>

<span data-ttu-id="8eeb4-139">**3. Ochrana a vrstvy webové aplikace**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-139">**3. Protection of App and Web Tiers**</span></span>

<span data-ttu-id="8eeb4-140">**4. Konfigurace sítě**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-140">**4. Networking configuration**</span></span>

<span data-ttu-id="8eeb4-141">**5. Plán obnovení**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-141">**5. Recovery Plan**</span></span>

### <a name="1-setup-ad-and-dns-replication"></a><span data-ttu-id="8eeb4-142">1. Instalační program AD a replikaci DNS</span><span class="sxs-lookup"><span data-stu-id="8eeb4-142">1. Setup AD and DNS replication</span></span>

<span data-ttu-id="8eeb4-143">Služby Active Directory je potřeba v lokalitě hello zotavení po Havárii pro toofunction aplikace Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-143">Active Directory is required on hello DR site for Dynamics AX application toofunction.</span></span> <span data-ttu-id="8eeb4-144">Existují dvě možnosti doporučené podle složitosti hello hello zákazníka v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-144">There are two recommended choices based on hello complexity of hello customer’s on-premises environment.</span></span>

<span data-ttu-id="8eeb4-145">**Možnost 1**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-145">**Option 1**</span></span>

<span data-ttu-id="8eeb4-146">Pokud má zákazník hello malý počet aplikací a jeden řadič domény pro jeho celý místní lokality a budou se přebírání služeb při selhání celý web hello společně, pak doporučujeme používat automatické obnovení systému replikace tooreplicate hello řadič domény počítače toosecondary lokality ( platí pro tooSite lokality a lokality tooAzure).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-146">If hello customer has a small number of applications and a single domain controller for his entire on-premises site and will be failing over hello entire site together, then we recommend using ASR-Replication tooreplicate hello DC machine toosecondary site (applicable for both Site tooSite and Site tooAzure).</span></span>

<span data-ttu-id="8eeb4-147">**Možnost 2**</span><span class="sxs-lookup"><span data-stu-id="8eeb4-147">**Option 2**</span></span>

<span data-ttu-id="8eeb4-148">Pokud zákazník hello má velký počet aplikací a běží doménové struktury služby Active Directory a bude převzetí služeb při selhání-několik aplikací najednou, pak doporučujeme nastavení dalšího řadiče domény v lokalitě hello zotavení po Havárii (sekundární lokality nebo v Azure).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-148">If hello customer has a large number of applications and is running an Active Directory forest and will fail-over few applications at a time, then we recommend setting up an additional domain controller on hello DR site (secondary site or in Azure).</span></span>

<span data-ttu-id="8eeb4-149">Naleznete příliš[doprovodná příručka na zpřístupnění řadič domény v lokalitě zotavení po Havárii](site-recovery-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-149">Please refer too[companion guide on making a domain controller available on DR site](site-recovery-active-directory.md).</span></span> <span data-ttu-id="8eeb4-150">Pro zbývající část tohoto dokumentu budeme předpokládat, že řadič domény k dispozici na webu zotavení po Havárii.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-150">For remainder of this document we will assume a DC is available on DR site.</span></span>

### <a name="2-setup-sql-server-replication"></a><span data-ttu-id="8eeb4-151">2. Nastavení replikace systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="8eeb4-151">2. Setup SQL Server replication</span></span>
<span data-ttu-id="8eeb4-152">Podrobné technické informace o hello doporučená možnost ochrany naleznete v příručce toocompanion [vrstvy SQL](site-recovery-sql.md).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-152">Please refer toocompanion guide  for detailed technical guidance on hello recommended option for protecting [SQL tier](site-recovery-sql.md).</span></span>

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a><span data-ttu-id="8eeb4-153">3. Povolit ochranu pro Dynamics AX klienta a AOS virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8eeb4-153">3. Enable protection for Dynamics AX client and AOS VMs</span></span>
<span data-ttu-id="8eeb4-154">Provést příslušné konfigurace Azure Site Recovery na tom, jestli jsou hello virtuální počítače nasazené na základě [technologie Hyper-V](site-recovery-hyper-v-site-to-azure.md) nebo na [VMware](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8eeb4-154">Perform relevant Azure Site Recovery configuration based on whether hello VMs are deployed on [Hyper-V](site-recovery-hyper-v-site-to-azure.md) or on [VMware](site-recovery-vmware-to-azure.md).</span></span>

> [!TIP]
> <span data-ttu-id="8eeb4-155">Doporučené tooconfigure konzistentní frekvence havárií je 15 minut.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-155">Recommended Crash consistent frequency tooconfigure is 15 minutes.</span></span>
>

<span data-ttu-id="8eeb4-156">Hello níže snímku se zobrazuje stav ochrany hello Dynamics součásti virtuálních počítačů v případě ochrany 'VMware lokality tooAzure'.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-156">hello below snapshot shows hello protection status of Dynamics component VMs in ‘VMware site tooAzure’ protection scenario.</span></span>
<span data-ttu-id="8eeb4-157">![Chráněné položky](./media/site-recovery-dynamics-ax/protecteditems.png)</span><span class="sxs-lookup"><span data-stu-id="8eeb4-157">![Protected items ](./media/site-recovery-dynamics-ax/protecteditems.png)</span></span>

### <a name="4-configure-networking"></a><span data-ttu-id="8eeb4-158">4. Konfigurace sítí</span><span class="sxs-lookup"><span data-stu-id="8eeb4-158">4. Configure Networking</span></span>
<span data-ttu-id="8eeb4-159">Konfigurace virtuálního počítače výpočty a síť nastavení</span><span class="sxs-lookup"><span data-stu-id="8eeb4-159">Configure VM Compute and Network Settings</span></span>

<span data-ttu-id="8eeb4-160">Hello AX klienta a virtuální počítače AOS konfigurace nastavení sítě v Azure Site Recovery, sítě virtuálních počítačů hello získat správného síťového zotavení po Havárii připojené toohello po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-160">For hello AX client and AOS VMs configure network settings in Azure Site Recovery so that hello VM networks get attached toohello right DR network after failover.</span></span> <span data-ttu-id="8eeb4-161">Zkontrolujte, zda síť hello zotavení po Havárii pro tyto vrstvy směrovatelné toohello SQL vrstvy.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-161">Ensure hello DR network for these tiers is routable toohello SQL tier.</span></span>

<span data-ttu-id="8eeb4-162">Můžete vybrat hello virtuálních počítačů v hello replikované položky tooconfigure hello síťová nastavení, jak je znázorněno níže hello snímku.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-162">You can select hello VM in hello replicated items tooconfigure hello network settings as shown in hello snapshot below.</span></span>

* <span data-ttu-id="8eeb4-163">Pro servery AOS vyberte skupinu dostupnosti správné hello.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-163">For AOS servers select hello correct availability set.</span></span>

* <span data-ttu-id="8eeb4-164">Pokud používáte statickou IP adresu můžete zadat hello IP, který chcete hello tootake virtuálního počítače v hello **cílová IP adresa** pole ![nastavení sítě](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span><span class="sxs-lookup"><span data-stu-id="8eeb4-164">If you are using a static IP then specify hello IP that you want hello virtual machine tootake in hello **Target IP** field ![Network Settings ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span></span>



### <a name="5-creating-a-recovery-plan"></a><span data-ttu-id="8eeb4-165">5. Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="8eeb4-165">5. Creating a recovery plan</span></span>

<span data-ttu-id="8eeb4-166">V Azure Site Recovery tooautomate hello převzetí služeb při selhání procesu můžete vytvořit plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-166">You can create a recovery plan in Azure Site Recovery tooautomate hello failover process.</span></span> <span data-ttu-id="8eeb4-167">Přidáte aplikaci vrstvy a webovou vrstvu v hello plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-167">Add app tier and web tier in hello Recovery Plan.</span></span> <span data-ttu-id="8eeb4-168">Pořadí je v různých skupinách, které hello front-end vypnutí před vrstvy aplikace.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-168">Order them in different groups so that hello front-end shutdown before app tier.</span></span>

1)  <span data-ttu-id="8eeb4-169">Vyberte hello trezoru Azure Site Recovery ve vašem předplatném a klikněte na dlaždici plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-169">Select hello Azure Site Recovery vault in your subscription and click on ‘Recovery Plans’ tile.</span></span>

2)  <span data-ttu-id="8eeb4-170">Klikněte na ' + plánování a zadat název obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-170">Click on ‘+ Recovery plan and specify a name.</span></span>

3)  <span data-ttu-id="8eeb4-171">Vyberte hello 'Source' a "Target".</span><span class="sxs-lookup"><span data-stu-id="8eeb4-171">Select hello ‘Source’ and ‘Target’.</span></span> <span data-ttu-id="8eeb4-172">cíl Hello může být Azure nebo sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-172">hello target can be Azure or secondary site.</span></span> <span data-ttu-id="8eeb4-173">V případě, že zvolíte Azure, je nutné zadat model nasazení hello</span><span class="sxs-lookup"><span data-stu-id="8eeb4-173">In case you choose Azure, you must specify hello deployment model</span></span>

![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  <span data-ttu-id="8eeb4-175">Vyberte hello AOS a plán obnovení toohello klientské virtuální počítače a klikněte na tlačítko ✓.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-175">Select hello AOS and client VMs toohello recovery plan and click ✓.</span></span>
<span data-ttu-id="8eeb4-176">![Vytvoření plánu obnovení](./media/site-recovery-dynamics-ax/selectvms.png)</span><span class="sxs-lookup"><span data-stu-id="8eeb4-176">![Create Recovery Plan](./media/site-recovery-dynamics-ax/selectvms.png)</span></span>


![Plán obnovení](./media/site-recovery-dynamics-ax/recoveryplan.png)

<span data-ttu-id="8eeb4-178">Plán obnovení hello pro aplikaci Dynamics AX můžete přizpůsobit přidáním různé kroky podle níže uvedeného postupu.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-178">You can customize hello recovery plan for Dynamics AX application by adding various steps as detailed below.</span></span> <span data-ttu-id="8eeb4-179">Hello výše snímku ukazuje hello plán dokončení obnovení po přidání všech kroků hello.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-179">hello above snapshot shows hello complete recovery plan after adding all hello steps.</span></span>

<span data-ttu-id="8eeb4-180">*Pomocí následujících kroků:*</span><span class="sxs-lookup"><span data-stu-id="8eeb4-180">*Steps:*</span></span>

<span data-ttu-id="8eeb4-181">*1. SQL Server převzít kroky*</span><span class="sxs-lookup"><span data-stu-id="8eeb4-181">*1. SQL Server fail over steps*</span></span>

<span data-ttu-id="8eeb4-182">Odkazovat příliš['Řešení zotavení po Havárii serveru SQL'](site-recovery-sql.md) doprovodná příručka podrobnosti o serveru konkrétní tooSQL kroky obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-182">Refer too[‘SQL Server DR Solution’](site-recovery-sql.md) companion guide  for details about recovery steps specific tooSQL server.</span></span>

<span data-ttu-id="8eeb4-183">*2. Převzetí služeb při selhání skupiny 1: Převzít služby virtuálních počítačů AOS hello*</span><span class="sxs-lookup"><span data-stu-id="8eeb4-183">*2. Failover Group 1: Fail over hello AOS VMs*</span></span>

<span data-ttu-id="8eeb4-184">Ujistěte se, že je vybraný bod obnovení hello co možná toohello databáze PIT, ale není dopředu.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-184">Make sure that hello recovery point selected is as close as possible toohello database PIT but not ahead.</span></span>

<span data-ttu-id="8eeb4-185">*3. Skript: Nástroj pro vyrovnávání zatížení přidat (pouze E-A)* přidat skript (prostřednictvím služby Azure automation) po skupině AOS virtuální počítač se zobrazí tooadd tooit nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-185">*3. Script: Add load balancer (Only E-A)* Add a script (via Azure automation) after AOS VM group comes up tooadd a load balancer tooit.</span></span> <span data-ttu-id="8eeb4-186">Tento úkol můžete použít skript toodo.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-186">You can use a script toodo this task.</span></span> <span data-ttu-id="8eeb4-187">Najdete v článku [jak tooadd službu Vyrovnávání zatížení pro zotavení po Havárii vícevrstvé aplikace](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span><span class="sxs-lookup"><span data-stu-id="8eeb4-187">Refer article [how tooadd load balancer for multi-tier application DR](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span></span>

<span data-ttu-id="8eeb4-188">*4. Skupiny převzetí služeb při selhání 2: Převzít služby hello AX klientské virtuální počítače.*</span><span class="sxs-lookup"><span data-stu-id="8eeb4-188">*4. Failover Group 2: Fail over hello AX client VMs.*</span></span>
<span data-ttu-id="8eeb4-189">Selhání hello webovou vrstvu virtuálních počítačů v rámci plánu obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-189">Fail over hello web tier VMs as part of hello recovery plan.</span></span>


### <a name="doing-a-test-failover"></a><span data-ttu-id="8eeb4-190">Provádění testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="8eeb4-190">Doing a test failover</span></span>

<span data-ttu-id="8eeb4-191">Odkazovat too'AD řešení zotavení po Havárii ' a 'Řešení zotavení po Havárii serveru SQL' doprovodných průvodců pro konkrétní tooAD aspekty a SQL server v uvedeném pořadí během testovacího převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-191">Refer too‘AD DR Solution ’ and ‘SQL Server DR solution ’ companion guides for considerations specific tooAD and SQL server respectively during Test Failover.</span></span>

1.  <span data-ttu-id="8eeb4-192">Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-192">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="8eeb4-193">Klikněte na plán obnovení hello vytvořené pro Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-193">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="8eeb4-194">Klikněte na "Testovací převzetí služeb".</span><span class="sxs-lookup"><span data-stu-id="8eeb4-194">Click on ‘Test Failover’.</span></span>
4.  <span data-ttu-id="8eeb4-195">Vyberte hello virtuální sítě toostart hello testovací převzetí služeb při selhání procesu.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-195">Select hello virtual network toostart hello test fail-over process.</span></span>
5.  <span data-ttu-id="8eeb4-196">Jakmile sekundární prostředí hello zapnutý, můžete provést vaše ověření.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-196">Once hello secondary environment is up, you can perform your validations.</span></span>
6.  <span data-ttu-id="8eeb4-197">Po dokončení ověření hello se můžete vybrat ověření dokončení a hello testovací převzetí služeb při selhání prostředí bude vyčištěna.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-197">Once hello validations are complete, you can select ‘Validations complete’ and hello test failover environment will be cleaned.</span></span>

<span data-ttu-id="8eeb4-198">Postupujte podle [v tomto návodu](site-recovery-test-failover-to-azure.md) toodo testovací převzetí služeb.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-198">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

### <a name="doing-a-failover"></a><span data-ttu-id="8eeb4-199">Převzetím služeb</span><span class="sxs-lookup"><span data-stu-id="8eeb4-199">Doing a failover</span></span>

1.  <span data-ttu-id="8eeb4-200">Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-200">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="8eeb4-201">Klikněte na plán obnovení hello vytvořené pro Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-201">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="8eeb4-202">Klikněte na 'převzetí služeb při selhání a vyberte 'Převzetí služeb při selhání'.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-202">Click on ‘Failover’ and select ‘ Failover’.</span></span>
4.  <span data-ttu-id="8eeb4-203">Vyberte hello Cílová síť a klikněte na tlačítko ✓ toostart hello převzetí služeb při selhání procesu.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-203">Select hello target network and click ✓ toostart hello failover process.</span></span>

<span data-ttu-id="8eeb4-204">Postupujte podle [v tomto návodu](site-recovery-failover.md) při dělají převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-204">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

### <a name="perform-a-failback"></a><span data-ttu-id="8eeb4-205">Provést navrácení služeb po obnovení</span><span class="sxs-lookup"><span data-stu-id="8eeb4-205">Perform a Failback</span></span>

<span data-ttu-id="8eeb4-206">Odkazovat too'SQL řešení zotavení po Havárii serveru, doprovodná Příručka pro server konkrétní tooSQL aspekty během navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-206">Refer too‘SQL Server DR Solution ’ companion guide for considerations specific tooSQL server during Failback.</span></span>

1.  <span data-ttu-id="8eeb4-207">Přejděte tooAzure portál a vyberte svůj trezor Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-207">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="8eeb4-208">Klikněte na plán obnovení hello vytvořené pro Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-208">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="8eeb4-209">Klikněte na 'převzetí služeb při selhání a vyberte převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-209">Click on ‘Failover’ and select failover.</span></span>
4.  <span data-ttu-id="8eeb4-210">Klikněte na 'Změnu Direction'.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-210">Click on ‘Change Direction’.</span></span>
5.  <span data-ttu-id="8eeb4-211">Vyberte požadované možnosti hello - synchronizace dat a možností vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8eeb4-211">Select hello appropriate options - data synchronization and VM creation options</span></span>
6.  <span data-ttu-id="8eeb4-212">Klikněte na tlačítko ✓ toostart hello 'Navrácení služeb po obnovení' proces.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-212">Click ✓ toostart hello ‘Failback’ process.</span></span>


<span data-ttu-id="8eeb4-213">Postupujte podle [v tomto návodu](site-recovery-failback-azure-to-vmware.md) při dělají navrácení služeb po obnovení.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-213">Follow [this guidance](site-recovery-failback-azure-to-vmware.md) when you are doing a failback.</span></span>

##<a name="summary"></a><span data-ttu-id="8eeb4-214">Souhrn</span><span class="sxs-lookup"><span data-stu-id="8eeb4-214">Summary</span></span>
<span data-ttu-id="8eeb4-215">Pomocí Azure Site Recovery, můžete vytvořit plán obnovení po havárii dokončení automatizované pro vaši aplikaci Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-215">Using Azure Site Recovery, you can create a complete automated disaster recovery plan for your Dynamics AX application.</span></span> <span data-ttu-id="8eeb4-216">Můžete zahájit převzetí služeb při selhání hello během několika sekund z libovolného místa v hello událostí přerušení a zprovoznění aplikace hello v minutách.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-216">You can initiate hello failover within seconds from anywhere in hello event of a disruption and get hello application up and running in minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8eeb4-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8eeb4-217">Next steps</span></span>
<span data-ttu-id="8eeb4-218">Čtení [jaké úlohy mohu ochránit?](site-recovery-workload.md) toolearn informace o ochraně podnikové úlohy s Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8eeb4-218">Read [What workloads can I protect?](site-recovery-workload.md) toolearn more about protecting enterprise workloads with Azure Site Recovery.</span></span>
