---
title: "Vysoká dostupnost SAP HANA na virtuálních počítačích Azure (VM) | Microsoft Docs"
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
ms.openlocfilehash: 951150e621d21037b0adde7287b9f985290d8d11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="8fc8c-103">Vysoká dostupnost SAP HANA na virtuálních počítačích Azure (VM)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="8fc8c-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="8fc8c-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="8fc8c-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="8fc8c-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="8fc8c-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="8fc8c-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="8fc8c-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="8fc8c-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="8fc8c-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="8fc8c-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="8fc8c-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="8fc8c-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="8fc8c-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="8fc8c-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="8fc8c-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="8fc8c-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="8fc8c-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="8fc8c-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="8fc8c-113">Na místě, můžete použít buď replikaci HANA systému nebo používat sdílené úložiště, k vytvoření vysoké dostupnosti pro SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-113">On-premises, you can use either HANA System Replication or use shared storage to establish high availability for SAP HANA.</span></span>
<span data-ttu-id="8fc8c-114">Momentálně podporujeme jenom nastavení replikace systému HANA v Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="8fc8c-115">SAP HANA replikace se skládá z jednoho hlavní uzel a alespoň jeden podřízený uzel.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="8fc8c-116">Změny dat na hlavní uzel se replikují na podřízené uzly synchronně nebo asynchronně.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-116">Changes to the data on the master node are replicated to the slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="8fc8c-117">Tento článek popisuje postup nasazení virtuálních počítačů, konfiguraci virtuálních počítačů, nainstalujte rozhraní framework clusteru, nainstalovat a nakonfigurovat replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-117">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="8fc8c-118">V konfiguraci příklad instalace příkazy čísla instance atd 03 a HDB ID HANA systému se používá.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-118">In the example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="8fc8c-119">Přečtěte si tyto poznámky k SAP a dokumenty Paper nejprve</span><span class="sxs-lookup"><span data-stu-id="8fc8c-119">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="8fc8c-120">Poznámka SAP [1928533], na kterém je:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="8fc8c-121">Seznam velikostí virtuálních počítačů Azure, které jsou podporovány pro nasazení softwaru SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-121">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="8fc8c-122">Kapacita důležité informace o velikosti virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="8fc8c-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="8fc8c-123">Podporované SAP software a operační systém (OS) a kombinace databáze</span><span class="sxs-lookup"><span data-stu-id="8fc8c-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="8fc8c-124">Požadovaná verze SAP jádra pro Windows a Linux v Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8fc8c-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="8fc8c-125">Poznámka SAP [2015553] uvádí požadavky pro nasazení softwaru podporovaných SAP SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="8fc8c-126">Poznámka SAP [2205917] se doporučuje nastavení operačního systému SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="8fc8c-127">Poznámka SAP [1944799] má SAP HANA pokyny pro SUSE Linux Enterprise Server pro aplikace SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="8fc8c-128">Poznámka SAP [2178632] obsahuje podrobné informace o veškeré monitorování metriky pro SAP v Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="8fc8c-129">Poznámka SAP [2191498] má požadovaná verze SAP hostitele agenta pro Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-129">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="8fc8c-130">Poznámka SAP [2243692] obsahuje informace o licencích SAP v systému Linux v Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="8fc8c-131">Poznámka SAP [1984787] má obecné informace o SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="8fc8c-132">Poznámka SAP [1999351] Další informace o řešení problémů s Azure rozšířené monitorování rozšíření pro SAP.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-132">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="8fc8c-133">[SAP komunity WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) má všechny požadované SAP poznámky pro Linux.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="8fc8c-134">[Azure virtuálních počítačů, plánování a implementace pro SAP v systému Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="8fc8c-135">[Nasazení virtuálních počítačů Azure pro SAP v systému Linux (v tomto článku)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="8fc8c-136">[Nasazení virtuálních počítačů databázového systému Azure pro SAP v systému Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="8fc8c-137">[SAP HANA SR výkonu optimalizované scénář] [ suse-hana-ha-guide] průvodci obsahuje všechny požadované informace pro nastavení replikace systému SAP HANA na místě.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="8fc8c-138">Tohoto průvodce použijte jako Směrný plán.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="8fc8c-139">Nasazení Linux</span><span class="sxs-lookup"><span data-stu-id="8fc8c-139">Deploying Linux</span></span>

<span data-ttu-id="8fc8c-140">Agent prostředků pro SAP HANA je součástí systému SUSE Linux Enterprise Server pro aplikace SAP.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-140">The resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="8fc8c-141">Azure Marketplace obsahuje bitovou kopii pro SUSE Linux Enterprise Server pro SAP 12 aplikace s BYOS (přineste si vlastní předplatné), který můžete použít k nasazení nových virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-141">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use to deploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="8fc8c-142">Ruční nasazení</span><span class="sxs-lookup"><span data-stu-id="8fc8c-142">Manual Deployment</span></span>

1. <span data-ttu-id="8fc8c-143">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="8fc8c-143">Create a Resource Group</span></span>
1. <span data-ttu-id="8fc8c-144">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="8fc8c-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="8fc8c-145">Vytvořte dva účty úložiště</span><span class="sxs-lookup"><span data-stu-id="8fc8c-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="8fc8c-146">Vytvořit skupinu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="8fc8c-146">Create an Availability Set</span></span>  
   <span data-ttu-id="8fc8c-147">Sada maximální aktualizace domény</span><span class="sxs-lookup"><span data-stu-id="8fc8c-147">Set max update domain</span></span>
1. <span data-ttu-id="8fc8c-148">Vytvořit nástroj pro vyrovnávání zatížení (interní)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="8fc8c-149">Vyberte virtuální síť kroku výše</span><span class="sxs-lookup"><span data-stu-id="8fc8c-149">Select VNET of step above</span></span>
1. <span data-ttu-id="8fc8c-150">Vytvoření virtuálního počítače 1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="8fc8c-151">https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="8fc8c-152">SLES pro SAP aplikace 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="8fc8c-153">Vyberte účet úložiště 1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="8fc8c-154">Vyberte sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-154">Select Availability Set</span></span>  
1. <span data-ttu-id="8fc8c-155">Vytvoření virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="8fc8c-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="8fc8c-156">https://Portal.Azure.com/#Create/SuSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="8fc8c-157">SLES pro SAP aplikace 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="8fc8c-158">Vyberte účet úložiště 2</span><span class="sxs-lookup"><span data-stu-id="8fc8c-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="8fc8c-159">Vyberte sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-159">Select Availability Set</span></span>  
1. <span data-ttu-id="8fc8c-160">Přidat datových disků</span><span class="sxs-lookup"><span data-stu-id="8fc8c-160">Add Data Disks</span></span>
1. <span data-ttu-id="8fc8c-161">Konfigurace pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8fc8c-161">Configure the load balancer</span></span>
    1. <span data-ttu-id="8fc8c-162">Vytvořte fond IP front-endu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="8fc8c-163">Otevřete nástroj pro vyrovnávání zatížení, vyberte fond IP front-endu a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="8fc8c-163">Open the load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="8fc8c-164">Zadejte název nového fondu IP front-endu (například hana-front-endu)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-164">Enter the name of the new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="8fc8c-165">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="8fc8c-165">Click OK</span></span>
        1. <span data-ttu-id="8fc8c-166">Po vytvoření nového fondu IP front-endu, zapište si jeho IP adresu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-166">After the new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="8fc8c-167">Vytvořte fond back-end</span><span class="sxs-lookup"><span data-stu-id="8fc8c-167">Create a backend pool</span></span>
        1. <span data-ttu-id="8fc8c-168">Otevřete nástroj pro vyrovnávání zatížení, zvolte back-endové fondy a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="8fc8c-168">Open the load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="8fc8c-169">Zadejte název nového fondu back-end (například hana-back-end)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-169">Enter the name of the new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="8fc8c-170">Klikněte na tlačítko Přidat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="8fc8c-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="8fc8c-171">Vyberte jste dříve vytvořili sadu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="8fc8c-171">Select the Availability Set you created earlier</span></span>
        1. <span data-ttu-id="8fc8c-172">Vyberte virtuální počítače clusteru SAP HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-172">Select the virtual machines of the SAP HANA cluster</span></span>
        1. <span data-ttu-id="8fc8c-173">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="8fc8c-173">Click OK</span></span>
    1. <span data-ttu-id="8fc8c-174">Vytvoření test stavu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-174">Create a health probe</span></span>
       1. <span data-ttu-id="8fc8c-175">Otevřete nástroj pro vyrovnávání zatížení, zvolte sondy stavu služby a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="8fc8c-175">Open the load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="8fc8c-176">Zadejte název nové kontroly stavu (například hana-hp)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-176">Enter the name of the new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="8fc8c-177">Vyberte TCP jako protokol, port 625**03**, zachovat Interval 5 a prahová hodnota špatného stavu 2</span><span class="sxs-lookup"><span data-stu-id="8fc8c-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="8fc8c-178">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="8fc8c-178">Click OK</span></span>
    1. <span data-ttu-id="8fc8c-179">Vytvoření pravidel vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="8fc8c-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="8fc8c-180">Otevřete nástroj pro vyrovnávání zatížení, zvolte pravidla Vyrovnávání zatížení a klikněte na tlačítko Přidat</span><span class="sxs-lookup"><span data-stu-id="8fc8c-180">Open the load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="8fc8c-181">Zadejte název nové pravidlo Vyrovnávání zatížení (například hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-181">Enter the name of the new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="8fc8c-182">Vyberte IP adresu front-endu a back-endový fond a stav testu jste vytvořili dříve (například hana-front-endu)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-182">Select the frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="8fc8c-183">Zachovat protokol TCP, zadejte port 3**03**15</span><span class="sxs-lookup"><span data-stu-id="8fc8c-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="8fc8c-184">Časový limit nečinnosti zvýšení do 30 minut</span><span class="sxs-lookup"><span data-stu-id="8fc8c-184">Increase idle timeout to 30 minutes</span></span>
       1. <span data-ttu-id="8fc8c-185">**Nezapomeňte povolit plovoucí IP adresa**</span><span class="sxs-lookup"><span data-stu-id="8fc8c-185">**Make sure to enable Floating IP**</span></span>
        1. <span data-ttu-id="8fc8c-186">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="8fc8c-186">Click OK</span></span>
        1. <span data-ttu-id="8fc8c-187">Opakujte předchozí kroky pro port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="8fc8c-187">Repeat the steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="8fc8c-188">Nasazení pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8fc8c-188">Deploy with template</span></span>
<span data-ttu-id="8fc8c-189">Jeden z šablony rychlý start způsobem můžete na githubu nasadit všechny požadované prostředky.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-189">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="8fc8c-190">Šablona nasadí virtuální počítače, nástroj pro vyrovnávání zatížení, dostupnosti apod. Postupujte podle těchto kroků nasadíte šablony:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-190">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="8fc8c-191">Otevřete [databáze šablony] [ template-multisid-db] nebo [konvergované šablony] [ template-converged] na portálu Azure, pouze vytvoří šablona databáze Vyrovnávání zatížení pravidel pro databáze, zatímco sblížené Šablona také vytváří pravidla Vyrovnávání zatížení ASC nebo SCS a instance YBRAT (pouze Linux).</span><span class="sxs-lookup"><span data-stu-id="8fc8c-191">Open the [database template][template-multisid-db] or the [converged template][template-converged] on the Azure Portal The database template only creates the load-balancing rules for a database whereas the converged template also creates the load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="8fc8c-192">Pokud máte v plánu pro instalaci systému SAP NetWeaver na základě a také chcete nainstalovat instanci ASC nebo SCS stejné počítače, použijte [konvergované šablony][template-converged].</span><span class="sxs-lookup"><span data-stu-id="8fc8c-192">If you plan to install an SAP NetWeaver based system and you also want to install the ASCS/SCS instance on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="8fc8c-193">Zadejte následující parametry</span><span class="sxs-lookup"><span data-stu-id="8fc8c-193">Enter the following parameters</span></span>
    1. <span data-ttu-id="8fc8c-194">Id systému SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-194">Sap System Id</span></span>  
       <span data-ttu-id="8fc8c-195">Zadejte Id systému SAP systému SAP, který chcete nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-195">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="8fc8c-196">Identifikátor se použije jako předpona pro prostředky, které jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-196">The Id will be used as a prefix for the resources that are deployed.</span></span>
    1. <span data-ttu-id="8fc8c-197">Typ zásobníku (platí pouze pokud použijete šablonu sblížené)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-197">Stack Type (only applicable if you use the converged template)</span></span>  
       <span data-ttu-id="8fc8c-198">Vyberte typ SAP NetWeaver zásobníku</span><span class="sxs-lookup"><span data-stu-id="8fc8c-198">Select the SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="8fc8c-199">Typ operačního systému</span><span class="sxs-lookup"><span data-stu-id="8fc8c-199">Os Type</span></span>  
       <span data-ttu-id="8fc8c-200">Vyberte jednu z distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-200">Select one of the Linux distributions.</span></span> <span data-ttu-id="8fc8c-201">V tomto příkladu vyberte SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="8fc8c-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="8fc8c-202">Typ databázového</span><span class="sxs-lookup"><span data-stu-id="8fc8c-202">Db Type</span></span>  
       <span data-ttu-id="8fc8c-203">Vyberte HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-203">Select HANA</span></span>
    1. <span data-ttu-id="8fc8c-204">Velikost systému SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-204">Sap System Size</span></span>  
       <span data-ttu-id="8fc8c-205">Množství protokoly SAP bude poskytovat nový systém.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-205">The amount of SAPS the new system will provide.</span></span> <span data-ttu-id="8fc8c-206">Pokud si nejste jisti kolik protokoly SAP, systém bude vyžadovat, požádejte SAP technologie partnera nebo systémový integrátor</span><span class="sxs-lookup"><span data-stu-id="8fc8c-206">If you are not sure how many SAPS the system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="8fc8c-207">Dostupnost systému</span><span class="sxs-lookup"><span data-stu-id="8fc8c-207">System Availability</span></span>  
       <span data-ttu-id="8fc8c-208">Vyberte HA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-208">Select HA</span></span>
    1. <span data-ttu-id="8fc8c-209">Uživatelské jméno správce a heslo správce</span><span class="sxs-lookup"><span data-stu-id="8fc8c-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="8fc8c-210">Po vytvoření nového uživatele, který lze použít pro přihlášení k počítači.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-210">A new user is created that can be used to log on to the machine.</span></span>
    1. <span data-ttu-id="8fc8c-211">Nový nebo existující podsíť</span><span class="sxs-lookup"><span data-stu-id="8fc8c-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="8fc8c-212">Určuje, zda mají být vytvořeny nové virtuální sítě a podsítě nebo by měl použít existující podsítí.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="8fc8c-213">Pokud již máte virtuální síť, která je připojen k síti na pracovišti, vyberte existující.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-213">If you already have a virtual network that is connected to your on-premises network, select existing.</span></span>
    1. <span data-ttu-id="8fc8c-214">Id podsítě</span><span class="sxs-lookup"><span data-stu-id="8fc8c-214">Subnet Id</span></span>  
    <span data-ttu-id="8fc8c-215">ID podsítě, ke které by měl být připojený virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-215">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="8fc8c-216">Vyberte podsíť virtuální sítě VPN nebo Expressroute připojit virtuální počítač k síti na pracovišti.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-216">Select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="8fc8c-217">ID obvykle vypadá /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="8fc8c-217">The ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="8fc8c-218">Nastavení Linux HA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-218">Setting up Linux HA</span></span>

<span data-ttu-id="8fc8c-219">Následující položky jsou s předponou buď [A] - platí pro všechny uzly [1] - platí jenom pro uzel 1 nebo [2] - platí jenom pro uzel 2.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-219">The following items are prefixed with either [A] - applicable to all nodes, [1] - only applicable to node 1 or [2] - only applicable to node 2.</span></span>

1. <span data-ttu-id="8fc8c-220">[A] SLES pro SAP BYOS jenom - SLES zaregistrovat, abyste mohli použít úložiště</span><span class="sxs-lookup"><span data-stu-id="8fc8c-220">[A] SLES for SAP BYOS only - Register SLES to be able to use the repositories</span></span>
1. <span data-ttu-id="8fc8c-221">[A] SLES pro SAP BYOS pouze – přidání modulu veřejného cloudu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="8fc8c-222">[A] aktualizace SLES</span><span class="sxs-lookup"><span data-stu-id="8fc8c-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="8fc8c-223">[1] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="8fc8c-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="8fc8c-224">[2] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="8fc8c-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="8fc8c-225">[1] přístupu ssh</span><span class="sxs-lookup"><span data-stu-id="8fc8c-225">[1] Enable ssh access</span></span>
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="8fc8c-226">[A] nainstalovat rozšíření HA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="8fc8c-227">[A] rozložení disku instalační program</span><span class="sxs-lookup"><span data-stu-id="8fc8c-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="8fc8c-228">LVM</span><span class="sxs-lookup"><span data-stu-id="8fc8c-228">LVM</span></span>  
    <span data-ttu-id="8fc8c-229">Obecně doporučujeme používat LVM pro svazky, které ukládají data a soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-229">We generally recommend to use LVM for volumes that store data and log files.</span></span> <span data-ttu-id="8fc8c-230">Následující příklad předpokládá, že máte virtuální počítače čtyři datových disků připojených, které se mají použít k vytvoření dva svazky.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-230">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
        * <span data-ttu-id="8fc8c-231">Vytvořte fyzických svazků pro všechny disky, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-231">Create physical volumes for all disks that you want to use.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="8fc8c-232">Vytvoření skupiny svazku pro datové soubory, jedna skupina svazku pro soubory protokolů a jeden pro do sdíleného adresáře SAP HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-232">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="8fc8c-233">Vytvoření logické svazky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-233">Create the logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="8fc8c-234">Vytvořte připojení adresáře a zkopírujte identifikátor UUID všechny logické svazky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-234">Create the mount directories and copy the UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="8fc8c-235">Vytvoření položky fstab pro tři logické svazky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-235">Create fstab entries for the three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="8fc8c-236">Vložení tohoto řádku /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="8fc8c-236">Insert this line to /etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="8fc8c-237">Připojit nové svazky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-237">Mount the new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="8fc8c-238">Nešifrovaná disky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-238">Plain Disks</span></span>  
       <span data-ttu-id="8fc8c-239">Pro malé nebo ukázku systémů, můžete umístit HANA soubory protokolu a data na jeden disk.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="8fc8c-240">Následující příkazy na /dev/sdc vytvořit oddíl a naformátovat s xfs.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-240">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a><span data-ttu-id="8fc8c-241">Poznamenejte si id /dev/sdc1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-241">write down the id of /dev/sdc1</span></span>
    <span data-ttu-id="8fc8c-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="8fc8c-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="8fc8c-243">[A] instalační program rozlišení názvu hostitele pro všechny hostitele</span><span class="sxs-lookup"><span data-stu-id="8fc8c-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="8fc8c-244">Můžete buď použít DNS server, nebo upravit/etc/hosts na všech uzlech.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-244">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="8fc8c-245">Tento příklad ukazuje, jak chcete použít soubor/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-245">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="8fc8c-246">Nahraďte adresu IP a název hostitele v následujících příkazech</span><span class="sxs-lookup"><span data-stu-id="8fc8c-246">Replace the IP address and the hostname in the following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="8fc8c-247">Vložte následující řádky, které se/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-247">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="8fc8c-248">Změnit IP adresu a název hostitele tak, aby odpovídaly prostředí</span><span class="sxs-lookup"><span data-stu-id="8fc8c-248">Change the IP address and hostname to match your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="8fc8c-249">[1] instalaci clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="8fc8c-250">[2] Přidání uzlu do clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-250">[2] Add node to cluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="8fc8c-251">[A] změnit heslo hacluster na stejné heslo</span><span class="sxs-lookup"><span data-stu-id="8fc8c-251">[A] Change hacluster password to the same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="8fc8c-252">[A] nakonfigurujte corosync používají jiné přenos a přidání seznamu.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-252">[A] Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="8fc8c-253">V opačném případě nebude fungovat clusteru.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="8fc8c-254">Do souboru přidejte následující obsah tučně.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-254">Add the following bold content to the file.</span></span>
    
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

    <span data-ttu-id="8fc8c-255">Potom restartujte službu corosync</span><span class="sxs-lookup"><span data-stu-id="8fc8c-255">Then restart the corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="8fc8c-256">[A] instalovat balíčky HANA HA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="8fc8c-257">Instalace SAP HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-257">Installing SAP HANA</span></span>

<span data-ttu-id="8fc8c-258">Postupujte podle kapitoly 4 z [SAP HANA SR výkonu optimalizované scénář průvodce] [ suse-hana-ha-guide] nainstalovat replikaci systému SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-258">Follow chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span>

1. <span data-ttu-id="8fc8c-259">[A] spusťte hdblcm z disku DVD HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-259">[A] Run hdblcm from the HANA DVD</span></span>
    * <span data-ttu-id="8fc8c-260">Zvolte instalace-1 ></span><span class="sxs-lookup"><span data-stu-id="8fc8c-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="8fc8c-261">Vyberte další součásti k instalaci -> 1</span><span class="sxs-lookup"><span data-stu-id="8fc8c-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="8fc8c-262">Zadejte instalační cestu [/ hana/sdílené]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-263">Zadejte název místního hostitele [.]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-264">Opravdu chcete přidat další hostitele do systému?</span><span class="sxs-lookup"><span data-stu-id="8fc8c-264">Do you want to add additional hosts to the system?</span></span> <span data-ttu-id="8fc8c-265">(Ano/Ne) [n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-266">Zadejte ID systému SAP HANA:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="8fc8c-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="8fc8c-267">Zadejte čísla Instance [00]:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="8fc8c-268">Čísla HANA Instance.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-268">HANA Instance number.</span></span> <span data-ttu-id="8fc8c-269">Použít 03, když se používá šablony Azure nebo udělali v předchozím příkladu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-269">Use 03 if you used the Azure Template or followed the example above</span></span>
    * <span data-ttu-id="8fc8c-270">Vyberte režim databáze / zadejte Index [1]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-271">Vyberte použití systému / zadejte Index [4]:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="8fc8c-272">Vyberte systém využití</span><span class="sxs-lookup"><span data-stu-id="8fc8c-272">Select the system Usage</span></span>
    * <span data-ttu-id="8fc8c-273">Zadejte umístění datových svazků [/ hana/data/HDB]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-274">Zadejte umístění protokolu svazků [/ hana/log/HDB]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-275">Omezení přidělení paměti maximální?</span><span class="sxs-lookup"><span data-stu-id="8fc8c-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="8fc8c-276">[n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-277">Zadejte název hostitele certifikát pro hostitele,..."</span><span class="sxs-lookup"><span data-stu-id="8fc8c-277">Enter Certificate Host Name For Host '...'</span></span> <span data-ttu-id="8fc8c-278">[...]: -> ZADEJTE</span><span class="sxs-lookup"><span data-stu-id="8fc8c-278">[...]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-279">Zadejte SAP hostitele agenta uživatele (sapadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-279">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="8fc8c-280">Potvrďte SAP hostitele agenta uživatele (sapadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-280">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="8fc8c-281">Zadejte správce systému (hdbadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-281">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="8fc8c-282">Potvrzení správce systému (hdbadm) heslo:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-282">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="8fc8c-283">Zadejte domovský adresář správce systému [/ usr/sap nebo HDB/domovské]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-283">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-284">Zadejte prostředí přihlášení správce systému [/ bin/dílet]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-284">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-285">Zadejte ID uživatele správce systému [1001]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-285">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-286">Zadejte ID ze skupiny uživatelů (sapsys) [79]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-286">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-287">Zadejte heslo k databázi uživatelů (systém):</span><span class="sxs-lookup"><span data-stu-id="8fc8c-287">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="8fc8c-288">Potvrďte heslo k databázi uživatelů (systém):</span><span class="sxs-lookup"><span data-stu-id="8fc8c-288">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="8fc8c-289">Restartování systému po restartování počítače?</span><span class="sxs-lookup"><span data-stu-id="8fc8c-289">Restart system after machine reboot?</span></span> <span data-ttu-id="8fc8c-290">[n]: -> zadejte</span><span class="sxs-lookup"><span data-stu-id="8fc8c-290">[n]: -> ENTER</span></span>
    * <span data-ttu-id="8fc8c-291">Chcete pokračovat?</span><span class="sxs-lookup"><span data-stu-id="8fc8c-291">Do you want to continue?</span></span> <span data-ttu-id="8fc8c-292">(Ano/Ne):</span><span class="sxs-lookup"><span data-stu-id="8fc8c-292">(y/n):</span></span>  
  <span data-ttu-id="8fc8c-293">Ověřit, souhrn a zadejte y můžete pokračovat</span><span class="sxs-lookup"><span data-stu-id="8fc8c-293">Validate the summary and enter y to continue</span></span>
1. <span data-ttu-id="8fc8c-294">[A] Agent hostitele upgradu SAP</span><span class="sxs-lookup"><span data-stu-id="8fc8c-294">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="8fc8c-295">Stáhněte si nejnovější archivu SAP Agent hostitele z [SAP Softwarecenter] [ sap-swcenter] a spusťte následující příkaz k aktualizaci agenta.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-295">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="8fc8c-296">Nahraďte cestu do archivu tak, aby odkazoval na soubor, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-296">Replace the path to the archive to point to the file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. <span data-ttu-id="8fc8c-297">[1] vytvoření HANA replikace (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-297">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="8fc8c-298">Spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-298">Run the following command.</span></span> <span data-ttu-id="8fc8c-299">Nezapomeňte nahradit tučné řetězce (HANA systému ID HDB a číslo instance 03) s hodnotami instalace SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-299">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="8fc8c-300">[A] vytvoření položky úložiště klíčů (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-300">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="8fc8c-301">[1] zálohy databáze (jako uživatel root)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-301">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="8fc8c-302">[1] přepnout na sapsid uživatele (například hdbadm) a vytvořte primární lokality.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-302">[1] Switch to the sapsid user (for example hdbadm) and create the primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="8fc8c-303">[2] přepnout na sapsid uživatele (například hdbadm) a vytvořit sekundární lokalitu.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-303">[2] Switch to the sapsid user (for example hdbadm) and create the secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="8fc8c-304">Konfigurace architektury clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-304">Configure Cluster Framework</span></span>

<span data-ttu-id="8fc8c-305">Změňte výchozí nastavení</span><span class="sxs-lookup"><span data-stu-id="8fc8c-305">Change the default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-defaults.txt
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="8fc8c-306">Nyní se nám načíst soubor do clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-306">now we load the file to the cluster</span></span>
<span data-ttu-id="8fc8c-307">sudo crm nakonfigurovat aktualizace zatížení crm-defaults.txt</span><span class="sxs-lookup"><span data-stu-id="8fc8c-307">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="8fc8c-308">Vytvoření STONITH zařízení</span><span class="sxs-lookup"><span data-stu-id="8fc8c-308">Create STONITH device</span></span>

<span data-ttu-id="8fc8c-309">STONITH zařízení používá objekt služby k autorizaci s Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-309">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="8fc8c-310">Postupujte podle těchto kroků můžete vytvořit objekt služby.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-310">Please follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="8fc8c-311">Přejděte na <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="8fc8c-311">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="8fc8c-312">Otevřete okno Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8fc8c-312">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="8fc8c-313">Přejděte k vlastnostem a poznamenejte si ID adresáře.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-313">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="8fc8c-314">Toto je **id klienta**.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-314">This is the **tenant id**.</span></span>
1. <span data-ttu-id="8fc8c-315">Klikněte na možnost registrace aplikace</span><span class="sxs-lookup"><span data-stu-id="8fc8c-315">Click App registrations</span></span>
1. <span data-ttu-id="8fc8c-316">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-316">Click Add</span></span>
1. <span data-ttu-id="8fc8c-317">Zadejte název, vyberte typ aplikace "Aplikace webového rozhraní API", zadejte přihlašovací adresu URL (například http://localhost) a klikněte na možnost vytvořit</span><span class="sxs-lookup"><span data-stu-id="8fc8c-317">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="8fc8c-318">Adresa URL přihlašování se nepoužívá a může být libovolná platná adresa URL</span><span class="sxs-lookup"><span data-stu-id="8fc8c-318">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="8fc8c-319">Vyberte nové aplikace a na kartě nastavení klikněte na klíče</span><span class="sxs-lookup"><span data-stu-id="8fc8c-319">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="8fc8c-320">Zadejte popis pro nový klíč, vyberte "Je platné stále" a klikněte na Uložit</span><span class="sxs-lookup"><span data-stu-id="8fc8c-320">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="8fc8c-321">Poznamenejte si hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-321">Write down the Value.</span></span> <span data-ttu-id="8fc8c-322">Použije se jako **heslo** pro objekt služby</span><span class="sxs-lookup"><span data-stu-id="8fc8c-322">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="8fc8c-323">Poznamenejte si ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-323">Write down the Application Id.</span></span> <span data-ttu-id="8fc8c-324">Se používá jako uživatelské jméno (**přihlašovacího id** v následujících krocích) instančního objektu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-324">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="8fc8c-325">Objekt služby nemá oprávnění pro přístup k prostředkům Azure ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-325">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="8fc8c-326">Musíte poskytnout oprávnění objektu služby spuštění a zastavení (zrušit přidělení) všechny virtuální počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-326">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="8fc8c-327">Přejděte na https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="8fc8c-327">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="8fc8c-328">Otevře se okno všechny prostředky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-328">Open the All resources blade</span></span>
1. <span data-ttu-id="8fc8c-329">Vyberte virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="8fc8c-329">Select the virtual machine</span></span>
1. <span data-ttu-id="8fc8c-330">Klikněte na řízení přístupu (IAM)</span><span class="sxs-lookup"><span data-stu-id="8fc8c-330">Click Access control (IAM)</span></span>
1. <span data-ttu-id="8fc8c-331">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-331">Click Add</span></span>
1. <span data-ttu-id="8fc8c-332">Vyberte roli vlastníka</span><span class="sxs-lookup"><span data-stu-id="8fc8c-332">Select the role Owner</span></span>
1. <span data-ttu-id="8fc8c-333">Zadejte název aplikace, kterou jste vytvořili výše</span><span class="sxs-lookup"><span data-stu-id="8fc8c-333">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="8fc8c-334">Klikněte na tlačítko OK</span><span class="sxs-lookup"><span data-stu-id="8fc8c-334">Click OK</span></span>

<span data-ttu-id="8fc8c-335">Poté, co jste upravili oprávnění pro virtuální počítače, můžete nakonfigurovat zařízení STONITH v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-335">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="8fc8c-336">Nyní se nám načíst soubor do clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-336">now we load the file to the cluster</span></span>
<span data-ttu-id="8fc8c-337">sudo crm nakonfigurovat aktualizace zatížení crm-fencing.txt</span><span class="sxs-lookup"><span data-stu-id="8fc8c-337">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="8fc8c-338">Vytvořit prostředky SAP HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-338">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system id
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="8fc8c-339">Nyní se nám načíst soubor do clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-339">now we load the file to the cluster</span></span>
<span data-ttu-id="8fc8c-340">sudo crm nakonfigurovat aktualizace zatížení crm-saphanatop.txt</span><span class="sxs-lookup"><span data-stu-id="8fc8c-340">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="8fc8c-341">Nyní se nám načíst soubor do clusteru</span><span class="sxs-lookup"><span data-stu-id="8fc8c-341">now we load the file to the cluster</span></span>
<span data-ttu-id="8fc8c-342">sudo crm nakonfigurovat aktualizace zatížení crm-saphana.txt</span><span class="sxs-lookup"><span data-stu-id="8fc8c-342">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="8fc8c-343">Nastavení clusteru s podporou testu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-343">Test cluster setup</span></span>
<span data-ttu-id="8fc8c-344">V následující kapitole popisují, jak můžete otestovat vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-344">The following chapter describe how you can test your setup.</span></span> <span data-ttu-id="8fc8c-345">Každý test předpokládá, že jsou kořenové a hlavním serveru SAP HANA běží na saphanavm1 virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-345">Every test assumes that you are root and the SAP HANA master is running on the virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="8fc8c-346">Vymezení testu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-346">Fencing Test</span></span>

<span data-ttu-id="8fc8c-347">Instalační program agenta vymezení můžete otestovat zakázáním síťové rozhraní na uzlu saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-347">You can test the setup of the fencing agent by disabling the network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="8fc8c-348">Virtuální počítač by měl nyní restartovat, nebo byla zastavena v závislosti na konfiguraci clusteru.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-348">The virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="8fc8c-349">Pokud jste nastavili stonith akce, která bude vypnuto, bude nutné zastavit virtuální počítač a prostředky se migrují do spuštěného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-349">If you set the stonith-action to off, the virtual machine will be stopped and the resources are migrated to the running virtual machine.</span></span>

<span data-ttu-id="8fc8c-350">Po spuštění virtuálního počítače, SAP HANA prostředků se nepodaří spustit jako sekundární Pokud nastavíte AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="8fc8c-350">Once you start the virtual machine again, the SAP HANA resource will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="8fc8c-351">V takovém případě musíte nakonfigurovat instanci HANA jako sekundární spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-351">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="8fc8c-352">Testování ruční převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="8fc8c-352">Testing a manual failover</span></span>

<span data-ttu-id="8fc8c-353">Zastavování služby kardiostimulátor na uzlu saphanavm1, můžete otestovat ruční převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-353">You can test a manual failover by stopping the pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="8fc8c-354">Po převzetí služeb při selhání můžete spustit službu znovu.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-354">After the failover, you can start the service again.</span></span> <span data-ttu-id="8fc8c-355">SAP HANA prostředku saphanavm1 nebude možné spustit jako sekundární Pokud nastavíte AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="8fc8c-355">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="8fc8c-356">V takovém případě musíte nakonfigurovat instanci HANA jako sekundární spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-356">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="8fc8c-357">Testování migrace</span><span class="sxs-lookup"><span data-stu-id="8fc8c-357">Testing a migration</span></span>

<span data-ttu-id="8fc8c-358">Spuštěním následujícího příkazu můžete migrovat hlavní uzel SAP HANA</span><span class="sxs-lookup"><span data-stu-id="8fc8c-358">You can migrate the SAP HANA master node by executing the following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="8fc8c-359">To je potřeba migrovat SAP HANA hlavní uzel a skupiny, která obsahuje virtuální IP adresu, kterou saphanavm2.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-359">This should migrate the SAP HANA master node and the group that contains the virtual IP address to saphanavm2.</span></span>
<span data-ttu-id="8fc8c-360">SAP HANA prostředku saphanavm1 nebude možné spustit jako sekundární Pokud nastavíte AUTOMATED_REGISTER = "false".</span><span class="sxs-lookup"><span data-stu-id="8fc8c-360">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="8fc8c-361">V takovém případě musíte nakonfigurovat instanci HANA jako sekundární spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="8fc8c-361">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="8fc8c-362">Migrace vytvoří omezení umístění, které je nutné znovu odstranit.</span><span class="sxs-lookup"><span data-stu-id="8fc8c-362">The migration creates location contraints that need to be deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like the following contraint. You should have two contraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="8fc8c-363">Musíte také čištění stav prostředku sekundárního uzlu</span><span class="sxs-lookup"><span data-stu-id="8fc8c-363">You also need to cleanup the state of the secondary node resource</span></span>

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="8fc8c-364">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8fc8c-364">Next steps</span></span>
* <span data-ttu-id="8fc8c-365">[Azure virtuálních počítačů, plánování a implementace pro SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-365">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="8fc8c-366">[Nasazení virtuálních počítačů Azure pro SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-366">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="8fc8c-367">[Nasazení virtuálních počítačů databázového systému Azure pro SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="8fc8c-367">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="8fc8c-368">Další informace o vytvoření vysoké dostupnosti a plán pro zotavení po havárii SAP HANA v Azure (velké instance) naleznete v tématu [SAP HANA (velké instance) vysoké dostupnosti a zotavení po havárii v Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="8fc8c-368">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
