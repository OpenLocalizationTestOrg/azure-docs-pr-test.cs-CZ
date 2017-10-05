---
title: "Azure virtuální počítače vysoká dostupnost pro SAP NetWeaver na SUSE Linux Enterprise Server pro aplikace SAP | Microsoft Docs"
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
ms.openlocfilehash: 16e09797926f29bc18cb05671c986c74f9c2d4f8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="f1ae7-103">Vysoká dostupnost pro SAP NetWeaver na virtuálních počítačích Azure na SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="f1ae7-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="f1ae7-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="f1ae7-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="f1ae7-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="f1ae7-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="f1ae7-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="f1ae7-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="f1ae7-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="f1ae7-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="f1ae7-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="f1ae7-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="f1ae7-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="f1ae7-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="f1ae7-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="f1ae7-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="f1ae7-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="f1ae7-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="f1ae7-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="f1ae7-113">Tento článek popisuje postup nasazení virtuálních počítačů, konfiguraci virtuálních počítačů, nainstalujte rozhraní framework clusteru a nainstalujte systému SAP NetWeaver 7.50 vysoce dostupný.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-113">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="f1ae7-114">V konfiguraci příklad příkazy instalace atd. Počet instancí ASC 00, čísla instance YBRAT 02 a NWS ID systému SAP se používá.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-114">In the example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="f1ae7-115">Názvy prostředků (například virtuální počítače, virtuální sítě) v příkladu předpokládá, že jste použili [konvergované šablony] [ template-converged] systémem SAP NWS ID pro vytvoření prostředků.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-115">The names of the resources (for example virtual machines, virtual networks) in the example assume that you have used the [converged template][template-converged] with SAP system ID NWS to create the resources.</span></span>

