---
title: "Kontejner monitorování řešení v Azure Log Analytics | Microsoft Docs"
description: "Řešení monitorování kontejneru v analýzy protokolů umožňuje zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: b2e03531ee401f4552198e5dd50fbfe1d970f0e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a><span data-ttu-id="c43f3-103">Řešení monitorování kontejneru v analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c43f3-103">Container Monitoring solution in Log Analytics</span></span>

![Kontejnery – symbol](./media/log-analytics-containers/containers-symbol.png)

<span data-ttu-id="c43f3-105">Tento článek popisuje, jak nastavit a použít řešení monitorování kontejneru v analýzy protokolů, která slouží k zobrazení a správa Docker a Windows hostitele kontejneru na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="c43f3-105">This article describes how to set up and use the Container Monitoring solution in Log Analytics, which helps you view and manage your Docker and Windows container hosts in a single location.</span></span> <span data-ttu-id="c43f3-106">Docker je systém virtualizační software používá k vytvoření kontejnerů, které automatizují nasazení softwaru na počítačovou infrastrukturu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-106">Docker is a software virtualization system used to create containers that automate software deployment to their IT infrastructure.</span></span>

<span data-ttu-id="c43f3-107">Řešení ukazuje jsou kontejnery, které běží, jaké kontejneru image se používáte, a kde kontejnery běží.</span><span class="sxs-lookup"><span data-stu-id="c43f3-107">The solution shows which containers are running, what container image they’re running, and where containers are running.</span></span> <span data-ttu-id="c43f3-108">Můžete zobrazit podrobné informace o auditování zobrazující příkazy používané s kontejnery.</span><span class="sxs-lookup"><span data-stu-id="c43f3-108">You can view detailed audit information showing commands used with containers.</span></span> <span data-ttu-id="c43f3-109">A kontejnery můžete vyřešit tak, že zobrazení a hledání centralizované protokoly bez nutnosti Chcete-li zobrazit hostitelů Docker nebo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c43f3-109">And, you can troubleshoot containers by viewing and searching centralized logs without having to remotely view Docker or Windows hosts.</span></span> <span data-ttu-id="c43f3-110">Můžete najít kontejnery, které může být aktivní nebo využívání nadbytečné prostředky na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="c43f3-110">You can find containers that may be noisy and consuming excess resources on a host.</span></span> <span data-ttu-id="c43f3-111">A lze je zobrazit centralizované procesoru, paměti, úložiště a využití a výkonu informace o síti pro kontejnery.</span><span class="sxs-lookup"><span data-stu-id="c43f3-111">And, you can view centralized CPU, memory, storage, and network usage and performance information for containers.</span></span> <span data-ttu-id="c43f3-112">V počítačích se systémem Windows, můžete centralizovat a porovnání protokoly ze systému Windows Server, technologie Hyper-V a Docker kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-112">On computers running Windows, you can centralize and compare logs from Windows Server, Hyper-V, and Docker containers.</span></span> <span data-ttu-id="c43f3-113">Řešení podporuje následující orchestrators kontejneru:</span><span class="sxs-lookup"><span data-stu-id="c43f3-113">The solution supports the following container orchestrators:</span></span>

- <span data-ttu-id="c43f3-114">Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="c43f3-114">Docker Swarm</span></span>
- <span data-ttu-id="c43f3-115">DC/OS</span><span class="sxs-lookup"><span data-stu-id="c43f3-115">DC/OS</span></span>
- <span data-ttu-id="c43f3-116">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="c43f3-116">Kubernetes</span></span>
- <span data-ttu-id="c43f3-117">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c43f3-117">Service Fabric</span></span>
- <span data-ttu-id="c43f3-118">Red Hat OpenShift</span><span class="sxs-lookup"><span data-stu-id="c43f3-118">Red Hat OpenShift</span></span>


<span data-ttu-id="c43f3-119">Následující diagram znázorňuje vztahy mezi různými hostiteli kontejneru a agentů v OMS.</span><span class="sxs-lookup"><span data-stu-id="c43f3-119">The following diagram shows the relationships between various container hosts and agents with OMS.</span></span>

