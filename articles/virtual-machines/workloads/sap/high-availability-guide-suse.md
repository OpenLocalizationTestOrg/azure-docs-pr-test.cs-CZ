---
title: "aaaAzure vysoké dostupnosti virtuálních počítačů pro SAP NetWeaver na SUSE Linux Enterprise Server pro aplikace SAP | Microsoft Docs"
description: "Vysoká dostupnost příručce pro SAP NetWeaver na SUSE Linux Enterprise Server pro aplikace SAP"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: e944103df92d5ffec9196189f138e25972bea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="1417c-103">Vysoká dostupnost pro SAP NetWeaver na virtuálních počítačích Azure na SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="1417c-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

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
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="1417c-113">Tento článek popisuje, jak toodeploy hello virtuálních počítačů, konfiguraci hello virtuálních počítačů, nainstalujte rozhraní framework hello clusteru a nainstalujte vysokou dostupností systému SAP NetWeaver 7.50.</span><span class="sxs-lookup"><span data-stu-id="1417c-113">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="1417c-114">V příkladu konfigurace hello příkazy instalace atd. Počet instancí ASC 00, čísla instance YBRAT 02 a NWS ID systému SAP se používá.</span><span class="sxs-lookup"><span data-stu-id="1417c-114">In hello example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="1417c-115">Hello názvy prostředků hello (třeba virtuální počítače, virtuální sítě) v příkladu hello předpokládá, že jste použili hello [konvergované šablony] [ template-converged] s SAP systému ID NWS toocreate hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-115">hello names of hello resources (for example virtual machines, virtual networks) in hello example assume that you have used hello [converged template][template-converged] with SAP system ID NWS toocreate hello resources.</span></span>