<span data-ttu-id="f1ae7-116">Přečtěte si tyto poznámky k SAP a dokumenty Paper nejprve</span><span class="sxs-lookup"><span data-stu-id="f1ae7-116">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="f1ae7-117">Poznámka SAP [1928533], na kterém je:</span><span class="sxs-lookup"><span data-stu-id="f1ae7-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="f1ae7-118">Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro nasazení softwaru SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-118">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="f1ae7-119">Kapacita důležité informace o velikosti virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="f1ae7-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="f1ae7-120">Podporované SAP software a operační systém (OS) a kombinace databáze</span><span class="sxs-lookup"><span data-stu-id="f1ae7-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="f1ae7-121">Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f1ae7-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="f1ae7-122">Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="f1ae7-123">Poznámka SAP [2205917] se doporučuje nastavení operačního systému SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f1ae7-124">Poznámka SAP [1944799] má SAP HANA pokyny pro SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f1ae7-125">Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="f1ae7-126">Poznámka SAP [2191498] má požadovaná verze SAP hostitele agenta pro Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-126">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="f1ae7-127">Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="f1ae7-128">Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="f1ae7-129">Poznámka SAP [1999351] Další informace o řešení problémů s Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-129">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="f1ae7-130">[SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="f1ae7-131">[Azure virtuálních počítačů, plánování a implementace pro SAP v systému Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="f1ae7-132">[Nasazení virtuálních počítačů Azure pro SAP v systému Linux (v tomto článku)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="f1ae7-133">[Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="f1ae7-134">[SAP HANA SR výkonu optimalizované scénář][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="f1ae7-135">V Průvodci obsahuje všechny požadované informace pro nastavení replikace systému SAP HANA na místě.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-135">The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="f1ae7-136">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="f1ae7-137">[Vysoce dostupné systému souborů NFS úložiště s DRBD a kardiostimulátor] [ suse-drbd-guide] obsahuje všechny požadované informace k nastavení systému souborů NFS server s vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] The guide contains all required information to set up a highly available NFS server.</span></span> <span data-ttu-id="f1ae7-138">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="f1ae7-139">Přehled</span><span class="sxs-lookup"><span data-stu-id="f1ae7-139">Overview</span></span>

<span data-ttu-id="f1ae7-140">K dosažení vysoké dostupnosti, vyžaduje SAP NetWeaver serverem NFS.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-140">To achieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="f1ae7-141">Server systému souborů NFS je nakonfigurován v samostatném clusteru a mohou být využívána na více systémů SAP.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-141">The NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![Přehled SAP NetWeaver vysokou dostupnost](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="f1ae7-143">Na serveru NFS, SAP NetWeaver ASC, SAP NetWeaver SCS, SAP NetWeaver YBRAT a databázi SAP HANA používat virtuální název hostitele a virtuální IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-143">The NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and the SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="f1ae7-144">K použití virtuální IP adresy se v Azure, vyžaduje nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-144">On Azure, a load balancer is required to use a virtual IP address.</span></span> <span data-ttu-id="f1ae7-145">Následující seznam obsahuje konfiguraci služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-145">The following list shows the configuration of the load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="f1ae7-146">Server systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-146">NFS Server</span></span>
* <span data-ttu-id="f1ae7-147">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="f1ae7-147">Frontend configuration</span></span>
  * <span data-ttu-id="f1ae7-148">IP adresa 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="f1ae7-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="f1ae7-149">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="f1ae7-149">Backend configuration</span></span>
  * <span data-ttu-id="f1ae7-150">Připojené k primární síťová rozhraní všech virtuálních počítačů, které by měly být součástí clusteru systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-150">Connected to primary network interfaces of all virtual machines that should be part of the NFS cluster</span></span>
* <span data-ttu-id="f1ae7-151">Port testu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-151">Probe Port</span></span>
  * <span data-ttu-id="f1ae7-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="f1ae7-152">Port 61000</span></span>
* <span data-ttu-id="f1ae7-153">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="f1ae7-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-154">2049 TCP</span></span> 
  * <span data-ttu-id="f1ae7-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="f1ae7-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-156">(A)SCS</span></span>
* <span data-ttu-id="f1ae7-157">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="f1ae7-157">Frontend configuration</span></span>
  * <span data-ttu-id="f1ae7-158">IP adresa 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="f1ae7-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="f1ae7-159">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="f1ae7-159">Backend configuration</span></span>
  * <span data-ttu-id="f1ae7-160">Připojené k primární síťová rozhraní všech virtuálních počítačů, které by měly být součástí (A) SCS/YBRAT clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-160">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="f1ae7-161">Port testu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-161">Probe Port</span></span>
  * <span data-ttu-id="f1ae7-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1ae7-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f1ae7-163">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="f1ae7-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f1ae7-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f1ae7-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f1ae7-167">81**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f1ae7-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="f1ae7-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="f1ae7-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="f1ae7-171">YBRAT</span><span class="sxs-lookup"><span data-stu-id="f1ae7-171">ERS</span></span>
* <span data-ttu-id="f1ae7-172">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="f1ae7-172">Frontend configuration</span></span>
  * <span data-ttu-id="f1ae7-173">IP adresa 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="f1ae7-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="f1ae7-174">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="f1ae7-174">Backend configuration</span></span>
  * <span data-ttu-id="f1ae7-175">Připojené k primární síťová rozhraní všech virtuálních počítačů, které by měly být součástí (A) SCS/YBRAT clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-175">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="f1ae7-176">Port testu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-176">Probe Port</span></span>
  * <span data-ttu-id="f1ae7-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1ae7-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f1ae7-178">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="f1ae7-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f1ae7-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="f1ae7-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="f1ae7-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="f1ae7-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-183">SAP HANA</span></span>
* <span data-ttu-id="f1ae7-184">Front-endovou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="f1ae7-184">Frontend configuration</span></span>
  * <span data-ttu-id="f1ae7-185">IP adresa 10.0.0.12</span><span class="sxs-lookup"><span data-stu-id="f1ae7-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="f1ae7-186">Konfigurace back-end</span><span class="sxs-lookup"><span data-stu-id="f1ae7-186">Backend configuration</span></span>
  * <span data-ttu-id="f1ae7-187">Připojené k primární síťová rozhraní všech virtuálních počítačů, které by měly být součástí clusteru HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-187">Connected to primary network interfaces of all virtual machines that should be part of the HANA cluster</span></span>
* <span data-ttu-id="f1ae7-188">Port testu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-188">Probe Port</span></span>
  * <span data-ttu-id="f1ae7-189">Port 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1ae7-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f1ae7-190">Pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="f1ae7-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="f1ae7-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="f1ae7-193">Nastavení serveru s vysokou dostupností systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="f1ae7-194">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="f1ae7-194">Deploying Linux</span></span>

<span data-ttu-id="f1ae7-195">Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro 12 aplikace SAP, který můžete použít k nasazení nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-195">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span>
<span data-ttu-id="f1ae7-196">Jeden z šablony rychlý start způsobem můžete na githubu nasadit všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-196">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="f1ae7-197">Šablona nasadí virtuální počítače, nástroj pro vyrovnávání zatížení, dostupnosti apod. Postupujte podle těchto kroků nasadíte šablony:</span><span class="sxs-lookup"><span data-stu-id="f1ae7-197">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="f1ae7-198">Otevřete [šablony serveru SAP souboru] [ template-file-server] na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f1ae7-198">Open the [SAP file server template][template-file-server] in the Azure portal</span></span>   
1. <span data-ttu-id="f1ae7-199">Zadejte následující parametry</span><span class="sxs-lookup"><span data-stu-id="f1ae7-199">Enter the following parameters</span></span>
   1. <span data-ttu-id="f1ae7-200">Předpona prostředků</span><span class="sxs-lookup"><span data-stu-id="f1ae7-200">Resource Prefix</span></span>  
      <span data-ttu-id="f1ae7-201">Zadejte předponu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-201">Enter the prefix you want to use.</span></span> <span data-ttu-id="f1ae7-202">Hodnota se používá jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-202">The value is used as a prefix for the resources that are deployed.</span></span>
   2. <span data-ttu-id="f1ae7-203">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="f1ae7-203">Os Type</span></span>  
      <span data-ttu-id="f1ae7-204">Vyberte jednu z distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-204">Select one of the Linux distributions.</span></span> <span data-ttu-id="f1ae7-205">V tomto příkladu vyberte SLES 12</span><span class="sxs-lookup"><span data-stu-id="f1ae7-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="f1ae7-206">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="f1ae7-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="f1ae7-207">Po vytvoření nového uživatele, který lze použít pro přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-207">A new user is created that can be used to log on to the machine.</span></span>
   4. <span data-ttu-id="f1ae7-208">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="f1ae7-208">Subnet Id</span></span>  
      <span data-ttu-id="f1ae7-209">ID podsítě, ke které by měl být připojený virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-209">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="f1ae7-210">Ponechte prázdné, pokud chcete vytvořit novou virtuální síť, nebo vyberte podsíť virtuální sítě VPN nebo Expressroute připojit virtuální počítač k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-210">Leave empty if you want to create a new virtual network or select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="f1ae7-211">ID obvykle vypadá /subscriptions/**&lt;id předplatného&gt;**/resourceGroups/**&lt;název skupiny prostředků&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;název virtuální sítě&gt;**/subnets/**&lt;název podsítě.&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1ae7-211">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="f1ae7-212">Instalace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-212">Installation</span></span>

<span data-ttu-id="f1ae7-213">Následující položky jsou předponou buď **[A]** – platí pro všechny uzly, **[1]** – platí jenom pro uzel 1 nebo **[2]** – platí jenom pro uzel 2.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-213">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="f1ae7-214">**[A]**  Aktualizovat SLES</span><span class="sxs-lookup"><span data-stu-id="f1ae7-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="f1ae7-215">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="f1ae7-216">**[2]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="f1ae7-217">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="f1ae7-218">**[A]**  Nainstalovat HA rozšíření</span><span class="sxs-lookup"><span data-stu-id="f1ae7-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="f1ae7-219">**[A]**  Nastavit rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="f1ae7-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="f1ae7-220">Můžete buď použít DNS server, nebo upravit/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-220">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f1ae7-221">Tento příklad ukazuje, jak chcete použít soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-221">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f1ae7-222">Nahraďte adresu IP a název hostitele v následujících příkazech</span><span class="sxs-lookup"><span data-stu-id="f1ae7-222">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="f1ae7-223">Vložte následující řádky, které se/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-223">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f1ae7-224">Změnit IP adresu a název hostitele tak, aby odpovídaly prostředí</span><span class="sxs-lookup"><span data-stu-id="f1ae7-224">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-225">**[1]**  Instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="f1ae7-226">**[2]**  Přidat uzel do clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-226">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="f1ae7-227">**[A]**  Změnit heslo hacluster na stejné heslo</span><span class="sxs-lookup"><span data-stu-id="f1ae7-227">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="f1ae7-228">**[A]**  Konfigurace corosync používají jiné přenos a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-228">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="f1ae7-229">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="f1ae7-230">Do souboru přidejte následující obsah tučně.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-230">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="f1ae7-231">Potom restartujte službu corosync</span><span class="sxs-lookup"><span data-stu-id="f1ae7-231">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="f1ae7-232">**[A]**  Nainstalovat komponenty drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="f1ae7-233">**[A]**  Vytvořit oddíl drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-233">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="f1ae7-234">**[A]**  Vytvořit LVM konfigurace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="f1ae7-235">**[A]**  Vytvořte zařízení drbd systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-235">**[A]** Create the NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="f1ae7-236">Vložit konfigurace pro nové zařízení drbd a ukončení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-236">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f1ae7-237">Vytvořte drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="f1ae7-237">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f1ae7-238">**[1]**  Přeskočit počáteční synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f1ae7-239">**[1]**  Nastavte primární uzel</span><span class="sxs-lookup"><span data-stu-id="f1ae7-239">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f1ae7-240">**[1]**  Počkejte, dokud se synchronizují nová zařízení drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-240">**[1]** Wait until the new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="f1ae7-241">**[1]**  Vytvořit systémy souborů na zařízeních drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-241">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="f1ae7-242">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="f1ae7-243">**[1]**  Změnit výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-243">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="f1ae7-244">**[1]**  Přidat zařízení drbd systému souborů NFS konfigurace clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-244">**[1]** Add the NFS drbd device to the cluster configuration</span></span>

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

1. <span data-ttu-id="f1ae7-245">**[1]**  Vytvořit server systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-245">**[1]** Create the NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="f1ae7-246">**[1]**  Vytvořit prostředky systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-246">**[1]** Create the NFS File System resources</span></span>

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

1. <span data-ttu-id="f1ae7-247">**[1]**  Vytvořit exportuje systému souborů NFS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-247">**[1]** Create the NFS exports</span></span>

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

1. <span data-ttu-id="f1ae7-248">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="f1ae7-248">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="f1ae7-249">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-249">Create STONITH device</span></span>

<span data-ttu-id="f1ae7-250">STONITH zařízení používá objekt služby k autorizaci s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-250">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="f1ae7-251">Postupujte podle těchto kroků můžete vytvořit objekt služby.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-251">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="f1ae7-252">Přejděte na <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="f1ae7-252">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="f1ae7-253">Otevřete okno Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f1ae7-253">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="f1ae7-254">Přejděte k vlastnostem a poznamenejte si ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-254">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="f1ae7-255">Toto je **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-255">This is the **tenant id**.</span></span>
1. <span data-ttu-id="f1ae7-256">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-256">Click App registrations</span></span>
1. <span data-ttu-id="f1ae7-257">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-257">Click Add</span></span>
1. <span data-ttu-id="f1ae7-258">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="f1ae7-258">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="f1ae7-259">Adresa URL přihlašování se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="f1ae7-259">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="f1ae7-260">Vyberte nové aplikace a na kartě nastavení klikněte na klíče</span><span class="sxs-lookup"><span data-stu-id="f1ae7-260">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="f1ae7-261">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="f1ae7-261">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="f1ae7-262">Poznamenejte si hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-262">Write down the Value.</span></span> <span data-ttu-id="f1ae7-263">Použije se jako **heslo** pro objekt služby</span><span class="sxs-lookup"><span data-stu-id="f1ae7-263">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="f1ae7-264">Poznamenejte si ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-264">Write down the Application Id.</span></span> <span data-ttu-id="f1ae7-265">Se používá jako uživatelské jméno (**přihlašovacího id** v následujících krocích) instančního objektu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-265">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="f1ae7-266">Objekt služby nemá oprávnění pro přístup k prostředkům Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-266">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="f1ae7-267">Musíte poskytnout oprávnění objektu služby spuštění a zastavení (zrušit přidělení) všechny virtuální počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-267">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="f1ae7-268">Přejděte na https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f1ae7-268">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="f1ae7-269">Otevře se okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-269">Open the All resources blade</span></span>
1. <span data-ttu-id="f1ae7-270">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f1ae7-270">Select the virtual machine</span></span>
1. <span data-ttu-id="f1ae7-271">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-271">Click Access control (IAM)</span></span>
1. <span data-ttu-id="f1ae7-272">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-272">Click Add</span></span>
1. <span data-ttu-id="f1ae7-273">Vyberte roli vlastníka</span><span class="sxs-lookup"><span data-stu-id="f1ae7-273">Select the role Owner</span></span>
1. <span data-ttu-id="f1ae7-274">Zadejte název aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="f1ae7-274">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="f1ae7-275">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f1ae7-275">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="f1ae7-276">**[1]**  Vytvořit STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-276">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="f1ae7-277">Poté, co jste upravili oprávnění pro virtuální počítače, můžete nakonfigurovat zařízení STONITH v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-277">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="f1ae7-278">**[1]**  Povolit používání STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-278">**[1]** Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="f1ae7-279">Nastavení (A) SCS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-279">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="f1ae7-280">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="f1ae7-280">Deploying Linux</span></span>

<span data-ttu-id="f1ae7-281">Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro 12 aplikace SAP, který můžete použít k nasazení nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-281">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span> <span data-ttu-id="f1ae7-282">Bitová kopie marketplace obsahuje agenta prostředků pro SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-282">The marketplace image contains the resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="f1ae7-283">Jeden z šablony rychlý start způsobem můžete na githubu nasadit všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-283">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="f1ae7-284">Šablona nasadí virtuální počítače, nástroj pro vyrovnávání zatížení, dostupnosti apod. Postupujte podle těchto kroků nasadíte šablony:</span><span class="sxs-lookup"><span data-stu-id="f1ae7-284">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="f1ae7-285">Otevřete [ASC nebo SCS více SID šablony] [ template-multisid-xscs] nebo [konvergované šablony] [ template-converged] na Azure portálu ASC nebo SCS pouze vytvoří šablona pravidla Vyrovnávání zatížení pro SAP NetWeaver ASC nebo SCS a instance YBRAT (pouze Linux) zatímco sblížené Šablona také vytváří pravidla Vyrovnávání zatížení pro databázi (například Microsoft SQL Server nebo SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="f1ae7-285">Open the [ASCS/SCS Multi SID template][template-multisid-xscs] or the [converged template][template-converged] on the Azure portal The ASCS/SCS template only creates the load-balancing rules for the SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas the converged template also creates the load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="f1ae7-286">Pokud máte v plánu pro instalaci systému SAP NetWeaver na základě a také chcete databázi nainstalovat na stejný počítače, použijte [konvergované šablony][template-converged].</span><span class="sxs-lookup"><span data-stu-id="f1ae7-286">If you plan to install an SAP NetWeaver based system and you also want to install the database on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="f1ae7-287">Zadejte následující parametry</span><span class="sxs-lookup"><span data-stu-id="f1ae7-287">Enter the following parameters</span></span>
   1. <span data-ttu-id="f1ae7-288">Předpona prostředků (pouze šablony SID více ASC nebo SCS)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-288">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="f1ae7-289">Zadejte předponu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-289">Enter the prefix you want to use.</span></span> <span data-ttu-id="f1ae7-290">Hodnota se používá jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-290">The value is used as a prefix for the resources that are deployed.</span></span>
   3. <span data-ttu-id="f1ae7-291">Id systému SAP (pouze sblížené šablony)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-291">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="f1ae7-292">Zadejte Id systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-292">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="f1ae7-293">Identifikátor se používá jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-293">The Id is used as a prefix for the resources that are deployed.</span></span>
   4. <span data-ttu-id="f1ae7-294">Typ zásobníku</span><span class="sxs-lookup"><span data-stu-id="f1ae7-294">Stack Type</span></span>  
      <span data-ttu-id="f1ae7-295">Vyberte typ SAP NetWeaver zásobníku</span><span class="sxs-lookup"><span data-stu-id="f1ae7-295">Select the SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="f1ae7-296">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="f1ae7-296">Os Type</span></span>  
      <span data-ttu-id="f1ae7-297">Vyberte jednu z distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-297">Select one of the Linux distributions.</span></span> <span data-ttu-id="f1ae7-298">V tomto příkladu vyberte SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="f1ae7-298">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="f1ae7-299">Typ databázového</span><span class="sxs-lookup"><span data-stu-id="f1ae7-299">Db Type</span></span>  
      <span data-ttu-id="f1ae7-300">Vyberte HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-300">Select HANA</span></span>
   7. <span data-ttu-id="f1ae7-301">Velikost systému SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-301">Sap System Size</span></span>  
      <span data-ttu-id="f1ae7-302">Množství protokoly SAP poskytuje nový systém.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-302">The amount of SAPS the new system provides.</span></span> <span data-ttu-id="f1ae7-303">Pokud si nejste jisti kolik protokoly SAP vyžaduje systém, požádejte SAP technologie partnera nebo systémový integrátor</span><span class="sxs-lookup"><span data-stu-id="f1ae7-303">If you are not sure how many SAPS the system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="f1ae7-304">Dostupnost systému</span><span class="sxs-lookup"><span data-stu-id="f1ae7-304">System Availability</span></span>  
      <span data-ttu-id="f1ae7-305">Vyberte HA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-305">Select HA</span></span>
   9. <span data-ttu-id="f1ae7-306">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="f1ae7-306">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="f1ae7-307">Po vytvoření nového uživatele, který lze použít pro přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-307">A new user is created that can be used to log on to the machine.</span></span>
   10. <span data-ttu-id="f1ae7-308">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="f1ae7-308">Subnet Id</span></span>  
   <span data-ttu-id="f1ae7-309">ID podsítě, ke které by měl být připojený virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-309">The ID of the subnet to which the virtual machines should be connected to.</span></span>  <span data-ttu-id="f1ae7-310">Ponechte prázdné, pokud chcete vytvořit novou virtuální síť, nebo vyberte stejné podsíti, který můžete použít nebo vytvořit jako součást nasazení serveru NFS.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-310">Leave empty if you want to create a new virtual network or select the same subnet that you used or created as part of the NFS server deployment.</span></span> <span data-ttu-id="f1ae7-311">ID obvykle vypadá /subscriptions/**&lt;id předplatného&gt;**/resourceGroups/**&lt;název skupiny prostředků&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;název virtuální sítě&gt;**/subnets/**&lt;název podsítě.&gt;**</span><span class="sxs-lookup"><span data-stu-id="f1ae7-311">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="f1ae7-312">Instalace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-312">Installation</span></span>

<span data-ttu-id="f1ae7-313">Následující položky jsou předponou buď **[A]** – platí pro všechny uzly, **[1]** – platí jenom pro uzel 1 nebo **[2]** – platí jenom pro uzel 2.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-313">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="f1ae7-314">**[A]**  Aktualizovat SLES</span><span class="sxs-lookup"><span data-stu-id="f1ae7-314">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="f1ae7-315">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="f1ae7-316">**[2]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-316">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="f1ae7-317">**[1]**  Přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="f1ae7-317">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="f1ae7-318">**[A]**  Nainstalovat HA rozšíření</span><span class="sxs-lookup"><span data-stu-id="f1ae7-318">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="f1ae7-319">**[A]**  Aktualizace SAP prostředků agentů</span><span class="sxs-lookup"><span data-stu-id="f1ae7-319">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="f1ae7-320">Oprava pro balíček prostředků agentů je potřeba použít novou konfiguraci, který je popsaný v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-320">A patch for the resource-agents package is required to use the new configuration, that is described in this article.</span></span> <span data-ttu-id="f1ae7-321">Můžete zkontrolovat, pokud je oprava již nainstalována pomocí následujícího příkazu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-321">You can check, if the patch is already installed with the following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="f1ae7-322">Výstup by měl vypadat přibližně</span><span class="sxs-lookup"><span data-stu-id="f1ae7-322">The output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="f1ae7-323">Pokud příkaz grep nenajde parametr IS_ERS, budete muset nainstalovat opravu uvedené na [stránce pro stažení SUSE](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-323">If the grep command does not find the IS_ERS parameter, you need to install the patch listed on [the SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="f1ae7-324">**[A]**  Nastavit rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="f1ae7-324">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="f1ae7-325">Můžete buď použít DNS server, nebo upravit/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-325">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f1ae7-326">Tento příklad ukazuje, jak chcete použít soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-326">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f1ae7-327">Nahraďte adresu IP a název hostitele v následujících příkazech</span><span class="sxs-lookup"><span data-stu-id="f1ae7-327">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="f1ae7-328">Vložte následující řádky, které se/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-328">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f1ae7-329">Změnit IP adresu a název hostitele tak, aby odpovídaly prostředí</span><span class="sxs-lookup"><span data-stu-id="f1ae7-329">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-330">**[1]**  Instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-330">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="f1ae7-331">**[2]**  Přidat uzel do clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-331">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="f1ae7-332">**[A]**  Změnit heslo hacluster na stejné heslo</span><span class="sxs-lookup"><span data-stu-id="f1ae7-332">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="f1ae7-333">**[A]**  Konfigurace corosync používají jiné přenos a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-333">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="f1ae7-334">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-334">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="f1ae7-335">Do souboru přidejte následující obsah tučně.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-335">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="f1ae7-336">Potom restartujte službu corosync</span><span class="sxs-lookup"><span data-stu-id="f1ae7-336">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="f1ae7-337">**[A]**  Nainstalovat komponenty drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-337">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="f1ae7-338">**[A]**  Vytvořit oddíl drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-338">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="f1ae7-339">**[A]**  Vytvořit LVM konfigurace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-339">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-340">**[A]**  Vytvořte SCS drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-340">**[A]** Create the SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="f1ae7-341">Vložit konfigurace pro nové zařízení drbd a ukončení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-341">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f1ae7-342">Vytvořte drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="f1ae7-342">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="f1ae7-343">**[A]**  Vytvořte YBRAT drbd zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-343">**[A]** Create the ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="f1ae7-344">Vložit konfigurace pro nové zařízení drbd a ukončení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-344">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f1ae7-345">Vytvořte drbd zařízení a spusťte ji</span><span class="sxs-lookup"><span data-stu-id="f1ae7-345">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f1ae7-346">**[1]**  Přeskočit počáteční synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-346">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f1ae7-347">**[1]**  Nastavte primární uzel</span><span class="sxs-lookup"><span data-stu-id="f1ae7-347">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f1ae7-348">**[1]**  Počkejte, dokud se synchronizují nová zařízení drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-348">**[1]** Wait until the new drbd devices are synchronized</span></span>

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

1. <span data-ttu-id="f1ae7-349">**[1]**  Vytvořit systémy souborů na zařízeních drbd</span><span class="sxs-lookup"><span data-stu-id="f1ae7-349">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="f1ae7-350">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-350">Configure Cluster Framework</span></span>

<span data-ttu-id="f1ae7-351">**[1]**  Změnit výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-351">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="f1ae7-352">Příprava pro SAP NetWeaver instalace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-352">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="f1ae7-353">**[A]**  Vytvoření sdíleného adresáře</span><span class="sxs-lookup"><span data-stu-id="f1ae7-353">**[A]** Create the shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="f1ae7-354">**[A]**  Konfigurace autofs</span><span class="sxs-lookup"><span data-stu-id="f1ae7-354">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="f1ae7-355">Vytvořte soubor s</span><span class="sxs-lookup"><span data-stu-id="f1ae7-355">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="f1ae7-356">Restartujte autofs připojit nové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-356">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="f1ae7-357">**[A]**  Konfigurace ODKLÁDACÍHO souboru</span><span class="sxs-lookup"><span data-stu-id="f1ae7-357">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="f1ae7-358">Restartujte agenta aktivovat změn</span><span class="sxs-lookup"><span data-stu-id="f1ae7-358">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="f1ae7-359">Instalace SAP NetWeaver ASC nebo YBRAT</span><span class="sxs-lookup"><span data-stu-id="f1ae7-359">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="f1ae7-360">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="f1ae7-360">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

   <span data-ttu-id="f1ae7-361">Ujistěte se, zda je stav clusteru ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-361">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f1ae7-362">Není důležité, na který uzel prostředky jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-362">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f1ae7-363">**[1]**  Nainstalovat SAP NetWeaver ASC</span><span class="sxs-lookup"><span data-stu-id="f1ae7-363">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="f1ae7-364">Instalace SAP NetWeaver ASC jako kořenového na prvním uzlu pomocí virtuální název hostitele, který se mapuje na adresu IP front-endové konfigurace služby Vyrovnávání zatížení pro ASC například <b>nws Asc</b>, <b>10.0.0.10</b> a číslo instance, který jste použili pro kontrolu služby Vyrovnávání zatížení například <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-364">Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and the instance number that you used for the probe of the load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="f1ae7-365">Parametr sapinst SAPINST_REMOTE_ACCESS_USER můžete povolit uživateli nekořenovými pro připojení k sapinst.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-365">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-366">**[1]**  Vytvoření virtuální IP prostředků a test stavu nástroje pro vyrovnávání zatížení interní</span><span class="sxs-lookup"><span data-stu-id="f1ae7-366">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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
   # Do you still want to commit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="f1ae7-367">Ujistěte se, zda je stav clusteru ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-367">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f1ae7-368">Není důležité, na který uzel prostředky jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-368">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f1ae7-369">**[2]**  Nainstalovat SAP NetWeaver YBRAT</span><span class="sxs-lookup"><span data-stu-id="f1ae7-369">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="f1ae7-370">Instalace SAP NetWeaver YBRAT jako kořenového na druhém uzlu pomocí virtuální název hostitele, který se mapuje na adresu IP front-endové konfigurace služby Vyrovnávání zatížení pro YBRAT například <b>nws ybrat</b>, <b>10.0.0.11</b> a číslo instance, který jste použili pro kontrolu služby Vyrovnávání zatížení například <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-370">Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and the instance number that you used for the probe of the load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="f1ae7-371">Parametr sapinst SAPINST_REMOTE_ACCESS_USER můžete povolit uživateli nekořenovými pro připojení k sapinst.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-371">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="f1ae7-372">Použijte prosím SWPM SP 20 PL 05 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-372">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="f1ae7-373">Nižší verze nenastavujte oprávnění správně a instalace se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-373">Lower versions do not set the permissions correctly and the installation will fail.</span></span>
   > 

1. <span data-ttu-id="f1ae7-374">**[1]**  Adapt ASC nebo SCS a YBRAT instance profily</span><span class="sxs-lookup"><span data-stu-id="f1ae7-374">**[1]** Adapt the ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="f1ae7-375">ASC nebo SCS profilu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-375">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="f1ae7-376">YBRAT profilu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-376">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="f1ae7-377">**[A]**  Konfigurace zachování</span><span class="sxs-lookup"><span data-stu-id="f1ae7-377">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="f1ae7-378">Komunikace mezi serverem aplikace SAP NetWeaver a ASC nebo SCS směrován přes softwarovému Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-378">The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="f1ae7-379">Nástroje pro vyrovnávání zatížení odpojí neaktivní připojení po vypršení časového limitu se dají konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-379">The load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="f1ae7-380">K tomu potřebujete nastavení parametru v profilu SAP NetWeaver ASC nebo SCS a změnit nastavení systému Linux.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-380">To prevent this you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings.</span></span> <span data-ttu-id="f1ae7-381">Přečtěte si prosím [1410736 Poznámka SAP] [ 1410736] Další informace.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-381">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="f1ae7-382">Již byl přidán ASC nebo SCS profil parametr enque/encni/set_so_keepalive v posledním kroku.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-382">The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.</span></span>

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="f1ae7-383">**[A]**  Nakonfigurujte uživatele, SAP po instalaci</span><span class="sxs-lookup"><span data-stu-id="f1ae7-383">**[A]** Configure the SAP users after the installation</span></span>
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="f1ae7-384">**[1]**  Přidejte do souboru sapservice služby ASC a YBRAT SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-384">**[1]** Add the ASCS and ERS SAP services to the sapservice file</span></span>

   <span data-ttu-id="f1ae7-385">Přidáte ASC služby položku na druhém uzlu a zkopírujte položku služby YBRAT do prvního uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-385">Add the ASCS service entry to the second node and copy the ERS service entry to the first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="f1ae7-386">**[1]**  Vytvořit prostředky clusteru SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-386">**[1]** Create the SAP cluster resources</span></span>

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

   <span data-ttu-id="f1ae7-387">Ujistěte se, zda je stav clusteru ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-387">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f1ae7-388">Není důležité, na který uzel prostředky jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-388">It is not important on which node the resources are running.</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="f1ae7-389">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-389">Create STONITH device</span></span>

<span data-ttu-id="f1ae7-390">STONITH zařízení používá objekt služby k autorizaci s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-390">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="f1ae7-391">Postupujte podle těchto kroků můžete vytvořit objekt služby.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-391">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="f1ae7-392">Přejděte na <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="f1ae7-392">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="f1ae7-393">Otevřete okno Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f1ae7-393">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="f1ae7-394">Přejděte k vlastnostem a poznamenejte si ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-394">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="f1ae7-395">Toto je **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-395">This is the **tenant id**.</span></span>
1. <span data-ttu-id="f1ae7-396">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-396">Click App registrations</span></span>
1. <span data-ttu-id="f1ae7-397">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-397">Click Add</span></span>
1. <span data-ttu-id="f1ae7-398">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="f1ae7-398">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="f1ae7-399">Adresa URL přihlašování se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="f1ae7-399">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="f1ae7-400">Vyberte nové aplikace a na kartě nastavení klikněte na klíče</span><span class="sxs-lookup"><span data-stu-id="f1ae7-400">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="f1ae7-401">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="f1ae7-401">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="f1ae7-402">Poznamenejte si hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-402">Write down the Value.</span></span> <span data-ttu-id="f1ae7-403">Použije se jako **heslo** pro objekt služby</span><span class="sxs-lookup"><span data-stu-id="f1ae7-403">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="f1ae7-404">Poznamenejte si ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-404">Write down the Application Id.</span></span> <span data-ttu-id="f1ae7-405">Se používá jako uživatelské jméno (**přihlašovacího id** v následujících krocích) instančního objektu</span><span class="sxs-lookup"><span data-stu-id="f1ae7-405">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="f1ae7-406">Objekt služby nemá oprávnění pro přístup k prostředkům Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-406">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="f1ae7-407">Musíte poskytnout oprávnění objektu služby spuštění a zastavení (zrušit přidělení) všechny virtuální počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-407">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="f1ae7-408">Přejděte na https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f1ae7-408">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="f1ae7-409">Otevře se okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-409">Open the All resources blade</span></span>
1. <span data-ttu-id="f1ae7-410">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="f1ae7-410">Select the virtual machine</span></span>
1. <span data-ttu-id="f1ae7-411">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-411">Click Access control (IAM)</span></span>
1. <span data-ttu-id="f1ae7-412">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-412">Click Add</span></span>
1. <span data-ttu-id="f1ae7-413">Vyberte roli vlastníka</span><span class="sxs-lookup"><span data-stu-id="f1ae7-413">Select the role Owner</span></span>
1. <span data-ttu-id="f1ae7-414">Zadejte název aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="f1ae7-414">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="f1ae7-415">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="f1ae7-415">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="f1ae7-416">**[1]**  Vytvořit STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-416">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="f1ae7-417">Poté, co jste upravili oprávnění pro virtuální počítače, můžete nakonfigurovat zařízení STONITH v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-417">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="f1ae7-418">**[1]**  Povolit používání STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-418">**[1]** Enable the use of a STONITH device</span></span>

<span data-ttu-id="f1ae7-419">Povolit použití funkce STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="f1ae7-419">Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="f1ae7-420">Instalace databáze</span><span class="sxs-lookup"><span data-stu-id="f1ae7-420">Install database</span></span>

<span data-ttu-id="f1ae7-421">V tomto příkladu je replikaci systému SAP HANA nainstalovaný a nakonfigurovaný.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-421">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="f1ae7-422">Ve stejném clusteru jako SAP NetWeaver ASC nebo SCS a YBRAT se spustí SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-422">SAP HANA will run in the same cluster as the SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="f1ae7-423">Můžete taky nainstalovat SAP HANA na vyhrazeném clusteru.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-423">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="f1ae7-424">V tématu [vysokou dostupnost z SAP HANA ve virtuálních počítačích Azure (VM)] [ sap-hana-ha] Další informace.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-424">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="f1ae7-425">Příprava pro instalaci SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-425">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="f1ae7-426">Obecně doporučujeme používat LVM pro svazky, které ukládají data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-426">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="f1ae7-427">Pro účely testování můžete také zvolit k uložení dat a souboru přímo na prostý disku protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-427">For testing purposes, you can also choose to store the data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="f1ae7-428">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="f1ae7-428">**[A]** LVM</span></span>  
   <span data-ttu-id="f1ae7-429">Následující příklad předpokládá, že máte virtuální počítače čtyři datových disků připojených, které se mají použít k vytvoření dva svazky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-429">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
   
   <span data-ttu-id="f1ae7-430">Vytvořte fyzických svazků pro všechny disky, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-430">Create physical volumes for all disks that you want to use.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="f1ae7-431">Vytvoření skupiny svazku pro datové soubory, jedna skupina svazku pro soubory protokolů a jeden pro do sdíleného adresáře SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-431">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="f1ae7-432">Vytvoření logické svazky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-432">Create the logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="f1ae7-433">Vytvořte připojení adresáře a zkopírujte identifikátor UUID všechny logické svazky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-433">Create the mount directories and copy the UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="f1ae7-434">Vytvořit záznamy autofs pro tři logické svazky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-434">Create autofs entries for the three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="f1ae7-435">Vložení tohoto řádku sudo vi /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="f1ae7-435">Insert this line to sudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="f1ae7-436">Připojit nové svazky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-436">Mount the new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="f1ae7-437">**[A]**  Prostý disky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-437">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="f1ae7-438">Pro malé nebo ukázku systémů, můžete umístit HANA soubory protokolu a data na jeden disk.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-438">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="f1ae7-439">Následující příkazy na /dev/sdc vytvořit oddíl a naformátovat s xfs.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-439">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down the id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="f1ae7-440">Vložení tohoto řádku /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="f1ae7-440">Insert this line to /etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="f1ae7-441">Vytvořte cílový adresář a připojení disku.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-441">Create the target directory and mount the disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="f1ae7-442">Instalace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-442">Installing SAP HANA</span></span>

<span data-ttu-id="f1ae7-443">Následující kroky jsou založeny na kapitoly 4 z [SAP HANA SR výkonu optimalizované scénář průvodce] [ suse-hana-ha-guide] nainstalovat replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-443">The following steps are based on chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span> <span data-ttu-id="f1ae7-444">Přečtěte si ji před pokračováním instalace.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-444">Please read it before you continue the installation.</span></span>

1. <span data-ttu-id="f1ae7-445">**[A]**  Hdblcm spustit z disku DVD HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-445">**[A]** Run hdblcm from the HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="f1ae7-446">**[A]**  Upgradu agenta hostitele SAP</span><span class="sxs-lookup"><span data-stu-id="f1ae7-446">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="f1ae7-447">Stáhněte si nejnovější archivu SAP Agent hostitele z [SAP Softwarecenter] [ sap-swcenter] a spusťte následující příkaz k aktualizaci agenta.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-447">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="f1ae7-448">Nahraďte cestu do archivu tak, aby odkazoval na soubor, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-448">Replace the path to the archive to point to the file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path to SAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="f1ae7-449">**[1]**  Vytvořit HANA replikace (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-449">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="f1ae7-450">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-450">Run the following command.</span></span> <span data-ttu-id="f1ae7-451">Nezapomeňte nahradit tučné řetězce (HANA systému ID HDB a číslo instance 03) s hodnotami instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-451">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="f1ae7-452">**[A]**  Vytvořit položku úložiště klíčů (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-452">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-453">**[1]**  Zálohy databáze (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="f1ae7-453">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="f1ae7-454">**[1]**  Přepnout uživatele sapsid HANA a vytvořit primární lokality.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-454">**[1]** Switch to the HANA sapsid user and create the primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-455">**[2]**  Přepnout uživatele sapsid HANA a vytvořit sekundární lokalitu.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-455">**[2]** Switch to the HANA sapsid user and create the secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="f1ae7-456">**[1]**  Prostředky clusteru vytvořit SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-456">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="f1ae7-457">Nejprve vytvořte topologii.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-457">First, create the topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number and HANA system id
   
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
   
   <span data-ttu-id="f1ae7-458">Dále vytvořte HANA prostředky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-458">Next, create the HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
    
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

   <span data-ttu-id="f1ae7-459">Ujistěte se, zda je stav clusteru ok a zda jsou spuštěny všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-459">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f1ae7-460">Není důležité, na který uzel prostředky jsou spuštěné.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-460">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f1ae7-461">**[1]**  Nainstalovat instanci databáze SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="f1ae7-461">**[1]** Install the SAP NetWeaver database instance</span></span>

   <span data-ttu-id="f1ae7-462">Instalovat instanci databáze SAP NetWeaver jako kořenové pomocí virtuální název hostitele, který se mapuje na adresu IP front-endové konfigurace služby Vyrovnávání zatížení pro databázi, například <b>nws-db</b> a <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-462">Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="f1ae7-463">Parametr sapinst SAPINST_REMOTE_ACCESS_USER můžete povolit uživateli nekořenovými pro připojení k sapinst.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-463">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="f1ae7-464">Instalace serveru aplikace SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="f1ae7-464">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="f1ae7-465">Postupujte podle těchto kroků nainstalujte SAP aplikační server.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-465">Follow these steps to install an SAP application server.</span></span> <span data-ttu-id="f1ae7-466">Mocí následujících kroky předpokládají, nainstalovat aplikační server na server, která se liší od serverů ASC nebo SCS a HANA.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-466">The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="f1ae7-467">V opačném případě některé z následujících kroků (např. konfigurace rozlišení názvu hostitele) nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-467">Otherwise some of the steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="f1ae7-468">Instalační program rozlišení názvu hostitele</span><span class="sxs-lookup"><span data-stu-id="f1ae7-468">Setup host name resolution</span></span>    
   <span data-ttu-id="f1ae7-469">Můžete buď použít DNS server, nebo upravit/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-469">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f1ae7-470">Tento příklad ukazuje, jak chcete použít soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-470">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f1ae7-471">Nahraďte adresu IP a název hostitele v následujících příkazech</span><span class="sxs-lookup"><span data-stu-id="f1ae7-471">Replace the IP address and the hostname in the following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="f1ae7-472">Vložte následující řádky, které se/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-472">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f1ae7-473">Změnit IP adresu a název hostitele tak, aby odpovídaly prostředí</span><span class="sxs-lookup"><span data-stu-id="f1ae7-473">Change the IP address and hostname to match your environment</span></span>    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of the application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-474">Vytvoření adresáře sapmnt</span><span class="sxs-lookup"><span data-stu-id="f1ae7-474">Create the sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="f1ae7-475">Konfigurace autofs</span><span class="sxs-lookup"><span data-stu-id="f1ae7-475">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="f1ae7-476">Vytvořte nový soubor s</span><span class="sxs-lookup"><span data-stu-id="f1ae7-476">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="f1ae7-477">Restartujte autofs připojit nové sdílené složky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-477">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="f1ae7-478">Konfigurace ODKLÁDACÍ soubor</span><span class="sxs-lookup"><span data-stu-id="f1ae7-478">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="f1ae7-479">Restartujte agenta aktivovat změn</span><span class="sxs-lookup"><span data-stu-id="f1ae7-479">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="f1ae7-480">Instalace serveru SAP NetWeaver aplikace</span><span class="sxs-lookup"><span data-stu-id="f1ae7-480">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="f1ae7-481">Instalace serveru primární nebo další aplikace SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-481">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="f1ae7-482">Parametr sapinst SAPINST_REMOTE_ACCESS_USER můžete povolit uživateli nekořenovými pro připojení k sapinst.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-482">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="f1ae7-483">Zabezpečené úložiště pověření aktualizace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f1ae7-483">Update SAP HANA secure store</span></span>

   <span data-ttu-id="f1ae7-484">Aktualizujte zabezpečené úložiště SAP HANA tak, aby odkazoval na virtuální název tohoto nastavení replikace systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="f1ae7-484">Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="f1ae7-485">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1ae7-485">Next steps</span></span>
* <span data-ttu-id="f1ae7-486">[Azure virtuálních počítačů, plánování a implementace pro SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-486">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="f1ae7-487">[Nasazení virtuálních počítačů Azure pro SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-487">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="f1ae7-488">[Nasazení virtuálních počítačů databázového systému Azure pro SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-488">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="f1ae7-489">Další informace o vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA v Azure (velké instance) naleznete v tématu [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="f1ae7-489">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="f1ae7-490">Další informace o vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA na virtuálních počítačích Azure naleznete v tématu [vysokou dostupnost z SAP HANA ve virtuálních počítačích Azure (VM)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="f1ae7-490">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>