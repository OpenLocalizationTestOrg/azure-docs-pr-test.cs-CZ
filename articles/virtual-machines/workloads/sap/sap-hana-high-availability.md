---
title: "aaaHigh dostupnosti SAP HANA ve virtuálních počítačích Azure (VM) | Microsoft Docs"
description: "Vytvořte vysokou dostupnost SAP HANA na virtuálních počítačích Azure (VM)."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="f62ac-103">Vysoká dostupnost SAP HANA na virtuálních počítačích Azure (VM)</span><span class="sxs-lookup"><span data-stu-id="f62ac-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="f62ac-113">Na místě, můžete použít buď replikaci HANA systému nebo používat sdílené úložiště tooestablish vysokou dostupnost pro SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f62ac-113">On-premises, you can use either HANA System Replication or use shared storage tooestablish high availability for SAP HANA.</span></span>
<span data-ttu-id="f62ac-114">Momentálně podporujeme jenom nastavení replikace systému HANA v Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="f62ac-115">SAP HANA replikace se skládá z jednoho hlavní uzel a alespoň jeden podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="f62ac-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="f62ac-116">Změny toohello, jsou data na hlavní uzel hello replikují toohello podřízené uzly synchronně nebo asynchronně.</span><span class="sxs-lookup"><span data-stu-id="f62ac-116">Changes toohello data on hello master node are replicated toohello slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="f62ac-117">Tento článek popisuje, jak toodeploy hello virtuálních počítačů, konfiguraci hello virtuálních počítačů, nainstalujte rozhraní framework hello clusteru, nainstalovat a nakonfigurovat replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f62ac-117">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="f62ac-118">V hello příklad konfigurace instalace příkazy čísla instance atd 03 a HDB ID HANA systému se používá.</span><span class="sxs-lookup"><span data-stu-id="f62ac-118">In hello example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="f62ac-119">Přečtěte si následující poznámky k SAP a dokumenty Paper nejprve hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-119">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="f62ac-120">Poznámka SAP [1928533], na kterém je:</span><span class="sxs-lookup"><span data-stu-id="f62ac-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="f62ac-121">Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro hello nasazení SAP softwaru</span><span class="sxs-lookup"><span data-stu-id="f62ac-121">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="f62ac-122">Kapacita důležité informace o velikosti virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f62ac-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="f62ac-123">Podporované SAP software a operační systém (OS) a kombinace databáze</span><span class="sxs-lookup"><span data-stu-id="f62ac-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="f62ac-124">Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f62ac-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="f62ac-125">Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="f62ac-126">Poznámka SAP [2205917] se doporučuje nastavení operačního systému SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="f62ac-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f62ac-127">Poznámka SAP [1944799] má SAP HANA pokyny pro SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="f62ac-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f62ac-128">Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="f62ac-129">Poznámka SAP [2191498] hello vyžaduje verze SAP hostitele agenta pro Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-129">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="f62ac-130">Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="f62ac-131">Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="f62ac-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="f62ac-132">Poznámka SAP [1999351] obsahuje další informace o řešení potíží pro hello Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="f62ac-132">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="f62ac-133">[SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.</span><span class="sxs-lookup"><span data-stu-id="f62ac-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="f62ac-134">[Azure virtuálních počítačů, plánování a implementace pro SAP v systému Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="f62ac-135">[Nasazení virtuálních počítačů Azure pro SAP v systému Linux (v tomto článku)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="f62ac-136">[Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="f62ac-137">[SAP HANA SR výkonu optimalizované scénář] [ suse-hana-ha-guide] hello Průvodce obsahuje všechny požadované informace tooset replikaci systému SAP HANA místně.</span><span class="sxs-lookup"><span data-stu-id="f62ac-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="f62ac-138">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="f62ac-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="f62ac-139">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="f62ac-139">Deploying Linux</span></span>

<span data-ttu-id="f62ac-140">agent Hello prostředků pro SAP HANA je součástí systému SUSE Linux Enterprise Server pro aplikace SAP.</span><span class="sxs-lookup"><span data-stu-id="f62ac-140">hello resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="f62ac-141">Hello Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro SAP 12 aplikace s BYOS (přineste si vlastní předplatné), které můžete použít toodeploy nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f62ac-141">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use toodeploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="f62ac-142">Ruční nasazení</span><span class="sxs-lookup"><span data-stu-id="f62ac-142">Manual Deployment</span></span>

1. <span data-ttu-id="f62ac-143">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="f62ac-143">Create a Resource Group</span></span>
1. <span data-ttu-id="f62ac-144">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="f62ac-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="f62ac-145">Vytvořte dva účty úložiště</span><span class="sxs-lookup"><span data-stu-id="f62ac-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="f62ac-146">Vytvořit skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f62ac-146">Create an Availability Set</span></span>  
   <span data-ttu-id="f62ac-147">Sada maximální aktualizace domény</span><span class="sxs-lookup"><span data-stu-id="f62ac-147">Set max update domain</span></span>
1. <span data-ttu-id="f62ac-148">Vytvořit nástroj pro vyrovnávání zatížení (interní)</span><span class="sxs-lookup"><span data-stu-id="f62ac-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="f62ac-149">Vyberte virtuální síť kroku výše</span><span class="sxs-lookup"><span data-stu-id="f62ac-149">Select VNET of step above</span></span>
1. <span data-ttu-id="f62ac-150">Vytvoření virtuálního počítače 1</span><span class="sxs-lookup"><span data-stu-id="f62ac-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="f62ac-151">https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="f62ac-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="f62ac-152">SLES pro SAP aplikace 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="f62ac-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="f62ac-153">Vyberte účet úložiště 1</span><span class="sxs-lookup"><span data-stu-id="f62ac-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="f62ac-154">Vyberte sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f62ac-154">Select Availability Set</span></span>  
1. <span data-ttu-id="f62ac-155">Vytvoření virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="f62ac-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="f62ac-156">https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="f62ac-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="f62ac-157">SLES pro SAP aplikace 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="f62ac-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="f62ac-158">Vyberte účet úložiště 2</span><span class="sxs-lookup"><span data-stu-id="f62ac-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="f62ac-159">Vyberte sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f62ac-159">Select Availability Set</span></span>  
1. <span data-ttu-id="f62ac-160">Přidat datových disků</span><span class="sxs-lookup"><span data-stu-id="f62ac-160">Add Data Disks</span></span>
1. <span data-ttu-id="f62ac-161">Konfigurace pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-161">Configure hello load balancer</span></span>
    1. <span data-ttu-id="f62ac-162">Vytvořte fond IP front-endu</span><span class="sxs-lookup"><span data-stu-id="f62ac-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="f62ac-163">Otevřete nástroj pro vyrovnávání zatížení hello, vyberte fond IP front-endu a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="f62ac-163">Open hello load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="f62ac-164">Zadejte název hello hello nové front-endu fond IP adres (například hana-front-endu)</span><span class="sxs-lookup"><span data-stu-id="f62ac-164">Enter hello name of hello new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="f62ac-165">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f62ac-165">Click OK</span></span>
        1. <span data-ttu-id="f62ac-166">Jakmile je vytvořen fond IP front-endu nové hello, zapište si jeho IP adresu</span><span class="sxs-lookup"><span data-stu-id="f62ac-166">After hello new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="f62ac-167">Vytvořte fond back-end</span><span class="sxs-lookup"><span data-stu-id="f62ac-167">Create a backend pool</span></span>
        1. <span data-ttu-id="f62ac-168">Otevřete nástroj pro vyrovnávání zatížení hello, zvolte back-endové fondy a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="f62ac-168">Open hello load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="f62ac-169">Zadejte název hello hello nový back-end fondu (například hana-back-end)</span><span class="sxs-lookup"><span data-stu-id="f62ac-169">Enter hello name of hello new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="f62ac-170">Klikněte na tlačítko Přidat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f62ac-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="f62ac-171">Vyberte hello jste dříve vytvořili sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f62ac-171">Select hello Availability Set you created earlier</span></span>
        1. <span data-ttu-id="f62ac-172">Vyberte hello virtuální počítače clusteru hello SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f62ac-172">Select hello virtual machines of hello SAP HANA cluster</span></span>
        1. <span data-ttu-id="f62ac-173">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f62ac-173">Click OK</span></span>
    1. <span data-ttu-id="f62ac-174">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="f62ac-174">Create a health probe</span></span>
       1. <span data-ttu-id="f62ac-175">Otevřete nástroj pro vyrovnávání zatížení hello, zvolte sondy stavu služby a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="f62ac-175">Open hello load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="f62ac-176">Zadejte název hello hello nové kontroly stavu (například hana-hp)</span><span class="sxs-lookup"><span data-stu-id="f62ac-176">Enter hello name of hello new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="f62ac-177">Vyberte TCP jako protokol, port 625**03**, zachovat Interval 5 a prahová hodnota špatného stavu 2</span><span class="sxs-lookup"><span data-stu-id="f62ac-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="f62ac-178">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f62ac-178">Click OK</span></span>
    1. <span data-ttu-id="f62ac-179">Vytvoření pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f62ac-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="f62ac-180">Otevřete nástroj pro vyrovnávání zatížení hello, zvolte pravidla Vyrovnávání zatížení a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="f62ac-180">Open hello load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="f62ac-181">Zadejte název hello hello nové pravidlo Vyrovnávání zatížení (například hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="f62ac-181">Enter hello name of hello new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="f62ac-182">Vyberte hello front-endovou IP adresu, back-endový fond a stav testu jste vytvořili dříve (například hana-front-endu)</span><span class="sxs-lookup"><span data-stu-id="f62ac-182">Select hello frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="f62ac-183">Zachovat protokol TCP, zadejte port 3**03**15</span><span class="sxs-lookup"><span data-stu-id="f62ac-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="f62ac-184">Zvýšit časový limit nečinnosti too30 minut</span><span class="sxs-lookup"><span data-stu-id="f62ac-184">Increase idle timeout too30 minutes</span></span>
       1. <span data-ttu-id="f62ac-185">**Ujistěte se, že tooenable plovoucí IP adresa**</span><span class="sxs-lookup"><span data-stu-id="f62ac-185">**Make sure tooenable Floating IP**</span></span>
        1. <span data-ttu-id="f62ac-186">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f62ac-186">Click OK</span></span>
        1. <span data-ttu-id="f62ac-187">Opakujte kroky hello výše pro port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="f62ac-187">Repeat hello steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="f62ac-188">Nasazení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="f62ac-188">Deploy with template</span></span>
<span data-ttu-id="f62ac-189">Můžete vytvořit jednu z šablon úvodní hello na githubu toodeploy všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="f62ac-189">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="f62ac-190">Šablona Hello nasadí hello virtuální počítače, nástroj pro vyrovnávání zatížení hello, dostupnosti atd. Postupujte podle těchto kroků toodeploy hello šablony:</span><span class="sxs-lookup"><span data-stu-id="f62ac-190">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="f62ac-191">Otevřete hello [databáze šablony] [ template-multisid-db] nebo hello [konvergované šablony] [ template-converged] na hello portálu Azure vytvoří šablona databáze hello pouze hello pravidla Vyrovnávání zatížení pro databázi zatímco hello sblížené šablona vytvoří také hello pravidla Vyrovnávání zatížení pro služby ASC nebo SCS a instance YBRAT (pouze Linux).</span><span class="sxs-lookup"><span data-stu-id="f62ac-191">Open hello [database template][template-multisid-db] or hello [converged template][template-converged] on hello Azure Portal hello database template only creates hello load-balancing rules for a database whereas hello converged template also creates hello load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="f62ac-192">Pokud máte v plánu tooinstall SAP NetWeaver na základě systému a chcete tooinstall hello ASC nebo SCS instance na hello stejné počítače, použijte hello [konvergované šablony][template-converged].</span><span class="sxs-lookup"><span data-stu-id="f62ac-192">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello ASCS/SCS instance on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="f62ac-193">Zadejte následující parametry hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-193">Enter hello following parameters</span></span>
    1. <span data-ttu-id="f62ac-194">Id systému SAP</span><span class="sxs-lookup"><span data-stu-id="f62ac-194">Sap System Id</span></span>  
       <span data-ttu-id="f62ac-195">Zadejte systému SAP hello Id hello chcete tooinstall systému SAP.</span><span class="sxs-lookup"><span data-stu-id="f62ac-195">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="f62ac-196">Hello Id se použije jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="f62ac-196">hello Id will be used as a prefix for hello resources that are deployed.</span></span>
    1. <span data-ttu-id="f62ac-197">Typ zásobníku (platí pouze pokud použijete šablonu sblížené hello)</span><span class="sxs-lookup"><span data-stu-id="f62ac-197">Stack Type (only applicable if you use hello converged template)</span></span>  
       <span data-ttu-id="f62ac-198">Vyberte typ zásobníku SAP NetWeaver hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-198">Select hello SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="f62ac-199">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="f62ac-199">Os Type</span></span>  
       <span data-ttu-id="f62ac-200">Vyberte jednu z Linuxových distribucích hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-200">Select one of hello Linux distributions.</span></span> <span data-ttu-id="f62ac-201">V tomto příkladu vyberte SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="f62ac-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="f62ac-202">Typ databázového</span><span class="sxs-lookup"><span data-stu-id="f62ac-202">Db Type</span></span>  
       <span data-ttu-id="f62ac-203">Vyberte HANA</span><span class="sxs-lookup"><span data-stu-id="f62ac-203">Select HANA</span></span>
    1. <span data-ttu-id="f62ac-204">Velikost systému SAP</span><span class="sxs-lookup"><span data-stu-id="f62ac-204">Sap System Size</span></span>  
       <span data-ttu-id="f62ac-205">poskytne Hello množství protokoly SAP hello nový systém.</span><span class="sxs-lookup"><span data-stu-id="f62ac-205">hello amount of SAPS hello new system will provide.</span></span> <span data-ttu-id="f62ac-206">Pokud si nejste jisti, kolik systému hello protokoly SAP bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor</span><span class="sxs-lookup"><span data-stu-id="f62ac-206">If you are not sure how many SAPS hello system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="f62ac-207">Dostupnost systému</span><span class="sxs-lookup"><span data-stu-id="f62ac-207">System Availability</span></span>  
       <span data-ttu-id="f62ac-208">Vyberte HA</span><span class="sxs-lookup"><span data-stu-id="f62ac-208">Select HA</span></span>
    1. <span data-ttu-id="f62ac-209">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="f62ac-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="f62ac-210">Po vytvoření nového uživatele, který lze použít toolog na toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="f62ac-210">A new user is created that can be used toolog on toohello machine.</span></span>
    1. <span data-ttu-id="f62ac-211">Nový nebo existující podsíť</span><span class="sxs-lookup"><span data-stu-id="f62ac-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="f62ac-212">Určuje, zda mají být vytvořeny nové virtuální sítě a podsítě nebo by měl použít existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="f62ac-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="f62ac-213">Pokud již máte virtuální síť, která je připojená tooyour do místní sítě, vyberte existující.</span><span class="sxs-lookup"><span data-stu-id="f62ac-213">If you already have a virtual network that is connected tooyour on-premises network, select existing.</span></span>
    1. <span data-ttu-id="f62ac-214">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="f62ac-214">Subnet Id</span></span>  
    <span data-ttu-id="f62ac-215">ID Hello hello podsíť toowhich hello virtuálních počítačů musí být připojené k.</span><span class="sxs-lookup"><span data-stu-id="f62ac-215">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="f62ac-216">Vyberte podsíť hello VPN nebo Express Route virtuální sítě tooconnect hello virtuálního počítače tooyour místní sítě.</span><span class="sxs-lookup"><span data-stu-id="f62ac-216">Select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="f62ac-217">Hello ID obvykle vypadá /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="f62ac-217">hello ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="f62ac-218">Nastavení Linux HA</span><span class="sxs-lookup"><span data-stu-id="f62ac-218">Setting up Linux HA</span></span>

<span data-ttu-id="f62ac-219">Hello následující položky jsou předponou buď [A] - použít tooall uzly, [1] - platí jenom toonode, 1 nebo [2] - platí jenom toonode 2.</span><span class="sxs-lookup"><span data-stu-id="f62ac-219">hello following items are prefixed with either [A] - applicable tooall nodes, [1] - only applicable toonode 1 or [2] - only applicable toonode 2.</span></span>

1. <span data-ttu-id="f62ac-220">[A] SLES pro SAP BYOS jenom - zaregistrovat SLES toobe možné toouse hello úložiště</span><span class="sxs-lookup"><span data-stu-id="f62ac-220">[A] SLES for SAP BYOS only - Register SLES toobe able toouse hello repositories</span></span>
1. <span data-ttu-id="f62ac-221">[A] SLES pro SAP BYOS pouze – přidání modulu veřejného cloudu</span><span class="sxs-lookup"><span data-stu-id="f62ac-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="f62ac-222">[A] aktualizace SLES</span><span class="sxs-lookup"><span data-stu-id="f62ac-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="f62ac-223">[1] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f62ac-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="f62ac-224">[2] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f62ac-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="f62ac-225">[1] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f62ac-225">[1] Enable ssh access</span></span>
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="f62ac-226">[A] nainstalovat rozšíření HA</span><span class="sxs-lookup"><span data-stu-id="f62ac-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="f62ac-227">[A] rozložení disku instalační program</span><span class="sxs-lookup"><span data-stu-id="f62ac-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="f62ac-228">LVM</span><span class="sxs-lookup"><span data-stu-id="f62ac-228">LVM</span></span>  
    <span data-ttu-id="f62ac-229">Obecně doporučujeme toouse LVM pro svazky, které ukládají data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="f62ac-229">We generally recommend toouse LVM for volumes that store data and log files.</span></span> <span data-ttu-id="f62ac-230">Následující příklad Hello předpokládá, že máte hello virtuální počítače čtyři datových disků připojených, které by měly být použité toocreate dva svazky.</span><span class="sxs-lookup"><span data-stu-id="f62ac-230">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
        * <span data-ttu-id="f62ac-231">Vytvořte pro všechny disky, které chcete toouse fyzických svazků.</span><span class="sxs-lookup"><span data-stu-id="f62ac-231">Create physical volumes for all disks that you want toouse.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="f62ac-232">Vytvořit skupinu svazku pro hello datové soubory, jedna skupina svazku pro soubory protokolu hello a jeden pro sdílený adresář hello SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f62ac-232">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="f62ac-233">Vytvoření logické svazky hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-233">Create hello logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="f62ac-234">Vytvořte hello připojení adresáře a zkopírujte hello UUID všechny logické svazky</span><span class="sxs-lookup"><span data-stu-id="f62ac-234">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="f62ac-235">Vytvořit záznamy fstab pro hello tři logické svazky</span><span class="sxs-lookup"><span data-stu-id="f62ac-235">Create fstab entries for hello three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="f62ac-236">Vložit tento řádek příliš/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="f62ac-236">Insert this line too/etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="f62ac-237">Připojit nové svazky hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-237">Mount hello new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="f62ac-238">Nešifrovaná disky</span><span class="sxs-lookup"><span data-stu-id="f62ac-238">Plain Disks</span></span>  
       <span data-ttu-id="f62ac-239">Pro malé nebo ukázku systémů, můžete umístit HANA soubory protokolu a data na jeden disk.</span><span class="sxs-lookup"><span data-stu-id="f62ac-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="f62ac-240">Hello následující příkazy na /dev/sdc vytvořit oddíl a naformátovat ho s xfs.</span><span class="sxs-lookup"><span data-stu-id="f62ac-240">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a><span data-ttu-id="f62ac-241">Poznamenejte si hello id /dev/sdc1</span><span class="sxs-lookup"><span data-stu-id="f62ac-241">write down hello id of /dev/sdc1</span></span>
    <span data-ttu-id="f62ac-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="f62ac-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="f62ac-243">[A] instalační program rozlišení názvu hostitele pro všechny hostitele</span><span class="sxs-lookup"><span data-stu-id="f62ac-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="f62ac-244">Můžete buď použít DNS server nebo úpravám hello/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="f62ac-244">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="f62ac-245">Tento příklad ukazuje, jak toouse hello soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f62ac-245">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="f62ac-246">Nahraďte hello IP adresu a název hostitele hello v hello následující příkazy</span><span class="sxs-lookup"><span data-stu-id="f62ac-246">Replace hello IP address and hello hostname in hello following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="f62ac-247">Následující hello vložení řádků příliš/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f62ac-247">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="f62ac-248">Změnit hello IP adresu a název hostitele toomatch prostředí</span><span class="sxs-lookup"><span data-stu-id="f62ac-248">Change hello IP address and hostname toomatch your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="f62ac-249">[1] instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="f62ac-250">[2] přidat uzel toocluster</span><span class="sxs-lookup"><span data-stu-id="f62ac-250">[2] Add node toocluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="f62ac-251">[A] změna hacluster heslo toohello stejné heslo</span><span class="sxs-lookup"><span data-stu-id="f62ac-251">[A] Change hacluster password toohello same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="f62ac-252">[A] konfigurace corosync toouse jiných přenosu a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="f62ac-252">[A] Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="f62ac-253">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="f62ac-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="f62ac-254">Přidejte následující soubor tučné obsahu toohello hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-254">Add hello following bold content toohello file.</span></span>
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    <span data-ttu-id="f62ac-255">Potom restartujte službu corosync hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-255">Then restart hello corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="f62ac-256">[A] instalovat balíčky HANA HA</span><span class="sxs-lookup"><span data-stu-id="f62ac-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="f62ac-257">Instalace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f62ac-257">Installing SAP HANA</span></span>

<span data-ttu-id="f62ac-258">Postupujte podle kapitoly 4 hello [SAP HANA SR výkonu optimalizované scénář průvodce] [ suse-hana-ha-guide] tooinstall replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f62ac-258">Follow chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span>

1. <span data-ttu-id="f62ac-259">[A] spusťte hdblcm z hello HANA DVD</span><span class="sxs-lookup"><span data-stu-id="f62ac-259">[A] Run hdblcm from hello HANA DVD</span></span>
    * <span data-ttu-id="f62ac-260">Zvolte instalace-1 ></span><span class="sxs-lookup"><span data-stu-id="f62ac-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="f62ac-261">Vyberte další součásti k instalaci -> 1</span><span class="sxs-lookup"><span data-stu-id="f62ac-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="f62ac-262">Zadejte instalační cestu [/ hana/sdílené]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-263">Zadejte název místního hostitele [.]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-264">Chcete, aby tooadd další hostitele toohello systému?</span><span class="sxs-lookup"><span data-stu-id="f62ac-264">Do you want tooadd additional hosts toohello system?</span></span> <span data-ttu-id="f62ac-265">(Ano/Ne) [n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-266">Zadejte ID systému SAP HANA:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="f62ac-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="f62ac-267">Zadejte čísla Instance [00]:</span><span class="sxs-lookup"><span data-stu-id="f62ac-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="f62ac-268">Čísla HANA Instance.</span><span class="sxs-lookup"><span data-stu-id="f62ac-268">HANA Instance number.</span></span> <span data-ttu-id="f62ac-269">Použijte 03, pokud používá hello šablony Azure nebo postupovali podle výše uvedeném příkladu hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-269">Use 03 if you used hello Azure Template or followed hello example above</span></span>
    * <span data-ttu-id="f62ac-270">Vyberte režim databáze / zadejte Index [1]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-271">Vyberte použití systému / zadejte Index [4]:</span><span class="sxs-lookup"><span data-stu-id="f62ac-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="f62ac-272">Vyberte systém hello využití</span><span class="sxs-lookup"><span data-stu-id="f62ac-272">Select hello system Usage</span></span>
    * <span data-ttu-id="f62ac-273">Zadejte umístění datových svazků [/ hana/data/HDB]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-274">Zadejte umístění protokolu svazků [/ hana/log/HDB]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-275">Omezení přidělení paměti maximální?</span><span class="sxs-lookup"><span data-stu-id="f62ac-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="f62ac-276">[n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-277">Zadejte název hostitele certifikát pro hostitele,..." [...]: -> Zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-277">Enter Certificate Host Name For Host '...' [...]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-278">Zadejte SAP hostitele agenta uživatele (sapadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="f62ac-278">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="f62ac-279">Potvrďte SAP hostitele agenta uživatele (sapadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="f62ac-279">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="f62ac-280">Zadejte správce systému (hdbadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="f62ac-280">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="f62ac-281">Potvrzení správce systému (hdbadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="f62ac-281">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="f62ac-282">Zadejte domovský adresář správce systému [/ usr/sap nebo HDB/domovské]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-282">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-283">Zadejte prostředí přihlášení správce systému [/ bin/dílet]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-283">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-284">Zadejte ID uživatele správce systému [1001]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-284">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-285">Zadejte ID ze skupiny uživatelů (sapsys) [79]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-285">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-286">Zadejte heslo k databázi uživatelů (systém):</span><span class="sxs-lookup"><span data-stu-id="f62ac-286">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="f62ac-287">Potvrďte heslo k databázi uživatelů (systém):</span><span class="sxs-lookup"><span data-stu-id="f62ac-287">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="f62ac-288">Restartování systému po restartování počítače?</span><span class="sxs-lookup"><span data-stu-id="f62ac-288">Restart system after machine reboot?</span></span> <span data-ttu-id="f62ac-289">[n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="f62ac-289">[n]: -> ENTER</span></span>
    * <span data-ttu-id="f62ac-290">Chcete toocontinue?</span><span class="sxs-lookup"><span data-stu-id="f62ac-290">Do you want toocontinue?</span></span> <span data-ttu-id="f62ac-291">(Ano/Ne):</span><span class="sxs-lookup"><span data-stu-id="f62ac-291">(y/n):</span></span>  
  <span data-ttu-id="f62ac-292">Ověřit hello souhrnné a zadejte y toocontinue</span><span class="sxs-lookup"><span data-stu-id="f62ac-292">Validate hello summary and enter y toocontinue</span></span>
1. <span data-ttu-id="f62ac-293">[A] Agent hostitele upgradu SAP</span><span class="sxs-lookup"><span data-stu-id="f62ac-293">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="f62ac-294">Stáhnout nejnovější archivu Agent hostitele SAP hello z hello [SAP Softwarecenter] [ sap-swcenter] a spusťte hello následující příkaz tooupgrade hello agenta.</span><span class="sxs-lookup"><span data-stu-id="f62ac-294">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="f62ac-295">Nahraďte hello cesta toohello toopoint toohello soubor archivu, které jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="f62ac-295">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. <span data-ttu-id="f62ac-296">[1] vytvoření HANA replikace (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f62ac-296">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="f62ac-297">Spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-297">Run hello following command.</span></span> <span data-ttu-id="f62ac-298">Ujistěte se, že tooreplace tučné řetězce (HANA systému ID HDB a číslo instance 03) s hodnotami hello instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f62ac-298">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="f62ac-299">[A] vytvoření položky úložiště klíčů (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f62ac-299">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="f62ac-300">[1] zálohy databáze (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f62ac-300">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="f62ac-301">[1] přepněte toohello sapsid uživatele (například hdbadm) a vytvořit hello primární lokality.</span><span class="sxs-lookup"><span data-stu-id="f62ac-301">[1] Switch toohello sapsid user (for example hdbadm) and create hello primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="f62ac-302">[2] přepněte toohello sapsid uživatele (například hdbadm) a vytvořit hello sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="f62ac-302">[2] Switch toohello sapsid user (for example hdbadm) and create hello secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="f62ac-303">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-303">Configure Cluster Framework</span></span>

<span data-ttu-id="f62ac-304">Změňte výchozí nastavení hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-304">Change hello default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="f62ac-305">Nyní se nám načíst hello souboru toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-305">now we load hello file toohello cluster</span></span>
<span data-ttu-id="f62ac-306">sudo crm nakonfigurovat aktualizace zatížení crm-defaults.txt</span><span class="sxs-lookup"><span data-stu-id="f62ac-306">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="f62ac-307">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f62ac-307">Create STONITH device</span></span>

<span data-ttu-id="f62ac-308">zařízení STONITH Hello používá tooauthorize objekt služby pro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f62ac-308">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="f62ac-309">Postupujte podle těchto kroků toocreate hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="f62ac-309">Please follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="f62ac-310">Přejděte příliš<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="f62ac-310">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="f62ac-311">Okno Azure Active Directory otevřete hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-311">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="f62ac-312">Přejděte tooProperties a zapište hello ID adresáře. Toto je hello **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="f62ac-312">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="f62ac-313">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="f62ac-313">Click App registrations</span></span>
1. <span data-ttu-id="f62ac-314">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f62ac-314">Click Add</span></span>
1. <span data-ttu-id="f62ac-315">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="f62ac-315">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="f62ac-316">Hello přihlašovací adresa URL se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="f62ac-316">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="f62ac-317">Vyberte hello nové aplikace a klikněte na tlačítko klíče na kartě nastavení hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-317">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="f62ac-318">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="f62ac-318">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="f62ac-319">Poznamenejte si hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-319">Write down hello Value.</span></span> <span data-ttu-id="f62ac-320">Použije se jako hello **heslo** pro hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="f62ac-320">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="f62ac-321">Zapište hello ID aplikace. Použije se jako hello uživatelské jméno (**přihlašovacího id** v následujících kroků hello) z hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="f62ac-321">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="f62ac-322">Hello instanční objekt nemá oprávnění tooaccess vašich prostředků Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f62ac-322">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="f62ac-323">Je třeba toogive hello instanční objekt oprávnění toostart a zastavení (zrušit přidělení) všechny virtuální počítače clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-323">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="f62ac-324">Přejděte toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f62ac-324">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="f62ac-325">Otevřete hello okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="f62ac-325">Open hello All resources blade</span></span>
1. <span data-ttu-id="f62ac-326">Vyberte virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-326">Select hello virtual machine</span></span>
1. <span data-ttu-id="f62ac-327">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="f62ac-327">Click Access control (IAM)</span></span>
1. <span data-ttu-id="f62ac-328">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f62ac-328">Click Add</span></span>
1. <span data-ttu-id="f62ac-329">Vyberte roli hello vlastníka</span><span class="sxs-lookup"><span data-stu-id="f62ac-329">Select hello role Owner</span></span>
1. <span data-ttu-id="f62ac-330">Zadejte název hello hello aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="f62ac-330">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="f62ac-331">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f62ac-331">Click OK</span></span>

<span data-ttu-id="f62ac-332">Poté, co jste upravili hello oprávnění pro hello virtuální počítače, můžete nakonfigurovat zařízení STONITH hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f62ac-332">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="f62ac-333">Nyní se nám načíst hello souboru toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-333">now we load hello file toohello cluster</span></span>
<span data-ttu-id="f62ac-334">sudo crm nakonfigurovat aktualizace zatížení crm-fencing.txt</span><span class="sxs-lookup"><span data-stu-id="f62ac-334">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="f62ac-335">Vytvořit prostředky SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f62ac-335">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="f62ac-336">Nyní se nám načíst hello souboru toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-336">now we load hello file toohello cluster</span></span>
<span data-ttu-id="f62ac-337">sudo crm nakonfigurovat aktualizace zatížení crm-saphanatop.txt</span><span class="sxs-lookup"><span data-stu-id="f62ac-337">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="f62ac-338">Nyní se nám načíst hello souboru toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="f62ac-338">now we load hello file toohello cluster</span></span>
<span data-ttu-id="f62ac-339">sudo crm nakonfigurovat aktualizace zatížení crm-saphana.txt</span><span class="sxs-lookup"><span data-stu-id="f62ac-339">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="f62ac-340">Nastavení clusteru s podporou testu</span><span class="sxs-lookup"><span data-stu-id="f62ac-340">Test cluster setup</span></span>
<span data-ttu-id="f62ac-341">Hello následující kapitole popisují, jak můžete otestovat vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="f62ac-341">hello following chapter describe how you can test your setup.</span></span> <span data-ttu-id="f62ac-342">Každý test předpokládá, že jsou kořenové a hlavní SAP HANA hello běží na saphanavm1 hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f62ac-342">Every test assumes that you are root and hello SAP HANA master is running on hello virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="f62ac-343">Vymezení testu</span><span class="sxs-lookup"><span data-stu-id="f62ac-343">Fencing Test</span></span>

<span data-ttu-id="f62ac-344">Instalační program hello hello vymezení agenta můžete otestovat zakázáním hello síťové rozhraní na uzlu saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="f62ac-344">You can test hello setup of hello fencing agent by disabling hello network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="f62ac-345">Hello virtuální počítač by měl nyní restartovat nebo byla zastavena v závislosti na konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="f62ac-345">hello virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="f62ac-346">Pokud jste nastavili hello stonith akce toooff, hello virtuálního počítače se zastaví a hello prostředky jsou migrované toohello spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f62ac-346">If you set hello stonith-action toooff, hello virtual machine will be stopped and hello resources are migrated toohello running virtual machine.</span></span>

<span data-ttu-id="f62ac-347">Po spuštění virtuálního počítače hello, hello SAP HANA prostředků se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="f62ac-347">Once you start hello virtual machine again, hello SAP HANA resource will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="f62ac-348">V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f62ac-348">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="f62ac-349">Testování ruční převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="f62ac-349">Testing a manual failover</span></span>

<span data-ttu-id="f62ac-350">Zastavování služby kardiostimulátor hello na uzlu saphanavm1, můžete otestovat ruční převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f62ac-350">You can test a manual failover by stopping hello pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="f62ac-351">Po hello převzetí služeb při selhání můžete službu hello spustit znovu.</span><span class="sxs-lookup"><span data-stu-id="f62ac-351">After hello failover, you can start hello service again.</span></span> <span data-ttu-id="f62ac-352">Hello SAP HANA prostředek na saphanavm1 se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="f62ac-352">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="f62ac-353">V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f62ac-353">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="f62ac-354">Testování migrace</span><span class="sxs-lookup"><span data-stu-id="f62ac-354">Testing a migration</span></span>

<span data-ttu-id="f62ac-355">Můžete migrovat hello SAP HANA hlavního uzlu tak, že spustíte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-355">You can migrate hello SAP HANA master node by executing hello following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="f62ac-356">To je potřeba migrovat hello SAP HANA hlavní uzel a hello skupinu, která obsahuje toosaphanavm2 hello virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f62ac-356">This should migrate hello SAP HANA master node and hello group that contains hello virtual IP address toosaphanavm2.</span></span>
<span data-ttu-id="f62ac-357">Hello SAP HANA prostředek na saphanavm1 se nezdaří toostart jako sekundární, pokud jste nastavili AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="f62ac-357">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="f62ac-358">V takovém případě je třeba tooconfigure hello HANA instance jako sekundární spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f62ac-358">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="f62ac-359">migrace Hello vytvoří omezení umístění, vyžadující toobe znovu odstranit.</span><span class="sxs-lookup"><span data-stu-id="f62ac-359">hello migration creates location contraints that need toobe deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="f62ac-360">Musíte taky toocleanup hello stav prostředku sekundární uzel hello</span><span class="sxs-lookup"><span data-stu-id="f62ac-360">You also need toocleanup hello state of hello secondary node resource</span></span>

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="f62ac-361">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f62ac-361">Next steps</span></span>
* <span data-ttu-id="f62ac-362">[Azure virtuálních počítačů, plánování a implementace pro SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-362">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="f62ac-363">[Nasazení virtuálních počítačů Azure pro SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-363">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="f62ac-364">[Nasazení virtuálních počítačů databázového systému Azure pro SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f62ac-364">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="f62ac-365">jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="f62ac-365">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