![Diagram kontejnery](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a><span data-ttu-id="c43f3-121">Požadavky na systém</span><span class="sxs-lookup"><span data-stu-id="c43f3-121">System requirements</span></span>

<span data-ttu-id="c43f3-122">Než začnete, zkontrolujte následující podrobnosti k ověření, že splňujete požadavky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-122">Before starting, review the following details to verify you meet the prerequisites.</span></span>

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a><span data-ttu-id="c43f3-123">Podpora řešení monitorování kontejneru pro platformy Docker Orchestrator a verze operačního systému</span><span class="sxs-lookup"><span data-stu-id="c43f3-123">Container monitoring solution support for Docker Orchestrator and OS platform</span></span>
<span data-ttu-id="c43f3-124">Následující tabulka popisuje Docker orchestration a monitorování podporu kontejneru inventáře, výkonu a protokoly s analýzy protokolů operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c43f3-124">The following table outlines the Docker orchestration and operating system monitoring support of container inventory, performance, and logs with Log Analytics.</span></span>   

| | <span data-ttu-id="c43f3-125">ACS</span><span class="sxs-lookup"><span data-stu-id="c43f3-125">ACS</span></span> | <span data-ttu-id="c43f3-126">Linux</span><span class="sxs-lookup"><span data-stu-id="c43f3-126">Linux</span></span> | <span data-ttu-id="c43f3-127">Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-127">Windows</span></span> | <span data-ttu-id="c43f3-128">Kontejner</span><span class="sxs-lookup"><span data-stu-id="c43f3-128">Container</span></span><br><span data-ttu-id="c43f3-129">Inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-129">Inventory</span></span> | <span data-ttu-id="c43f3-130">Image</span><span class="sxs-lookup"><span data-stu-id="c43f3-130">Image</span></span><br><span data-ttu-id="c43f3-131">Inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-131">Inventory</span></span> | <span data-ttu-id="c43f3-132">Node</span><span class="sxs-lookup"><span data-stu-id="c43f3-132">Node</span></span><br><span data-ttu-id="c43f3-133">Inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-133">Inventory</span></span> | <span data-ttu-id="c43f3-134">Kontejner</span><span class="sxs-lookup"><span data-stu-id="c43f3-134">Container</span></span><br><span data-ttu-id="c43f3-135">Výkon</span><span class="sxs-lookup"><span data-stu-id="c43f3-135">Performance</span></span> | <span data-ttu-id="c43f3-136">Kontejner</span><span class="sxs-lookup"><span data-stu-id="c43f3-136">Container</span></span><br><span data-ttu-id="c43f3-137">Událost</span><span class="sxs-lookup"><span data-stu-id="c43f3-137">Event</span></span> | <span data-ttu-id="c43f3-138">Událost</span><span class="sxs-lookup"><span data-stu-id="c43f3-138">Event</span></span><br><span data-ttu-id="c43f3-139">Protokol</span><span class="sxs-lookup"><span data-stu-id="c43f3-139">Log</span></span> | <span data-ttu-id="c43f3-140">Kontejner</span><span class="sxs-lookup"><span data-stu-id="c43f3-140">Container</span></span><br><span data-ttu-id="c43f3-141">Protokol</span><span class="sxs-lookup"><span data-stu-id="c43f3-141">Log</span></span> |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| <span data-ttu-id="c43f3-142">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="c43f3-142">Kubernetes</span></span> | <span data-ttu-id="c43f3-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-143">&#8226;</span></span> | <span data-ttu-id="c43f3-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-144">&#8226;</span></span> | | <span data-ttu-id="c43f3-145">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-145">&#8226;</span></span> | <span data-ttu-id="c43f3-146">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-146">&#8226;</span></span> | <span data-ttu-id="c43f3-147">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-147">&#8226;</span></span> | <span data-ttu-id="c43f3-148">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-148">&#8226;</span></span> | <span data-ttu-id="c43f3-149">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-149">&#8226;</span></span> | <span data-ttu-id="c43f3-150">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-150">&#8226;</span></span> | <span data-ttu-id="c43f3-151">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-151">&#8226;</span></span> |
| <span data-ttu-id="c43f3-152">Mesosphere</span><span class="sxs-lookup"><span data-stu-id="c43f3-152">Mesosphere</span></span><br><span data-ttu-id="c43f3-153">DC/OS</span><span class="sxs-lookup"><span data-stu-id="c43f3-153">DC/OS</span></span> | <span data-ttu-id="c43f3-154">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-154">&#8226;</span></span> | <span data-ttu-id="c43f3-155">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-155">&#8226;</span></span> | | <span data-ttu-id="c43f3-156">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-156">&#8226;</span></span> | <span data-ttu-id="c43f3-157">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-157">&#8226;</span></span> | <span data-ttu-id="c43f3-158">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-158">&#8226;</span></span> | <span data-ttu-id="c43f3-159">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-159">&#8226;</span></span>| <span data-ttu-id="c43f3-160">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-160">&#8226;</span></span> | <span data-ttu-id="c43f3-161">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-161">&#8226;</span></span> | <span data-ttu-id="c43f3-162">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-162">&#8226;</span></span> |
| <span data-ttu-id="c43f3-163">Docker</span><span class="sxs-lookup"><span data-stu-id="c43f3-163">Docker</span></span><br><span data-ttu-id="c43f3-164">Swarm</span><span class="sxs-lookup"><span data-stu-id="c43f3-164">Swarm</span></span> | <span data-ttu-id="c43f3-165">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-165">&#8226;</span></span> | <span data-ttu-id="c43f3-166">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-166">&#8226;</span></span> | <span data-ttu-id="c43f3-167">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-167">&#8226;</span></span> | <span data-ttu-id="c43f3-168">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-168">&#8226;</span></span> | <span data-ttu-id="c43f3-169">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-169">&#8226;</span></span> | <span data-ttu-id="c43f3-170">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-170">&#8226;</span></span> | <span data-ttu-id="c43f3-171">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-171">&#8226;</span></span> | <span data-ttu-id="c43f3-172">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-172">&#8226;</span></span> | | <span data-ttu-id="c43f3-173">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-173">&#8226;</span></span> |
| <span data-ttu-id="c43f3-174">Služba</span><span class="sxs-lookup"><span data-stu-id="c43f3-174">Service</span></span><br><span data-ttu-id="c43f3-175">Prostředky infrastruktury</span><span class="sxs-lookup"><span data-stu-id="c43f3-175">Fabric</span></span> | | | <span data-ttu-id="c43f3-176">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-176">&#8226;</span></span> | <span data-ttu-id="c43f3-177">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-177">&#8226;</span></span> | <span data-ttu-id="c43f3-178">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-178">&#8226;</span></span> | <span data-ttu-id="c43f3-179">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-179">&#8226;</span></span> | <span data-ttu-id="c43f3-180">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-180">&#8226;</span></span> | <span data-ttu-id="c43f3-181">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-181">&#8226;</span></span> | <span data-ttu-id="c43f3-182">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-182">&#8226;</span></span> | <span data-ttu-id="c43f3-183">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-183">&#8226;</span></span> |
| <span data-ttu-id="c43f3-184">Otevřete Red Hat</span><span class="sxs-lookup"><span data-stu-id="c43f3-184">Red Hat Open</span></span><br><span data-ttu-id="c43f3-185">Posunutí</span><span class="sxs-lookup"><span data-stu-id="c43f3-185">Shift</span></span> | | <span data-ttu-id="c43f3-186">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-186">&#8226;</span></span> | | <span data-ttu-id="c43f3-187">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-187">&#8226;</span></span> | <span data-ttu-id="c43f3-188">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-188">&#8226;</span></span>| <span data-ttu-id="c43f3-189">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-189">&#8226;</span></span> | <span data-ttu-id="c43f3-190">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-190">&#8226;</span></span> | <span data-ttu-id="c43f3-191">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-191">&#8226;</span></span> | | <span data-ttu-id="c43f3-192">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-192">&#8226;</span></span> |
| <span data-ttu-id="c43f3-193">Windows Server</span><span class="sxs-lookup"><span data-stu-id="c43f3-193">Windows Server</span></span><br><span data-ttu-id="c43f3-194">(samostatně)</span><span class="sxs-lookup"><span data-stu-id="c43f3-194">(standalone)</span></span> | | | <span data-ttu-id="c43f3-195">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-195">&#8226;</span></span> | <span data-ttu-id="c43f3-196">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-196">&#8226;</span></span> | <span data-ttu-id="c43f3-197">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-197">&#8226;</span></span> | <span data-ttu-id="c43f3-198">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-198">&#8226;</span></span> | <span data-ttu-id="c43f3-199">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-199">&#8226;</span></span> | <span data-ttu-id="c43f3-200">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-200">&#8226;</span></span> | | <span data-ttu-id="c43f3-201">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-201">&#8226;</span></span> |
| <span data-ttu-id="c43f3-202">Linux Server</span><span class="sxs-lookup"><span data-stu-id="c43f3-202">Linux Server</span></span><br><span data-ttu-id="c43f3-203">(samostatně)</span><span class="sxs-lookup"><span data-stu-id="c43f3-203">(standalone)</span></span> | | <span data-ttu-id="c43f3-204">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-204">&#8226;</span></span> | | <span data-ttu-id="c43f3-205">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-205">&#8226;</span></span> | <span data-ttu-id="c43f3-206">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-206">&#8226;</span></span> | <span data-ttu-id="c43f3-207">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-207">&#8226;</span></span> | <span data-ttu-id="c43f3-208">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-208">&#8226;</span></span> | <span data-ttu-id="c43f3-209">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-209">&#8226;</span></span> | | <span data-ttu-id="c43f3-210">&#8226;</span><span class="sxs-lookup"><span data-stu-id="c43f3-210">&#8226;</span></span> |


### <a name="docker-versions-supported-on-linux"></a><span data-ttu-id="c43f3-211">Verze docker podporované v systému Linux</span><span class="sxs-lookup"><span data-stu-id="c43f3-211">Docker versions supported on Linux</span></span>

- <span data-ttu-id="c43f3-212">Docker 1.11 k 1.13</span><span class="sxs-lookup"><span data-stu-id="c43f3-212">Docker 1.11 to 1.13</span></span>
- <span data-ttu-id="c43f3-213">V17.06 docker CE a EE</span><span class="sxs-lookup"><span data-stu-id="c43f3-213">Docker CE and EE v17.06</span></span>

### <a name="x64-linux-distributions-supported-as-container-hosts"></a><span data-ttu-id="c43f3-214">x64 Linuxových distribucích podporovány jako hostitele kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-214">x64 Linux distributions supported as container hosts</span></span>

- <span data-ttu-id="c43f3-215">Ubuntu 14.04 LTS a 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="c43f3-215">Ubuntu 14.04 LTS and 16.04 LTS</span></span>
- <span data-ttu-id="c43f3-216">CoreOS(stable)</span><span class="sxs-lookup"><span data-stu-id="c43f3-216">CoreOS(stable)</span></span>
- <span data-ttu-id="c43f3-217">Amazon Linux 2016.09.0</span><span class="sxs-lookup"><span data-stu-id="c43f3-217">Amazon Linux 2016.09.0</span></span>
- <span data-ttu-id="c43f3-218">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="c43f3-218">openSUSE 13.2</span></span>
- <span data-ttu-id="c43f3-219">openSUSE LEAP 42.2</span><span class="sxs-lookup"><span data-stu-id="c43f3-219">openSUSE LEAP 42.2</span></span>
- <span data-ttu-id="c43f3-220">CentOS 7.2 a 7.3</span><span class="sxs-lookup"><span data-stu-id="c43f3-220">CentOS 7.2 and 7.3</span></span>
- <span data-ttu-id="c43f3-221">SLES 12</span><span class="sxs-lookup"><span data-stu-id="c43f3-221">SLES 12</span></span>
- <span data-ttu-id="c43f3-222">RHEL 7.2 a 7.3</span><span class="sxs-lookup"><span data-stu-id="c43f3-222">RHEL 7.2 and 7.3</span></span>
- <span data-ttu-id="c43f3-223">Red Hat OpenShift kontejneru platformy (OCP) 3.4 a 3.5</span><span class="sxs-lookup"><span data-stu-id="c43f3-223">Red Hat OpenShift Container Platform (OCP) 3.4 and 3.5</span></span>
- <span data-ttu-id="c43f3-224">Služba ACS Mesosphere DC/OS 1.7.3 až 1.8.8</span><span class="sxs-lookup"><span data-stu-id="c43f3-224">ACS Mesosphere DC/OS 1.7.3 to 1.8.8</span></span>
- <span data-ttu-id="c43f3-225">Služba ACS Kubernetes 1.4.5 na 1.6</span><span class="sxs-lookup"><span data-stu-id="c43f3-225">ACS Kubernetes 1.4.5 to 1.6</span></span>
- <span data-ttu-id="c43f3-226">Služba ACS Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="c43f3-226">ACS Docker Swarm</span></span>

### <a name="supported-windows-operating-system"></a><span data-ttu-id="c43f3-227">Podporovaný operační systém Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-227">Supported Windows operating system</span></span>

- <span data-ttu-id="c43f3-228">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="c43f3-228">Windows Server 2016</span></span>
- <span data-ttu-id="c43f3-229">Windows 10 Anniversary Edition (Professional nebo Enterprise)</span><span class="sxs-lookup"><span data-stu-id="c43f3-229">Windows 10 Anniversary Edition (Professional or Enterprise)</span></span>

### <a name="docker-versions-supported-on-windows"></a><span data-ttu-id="c43f3-230">Verze docker podporované v systému Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-230">Docker versions supported on Windows</span></span>

- <span data-ttu-id="c43f3-231">Docker 1.12 a 1.13</span><span class="sxs-lookup"><span data-stu-id="c43f3-231">Docker 1.12 and 1.13</span></span>
- <span data-ttu-id="c43f3-232">Docker 17.03.0 a novější</span><span class="sxs-lookup"><span data-stu-id="c43f3-232">Docker 17.03.0 and later</span></span>

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="c43f3-233">Instalace a konfigurace řešení</span><span class="sxs-lookup"><span data-stu-id="c43f3-233">Installing and configuring the solution</span></span>
<span data-ttu-id="c43f3-234">Použijte následující informace k instalaci a konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="c43f3-234">Use the following information to install and configure the solution.</span></span>

1. <span data-ttu-id="c43f3-235">Přidat kontejner monitorování řešení do pracovního prostoru OMS z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) nebo pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-235">Add the Container Monitoring solution to your OMS workspace from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) or by using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