<span data-ttu-id="1417c-116">Přečtěte si následující poznámky k SAP a dokumenty Paper nejprve hello</span><span class="sxs-lookup"><span data-stu-id="1417c-116">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="1417c-117">Poznámka SAP [1928533], na kterém je:</span><span class="sxs-lookup"><span data-stu-id="1417c-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="1417c-118">Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro hello nasazení SAP softwaru</span><span class="sxs-lookup"><span data-stu-id="1417c-118">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="1417c-119">Kapacita důležité informace o velikosti virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="1417c-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="1417c-120">Podporované SAP software a operační systém (OS) a kombinace databáze</span><span class="sxs-lookup"><span data-stu-id="1417c-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="1417c-121">Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1417c-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="1417c-122">Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="1417c-123">Poznámka SAP [2205917] se doporučuje nastavení operačního systému SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="1417c-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="1417c-124">Poznámka SAP [1944799] má SAP HANA pokyny pro SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="1417c-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="1417c-125">Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="1417c-126">Poznámka SAP [2191498] hello vyžaduje verze SAP hostitele agenta pro Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-126">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="1417c-127">Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="1417c-128">Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="1417c-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="1417c-129">Poznámka SAP [1999351] obsahuje další informace o řešení potíží pro hello Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="1417c-129">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="1417c-130">[SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.</span><span class="sxs-lookup"><span data-stu-id="1417c-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="1417c-131">[Azure virtuálních počítačů, plánování a implementace pro SAP v systému Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="1417c-132">[Nasazení virtuálních počítačů Azure pro SAP v systému Linux (v tomto článku)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="1417c-133">[Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="1417c-134">[SAP HANA SR výkonu optimalizované scénář][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="1417c-135">Průvodce Hello obsahuje všechny požadované informace tooset replikaci systému SAP HANA místně.</span><span class="sxs-lookup"><span data-stu-id="1417c-135">hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="1417c-136">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="1417c-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="1417c-137">[Vysoce dostupné systému souborů NFS úložiště s DRBD a kardiostimulátor] [ suse-drbd-guide] hello Průvodce obsahuje všechny požadované informace tooset vytvořit vysoce dostupný server systému souborů NFS.</span><span class="sxs-lookup"><span data-stu-id="1417c-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] hello guide contains all required information tooset up a highly available NFS server.</span></span> <span data-ttu-id="1417c-138">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="1417c-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="1417c-139">Přehled</span><span class="sxs-lookup"><span data-stu-id="1417c-139">Overview</span></span>

<span data-ttu-id="1417c-140">tooachieve vysokou dostupnost, SAP NetWeaver vyžaduje, aby server systému souborů NFS.</span><span class="sxs-lookup"><span data-stu-id="1417c-140">tooachieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="1417c-141">Hello systému souborů NFS server je nakonfigurován v samostatném clusteru a mohou být využívána na více systémů SAP.</span><span class="sxs-lookup"><span data-stu-id="1417c-141">hello NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![Přehled SAP NetWeaver vysokou dostupnost](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="1417c-143">Hello serveru NFS, SAP NetWeaver ASC, SAP NetWeaver SCS, SAP NetWeaver YBRAT a hello databázi SAP HANA použijte virtuální název hostitele a virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1417c-143">hello NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and hello SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="1417c-144">V Azure je nástroj pro vyrovnávání zatížení vyžaduje toouse virtuální IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1417c-144">On Azure, a load balancer is required toouse a virtual IP address.</span></span> <span data-ttu-id="1417c-145">Hello následující seznam obsahuje hello konfigurace služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-145">hello following list shows hello configuration of hello load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="1417c-146">Server systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="1417c-146">NFS Server</span></span>
* <span data-ttu-id="1417c-147">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="1417c-147">Frontend configuration</span></span>
  * <span data-ttu-id="1417c-148">IP adresa 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="1417c-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="1417c-149">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="1417c-149">Backend configuration</span></span>
  * <span data-ttu-id="1417c-150">Připojení rozhraní sítě tooprimary všech virtuálních počítačů, které by měly být součástí clusteru systému souborů NFS hello</span><span class="sxs-lookup"><span data-stu-id="1417c-150">Connected tooprimary network interfaces of all virtual machines that should be part of hello NFS cluster</span></span>
* <span data-ttu-id="1417c-151">Port testu</span><span class="sxs-lookup"><span data-stu-id="1417c-151">Probe Port</span></span>
  * <span data-ttu-id="1417c-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="1417c-152">Port 61000</span></span>
* <span data-ttu-id="1417c-153">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1417c-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="1417c-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-154">2049 TCP</span></span> 
  * <span data-ttu-id="1417c-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="1417c-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="1417c-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="1417c-156">(A)SCS</span></span>
* <span data-ttu-id="1417c-157">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="1417c-157">Frontend configuration</span></span>
  * <span data-ttu-id="1417c-158">IP adresa 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="1417c-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="1417c-159">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="1417c-159">Backend configuration</span></span>
  * <span data-ttu-id="1417c-160">Síťová rozhraní připojená tooprimary všech virtuálních počítačů, které by měly být součástí clusteru SCS/YBRAT hello (A)</span><span class="sxs-lookup"><span data-stu-id="1417c-160">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="1417c-161">Port testu</span><span class="sxs-lookup"><span data-stu-id="1417c-161">Probe Port</span></span>
  * <span data-ttu-id="1417c-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="1417c-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="1417c-163">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1417c-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="1417c-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="1417c-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="1417c-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="1417c-167">81**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="1417c-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="1417c-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="1417c-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="1417c-171">YBRAT</span><span class="sxs-lookup"><span data-stu-id="1417c-171">ERS</span></span>
* <span data-ttu-id="1417c-172">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="1417c-172">Frontend configuration</span></span>
  * <span data-ttu-id="1417c-173">IP adresa 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="1417c-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="1417c-174">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="1417c-174">Backend configuration</span></span>
  * <span data-ttu-id="1417c-175">Síťová rozhraní připojená tooprimary všech virtuálních počítačů, které by měly být součástí clusteru SCS/YBRAT hello (A)</span><span class="sxs-lookup"><span data-stu-id="1417c-175">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="1417c-176">Port testu</span><span class="sxs-lookup"><span data-stu-id="1417c-176">Probe Port</span></span>
  * <span data-ttu-id="1417c-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="1417c-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="1417c-178">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1417c-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="1417c-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="1417c-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="1417c-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="1417c-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="1417c-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-183">SAP HANA</span></span>
* <span data-ttu-id="1417c-184">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="1417c-184">Frontend configuration</span></span>
  * <span data-ttu-id="1417c-185">IP adresa 10.0.0.12</span><span class="sxs-lookup"><span data-stu-id="1417c-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="1417c-186">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="1417c-186">Backend configuration</span></span>
  * <span data-ttu-id="1417c-187">Připojení rozhraní sítě tooprimary všech virtuálních počítačů, které by měly být součástí clusteru HANA hello</span><span class="sxs-lookup"><span data-stu-id="1417c-187">Connected tooprimary network interfaces of all virtual machines that should be part of hello HANA cluster</span></span>
* <span data-ttu-id="1417c-188">Port testu</span><span class="sxs-lookup"><span data-stu-id="1417c-188">Probe Port</span></span>
  * <span data-ttu-id="1417c-189">Port 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="1417c-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="1417c-190">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="1417c-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="1417c-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="1417c-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="1417c-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="1417c-193">Nastavení serveru s vysokou dostupností systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="1417c-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="1417c-194">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="1417c-194">Deploying Linux</span></span>

<span data-ttu-id="1417c-195">Hello Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro SAP 12 aplikace, které můžete použít toodeploy nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1417c-195">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span>
<span data-ttu-id="1417c-196">Můžete vytvořit jednu z šablon úvodní hello na githubu toodeploy všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-196">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="1417c-197">Šablona Hello nasadí hello virtuální počítače, nástroj pro vyrovnávání zatížení hello, dostupnosti atd. Postupujte podle těchto kroků toodeploy hello šablony:</span><span class="sxs-lookup"><span data-stu-id="1417c-197">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="1417c-198">Otevřete hello [šablony serveru SAP souboru] [ template-file-server] v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1417c-198">Open hello [SAP file server template][template-file-server] in hello Azure portal</span></span>   
1. <span data-ttu-id="1417c-199">Zadejte následující parametry hello</span><span class="sxs-lookup"><span data-stu-id="1417c-199">Enter hello following parameters</span></span>
   1. <span data-ttu-id="1417c-200">Předpona prostředků</span><span class="sxs-lookup"><span data-stu-id="1417c-200">Resource Prefix</span></span>  
      <span data-ttu-id="1417c-201">Zadejte předponu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="1417c-201">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="1417c-202">Hodnota Hello slouží jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="1417c-202">hello value is used as a prefix for hello resources that are deployed.</span></span>
   2. <span data-ttu-id="1417c-203">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="1417c-203">Os Type</span></span>  
      <span data-ttu-id="1417c-204">Vyberte jednu z Linuxových distribucích hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-204">Select one of hello Linux distributions.</span></span> <span data-ttu-id="1417c-205">V tomto příkladu vyberte SLES 12</span><span class="sxs-lookup"><span data-stu-id="1417c-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="1417c-206">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="1417c-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="1417c-207">Po vytvoření nového uživatele, který lze použít toolog na toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="1417c-207">A new user is created that can be used toolog on toohello machine.</span></span>
   4. <span data-ttu-id="1417c-208">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="1417c-208">Subnet Id</span></span>  
      <span data-ttu-id="1417c-209">ID Hello hello podsíť toowhich hello virtuálních počítačů musí být připojené k.</span><span class="sxs-lookup"><span data-stu-id="1417c-209">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="1417c-210">Chcete-li toocreate nové virtuální sítě nebo vyberte podsíť hello VPN nebo Express Route virtuální sítě tooconnect hello virtuálního počítače tooyour místní sítě, nechte pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="1417c-210">Leave empty if you want toocreate a new virtual network or select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="1417c-211">Hello ID obvykle vypadá /subscriptions/**&lt;id předplatného&gt;**/resourceGroups/**&lt;název skupiny prostředků&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;název virtuální sítě&gt;**/subnets/**&lt;název podsítě.&gt;**</span><span class="sxs-lookup"><span data-stu-id="1417c-211">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="1417c-212">Instalace</span><span class="sxs-lookup"><span data-stu-id="1417c-212">Installation</span></span>

<span data-ttu-id="1417c-213">Hello následující položky jsou s předponou buď **[A]** -použít tooall uzlů, **[1]** -pouze použít toonode 1 nebo **[2]** -pouze použít toonode 2.</span><span class="sxs-lookup"><span data-stu-id="1417c-213">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="1417c-214">**[A]**  Aktualizovat SLES</span><span class="sxs-lookup"><span data-stu-id="1417c-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="1417c-215">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="1417c-216">**[2]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="1417c-217">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="1417c-218">**[A]**  Nainstalovat HA rozšíření</span><span class="sxs-lookup"><span data-stu-id="1417c-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="1417c-219">**[A]**  Nastavit rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="1417c-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="1417c-220">Můžete buď použít DNS server nebo úpravám hello/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="1417c-220">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="1417c-221">Tento příklad ukazuje, jak toouse hello soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-221">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="1417c-222">Nahraďte hello IP adresu a název hostitele hello v hello následující příkazy</span><span class="sxs-lookup"><span data-stu-id="1417c-222">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="1417c-223">Následující hello vložení řádků příliš/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-223">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="1417c-224">Změnit hello IP adresu a název hostitele toomatch prostředí</span><span class="sxs-lookup"><span data-stu-id="1417c-224">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="1417c-225">**[1]**  Instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="1417c-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="1417c-226">**[2]**  Přidat uzel toocluster</span><span class="sxs-lookup"><span data-stu-id="1417c-226">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="1417c-227">**[A]**  Změnu hacluster heslo toohello stejné heslo</span><span class="sxs-lookup"><span data-stu-id="1417c-227">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="1417c-228">**[A]**  Konfigurace corosync toouse jiných přenosu a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="1417c-228">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="1417c-229">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="1417c-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="1417c-230">Přidejte následující soubor tučné obsahu toohello hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-230">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="1417c-231">Potom restartujte službu corosync hello</span><span class="sxs-lookup"><span data-stu-id="1417c-231">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="1417c-232">**[A]**  Nainstalovat komponenty drbd</span><span class="sxs-lookup"><span data-stu-id="1417c-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="1417c-233">**[A]**  Vytvořit oddíl pro hello drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-233">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="1417c-234">**[A]**  Vytvořit LVM konfigurace</span><span class="sxs-lookup"><span data-stu-id="1417c-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="1417c-235">**[A]**  Vytvořit hello systému souborů NFS drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-235">**[A]** Create hello NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="1417c-236">Vložit hello konfigurace pro nové zařízení drbd hello a ukončení</span><span class="sxs-lookup"><span data-stu-id="1417c-236">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="1417c-237">Vytvoření hello drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="1417c-237">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="1417c-238">**[1]**  Přeskočit počáteční synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="1417c-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="1417c-239">**[1]**  Sadu hello primárního uzlu</span><span class="sxs-lookup"><span data-stu-id="1417c-239">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="1417c-240">**[1]**  Počkejte, dokud se synchronizují hello nová drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-240">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="1417c-241">**[1]**  Vytvořit systémy souborů na hello drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-241">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="1417c-242">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="1417c-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="1417c-243">**[1]**  Změnit výchozí nastavení hello</span><span class="sxs-lookup"><span data-stu-id="1417c-243">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="1417c-244">**[1]**  Přidat hello systému souborů NFS drbd zařízení toohello konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="1417c-244">**[1]** Add hello NFS drbd device toohello cluster configuration</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="1417c-245">**[1]**  Vytvořit server systému souborů NFS hello</span><span class="sxs-lookup"><span data-stu-id="1417c-245">**[1]** Create hello NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="1417c-246">**[1]**  Vytvořit prostředky systému souborů NFS hello</span><span class="sxs-lookup"><span data-stu-id="1417c-246">**[1]** Create hello NFS File System resources</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="1417c-247">**[1]**  Vytvořit exportuje hello systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="1417c-247">**[1]** Create hello NFS exports</span></span>

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="1417c-248">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní hello</span><span class="sxs-lookup"><span data-stu-id="1417c-248">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="1417c-249">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-249">Create STONITH device</span></span>

<span data-ttu-id="1417c-250">zařízení STONITH Hello používá tooauthorize objekt služby pro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-250">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="1417c-251">Postupujte podle těchto kroků toocreate hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="1417c-251">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="1417c-252">Přejděte příliš<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="1417c-252">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="1417c-253">Okno Azure Active Directory otevřete hello</span><span class="sxs-lookup"><span data-stu-id="1417c-253">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="1417c-254">Přejděte tooProperties a zapište hello ID adresáře. Toto je hello **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="1417c-254">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="1417c-255">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="1417c-255">Click App registrations</span></span>
1. <span data-ttu-id="1417c-256">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="1417c-256">Click Add</span></span>
1. <span data-ttu-id="1417c-257">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="1417c-257">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="1417c-258">Hello přihlašovací adresa URL se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="1417c-258">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="1417c-259">Vyberte hello nové aplikace a klikněte na tlačítko klíče na kartě nastavení hello</span><span class="sxs-lookup"><span data-stu-id="1417c-259">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="1417c-260">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="1417c-260">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="1417c-261">Poznamenejte si hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-261">Write down hello Value.</span></span> <span data-ttu-id="1417c-262">Použije se jako hello **heslo** pro hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1417c-262">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="1417c-263">Zapište hello ID aplikace. Použije se jako hello uživatelské jméno (**přihlašovacího id** v následujících kroků hello) z hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1417c-263">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="1417c-264">Hello instanční objekt nemá oprávnění tooaccess vašich prostředků Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1417c-264">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="1417c-265">Je třeba toogive hello instanční objekt oprávnění toostart a zastavení (zrušit přidělení) všechny virtuální počítače clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-265">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="1417c-266">Přejděte toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="1417c-266">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="1417c-267">Otevřete hello okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="1417c-267">Open hello All resources blade</span></span>
1. <span data-ttu-id="1417c-268">Vyberte virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="1417c-268">Select hello virtual machine</span></span>
1. <span data-ttu-id="1417c-269">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="1417c-269">Click Access control (IAM)</span></span>
1. <span data-ttu-id="1417c-270">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="1417c-270">Click Add</span></span>
1. <span data-ttu-id="1417c-271">Vyberte roli hello vlastníka</span><span class="sxs-lookup"><span data-stu-id="1417c-271">Select hello role Owner</span></span>
1. <span data-ttu-id="1417c-272">Zadejte název hello hello aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="1417c-272">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="1417c-273">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="1417c-273">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="1417c-274">**[1]**  Vytvořit hello STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-274">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="1417c-275">Poté, co jste upravili hello oprávnění pro hello virtuální počítače, můžete nakonfigurovat zařízení STONITH hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-275">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="1417c-276">**[1]**  Povolit použití hello STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-276">**[1]** Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="1417c-277">Nastavení (A) SCS</span><span class="sxs-lookup"><span data-stu-id="1417c-277">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="1417c-278">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="1417c-278">Deploying Linux</span></span>

<span data-ttu-id="1417c-279">Hello Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro SAP 12 aplikace, které můžete použít toodeploy nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1417c-279">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span> <span data-ttu-id="1417c-280">bitová kopie Hello marketplace obsahuje hello prostředků agenta pro SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="1417c-280">hello marketplace image contains hello resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="1417c-281">Můžete vytvořit jednu z šablon úvodní hello na githubu toodeploy všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-281">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="1417c-282">Šablona Hello nasadí hello virtuální počítače, nástroj pro vyrovnávání zatížení hello, dostupnosti atd. Postupujte podle těchto kroků toodeploy hello šablony:</span><span class="sxs-lookup"><span data-stu-id="1417c-282">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="1417c-283">Otevřete hello [ASC nebo SCS více SID šablony] [ template-multisid-xscs] nebo hello [konvergované šablony] [ template-converged] na hello Azure portálu hello ASC nebo SCS šablony pouze Vytvoří hello pravidla Vyrovnávání zatížení pro hello SAP NetWeaver ASC nebo SCS a instance YBRAT (pouze Linux), zatímco sblížené šablony hello také vytvoří hello pravidla Vyrovnávání zatížení pro databázi (například Microsoft SQL Server nebo SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="1417c-283">Open hello [ASCS/SCS Multi SID template][template-multisid-xscs] or hello [converged template][template-converged] on hello Azure portal hello ASCS/SCS template only creates hello load-balancing rules for hello SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas hello converged template also creates hello load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="1417c-284">Pokud máte v plánu tooinstall SAP NetWeaver na základě systému a chcete tooinstall hello databáze na hello stejné počítače, použijte hello [konvergované šablony][template-converged].</span><span class="sxs-lookup"><span data-stu-id="1417c-284">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello database on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="1417c-285">Zadejte následující parametry hello</span><span class="sxs-lookup"><span data-stu-id="1417c-285">Enter hello following parameters</span></span>
   1. <span data-ttu-id="1417c-286">Předpona prostředků (pouze šablony SID více ASC nebo SCS)</span><span class="sxs-lookup"><span data-stu-id="1417c-286">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="1417c-287">Zadejte předponu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="1417c-287">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="1417c-288">Hodnota Hello slouží jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="1417c-288">hello value is used as a prefix for hello resources that are deployed.</span></span>
   3. <span data-ttu-id="1417c-289">Id systému SAP (pouze sblížené šablony)</span><span class="sxs-lookup"><span data-stu-id="1417c-289">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="1417c-290">Zadejte systému SAP hello Id hello chcete tooinstall systému SAP.</span><span class="sxs-lookup"><span data-stu-id="1417c-290">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="1417c-291">Hello Id slouží jako předpona pro hello prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="1417c-291">hello Id is used as a prefix for hello resources that are deployed.</span></span>
   4. <span data-ttu-id="1417c-292">Typ zásobníku</span><span class="sxs-lookup"><span data-stu-id="1417c-292">Stack Type</span></span>  
      <span data-ttu-id="1417c-293">Vyberte typ zásobníku SAP NetWeaver hello</span><span class="sxs-lookup"><span data-stu-id="1417c-293">Select hello SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="1417c-294">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="1417c-294">Os Type</span></span>  
      <span data-ttu-id="1417c-295">Vyberte jednu z Linuxových distribucích hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-295">Select one of hello Linux distributions.</span></span> <span data-ttu-id="1417c-296">V tomto příkladu vyberte SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="1417c-296">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="1417c-297">Typ databázového</span><span class="sxs-lookup"><span data-stu-id="1417c-297">Db Type</span></span>  
      <span data-ttu-id="1417c-298">Vyberte HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-298">Select HANA</span></span>
   7. <span data-ttu-id="1417c-299">Velikost systému SAP</span><span class="sxs-lookup"><span data-stu-id="1417c-299">Sap System Size</span></span>  
      <span data-ttu-id="1417c-300">poskytuje Hello množství protokoly SAP hello nový systém.</span><span class="sxs-lookup"><span data-stu-id="1417c-300">hello amount of SAPS hello new system provides.</span></span> <span data-ttu-id="1417c-301">Pokud si nejste jisti, kolik protokoly SAP hello systém vyžaduje, požádejte SAP technologie partnera nebo systémový integrátor</span><span class="sxs-lookup"><span data-stu-id="1417c-301">If you are not sure how many SAPS hello system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="1417c-302">Dostupnost systému</span><span class="sxs-lookup"><span data-stu-id="1417c-302">System Availability</span></span>  
      <span data-ttu-id="1417c-303">Vyberte HA</span><span class="sxs-lookup"><span data-stu-id="1417c-303">Select HA</span></span>
   9. <span data-ttu-id="1417c-304">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="1417c-304">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="1417c-305">Po vytvoření nového uživatele, který lze použít toolog na toohello počítači.</span><span class="sxs-lookup"><span data-stu-id="1417c-305">A new user is created that can be used toolog on toohello machine.</span></span>
   10. <span data-ttu-id="1417c-306">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="1417c-306">Subnet Id</span></span>  
   <span data-ttu-id="1417c-307">ID Hello hello podsíť toowhich hello virtuálních počítačů musí být připojené k.</span><span class="sxs-lookup"><span data-stu-id="1417c-307">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span>  <span data-ttu-id="1417c-308">Pokud chcete toocreate nové virtuální sítě nebo vyberte hello stejné podsíti, která používá nebo vytvořené jako součást nasazení serveru NFS hello, nechte pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="1417c-308">Leave empty if you want toocreate a new virtual network or select hello same subnet that you used or created as part of hello NFS server deployment.</span></span> <span data-ttu-id="1417c-309">Hello ID obvykle vypadá /subscriptions/**&lt;id předplatného&gt;**/resourceGroups/**&lt;název skupiny prostředků&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;název virtuální sítě&gt;**/subnets/**&lt;název podsítě.&gt;**</span><span class="sxs-lookup"><span data-stu-id="1417c-309">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="1417c-310">Instalace</span><span class="sxs-lookup"><span data-stu-id="1417c-310">Installation</span></span>

<span data-ttu-id="1417c-311">Hello následující položky jsou s předponou buď **[A]** -použít tooall uzlů, **[1]** -pouze použít toonode 1 nebo **[2]** -pouze použít toonode 2.</span><span class="sxs-lookup"><span data-stu-id="1417c-311">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="1417c-312">**[A]**  Aktualizovat SLES</span><span class="sxs-lookup"><span data-stu-id="1417c-312">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="1417c-313">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-313">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="1417c-314">**[2]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-314">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="1417c-315">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="1417c-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="1417c-316">**[A]**  Nainstalovat HA rozšíření</span><span class="sxs-lookup"><span data-stu-id="1417c-316">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="1417c-317">**[A]**  Aktualizace SAP prostředků agentů</span><span class="sxs-lookup"><span data-stu-id="1417c-317">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="1417c-318">Oprava pro balíček prostředků agenty hello je požadovaná toouse hello novou konfiguraci, který je popsaný v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="1417c-318">A patch for hello resource-agents package is required toouse hello new configuration, that is described in this article.</span></span> <span data-ttu-id="1417c-319">Můžete zkontrolovat, je-li oprava hello je již nainstalován ve hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="1417c-319">You can check, if hello patch is already installed with hello following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="1417c-320">výstup Hello by měl vypadat přibližně</span><span class="sxs-lookup"><span data-stu-id="1417c-320">hello output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="1417c-321">Pokud příkaz grep hello nenajde hello IS_ERS parametru, je třeba tooinstall hello opravy uvedené na [stránce pro stažení hello SUSE](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="1417c-321">If hello grep command does not find hello IS_ERS parameter, you need tooinstall hello patch listed on [hello SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="1417c-322">**[A]**  Nastavit rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="1417c-322">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="1417c-323">Můžete buď použít DNS server nebo úpravám hello/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="1417c-323">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="1417c-324">Tento příklad ukazuje, jak toouse hello soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-324">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="1417c-325">Nahraďte hello IP adresu a název hostitele hello v hello následující příkazy</span><span class="sxs-lookup"><span data-stu-id="1417c-325">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="1417c-326">Následující hello vložení řádků příliš/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-326">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="1417c-327">Změnit hello IP adresu a název hostitele toomatch prostředí</span><span class="sxs-lookup"><span data-stu-id="1417c-327">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="1417c-328">**[1]**  Instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="1417c-328">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="1417c-329">**[2]**  Přidat uzel toocluster</span><span class="sxs-lookup"><span data-stu-id="1417c-329">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="1417c-330">**[A]**  Změnu hacluster heslo toohello stejné heslo</span><span class="sxs-lookup"><span data-stu-id="1417c-330">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="1417c-331">**[A]**  Konfigurace corosync toouse jiných přenosu a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="1417c-331">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="1417c-332">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="1417c-332">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="1417c-333">Přidejte následující soubor tučné obsahu toohello hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-333">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="1417c-334">Potom restartujte službu corosync hello</span><span class="sxs-lookup"><span data-stu-id="1417c-334">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="1417c-335">**[A]**  Nainstalovat komponenty drbd</span><span class="sxs-lookup"><span data-stu-id="1417c-335">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="1417c-336">**[A]**  Vytvořit oddíl pro hello drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-336">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="1417c-337">**[A]**  Vytvořit LVM konfigurace</span><span class="sxs-lookup"><span data-stu-id="1417c-337">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="1417c-338">**[A]**  Vytvořit hello SCS drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-338">**[A]** Create hello SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="1417c-339">Vložit hello konfigurace pro nové zařízení drbd hello a ukončení</span><span class="sxs-lookup"><span data-stu-id="1417c-339">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="1417c-340">Vytvoření hello drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="1417c-340">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="1417c-341">**[A]**  Vytvořit hello YBRAT drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-341">**[A]** Create hello ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="1417c-342">Vložit hello konfigurace pro nové zařízení drbd hello a ukončení</span><span class="sxs-lookup"><span data-stu-id="1417c-342">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="1417c-343">Vytvoření hello drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="1417c-343">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="1417c-344">**[1]**  Přeskočit počáteční synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="1417c-344">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="1417c-345">**[1]**  Sadu hello primárního uzlu</span><span class="sxs-lookup"><span data-stu-id="1417c-345">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="1417c-346">**[1]**  Počkejte, dokud se synchronizují hello nová drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-346">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="1417c-347">**[1]**  Vytvořit systémy souborů na hello drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-347">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="1417c-348">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="1417c-348">Configure Cluster Framework</span></span>

<span data-ttu-id="1417c-349">**[1]**  Změnit výchozí nastavení hello</span><span class="sxs-lookup"><span data-stu-id="1417c-349">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="1417c-350">Příprava pro SAP NetWeaver instalace</span><span class="sxs-lookup"><span data-stu-id="1417c-350">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="1417c-351">**[A]**  Vytvořit hello sdíleného adresáře</span><span class="sxs-lookup"><span data-stu-id="1417c-351">**[A]** Create hello shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="1417c-352">**[A]**  Konfigurace autofs</span><span class="sxs-lookup"><span data-stu-id="1417c-352">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="1417c-353">Vytvořte soubor s</span><span class="sxs-lookup"><span data-stu-id="1417c-353">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="1417c-354">Restartujte nové sdílené složky autofs toomount hello</span><span class="sxs-lookup"><span data-stu-id="1417c-354">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="1417c-355">**[A]**  Konfigurace ODKLÁDACÍHO souboru</span><span class="sxs-lookup"><span data-stu-id="1417c-355">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="1417c-356">Restartujte hello agenta tooactivate hello změn</span><span class="sxs-lookup"><span data-stu-id="1417c-356">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="1417c-357">Instalace SAP NetWeaver ASC nebo YBRAT</span><span class="sxs-lookup"><span data-stu-id="1417c-357">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="1417c-358">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní hello</span><span class="sxs-lookup"><span data-stu-id="1417c-358">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="1417c-359">Ujistěte se, zda je stav clusteru hello ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-359">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="1417c-360">Není důležité na prostředky, ke kterým uzlu hello běží.</span><span class="sxs-lookup"><span data-stu-id="1417c-360">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. <span data-ttu-id="1417c-361">**[1]**  Nainstalovat SAP NetWeaver ASC</span><span class="sxs-lookup"><span data-stu-id="1417c-361">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="1417c-362">Nainstalujte SAP NetWeaver ASC jako kořenová na prvním uzlu hello pomocí virtuální název hostitele, který mapuje toohello IP adresu konfigurace front-endové služby Vyrovnávání zatížení hello pro hello ASC například <b>nws Asc</b>, <b>10.0.0.10</b>a číslo instance hello, který jste použili například u testu paměti hello nástroje pro vyrovnávání zatížení hello <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="1417c-362">Install SAP NetWeaver ASCS as root on hello first node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and hello instance number that you used for hello probe of hello load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="1417c-363">Můžete použít hello sapinst parametr SAPINST_REMOTE_ACCESS_USER tooallow toosapinst tooconnect bez kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1417c-363">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="1417c-364">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní hello</span><span class="sxs-lookup"><span data-stu-id="1417c-364">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want toocommit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="1417c-365">Ujistěte se, zda je stav clusteru hello ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-365">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="1417c-366">Není důležité na prostředky, ke kterým uzlu hello běží.</span><span class="sxs-lookup"><span data-stu-id="1417c-366">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="1417c-367">**[2]**  Nainstalovat SAP NetWeaver YBRAT</span><span class="sxs-lookup"><span data-stu-id="1417c-367">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="1417c-368">Nainstalujte SAP NetWeaver YBRAT jako kořenová na hello druhého uzlu pomocí virtuální název hostitele, který mapuje toohello IP adresu konfigurace front-endové služby Vyrovnávání zatížení hello pro hello YBRAT například <b>nws ybrat</b>, <b>10.0.0.11</b> a číslo instance hello, který jste použili například u testu paměti hello nástroje pro vyrovnávání zatížení hello <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="1417c-368">Install SAP NetWeaver ERS as root on hello second node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and hello instance number that you used for hello probe of hello load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="1417c-369">Můžete použít hello sapinst parametr SAPINST_REMOTE_ACCESS_USER tooallow toosapinst tooconnect bez kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1417c-369">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="1417c-370">Použijte prosím SWPM SP 20 PL 05 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1417c-370">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="1417c-371">Nižší verze nenastavujte hello oprávnění správně a hello instalace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1417c-371">Lower versions do not set hello permissions correctly and hello installation will fail.</span></span>
   > 

1. <span data-ttu-id="1417c-372">**[1]**  Přizpůsobit hello ASC nebo SCS a YBRAT instance profily</span><span class="sxs-lookup"><span data-stu-id="1417c-372">**[1]** Adapt hello ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="1417c-373">ASC nebo SCS profilu</span><span class="sxs-lookup"><span data-stu-id="1417c-373">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change hello restart command tooa start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add hello keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="1417c-374">YBRAT profilu</span><span class="sxs-lookup"><span data-stu-id="1417c-374">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="1417c-375">**[A]**  Konfigurace zachování</span><span class="sxs-lookup"><span data-stu-id="1417c-375">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="1417c-376">Hello komunikace mezi serverem aplikace SAP NetWeaver hello a hello ASC nebo SCS směrován přes softwarovému Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="1417c-376">hello communication between hello SAP NetWeaver application server and hello ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="1417c-377">Nástroj pro vyrovnávání zatížení Hello odpojí neaktivní připojení po vypršení časového limitu se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="1417c-377">hello load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="1417c-378">tooprevent to potřebujete tooset parametr v hello SAP NetWeaver ASC nebo SCS profil a změnit nastavení systému Linux hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-378">tooprevent this you need tooset a parameter in hello SAP NetWeaver ASCS/SCS profile and change hello Linux system settings.</span></span> <span data-ttu-id="1417c-379">Přečtěte si prosím [1410736 Poznámka SAP] [ 1410736] Další informace.</span><span class="sxs-lookup"><span data-stu-id="1417c-379">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="1417c-380">Hello ASC nebo SCS profil parametr enque/encni/set_so_keepalive již byl přidán v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-380">hello ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in hello last step.</span></span>

   <pre><code> 
   # Change hello Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="1417c-381">**[A]**  Konfigurovat hello SAP uživatele po instalaci hello</span><span class="sxs-lookup"><span data-stu-id="1417c-381">**[A]** Configure hello SAP users after hello installation</span></span>
 
   <pre><code>
   # Add sidadm toohello haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="1417c-382">**[1]**  Přidat hello ASC a SAP YBRAT služby toohello sapservice souboru</span><span class="sxs-lookup"><span data-stu-id="1417c-382">**[1]** Add hello ASCS and ERS SAP services toohello sapservice file</span></span>

   <span data-ttu-id="1417c-383">Přidejte hello ASC služby položka toohello druhý uzel a kopírování hello YBRAT služby položka toohello prvního uzlu.</span><span class="sxs-lookup"><span data-stu-id="1417c-383">Add hello ASCS service entry toohello second node and copy hello ERS service entry toohello first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="1417c-384">**[1]**  Vytvořit prostředky clusteru SAP hello</span><span class="sxs-lookup"><span data-stu-id="1417c-384">**[1]** Create hello SAP cluster resources</span></span>

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   <span data-ttu-id="1417c-385">Ujistěte se, zda je stav clusteru hello ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-385">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="1417c-386">Není důležité na prostředky, ke kterým uzlu hello běží.</span><span class="sxs-lookup"><span data-stu-id="1417c-386">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="1417c-387">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-387">Create STONITH device</span></span>

<span data-ttu-id="1417c-388">zařízení STONITH Hello používá tooauthorize objekt služby pro Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1417c-388">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="1417c-389">Postupujte podle těchto kroků toocreate hlavní název služby.</span><span class="sxs-lookup"><span data-stu-id="1417c-389">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="1417c-390">Přejděte příliš<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="1417c-390">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="1417c-391">Okno Azure Active Directory otevřete hello</span><span class="sxs-lookup"><span data-stu-id="1417c-391">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="1417c-392">Přejděte tooProperties a zapište hello ID adresáře. Toto je hello **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="1417c-392">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="1417c-393">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="1417c-393">Click App registrations</span></span>
1. <span data-ttu-id="1417c-394">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="1417c-394">Click Add</span></span>
1. <span data-ttu-id="1417c-395">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="1417c-395">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="1417c-396">Hello přihlašovací adresa URL se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="1417c-396">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="1417c-397">Vyberte hello nové aplikace a klikněte na tlačítko klíče na kartě nastavení hello</span><span class="sxs-lookup"><span data-stu-id="1417c-397">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="1417c-398">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="1417c-398">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="1417c-399">Poznamenejte si hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-399">Write down hello Value.</span></span> <span data-ttu-id="1417c-400">Použije se jako hello **heslo** pro hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1417c-400">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="1417c-401">Zapište hello ID aplikace. Použije se jako hello uživatelské jméno (**přihlašovacího id** v následujících kroků hello) z hello instančního objektu</span><span class="sxs-lookup"><span data-stu-id="1417c-401">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="1417c-402">Hello instanční objekt nemá oprávnění tooaccess vašich prostředků Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1417c-402">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="1417c-403">Je třeba toogive hello instanční objekt oprávnění toostart a zastavení (zrušit přidělení) všechny virtuální počítače clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-403">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="1417c-404">Přejděte toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="1417c-404">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="1417c-405">Otevřete hello okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="1417c-405">Open hello All resources blade</span></span>
1. <span data-ttu-id="1417c-406">Vyberte virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="1417c-406">Select hello virtual machine</span></span>
1. <span data-ttu-id="1417c-407">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="1417c-407">Click Access control (IAM)</span></span>
1. <span data-ttu-id="1417c-408">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="1417c-408">Click Add</span></span>
1. <span data-ttu-id="1417c-409">Vyberte roli hello vlastníka</span><span class="sxs-lookup"><span data-stu-id="1417c-409">Select hello role Owner</span></span>
1. <span data-ttu-id="1417c-410">Zadejte název hello hello aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="1417c-410">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="1417c-411">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="1417c-411">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="1417c-412">**[1]**  Vytvořit hello STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-412">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="1417c-413">Poté, co jste upravili hello oprávnění pro hello virtuální počítače, můžete nakonfigurovat zařízení STONITH hello v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-413">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="1417c-414">**[1]**  Povolit použití hello STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-414">**[1]** Enable hello use of a STONITH device</span></span>

<span data-ttu-id="1417c-415">Povolit použití hello STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="1417c-415">Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="1417c-416">Instalace databáze</span><span class="sxs-lookup"><span data-stu-id="1417c-416">Install database</span></span>

<span data-ttu-id="1417c-417">V tomto příkladu je replikaci systému SAP HANA nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="1417c-417">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="1417c-418">SAP HANA se spustí v hello stejné jako hello SAP NetWeaver ASC nebo SCS a YBRAT clusteru.</span><span class="sxs-lookup"><span data-stu-id="1417c-418">SAP HANA will run in hello same cluster as hello SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="1417c-419">Můžete taky nainstalovat SAP HANA na vyhrazeném clusteru.</span><span class="sxs-lookup"><span data-stu-id="1417c-419">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="1417c-420">V tématu [vysokou dostupnost z SAP HANA ve virtuálních počítačích Azure (VM)] [ sap-hana-ha] Další informace.</span><span class="sxs-lookup"><span data-stu-id="1417c-420">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="1417c-421">Příprava pro instalaci SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-421">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="1417c-422">Obecně doporučujeme používat LVM pro svazky, které ukládají data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="1417c-422">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="1417c-423">Pro účely testování můžete také soubor protokolu a data hello toostore přímo na prostý disku.</span><span class="sxs-lookup"><span data-stu-id="1417c-423">For testing purposes, you can also choose toostore hello data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="1417c-424">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="1417c-424">**[A]** LVM</span></span>  
   <span data-ttu-id="1417c-425">Následující příklad Hello předpokládá, že máte hello virtuální počítače čtyři datových disků připojených, které by měly být použité toocreate dva svazky.</span><span class="sxs-lookup"><span data-stu-id="1417c-425">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
   
   <span data-ttu-id="1417c-426">Vytvořte pro všechny disky, které chcete toouse fyzických svazků.</span><span class="sxs-lookup"><span data-stu-id="1417c-426">Create physical volumes for all disks that you want toouse.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="1417c-427">Vytvořit skupinu svazku pro hello datové soubory, jedna skupina svazku pro soubory protokolu hello a jeden pro sdílený adresář hello SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-427">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="1417c-428">Vytvoření logické svazky hello</span><span class="sxs-lookup"><span data-stu-id="1417c-428">Create hello logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="1417c-429">Vytvořte hello připojení adresáře a zkopírujte hello UUID všechny logické svazky</span><span class="sxs-lookup"><span data-stu-id="1417c-429">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="1417c-430">Vytvořit záznamy autofs pro hello tři logické svazky</span><span class="sxs-lookup"><span data-stu-id="1417c-430">Create autofs entries for hello three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="1417c-431">Vložit tento řádek toosudo vi /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="1417c-431">Insert this line toosudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="1417c-432">Připojit nové svazky hello</span><span class="sxs-lookup"><span data-stu-id="1417c-432">Mount hello new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="1417c-433">**[A]**  Prostý disky</span><span class="sxs-lookup"><span data-stu-id="1417c-433">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="1417c-434">Pro malé nebo ukázku systémů, můžete umístit HANA soubory protokolu a data na jeden disk.</span><span class="sxs-lookup"><span data-stu-id="1417c-434">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="1417c-435">Hello následující příkazy na /dev/sdc vytvořit oddíl a naformátovat ho s xfs.</span><span class="sxs-lookup"><span data-stu-id="1417c-435">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down hello id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="1417c-436">Vložit tento řádek too/etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="1417c-436">Insert this line too/etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="1417c-437">Vytvořte hello cílový adresář a připojte hello disk.</span><span class="sxs-lookup"><span data-stu-id="1417c-437">Create hello target directory and mount hello disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="1417c-438">Instalace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-438">Installing SAP HANA</span></span>

<span data-ttu-id="1417c-439">Hello následující kroky jsou založeny na kapitoly 4 hello [SAP HANA SR výkonu optimalizované scénář průvodce] [ suse-hana-ha-guide] tooinstall replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="1417c-439">hello following steps are based on chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span> <span data-ttu-id="1417c-440">Přečtěte si ji před pokračováním instalace hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-440">Please read it before you continue hello installation.</span></span>

1. <span data-ttu-id="1417c-441">**[A]**  Spustit hdblcm z hello HANA DVD</span><span class="sxs-lookup"><span data-stu-id="1417c-441">**[A]** Run hdblcm from hello HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="1417c-442">**[A]**  Upgradu agenta hostitele SAP</span><span class="sxs-lookup"><span data-stu-id="1417c-442">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="1417c-443">Stáhnout nejnovější archivu Agent hostitele SAP hello z hello [SAP Softwarecenter] [ sap-swcenter] a spusťte hello následující příkaz tooupgrade hello agenta.</span><span class="sxs-lookup"><span data-stu-id="1417c-443">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="1417c-444">Nahraďte hello cesta toohello toopoint toohello soubor archivu, které jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="1417c-444">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path tooSAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="1417c-445">**[1]**  Vytvořit HANA replikace (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="1417c-445">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="1417c-446">Spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="1417c-446">Run hello following command.</span></span> <span data-ttu-id="1417c-447">Ujistěte se, že tooreplace tučné řetězce (HANA systému ID HDB a číslo instance 03) s hodnotami hello instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="1417c-447">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="1417c-448">**[A]**  Vytvořit položku úložiště klíčů (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="1417c-448">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="1417c-449">**[1]**  Zálohy databáze (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="1417c-449">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="1417c-450">**[1]**  Přepnout uživatele sapsid HANA toohello a vytvořit hello primární lokality.</span><span class="sxs-lookup"><span data-stu-id="1417c-450">**[1]** Switch toohello HANA sapsid user and create hello primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="1417c-451">**[2]**  Přepnout uživatele sapsid HANA toohello a vytvořit hello sekundární lokality.</span><span class="sxs-lookup"><span data-stu-id="1417c-451">**[2]** Switch toohello HANA sapsid user and create hello secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="1417c-452">**[1]**  Prostředky clusteru vytvořit SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-452">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="1417c-453">Nejprve vytvořte hello topologie.</span><span class="sxs-lookup"><span data-stu-id="1417c-453">First, create hello topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number and HANA system id
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   <span data-ttu-id="1417c-454">Dále vytvořte hello HANA prostředky</span><span class="sxs-lookup"><span data-stu-id="1417c-454">Next, create hello HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="1417c-455">Ujistěte se, zda je stav clusteru hello ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1417c-455">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="1417c-456">Není důležité na prostředky, ke kterým uzlu hello běží.</span><span class="sxs-lookup"><span data-stu-id="1417c-456">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="1417c-457">**[1]**  Instance databáze SAP NetWeaver hello instalace</span><span class="sxs-lookup"><span data-stu-id="1417c-457">**[1]** Install hello SAP NetWeaver database instance</span></span>

   <span data-ttu-id="1417c-458">Instalace hello SAP NetWeaver instanci databáze jako kořenová pomocí virtuální název hostitele, který mapuje toohello IP adresu konfigurace front-endové služby Vyrovnávání zatížení hello hello databáze například <b>nws-db</b> a <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="1417c-458">Install hello SAP NetWeaver database instance as root using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="1417c-459">Můžete použít hello sapinst parametr SAPINST_REMOTE_ACCESS_USER tooallow toosapinst tooconnect bez kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1417c-459">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="1417c-460">Instalace serveru aplikace SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="1417c-460">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="1417c-461">Postupujte podle těchto kroků tooinstall SAP aplikační server.</span><span class="sxs-lookup"><span data-stu-id="1417c-461">Follow these steps tooinstall an SAP application server.</span></span> <span data-ttu-id="1417c-462">mocí následujících kroků Hello předpokládá instalace hello aplikační server na serveru, která se liší od hello ASC nebo SCS a HANA servery.</span><span class="sxs-lookup"><span data-stu-id="1417c-462">hello steps bellow assume that you install hello application server on a server different from hello ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="1417c-463">V opačném případě některé z kroků hello dole (např. konfigurace rozlišení názvu hostitele) nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="1417c-463">Otherwise some of hello steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="1417c-464">Instalační program rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="1417c-464">Setup host name resolution</span></span>    
   <span data-ttu-id="1417c-465">Můžete buď použít DNS server nebo úpravám hello/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="1417c-465">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="1417c-466">Tento příklad ukazuje, jak toouse hello soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-466">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="1417c-467">Nahraďte hello IP adresu a název hostitele hello v hello následující příkazy</span><span class="sxs-lookup"><span data-stu-id="1417c-467">Replace hello IP address and hello hostname in hello following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="1417c-468">Následující hello vložení řádků příliš/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="1417c-468">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="1417c-469">Změnit hello IP adresu a název hostitele toomatch prostředí</span><span class="sxs-lookup"><span data-stu-id="1417c-469">Change hello IP address and hostname toomatch your environment</span></span>    
    
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of hello application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="1417c-470">Vytvořte adresář sapmnt hello</span><span class="sxs-lookup"><span data-stu-id="1417c-470">Create hello sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="1417c-471">Konfigurace autofs</span><span class="sxs-lookup"><span data-stu-id="1417c-471">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="1417c-472">Vytvořte nový soubor s</span><span class="sxs-lookup"><span data-stu-id="1417c-472">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="1417c-473">Restartujte nové sdílené složky autofs toomount hello</span><span class="sxs-lookup"><span data-stu-id="1417c-473">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="1417c-474">Konfigurace ODKLÁDACÍ soubor</span><span class="sxs-lookup"><span data-stu-id="1417c-474">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="1417c-475">Restartujte hello agenta tooactivate hello změn</span><span class="sxs-lookup"><span data-stu-id="1417c-475">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="1417c-476">Instalace serveru SAP NetWeaver aplikace</span><span class="sxs-lookup"><span data-stu-id="1417c-476">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="1417c-477">Instalace serveru primární nebo další aplikace SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="1417c-477">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="1417c-478">Můžete použít hello sapinst parametr SAPINST_REMOTE_ACCESS_USER tooallow toosapinst tooconnect bez kořenového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1417c-478">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="1417c-479">Zabezpečené úložiště pověření aktualizace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="1417c-479">Update SAP HANA secure store</span></span>

   <span data-ttu-id="1417c-480">Aktualizace hello SAP HANA zabezpečeného úložiště toopoint toohello virtuální název instalačního programu hello replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="1417c-480">Update hello SAP HANA secure store toopoint toohello virtual name of hello SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="1417c-481">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1417c-481">Next steps</span></span>
* <span data-ttu-id="1417c-482">[Azure virtuálních počítačů, plánování a implementace pro SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-482">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="1417c-483">[Nasazení virtuálních počítačů Azure pro SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-483">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="1417c-484">[Nasazení virtuálních počítačů databázového systému Azure pro SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="1417c-484">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="1417c-485">jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA v Azure (velké instance), najdete v části toolearn [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="1417c-485">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="1417c-486">jak tooestablish vysokou dostupnost a plán pro zotavení po havárii SAP HANA na virtuálních počítačích Azure, najdete v části toolearn [vysokou dostupnost z SAP HANA ve virtuálních počítačích Azure (VM)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="1417c-486">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>