2. <span data-ttu-id="c43f3-236">Nainstalovat a používat Docker s agentem OMS.</span><span class="sxs-lookup"><span data-stu-id="c43f3-236">Install and use Docker with an OMS agent.</span></span>  <span data-ttu-id="c43f3-237">V závislosti na použitém operačním systému, můžete vybrat z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="c43f3-237">Based on your operating system, you can choose from the following methods:</span></span>

  * <span data-ttu-id="c43f3-238">Na podporovaných operačních systémů Linux, nainstalujte a spusťte Docker a poté nainstalujte a nakonfigurujte [OMS agenta pro Linux](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-238">On supported Linux operating systems, install and run Docker and then install and configure the [OMS Agent for Linux](log-analytics-agent-linux.md).</span></span>  
  * <span data-ttu-id="c43f3-239">Na jádro operačního systému nelze spustit agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="c43f3-239">On CoreOS, you cannot run the OMS Agent for Linux.</span></span> <span data-ttu-id="c43f3-240">Místo toho spustíte kontejnerizované verzi agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="c43f3-240">Instead, you run a containerized version of the OMS Agent for Linux.</span></span> <span data-ttu-id="c43f3-241">Zkontrolujte [Linux kontejneru hostitele, včetně CoreOS](#for-all-linux-container-hosts-including-coreos) nebo [hostitele kontejner Azure Government Linux včetně CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) při práci s kontejnery v cloudu Azure Government.</span><span class="sxs-lookup"><span data-stu-id="c43f3-241">Review [Linux container hosts including CoreOS](#for-all-linux-container-hosts-including-coreos) or [Azure Government Linux container hosts including CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) if you are working with containers in Azure Government Cloud.</span></span>
  * <span data-ttu-id="c43f3-242">V systému Windows Server 2016 a Windows 10 nainstalujte modul Docker a klienta pak připojit agenta pro shromažďování informací a odeslat ho k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-242">On Windows Server 2016 and Windows 10, install the Docker Engine and client then connect an agent to gather information and send it to Log Analytics.</span></span>  

### <a name="container-services"></a><span data-ttu-id="c43f3-243">Služby kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-243">Container services</span></span>

- <span data-ttu-id="c43f3-244">Pokud máte Red Hat OpenShift prostředí, přečtěte si [konfigurace agenta OMS pro Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span><span class="sxs-lookup"><span data-stu-id="c43f3-244">If you have a Red Hat OpenShift environment, review [Configure an OMS agent for Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).</span></span>
- <span data-ttu-id="c43f3-245">Pokud máte Kubernetes clusteru Azure Container Service pomocí, přečtěte si [monitorování clusteru Azure Container Service s Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-245">If you have a Kubernetes cluster using the Azure Container Service, review [Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).</span></span>
- <span data-ttu-id="c43f3-246">Pokud máte cluster Azure Container Service DC/OS, přečtěte si informace v [monitorování clusteru Azure Container Service DC/OS s Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-246">If you have an Azure Container Service DC/OS cluster, learn more at [Monitor an Azure Container Service DC/OS cluster with Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).</span></span>
- <span data-ttu-id="c43f3-247">Pokud máte prostředí režimu Docker Swarm, další informace v [konfigurace agenta OMS pro Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span><span class="sxs-lookup"><span data-stu-id="c43f3-247">If you have a Docker Swarm mode environment, learn more at [Configure an OMS agent for Docker Swarm](#configure-an-oms-agent-for-docker-swarm).</span></span>
- <span data-ttu-id="c43f3-248">Pokud používáte kontejnery s Service Fabric, další informace v [přehled Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-248">If you use containers with Service Fabric, learn more at [Overview of Azure Service Fabric](../service-fabric/service-fabric-overview.md).</span></span>
- <span data-ttu-id="c43f3-249">Zkontrolujte [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) článek Další informace o tom, jak nainstalovat a nakonfigurovat vaše Docker moduly v počítačích se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c43f3-249">Review the [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) article for additional information about how to install and configure your Docker Engines on computers running Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c43f3-250">Docker musí být spuštěna **před** nainstalujete [OMS agenta pro Linux](log-analytics-agent-linux.md) v hostitelích kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-250">Docker must be running **before** you install the [OMS Agent for Linux](log-analytics-agent-linux.md) on your container hosts.</span></span> <span data-ttu-id="c43f3-251">Pokud jste již nainstalovali agenta před instalací Docker, budete muset znovu nainstalujte agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="c43f3-251">If you've already installed the agent before installing Docker, you need to reinstall the OMS Agent for Linux.</span></span> <span data-ttu-id="c43f3-252">Další informace o Docker najdete v tématu [Docker webu](https://www.docker.com).</span><span class="sxs-lookup"><span data-stu-id="c43f3-252">For more information about Docker, see the [Docker website](https://www.docker.com).</span></span>


## <a name="linux-container-hosts"></a><span data-ttu-id="c43f3-253">Linux kontejneru hostitele</span><span class="sxs-lookup"><span data-stu-id="c43f3-253">Linux container hosts</span></span>

<span data-ttu-id="c43f3-254">Po instalaci Docker, použijte následující nastavení pro svého hostitele kontejneru konfigurace agenta pro použití s Docker.</span><span class="sxs-lookup"><span data-stu-id="c43f3-254">After you've installed Docker, use the following settings for your container host to configure the agent for use with Docker.</span></span> <span data-ttu-id="c43f3-255">Je třeba nejprve vaše OMS ID a klíč, který můžete najít na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c43f3-255">First you need your OMS workspace ID and key, which you can find in the Azure portal.</span></span> <span data-ttu-id="c43f3-256">V pracovním prostoru, klikněte na tlačítko **rychlý Start** > **počítače** zobrazíte vaše **ID pracovního prostoru** a **primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="c43f3-256">In your workspace, click **Quick Start** > **Computers** to view your **Workspace ID** and **Primary Key**.</span></span>  <span data-ttu-id="c43f3-257">Zkopírujte a vložte do vašeho oblíbeného editoru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-257">Copy and paste both into your favorite editor.</span></span>

### <a name="for-all-linux-container-hosts-except-coreos"></a><span data-ttu-id="c43f3-258">Pro všechny hostitele kontejneru Linux s výjimkou CoreOS</span><span class="sxs-lookup"><span data-stu-id="c43f3-258">For all Linux container hosts except CoreOS</span></span>

- <span data-ttu-id="c43f3-259">Další informace a kroky k instalaci agenta OMS pro Linux najdete v tématu [připojení počítačů Linux k Operations Management Suite (OMS)](log-analytics-agent-linux.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-259">For more information and steps on how to install the OMS Agent for Linux, see [Connect your Linux Computers to Operations Management Suite (OMS)](log-analytics-agent-linux.md).</span></span>

### <a name="for-all-linux-container-hosts-including-coreos"></a><span data-ttu-id="c43f3-260">Pro všechny hostitele Linux kontejneru, včetně CoreOS</span><span class="sxs-lookup"><span data-stu-id="c43f3-260">For all Linux container hosts including CoreOS</span></span>

<span data-ttu-id="c43f3-261">Spusťte kontejner OMS, který chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="c43f3-261">Start the OMS container that you want to monitor.</span></span> <span data-ttu-id="c43f3-262">Upravit a použít v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c43f3-262">Modify and use the following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a><span data-ttu-id="c43f3-263">Pro všechny hostitele kontejner Azure Government Linux včetně CoreOS</span><span class="sxs-lookup"><span data-stu-id="c43f3-263">For all Azure Government Linux container hosts including CoreOS</span></span>

<span data-ttu-id="c43f3-264">Spusťte kontejner OMS, který chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="c43f3-264">Start the OMS container that you want to monitor.</span></span> <span data-ttu-id="c43f3-265">Upravit a použít v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c43f3-265">Modify and use the following example:</span></span>

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a><span data-ttu-id="c43f3-266">Přepínání z pomocí nainstalovaného agenta systému Linux na jednu v kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-266">Switching from using an installed Linux agent to one in a container</span></span>
<span data-ttu-id="c43f3-267">Pokud dříve použít agenta přímo nainstalovat a chcete místo toho použít agenta spuštěného v kontejneru, je nutné nejprve odebrat agenta OMS pro Linux.</span><span class="sxs-lookup"><span data-stu-id="c43f3-267">If you previously used the directly-installed agent and want to instead use an agent running in a container, you must first remove the OMS Agent for Linux.</span></span> <span data-ttu-id="c43f3-268">V tématu [odinstalování agenta OMS pro Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) pochopit, jak úspěšně odinstalace agenta.</span><span class="sxs-lookup"><span data-stu-id="c43f3-268">See [Uninstalling the OMS Agent for Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) to understand how to successfully uninstall the agent.</span></span>  

### <a name="configure-an-oms-agent-for-docker-swarm"></a><span data-ttu-id="c43f3-269">Konfigurace agenta OMS pro Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="c43f3-269">Configure an OMS agent for Docker Swarm</span></span>

<span data-ttu-id="c43f3-270">Spuštěním agenta OMS jako globální služby v nástroji Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="c43f3-270">You can run the OMS Agent as a global service on Docker Swarm.</span></span> <span data-ttu-id="c43f3-271">Použijte následující informace pro vytvoření služby OMS Agent.</span><span class="sxs-lookup"><span data-stu-id="c43f3-271">Use the following information to create an OMS Agent service.</span></span> <span data-ttu-id="c43f3-272">Je třeba vložit ID pracovního prostoru OMS a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c43f3-272">You need to insert your OMS Workspace ID and Primary Key.</span></span>

- <span data-ttu-id="c43f3-273">Spusťte následující na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="c43f3-273">Run the following on the master node.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a><span data-ttu-id="c43f3-274">Konfigurace agenta OMS pro OpenShift Red Hat</span><span class="sxs-lookup"><span data-stu-id="c43f3-274">Configure an OMS Agent for Red Hat OpenShift</span></span>
<span data-ttu-id="c43f3-275">Existují tři způsoby, jak přidat agenta OMS na Red Hat OpenShift spustit shromažďování dat monitorování kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-275">There are three ways to add the OMS Agent to Red Hat OpenShift to start collecting container monitoring data.</span></span>

* <span data-ttu-id="c43f3-276">[Nainstalovat agenta OMS pro Linux](log-analytics-agent-linux.md) přímo na každém uzlu OpenShift</span><span class="sxs-lookup"><span data-stu-id="c43f3-276">[Install the OMS Agent for Linux](log-analytics-agent-linux.md) directly on each OpenShift node</span></span>  
* <span data-ttu-id="c43f3-277">[Povolit rozšíření virtuálního počítače Log Analytics](log-analytics-azure-vm-extension.md) na každém uzlu OpenShift umístěných v Azure</span><span class="sxs-lookup"><span data-stu-id="c43f3-277">[Enable Log Analytics VM Extension](log-analytics-azure-vm-extension.md) on each OpenShift node residing in Azure</span></span>  
* <span data-ttu-id="c43f3-278">Nainstalovat agenta OMS jako OpenShift démon sada</span><span class="sxs-lookup"><span data-stu-id="c43f3-278">Install the OMS Agent as a OpenShift daemon-set</span></span>  

<span data-ttu-id="c43f3-279">V této části nabídneme kroky potřebné k instalaci agenta OMS jako démon OpenShift-set.</span><span class="sxs-lookup"><span data-stu-id="c43f3-279">In this section we cover the steps required to install the OMS Agent as an OpenShift daemon-set.</span></span>  

1. <span data-ttu-id="c43f3-280">Přihlaste se k hlavní uzel OpenShift a zkopírujte soubor yaml [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) z webu GitHub na hlavní uzel a změňte hodnotu s vaše ID pracovního prostoru OMS a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c43f3-280">Sign on to the OpenShift master node and copy the yaml file [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) from GitHub to your master node and modify the value with your OMS Workspace ID and with your Primary Key.</span></span>
2. <span data-ttu-id="c43f3-281">Spusťte následující příkazy pro vytvoření projektu pro OMS a nastavení uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-281">Run the following commands to create a project for OMS and set the user account.</span></span>

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="c43f3-282">Chcete-li nasadit sadu démon, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-282">To deploy the daemon-set, run the following:</span></span>

    `oc create -f ocp-omsagent.yaml`

5. <span data-ttu-id="c43f3-283">Ověřte, zda že je nakonfigurován a funguje správně, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c43f3-283">To verify it is configured and working correctly, type the following:</span></span>

    `oc describe daemonset omsagent`  

    <span data-ttu-id="c43f3-284">a měl by vypadat jako výstup:</span><span class="sxs-lookup"><span data-stu-id="c43f3-284">and the output should resemble:</span></span>

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

<span data-ttu-id="c43f3-285">Pokud chcete použít k zabezpečení ID pracovního prostoru OMS a primární klíč při použití souboru démon set yaml agenta OMS tajné klíče, proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-285">If you want to use secrets to secure your OMS Workspace ID and Primary Key when using the OMS Agent daemon-set yaml file, perform the following steps.</span></span>

1. <span data-ttu-id="c43f3-286">Přihlaste se k hlavní uzel OpenShift a zkopírujte soubor yaml [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) a tajný klíč generování skriptu [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-286">Sign on to the OpenShift master node and copy the yaml file [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) and secret generating script [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) from GitHub.</span></span>  <span data-ttu-id="c43f3-287">Tento skript vygeneruje soubor yaml tajné klíče pro ID pracovního prostoru OMS a primární klíč zabezpečit vaše secrete informace.</span><span class="sxs-lookup"><span data-stu-id="c43f3-287">This script will generate the secrets yaml file for OMS Workspace ID and Primary Key to secure your secrete information.</span></span>  
2. <span data-ttu-id="c43f3-288">Spusťte následující příkazy pro vytvoření projektu pro OMS a nastavení uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-288">Run the following commands to create a project for OMS and set the user account.</span></span> <span data-ttu-id="c43f3-289">Tajný klíč generování skriptu požádá o vaše ID pracovního prostoru OMS <WSID> a primární klíč <KEY> a po dokončení zpracování se vytvoří soubor ocp-secret.yaml.</span><span class="sxs-lookup"><span data-stu-id="c43f3-289">The secret generating script asks for your OMS Workspace ID <WSID> and Primary Key <KEY> and upon completion, it creates the ocp-secret.yaml file.</span></span>  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. <span data-ttu-id="c43f3-290">Nasaďte soubor tajný spuštěním následujícího:</span><span class="sxs-lookup"><span data-stu-id="c43f3-290">Deploy the secret file by running the following:</span></span>

    `oc create -f ocp-secret.yaml`

5. <span data-ttu-id="c43f3-291">Ověření nasazení tak, že spustíte následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-291">Verify deployment by running the following:</span></span>

    `oc describe secret omsagent-secret`  

    <span data-ttu-id="c43f3-292">a měl by vypadat jako výstup:</span><span class="sxs-lookup"><span data-stu-id="c43f3-292">and the  output should resemble:</span></span>  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. <span data-ttu-id="c43f3-293">Nasaďte soubor démon set yaml OMS agenta spuštěním následujícího:</span><span class="sxs-lookup"><span data-stu-id="c43f3-293">Deploy the OMS Agent daemon-set yaml file by running the following:</span></span>

    `oc create -f ocp-ds-omsagent.yaml`  

7. <span data-ttu-id="c43f3-294">Ověření nasazení tak, že spustíte následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-294">Verify deployment by running the following:</span></span>

    `oc describe ds oms`

    <span data-ttu-id="c43f3-295">a měl by vypadat jako výstup:</span><span class="sxs-lookup"><span data-stu-id="c43f3-295">and the output should resemble:</span></span>

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a><span data-ttu-id="c43f3-296">Zabezpečit vaše tajné informace pro Docker Swarm a Kubernetes</span><span class="sxs-lookup"><span data-stu-id="c43f3-296">Secure your secret information for Docker Swarm and Kubernetes</span></span>

<span data-ttu-id="c43f3-297">Pro Docker Swarm a Kubernetes kontejneru služby můžete zabezpečit tajný ID pracovního prostoru OMS a primární klíče.</span><span class="sxs-lookup"><span data-stu-id="c43f3-297">You can secure your secret OMS Workspace ID and Primary Keys for Docker Swarm and Kubernetes container services.</span></span>

#### <a name="secure-secrets-for-docker-swarm"></a><span data-ttu-id="c43f3-298">Zabezpečené tajné klíče pro Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="c43f3-298">Secure secrets for Docker Swarm</span></span>

<span data-ttu-id="c43f3-299">Pro Docker Swarm Jakmile se vytvoří tajný klíč pro ID pracovního prostoru a primární klíč, můžete spustit vytvořením službu Docker OMSagent.</span><span class="sxs-lookup"><span data-stu-id="c43f3-299">For Docker Swarm, once the secret for Workspace ID and Primary Key is created, you can run the create the Docker service for OMSagent.</span></span> <span data-ttu-id="c43f3-300">Použijte následující informace pro vytvoření tajných informací.</span><span class="sxs-lookup"><span data-stu-id="c43f3-300">Use the following information to create your secret information.</span></span>

1. <span data-ttu-id="c43f3-301">Spusťte následující na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="c43f3-301">Run the following on the master node.</span></span>

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. <span data-ttu-id="c43f3-302">Ověřte, že tajné klíče byly vytvořeny správně.</span><span class="sxs-lookup"><span data-stu-id="c43f3-302">Verify that secrets were created properly.</span></span>

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. <span data-ttu-id="c43f3-303">Spusťte následující příkaz připojit tajné klíče kontejnerizované Agent OMS.</span><span class="sxs-lookup"><span data-stu-id="c43f3-303">Run the following command to mount the secrets to the containerized OMS Agent.</span></span>

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a><span data-ttu-id="c43f3-304">Zabezpečené tajné klíče pro Kubernetes s yaml soubory</span><span class="sxs-lookup"><span data-stu-id="c43f3-304">Secure secrets for Kubernetes with yaml files</span></span>

<span data-ttu-id="c43f3-305">Pro Kubernetes pomocí skriptu pro generování souboru yaml tajné klíče pro ID pracovního prostoru a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c43f3-305">For Kubernetes, you use a script to generate the secrets yaml file for your Workspace ID and Primary Key.</span></span> <span data-ttu-id="c43f3-306">Na [OMS Docker Kubernetes Githubu](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) stránky, jsou soubory, které můžete použít s nebo bez tajné informace.</span><span class="sxs-lookup"><span data-stu-id="c43f3-306">At the [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) page, there are files that you can use with or without your secret information.</span></span>

- <span data-ttu-id="c43f3-307">DaemonSet výchozí agenta OMS nemá tajné informace (omsagent.yaml)</span><span class="sxs-lookup"><span data-stu-id="c43f3-307">The Default OMS Agent DaemonSet does not have secret information (omsagent.yaml)</span></span>
- <span data-ttu-id="c43f3-308">Soubor yaml DaemonSet agenta OMS používá tajné informace (omsagent – ds-secrets.yaml) s tajný generování skriptů pro generování souboru yaml (omsagentsecret.yaml) tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-308">The OMS Agent DaemonSet yaml file uses secret information (omsagent-ds-secrets.yaml) with secret generation scripts to generate the secrets yaml (omsagentsecret.yaml) file.</span></span>

<span data-ttu-id="c43f3-309">Můžete vytvořit omsagent DaemonSets s nebo bez tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-309">You can choose to create omsagent DaemonSets with or without secrets.</span></span>

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a><span data-ttu-id="c43f3-310">Výchozí soubor yaml OMSagent DaemonSet bez tajné klíče</span><span class="sxs-lookup"><span data-stu-id="c43f3-310">Default OMSagent DaemonSet yaml file without secrets</span></span>

- <span data-ttu-id="c43f3-311">Výchozí DaemonSet agenta OMS yaml souboru, nahraďte `<WSID>` a `<KEY>` WSID a klíč.</span><span class="sxs-lookup"><span data-stu-id="c43f3-311">For the default OMS Agent DaemonSet yaml file, replace the `<WSID>` and `<KEY>` to your WSID and KEY.</span></span> <span data-ttu-id="c43f3-312">Zkopírujte soubor do hlavní uzel a spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-312">Copy the file to your master node and run the following:</span></span>

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a><span data-ttu-id="c43f3-313">Výchozí OMSagent DaemonSet yaml soubor obsahující tajné údaje</span><span class="sxs-lookup"><span data-stu-id="c43f3-313">Default OMSagent DaemonSet yaml file with secrets</span></span>

1. <span data-ttu-id="c43f3-314">Pokud chcete používat DaemonSet agenta OMS pomocí tajné informace, nejprve vytvořte těchto tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-314">To use OMS Agent DaemonSet using secret information, create the secrets first.</span></span>
    1. <span data-ttu-id="c43f3-315">Zkopírujte skript a soubor tajný šablony a ujistěte se, že jsou ve stejném adresáři.</span><span class="sxs-lookup"><span data-stu-id="c43f3-315">Copy the script and secret template file and make sure they are on the same directory.</span></span>
        - <span data-ttu-id="c43f3-316">Generování skriptu - tajný klíč gen.sh tajný klíč</span><span class="sxs-lookup"><span data-stu-id="c43f3-316">Secret generating script - secret-gen.sh</span></span>
        - <span data-ttu-id="c43f3-317">Šablona tajné – template.yaml tajný klíč</span><span class="sxs-lookup"><span data-stu-id="c43f3-317">secret template - secret-template.yaml</span></span>
    2. <span data-ttu-id="c43f3-318">Spusťte skript, jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-318">Run the script, like the following example.</span></span> <span data-ttu-id="c43f3-319">Skript vyzve k zadání ID pracovního prostoru OMS a primární klíč a po zadání je skript vytvoří soubor tajný yaml, můžete ji spustit.</span><span class="sxs-lookup"><span data-stu-id="c43f3-319">The script asks for the OMS Workspace ID and Primary Key and after you enter them, the script creates a secret yaml file so you can run it.</span></span>   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. <span data-ttu-id="c43f3-320">Vytvoření tajných klíčů pod spuštěním následujícího:</span><span class="sxs-lookup"><span data-stu-id="c43f3-320">Create the secrets pod by running the following:</span></span>
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. <span data-ttu-id="c43f3-321">Pokud chcete ověřit, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-321">To verify, run the following:</span></span>

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        <span data-ttu-id="c43f3-322">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c43f3-322">Output should resemble:</span></span>

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        <span data-ttu-id="c43f3-323">Výstup by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="c43f3-323">Output should resemble:</span></span>

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. <span data-ttu-id="c43f3-324">Vytvoření vaší omsagent démon set spuštěním``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="c43f3-324">Create your omsagent daemon-set by running ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

2. <span data-ttu-id="c43f3-325">Ověřte, zda DaemonSet agenta OMS je spuštěna, podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="c43f3-325">Verify that the OMS Agent DaemonSet is running, similar to the following:</span></span>

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


<span data-ttu-id="c43f3-326">Pro Kubernetes pomocí skriptu pro generování souboru yaml tajné klíče pro ID pracovního prostoru a primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c43f3-326">For Kubernetes, use a script to generate the secrets yaml file for Workspace ID and Primary Key.</span></span> <span data-ttu-id="c43f3-327">Následující příklad informace s [omsagent yaml souboru](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) zabezpečit vaše tajné informace.</span><span class="sxs-lookup"><span data-stu-id="c43f3-327">Use the following example information with the [omsagent yaml file](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) to secure your secret information.</span></span>

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a><span data-ttu-id="c43f3-328">Hostitelé kontejneru systému Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-328">Windows container hosts</span></span>

### <a name="preparation-before-installing-windows-agents"></a><span data-ttu-id="c43f3-329">Příprava před instalací agentů v systému Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-329">Preparation before installing Windows agents</span></span>

<span data-ttu-id="c43f3-330">Před instalací agentů do počítačů se systémem Windows, musíte nakonfigurovat službu Docker.</span><span class="sxs-lookup"><span data-stu-id="c43f3-330">Before you install agents on computers running Windows, you need to configure the Docker service.</span></span> <span data-ttu-id="c43f3-331">Konfigurace umožňuje agent služby Windows nebo analýzy protokolů rozšíření virtuálního počítače používat soketu Docker TCP tak agentů můžete přistupovat vzdáleně démon Docker a k zaznamenání dat pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="c43f3-331">The configuration allows the Windows agent or the Log Analytics virtual machine extension to use the Docker TCP socket so that the agents can access the Docker daemon remotely and to capture data for monitoring.</span></span>

#### <a name="to-start-docker-and-verify-its-configuration"></a><span data-ttu-id="c43f3-332">Spuštění Docker a ověřte jeho konfiguraci</span><span class="sxs-lookup"><span data-stu-id="c43f3-332">To start Docker and verify its configuration</span></span>

<span data-ttu-id="c43f3-333">Je třeba kroky potřebné k nastavení TCP pojmenovaný kanál pro systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="c43f3-333">There are steps needed to set up TCP named pipe for Windows Server:</span></span>

1. <span data-ttu-id="c43f3-334">V prostředí Windows PowerShell povolte kanálu TCP a pojmenovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="c43f3-334">In Windows PowerShell, enable TCP pipe and named pipe.</span></span>

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. <span data-ttu-id="c43f3-335">Konfigurace Docker pomocí konfiguračního souboru pro TCP kanálu a pojmenovaný kanál.</span><span class="sxs-lookup"><span data-stu-id="c43f3-335">Configure Docker with the configuration file for TCP pipe and named pipe.</span></span> <span data-ttu-id="c43f3-336">Konfigurační soubor se nachází v C:\ProgramData\docker\config\daemon.json.</span><span class="sxs-lookup"><span data-stu-id="c43f3-336">The configuration file is located at C:\ProgramData\docker\config\daemon.json.</span></span>

    <span data-ttu-id="c43f3-337">V souboru daemon.json budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="c43f3-337">In the daemon.json file, you will need the following:</span></span>

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

<span data-ttu-id="c43f3-338">Další informace o konfiguraci démon Docker použít s kontejnery Windows najdete v tématu [modulu Docker v systému Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span><span class="sxs-lookup"><span data-stu-id="c43f3-338">For more information about the Docker daemon configuration used with Windows Containers, see [Docker Engine on Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).</span></span>


### <a name="install-windows-agents"></a><span data-ttu-id="c43f3-339">Instalace agentů v systému Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-339">Install Windows agents</span></span>

<span data-ttu-id="c43f3-340">Chcete-li povolit monitorování kontejneru systému Windows a technologie Hyper-V, nainstalujte Microsoft Monitoring Agent (MMA) na počítačích s Windows, které jsou hostiteli kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-340">To enable Windows and Hyper-V container monitoring, install the Microsoft Monitoring Agent (MMA) on Windows computers that are container hosts.</span></span> <span data-ttu-id="c43f3-341">Pro počítače se systémem Windows v místním prostředí, najdete v části [počítače se systémem Windows se připojit k analýze protokolů](log-analytics-windows-agents.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-341">For computers running Windows in your on-premises environment, see [Connect Windows computers to Log Analytics](log-analytics-windows-agents.md).</span></span> <span data-ttu-id="c43f3-342">Pro virtuální počítače běžící v Azure, připojte je k analýze protokolů pomocí [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-342">For virtual machines running in Azure, connect them to Log Analytics using the [virtual machine extension](log-analytics-azure-vm-extension.md).</span></span>

<span data-ttu-id="c43f3-343">Můžete monitorovat kontejnery Windows spuštěné v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c43f3-343">You can monitor Windows containers running on Service Fabric.</span></span> <span data-ttu-id="c43f3-344">Ale pouze [virtuální počítače běžící v Azure](log-analytics-azure-vm-extension.md) a [počítačů se systémem Windows v místním prostředí](log-analytics-windows-agents.md) jsou aktuálně podporovány pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c43f3-344">However, only [virtual machines running in Azure](log-analytics-azure-vm-extension.md) and [computers running Windows in your on-premises environment](log-analytics-windows-agents.md) are currently supported for Service Fabric.</span></span>

<span data-ttu-id="c43f3-345">Můžete ověřit, jestli je řešení monitorování kontejneru správně nastavené pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="c43f3-345">You can verify that the Container Monitoring solution is set correctly for Windows.</span></span> <span data-ttu-id="c43f3-346">Chcete-li zkontrolovat, jestli byla sada management pack správně ke stažení, vyhledejte *ContainerManagement.xxx*.</span><span class="sxs-lookup"><span data-stu-id="c43f3-346">To check whether the management pack was download properly, look for *ContainerManagement.xxx*.</span></span> <span data-ttu-id="c43f3-347">Soubory musí být ve složce C:\Program Files\Microsoft Monitoring Agent\Agent\Health služby State\Management balíčky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-347">The files should be in the C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs folder.</span></span>


## <a name="solution-components"></a><span data-ttu-id="c43f3-348">Součásti řešení</span><span class="sxs-lookup"><span data-stu-id="c43f3-348">Solution components</span></span>

<span data-ttu-id="c43f3-349">Pokud používáte agenty se systémem Windows, je při přidání tohoto řešení následující sady management pack nainstalované na každém počítači s agentem.</span><span class="sxs-lookup"><span data-stu-id="c43f3-349">If you are using Windows agents, then the following management pack is installed on each computer with an agent when you add this solution.</span></span> <span data-ttu-id="c43f3-350">Je požadován pro sadu management pack bez konfigurace nebo údržby.</span><span class="sxs-lookup"><span data-stu-id="c43f3-350">No configuration or maintenance is required for the management pack.</span></span>

- <span data-ttu-id="c43f3-351">*ContainerManagement.xxx* nainstalované v C:\Program Files\Microsoft Monitoring Agent\Agent\Health State\Management SP</span><span class="sxs-lookup"><span data-stu-id="c43f3-351">*ContainerManagement.xxx* installed in C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs</span></span>

## <a name="container-data-collection-details"></a><span data-ttu-id="c43f3-352">Kontejner dat kolekce podrobnosti</span><span class="sxs-lookup"><span data-stu-id="c43f3-352">Container data collection details</span></span>
<span data-ttu-id="c43f3-353">Řešení monitorování kontejneru shromažďuje různé metriky a protokolu údaje o výkonu z kontejneru hostitelů a kontejnery pomocí agentů, které povolíte.</span><span class="sxs-lookup"><span data-stu-id="c43f3-353">The Container Monitoring solution collects various performance metrics and log data from container hosts and containers using agents that you enable.</span></span>

<span data-ttu-id="c43f3-354">Data jsou shromažďována každé tři minuty následující typy agenta.</span><span class="sxs-lookup"><span data-stu-id="c43f3-354">Data is collected every three minutes by the following agent types.</span></span>

- [<span data-ttu-id="c43f3-355">OMS agenta pro Linux</span><span class="sxs-lookup"><span data-stu-id="c43f3-355">OMS Agent for Linux</span></span>](log-analytics-linux-agents.md)
- [<span data-ttu-id="c43f3-356">Agent služby Windows</span><span class="sxs-lookup"><span data-stu-id="c43f3-356">Windows agent</span></span>](log-analytics-windows-agents.md)
- [<span data-ttu-id="c43f3-357">Rozšíření virtuálního počítače analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c43f3-357">Log Analytics VM extension</span></span>](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a><span data-ttu-id="c43f3-358">Záznamů kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-358">Container records</span></span>

<span data-ttu-id="c43f3-359">V následující tabulce jsou uvedeny příklady záznamů shromážděných řešením pro monitorování kontejneru a datové typy, které se zobrazí ve výsledcích hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-359">The following table shows examples of records collected by the Container Monitoring solution and the data types that appear in log search results.</span></span>

| <span data-ttu-id="c43f3-360">Datový typ</span><span class="sxs-lookup"><span data-stu-id="c43f3-360">Data type</span></span> | <span data-ttu-id="c43f3-361">Datový typ v hledání protokolů</span><span class="sxs-lookup"><span data-stu-id="c43f3-361">Data type in Log Search</span></span> | <span data-ttu-id="c43f3-362">Pole</span><span class="sxs-lookup"><span data-stu-id="c43f3-362">Fields</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c43f3-363">Výkon pro hostitele a kontejnery</span><span class="sxs-lookup"><span data-stu-id="c43f3-363">Performance for hosts and containers</span></span> | `Type=Perf` | <span data-ttu-id="c43f3-364">Počítač, ObjectName, název_čítače &#40; % času procesoru, Disk načte MB, zapíše MB, MB využití paměti, disku sítě přijatých bajtů, síti odesílat bajtů, procesor doba využití sítě &#41; přepočtené, TimeGenerated, Cesta_k_čítači, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="c43f3-364">Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem</span></span> |
| <span data-ttu-id="c43f3-365">Kontejner inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-365">Container inventory</span></span> | `Type=ContainerInventory` | <span data-ttu-id="c43f3-366">TimeGenerated, počítače a název kontejneru, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, příkazu, CreatedTime, StartedTime, FinishedTime, SourceSystem, identifikátor ContainerID, ID obrázku</span><span class="sxs-lookup"><span data-stu-id="c43f3-366">TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID</span></span> |
| <span data-ttu-id="c43f3-367">Kontejner image inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-367">Container image inventory</span></span> | `Type=ContainerImageInventory` | <span data-ttu-id="c43f3-368">TimeGenerated, počítače, Image, ImageTag, ImageSize, VirtualSize, spuštění, pozastavena, zastavit, se nezdařilo, SourceSystem, ID obrázku, TotalContainer</span><span class="sxs-lookup"><span data-stu-id="c43f3-368">TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer</span></span> |
| <span data-ttu-id="c43f3-369">Kontejner protokolu</span><span class="sxs-lookup"><span data-stu-id="c43f3-369">Container log</span></span> | `Type=ContainerLog` | <span data-ttu-id="c43f3-370">TimeGenerated, počítač, ID bitové kopie, název kontejneru, LogEntrySource, LogEntry, SourceSystem, identifikátor ContainerID</span><span class="sxs-lookup"><span data-stu-id="c43f3-370">TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="c43f3-371">Protokol služby kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-371">Container service log</span></span> | `Type=ContainerServiceLog`  | <span data-ttu-id="c43f3-372">TimeGenerated, počítače, TimeOfCommand, Image, příkazu, SourceSystem, identifikátor ContainerID</span><span class="sxs-lookup"><span data-stu-id="c43f3-372">TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID</span></span> |
| <span data-ttu-id="c43f3-373">Uzel inventáře kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-373">Container node inventory</span></span> | `Type=ContainerNodeInventory_CL`| <span data-ttu-id="c43f3-374">TimeGenerated, počítače, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="c43f3-374">TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem</span></span>|
| <span data-ttu-id="c43f3-375">Kubernetes inventáře</span><span class="sxs-lookup"><span data-stu-id="c43f3-375">Kubernetes inventory</span></span> | `Type=KubePodInventory_CL` | <span data-ttu-id="c43f3-376">TimeGenerated, počítače, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="c43f3-376">TimeGenerated, Computer, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem</span></span> |
| <span data-ttu-id="c43f3-377">Proces kontejneru</span><span class="sxs-lookup"><span data-stu-id="c43f3-377">Container process</span></span> | `Type=ContainerProcess_CL` | <span data-ttu-id="c43f3-378">TimeGenerated, počítače, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span><span class="sxs-lookup"><span data-stu-id="c43f3-378">TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem</span></span> |
| <span data-ttu-id="c43f3-379">Kubernetes události</span><span class="sxs-lookup"><span data-stu-id="c43f3-379">Kubernetes events</span></span> | `Type=KubeEvents_CL` | <span data-ttu-id="c43f3-380">TimeGenerated, počítače, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, zprávy</span><span class="sxs-lookup"><span data-stu-id="c43f3-380">TimeGenerated, Computer, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, Message</span></span> |

<span data-ttu-id="c43f3-381">Popisky připojenou k *PodLabel* datové typy jsou vlastní štítky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-381">Labels appended to *PodLabel* data types are your own custom labels.</span></span> <span data-ttu-id="c43f3-382">Připojením PodLabel popisky uvedené v tabulce jsou uvedeny příklady.</span><span class="sxs-lookup"><span data-stu-id="c43f3-382">The appended PodLabel labels shown in the table are examples.</span></span> <span data-ttu-id="c43f3-383">Ano `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` se liší v sadě dat vaše prostředí a obecně vypadat jako `PodLabel_yourlabel_s`.</span><span class="sxs-lookup"><span data-stu-id="c43f3-383">So, `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` will differ in your environment's data set and generically resemble `PodLabel_yourlabel_s`.</span></span>


## <a name="monitor-containers"></a><span data-ttu-id="c43f3-384">Monitorování kontejnery</span><span class="sxs-lookup"><span data-stu-id="c43f3-384">Monitor containers</span></span>
<span data-ttu-id="c43f3-385">Až budete mít řešení povoleno na portálu OMS **kontejnery** dlaždice se zobrazí souhrnné informace o kontejneru hostitelů a kontejnerů, které jsou spuštěné v hostitelích.</span><span class="sxs-lookup"><span data-stu-id="c43f3-385">After you have the solution enabled in the OMS portal, the **Containers** tile shows summary information about your container hosts and the containers running in hosts.</span></span>

![Dlaždice kontejnery](./media/log-analytics-containers/containers-title.png)

<span data-ttu-id="c43f3-387">Na dlaždici ukazuje přehled o tom, kolik kontejnery, které máte v prostředí a jestli se nezdařila, spuštěná nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="c43f3-387">The tile shows an overview of how many containers you have in the environment and whether they're failed, running, or stopped.</span></span>

### <a name="using-the-containers-dashboard"></a><span data-ttu-id="c43f3-388">Pomocí řídicího panelu kontejnery</span><span class="sxs-lookup"><span data-stu-id="c43f3-388">Using the Containers dashboard</span></span>
<span data-ttu-id="c43f3-389">Klikněte **kontejnery** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="c43f3-389">Click the **Containers** tile.</span></span> <span data-ttu-id="c43f3-390">Zde se zobrazí zobrazení uspořádané podle:</span><span class="sxs-lookup"><span data-stu-id="c43f3-390">From there you'll see views organized by:</span></span>

- <span data-ttu-id="c43f3-391">**Události kontejneru** -zobrazuje stav kontejneru a počítače s kontejnery se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="c43f3-391">**Container Events** - Shows container status and computers with failed containers.</span></span>
- <span data-ttu-id="c43f3-392">**Kontejner protokoly** – zobrazuje graf soubory protokolu kontejneru vygeneroval přes čas a seznam počítačů s nejvyšší počet souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-392">**Container Logs** - Shows a chart of container log files generated over time and a list of computers with the highest number of log files.</span></span>
- <span data-ttu-id="c43f3-393">**Události Kubernetes** – zobrazuje graf Kubernetes události generované přes čas a seznam důvodů, proč pracovními stanicemi soustředěnými kolem vygenerované události.</span><span class="sxs-lookup"><span data-stu-id="c43f3-393">**Kubernetes Events** - Shows a chart of Kubernetes events generated over time and a list of the reasons why pods generated the events.</span></span> <span data-ttu-id="c43f3-394">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="c43f3-394">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="c43f3-395">**Kubernetes Namespace inventáře** – zobrazuje počet obory názvů a pracovními stanicemi soustředěnými kolem a zobrazí jejich hierarchie.</span><span class="sxs-lookup"><span data-stu-id="c43f3-395">**Kubernetes Namespace Inventory** - Shows the number of namespaces and pods and shows their hierarchy.</span></span> <span data-ttu-id="c43f3-396">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="c43f3-396">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="c43f3-397">**Uzel inventáře kontejneru** – zobrazuje počet orchestration typy používané v kontejneru uzly nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="c43f3-397">**Container Node Inventory** - Shows the number of orchestration types used on container nodes/hosts.</span></span> <span data-ttu-id="c43f3-398">Uzly počítače nebo hostitelé jsou také uvedeny podle počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-398">The computer nodes/hosts are also listed by the number of containers.</span></span> <span data-ttu-id="c43f3-399">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="c43f3-399">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="c43f3-400">**Kontejner Imagí inventáře** -zobrazuje celkový počet kontejneru obrázků použitých a počet typů bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c43f3-400">**Container Images Inventory** - Shows the total number of container images used and number of image types.</span></span> <span data-ttu-id="c43f3-401">Počet bitových kopií jsou také uvedené podle značky obrázku.</span><span class="sxs-lookup"><span data-stu-id="c43f3-401">The number of images are also listed by the image tag.</span></span>
- <span data-ttu-id="c43f3-402">**Kontejnery stav** -zobrazuje celkový počet kontejneru uzly nebo hostitelské počítače, které mají spuštěných kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-402">**Containers Status** - Shows the total number of container nodes/host computers that have running containers.</span></span> <span data-ttu-id="c43f3-403">Počítače jsou také uvedeny podle počet probíhajících hostitele.</span><span class="sxs-lookup"><span data-stu-id="c43f3-403">Computers are also listed by the number of running hosts.</span></span>
- <span data-ttu-id="c43f3-404">**Kontejner proces** -zobrazuje spojnicový graf kontejneru procesů spuštěných v čase.</span><span class="sxs-lookup"><span data-stu-id="c43f3-404">**Container Process** - Shows a line chart of container processes running over time.</span></span> <span data-ttu-id="c43f3-405">Kontejnery také uvádí spuštěním příkazu/procesu v rámci kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-405">Containers are also listed by running command/process within containers.</span></span> <span data-ttu-id="c43f3-406">*Tato datová sada se používá jenom v prostředích Linux.*</span><span class="sxs-lookup"><span data-stu-id="c43f3-406">*This data set is used only in Linux environments.*</span></span>
- <span data-ttu-id="c43f3-407">**Výkon procesoru kontejneru** – zobrazuje graf řádku průměrné využití procesoru v čase pro počítač uzly nebo hostitele.</span><span class="sxs-lookup"><span data-stu-id="c43f3-407">**Container CPU Performance** - Shows a line chart of the average CPU utilization over time for computer nodes/hosts.</span></span> <span data-ttu-id="c43f3-408">Také seznamy počítače uzly nebo hostitelů na základě průměrné využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-408">Also lists the computer nodes/hosts based on average CPU utilization.</span></span>
- <span data-ttu-id="c43f3-409">**Kontejner výkonu paměti** -znázorňuje spojnicový graf využití paměti v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="c43f3-409">**Container Memory Performance** - Shows a line chart of memory usage over time.</span></span> <span data-ttu-id="c43f3-410">Také uvádí na název instance na základě využití paměti počítače.</span><span class="sxs-lookup"><span data-stu-id="c43f3-410">Also lists computer memory utilization based on instance name.</span></span>
- <span data-ttu-id="c43f3-411">**Výkon počítače** -znázorňuje spojnicových grafů procent výkonu procesoru v čase, procentuální využití paměti nad čas a MB volného místa na disku v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="c43f3-411">**Computer Performance** - Shows line charts of the percent of CPU performance over time, percent of memory usage over time, and megabytes of free disk space over time.</span></span> <span data-ttu-id="c43f3-412">Pozastavte ukazatel myši nad kterýkoli řádek v grafu zobrazíte další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c43f3-412">You can hover over any line in a chart to view more details.</span></span>


<span data-ttu-id="c43f3-413">Každou oblast řídicí panel je vizuální reprezentace hledání, které běží na shromážděná data.</span><span class="sxs-lookup"><span data-stu-id="c43f3-413">Each area of the dashboard is a visual representation of a search that is run on collected data.</span></span>

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash01.png)

![Řídicí panel kontejnery](./media/log-analytics-containers/containers-dash02.png)

<span data-ttu-id="c43f3-416">V **stav kontejneru** oblast, klikněte na hlavní oblasti, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c43f3-416">In the **Container Status** area, click the top area, as shown below.</span></span>

![Stav kontejnery](./media/log-analytics-containers/containers-status.png)

<span data-ttu-id="c43f3-418">Protokol hledání se otevře, zobrazení informací o stavu kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-418">Log Search opens, displaying information about the state of your containers.</span></span>

![Hledání protokolů pro kontejnery](./media/log-analytics-containers/containers-log-search.png)

<span data-ttu-id="c43f3-420">Zde můžete upravit vyhledávací dotaz jak v hotové najít konkrétní informace, že vás zajímá.</span><span class="sxs-lookup"><span data-stu-id="c43f3-420">From here, you can edit the search query to modify it to find the specific information you're interested in.</span></span> <span data-ttu-id="c43f3-421">Další informace o protokolu hledání najdete v tématu [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="c43f3-421">For more information about Log Searches, see [Log searches in Log Analytics](log-analytics-log-searches.md).</span></span>

## <a name="troubleshoot-by-finding-a-failed-container"></a><span data-ttu-id="c43f3-422">Řešení potíží s tak, že najdete kontejner se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="c43f3-422">Troubleshoot by finding a failed container</span></span>

<span data-ttu-id="c43f3-423">Analýzy protokolů označí kontejneru jako **se nezdařilo** Pokud byl ukončen s nenulový ukončovací kód.</span><span class="sxs-lookup"><span data-stu-id="c43f3-423">Log Analytics marks a container as **Failed** if it has exited with a non-zero exit code.</span></span> <span data-ttu-id="c43f3-424">Zobrazí se přehled chyb nebo selhání v prostředí v **se nezdařilo kontejnery** oblasti.</span><span class="sxs-lookup"><span data-stu-id="c43f3-424">You can see an overview of the errors and failures in the environment in the **Failed Containers** area.</span></span>

### <a name="to-find-failed-containers"></a><span data-ttu-id="c43f3-425">K vyhledání chybných kontejnery</span><span class="sxs-lookup"><span data-stu-id="c43f3-425">To find failed containers</span></span>
1. <span data-ttu-id="c43f3-426">Klikněte **stav kontejneru** oblasti.</span><span class="sxs-lookup"><span data-stu-id="c43f3-426">Click the **Container Status** area.</span></span>  
   <span data-ttu-id="c43f3-427">![Stav kontejnery](./media/log-analytics-containers/containers-status.png)</span><span class="sxs-lookup"><span data-stu-id="c43f3-427">![containers status](./media/log-analytics-containers/containers-status.png)</span></span>
2. <span data-ttu-id="c43f3-428">Protokol hledání se zobrazí stav kontejnery, podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-428">Log Search opens and displays the state of your containers, similar to the following.</span></span>  
   ![kontejnery stavu](./media/log-analytics-containers/containers-log-search.png)
3. <span data-ttu-id="c43f3-430">Klikněte na tlačítko agregovaná hodnota selhání kontejnery zobrazíte další informace.</span><span class="sxs-lookup"><span data-stu-id="c43f3-430">Next, click the aggregated value of failed containers to view additional information.</span></span> <span data-ttu-id="c43f3-431">Rozbalte položku **zobrazit další** zobrazíte ID obrázku.</span><span class="sxs-lookup"><span data-stu-id="c43f3-431">Expand **show more** to view the image ID.</span></span>  
   <span data-ttu-id="c43f3-432">![Neúspěšné kontejnery](./media/log-analytics-containers/containers-state-failed.png)</span><span class="sxs-lookup"><span data-stu-id="c43f3-432">![failed containers](./media/log-analytics-containers/containers-state-failed.png)</span></span>  
4. <span data-ttu-id="c43f3-433">Potom zadejte následující příkaz v vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="c43f3-433">Next, type the following in the search query.</span></span> <span data-ttu-id="c43f3-434">`Type=ContainerInventory <ImageID>`Chcete-li zobrazit podrobnosti o bitovou kopii například velikost bitové kopie a počet zastaven a k selhání bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c43f3-434">`Type=ContainerInventory <ImageID>` to see details about the image such as image size and number of stopped and failed images.</span></span>  
   <span data-ttu-id="c43f3-435">![Neúspěšné kontejnery](./media/log-analytics-containers/containers-failed04.png)</span><span class="sxs-lookup"><span data-stu-id="c43f3-435">![failed containers](./media/log-analytics-containers/containers-failed04.png)</span></span>

## <a name="search-logs-for-container-data"></a><span data-ttu-id="c43f3-436">Hledání protokoly pro kontejner dat</span><span class="sxs-lookup"><span data-stu-id="c43f3-436">Search logs for container data</span></span>
<span data-ttu-id="c43f3-437">Pokud se řešení potíží s konkrétní chyby, může pomoct zobrazíte, kde je vznikl ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="c43f3-437">When you're troubleshooting a specific error, it can help to see where it is occurring in your environment.</span></span> <span data-ttu-id="c43f3-438">Následující typy protokolu vám pomůže vytvořit dotazy k vrácení informací, že chcete.</span><span class="sxs-lookup"><span data-stu-id="c43f3-438">The following log types will help you create queries to return the information you want.</span></span>


- <span data-ttu-id="c43f3-439">**ContainerImageInventory** – tento typ použijte, když se pokoušíte najít informace o uspořádané podle bitové kopie a chcete-li zobrazit informace o obrázku například image ID nebo velikosti.</span><span class="sxs-lookup"><span data-stu-id="c43f3-439">**ContainerImageInventory** – Use this type when you're trying to find information organized by image and to view image information such as image IDs or sizes.</span></span>
- <span data-ttu-id="c43f3-440">**ContainerInventory** – tento typ použijte, pokud chcete informace o umístění kontejneru, jaké jsou jejich názvy a co bitové kopie, že používáte.</span><span class="sxs-lookup"><span data-stu-id="c43f3-440">**ContainerInventory** – Use this type when you want information about container location, what their names are, and what images they're running.</span></span>
- <span data-ttu-id="c43f3-441">**ContainerLog** – tento typ použijte, pokud chcete najít informace o konkrétní chybě protokolu a položky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-441">**ContainerLog** – Use this type when you want to find specific error log information and entries.</span></span>
- <span data-ttu-id="c43f3-442">**ContainerNodeInventory_CL** tento typ můžete použít informace o uzlu hostitele nebo kde je umístěný kontejnery.</span><span class="sxs-lookup"><span data-stu-id="c43f3-442">**ContainerNodeInventory_CL**  Use this type when you want the information about host/node where containers are residing.</span></span> <span data-ttu-id="c43f3-443">Poskytuje Docker verze, typ orchestration, úložiště a informace o síti.</span><span class="sxs-lookup"><span data-stu-id="c43f3-443">It provides you Docker version, orchestration type, storage, and network information.</span></span>
- <span data-ttu-id="c43f3-444">**ContainerProcess_CL** pomocí tohoto typu lze rychle zobrazit proces, který běží v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-444">**ContainerProcess_CL** Use this type to quickly see the process running within the container.</span></span>
- <span data-ttu-id="c43f3-445">**ContainerServiceLog** – tento typ použijte, když se pokoušíte najít informace protokolu auditu pro démona Docker, jako je například spuštění, zastavení, odstraňovat nebo příkazy pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-445">**ContainerServiceLog** – Use this type when you're trying to find audit trail information for the Docker daemon, such as start, stop, delete, or pull commands.</span></span>
- <span data-ttu-id="c43f3-446">**KubeEvents_CL** použít tento typ sledovat Kubernetes události.</span><span class="sxs-lookup"><span data-stu-id="c43f3-446">**KubeEvents_CL**  Use this type to see the Kubernetes events.</span></span>
- <span data-ttu-id="c43f3-447">**KubePodInventory_CL** tento typ použijte, pokud chcete zjistit informace o clusteru hierarchie.</span><span class="sxs-lookup"><span data-stu-id="c43f3-447">**KubePodInventory_CL**  Use this type when you want to understand the cluster hierarchy information.</span></span>


### <a name="to-search-logs-for-container-data"></a><span data-ttu-id="c43f3-448">K vyhledání protokoly pro kontejner dat</span><span class="sxs-lookup"><span data-stu-id="c43f3-448">To search logs for container data</span></span>
* <span data-ttu-id="c43f3-449">Vyberte obrázek, který znáte selhával a najít v souborech protokolů chyb pro ni.</span><span class="sxs-lookup"><span data-stu-id="c43f3-449">Choose an image that you know has failed recently and find the error logs for it.</span></span> <span data-ttu-id="c43f3-450">Začněte tím, že název kontejneru, který běží této bitové kopie s hledání **ContainerInventory** vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="c43f3-450">Start by finding a container name that is running that image with a **ContainerInventory** search.</span></span> <span data-ttu-id="c43f3-451">Například vyhledejte`Type=ContainerInventory ubuntu Failed`</span><span class="sxs-lookup"><span data-stu-id="c43f3-451">For example, search for `Type=ContainerInventory ubuntu Failed`</span></span>  
    <span data-ttu-id="c43f3-452">![Hledat kontejnery Ubuntu](./media/log-analytics-containers/search-ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="c43f3-452">![Search for Ubuntu containers](./media/log-analytics-containers/search-ubuntu.png)</span></span>

  <span data-ttu-id="c43f3-453">Název kontejneru Další **název**a vyhledejte tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="c43f3-453">The name of the container next to **Name**, and search for those logs.</span></span> <span data-ttu-id="c43f3-454">V tomto příkladu je to `Type=ContainerLog cranky_stonebreaker`.</span><span class="sxs-lookup"><span data-stu-id="c43f3-454">In this example, it is `Type=ContainerLog cranky_stonebreaker`.</span></span>

<span data-ttu-id="c43f3-455">**Informace o zobrazení výkonu**</span><span class="sxs-lookup"><span data-stu-id="c43f3-455">**View performance information**</span></span>

<span data-ttu-id="c43f3-456">Pokud jste od vytvořit dotazy, může pomoct vidět co je možné nejprve.</span><span class="sxs-lookup"><span data-stu-id="c43f3-456">When you're beginning to construct queries, it can help to see what's possible first.</span></span> <span data-ttu-id="c43f3-457">Například pokud chcete zobrazit všechny údaje o výkonu, zkuste široký dotaz zadáním následujících vyhledávací dotaz.</span><span class="sxs-lookup"><span data-stu-id="c43f3-457">For example, to see all performance data, try a broad query by typing the following search query.</span></span>

```
Type=Perf
```

![kontejnery výkonu](./media/log-analytics-containers/containers-perf01.png)

<span data-ttu-id="c43f3-459">Data výkonu, která se zobrazuje v určitém kontejneru zadáním názvu je napravo od dotazu, můžete určit obor.</span><span class="sxs-lookup"><span data-stu-id="c43f3-459">You can scope the performance data you're seeing to a specific container by typing the name of it to the right of your query.</span></span>

```
Type=Perf <containerName>
```

<span data-ttu-id="c43f3-460">Který zobrazí seznam metriky výkonu, které se shromažďují pro jednotlivé kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c43f3-460">That shows the list of performance metrics that are collected for an individual container.</span></span>

![kontejnery výkonu](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a><span data-ttu-id="c43f3-462">Příklad protokolu vyhledávací dotazy</span><span class="sxs-lookup"><span data-stu-id="c43f3-462">Example log search queries</span></span>
<span data-ttu-id="c43f3-463">Je často užitečné k vytvoření dotazů počínaje příklad nebo dva a pak úpravy, aby odpovídaly vašemu prostředí.</span><span class="sxs-lookup"><span data-stu-id="c43f3-463">It's often useful to build queries starting with an example or two and then modifying them to fit your environment.</span></span> <span data-ttu-id="c43f3-464">Jako počáteční bod, můžete vyzkoušet **ukázkové dotazy** oblasti, které vám umožní vytvořit složitější dotazy.</span><span class="sxs-lookup"><span data-stu-id="c43f3-464">As a starting point, you can experiment with the **Sample Queries** area to help you build more advanced queries.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Kontejnery dotazy](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a><span data-ttu-id="c43f3-466">Uložení protokolu vyhledávacích dotazů</span><span class="sxs-lookup"><span data-stu-id="c43f3-466">Saving log search queries</span></span>
<span data-ttu-id="c43f3-467">Uložení dotazů je standardní funkce v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-467">Saving queries is a standard feature in Log Analytics.</span></span> <span data-ttu-id="c43f3-468">Je uložíte, budete mít ty, které jste najít užitečné užitečný pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="c43f3-468">By saving them, you'll have those that you've found useful handy for future use.</span></span>

<span data-ttu-id="c43f3-469">Jakmile vytvoříte dotaz, který pro vás užitečné, uložte kliknutím na **Oblíbené** v horní části stránky hledání protokolu.</span><span class="sxs-lookup"><span data-stu-id="c43f3-469">After you create a query that you find useful, save it by clicking **Favorites** at the top of the Log Search page.</span></span> <span data-ttu-id="c43f3-470">Potom ho později snadno přístup **vlastní řídicí panel** stránky.</span><span class="sxs-lookup"><span data-stu-id="c43f3-470">Then you can easily access it later from the **My Dashboard** page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c43f3-471">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c43f3-471">Next steps</span></span>
* <span data-ttu-id="c43f3-472">[V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné kontejneru datových záznamů.</span><span class="sxs-lookup"><span data-stu-id="c43f3-472">[Search logs](log-analytics-log-searches.md) to view detailed container data records.</span></span>
